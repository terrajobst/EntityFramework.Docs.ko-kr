---
title: 관련 엔터티-EF6 로드
author: divega
ms.date: 10/23/2016
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: 2d33d9db8acc61f7d556e3eca46b1ea90198723e
ms.sourcegitcommit: 15022dd06d919c29b1189c82611ea32f9fdc6617
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47415759"
---
# <a name="loading-related-entities"></a>관련된 엔터티 로드
Entity Framework는 선행 로딩, 지연 된 로드 및 명시적 로드 관련 된 데이터를 로드 하는 세 가지를 지원 합니다. 이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

## <a name="eagerly-loading"></a>즉시 로드  

즉시 로드는 여기서 한 형식의 엔터티에 대 한 쿼리를 관련된 엔터티도 로드를 쿼리의 일부로 프로세스. 즉시 로드는 Include 메서드를 사용 하 여 수행 됩니다. 예를 들어 아래 쿼리에서 블로그 및 관련 된 각 블로그 게시물을 모든 로드 됩니다.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                        .Include(b => b.Posts)
                        .ToList();

    // Load one blogs and its related posts
    var blog1 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include(b => b.Posts)
                       .FirstOrDefault();

    // Load all blogs and related posts  
    // using a string to specify the relationship
    var blogs2 = context.Blogs
                        .Include("Posts")
                        .ToList();

    // Load one blog and its related posts  
    // using a string to specify the relationship
    var blog2 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include("Posts")
                       .FirstOrDefault();
}
```  

따라서 Include System.Data.Entity 네임 스페이스의 확장 메서드는 해당 네임 스페이스를 사용 하 고 있는지 확인 합니다.  

### <a name="eagerly-loading-multiple-levels"></a>즉시 로드 하는 여러 수준  

도 적극적으로 여러 수준의 관련된 엔터티를 로드 하는 것이 가능 합니다. 아래 쿼리에서 컬렉션 및 참조 탐색 속성에 대 한이 작업을 수행 하는 방법의 예제를 보여 줍니다.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                        .Include(b => b.Posts.Select(p => p.Comments))
                        .ToList();

    // Load all users, their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                        .Include("Posts.Comments")
                        .ToList();

    // Load all users, their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

필터는 관련된 엔터티가 로드 되어 현재 수는 note 합니다. 포함은 항상 모든 관련된 엔터티를 가져옵니다.  

## <a name="lazy-loading"></a>지연 로드  

지연 로드는 가능해 집니다 엔터티 또는 엔터티 컬렉션은 자동으로 데이터베이스에서 처음 로드 엔터티/엔터티를 참조 하는 속성에 액세스 하는 프로세스입니다. POCO 엔터티 형식을 사용 하는 경우에 지연 로드 프록시 파생된 형식의 인스턴스를 만들고 다음 로드 후크를 추가 하려면 가상 속성을 재정의 하 여 이루어집니다. 예를 들어, 아래에 정의 된 블로그 엔터티 클래스를 사용 하는 경우 관련된 게시물 로드할 처음 게시물 탐색 속성에 액세스 합니다.  

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

### <a name="turn-lazy-loading-off-for-serialization"></a>지연 serialization에 대 한 오프 로드 설정  

지연 로드 하 고 직렬화도 혼합 금지 하 고 주의 하지 않으면 지연 로드가 사용 하도록 설정 해 서 전체 데이터베이스에 대 한 쿼리를 종료할 수 있습니다. 대부분의 serializer 형식의 인스턴스에서 각 속성에 액세스 하 여 작동 합니다. 속성 액세스는 더 많은 엔터티 serialize 하므로 지연 로딩을 트리거합니다. 해당 엔터티에서 속성에 액세스 하 고 더 많은 엔터티가 로드 됩니다. 지연 엔터티를 serialize 하기 전에 오프 로드 설정 하는 것이 좋습니다. 다음 섹션에서는이 작업을 수행 하는 방법을 보여 줍니다.  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a>특정 탐색 속성에 대 한 로드 지연 해제  

게시물 속성 비가상 만들어 게시물 컬렉션의 지연 로드를 해제할 수 있습니다.  

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

게시물의 로드 컬렉션도 수행할 수 있습니다 즉시 로드를 사용 하 여 (참조 *즉시 로드* 위에) 또는 Load 메서드 (참조 *명시적으로 로드* 아래).  

### <a name="turn-off-lazy-loading-for-all-entities"></a>모든 엔터티에 대 한 로드 지연 해제  

구성 속성에 플래그를 설정 하 여 컨텍스트에서 모든 엔터티에 대 한 지연 로드 해제 수 있습니다. 예를 들어:  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

즉시 로드를 사용 하 여 관련된 엔터티 로드 수행 여전히 수 있습니다 (참조 *즉시 로드* 위에) 또는 Load 메서드 (참조 *명시적으로 로드* 아래).  

## <a name="explicitly-loading"></a>명시적으로 로드  

사용 하지 않도록 설정 하는 지연 로드를 사용 하 여도 관련된 엔터티의 지연 로드 수는 있지만 명시적으로 호출을 사용 하 여 수행 해야 합니다. 이렇게 하려면 관련된 엔터티의 항목 Load 메서드를 사용 합니다. 예를 들어:  

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string  
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog  
    // using a string to specify the relationship
    context.Entry(blog).Collection("Posts").Load();
}
```  

참고 엔터티가 참조 메서드를 사용 해야는 다른 단일 엔터티로 탐색 속성이 있습니다. 반면에 수집 방법은 때 사용할 엔터티의 탐색 속성이 다른 엔터티 컬렉션에 있습니다.  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a>명시적으로 관련된 엔터티를 로드 하는 경우 필터를 적용 합니다.  

쿼리 메서드에 관련된 엔터티를 로드 하는 경우 Entity Framework를 사용 하는 기본 쿼리에 대 한 액세스를 제공 합니다. ToList, 부하 등과 같은 LINQ 확장 메서드를 호출 하 여 실행 하기 전에 쿼리에 필터를 적용 한 다음 LINQ를 사용할 수 있습니다. 쿼리 메서드에 대 한 참조 및 컬렉션 탐색 속성을 사용 하 여 사용할 수 있지만 컬렉션에 대 한 가장 유용한 컬렉션의 일부만 로드를 사용할 수 있습니다. 예를 들어:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
           .Collection(b => b.Posts)
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
           .Collection("Posts")
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();
}
```  

쿼리 메서드를 사용 하는 경우에 기능을 해제 하려면 지연 로드는 탐색 속성에 대 한 일반적으로 좋습니다. 그렇지 않으면 전체 컬렉션을 수 있습니다 로드 자동으로 지연 로드 메커니즘에 의해 필터링 된 쿼리가 실행 된 전후 때문입니다.  

Note는 람다 식 대신 문자열로 관계를 지정할 수 있지만 반환 되는 IQueryable 제네릭이 아닌 문자열을 사용 하는 경우 고 캐스트 메서드는 일반적으로 지나야 유용한 것을 사용 하 여 수행할 수 있습니다.  

## <a name="using-query-to-count-related-entities-without-loading-them"></a>쿼리를 사용 하 여 관련된 엔터티 로드 하지 않고 계산  

경우에 따라 실제로 이러한 모든 엔터티를 로드 하는 비용 없이 데이터베이스의 다른 엔터티와 관련 엔터티 인지 알고 있어야 유용 합니다. 이렇게 하려면 LINQ 계산 방법 사용 하 여 쿼리 메서드를 사용할 수 있습니다. 예를 들어:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has  
    var postCount = context.Entry(blog)
                           .Collection(b => b.Posts)
                           .Query()
                           .Count();
}
```  
