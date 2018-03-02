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
# <a name="loading-related-data"></a>관련된 데이터를 로드 합니다.

Entity Framework Core를 사용 하면 모델에서 탐색 속성을 사용 하 여 관련된 엔터티 로드 수 있습니다. 관련된 데이터를 로드 하는 데 사용 하는 일반적인 O/RM 패턴 세 가지가 있습니다.
* **즉시 로드** 초기 쿼리의 일부로 관련된 데이터가 데이터베이스에서 로드 될 것을 의미 합니다.
* **명시적 로드** 나중에 관련된 된 데이터는 데이터베이스에서 명시적으로 로드 의미 합니다.
* **지연 로드** 탐색 속성에 액세스할 때 관련된 데이터는 데이터베이스에서 로드 투명 하 게 한다는 것을 의미 합니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)을 볼 수 있습니다.

## <a name="eager-loading"></a>즉시 로드

사용할 수는 `Include` 메서드를 쿼리 결과에 포함할 관련된 데이터를 지정 합니다. 다음 예제에서 결과에 반환 되는 블로그에 포함 됩니다는 `Posts` 속성 게시물 채워집니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Framework Core가 자동으로 픽스업 탐색 속성 컨텍스트 인스턴스에 이전에 로드 된 다른 엔터티를 엔터티 하 고 있습니다. 따라서 탐색 속성에 대 한 데이터를 명시적으로 포함 하지, 경우에 일부 속성이 채울 수 있습니다 또는 이전에 로드 된 모든 관련된 엔터티.


단일 쿼리에 여러 관계에서 관련 된 데이터를 포함할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>여러 수준을 포함 하 여

관계를 사용 하 여 관련된 데이터의 여러 수준이 포함을 통해 드릴 다운할 수 있습니다는 `ThenInclude` 메서드. 다음 예제에서는 모든 블로그, 해당 관련 된 게시물 및 각 게시물의 작성자를 로드합니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> 현재 버전의 Visual Studio는 잘못 된 코드 완성 옵션을 제공 하 고 올바른 식을 사용 하는 경우 구문 오류와 함께 플래그가 지정 될 수 있습니다는 `ThenInclude` 메서드 후 컬렉션 탐색 속성입니다. 이 https://github.com/dotnet/roslyn/issues/8237에서 추적 프로그램 IntelliSense 버그가 있음을 나타냅니다. 으로 코드가 올바른지와 성공적으로 컴파일할 수 있습니다 이러한 잘못 된 구문 오류는 무시 해도 됩니다. 

에 대 한 여러 호출을 체인화할 수 `ThenInclude` 추가 관련된 데이터의 수준을 포함 하 여 계속 합니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

동일한 쿼리에서 여러 수준 및 여러 루트의 관련된 데이터를 포함 하도록이 모든를 결합할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

포함 되 고 있는 엔터티 중 하나에 대 한 여러 관련된 엔터티를 포함 하려고 수 있습니다. 예를 들어, 쿼리할 때 `Blog`포함 s, `Posts` 모두 포함 하려면는 `Author` 및 `Tags` 의 `Posts`합니다. 이 수행 하려면 루트에서 시작 하는 경로 포함 하는 각 지정 해야 합니다. 예를 들어 `Blog -> Posts -> Author` 및 `Blog -> Posts -> Tags`합니다. 받아볼 수 EF 통합 대부분의 경우에서 중복 조인 해 서 조인을 SQL을 생성할 때 것은 아닙니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>파생된 형식에 대해 포함

탐색을 사용 하 여 파생 된 형식에 대해서만 정의할의 관련된 데이터를 포함할 수 있습니다 `Include` 및 `ThenInclude`합니다. 

다음 모델을 가정합니다.

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

내용을 `School` 학생 인 모든 사용자의 탐색 수 로드 되도록 할 다양 한 패턴을 사용 하 여:

- cast 사용
```Csharp
context.People.Include(person => ((Student)person).School).ToList()
```

- 사용 하 여 `as` 연산자
```Csharp
context.People.Include(person => (person as Student).School).ToList()
```

- 오버 로드를 사용 하 여 `Include` 형식의 매개 변수를 사용 하는 `string`
```Csharp
context.People.Include("Student").ToList()
```

### <a name="ignored-includes"></a>무시 포함

쿼리 시작 된 엔터티 형식의 인스턴스를 더 이상 반환 되도록 쿼리를 변경 하면 포함 연산자는 무시 됩니다.

Include 연산자 기반으로 다음 예제에서는 `Blog`, 하지만 다음의 `Select` 연산자는 익명 형식을 반환 하도록 쿼리를 변경 하는 데 사용 됩니다. 이 경우에 포함 연산자 효과가 없습니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

EF 코어 기본적으로 경고를 기록 합니다 포함 경우 연산자는 무시 됩니다. 참조 [로깅](../miscellaneous/logging.md) 로깅 출력 보기에 대 한 자세한 내용은 합니다. Include 연산자를 throw 하거나 아무 작업도 수행 하지를 무시할 경우 동작을 변경할 수 있습니다. 일반적으로 컨텍스트-에 대 한 옵션을 설정할 때 이렇게 `DbContext.OnConfiguring`, 또는 `Startup.cs` ASP.NET Core를 사용 하는 경우.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>명시적 로드

> [!NOTE]  
> 이 기능은 EF 코어 1.1에서 도입 되었습니다.

탐색 속성을 통해 명시적으로 로드할 수는 `DbContext.Entry(...)` API입니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

관련된 엔터티를 반환 하는 별도 쿼리를 실행 하 여 탐색 속성을 명시적으로 로드할 수 있습니다. 변경 추적이 설정 된 경우 엔터티를 로드할 때 EF 코어는 자동으로 새로 로드 된 엔터티를 이미 로드 된 모든 엔터티를 참조의 탐색 속성을 설정 하 고 참조를 이미 로드 된 엔터티의 탐색 속성을 설정 된 새로 로드 된 엔터티입니다.

### <a name="querying-related-entities"></a>관련된 엔터티를 쿼리합니다.

탐색 속성의 내용을 제공 하는 LINQ 쿼리를 가져올 수 있습니다.

이 옵션을 사용 하면 메모리에 로드 하기 없이 관련된 엔터티를 통해 집계 연산자를 실행 하는 등 작업을 수행할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

메모리에는 관련된 엔터티 로드 되어 필터링 할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>지연 로드

> [!NOTE]  
> 이 기능은 EF 코어 2.1에 도입 되었습니다.

지연 로드를 사용 하는 가장 간단한 방법은 설치 된 경우는 [Microsoft.EntityFramworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) 패키지 및을 호출 하 여 설정 되어 있으므로 `UseLazyLoadingProxies`합니다. 예:
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
또는 AddDbContext를 사용 하는 경우:
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
EF 코어 가능한-재정의할 수 있는 탐색 속성에 대 한 지연 로딩이 하면 다음 여야 합니다. `virtual` 및에서 상속 될 수 있는 클래스에 있습니다. 예를 들어 다음과 같은 엔터티는 `Post.Blog` 및 `Blog.Posts` 탐색 속성이 지연 로드 됩니다.
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
### <a name="lazy-loading-without-proxies"></a>프록시 없이 Lazy 로드

Lazy 로드 프록시를 삽입 하 여 작업의 `ILazyLoader` 에 설명 된 대로 엔터티로 서비스 [엔터티 형식 생성자](../modeling/constructors.md)합니다. 예:
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
엔터티 인스턴스를 사용 하 여 만든 있습니다 고 엔터티 형식에서 상속 되어야 또는 가상 탐색 속성에는 필요 하지 않으며이 `new` 지연 로드 한 번에 한 컨텍스트에 연결 합니다. 하지만에 대 한 참조가 필요는 `ILazyLoader` 엔터티 형식 EF 핵심 어셈블리를 결합 하는 서비스입니다. 이 EF 코어를 방지 하기 위해는 `ILazyLoader.Load` 메서드를 대리자로 삽입 합니다. 예:
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
사용 하 여 위의 코드는 `Load` 확장 메서드는 대리자를 사용 하 여 수행 되는 클리너 비트:
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
> 지연 로드 대리자에 대 한 생성자 매개 변수는 "lazyLoader" 호출 되어야 합니다. 이후 버전에 대 한이 계획 되어 있습니다. 다른 이름을 사용할 구성입니다.

## <a name="related-data-and-serialization"></a>관련된 데이터와 serialization

EF 코어가 자동으로 픽스업 탐색 속성을 실행 하 게 주기와 개체 그래프에 때문에 있습니다. 예를 들어, 블로그 및 것 로드 하는 관련 된 게시물 게시물의 컬렉션을 참조 하는 블로그 개체에서 발생 합니다. 각 해당 게시에 대 한 역참조 블로그 갖습니다.

일부 직렬화 프레임 워크는 이러한 주기를 허용 하지 않습니다. 예를 들어 순환이 발생 Json.NET 다음 예외를 throw 합니다.

> Newtonsoft.Json.JsonSerializationException: 자체 'MyApplication.Models.Blog' 형식과 '블로그' 속성에 대 한 검색 된 루프를 참조 합니다.

ASP.NET Core를 사용 하는 경우 개체 그래프에서 발견 된 주기를 무시 하도록 Json.NET을 구성할 수 있습니다. 이 작업은 `ConfigureServices(...)` 메서드에서 `Startup.cs`합니다.

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
