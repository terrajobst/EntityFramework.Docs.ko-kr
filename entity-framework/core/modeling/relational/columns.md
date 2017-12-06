---
title: "열 매핑-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
ms.technology: entity-framework-core
uid: core/modeling/relational/columns
ms.openlocfilehash: 697b966dbac892e332fe65feaa4dd11f00dd8298
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="column-mapping"></a><span data-ttu-id="83b13-102">열 매핑</span><span class="sxs-lookup"><span data-stu-id="83b13-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="83b13-103">이 섹션의 구성을 일반적 관계형 데이터베이스에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="83b13-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="83b13-104">여기에 표시 된 확장 메서드를 사용할 수 있는 관계형 데이터베이스 공급자를 설치할 때 (공유 인해 *Microsoft.EntityFrameworkCore.Relational* 패키지).</span><span class="sxs-lookup"><span data-stu-id="83b13-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="83b13-105">열 매핑에서 쿼리 하는 및에 데이터베이스에 저장 하는 열 데이터를 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="83b13-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="83b13-106">규칙</span><span class="sxs-lookup"><span data-stu-id="83b13-106">Conventions</span></span>

<span data-ttu-id="83b13-107">규칙에 따라 각 속성은 속성과 동일한 이름 가진 열에 매핑할 설치 됩니다.</span><span class="sxs-lookup"><span data-stu-id="83b13-107">By convention, each property will be setup to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="83b13-108">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="83b13-108">Data Annotations</span></span>

<span data-ttu-id="83b13-109">속성이 매핑되는 열을 구성 하려면 데이터 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="83b13-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=3)] -->
``` csharp
public class Blog
{
    [Column("blog_id")]
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="83b13-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="83b13-110">Fluent API</span></span>

<span data-ttu-id="83b13-111">속성이 매핑되는 열을 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="83b13-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.BlogId)
            .HasColumnName("blog_id");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
