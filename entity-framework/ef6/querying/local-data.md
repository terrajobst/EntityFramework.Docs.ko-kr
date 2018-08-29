---
title: EF6 로컬 데이터
author: divega
ms.date: 2016-10-23
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: dac1a1de20398501c706b118443743d47970df17
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994276"
---
# <a name="local-data"></a>로컬 데이터
DbSet에 대해 직접 LINQ 쿼리를 실행는 항상 보낼 쿼리를 데이터베이스에 있지만 현재 메모리 내 DbSet.Local 속성을 사용 하는 데이터에 액세스할 수 있습니다. 또한 DbContext.Entry 및 DbContext.ChangeTracker.Entries 메서드를 사용 하 여 엔터티에 대 한 EF가 추적 하는 추가 정보를 액세스할 수 있습니다. 이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

## <a name="using-local-to-look-at-local-data"></a>로컬을 사용 하 여 로컬 데이터 확인  

DbSet의 로컬 속성을 컨텍스트에 의해 추적 되 고 현재는 삭제로 표시 되지 않은 엔터티 집합의에 대 한 간단한 액세스를 제공 합니다. 데이터베이스를 전송할 수 있도록 쿼리를 생성 하지 않습니다 로컬 속성에 액세스 합니다. 이 쿼리를 이미 수행 된 후 일반적으로 사용 되는 것을 의미 합니다. Load 확장 메서드는 결과 추적 하는 컨텍스트는 쿼리를 실행 하 사용할 수 있습니다. 예를 들어:  

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

두 개의 블로그에서-' ADO.NET' 1 BlogId 사용 하 여-블로그와 ' Visual Studio 블로그 ' 2 BlogId 사용 하 여 데이터베이스에 있으면 다음과 같은 출력이 기대할 수 없습니다.  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

이 세 개의 점을 보여 줍니다.  

- 새 블로그 ' 내 새 블로그 ' 아직 데이터베이스에 저장 되지 않은 경우에 로컬 컬렉션에 포함 됩니다. 이 블로그는 엔터티에 대 한 실제 키를 아직 생성 되지 않은 데이터베이스 때문에 0의 기본 키를 있습니다.  
- ' ADO.NET 블로그 ' 여전히 컨텍스트에 의해 추적 되는 경우에 로컬 컬렉션에 포함 되지 않습니다. 즉, DbSet 있으므로 삭제 됨으로 표시에서 제거 했습니다.  
- DbSet을 쿼리 하는 데 사용 되는 경우에 삭제 (ADO.NET 블로그)에 대 한 표시 블로그 결과에 포함 되 고 데이터베이스에 아직 저장 되지 않은 새 블로그 (내 새 블로그) 결과에 포함 되지 않습니다. 이 데이터베이스에 대해 쿼리를 수행 하는 DbSet는 항상 반환 된 결과 데이터베이스에 포함 된 내용 반영 때문입니다.  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a>에서 추가 및 제거할 엔터티 컨텍스트를 로컬을 사용 하 여  

DbSet에 로컬 속성을 반환 합니다는 [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) 컨텍스트의 콘텐츠로 동기화 상태로 유지 되도록 후크 이벤트를 사용 하 여 합니다. 즉, 엔터티 추가 또는 로컬 컬렉션 또는 DbSet에서 제거 될 수 있습니다. 즉, 컨텍스트에 새 엔터티를 제공 하는 쿼리가 해당 엔터티를 사용 하 여 업데이트 되 고 로컬 컬렉션에 포함 됩니다. 예를 들어:  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework").Load();  

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

' Entity framework' 및 'asp.net' 출력을 사용 하 여 태그가 지정 된 몇 가지 게시물 했습니다 가정 하 고 코드는 다음과 같을 수 있습니다.  

```  
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

이 세 개의 점을 보여 줍니다.  

- '의 새로운 EF' 새 게시물이 추가 된 로컬 컬렉션 Added 상태의 컨텍스트에 의해 추적 됩니다. 따라서 삽입 되도록 데이터베이스로 SaveChanges가 호출 될 때입니다.  
- 로컬 컬렉션 (EF 초보자 가이드)에서 제거 된 게시물 컨텍스트에서 삭제 된 것으로 표시 됩니다. 이 따라서 삭제할 데이터베이스에서 SaveChanges가 호출 될 때입니다.  
- 두 번째 쿼리를 사용 하 여 컨텍스트에 로드 추가 게시물 (ASP.NET 초보자 가이드)는 자동으로 로컬 컬렉션에 추가 됩니다.  

로컬에서 주목 해야 할 사항이 하나 이므로 ObservableCollection 성능은 다 수의 엔터티 좋지 않습니다. 따라서 컨텍스트에서 수천 개의 엔터티로 처리 하는 경우 없을 로컬을 사용 하는 것이 좋습니다.  

## <a name="using-local-for-wpf-data-binding"></a>WPF 데이터 바인딩에 대 한 로컬을 사용 하 여  

DbSet에 로컬 속성 ObservableCollection 인스턴스에 있기 때문에 직접 WPF 응용 프로그램에서 데이터 바인딩에 사용할 수 있습니다. 즉, 자동으로 이전 섹션에 설명 된 대로 컨텍스트의 내용과 동기화 상태를 유지 하 고 컨텍스트의 콘텐츠는 자동으로 된 동기화를 유지 합니다. 로컬 데이터베이스 쿼리를 생성 하지 않습니다 때문에 바인딩할 항목으로 여기에 대 한 데이터를 사용 하 여 로컬 컬렉션을 미리 채울 필요가 note 합니다.  

이 전체 WPF 데이터 바인딩 샘플에 대 한 적절 한 위치 하지만 주요 요소는:  

- 바인딩 소스를 설정 합니다.  
- 집합의 로컬 속성에 바인딩  
- 로컬을 데이터베이스에 쿼리를 사용 하 여 채웁니다.  

## <a name="wpf-binding-to-navigation-properties"></a>탐색 속성을 WPF 바인딩  

수행 하려는 경우 마스터/세부 정보 데이터 바인딩을 사용 하면 세부 정보 뷰에서 엔터티 중 하나는 탐색 속성에 바인딩할 수도 있습니다. 이 작업을 수행 하는 쉬운 방법을 탐색 속성에 대 한 ObservableCollection를 사용 하는 것입니다. 예를 들어:  

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

## <a name="using-local-to-clean-up-entities-in-savechanges"></a>로컬을 사용 하 여 SaveChanges의 엔터티를 정리 하려면  

대부분의 경우에서 엔터티는 탐색 속성에서 제거 표시 되지 않습니다 자동으로 컨텍스트에서 삭제 합니다. 예를 들어, 게시 하는 다음 Blog.Posts 컬렉션에서 Post 개체를 제거 하면 됩니다 수 자동으로 삭제 되지 SaveChanges가 호출 될 때입니다. 삭제할 필요할 경우 현 수 이러한 엔터티를 찾아 SaveChanges를 호출 하기 전에 또는 재정의 SaveChanges의 일부로 삭제 된 것으로 표시 해야 할 수 있습니다. 예를 들어:  

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

위의 코드는 모든 게시물 및 모든 없는 블로그에 대 한 참조를 삭제 된 것으로 표시를 찾으려면 로컬 컬렉션을 사용 합니다. ToList 호출 이므로 필요를 제거 하 여 컬렉션을 수정할 수는 그렇지 않은 경우 열거 되는 동안 호출 합니다. 대부분의 경우에서 ToList를 먼저 사용 하지 않고 로컬 속성에 대해 직접 쿼리할 수 있습니다.  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a>로컬 및 ToBindingList for Windows Forms 데이터 바인딩 사용  

Windows Forms에는 ObservableCollection를 직접 사용 하 여 완전 한 충실도 데이터 바인딩을 지원 하지 않습니다. 그러나 이전 섹션에 설명 된 모든 이점을 누릴 수 데이터 바인딩 위한 DbSet 로컬 속성을 계속 사용할 수 있습니다. 만드는 ToBindingList 확장 메서드를 통해 얻을 수는 [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) 로컬 ObservableCollection 구현을 지원 합니다.  

이 전체 Windows Forms 데이터 바인딩 샘플에 대 한 적절 한 위치 하지만 주요 요소는:  

- 개체 바인딩 소스를 설정 합니다.  
- Local.ToBindingList()를 사용 하 여 집합의 로컬 속성에 바인딩  
- 로컬 데이터베이스에 쿼리를 사용 하 여 채우기  

## <a name="getting-detailed-information-about-tracked-entities"></a>추적 된 엔터티에 대 한 자세한 정보 가져오기  

이 시리즈의 예제에서는 많은 엔터티에 대 한 DbEntityEntry 인스턴스를 반환 하려면 진입점 메서드를 사용 합니다. 이 항목 개체의 현재 상태와 같은 엔터티에 대 한 정보를 수집 및 관련된 엔터티를 명시적으로 로드와 같은 엔터티에 대 한 작업을 수행 하기 위한 시작 점으로 사용 합니다.  

항목 메서드 컨텍스트에 의해 추적 되는 많은 또는 모든 엔터티의 DbEntityEntry 개체를 반환 합니다. 이 정보를 수집 또는 단일 항목 뿐 아니라 많은 엔터티에서 작업을 수행할 수 있습니다. 예를 들어:  

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

예제에는 작성자 및 판독기 클래스를 도입한-IPerson 인터페이스를 구현 하는 두이 클래스 모두 알 수 있습니다.  

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

다음 데이터를 데이터베이스에 있는 경우를 가정해 봅니다.

BlogId 사용 하 여 블로그 = 1, 이름 = ' ADO.NET 블로그 '  
BlogId 사용 하 여 블로그 = 2, 이름 = 'Visual Studio 블로그'  
BlogId 사용 하 여 블로그 = 3, 이름 = '.NET Framework 블로그'  
AuthorId 작성 = 1, 이름 = ' Joe가 '  
ReaderId 사용 하 여 판독기 = 1, 이름 = ' John Doe'  

코드를 실행 한 결과 다음과 같습니다.  

```  
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

이러한 예제에는 여러 지점을 보여 줍니다.  

- 항목 메서드는 삭제를 비롯 한 모든 상태의 엔터티에 대 한 항목을 반환 합니다. 비교 하려면이 옵션을 제외 하는 로컬 엔터티를 삭제 합니다.  
- 모든 엔터티 형식에 대 한 항목은 제네릭이 아닌 항목 메서드를 사용 하는 경우 반환 됩니다. 일반 항목 메서드를 사용할 때 항목이 제네릭 형식의 인스턴스는 엔터티에 대 한만 반환 됩니다. 이 블로그 모두에 대 한 항목을 가져오려면 위의 사용 되었습니다. 또한 IPerson를 구현 하는 모든 엔터티에 대 한 항목을 가져오려고 에서도 사용 되었습니다. 이 제네릭 형식 실제 엔터티 형식일 필요가 없습니다 보여 줍니다.  
- 반환 된 결과 필터링 할 개체에는 LINQ는 사용할 수 있습니다. 이 수정 될 때 모든 형식의 엔터티를 찾을 수 위에서 사용 되었습니다.  

참고 DbEntityEntry 인스턴스에 항상 null이 아닌 엔터티를 포함 합니다. 관계 항목과 스텁 항목으로 표현 되지 않는 DbEntityEntry 인스턴스 이므로 이러한 항목에 대해 필터링 할 필요가 없습니다.
