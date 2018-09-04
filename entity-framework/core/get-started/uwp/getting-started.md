---
title: UWP에서 시작 - 새 데이터베이스 - EF 코어
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996912"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="c6793-102">UWP(유니버설 Windows 플랫폼)에서 새 데이터베이스로 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="c6793-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="c6793-103">이 자습서에서는 Entity Framework Core를 사용하여 로컬 SQLite 데이터베이스에 대한 기본 데이터 액세스를 수행하는 UWP(유니버설 Windows 플랫폼) 응용 프로그램을 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="c6793-104">[GitHub에서 이 아티클의 샘플을 봅니다](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="c6793-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c6793-105">전제 조건</span><span class="sxs-lookup"><span data-stu-id="c6793-105">Prerequisites</span></span>

* <span data-ttu-id="c6793-106">[Windows 10 Fall Creators Update(10.0; 빌드 16299) 이상](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="c6793-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="c6793-107">[Visual Studio 2017 버전 15.7 이상](https://www.visualstudio.com/downloads/)(**유니버설 Windows 플랫폼 개발** 워크로드 포함).</span><span class="sxs-lookup"><span data-stu-id="c6793-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="c6793-108">[.NET Core 2.1 SDK 이상](https://www.microsoft.com/net/core) 또는 이후.</span><span class="sxs-lookup"><span data-stu-id="c6793-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="create-a-model-project"></a><span data-ttu-id="c6793-109">모델 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="c6793-109">Create a model project</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c6793-110">.NET Core 도구가 UWP 프로젝트와 상호 작용하는 방법의 제한 사항으로 인해 **PMC(패키지 관리자 콘솔)** 에서 마이그레이션 명령을 실행할 수 있으려면 모델을 UWP 이외 프로젝트에 배치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-110">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the **Package Manager Console** (PMC)</span></span>

* <span data-ttu-id="c6793-111">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-111">Open Visual Studio</span></span>

* <span data-ttu-id="c6793-112">**파일 > 새로 만들기 > 프로젝트**</span><span class="sxs-lookup"><span data-stu-id="c6793-112">**File > New > Project**</span></span>

* <span data-ttu-id="c6793-113">왼쪽 메뉴에서 **설치됨 > Visual C# > .NET Standard**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-113">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="c6793-114">**클래스 라이브러리(.NET Standard)** 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-114">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="c6793-115">프로젝트 이름을 *Blogging.Model*로 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-115">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="c6793-116">솔루션의 이름을 *Blogging*으로 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-116">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="c6793-117">**확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-117">Click **OK**.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="c6793-118">Entity Framework Core 설치</span><span class="sxs-lookup"><span data-stu-id="c6793-118">Install Entity Framework Core</span></span>

<span data-ttu-id="c6793-119">EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="c6793-120">이 자습서에서는 SQLite를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-120">This tutorial uses SQLite.</span></span> <span data-ttu-id="c6793-121">사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="c6793-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="c6793-122">**도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**.</span><span class="sxs-lookup"><span data-stu-id="c6793-122">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="c6793-123">`Install-Package Microsoft.EntityFrameworkCore.Sqlite` 실행</span><span class="sxs-lookup"><span data-stu-id="c6793-123">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="c6793-124">이 자습서의 뒷부분에서는 일부 Entity Framework Core 도구를 사용하여 데이터베이스를 유지 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-124">Later in this tutorial you will be using some Entity Framework Core tools to maintain the database.</span></span> <span data-ttu-id="c6793-125">따라서 도구 패키지도 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-125">So install the tools package as well.</span></span>

* <span data-ttu-id="c6793-126">`Install-Package Microsoft.EntityFrameworkCore.Tools` 실행</span><span class="sxs-lookup"><span data-stu-id="c6793-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="c6793-127">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="c6793-127">Create the model</span></span>

<span data-ttu-id="c6793-128">이제 모델을 구성하는 컨텍스트 및 엔터티 클래스를 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-128">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="c6793-129">*Class1.cs*를 삭제합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="c6793-130">다음 코드로 *Model.cs*를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="c6793-131">새 UWP 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="c6793-131">Create a new UWP project</span></span>

* <span data-ttu-id="c6793-132">**솔루션 탐색기**에서 솔루션을 마우스 오른쪽 단추로 클릭한 다음, **추가 > 새 프로젝트**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="c6793-133">왼쪽 메뉴에서 **설치됨 > Visual C# > Windows 유니버설**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-133">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="c6793-134">**빈 앱(유니버설 Windows)** 프로젝트 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-134">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="c6793-135">*Blogging.UWP* 프로젝트 이름을 지정하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-135">Name the project *Blogging.UWP*, and click **OK**</span></span>

* <span data-ttu-id="c6793-136">대상 및 최소 버전을 **Windows 10 Fall Creators Update(10.0; 빌드 16299.0)** 이상으로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-136">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="c6793-137">초기 마이그레이션 만들기</span><span class="sxs-lookup"><span data-stu-id="c6793-137">Create the initial migration</span></span>

<span data-ttu-id="c6793-138">이제 모델이 있으므로, 처음 실행될 때 데이터베이스를 만들 수 있도록 앱을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-138">Now that you have a model, set up the app to create a database the first time it runs.</span></span> <span data-ttu-id="c6793-139">이 섹션에서는 초기 마이그레이션을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-139">In this section, you create the initial migration.</span></span> <span data-ttu-id="c6793-140">다음 섹션에서는 앱이 시작될 때 이 마이그레이션을 적용하는 코드를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-140">In the following section, you add code that applies this migration when the app starts.</span></span>

<span data-ttu-id="c6793-141">마이그레이션 도구는 UWP가 아닌 시작 프로젝트가 필요하므로 먼저 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-141">Migrations tools require a non-UWP startup project, so create that first.</span></span>

* <span data-ttu-id="c6793-142">**솔루션 탐색기**에서 솔루션을 마우스 오른쪽 단추로 클릭한 다음, **추가 > 새 프로젝트**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-142">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="c6793-143">왼쪽 메뉴에서 **설치됨 Visual C# > .NET Core**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-143">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="c6793-144">**콘솔 앱(.NET Core)** 프로젝트 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-144">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="c6793-145">*Blogging.Migrations.Startup* 프로젝트 이름을 지정하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-145">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="c6793-146">*Blogging.Migrations.Startup* 프로젝트의 프로젝트 참조를 *Blogging.Model* 프로젝트에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-146">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

<span data-ttu-id="c6793-147">이제 초기 마이그레이션을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-147">Now you can create your initial migration.</span></span>

* <span data-ttu-id="c6793-148">**도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="c6793-148">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="c6793-149">*Blogging.Model* 프로젝트를 **기본 프로젝트**로 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-149">Select the *Blogging.Model* project as the **Default project**.</span></span>

* <span data-ttu-id="c6793-150">**솔루션 탐색기**에서 *Blogging.Migrations.Startup* 프로젝트를 시작 프로젝트로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-150">In **Solution Explorer**, set the *Blogging.Migrations.Startup* project as the startup project.</span></span>

* <span data-ttu-id="c6793-151">`Add-Migration InitialCreate`를 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-151">Run `Add-Migration InitialCreate`.</span></span>

  <span data-ttu-id="c6793-152">이 명령은 모델에 대한 초기 테이블 집합을 만드는 마이그레이션을 스캐폴딩합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-152">This command scaffolds a migration that creates the initial set of tables for your model.</span></span>

## <a name="create-the-database-on-app-startup"></a><span data-ttu-id="c6793-153">앱 시작 시 데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="c6793-153">Create the database on app startup</span></span>

<span data-ttu-id="c6793-154">앱이 실행되는 장치에서 데이터베이스를 만들려면 응용 프로그램 시작 시 로컬 데이터베이스에 보류 중인 마이그레이션을 적용할 코드를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-154">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="c6793-155">이렇게 하면 앱이 처음 실행될 때 로컬 데이터베이스가 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-155">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="c6793-156">*Blogging.UWP* 프로젝트의 프로젝트 참조를 *Blogging.Model* 프로젝트에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-156">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="c6793-157">*App.xaml.cs*를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-157">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="c6793-158">강조 표시된 코드를 추가하여 보류 중인 마이그레이션을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-158">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="c6793-159">모델을 변경하는 경우 `Add-Migration` 명령을 사용하여 새 마이그레이션을 스캐폴딩하고 데이터베이스에 해당 변경 내용을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-159">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="c6793-160">보류 중인 마이그레이션은 응용 프로그램이 시작될 때 각 장치의 로컬 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-160">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="c6793-161">EF는 데이터베이스의 `__EFMigrationsHistory` 테이블을 사용하여 이미 데이터베이스에 적용된 마이그레이션을 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-161">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="c6793-162">모델 사용</span><span class="sxs-lookup"><span data-stu-id="c6793-162">Use the model</span></span>

<span data-ttu-id="c6793-163">이제 모델을 사용하여 데이터 액세스를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-163">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="c6793-164">*MainPage.xaml*을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-164">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="c6793-165">아래에 강조 표시된 페이지 로드 처리기 및 UI 콘텐츠를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-165">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="c6793-166">이제 코드를 추가하여 데이터베이스와 UI 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-166">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="c6793-167">*MainPage.xaml.cs*를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-167">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="c6793-168">다음 목록에서 강조 표시된 코드를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-168">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="c6793-169">이제 응용 프로그램을 실행하여 작동하는지 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-169">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="c6793-170">**솔루션 탐색기**에서 *Blogging.UWP* 프로젝트를 마우스 오른쪽 단추로 클릭한 다음, **배포**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-170">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="c6793-171">*Blogging.UWP*를 시작 프로젝트로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-171">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="c6793-172">**디버그 > 디버깅하지 않고 시작**</span><span class="sxs-lookup"><span data-stu-id="c6793-172">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="c6793-173">앱을 빌드하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-173">The app builds and runs.</span></span>

* <span data-ttu-id="c6793-174">URL을 입력하고 **추가** 단추를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-174">Enter a URL and click the **Add** button</span></span>

  ![이미지](_static/create.png)

  ![이미지](_static/list.png)

  <span data-ttu-id="c6793-177">짜잔!</span><span class="sxs-lookup"><span data-stu-id="c6793-177">Tada!</span></span> <span data-ttu-id="c6793-178">이제 Entity Framework Core를 실행하는 간단한 UWP 앱이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6793-178">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6793-179">다음 단계</span><span class="sxs-lookup"><span data-stu-id="c6793-179">Next steps</span></span>

<span data-ttu-id="c6793-180">UWP와 함께 EF Core를 사용할 때 알아야 할 호환성 및 성능 정보는 [EF Core에서 지원하는 .NET 구현](../../platforms/index.md#universal-windows-platform)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="c6793-180">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="c6793-181">Entity Framework Core 기능에 대해 자세히 알아보려면 이 설명서의 다른 문서를 확인하세요.</span><span class="sxs-lookup"><span data-stu-id="c6793-181">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
