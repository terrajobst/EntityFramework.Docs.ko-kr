---
title: "동시성 충돌-EF 핵심 처리"
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 288d9c6fced5ebbaa2c366248c68547502c3698e
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/05/2018
---
# <a name="handling-concurrency-conflicts"></a>동시성 충돌 처리

> [!NOTE]
> 이 페이지는 EF 코어에서 동시성의 작동 방식 및 응용 프로그램에서 동시성 충돌을 처리 하는 방법을 설명 합니다. 참조 [동시성 토큰](xref:core/modeling/concurrency) 모델에서 동시성 토큰을 구성 하는 방법에 대 한 자세한 내용은 합니다.

> [!TIP]
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/)을 볼 수 있습니다.

_데이터베이스 동시성_ 수 있는 여러 프로세스 또는 사용자가 액세스 하거나 동시에 데이터베이스의 동일한 데이터를 변경 하는 경우 참조 합니다. _동시성 제어_ 란 동시 변경의 존재에 데이터 일관성을 보장 하는 데 사용 되는 특정 메커니즘을 지칭 합니다.

EF 코어 구현 _낙관적 동시성 제어_, 여러 프로세스 수 또는 사용자가 동기화의 오버 헤드 없이 독립적으로 변경할 또는 잠금을 합니다. 이상적인 상황에서 이러한 변경 내용은 서로를 방해 하지 않도록 하 고 성공 하는 일을 할 수 있습니다. 최악의 경우에서 두 개 이상의 프로세스가 변경 내용 충돌을 시도 하 고 둘 중 하나만 성공적으로.

## <a name="how-concurrency-control-works-in-ef-core"></a>EF 코어에서 동시성 제어의 작동 방식

속성이 동시성 토큰은 낙관적 동시성 제어를 구현 하는 데 사용 되는 대로 구성: 업데이트 또는 삭제 작업을 수행 하는 동안 때마다 `SaveChanges`, 원본에 대해 데이터베이스의 동시성 토큰의 값을 비교 EF 코어 읽은 값입니다.

- 값이 일치 작업을 완료할 수 있습니다.
- 값이 일치 하지 않는 경우 다른 사용자는 충돌 하는 연산을 수행 하 고 현재 트랜잭션을 중단 EF 핵심 가정 합니다.

다른 사용자는 현재 작업과 충돌 하는 작업을 수행한 경우 상황 이라고 _동시성 충돌_합니다.

데이터베이스 공급자는 동시성 토큰 값의 비교를 구현 해야 합니다.

관계형 데이터베이스 EF 코어에서 동시성 토큰의 값에 대 한 확인을 포함 된 `WHERE` 모든 절 `UPDATE` 또는 `DELETE` 문. EF 코어 명령문을 실행 한 후 영향을 받는 행의 수를 읽습니다.

행이 없는 영향을 받는 동시성 충돌이 감지 되 면 및 EF 코어 throw `DbUpdateConcurrencyException`합니다.

예를 들어 구성 해야 할 수 있습니다 `LastName` 에 `Person` 동시성 토큰 되도록 합니다. 사용자 업데이트 작업은 동시성 확인에 포함 됩니다는 `WHERE` 절:

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>동시성 충돌 해결

한 사용자의 일부 변경 내용을 저장 하려고 하는 경우 앞의 예제를 계속 한 `Person`, 다른 사용자가 이미 변경 된의 `LastName` 는 예외가 throw 됩니다.

이 시점에서 응용 프로그램 수 단순히 업데이트 충돌 하는 변경 내용으로 인해 성공 하지 않았음을 사용자에 게 알립니다 및에서 이동 합니다. 하지만이 레코드에는 여전히 동일한 실제 사용자 나타냅니다 확인 하 고 작업을 다시 시도를 사용자를 요청할 수 있습니다.

이 프로세스의 예는 _동시성 충돌을 해결_합니다.

현재에서 보류 중인 변경 내용 병합에서는 동시성 충돌을 해결 `DbContext` 데이터베이스의 값을 사용 합니다. 어떤 값이 병합 가져올 응용 프로그램에 따라 달라 집니다 및 사용자 입력에 따라 리디렉션될 수 있습니다.

**동시성 충돌을 해결 하는 데 사용할 수 있는 값의 세 가지 집합이 있습니다.**

* **현재 값** 응용 프로그램 데이터베이스에 기록 하려고 했던 되는 값입니다.

* **원래 값** 편집을 수행 하기 전에 원래 데이터베이스에서 검색 되는 값입니다.

* **값을 데이터베이스** 현재 데이터베이스에 저장 하는 값입니다.

동시성 충돌을 처리 하는 일반적인 방법이입니다.

1. Catch `DbUpdateConcurrencyException` 중 `SaveChanges`합니다.
2. 사용 하 여 `DbUpdateConcurrencyException.Entries` 영향을 받는 엔터티에 대 한 새 변경 집합을 준비 합니다.
3. 데이터베이스의 현재 값을 반영 하도록 동시성 토큰의 원래 값을 새로 고칩니다.
4. 없는 충돌이 발생할 때까지 프로세스를 다시 시도 합니다.

다음 예에서 `Person.FirstName` 및 `Person.LastName` 동시성 토큰으로 설정 되도록 합니다. 한 `// TODO:` 을 저장할 값을 선택할 수 있는 응용 프로그램별 논리를 포함 하는 위치에 대 한 설명입니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
