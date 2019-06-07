---
title: ASP.NET Core에서 시작 - 새 데이터베이스 - EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 2eb1668b8c077fabc9cb21088452fd1bead7ff22
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452242"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="986ae-102">ASP.NET Core에서 새 데이터베이스로 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="986ae-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="986ae-103">이 자습서에서는 Entity Framework Core를 사용하여 기본 데이터 액세스를 수행하는 ASP.NET Core MVC 애플리케이션을 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="986ae-104">이 자습서에서는 마이그레이션을 사용하여 데이터 모델에서 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-104">The tutorial uses migrations to create the database from the data model.</span></span>

<span data-ttu-id="986ae-105">Windows에서 Visual Studio 2017을 사용하거나 Windows, macOS 또는 Linux에서 .NET Core CLI를 사용하여 자습서를 진행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-105">You can follow the tutorial by using Visual Studio 2017 on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="986ae-106">GitHub에서 이 문서의 샘플을 봅니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-106">View this article's sample on GitHub:</span></span>
* [<span data-ttu-id="986ae-107">SQL Server가 있는 Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="986ae-107">Visual Studio 2017 with SQL Server</span></span>](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* <span data-ttu-id="986ae-108">[SQLite가 있는 .NET Core CLI](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite)</span><span class="sxs-lookup"><span data-stu-id="986ae-108">[.NET Core CLI with SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="986ae-109">전제 조건</span><span class="sxs-lookup"><span data-stu-id="986ae-109">Prerequisites</span></span>

<span data-ttu-id="986ae-110">다음 소프트웨어를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-110">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="986ae-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="986ae-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="986ae-112">다음 워크로드가 포함된 [Visual Studio 2017 버전 15.7 이상](https://www.visualstudio.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="986ae-112">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="986ae-113">**ASP.NET 및 웹 개발**(**웹 및 클라우드** 아래)</span><span class="sxs-lookup"><span data-stu-id="986ae-113">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="986ae-114">**.NET Core 플랫폼 간 개발**(**기타 도구 집합** 아래)</span><span class="sxs-lookup"><span data-stu-id="986ae-114">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="986ae-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="986ae-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="986ae-116">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="986ae-116">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="986ae-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="986ae-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="986ae-118">새 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="986ae-118">Create a new project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="986ae-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="986ae-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="986ae-120">Visual Studio 2017을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-120">Open Visual Studio 2017</span></span>
* <span data-ttu-id="986ae-121">**파일 > 새로 만들기 > 프로젝트**</span><span class="sxs-lookup"><span data-stu-id="986ae-121">**File > New > Project**</span></span>
* <span data-ttu-id="986ae-122">왼쪽 메뉴에서 **설치됨 > Visual C# > .NET Core**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-122">From the left menu, select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="986ae-123">**새 ASP.NET Core 웹 애플리케이션**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-123">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="986ae-124">이름으로 **EFGetStarted.AspNetCore.NewDb**를 입력하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-124">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="986ae-125">**새 ASP.NET Core 웹 애플리케이션** 대화 상자에서:</span><span class="sxs-lookup"><span data-stu-id="986ae-125">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="986ae-126">드롭다운 목록에서 **.NET Core** 및 **ASP.NET Core 2.1**이 선택되어 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-126">Make sure that **.NET Core** and **ASP.NET Core 2.1** are selected in the drop-down lists</span></span>
  * <span data-ttu-id="986ae-127">**웹 애플리케이션(모델-뷰-컨트롤러)** 프로젝트 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-127">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="986ae-128">**인증**이 **인증 없음**으로 설정되었는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-128">Make sure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="986ae-129">**확인** 을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-129">Click **OK**</span></span>

<span data-ttu-id="986ae-130">경고: **인증**에 **없음** 대신 **개별 사용자 계정**을 사용하는 경우 Entity Framework Core 모델은 `Models\IdentityModel.cs`의 프로젝트에 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-130">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to the project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="986ae-131">이 자습서에서 배운 기술을 사용하면 두 번째 모델을 추가하거나 기존 모델을 확장하여 엔터티 클래스를 포함하도록 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-131">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="986ae-132">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="986ae-132">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="986ae-133">다음 명령을 실행하여 MVC 프로젝트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-133">Run the following command to create an MVC project:</span></span>

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* <span data-ttu-id="986ae-134">프로젝트 디렉터리로 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-134">Change to the project directory.</span></span> <span data-ttu-id="986ae-135">입력하는 다음 명령은 새 프로젝트에 대해 실행해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-135">The next commands you enter need to run against the new project.</span></span>

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a><span data-ttu-id="986ae-136">Entity Framework Core 설치</span><span class="sxs-lookup"><span data-stu-id="986ae-136">Install Entity Framework Core</span></span>

<span data-ttu-id="986ae-137">EF Core를 설치하려면 대상으로 지정할 EF Core 데이터베이스 공급자에 대한 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="986ae-138">사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="986ae-138">For a list of available providers, see [Database Providers](../../providers/index.md).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="986ae-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="986ae-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="986ae-140">이 자습서에서는 SQL Server를 사용하기 때문에 공급자 패키지를 설치할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-140">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="986ae-141">SQL Server 공급자 패키지는 [Microsoft.AspnetCore.App 메타패키지](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1)에 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-141">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="986ae-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="986ae-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="986ae-143">이 자습서에서는 .NET Core가 지원하는 모든 플랫폼에서 실행되는 SQLite를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-143">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span>

* <span data-ttu-id="986ae-144">다음 명령을 실행하여 SQLite 공급자를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-144">Run the following command to install the SQLite provider:</span></span>

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a><span data-ttu-id="986ae-145">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="986ae-145">Create the model</span></span>

<span data-ttu-id="986ae-146">모델을 구성하는 컨텍스트 클래스 및 엔터티 클래스를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-146">Define a context class and entity classes that make up the model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="986ae-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="986ae-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="986ae-148">**Models** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 > 클래스**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-148">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="986ae-149">이름으로 **Model.cs**를 입력하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-149">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="986ae-150">파일의 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-150">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="986ae-151">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="986ae-151">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="986ae-152">**모델** 폴더에서 다음 코드로 **Model.cs**를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-152">In the **Models** folder create **Model.cs** with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

<span data-ttu-id="986ae-153">프로덕션 앱은 일반적으로 각 클래스를 별도의 파일에 넣습니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-153">A production app would typically put each class in a separate file.</span></span> <span data-ttu-id="986ae-154">편의상 이 자습서는 이러한 클래스를 하나의 파일에 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-154">For the sake of simplicity, this tutorial puts these classes in one file.</span></span>

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="986ae-155">종속성 주입으로 컨텍스트 등록</span><span class="sxs-lookup"><span data-stu-id="986ae-155">Register the context with dependency injection</span></span>

<span data-ttu-id="986ae-156">`BloggingContext`를 MVC 컨트롤러에 사용할 수 있도록 하려면 `Startup.cs`의 서비스로 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-156">To make `BloggingContext` available to MVC controllers, register it as a service in `Startup.cs`.</span></span>

<span data-ttu-id="986ae-157">애플리케이션 시작 중에 서비스(예: `BloggingContext`)가 [종속성 주입](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html)에 등록되므로 생성자 매개 변수 및 속성을 통해 서비스(예: MVC 컨트롤러)를 사용하는 구성 요소에 자동으로 제공될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-157">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup so that they can be provided automatically to components that consume services (such as MVC controllers) via constructor parameters and properties.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="986ae-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="986ae-158">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="986ae-159">**Startup.cs**에서 다음 `using` 문을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-159">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* <span data-ttu-id="986ae-160">다음 강조 표시된 코드를 `ConfigureServices` 메서드에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-160">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="986ae-161">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="986ae-161">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="986ae-162">**Startup.cs**에서 다음 `using` 문을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-162">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* <span data-ttu-id="986ae-163">다음 강조 표시된 코드를 `ConfigureServices` 메서드에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-163">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

<span data-ttu-id="986ae-164">일반적으로 프로덕션 앱은 연결 문자열을 구성 파일 또는 환경 변수에 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-164">A production app would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="986ae-165">편의상 이 자습서는 코드에서 정의했습니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-165">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="986ae-166">자세한 내용은 [연결 문자열](../../miscellaneous/connection-strings.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="986ae-166">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="986ae-167">데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="986ae-167">Create the database</span></span>

<span data-ttu-id="986ae-168">다음 단계에서는 [마이그레이션](xref:core/managing-schemas/migrations/index)을 사용하여 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-168">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="986ae-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="986ae-169">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="986ae-170">**도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="986ae-170">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="986ae-171">다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-171">Run the following commands:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="986ae-172">`The term 'add-migration' is not recognized as the name of a cmdlet`이라는 오류가 표시되면 Visual Studio를 닫았다가 다시 엽니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-172">If you get an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>

  <span data-ttu-id="986ae-173">`Add-Migration` 명령은 마이그레이션을 스캐폴딩하여 모델에 대한 초기 테이블 집합을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-173">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="986ae-174">`Update-Database` 명령은 데이터베이스를 만들고 새 마이그레이션을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-174">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="986ae-175">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="986ae-175">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="986ae-176">다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-176">Run the following commands:</span></span>

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="986ae-177">`migrations` 명령은 마이그레이션을 스캐폴딩하여 모델에 대한 초기 테이블 집합을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-177">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="986ae-178">`database update` 명령은 데이터베이스를 만들고 새 마이그레이션을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-178">The `database update` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-a-controller"></a><span data-ttu-id="986ae-179">컨트롤러 만들기</span><span class="sxs-lookup"><span data-stu-id="986ae-179">Create a controller</span></span>

<span data-ttu-id="986ae-180">`Blog` 엔터티에 대한 컨트롤러 및 보기를 스캐폴드합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-180">Scaffold a controller and views for the `Blog` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="986ae-181">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="986ae-181">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="986ae-182">**솔루션 탐색기**에서 **컨트롤러** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 > 컨트롤러**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-182">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="986ae-183">**보기 포함 MVC 컨트롤러, Entity Framework 사용**을 선택하고 **추가**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-183">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="986ae-184">**모델 클래스**를 **Blog**로 설정하고 **데이터 컨텍스트 클래스**를 **BloggingContext**로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-184">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="986ae-185">**추가**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-185">Click **Add**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="986ae-186">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="986ae-186">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="986ae-187">다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-187">Run the following commands:</span></span>

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  <span data-ttu-id="986ae-188">`tool install` 및 `add package` 명령은 컨트롤러 및 뷰를 스캐폴딩할 수 있는 도구를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-188">The `tool install` and `add package` commands install the tooling that can scaffold controllers and views.</span></span> <span data-ttu-id="986ae-189">`restore` 명령은 모든 프로젝트의 패키지가 다운로드되는지 확인하고 `aspnet-codegenerator` 명령은 스캐폴딩을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-189">The `restore` command ensures that all of the project's packages are downloaded, and the `aspnet-codegenerator` command does the scaffolding.</span></span>
---

<span data-ttu-id="986ae-190">스캐폴딩 엔진은 다음 파일을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-190">The scaffolding engine creates the following files:</span></span>

* <span data-ttu-id="986ae-191">컨트롤러(*Controllers/BlogsController.cs*)</span><span class="sxs-lookup"><span data-stu-id="986ae-191">A controller (*Controllers/BlogsController.cs*)</span></span>
* <span data-ttu-id="986ae-192">만들기, 삭제, 세부 정보, 편집 및 인덱스 페이지에 대한 Razor 뷰(_Views/Blogs/\*.cshtml_)</span><span class="sxs-lookup"><span data-stu-id="986ae-192">Razor views for Create, Delete, Details, Edit, and Index pages (_Views/Blogs/\*.cshtml_)</span></span>

## <a name="run-the-application"></a><span data-ttu-id="986ae-193">애플리케이션 실행</span><span class="sxs-lookup"><span data-stu-id="986ae-193">Run the application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="986ae-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="986ae-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="986ae-195">**디버그** > **디버깅하지 않고 시작**</span><span class="sxs-lookup"><span data-stu-id="986ae-195">**Debug** > **Start Without Debugging**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="986ae-196">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="986ae-196">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet run
```
---

* <span data-ttu-id="986ae-197">`/Blogs`로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-197">Navigate to `/Blogs`</span></span>

* <span data-ttu-id="986ae-198">**새로 만들기** 링크를 사용하여 일부 블로그 항목을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-198">Use the **Create New** link to create some blog entries.</span></span>

  ![페이지 만들기](_static/create.png)

* <span data-ttu-id="986ae-200">**세부 정보**를 테스트하고, 링크를 **편집** 및 **삭제**합니다.</span><span class="sxs-lookup"><span data-stu-id="986ae-200">Test the **Details**, **Edit**, and **Delete** links.</span></span>

  ![인덱스 페이지](_static/index-new-db.png)

## <a name="additional-tutorials"></a><span data-ttu-id="986ae-202">추가 자습서</span><span class="sxs-lookup"><span data-stu-id="986ae-202">Additional tutorials</span></span>

* [<span data-ttu-id="986ae-203">SQLite를 사용하여 .NET Core에서 새 데이터베이스로 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="986ae-203">Get started with EF Core on .NET Core with a new database using SQLite</span></span>](xref:core/get-started/netcore/new-db-sqlite)
* <span data-ttu-id="986ae-204">ASP.NET Core MVC:</span><span class="sxs-lookup"><span data-stu-id="986ae-204">ASP.NET Core MVC:</span></span>
  * [<span data-ttu-id="986ae-205">ASP.NET Core MVC 시작</span><span class="sxs-lookup"><span data-stu-id="986ae-205">Get started with ASP.NET Core MVC</span></span>](/aspnet/core/tutorials/first-mvc-app/start-mvc)
  * [<span data-ttu-id="986ae-206">ASP.NET MVC 웹앱에서 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="986ae-206">Get started with EF Core in an ASP.NET MVC web app</span></span>](/aspnet/core/data/ef-mvc/intro)
* <span data-ttu-id="986ae-207">[Razor Pages](/aspnet/core/razor-pages/index):</span><span class="sxs-lookup"><span data-stu-id="986ae-207">[Razor Pages](/aspnet/core/razor-pages/index):</span></span>
  * <span data-ttu-id="986ae-208">[Get started with Razor Pages in ASP.NET Core](/aspnet/core/tutorials/razor-pages/razor-pages-start)(ASP.NET Core에서 Razor Pages 시작)</span><span class="sxs-lookup"><span data-stu-id="986ae-208">[Get started with Razor Pages in ASP.NET Core](/aspnet/core/tutorials/razor-pages/razor-pages-start)</span></span>
  * [<span data-ttu-id="986ae-209">ASP.NET Core에서 Entity Framework Core를 사용한 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="986ae-209">Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
