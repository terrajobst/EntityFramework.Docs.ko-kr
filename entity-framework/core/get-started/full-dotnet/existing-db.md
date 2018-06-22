---
title: .NET Framework에서 시작 - 기존 데이터베이스 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 3cd69109e3cf8dbc103f9eea6e2553df17f29a98
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812627"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="aaae6-102">.NET Framework에서 기존 데이터베이스로 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="aaae6-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="aaae6-103">이 연습에서는 Entity Framework를 사용하여 Microsoft SQL Server 데이터베이스에 대해 기본 데이터 액세스를 수행하는 콘솔 응용 프로그램을 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="aaae6-104">리버스 엔지니어링을 사용하여 기존 데이터베이스를 기반으로 Entity Framework 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="aaae6-105">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aaae6-106">전제 조건</span><span class="sxs-lookup"><span data-stu-id="aaae6-106">Prerequisites</span></span>

<span data-ttu-id="aaae6-107">이 연습을 완료하려면 다음 필수 구성 요소가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="aaae6-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="aaae6-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* [<span data-ttu-id="aaae6-109">NuGet 패키지 관리자의 최신 버전</span><span class="sxs-lookup"><span data-stu-id="aaae6-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="aaae6-110">Windows PowerShell의 최신 버전</span><span class="sxs-lookup"><span data-stu-id="aaae6-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [<span data-ttu-id="aaae6-111">Blogging 데이터베이스</span><span class="sxs-lookup"><span data-stu-id="aaae6-111">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="aaae6-112">Blogging 데이터베이스</span><span class="sxs-lookup"><span data-stu-id="aaae6-112">Blogging database</span></span>

<span data-ttu-id="aaae6-113">이 자습서에서는 LocalDb 인스턴스의 **Blogging** 데이터베이스를 기존 데이터베이스로 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="aaae6-114">**이미 다른 자습서의 일부로 Blogging** 데이터베이스를 만든 경우 다음 단계를 건너뛸 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-114">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="aaae6-115">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-115">Open Visual Studio</span></span>

* <span data-ttu-id="aaae6-116">도구 > 데이터베이스에 연결...</span><span class="sxs-lookup"><span data-stu-id="aaae6-116">Tools > Connect to Database...</span></span>

* <span data-ttu-id="aaae6-117">**Microsoft SQL Server**를 선택하고 **계속**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-117">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="aaae6-118">**서버 이름**으로 **(localdb)\mssqllocaldb**를 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="aaae6-119">**데이터베이스 이름**으로 **master**를 입력하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-119">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="aaae6-120">이제 master 데이터베이스가 **서버 탐색기**의 **데이터 연결**에 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="aaae6-121">**서버 탐색기**에서 데이터베이스를 마우스 오른쪽 단추로 클릭하고 **새 쿼리**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="aaae6-122">아래 나열된 스크립트를 쿼리 편집기로 복사합니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-122">Copy the script, listed below, into the query editor</span></span>

* <span data-ttu-id="aaae6-123">쿼리 편집기를 마우스 오른쪽 단추로 클릭하고 **실행**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="aaae6-124">새 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="aaae6-124">Create a new project</span></span>

* <span data-ttu-id="aaae6-125">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-125">Open Visual Studio</span></span>

* <span data-ttu-id="aaae6-126">파일 > 새로 만들기 > 프로젝트...</span><span class="sxs-lookup"><span data-stu-id="aaae6-126">File > New > Project...</span></span>

* <span data-ttu-id="aaae6-127">왼쪽 메뉴에서 [템플릿] > [Visual C#] > [Windows]를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-127">From the left menu select Templates > Visual C# > Windows</span></span>

* <span data-ttu-id="aaae6-128">**콘솔 응용 프로그램** 프로젝트 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-128">Select the **Console Application** project template</span></span>

* <span data-ttu-id="aaae6-129">**.NET Framework 4.5.1** 이상을 대상으로 하는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-129">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="aaae6-130">프로젝트에 이름을 지정하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-130">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="aaae6-131">Entity Framework 설치</span><span class="sxs-lookup"><span data-stu-id="aaae6-131">Install Entity Framework</span></span>

<span data-ttu-id="aaae6-132">EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-132">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="aaae6-133">이 연습에서는 SQL Server를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-133">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="aaae6-134">사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="aaae6-134">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="aaae6-135">도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔</span><span class="sxs-lookup"><span data-stu-id="aaae6-135">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="aaae6-136">`Install-Package Microsoft.EntityFrameworkCore.SqlServer` 실행</span><span class="sxs-lookup"><span data-stu-id="aaae6-136">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="aaae6-137">기존 데이터베이스에서 리버스 엔지니어링을 사용하려면 몇 개의 다른 패키지도 설치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-137">To enable reverse engineering from an existing database we need to install a couple of other packages too.</span></span>

* <span data-ttu-id="aaae6-138">`Install-Package Microsoft.EntityFrameworkCore.Tools` 실행</span><span class="sxs-lookup"><span data-stu-id="aaae6-138">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="aaae6-139">모델 리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="aaae6-139">Reverse engineer your model</span></span>

<span data-ttu-id="aaae6-140">이제 기존 데이터베이스를 기반으로 EF 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-140">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="aaae6-141">도구 -> NuGet 패키지 관리자 -> 패키지 관리자 콘솔</span><span class="sxs-lookup"><span data-stu-id="aaae6-141">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="aaae6-142">다음 명령을 실행하여 기존 데이터베이스에서 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-142">Run the following command to create a model from the existing database</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="aaae6-143">리버스 엔지니어링 프로세스에서는 기존 데이터베이스의 스키마를 기반으로 엔터티 클래스 및 파생 컨텍스트를 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-143">The reverse engineer process created entity classes and a derived context based on the schema of the existing database.</span></span> <span data-ttu-id="aaae6-144">엔터티 클래스는 쿼리하고 저장할 데이터를 나타내는 단순 C# 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-144">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)] -->
``` csharp
using System;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class Blog
    {
        public Blog()
        {
            Post = new HashSet<Post>();
        }

        public int BlogId { get; set; }
        public string Url { get; set; }

        public virtual ICollection<Post> Post { get; set; }
    }
}
```

<span data-ttu-id="aaae6-145">컨텍스트는 데이터베이스가 있는 세션을 나타내며 이를 통해 엔터티 클래스의 인스턴스를 쿼리하고 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-145">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class BloggingContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>(entity =>
            {
                entity.Property(e => e.Url).IsRequired();
            });

            modelBuilder.Entity<Post>(entity =>
            {
                entity.HasOne(d => d.Blog)
                    .WithMany(p => p.Post)
                    .HasForeignKey(d => d.BlogId);
            });
        }

        public virtual DbSet<Blog> Blog { get; set; }
        public virtual DbSet<Post> Post { get; set; }
    }
}
```

## <a name="use-your-model"></a><span data-ttu-id="aaae6-146">모델 사용</span><span class="sxs-lookup"><span data-stu-id="aaae6-146">Use your model</span></span>

<span data-ttu-id="aaae6-147">이제 모델을 사용하여 데이터 액세스를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-147">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="aaae6-148">*Program.cs*를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-148">Open *Program.cs*</span></span>

* <span data-ttu-id="aaae6-149">파일의 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-149">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blog.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blog)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* <span data-ttu-id="aaae6-150">디버그 > 디버깅하지 않고 시작</span><span class="sxs-lookup"><span data-stu-id="aaae6-150">Debug > Start Without Debugging</span></span>

<span data-ttu-id="aaae6-151">하나의 블로그가 데이터베이스에 저장되고 모든 블로그의 세부 정보가 콘솔에 출력되는 것을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="aaae6-151">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![이미지](_static/output-existing-db.png)
