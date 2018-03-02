---
title: "로드 된 데이터-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: dadc6235c3879ae27ad5c99988a5e594872045df
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="loading-related-data"></a><span data-ttu-id="d284c-102">관련된 데이터를 로드 합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-102">Loading Related Data</span></span>

<span data-ttu-id="d284c-103">Entity Framework Core를 사용 하면 모델에서 탐색 속성을 사용 하 여 관련된 엔터티 로드 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="d284c-104">관련된 데이터를 로드 하는 데 사용 하는 일반적인 O/RM 패턴 세 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="d284c-105">**즉시 로드** 초기 쿼리의 일부로 관련된 데이터가 데이터베이스에서 로드 될 것을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="d284c-106">**명시적 로드** 나중에 관련된 된 데이터는 데이터베이스에서 명시적으로 로드 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="d284c-107">**지연 로드** 탐색 속성에 액세스할 때 관련된 데이터는 데이터베이스에서 로드 투명 하 게 한다는 것을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="d284c-108">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="d284c-109">즉시 로드</span><span class="sxs-lookup"><span data-stu-id="d284c-109">Eager loading</span></span>

<span data-ttu-id="d284c-110">사용할 수는 `Include` 메서드를 쿼리 결과에 포함할 관련된 데이터를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="d284c-111">다음 예제에서 결과에 반환 되는 블로그에 포함 됩니다는 `Posts` 속성 게시물 채워집니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="d284c-112">Framework Core가 자동으로 픽스업 탐색 속성 컨텍스트 인스턴스에 이전에 로드 된 다른 엔터티를 엔터티 하 고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="d284c-113">따라서 탐색 속성에 대 한 데이터를 명시적으로 포함 하지, 경우에 일부 속성이 채울 수 있습니다 또는 이전에 로드 된 모든 관련된 엔터티.</span><span class="sxs-lookup"><span data-stu-id="d284c-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="d284c-114">단일 쿼리에 여러 관계에서 관련 된 데이터를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="d284c-115">여러 수준을 포함 하 여</span><span class="sxs-lookup"><span data-stu-id="d284c-115">Including multiple levels</span></span>

<span data-ttu-id="d284c-116">관계를 사용 하 여 관련된 데이터의 여러 수준이 포함을 통해 드릴 다운할 수 있습니다는 `ThenInclude` 메서드.</span><span class="sxs-lookup"><span data-stu-id="d284c-116">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="d284c-117">다음 예제에서는 모든 블로그, 해당 관련 된 게시물 및 각 게시물의 작성자를 로드합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="d284c-118">현재 버전의 Visual Studio는 잘못 된 코드 완성 옵션을 제공 하 고 올바른 식을 사용 하는 경우 구문 오류와 함께 플래그가 지정 될 수 있습니다는 `ThenInclude` 메서드 후 컬렉션 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="d284c-119">이 https://github.com/dotnet/roslyn/issues/8237에서 추적 프로그램 IntelliSense 버그가 있음을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="d284c-120">으로 코드가 올바른지와 성공적으로 컴파일할 수 있습니다 이러한 잘못 된 구문 오류는 무시 해도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="d284c-121">에 대 한 여러 호출을 체인화할 수 `ThenInclude` 추가 관련된 데이터의 수준을 포함 하 여 계속 합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="d284c-122">동일한 쿼리에서 여러 수준 및 여러 루트의 관련된 데이터를 포함 하도록이 모든를 결합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="d284c-123">포함 되 고 있는 엔터티 중 하나에 대 한 여러 관련된 엔터티를 포함 하려고 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="d284c-124">예를 들어, 쿼리할 때 `Blog`포함 s, `Posts` 모두 포함 하려면는 `Author` 및 `Tags` 의 `Posts`합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-124">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="d284c-125">이 수행 하려면 루트에서 시작 하는 경로 포함 하는 각 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="d284c-126">예를 들어 `Blog -> Posts -> Author` 및 `Blog -> Posts -> Tags`합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="d284c-127">받아볼 수 EF 통합 대부분의 경우에서 중복 조인 해 서 조인을 SQL을 생성할 때 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-127">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="d284c-128">파생된 형식에 대해 포함</span><span class="sxs-lookup"><span data-stu-id="d284c-128">Include on derived types</span></span>

<span data-ttu-id="d284c-129">탐색을 사용 하 여 파생 된 형식에 대해서만 정의할의 관련된 데이터를 포함할 수 있습니다 `Include` 및 `ThenInclude`합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="d284c-130">다음 모델을 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-130">Given the following model:</span></span>

```Csharp
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

<span data-ttu-id="d284c-131">내용을 `School` 학생 인 모든 사용자의 탐색 수 로드 되도록 할 다양 한 패턴을 사용 하 여:</span><span class="sxs-lookup"><span data-stu-id="d284c-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="d284c-132">cast 사용</span><span class="sxs-lookup"><span data-stu-id="d284c-132">using cast</span></span>
```Csharp
context.People.Include(person => ((Student)person).School).ToList()
```

- <span data-ttu-id="d284c-133">사용 하 여 `as` 연산자</span><span class="sxs-lookup"><span data-stu-id="d284c-133">using `as` operator</span></span>
```Csharp
context.People.Include(person => (person as Student).School).ToList()
```

- <span data-ttu-id="d284c-134">오버 로드를 사용 하 여 `Include` 형식의 매개 변수를 사용 하는 `string`</span><span class="sxs-lookup"><span data-stu-id="d284c-134">using overload of `Include` that takes parameter of type `string`</span></span>
```Csharp
context.People.Include("Student").ToList()
```

### <a name="ignored-includes"></a><span data-ttu-id="d284c-135">무시 포함</span><span class="sxs-lookup"><span data-stu-id="d284c-135">Ignored includes</span></span>

<span data-ttu-id="d284c-136">쿼리 시작 된 엔터티 형식의 인스턴스를 더 이상 반환 되도록 쿼리를 변경 하면 포함 연산자는 무시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="d284c-137">Include 연산자 기반으로 다음 예제에서는 `Blog`, 하지만 다음의 `Select` 연산자는 익명 형식을 반환 하도록 쿼리를 변경 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="d284c-138">이 경우에 포함 연산자 효과가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="d284c-139">EF 코어 기본적으로 경고를 기록 합니다 포함 경우 연산자는 무시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="d284c-140">참조 [로깅](../miscellaneous/logging.md) 로깅 출력 보기에 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="d284c-141">Include 연산자를 throw 하거나 아무 작업도 수행 하지를 무시할 경우 동작을 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="d284c-142">일반적으로 컨텍스트-에 대 한 옵션을 설정할 때 이렇게 `DbContext.OnConfiguring`, 또는 `Startup.cs` ASP.NET Core를 사용 하는 경우.</span><span class="sxs-lookup"><span data-stu-id="d284c-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="d284c-143">명시적 로드</span><span class="sxs-lookup"><span data-stu-id="d284c-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="d284c-144">이 기능은 EF 코어 1.1에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="d284c-145">탐색 속성을 통해 명시적으로 로드할 수는 `DbContext.Entry(...)` API입니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="d284c-146">관련된 엔터티를 반환 하는 별도 쿼리를 실행 하 여 탐색 속성을 명시적으로 로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-146">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="d284c-147">변경 추적이 설정 된 경우 엔터티를 로드할 때 EF 코어는 자동으로 새로 로드 된 엔터티를 이미 로드 된 모든 엔터티를 참조의 탐색 속성을 설정 하 고 참조를 이미 로드 된 엔터티의 탐색 속성을 설정 된 새로 로드 된 엔터티입니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="d284c-148">관련된 엔터티를 쿼리합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-148">Querying related entities</span></span>

<span data-ttu-id="d284c-149">탐색 속성의 내용을 제공 하는 LINQ 쿼리를 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="d284c-150">이 옵션을 사용 하면 메모리에 로드 하기 없이 관련된 엔터티를 통해 집계 연산자를 실행 하는 등 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="d284c-151">메모리에는 관련된 엔터티 로드 되어 필터링 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="d284c-152">지연 로드</span><span class="sxs-lookup"><span data-stu-id="d284c-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="d284c-153">이 기능은 EF 코어 2.1에 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="d284c-154">지연 로드를 사용 하는 가장 간단한 방법은 설치 된 경우는 [Microsoft.EntityFramworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) 패키지 및을 호출 하 여 설정 되어 있으므로 `UseLazyLoadingProxies`합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFramworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="d284c-155">예:</span><span class="sxs-lookup"><span data-stu-id="d284c-155">For example:</span></span>
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="d284c-156">또는 AddDbContext를 사용 하는 경우:</span><span class="sxs-lookup"><span data-stu-id="d284c-156">Or when using AddDbContext:</span></span>
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
<span data-ttu-id="d284c-157">EF 코어 가능한-재정의할 수 있는 탐색 속성에 대 한 지연 로딩이 하면 다음 여야 합니다. `virtual` 및에서 상속 될 수 있는 클래스에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-157">EF Core will then enable lazy-loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="d284c-158">예를 들어 다음과 같은 엔터티는 `Post.Blog` 및 `Blog.Posts` 탐색 속성이 지연 로드 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
```Csharp
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
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="d284c-159">프록시 없이 Lazy 로드</span><span class="sxs-lookup"><span data-stu-id="d284c-159">Lazy-loading without proxies</span></span>

<span data-ttu-id="d284c-160">Lazy 로드 프록시를 삽입 하 여 작업의 `ILazyLoader` 에 설명 된 대로 엔터티로 서비스 [엔터티 형식 생성자](../modeling/constructors.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="d284c-161">예:</span><span class="sxs-lookup"><span data-stu-id="d284c-161">For example:</span></span>
```Csharp
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
        get => LazyLoader?.Load(this, ref _posts);
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
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="d284c-162">엔터티 인스턴스를 사용 하 여 만든 있습니다 고 엔터티 형식에서 상속 되어야 또는 가상 탐색 속성에는 필요 하지 않으며이 `new` 지연 로드 한 번에 한 컨텍스트에 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-162">This doesn't require entity types to be inherited from or navigation properties to be virtual and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="d284c-163">하지만에 대 한 참조가 필요는 `ILazyLoader` 엔터티 형식 EF 핵심 어셈블리를 결합 하는 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-163">However, it requires a reference to the `ILazyLoader` service, which couples entity types to the EF Core assembly.</span></span> <span data-ttu-id="d284c-164">이 EF 코어를 방지 하기 위해는 `ILazyLoader.Load` 메서드를 대리자로 삽입 합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-164">To avoid this EF Core allows the `ILazyLoader.Load` method to be injected as a delegate.</span></span> <span data-ttu-id="d284c-165">예:</span><span class="sxs-lookup"><span data-stu-id="d284c-165">For example:</span></span>
```Csharp
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
        get => LazyLoader?.Load(this, ref _posts);
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
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="d284c-166">사용 하 여 위의 코드는 `Load` 확장 메서드는 대리자를 사용 하 여 수행 되는 클리너 비트:</span><span class="sxs-lookup"><span data-stu-id="d284c-166">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
```Csharp
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
> <span data-ttu-id="d284c-167">지연 로드 대리자에 대 한 생성자 매개 변수는 "lazyLoader" 호출 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-167">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="d284c-168">이후 버전에 대 한이 계획 되어 있습니다. 다른 이름을 사용할 구성입니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-168">Configuration to use a different name this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="d284c-169">관련된 데이터와 serialization</span><span class="sxs-lookup"><span data-stu-id="d284c-169">Related data and serialization</span></span>

<span data-ttu-id="d284c-170">EF 코어가 자동으로 픽스업 탐색 속성을 실행 하 게 주기와 개체 그래프에 때문에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-170">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="d284c-171">예를 들어, 블로그 및 것 로드 하는 관련 된 게시물 게시물의 컬렉션을 참조 하는 블로그 개체에서 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-171">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="d284c-172">각 해당 게시에 대 한 역참조 블로그 갖습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-172">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="d284c-173">일부 직렬화 프레임 워크는 이러한 주기를 허용 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-173">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="d284c-174">예를 들어 순환이 발생 Json.NET 다음 예외를 throw 합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-174">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="d284c-175">Newtonsoft.Json.JsonSerializationException: 자체 'MyApplication.Models.Blog' 형식과 '블로그' 속성에 대 한 검색 된 루프를 참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-175">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="d284c-176">ASP.NET Core를 사용 하는 경우 개체 그래프에서 발견 된 주기를 무시 하도록 Json.NET을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-176">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="d284c-177">이 작업은 `ConfigureServices(...)` 메서드에서 `Startup.cs`합니다.</span><span class="sxs-lookup"><span data-stu-id="d284c-177">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

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
