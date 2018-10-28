---
title: .NET Core에서 시작 - 새 데이터베이스 - EF Core
author: rick-anderson
ms.author: riande
description: Entity Framework Core를 사용하여 .NET Core 시작
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 6cebe14e179cb6998592f5d3823c114b3bda0138
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022313"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="7f1bb-103">.NET Core 콘솔 앱에서 새 데이터베이스로 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="7f1bb-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="7f1bb-104">이 자습서에서는 Entity Framework Core를 사용하여 SQLite 데이터베이스에 대한 데이터 액세스를 수행하는 .NET Core 콘솔 앱을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="7f1bb-105">마이그레이션을 사용하여 모델에서 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-105">You use migrations to create the database from the model.</span></span> <span data-ttu-id="7f1bb-106">ASP.NET Core MVC를 사용하는 Visual Studio 버전에 대해서는 [ASP.NET Core - 새 데이터베이스](xref:core/get-started/aspnetcore/new-db)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="7f1bb-107">[GitHub에서 이 아티클의 샘플을 봅니다](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span><span class="sxs-lookup"><span data-stu-id="7f1bb-107">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7f1bb-108">전제 조건</span><span class="sxs-lookup"><span data-stu-id="7f1bb-108">Prerequisites</span></span>

* [<span data-ttu-id="7f1bb-109">.NET Core 2.1 SDK</span><span class="sxs-lookup"><span data-stu-id="7f1bb-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="7f1bb-110">새 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="7f1bb-110">Create a new project</span></span>

* <span data-ttu-id="7f1bb-111">새 콘솔 프로젝트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a><span data-ttu-id="7f1bb-112">현재 디렉터리 변경</span><span class="sxs-lookup"><span data-stu-id="7f1bb-112">Change the current directory</span></span>

<span data-ttu-id="7f1bb-113">이후 단계에서는 응용 프로그램에 대해 `dotnet` 명령을 실행해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-113">In subsequent steps, we need to issue `dotnet` commands against the application.</span></span>

* <span data-ttu-id="7f1bb-114">현재 디렉터리를 다음과 같은 응용 프로그램 디렉터리로 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-114">We change the current directory to the application's directory like this:</span></span>

  ``` Console
  cd ConsoleApp.SQLite/
  ```
## <a name="install-entity-framework-core"></a><span data-ttu-id="7f1bb-115">Entity Framework Core 설치</span><span class="sxs-lookup"><span data-stu-id="7f1bb-115">Install Entity Framework Core</span></span>

<span data-ttu-id="7f1bb-116">EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="7f1bb-117">이 연습에서는 SQLite를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-117">This walkthrough uses SQLite.</span></span> <span data-ttu-id="7f1bb-118">사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="7f1bb-119">Microsoft.EntityFrameworkCore.Sqlite 및 Microsoft.EntityFrameworkCore.Design 설치</span><span class="sxs-lookup"><span data-stu-id="7f1bb-119">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="7f1bb-120">`dotnet restore`를 실행하여 새 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-120">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="7f1bb-121">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="7f1bb-121">Create the model</span></span>

<span data-ttu-id="7f1bb-122">모델을 구성하는 컨텍스트 및 엔터티 클래스를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-122">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="7f1bb-123">다음 콘텐츠를 사용하여 새 *Model.cs* 파일을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-123">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="7f1bb-124">팁: 실제 응용 프로그램에서는 각 클래스를 별도의 파일에 저장하고 구성 파일 또는 환경 변수에 연결 문자열을 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-124">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="7f1bb-125">자습서를 간단히 유지하기 위해 모든 항목이 하나의 파일에 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-125">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="7f1bb-126">데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="7f1bb-126">Create the database</span></span>

<span data-ttu-id="7f1bb-127">모델을 만든 후 [마이그레이션](xref:core/managing-schemas/migrations/index)을 사용하여 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-127">Once you have a model, you use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

* <span data-ttu-id="7f1bb-128">`dotnet ef migrations add InitialCreate`를 실행하여 마이그레이션을 스캐폴딩하고 모델에 대한 초기 테이블 집합을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-128">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="7f1bb-129">`dotnet ef database update`를 실행하여 새 마이그레이션을 데이터베이스에 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-129">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="7f1bb-130">이 명령은 마이그레이션을 적용하기 전에 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-130">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="7f1bb-131">*blogging.db*\* SQLite DB는 프로젝트 디렉터리에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-131">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="7f1bb-132">모델 사용</span><span class="sxs-lookup"><span data-stu-id="7f1bb-132">Use the model</span></span>

* <span data-ttu-id="7f1bb-133">*Program.cs*를 열고 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-133">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="7f1bb-134">콘솔에서 앱을 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-134">Test the app from the console.</span></span> <span data-ttu-id="7f1bb-135">Visual Studio에서 앱을 실행하려면 [Visual Studio 참고](#vs)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-135">See the [Visual Studio note](#vs) to run the app from Visual Studio.</span></span>

  `dotnet run`

  <span data-ttu-id="7f1bb-136">하나의 블로그가 데이터베이스에 저장되고 모든 블로그의 세부 정보가 콘솔에 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-136">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="7f1bb-137">모델 변경:</span><span class="sxs-lookup"><span data-stu-id="7f1bb-137">Changing the model:</span></span>

- <span data-ttu-id="7f1bb-138">모델을 변경하는 경우 `dotnet ef migrations add` 명령을 사용하여 [마이그레이션](xref:core/managing-schemas/migrations/index)을 스캐폴딩할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-138">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](xref:core/managing-schemas/migrations/index).</span></span> <span data-ttu-id="7f1bb-139">스캐폴딩된 코드를 확인하고 필요한 내용을 변경한 후 `dotnet ef database update` 명령을 사용하여 스키마 변경 내용을 데이터베이스에 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-139">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="7f1bb-140">EF Core는 데이터베이스의 `__EFMigrationsHistory` 테이블을 사용하여 이미 데이터베이스에 적용된 마이그레이션을 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-140">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="7f1bb-141">SQLite 데이터베이스 엔진은 대부분의 다른 관계형 데이터베이스에서 지원하는 특정 스키마 변경을 지원하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-141">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="7f1bb-142">예를 들어 `DropColumn` 작업은 지원되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-142">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="7f1bb-143">EF Core 마이그레이션은 이러한 작업에 대한 코드를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-143">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="7f1bb-144">그러나 데이터베이스에 적용하거나 스크립트를 생성하려고 하면 EF Core에서 예외를 throw합니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-144">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="7f1bb-145">[SQLite 제한 사항](../../providers/sqlite/limitations.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-145">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="7f1bb-146">새 개발의 경우 모델이 변경될 때 마이그레이션을 사용하는 대신 데이터베이스를 삭제하고 새 개발을 만드는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-146">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>

<a name="vs"></a>
### <a name="run-from-visual-studio"></a><span data-ttu-id="7f1bb-147">Visual Studio에서 실행</span><span class="sxs-lookup"><span data-stu-id="7f1bb-147">Run from Visual Studio</span></span>

<span data-ttu-id="7f1bb-148">이 샘플을 Visual Studio에서 실행하려면 수동으로 작업 디렉터리를 프로젝트의 루트로 설정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-148">To run this sample from Visual Studio, you must set the working directory manually to be the root of the project.</span></span> <span data-ttu-id="7f1bb-149">작업 디렉터리를 설정하지 않으면 다음 `Microsoft.Data.Sqlite.SqliteException`이 throw됩니다. `SQLite Error 1: 'no such table: Blogs'`</span><span class="sxs-lookup"><span data-stu-id="7f1bb-149">If  you don't set the working directory, the following `Microsoft.Data.Sqlite.SqliteException` is thrown: `SQLite Error 1: 'no such table: Blogs'`.</span></span>

<span data-ttu-id="7f1bb-150">작업 디렉터리를 설정하려면</span><span class="sxs-lookup"><span data-stu-id="7f1bb-150">To set the working directory:</span></span>

* <span data-ttu-id="7f1bb-151">**솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-151">In **Solution Explorer**, right click the project and then select **Properties**.</span></span>
* <span data-ttu-id="7f1bb-152">왼쪽 창에서 **디버그** 탭을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-152">Select the **Debug** tab in the left pane.</span></span>
* <span data-ttu-id="7f1bb-153">**작업 디렉터리**를 프로젝트 디렉터리로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-153">Set **Working directory** to the project directory.</span></span>
* <span data-ttu-id="7f1bb-154">변경 내용을 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="7f1bb-154">Save the changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7f1bb-155">추가 리소스</span><span class="sxs-lookup"><span data-stu-id="7f1bb-155">Additional Resources</span></span>

* [<span data-ttu-id="7f1bb-156">자습서: SQLite를 사용하여 ASP.NET Core에서 새 데이터베이스로 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="7f1bb-156">Tutorial: Get started with EF Core on ASP.NET Core with a new database using SQLite</span></span>](xref:core/get-started/aspnetcore/new-db)
* [<span data-ttu-id="7f1bb-157">자습서: ASP.NET Core에서 Razor Pages 시작</span><span class="sxs-lookup"><span data-stu-id="7f1bb-157">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="7f1bb-158">자습서: ASP.NET Core에서 Entity Framework Core를 사용한 Razor 페이지</span><span class="sxs-lookup"><span data-stu-id="7f1bb-158">Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
