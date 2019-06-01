---
title: 상속 (관계형 데이터베이스)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 2d0a2abc554f5f115479f886ca3f9f4f01b80b5b
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452285"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="1c741-102">상속 (관계형 데이터베이스)</span><span class="sxs-lookup"><span data-stu-id="1c741-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="1c741-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="1c741-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="1c741-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="1c741-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="1c741-105">EF 모델의 상속에는 데이터베이스에서 엔터티 클래스에서 상속은 표시 하는 방법을 제어 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1c741-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="1c741-106">현재, 계층당 하나의 테이블 (TPH) 패턴에만 EF Core에서 구현 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1c741-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="1c741-107">다른 일반 패턴도 테이블 형식당 (TPT) 등의 테이블 구체적인-형식당 하나의 TPC ()는 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="1c741-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="1c741-108">규칙</span><span class="sxs-lookup"><span data-stu-id="1c741-108">Conventions</span></span>

<span data-ttu-id="1c741-109">규칙에 따라 상속 매핑될 당 계층당 테이블 (TPH) 패턴을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="1c741-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="1c741-110">TPH는 계층의 모든 형식에 대 한 데이터를 저장 하는 단일 테이블을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="1c741-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="1c741-111">판별자 열은 각 행이 나타내는 하는 형식을 식별 하기 위해 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1c741-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="1c741-112">두 개 이상의 상속 된 형식이 모델에 명시적으로 포함 된 경우 EF Core에서 상속만 설치 됩니다 (참조 [상속](../inheritance.md) 자세한).</span><span class="sxs-lookup"><span data-stu-id="1c741-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="1c741-113">다음은 간단한 상속 시나리오 및 패턴을 TPH를 사용 하 여 관계형 데이터베이스 테이블에 저장 된 데이터를 보여 주는 예제입니다.</span><span class="sxs-lookup"><span data-stu-id="1c741-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="1c741-114">합니다 *판별자* 열의 형식을 식별 *블로그* 각 행에 저장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1c741-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![이미지](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="1c741-116">데이터베이스 열 때는 자동으로 필요에 따라 null 허용 TPH 매핑이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1c741-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1c741-117">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="1c741-117">Data Annotations</span></span>

<span data-ttu-id="1c741-118">상속을 구성 하려면 데이터 주석을 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="1c741-118">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="1c741-119">Fluent API</span><span class="sxs-lookup"><span data-stu-id="1c741-119">Fluent API</span></span>

<span data-ttu-id="1c741-120">판별자 열 및 계층의 각 형식을 식별 하는 데 사용 되는 값의 종류와 이름을 구성 하는 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1c741-120">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="1c741-121">판별자 속성 구성</span><span class="sxs-lookup"><span data-stu-id="1c741-121">Configuring the discriminator property</span></span>

<span data-ttu-id="1c741-122">위의 예제에서 판별자로 만들어집니다를 [섀도 속성](xref:core/modeling/shadow-properties) 에서 계층의 기준 엔터티입니다.</span><span class="sxs-lookup"><span data-stu-id="1c741-122">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="1c741-123">모델의 속성 이므로 다른 속성과 마찬가지로 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1c741-123">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="1c741-124">예를 들어, 기본값, 규칙에 따라 판별자를 사용 하는 경우 최대 길이 설정 하려면:</span><span class="sxs-lookup"><span data-stu-id="1c741-124">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="1c741-125">판별자 엔터티에 실제 CLR 속성에도 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1c741-125">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="1c741-126">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="1c741-126">For example:</span></span>
```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

<span data-ttu-id="1c741-127">이러한 두 가지를 결합 있기를 모두 매핑하고 판별자의 실제 속성 구성:</span><span class="sxs-lookup"><span data-stu-id="1c741-127">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
