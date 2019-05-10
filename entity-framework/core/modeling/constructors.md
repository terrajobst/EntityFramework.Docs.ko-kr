---
title: 생성자-EF Core를 사용 하 여 엔터티 형식
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: 5bf49718f02c1860871b1f4c255ec4d98fce2fc7
ms.sourcegitcommit: 960e42a01b3a2f76da82e074f64f52252a8afecc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65405254"
---
# <a name="entity-types-with-constructors"></a>생성자를 사용 하 여 엔터티 형식

> [!NOTE]  
> 이 기능은 EF Core 2.1의 새로운 기능입니다.

EF Core 2.1부터 있기 이제 엔터티의 인스턴스를 만들 때이 생성자를 호출 하는 EF Core를 매개 변수를 사용 하 여 생성자를 정의 합니다. 생성자 매개 변수에 매핑된 속성에 바인딩할 수 있습니다 또는 동작에 지연 로드와 같은 다양 한 유형의 서비스를 용이 하 게 합니다.

> [!NOTE]  
> EF Core 2.1부터 모든 생성자 바인딩은 일반적입니다. 구성의 특정 생성자를 사용 하 여 향후 릴리스에 예정 되어 있습니다.

## <a name="binding-to-mapped-properties"></a>매핑된 속성에 바인딩

일반적인 블로그/Post 모델이 있다고 가정 합니다.

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

EF Core는 이러한 형식의 인스턴스를 만들 때와 같은 쿼리 결과 대 한 기본 매개 변수가 없는 생성자를 호출 하 고 데이터베이스에서 각 속성 값을 설정 합니다. 그러나 EF Core에서 사용 하 여 매개 변수가 있는 생성자를 발견 한 경우 매개 변수 이름과 일치 하는 매핑된 속성을 다음 해당 속성 값을 사용 하 여 매개 변수가 있는 생성자를 대신 호출 됩니다 하 고 각 속성을 명시적으로 설정 하지 것입니다. 예를 들어:

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
알아두어야 할 사항:
* 일부 속성은 생성자 매개 변수를 가지도록 해야 합니다. 예를 들어, EF Core가 일반적인 방법으로 생성자를 호출한 후에 설정 됩니다 Post.Content 속성 모든 생성자 매개 변수에 의해 설정 되지 않았습니다.
* 매개 변수 형식과 이름을 일치 해야 속성 형식 이름과 속성 될 수 있다는 점이 파스칼식 대/소문자 매개 변수는 카멜식 대 합니다.
* EF Core (블로그 게시물 위의 등)의 탐색 속성을 설정할 수 없습니다는 생성자를 사용 합니다.
* 생성자는 public 일 수, 개인, 또는 다른 액세스 가능성을 갖도록 합니다. 그러나 지연 로드 프록시 생성자를 상속 하는 프록시 클래스에서 액세스할 수 있는지 필요 합니다. 일반적으로 public 또는 protected 있도록 의미 합니다.

### <a name="read-only-properties"></a>읽기 전용 속성

속성 생성자를 통해 설정 되 면 그 중 일부 읽기 전용으로 설정 하는 것이 가능 합니다. EF Core이 기술을 지원 하지만 일부의 사항을 확인 해야 합니다.
* 속성 setter 없이 규칙에 따라 매핑되지 않습니다. (속성 매핑하지 말아야, 계산 된 속성 등을 매핑하는 이렇게 합니다.)
* 자동으로 생성 된 키 값을 사용 하 여 키 값을 새 엔터티를 삽입 하는 경우 키 생성기에서 설정 해야 하므로 읽기 전용을 사용 하는 키 속성이 필요 합니다.

이러한 것을 방지 하는 간편한 방법은 private setter를 사용 하는 것입니다. 예를 들어:
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
EF Core를 읽기 / 쓰기, 즉, 모든 속성이 이전과 매핑되는 키 수는 저장소 생성 될 private setter 사용 하 여 속성을 볼 수 있습니다.

Private setter를 사용 하는 대신 실제로 읽기 전용 속성을 확인 하 고 OnModelCreating에 명시적 매핑을 추가 하는 것입니다. 마찬가지로, 일부 속성이 완전히 제거 고 필드만 바꿀 수 있습니다. 예를 들어 이러한 엔터티 형식:

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
및 OnModelCreating에서이 구성:
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
유의 사항은 다음과 같습니다.
* "Property" 키 필드 되었습니다. 있지는 `readonly` 필드 저장소 생성 키를 사용할 수 있도록 합니다.
* 다른 속성은 생성자 에서만 설정할 읽기 전용 속성입니다.
* 기본 키 값은만 EF에서 설정, 데이터베이스에서 읽은 다음 경우 생성자에 포함할 필요가 없습니다. 이 간단한 필드로 "property" 키를 유지 하 고 선택을 취소 하 고 설정 하지 않아야 명시적으로 새 블로그 또는 게시물을 만들 때 만듭니다.

> [!NOTE]  
> 이 코드는 '169' 나타내는 필드가 사용 되지 않는 경고는 컴파일러에서 발생 합니다. 이 실제로 EF Core는 필드를 사용 하는 extralinguistic 방식 이므로 무시할 수 있습니다.

## <a name="injecting-services"></a>서비스를 주입합니다.

또한 EF Core는 엔터티 형식 생성자에 "서비스"를 주입할 수 있습니다. 예를 들어, 다음 주입할 수 있습니다.
* `DbContext` -현재 컨텍스트 인스턴스에 파생된 DbContext 형식으로 입력할 수 있습니다
* `ILazyLoader` -지연 로드 서비스 참조를 [지연 로드 설명서](../querying/related-data.md) 대 한 자세한 내용은
* `Action<object, string>` -지연 로드 대리자-참조를 [지연 로드 설명서](../querying/related-data.md) 대 한 자세한 내용은
* `IEntityType` -이 엔터티 형식과 연결 된 EF Core 메타 데이터

> [!NOTE]  
> EF Core 2.1부터 EF Core에서 알려진 서비스만 주입할 수 있습니다. 응용 프로그램 서비스를 주입 하는 것에 대 한 지원은 이후 버전으로 간주 되 고 됩니다.

예를 들어, 선택적으로 모두 로드 하지 않고 관련된 엔터티에 대 한 정보를 가져오려면 데이터베이스에 액세스 하는 삽입 된 DbContext은 사용할 수 있습니다. 아래 예제에서 게시물을 로드 하지 않고 블로그 게시물의 번호를 가져오려면이 사용 됩니다.

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
이 주의 해야 할 몇 가지 사항은 다음과 같습니다.
* 생성자 이므로 개인만 EF Core에 의해 호출 되 고 일반 용도로 다른 공용 생성자가입니다.
* 삽입 된 서비스 (즉, 컨텍스트)를 사용 하 여 코드에 대 한 방어는 중인 `null` 경우 EF Core는 인스턴스를 만들지를 처리 합니다.
* 서비스는 읽기/쓰기 속성에 저장 되기 때문에 새 컨텍스트 인스턴스를 엔터티에 연결 되 면 재설정 됩니다.

> [!WARNING]  
> 엔터티 형식에 EF Core에 직접 연결 하므로 이와 같은 DbContext 삽입 안티패턴 종종 간주 됩니다. 다음과 같은 서비스 삽입을 사용 하기 전에 모든 옵션을 신중 하 게 고려 합니다.
