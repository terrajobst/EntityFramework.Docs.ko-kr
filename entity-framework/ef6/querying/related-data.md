---
title: 관련 엔터티 로드-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: c359d8d32a88049213fd5e98e99fe49d7e3121a3
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005483"
---
# <a name="loading-related-entities"></a><span data-ttu-id="950c6-102">관련 엔터티 로드</span><span class="sxs-lookup"><span data-stu-id="950c6-102">Loading Related Entities</span></span>

<span data-ttu-id="950c6-103">Entity Framework는 관련 데이터를 로드 하는 세 가지 방법, 즉 즉시 로드, 지연 로드 및 명시적 로드를 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-103">Entity Framework supports three ways to load related data - eager loading, lazy loading and explicit loading.</span></span> <span data-ttu-id="950c6-104">이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>

## <a name="eagerly-loading"></a><span data-ttu-id="950c6-105">적극적 로드</span><span class="sxs-lookup"><span data-stu-id="950c6-105">Eagerly Loading</span></span>

<span data-ttu-id="950c6-106">즉시 로드는 한 엔터티 형식에 대 한 쿼리가 쿼리의 일부로 관련 엔터티를 로드 하는 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-106">Eager loading is the process whereby a query for one type of entity also loads related entities as part of the query.</span></span> <span data-ttu-id="950c6-107">즉시 로드는 Include 메서드를 사용 하 여 구현 됩니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-107">Eager loading is achieved by use of the Include method.</span></span> <span data-ttu-id="950c6-108">예를 들어 아래 쿼리는 블로그 및 각 블로그와 관련 된 모든 게시물을 로드 합니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-108">For example, the queries below will load blogs and all the posts related to each blog.</span></span>

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts.
    var blogs1 = context.Blogs
                        .Include(b => b.Posts)
                        .ToList();

    // Load one blog and its related posts.
    var blog1 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include(b => b.Posts)
                       .FirstOrDefault();

    // Load all blogs and related posts
    // using a string to specify the relationship.
    var blogs2 = context.Blogs
                        .Include("Posts")
                        .ToList();

    // Load one blog and its related posts
    // using a string to specify the relationship.
    var blog2 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include("Posts")
                       .FirstOrDefault();
}
```

> [!NOTE]
> <span data-ttu-id="950c6-109">Include는 System.object 네임 스페이스의 확장 메서드 이므로 해당 네임 스페이스를 사용 하 고 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-109">Include is an extension method in the System.Data.Entity namespace so make sure you are using that namespace.</span></span>

### <a name="eagerly-loading-multiple-levels"></a><span data-ttu-id="950c6-110">여러 수준 적극적 로드</span><span class="sxs-lookup"><span data-stu-id="950c6-110">Eagerly loading multiple levels</span></span>

<span data-ttu-id="950c6-111">또한 여러 수준의 관련 엔터티를 적극적으로 로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-111">It is also possible to eagerly load multiple levels of related entities.</span></span> <span data-ttu-id="950c6-112">다음 쿼리는 컬렉션 및 참조 탐색 속성 모두에 대해이 작업을 수행 하는 방법의 예를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-112">The queries below show examples of how to do this for both collection and reference navigation properties.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments.
    var blogs1 = context.Blogs
                        .Include(b => b.Posts.Select(p => p.Comments))
                        .ToList();

    // Load all users, their related profiles, and related avatar.
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships.
    var blogs2 = context.Blogs
                        .Include("Posts.Comments")
                        .ToList();

    // Load all users, their related profiles, and related avatar  
    // using a string to specify the relationships.
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

> [!NOTE]
> <span data-ttu-id="950c6-113">현재 로드 된 관련 엔터티는 필터링 할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-113">It is not currently possible to filter which related entities are loaded.</span></span> <span data-ttu-id="950c6-114">Include는 항상 관련 된 모든 엔터티를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-114">Include will always bring in all related entities.</span></span>  

## <a name="lazy-loading"></a><span data-ttu-id="950c6-115">지연 로드</span><span class="sxs-lookup"><span data-stu-id="950c6-115">Lazy Loading</span></span>

<span data-ttu-id="950c6-116">지연 로드는 엔터티/엔터티를 참조 하는 속성에 처음 액세스할 때 엔터티 또는 엔터티 컬렉션이 데이터베이스에서 자동으로 로드 되는 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-116">Lazy loading is the process whereby an entity or collection of entities is automatically loaded from the database the first time that a property referring to the entity/entities is accessed.</span></span> <span data-ttu-id="950c6-117">POCO 엔터티 형식을 사용 하는 경우 파생 된 프록시 형식의 인스턴스를 만든 다음 가상 속성을 재정의 하 여 로딩 후크를 추가 하는 방법으로 지연 로드를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-117">When using POCO entity types, lazy loading is achieved by creating instances of derived proxy types and then overriding virtual properties to add the loading hook.</span></span> <span data-ttu-id="950c6-118">예를 들어 아래에 정의 된 블로그 엔터티 클래스를 사용 하는 경우 게시물 탐색 속성에 처음 액세스할 때 관련 게시물이 로드 됩니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-118">For example, when using the Blog entity class defined below, the related Posts will be loaded the first time the Posts navigation property is accessed:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }
    public string Tags { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

### <a name="turn-lazy-loading-off-for-serialization"></a><span data-ttu-id="950c6-119">Serialization에 대해 지연 로드 해제</span><span class="sxs-lookup"><span data-stu-id="950c6-119">Turn lazy loading off for serialization</span></span>

<span data-ttu-id="950c6-120">지연 로드와 serialization은 함께 사용 되지 않습니다. 또한 지연 로드를 사용 하는 경우에만 전체 데이터베이스에 대 한 쿼리를 종료할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-120">Lazy loading and serialization don’t mix well, and if you aren’t careful you can end up querying for your entire database just because lazy loading is enabled.</span></span> <span data-ttu-id="950c6-121">대부분의 serializer는 형식의 인스턴스에서 각 속성에 액세스 하 여 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-121">Most serializers work by accessing each property on an instance of a type.</span></span> <span data-ttu-id="950c6-122">속성 액세스는 지연 로드를 트리거하고 추가 엔터티는 serialize 됩니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-122">Property access triggers lazy loading, so more entities get serialized.</span></span> <span data-ttu-id="950c6-123">이러한 엔터티 속성에 액세스 하는 경우에도 더 많은 엔터티가 로드 됩니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-123">On those entities properties are accessed, and even more entities are loaded.</span></span> <span data-ttu-id="950c6-124">엔터티를 serialize 하기 전에 지연 로드를 해제 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-124">It’s a good practice to turn lazy loading off before you serialize an entity.</span></span> <span data-ttu-id="950c6-125">다음 섹션에서는이 작업을 수행 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-125">The following sections show how to do this.</span></span>

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a><span data-ttu-id="950c6-126">특정 탐색 속성에 대해 지연 로드 해제</span><span class="sxs-lookup"><span data-stu-id="950c6-126">Turning off lazy loading for specific navigation properties</span></span>

<span data-ttu-id="950c6-127">게시 속성을 비가상으로 설정 하 여 게시물 컬렉션의 지연 로드를 해제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-127">Lazy loading of the Posts collection can be turned off by making the Posts property non-virtual:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }
    public string Tags { get; set; }

    public ICollection<Post> Posts { get; set; }
}
```

<span data-ttu-id="950c6-128">즉시 로드 (위의 *적극적 로드* 참조) 또는 Load 메서드 (아래 *명시적 로드* 참조)를 사용 하 여 게시물 컬렉션 로드를 계속 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-128">Loading of the Posts collection can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>

### <a name="turn-off-lazy-loading-for-all-entities"></a><span data-ttu-id="950c6-129">모든 엔터티에 대해 지연 로드 해제</span><span class="sxs-lookup"><span data-stu-id="950c6-129">Turn off lazy loading for all entities</span></span>

<span data-ttu-id="950c6-130">구성 속성에서 플래그를 설정 하 여 컨텍스트의 모든 엔터티에 대해 지연 로드를 해제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-130">Lazy loading can be turned off for all entities in the context by setting a flag on the Configuration property.</span></span> <span data-ttu-id="950c6-131">예:</span><span class="sxs-lookup"><span data-stu-id="950c6-131">For example:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```

<span data-ttu-id="950c6-132">즉시 로드를 사용 하 여 관련 엔터티를 로드할 수 있습니다 (위의 *적극적 로드* 참조) 또는 Load 메서드 (아래 *명시적 로드* 참조).</span><span class="sxs-lookup"><span data-stu-id="950c6-132">Loading of related entities can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>

## <a name="explicitly-loading"></a><span data-ttu-id="950c6-133">명시적 로드</span><span class="sxs-lookup"><span data-stu-id="950c6-133">Explicitly Loading</span></span>

<span data-ttu-id="950c6-134">지연 로드를 사용 하지 않도록 설정한 경우에도 관련 된 엔터티를 지연 로드 하는 것은 가능 하지만 명시적 호출을 사용 하 여 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-134">Even with lazy loading disabled it is still possible to lazily load related entities, but it must be done with an explicit call.</span></span> <span data-ttu-id="950c6-135">이렇게 하려면 관련 엔터티의 항목에서 Load 메서드를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-135">To do so you use the Load method on the related entity’s entry.</span></span> <span data-ttu-id="950c6-136">예:</span><span class="sxs-lookup"><span data-stu-id="950c6-136">For example:</span></span>

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post.
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string.
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog.
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog
    // using a string to specify the relationship.
    context.Entry(blog).Collection("Posts").Load();
}
```

> [!NOTE]
> <span data-ttu-id="950c6-137">엔터티에 다른 단일 엔터티에 대 한 탐색 속성이 있는 경우 참조 메서드를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-137">The Reference method should be used when an entity has a navigation property to another single entity.</span></span> <span data-ttu-id="950c6-138">반면, 엔터티에 다른 엔터티 컬렉션에 대 한 탐색 속성이 있는 경우 컬렉션 메서드를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-138">On the other hand, the Collection method should be used when an entity has a navigation property to a collection of other entities.</span></span>

### <a name="applying-filters-when-explicitly-loading-related-entities"></a><span data-ttu-id="950c6-139">관련 엔터티를 명시적으로 로드할 때 필터 적용</span><span class="sxs-lookup"><span data-stu-id="950c6-139">Applying filters when explicitly loading related entities</span></span>

<span data-ttu-id="950c6-140">쿼리 메서드는 관련 엔터티를 로드할 때 Entity Framework 사용할 기본 쿼리에 대 한 액세스를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-140">The Query method provides access to the underlying query that Entity Framework will use when loading related entities.</span></span> <span data-ttu-id="950c6-141">그런 다음 linq를 사용 하 여 쿼리에 필터를 적용 한 후에는 ToList, Load 등의 LINQ 확장 메서드를 호출 하 여 필터를 실행할 수 있습니다. 쿼리 메서드는 참조 및 컬렉션 탐색 속성 모두와 함께 사용할 수 있지만 컬렉션의 일부를 로드 하는 데 사용할 수 있는 컬렉션에는 가장 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-141">You can then use LINQ to apply filters to the query before executing it with a call to a LINQ extension method such as ToList, Load, etc. The Query method can be used with both reference and collection navigation properties but is most useful for collections where it can be used to load only part of the collection.</span></span> <span data-ttu-id="950c6-142">예:</span><span class="sxs-lookup"><span data-stu-id="950c6-142">For example:</span></span>

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog.
    context.Entry(blog)
           .Collection(b => b.Posts)
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog
    // using a string to specify the relationship.
    context.Entry(blog)
           .Collection("Posts")
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();
}
```

<span data-ttu-id="950c6-143">쿼리 메서드를 사용 하는 경우 일반적으로 탐색 속성에 대해 지연 로드를 해제 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-143">When using the Query method it is usually best to turn off lazy loading for the navigation property.</span></span> <span data-ttu-id="950c6-144">그 이유는 필터링 된 쿼리가 실행 되기 전이나 후에 지연 로드 메커니즘에 의해 전체 컬렉션이 자동으로 로드 될 수 있기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-144">This is because otherwise the entire collection may get loaded automatically by the lazy loading mechanism either before or after the filtered query has been executed.</span></span>

> [!NOTE]
> <span data-ttu-id="950c6-145">이 관계는 람다 식 대신 문자열로 지정할 수 있지만, 반환 되는 IQueryable은 문자열이 사용 될 때 일반적이 지 않으며이를 사용 하 여 유용한 작업을 수행 하기 전에 캐스트 메서드가 일반적으로 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-145">While the relationship can be specified as a string instead of a lambda expression, the returned IQueryable is not generic when a string is used and so the Cast method is usually needed before anything useful can be done with it.</span></span>

## <a name="using-query-to-count-related-entities-without-loading-them"></a><span data-ttu-id="950c6-146">쿼리를 사용 하 여 관련 엔터티를 로드 하지 않고 계산</span><span class="sxs-lookup"><span data-stu-id="950c6-146">Using Query to count related entities without loading them</span></span>

<span data-ttu-id="950c6-147">경우에 따라 실제로 모든 엔터티를 로드 하는 데 드는 비용을 들이지 않고 데이터베이스의 다른 엔터티와 관련 된 엔터티 수를 파악 하는 것이 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-147">Sometimes it is useful to know how many entities are related to another entity in the database without actually incurring the cost of loading all those entities.</span></span> <span data-ttu-id="950c6-148">LINQ Count 메서드를 사용 하는 쿼리 메서드를 사용 하 여이 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="950c6-148">The Query method with the LINQ Count method can be used to do this.</span></span> <span data-ttu-id="950c6-149">예:</span><span class="sxs-lookup"><span data-stu-id="950c6-149">For example:</span></span>

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has.
    var postCount = context.Entry(blog)
                           .Collection(b => b.Posts)
                           .Query()
                           .Count();
}
```
