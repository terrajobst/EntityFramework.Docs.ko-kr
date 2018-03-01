---
title: "EF 코어-한 생성자가 있는 엔터티 형식"
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 2632488569c538a11c7a31a9a866d2fadb29eeb5
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="entity-types-with-constructors"></a>생성자가 있는 엔터티 형식

> [!NOTE]  
> 이 기능은 EF 코어 2.1의 새로운 기능입니다.

EF 코어 2.1 이상에서는 되었습니다 매개 변수와 함께 생성자를 정의 하는 엔터티의 인스턴스를 만들 때이 생성자를 호출 하는 EF 코어 수입니다. 매핑된 속성에는 생성자 매개 변수를 바인딩할 수 있습니다 또는 다양 한 종류의 용이 하 게 하려면 서비스에 동작 지연 로드 합니다.

> [!NOTE]  
> EF 코어 2.1을 기준으로 모든 생성자 바인딩은 일반적으로입니다. 특정 생성자를 사용 하는 구성 이후 버전에 대 한 계획 되어 있습니다.

## <a name="binding-to-mapped-properties"></a>매핑된 속성에 바인딩

일반적인 블로그/Post 모델을 고려 합니다.

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

EF 코어 이러한 형식의 인스턴스를 만들 때와 같은 쿼리 결과 대 한 기본 매개 변수가 없는 생성자를 호출 되며 다음 데이터베이스에서 각 속성 값을 설정 합니다. 그러나 EF 코어와 매개 변수가 있는 생성자가 발견 매개 변수 이름과 일치 하는 형식 속성을 매핑할 경우 해당 속성에 대 한 값을 가진 매개 변수가 있는 생성자를 대신 호출 됩니다 하 고 각 속성을 명시적으로 설정 하지 것입니다. 예:

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
몇 가지 사항에 유의 해야 합니다.
* 일부 속성은 생성자 매개 변수 해야 합니다. 예를 들어, EF 코어가 일반적인 방법으로 생성자를 호출한 후에 설정 됩니다 Post.Content 속성 모든 생성자 매개 변수에 의해 설정 되지 않았습니다.
* 매개 변수 형식 및 이름이 일치 해야 속성 형식 및 이름, 속성 될 수 있다는 점이 파스칼식 대/소문자를 사용 하는 동안 매개 변수는 카멜식 대/소문자를 사용 합니다.
* EF 코어 (블로그 게시물 위의 등)의 탐색 속성을 설정할 수 없습니다는 생성자를 사용 합니다.
* 생성자는 공용 일 수, 개인, 또는 다른 수준의 액세스 권한이 부여 됩니다.

### <a name="read-only-properties"></a>읽기 전용 속성

속성은 생성자를 통해 설정 되 면 그 중 일부를 읽기 전용으로 설정 하 여 가능 합니다. EF 코어가 기능이 지원 되지만 하는 데 몇 가지:
* 속성 getter 없이 일반적으로 매핑되지 않습니다. (이렇게 속성을 매핑하지 말아야, 예: 계산 된 속성을 매핑할 경향이 있습니다.)
* 자동으로 생성 된 키 값을 사용 하 여 키 값을 새 엔터티를 삽입할 때 키 생성기에서 설정 해야 하므로 읽기-쓰기는 키 속성이 필요 합니다.

이러한 것 들을 방지 하기 위해 쉽게 개인 setter를 사용 하는 것입니다. 예:
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
EF 코어를 읽기 / 쓰기, 즉 모든 속성이 이전과 매핑되는 및 키 수는 저장소 생성 될 개인 setter 사용 하 여 속성을 볼 수 있습니다.

개인 setter를 사용 하는 대신 실제로 읽기 전용 속성을 확인 하 고 명시적 매핑을 OnModelCreating에 추가 되려고 합니다. 마찬가지로, 일부 속성 완전히 제거 하 고 필드에만 아래 템플릿으로 바뀝니다. 예를 들어 이러한 엔터티 형식:

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
및이 구성 OnModelCreating에서:
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
참고 사항:
* "Property" 키 이제 필드입니다. 없으면는 `readonly` 필드 저장소 생성 키를 사용할 수 있도록 합니다. 
* 다른 속성은 생성자에만 설정 하는 읽기 전용 속성입니다.
* 기본 키 값은만 EF 여 설정, 데이터베이스에서 읽이 없기 생성자에 포함할 필요가 없습니다. 이 간단한 필드와 "property" 키를 유지 하 고 해당 설정 하지 않아야 명시적으로 새 블로그 또는 게시를 만들 때의 선택을 취소 합니다.

> [!NOTE]  
> 이 코드는 컴파일러에 필드가 사용 되지 않는 '169' 나타내는 경고 발생 합니다. 실제로 EF 코어에서 사용 중 이므로 필드 extralinguistic 방식으로 무시할 수 있습니다.

## <a name="injecting-services"></a>서비스를 삽입합니다.

또한 EF 코어는 엔터티 형식 생성자에 "서비스"를 주입할 수 있습니다. 예를 들어 다음 삽입 될 수 있습니다.
* `DbContext` -파생 된 DbContext 형식으로 입력할 수도 있습니다는 현재 컨텍스트 인스턴스
* `ILazyLoader` -지연 로드 서비스--참조는 [지연 로드 설명서](../querying/related-data.md) 자세한 내용을 보려면
* `Action<object, string>` -지연 로드 대리자-참조는 [지연 로드 설명서](../querying/related-data.md) 자세한 내용을 보려면
* `IEntityType` -이 엔터티 형식 연관 된 EF 코어 메타 데이터

> [!NOTE]  
> EF 코어 2.1부터 EF 코어에 알려진 서비스에만 삽입 될 수 있습니다. 응용 프로그램 서비스를 삽입 하는 것에 대 한 지원이 향후 릴리스에 간주 되 고 됩니다.

예를 들어 선택적으로 모두 로드 하지 않고도 관련된 엔터티에 대 한 정보를 가져오려면 데이터베이스에 액세스 하는 삽입 된 DbContext은 사용할 수 있습니다. 아래 예에 있는 게시물을 로드 하지 않고의 블로그 게시물의 수를 가져오는 데는이:

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
이 대 한 감지 하는 데 몇 가지 사항은 다음과 같습니다.
* 가구의 EF 코어에 호출 하지만 일반 용도로 다른 공용 생성자는 생성자가 private, 합니다.
* 삽입 된 서비스를 사용 하 여 코드 (즉, 컨텍스트)에 대 한 방어가 되 고 `null` EF 코어 인스턴스 작성 하지는 경우를 처리 합니다.
* 서비스는 읽기/쓰기 속성에 저장 되기 때문에 엔터티가 새 컨텍스트 인스턴스에 연결 되 면 재설정 됩니다.

> [!WARNING]  
> EF 코어로 직접 엔터티 형식을 결합 하므로 이와 같은 DbContext를 삽입 한 안티패턴 종종 간주 됩니다. 다음과 같이 서비스 삽입을 사용 하기 전에 모든 옵션을 신중 하 게 고려 합니다.
