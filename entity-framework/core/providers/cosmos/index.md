---
title: Azure Cosmos DB 공급자 - EF Core
description: Entity Framework Core를 Azure Cosmos DB SQL API에서 사용할 수 있도록 허용하는 데이터베이스 공급자에 관한 문서
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/index
ms.openlocfilehash: 162e5d387bcbfbf1e90baf27fc62dd2ed562ae58
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824543"
---
# <a name="ef-core-azure-cosmos-db-provider"></a>EF Core Azure Cosmos DB 공급자

> [!NOTE]
> 이 공급자는 EF Core 3.0에서 새로 제공됩니다.

이 데이터베이스 공급자를 설치하면 Entity Framework Core를 Azure Cosmos DB에서 사용할 수 있습니다. 공급자는 [Entity Framework Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore)의 일부로 유지 관리됩니다.

이 섹션을 읽기 전에 [Azure Cosmos DB 설명서](/azure/cosmos-db/introduction)를 숙지하는 것이 좋습니다.

> [!NOTE]
> 이 공급자는 Azure Cosmos DB의 SQL API에서만 작동합니다.

## <a name="install"></a>설치

[Microsoft.EntityFrameworkCore.Cosmos NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/)를 설치합니다.

## <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a>시작

> [!TIP]  
> [GitHub에서 이 문서의 샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos)을 볼 수 있습니다.

다른 공급자와 마찬가지로, 첫 번째 단계는 [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos)를 호출하는 것입니다.

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> 여기서는 간단하게 엔드포인트 및 키를 하드 코딩했지만 프로덕션 앱에서는 이들을 [안전하게 저장](/aspnet/core/security/app-secrets#secret-manager)해야 합니다.

이 예제에서 `Order`는 [소유된 형식](../../modeling/owned-entities.md) `StreetAddress`에 대한 참조를 포함하는 간단한 엔터티입니다.

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

데이터 저장 및 쿼리는 일반적인 EF 패턴을 따릅니다.

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> 필요한 컨테이너를 만들고 모델에 있는 경우 [시드 데이터](../../modeling/data-seeding.md)를 삽입하려면 [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync)를 호출해야 합니다. 그러나 성능 문제가 발생할 수 있으므로 `EnsureCreatedAsync`는 정상 작업이 아닌 배포 중에만 호출해야 합니다.

## <a name="cosmos-specific-model-customization"></a>Cosmos 특정 모델 사용자 지정

기본적으로 모든 엔터티 형식은 파생 컨텍스트(이 경우 `"OrderContext"`)의 이름을 따서 동일한 컨테이너에 매핑됩니다. 기본 컨테이너 이름을 변경하려면 [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer)를 사용합니다.

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

엔터티 형식을 다른 컨테이너에 매핑하려면 [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer)를 사용합니다.

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

지정된 항목이 나타내는 엔터티 형식을 식별하기 위해 EF Core는 파생된 엔터티 형식이 없더라도 판별자 값을 추가합니다. 판별자의 이름과 값은 [변경할 수 있습니다](../../modeling/inheritance.md).

다른 엔터티 형식이 동일한 컨테이너에 저장되지 않을 경우 [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator)를 호출하여 판별자를 제거할 수 있습니다.

[!code-csharp[NoDiscriminator](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=NoDiscriminator)]

### <a name="partition-keys"></a>파티션 키

기본적으로 EF Core는 항목을 삽입할 때 값을 제공하지 않고 파티션 키가 `"__partitionKey"`(으)로 설정된 컨테이너를 만듭니다. 그러나 Azure Cosmos의 성능을 완벽하게 활용하려면 [신중하게 선택한 파티션 키](/azure/cosmos-db/partition-data)를 사용해야 합니다. [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey)를 호출하여 구성할 수 있습니다.

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PartitionKey)]

> [!NOTE]
>파티션 키 속성은 [문자열로 변환](xref:core/modeling/value-conversions)되는 경우 모든 형식을 사용할 수 있습니다.

일단 구성된 경우, 파티션 키 속성에는 항상 null이 아닌 값이 있어야 합니다. 쿼리를 발행하는 경우 단일 파티션으로 만들 수 있는 조건을 추가할 수 있습니다.

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=PartitionKey)]

## <a name="embedded-entities"></a>포함된 엔터티

Cosmos의 경우 소유된 엔터티는 소유자와 동일한 항목에 포함됩니다. 속성 이름을 변경하려면 [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty)를 사용합니다.

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

이 구성을 사용하면 위 예제의 순서가 다음과 같이 저장됩니다.

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
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-692e763901d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163674
}
```

소유된 엔터티의 컬렉션도 포함됩니다. 다음 예제에서는 `Distributor` 클래스를 `StreetAddress`의 컬렉션과 함께 사용합니다.

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

소유된 엔터티는 저장할 명시적 키 값을 제공할 필요가 없습니다.

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

해당 값은 다음과 같은 방식으로 유지됩니다.

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Street": "500 S 48th Street"
        },
        {
            "City": "Anaheim",
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

내부적으로 EF Core는 추적된 모든 엔터티에 대한 고유 키 값을 항상 가지고 있어야 합니다. 소유된 형식의 컬렉션에 대해 기본적으로 생성되는 기본 키는 소유자를 가리키는 외래 키 속성과 JSON 배열의 인덱스에 해당하는 `int` 속성으로 구성됩니다. 항목 API를 사용하여 다음 값을 검색할 수 있습니다.

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> 필요한 경우 소유된 엔터티 형식에 대한 기본 기본 키를 변경할 수 있지만 키 값을 명시적으로 제공해야 합니다.

## <a name="working-with-disconnected-entities"></a>연결이 끊긴 엔터티 사용

모든 항목은 지정된 파티션 키에 고유한 `id` 값이 있어야 합니다. 기본적으로 EF Core는 '|'를 구분 기호로 사용하여 판별자 및 기본 키 값을 연결하고 값을 생성합니다. 키 값은 엔터티가 `Added` 상태로 전환될 때만 생성됩니다. 이 경우 .NET 형식에 값을 저장할 `id` 속성이 없는 경우 [엔터티를 연결](../../saving/disconnected-entities.md)할 때 문제가 발생할 수 있습니다.

이 제한 사항을 해결하려면 `id` 값을 수동으로 만들어 설정하거나 먼저 엔터티를 추가된 것으로 표시한 다음 원하는 상태로 변경할 수 있습니다.

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

결과 JSON은 다음과 같습니다.

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Street": "500 S 48th Street"
        }
    ],
    "_rid": "JBwtAN8oNYEBAAAAAAAAAA==",
    "_self": "dbs/JBwtAA==/colls/JBwtAN8oNYE=/docs/JBwtAN8oNYEBAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-9377-d7a1ae7c01d5\"",
    "_attachments": "attachments/",
    "_ts": 1572917100
}
```
