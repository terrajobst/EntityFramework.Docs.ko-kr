---
title: 생성자를 사용 하는 엔터티 형식-EF Core
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: ddfaa8eebde388a9d3309f21b8891de593077956
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811886"
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="c605e-102">생성자가 포함 된 엔터티 형식</span><span class="sxs-lookup"><span data-stu-id="c605e-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="c605e-103">이 기능은 EF Core 2.1의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="c605e-104">EF Core 2.1부터 이제 매개 변수를 사용 하 여 생성자를 정의 하 고 엔터티 인스턴스를 만들 때이 생성자를 호출 EF Core 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="c605e-105">생성자 매개 변수를 매핑된 속성 또는 다양 한 종류의 서비스에 바인딩하여 지연 로드와 같은 동작을 쉽게 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="c605e-106">EF Core 2.1를 기준으로 모든 생성자 바인딩은 규칙을 기준으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="c605e-107">사용할 특정 생성자의 구성은 이후 릴리스에 사용할 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="c605e-108">매핑된 속성에 바인딩</span><span class="sxs-lookup"><span data-stu-id="c605e-108">Binding to mapped properties</span></span>

<span data-ttu-id="c605e-109">일반적인 블로그/포스트 모델을 고려 합니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-109">Consider a typical Blog/Post model:</span></span>

``` csharp
public class Blog
{
    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="c605e-110">쿼리 결과의 경우 처럼 이러한 형식의 인스턴스를 만들 EF Core 때 먼저 기본 매개 변수가 없는 생성자를 호출한 다음 각 속성을 데이터베이스의 값으로 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="c605e-111">그러나 EF Core는 매핑된 속성의 값과 일치 하는 매개 변수 이름 및 형식이 포함 된 매개 변수가 있는 생성자를 찾은 경우 대신 해당 속성에 대 한 값으로 매개 변수가 있는 생성자를 호출 하 고 각 속성을 명시적으로 설정 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="c605e-112">예를 들면,</span><span class="sxs-lookup"><span data-stu-id="c605e-112">For example:</span></span>

``` csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="c605e-113">유의 해야 할 몇 가지 사항은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-113">Some things to note:</span></span>

* <span data-ttu-id="c605e-114">일부 속성에는 생성자 매개 변수를 사용할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="c605e-115">예를 들어, Post. Content 속성은 생성자 매개 변수에 의해 설정 되지 않으므로 EF Core은 일반적인 방식으로 생성자를 호출한 후이를 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="c605e-116">매개 변수 형식 및 이름은 속성 형식 및 이름과 일치 해야 합니다. 단, 매개 변수는 카멜식 대/소문자를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="c605e-117">EF Core 생성자를 사용 하 여 탐색 속성 (예: 블로그의 블로그 또는 게시물)을 설정할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="c605e-118">생성자는 public, private 또는 다른 접근성이 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-118">The constructor can be public, private, or have any other accessibility.</span></span> <span data-ttu-id="c605e-119">그러나 지연 로드 프록시의 경우에는 상속 프록시 클래스에서 생성자에 액세스할 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-119">However, lazy-loading proxies require that the constructor is accessible from the inheriting proxy class.</span></span> <span data-ttu-id="c605e-120">일반적으로이는 공용 또는 보호 됨을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-120">Usually this means making it either public or protected.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="c605e-121">읽기 전용 속성</span><span class="sxs-lookup"><span data-stu-id="c605e-121">Read-only properties</span></span>

<span data-ttu-id="c605e-122">생성자를 통해 속성을 설정 하는 경우 속성 중 일부를 읽기 전용으로 설정 하는 것이 좋을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-122">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="c605e-123">EF Core 지원 하지만 다음과 같은 몇 가지 사항을 확인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-123">EF Core supports this, but there are some things to look out for:</span></span>

* <span data-ttu-id="c605e-124">Setter가 없는 속성은 규칙에 따라 매핑되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-124">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="c605e-125">이렇게 하면 계산 된 속성과 같이 매핑해야 하는 속성을 매핑하는 경향이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-125">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="c605e-126">새 엔터티를 삽입할 때 키 생성기에서 키 값을 설정 해야 하므로 자동으로 생성 된 키 값을 사용 하려면 읽기/쓰기 키 속성이 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-126">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="c605e-127">이러한 작업을 방지 하는 쉬운 방법은 전용 setter를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-127">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="c605e-128">예를 들면,</span><span class="sxs-lookup"><span data-stu-id="c605e-128">For example:</span></span>

``` csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; private set; }

    public string Name { get; private set; }
    public string Author { get; private set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; private set; }

    public string Title { get; private set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; private set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="c605e-129">EF Core은 개인 setter가 있는 속성을 읽기/쓰기로 표시 합니다. 즉, 모든 속성이 이전 처럼 매핑되고 키가 계속 저장 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-129">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="c605e-130">Private setter를 사용 하는 대신 속성을 읽기 전용으로 설정 하 고 OnModelCreating에서 보다 명시적인 매핑을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-130">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="c605e-131">마찬가지로 일부 속성은 완전히 제거 하 고 필드만 바꿀 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-131">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="c605e-132">예를 들어 다음 엔터티 형식을 고려 합니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-132">For example, consider these entity types:</span></span>

``` csharp
public class Blog
{
    private int _id;

    public Blog(string name, string author)
    {
        Name = name;
        Author = author;
    }

    public string Name { get; }
    public string Author { get; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    private int _id;

    public Post(string title, DateTime postedOn)
    {
        Title = title;
        PostedOn = postedOn;
    }

    public string Title { get; }
    public string Content { get; set; }
    public DateTime PostedOn { get; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="c605e-133">그리고 OnModelCreating에서 다음 구성을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-133">And this configuration in OnModelCreating:</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Author);
            b.Property(e => e.Name);
        });

    modelBuilder.Entity<Post>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Title);
            b.Property(e => e.PostedOn);
        });
}
```

<span data-ttu-id="c605e-134">참고 사항:</span><span class="sxs-lookup"><span data-stu-id="c605e-134">Things to note:</span></span>

* <span data-ttu-id="c605e-135">이제 "속성" 키가 필드입니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-135">The key "property" is now a field.</span></span> <span data-ttu-id="c605e-136">저장소에서 생성 된 키를 사용할 수 있도록 `readonly` 필드가 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-136">It is not a `readonly` field so that store-generated keys can be used.</span></span>
* <span data-ttu-id="c605e-137">다른 속성은 생성자 에서만 설정 되는 읽기 전용 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-137">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="c605e-138">기본 키 값이 EF로만 설정 되거나 데이터베이스에서 읽은 경우 생성자에 포함할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-138">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="c605e-139">이렇게 하면 "속성" 키가 단순 필드로 유지 되 고 새 블로그 또는 게시물을 만들 때 명시적으로 설정 하지 않아야 한다는 것을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-139">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="c605e-140">이 코드는 필드가 사용 되지 않음을 나타내는 컴파일러 경고 ' 169 '을 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-140">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="c605e-141">이는 실제로 extralinguistic 된 방식으로 필드를 사용 EF Core 때문에 무시 해도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-141">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="c605e-142">서비스 삽입</span><span class="sxs-lookup"><span data-stu-id="c605e-142">Injecting services</span></span>

<span data-ttu-id="c605e-143">EF Core 엔터티 형식의 생성자에 "services"를 주입할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-143">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="c605e-144">예를 들어 다음을 삽입할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-144">For example, the following can be injected:</span></span>

* <span data-ttu-id="c605e-145">`DbContext`-파생 DbContext 형식으로 형식화 될 수도 있는 현재 컨텍스트 인스턴스입니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-145">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="c605e-146">`ILazyLoader`-지연 로드 서비스입니다. 자세한 내용은 [지연 로드 설명서](../querying/related-data.md) 를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c605e-146">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="c605e-147">`Action<object, string>`-지연 로드 대리자-자세한 내용은 [지연 로드 설명서](../querying/related-data.md) 를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c605e-147">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="c605e-148">`IEntityType`-이 엔터티 형식과 연결 된 EF Core 메타 데이터</span><span class="sxs-lookup"><span data-stu-id="c605e-148">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="c605e-149">EF Core 2.1를 기준으로 EF Core에서 알려진 서비스만 삽입할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-149">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="c605e-150">응용 프로그램 서비스 삽입에 대 한 지원은 향후 릴리스에 고려 되 고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-150">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="c605e-151">예를 들어 삽입 된 DbContext를 사용 하 여 데이터베이스에 선택적으로 액세스 하 여 관련 엔터티에 대 한 정보를 모두 로드 하지 않고 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-151">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="c605e-152">아래 예제에서는 게시물을 로드 하지 않고 블로그의 게시물 수를 가져오는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-152">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

``` csharp
public class Blog
{
    public Blog()
    {
    }

    private Blog(BloggingContext context)
    {
        Context = context;
    }

    private BloggingContext Context { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; set; }

    public int PostsCount
        => Posts?.Count
           ?? Context?.Set<Post>().Count(p => Id == EF.Property<int?>(p, "BlogId"))
           ?? 0;
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="c605e-153">다음 사항에 유의 하세요.</span><span class="sxs-lookup"><span data-stu-id="c605e-153">A few things to notice about this:</span></span>

* <span data-ttu-id="c605e-154">생성자는 EF Core에 의해서만 호출 되며 일반적으로 사용 되는 다른 public 생성자가 있기 때문에 private입니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-154">The constructor is private, since it is only ever called by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="c605e-155">삽입 된 서비스 (즉, 컨텍스트)를 사용 하는 코드는 EF Core 인스턴스를 만들지 않는 경우를 처리 하기 위해 `null` 되지 않도록 방어 합니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-155">The code using the injected service (that is, the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="c605e-156">서비스는 읽기/쓰기 속성에 저장 되기 때문에 엔터티가 새 컨텍스트 인스턴스에 연결 될 때 다시 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-156">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="c605e-157">이와 같이 DbContext를 삽입 하는 것은 엔터티 형식을 EF Core 직접 결합 때문에 앤티 무늬로 간주 되는 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-157">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="c605e-158">이와 같이 서비스 주입을 사용 하기 전에 모든 옵션을 신중 하 게 고려해 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c605e-158">Carefully consider all options before using service injection like this.</span></span>
