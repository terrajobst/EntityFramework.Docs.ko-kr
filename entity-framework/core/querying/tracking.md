---
title: 추적 및 비 추적 쿼리 - EF Core
author: smitpatel
ms.date: 10/10/2019
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: 66988f936ab75e17620398c8f21e4a32bbc950bd
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445947"
---
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="3f755-102">추적 및 비 추적 쿼리</span><span class="sxs-lookup"><span data-stu-id="3f755-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="3f755-103">추적 동작은 Entity Framework Core가 해당 변경 추적 장치에서 엔터티 인스턴스에 대한 정보를 유지할지 여부를 제어합니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-103">Tracking behavior controls if Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="3f755-104">엔터티를 추적하는 경우 엔터티에서 검색된 변경 내용은 `SaveChanges()` 중 데이터베이스에 유지됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="3f755-105">EF Core는 추적 쿼리 결과에 있는 엔터티와 변경 추적 장치에 있는 엔터티 간 탐색 속성도 수정합니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-105">EF Core will also fix up navigation properties between the entities in a tracking query result and the entities that are in the change tracker.</span></span>

> [!NOTE]
> <span data-ttu-id="3f755-106">[키 없는 엔터티 형식](xref:core/modeling/keyless-entity-types)은 추적되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-106">[Keyless entity types](xref:core/modeling/keyless-entity-types) are never tracked.</span></span> <span data-ttu-id="3f755-107">이 문서에서 언급되는 모든 엔터티 형식은 키가 정의된 엔터티 형식을 가리킵니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-107">Wherever this article mentions entity types, it refers to entity types which have a key defined.</span></span>

> [!TIP]  
> <span data-ttu-id="3f755-108">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="3f755-109">추적 쿼리</span><span class="sxs-lookup"><span data-stu-id="3f755-109">Tracking queries</span></span>

<span data-ttu-id="3f755-110">기본적으로 엔터티 형식을 반환하는 쿼리는 추적됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-110">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="3f755-111">즉, 해당 엔터티 인스턴스를 변경하고 `SaveChanges()`에서 해당 변경 내용을 유지하도록 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-111">Which means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span> <span data-ttu-id="3f755-112">다음 예제에서는 `SaveChanges()` 중에 블로그 등급 변경을 검색하고 데이터베이스에 유지합니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-112">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#Tracking)]

## <a name="no-tracking-queries"></a><span data-ttu-id="3f755-113">비 추적 쿼리</span><span class="sxs-lookup"><span data-stu-id="3f755-113">No-tracking queries</span></span>

<span data-ttu-id="3f755-114">비 추적 쿼리는 읽기 전용 시나리오에서 결과가 사용되는 경우에 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-114">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="3f755-115">이 쿼리는 변경 내용 추적 정보를 설정할 필요가 없기 때문에 더 빠르게 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-115">They're quicker to execute because there's no need to set up the change tracking information.</span></span> <span data-ttu-id="3f755-116">데이터베이스에서 검색된 엔터티를 업데이트할 필요가 없는 경우 비 추적 쿼리를 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-116">If you don't need to update the entities retrieved from the database, then a no-tracking query should be used.</span></span> <span data-ttu-id="3f755-117">개별 쿼리를 비 추적 쿼리로 전환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-117">You can swap an individual query to be no-tracking.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#NoTracking)]

<span data-ttu-id="3f755-118">컨텍스트 인스턴스 수준에서 기본 추적 동작을 변경할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-118">You can also change the default tracking behavior at the context instance level:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ContextDefaultTrackingBehavior)]

## <a name="identity-resolution"></a><span data-ttu-id="3f755-119">ID 확인</span><span class="sxs-lookup"><span data-stu-id="3f755-119">Identity resolution</span></span>

<span data-ttu-id="3f755-120">추적 쿼리는 변경 추적 장치를 사용하므로 EF Core는 추적 쿼리에서 ID 확인을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-120">Since a tracking query uses the change tracker, EF Core will do identity resolution in a tracking query.</span></span> <span data-ttu-id="3f755-121">엔터티를 구체화할 때 엔터티 인스턴스가 이미 추적 중인 경우 EF Core는 변경 추적기에서 동일한 엔터티 인스턴스를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-121">When materializing an entity, EF Core will return the same entity instance from the change tracker if it's already being tracked.</span></span> <span data-ttu-id="3f755-122">결과에 동일한 엔터티가 여러 번 포함되는 경우 발생할 때마다 동일한 인스턴스를 다시 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-122">If the result contains same entity multiple times, you get back same instance for each occurrence.</span></span> <span data-ttu-id="3f755-123">비 추적 쿼리는 변경 추적 장치를 사용하지 않으며 ID 확인을 수행하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-123">No-tracking queries don't use the change tracker and don't do identity resolution.</span></span> <span data-ttu-id="3f755-124">따라서 동일한 엔터티가 결과에 여러 번 포함되는 경우에도 엔터티의 새 인스턴스를 다시 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-124">So you get back new instance of entity even when the same entity is contained in the result multiple times.</span></span> <span data-ttu-id="3f755-125">이 동작은 EF Core 3.0 이전 버전과는 다릅니다. [이전 버전](#previous-versions)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="3f755-125">This behavior was different in versions before EF Core 3.0, see [previous versions](#previous-versions).</span></span>

## <a name="tracking-and-custom-projections"></a><span data-ttu-id="3f755-126">추적 및 사용자 지정 프로젝션</span><span class="sxs-lookup"><span data-stu-id="3f755-126">Tracking and custom projections</span></span>

<span data-ttu-id="3f755-127">쿼리의 결과 형식이 엔터티 형식이 아닌 경우에도 EF Core는 결과에 포함된 엔터티 형식을 기본적으로 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-127">Even if the result type of the query isn't an entity type, EF Core will still track entity types contained in the result by default.</span></span> <span data-ttu-id="3f755-128">무명 형식을 반환하는 다음 쿼리에서는 결과 집합에 있는 `Blog`의 인스턴스가 추적됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-128">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection1)]

<span data-ttu-id="3f755-129">결과 집합에 LINQ 컴퍼지션에서 나오는 엔터티 형식이 포함되어 있으면 EF Core가 이를 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-129">If the result set contains entity types coming out from LINQ composition, EF Core will track them.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

<span data-ttu-id="3f755-130">결과 집합에 포함된 엔터티 형식이 없으면 추적이 수행되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-130">If the result set doesn't contain any entity types, then no tracking is done.</span></span> <span data-ttu-id="3f755-131">다음 쿼리에서는 엔터티의 값 일부가 있는 무명 형식을 반환합니다(하지만 실제 엔터티 형식의 인스턴스는 반환하지 않습니다).</span><span class="sxs-lookup"><span data-stu-id="3f755-131">In the following query, we return an anonymous type with some of the values from the entity (but no instances of the actual entity type).</span></span> <span data-ttu-id="3f755-132">쿼리에서 나오는 추적되는 엔터티는 없습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-132">There are no tracked entities coming out of the query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection3)]

 <span data-ttu-id="3f755-133">EF Core는 최상위 프로젝션에서의 클라이언트 평가 수행을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-133">EF Core supports doing client evaluation in the top-level projection.</span></span> <span data-ttu-id="3f755-134">EF Core가 클라이언트 평가를 위해 엔터티 인스턴스를 구체화하는 경우 이 엔터티 인스턴스가 추적됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-134">If EF Core materializes an entity instance for client evaluation, it will be tracked.</span></span> <span data-ttu-id="3f755-135">여기서는 클라이언트 메서드 `StandardizeURL`에 `blog` 엔터티를 전달하므로 EF Core는 블로그 인스턴스도 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-135">Here, since we're passing `blog` entities to the client method `StandardizeURL`, EF Core will track the blog instances too.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientMethod)]

<span data-ttu-id="3f755-136">EF Core는 결과에 포함된 키 없는 엔터티 인스턴스를 추적하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-136">EF Core doesn't track the keyless entity instances contained in the result.</span></span> <span data-ttu-id="3f755-137">그러나 EF Core는 위의 규칙에 따라 키가 있는 엔터티 형식의 다른 인스턴스를 모두 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-137">But EF Core tracks all the other instances of entity types with key according to rules above.</span></span>

<span data-ttu-id="3f755-138">위의 규칙 일부는 EF Core 3.0 이전에는 다르게 작동했습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-138">Some of the above rules worked differently before EF Core 3.0.</span></span> <span data-ttu-id="3f755-139">자세한 내용은 [이전 버전](#previous-versions)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="3f755-139">For more information, see [previous versions](#previous-versions).</span></span>

## <a name="previous-versions"></a><span data-ttu-id="3f755-140">이전 버전</span><span class="sxs-lookup"><span data-stu-id="3f755-140">Previous versions</span></span>

<span data-ttu-id="3f755-141">버전 3.0 이전에는 EF Core의 추적 수행 방법에 약간의 차이가 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-141">Before version 3.0, EF Core had some differences in how tracking was done.</span></span> <span data-ttu-id="3f755-142">주목할 만한 차이점은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-142">Notable differences are as follows:</span></span>

- <span data-ttu-id="3f755-143">[클라이언트 및 서버 평가](xref:core/querying/client-eval) 페이지에 설명한 것처럼 버전 3.0 이전에는 EF Core가 쿼리의 모든 부분에서 클라이언트 평가를 지원했습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-143">As explained in [Client vs Server Evaluation](xref:core/querying/client-eval) page, EF Core supported client evaluation in any part of the query before version 3.0.</span></span> <span data-ttu-id="3f755-144">클라이언트 평가로 인해 결과에 포함되지 않는 엔터티의 구체화가 수행되었습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-144">Client evaluation caused materialization of entities, which weren't part of the result.</span></span> <span data-ttu-id="3f755-145">따라서 EF Core는 결과를 분석하여 추적할 항목을 검색했습니다. 이 설계에는 다음과 같은 몇 가지 차이점이 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-145">So EF Core analyzed the result to detect what to track. This design had certain differences as follows:</span></span>
  - <span data-ttu-id="3f755-146">구체화를 초래했지만 구체화된 엔터티 인스턴스를 반환하지 않은 프로젝션의 클라이언트 평가는 추적되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-146">Client evaluation in the projection, which caused materialization but didn't return the materialized entity instance wasn't tracked.</span></span> <span data-ttu-id="3f755-147">다음 예는 `blog` 엔터티를 추적하지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-147">The following example didn't track `blog` entities.</span></span>
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

  - <span data-ttu-id="3f755-148">EF Core는 특정한 경우에 LINQ 컴퍼지션에서 나오는 개체를 추적하지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-148">EF Core didn't track the objects coming out of LINQ composition in certain cases.</span></span> <span data-ttu-id="3f755-149">다음 예는 `Post`를 추적하지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-149">The following example didn't track `Post`.</span></span>
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

- <span data-ttu-id="3f755-150">쿼리 결과에 키 없는 엔터티 형식이 포함될 때마다 전체 쿼리가 비 추적 쿼리로 전환되었습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-150">Whenever query results contained keyless entity types, the whole query was made non-tracking.</span></span> <span data-ttu-id="3f755-151">즉, 키가 있고 결과에 포함된 엔터티 형식도 추적되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-151">That means that entity types with keys, which are in result weren't being tracked either.</span></span>
- <span data-ttu-id="3f755-152">EF Core는 비 추적 쿼리에서 ID 확인을 수행했습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-152">EF Core did identity resolution in no-tracking query.</span></span> <span data-ttu-id="3f755-153">EF Core는 약한 참조를 사용하여 이미 반환된 엔터티를 추적했습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-153">It used weak references to keep track of entities that had already been returned.</span></span> <span data-ttu-id="3f755-154">따라서 결과 집합에 동일한 엔터티가 여러 번 포함된 경우 발생할 때마다 동일한 인스턴스를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-154">So if a result set contained the same entity multiples times, you would get the same instance for each occurrence.</span></span> <span data-ttu-id="3f755-155">ID가 동일한 이전 결과가 범위를 벗어나고 가비지가 수집된 경우에도 EF Core는 새 인스턴스를 반환했습니다.</span><span class="sxs-lookup"><span data-stu-id="3f755-155">Though if a previous result with the same identity went out of scope and got garbage collected, EF Core returned a new instance.</span></span>
