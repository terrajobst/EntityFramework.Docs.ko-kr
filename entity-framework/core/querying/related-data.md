---
title: 관련 데이터 로드 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 65cfea07a40939c1c3615c97ec785a4082b21de5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994790"
---
# <a name="loading-related-data"></a>관련 데이터 로드

Entity Framework Core에서는 모델의 탐색 속성을 사용하여 관련 엔터티를 로드할 수 있습니다. 관련 데이터를 로드하는 데 사용되는 세 개의 일반적인 O/RM 패턴이 있습니다.
* **즉시 로드**는 관련 데이터가 초기 쿼리의 일부로 데이터베이스에서 로드됨을 의미합니다.
* **명시적 로드**는 관련 데이터가 나중에 데이터베이스에서 명시적으로 로드됨을 의미합니다.
* **지연 로드**는 탐색 속성에 액세스할 때 관련 데이터가 데이터베이스에서 투명하게 로드됨을 의미합니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)을 볼 수 있습니다.

## <a name="eager-loading"></a>즉시 로드

`Include` 메서드를 사용하여 쿼리 결과에 포함할 관련 데이터를 지정할 수 있습니다. 다음 예제에서 결과에 반환되는 블로그에는 관련 게시물로 채워진 `Posts` 속성이 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core는 이전에 컨텍스트 인스턴스에 로드된 다른 엔터티로 탐색 속성을 자동으로 수정합니다. 따라서 탐색 속성에 대한 데이터를 명시적으로 포함하지 않더라도 관련 엔터티의 일부 또는 전체가 이전에 로드된 경우 속성이 채워질 수 있습니다.


여러 관계의 관련 데이터를 단일 쿼리에 포함할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>여러 수준 포함

`ThenInclude` 메서드를 사용하여 여러 수준의 관련 데이터를 포함하도록 관계를 드릴다운할 수 있습니다. 다음 예제에서는 모든 블로그, 관련 게시물 및 각 게시물의 작성자를 로드합니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> 현재 버전의 Visual Studio에서는 잘못된 코드 완성 옵션을 제공하므로 컬렉션 탐색 속성 뒤에 `ThenInclude` 메서드를 사용하면 올바른 식에 구문 오류 플래그가 지정될 수 있습니다. 이는 https://github.com/dotnet/roslyn/issues/8237에서 추적되는 IntelliSense 버그의 증상입니다. 코드가 올바르고 성공적으로 컴파일할 수 있는 한 잘못된 구문 오류는 무시해도 안전합니다. 

`ThenInclude`에 대한 여러 호출을 연결하여 관련 데이터의 추가 수준을 계속 포함할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

이 호출을 모두 결합하여 여러 수준 및 여러 루트의 관련 데이터를 동일한 쿼리에 포함할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

포함하려는 엔터티 중 하나에 대한 여러 관련 엔터티를 포함할 수 있습니다. 예를 들어 `Blog`를 쿼리하는 경우 `Posts`를 포함한 다음, `Posts`의 `Author` 및 `Tags`를 모두 포함할 수 있습니다. 이렇게 하려면 루트에서 시작하는 각 포함 경로를 지정해야 합니다. 예를 들어 `Blog -> Posts -> Author` 및 `Blog -> Posts -> Tags`를 지정합니다. 그렇다고 중복 조인을 가져온다는 의미는 아니며, 대부분의 경우 EF에서 SQL을 생성할 때 조인을 통합합니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>파생 형식에 포함

`Include` 및 `ThenInclude`를 사용하여 파생 형식에만 정의된 탐색의 관련 데이터를 포함할 수 있습니다. 

다음과 같은 모델을 가정합니다.

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

다양한 패턴을 사용하여 학생인 모든 사람의 `School` 탐색 콘텐츠를 로드할 수 있습니다.

- 캐스트 사용
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- `as` 연산자 사용
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- `string` 형식의 매개 변수를 사용하는 `Include`의 오버로드 사용
  ```csharp
  context.People.Include("Student").ToList()
  ```

### <a name="ignored-includes"></a>무시된 포함

쿼리가 시작된 엔터티 형식의 인스턴스를 더 이상 반환하지 않도록 쿼리를 변경하면 포함 연산자가 무시됩니다.

다음 예제에서 포함 연산자는 `Blog`를 기반으로 하고, `Select` 연산자는 무명 형식을 반환하도록 쿼리를 변경하는 데 사용됩니다. 이 경우 포함 연산자는 아무 효과가 없습니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

기본적으로 EF Core는 포함 연산자가 무시될 때 경고를 로깅합니다. 로깅 출력 보기에 대한 자세한 내용은 [로깅](../miscellaneous/logging.md)을 참조하세요. 포함 연산자가 무시될 때 동작을 throw나 아무 작업도 하지 않음으로 변경할 수 있습니다. 이 작업은 컨텍스트에 대한 옵션을 설정할 때 수행하며, 일반적으로 `DbContext.OnConfiguring`에서나 `Startup.cs`(ASP.NET Core를 사용하는 경우)에서 수행합니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>명시적 로드

> [!NOTE]  
> 이 기능은 EF Core 1.1에서 도입되었습니다.

`DbContext.Entry(...)` API를 통해 탐색 속성을 명시적으로 로드할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

관련 엔터티를 반환하는 별도의 쿼리를 실행하여 탐색 속성을 명시적으로 로드할 수도 있습니다. 변경 내용 추적이 사용되는 경우 엔터티를 로드하면 EF Core는 새로 로드된 엔터티의 탐색 속성을 이미 로드된 엔터티를 참조하도록 자동으로 설정하고, 이미 로드된 엔터티의 탐색 속성을 새로 로드된 엔터티를 참조하도록 설정합니다.

### <a name="querying-related-entities"></a>관련 엔터티 쿼리

탐색 속성의 내용을 나타내는 LINQ 쿼리를 가져올 수도 있습니다.

따라서 메모리로 로드하지 않고도 관련 엔터티에 대해 집계 연산자를 실행하는 것과 같은 작업을 수행할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

메모리로 로드되는 관련 엔터티를 필터링할 수도 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>지연 로드

> [!NOTE]  
> 이 기능은 EF Core 2.1에서 도입되었습니다.

지연 로드를 사용하는 가장 간단한 방법은 [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) 패키지를 설치하고 이를 사용하여 `UseLazyLoadingProxies`를 호출하는 것입니다. 예:
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
또는 AddDbContext를 사용하는 경우:
```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```
EF Core에서 재정의할 수 있는 모든 탐색 속성에 대해 지연 로드를 사용합니다. 즉, `virtual`이어야 하고 상속될 수 있는 클래스에 있어야 합니다. 예를 들어 다음 엔터티에서 `Post.Blog` 및 `Blog.Posts` 탐색 속성은 지연 로드됩니다.
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
### <a name="lazy-loading-without-proxies"></a>프록시 없는 지연 로드

지연 로드 프록시는 [엔터티 형식 생성자](../modeling/constructors.md)에 설명된 대로 `ILazyLoader` 서비스를 엔터티에 삽입하여 작동합니다. 예:
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
이 경우에는 상속되는 엔터티 형식이나 탐색 속성이 가상일 필요가 없으며 컨텍스트에 연결되면 `new`로 만든 엔터티 인스턴스가 지연 로드되도록 합니다. 하지만 [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) 패키지에 정의된 `ILazyLoader` 서비스에 대한 참조가 필요합니다. 이 패키지에는 최소의 형식 집합이 포함되어 있으므로 이 패키지에 따른 영향이 거의 없습니다. 그러나 엔터티 형식의 EF Core 패키지를 전혀 사용하지 않으려면 `ILazyLoader.Load` 메서드를 대리자로 삽입할 수 있습니다. 예:
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
위의 코드에서는 `Load` 확장 메서드를 사용하여 대리자 사용을 좀 더 깔끔하게 합니다.
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
> 지연 로드 대리자에 대한 생성자 매개 변수를 “lazyLoader”라고 합니다. 이와 다른 이름을 사용하는 구성은 향후 릴리스에 포함될 예정입니다.

## <a name="related-data-and-serialization"></a>관련 데이터 및 serialization

EF Core는 탐색 속성을 자동으로 수정하므로 개체 그래프의 주기로 끝날 수 있습니다. 예를 들어 블로그와 관련 게시물을 로드하면 게시물 모음을 참조하는 블로그 개체가 생성됩니다. 각 게시물은 다시 블로그를 참조합니다.

일부 serialization 프레임워크에서는 이러한 주기를 허용하지 않습니다. 예를 들어 주기가 발생하면 Json.NET은 다음 예외를 throw합니다.

> Newtonsoft.Json.JsonSerializationException: 형식이 ‘MyApplication.Models.Blog’인 ‘Blog’ 속성에 대해 자체 참조 루프가 검색되었습니다.

ASP.NET Core를 사용하는 경우 개체 그래프에서 찾은 주기를 무시하도록 Json.NET을 구성할 수 있습니다. 이 작업은 `Startup.cs`의 `ConfigureServices(...)` 메서드에서 수행됩니다.

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
