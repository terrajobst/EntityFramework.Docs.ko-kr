---
title: 전역 쿼리 필터 - EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 9262ff7970b0502945480c673315071cbc3f44b9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413764"
---
# <a name="global-query-filters"></a>전역 쿼리 필터

> [!NOTE]
> 이 기능은 EF Core 2.0에서 도입되었습니다.

전역 쿼리 필터는 메타데이터 모델(일반적으로 *OnModelCreating*)의 엔터티 형식에 적용되는 LINQ 쿼리 조건자(일반적으로 LINQ *Where* 쿼리 연산자에 전달되는 부울 식)입니다. 이러한 필터는 Include 사용이나 직접 탐색 속성 참조 등의 간접 참조되는 엔터티 형식 등을 포함하여 해당 엔터티 형식과 관련한 모든 LINQ 쿼리에 자동으로 적용됩니다. 이 기능의 몇 가지 일반적인 용도는 다음과 같습니다.

* **일시 삭제** - 엔터티 형식이 *IsDeleted* 속성을 정의합니다.
* **다중 테넌트** - 엔터티 형식이 *TenantId* 속성을 정의합니다.

## <a name="example"></a>예제

다음 예제에서는 전역 쿼리 필터를 사용하여 간단한 블로그 모델에서 일시 삭제 및 다중 테넌트 쿼리 동작을 구현하는 방법을 보여줍니다.

> [!TIP]
> GitHub에서 이 문서의 [샘플](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters)을 볼 수 있습니다.

먼저 엔터티를 정의합니다.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

_블로그_ 엔터티에서 _tenantId_ 필드의 선언을 확인하세요. 이 선언은 각 Blog 인스턴스를 특정 테넌트와 연결하는 데 사용됩니다. 또한 _IsDeleted_ 속성은 _Post_ 엔터티 형식에 정의되어 있습니다. 이 속성은 _Post_ 인스턴스가 “일시 삭제”되었는지 여부를 추적하는 데 사용됩니다. 즉, 기본 데이터를 물리적으로 제거하지 않아도 인스턴스가 삭제된 것으로 표시됩니다.

다음으로, _API를 사용하여_OnModelCreating`HasQueryFilter`에서 쿼리 필터를 구성합니다.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

이제 _HasQueryFilter_ 호출에 전달된 조건자 식이 해당 형식에 대한 모든 LINQ 쿼리에 자동으로 적용됩니다.

> [!TIP]
> DbContext 인스턴스 수준 필드를 사용합니다. `_tenantId`는 현재 테넌트를 설정하는 데 사용됩니다. 모델 수준 필터는 올바른 컨텍스트 인스턴스(즉, 쿼리를 실행하는 인스턴스)의 값을 사용합니다.

> [!NOTE]
> 현재 동일한 엔터티에 여러 개의 쿼리 필터를 정의할 수 없습니다. 마지막 필터만 적용됩니다. 그러나 논리 _AND_ 연산자([C#의 `&&`](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-))를 사용하여 여러 조건으로 단일 필터를 정의할 수 있습니다.

## <a name="disabling-filters"></a>필터 사용 안 함

`IgnoreQueryFilters()` 연산자를 사용하여 개별 LINQ 쿼리에 대해 필터를 사용하지 않도록 설정할 수 있습니다.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>제한 사항

전역 쿼리 필터에는 다음 제한 사항이 있습니다.

* 상속 계층 구조의 루트 엔터티 형식에 대해서만 필터를 정의할 수 있습니다.
