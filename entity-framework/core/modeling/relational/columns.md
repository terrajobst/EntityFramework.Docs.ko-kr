---
title: 열 매핑-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: bca9ca22d211aa58a3bba00f6e4d54b8fe4a0df8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996206"
---
# <a name="column-mapping"></a><span data-ttu-id="0b67f-102">열 매핑</span><span class="sxs-lookup"><span data-stu-id="0b67f-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="0b67f-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="0b67f-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="0b67f-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="0b67f-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="0b67f-105">열 매핑 열 데이터에서 쿼리 및 데이터베이스에 저장 해야 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b67f-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="0b67f-106">규칙</span><span class="sxs-lookup"><span data-stu-id="0b67f-106">Conventions</span></span>

<span data-ttu-id="0b67f-107">규칙에 따라 각 속성을 속성으로 같은 이름의 열에 매핑하려면 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0b67f-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="0b67f-108">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="0b67f-108">Data Annotations</span></span>

<span data-ttu-id="0b67f-109">속성이 매핑되는 열을 구성 하려면 데이터 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b67f-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=3)] -->
``` csharp
public class Blog
{
    [Column("blog_id")]
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="0b67f-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="0b67f-110">Fluent API</span></span>

<span data-ttu-id="0b67f-111">속성이 매핑되는 열을 구성 하기 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b67f-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

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
