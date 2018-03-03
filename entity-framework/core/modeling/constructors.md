---
title: "EF 코어-한 생성자가 있는 엔터티 형식"
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 38ab0c1c3cd8c490875abf30b8478c99bc58630f
ms.sourcegitcommit: 60b831318c4f5ec99061e8af6a7c9e7c03b3469c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/02/2018
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="c1cfa-102">생성자가 있는 엔터티 형식</span><span class="sxs-lookup"><span data-stu-id="c1cfa-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="c1cfa-103">이 기능은 EF 코어 2.1의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="c1cfa-104">EF 코어 2.1 이상에서는 되었습니다 매개 변수와 함께 생성자를 정의 하는 엔터티의 인스턴스를 만들 때이 생성자를 호출 하는 EF 코어 수입니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="c1cfa-105">매핑된 속성에는 생성자 매개 변수를 바인딩할 수 있습니다 또는 다양 한 종류의 용이 하 게 하려면 서비스에 동작 지연 로드 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="c1cfa-106">EF 코어 2.1을 기준으로 모든 생성자 바인딩은 일반적으로입니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="c1cfa-107">특정 생성자를 사용 하는 구성 이후 버전에 대 한 계획 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="c1cfa-108">매핑된 속성에 바인딩</span><span class="sxs-lookup"><span data-stu-id="c1cfa-108">Binding to mapped properties</span></span>

<span data-ttu-id="c1cfa-109">일반적인 블로그/Post 모델을 고려 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-109">Consider a typical Blog/Post model:</span></span>

```Csharp
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

<span data-ttu-id="c1cfa-110">EF 코어 이러한 형식의 인스턴스를 만들 때와 같은 쿼리 결과 대 한 기본 매개 변수가 없는 생성자를 호출 되며 다음 데이터베이스에서 각 속성 값을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="c1cfa-111">그러나 EF 코어와 매개 변수가 있는 생성자가 발견 매개 변수 이름과 일치 하는 형식 속성을 매핑할 경우 해당 속성에 대 한 값을 가진 매개 변수가 있는 생성자를 대신 호출 됩니다 하 고 각 속성을 명시적으로 설정 하지 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="c1cfa-112">예:</span><span class="sxs-lookup"><span data-stu-id="c1cfa-112">For example:</span></span>

```Csharp
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
<span data-ttu-id="c1cfa-113">몇 가지 사항에 유의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-113">Some things to note:</span></span>
* <span data-ttu-id="c1cfa-114">일부 속성은 생성자 매개 변수 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="c1cfa-115">예를 들어, EF 코어가 일반적인 방법으로 생성자를 호출한 후에 설정 됩니다 Post.Content 속성 모든 생성자 매개 변수에 의해 설정 되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="c1cfa-116">매개 변수 형식 및 이름이 일치 해야 속성 형식 및 이름, 속성 될 수 있다는 점이 파스칼식 대/소문자를 사용 하는 동안 매개 변수는 카멜식 대/소문자를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="c1cfa-117">EF 코어 (블로그 게시물 위의 등)의 탐색 속성을 설정할 수 없습니다는 생성자를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="c1cfa-118">생성자는 공용 일 수, 개인, 또는 다른 수준의 액세스 권한이 부여 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-118">The constructor can be public, private, or have any other accessibility.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="c1cfa-119">읽기 전용 속성</span><span class="sxs-lookup"><span data-stu-id="c1cfa-119">Read-only properties</span></span>

<span data-ttu-id="c1cfa-120">속성은 생성자를 통해 설정 되 면 그 중 일부를 읽기 전용으로 설정 하 여 가능 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-120">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="c1cfa-121">EF 코어가 기능이 지원 되지만 하는 데 몇 가지:</span><span class="sxs-lookup"><span data-stu-id="c1cfa-121">EF Core supports this, but there are some things to look out for:</span></span>
* <span data-ttu-id="c1cfa-122">속성 setter 없이 일반적으로 매핑되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-122">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="c1cfa-123">(이렇게 속성을 매핑하지 말아야, 예: 계산 된 속성을 매핑할 경향이 있습니다.)</span><span class="sxs-lookup"><span data-stu-id="c1cfa-123">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="c1cfa-124">자동으로 생성 된 키 값을 사용 하 여 키 값을 새 엔터티를 삽입할 때 키 생성기에서 설정 해야 하므로 읽기-쓰기는 키 속성이 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-124">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="c1cfa-125">이러한 것 들을 방지 하기 위해 쉽게 개인 setter를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-125">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="c1cfa-126">예:</span><span class="sxs-lookup"><span data-stu-id="c1cfa-126">For example:</span></span>
```Csharp
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
<span data-ttu-id="c1cfa-127">EF 코어를 읽기 / 쓰기, 즉 모든 속성이 이전과 매핑되는 및 키 수는 저장소 생성 될 개인 setter 사용 하 여 속성을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-127">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="c1cfa-128">개인 setter를 사용 하는 대신 실제로 읽기 전용 속성을 확인 하 고 명시적 매핑을 OnModelCreating에 추가 되려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-128">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="c1cfa-129">마찬가지로, 일부 속성 완전히 제거 하 고 필드에만 아래 템플릿으로 바뀝니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-129">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="c1cfa-130">예를 들어 이러한 엔터티 형식:</span><span class="sxs-lookup"><span data-stu-id="c1cfa-130">For example, consider these entity types:</span></span>

```Csharp
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
<span data-ttu-id="c1cfa-131">및이 구성 OnModelCreating에서:</span><span class="sxs-lookup"><span data-stu-id="c1cfa-131">And this configuration in OnModelCreating:</span></span>
```Csharp
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
<span data-ttu-id="c1cfa-132">참고 사항:</span><span class="sxs-lookup"><span data-stu-id="c1cfa-132">Things to note:</span></span>
* <span data-ttu-id="c1cfa-133">"Property" 키 이제 필드입니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-133">The key "property" is now a field.</span></span> <span data-ttu-id="c1cfa-134">없으면는 `readonly` 필드 저장소 생성 키를 사용할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-134">It is not a `readonly` field so that store-generated keys can be used.</span></span> 
* <span data-ttu-id="c1cfa-135">다른 속성은 생성자에만 설정 하는 읽기 전용 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-135">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="c1cfa-136">기본 키 값은만 EF 여 설정, 데이터베이스에서 읽이 없기 생성자에 포함할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-136">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="c1cfa-137">이 간단한 필드와 "property" 키를 유지 하 고 해당 설정 하지 않아야 명시적으로 새 블로그 또는 게시를 만들 때의 선택을 취소 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-137">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="c1cfa-138">이 코드는 컴파일러에 필드가 사용 되지 않는 '169' 나타내는 경고 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-138">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="c1cfa-139">실제로 EF 코어에서 사용 중 이므로 필드 extralinguistic 방식으로 무시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-139">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="c1cfa-140">서비스를 삽입합니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-140">Injecting services</span></span>

<span data-ttu-id="c1cfa-141">또한 EF 코어는 엔터티 형식 생성자에 "서비스"를 주입할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-141">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="c1cfa-142">예를 들어 다음 삽입 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-142">For example, the following can be injected:</span></span>
* <span data-ttu-id="c1cfa-143">`DbContext` -파생 된 DbContext 형식으로 입력할 수도 있습니다는 현재 컨텍스트 인스턴스</span><span class="sxs-lookup"><span data-stu-id="c1cfa-143">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="c1cfa-144">`ILazyLoader` -지연 로드 서비스--참조는 [지연 로드 설명서](../querying/related-data.md) 자세한 내용을 보려면</span><span class="sxs-lookup"><span data-stu-id="c1cfa-144">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="c1cfa-145">`Action<object, string>` -지연 로드 대리자-참조는 [지연 로드 설명서](../querying/related-data.md) 자세한 내용을 보려면</span><span class="sxs-lookup"><span data-stu-id="c1cfa-145">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="c1cfa-146">`IEntityType` -이 엔터티 형식 연관 된 EF 코어 메타 데이터</span><span class="sxs-lookup"><span data-stu-id="c1cfa-146">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="c1cfa-147">EF 코어 2.1부터 EF 코어에 알려진 서비스에만 삽입 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-147">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="c1cfa-148">응용 프로그램 서비스를 삽입 하는 것에 대 한 지원이 향후 릴리스에 간주 되 고 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-148">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="c1cfa-149">예를 들어 선택적으로 모두 로드 하지 않고도 관련된 엔터티에 대 한 정보를 가져오려면 데이터베이스에 액세스 하는 삽입 된 DbContext은 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-149">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="c1cfa-150">아래 예에 있는 게시물을 로드 하지 않고의 블로그 게시물의 수를 가져오는 데는이:</span><span class="sxs-lookup"><span data-stu-id="c1cfa-150">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

```Csharp
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
<span data-ttu-id="c1cfa-151">이 대 한 감지 하는 데 몇 가지 사항은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-151">A few things to notice about this:</span></span>
* <span data-ttu-id="c1cfa-152">가구의 EF 코어에 호출 하지만 일반 용도로 다른 공용 생성자는 생성자가 private, 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-152">The constructor is private, since it is only ever call by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="c1cfa-153">삽입 된 서비스를 사용 하 여 코드 (즉, 컨텍스트)에 대 한 방어가 되 고 `null` EF 코어 인스턴스 작성 하지는 경우를 처리 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-153">The code using the injected service (i.e. the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="c1cfa-154">서비스는 읽기/쓰기 속성에 저장 되기 때문에 엔터티가 새 컨텍스트 인스턴스에 연결 되 면 재설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-154">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="c1cfa-155">EF 코어로 직접 엔터티 형식을 결합 하므로 이와 같은 DbContext를 삽입 한 안티패턴 종종 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-155">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="c1cfa-156">다음과 같이 서비스 삽입을 사용 하기 전에 모든 옵션을 신중 하 게 고려 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1cfa-156">Carefully consider all options before using service injection like this.</span></span>
