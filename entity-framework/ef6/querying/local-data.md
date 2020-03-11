---
title: 로컬 데이터-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: efd646348d8a18bbeed2d0a0e708d4d36eb26eac
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414484"
---
# <a name="local-data"></a>로컬 데이터
DbSet에 대해 LINQ 쿼리를 직접 실행 하면 항상 데이터베이스에 쿼리가 전송 되지만 DbSet. Local 속성을 사용 하 여 현재 메모리 내 데이터에 액세스할 수 있습니다. DbContext 및 DbContext 메서드를 사용 하 여 EF가 엔터티에 대해 추적 하는 추가 정보에 액세스할 수도 있습니다. 이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

## <a name="using-local-to-look-at-local-data"></a>로컬 데이터를 확인 하기 위해 로컬 사용  

DbSet의 Local 속성은 현재 컨텍스트에서 추적 되 고 있고 삭제 된 것으로 표시 되지 않은 집합의 엔터티에 대 한 간단한 액세스를 제공 합니다. 로컬 속성에 액세스 하면 쿼리가 데이터베이스로 전송 되지 않습니다. 즉, 쿼리를 이미 수행한 후에 일반적으로 사용 됩니다. 로드 확장 메서드를 사용 하 여 컨텍스트가 결과를 추적 하도록 쿼리를 실행할 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs from the database into the context
    context.Blogs.Load();

    // Add a new blog to the context
    context.Blogs.Add(new Blog { Name = "My New Blog" });

    // Mark one of the existing blogs as Deleted
    context.Blogs.Remove(context.Blogs.Find(1));

    // Loop over the blogs in the context.
    Console.WriteLine("In Local: ");
    foreach (var blog in context.Blogs.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }

    // Perform a query against the database.
    Console.WriteLine("\nIn DbSet query: ");
    foreach (var blog in context.Blogs)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }
}
```  

BlogId가 1이 고 BlogId가 2 인 ' ADO.NET Blog ' 라는 두 개의 블로그가 데이터베이스에 있는 경우 다음 출력을 예측할 수 있습니다.  

```console
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

다음은 세 가지 요소를 보여 줍니다.  

- 새 블로그 ' 내 새 블로그 '는 아직 데이터베이스에 저장 되지 않은 경우에도 로컬 컬렉션에 포함 됩니다. 데이터베이스에서 엔터티에 대 한 실제 키를 아직 생성 하지 않았기 때문에이 블로그의 기본 키는 0입니다.  
- ' ADO.NET Blog '는 컨텍스트에서 여전히 추적 되는 경우에도 로컬 컬렉션에 포함 되지 않습니다. DbSet에서 해당 파일을 제거 하 여 삭제 된 것으로 표시 했기 때문입니다.  
- DbSet를 사용 하 여 쿼리를 수행 하는 경우 삭제 하도록 표시 된 블로그 (ADO.NET 블로그)가 결과에 포함 되 고 데이터베이스에 아직 저장 되지 않은 새 블로그 (새 블로그)가 결과에 포함 되지 않습니다. 이는 DbSet이 데이터베이스에 대해 쿼리를 수행 하 고 반환 된 결과에 항상 데이터베이스의 내용이 반영 되기 때문입니다.  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a>로컬를 사용 하 여 컨텍스트에서 엔터티 추가 및 제거  

DbSet의 Local 속성은 컨텍스트의 내용과 동기화 된 상태로 유지 되도록 후크 된 이벤트가 포함 된 [system.collections.objectmodel.observablecollection](https://msdn.microsoft.com/library/ms668604.aspx) 를 반환 합니다. 이는 로컬 컬렉션 또는 DbSet에서 엔터티를 추가 하거나 제거할 수 있음을 의미 합니다. 또한 새 엔터티를 컨텍스트에 가져오는 쿼리를 통해 로컬 컬렉션이 해당 엔터티로 업데이트 됩니다. 예를 들면 다음과 같습니다.  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework")).Load();  

    // Get the local collection and make some changes to it
    var localPosts = context.Posts.Local;
    localPosts.Add(new Post { Name = "What's New in EF" });
    localPosts.Remove(context.Posts.Find(1));  

    // Loop over the posts in the context.
    Console.WriteLine("In Local after entity-framework query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }

    var post1 = context.Posts.Find(1);
    Console.WriteLine(
        "State of post 1: {0} is {1}",
        post1.Name,  
        context.Entry(post1).State);  

    // Query some more posts from the database
    context.Posts.Where(p => p.Tags.Contains("asp.net").Load();  

    // Loop over the posts in the context again.
    Console.WriteLine("\nIn Local after asp.net query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }
}
```  

' Entity framework ' 및 ' asp.net '로 태그가 지정 된 게시물이 몇 가지 있다고 가정 하면 다음과 같은 출력이 표시 될 수 있습니다.  

```console
In Local after entity-framework query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
State of post 1: EF Beginners Guide is Deleted

In Local after asp.net query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
Found 4: ASP.NET Beginners Guide with state Unchanged
```  

다음은 세 가지 요소를 보여 줍니다.  

- 로컬 컬렉션에 추가 된 새 게시물 ' EF의 새로운 기능 '은 추가 된 상태의 컨텍스트에 의해 추적 됩니다. 따라서 SaveChanges가 호출 될 때 데이터베이스에 삽입 됩니다.  
- 로컬 컬렉션 (EF 초보자 가이드)에서 제거 된 게시물이 이제 컨텍스트에서 삭제 된 것으로 표시 되었습니다. 따라서 SaveChanges가 호출 될 때 데이터베이스에서 삭제 됩니다.  
- 두 번째 쿼리를 사용 하 여 컨텍스트에 로드 된 추가 게시물 (ASP.NET 초보자 가이드)이 로컬 컬렉션에 자동으로 추가 됩니다.  

로컬에 주목 해야 하는 한 가지 마지막 사항은 System.collections.objectmodel.observablecollection 된 성능은 많은 엔터티에 적합 하지 않기 때문입니다. 따라서 컨텍스트에서 수천 개의 엔터티를 처리 하는 경우 로컬을 사용 하지 않는 것이 좋습니다.  

## <a name="using-local-for-wpf-data-binding"></a>WPF 데이터 바인딩에 로컬 사용  

DbSet의 로컬 속성은 System.collections.objectmodel.observablecollection의 인스턴스인 WPF 응용 프로그램의 데이터 바인딩에 대해 직접 사용할 수 있습니다. 이전 섹션에서 설명한 것 처럼이는 컨텍스트의 콘텐츠와 자동으로 동기화 된 상태로 유지 되며 컨텍스트의 콘텐츠가 자동으로 동기화 상태로 유지 됨을 의미 합니다. 로컬 컬렉션에는 데이터를 사용 하 여 로컬 컬렉션을 미리 채워야 합니다 .이 경우 로컬 컬렉션에는 데이터베이스 쿼리가 발생 하지 않기 때문입니다.  

이는 전체 WPF 데이터 바인딩 샘플에 적합 한 위치가 아니지만 주요 요소는 다음과 같습니다.  

- 바인딩 소스 설정  
- 집합의 로컬 속성에 바인딩  
- 데이터베이스에 대 한 쿼리를 사용 하 여 로컬을 채웁니다.  

## <a name="wpf-binding-to-navigation-properties"></a>탐색 속성에 WPF 바인딩  

마스터/세부 데이터 바인딩을 수행 하는 경우 엔터티 중 하나의 탐색 속성에 세부 정보 보기를 바인딩할 수 있습니다. 이 작업을 수행 하는 쉬운 방법은 탐색 속성에 System.collections.objectmodel.observablecollection를 사용 하는 것입니다. 예를 들면 다음과 같습니다.  

``` csharp
public class Blog
{
    private readonly ObservableCollection<Post> _posts =
        new ObservableCollection<Post>();

    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual ObservableCollection<Post> Posts
    {
        get { return _posts; }
    }
}
```  

## <a name="using-local-to-clean-up-entities-in-savechanges"></a>로컬를 사용 하 여 SaveChanges에서 엔터티 정리  

대부분의 경우 탐색 속성에서 제거 된 엔터티는 컨텍스트에서 자동으로 삭제 된 것으로 표시 되지 않습니다. 예를 들어 블로그에서 게시 개체를 제거 하는 경우에는 SaveChanges가 호출 될 때 해당 게시물이 자동으로 삭제 되지 않습니다. 이를 삭제 해야 하는 경우에는 SaveChanges를 호출 하기 전에 이러한 현 수 엔터티를 찾아서 삭제 됨으로 표시 해야 합니다. 예를 들면 다음과 같습니다.  

``` csharp
public override int SaveChanges()
{
    foreach (var post in this.Posts.Local.ToList())
    {
        if (post.Blog == null)
        {
            this.Posts.Remove(post);
        }
    }

    return base.SaveChanges();
}
```  

위의 코드는 로컬 컬렉션을 사용 하 여 모든 게시물을 찾고 블로그 참조가 없는 모든 게시물을 삭제 된 것으로 표시 합니다. 그렇지 않으면 열거를 열거 하는 동안 Remove 호출을 통해 컬렉션을 수정 하기 때문에 ToList 호출이 필요 합니다. 대부분의 다른 경우에는 먼저 ToList를 사용 하지 않고 로컬 속성에 대해 직접 쿼리할 수 있습니다.  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a>Windows Forms 데이터 바인딩에 로컬 및 ToBindingList 사용  

Windows Forms는 System.collections.objectmodel.observablecollection를 직접 사용 하는 완전 한 충실도 데이터 바인딩을 지원 하지 않습니다. 그러나 데이터 바인딩에 대해 DbSet Local 속성을 사용 하 여 이전 섹션에서 설명한 모든 이점을 얻을 수 있습니다. 이는 로컬 System.collections.objectmodel.observablecollection 지원 되는 [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) 구현을 만드는 ToBindingList 확장 메서드를 통해 수행 됩니다.  

이는 전체 Windows Forms 데이터 바인딩 샘플에 적합 한 위치가 아니지만 주요 요소는 다음과 같습니다.  

- 개체 바인딩 원본 설정  
- Local. ToBindingList ()를 사용 하 여 집합의 로컬 속성에 바인딩합니다.  
- 데이터베이스에 대 한 쿼리를 사용 하 여 로컬 채우기  

## <a name="getting-detailed-information-about-tracked-entities"></a>추적 된 엔터티에 대 한 자세한 정보 가져오기  

이 시리즈의 많은 예제에서는 Entry 메서드를 사용 하 여 엔터티에 대 한 DbEntityEntry 인스턴스를 반환 합니다. 그런 다음이 항목 개체는 현재 상태와 같은 엔터티에 대 한 정보를 수집 하기 위한 시작 지점 역할을 하며 관련 엔터티를 명시적으로 로드 하는 것과 같은 엔터티에 대 한 작업을 수행 합니다.  

항목 메서드는 컨텍스트에 의해 추적 되는 여러 엔터티 또는 모든 엔터티에 대해 DbEntityEntry 개체를 반환 합니다. 이를 통해 단일 항목 뿐만 아니라 많은 엔터티에서 정보를 수집 하거나 작업을 수행할 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp
using (var context = new BloggingContext())
{
    // Load some entities into the context
    context.Blogs.Load();
    context.Authors.Load();
    context.Readers.Load();

    // Make some changes
    context.Blogs.Find(1).Title = "The New ADO.NET Blog";
    context.Blogs.Remove(context.Blogs.Find(2));
    context.Authors.Add(new Author { Name = "Jane Doe" });
    context.Readers.Find(1).Username = "johndoe1987";

    // Look at the state of all entities in the context
    Console.WriteLine("All tracked entities: ");
    foreach (var entry in context.ChangeTracker.Entries())
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Find modified entities of any type
    Console.WriteLine("\nAll modified entities: ");
    foreach (var entry in context.ChangeTracker.Entries()
                              .Where(e => e.State == EntityState.Modified))
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Get some information about just the tracked blogs
    Console.WriteLine("\nTracked blogs: ");
    foreach (var entry in context.ChangeTracker.Entries<Blog>())
    {
        Console.WriteLine(
            "Found Blog {0}: {1} with original Name {2}",
            entry.Entity.BlogId,  
            entry.Entity.Name,
            entry.Property(p => p.Name).OriginalValue);
    }

    // Find all people (author or reader)
    Console.WriteLine("\nPeople: ");
    foreach (var entry in context.ChangeTracker.Entries<IPerson>())
    {
        Console.WriteLine("Found Person {0}", entry.Entity.Name);
    }
}
```  

예제에 작성자 및 판독기 클래스를 도입 하는 것을 알 수 있습니다. 이러한 클래스는 둘 다 IPerson 인터페이스를 구현 합니다.  

``` csharp
public class Author : IPerson
{
    public int AuthorId { get; set; }
    public string Name { get; set; }
    public string Biography { get; set; }
}

public class Reader : IPerson
{
    public int ReaderId { get; set; }
    public string Name { get; set; }
    public string Username { get; set; }
}

public interface IPerson
{
    string Name { get; }
}
```  

데이터베이스에 다음 데이터가 있다고 가정해 보겠습니다.

BlogId = 1이 고 이름이 ' ADO.NET Blog ' 인 블로그  
BlogId = 2 및 Name = ' Visual Studio 블로그 '를 사용 하는 블로그  
BlogId = 3이 고 이름이 ' .NET Framework Blog ' 인 블로그  
AuthorId = 1이 고 이름이 ' Joe Bloggs ' 인 작성자  
ReaderId = 1이 고 이름이 ' John Doe ' 인 판독기  

코드 실행의 출력은 다음과 같습니다.  

```console
All tracked entities:
Found entity of type Blog with state Modified
Found entity of type Blog with state Deleted
Found entity of type Blog with state Unchanged
Found entity of type Author with state Unchanged
Found entity of type Author with state Added
Found entity of type Reader with state Modified

All modified entities:
Found entity of type Blog with state Modified
Found entity of type Reader with state Modified

Tracked blogs:
Found Blog 1: The New ADO.NET Blog with original Name ADO.NET Blog
Found Blog 2: The Visual Studio Blog with original Name The Visual Studio Blog
Found Blog 3: .NET Framework Blog with original Name .NET Framework Blog

People:
Found Person John Doe
Found Person Joe Bloggs
Found Person Jane Doe
```  

다음 예제에서는 여러 가지 사항을 보여 줍니다.  

- 항목 메서드는 삭제를 포함 하 여 모든 상태의 엔터티에 대 한 항목을 반환 합니다. 이를 로컬과 비교 하 여 삭제 된 엔터티를 제외 합니다.  
- 제네릭이 아닌 항목 메서드를 사용 하는 경우 모든 엔터티 형식에 대 한 항목이 반환 됩니다. 제네릭 항목 메서드를 사용 하는 경우 제네릭 형식의 인스턴스인 엔터티에 대해서만 항목이 반환 됩니다. 이는 모든 블로그의 항목을 가져오기 위해 위에서 사용 되었습니다. 또한 IPerson을 구현 하는 모든 엔터티에 대 한 항목을 가져오는 데 사용 되었습니다. 이는 제네릭 형식이 실제 엔터티 형식이 아닐 수도 있음을 보여 줍니다.  
- 반환 된 결과를 필터링 하는 데 LINQ to Objects를 사용할 수 있습니다. 이는 수정 된 동안 모든 형식의 엔터티를 찾는 데 사용 되었습니다.  

DbEntityEntry 인스턴스는 항상 null이 아닌 엔터티를 포함 합니다. 관계 항목 및 스텁 항목은 DbEntityEntry 인스턴스로 표시 되지 않으므로 이러한 항목을 필터링 할 필요가 없습니다.
