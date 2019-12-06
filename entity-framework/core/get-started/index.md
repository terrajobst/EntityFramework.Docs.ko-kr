---
title: 시작 - EF Core
author: rick-anderson
ms.date: 09/17/2019
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: d46c4bb9ac6c8f718b4da5ecd82d54710d41935f
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824495"
---
# <a name="getting-started-with-ef-core"></a><span data-ttu-id="be61e-102">EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="be61e-102">Getting Started with EF Core</span></span>

<span data-ttu-id="be61e-103">이 자습서에서는 Entity Framework Core를 사용하여 SQLite 데이터베이스에 대한 데이터 액세스를 수행하는 .NET Core 콘솔 앱을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-103">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="be61e-104">Windows에서 Visual Studio를 사용하거나 Windows, macOS 또는 Linux에서 .NET Core CLI를 사용하여 자습서를 진행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-104">You can follow the tutorial by using Visual Studio on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="be61e-105">[GitHub에서 이 아티클의 샘플을 봅니다](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span><span class="sxs-lookup"><span data-stu-id="be61e-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="be61e-106">전제 조건</span><span class="sxs-lookup"><span data-stu-id="be61e-106">Prerequisites</span></span>

<span data-ttu-id="be61e-107">다음 소프트웨어를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-107">Install the following software:</span></span>

## <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="be61e-108">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="be61e-108">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="be61e-109">[.NET Core 3.0 SDK](https://www.microsoft.com/net/download/core)</span><span class="sxs-lookup"><span data-stu-id="be61e-109">[.NET Core 3.0 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be61e-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be61e-110">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="be61e-111">이 워크로드의 경우 [Visual Studio 2019 버전 16.3 이상](https://www.visualstudio.com/downloads/):</span><span class="sxs-lookup"><span data-stu-id="be61e-111">[Visual Studio 2019 version 16.3 or later](https://www.visualstudio.com/downloads/) with this  workload:</span></span>
  * <span data-ttu-id="be61e-112">**.NET Core 플랫폼 간 개발**(**기타 도구 집합** 아래)</span><span class="sxs-lookup"><span data-stu-id="be61e-112">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="be61e-113">새 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="be61e-113">Create a new project</span></span>

## <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="be61e-114">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="be61e-114">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new console -o EFGetStarted
cd EFGetStarted
```

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be61e-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be61e-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="be61e-116">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-116">Open Visual Studio</span></span>
* <span data-ttu-id="be61e-117">**새 프로젝트 만들기**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-117">Click **Create a new project**</span></span>
* <span data-ttu-id="be61e-118">**C#** 태그가 있는 **콘솔 앱(.NET Core)** 을 선택하고 **다음**을 클릭합니다</span><span class="sxs-lookup"><span data-stu-id="be61e-118">Select **Console App (.NET Core)** with the **C#** tag and click **Next**</span></span>
* <span data-ttu-id="be61e-119">이름에 **EFGetStarted**를 입력하고 **만들기**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-119">Enter **EFGetStarted** for the name and click **Create**</span></span>

---

## <a name="install-entity-framework-core"></a><span data-ttu-id="be61e-120">Entity Framework Core 설치</span><span class="sxs-lookup"><span data-stu-id="be61e-120">Install Entity Framework Core</span></span>

<span data-ttu-id="be61e-121">EF Core를 설치하려면 대상으로 지정할 EF Core 데이터베이스 공급자에 대한 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-121">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="be61e-122">이 자습서에서는 .NET Core가 지원하는 모든 플랫폼에서 실행되는 SQLite를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-122">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span> <span data-ttu-id="be61e-123">사용 가능한 공급자 목록은 [데이터베이스 공급자](../providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="be61e-123">For a list of available providers, see [Database Providers](../providers/index.md).</span></span>

## <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="be61e-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="be61e-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be61e-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be61e-125">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="be61e-126">**도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="be61e-126">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="be61e-127">다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-127">Run the following commands:</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

<span data-ttu-id="be61e-128">팁: 프로젝트를 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리**를 선택하여 패키지를 설치할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-128">Tip: You can also install packages by right-clicking on the project and selecting **Manage NuGet Packages**</span></span>

---

## <a name="create-the-model"></a><span data-ttu-id="be61e-129">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="be61e-129">Create the model</span></span>

<span data-ttu-id="be61e-130">모델을 구성하는 컨텍스트 클래스 및 엔터티 클래스를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-130">Define a context class and entity classes that make up the model.</span></span>

## <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="be61e-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="be61e-131">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="be61e-132">프로젝트 디렉터리에서 다음 코드를 사용하여 **Model.cs**를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-132">In the project directory, create **Model.cs** with the following code</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be61e-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be61e-133">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="be61e-134">프로젝트를 마우스 오른쪽 단추로 클릭하고 **추가 > 클래스**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-134">Right-click on the project and select **Add > Class**</span></span>
* <span data-ttu-id="be61e-135">이름으로 **Model.cs**를 입력하고 **추가**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-135">Enter **Model.cs** as the name and click **Add**</span></span>
* <span data-ttu-id="be61e-136">파일의 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-136">Replace the contents of the file with the following code</span></span>

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

<span data-ttu-id="be61e-137">EF Core는 모델을 기존 데이터베이스에서 [리버스 엔지니어링](../managing-schemas/scaffolding.md)할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-137">EF Core can also [reverse engineer](../managing-schemas/scaffolding.md) a model from an existing database.</span></span>

<span data-ttu-id="be61e-138">팁: 실제 앱에서는 각 클래스를 별도의 파일에 저장하고 구성 파일 또는 환경 변수에 [연결 문자열](../miscellaneous/connection-strings.md)을 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-138">Tip: In a real app, you put each class in a separate file and put the [connection string](../miscellaneous/connection-strings.md) in a configuration file or environment variable.</span></span> <span data-ttu-id="be61e-139">자습서를 간단히 유지하기 위해 모든 항목이 하나의 파일에 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-139">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="be61e-140">데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="be61e-140">Create the database</span></span>

<span data-ttu-id="be61e-141">다음 단계에서는 [마이그레이션](xref:core/managing-schemas/migrations/index)을 사용하여 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-141">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

## <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="be61e-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="be61e-142">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="be61e-143">다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-143">Run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="be61e-144">그러면 [dotnet ef](../miscellaneous/cli/dotnet.md)와 프로젝트에서 명령을 실행하는 데 필요한 디자인 패키지가 설치됩니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-144">This installs [dotnet ef](../miscellaneous/cli/dotnet.md) and the design package which is required to run the command on a project.</span></span> <span data-ttu-id="be61e-145">`migrations` 명령은 마이그레이션을 스캐폴딩하여 모델에 대한 초기 테이블 집합을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-145">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="be61e-146">`database update` 명령은 데이터베이스를 만들고 새 마이그레이션을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-146">The `database update` command creates the database and applies the new migration to it.</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be61e-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be61e-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="be61e-148">**패키지 관리자 콘솔**에서 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-148">Run the following commands in **Package Manager Console**</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="be61e-149">그러면 [EF Core용 PMC 도구](../miscellaneous/cli/powershell.md)가 설치됩니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-149">This installs the [PMC tools for EF Core](../miscellaneous/cli/powershell.md).</span></span> <span data-ttu-id="be61e-150">`Add-Migration` 명령은 마이그레이션을 스캐폴딩하여 모델에 대한 초기 테이블 집합을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-150">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="be61e-151">`Update-Database` 명령은 데이터베이스를 만들고 새 마이그레이션을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-151">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-read-update--delete"></a><span data-ttu-id="be61e-152">만들기, 읽기, 업데이트 및 삭제</span><span class="sxs-lookup"><span data-stu-id="be61e-152">Create, read, update & delete</span></span>

* <span data-ttu-id="be61e-153">*Program.cs*를 열고 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-153">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a><span data-ttu-id="be61e-154">앱 실행</span><span class="sxs-lookup"><span data-stu-id="be61e-154">Run the app</span></span>

## <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="be61e-155">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="be61e-155">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet run
```

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be61e-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be61e-156">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="be61e-157">Visual Studio는 .NET Core 콘솔 앱을 실행할 때 일관되지 않은 작업 디렉터리를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-157">Visual Studio uses an inconsistent working directory when running .NET Core console apps.</span></span> <span data-ttu-id="be61e-158">([dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619) 참조) 이로 인해 다음과 같은 예외가 발생합니다. *해당 테이블이 없습니다. Blogs*.</span><span class="sxs-lookup"><span data-stu-id="be61e-158">(see [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)) This results in an exception being thrown: *no such table: Blogs*.</span></span> <span data-ttu-id="be61e-159">작업 디렉터리를 업데이트하려면</span><span class="sxs-lookup"><span data-stu-id="be61e-159">To update the working directory:</span></span>

* <span data-ttu-id="be61e-160">프로젝트를 마우스 오른쪽 단추로 클릭하고 **프로젝트 파일 편집**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-160">Right-click on the project and select **Edit Project File**</span></span>
* <span data-ttu-id="be61e-161">*Targetframework* 속성 바로 아래에 다음을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-161">Just below the *TargetFramework* property, add the following:</span></span>

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* <span data-ttu-id="be61e-162">파일 저장</span><span class="sxs-lookup"><span data-stu-id="be61e-162">Save the file</span></span>

<span data-ttu-id="be61e-163">이제 앱을 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-163">Now you can run the app:</span></span>

* <span data-ttu-id="be61e-164">**디버그 > 디버깅하지 않고 시작**</span><span class="sxs-lookup"><span data-stu-id="be61e-164">**Debug > Start Without Debugging**</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="be61e-165">다음 단계</span><span class="sxs-lookup"><span data-stu-id="be61e-165">Next steps</span></span>

* <span data-ttu-id="be61e-166">[ASP.NET Core 자습서](/aspnet/core/data/ef-rp/intro)에 따라 웹앱에서 EF Core를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-166">Follow the [ASP.NET Core Tutorial](/aspnet/core/data/ef-rp/intro) to use EF Core in a web app</span></span>
* <span data-ttu-id="be61e-167">[LINQ 쿼리 식](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)에 대해 자세히 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-167">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
* <span data-ttu-id="be61e-168">[모델을 구성](xref:core/modeling/index)하여 [필수](xref:core/modeling/required-optional) 및 [최대 길이](xref:core/modeling/max-length)와 같은 항목을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-168">[Configure your model](xref:core/modeling/index) to specify things like [required](xref:core/modeling/required-optional) and [maximum length](xref:core/modeling/max-length)</span></span>
* <span data-ttu-id="be61e-169">모델을 변경한 후 [마이그레이션](xref:core/managing-schemas/migrations/index)을 사용하여 데이터베이스 스키마를 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="be61e-169">Use [Migrations](xref:core/managing-schemas/migrations/index) to update the database schema after changing your model</span></span>
