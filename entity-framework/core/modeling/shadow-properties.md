---
title: 섀도 속성-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: b7b7b10642564dfa3dbc05755188b5b5c63e0d03
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993804"
---
# <a name="shadow-properties"></a>섀도 속성

섀도 속성은.NET 엔터티 클래스에 정의 되지 않은 하지만 EF Core 모델에서 해당 엔터티 형식에 대해 정의 된 속성입니다. 값 및 이러한 속성의 상태를 변경 추적기에서 순수 하 게 유지 됩니다.

섀도 속성 매핑된 엔터티 형식에서 노출 되지 않아야 하는 데이터베이스에 데이터가 있을 때 유용 합니다. 여기서 두 엔터티 간의 관계 데이터베이스의 외래 키 값이 표현 하지만 관계 엔터티 형식 간의 탐색 속성을 사용 하 여 엔터티 형식에 대해 관리 되는 외래 키 속성에 대해 가장 자주 사용 됩니다.

섀도 속성 값을 구한 후 통해 변경할 수는 `ChangeTracker` API.

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

섀도 속성을 통해 LINQ 쿼리에서 참조할 수 있습니다는 `EF.Property` 정적 메서드입니다.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>규칙

섀도 속성 관계 검색 되지만 외래 키 속성이 종속 엔터티 클래스에 위치한 경우 규칙에 따라 만들 수 있습니다. 이 경우 그림자 외래 키 속성을 소개 합니다. 섀도 외래 키 속성의 이름은 `<navigation property name><principal key property name>` (탐색 창에 종속 엔터티가 주 엔터티를 가리키는 명명 사용 됨). 주 키 속성 이름에는 탐색 속성의 이름을 포함 되어 있으면 이름이 됩니다 `<principal key property name>`합니다. 종속 엔터티의 탐색 속성이 없는 경우에 보안 주체 유형 이름은 해당 위치에 사용 됩니다.

예를 들어, 다음 코드 목록은 하면를 `BlogId` 소개 되 섀도 속성은 `Post` 엔터티.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/ShadowForeignKey.cs)] -->
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

데이터 주석을 사용한 섀도 속성을 만들 수 있습니다.

## <a name="fluent-api"></a>Fluent API

섀도 속성을 구성 하는 Fluent API를 사용할 수 있습니다. 문자열 오버 로드를 호출한 후 `Property` 다른 속성의 경우 구성 호출을 연결할 수 있습니다.

이름을 제공 하는 경우는 `Property` 섀도 속성, 엔터티 클래스에 정의 된 기존 속성의 이름을 일치 하는 메서드가 다음 코드는 새 섀도 속성을 소개 하는 것이 아니라 기존 속성에 구성 됩니다.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/ShadowProperty.cs?highlight=7,8)] -->
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
