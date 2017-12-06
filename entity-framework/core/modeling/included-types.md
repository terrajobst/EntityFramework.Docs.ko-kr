---
title: "형식-EF 코어 제외 및 포함"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
ms.technology: entity-framework-core
uid: core/modeling/included-types
ms.openlocfilehash: a8d7293a144968d2506bdcc76e55a1a0b1e3fd4b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="including--excluding-types"></a>포함 하 여 & 형식 제외

형식을 포함 하 여에 모델 EF에 대 한 메타 데이터에를 입력 하 고 읽기와 쓰기 보낸 사람/받는 데이터베이스 인스턴스를 시도 합니다.

## <a name="conventions"></a>규칙

형식에 노출 되는 규칙에 따라 `DbSet` 에서 컨텍스트 속성은 모델에 포함 합니다. 또한 형식에서 설명 하는 `OnModelCreating` 메서드가 포함 됩니다. 마지막으로, 재귀적으로 검색 된 형식의 탐색 속성을 탐색 하 여 발견 되는 형식은 모델에도 포함 됩니다.

**예를 들어 다음 코드 샘플에서는 세 가지 유형 모두 검색 됩니다.**

* `Blog`에 노출 되기 때문에 `DbSet` 컨텍스트의 속성

* `Post`통해 발견 되는 `Blog.Posts` 탐색 속성

* `AuditEntry`에 설명 되어 있으므로`OnModelCreating`

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/IncludedTypes.cs?highlight=3,7,16)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<AuditEntry>();
    }
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

public class AuditEntry
{
    public int AuditEntryId { get; set; }
    public string Username { get; set; }
    public string Action { get; set; }
}
```

## <a name="data-annotations"></a>데이터 주석

모델에서 형식을 제외 하려면 데이터 주석을 사용할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=9)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

[NotMapped]
public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API

모델에서 형식을 제외 하려면 Fluent API를 사용할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Ignore<BlogMetadata>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```
