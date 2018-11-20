---
title: .NET Framework에서 시작 - 기존 데이터베이스 - EF Core
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 1b90c491a3b2025da750a3266ff45d9d92bb1d0d
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688617"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="ef017-102">.NET Framework에서 기존 데이터베이스로 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="ef017-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="ef017-103">이 자습서에서는 Entity Framework를 사용하여 Microsoft SQL Server 데이터베이스에 대해 기본 데이터 액세스를 수행하는 콘솔 응용 프로그램을 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="ef017-104">기존 데이터베이스를 리버스 엔지니어링하여 Entity Framework 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-104">You create an Entity Framework model by reverse engineering an existing database.</span></span>

<span data-ttu-id="ef017-105">[GitHub에서 이 아티클의 샘플을 봅니다](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="ef017-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef017-106">전제 조건</span><span class="sxs-lookup"><span data-stu-id="ef017-106">Prerequisites</span></span>

* [<span data-ttu-id="ef017-107">Visual Studio 2017 버전 15.7 이상</span><span class="sxs-lookup"><span data-stu-id="ef017-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a><span data-ttu-id="ef017-108">Blogging 데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="ef017-108">Create Blogging database</span></span>

<span data-ttu-id="ef017-109">이 자습서에서는 LocalDb 인스턴스의 **Blogging** 데이터베이스를 기존 데이터베이스로 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-109">This tutorial uses a **Blogging** database on the LocalDb instance as the existing database.</span></span> <span data-ttu-id="ef017-110">**이미 다른 자습서의 일부로 Blogging** 데이터베이스를 만든 경우 다음 단계를 건너뜁니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-110">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="ef017-111">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-111">Open Visual Studio</span></span>

* <span data-ttu-id="ef017-112">**도구 > 데이터베이스에 연결...**</span><span class="sxs-lookup"><span data-stu-id="ef017-112">**Tools > Connect to Database...**</span></span>

* <span data-ttu-id="ef017-113">**Microsoft SQL Server**를 선택하고 **계속**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-113">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="ef017-114">**서버 이름**으로 **(localdb)\mssqllocaldb**를 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-114">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="ef017-115">**데이터베이스 이름**으로 **master**를 입력하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-115">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="ef017-116">이제 master 데이터베이스가 **서버 탐색기**의 **데이터 연결**에 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-116">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="ef017-117">**서버 탐색기**에서 데이터베이스를 마우스 오른쪽 단추로 클릭하고 **새 쿼리**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-117">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="ef017-118">아래 나열된 스크립트를 쿼리 편집기로 복사합니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-118">Copy the script listed below into the query editor</span></span>

* <span data-ttu-id="ef017-119">쿼리 편집기를 마우스 오른쪽 단추로 클릭하고 **실행**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-119">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="ef017-120">새 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="ef017-120">Create a new project</span></span>

* <span data-ttu-id="ef017-121">Visual Studio 2017을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-121">Open Visual Studio 2017</span></span>

* <span data-ttu-id="ef017-122">**파일 > 새로 만들기 > 프로젝트...**</span><span class="sxs-lookup"><span data-stu-id="ef017-122">**File > New > Project...**</span></span>

* <span data-ttu-id="ef017-123">왼쪽 메뉴에서 **설치됨 > Visual C# > Windows Desktop**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-123">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="ef017-124">**콘솔 앱(.NET Framework)** 프로젝트 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-124">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="ef017-125">프로젝트가 **.NET Framework 4.6.1** 이상을 대상으로 지정하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-125">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="ef017-126">프로젝트 이름을 *ConsoleApp.ExistingDb*로 지정하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-126">Name the project *ConsoleApp.ExistingDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="ef017-127">Entity Framework 설치</span><span class="sxs-lookup"><span data-stu-id="ef017-127">Install Entity Framework</span></span>

<span data-ttu-id="ef017-128">EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-128">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="ef017-129">이 자습서에서는 SQL Server를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-129">This tutorial uses SQL Server.</span></span> <span data-ttu-id="ef017-130">사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ef017-130">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="ef017-131">**도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="ef017-131">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="ef017-132">`Install-Package Microsoft.EntityFrameworkCore.SqlServer` 실행</span><span class="sxs-lookup"><span data-stu-id="ef017-132">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="ef017-133">다음 단계에서는 일부 Entity Framework Tools를 사용하여 데이터베이스를 리버스 엔지니어링합니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-133">In the next step, you use some Entity Framework Tools to reverse engineer the database.</span></span> <span data-ttu-id="ef017-134">따라서 도구 패키지도 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-134">So install the tools package as well.</span></span>

* <span data-ttu-id="ef017-135">`Install-Package Microsoft.EntityFrameworkCore.Tools` 실행</span><span class="sxs-lookup"><span data-stu-id="ef017-135">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-the-model"></a><span data-ttu-id="ef017-136">모델 리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="ef017-136">Reverse engineer the model</span></span>

<span data-ttu-id="ef017-137">이제 기존 데이터베이스를 기반으로 EF 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-137">Now it's time to create the EF model based on an existing database.</span></span>

* <span data-ttu-id="ef017-138">**도구 -> NuGet 패키지 관리자 -> 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="ef017-138">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>

* <span data-ttu-id="ef017-139">다음 명령을 실행하여 기존 데이터베이스에서 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-139">Run the following command to create a model from the existing database</span></span>

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> <span data-ttu-id="ef017-140">위의 명령에 `-Tables` 인수를 추가하여 엔터티를 생성할 테이블을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-140">You can specify the tables to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="ef017-141">예를 들어, `-Tables Blog,Post`을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-141">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="ef017-142">리버스 엔지니어링 프로세스에서는 기존 데이터베이스의 스키마를 기반으로 엔터티 클래스(`Blog` 및 `Post`) 및 파생 컨텍스트(`BloggingContext`)를 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-142">The reverse engineer process created entity classes (`Blog` and `Post`) and a derived context (`BloggingContext`) based on the schema of the existing database.</span></span>

<span data-ttu-id="ef017-143">엔터티 클래스는 쿼리하고 저장할 데이터를 나타내는 단순 C# 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-143">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="ef017-144">`Blog` 및 `Post` 엔터티 클래스는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-144">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> <span data-ttu-id="ef017-145">지연 로드를 사용하도록 설정하려면 탐색 속성을 `virtual`로 만들 수 있습니다(Blog.Post 및 Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="ef017-145">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

<span data-ttu-id="ef017-146">컨텍스트는 데이터베이스를 포함한 세션을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-146">The context represents a session with the database.</span></span> <span data-ttu-id="ef017-147">엔터티 클래스의 인스턴스를 쿼리하고 저장하는 데 사용할 수 있는 메서드가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-147">It has methods that you can use to query and save instances of the entity classes.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a><span data-ttu-id="ef017-148">모델 사용</span><span class="sxs-lookup"><span data-stu-id="ef017-148">Use the model</span></span>

<span data-ttu-id="ef017-149">이제 모델을 사용하여 데이터 액세스를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-149">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="ef017-150">*Program.cs*를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-150">Open *Program.cs*</span></span>

* <span data-ttu-id="ef017-151">파일의 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-151">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* <span data-ttu-id="ef017-152">디버그 > 디버깅하지 않고 시작</span><span class="sxs-lookup"><span data-stu-id="ef017-152">Debug > Start Without Debugging</span></span>

  <span data-ttu-id="ef017-153">하나의 블로그가 데이터베이스에 저장되고 모든 블로그의 세부 정보가 콘솔에 출력되는 것을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ef017-153">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![이미지](_static/output-existing-db.png)

## <a name="next-steps"></a><span data-ttu-id="ef017-155">다음 단계</span><span class="sxs-lookup"><span data-stu-id="ef017-155">Next steps</span></span>

<span data-ttu-id="ef017-156">컨텍스트 및 엔터티 클래스를 스캐폴드하는 방법에 대한 자세한 내용은 다음 문서를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ef017-156">For more information about how to scaffold a context and entity classes, see the following articles:</span></span>
* [<span data-ttu-id="ef017-157">리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="ef017-157">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
* [<span data-ttu-id="ef017-158">Entity Framework Core 도구 참조 - .NET CLI</span><span class="sxs-lookup"><span data-stu-id="ef017-158">Entity Framework Core tools reference - .NET CLI</span></span>](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [<span data-ttu-id="ef017-159">Entity Framework Core 도구 참조 - 패키지 관리자 콘솔</span><span class="sxs-lookup"><span data-stu-id="ef017-159">Entity Framework Core tools reference - Package Manager Console</span></span>](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)