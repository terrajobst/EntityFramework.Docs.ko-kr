---
title: 상속 (관계형 데이터베이스)-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 22eed0002b5903d3cfd18a7e4af0fcd2d46a5c4c
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152365"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="9ca34-102">상속 (관계형 데이터베이스)</span><span class="sxs-lookup"><span data-stu-id="9ca34-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="9ca34-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="9ca34-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="9ca34-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="9ca34-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="9ca34-105">상속 EF 모델에는 데이터베이스에서 엔터티 클래스에서 상속 표현 방법을 제어 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9ca34-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="9ca34-106">현재 계층-테이블 (TPH) 패턴만 EF 코어에서 구현 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9ca34-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="9ca34-107">테이블 콘크리트-형식당 하나의 TPC ()는 사용할 수 없습니다 및 다른 일반 패턴도 같은 테이블 형식당 (TPT).</span><span class="sxs-lookup"><span data-stu-id="9ca34-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="9ca34-108">규칙</span><span class="sxs-lookup"><span data-stu-id="9ca34-108">Conventions</span></span>

<span data-ttu-id="9ca34-109">규칙에 따라 테이블 계층당 (TPH) 패턴을 사용 하 여 상속 매핑되지 것입니다.</span><span class="sxs-lookup"><span data-stu-id="9ca34-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="9ca34-110">TPH 계층의 모든 형식에 대 한 데이터를 저장 하는 단일 테이블을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="9ca34-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="9ca34-111">판별자 열은 각 행이 나타내는 유형을 식별 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9ca34-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="9ca34-112">EF 코어는 두 개 이상의 상속 된 형식이 모델에 명시적으로 포함 된 경우에 상속을 설치 (참조 [상속](../inheritance.md) 자세한 세부 정보에 대 한).</span><span class="sxs-lookup"><span data-stu-id="9ca34-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="9ca34-113">다음은 간단한 상속 시나리오 및 TPH 패턴을 사용 하 여 관계형 데이터베이스 테이블에 저장 된 데이터를 보여 주는 예제입니다.</span><span class="sxs-lookup"><span data-stu-id="9ca34-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="9ca34-114">*판별자* 열 유형을 식별 *블로그* 각 행에 저장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9ca34-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="9ca34-116">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="9ca34-116">Data Annotations</span></span>

<span data-ttu-id="9ca34-117">상속을 구성 하 데이터 주석을 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="9ca34-117">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="9ca34-118">Fluent API</span><span class="sxs-lookup"><span data-stu-id="9ca34-118">Fluent API</span></span>

<span data-ttu-id="9ca34-119">판별자 열 및 계층 구조에서 각 유형을 식별 하는 데 사용 되는 값의 형식과 이름을 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9ca34-119">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="9ca34-120">판별자 속성 구성</span><span class="sxs-lookup"><span data-stu-id="9ca34-120">Configuring the discriminator property</span></span>

<span data-ttu-id="9ca34-121">위의 예에 판별자로 만들어집니다는 [속성 그림자](xref:core/modeling/shadow-properties) 에 계층의 기준 엔터티입니다.</span><span class="sxs-lookup"><span data-stu-id="9ca34-121">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="9ca34-122">모델의 속성 이므로, 다른 속성과 마찬가지로 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9ca34-122">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="9ca34-123">예를 들어, 기본값, 규칙을 통한 판별자를 사용 하는 경우 최대 길이 설정 하려면:</span><span class="sxs-lookup"><span data-stu-id="9ca34-123">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="9ca34-124">판별자는 엔터티의 실제 CLR 속성에도 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9ca34-124">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="9ca34-125">예:</span><span class="sxs-lookup"><span data-stu-id="9ca34-125">For example:</span></span>
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

<span data-ttu-id="9ca34-126">이러한 두 가지를 함께 결합 수 있기를 모두 판별자의 실제 속성 매핑 구성:</span><span class="sxs-lookup"><span data-stu-id="9ca34-126">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
