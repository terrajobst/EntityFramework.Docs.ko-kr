---
title: Azure Cosmos DB 공급자 - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 28264681-4486-4891-888c-be5e4ade24f1
uid: core/providers/cosmos/index
ms.openlocfilehash: 683436aa485d2fef9aa8bf6c6ff02b00dfeb28cf
ms.sourcegitcommit: 2caec1e63f2ce1d9439ef6193df5a77da2fedd0f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317570"
---
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="eff31-102">EF Core Azure Cosmos DB 공급자</span><span class="sxs-lookup"><span data-stu-id="eff31-102">EF Core Azure Cosmos DB Provider</span></span>

>[!NOTE]
> <span data-ttu-id="eff31-103">이 공급자는 EF Core 3.0에서 새로 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-103">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="eff31-104">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 Azure Cosmos DB에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-104">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="eff31-105">공급자는 [Entity Framework Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="eff31-106">이 섹션을 읽기 전에 [Azure Cosmos DB 설명서](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction)를 숙지하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-106">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) before reading this section.</span></span>

## <a name="install"></a><span data-ttu-id="eff31-107">설치</span><span class="sxs-lookup"><span data-stu-id="eff31-107">Install</span></span>

<span data-ttu-id="eff31-108">[Microsoft.EntityFrameworkCore.Cosmos NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-108">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

## <a name="get-started"></a><span data-ttu-id="eff31-109">시작</span><span class="sxs-lookup"><span data-stu-id="eff31-109">Get Started</span></span>

> [!TIP]  
> <span data-ttu-id="eff31-110">[GitHub에서 이 문서의 샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-110">You can view this article's [sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="eff31-111">다른 공급자와 마찬가지로 첫 번째 단계는 `UseCosmos`를 호출하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-111">Like for other providers the first step is to call `UseCosmos`:</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> <span data-ttu-id="eff31-112">여기서는 간단하게 엔드포인트 및 키를 하드 코딩했지만 프로덕션 앱에서는 이들을 [안전하게 저장](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-112">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securily](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)</span></span>

<span data-ttu-id="eff31-113">이 예제에서 `Order`는 [소유된 형식](../../modeling/owned-entities.md) `StreetAddress`에 대한 참조를 포함하는 간단한 엔터티입니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-113">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="eff31-114">데이터 저장 및 쿼리는 일반적인 EF 패턴을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-114">Saving and quering data follows the normal EF pattern:</span></span>

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> <span data-ttu-id="eff31-115">필요한 컬렉션을 만들고 모델에 있는 경우 [시드 데이터](../../modeling/data-seeding.md)를 삽입하려면 `EnsureCreated`를 호출해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-115">Calling `EnsureCreated` is necessary to create the required collections and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="eff31-116">그러나 성능 문제가 발생할 수 있으므로 `EnsureCreated`는 정상 작업이 아닌 배포 중에만 호출해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-116">However `EnsureCreated` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="eff31-117">Cosmos 특정 모델 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="eff31-117">Cosmos-specific Model Customization</span></span>

<span data-ttu-id="eff31-118">기본적으로 모든 엔터티 형식은 파생 컨텍스트(이 경우 `"OrderContext"`)의 이름을 따서 동일한 컨테이너에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-118">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="eff31-119">기본 컨테이너 이름을 변경하려면 `HasDefaultContainer`를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-119">To change the default container name use `HasDefaultContainer`:</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="eff31-120">엔터티 형식을 다른 컨테이너에 매핑하려면 `ToContainer`를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-120">To map an entity type to a different container use `ToContainer`:</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="eff31-121">지정된 항목이 나타내는 엔터티 형식을 식별하기 위해 EF Core는 파생된 엔터티 형식이 없더라도 판별자 값을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-121">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="eff31-122">판별자의 이름과 값은 [변경할 수 있습니다](../../modeling/inheritance.md).</span><span class="sxs-lookup"><span data-stu-id="eff31-122">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

## <a name="embedded-entities"></a><span data-ttu-id="eff31-123">포함된 엔터티</span><span class="sxs-lookup"><span data-stu-id="eff31-123">Embedded Entities</span></span>

<span data-ttu-id="eff31-124">Cosmos의 경우 소유된 엔터티는 소유자와 동일한 항목에 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-124">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="eff31-125">속성 이름을 변경하려면 `ToJsonProperty`를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-125">To change a property name use `ToJsonProperty`:</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="eff31-126">이 구성을 사용하면 위 예제의 순서가 다음과 같이 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-126">With this configuration the order from the example above is stored like this:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "221 B Baker St"
    },
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-692e763901d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163674
}
```

<span data-ttu-id="eff31-127">소유된 엔터티의 컬렉션도 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-127">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="eff31-128">다음 예제에서는 `Distributor` 클래스를 `StreetAddress`의 컬렉션과 함께 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-128">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="eff31-129">소유된 엔터티는 저장할 명시적 키 값을 제공할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-129">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="eff31-130">해당 값은 다음과 같은 방식으로 유지됩니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-130">They will be persisted in this way:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Discriminator": "StreetAddress",
            "Street": "500 S 48th Street"
        },
        {
            "City": "Anaheim",
            "Discriminator": "StreetAddress",
            "Street": "5650 Dolly Ave"
        }
    ],
    "_rid": "6QEKANzISj0BAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKANzISj0=/docs/6QEKANzISj0BAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-7b2b439701d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163705
}
```

<span data-ttu-id="eff31-131">내부적으로 EF Core는 추적된 모든 엔터티에 대한 고유 키 값을 항상 가지고 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-131">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="eff31-132">소유된 형식의 컬렉션에 대해 기본적으로 생성되는 기본 키는 소유자를 가리키는 외래 키 속성과 JSON 배열의 인덱스에 해당하는 `int` 속성으로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-132">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="eff31-133">항목 API를 사용하여 다음 값을 검색할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-133">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="eff31-134">필요한 경우 소유된 엔터티 형식에 대한 기본 기본 키를 변경할 수 있지만 키 값을 명시적으로 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-134">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="eff31-135">연결이 끊긴 엔터티 사용</span><span class="sxs-lookup"><span data-stu-id="eff31-135">Working with Disconnected Entities</span></span>

<span data-ttu-id="eff31-136">모든 항목은 지정된 파티션 키에 고유한 `id` 값이 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-136">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="eff31-137">기본적으로 EF Core는 '|'를 구분 기호로 사용하여 판별자 및 기본 키 값을 연결하고 값을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-137">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="eff31-138">키 값은 엔터티가 `Added` 상태로 전환될 때만 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-138">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="eff31-139">이 경우 .NET 형식에 값을 저장할 `id` 속성이 없는 경우 [엔터티를 연결](../../saving/disconnected-entities.md)할 때 문제가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-139">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="eff31-140">이 제한 사항을 해결하려면 `id` 값을 수동으로 만들어 설정하거나 먼저 엔터티를 추가된 것으로 표시한 다음 원하는 상태로 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-140">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="eff31-141">결과 JSON은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="eff31-141">This is the resulting JSON:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "3 Abbey Road"
    },
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-8f7ac48f01d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163739
}
```
