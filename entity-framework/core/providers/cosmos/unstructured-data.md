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
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="43e6a-103">EF Core Azure Cosmos DB 공급자에서 비구조적 데이터 작업</span><span class="sxs-lookup"><span data-stu-id="43e6a-103">Working with Unstructured Data in EF Core Azure Cosmos DB Provider</span></span>

<span data-ttu-id="43e6a-104">EF Core은 모델에 정의 된 스키마를 따르는 데이터를 쉽게 사용할 수 있도록 설계 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="43e6a-104">EF Core was designed to make it easy to work with data that follows a schema defined in the model.</span></span> <span data-ttu-id="43e6a-105">그러나 Azure Cosmos DB의 장점 중 하나는 저장 되는 데이터의 모양에 대 한 유연성입니다.</span><span class="sxs-lookup"><span data-stu-id="43e6a-105">However one of the strengths of Azure Cosmos DB is the flexibility in the shape of the data stored.</span></span>

## <a name="accessing-the-raw-json"></a><span data-ttu-id="43e6a-106">원시 JSON 액세스</span><span class="sxs-lookup"><span data-stu-id="43e6a-106">Accessing the raw JSON</span></span>

<span data-ttu-id="43e6a-107">저장소에서 받은 데이터를 나타내는 `JObject`를 포함 하는 `"__jObject"` 라는 [섀도 상태의](../../modeling/shadow-properties.md) 특수 속성을 통해 EF Core에서 추적 되지 않는 속성에 액세스할 수 있으며 저장 되는 데이터에 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="43e6a-107">It is possible to access the properties that are not tracked by EF Core through a special property in [shadow-state](../../modeling/shadow-properties.md) named `"__jObject"` that contains a `JObject` representing the data recieved from the store and data that will be stored:</span></span>

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
> <span data-ttu-id="43e6a-108">`"__jObject"` 속성은 EF Core 인프라의 일부 이며, 이후 릴리스에서 다른 동작이 발생할 가능성이 있으므로 마지막 수단 으로만 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="43e6a-108">The `"__jObject"` property is part of the EF Core infrastructure and should only be used as a last resort as it is likely to have different behavior in future releases.</span></span>

> [!NOTE]
> <span data-ttu-id="43e6a-109">엔터티에 대 한 변경 내용은 `SaveChanges`중에 `"__jObject"`에 저장 된 값을 재정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="43e6a-109">Changes to the entity will override the values stored in `"__jObject"` during `SaveChanges`.</span></span>

## <a name="using-cosmosclient"></a><span data-ttu-id="43e6a-110">CosmosClient 사용</span><span class="sxs-lookup"><span data-stu-id="43e6a-110">Using CosmosClient</span></span>

<span data-ttu-id="43e6a-111">완전히 분리 EF Core [AZURE COSMOS DB SDK의 일부인](/azure/cosmos-db/sql-api-get-started) [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) 개체를 `DbContext`에서 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="43e6a-111">To decouple completely from EF Core get the [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) object that is [part of the Azure Cosmos DB SDK](/azure/cosmos-db/sql-api-get-started) from `DbContext`:</span></span>

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a><span data-ttu-id="43e6a-112">누락 된 속성 값</span><span class="sxs-lookup"><span data-stu-id="43e6a-112">Missing property values</span></span>

<span data-ttu-id="43e6a-113">이전 예제에서는 순서에서 `"TrackingNumber"` 속성을 제거 했습니다.</span><span class="sxs-lookup"><span data-stu-id="43e6a-113">In the previous example we removed the `"TrackingNumber"` property from the order.</span></span> <span data-ttu-id="43e6a-114">Cosmos DB에서 인덱싱이 작동 하는 방식 때문에, 누락 된 속성을 참조 하는 쿼리가 프로젝션의 다른 위치에서 예기치 않은 결과를 반환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="43e6a-114">Because of how indexing works in Cosmos DB, queries that reference the missing property somewhere else than in the projection could return unexpected results.</span></span> <span data-ttu-id="43e6a-115">예를 들면,</span><span class="sxs-lookup"><span data-stu-id="43e6a-115">For example:</span></span>

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

<span data-ttu-id="43e6a-116">정렬 된 쿼리는 실제로 결과를 반환 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="43e6a-116">The sorted query actually returns no results.</span></span> <span data-ttu-id="43e6a-117">즉, 저장소를 직접 사용 하는 경우 EF Core에 의해 매핑되는 속성을 항상 채워야 합니다.</span><span class="sxs-lookup"><span data-stu-id="43e6a-117">This means that one should take care to always populate properties mapped by EF Core when working with the store directly.</span></span>

> [!NOTE]
> <span data-ttu-id="43e6a-118">이 동작은 Cosmos의 이후 버전에서 변경 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="43e6a-118">This behavior might change in future versions of Cosmos.</span></span> <span data-ttu-id="43e6a-119">예를 들어 현재 인덱싱 정책이 복합 인덱스 {Id/?를 정의 하는 경우</span><span class="sxs-lookup"><span data-stu-id="43e6a-119">For instance, currently if the indexing policy defines the composite index {Id/?</span></span> <span data-ttu-id="43e6a-120">ASC, TrackingNumber/?</span><span class="sxs-lookup"><span data-stu-id="43e6a-120">ASC, TrackingNumber/?</span></span> <span data-ttu-id="43e6a-121">ASC)}의 경우 ' ORDER BY c.Id ASC, ASC '가 있는 쿼리는 `"TrackingNumber"` 속성이 누락 된 항목 __을 반환 합니다__ .</span><span class="sxs-lookup"><span data-stu-id="43e6a-121">ASC)}, then a query the has 'ORDER BY c.Id ASC, c.Discriminator ASC' __would__ return items that are missing the `"TrackingNumber"` property.</span></span>
