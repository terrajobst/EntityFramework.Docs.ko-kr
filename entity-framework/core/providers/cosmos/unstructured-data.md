---
title: Azure Cosmos DB 공급자-비구조적 데이터 작업-EF Core
description: Entity Framework Core를 사용 하 여 Azure Cosmos DB 비구조적 데이터로 작업 하는 방법
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 0bfccbfd3af6e209967004752b5a3947d644544b
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655512"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a>EF Core Azure Cosmos DB 공급자에서 비구조적 데이터 작업

EF Core은 모델에 정의 된 스키마를 따르는 데이터를 쉽게 사용할 수 있도록 설계 되었습니다. 그러나 Azure Cosmos DB의 장점 중 하나는 저장 되는 데이터의 모양에 대 한 유연성입니다.

## <a name="accessing-the-raw-json"></a>원시 JSON 액세스

저장소에서 받은 데이터를 나타내는 `JObject`를 포함 하는 `"__jObject"` 라는 [섀도 상태의](../../modeling/shadow-properties.md) 특수 속성을 통해 EF Core에서 추적 되지 않는 속성에 액세스할 수 있으며 저장 되는 데이터에 액세스할 수 있습니다.

[!code-csharp[Unmapped](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=23,24&name=Unmapped)]

``` json
{
    "Id": 1,
    "PartitionKey": "1",
    "TrackingNumber": null,
    "id": "1",
    "Address": {
        "ShipsToCity": "London",
        "ShipsToStreet": "221 B Baker St"
    },
    "_rid": "eLMaAK8TzkIBAAAAAAAAAA==",
    "_self": "dbs/eLMaAA==/colls/eLMaAK8TzkI=/docs/eLMaAK8TzkIBAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683e-0a12bf8d01d5\"",
    "_attachments": "attachments/",
    "BillingAddress": "Clarence House",
    "_ts": 1568164374
}
```

> [!WARNING]
> `"__jObject"` 속성은 EF Core 인프라의 일부 이며, 이후 릴리스에서 다른 동작이 발생할 가능성이 있으므로 마지막 수단 으로만 사용 해야 합니다.

> [!NOTE]
> 엔터티에 대 한 변경 내용은 `SaveChanges`중에 `"__jObject"`에 저장 된 값을 재정의 합니다.

## <a name="using-cosmosclient"></a>CosmosClient 사용

완전히 분리 EF Core [AZURE COSMOS DB SDK의 일부인](/azure/cosmos-db/sql-api-get-started) [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) 개체를 `DbContext`에서 가져옵니다.

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a>누락 된 속성 값

이전 예제에서는 순서에서 `"TrackingNumber"` 속성을 제거 했습니다. Cosmos DB에서 인덱싱이 작동 하는 방식 때문에, 누락 된 속성을 참조 하는 쿼리가 프로젝션의 다른 위치에서 예기치 않은 결과를 반환할 수 있습니다. 예를 들면,

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

정렬 된 쿼리는 실제로 결과를 반환 하지 않습니다. 즉, 저장소를 직접 사용 하는 경우 EF Core에 의해 매핑되는 속성을 항상 채워야 합니다.

> [!NOTE]
> 이 동작은 Cosmos의 이후 버전에서 변경 될 수 있습니다. 예를 들어 현재 인덱싱 정책이 복합 인덱스 {Id/?를 정의 하는 경우 ASC, TrackingNumber/? ASC)}의 경우 ' ORDER BY c.Id ASC, ASC '가 있는 쿼리는 `"TrackingNumber"` 속성이 누락 된 항목 __을 반환 합니다__ .
