---
title: .NET Framework에서 시작 - 새 데이터베이스 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812523"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="a883e-102">.NET Framework에서 새 데이터베이스로 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="a883e-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="a883e-103">이 연습에서는 Entity Framework를 사용하여 Microsoft SQL Server 데이터베이스에 대해 기본 데이터 액세스를 수행하는 콘솔 응용 프로그램을 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="a883e-104">마이그레이션을 사용하여 모델에서 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-104">You will use migrations to create the database from your model.</span></span>

> [!TIP]  
> <span data-ttu-id="a883e-105">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a883e-106">전제 조건</span><span class="sxs-lookup"><span data-stu-id="a883e-106">Prerequisites</span></span>

<span data-ttu-id="a883e-107">이 연습을 완료하려면 다음 필수 구성 요소가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="a883e-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="a883e-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* [<span data-ttu-id="a883e-109">NuGet 패키지 관리자의 최신 버전</span><span class="sxs-lookup"><span data-stu-id="a883e-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="a883e-110">Windows PowerShell의 최신 버전</span><span class="sxs-lookup"><span data-stu-id="a883e-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a><span data-ttu-id="a883e-111">새 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="a883e-111">Create a new project</span></span>

* <span data-ttu-id="a883e-112">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-112">Open Visual Studio</span></span>

* <span data-ttu-id="a883e-113">파일 > 새로 만들기 > 프로젝트...</span><span class="sxs-lookup"><span data-stu-id="a883e-113">File > New > Project...</span></span>

* <span data-ttu-id="a883e-114">왼쪽 메뉴에서 [템플릿] > [Visual C#] > [Windows 클래식 바탕 화면]을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-114">From the left menu select Templates > Visual C# > Windows Classic Desktop</span></span>

* <span data-ttu-id="a883e-115">**콘솔 앱(.NET Framework)** 프로젝트 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-115">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="a883e-116">**.NET Framework 4.5.1** 이상을 대상으로 하는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-116">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="a883e-117">프로젝트에 이름을 지정하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-117">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="a883e-118">Entity Framework 설치</span><span class="sxs-lookup"><span data-stu-id="a883e-118">Install Entity Framework</span></span>

<span data-ttu-id="a883e-119">EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="a883e-120">이 연습에서는 SQL Server를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-120">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="a883e-121">사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="a883e-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="a883e-122">도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔</span><span class="sxs-lookup"><span data-stu-id="a883e-122">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="a883e-123">`Install-Package Microsoft.EntityFrameworkCore.SqlServer` 실행</span><span class="sxs-lookup"><span data-stu-id="a883e-123">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="a883e-124">이 연습의 뒷부분에서는 일부 Entity Framework Tools를 사용하여 데이터베이스를 유지 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-124">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="a883e-125">따라서 도구 패키지도 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-125">So we will install the tools package as well.</span></span>

* <span data-ttu-id="a883e-126">`Install-Package Microsoft.EntityFrameworkCore.Tools` 실행</span><span class="sxs-lookup"><span data-stu-id="a883e-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="a883e-127">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="a883e-127">Create your model</span></span>

<span data-ttu-id="a883e-128">이제 모델을 구성하는 컨텍스트 및 엔터티 클래스를 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-128">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="a883e-129">프로젝트 > 클래스 추가...</span><span class="sxs-lookup"><span data-stu-id="a883e-129">Project > Add Class...</span></span>

* <span data-ttu-id="a883e-130">이름으로 *Model.cs*를 입력하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-130">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="a883e-131">파일의 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-131">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }

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

> [!TIP]  
> <span data-ttu-id="a883e-132">실제 응용 프로그램에서는 각 클래스를 별도의 파일에 저장하고, `App.Config` 파일에 연결 문자열을 저장하고, `ConfigurationManager`를 사용하여 파일을 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-132">In a real application you would put each class in a separate file and put the connection string in the `App.Config` file and read it out using `ConfigurationManager`.</span></span> <span data-ttu-id="a883e-133">간단한 설명을 위해 모든 항목을 이 학습서에 대한 단일 코드 파일에 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-133">For the sake of simplicity, we are putting everything in a single code file for this tutorial.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="a883e-134">데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="a883e-134">Create your database</span></span>

<span data-ttu-id="a883e-135">이제 모델이 있으므로 마이그레이션을 사용하여 데이터베이스를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-135">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="a883e-136">도구 -> NuGet 패키지 관리자 -> 패키지 관리자 콘솔</span><span class="sxs-lookup"><span data-stu-id="a883e-136">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="a883e-137">`Add-Migration MyFirstMigration`를 실행하여 마이그레이션을 스캐폴딩하고 모델에 대한 초기 테이블 집합을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-137">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

* <span data-ttu-id="a883e-138">`Update-Database`를 실행하여 새 마이그레이션을 데이터베이스에 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-138">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="a883e-139">데이터베이스가 아직 없기 때문에 마이그레이션이 적용되기 전에 사용자의 데이터베이스가 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-139">Because your database doesn't exist yet, it will be created for you before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="a883e-140">나중에 모델을 변경할 경우 `Add-Migration` 명령을 사용하여 새 마이그레이션을 스캐폴딩하고 데이터베이스에서 해당 스키마를 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-140">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="a883e-141">스캐폴딩된 코드를 확인하고 필요한 내용을 변경한 후 `Update-Database` 명령을 사용하여 변경 내용을 데이터베이스에 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-141">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
><span data-ttu-id="a883e-142">EF는 데이터베이스의 `__EFMigrationsHistory` 테이블을 사용하여 이미 데이터베이스에 적용된 마이그레이션을 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-142">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="a883e-143">모델 사용</span><span class="sxs-lookup"><span data-stu-id="a883e-143">Use your model</span></span>

<span data-ttu-id="a883e-144">이제 모델을 사용하여 데이터 액세스를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-144">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="a883e-145">*Program.cs*를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-145">Open *Program.cs*</span></span>

* <span data-ttu-id="a883e-146">파일의 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-146">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blogs)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* <span data-ttu-id="a883e-147">디버그 > 디버깅하지 않고 시작</span><span class="sxs-lookup"><span data-stu-id="a883e-147">Debug > Start Without Debugging</span></span>

<span data-ttu-id="a883e-148">하나의 블로그가 데이터베이스에 저장되고 모든 블로그의 세부 정보가 콘솔에 출력되는 것을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a883e-148">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![이미지](_static/output-new-db.png)
