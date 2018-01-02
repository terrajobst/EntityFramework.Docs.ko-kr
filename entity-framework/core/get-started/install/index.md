---
title: "EF Core 설치"
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 31b96ebd0ae282b88be98988eff6263084dc5dd5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
# <a name="installing-ef-core"></a><span data-ttu-id="aab96-102">EF Core 설치</span><span class="sxs-lookup"><span data-stu-id="aab96-102">Installing EF Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aab96-103">필수 구성 요소</span><span class="sxs-lookup"><span data-stu-id="aab96-103">Prerequisites</span></span>

<span data-ttu-id="aab96-104">.NET Core 2.0 응용 프로그램(.NET Core를 대상으로 하는 ASP.NET Core 2.0 응용 프로그램 포함)을 개발하기 위해서는 플랫폼에 맞는 [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core)를 다운로드하여 설치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-104">In order to develop .NET Core 2.0 applications (including ASP.NET Core 2.0 applications that target .NET Core) you will need to download and install a version of the [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) that is appropriate to your platform.</span></span> <span data-ttu-id="aab96-105">**Visual Studio 2017 15.3 버전을 설치한 경우에도 마찬가지입니다.**</span><span class="sxs-lookup"><span data-stu-id="aab96-105">**This is true even if you have installed Visual Studio 2017 version 15.3.**</span></span>

<span data-ttu-id="aab96-106">EF Core 2.0 또는 기타 .NET Standard 2.0 라이브러리를 .NET Core 2.0 이외의 .NET 플랫폼(예: .NET Framework 4.6.1 이상)에서 사용하려면 .NET Standard 2.0을 인지하는 NuGet 버전과 그에 호환되는 프레임워크가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-106">In order to use EF Core 2.0 or any other .NET Standard 2.0 library with a .NET platforms besides .NET Core 2.0 (e.g. with .NET Framework 4.6.1 or greater) you will need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="aab96-107">이것은 몇 가지 방법으로 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-107">Here are a few ways you can obtain this:</span></span>

* <span data-ttu-id="aab96-108">Visual Studio 2017 15.3 버전 설치</span><span class="sxs-lookup"><span data-stu-id="aab96-108">Install Visual Studio 2017 version 15.3</span></span>
* <span data-ttu-id="aab96-109">Visual Studio 2015를 사용할 경우 [NuGet 클라이언트 버전 3.6.0 다운로드 및 업그레이드](https://www.nuget.org/downloads)</span><span class="sxs-lookup"><span data-stu-id="aab96-109">If you are using Visual Studio 2015, [download and upgrade NuGet client to version 3.6.0](https://www.nuget.org/downloads)</span></span>

<span data-ttu-id="aab96-110">이전 버전의 Visual Studio로 만들었고 .NET Framework를 대상으로 하는 프로젝트는 .NET Standard 2.0 라이브러리와의 호환을 위해 추가적인 수정이 필요할 수 있습니다. </span><span class="sxs-lookup"><span data-stu-id="aab96-110">Projects created with previous versions of Visual Studio and targeting .NET Framework may need additional modifications in order to be compatible with .NET Standard 2.0 libraries:</span></span>

* <span data-ttu-id="aab96-111">프로젝트 파일을 편집하고 최초 속성 그룹에 다음 항목이 표시되는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-111">Edit the project file and make sure the following entry appears in the initial property group:</span></span>
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* <span data-ttu-id="aab96-112">테스트 프로젝트의 경우 다음 항목도 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-112">For test projects, also make sure the following entry is present:</span></span>
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a><span data-ttu-id="aab96-113">비트 가져오기</span><span class="sxs-lookup"><span data-stu-id="aab96-113">Getting the bits</span></span>
<span data-ttu-id="aab96-114">EF Core 런타임 라이브러리를 응용 프로그램에 추가하기 위해 권장되는 방법은 NuGet으로부터 EF Core 데이터베이스 공급자를 설치하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-114">The recommended way to add EF Core runtime libraries into an application is to install an EF Core database provider from NuGet.</span></span>

<span data-ttu-id="aab96-115">런타임 라이브러리 외에도 마이그레이션 만들기 및 적용, 기존 데이터베이스 기반 모델 만들기 등과 같이, 설계 시점에 프로젝트의 여러 EF Core 관련 작업을 더 간편하게 수행할 수 있게 하는 도구를 설치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-115">Besides the runtime libraries, you can install tools which make it easier to perform several EF Core-related tasks in your project at design time, such as creating and applying migrations, and creating a model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="aab96-116">타사 데이터베이스 공급자를 사용하는 응용 프로그램을 업데이트해야 할 경우 항상 공급자의 업데이트가 사용할 EF Core 버전과 호환되는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-116">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="aab96-117">예를 들어</span><span class="sxs-lookup"><span data-stu-id="aab96-117">E.g.</span></span> <span data-ttu-id="aab96-118">이전 버전에 대한 데이터베이스 공급자는 EF Core 런타임 버전 2.0과 호환되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-118">database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>  

> [!TIP]  
> <span data-ttu-id="aab96-119">ASP.NET Core 2.0을 대상으로 하는 응용 프로그램은 추가적인 종속성 없이 타사 데이터베이스 공급자와 함께 EF Core 2.0을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-119">Applications targeting ASP.NET Core 2.0 can use EF Core 2.0 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="aab96-120">이전 버전의 ASP.NET Core를 대상으로 응용 프로그램은 EF Core 2.0을 사용하려면 ASP.NET Core 2.0으로 업그레이드해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-120">Applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.0 in order to use EF Core 2.0.</span></span>

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a><span data-ttu-id="aab96-121">.NET Core 명령줄 인터페이스(CLI)를 사용한 플랫폼 간 개발</span><span class="sxs-lookup"><span data-stu-id="aab96-121">Cross-platform development using the .NET Core Command Line Interface (CLI)</span></span>

<span data-ttu-id="aab96-122">[.NET Core](https://www.microsoft.com/net/download/core)를 대상으로 하는 응용 프로그램을 개발하기 위해 자주 사용하는 텍스트 편집기와 함께 [`dotnet` CLI 명령](https://docs.microsoft.com/dotnet/core/tools/)을 사용하거나, Visual Studio, Visual Studio for Mac 또는 Visual Studio Code 같은 IDE(Integrated Development Environment)를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-122">To develop applications that target [.NET Core](https://www.microsoft.com/net/download/core) you can choose to use the [`dotnet` CLI commands](https://docs.microsoft.com/dotnet/core/tools/) in combination with your favorite text editor, or an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio for Mac or Visual Studio Code.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="aab96-123">.NET Core를 대상으로 하는 응용 프로그램은 특정 Visual Studio 버전이 필요합니다. 즉 .NET Core 1.x 개발에는 Visual Studio 2017이, .NET Core 2.0 개발에는 Visual Studio 2017 버전 15.3이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-123">Applications that target .NET Core require specific versions of Visual Studio, e.g. .NET Core 1.x development requires Visual Studio 2017, while .NET Core 2.0 development requires Visual Studio 2017 version 15.3.</span></span>

<span data-ttu-id="aab96-124">플랫폼 간 .NET Core 응용 프로그램에서 SQL Server 공급자를 설치하거나 업그레이드하려면 응용 프로그램의 디렉터리로 전환한 다음 명령줄에서 다음을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-124">To install or upgrade the SQL Server provider in a cross-platform .NET Core application, switch to the application's directory and run the following in a command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="aab96-125">`dotnet add package` 명령에서 `-v` 한정자를 사용하여 특정 버전 설치를 지시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-125">You can indicate a specific version install in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="aab96-126">예를 들어</span><span class="sxs-lookup"><span data-stu-id="aab96-126">E.g.</span></span> <span data-ttu-id="aab96-127">EF Core 2.0 패키지를 설치하려면 명령에 `-v 2.0.0`을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-127">to install EF Core 2.0 packages, append `-v 2.0.0` to the command.</span></span>

<span data-ttu-id="aab96-128">EF Core에는 `dotnet ef`로 시작하는 [`dotnet` CLI](../../miscellaneous/cli/dotnet.md)에 대한 추가 명령 집합이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-128">EF Core includes a set of [additional commands for the `dotnet` CLI](../../miscellaneous/cli/dotnet.md), starting with `dotnet ef`.</span></span> <span data-ttu-id="aab96-129">`dotnet ef` CLI 명령을 사용하려면 응용 프로그램의 `.csproj` 파일에 다음 항목이 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-129">In order to use the `dotnet ef` CLI commands, your application’s `.csproj` file needs to contain the following entry:</span></span>

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="aab96-130">EF Core용.NET Core CLI 도구에도 Microsoft.EntityFrameworkCore.Design이라고 하는 별도의 패키지가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-130">The .NET Core CLI tools for EF Core also require a separate package called Microsoft.EntityFrameworkCore.Design.</span></span> <span data-ttu-id="aab96-131">다음을 통해 프로젝트에 추가하기만 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-131">You can simply add it to the project using:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> <span data-ttu-id="aab96-132">항상 런타임 페키지의 주 버전에 부합하는 도구 패키지 버전을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-132">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a><span data-ttu-id="aab96-133">Visual Studio 개발 </span><span class="sxs-lookup"><span data-stu-id="aab96-133">Visual Studio development</span></span>

<span data-ttu-id="aab96-134">Visual Studio를 사용하여 .NET Core, .NET Framework 또는 기타 EF Core에서 지원하는 플랫폼을 대상으로 하는 여러 다양한 응용 프로그램 유형을 개발할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-134">You can develop many different types of applications that target .NET Core, .NET Framework, or other platforms supported by EF Core using Visual Studio.</span></span>

<span data-ttu-id="aab96-135">두 가지 방법으로 Visual Studio에서 응용 프로그램에 EF Core 데이터베이스 공급자를 설치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-135">There are two ways you can install an EF Core database provider in your application from Visual Studio:</span></span>

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a><span data-ttu-id="aab96-136">NuGet의 [패키지 관리자 사용자 인터페이스](https://docs.microsoft.com/nuget/tools/package-manager-ui) 사용</span><span class="sxs-lookup"><span data-stu-id="aab96-136">Using NuGet's [Package Manager User Interface](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span></span>

* <span data-ttu-id="aab96-137">**프로젝트 > NuGet 패키지 관리** 메뉴 선택</span><span class="sxs-lookup"><span data-stu-id="aab96-137">Select on the menu **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="aab96-138">**찾아보기** 또는 **업데이트** 탭 클릭</span><span class="sxs-lookup"><span data-stu-id="aab96-138">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="aab96-139">`Microsoft.EntityFrameworkCore.SqlServer` 패키지와 원하는 버전을 선택하고 확인</span><span class="sxs-lookup"><span data-stu-id="aab96-139">Select the `Microsoft.EntityFrameworkCore.SqlServer` package and the desired version and confirm</span></span>

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a><span data-ttu-id="aab96-140">NuGet의 [PMC(패키지 관리자 콘솔)](https://docs.microsoft.com/nuget/tools/package-manager-console) 사용</span><span class="sxs-lookup"><span data-stu-id="aab96-140">Using NuGet's [Package Manager Console (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span></span>

* <span data-ttu-id="aab96-141">**도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔** 메뉴 선택</span><span class="sxs-lookup"><span data-stu-id="aab96-141">Select on the menu **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="aab96-142">PMC에서 다음 명령 입력 및 실행</span><span class="sxs-lookup"><span data-stu-id="aab96-142">Type and run the following command in the PMC:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="aab96-143">이미 설치된 패키지를 더 최신 버전으로 업데이트하는 대신 `Update-Package` 명령을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-143">You can use the `Update-Package` command instead to update a package that is already installed to a more recent  version</span></span>

* <span data-ttu-id="aab96-144">특정 버전을 지정하려면 `-Version` 한정자를 사용합니다. 즉 EF Core 2.0 패키지를 설치하려면 명령에 `-Version 2.0.0`을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-144">To specify a specific version, you can use the `-Version` modifier, e.g. to install EF Core 2.0 packages, append `-Version 2.0.0` to the commands</span></span>

#### <a name="tools"></a><span data-ttu-id="aab96-145">도구</span><span class="sxs-lookup"><span data-stu-id="aab96-145">Tools</span></span>

<span data-ttu-id="aab96-146">Visual Studio에서[ PMC 내부에서 실행되는 EF Core 명령](../../miscellaneous/cli/powershell.md)의 PowerShell 버전도 있습니다. 기능은 `dotnet ef` 명령과 유사합니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-146">There is also a PowerShell version of the [EF Core commands which run inside the PMC](../../miscellaneous/cli/powershell.md) in Visual Studio, with similar capabilities to the `dotnet ef` commands.</span></span> <span data-ttu-id="aab96-147">이 버전을 사용하려면 패키지 관리자 UI 또는 PMC를 사용하여 `Microsoft.EntityFrameworkCore.Tools` 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-147">In order to use these, install the `Microsoft.EntityFrameworkCore.Tools` package using either the Package Manager UI or the PMC.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="aab96-148">항상 런타임 페키지의 주 버전에 부합하는 도구 패키지 버전을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-148">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

> [!TIP]  
> <span data-ttu-id="aab96-149">Visual Studio의 PMC에서 `dotnet ef` 명령을 사용할 수 있으나 PowerShell 버전만큼 편리하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-149">Although it is possible to use the `dotnet ef` commands from the PMC in Visual Studio, it is far more convenient to use the PowerShell version:</span></span>
> * <span data-ttu-id="aab96-150">수동으로 디렉터리를 전환하지 않고 PMC에서 선택한 현재 프로젝트에서 자동으로 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-150">They automatically work with the current project selected in the PMC without requiring manually switching directories.</span></span>  
> * <span data-ttu-id="aab96-151">명령을 완료하면 자동으로 Visual Studio에서 명령을 통해 생성된 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-151">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="aab96-152">**EF Core 2.0에서 사용되지 않는 패키지:** EF Core 2.0으로 기존 응용 프로그램을 업그레이드하면 구 버전 EF Core 패키지에 대한 몇 가지 참조를 수동으로 제거해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-152">**Deprecated packages in EF Core 2.0:** If you are upgrading an existing application to EF Core 2.0, some references to older EF Core packages may need to be removed manually.</span></span> <span data-ttu-id="aab96-153">특히, `Microsoft.EntityFrameworkCore.SqlServer.Design` 같은 데이터베이스 공급자 설계 시간 패키지는 EF Core 2.0에서 더 이상 지원되지 않거나 필요하지 않지만 다른 패키지를 업그레이드할 때 자동으로 제거되지도 않습니다.</span><span class="sxs-lookup"><span data-stu-id="aab96-153">In particular, database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.0, but will not be automatically removed when upgrading the other packages.</span></span>
