---
title: 상속 (관계형 데이터베이스)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 381d1878007bb78b359eb49649f4356f1e5eb04a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655637"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="5777b-102">상속(관계형데이터베이스)</span><span class="sxs-lookup"><span data-stu-id="5777b-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="5777b-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="5777b-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="5777b-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="5777b-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="5777b-105">EF 모델의 상속은 데이터베이스에서 엔터티 클래스의 상속이 표시 되는 방식을 제어 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5777b-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="5777b-106">현재는 EF Core에서 TPH (계층당 하나의 테이블) 패턴만 구현 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5777b-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="5777b-107">형식당 하나의 테이블 (TPT) 및 TPC (테이블당)와 같은 기타 일반적인 패턴은 아직 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="5777b-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="5777b-108">규칙</span><span class="sxs-lookup"><span data-stu-id="5777b-108">Conventions</span></span>

<span data-ttu-id="5777b-109">규칙에 따라 상속은 TPH (계층당 하나의 테이블) 패턴을 사용 하 여 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="5777b-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="5777b-110">TPH는 단일 테이블을 사용 하 여 계층의 모든 형식에 대 한 데이터를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="5777b-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="5777b-111">판별자 열은 각 행이 나타내는 유형을 식별 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5777b-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="5777b-112">EF Core는 둘 이상의 상속 된 형식이 모델에 명시적으로 포함 된 경우에만 상속을 설정 합니다 (자세한 내용은 [상속](../inheritance.md) 참조).</span><span class="sxs-lookup"><span data-stu-id="5777b-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="5777b-113">다음은 TPH 패턴을 사용 하 여 관계형 데이터베이스 테이블에 저장 된 데이터와 간단한 상속 시나리오를 보여 주는 예입니다.</span><span class="sxs-lookup"><span data-stu-id="5777b-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="5777b-114">*판별자* 열은 각 행에 저장 되는 *블로그의* 유형을 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="5777b-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![이미지](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="5777b-116">TPH 매핑을 사용 하는 경우 필요에 따라 데이터베이스 열이 자동으로 nullable로 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5777b-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5777b-117">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="5777b-117">Data Annotations</span></span>

<span data-ttu-id="5777b-118">데이터 주석은 상속을 구성 하는 데 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="5777b-118">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="5777b-119">흐름 API</span><span class="sxs-lookup"><span data-stu-id="5777b-119">Fluent API</span></span>

<span data-ttu-id="5777b-120">흐름 API를 사용 하 여 판별자 열의 이름 및 유형과 계층의 각 유형을 식별 하는 데 사용 되는 값을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5777b-120">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="5777b-121">판별자 속성 구성</span><span class="sxs-lookup"><span data-stu-id="5777b-121">Configuring the discriminator property</span></span>

<span data-ttu-id="5777b-122">위의 예제에서 판별자는 계층의 기본 엔터티에서 [그림자 속성](xref:core/modeling/shadow-properties) 으로 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5777b-122">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="5777b-123">이 속성은 모델의 속성 이므로 다른 속성과 마찬가지로 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5777b-123">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="5777b-124">예를 들어, 기본 규칙 판별자를 사용 하는 경우 최대 길이를 설정 하려면 다음을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="5777b-124">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="5777b-125">판별자는 엔터티의 실제 CLR 속성에 매핑될 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5777b-125">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="5777b-126">예를 들면,</span><span class="sxs-lookup"><span data-stu-id="5777b-126">For example:</span></span>

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

<span data-ttu-id="5777b-127">이러한 두 항목을 함께 결합 하 여 판별자를 실제 속성에 매핑하고 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5777b-127">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>

```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
