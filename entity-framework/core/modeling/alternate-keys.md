---
title: 대체 키-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: 87df5d174a1db12fb3ab763ac76c3b863a83087e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197469"
---
# <a name="alternate-keys"></a><span data-ttu-id="d8177-102">대체 키</span><span class="sxs-lookup"><span data-stu-id="d8177-102">Alternate Keys</span></span>

<span data-ttu-id="d8177-103">대체 키는 기본 키 외에도 각 엔터티 인스턴스에 대 한 대체 고유 식별자 역할을 합니다.</span><span class="sxs-lookup"><span data-stu-id="d8177-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="d8177-104">대체 키를 관계의 대상으로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d8177-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="d8177-105">관계형 데이터베이스를 사용 하는 경우이는 대체 키 열에 대 한 고유 인덱스/제약 조건 개념 및 열을 참조 하는 하나 이상의 foreign key 제약 조건에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="d8177-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="d8177-106">열의 고유성을 강제 적용 하려는 경우에는 대체 키가 아닌 고유 인덱스가 필요 합니다. [인덱스](indexes.md)를 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="d8177-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="d8177-107">EF에서 대체 키는 외래 키의 대상으로 사용할 수 있기 때문에 고유 인덱스 보다 더 많은 기능을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="d8177-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="d8177-108">일반적으로 필요에 따라 대체 키가 소개 되며 수동으로 구성할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="d8177-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="d8177-109">자세한 내용은 [규칙](#conventions) 을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="d8177-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="d8177-110">규칙</span><span class="sxs-lookup"><span data-stu-id="d8177-110">Conventions</span></span>

<span data-ttu-id="d8177-111">규칙에 따라 기본 키가 아닌 속성을 관계의 대상으로 식별 하는 경우에 대체 키가 도입 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d8177-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/AlternateKey.cs?highlight=12)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="d8177-112">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="d8177-112">Data Annotations</span></span>

<span data-ttu-id="d8177-113">데이터 주석을 사용 하 여 대체 키를 구성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="d8177-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d8177-114">Fluent API</span><span class="sxs-lookup"><span data-stu-id="d8177-114">Fluent API</span></span>

<span data-ttu-id="d8177-115">흐름 API를 사용 하 여 단일 속성을 대체 키로 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d8177-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?highlight=7,8)] -->
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

<span data-ttu-id="d8177-116">흐름 API를 사용 하 여 여러 속성을 대체 키 (복합 대체 키 라고 함)로 구성할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d8177-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?highlight=7,8)] -->
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
