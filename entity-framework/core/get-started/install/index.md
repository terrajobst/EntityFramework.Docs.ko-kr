---
title: Entity Framework Core 설치
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 58c79d477d590eea355a922b3e1233bbecb305cc
ms.sourcegitcommit: a6082a2caee62029f101eb1000656966195cd6ee
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53181983"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="db2fc-102">Entity Framework Core 설치</span><span class="sxs-lookup"><span data-stu-id="db2fc-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db2fc-103">전제 조건</span><span class="sxs-lookup"><span data-stu-id="db2fc-103">Prerequisites</span></span>

* <span data-ttu-id="db2fc-104">EF Core는 [.NET Standard 2.0](/dotnet/standard/net-standard) 라이브러리입니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-104">EF Core is a [.NET Standard 2.0](/dotnet/standard/net-standard) library.</span></span> <span data-ttu-id="db2fc-105">따라서 EF Core는 .NET Standard 2.0을 지원하는 .NET 구현이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-105">So EF Core requires a .NET implementation that supports .NET Standard 2.0 to run.</span></span> <span data-ttu-id="db2fc-106">EF Core는 다른 .NET Standard 2.0 라이브러리에서도 참조할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-106">EF Core can also be referenced by other .NET Standard 2.0 libraries.</span></span> 

* <span data-ttu-id="db2fc-107">예를 들어 .NET Core를 사용하여 EF Core를 대상으로 하는 앱을 개발할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-107">For example, you can use EF Core to develop apps that target .NET Core.</span></span> <span data-ttu-id="db2fc-108">.NET Core 앱을 빌드하려면 [.NET Core SDK](https://dotnet.microsoft.com/download)가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-108">Building .NET Core apps requires the [.NET Core SDK](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="db2fc-109">필요에 따라 Visual Studio, Visual Studio for Mac 또는 Visual Studio Code와 같은 개발 환경을 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-109">Optionally, you can also use a development environment like Visual Studio, Visual Studio for Mac, or Visual Studio Code.</span></span> <span data-ttu-id="db2fc-110">자세한 내용은 [.NET Core 시작](/dotnet/core/get-started)을 확인하세요.</span><span class="sxs-lookup"><span data-stu-id="db2fc-110">For more information, check [Getting Started with .NET Core](/dotnet/core/get-started).</span></span>

* <span data-ttu-id="db2fc-111">EF Core를 통해 Visual Studio를 사용하여 Windows에서 .NET Framework 4.6.1 이상을 대상으로 하는 애플리케이션을 개발할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-111">You can use EF Core to develop applications that target .NET Framework 4.6.1 or later on Windows, using Visual Studio.</span></span> <span data-ttu-id="db2fc-112">Visual Studio의 최신 버전을 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-112">The latest version of Visual Studio is recommended.</span></span> <span data-ttu-id="db2fc-113">Visual Studio 2015와 같은 이전 버전을 사용하려면 [NuGet 클라이언트를 버전 3.6.0로 업그레이드](https://www.nuget.org/downloads)하여 .NET Standard 2.0 라이브러리와 함께 작동하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-113">If you want to use an older version, like Visual Studio 2015, make sure you [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads) to work with .NET Standard 2.0 libraries.</span></span>

* <span data-ttu-id="db2fc-114">EF Core는 Xamarin 및 .NET 네이티브와 같은 다른 .NET 구현에서도 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-114">EF Core can run on other .NET implementations like Xamarin and .NET Native.</span></span> <span data-ttu-id="db2fc-115">하지만 실제로 해당 구현에는 앱에서 EF Core의 작동 방식에 영향을 미칠 수 있는 런타임 제한이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-115">But in practice those implementations have runtime limitations that may affect how well EF Core works on your app.</span></span> <span data-ttu-id="db2fc-116">자세한 내용은 [EF Core에서 지원되는 .NET 구현](xref:core/platforms/index)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="db2fc-116">For more information, see [.NET implementations supported by EF Core](xref:core/platforms/index).</span></span>

* <span data-ttu-id="db2fc-117">마지막으로, 다른 데이터베이스 공급자는 특정 데이터베이스 엔진 버전, .NET 구현 또는 운영 체제가 필요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-117">Finally, different database providers may require specific database engine versions, .NET implementations, or operating systems.</span></span> <span data-ttu-id="db2fc-118">애플리케이션에 적합한 환경을 지원하는 [EF Core 데이터베이스 공급자](xref:core/providers/index)를 사용할 수 있는지 확인하세요.</span><span class="sxs-lookup"><span data-stu-id="db2fc-118">Make sure an [EF Core database provider](xref:core/providers/index) is available that supports the right environment for your application.</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="db2fc-119">Entity Framework Core 런타임 가져오기</span><span class="sxs-lookup"><span data-stu-id="db2fc-119">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="db2fc-120">EF Core를 애플리케이션에 추가하려면 사용할 데이터베이스 공급자에 대한 NuGet 패키지를 설치하세요.</span><span class="sxs-lookup"><span data-stu-id="db2fc-120">To add EF Core to an application, install the NuGet package for the database provider you want to use.</span></span>

<span data-ttu-id="db2fc-121">ASP.NET Core 애플리케이션을 빌드하는 경우 메모리 내 및 SQL Server 공급자를 설치할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-121">If you're building an ASP.NET Core application, you don't need to install the in-memory and SQL Server providers.</span></span> <span data-ttu-id="db2fc-122">이러한 공급자는 EF Core 런타임와 함께 ASP.NET Core의 현재 버전에 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-122">Those providers are included in current versions of ASP.NET Core, alongside the EF Core runtime.</span></span>  

<span data-ttu-id="db2fc-123">NuGet 패키지를 설치하거나 업데이트하려면 [.NET Core CLI(명령줄 인터페이스), Visual Studio 패키지 관리자 대화 상자 또는 Visual Studio 패키지 관리자 콘솔을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-123">To install or update NuGet packages, you can use the [.NET Core command-line interface (CLI), the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

### <a name="net-core-cli"></a><span data-ttu-id="db2fc-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="db2fc-124">.NET Core CLI</span></span>

* <span data-ttu-id="db2fc-125">운영 체제의 명령줄에서 다음 .NET Core CLI 명령을 사용하여 EF Core SQL Server 공급자를 설치하거나 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-125">Use the following .NET Core CLI command from the operating system's command line to install or update the EF Core SQL Server provider:</span></span>

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* <span data-ttu-id="db2fc-126">`dotnet add package` 명령에서 `-v` 한정자를 사용하여 특정 버전을 나타낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-126">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="db2fc-127">예를 들어 EF Core 2.2.0 패키지를 설치하려면 명령에 `-v 2.2.0`을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-127">For example, to install EF Core 2.2.0 packages, append `-v 2.2.0` to the command.</span></span>

<span data-ttu-id="db2fc-128">자세한 내용은 [.NET CLI(명령줄 인터페이스 도구)](/dotnet/core/tools/)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="db2fc-128">For more information, see [.NET command-line interface (CLI) tools](/dotnet/core/tools/).</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="db2fc-129">Visual Studio NuGet 패키지 관리자 대화 상자</span><span class="sxs-lookup"><span data-stu-id="db2fc-129">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="db2fc-130">Visual Studio 메뉴에서 **프로젝트 > NuGet 패키지 관리** 선택</span><span class="sxs-lookup"><span data-stu-id="db2fc-130">From the Visual Studio menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="db2fc-131">**찾아보기** 또는 **업데이트** 탭 클릭</span><span class="sxs-lookup"><span data-stu-id="db2fc-131">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="db2fc-132">SQL Server 공급자를 설치하거나 업데이트하려면 `Microsoft.EntityFrameworkCore.SqlServer` 패키지를 선택하고 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-132">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="db2fc-133">자세한 내용은 [NuGet 패키지 관리자 대화 상자](/nuget/tools/package-manager-ui)를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="db2fc-133">For more information, see [NuGet Package Manager Dialog](/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="db2fc-134">Visual Studio NuGet 패키지 관리자 콘솔</span><span class="sxs-lookup"><span data-stu-id="db2fc-134">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="db2fc-135">Visual Studio 메뉴에서 **도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔** 선택</span><span class="sxs-lookup"><span data-stu-id="db2fc-135">From the Visual Studio menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="db2fc-136">SQL Server 공급자를 설치하려면 패키지 관리자 콘솔에서 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-136">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="db2fc-137">공급자를 업데이트하려면 `Update-Package` 명령을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-137">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="db2fc-138">특정 버전을 지정하려면 `-Version` 한정자를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-138">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="db2fc-139">예를 들어 EF Core 2.2.0 패키지를 설치하려면 명령에 `-Version 2.2.0`을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-139">For example, to install EF Core 2.2.0 packages, append `-Version 2.2.0` to the commands</span></span>

<span data-ttu-id="db2fc-140">자세한 내용은 [패키지 관리자 콘솔](/nuget/tools/package-manager-console)을 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="db2fc-140">For more information, see [Package Manager Console](/nuget/tools/package-manager-console).</span></span>

## <a name="get-the-entity-framework-core-tools"></a><span data-ttu-id="db2fc-141">Entity Framework Core 도구 가져오기</span><span class="sxs-lookup"><span data-stu-id="db2fc-141">Get the Entity Framework Core tools</span></span>

<span data-ttu-id="db2fc-142">프로젝트에서 EF Core 관련 작업(예: 데이터베이스 마이그레이션 생성 및 적용)을 수행하거나 기존 데이터베이스를 기반으로 EF Core 모델을 생성하는 도구를 설치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-142">You can install tools to carry out EF Core-related tasks in your project, like creating and applying database migrations, or creating an EF Core model based on an existing database.</span></span>

<span data-ttu-id="db2fc-143">사용 가능한 두 가지 도구 집합:</span><span class="sxs-lookup"><span data-stu-id="db2fc-143">Two sets of tools are available:</span></span>

* <span data-ttu-id="db2fc-144">[.NET Core CLI(명령줄 인터페이스) 도구](xref:core/miscellaneous/cli/dotnet)는 Windows, Linux 또는 macOS에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-144">The [.NET Core command-line interface (CLI) tools](xref:core/miscellaneous/cli/dotnet) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="db2fc-145">이러한 명령은 `dotnet ef`로 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-145">These commands begin with `dotnet ef`.</span></span> 

* <span data-ttu-id="db2fc-146">[PMC(패키지 관리자 콘솔 도구)](xref:core/miscellaneous/cli/powershell)는 Windows의 Visual Studio에서 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-146">The [Package Manager Console (PMC) tools](xref:core/miscellaneous/cli/powershell) run in Visual Studio on Windows.</span></span> <span data-ttu-id="db2fc-147">이러한 명령은 동사(예: `Add-Migration`, `Update-Database`)로 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-147">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="db2fc-148">패키지 관리자 콘솔에서 `dotnet ef` 명령을 사용할 수도 있지만 Visual Studio를 사용할 경우 패키지 관리자 콘솔 도구를 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-148">Although you can also use the `dotnet ef` commands from the Package Manager Console, it's recommended to use the Package Manager Console tools when you're using Visual Studio:</span></span>

* <span data-ttu-id="db2fc-149">수동으로 디렉터리를 전환하지 않고 Visual Studio의 PMC에서 선택한 현재 프로젝트에서 자동으로 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-149">They automatically work with the current project selected in the PMC in Visual Studio, without requiring manually switching directories.</span></span>  

* <span data-ttu-id="db2fc-150">명령을 완료하면 자동으로 Visual Studio에서 명령을 통해 생성된 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-150">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a><span data-ttu-id="db2fc-151">.NET Core CLI 도구 가져오기</span><span class="sxs-lookup"><span data-stu-id="db2fc-151">Get the .NET Core CLI tools</span></span>

<span data-ttu-id="db2fc-152">.NET core CLI 도구에는 [필수 구성 요소](#prerequisites)에서 앞서 언급한 .NET Core SDK를 필요로 합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-152">.NET Core CLI tools require the .NET Core SDK, mentioned earlier in [Prerequisites](#prerequisites).</span></span>

<span data-ttu-id="db2fc-153">`dotnet ef` 명령은 .NET Core SDK의 최신 버전에 포함되어 있지만, 특정 프로젝트에서 명령을 사용하려면 `Microsoft.EntityFrameworkCore.Design` 패키지를 설치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-153">The `dotnet ef` commands are included in current versions of the .NET Core SDK, but to enable the commands on a specific project, you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

<span data-ttu-id="db2fc-154">ASP.NET Core 앱의 경우 이 패키지는 자동으로 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-154">For ASP.NET Core apps, this package is included automatically.</span></span>

> [!IMPORTANT]      
> <span data-ttu-id="db2fc-155">항상 런타임 패키지의 주 버전에 부합하는 도구 패키지 버전을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-155">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="db2fc-156">패키지 관리자 콘솔 도구 가져오기</span><span class="sxs-lookup"><span data-stu-id="db2fc-156">Get the Package Manager Console tools</span></span>

<span data-ttu-id="db2fc-157">EF Core용 패키지 관리자 콘솔 도구를 가져오려면 `Microsoft.EntityFrameworkCore.Tools` 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-157">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="db2fc-158">예를 들어 Visual Studio에서:</span><span class="sxs-lookup"><span data-stu-id="db2fc-158">For example, from Visual Studio:</span></span>

``` PowerShell  
Install-Package Microsoft.EntityFrameworkCore.Tools
``` 

<span data-ttu-id="db2fc-159">ASP.NET Core 앱의 경우 이 패키지는 자동으로 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-159">For ASP.NET Core apps, this package is included automatically.</span></span>

## <a name="upgrading-to-the-latest-ef-core"></a><span data-ttu-id="db2fc-160">최신 EF Core로 업그레이드</span><span class="sxs-lookup"><span data-stu-id="db2fc-160">Upgrading to the latest EF Core</span></span>

* <span data-ttu-id="db2fc-161">새로운 버전의 EF Core를 릴리스할 때마다 Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite 및 Microsoft.EntityFrameworkCore.InMemory와 같은 EF Core 프로젝트의 일부인 새 버전의 공급자도 릴리스합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-161">Any time we release a new version of EF Core, we also release a new version of the providers that are part of the EF Core project, like Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite, and Microsoft.EntityFrameworkCore.InMemory.</span></span> <span data-ttu-id="db2fc-162">모든 개선 사항을 가져오려면 새 버전의 공급자로 업그레이드하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-162">You can just upgrade to the new version of the provider to get all the improvements.</span></span> 

* <span data-ttu-id="db2fc-163">EF Core는 SQL Server 및 메모리 내 공급자와 함께 ASP.NET Core의 현재 버전에 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-163">EF Core, together with the SQL Server and the in-memory providers are included in current versions of ASP.NET Core.</span></span> <span data-ttu-id="db2fc-164">기존 ASP.NET Core 애플리케이션을 최신 버전의 EF Core로 업그레이드하려면 항상 ASP.NET Core 버전을 업그레이드합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-164">To upgrade an existing ASP.NET Core application to a newer version of EF Core, always upgrade the version of ASP.NET Core.</span></span>

* <span data-ttu-id="db2fc-165">타사 데이터베이스 공급자를 사용하는 응용 프로그램을 업데이트해야 할 경우 항상 공급자의 업데이트가 사용할 EF Core 버전과 호환되는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-165">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="db2fc-166">예를 들어 이전 버전에 대한 데이터베이스 공급자는 EF Core 런타임 버전 2.0과 호환되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-166">For example, database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>

* <span data-ttu-id="db2fc-167">EF Core용 타사 공급자는 일반적으로 EF Core 런타임과 함께 패치 버전을 릴리스하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-167">Third-party providers for EF Core usually don't release patch versions alongside the EF Core runtime.</span></span> <span data-ttu-id="db2fc-168">타사 공급자를 사용하는 애플리케이션을 EF Core의 패치 버전으로 업그레이드하려면 Microsoft.EntityFrameworkCore 및 Microsoft.EntityFrameworkCore.Relational과 같은 개별 EF Core 런타임 구성 요소에 직접 참조를 추가해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-168">To upgrade an application that uses a third-party provider to a patch version of EF Core, you may need to add a direct reference to individual EF Core runtime components, such as Microsoft.EntityFrameworkCore, and Microsoft.EntityFrameworkCore.Relational.</span></span>

* <span data-ttu-id="db2fc-169">기존 애플리케이션을 최신 버전의 EF Core로 업그레이드할 경우, 이전 EF Core 패키지에 대한 일부 참조를 수동으로 제거해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-169">If you're upgrading an existing application to the latest version of EF Core, some references to older EF Core packages may need to be removed manually:</span></span>

  * <span data-ttu-id="db2fc-170">`Microsoft.EntityFrameworkCore.SqlServer.Design`과 같은 데이터베이스 공급자 디자인 타임 패키지는 EF Core 2.0 이상에서 더 이상 필요하거나 지원되지 않지만, 다른 패키지를 업그레이드할 때 자동으로 제거되지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-170">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported from EF Core 2.0 and later, but aren't automatically removed when upgrading the other packages.</span></span>

  * <span data-ttu-id="db2fc-171">.NET CLI 도구는 버전 2.1부터 .NET SDK에 포함되므로 해당 패키지에 대한 참조를 프로젝트 파일에서 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-171">The .NET CLI tools are included in the .NET SDK since version 2.1, so the reference to that package can be removed from the project file:</span></span>

    ```
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```

* <span data-ttu-id="db2fc-172">.NET Framework를 대상으로 하는 애플리케이션은 .NET Standard 2.0 라이브러리와 함께 작동하도록 변경해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-172">Applications that target .NET Framework may need changes to work with .NET Standard 2.0 libraries:</span></span>

  * <span data-ttu-id="db2fc-173">프로젝트 파일을 편집하고 최초 속성 그룹에 다음 항목이 표시되는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-173">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * <span data-ttu-id="db2fc-174">테스트 프로젝트의 경우 다음 항목도 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="db2fc-174">For test projects, also make sure the following entry is present:</span></span>

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
