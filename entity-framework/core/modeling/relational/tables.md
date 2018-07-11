---
title: 테이블 매핑-EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: ef6080b5d543c2a68a680be2b9effc1c9d531030
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949011"
---
# <a name="table-mapping"></a><span data-ttu-id="5853f-102">테이블 매핑</span><span class="sxs-lookup"><span data-stu-id="5853f-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="5853f-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="5853f-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="5853f-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="5853f-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="5853f-105">테이블 매핑 테이블 데이터에서 쿼리 및 데이터베이스에 저장 해야 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="5853f-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="5853f-106">규칙</span><span class="sxs-lookup"><span data-stu-id="5853f-106">Conventions</span></span>

<span data-ttu-id="5853f-107">규칙에 따라 각 엔터티에 설정할 같은 이름의 테이블에 매핑하는 `DbSet<TEntity>` 파생 컨텍스트의 엔터티를 노출 하는 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="5853f-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="5853f-108">없으면 `DbSet<TEntity>` 포함 된 지정된 된 엔터티 클래스 이름이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5853f-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5853f-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="5853f-109">Data Annotations</span></span>

<span data-ttu-id="5853f-110">데이터 주석 형식에 매핑되는 테이블을 구성 하려면 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5853f-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="5853f-111">테이블이 속한 스키마를 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5853f-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="5853f-112">Fluent API</span><span class="sxs-lookup"><span data-stu-id="5853f-112">Fluent API</span></span>

<span data-ttu-id="5853f-113">형식에 매핑되는 테이블을 구성 하는 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5853f-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="5853f-114">테이블이 속한 스키마를 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5853f-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
