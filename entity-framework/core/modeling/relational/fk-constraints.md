---
title: "Foreign Key 제약 조건-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
ms.technology: entity-framework-core
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 726f03e2ee4cd3ec851c9a861b75dd12f9203e9c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="18520-102">외래 키 제약 조건</span><span class="sxs-lookup"><span data-stu-id="18520-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="18520-103">이 섹션의 구성을 일반적 관계형 데이터베이스에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="18520-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="18520-104">여기에 표시 된 확장 메서드를 사용할 수 있는 관계형 데이터베이스 공급자를 설치할 때 (공유 인해 *Microsoft.EntityFrameworkCore.Relational* 패키지).</span><span class="sxs-lookup"><span data-stu-id="18520-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="18520-105">모델의 각 관계에 대 한 외래 키 제약 조건 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="18520-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="18520-106">규칙</span><span class="sxs-lookup"><span data-stu-id="18520-106">Conventions</span></span>

<span data-ttu-id="18520-107">규칙에 따라 foreign key 제약 조건 이름은 `FK_<dependent type name>_<principal type name>_<foreign key property name>`합니다.</span><span class="sxs-lookup"><span data-stu-id="18520-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="18520-108">복합 외래 키에 대 한 `<foreign key property name>` 외래 키 속성 이름에 밑줄 구분 된 목록이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="18520-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="18520-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="18520-109">Data Annotations</span></span>

<span data-ttu-id="18520-110">외래 키 제약 조건 이름은 데이터 주석을 사용 하 여 구성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="18520-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="18520-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="18520-111">Fluent API</span></span>

<span data-ttu-id="18520-112">관계의 외래 키 제약 조건 이름을 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="18520-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/RelationshipConstraintName.cs?highlight=12)] -->
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
            .HasForeignKey(p => p.BlogId)
            .HasConstraintName("ForeignKey_Post_Blog");
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

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```
