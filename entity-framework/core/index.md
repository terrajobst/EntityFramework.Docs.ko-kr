---
title: "요약 개요 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: c76b4cd318151b502c549fa0a82800f9987ed94c
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="entity-framework-core-quick-overview"></a><span data-ttu-id="01b62-102">Entity Framework Core 요약 개요</span><span class="sxs-lookup"><span data-stu-id="01b62-102">Entity Framework Core Quick Overview</span></span>

<span data-ttu-id="01b62-103">EF(Entity Framework) Core는 널리 사용되는 Entity Framework 데이터 액세스 기술의 가볍고 확장 가능하며 플랫폼 교차적인 버전입니다.</span><span class="sxs-lookup"><span data-stu-id="01b62-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="01b62-104">EF Core는 개체 관계형 매퍼(O/RM)로, .NET 개발자들이 .NET 개체를 사용하여 데이터베이스 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01b62-104">EF Core is an object-relational mapper (O/RM) that enables .NET developers to work with a database using .NET objects.</span></span> <span data-ttu-id="01b62-105">개발자들이 보통 작성해야 하는 데이터 액세스 코드가 대부분 필요하지 않게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01b62-105">It eliminates the need for most of the data-access code that developers usually need to write.</span></span> <span data-ttu-id="01b62-106">EF Core 는 여러 데이터베이스 엔진을 지원합니다. 자세한 내용은 [데이터베이스 공급자](providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="01b62-106">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

<span data-ttu-id="01b62-107">코드 작성을 통해 학습하려면 [시작](get-started/index.md) 가이드 중 하나를 사용하여 EF Core를 시작해 보는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="01b62-107">If you like to learn by writing code, we'd recommend one of our [Getting Started](get-started/index.md) guides to get you started with EF Core.</span></span>

## <a name="what-is-new-in-ef-core"></a><span data-ttu-id="01b62-108">EF Core의 새로운 기능</span><span class="sxs-lookup"><span data-stu-id="01b62-108">What is new in EF Core</span></span>

<span data-ttu-id="01b62-109">EF Core를 잘 알고 있고 최신 릴리스의 세부 정보로 직접 이동하려는 경우:</span><span class="sxs-lookup"><span data-stu-id="01b62-109">If you are familiar with EF Core and want to jump straight into the details of the latest releases:</span></span>

- <span data-ttu-id="01b62-110">**[EF Core 2.1의 새로운 기능(현재 미리 보기 상태)](xref:core/what-is-new/ef-core-2.1)**</span><span class="sxs-lookup"><span data-stu-id="01b62-110">**[What is new in EF Core 2.1 (currently in preview)](xref:core/what-is-new/ef-core-2.1)**</span></span>
- <span data-ttu-id="01b62-111">**[EF Core 2.0의 새로운 기능(출시된 최신 버전)](xref:core/what-is-new/ef-core-2.0)**</span><span class="sxs-lookup"><span data-stu-id="01b62-111">**[What is new in EF Core 2.0 (the latest released version)](xref:core/what-is-new/ef-core-2.0)**</span></span>
- <span data-ttu-id="01b62-112">**[EF Core 2.0으로 기존 응용 프로그램 업그레이드](xref:core/miscellaneous/1x-2x-upgrade)**</span><span class="sxs-lookup"><span data-stu-id="01b62-112">**[Upgrading existing applications to EF Core 2.0](xref:core/miscellaneous/1x-2x-upgrade)**</span></span>


## <a name="get-entity-framework-core"></a><span data-ttu-id="01b62-113">Entity Framework Core 구하기</span><span class="sxs-lookup"><span data-stu-id="01b62-113">Get Entity Framework Core</span></span>

<span data-ttu-id="01b62-114">사용할 데이터베이스 공급자에 대한 [NuGet 패키지를 설치합니다](https://docs.nuget.org/ndocs/quickstart/use-a-package).</span><span class="sxs-lookup"><span data-stu-id="01b62-114">[Install the NuGet package](https://docs.nuget.org/ndocs/quickstart/use-a-package) for the database provider you want to use.</span></span> <span data-ttu-id="01b62-115">예:</span><span class="sxs-lookup"><span data-stu-id="01b62-115">E.g.</span></span> <span data-ttu-id="01b62-116">명령줄에서 `dotnet` 도구를 사용하여 교차 플랫폼 개발에서 SQL Server 공급자를 설치하려면</span><span class="sxs-lookup"><span data-stu-id="01b62-116">to install the SQL Server provider in cross-platform development using `dotnet` tool in the command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="01b62-117">또는 Visual Studio에서 패키지 관리자 콘솔을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="01b62-117">Or in Visual Studio, using the Package Manager Console:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
<span data-ttu-id="01b62-118">사용 가능한 공급자에 대한 정보는 [데이터베이스 공급자](providers/index.md), 더 자세한 설치 단계는 [EF Core 설치](get-started/install/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="01b62-118">See [Database Providers](providers/index.md) for information on available providers and [Installing EF Core](get-started/install/index.md) for more detailed installation steps.</span></span>

## <a name="the-model"></a><span data-ttu-id="01b62-119">모델</span><span class="sxs-lookup"><span data-stu-id="01b62-119">The Model</span></span>

<span data-ttu-id="01b62-120">EF Core에서는 데이터 액세스가 모델을 통해 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="01b62-120">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="01b62-121">모델은 엔터티 클래스와, 데이터베이스와의 세션을 나타내는 파생된 컨텍스트로 구성되어 데이터를 쿼리하고 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01b62-121">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="01b62-122">자세한 내용은 [모델 만들기](modeling/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="01b62-122">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="01b62-123">기존 데이터베이스에서 모델을 생성하고 데이터베이스에 맞는 모델을 직접 작성하거나 EF 마이그레이션을 사용하여 모델로부터 데이터베이스를 만들 수 있습니다(이후 시간에 따라 모델 변경과 함께 확장).</span><span class="sxs-lookup"><span data-stu-id="01b62-123">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

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

## <a name="querying"></a><span data-ttu-id="01b62-124">쿼리</span><span class="sxs-lookup"><span data-stu-id="01b62-124">Querying</span></span>

<span data-ttu-id="01b62-125">엔터티 클래스의 인스턴스는 LINQ(Language Integrated Query)를 사용하여 데이터베이스에서 검색됩니다.</span><span class="sxs-lookup"><span data-stu-id="01b62-125">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="01b62-126">자세한 내용은 [데이터 쿼리](querying/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="01b62-126">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="01b62-127">데이터 저장</span><span class="sxs-lookup"><span data-stu-id="01b62-127">Saving Data</span></span>

<span data-ttu-id="01b62-128">데이터는 엔터티 클래스의 인스턴스를 통해 데이터베이스에서 만들어지고 삭제되며 수정됩니다.</span><span class="sxs-lookup"><span data-stu-id="01b62-128">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="01b62-129">자세한 내용은 자세한 내용은 [데이터 저장](saving/index.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="01b62-129">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
