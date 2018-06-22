---
title: UWP에서 시작 - 새 데이터베이스 - EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/01/2017
ms.locfileid: "26049836"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="34f8d-102">UWP(유니버설 Windows 플랫폼)에서 새 데이터베이스로 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="34f8d-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

> [!NOTE]
> <span data-ttu-id="34f8d-103">이 자습서에서는 EF Core 2.0.1(ASP.NET Core 및 .NET Core SDK 2.0.3과 함께 릴리스됨)을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-103">This tutorial uses EF Core 2.0.1 (released alongside ASP.NET Core and .NET Core SDK 2.0.3).</span></span> <span data-ttu-id="34f8d-104">EF Core 2.0.0에는 UWP 환경을 조성하는 데 필요한 일부 중요한 버그 수정이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-104">EF Core 2.0.0 lacks some crucial bug fixes required for a good UWP experience.</span></span>

<span data-ttu-id="34f8d-105">이 연습에서는 Entity Framework를 사용하여 로컬 SQLite 데이터베이스에 대해 기본 데이터 액세스를 수행하는 UWP(유니버설 Windows 플랫폼) 응용 프로그램을 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-105">In this walkthrough, you will build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="34f8d-106">**UWP의 LINQ 쿼리에서는 익명 형식을 사용하지 않는 것이 좋습니다**.</span><span class="sxs-lookup"><span data-stu-id="34f8d-106">**Consider avoiding anonymous types in LINQ queries on UWP**.</span></span> <span data-ttu-id="34f8d-107">UWP 응용 프로그램을 앱 스토어에 배포하려면 .NET 네이티브로 응용 프로그램을 컴파일해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-107">Deploying a UWP application to the app store requires your application to be compiled with .NET Native.</span></span> <span data-ttu-id="34f8d-108">익명 형식의 쿼리는 .NET 네이티브에서 성능이 저하됩니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-108">Queries with anonymous types have worse performance on .NET Native.</span></span>

> [!TIP]
> <span data-ttu-id="34f8d-109">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="34f8d-110">필수 구성 요소</span><span class="sxs-lookup"><span data-stu-id="34f8d-110">Prerequisites</span></span>

<span data-ttu-id="34f8d-111">이 연습을 완료하려면 다음 항목이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-111">The following items are required to complete this walkthrough:</span></span>

* <span data-ttu-id="34f8d-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10)(10.0.16299.0)</span><span class="sxs-lookup"><span data-stu-id="34f8d-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span></span>

* <span data-ttu-id="34f8d-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 이상.</span><span class="sxs-lookup"><span data-stu-id="34f8d-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

* <span data-ttu-id="34f8d-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 버전 15.4 이상(**유니버설 Windows 플랫폼 개발** 워크로드 포함).</span><span class="sxs-lookup"><span data-stu-id="34f8d-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.4 or later with the **Universal Windows Platform Development** workload.</span></span>

## <a name="create-a-new-model-project"></a><span data-ttu-id="34f8d-115">새 모델 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="34f8d-115">Create a new model project</span></span>

> [!WARNING]
> <span data-ttu-id="34f8d-116">.NET Core 도구가 UWP 프로젝트와 상호 작용하는 방법의 제한 사항으로 인해 패키지 관리자 콘솔에서 마이그레이션 명령을 실행할 수 있으려면 모델을 UWP 이외 프로젝트에 배치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-116">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the Package Manager Console</span></span>

* <span data-ttu-id="34f8d-117">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-117">Open Visual Studio</span></span>

* <span data-ttu-id="34f8d-118">파일 > 새로 만들기 > 프로젝트...</span><span class="sxs-lookup"><span data-stu-id="34f8d-118">File > New > Project...</span></span>

* <span data-ttu-id="34f8d-119">왼쪽 메뉴에서 [템플릿] > [Visual C#]을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-119">From the left menu select Templates > Visual C#</span></span>

* <span data-ttu-id="34f8d-120">**클래스 라이브러리(.NET Standard)** 프로젝트 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-120">Select the **Class Library (.NET Standard)** project template</span></span>

* <span data-ttu-id="34f8d-121">프로젝트에 이름을 지정하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-121">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="34f8d-122">Entity Framework 설치</span><span class="sxs-lookup"><span data-stu-id="34f8d-122">Install Entity Framework</span></span>

<span data-ttu-id="34f8d-123">EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-123">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="34f8d-124">이 연습에서는 SQLite를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-124">This walkthrough uses SQLite.</span></span> <span data-ttu-id="34f8d-125">사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="34f8d-125">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="34f8d-126">도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔</span><span class="sxs-lookup"><span data-stu-id="34f8d-126">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="34f8d-127">`Install-Package Microsoft.EntityFrameworkCore.Sqlite` 실행</span><span class="sxs-lookup"><span data-stu-id="34f8d-127">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="34f8d-128">이 연습의 뒷부분에서는 일부 Entity Framework Tools를 사용하여 데이터베이스를 유지 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-128">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="34f8d-129">따라서 도구 패키지도 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-129">So we will install the tools package as well.</span></span>

* <span data-ttu-id="34f8d-130">`Install-Package Microsoft.EntityFrameworkCore.Tools` 실행</span><span class="sxs-lookup"><span data-stu-id="34f8d-130">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

* <span data-ttu-id="34f8d-131">.csproj 파일을 편집하고 `<TargetFramework>netstandard2.0</TargetFramework>`를 `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-131">Edit the .csproj file and replace `<TargetFramework>netstandard2.0</TargetFramework>` with `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="34f8d-132">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="34f8d-132">Create your model</span></span>

<span data-ttu-id="34f8d-133">이제 모델을 구성하는 컨텍스트 및 엔터티 클래스를 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-133">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="34f8d-134">프로젝트 > 클래스 추가...</span><span class="sxs-lookup"><span data-stu-id="34f8d-134">Project > Add Class...</span></span>

* <span data-ttu-id="34f8d-135">이름으로 *Model.cs*를 입력하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-135">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="34f8d-136">파일의 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-136">Replace the contents of the file with the following code</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="34f8d-137">새 UWP 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="34f8d-137">Create a new UWP project</span></span>

* <span data-ttu-id="34f8d-138">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-138">Open Visual Studio</span></span>

* <span data-ttu-id="34f8d-139">파일 > 새로 만들기 > 프로젝트...</span><span class="sxs-lookup"><span data-stu-id="34f8d-139">File > New > Project...</span></span>

* <span data-ttu-id="34f8d-140">왼쪽 메뉴에서 [템플릿] > [Visual C#] > [Windows 유니버설]을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-140">From the left menu select Templates > Visual C# > Windows Universal</span></span>

* <span data-ttu-id="34f8d-141">**빈 앱(유니버설 Windows)** 프로젝트 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-141">Select the **Blank App (Universal Windows)** project template</span></span>

* <span data-ttu-id="34f8d-142">프로젝트에 이름을 지정하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-142">Give the project a name and click **OK**</span></span>

* <span data-ttu-id="34f8d-143">대상 및 최소 버전을 `Windows 10 Fall Creators Update (10.0; build 16299.0)` 이상으로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-143">Set the target and minimum versions to at least `Windows 10 Fall Creators Update (10.0; build 16299.0)`</span></span>

## <a name="create-your-database"></a><span data-ttu-id="34f8d-144">데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="34f8d-144">Create your database</span></span>

<span data-ttu-id="34f8d-145">이제 모델이 있으므로 마이그레이션을 사용하여 데이터베이스를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-145">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="34f8d-146">도구 -> NuGet 패키지 관리자 -> 패키지 관리자 콘솔</span><span class="sxs-lookup"><span data-stu-id="34f8d-146">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="34f8d-147">모델 프로젝트를 기본 프로젝트로 선택하고 시작 프로젝트로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-147">Select the model project as the Default project and set it as the startup project</span></span>

* <span data-ttu-id="34f8d-148">`Add-Migration MyFirstMigration`를 실행하여 마이그레이션을 스캐폴딩하고 모델에 대한 초기 테이블 집합을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-148">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

<span data-ttu-id="34f8d-149">앱이 실행되는 장치에서 데이터베이스를 만들기 위해 일부 코드를 추가하여 응용 프로그램 시작 시 로컬 데이터베이스에 보류 중인 마이그레이션을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-149">Since we want the database to be created on the device that the app runs on, we will add some code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="34f8d-150">이렇게 하면 앱이 처음 실행될 때 로컬 데이터베이스가 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-150">The first time that the app runs, this will take care of creating the local database for us.</span></span>

* <span data-ttu-id="34f8d-151">**솔루션 탐색기**에서 **App.xaml**을 마우스 오른쪽 단추로 클릭하고 **코드 보기**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-151">Right-click on **App.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="34f8d-152">파일 시작 부분에 강조 표시된 using을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-152">Add the highlighted using to the start of the file</span></span>

* <span data-ttu-id="34f8d-153">강조 표시된 코드를 추가하여 보류 중인 마이그레이션을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-153">Add the highlighted code to apply any pending migrations</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> <span data-ttu-id="34f8d-154">나중에 모델을 변경할 경우 `Add-Migration` 명령을 사용하여 새 마이그레이션을 스캐폴딩하고 데이터베이스에 해당 변경 내용을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-154">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="34f8d-155">보류 중인 마이그레이션은 응용 프로그램이 시작될 때 각 장치의 로컬 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-155">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="34f8d-156">EF는 데이터베이스의 `__EFMigrationsHistory` 테이블을 사용하여 이미 데이터베이스에 적용된 마이그레이션을 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-156">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="34f8d-157">모델 사용</span><span class="sxs-lookup"><span data-stu-id="34f8d-157">Use your model</span></span>

<span data-ttu-id="34f8d-158">이제 모델을 사용하여 데이터 액세스를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-158">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="34f8d-159">*MainPage.xaml*을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-159">Open *MainPage.xaml*</span></span>

* <span data-ttu-id="34f8d-160">아래에 강조 표시된 페이지 로드 처리기 및 UI 콘텐츠를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-160">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="34f8d-161">이제 코드를 추가하여 데이터베이스와 UI를 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-161">Now we'll add code to wire up the UI with the database</span></span>

* <span data-ttu-id="34f8d-162">**솔루션 탐색기**에서 **MainPage.xaml**을 마우스 오른쪽 단추로 클릭하고 **코드 보기**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-162">Right-click **MainPage.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="34f8d-163">다음 목록에서 강조 표시된 코드를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-163">Add the highlighted code from the following listing</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

<span data-ttu-id="34f8d-164">이제 응용 프로그램을 실행하여 작동하는지 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-164">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="34f8d-165">디버그 > 디버깅하지 않고 시작</span><span class="sxs-lookup"><span data-stu-id="34f8d-165">Debug > Start Without Debugging</span></span>

* <span data-ttu-id="34f8d-166">응용 프로그램이 빌드되고 시작됩니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-166">The application will build and launch</span></span>

* <span data-ttu-id="34f8d-167">URL을 입력하고 **추가** 단추를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-167">Enter a URL and click the **Add** button</span></span>

![image](_static/create.png)

![image](_static/list.png)

## <a name="next-steps"></a><span data-ttu-id="34f8d-170">다음 단계</span><span class="sxs-lookup"><span data-stu-id="34f8d-170">Next steps</span></span>

> [!TIP]
> <span data-ttu-id="34f8d-171">엔터티 형식에서 [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx)를 구현하고 `ChangeTrackingStrategy.ChangingAndChangedNotifications`를 사용하여 `SaveChanges()` 성능을 개선할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-171">`SaveChanges()` performance can be improved by implementing [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types and using `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span></span>

<span data-ttu-id="34f8d-172">짜잔!</span><span class="sxs-lookup"><span data-stu-id="34f8d-172">Tada!</span></span> <span data-ttu-id="34f8d-173">이제 Entity Framework를 실행하는 간단한 UWP 앱이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34f8d-173">You now have a simple UWP app running Entity Framework.</span></span>

<span data-ttu-id="34f8d-174">Entity Framework의 기능에 대해 자세히 알아보려면 이 문서의 다른 문서를 확인하세요.</span><span class="sxs-lookup"><span data-stu-id="34f8d-174">Check out other articles in this documentation to learn more about Entity Framework's features.</span></span>
