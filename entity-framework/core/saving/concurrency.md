---
title: 동시성 충돌 처리 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 2d8909585201a45eb020537847800f125b3b0120
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2018
ms.locfileid: "42447766"
---
# <a name="handling-concurrency-conflicts"></a>동시성 충돌 처리

> [!NOTE]
> 이 페이지에서는 EF Core에서 동시성의 작동 방식과 응용 프로그램에서 동시성 충돌을 처리하는 방법에 대해 설명합니다. 모델에서 동시성 토큰을 구성하는 방법에 대한 자세한 내용은 [동시성 토큰](xref:core/modeling/concurrency)을 참조하세요.

> [!TIP]
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/)을 볼 수 있습니다.

‘데이터베이스 동시성’이란 여러 프로세스 또는 사용자가 동시에 데이터베이스에서 동일한 데이터를 액세스하거나 변경하는 상황을 의미합니다. ‘동시성 제어’는 동시 변경 내용이 있을 경우 데이터 일관성을 보장하는 데 사용되는 특정 메커니즘을 의미합니다.

EF Core는 ‘낙관적 동시성 제어’를 구현하여 여러 프로세스 또는 사용자가 동기화 또는 잠금의 오버헤드 없이 독립적으로 변경할 수 있습니다. 이상적인 상황에서는 이러한 변경 내용이 서로 방해가 되지 않으므로 성공할 수 있습니다. 최악의 경우 둘 이상의 프로세스에서 충돌하는 변경을 수행하려고 하여 하나만 성공합니다.

## <a name="how-concurrency-control-works-in-ef-core"></a>EF Core에서 동시성 제어의 작동 방식

동시성 토큰으로 구성된 속성을 사용하여 낙관적 동시성 제어를 구현합니다. `SaveChanges` 중에 업데이트 또는 삭제 작업이 수행될 때마다 데이터베이스의 동시성 토큰 값이 EF Core에서 읽은 원래 값과 비교됩니다.

- 값이 일치하면 작업이 완료됩니다.
- 값이 일치하지 않으면 EF Core는 다른 사용자가 충돌하는 작업을 수행했다고 가정하고 현재 트랜잭션을 중단합니다.

다른 사용자가 현재 작업과 충돌하는 작업을 수행한 경우를 ‘동시성 충돌’이라고 합니다.

데이터베이스 공급자는 동시성 토큰 값 비교를 구현해야 합니다.

관계형 데이터베이스에서 EF Core는 `UPDATE` 또는 `DELETE` 문의 `WHERE` 절에 동시성 토큰 값 검사를 포함합니다. 문을 실행한 후 EF Core는 영향을 받은 행 수를 읽습니다.

영향을 받은 행이 없는 경우 동시성 충돌이 감지되고 EF Core는 `DbUpdateConcurrencyException`을 throw합니다.

예를 들어 `Person`의 `LastName`을 동시성 토큰으로 구성할 수 있습니다. 그러면 Person에 대한 업데이트 작업에서 `WHERE` 절에 동시성 검사를 포함합니다.

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>동시성 충돌 해결

이전 예제에서 한 사용자는 `Person`의 일부 변경 내용을 저장하려고 하는데 다른 사용자는 `LastName`을 이미 변경한 경우 예외가 throw됩니다.

이때 응용 프로그램은 충돌하는 변경 내용으로 인해 업데이트가 실패했음을 사용자에게 알리기만 하고 계속 진행할 수 있습니다. 그러나 이 레코드가 여전히 동일한 실제 사람을 나타내도록 하고 작업을 다시 시도하도록 사용자에게 메시지를 표시하는 것이 좋습니다.

이 프로세스는 ‘동시성 충돌 해결’의 예제입니다.

동시성 충돌 해결에는 현재 `DbContext`에서 보류 중인 변경 내용을 데이터베이스의 값과 병합하는 작업이 포함됩니다. 병합되는 값은 응용 프로그램에 따라 달라지며 사용자 입력으로 지정할 수 있습니다.

**세 가지 값 집합을 동시성 충돌 해결에 사용할 수 있습니다.**

* **현재 값**은 응용 프로그램이 데이터베이스에 쓰려고 시도하는 값입니다.

* **원래 값**은 편집을 수행하기 전에 데이터베이스에서 처음에 검색된 값입니다.

* **데이터베이스 값**은 현재 데이터베이스에 저장된 값입니다.

동시성 충돌을 처리하는 일반적인 접근 방법은 다음과 같습니다.

1. `SaveChanges` 중 `DbUpdateConcurrencyException`을 catch합니다.
2. `DbUpdateConcurrencyException.Entries`를 사용하여 영향을 받는 엔터티에 대해 새로운 변경 내용 집합을 준비합니다.
3. 데이터베이스의 현재 값을 반영하도록 동시성 토큰의 원래 값을 새로 고칩니다.
4. 충돌이 발생하지 않을 때까지 프로세스를 다시 시도합니다.

다음 예제에서는 `Person.FirstName` 및 `Person.LastName`을 동시성 토큰으로 설정합니다. 저장할 값을 선택하는 응용 프로그램별 논리를 포함하는 위치에 `// TODO:` 주석이 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
