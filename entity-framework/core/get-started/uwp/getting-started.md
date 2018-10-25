---
title: UWP에서 시작 - 새 데이터베이스 - EF 코어
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 48d26adbe17e4734753a7ada547b9c13317bef0d
ms.sourcegitcommit: 8b42045cd21f80f425a92f5e4e9dd4972a31720b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/14/2018
ms.locfileid: "49315622"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="822a0-102">UWP(유니버설 Windows 플랫폼)에서 새 데이터베이스로 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="822a0-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="822a0-103">이 자습서에서는 Entity Framework Core를 사용하여 로컬 SQLite 데이터베이스에 대한 기본 데이터 액세스를 수행하는 UWP(유니버설 Windows 플랫폼) 응용 프로그램을 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="822a0-104">[GitHub에서 이 아티클의 샘플을 봅니다](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="822a0-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="822a0-105">전제 조건</span><span class="sxs-lookup"><span data-stu-id="822a0-105">Prerequisites</span></span>

* <span data-ttu-id="822a0-106">[Windows 10 Fall Creators Update(10.0; 빌드 16299) 이상](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="822a0-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="822a0-107">[Visual Studio 2017 버전 15.7 이상](https://www.visualstudio.com/downloads/)(**유니버설 Windows 플랫폼 개발** 워크로드 포함).</span><span class="sxs-lookup"><span data-stu-id="822a0-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="822a0-108">[.NET Core 2.1 SDK 이상](https://www.microsoft.com/net/core) 또는 이후.</span><span class="sxs-lookup"><span data-stu-id="822a0-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="822a0-109">이 자습서에서는 Entity Framework Core [마이그레이션](xref:core/managing-schemas/migrations/index) 명령을 사용하여 데이터베이스의 스키마를 작성하고 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-109">This tutorial uses Entity Framework Core [migrations](xref:core/managing-schemas/migrations/index) commands to create and update the schema of the database.</span></span>
> <span data-ttu-id="822a0-110">이러한 명령은 UWP 프로젝트에 작동하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-110">These commands don't work directly with UWP projects.</span></span>
> <span data-ttu-id="822a0-111">이러한 이유로 응용 프로그램의 데이터 모델이 공유 라이브러리 프로젝트에 배치되고 별도의 .NET Core 콘솔 응용 프로그램이 명령을 실행하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-111">For this reason, the application's data model is placed in a shared library project, and a separate .NET Core console application is used to run the commands.</span></span>

## <a name="create-a-library-project-to-hold-the-data-model"></a><span data-ttu-id="822a0-112">데이터 모델을 보관할 라이브러리 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="822a0-112">Create a library project to hold the data model</span></span>

* <span data-ttu-id="822a0-113">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-113">Open Visual Studio</span></span>

* <span data-ttu-id="822a0-114">**파일 > 새로 만들기 > 프로젝트**</span><span class="sxs-lookup"><span data-stu-id="822a0-114">**File > New > Project**</span></span>

* <span data-ttu-id="822a0-115">왼쪽 메뉴에서 **설치됨 > Visual C# > .NET Standard**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-115">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="822a0-116">**클래스 라이브러리(.NET Standard)** 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-116">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="822a0-117">프로젝트 이름을 *Blogging.Model*로 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-117">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="822a0-118">솔루션의 이름을 *Blogging*으로 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-118">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="822a0-119">**확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-119">Click **OK**.</span></span>

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a><span data-ttu-id="822a0-120">데이터 모델 프로젝트에서 Entity Framework Core 런타임 설치</span><span class="sxs-lookup"><span data-stu-id="822a0-120">Install Entity Framework Core runtime in the data model project</span></span>

<span data-ttu-id="822a0-121">EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-121">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="822a0-122">이 자습서에서는 SQLite를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-122">This tutorial uses SQLite.</span></span> <span data-ttu-id="822a0-123">사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="822a0-123">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="822a0-124">**도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**.</span><span class="sxs-lookup"><span data-stu-id="822a0-124">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="822a0-125">라이브러리 프로젝트 *Blogging.Model*이 패키지 관리자 콘솔에서 **기본 프로젝트**로 선택되었는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-125">Make sure that the library project *Blogging.Model* is selected as the **Default Project** in the Package Manager Console.</span></span>

* <span data-ttu-id="822a0-126">`Install-Package Microsoft.EntityFrameworkCore.Sqlite` 실행</span><span class="sxs-lookup"><span data-stu-id="822a0-126">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="822a0-127">데이터 모델 만들기</span><span class="sxs-lookup"><span data-stu-id="822a0-127">Create the data model</span></span>

<span data-ttu-id="822a0-128">이제 모델을 구성하는 *DbContext* 및 엔터티 클래스를 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-128">Now it's time to define the *DbContext* and entity classes that make up the model.</span></span>

* <span data-ttu-id="822a0-129">*Class1.cs*를 삭제합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="822a0-130">다음 코드로 *Model.cs*를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a><span data-ttu-id="822a0-131">마이그레이션 명령을 실행할 새 콘솔 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="822a0-131">Create a new console project to run migrations commands</span></span>

* <span data-ttu-id="822a0-132">**솔루션 탐색기**에서 솔루션을 마우스 오른쪽 단추로 클릭한 다음, **추가 > 새 프로젝트**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="822a0-133">왼쪽 메뉴에서 **설치됨 Visual C# > .NET Core**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-133">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="822a0-134">**콘솔 앱(.NET Core)** 프로젝트 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-134">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="822a0-135">*Blogging.Migrations.Startup* 프로젝트 이름을 지정하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-135">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="822a0-136">*Blogging.Migrations.Startup* 프로젝트의 프로젝트 참조를 *Blogging.Model* 프로젝트에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-136">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a><span data-ttu-id="822a0-137">마이그레이션 시작 프로젝트에 Entity Framework Core 도구 설치</span><span class="sxs-lookup"><span data-stu-id="822a0-137">Install Entity Framework Core tools in the migrations startup project</span></span>

<span data-ttu-id="822a0-138">패키지 관리자 콘솔에서 EF Core 마이그레이션 명령을 사용하려면 콘솔 응용 프로그램에서 EF Core 도구 패키지를 설치하세요.</span><span class="sxs-lookup"><span data-stu-id="822a0-138">To enable the EF Core migration commands in the Package Manager Console, install the EF Core tools package in the console application.</span></span>

* <span data-ttu-id="822a0-139">**도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="822a0-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="822a0-140">`Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup` 실행</span><span class="sxs-lookup"><span data-stu-id="822a0-140">Run `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="822a0-141">초기 마이그레이션 만들기</span><span class="sxs-lookup"><span data-stu-id="822a0-141">Create the initial migration</span></span>

 <span data-ttu-id="822a0-142">시작 프로젝트로 콘솔 응용 프로그램을 지정하여 초기 마이그레이션을 만드세요.</span><span class="sxs-lookup"><span data-stu-id="822a0-142">Create the initial migration, specifying the console application as the startup project.</span></span>

* <span data-ttu-id="822a0-143">`Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup` 실행</span><span class="sxs-lookup"><span data-stu-id="822a0-143">Run `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span></span>

<span data-ttu-id="822a0-144">이 명령은 데이터 모델에 대한 초기 데이터베이스 테이블 집합을 만드는 마이그레이션을 스캐폴딩합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-144">This command scaffolds a migration that creates the initial set of database tables for your data model.</span></span>

## <a name="create-the-uwp-project"></a><span data-ttu-id="822a0-145">UWP 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="822a0-145">Create the UWP project</span></span>

* <span data-ttu-id="822a0-146">**솔루션 탐색기**에서 솔루션을 마우스 오른쪽 단추로 클릭한 다음, **추가 > 새 프로젝트**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-146">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="822a0-147">왼쪽 메뉴에서 **설치됨 > Visual C# > Windows 유니버설**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-147">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="822a0-148">**빈 앱(유니버설 Windows)** 프로젝트 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-148">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="822a0-149">*Blogging.UWP* 프로젝트 이름을 지정하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-149">Name the project *Blogging.UWP*, and click **OK**</span></span>

> [!IMPORTANT]
> <span data-ttu-id="822a0-150">대상 및 최소 버전을 **Windows 10 Fall Creators Update(10.0; 빌드 16299.0)** 이상으로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-150">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>
> <span data-ttu-id="822a0-151">이전 버전의 Windows 10에서는 Entity Framework Core에 필요한 .NET Standard 2.0을 지원하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-151">Previous  versions of Windows 10 do not support .NET Standard 2.0, which is required by Entity Framework Core.</span></span>

## <a name="add-code-to-create-the-database-on-application-startup"></a><span data-ttu-id="822a0-152">응용 프로그램 시작 시 데이터베이스를 만드는 코드 추가</span><span class="sxs-lookup"><span data-stu-id="822a0-152">Add code to create the database on application startup</span></span>

<span data-ttu-id="822a0-153">앱이 실행되는 장치에서 데이터베이스를 만들려면 응용 프로그램 시작 시 로컬 데이터베이스에 보류 중인 마이그레이션을 적용할 코드를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-153">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="822a0-154">이렇게 하면 앱이 처음 실행될 때 로컬 데이터베이스가 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-154">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="822a0-155">*Blogging.UWP* 프로젝트의 프로젝트 참조를 *Blogging.Model* 프로젝트에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-155">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="822a0-156">*App.xaml.cs*를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-156">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="822a0-157">강조 표시된 코드를 추가하여 보류 중인 마이그레이션을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-157">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="822a0-158">모델을 변경하는 경우 `Add-Migration` 명령을 사용하여 새 마이그레이션을 스캐폴딩하고 데이터베이스에 해당 변경 내용을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-158">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="822a0-159">보류 중인 마이그레이션은 응용 프로그램이 시작될 때 각 장치의 로컬 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-159">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="822a0-160">EF Core는 데이터베이스의 `__EFMigrationsHistory` 테이블을 사용하여 이미 데이터베이스에 적용된 마이그레이션을 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-160">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-data-model"></a><span data-ttu-id="822a0-161">데이터 모델 사용</span><span class="sxs-lookup"><span data-stu-id="822a0-161">Use the data model</span></span>

<span data-ttu-id="822a0-162">이제 EF Core를 사용하여 데이터 액세스를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-162">You can now use EF Core to perform data access.</span></span>

* <span data-ttu-id="822a0-163">*MainPage.xaml*을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-163">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="822a0-164">아래에 강조 표시된 페이지 로드 처리기 및 UI 콘텐츠를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-164">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="822a0-165">이제 코드를 추가하여 데이터베이스와 UI 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-165">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="822a0-166">*MainPage.xaml.cs*를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-166">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="822a0-167">다음 목록에서 강조 표시된 코드를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-167">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="822a0-168">이제 응용 프로그램을 실행하여 작동하는지 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-168">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="822a0-169">**솔루션 탐색기**에서 *Blogging.UWP* 프로젝트를 마우스 오른쪽 단추로 클릭한 다음, **배포**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-169">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="822a0-170">*Blogging.UWP*를 시작 프로젝트로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-170">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="822a0-171">**디버그 > 디버깅하지 않고 시작**</span><span class="sxs-lookup"><span data-stu-id="822a0-171">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="822a0-172">앱을 빌드하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-172">The app builds and runs.</span></span>

* <span data-ttu-id="822a0-173">URL을 입력하고 **추가** 단추를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-173">Enter a URL and click the **Add** button</span></span>

  ![이미지](_static/create.png)

  ![이미지](_static/list.png)

  <span data-ttu-id="822a0-176">짜잔!</span><span class="sxs-lookup"><span data-stu-id="822a0-176">Tada!</span></span> <span data-ttu-id="822a0-177">이제 Entity Framework Core를 실행하는 간단한 UWP 앱이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="822a0-177">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="822a0-178">다음 단계</span><span class="sxs-lookup"><span data-stu-id="822a0-178">Next steps</span></span>

<span data-ttu-id="822a0-179">UWP와 함께 EF Core를 사용할 때 알아야 할 호환성 및 성능 정보는 [EF Core에서 지원하는 .NET 구현](../../platforms/index.md#universal-windows-platform)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="822a0-179">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="822a0-180">Entity Framework Core 기능에 대해 자세히 알아보려면 이 설명서의 다른 문서를 확인하세요.</span><span class="sxs-lookup"><span data-stu-id="822a0-180">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
