---
title: 형식을 제외한 & 포함-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: ca83b1c432bdf4853dba81e12ec4a739bc8218dc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197380"
---
# <a name="including--excluding-types"></a>형식 포함 및 제외

모델에 형식을 포함 하면 EF는 해당 형식에 대 한 메타 데이터를 포함 하 고 데이터베이스에서 인스턴스를 읽고 쓰는 것을 시도 합니다.

## <a name="conventions"></a>규칙

규칙에 따라 컨텍스트의 속성에 `DbSet` 노출 되는 형식이 모델에 포함 됩니다. 또한 `OnModelCreating` 메서드에 언급 된 형식도 포함 됩니다. 마지막으로 검색 된 형식의 탐색 속성을 재귀적으로 탐색 하 여 발견 되는 모든 형식은 모델에도 포함 됩니다.

**예를 들어 다음 코드 목록에서는 세 가지 형식이 모두 검색 됩니다.**

* `Blog`컨텍스트의 `DbSet` 속성에 노출 되기 때문에

* `Post`탐색 속성을 `Blog.Posts` 통해 검색 되기 때문에

* `AuditEntry`에 설명 되어 있으므로`OnModelCreating`

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/IncludedTypes.cs?highlight=3,7,16)] -->
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

데이터 주석을 사용 하 여 모델에서 형식을 제외할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>Fluent API

흐름 API를 사용 하 여 모델에서 형식을 제외할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
