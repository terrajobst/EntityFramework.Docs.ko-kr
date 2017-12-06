---
title: "연결이 끊긴된 엔터티-EF 코어"
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: b9d9662ce277e4f7b3d6f997a5117a0592f59fa3
ms.sourcegitcommit: c72d85805db0aa95f980514a18381fdc5e17c786
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/01/2017
---
# <a name="disconnected-entities"></a><span data-ttu-id="7d7bd-102">연결이 끊긴된 엔터티</span><span class="sxs-lookup"><span data-stu-id="7d7bd-102">Disconnected entities</span></span>

<span data-ttu-id="7d7bd-103">DbContext 인스턴스는 자동으로 데이터베이스에서 반환 되는 엔터티를 추적 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="7d7bd-104">이러한 엔터티를 변경 하는 다음 검색 SaveChanges가 호출 하 고 필요에 따라 데이터베이스 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="7d7bd-105">참조 [기본 저장](basic.md) 및 [관련 데이터](related-data.md) 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="7d7bd-106">그러나 경우에 따라 엔터티 쿼리 컨텍스트 인스턴스를 사용 하 고 다음 다른 인스턴스를 사용 하 여를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="7d7bd-107">이 엔터티는 쿼리, 클라이언트에 전송, 수정, 요청에서 서버에 다시 보내고 위치 저장 한 다음 웹 응용 프로그램과 같은 "연결이 끊긴된 된" 시나리오에서 자주 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="7d7bd-108">이 경우 두 번째 컨텍스트 엔터티는 새 있는지 여부를 알아야 (삽입할지) 요구 사항이 나 (업데이트할지) 기존 인스턴스.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="7d7bd-109">이 문서를 볼 수 있습니다 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) GitHub에서 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) on GitHub.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="7d7bd-110">새 엔터티를 식별합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-110">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="7d7bd-111">새 엔터티를 식별 하는 클라이언트</span><span class="sxs-lookup"><span data-stu-id="7d7bd-111">Client identifies new entities</span></span>

<span data-ttu-id="7d7bd-112">클라이언트가 서버에 알리기 때문 기존 또는 새 엔터티가 인지 때 다루기가 가장 간단 하 게 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-112">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="7d7bd-113">예를 들어 자주 새 엔터티를 삽입 하는 요청 간에 차이가 있는 기존 엔터티 업데이트 요청을 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-113">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="7d7bd-114">이 섹션의 나머지 부분에서는 사례를 설명에 다른 방법으로 삽입 하거나 업데이트할 것인지 결정 하는 데 필요한 것입니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-114">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="7d7bd-115">자동 생성 키 사용</span><span class="sxs-lookup"><span data-stu-id="7d7bd-115">With auto-generated keys</span></span>

<span data-ttu-id="7d7bd-116">자동으로 생성 된 키의 값은 엔터티를 삽입 또는 업데이트할 수 있는지 여부를 결정 하기 위한 자주 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-116">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="7d7bd-117">키 하지 않은 경우 엔터티 새 이어야 하며 삽입 해야 합니다 (즉, 아직 CLR 기본값인 null, 0, 등)를 설정 했습니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-117">If the key has not been set (i.e. it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="7d7bd-118">반면에 키 값에 설정한 경우 다음 이미 이전에 저장 해야 하 고 이제을 업데이트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-118">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="7d7bd-119">즉, 키 값이 있으면 않으면 엔터티는 클라이언트에 보낸 쿼리 되었습니다 방문 하 여가 지금 업데이트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-119">In other words, if the key has a value, then entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="7d7bd-120">엔터티 형식을 알 수 있을 때 설정 되지 않은 키를 확인 하는 것이 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-120">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="7d7bd-121">그러나 EF에 모든 엔터티 형식 및 키 형식에 대 한이 작업을 수행 하는 기본 제공 방법이:</span><span class="sxs-lookup"><span data-stu-id="7d7bd-121">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="7d7bd-122">엔터티는 Added 상태의 경우에 엔터티는 컨텍스트에 의해 추적 되는 즉시 키가 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-122">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="7d7bd-123">엔터티 및 TrackGraph API를 사용 하는 경우 각 등는 마찬가지로으로 수행할 작업을 결정 그래프 검토 때 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-123">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="7d7bd-124">여기에 표시 된 방식에는 키 값만 사용 해야 _전에_ 엔터티를 추적 하는 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-124">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="7d7bd-125">다른 키와</span><span class="sxs-lookup"><span data-stu-id="7d7bd-125">With other keys</span></span>

<span data-ttu-id="7d7bd-126">키 값을 자동으로 생성 되지 않은 경우 identity 새 엔터티를 다른 메커니즘이 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-126">Some other mechanism is needed to identity new entities when key values are not generated automatically.</span></span> <span data-ttu-id="7d7bd-127">두 가지 일반적인 방법을이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-127">There are two general approaches to this:</span></span>
 * <span data-ttu-id="7d7bd-128">엔터티에 대 한 쿼리</span><span class="sxs-lookup"><span data-stu-id="7d7bd-128">Query for the entity</span></span>
 * <span data-ttu-id="7d7bd-129">클라이언트에서 플래그를 전달 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-129">Pass a flag from the client</span></span>

<span data-ttu-id="7d7bd-130">엔터티를 쿼리하려면 Find 메서드를 사용 하기만 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-130">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="7d7bd-131">클라이언트에서 플래그를 전달 하기 위한 전체 코드를 표시 하려면이 문서의 범위를 벗어납니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-131">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="7d7bd-132">웹 앱에서 일반적으로 다양 한 작업에 대 한 다른 요청을 수행 하거나 특정 상태 요청에 전달 다음 컨트롤러에 추출 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-132">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="7d7bd-133">단일 엔터티 저장</span><span class="sxs-lookup"><span data-stu-id="7d7bd-133">Saving single entities</span></span>

<span data-ttu-id="7d7bd-134">Insert 또는 update 필요 여부 다음 추가 또는 업데이트를 적절 하 게 사용할 수 있습니다 알려진 경우:</span><span class="sxs-lookup"><span data-stu-id="7d7bd-134">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="7d7bd-135">그러나 엔터티 키 값 자동 생성을 사용 하는 경우 업데이트 메서드 두 경우 모두에 대해 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-135">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="7d7bd-136">큐브의 Update 메서드에 일반적으로 업데이트를 하지 삽입에 대 한 엔터티를 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-136">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="7d7bd-137">그러나의 엔터티에 자동 생성 키를 대신 엔터티는에 대 한 자동으로 표시 한 다음 키 값이 없는 설정 된에 삽입 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-137">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="7d7bd-138">이 문제는 EF 코어 2.0에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-138">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="7d7bd-139">이전 릴리스에서 해야 하는 항상 명시적으로 추가 또는 업데이트를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-139">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="7d7bd-140">엔터티는 자동 생성 키를 사용 하지 않는 경우 응용 프로그램 엔터티 삽입 또는 업데이트 여부를 결정 해야 합니다: 예:</span><span class="sxs-lookup"><span data-stu-id="7d7bd-140">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="7d7bd-141">여기에 단계는.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-141">The steps here are:</span></span>
* <span data-ttu-id="7d7bd-142">데이터베이스 호출 되므로이 ID 가진 블로그 포함 되어 있지 않은 한 후에 null을 반환 찾기 추가 삽입을 위해 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-142">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="7d7bd-143">찾기 엔터티를 반환 하는 경우 다음 데이터베이스에 있으며 컨텍스트 이제 기존 엔터티를 추적</span><span class="sxs-lookup"><span data-stu-id="7d7bd-143">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="7d7bd-144">그런 다음 SetValues를 사용 하 여이 엔터티를 클라이언트에서 제공 되는 것에 모든 속성에 대 한 값을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-144">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="7d7bd-145">SetValues 호출은 필요에 따라 업데이트할 엔터티를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-145">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="7d7bd-146">추적 된 엔터티입니다의 값이 다른 속성을 수정한 SetValues은 표시만 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-146">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="7d7bd-147">즉,는 업데이트를 보낼 때 실제로 변경 된 열만 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-147">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="7d7bd-148">(및 변경 된 내용이, 업데이트가 전혀 전송 됩니다.)</span><span class="sxs-lookup"><span data-stu-id="7d7bd-148">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="7d7bd-149">그래프 작업</span><span class="sxs-lookup"><span data-stu-id="7d7bd-149">Working with graphs</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="7d7bd-150">모든 새/all 기존 엔터티</span><span class="sxs-lookup"><span data-stu-id="7d7bd-150">All new/all existing entities</span></span>

<span data-ttu-id="7d7bd-151">그래프 사용의 예로 삽입 되었거나 해당 컬렉션과 관련 된 게시물 함께 블로그 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-151">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="7d7bd-152">그래프에 있는 모든 엔터티를 삽입할 또는 모두를 업데이트 해야 하는 경우 프로세스는 단일 개체에 대해 위에서 설명한 것과 동일 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-152">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="7d7bd-153">예를 들어, 블로그 및 다음과 같이 만든 게시물의 그래프:</span><span class="sxs-lookup"><span data-stu-id="7d7bd-153">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="7d7bd-154">다음과 같이 삽입 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-154">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="7d7bd-155">블로그 및 삽입할 수 있는 모든 게시물 추가에 대 한 호출으로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-155">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="7d7bd-156">마찬가지로, 그래프에 있는 모든 엔터티를 업데이트 해야 하는 경우 다음 업데이트 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-156">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="7d7bd-157">블로그 및 모든 게시물 업데이트할 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-157">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="7d7bd-158">신규 및 기존 엔터티의 조합</span><span class="sxs-lookup"><span data-stu-id="7d7bd-158">Mix of new and existing entities</span></span>

<span data-ttu-id="7d7bd-159">자동 생성 키와 업데이트 다시 사용할 수 삽입 및 업데이트가 모두에 대 한 그래프에 삽입 해야 하는 엔터티 및 업데이트 해야 하는 것이 혼합 되어 있는 경우에:</span><span class="sxs-lookup"><span data-stu-id="7d7bd-159">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="7d7bd-160">업데이트는 그래프, 블로그 또는 기타 모든 엔터티를 업데이트 하도록 표시 된 동안 키 값 집합을 설치 하지 않은 경우 삽입에 대 한 게시의 모든 엔터티를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-160">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="7d7bd-161">이전에 자동 생성 키를 사용 하지 않는 경우 쿼리 및 처리 일부 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-161">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="7d7bd-162">삭제를 처리합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-162">Handling deletes</span></span>

<span data-ttu-id="7d7bd-163">삭제 후 처리 하기 어려울 수 있습니다 해야 삭제할 수 없으면 엔터티의 의미 하는 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-163">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="7d7bd-164">이 처리 하는 한 가지 방법은 엔터티는 실제로 삭제 하지 않고 삭제 하는 것으로 표시 되는 "소프트 삭제"를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-164">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="7d7bd-165">삭제 한 다음 업데이트와 동일 하 게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-165">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="7d7bd-166">소프트 삭제를 사용 하 여 구현할 수 있습니다 [필터 쿼리](xref:core/querying/filters)합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-166">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="7d7bd-167">True 삭제에 대 한 그래프 차이 기본적으로 이란 수행 하려면 쿼리 패턴의 확장을 사용 하는 일반적인 패턴은 예:</span><span class="sxs-lookup"><span data-stu-id="7d7bd-167">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="7d7bd-168">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="7d7bd-168">TrackGraph</span></span>

<span data-ttu-id="7d7bd-169">내부적으로 추가, 연결, 및 Update 사용 그래프 순회 Unchanged 여부 것으로 표시 되어야 합니다 (삽입)를 추가, 수정 (업데이트)에 대 한 각 엔터티에 대해 수행 하는 결정 (아무 작업도 수행할)을, 또는 (삭제)를 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-169">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="7d7bd-170">이 메커니즘은 TrackGraph API를 통해 노출 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-170">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="7d7bd-171">예를 들어, 엔터티의 그래프 다시 보내면 처리 방법을 나타내는 각 엔터티에 대해 일부 플래그를 설정 한다고 가정해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-171">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="7d7bd-172">TrackGraph이이 플래그를 처리 한 다음 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-172">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="7d7bd-173">플래그의 예제에서는 간단한 설명을 위해 엔터티의 일부로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-173">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="7d7bd-174">일반적으로 플래그의 일부는 DTO 또는 일부 다른 상태는 요청에 포함 된 것입니다.</span><span class="sxs-lookup"><span data-stu-id="7d7bd-174">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
