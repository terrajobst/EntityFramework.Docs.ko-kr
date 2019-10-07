---
title: 관련 데이터 로드 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 4e4ba21cd099daab4db8a8f358800fde26980c14
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813584"
---
# <a name="loading-related-data"></a><span data-ttu-id="f0ec4-102">관련 데이터 로드</span><span class="sxs-lookup"><span data-stu-id="f0ec4-102">Loading Related Data</span></span>

<span data-ttu-id="f0ec4-103">Entity Framework Core에서는 모델의 탐색 속성을 사용하여 관련 엔터티를 로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="f0ec4-104">관련 데이터를 로드하는 데 사용되는 세 개의 일반적인 O/RM 패턴이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="f0ec4-105">**즉시 로드**는 관련 데이터가 초기 쿼리의 일부로 데이터베이스에서 로드됨을 의미합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="f0ec4-106">**명시적 로드**는 관련 데이터가 나중에 데이터베이스에서 명시적으로 로드됨을 의미합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="f0ec4-107">**지연 로드**는 탐색 속성에 액세스할 때 관련 데이터가 데이터베이스에서 투명하게 로드됨을 의미합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="f0ec4-108">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="f0ec4-109">즉시 로드</span><span class="sxs-lookup"><span data-stu-id="f0ec4-109">Eager loading</span></span>

<span data-ttu-id="f0ec4-110">`Include` 메서드를 사용하여 쿼리 결과에 포함할 관련 데이터를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="f0ec4-111">다음 예제에서 결과에 반환되는 블로그에는 관련 게시물로 채워진 `Posts` 속성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="f0ec4-112">Entity Framework Core는 이전에 컨텍스트 인스턴스에 로드된 다른 엔터티로 탐색 속성을 자동으로 수정합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="f0ec4-113">따라서 탐색 속성에 대한 데이터를 명시적으로 포함하지 않더라도 관련 엔터티의 일부 또는 전체가 이전에 로드된 경우 속성이 채워질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

<span data-ttu-id="f0ec4-114">여러 관계의 관련 데이터를 단일 쿼리에 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="f0ec4-115">여러 수준 포함</span><span class="sxs-lookup"><span data-stu-id="f0ec4-115">Including multiple levels</span></span>

<span data-ttu-id="f0ec4-116">`ThenInclude` 메서드를 사용하여 여러 수준의 관련 데이터를 포함하도록 관계를 드릴다운할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-116">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="f0ec4-117">다음 예제에서는 모든 블로그, 관련 게시물 및 각 게시물의 작성자를 로드합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

<span data-ttu-id="f0ec4-118">`ThenInclude`에 대한 여러 호출을 연결하여 관련 데이터의 추가 수준을 계속 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-118">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="f0ec4-119">이 호출을 모두 결합하여 여러 수준 및 여러 루트의 관련 데이터를 동일한 쿼리에 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-119">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="f0ec4-120">포함하려는 엔터티 중 하나에 대한 여러 관련 엔터티를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-120">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="f0ec4-121">예를 들어 `Blogs`를 쿼리하는 경우 `Posts`를 포함한 다음, `Posts`의 `Author` 및 `Tags`를 모두 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-121">For example, when querying `Blogs`, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="f0ec4-122">이렇게 하려면 루트에서 시작하는 각 포함 경로를 지정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-122">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="f0ec4-123">예를 들어 `Blog -> Posts -> Author` 및 `Blog -> Posts -> Tags`를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-123">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="f0ec4-124">그렇다고 중복 조인을 가져온다는 의미는 아니며, 대부분의 경우 EF에서 SQL을 생성할 때 조인을 통합합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-124">This does not mean you will get redundant joins; in most cases, EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

> [!CAUTION]
> <span data-ttu-id="f0ec4-125">버전 3.0.0부터는 `Include`를 실행할 때마다 관계형 공급자가 생성한 SQL 쿼리에 JOIN이 추가되는 반면, 이전 버전에서는 추가 SQL 쿼리가 생성되었습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-125">Since version 3.0.0, each `Include` will cause an additional JOIN to be added to SQL queries produced by relational providers, whereas previous versions generated additional SQL queries.</span></span> <span data-ttu-id="f0ec4-126">이렇게 하면 쿼리 성능이 향상되거나 더 나빠지는 등의 큰 변화가 생길 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-126">This can significantly change the performance of your queries, for better or worse.</span></span> <span data-ttu-id="f0ec4-127">특히 아주 많은 수의 `Include` 연산자를 포함하는 LINQ 쿼리는 데카르트 급증 문제를 방지하기 위해 여러 개별 LINQ 쿼리로 분할해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-127">In particular, LINQ queries with an exceedingly high number of `Include` operators may need to be broken down into multiple separate LINQ queries in order to avoid the cartesian explosion problem.</span></span>

### <a name="include-on-derived-types"></a><span data-ttu-id="f0ec4-128">파생 형식에 포함</span><span class="sxs-lookup"><span data-stu-id="f0ec4-128">Include on derived types</span></span>

<span data-ttu-id="f0ec4-129">`Include` 및 `ThenInclude`를 사용하여 파생 형식에만 정의된 탐색의 관련 데이터를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="f0ec4-130">다음과 같은 모델을 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-130">Given the following model:</span></span>

```csharp
public class SchoolContext : DbContext
{
    public DbSet<Person> People { get; set; }
    public DbSet<School> Schools { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<School>().HasMany(s => s.Students).WithOne(s => s.School);
    }
}

public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class Student : Person
{
    public School School { get; set; }
}

public class School
{
    public int Id { get; set; }
    public string Name { get; set; }

    public List<Student> Students { get; set; }
}
```

<span data-ttu-id="f0ec4-131">다양한 패턴을 사용하여 학생인 모든 사람의 `School` 탐색 콘텐츠를 로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="f0ec4-132">캐스트 사용</span><span class="sxs-lookup"><span data-stu-id="f0ec4-132">using cast</span></span>
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- <span data-ttu-id="f0ec4-133">`as` 연산자 사용</span><span class="sxs-lookup"><span data-stu-id="f0ec4-133">using `as` operator</span></span>
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- <span data-ttu-id="f0ec4-134">`string` 형식의 매개 변수를 사용하는 `Include`의 오버로드 사용</span><span class="sxs-lookup"><span data-stu-id="f0ec4-134">using overload of `Include` that takes parameter of type `string`</span></span>
  ```csharp
  context.People.Include("School").ToList()
  ```

## <a name="explicit-loading"></a><span data-ttu-id="f0ec4-135">명시적 로드</span><span class="sxs-lookup"><span data-stu-id="f0ec4-135">Explicit loading</span></span>

<span data-ttu-id="f0ec4-136">`DbContext.Entry(...)` API를 통해 탐색 속성을 명시적으로 로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-136">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="f0ec4-137">관련 엔터티를 반환하는 별도의 쿼리를 실행하여 탐색 속성을 명시적으로 로드할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-137">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="f0ec4-138">변경 내용 추적이 사용되는 경우 엔터티를 로드하면 EF Core는 새로 로드된 엔터티의 탐색 속성을 이미 로드된 엔터티를 참조하도록 자동으로 설정하고, 이미 로드된 엔터티의 탐색 속성을 새로 로드된 엔터티를 참조하도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-138">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="f0ec4-139">관련 엔터티 쿼리</span><span class="sxs-lookup"><span data-stu-id="f0ec4-139">Querying related entities</span></span>

<span data-ttu-id="f0ec4-140">탐색 속성의 내용을 나타내는 LINQ 쿼리를 가져올 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-140">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="f0ec4-141">따라서 메모리로 로드하지 않고도 관련 엔터티에 대해 집계 연산자를 실행하는 것과 같은 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-141">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="f0ec4-142">메모리로 로드되는 관련 엔터티를 필터링할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-142">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="f0ec4-143">지연 로드</span><span class="sxs-lookup"><span data-stu-id="f0ec4-143">Lazy loading</span></span>

<span data-ttu-id="f0ec4-144">지연 로드를 사용하는 가장 간단한 방법은 [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) 패키지를 설치하고 이를 사용하여 `UseLazyLoadingProxies`를 호출하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-144">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="f0ec4-145">예:</span><span class="sxs-lookup"><span data-stu-id="f0ec4-145">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="f0ec4-146">또는 AddDbContext를 사용하는 경우:</span><span class="sxs-lookup"><span data-stu-id="f0ec4-146">Or when using AddDbContext:</span></span>

```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```

<span data-ttu-id="f0ec4-147">EF Core에서 재정의할 수 있는 모든 탐색 속성에 대해 지연 로드를 사용합니다. 즉, `virtual`이어야 하고 상속될 수 있는 클래스에 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-147">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="f0ec4-148">예를 들어 다음 엔터티에서 `Post.Blog` 및 `Blog.Posts` 탐색 속성은 지연 로드됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-148">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>

```csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public virtual Blog Blog { get; set; }
}
```

### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="f0ec4-149">프록시 없는 지연 로드</span><span class="sxs-lookup"><span data-stu-id="f0ec4-149">Lazy loading without proxies</span></span>

<span data-ttu-id="f0ec4-150">지연 로드 프록시는 [엔터티 형식 생성자](../modeling/constructors.md)에 설명된 대로 `ILazyLoader` 서비스를 엔터티에 삽입하여 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-150">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="f0ec4-151">예:</span><span class="sxs-lookup"><span data-stu-id="f0ec4-151">For example:</span></span>

```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```

<span data-ttu-id="f0ec4-152">이 경우에는 상속되는 엔터티 형식이나 탐색 속성이 가상일 필요가 없으며 컨텍스트에 연결되면 `new`로 만든 엔터티 인스턴스가 지연 로드되도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-152">This doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="f0ec4-153">하지만 [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) 패키지에 정의된 `ILazyLoader` 서비스에 대한 참조가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-153">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="f0ec4-154">이 패키지에는 최소의 형식 집합이 포함되어 있으므로 이 패키지에 따른 영향이 거의 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-154">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="f0ec4-155">그러나 엔터티 형식의 EF Core 패키지를 전혀 사용하지 않으려면 `ILazyLoader.Load` 메서드를 대리자로 삽입할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-155">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="f0ec4-156">예:</span><span class="sxs-lookup"><span data-stu-id="f0ec4-156">For example:</span></span>

```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```

<span data-ttu-id="f0ec4-157">위의 코드에서는 `Load` 확장 메서드를 사용하여 대리자 사용을 좀 더 깔끔하게 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-157">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>

```csharp
public static class PocoLoadingExtensions
{
    public static TRelated Load<TRelated>(
        this Action<object, string> loader,
        object entity,
        ref TRelated navigationField,
        [CallerMemberName] string navigationName = null)
        where TRelated : class
    {
        loader?.Invoke(entity, navigationName);

        return navigationField;
    }
}
```

> [!NOTE]  
> <span data-ttu-id="f0ec4-158">지연 로드 대리자에 대한 생성자 매개 변수를 “lazyLoader”라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-158">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="f0ec4-159">이와 다른 이름을 사용하는 구성은 향후 릴리스에 포함될 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-159">Configuration to use a different name than this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="f0ec4-160">관련 데이터 및 serialization</span><span class="sxs-lookup"><span data-stu-id="f0ec4-160">Related data and serialization</span></span>

<span data-ttu-id="f0ec4-161">EF Core는 탐색 속성을 자동으로 수정하므로 개체 그래프의 주기로 끝날 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-161">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="f0ec4-162">예를 들어 블로그와 관련 게시물을 로드하면 게시물 모음을 참조하는 블로그 개체가 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-162">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="f0ec4-163">각 게시물은 다시 블로그를 참조합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-163">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="f0ec4-164">일부 serialization 프레임워크에서는 이러한 주기를 허용하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-164">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="f0ec4-165">예를 들어 주기가 발생하면 Json.NET은 다음 예외를 throw합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-165">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="f0ec4-166">Newtonsoft.Json.JsonSerializationException: 형식이 'MyApplication.Models.Blog'인 'Blog' 속성에 대해 자체 참조 루프가 검색되었습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-166">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="f0ec4-167">ASP.NET Core를 사용하는 경우 개체 그래프에서 찾은 주기를 무시하도록 Json.NET을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-167">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="f0ec4-168">이 작업은 `Startup.cs`의 `ConfigureServices(...)` 메서드에서 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-168">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

```csharp
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

<span data-ttu-id="f0ec4-169">또 다른 대안은 탐색 속성 중 하나를 `[JsonIgnore]` 특성으로 데코레이트하는 것입니다. 이 특성은 Json.NET이 직렬화하는 동안 해당 탐색 속성을 트래버스하지 않도록 지시합니다.</span><span class="sxs-lookup"><span data-stu-id="f0ec4-169">Another alternative is to decorate one of the navigation properties with the `[JsonIgnore]` attribute, which instructs Json.NET to not traverse that navigation property while serializing.</span></span>
