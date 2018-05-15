---
title: ASP.NET Core에서 시작 - 새 데이터베이스 - EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 80477ca57b8b3df6de8ba3595c9056c6b8412040
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="c82c7-102">ASP.NET Core에서 새 데이터베이스로 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="c82c7-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="c82c7-103">이 연습에서는 Entity Framework Core를 사용하여 기본 데이터 액세스를 수행하는 ASP.NET Core MVC 응용 프로그램을 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="c82c7-104">마이그레이션을 사용하여 EF Core 모델에서 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-104">You will use migrations to create the database from your EF Core model.</span></span> <span data-ttu-id="c82c7-105">더 많은 Entity Framework Core 자습서를 보려면 [추가 리소스](#additional-resources)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="c82c7-105">See [Additional Resources](#additional-resources) for more Entity Framework Core tutorials.</span></span>

<span data-ttu-id="c82c7-106">이 자습서에는 다음이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-106">This tutorial requires:</span></span>
* <span data-ttu-id="c82c7-107">[Visual Studio 201715.3](https://www.visualstudio.com/downloads/) 및 다음 워크로드:</span><span class="sxs-lookup"><span data-stu-id="c82c7-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="c82c7-108">**ASP.NET 및 웹 개발**(**웹 및 클라우드** 아래)</span><span class="sxs-lookup"><span data-stu-id="c82c7-108">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="c82c7-109">**.NET Core 플랫폼 간 개발**(**기타 도구 집합** 아래)</span><span class="sxs-lookup"><span data-stu-id="c82c7-109">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="c82c7-110">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="c82c7-110">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>

> [!TIP]  
> <span data-ttu-id="c82c7-111">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) on GitHub.</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="c82c7-112">Visual Studio 2017에서 새 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="c82c7-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="c82c7-113">**파일 > 새로 만들기 > 프로젝트**</span><span class="sxs-lookup"><span data-stu-id="c82c7-113">**File > New > Project**</span></span>
* <span data-ttu-id="c82c7-114">왼쪽 메뉴에서 **설치됨 > 템플릿 > Visual C# > .NET Core**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-114">From the left menu select **Installed > Templates > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="c82c7-115">**새 ASP.NET Core 웹 응용 프로그램**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-115">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="c82c7-116">이름으로 **EFGetStarted.AspNetCore.NewDb**를 입력하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-116">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="c82c7-117">**새 ASP.NET Core 웹 응용 프로그램** 대화 상자에서:</span><span class="sxs-lookup"><span data-stu-id="c82c7-117">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="c82c7-118">드롭다운 목록에서 **.NET Core** 및 **ASP.NET Core 2.0** 옵션이 선택되어 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-118">Ensure the options **.NET Core** and **ASP.NET Core 2.0** are selected in the drop down lists</span></span>
  * <span data-ttu-id="c82c7-119">**웹 응용 프로그램(모델-뷰-컨트롤러)** 프로젝트 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-119">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="c82c7-120">**인증**이 **인증 없음**으로 설정되었는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-120">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="c82c7-121">**확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-121">Click **OK**</span></span>

<span data-ttu-id="c82c7-122">경고: **인증**에 **없음** 대신 **개별 사용자 계정**을 사용하는 경우 Entity Framework Core 모델은 `Models\IdentityModel.cs`의 프로젝트에 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-122">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="c82c7-123">이 연습에서 학습할 기술을 사용하면 두 번째 모델을 추가하거나 기존 모델을 확장하여 엔터티 클래스를 포함하도록 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-123">Using the techniques you will learn in this walkthrough, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="c82c7-124">Entity Framework Core 설치</span><span class="sxs-lookup"><span data-stu-id="c82c7-124">Install Entity Framework Core</span></span>

<span data-ttu-id="c82c7-125">대상으로 지정할 EF Core 데이터베이스 공급자에 대한 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-125">Install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="c82c7-126">이 연습에서는 SQL Server를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-126">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="c82c7-127">사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="c82c7-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="c82c7-128">**도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="c82c7-128">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="c82c7-129">`Install-Package Microsoft.EntityFrameworkCore.SqlServer` 실행</span><span class="sxs-lookup"><span data-stu-id="c82c7-129">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="c82c7-130">일부 Entity Framework Core Tools를 사용하여 EF Core 모델에서 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-130">We will be using some Entity Framework Core Tools to create a database from your EF Core model.</span></span> <span data-ttu-id="c82c7-131">따라서 도구 패키지도 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-131">So we will install the tools package as well:</span></span>

* <span data-ttu-id="c82c7-132">`Install-Package Microsoft.EntityFrameworkCore.Tools` 실행</span><span class="sxs-lookup"><span data-stu-id="c82c7-132">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="c82c7-133">일부 ASP.NET Core 스캐폴딩 도구를 사용하여 나중에 컨트롤러와 뷰를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-133">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="c82c7-134">따라서 이 디자인 패키지도 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-134">So we will install this design package as well:</span></span>

* <span data-ttu-id="c82c7-135">`Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design` 실행</span><span class="sxs-lookup"><span data-stu-id="c82c7-135">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="c82c7-136">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="c82c7-136">Create the model</span></span>

<span data-ttu-id="c82c7-137">모델을 구성하는 컨텍스트 및 엔터티 클래스를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-137">Define a context and entity classes that make up the model:</span></span>

* <span data-ttu-id="c82c7-138">**Models** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 > 클래스**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-138">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="c82c7-139">이름으로 **Model.cs**를 입력하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-139">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="c82c7-140">파일의 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-140">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="c82c7-141">참고: 일반적으로 실제 앱에서는 모델의 각 클래스를 별도의 파일에 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-141">Note: In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="c82c7-142">간단한 설명을 위해 모든 클래스를 이 학습서에 대한 하나의 파일에 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-142">For the sake of simplicity, we are putting all the classes in one file for this tutorial.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="c82c7-143">종속성 주입에 컨텍스트 등록</span><span class="sxs-lookup"><span data-stu-id="c82c7-143">Register your context with dependency injection</span></span>

<span data-ttu-id="c82c7-144">서비스(예: `BloggingContext`)는 응용 프로그램 시작 중에 [종속성 주입](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html)에 등록됩니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-144">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="c82c7-145">이러한 서비스(예: MVC 컨트롤러)가 필요한 구성 요소에는 생성자 매개 변수 또는 속성을 통해 이러한 서비스가 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-145">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="c82c7-146">MVC 컨트롤러에서 `BloggingContext`를 사용하려면 이 항목을 서비스로 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-146">In order for our MVC controllers to make use of `BloggingContext` we will register it as a service.</span></span>

* <span data-ttu-id="c82c7-147">**Startup.cs**를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-147">Open **Startup.cs**</span></span>
* <span data-ttu-id="c82c7-148">다음 `using` 문을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-148">Add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="c82c7-149">`AddDbContext` 메서드를 추가하여 서비스로 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-149">Add the `AddDbContext` method to register it as a service:</span></span>

* <span data-ttu-id="c82c7-150">`ConfigureServices` 메서드에 다음 코드를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-150">Add the following code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

<span data-ttu-id="c82c7-151">참고: 일반적으로 실제 앱은 연결 문자열을 구성 파일에 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-151">Note: A real app would generally put the connection string in a configuration file.</span></span> <span data-ttu-id="c82c7-152">간단한 설명을 위해 여기에서는 코드로 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-152">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="c82c7-153">자세한 내용은 [연결 문자열](../../miscellaneous/connection-strings.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="c82c7-153">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="c82c7-154">데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="c82c7-154">Create your database</span></span>

<span data-ttu-id="c82c7-155">모델을 만든 후 [마이그레이션](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)을 사용하여 데이터베이스를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-155">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="c82c7-156">PMC를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-156">Open the PMC:</span></span>

  <span data-ttu-id="c82c7-157">**도구 -> NuGet 패키지 관리자 -> 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="c82c7-157">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="c82c7-158">`Add-Migration InitialCreate`를 실행하여 마이그레이션을 스캐폴딩하고 모델에 대한 초기 테이블 집합을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-158">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="c82c7-159">`The term 'add-migration' is not recognized as the name of a cmdlet`이라는 오류가 표시되면 Visual Studio를 닫았다가 다시 엽니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-159">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="c82c7-160">`Update-Database`를 실행하여 새 마이그레이션을 데이터베이스에 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-160">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="c82c7-161">이 명령은 마이그레이션을 적용하기 전에 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-161">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="c82c7-162">컨트롤러 만들기</span><span class="sxs-lookup"><span data-stu-id="c82c7-162">Create a controller</span></span>

<span data-ttu-id="c82c7-163">프로젝트에서 스캐폴딩을 사용하도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-163">Enable scaffolding in the project:</span></span>

* <span data-ttu-id="c82c7-164">**솔루션 탐색기**에서 **컨트롤러** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 > 컨트롤러**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-164">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="c82c7-165">**최소 종속성**을 선택하고 **추가**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-165">Select **Minimal Dependencies** and click **Add**.</span></span>
* <span data-ttu-id="c82c7-166">*ScaffoldingReadMe.txt* 파일을 무시하거나 삭제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-166">You can ignore or delete the *ScaffoldingReadMe.txt* file.</span></span>

<span data-ttu-id="c82c7-167">이제 스캐폴딩이 사용 가능하므로 `Blog` 엔터티에 대한 컨트롤러를 스캐폴딩할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-167">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="c82c7-168">**솔루션 탐색기**에서 **컨트롤러** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 > 컨트롤러**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-168">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="c82c7-169">**보기 포함 MVC 컨트롤러, Entity Framework**를 선택하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-169">Select **MVC Controller with views, using Entity Framework** and click **Ok**.</span></span>
* <span data-ttu-id="c82c7-170">**모델 클래스**를 **Blog**로 설정하고 **데이터 컨텍스트 클래스**를 **BloggingContext**로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-170">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="c82c7-171">**추가**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-171">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="c82c7-172">응용 프로그램 실행</span><span class="sxs-lookup"><span data-stu-id="c82c7-172">Run the application</span></span>

<span data-ttu-id="c82c7-173">F5 키를 눌러 앱을 실행하고 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-173">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="c82c7-174">`/Blogs`로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-174">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="c82c7-175">만들기 링크를 사용하여 일부 블로그 항목을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-175">Use the create link to create some blog entries.</span></span> <span data-ttu-id="c82c7-176">세부 정보를 테스트하고 링크를 삭제합니다.</span><span class="sxs-lookup"><span data-stu-id="c82c7-176">Test the details and delete links.</span></span>

![이미지](_static/create.png)

![이미지](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="c82c7-179">추가 리소스</span><span class="sxs-lookup"><span data-stu-id="c82c7-179">Additional Resources</span></span>

* <span data-ttu-id="c82c7-180">[EF - SQLite를 사용하여 새 데이터베이스 만들기](xref:core/get-started/netcore/new-db-sqlite) - 플랫폼 간 콘솔 EF 자습서.</span><span class="sxs-lookup"><span data-stu-id="c82c7-180">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="c82c7-181">Mac 또는 Linux의 ASP.NET Core MVC 소개</span><span class="sxs-lookup"><span data-stu-id="c82c7-181">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="c82c7-182">Visual Studio의 ASP.NET Core MVC 소개</span><span class="sxs-lookup"><span data-stu-id="c82c7-182">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="c82c7-183">Visual Studio를 사용하여 ASP.NET Core 및 Entity Framework Core 시작</span><span class="sxs-lookup"><span data-stu-id="c82c7-183">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
