---
title: 연결이 끊긴 엔터티 - EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
uid: core/saving/disconnected-entities
ms.openlocfilehash: 421531e68ac98c0553938f1c24892701f22fef3c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413656"
---
# <a name="disconnected-entities"></a><span data-ttu-id="25f4a-102">연결이 끊긴 엔터티</span><span class="sxs-lookup"><span data-stu-id="25f4a-102">Disconnected entities</span></span>

<span data-ttu-id="25f4a-103">DbContext 인스턴스는 데이터베이스에서 반환된 엔터티를 자동으로 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="25f4a-104">이러한 엔터티의 변경 내용은 SaveChanges를 호출하고 필요에 따라 데이터베이스를 업데이트할 때 감지됩니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="25f4a-105">자세한 내용은 [기본 저장](basic.md) 및 [관련 데이터](related-data.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="25f4a-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="25f4a-106">그러나 하나의 컨텍스트 인스턴스를 사용하여 엔터티를 쿼리하고 다른 인스턴스를 사용하여 저장하는 경우가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="25f4a-107">이런 상황은 엔터티를 쿼리하고, 클라이언트로 전송하고, 수정하고, 요청에서 서버로 다시 전송한 후 저장하는 웹 애플리케이션과 같이 “연결이 끊긴” 시나리오에서 종종 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="25f4a-108">이 경우 두 번째 컨텍스트 인스턴스는 엔터티가 삽입해야 하는 새 엔터티인지 업데이트해야 하는 기존 엔터티인지를 알고 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

<!-- markdownlint-disable MD028 -->
> [!TIP]
> <span data-ttu-id="25f4a-109">GitHub에서 이 문서의 [샘플](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-109">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) on GitHub.</span></span>

> [!TIP]
> <span data-ttu-id="25f4a-110">EF Core는 지정된 기본 키 값이 있는 엔터티의 인스턴스 하나만 추적할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-110">EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="25f4a-111">문제가 되지 않도록 하는 가장 좋은 방법은 각 작업 단위에 대해 단기 컨텍스트를 사용하여 컨텍스트가 빈 상태로 시작되고 엔터티를 연결하고 해당 엔터티를 저장한 후 컨텍스트가 삭제되고 취소되도록 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-111">The best way to avoid this being an issue is to use a short-lived context for each unit-of-work such that the context starts empty, has entities attached to it, saves those entities, and then the context is disposed and discarded.</span></span>
<!-- markdownlint-enable MD028 -->

## <a name="identifying-new-entities"></a><span data-ttu-id="25f4a-112">새 엔터티 식별</span><span class="sxs-lookup"><span data-stu-id="25f4a-112">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="25f4a-113">클라이언트에서 새 엔터티 식별</span><span class="sxs-lookup"><span data-stu-id="25f4a-113">Client identifies new entities</span></span>

<span data-ttu-id="25f4a-114">처리할 가장 간단한 경우는 새 엔터티인지 기존 엔터티인지 여부를 클라이언트가 서버에 알리는 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-114">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="25f4a-115">예를 들어 새 엔터티를 삽입하는 요청은 기존 엔터티를 업데이트하는 요청과 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-115">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="25f4a-116">이 섹션의 나머지 부분에서는 삽입할지 업데이트할지를 다른 방법으로 확인해야 하는 경우에 대해 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-116">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="25f4a-117">자동 생성 키 사용</span><span class="sxs-lookup"><span data-stu-id="25f4a-117">With auto-generated keys</span></span>

<span data-ttu-id="25f4a-118">종종 자동으로 생성된 키 값을 사용하여 엔터티를 삽입해야 하는지 업데이트해야 하는지를 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-118">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="25f4a-119">키가 설정되지 않은 경우(즉, 아직 null, 0 등의 CLR 기본값이 있는 경우) 새 엔터티여야 하며 삽입해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-119">If the key has not been set (that is, it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="25f4a-120">반면 키 값이 설정된 경우 이전에 이미 저장되었으므로 업데이트해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-120">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="25f4a-121">즉, 키에 값이 있으면 엔터티를 쿼리하고 클라이언트로 전송했으며 이제 다시 돌아가 업데이트해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-121">In other words, if the key has a value, then the entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="25f4a-122">엔터티 형식을 알고 있으면 설정 해제된 키를 쉽게 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-122">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="25f4a-123">그러나 EF에는 엔터티 형식 및 키 형식에 따라 이 작업을 수행하는 기본 제공 방법도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-123">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="25f4a-124">엔터티가 Added 상태인 경우에도 키는 컨텍스트에서 엔터티를 추적하는 즉시 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-124">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="25f4a-125">따라서 TrackGraph API를 사용하는 경우와 같이 엔터티 그래프를 트래버스하고 각각에 대해 수행할 작업을 결정할 때 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-125">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="25f4a-126">키 값은 엔터티를 추적하기 위해 호출하기 ‘전에’ 여기에 표시된 방법으로만 사용해야 합니다. </span><span class="sxs-lookup"><span data-stu-id="25f4a-126">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="25f4a-127">다른 키 사용</span><span class="sxs-lookup"><span data-stu-id="25f4a-127">With other keys</span></span>

<span data-ttu-id="25f4a-128">키 값이 자동으로 생성되지 않는 경우 몇 가지 다른 메커니즘으로 새 엔터티를 식별해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-128">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="25f4a-129">두 가지 일반적인 접근 방법이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-129">There are two general approaches to this:</span></span>

* <span data-ttu-id="25f4a-130">엔터티 쿼리</span><span class="sxs-lookup"><span data-stu-id="25f4a-130">Query for the entity</span></span>
* <span data-ttu-id="25f4a-131">클라이언트에서 플래그 전달</span><span class="sxs-lookup"><span data-stu-id="25f4a-131">Pass a flag from the client</span></span>

<span data-ttu-id="25f4a-132">엔터티를 쿼리하려면 Find 메서드를 사용하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-132">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="25f4a-133">클라이언트에서 플래그를 전달하는 전체 코드를 보여주는 것은 이 문서의 범위를 벗어납니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-133">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="25f4a-134">웹앱에서는 일반적으로 작업에 따라 다른 요청을 하거나, 요청에서 몇 가지 상태를 전달한 후 컨트롤러에서 추출합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-134">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="25f4a-135">단일 엔터티 저장</span><span class="sxs-lookup"><span data-stu-id="25f4a-135">Saving single entities</span></span>

<span data-ttu-id="25f4a-136">삽입이 필요한지 업데이트가 필요한지를 알고 있는 경우 Add 또는 Update를 적절하게 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-136">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="25f4a-137">그러나 엔터티에서 자동 생성 키 값을 사용하는 경우에는 두 경우 모두에 Update 메서드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-137">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="25f4a-138">Update 메서드는 일반적으로 엔터티를 삽입이 아닌 업데이트하도록 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-138">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="25f4a-139">그러나 엔터티에 자동 생성 키가 있고 키 값이 설정되지 않은 경우 엔터티는 자동으로 삽입하도록 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-139">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="25f4a-140">이 동작은 EF Core 2.0에서 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-140">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="25f4a-141">이전 릴리스에서는 항상 Add 또는 Update를 명시적으로 선택해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-141">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="25f4a-142">엔터티에서 자동 생성 키를 사용하지 않는 경우 애플리케이션에서 엔터티를 삽입해야 하는지 업데이트해야 하는지를 결정해야 합니다. 예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-142">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="25f4a-143">단계는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-143">The steps here are:</span></span>

* <span data-ttu-id="25f4a-144">Find에서 null을 반환하는 경우 데이터베이스에 이 ID를 사용하는 블로그가 아직 포함되어 있지 않으므로 Add를 호출하여 삽입하도록 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-144">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="25f4a-145">Find에서 엔터티를 반환하는 경우 엔터티가 데이터베이스에 있고 컨텍스트에서 기존 엔터티를 추적하고 있는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-145">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="25f4a-146">그러면 SetValues를 사용하여 이 엔터티의 모든 속성에 대한 값을 클라이언트에서 가져온 값으로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-146">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="25f4a-147">SetValues 호출에서 필요에 따라 업데이트하도록 엔터티를 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-147">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="25f4a-148">SetValues는 추적된 엔터티의 값과 다른 값을 갖는 속성만 수정된 것으로 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-148">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="25f4a-149">즉, 업데이트가 전송되는 경우 실제로 변경된 열만 업데이트됩니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-149">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="25f4a-150">또한 변경된 내용이 없으면 업데이트가 전송되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-150">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="25f4a-151">그래프 작업</span><span class="sxs-lookup"><span data-stu-id="25f4a-151">Working with graphs</span></span>

### <a name="identity-resolution"></a><span data-ttu-id="25f4a-152">ID 확인</span><span class="sxs-lookup"><span data-stu-id="25f4a-152">Identity resolution</span></span>

<span data-ttu-id="25f4a-153">위에서 설명한 대로 EF Core는 지정된 기본 키 값이 있는 엔터티의 인스턴스 하나만 추적할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-153">As noted above, EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="25f4a-154">그래프로 작업할 때 이 고정 조건이 유지되도록 이상적으로 그래프를 만들어야 하며 컨텍스트는 하나의 작업 단위에만 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-154">When working with graphs the graph should ideally be created such that this invariant is maintained, and the context should be used for only one unit-of-work.</span></span> <span data-ttu-id="25f4a-155">그래프에 중복이 포함되어 있는 경우 EF로 보내기 전에 그래프를 처리하여 여러 인스턴스를 하나로 통합해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-155">If the graph does contain duplicates, then it will be necessary to process the graph before sending it to EF to consolidate multiple instances into one.</span></span> <span data-ttu-id="25f4a-156">인스턴스에 충돌하는 값과 관계가 있는 경우 간단하지 않을 수 있으므로 충돌 해결을 피하려면 애플리케이션 파이프라인에서 최대한 빨리 중복 통합을 수행해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-156">This may not be trivial where instances have conflicting values and relationships, so consolidating duplicates should be done as soon as possible in your application pipeline to avoid conflict resolution.</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="25f4a-157">모든 새 엔터티/모든 기존 엔터티</span><span class="sxs-lookup"><span data-stu-id="25f4a-157">All new/all existing entities</span></span>

<span data-ttu-id="25f4a-158">그래프 작업의 예로는 연관된 게시물 컬렉션과 함께 블로그를 삽입하거나 업데이트하는 작업이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-158">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="25f4a-159">그래프의 모든 엔터티를 삽입해야 하거나 모두 업데이트해야 하는 경우 프로세스는 위에서 단일 엔터티에 대해 설명한 것과 동일합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-159">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="25f4a-160">예를 들어 다음과 같이 블로그 및 게시물 그래프를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-160">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="25f4a-161">다음과 같이 삽입할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-161">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="25f4a-162">Add를 호출하여 블로그 및 모든 게시물이 삽입되도록 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-162">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="25f4a-163">마찬가지로 그래프의 모든 엔터티를 업데이트해야 하는 경우 Update를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-163">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="25f4a-164">블로그 및 모든 게시물이 업데이트되도록 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-164">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="25f4a-165">새 엔터티와 기존 엔터티의 혼합</span><span class="sxs-lookup"><span data-stu-id="25f4a-165">Mix of new and existing entities</span></span>

<span data-ttu-id="25f4a-166">자동 생성 키를 사용하면 그래프에 삽입할 엔터티와 업데이트할 엔터티가 혼합되어 있는 경우에도 삽입과 업데이트 모두에 Update를 다시 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-166">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="25f4a-167">Update는 키 값 집합이 없는 경우 그래프의 모든 엔터티인 블로그 또는 게시물을 삽입하도록 표시하고 다른 모든 엔터티는 업데이트하도록 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-167">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="25f4a-168">앞에서처럼 자동 생성 키를 사용하지 않는 경우 쿼리 및 일부 처리를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-168">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="25f4a-169">삭제 처리</span><span class="sxs-lookup"><span data-stu-id="25f4a-169">Handling deletes</span></span>

<span data-ttu-id="25f4a-170">엔터티가 없으면 삭제되어야 함을 의미하기 때문에 삭제는 처리하기가 어려울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-170">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="25f4a-171">삭제를 처리하는 한 가지 방법은 엔터티가 실제로 삭제되는 것이 아니라 삭제됨으로 표시되도록 “일시 삭제”를 사용하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-171">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="25f4a-172">그러면 삭제가 업데이트와 동일하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-172">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="25f4a-173">일시 삭제는 [쿼리 필터](xref:core/querying/filters)를 사용하여 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-173">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="25f4a-174">실제 삭제의 경우 일반적인 패턴은 쿼리 패턴의 확장을 사용하여 기본적으로 그래프 차이점을 수행하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-174">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff.</span></span> <span data-ttu-id="25f4a-175">예들 들어 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-175">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="25f4a-176">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="25f4a-176">TrackGraph</span></span>

<span data-ttu-id="25f4a-177">내부적으로 Add, Attach 및 Update는 그래프 통과를 사용하여 각 엔터티에 대해 추가됨(삽입), 수정됨(업데이트), 변경되지 않음(아무 작업도 하지 않음) 또는 삭제됨(삭제)으로 표시할지 여부를 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-177">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="25f4a-178">이 메커니즘은 TrackGraph API를 통해 노출됩니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-178">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="25f4a-179">예를 들어 클라이언트가 엔터티의 그래프를 다시 전송할 때 각 엔터티에 대해 처리되어야 하는 방법을 나타내는 몇 가지 플래그를 설정한다고 가정해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-179">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="25f4a-180">그런 다음, TrackGraph를 사용하여 이 플래그를 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-180">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="25f4a-181">플래그는 예제를 간단하게 나타내기 위해 엔터티의 일부로만 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-181">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="25f4a-182">일반적으로 플래그는 요청에 포함된 DTO 또는 다른 몇 가지 상태의 일부가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="25f4a-182">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
