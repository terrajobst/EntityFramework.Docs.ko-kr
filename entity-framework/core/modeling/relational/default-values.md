---
title: 기본 값-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
ms.technology: entity-framework-core
uid: core/modeling/relational/default-values
ms.openlocfilehash: 73b916b6d9f9c984c8ea010f2319eafa7d031a58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052763"
---
# <a name="default-values"></a><span data-ttu-id="99010-102">기본값</span><span class="sxs-lookup"><span data-stu-id="99010-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="99010-103">이 섹션의 구성을 일반적 관계형 데이터베이스에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="99010-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="99010-104">여기에 표시 된 확장 메서드를 사용할 수 있는 관계형 데이터베이스 공급자를 설치할 때 (공유 인해 *Microsoft.EntityFrameworkCore.Relational* 패키지).</span><span class="sxs-lookup"><span data-stu-id="99010-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="99010-105">열의 기본값은 새 행이 삽입 하지만 값이 없는 열에 대해 지정 된 경우 삽입 될 값입니다.</span><span class="sxs-lookup"><span data-stu-id="99010-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="99010-106">규칙</span><span class="sxs-lookup"><span data-stu-id="99010-106">Conventions</span></span>

<span data-ttu-id="99010-107">규칙에 따라 기본값을 구성 되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="99010-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="99010-108">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="99010-108">Data Annotations</span></span>

<span data-ttu-id="99010-109">데이터 주석을 사용 하 여 기본값을 설정할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="99010-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="99010-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="99010-110">Fluent API</span></span>

<span data-ttu-id="99010-111">속성에 대 한 기본값을 지정 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="99010-111">You can use the Fluent API to specify the default value for a property.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValue.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Rating)
            .HasDefaultValue(3);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public int Rating { get; set; }
}
```

<span data-ttu-id="99010-112">또한 기본 값을 계산 하는 데 사용 되는 SQL 단편을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="99010-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValueSql.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Created)
            .HasDefaultValueSql("getdate()");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public DateTime Created { get; set; }
}
```
