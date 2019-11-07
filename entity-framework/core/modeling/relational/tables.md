---
title: 테이블 매핑-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 474c49aca4c65cd5d58b184b1f3c2d30e7abff84
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656097"
---
# <a name="table-mapping"></a><span data-ttu-id="e0720-102">테이블 매핑</span><span class="sxs-lookup"><span data-stu-id="e0720-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="e0720-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="e0720-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="e0720-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="e0720-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="e0720-105">테이블 매핑은 데이터베이스에서 쿼리하고 저장할 테이블 데이터를 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="e0720-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="e0720-106">규칙</span><span class="sxs-lookup"><span data-stu-id="e0720-106">Conventions</span></span>

<span data-ttu-id="e0720-107">규칙에 따라 각 엔터티는 파생 컨텍스트에 엔터티를 노출하는 `DbSet<TEntity>` 속성과 이름이 같은 테이블에 매핑되도록 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="e0720-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="e0720-108">지정 된 엔터티에 대 한 `DbSet<TEntity>` 포함 되어 있지 않으면 클래스 이름이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e0720-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e0720-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="e0720-109">Data Annotations</span></span>

<span data-ttu-id="e0720-110">데이터 주석을 사용 하 여 형식이 매핑되는 테이블을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e0720-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

``` csharp
using System.ComponentModel.DataAnnotations.Schema;

[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="e0720-111">테이블이 속한 스키마를 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e0720-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="e0720-112">흐름 API</span><span class="sxs-lookup"><span data-stu-id="e0720-112">Fluent API</span></span>

<span data-ttu-id="e0720-113">흐름 API를 사용 하 여 형식이 매핑되는 테이블을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e0720-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;

class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="e0720-114">테이블이 속한 스키마를 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e0720-114">You can also specify a schema that the table belongs to.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/TableAndSchema.cs?name=Table&highlight=2)]
