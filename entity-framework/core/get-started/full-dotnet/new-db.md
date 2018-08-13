---
title: .NET Framework에서 시작 - 새 데이터베이스 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 088ac915041489242eb8090e7bf3a2bdc8036534
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614430"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="c7f1f-102">.NET Framework에서 새 데이터베이스로 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="c7f1f-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="c7f1f-103">이 자습서에서는 Entity Framework를 사용하여 Microsoft SQL Server 데이터베이스에 대해 기본 데이터 액세스를 수행하는 콘솔 응용 프로그램을 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="c7f1f-104">마이그레이션을 사용하여 모델에서 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-104">You use migrations to create the database from a model.</span></span>

<span data-ttu-id="c7f1f-105">[GitHub에서 이 아티클의 샘플을 봅니다](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span><span class="sxs-lookup"><span data-stu-id="c7f1f-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7f1f-106">전제 조건</span><span class="sxs-lookup"><span data-stu-id="c7f1f-106">Prerequisites</span></span>

* [<span data-ttu-id="c7f1f-107">Visual Studio 2017 버전 15.7 이상</span><span class="sxs-lookup"><span data-stu-id="c7f1f-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a><span data-ttu-id="c7f1f-108">새 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="c7f1f-108">Create a new project</span></span>

* <span data-ttu-id="c7f1f-109">Visual Studio 2017을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-109">Open Visual Studio 2017</span></span>

* <span data-ttu-id="c7f1f-110">**파일 > 새로 만들기 > 프로젝트...**</span><span class="sxs-lookup"><span data-stu-id="c7f1f-110">**File > New > Project...**</span></span>

* <span data-ttu-id="c7f1f-111">왼쪽 메뉴에서 **설치됨 > Visual C# > Windows Desktop**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-111">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="c7f1f-112">**콘솔 앱(.NET Framework)** 프로젝트 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-112">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="c7f1f-113">프로젝트가 **.NET Framework 4.6.1** 이상을 대상으로 지정하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-113">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="c7f1f-114">프로젝트 이름을 *ConsoleApp.NewDb*로 지정하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-114">Name the project *ConsoleApp.NewDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="c7f1f-115">Entity Framework 설치</span><span class="sxs-lookup"><span data-stu-id="c7f1f-115">Install Entity Framework</span></span>

<span data-ttu-id="c7f1f-116">EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="c7f1f-117">이 자습서에서는 SQL Server를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-117">This tutorial uses SQL Server.</span></span> <span data-ttu-id="c7f1f-118">사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="c7f1f-119">도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔</span><span class="sxs-lookup"><span data-stu-id="c7f1f-119">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="c7f1f-120">`Install-Package Microsoft.EntityFrameworkCore.SqlServer` 실행</span><span class="sxs-lookup"><span data-stu-id="c7f1f-120">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="c7f1f-121">이 자습서의 뒷부분에서는 일부 Entity Framework Tools를 사용하여 데이터베이스를 유지 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-121">Later in this tutorial you use some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="c7f1f-122">따라서 도구 패키지도 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-122">So install the tools package as well.</span></span>

* <span data-ttu-id="c7f1f-123">`Install-Package Microsoft.EntityFrameworkCore.Tools` 실행</span><span class="sxs-lookup"><span data-stu-id="c7f1f-123">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="c7f1f-124">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="c7f1f-124">Create the model</span></span>

<span data-ttu-id="c7f1f-125">이제 모델을 구성하는 컨텍스트 및 엔터티 클래스를 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-125">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="c7f1f-126">**프로젝트 > 클래스 추가...**</span><span class="sxs-lookup"><span data-stu-id="c7f1f-126">**Project > Add Class...**</span></span>

* <span data-ttu-id="c7f1f-127">이름으로 *Model.cs*를 입력하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-127">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="c7f1f-128">파일의 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-128">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> <span data-ttu-id="c7f1f-129">실제 응용 프로그램에서는 각 클래스를 별도의 파일에 저장하고 구성 파일 또는 환경 변수에 연결 문자열을 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-129">In a real application you would put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="c7f1f-130">편의상 모든 항목은 이 자습서의 단일 코드 파일에 위치합니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-130">For the sake of simplicity, everything is in a single code file for this tutorial.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="c7f1f-131">데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="c7f1f-131">Create the database</span></span>

<span data-ttu-id="c7f1f-132">이제 모델이 있으므로 마이그레이션을 사용하여 데이터베이스를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-132">Now that you have a model, you can use migrations to create a database.</span></span>

* <span data-ttu-id="c7f1f-133">**도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="c7f1f-133">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="c7f1f-134">`Add-Migration InitialCreate`를 실행하여 마이그레이션을 스캐폴딩하고 모델에 대한 초기 테이블 집합을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-134">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for the model.</span></span>

* <span data-ttu-id="c7f1f-135">`Update-Database`를 실행하여 새 마이그레이션을 데이터베이스에 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-135">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="c7f1f-136">데이터베이스가 아직 없기 때문에 마이그레이션이 적용되기 전에 데이터베이스가 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-136">Because the database doesn't exist yet, it will be created before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="c7f1f-137">모델을 변경할 경우 `Add-Migration` 명령을 사용하여 새 마이그레이션을 스캐폴딩하고 데이터베이스에서 해당 스키마를 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-137">If you make changes to the model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="c7f1f-138">스캐폴딩된 코드를 확인하고 필요한 내용을 변경한 후 `Update-Database` 명령을 사용하여 변경 내용을 데이터베이스에 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-138">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
> <span data-ttu-id="c7f1f-139">EF는 데이터베이스의 `__EFMigrationsHistory` 테이블을 사용하여 이미 데이터베이스에 적용된 마이그레이션을 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-139">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="c7f1f-140">모델 사용</span><span class="sxs-lookup"><span data-stu-id="c7f1f-140">Use the model</span></span>

<span data-ttu-id="c7f1f-141">이제 모델을 사용하여 데이터 액세스를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-141">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="c7f1f-142">*Program.cs*를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-142">Open *Program.cs*</span></span>

* <span data-ttu-id="c7f1f-143">파일의 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-143">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* <span data-ttu-id="c7f1f-144">**디버그 > 디버깅하지 않고 시작**</span><span class="sxs-lookup"><span data-stu-id="c7f1f-144">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="c7f1f-145">하나의 블로그가 데이터베이스에 저장되고 모든 블로그의 세부 정보가 콘솔에 출력되는 것을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-145">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![이미지](_static/output-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="c7f1f-147">추가 리소스</span><span class="sxs-lookup"><span data-stu-id="c7f1f-147">Additional Resources</span></span>

* [<span data-ttu-id="c7f1f-148">기존 데이터베이스를 포함한 .NET Framework의 EF Core</span><span class="sxs-lookup"><span data-stu-id="c7f1f-148">EF Core on .NET Framework with an existing database</span></span>](xref:core/get-started/full-dotnet/existing-db)
* <span data-ttu-id="c7f1f-149">[새 데이터베이스를 포함한 .NET Core의 EF Core - SQLite](xref:core/get-started/netcore/new-db-sqlite) - 플랫폼 간 콘솔 EF 자습서.</span><span class="sxs-lookup"><span data-stu-id="c7f1f-149">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
