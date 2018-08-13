---
title: .NET Framework에서 시작 - 기존 데이터베이스 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: d5c548927b736199c7d6fddc9c74139ca5f6614e
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614417"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="6cd5b-102">.NET Framework에서 기존 데이터베이스로 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="6cd5b-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="6cd5b-103">이 자습서에서는 Entity Framework를 사용하여 Microsoft SQL Server 데이터베이스에 대해 기본 데이터 액세스를 수행하는 콘솔 응용 프로그램을 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="6cd5b-104">기존 데이터베이스를 리버스 엔지니어링하여 Entity Framework 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-104">You create an Entity Framework model by reverse engineering an existing database.</span></span>

<span data-ttu-id="6cd5b-105">[GitHub에서 이 아티클의 샘플을 봅니다](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="6cd5b-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6cd5b-106">전제 조건</span><span class="sxs-lookup"><span data-stu-id="6cd5b-106">Prerequisites</span></span>

* [<span data-ttu-id="6cd5b-107">Visual Studio 2017 버전 15.7 이상</span><span class="sxs-lookup"><span data-stu-id="6cd5b-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a><span data-ttu-id="6cd5b-108">Blogging 데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="6cd5b-108">Create Blogging database</span></span>

<span data-ttu-id="6cd5b-109">이 자습서에서는 LocalDb 인스턴스의 **Blogging** 데이터베이스를 기존 데이터베이스로 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-109">This tutorial uses a **Blogging** database on the LocalDb instance as the existing database.</span></span> <span data-ttu-id="6cd5b-110">**이미 다른 자습서의 일부로 Blogging** 데이터베이스를 만든 경우 다음 단계를 건너뜁니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-110">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="6cd5b-111">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-111">Open Visual Studio</span></span>

* <span data-ttu-id="6cd5b-112">**도구 > 데이터베이스에 연결...**</span><span class="sxs-lookup"><span data-stu-id="6cd5b-112">**Tools > Connect to Database...**</span></span>

* <span data-ttu-id="6cd5b-113">**Microsoft SQL Server**를 선택하고 **계속**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-113">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="6cd5b-114">**서버 이름**으로 **(localdb)\mssqllocaldb**를 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-114">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="6cd5b-115">**데이터베이스 이름**으로 **master**를 입력하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-115">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="6cd5b-116">이제 master 데이터베이스가 **서버 탐색기**의 **데이터 연결**에 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-116">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="6cd5b-117">**서버 탐색기**에서 데이터베이스를 마우스 오른쪽 단추로 클릭하고 **새 쿼리**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-117">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="6cd5b-118">아래 나열된 스크립트를 쿼리 편집기로 복사합니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-118">Copy the script listed below into the query editor</span></span>

* <span data-ttu-id="6cd5b-119">쿼리 편집기를 마우스 오른쪽 단추로 클릭하고 **실행**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-119">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="6cd5b-120">새 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="6cd5b-120">Create a new project</span></span>

* <span data-ttu-id="6cd5b-121">Visual Studio 2017을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-121">Open Visual Studio 2017</span></span>

* <span data-ttu-id="6cd5b-122">**파일 > 새로 만들기 > 프로젝트...**</span><span class="sxs-lookup"><span data-stu-id="6cd5b-122">**File > New > Project...**</span></span>

* <span data-ttu-id="6cd5b-123">왼쪽 메뉴에서 **설치됨 > Visual C# > Windows Desktop**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-123">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="6cd5b-124">**콘솔 앱(.NET Framework)** 프로젝트 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-124">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="6cd5b-125">프로젝트가 **.NET Framework 4.6.1** 이상을 대상으로 지정하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-125">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="6cd5b-126">프로젝트 이름을 *ConsoleApp.ExistingDb*로 지정하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-126">Name the project *ConsoleApp.ExistingDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="6cd5b-127">Entity Framework 설치</span><span class="sxs-lookup"><span data-stu-id="6cd5b-127">Install Entity Framework</span></span>

<span data-ttu-id="6cd5b-128">EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-128">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="6cd5b-129">이 자습서에서는 SQL Server를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-129">This tutorial uses SQL Server.</span></span> <span data-ttu-id="6cd5b-130">사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-130">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="6cd5b-131">**도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="6cd5b-131">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="6cd5b-132">`Install-Package Microsoft.EntityFrameworkCore.SqlServer` 실행</span><span class="sxs-lookup"><span data-stu-id="6cd5b-132">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="6cd5b-133">다음 단계에서는 일부 Entity Framework Tools를 사용하여 데이터베이스를 리버스 엔지니어링합니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-133">In the next step, you use some Entity Framework Tools to reverse engineer the database.</span></span> <span data-ttu-id="6cd5b-134">따라서 도구 패키지도 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-134">So install the tools package as well.</span></span>

* <span data-ttu-id="6cd5b-135">`Install-Package Microsoft.EntityFrameworkCore.Tools` 실행</span><span class="sxs-lookup"><span data-stu-id="6cd5b-135">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-the-model"></a><span data-ttu-id="6cd5b-136">모델 리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="6cd5b-136">Reverse engineer the model</span></span>

<span data-ttu-id="6cd5b-137">이제 기존 데이터베이스를 기반으로 EF 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-137">Now it's time to create the EF model based on an existing database.</span></span>

* <span data-ttu-id="6cd5b-138">**도구 -> NuGet 패키지 관리자 -> 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="6cd5b-138">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>

* <span data-ttu-id="6cd5b-139">다음 명령을 실행하여 기존 데이터베이스에서 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-139">Run the following command to create a model from the existing database</span></span>

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> <span data-ttu-id="6cd5b-140">위의 명령에 `-Tables` 인수를 추가하여 엔터티를 생성할 테이블을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-140">You can specify the tables to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="6cd5b-141">예를 들어, `-Tables Blog,Post`을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-141">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="6cd5b-142">리버스 엔지니어링 프로세스에서는 기존 데이터베이스의 스키마를 기반으로 엔터티 클래스(`Blog` 및 `Post`) 및 파생 컨텍스트(`BloggingContext`)를 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-142">The reverse engineer process created entity classes (`Blog` and `Post`) and a derived context (`BloggingContext`) based on the schema of the existing database.</span></span>

<span data-ttu-id="6cd5b-143">엔터티 클래스는 쿼리하고 저장할 데이터를 나타내는 단순 C# 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-143">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="6cd5b-144">`Blog` 및 `Post` 엔터티 클래스는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-144">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> <span data-ttu-id="6cd5b-145">지연 로드를 사용하도록 설정하려면 탐색 속성을 `virtual`로 만들 수 있습니다(Blog.Post 및 Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="6cd5b-145">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

<span data-ttu-id="6cd5b-146">컨텍스트는 데이터베이스를 포함한 세션을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-146">The context represents a session with the database.</span></span> <span data-ttu-id="6cd5b-147">엔터티 클래스의 인스턴스를 쿼리하고 저장하는 데 사용할 수 있는 메서드가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-147">It has methods that you can use to query and save instances of the entity classes.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a><span data-ttu-id="6cd5b-148">모델 사용</span><span class="sxs-lookup"><span data-stu-id="6cd5b-148">Use the model</span></span>

<span data-ttu-id="6cd5b-149">이제 모델을 사용하여 데이터 액세스를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-149">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="6cd5b-150">*Program.cs*를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-150">Open *Program.cs*</span></span>

* <span data-ttu-id="6cd5b-151">파일의 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-151">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* <span data-ttu-id="6cd5b-152">디버그 > 디버깅하지 않고 시작</span><span class="sxs-lookup"><span data-stu-id="6cd5b-152">Debug > Start Without Debugging</span></span>

  <span data-ttu-id="6cd5b-153">하나의 블로그가 데이터베이스에 저장되고 모든 블로그의 세부 정보가 콘솔에 출력되는 것을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-153">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![이미지](_static/output-existing-db.png)

## <a name="additional-resources"></a><span data-ttu-id="6cd5b-155">추가 리소스</span><span class="sxs-lookup"><span data-stu-id="6cd5b-155">Additional Resources</span></span>

* [<span data-ttu-id="6cd5b-156">새 데이터베이스를 포함한 .NET Framework의 EF Core</span><span class="sxs-lookup"><span data-stu-id="6cd5b-156">EF Core on .NET Framework with a new database</span></span>](xref:core/get-started/full-dotnet/new-db)
* <span data-ttu-id="6cd5b-157">[새 데이터베이스를 포함한 .NET Core의 EF Core - SQLite](xref:core/get-started/netcore/new-db-sqlite) - 플랫폼 간 콘솔 EF 자습서.</span><span class="sxs-lookup"><span data-stu-id="6cd5b-157">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
