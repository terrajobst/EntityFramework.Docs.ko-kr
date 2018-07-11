---
title: 생성자-EF Core를 사용 하 여 엔터티 형식
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 80c7ee04d3bb0dd45b66ec7d6fec5ee7e3343026
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949207"
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="c6e64-102">생성자를 사용 하 여 엔터티 형식</span><span class="sxs-lookup"><span data-stu-id="c6e64-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="c6e64-103">이 기능은 EF Core 2.1의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="c6e64-104">EF Core 2.1부터 있기 이제 엔터티의 인스턴스를 만들 때이 생성자를 호출 하는 EF Core를 매개 변수를 사용 하 여 생성자를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="c6e64-105">생성자 매개 변수에 매핑된 속성에 바인딩할 수 있습니다 또는 동작에 지연 로드와 같은 다양 한 유형의 서비스를 용이 하 게 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="c6e64-106">EF Core 2.1부터 모든 생성자 바인딩은 일반적입니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="c6e64-107">구성의 특정 생성자를 사용 하 여 향후 릴리스에 예정 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="c6e64-108">매핑된 속성에 바인딩</span><span class="sxs-lookup"><span data-stu-id="c6e64-108">Binding to mapped properties</span></span>

<span data-ttu-id="c6e64-109">일반적인 블로그/Post 모델이 있다고 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-109">Consider a typical Blog/Post model:</span></span>

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

<span data-ttu-id="c6e64-110">EF Core는 이러한 형식의 인스턴스를 만들 때와 같은 쿼리 결과 대 한 기본 매개 변수가 없는 생성자를 호출 하 고 데이터베이스에서 각 속성 값을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="c6e64-111">그러나 EF Core에서 사용 하 여 매개 변수가 있는 생성자를 발견 한 경우 매개 변수 이름과 일치 하는 매핑된 속성을 다음 해당 속성 값을 사용 하 여 매개 변수가 있는 생성자를 대신 호출 됩니다 하 고 각 속성을 명시적으로 설정 하지 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="c6e64-112">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="c6e64-112">For example:</span></span>

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
<span data-ttu-id="c6e64-113">알아두어야 할 사항:</span><span class="sxs-lookup"><span data-stu-id="c6e64-113">Some things to note:</span></span>
* <span data-ttu-id="c6e64-114">일부 속성은 생성자 매개 변수를 가지도록 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="c6e64-115">예를 들어, EF Core가 일반적인 방법으로 생성자를 호출한 후에 설정 됩니다 Post.Content 속성 모든 생성자 매개 변수에 의해 설정 되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="c6e64-116">매개 변수 형식과 이름을 일치 해야 속성 형식 이름과 속성 될 수 있다는 점이 파스칼식 대/소문자 매개 변수는 카멜식 대 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="c6e64-117">EF Core (블로그 게시물 위의 등)의 탐색 속성을 설정할 수 없습니다는 생성자를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="c6e64-118">생성자는 public 일 수, 개인, 또는 다른 액세스 가능성을 갖도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-118">The constructor can be public, private, or have any other accessibility.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="c6e64-119">읽기 전용 속성</span><span class="sxs-lookup"><span data-stu-id="c6e64-119">Read-only properties</span></span>

<span data-ttu-id="c6e64-120">속성 생성자를 통해 설정 되 면 그 중 일부 읽기 전용으로 설정 하는 것이 가능 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-120">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="c6e64-121">EF Core이 기술을 지원 하지만 일부의 사항을 확인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-121">EF Core supports this, but there are some things to look out for:</span></span>
* <span data-ttu-id="c6e64-122">속성 setter 없이 규칙에 따라 매핑되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-122">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="c6e64-123">(속성 매핑하지 말아야, 계산 된 속성 등을 매핑하는 이렇게 합니다.)</span><span class="sxs-lookup"><span data-stu-id="c6e64-123">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="c6e64-124">자동으로 생성 된 키 값을 사용 하 여 키 값을 새 엔터티를 삽입 하는 경우 키 생성기에서 설정 해야 하므로 읽기 전용을 사용 하는 키 속성이 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-124">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="c6e64-125">이러한 것을 방지 하는 간편한 방법은 private setter를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-125">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="c6e64-126">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="c6e64-126">For example:</span></span>
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
<span data-ttu-id="c6e64-127">EF Core를 읽기 / 쓰기, 즉, 모든 속성이 이전과 매핑되는 키 수는 저장소 생성 될 private setter 사용 하 여 속성을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-127">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="c6e64-128">Private setter를 사용 하는 대신 실제로 읽기 전용 속성을 확인 하 고 OnModelCreating에 명시적 매핑을 추가 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-128">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="c6e64-129">마찬가지로, 일부 속성이 완전히 제거 고 필드만 바꿀 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-129">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="c6e64-130">예를 들어 이러한 엔터티 형식:</span><span class="sxs-lookup"><span data-stu-id="c6e64-130">For example, consider these entity types:</span></span>

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
<span data-ttu-id="c6e64-131">및 OnModelCreating에서이 구성:</span><span class="sxs-lookup"><span data-stu-id="c6e64-131">And this configuration in OnModelCreating:</span></span>
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
<span data-ttu-id="c6e64-132">유의 사항은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-132">Things to note:</span></span>
* <span data-ttu-id="c6e64-133">"Property" 키 필드 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-133">The key "property" is now a field.</span></span> <span data-ttu-id="c6e64-134">있지는 `readonly` 필드 저장소 생성 키를 사용할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-134">It is not a `readonly` field so that store-generated keys can be used.</span></span>
* <span data-ttu-id="c6e64-135">다른 속성은 생성자 에서만 설정할 읽기 전용 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-135">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="c6e64-136">기본 키 값은만 EF에서 설정, 데이터베이스에서 읽은 다음 경우 생성자에 포함할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-136">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="c6e64-137">이 간단한 필드로 "property" 키를 유지 하 고 선택을 취소 하 고 설정 하지 않아야 명시적으로 새 블로그 또는 게시물을 만들 때 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-137">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="c6e64-138">이 코드는 '169' 나타내는 필드가 사용 되지 않는 경고는 컴파일러에서 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-138">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="c6e64-139">이 실제로 EF Core는 필드를 사용 하는 extralinguistic 방식 이므로 무시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-139">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="c6e64-140">서비스를 주입합니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-140">Injecting services</span></span>

<span data-ttu-id="c6e64-141">또한 EF Core는 엔터티 형식 생성자에 "서비스"를 주입할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-141">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="c6e64-142">예를 들어, 다음 주입할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-142">For example, the following can be injected:</span></span>
* <span data-ttu-id="c6e64-143">`DbContext` -현재 컨텍스트 인스턴스에 파생된 DbContext 형식으로 입력할 수 있습니다</span><span class="sxs-lookup"><span data-stu-id="c6e64-143">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="c6e64-144">`ILazyLoader` -지연 로드 서비스 참조를 [지연 로드 설명서](../querying/related-data.md) 대 한 자세한 내용은</span><span class="sxs-lookup"><span data-stu-id="c6e64-144">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="c6e64-145">`Action<object, string>` -지연 로드 대리자-참조를 [지연 로드 설명서](../querying/related-data.md) 대 한 자세한 내용은</span><span class="sxs-lookup"><span data-stu-id="c6e64-145">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="c6e64-146">`IEntityType` -이 엔터티 형식과 연결 된 EF Core 메타 데이터</span><span class="sxs-lookup"><span data-stu-id="c6e64-146">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="c6e64-147">EF Core 2.1부터 EF Core에서 알려진 서비스만 주입할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-147">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="c6e64-148">응용 프로그램 서비스를 주입 하는 것에 대 한 지원은 이후 버전으로 간주 되 고 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-148">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="c6e64-149">예를 들어, 선택적으로 모두 로드 하지 않고 관련된 엔터티에 대 한 정보를 가져오려면 데이터베이스에 액세스 하는 삽입 된 DbContext은 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-149">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="c6e64-150">아래 예제에서 게시물을 로드 하지 않고 블로그 게시물의 번호를 가져오려면이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-150">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

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
<span data-ttu-id="c6e64-151">이 주의 해야 할 몇 가지 사항은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-151">A few things to notice about this:</span></span>
* <span data-ttu-id="c6e64-152">생성자 이므로 개인만 EF Core에 의해 호출 되 고 일반 용도로 다른 공용 생성자가입니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-152">The constructor is private, since it is only ever called by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="c6e64-153">삽입 된 서비스 (즉, 컨텍스트)를 사용 하 여 코드에 대 한 방어는 중인 `null` 경우 EF Core는 인스턴스를 만들지를 처리 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-153">The code using the injected service (that is, the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="c6e64-154">서비스는 읽기/쓰기 속성에 저장 되기 때문에 새 컨텍스트 인스턴스를 엔터티에 연결 되 면 재설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-154">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="c6e64-155">엔터티 형식에 EF Core에 직접 연결 하므로 이와 같은 DbContext 삽입 안티패턴 종종 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-155">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="c6e64-156">다음과 같은 서비스 삽입을 사용 하기 전에 모든 옵션을 신중 하 게 고려 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6e64-156">Carefully consider all options before using service injection like this.</span></span>
