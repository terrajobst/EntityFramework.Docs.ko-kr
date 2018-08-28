---
title: 기본값은-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: 341f243ddddc345bb4236e5c34f814694b71e32a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996254"
---
# <a name="default-values"></a><span data-ttu-id="848db-102">기본값</span><span class="sxs-lookup"><span data-stu-id="848db-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="848db-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="848db-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="848db-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="848db-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="848db-105">열의 기본값은 새 행이 삽입 하지만 열에 대 한 값을 지정 하는 삽입 되는 값입니다.</span><span class="sxs-lookup"><span data-stu-id="848db-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="848db-106">규칙</span><span class="sxs-lookup"><span data-stu-id="848db-106">Conventions</span></span>

<span data-ttu-id="848db-107">규칙에 따라 기본값이 구성 되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="848db-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="848db-108">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="848db-108">Data Annotations</span></span>

<span data-ttu-id="848db-109">하지 데이터 주석을 사용 하 여 기본값을 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="848db-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="848db-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="848db-110">Fluent API</span></span>

<span data-ttu-id="848db-111">속성에 대 한 기본값을 지정 하는 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="848db-111">You can use the Fluent API to specify the default value for a property.</span></span>

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

<span data-ttu-id="848db-112">또한 기본 값을 계산 하는 데 사용 되는 SQL 조각을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="848db-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

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
