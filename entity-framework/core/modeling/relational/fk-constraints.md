---
title: Foreign Key 제약 조건-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: a83f72b5d832e349fb4a5fb3b2de0b82bd79ef2a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993990"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="d3b50-102">Foreign Key 제약 조건</span><span class="sxs-lookup"><span data-stu-id="d3b50-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="d3b50-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="d3b50-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="d3b50-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="d3b50-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="d3b50-105">모델의 각 관계에 대 한 외래 키 제약 조건을 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="d3b50-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="d3b50-106">규칙</span><span class="sxs-lookup"><span data-stu-id="d3b50-106">Conventions</span></span>

<span data-ttu-id="d3b50-107">규칙에 따라 foreign key 제약 조건 이름은 `FK_<dependent type name>_<principal type name>_<foreign key property name>`합니다.</span><span class="sxs-lookup"><span data-stu-id="d3b50-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="d3b50-108">복합 외래 키에 대 한 `<foreign key property name>` 외래 키 속성 이름에 밑줄 구분 된 목록이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d3b50-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d3b50-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="d3b50-109">Data Annotations</span></span>

<span data-ttu-id="d3b50-110">외래 키 제약 조건 이름은 데이터 주석을 사용 하 여 구성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="d3b50-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d3b50-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="d3b50-111">Fluent API</span></span>

<span data-ttu-id="d3b50-112">관계의 외래 키 제약 조건 이름을 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d3b50-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

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
