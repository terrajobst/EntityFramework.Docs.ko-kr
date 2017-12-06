---
title: "로드 된 데이터-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: cd26bd2e6f85083f73d97b1356d0ba38f53e0b8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="loading-related-data"></a><span data-ttu-id="8039b-102">관련된 데이터를 로드 합니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-102">Loading Related Data</span></span>

<span data-ttu-id="8039b-103">Entity Framework Core를 사용 하면 모델에서 탐색 속성을 사용 하 여 관련된 엔터티 로드 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="8039b-104">관련된 데이터를 로드 하는 데 사용 하는 일반적인 O/RM 패턴 세 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="8039b-105">**즉시 로드** 초기 쿼리의 일부로 관련된 데이터가 데이터베이스에서 로드 될 것을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="8039b-106">**명시적 로드** 나중에 관련된 된 데이터는 데이터베이스에서 명시적으로 로드 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="8039b-107">**지연 로드** 탐색 속성에 액세스할 때 관련된 데이터는 데이터베이스에서 로드 투명 하 게 한다는 것을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span> <span data-ttu-id="8039b-108">지연 로드 불가능 아직 EF 코어 합니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-108">Lazy loading is not yet possible with EF Core.</span></span>

> [!TIP]  
> <span data-ttu-id="8039b-109">이 문서를 볼 수 있습니다 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub에서 합니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="8039b-110">즉시 로드</span><span class="sxs-lookup"><span data-stu-id="8039b-110">Eager loading</span></span>

<span data-ttu-id="8039b-111">사용할 수는 `Include` 메서드를 쿼리 결과에 포함할 관련된 데이터를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-111">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="8039b-112">다음 예제에서 결과에 반환 되는 블로그에 포함 됩니다는 `Posts` 속성 게시물 채워집니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-112">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="8039b-113">Framework Core가 자동으로 픽스업 탐색 속성 컨텍스트 인스턴스에 이전에 로드 된 다른 엔터티를 엔터티 하 고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-113">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="8039b-114">따라서 탐색 속성에 대 한 데이터를 명시적으로 포함 하지, 경우에 일부 속성이 채울 수 있습니다 또는 이전에 로드 된 모든 관련된 엔터티.</span><span class="sxs-lookup"><span data-stu-id="8039b-114">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="8039b-115">단일 쿼리에 여러 관계에서 관련 된 데이터를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-115">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="8039b-116">여러 수준을 포함 하 여</span><span class="sxs-lookup"><span data-stu-id="8039b-116">Including multiple levels</span></span>

<span data-ttu-id="8039b-117">관계를 사용 하 여 관련된 데이터의 여러 수준이 포함을 통해 드릴 다운할 수 있습니다는 `ThenInclude` 메서드.</span><span class="sxs-lookup"><span data-stu-id="8039b-117">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="8039b-118">다음 예제에서는 모든 블로그, 해당 관련 된 게시물 및 각 게시물의 작성자를 로드합니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-118">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

<span data-ttu-id="8039b-119">에 대 한 여러 호출을 체인화할 수 `ThenInclude` 추가 관련된 데이터의 수준을 포함 하 여 계속 합니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-119">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="8039b-120">동일한 쿼리에서 여러 수준 및 여러 루트의 관련된 데이터를 포함 하도록이 모든를 결합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-120">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="8039b-121">포함 되 고 있는 엔터티 중 하나에 대 한 여러 관련된 엔터티를 포함 하려고 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-121">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="8039b-122">예를 들어, 쿼리할 때 `Blog`포함 s, `Posts` 모두 포함 하려면는 `Author` 및 `Tags` 의 `Posts`합니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-122">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="8039b-123">이 수행 하려면 루트에서 시작 하는 경로 포함 하는 각 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-123">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="8039b-124">예를 들면 `Blog -> Posts -> Author` 및 `Blog -> Posts -> Tags`입니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-124">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="8039b-125">받아볼 수 EF 통합 대부분의 경우에서 중복 조인 해 서 조인을 SQL을 생성할 때 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-125">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a><span data-ttu-id="8039b-126">무시 포함</span><span class="sxs-lookup"><span data-stu-id="8039b-126">Ignored includes</span></span>

<span data-ttu-id="8039b-127">쿼리 시작 된 엔터티 형식의 인스턴스를 더 이상 반환 되도록 쿼리를 변경 하면 포함 연산자는 무시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-127">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="8039b-128">Include 연산자 기반으로 다음 예제에서는 `Blog`, 하지만 다음의 `Select` 연산자는 익명 형식을 반환 하도록 쿼리를 변경 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-128">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="8039b-129">이 경우에 포함 연산자 효과가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-129">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="8039b-130">EF 코어 기본적으로 경고를 기록 합니다 포함 경우 연산자는 무시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-130">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="8039b-131">참조 [로깅](../miscellaneous/logging.md) 로깅 출력 보기에 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-131">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="8039b-132">Include 연산자를 throw 하거나 아무 작업도 수행 하지를 무시할 경우 동작을 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-132">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="8039b-133">일반적으로 컨텍스트-에 대 한 옵션을 설정할 때 이렇게 `DbContext.OnConfiguring`, 또는 `Startup.cs` ASP.NET Core를 사용 하는 경우.</span><span class="sxs-lookup"><span data-stu-id="8039b-133">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="8039b-134">명시적 로드</span><span class="sxs-lookup"><span data-stu-id="8039b-134">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="8039b-135">이 기능은 EF 코어 1.1에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-135">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="8039b-136">탐색 속성을 통해 명시적으로 로드할 수는 `DbContext.Entry(...)` API입니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-136">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="8039b-137">관련된 엔터티를 반환 하는 별도 쿼리를 실행 하 여 탐색 속성을 명시적으로 로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-137">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="8039b-138">변경 추적이 설정 된 경우 엔터티를 로드할 때 EF 코어는 자동으로 새로 로드 된 엔터티를 이미 로드 된 모든 엔터티를 참조의 탐색 속성을 설정 하 고 참조를 이미 로드 된 엔터티의 탐색 속성을 설정 된 새로 로드 된 엔터티입니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-138">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="8039b-139">관련된 엔터티를 쿼리합니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-139">Querying related entities</span></span>

<span data-ttu-id="8039b-140">탐색 속성의 내용을 제공 하는 LINQ 쿼리를 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-140">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="8039b-141">이 옵션을 사용 하면 메모리에 로드 하기 없이 관련된 엔터티를 통해 집계 연산자를 실행 하는 등 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-141">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="8039b-142">메모리에는 관련된 엔터티 로드 되어 필터링 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-142">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="8039b-143">지연 로드</span><span class="sxs-lookup"><span data-stu-id="8039b-143">Lazy loading</span></span>

<span data-ttu-id="8039b-144">지연 로드 EF 코어에 아직 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-144">Lazy loading is not yet supported by EF Core.</span></span> <span data-ttu-id="8039b-145">볼 수는 [백로그에 항목 한 지연 로딩이](https://github.com/aspnet/EntityFramework/issues/3797) 이 기능을 추적 하 합니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-145">You can view the [lazy loading item on our backlog](https://github.com/aspnet/EntityFramework/issues/3797) to track this feature.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="8039b-146">관련된 데이터와 serialization</span><span class="sxs-lookup"><span data-stu-id="8039b-146">Related data and serialization</span></span>

<span data-ttu-id="8039b-147">EF 코어가 자동으로 픽스업 탐색 속성을 실행 하 게 주기와 개체 그래프에 때문에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-147">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="8039b-148">예를 들어, 블로그 및 것 로드 하는 관련 된 게시물 게시물의 컬렉션을 참조 하는 블로그 개체에서 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-148">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="8039b-149">각 해당 게시에 대 한 역참조 블로그 갖습니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-149">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="8039b-150">일부 직렬화 프레임 워크는 이러한 주기를 허용 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-150">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="8039b-151">예를 들어, 사이클은 발견 Json.NET 다음 예외를 throw 합니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-151">For example, Json.NET will throw the following exception if a cycle is encoutered.</span></span>

> <span data-ttu-id="8039b-152">Newtonsoft.Json.JsonSerializationException: 자체 'MyApplication.Models.Blog' 형식과 '블로그' 속성에 대 한 검색 된 루프를 참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-152">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="8039b-153">ASP.NET Core를 사용 하는 경우 개체 그래프에서 발견 된 주기를 무시 하도록 Json.NET을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-153">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="8039b-154">이 작업은 `ConfigureServices(...)` 메서드에서 `Startup.cs`합니다.</span><span class="sxs-lookup"><span data-stu-id="8039b-154">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
