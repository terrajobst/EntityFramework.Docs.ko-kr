---
title: Azure Cosmos DB 공급자-제한 EF Core
description: Entity Framework Core Azure Cosmos DB 공급자의 제한 사항
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 2631526b152d6ddcacf25173c8d51e4e3cb24500
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414562"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a><span data-ttu-id="5cd22-103">EF Core Azure Cosmos DB 공급자 제한 사항</span><span class="sxs-lookup"><span data-stu-id="5cd22-103">EF Core Azure Cosmos DB Provider Limitations</span></span>

<span data-ttu-id="5cd22-104">Cosmos 공급자에는 몇 가지 제한 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5cd22-104">The Cosmos provider has a number of limitations.</span></span> <span data-ttu-id="5cd22-105">이러한 제한 사항 중 상당수는 기본 Cosmos 데이터베이스 엔진에 대 한 제한의 결과 이며 EF에 국한 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5cd22-105">Many of these limitations are a result of limitations in the underlying Cosmos database engine and are not specific to EF.</span></span> <span data-ttu-id="5cd22-106">그러나 대부분은 [아직 구현 되지 않았습니다](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="5cd22-106">But most simply [haven't been implemented yet](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span></span>

## <a name="temporary-limitations"></a><span data-ttu-id="5cd22-107">임시 제한 사항</span><span class="sxs-lookup"><span data-stu-id="5cd22-107">Temporary limitations</span></span>

- <span data-ttu-id="5cd22-108">컨테이너에 매핑되는 상속이 없는 엔터티 형식이 하나만 있는 경우에도 판별자 속성이 계속 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5cd22-108">Even if there is only one entity type without inheritance mapped to a container it still has a discriminator property.</span></span>
- <span data-ttu-id="5cd22-109">파티션 키가 있는 엔터티 형식은 일부 시나리오에서 제대로 작동 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5cd22-109">Entity types with partition keys don't work correctly in some scenarios</span></span>
- <span data-ttu-id="5cd22-110">`Include` 호출은 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5cd22-110">`Include` calls are not supported</span></span>
- <span data-ttu-id="5cd22-111">`Join` 호출은 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5cd22-111">`Join` calls are not supported</span></span>

## <a name="azure-cosmos-db-sdk-limitations"></a><span data-ttu-id="5cd22-112">Azure Cosmos DB SDK 제한 사항</span><span class="sxs-lookup"><span data-stu-id="5cd22-112">Azure Cosmos DB SDK limitations</span></span>

- <span data-ttu-id="5cd22-113">비동기 메서드만 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5cd22-113">Only async methods are provided</span></span>

> [!WARNING]
> <span data-ttu-id="5cd22-114">에서 사용 하 EF Core 하위 수준 메서드의 동기화 버전은 없으므로 반환 된 `Task`에 대 한 `.Wait()`를 호출 하 여 해당 기능을 현재 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="5cd22-114">Since there are no sync versions of the low level methods EF Core relies on, the corresponding functionality is currently implemented by calling `.Wait()` on the returned `Task`.</span></span> <span data-ttu-id="5cd22-115">즉, `SaveChanges`와 같은 메서드를 사용 하거나 비동기 항목 대신 `ToList`를 사용 하면 응용 프로그램에서 교착 상태가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5cd22-115">This means that using methods like `SaveChanges`, or `ToList` instead of their async counterparts could lead to a deadlock in your application</span></span>

## <a name="azure-cosmos-db-limitations"></a><span data-ttu-id="5cd22-116">Azure Cosmos DB 제한 사항</span><span class="sxs-lookup"><span data-stu-id="5cd22-116">Azure Cosmos DB limitations</span></span>

<span data-ttu-id="5cd22-117">[지원 되는 Azure Cosmos DB 기능](/azure/cosmos-db/modeling-data)에 대 한 전체 개요를 볼 수 있으며, 관계형 데이터베이스에 비해 가장 주목할 만한 차이점은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="5cd22-117">You can see the full overview of [Azure Cosmos DB supported features](/azure/cosmos-db/modeling-data), these are the most notable differences compared to a relational database:</span></span>

- <span data-ttu-id="5cd22-118">클라이언트에서 시작한 트랜잭션은 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5cd22-118">Client-initiated transactions are not supported</span></span>
- <span data-ttu-id="5cd22-119">일부 파티션 간 쿼리는 관련 연산자에 따라 지원 되지 않거나 훨씬 느립니다.</span><span class="sxs-lookup"><span data-stu-id="5cd22-119">Some cross-partition queries are either not supported or much slower depending on the operators involved</span></span>
