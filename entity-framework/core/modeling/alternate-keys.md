---
title: 대체 키-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
ms.technology: entity-framework-core
uid: core/modeling/alternate-keys
ms.openlocfilehash: 09f86a8932b71ec8f30ee90a088091a00233c20f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052473"
---
# <a name="alternate-keys"></a><span data-ttu-id="9e5d9-102">대체 키</span><span class="sxs-lookup"><span data-stu-id="9e5d9-102">Alternate Keys</span></span>

<span data-ttu-id="9e5d9-103">대체 키의 기본 키 외에도 각 엔터티 인스턴스에 대 한 대체 고유 식별자로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9e5d9-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="9e5d9-104">대체 키 관계의 대상으로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9e5d9-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="9e5d9-105">관계형 데이터베이스를 사용 하는 경우에 대체 키 열과 하나 이상의 외래 키 제약 조건이 있는 열을 참조 하는 고유 인덱스/제약 조건의 개념에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="9e5d9-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="9e5d9-106">대체 키 보다는 고유 인덱스를 원하는 다음 열의 고유성을 강제 적용 하려면 참조 [인덱스](indexes.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="9e5d9-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="9e5d9-107">EF, 대체 키 외래 키의 대상으로 사용할 수 있기 때문에 고유 인덱스에 비해 향상 된 기능을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="9e5d9-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="9e5d9-108">필요할 때에 대 한 대체 키 일반적으로 도입 하 고 수동으로 구성할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="9e5d9-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="9e5d9-109">참조 [규칙](#conventions) 내용을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="9e5d9-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="9e5d9-110">규칙</span><span class="sxs-lookup"><span data-stu-id="9e5d9-110">Conventions</span></span>

<span data-ttu-id="9e5d9-111">기본 키 관계의 대상으로 없는 속성을 식별 하는 경우 규칙에 따라 대체 키를 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="9e5d9-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="9e5d9-112">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="9e5d9-112">Data Annotations</span></span>

<span data-ttu-id="9e5d9-113">데이터 주석을 사용 하 여 대체 키를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9e5d9-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="9e5d9-114">Fluent API</span><span class="sxs-lookup"><span data-stu-id="9e5d9-114">Fluent API</span></span>

<span data-ttu-id="9e5d9-115">대체 키 수를 단일 속성을 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9e5d9-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

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

<span data-ttu-id="9e5d9-116">대체 키 (복합 대체 키로 알려짐)를 여러 속성을 구성을 Fluent API를 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9e5d9-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

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
