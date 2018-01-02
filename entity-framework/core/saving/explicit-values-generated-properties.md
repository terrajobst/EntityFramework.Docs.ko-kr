---
title: "생성 된 속성-EF 코어에 대 한 명시적 값을 설정합니다."
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
ms.technology: entity-framework-core
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: f34e92d9a3b10b6ff904257ccd047a8acdaad231
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
# <a name="setting-explicit-values-for-generated-properties"></a>생성 된 속성에 대 한 명시적 값을 설정합니다.

생성 된 속성 값 (또는 중 하나가 EF 데이터베이스)에 생성 되는 속성은 엔터티 추가 하거나 업데이트 합니다. 참조 [속성 생성](../modeling/generated-properties.md) 자세한 정보에 대 한 합니다.

생성 된 하나 있는 것 보다는 생성된 된 속성에 대 한 명시적 값을 설정 하려는 경우가 있습니다.

> [!TIP]  
> 이 문서를 볼 수 있습니다 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/ExplicitValuesGenerateProperties/) GitHub에서 합니다.

## <a name="the-model"></a>모델

이 문서에 사용 되는 모델 포함 하는 하나의 `Employee` 엔터티.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>추가에서 명시적 값을 저장합니다.

`Employee.EmploymentStarted` 속성 (기본값을 사용 하 여) 새 엔터티에 대 한 데이터베이스에서 생성 된 값을 포함 하도록 구성 되어 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

다음 코드를 데이터베이스에 두 명의 직원이 삽입 합니다.
* 에 할당 된 값에 대 한 첫 번째 `Employee.EmploymentStarted` 속성을 상태로 유지 됩니다에 대 한 CLR 기본 값으로 설정 `DateTime`합니다.
* 두 번째에 대 한 명시적 값을 설정한 `1-Jan-2000`합니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

출력은 데이터베이스는 첫 번째 직원에 대 한 값을 생성 하 고 우리의 명시적 값이 두 번째에 대 한 사용을 보여 줍니다.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>SQL Server ID 열에 명시적 값

일반적으로는 `Employee.EmployeeId` 속성은 생성 된 저장소 `IDENTITY` 열입니다.

대부분의 경우 키 속성에 대 한 위에 표시 된 접근 방식을 사용 합니다. 그러나 SQL Server에 명시적 값을 삽입할 `IDENTITY` 열을 수동으로 사용 하도록 설정 해야 `IDENTITY_INSERT` 호출 하기 전에 `SaveChanges()`합니다.

> [!NOTE]  
> 했으므로 [기능 요청](https://github.com/aspnet/EntityFramework/issues/703) SQL Server 공급자 내에서 자동으로 이렇게 하려면 백로그에 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

출력에 제공 된 id는 데이터베이스에 저장 된 표시 됩니다.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>명시적 값을 업데이트 하는 동안 설정

`Employee.LastPayRaise` 속성 업데이트 하는 동안 데이터베이스에서 생성 하는 값을 포함 하도록 구성 되어 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> 기본적으로 EF 코어 업데이트 하는 동안 생성 하도록 구성 된 속성에 대 한 명시적 값을 저장 하려고 하면 예외가 throw 됩니다. 이 문제를 방지 하려면 드롭다운 하위 수준 메타 데이터 API 하 고 설정 된 `AfterSaveBehavior` (위와 같이).

> [!NOTE]  
> **EF 코어 2.0의 변경 내용:** 후 저장 동작을 통해 제어 된 이전 릴리스에서 `IsReadOnlyAfterSave` 플래그입니다. 이 플래그를 사용 하지 않는 하 고로 대체 되었습니다 `AfterSaveBehavior`합니다.

에 대 한 값을 생성 하는 데이터베이스에서 트리거도 되는 `LastPayRaise` 열 중 `UPDATE` 작업 합니다.

[!code-sql[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

다음 코드는 데이터베이스에 두 명의 직원의 급여를 증가합니다.
* 에 할당 된 값에 대 한 첫 번째 `Employee.LastPayRaise` 속성을 설정 하므로 null로 합니다.
* 두 번째에 대 한 명시적 값 1 주일 전의 (뒤로 사용량 기준 과금 데이트 raise) 설정 합니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

출력은 데이터베이스는 첫 번째 직원에 대 한 값을 생성 하 고 우리의 명시적 값이 두 번째에 대 한 사용을 보여 줍니다.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```