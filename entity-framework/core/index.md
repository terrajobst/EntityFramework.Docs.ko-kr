---
title: 개요 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: ee3fac9e9103749195886a632fbeac3163a46689
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250545"
---
# <a name="entity-framework-core"></a><span data-ttu-id="b0ac8-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="b0ac8-102">Entity Framework Core</span></span>

<span data-ttu-id="b0ac8-103">EF(Entity Framework) Core는 널리 사용되는 Entity Framework 데이터 액세스 기술의 가볍고 확장 가능하며 플랫폼 교차적인 버전입니다.</span><span class="sxs-lookup"><span data-stu-id="b0ac8-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="b0ac8-104">EF Core는 O/RM(개체 관계형 매퍼)으로 사용될 수 있으며, 이를 통해 .NET 개발자는 .NET 개체를 사용하여 데이터베이스를 작업할 수 있으며 일반적으로 써야 하는 대부분의 데이터 액세스 코드가 필요하지 않게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b0ac8-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="b0ac8-105">EF Core 는 여러 데이터베이스 엔진을 지원합니다. 자세한 내용은 [데이터베이스 공급자](providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="b0ac8-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="b0ac8-106">모델</span><span class="sxs-lookup"><span data-stu-id="b0ac8-106">The Model</span></span>

<span data-ttu-id="b0ac8-107">EF Core에서는 데이터 액세스가 모델을 통해 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="b0ac8-107">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="b0ac8-108">모델은 엔터티 클래스와, 데이터베이스와의 세션을 나타내는 파생된 컨텍스트로 구성되어 데이터를 쿼리하고 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b0ac8-108">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="b0ac8-109">자세한 내용은 [모델 만들기](modeling/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="b0ac8-109">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="b0ac8-110">기존 데이터베이스에서 모델을 생성하고 데이터베이스에 맞는 모델을 직접 작성하거나 EF 마이그레이션을 사용하여 모델로부터 데이터베이스를 만들 수 있습니다(이후 시간에 따라 모델 변경과 함께 확장).</span><span class="sxs-lookup"><span data-stu-id="b0ac8-110">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace Intro
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }
        public int Rating { get; set; }
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
}
```

## <a name="querying"></a><span data-ttu-id="b0ac8-111">쿼리</span><span class="sxs-lookup"><span data-stu-id="b0ac8-111">Querying</span></span>

<span data-ttu-id="b0ac8-112">엔터티 클래스의 인스턴스는 LINQ(Language Integrated Query)를 사용하여 데이터베이스에서 검색됩니다.</span><span class="sxs-lookup"><span data-stu-id="b0ac8-112">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="b0ac8-113">자세한 내용은 [데이터 쿼리](querying/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="b0ac8-113">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="b0ac8-114">데이터 저장</span><span class="sxs-lookup"><span data-stu-id="b0ac8-114">Saving Data</span></span>

<span data-ttu-id="b0ac8-115">데이터는 엔터티 클래스의 인스턴스를 통해 데이터베이스에서 만들어지고 삭제되며 수정됩니다.</span><span class="sxs-lookup"><span data-stu-id="b0ac8-115">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="b0ac8-116">자세한 내용은 자세한 내용은 [데이터 저장](saving/index.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="b0ac8-116">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```

## <a name="next-steps"></a><span data-ttu-id="b0ac8-117">다음 단계</span><span class="sxs-lookup"><span data-stu-id="b0ac8-117">Next steps</span></span>

<span data-ttu-id="b0ac8-118">기본 자습서는 [Entity Framework Core 시작](get-started/index.md)을 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="b0ac8-118">For introductory tutorials, see [Getting Started with Entity Framework Core](get-started/index.md).</span></span>

