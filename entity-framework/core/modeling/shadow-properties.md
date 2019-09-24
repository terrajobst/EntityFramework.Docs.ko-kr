---
title: 그림자 속성-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 5fdc4c50c295f73d0fa5eef3518adf4d3eb95599
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197707"
---
# <a name="shadow-properties"></a>섀도 속성

섀도 속성은 .NET 엔터티 클래스에 정의 되어 있지 않지만 EF Core 모델에서 해당 엔터티 형식에 대해 정의 되는 속성입니다. 이러한 속성의 값과 상태는 전적으로 변경 추적에서 유지 관리 됩니다.

섀도 속성은 매핑된 엔터티 형식에 노출 되지 않아야 하는 데이터가 데이터베이스에 있는 경우에 유용 합니다. 이러한 속성은 외래 키 속성에 사용 되는 경우가 가장 많습니다. 여기서 두 엔터티 간의 관계는 데이터베이스의 외래 키 값으로 표시 되지만 엔터티 형식 간의 탐색 속성을 사용 하 여 엔터티 형식에서 관계가 관리 됩니다.

API를 `ChangeTracker` 통해 섀도 속성 값을 가져오고 변경할 수 있습니다.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

정적 메서드를 `EF.Property` 통해 LINQ 쿼리에서 섀도 속성을 참조할 수 있습니다.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>규칙

그림자 속성은 관계가 검색 되었지만 종속 엔터티 클래스에 외래 키 속성이 없는 경우 규칙에 따라 만들 수 있습니다. 이 경우에는 섀도 외래 키 속성이 도입 됩니다. 섀도 외래 키 속성의 이름은 `<navigation property name><principal key property name>` 로 지정 됩니다 (주 엔터티를 가리키는 종속 엔터티에 대 한 탐색은 명명에 사용 됨). 주 키 속성 이름에 탐색 속성의 이름이 포함 된 경우이 이름은 `<principal key property name>`입니다. 종속 엔터티에 대 한 탐색 속성이 없는 경우 주 형식 이름이 대신 사용 됩니다.

예를 들어 다음 코드 목록에서는 `BlogId` `Post` 엔터티에 대해 섀도 속성이 도입 됩니다.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/ShadowForeignKey.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a>데이터 주석

데이터 주석으로는 섀도 속성을 만들 수 없습니다.

## <a name="fluent-api"></a>Fluent API

흐름 API를 사용 하 여 섀도 속성을 구성할 수 있습니다. 의 `Property` 문자열 오버 로드를 호출한 후에는 다른 속성에 대 한 구성 호출을 연결할 수 있습니다.

`Property` 메서드에 제공 된 이름이 기존 속성의 이름 (shadow 속성 또는 엔터티 클래스에 정의 된 속성)과 일치 하는 경우 코드는 새 shadow 속성을 도입 하는 대신 기존 속성을 구성 합니다.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/ShadowProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property<DateTime>("LastUpdated");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
