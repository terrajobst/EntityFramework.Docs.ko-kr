---
title: 대체 키-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: b26d8bc1630af9e811d9c4e7da850a618bc8042e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996973"
---
# <a name="alternate-keys"></a><span data-ttu-id="36f84-102">대체 키</span><span class="sxs-lookup"><span data-stu-id="36f84-102">Alternate Keys</span></span>

<span data-ttu-id="36f84-103">대체 키를 키 외에도 각 엔터티 인스턴스에 대 한 대체 고유 식별자로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="36f84-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="36f84-104">대체 키 관계의 대상으로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="36f84-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="36f84-105">관계형 데이터베이스를 사용 하는 경우 하나 이상의 외래 키 제약 조건 열을 참조 하는 대체 키 열에 고유 인덱스/제약 조건의 개념에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="36f84-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="36f84-106">대체 키 보다는 고유 인덱스를 원하는 열의 고유성을 적용 하려는 경우 참조 [인덱스](indexes.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="36f84-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="36f84-107">EF의 대체 키를 외래 키의 대상으로 사용할 수 있기 때문에 고유 인덱스에 비해 향상 된 기능을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="36f84-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="36f84-108">대체 키가 필요한 경우에 대 한 일반적으로 도입 하 고 수동으로 구성할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="36f84-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="36f84-109">참조 [규칙](#conventions) 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="36f84-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="36f84-110">규칙</span><span class="sxs-lookup"><span data-stu-id="36f84-110">Conventions</span></span>

<span data-ttu-id="36f84-111">관계의 대상으로 기본 키가 아닌 속성을 식별 하는 경우 규칙에 따라 대체 키를 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="36f84-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/AlternateKey.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogUrl)
            .HasPrincipalKey(b => b.Url);
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

    public string BlogUrl { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="36f84-112">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="36f84-112">Data Annotations</span></span>

<span data-ttu-id="36f84-113">데이터 주석을 사용 하 여 대체 키를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="36f84-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="36f84-114">Fluent API</span><span class="sxs-lookup"><span data-stu-id="36f84-114">Fluent API</span></span>

<span data-ttu-id="36f84-115">대체 키를 단일 속성을 구성 하는 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="36f84-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate);
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```

<span data-ttu-id="36f84-116">또한 (복합 대체 키 라고도 함) 하는 대체 키로 여러 속성을 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="36f84-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```
