---
title: Azure Cosmos DB 공급자-제한 EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 9d02a2cd-484e-4687-b8a8-3748ba46dbc9
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 8dcc82a68c89e21ad1902a0bbbce8ebbc3535801
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150766"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a><span data-ttu-id="93115-102">EF Core Azure Cosmos DB 공급자 제한 사항</span><span class="sxs-lookup"><span data-stu-id="93115-102">EF Core Azure Cosmos DB Provider Limitations</span></span>

<span data-ttu-id="93115-103">Cosmos 공급자에는 몇 가지 제한 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93115-103">The Cosmos provider has a number of limitations.</span></span> <span data-ttu-id="93115-104">이러한 제한 사항 중 상당수는 기본 Cosmos 데이터베이스 엔진에 대 한 제한의 결과 이며 EF에 국한 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="93115-104">Many of these limitations are a result of limitations in the underlying Cosmos database engine and are not specific to EF.</span></span> <span data-ttu-id="93115-105">그러나 대부분은 [아직 구현 되지 않았습니다](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="93115-105">But most simply [haven't been implemented yet](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span></span>

## <a name="temporary-limitations"></a><span data-ttu-id="93115-106">임시 제한 사항</span><span class="sxs-lookup"><span data-stu-id="93115-106">Temporary limitations</span></span>

- <span data-ttu-id="93115-107">컨테이너에 매핑되는 상속이 없는 엔터티 형식이 하나만 있는 경우에도 판별자 속성이 계속 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="93115-107">Even if there is only one entity type without inheritance mapped to a container it still has a discriminator property.</span></span>
- <span data-ttu-id="93115-108">파티션 키가 있는 엔터티 형식은 일부 시나리오에서 제대로 작동 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="93115-108">Entity types with partition keys don't work correctly in some scenarios</span></span>
- <span data-ttu-id="93115-109">`Include`호출이 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="93115-109">`Include` calls are not supported</span></span>
- <span data-ttu-id="93115-110">`Join`호출이 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="93115-110">`Join` calls are not supported</span></span>

## <a name="azure-cosmos-db-sdk-limitations"></a><span data-ttu-id="93115-111">Azure Cosmos DB SDK 제한 사항</span><span class="sxs-lookup"><span data-stu-id="93115-111">Azure Cosmos DB SDK limitations</span></span>

- <span data-ttu-id="93115-112">비동기 메서드만 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="93115-112">Only async methods are provided</span></span>

> [!WARNING]
> <span data-ttu-id="93115-113">에서 사용 하 EF Core 하위 수준 메서드의 동기화 버전은 없으므로 반환 `.Wait()` `Task`된에서를 호출 하 여 해당 기능을 현재 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="93115-113">Since there are no sync versions of the low level methods EF Core relies on, the corresponding functionality is currently implemented by calling `.Wait()` on the returned `Task`.</span></span> <span data-ttu-id="93115-114">즉,와 같은 `SaveChanges`메서드를 사용 하거나 `ToList` 비동기 항목 대신 메서드를 사용 하면 응용 프로그램에서 교착 상태가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93115-114">This means that using methods like `SaveChanges`, or `ToList` instead of their async counterparts could lead to a deadlock in your application</span></span>

## <a name="azure-cosmos-db-limitations"></a><span data-ttu-id="93115-115">Azure Cosmos DB 제한 사항</span><span class="sxs-lookup"><span data-stu-id="93115-115">Azure Cosmos DB limitations</span></span>

<span data-ttu-id="93115-116">[지원 되는 Azure Cosmos DB 기능](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data)에 대 한 전체 개요를 볼 수 있으며, 관계형 데이터베이스에 비해 가장 주목할 만한 차이점은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="93115-116">You can see the full overview of [Azure Cosmos DB supported features](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data), these are the most notable differences compared to a relational database:</span></span>

- <span data-ttu-id="93115-117">클라이언트에서 시작한 트랜잭션은 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="93115-117">Client-initiated transactions are not supported</span></span>
- <span data-ttu-id="93115-118">일부 파티션 간 쿼리는 관련 연산자에 따라 지원 되지 않거나 훨씬 느립니다.</span><span class="sxs-lookup"><span data-stu-id="93115-118">Some cross-partition queries are either not supported or much slower depending on the operators involved</span></span>