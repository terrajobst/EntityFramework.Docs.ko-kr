---
title: 형식-EF Core 포함 및 제외
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: a5a14f62524754fed179e9a41fac5e29faf185ca
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996152"
---
# <a name="including--excluding-types"></a><span data-ttu-id="e1ba6-102">형식 포함 및 제외</span><span class="sxs-lookup"><span data-stu-id="e1ba6-102">Including & Excluding Types</span></span>

<span data-ttu-id="e1ba6-103">입력 하 고 읽기 및 쓰기 데이터베이스를 인스턴스 하려고 하는 EF에 대 한 메타 데이터에는 모델 수단에 형식을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="e1ba6-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="e1ba6-104">규칙</span><span class="sxs-lookup"><span data-stu-id="e1ba6-104">Conventions</span></span>

<span data-ttu-id="e1ba6-105">형식에서 노출 되는 규칙에 따라 `DbSet` 컨텍스트에서 속성이 모델에 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e1ba6-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="e1ba6-106">또한에 언급 된 형식에는 `OnModelCreating` 메서드도 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e1ba6-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="e1ba6-107">마지막으로 재귀적으로 검색 된 형식의 탐색 속성을 탐색 하 여 발견 되는 모든 형식 모델에도 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e1ba6-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="e1ba6-108">**예를 들어, 다음 코드 샘플에서는 세 가지 유형은 모두 검색 됩니다.**</span><span class="sxs-lookup"><span data-stu-id="e1ba6-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="e1ba6-109">`Blog` 에 공개 되어 있으므로 `DbSet` 컨텍스트 속성</span><span class="sxs-lookup"><span data-stu-id="e1ba6-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="e1ba6-110">`Post` 통해 발견 되는 `Blog.Posts` 탐색 속성</span><span class="sxs-lookup"><span data-stu-id="e1ba6-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="e1ba6-111">`AuditEntry` 에 설명 되어 있으므로 `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="e1ba6-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="e1ba6-112">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="e1ba6-112">Data Annotations</span></span>

<span data-ttu-id="e1ba6-113">데이터 주석은 모델에서 형식을 제외 하려면 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e1ba6-113">You can use Data Annotations to exclude a type from the model.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="e1ba6-114">Fluent API</span><span class="sxs-lookup"><span data-stu-id="e1ba6-114">Fluent API</span></span>

<span data-ttu-id="e1ba6-115">모델에서 형식을 제외 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e1ba6-115">You can use the Fluent API to exclude a type from the model.</span></span>

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
