---
title: Entity Framework Core 설치
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 455eccbb149f0980cefa250ef5db443c73e66603
ms.sourcegitcommit: ae399f9f3d1bae2c446b552247bd3af3ca5a2cf9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48575641"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="4f3b6-102">Entity Framework Core 설치</span><span class="sxs-lookup"><span data-stu-id="4f3b6-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4f3b6-103">전제 조건</span><span class="sxs-lookup"><span data-stu-id="4f3b6-103">Prerequisites</span></span>

* <span data-ttu-id="4f3b6-104">.NET Core 2.1 대상 앱을 개발하려면 [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-104">To develop apps that target .NET Core 2.1, install [the .NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="4f3b6-105">최신 버전의 Visual Studio 2017이 있는 경우에도 이 SDK를 설치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-105">The SDK has to be installed even if you have the latest version of Visual Studio 2017.</span></span>

* <span data-ttu-id="4f3b6-106">.NET Core 2.1 대상 앱 개발을 위해 Visual Studio를 사용하려면 Visual Studio 2017 버전 15.7 이상을 설치하십시오.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-106">To use Visual Studio for development of apps that target .NET Core 2.1, install Visual Studio 2017 version 15.7 or later.</span></span>

* <span data-ttu-id="4f3b6-107">ASP.NET Core 응용 프로그램에서 Entity Framework 2.1을 사용하려면 ASP.NET Core 2.1을 사용하십시오.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-107">To use Entity Framework 2.1 in ASP.NET Core applications, use ASP.NET Core 2.1.</span></span> <span data-ttu-id="4f3b6-108">이전 버전의 ASP.NET Core를 사용하는 응용 프로그램은 2.1로 업그레이드해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-108">Applications that use earlier versions of ASP.NET Core must be updated to 2.1.</span></span>

* <span data-ttu-id="4f3b6-109">.NET Framework 4.6.1 이상을 대상으로 하는 앱에 Visual Studio 2015를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-109">You can use Visual Studio 2015 for apps that target the .NET Framework 4.6.1 or later.</span></span> <span data-ttu-id="4f3b6-110">하지만 .NET Standard 2.0 및 호환 프레임워크를 인식하는 NuGet 버전이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-110">But you need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="4f3b6-111">Visual Studio 2015에서 이를 가져오려면, [NuGet 클라이언트를 버전 3.6.0으로 업그레이드하십시오](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="4f3b6-111">To get that in Visual Studio 2015, [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads).</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="4f3b6-112">Entity Framework Core 런타임 가져오기</span><span class="sxs-lookup"><span data-stu-id="4f3b6-112">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="4f3b6-113">EF Core 런타임 라이브러리를 응용 프로그램에 추가하려면 사용하려는 데이터베이스 공급자에 대한 NuGet 패키지를 설치하십시오.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-113">To add EF Core runtime libraries to an application, install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="4f3b6-114">지원되는 공급자 및 해당 NuGet 패키지 이름 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-114">For a list of supported providers and their NuGet package names, see [Database providers](../../providers/index.md).</span></span>

<span data-ttu-id="4f3b6-115">NuGet 패키지를 설치하거나 업데이트하려면 .NET Core CLI, Visual Studio 패키지 관리자 대화 상자 또는 Visual Studio 패키지 관리자 콘솔을 사용하십시오.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-115">To install or update NuGet packages, use the .NET Core CLI, the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

<span data-ttu-id="4f3b6-116">ASP.NET Core 2.1 응용 프로그램의 경우 메모리 내 및 SQL Server 공급자가 자동으로 포함되므로 이를 개별적으로 설치할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-116">For ASP.NET Core 2.1 applications, the in-memory and SQL Server providers are automatically included, so there's no need to install them separately.</span></span>

> [!TIP]  
> <span data-ttu-id="4f3b6-117">타사 데이터베이스 공급자를 사용하는 응용 프로그램을 업데이트해야 할 경우 항상 공급자의 업데이트가 사용할 EF Core 버전과 호환되는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-117">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="4f3b6-118">예를 들어 이전 버전에 대한 데이터베이스 공급자는 EF Core 런타임 버전 2.1과 호환되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-118">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

### <a name="net-core-cli"></a><span data-ttu-id="4f3b6-119">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4f3b6-119">.NET Core CLI</span></span>

<span data-ttu-id="4f3b6-120">다음 .NET Core CLI 명령은 SQL Server 공급자를 설치하거나 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-120">The following .NET Core CLI command installs or updates the SQL Server provider:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="4f3b6-121">`dotnet add package` 명령에서 `-v` 한정자를 사용하여 특정 버전을 나타낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-121">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="4f3b6-122">예를 들어 EF Core 2.1.0 패키지를 설치하려면 명령에 `-v 2.1.0`을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-122">For example, to install EF Core 2.1.0 packages, append `-v 2.1.0` to the command.</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="4f3b6-123">Visual Studio NuGet 패키지 관리자 대화 상자</span><span class="sxs-lookup"><span data-stu-id="4f3b6-123">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="4f3b6-124">메뉴에서 **프로젝트 > NuGet 패키지 관리** 선택</span><span class="sxs-lookup"><span data-stu-id="4f3b6-124">From the menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="4f3b6-125">**찾아보기** 또는 **업데이트** 탭 클릭</span><span class="sxs-lookup"><span data-stu-id="4f3b6-125">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="4f3b6-126">SQL Server 공급자를 설치하거나 업데이트하려면 `Microsoft.EntityFrameworkCore.SqlServer` 패키지를 선택하고 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-126">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="4f3b6-127">자세한 내용은 [NuGet 패키지 관리자 대화 상자](https://docs.microsoft.com/nuget/tools/package-manager-ui)를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-127">For more information, see [NuGet Package Manager Dialog](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="4f3b6-128">Visual Studio NuGet 패키지 관리자 콘솔</span><span class="sxs-lookup"><span data-stu-id="4f3b6-128">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="4f3b6-129">메뉴에서 **도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔** 선택</span><span class="sxs-lookup"><span data-stu-id="4f3b6-129">From the menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="4f3b6-130">SQL Server 공급자를 설치하려면 패키지 관리자 콘솔에서 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-130">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="4f3b6-131">공급자를 업데이트하려면 `Update-Package` 명령을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-131">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="4f3b6-132">특정 버전을 지정하려면 `-Version` 한정자를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-132">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="4f3b6-133">예를 들어 EF Core 2.1.0 패키지를 설치하려면 명령에 `-Version 2.1.0`을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-133">For example, to install EF Core 2.1.0 packages, append `-Version 2.1.0` to the commands</span></span>

<span data-ttu-id="4f3b6-134">자세한 내용은 [패키지 관리자 콘솔](https://docs.microsoft.com/nuget/tools/package-manager-console)을 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-134">For more information, see [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console).</span></span>

## <a name="get-entity-framework-core-tools"></a><span data-ttu-id="4f3b6-135">Entity Framework Core 도구</span><span class="sxs-lookup"><span data-stu-id="4f3b6-135">Get Entity Framework Core tools</span></span>

<span data-ttu-id="4f3b6-136">런타임 라이브러리 외에도 디자인 타임에 프로젝트에서 일부 EF Core 관련 작업을 수행할 수 있는 도구를 설치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-136">Besides the runtime libraries, you can install tools that can perform some EF Core-related tasks in your project at design time.</span></span> <span data-ttu-id="4f3b6-137">예를 들어 마이그레이션을 만들고, 마이그레이션을 적용하고, 기존 데이터베이스를 기준으로 모델을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-137">For example, you can create migrations, apply migrations, and create a model based on an existing database.</span></span>

<span data-ttu-id="4f3b6-138">사용 가능한 두 가지 도구 집합:</span><span class="sxs-lookup"><span data-stu-id="4f3b6-138">Two sets of tools are available:</span></span>
* <span data-ttu-id="4f3b6-139">.NET Core [CLI(명령줄 인터페이스) 도구](../../miscellaneous/cli/dotnet.md)는 Windows, Linux 또는 macOS에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-139">The .NET Core [command-line interface (CLI) tools](../../miscellaneous/cli/dotnet.md) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="4f3b6-140">이러한 명령은 `dotnet ef`로 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-140">These commands begin with `dotnet ef`.</span></span> 
* <span data-ttu-id="4f3b6-141">[패키지 관리자 콘솔 도구](../../miscellaneous/cli/powershell.md)는 Windows의 Visual Studio 2017에서 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-141">The [Package Manager Console tools](../../miscellaneous/cli/powershell.md)  run in Visual Studio 2017 on Windows.</span></span> <span data-ttu-id="4f3b6-142">이러한 명령은 동사(예: `Add-Migration`, `Update-Database`)로 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-142">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="4f3b6-143">패키지 관리자 콘솔에서 `dotnet ef` 명령을 사용할 수 있지만 Visual Studio를 사용할 경우 패키지 관리자 콘솔 도구를 사용하는 것이 더 편리합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-143">Although you can use the `dotnet ef` commands from the Package Manager Console, it's more convenient to use the Package Manager Console tools when you're using Visual Studio:</span></span>
* <span data-ttu-id="4f3b6-144">수동으로 디렉터리를 전환하지 않고 패키지 관리자 콘솔에서 현재 선택된 프로젝트에서 자동으로 작업합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-144">They automatically work with the current project selected in the Package Manager Console without requiring manually switching directories.</span></span>  
* <span data-ttu-id="4f3b6-145">명령을 완료하면 자동으로 Visual Studio에서 명령을 통해 생성된 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-145">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-cli-tools"></a><span data-ttu-id="4f3b6-146">CLI 도구 가져오기</span><span class="sxs-lookup"><span data-stu-id="4f3b6-146">Get the CLI tools</span></span>

<span data-ttu-id="4f3b6-147">`dotnet ef` 명령은 .NET Core SDK에 포함되어 있지만, `Microsoft.EntityFrameworkCore.Design` 패키지를 설치하기 위해 필요한 명령을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-147">The `dotnet ef` commands are included in the .NET Core SDK, but to enable the commands you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

<span data-ttu-id="4f3b6-148">ASP.NET Core 2.1 앱의 경우 이 패키지는 자동으로 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-148">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

<span data-ttu-id="4f3b6-149">이전에 [필수 구성 요소](#prerequisites)에 설명된 것처럼 .NET Core 2.1 SDK도 설치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-149">As explained earlier in [Prerequisites](#prerequisites), you also need to install the .NET Core 2.1 SDK.</span></span>

> [!IMPORTANT]      
> <span data-ttu-id="4f3b6-150">항상 런타임 패키지의 주 버전에 부합하는 도구 패키지 버전을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-150">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="4f3b6-151">패키지 관리자 콘솔 도구 가져오기</span><span class="sxs-lookup"><span data-stu-id="4f3b6-151">Get the Package Manager Console tools</span></span>

<span data-ttu-id="4f3b6-152">EF Core용 패키지 관리자 콘솔 도구를 가져오려면 `Microsoft.EntityFrameworkCore.Tools` 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-152">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

<span data-ttu-id="4f3b6-153">ASP.NET Core 2.1 앱의 경우 이 패키지는 자동으로 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-153">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

## <a name="upgrading-to-ef-core-21"></a><span data-ttu-id="4f3b6-154">EF Core 2.1로 업그레이드</span><span class="sxs-lookup"><span data-stu-id="4f3b6-154">Upgrading to EF Core 2.1</span></span>

<span data-ttu-id="4f3b6-155">기존 응용 프로그램을 EF Core 2.1로 업그레이드할 경우, 이전 EF Core 패키지에 대한 일부 참조를 수동으로 제거해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-155">If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>

* <span data-ttu-id="4f3b6-156">`Microsoft.EntityFrameworkCore.SqlServer.Design` 같은 데이터베이스 공급자 디자인 타임 패키지는 EF Core 2.1에서 더 이상 지원되지 않거나 필요하지 않지만, 다른 패키지를 업그레이드할 때 자동으로 제거되지도 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-156">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but aren't automatically removed when upgrading the other packages.</span></span>

* <span data-ttu-id="4f3b6-157">이제 .NET CLI 도구가 .NET SDK에 포함되므로 해당 패키지에 대한 참조를 *.csproj* 파일에서 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-157">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

<span data-ttu-id="4f3b6-158">.NET Framework를 대상으로 하고 이전 버전의 Visual Studio로 제작된 응용 프로그램의 경우, .NET Standard 2.0 라이브러리와 호환되는지 확인하십시오.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-158">For applications that target the .NET Framework and were created by earlier versions of Visual Studio, make sure that they are compatible with .NET Standard 2.0 libraries:</span></span>

  * <span data-ttu-id="4f3b6-159">프로젝트 파일을 편집하고 최초 속성 그룹에 다음 항목이 표시되는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-159">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * <span data-ttu-id="4f3b6-160">테스트 프로젝트의 경우 다음 항목도 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="4f3b6-160">For test projects, also make sure the following entry is present:</span></span>

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
