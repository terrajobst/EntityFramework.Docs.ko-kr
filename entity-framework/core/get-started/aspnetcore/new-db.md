---
title: ASP.NET Core에서 시작 - 새 데이터베이스 - EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 08/03/2018
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 9e86bc9cff028ad9791f23cbb45f0a93110c0064
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614352"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="61543-102">ASP.NET Core에서 새 데이터베이스로 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="61543-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="61543-103">이 자습서에서는 Entity Framework Core를 사용하여 기본 데이터 액세스를 수행하는 ASP.NET Core MVC 응용 프로그램을 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="61543-104">마이그레이션을 사용하여 EF Core 모델에서 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="61543-104">You use migrations to create the database from your EF Core model.</span></span>

<span data-ttu-id="61543-105">[GitHub에서 이 아티클의 샘플을 봅니다](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span><span class="sxs-lookup"><span data-stu-id="61543-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="61543-106">전제 조건</span><span class="sxs-lookup"><span data-stu-id="61543-106">Prerequisites</span></span>

<span data-ttu-id="61543-107">다음 소프트웨어를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-107">Install the following software:</span></span>

* <span data-ttu-id="61543-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) 및 다음 워크로드:</span><span class="sxs-lookup"><span data-stu-id="61543-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="61543-109">**ASP.NET 및 웹 개발**(**웹 및 클라우드** 아래)</span><span class="sxs-lookup"><span data-stu-id="61543-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="61543-110">**.NET Core 플랫폼 간 개발**(**기타 도구 집합** 아래)</span><span class="sxs-lookup"><span data-stu-id="61543-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="61543-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="61543-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="61543-112">Visual Studio 2017에서 새 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="61543-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="61543-113">Visual Studio 2017을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="61543-113">Open Visual Studio 2017</span></span>
* <span data-ttu-id="61543-114">**파일 > 새로 만들기 > 프로젝트**</span><span class="sxs-lookup"><span data-stu-id="61543-114">**File > New > Project**</span></span>
* <span data-ttu-id="61543-115">왼쪽 메뉴에서 **설치됨 Visual C# > .NET Core**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-115">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="61543-116">**새 ASP.NET Core 웹 응용 프로그램**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-116">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="61543-117">이름으로 **EFGetStarted.AspNetCore.NewDb**를 입력하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-117">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="61543-118">**새 ASP.NET Core 웹 응용 프로그램** 대화 상자에서:</span><span class="sxs-lookup"><span data-stu-id="61543-118">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="61543-119">드롭다운 목록에서 **.NET Core** 및 **ASP.NET Core 2.1** 옵션이 선택되어 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-119">Ensure the options **.NET Core** and **ASP.NET Core 2.1** are selected in the drop down lists</span></span>
  * <span data-ttu-id="61543-120">**웹 응용 프로그램(모델-뷰-컨트롤러)** 프로젝트 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-120">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="61543-121">**인증**이 **인증 없음**으로 설정되었는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-121">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="61543-122">**확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-122">Click **OK**</span></span>

<span data-ttu-id="61543-123">경고: **인증**에 **없음** 대신 **개별 사용자 계정**을 사용하는 경우 Entity Framework Core 모델은 `Models\IdentityModel.cs`의 프로젝트에 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="61543-123">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="61543-124">이 자습서에서 배운 기술을 사용하면 두 번째 모델을 추가하거나 기존 모델을 확장하여 엔터티 클래스를 포함하도록 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="61543-124">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="61543-125">Entity Framework Core 설치</span><span class="sxs-lookup"><span data-stu-id="61543-125">Install Entity Framework Core</span></span>

<span data-ttu-id="61543-126">EF Core를 설치하려면 대상으로 지정할 EF Core 데이터베이스 공급자에 대한 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-126">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="61543-127">사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="61543-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="61543-128">이 자습서에서는 SQL Server를 사용하기 때문에 공급자 패키지를 설치할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="61543-128">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="61543-129">SQL Server 공급자 패키지는 [Microsoft.AspnetCore.App 메타패키지](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1)에 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="61543-129">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="create-the-model"></a><span data-ttu-id="61543-130">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="61543-130">Create the model</span></span>

<span data-ttu-id="61543-131">모델을 구성하는 컨텍스트 클래스 및 엔터티 클래스를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-131">Define a context class and entity classes that make up the model:</span></span>

* <span data-ttu-id="61543-132">**Models** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 > 클래스**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-132">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="61543-133">이름으로 **Model.cs**를 입력하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-133">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="61543-134">파일의 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="61543-134">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="61543-135">일반적으로 실제 앱에서는 모델의 각 클래스를 별도의 파일에 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-135">In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="61543-136">편의상 이 자습서는 모든 클래스를 하나의 파일에 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-136">For the sake of simplicity, this tutorial puts all the classes in one file.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="61543-137">종속성 주입에 컨텍스트 등록</span><span class="sxs-lookup"><span data-stu-id="61543-137">Register your context with dependency injection</span></span>

<span data-ttu-id="61543-138">서비스(예: `BloggingContext`)는 응용 프로그램 시작 중에 [종속성 주입](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html)에 등록됩니다.</span><span class="sxs-lookup"><span data-stu-id="61543-138">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="61543-139">이러한 서비스(예: MVC 컨트롤러)가 필요한 구성 요소에는 생성자 매개 변수 또는 속성을 통해 이러한 서비스가 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="61543-139">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="61543-140">`BloggingContext`를 MVC 컨트롤러에 사용할 수 있도록 서비스로 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-140">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="61543-141">**Startup.cs**를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="61543-141">Open **Startup.cs**</span></span>
* <span data-ttu-id="61543-142">다음 `using` 문을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-142">Add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="61543-143">`AddDbContext` 메서드를 호출하여 컨텍스트를 서비스로 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-143">Call the `AddDbContext` method to register the context as a service.</span></span>

* <span data-ttu-id="61543-144">다음 강조 표시된 코드를 `ConfigureServices` 메서드에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-144">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

<span data-ttu-id="61543-145">참고: 일반적으로 실제 앱은 연결 문자열을 구성 파일 또는 환경 변수에 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-145">Note: A real app would generally put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="61543-146">편의상 이 자습서는 코드에서 정의했습니다.</span><span class="sxs-lookup"><span data-stu-id="61543-146">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="61543-147">자세한 내용은 [연결 문자열](../../miscellaneous/connection-strings.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="61543-147">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="61543-148">데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="61543-148">Create the database</span></span>

<span data-ttu-id="61543-149">모델을 만든 후 [마이그레이션](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)을 사용하여 데이터베이스를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="61543-149">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="61543-150">**도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="61543-150">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="61543-151">`Add-Migration InitialCreate`를 실행하여 마이그레이션을 스캐폴딩하고 모델에 대한 초기 테이블 집합을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="61543-151">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="61543-152">`The term 'add-migration' is not recognized as the name of a cmdlet`이라는 오류가 표시되면 Visual Studio를 닫았다가 다시 엽니다.</span><span class="sxs-lookup"><span data-stu-id="61543-152">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="61543-153">`Update-Database`를 실행하여 새 마이그레이션을 데이터베이스에 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-153">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="61543-154">이 명령은 마이그레이션을 적용하기 전에 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="61543-154">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="61543-155">컨트롤러 만들기</span><span class="sxs-lookup"><span data-stu-id="61543-155">Create a controller</span></span>

<span data-ttu-id="61543-156">`Blog` 엔터티에 대한 컨트롤러 및 보기를 스캐폴드합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-156">Scaffold a controller and views for the `Blog` entity.</span></span>

* <span data-ttu-id="61543-157">**솔루션 탐색기**에서 **컨트롤러** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 > 컨트롤러**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-157">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="61543-158">**보기 포함 MVC 컨트롤러, Entity Framework 사용**을 선택하고 **추가**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-158">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="61543-159">**모델 클래스**를 **Blog**로 설정하고 **데이터 컨텍스트 클래스**를 **BloggingContext**로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-159">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="61543-160">**추가**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-160">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="61543-161">응용 프로그램 실행</span><span class="sxs-lookup"><span data-stu-id="61543-161">Run the application</span></span>

<span data-ttu-id="61543-162">F5 키를 눌러 앱을 실행하고 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-162">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="61543-163">`/Blogs`로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-163">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="61543-164">만들기 링크를 사용하여 일부 블로그 항목을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="61543-164">Use the create link to create some blog entries.</span></span> <span data-ttu-id="61543-165">세부 정보를 테스트하고 링크를 삭제합니다.</span><span class="sxs-lookup"><span data-stu-id="61543-165">Test the details and delete links.</span></span>

![이미지](_static/create.png)

![이미지](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="61543-168">추가 리소스</span><span class="sxs-lookup"><span data-stu-id="61543-168">Additional Resources</span></span>

* <span data-ttu-id="61543-169">[EF - SQLite를 사용하여 새 데이터베이스 만들기](xref:core/get-started/netcore/new-db-sqlite) - 플랫폼 간 콘솔 EF 자습서.</span><span class="sxs-lookup"><span data-stu-id="61543-169">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="61543-170">Mac 또는 Linux의 ASP.NET Core MVC 소개</span><span class="sxs-lookup"><span data-stu-id="61543-170">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="61543-171">Visual Studio의 ASP.NET Core MVC 소개</span><span class="sxs-lookup"><span data-stu-id="61543-171">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="61543-172">Visual Studio를 사용하여 ASP.NET Core 및 Entity Framework Core 시작</span><span class="sxs-lookup"><span data-stu-id="61543-172">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
