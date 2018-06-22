---
title: 테이블 매핑-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: 73957d9c77e6856bfb65e10e6b373c337101f7d9
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052733"
---
# <a name="table-mapping"></a><span data-ttu-id="39d3c-102">테이블 매핑</span><span class="sxs-lookup"><span data-stu-id="39d3c-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="39d3c-103">이 섹션의 구성을 일반적 관계형 데이터베이스에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="39d3c-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="39d3c-104">여기에 표시 된 확장 메서드를 사용할 수 있는 관계형 데이터베이스 공급자를 설치할 때 (공유 인해 *Microsoft.EntityFrameworkCore.Relational* 패키지).</span><span class="sxs-lookup"><span data-stu-id="39d3c-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="39d3c-105">매핑 테이블에서 쿼리 하는 및에 데이터베이스에 저장 하는 테이블 데이터를 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="39d3c-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="39d3c-106">규칙</span><span class="sxs-lookup"><span data-stu-id="39d3c-106">Conventions</span></span>

<span data-ttu-id="39d3c-107">규칙에 따라 각 엔터티에 동일한 이름 갖는 테이블에 매핑하기 위해 설치 프로그램 됩니다는 `DbSet<TEntity>` 파생 컨텍스트에 엔터티를 노출 하는 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="39d3c-107">By convention, each entity will be setup to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="39d3c-108">하지 않으면 `DbSet<TEntity>` 포함 된 지정된 된 엔터티의 클래스 이름이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="39d3c-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="39d3c-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="39d3c-109">Data Annotations</span></span>

<span data-ttu-id="39d3c-110">형식에 매핑되는 테이블을 구성 하려면 데이터 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="39d3c-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="39d3c-111">테이블이 속한 스키마를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="39d3c-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="39d3c-112">Fluent API</span><span class="sxs-lookup"><span data-stu-id="39d3c-112">Fluent API</span></span>

<span data-ttu-id="39d3c-113">형식에 매핑되는 테이블을 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="39d3c-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
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

<span data-ttu-id="39d3c-114">테이블이 속한 스키마를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="39d3c-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
