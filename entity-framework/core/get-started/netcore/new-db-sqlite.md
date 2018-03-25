---
title: .NET Core에서 시작 - 새 데이터베이스 - EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Entity Framework Core를 사용하여 .NET Core 시작
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 2511dfa3f3262bb12c2058dc1c402b7dcc4c670d
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/23/2018
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="5d7db-104">.NET Core 콘솔 앱에서 새 데이터베이스로 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="5d7db-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="5d7db-105">이 연습에서는 Entity Framework Core를 사용하여 SQLite 데이터베이스에 대해 기본 데이터 액세스를 수행하는 .NET Core 콘솔 앱을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-105">In this walkthrough, you will create a .NET Core console app that performs basic data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="5d7db-106">마이그레이션을 사용하여 모델에서 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-106">You will use migrations to create the database from your model.</span></span> <span data-ttu-id="5d7db-107">ASP.NET Core MVC를 사용하는 Visual Studio 버전에 대해서는 [ASP.NET Core - 새 데이터베이스](xref:core/get-started/aspnetcore/new-db)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="5d7db-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!TIP]  
> <span data-ttu-id="5d7db-108">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5d7db-109">전제 조건</span><span class="sxs-lookup"><span data-stu-id="5d7db-109">Prerequisites</span></span>

<span data-ttu-id="5d7db-110">이 연습을 완료하려면 다음 필수 구성 요소가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-110">The following prerequisites are needed to complete this walkthrough:</span></span>
* <span data-ttu-id="5d7db-111">.NET Core를 지원하는 운영 체제.</span><span class="sxs-lookup"><span data-stu-id="5d7db-111">An operating system that supports .NET Core.</span></span>
* <span data-ttu-id="5d7db-112">[.NET Core SDK](https://www.microsoft.com/net/core) 2.0(몇 가지 수정 사항이 있지만 이전 버전으로 응용 프로그램을 만드는 데 지침이 사용될 수 있음).</span><span class="sxs-lookup"><span data-stu-id="5d7db-112">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.0 (although the instructions can be used to create an application with a previous version with very few modifications).</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="5d7db-113">새 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="5d7db-113">Create a new project</span></span>

* <span data-ttu-id="5d7db-114">프로젝트에 대한 새로운 `ConsoleApp.SQLite` 폴더를 만들고 `dotnet` 명령을 사용하여 .NET Core 앱으로 채웁니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-114">Create a new `ConsoleApp.SQLite` folder for your project and use the `dotnet` command to populate it with a .NET Core app.</span></span>

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="5d7db-115">Entity Framework Core 설치</span><span class="sxs-lookup"><span data-stu-id="5d7db-115">Install Entity Framework Core</span></span>

<span data-ttu-id="5d7db-116">EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="5d7db-117">이 연습에서는 SQLite를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-117">This walkthrough uses SQLite.</span></span> <span data-ttu-id="5d7db-118">사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="5d7db-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="5d7db-119">Microsoft.EntityFrameworkCore.Sqlite 및 Microsoft.EntityFrameworkCore.Design 설치</span><span class="sxs-lookup"><span data-stu-id="5d7db-119">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="5d7db-120">`ConsoleApp.SQLite.csproj`를 수동으로 편집하여 DotNetCliToolReference를 Microsoft.EntityFrameworkCore.Tools.DotNet에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-120">Manually edit `ConsoleApp.SQLite.csproj` to add a DotNetCliToolReference to Microsoft.EntityFrameworkCore.Tools.DotNet:</span></span>

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

<span data-ttu-id="5d7db-121">이제 `ConsoleApp.SQLite.csproj`에 다음이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-121">`ConsoleApp.SQLite.csproj` should now contain the following:</span></span>

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 <span data-ttu-id="5d7db-122">참고: 위에서 사용된 버전 번호는 게시 시에 올바른 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-122">Note: The version numbers used above were correct at the time of publishing.</span></span>

*  <span data-ttu-id="5d7db-123">`dotnet restore`를 실행하여 새 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-123">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="5d7db-124">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="5d7db-124">Create the model</span></span>

<span data-ttu-id="5d7db-125">모델을 구성하는 컨텍스트 및 엔터티 클래스를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-125">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="5d7db-126">다음 콘텐츠를 사용하여 새 *Model.cs* 파일을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-126">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="5d7db-127">실제 응용 프로그램에서는 각 클래스를 별도의 파일에 저장하고 구성 파일에 연결 문자열을 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-127">Tip: In a real application you would put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="5d7db-128">자습서를 간단히 유지하기 위해 모든 항목을 하나의 파일에 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-128">To keep the tutorial simple, we are putting everything in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="5d7db-129">데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="5d7db-129">Create the database</span></span>

<span data-ttu-id="5d7db-130">모델을 만든 후 [마이그레이션](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)을 사용하여 데이터베이스를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-130">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="5d7db-131">`dotnet ef migrations add InitialCreate`를 실행하여 마이그레이션을 스캐폴딩하고 모델에 대한 초기 테이블 집합을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-131">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="5d7db-132">`dotnet ef database update`를 실행하여 새 마이그레이션을 데이터베이스에 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-132">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="5d7db-133">이 명령은 마이그레이션을 적용하기 전에 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-133">This command creates the database before applying migrations.</span></span>

> [!NOTE]  
> <span data-ttu-id="5d7db-134">SQLite에서 상대 경로를 사용하는 경우 경로는 응용 프로그램의 주 어셈블리를 기준으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-134">When using relative paths with SQLite, the path will be relative to the application's main assembly.</span></span> <span data-ttu-id="5d7db-135">이 샘플에서는 기본 바이너리가 `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`이므로 SQLite 데이터베이스가 `bin/Debug/netcoreapp2.0/blogging.db`에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-135">In this sample, the main binary is `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, so the SQLite database will be in `bin/Debug/netcoreapp2.0/blogging.db`.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="5d7db-136">모델 사용</span><span class="sxs-lookup"><span data-stu-id="5d7db-136">Use your model</span></span>

* <span data-ttu-id="5d7db-137">*Program.cs*를 열고 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-137">Open *Program.cs* and replace the contents with the following code:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="5d7db-138">앱을 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-138">Test the app:</span></span>

 `dotnet run`

 <span data-ttu-id="5d7db-139">하나의 블로그가 데이터베이스에 저장되고 모든 블로그의 세부 정보가 콘솔에 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-139">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="5d7db-140">모델 변경:</span><span class="sxs-lookup"><span data-stu-id="5d7db-140">Changing the model:</span></span>

- <span data-ttu-id="5d7db-141">모델을 변경할 경우 `dotnet ef migrations add` 명령을 사용하여 새 [마이그레이션](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)을 스캐폴딩하고 데이터베이스에서 해당 스키마를 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-141">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="5d7db-142">스캐폴딩된 코드를 확인하고 필요한 내용을 변경한 후 `dotnet ef database update` 명령을 사용하여 변경 내용을 데이터베이스에 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-142">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="5d7db-143">EF는 데이터베이스의 `__EFMigrationsHistory` 테이블을 사용하여 이미 데이터베이스에 적용된 마이그레이션을 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-143">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="5d7db-144">SQLite의 제한 사항으로 인해 SQLite는 일부 마이그레이션(스키마 변경)을 지원하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-144">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="5d7db-145">[SQLite 제한 사항](../../providers/sqlite/limitations.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="5d7db-145">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="5d7db-146">새 개발의 경우 모델이 변경될 때 마이그레이션을 사용하는 대신 데이터베이스를 삭제하고 새 개발을 만드는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="5d7db-146">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5d7db-147">추가 리소스</span><span class="sxs-lookup"><span data-stu-id="5d7db-147">Additional Resources</span></span>

* <span data-ttu-id="5d7db-148">[.NET Core - SQLite를 사용하여 새 데이터베이스 만들기](xref:core/get-started/netcore/new-db-sqlite) - 플랫폼 간 콘솔 EF 자습서.</span><span class="sxs-lookup"><span data-stu-id="5d7db-148">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="5d7db-149">Mac 또는 Linux의 ASP.NET Core MVC 소개</span><span class="sxs-lookup"><span data-stu-id="5d7db-149">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="5d7db-150">Visual Studio의 ASP.NET Core MVC 소개</span><span class="sxs-lookup"><span data-stu-id="5d7db-150">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="5d7db-151">Visual Studio를 사용하여 ASP.NET Core 및 Entity Framework Core 시작</span><span class="sxs-lookup"><span data-stu-id="5d7db-151">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
