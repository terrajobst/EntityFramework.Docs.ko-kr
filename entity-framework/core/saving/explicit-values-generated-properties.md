---
title: 생성된 속성에 대한 명시적 값 설정 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: ea469b9b7199cc767b2d0da1a5999026f938d087
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656252"
---
# <a name="setting-explicit-values-for-generated-properties"></a>생성된 속성에 대한 명시적 값 설정

생성된 속성은 엔터티가 추가되거나 업데이트될 때 EF나 데이터베이스에서 값이 생성되는 속성입니다. 자세한 내용은 [생성된 속성](../modeling/generated-properties.md)을 참조하세요.

생성된 속성에 대해 값을 생성하지 않고 명시적 값을 설정하려는 경우가 있을 수 있습니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/ExplicitValuesGenerateProperties/)을 볼 수 있습니다.

## <a name="the-model"></a>모델

이 문서에 사용된 모델에는 단일 `Employee` 엔터티가 포함되어 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>추가 중 명시적 값 저장

`Employee.EmploymentStarted` 속성은 기본값을 사용하여 새 엔터티에 대해 데이터베이스에서 값을 생성하도록 구성됩니다.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

다음 코드에서는 데이터베이스에 두 명의 직원을 삽입합니다.

* 첫 번째의 경우 `Employee.EmploymentStarted` 속성에 값이 할당되지 않으므로 `DateTime`에 대한 CLR 기본값으로 설정됩니다.
* 두 번째의 경우 `1-Jan-2000`이라는 명시적 값을 설정했습니다.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

출력에서는 데이터베이스가 첫 번째 직원에 대해서는 값을 생성하고 두 번째에는 명시적 값을 사용했음을 보여줍니다.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>SQL Server IDENTITY 열의 명시적 값

일반적으로 `Employee.EmployeeId` 속성은 저장소 생성 `IDENTITY` 열입니다.

대부분의 경우 위에 표시된 접근 방법은 주요 속성에 대해 작동합니다. 그러나 SQL Server `IDENTITY` 열에 명시적 값을 삽입하려면 `SaveChanges()`를 호출하기 전에 `IDENTITY_INSERT`를 수동으로 사용해야 합니다.

> [!NOTE]  
> SQL Server 공급자 내에서 자동으로 이 작업을 수행하려면 백로그에 [기능 요청](https://github.com/aspnet/EntityFramework/issues/703)이 있어야 합니다.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

출력에서는 제공된 ID가 데이터베이스에 저장되었음을 보여줍니다.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>업데이트 중 명시적 값 설정

`Employee.LastPayRaise` 속성은 업데이트 중에 데이터베이스에서 값을 생성하도록 구성됩니다.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> 기본적으로 업데이트 중에 생성되도록 구성된 속성에 대한 명시적 값을 저장하려고 하면 EF Core에서 예외를 throw합니다. 이를 방지하려면 하위 수준 메타데이터 API로 드롭다운하고 위에 표시된 대로 `AfterSaveBehavior`를 설정해야 합니다.

> [!NOTE]  
> **EF Core 2.0의 변경 내용:** 이전 릴리스에서는 after-save 동작이 `IsReadOnlyAfterSave` 플래그를 통해 제어되었습니다. 이 플래그는 사용되지 않으며 `AfterSaveBehavior`로 대체되었습니다.

데이터베이스에는 `UPDATE` 작업 중에 `LastPayRaise` 열에 대한 값을 생성하는 트리거도 있습니다.

[!code-sql[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

다음 코드에서는 데이터베이스에서 두 직원의 급여를 올립니다.

* 첫 번째의 경우 `Employee.LastPayRaise` 속성에 값이 할당되지 않으므로 null로 설정됩니다.
* 두 번째의 경우 1주일 전(급여 인상 날짜 소급)이라는 명시적 값을 설정했습니다.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

출력에서는 데이터베이스가 첫 번째 직원에 대해서는 값을 생성하고 두 번째에는 명시적 값을 사용했음을 보여줍니다.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
