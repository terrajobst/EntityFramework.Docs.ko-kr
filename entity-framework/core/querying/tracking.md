---
title: 추적 및 비 추적 쿼리 - EF Core
author: smitpatel
ms.date: 10/10/2019
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: a6c71c12f429f1324abe91d1b2cef96312bec051
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413698"
---
# <a name="tracking-vs-no-tracking-queries"></a>추적 및 비 추적 쿼리

추적 동작은 Entity Framework Core가 해당 변경 추적 장치에서 엔터티 인스턴스에 대한 정보를 유지할지 여부를 제어합니다. 엔터티를 추적하는 경우 엔터티에서 검색된 변경 내용은 `SaveChanges()` 중 데이터베이스에 유지됩니다. EF Core는 추적 쿼리 결과에 있는 엔터티와 변경 추적 장치에 있는 엔터티 간 탐색 속성도 수정합니다.

> [!NOTE]
> [키 없는 엔터티 형식](xref:core/modeling/keyless-entity-types)은 추적되지 않습니다. 이 문서에서 언급되는 모든 엔터티 형식은 키가 정의된 엔터티 형식을 가리킵니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying)을 볼 수 있습니다.

## <a name="tracking-queries"></a>추적 쿼리

기본적으로 엔터티 형식을 반환하는 쿼리는 추적됩니다. 즉, 해당 엔터티 인스턴스를 변경하고 `SaveChanges()`에서 해당 변경 내용을 유지하도록 할 수 있습니다. 다음 예제에서는 `SaveChanges()` 중에 블로그 등급 변경을 검색하고 데이터베이스에 유지합니다.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#Tracking)]

## <a name="no-tracking-queries"></a>비 추적 쿼리

비 추적 쿼리는 읽기 전용 시나리오에서 결과가 사용되는 경우에 유용합니다. 이 쿼리는 변경 내용 추적 정보를 설정할 필요가 없기 때문에 더 빠르게 실행할 수 있습니다. 데이터베이스에서 검색된 엔터티를 업데이트할 필요가 없는 경우 비 추적 쿼리를 사용해야 합니다. 개별 쿼리를 비 추적 쿼리로 전환할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#NoTracking)]

컨텍스트 인스턴스 수준에서 기본 추적 동작을 변경할 수도 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ContextDefaultTrackingBehavior)]

## <a name="identity-resolution"></a>ID 확인

추적 쿼리는 변경 추적 장치를 사용하므로 EF Core는 추적 쿼리에서 ID 확인을 수행합니다. 엔터티를 구체화할 때 엔터티 인스턴스가 이미 추적 중인 경우 EF Core는 변경 추적기에서 동일한 엔터티 인스턴스를 반환합니다. 결과에 동일한 엔터티가 여러 번 포함되는 경우 발생할 때마다 동일한 인스턴스를 다시 가져옵니다. 비 추적 쿼리는 변경 추적 장치를 사용하지 않으며 ID 확인을 수행하지 않습니다. 따라서 동일한 엔터티가 결과에 여러 번 포함되는 경우에도 엔터티의 새 인스턴스를 다시 가져옵니다. 이 동작은 EF Core 3.0 이전 버전과는 다릅니다. [이전 버전](#previous-versions)을 참조하세요.

## <a name="tracking-and-custom-projections"></a>추적 및 사용자 지정 프로젝션

쿼리의 결과 형식이 엔터티 형식이 아닌 경우에도 EF Core는 결과에 포함된 엔터티 형식을 기본적으로 추적합니다. 무명 형식을 반환하는 다음 쿼리에서는 결과 집합에 있는 `Blog`의 인스턴스가 추적됩니다.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection1)]

결과 집합에 LINQ 컴퍼지션에서 나오는 엔터티 형식이 포함되어 있으면 EF Core가 이를 추적합니다.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

결과 집합에 포함된 엔터티 형식이 없으면 추적이 수행되지 않습니다. 다음 쿼리에서는 엔터티의 값 일부가 있는 무명 형식을 반환합니다(하지만 실제 엔터티 형식의 인스턴스는 반환하지 않습니다). 쿼리에서 나오는 추적되는 엔터티는 없습니다.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection3)]

 EF Core는 최상위 프로젝션에서의 클라이언트 평가 수행을 지원합니다. EF Core가 클라이언트 평가를 위해 엔터티 인스턴스를 구체화하는 경우 이 엔터티 인스턴스가 추적됩니다. 여기서는 클라이언트 메서드 `StandardizeURL`에 `blog` 엔터티를 전달하므로 EF Core는 블로그 인스턴스도 추적합니다.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientMethod)]

EF Core는 결과에 포함된 키 없는 엔터티 인스턴스를 추적하지 않습니다. 그러나 EF Core는 위의 규칙에 따라 키가 있는 엔터티 형식의 다른 인스턴스를 모두 추적합니다.

위의 규칙 일부는 EF Core 3.0 이전에는 다르게 작동했습니다. 자세한 내용은 [이전 버전](#previous-versions)을 참조하세요.

## <a name="previous-versions"></a>이전 버전

버전 3.0 이전에는 EF Core의 추적 수행 방법에 약간의 차이가 있었습니다. 주목할 만한 차이점은 다음과 같습니다.

- [클라이언트 및 서버 평가](xref:core/querying/client-eval) 페이지에 설명한 것처럼 버전 3.0 이전에는 EF Core가 쿼리의 모든 부분에서 클라이언트 평가를 지원했습니다. 클라이언트 평가로 인해 결과에 포함되지 않는 엔터티의 구체화가 수행되었습니다. 따라서 EF Core는 결과를 분석하여 추적할 항목을 검색했습니다. 이 설계에는 다음과 같은 몇 가지 차이점이 있었습니다.
  - 구체화를 초래했지만 구체화된 엔터티 인스턴스를 반환하지 않은 프로젝션의 클라이언트 평가는 추적되지 않았습니다. 다음 예는 `blog` 엔터티를 추적하지 않았습니다.
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

  - EF Core는 특정한 경우에 LINQ 컴퍼지션에서 나오는 개체를 추적하지 않았습니다. 다음 예는 `Post`를 추적하지 않았습니다.
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

- 쿼리 결과에 키 없는 엔터티 형식이 포함될 때마다 전체 쿼리가 비 추적 쿼리로 전환되었습니다. 즉, 키가 있고 결과에 포함된 엔터티 형식도 추적되지 않았습니다.
- EF Core는 비 추적 쿼리에서 ID 확인을 수행했습니다. EF Core는 약한 참조를 사용하여 이미 반환된 엔터티를 추적했습니다. 따라서 결과 집합에 동일한 엔터티가 여러 번 포함된 경우 발생할 때마다 동일한 인스턴스를 가져옵니다. ID가 동일한 이전 결과가 범위를 벗어나고 가비지가 수집된 경우에도 EF Core는 새 인스턴스를 반환했습니다.
