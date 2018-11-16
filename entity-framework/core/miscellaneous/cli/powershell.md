---
title: EF Core 도구 참조 (패키지 관리자 콘솔)-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 468698d1bbd17d4ad10b1b1601bfbc315a01c1ff
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688708"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="d2f9c-102">Entity Framework Core 도구 참조-Visual Studio에서 패키지 관리자 콘솔</span><span class="sxs-lookup"><span data-stu-id="d2f9c-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="d2f9c-103">Entity Framework Core에 대 한 패키지 관리자 콘솔 (PMC) 도구는 디자인 타임 개발 작업을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="d2f9c-104">만드는 예를 들어 [마이그레이션을](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations)마이그레이션을 적용 하 고 기존 데이터베이스를 기반으로 모델에 대 한 코드를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="d2f9c-105">명령을 사용 하 여 Visual Studio 내부에서 실행 합니다 [패키지 관리자 콘솔](/nuget/tools/package-manager-console)합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="d2f9c-106">이러한 도구는 .NET Core와 .NET Framework 프로젝트 모두에서 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="d2f9c-107">권장 하는 경우에 Visual Studio를 사용 하지 않는, 합니다 [EF Core 명령줄 도구](dotnet.md) 대신 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="d2f9c-108">CLI 도구는 크로스 플랫폼 및 명령 프롬프트 내에서 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="d2f9c-109">도구 설치</span><span class="sxs-lookup"><span data-stu-id="d2f9c-109">Installing the tools</span></span>

<span data-ttu-id="d2f9c-110">설치 및 업데이트 된 도구에 대 한 절차에서는 ASP.NET Core 2.1 이상 및 이전 버전 또는 기타 프로젝트 형식에 따라 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="d2f9c-111">ASP.NET Core 2.1 이상 버전</span><span class="sxs-lookup"><span data-stu-id="d2f9c-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="d2f9c-112">때문에 ASP.NET Core 2.1 이상 프로젝트에서 도구를 자동으로 포함 됩니다는 `Microsoft.EntityFrameworkCore.Tools` 패키지에 포함 됩니다 합니다 [Microsoft.AspNetCore.App 메타 패키지](/aspnet/core/fundamentals/metapackage-app)합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d2f9c-113">따라서 tools를 설치 하려면 아무 작업도 수행할 필요가 없습니다 있지만 필요가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>
* <span data-ttu-id="d2f9c-114">새 프로젝트의 도구를 사용 하기 전에 패키지를 복원 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="d2f9c-115">도구를 최신 버전으로 업데이트 하는 패키지를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="d2f9c-116">를 최신 버전의 도구를 보시기 했는지 확인 하려면 다음 단계를 수행할 수도 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="d2f9c-117">편집 하 *.csproj* 파일을 최신 버전을 지정 하는 줄을 추가 합니다 [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) 패키지 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="d2f9c-118">예를 들어, 합니다 *.csproj* 파일이 포함 될 수 있습니다는 `ItemGroup` 다음과 유사 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="d2f9c-119">다음 예제와 같이 메시지가 표시 되 면 도구를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="d2f9c-120">EF Core 도구 버전 '2.1.1-rtm-30846' 런타임 '2.1.3-rtm-32065' 보다 오래 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="d2f9c-121">최신 기능 및 버그 수정에 대 한 도구를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="d2f9c-122">도구를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-122">To update the tools:</span></span>
* <span data-ttu-id="d2f9c-123">최신.NET Core SDK를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="d2f9c-124">Visual Studio 최신 버전으로 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="d2f9c-125">편집 된 *.csproj* 앞에 표시 된 것 처럼 최신 도구 패키지에 대 한 패키지 참조를 포함 하도록 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="d2f9c-126">다른 버전 및 프로젝트 형식</span><span class="sxs-lookup"><span data-stu-id="d2f9c-126">Other versions and project types</span></span>

<span data-ttu-id="d2f9c-127">다음 명령을 실행 하 여 패키지 관리자 콘솔 도구를 설치 **패키지 관리자 콘솔**:</span><span class="sxs-lookup"><span data-stu-id="d2f9c-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="d2f9c-128">다음 명령을 실행 하 여 도구를 업데이트 **패키지 관리자 콘솔**합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="d2f9c-129">설치 확인</span><span class="sxs-lookup"><span data-stu-id="d2f9c-129">Verify the installation</span></span>

<span data-ttu-id="d2f9c-130">이 명령을 실행 하 여 도구 설치 되어 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="d2f9c-131">출력은 다음과 같습니다 (이 알 수 없는 버전을 사용 하는 도구):</span><span class="sxs-lookup"><span data-stu-id="d2f9c-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a><span data-ttu-id="d2f9c-132">도구를 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="d2f9c-132">Using the tools</span></span>

<span data-ttu-id="d2f9c-133">도구 사용 하기 전에:</span><span class="sxs-lookup"><span data-stu-id="d2f9c-133">Before using the tools:</span></span>
* <span data-ttu-id="d2f9c-134">대상 및 시작 프로젝트 간의 차이점을 이해 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="d2f9c-135">.NET Standard 클래스 라이브러리와 도구를 사용 하는 방법에 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="d2f9c-136">ASP.NET Core 프로젝트에 대 한 환경을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="d2f9c-137">대상 및 시작 프로젝트</span><span class="sxs-lookup"><span data-stu-id="d2f9c-137">Target and startup project</span></span>

<span data-ttu-id="d2f9c-138">명령 참조는 *프로젝트* 와 *시작 프로젝트*합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="d2f9c-139">합니다 *프로젝트* 이 라고도 합니다 *대상 프로젝트* 명령을 추가 하거나 파일을 제거할 위치 이기 때문에 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="d2f9c-140">기본적으로 **기본 프로젝트** 에서 선택한 **패키지 관리자 콘솔** 대상 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="d2f9c-141">사용 하 여 대상 프로젝트로 다른 프로젝트를 지정할 수 있습니다 합니다 <nobr> `--project` </nobr> 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="d2f9c-142">합니다 *시작 프로젝트* 도구 빌드 및 실행 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="d2f9c-143">도구는 데이터베이스 연결 문자열 및 모델의 구성 등 프로젝트에 대 한 정보를 가져오려면 디자인 타임에 응용 프로그램 코드를 실행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="d2f9c-144">기본적으로 **시작 프로젝트** 에 **솔루션 탐색기** 시작 프로젝트 인지 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="d2f9c-145">사용 하 여 시작 프로젝트로 다른 프로젝트를 지정할 수 있습니다 합니다 <nobr> `--startup-project` </nobr> 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="d2f9c-146">시작 프로젝트 및 대상 프로젝트에서 동일한 프로젝트 많습니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="d2f9c-147">일반적인 시나리오를 별도 프로젝트 되는 경우:</span><span class="sxs-lookup"><span data-stu-id="d2f9c-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="d2f9c-148">EF Core 컨텍스트 및 엔터티 클래스는.NET Core 클래스 라이브러리에는.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="d2f9c-149">.NET Core 콘솔 앱 또는 웹 앱을 클래스 라이브러리를 참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="d2f9c-150">수 이기도 [EF Core 컨텍스트를 별도 클래스 라이브러리에 마이그레이션 코드를 배치](xref:core/managing-schemas/migrations/projects)합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="d2f9c-151">다른 대상 프레임 워크</span><span class="sxs-lookup"><span data-stu-id="d2f9c-151">Other target frameworks</span></span>

<span data-ttu-id="d2f9c-152">패키지 관리자 콘솔 도구는.NET Core 또는.NET Framework 프로젝트를 사용 하 여 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="d2f9c-153">.NET Core 또는.NET Framework 프로젝트를.NET Standard 클래스 라이브러리는 EF Core 모델에 있는 앱 없을 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="d2f9c-154">예를 들어, Xamarin 및 유니버설 Windows 플랫폼 앱의 마찬가지입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="d2f9c-155">이러한 경우를 도구용 시작 프로젝트 역할을 위해서만 사용 되는.NET Core 또는.net 콘솔 앱 프로젝트를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="d2f9c-156">프로젝트는 실제 코드가 없는 임시 프로젝트 일 수 &mdash; 는 도구에 대 한 대상을 제공에 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="d2f9c-157">필요한 더미 프로젝트 이유는 무엇입니까?</span><span class="sxs-lookup"><span data-stu-id="d2f9c-157">Why is a dummy project required?</span></span> <span data-ttu-id="d2f9c-158">앞에서 설명한 대로 도구는 디자인 타임에 응용 프로그램 코드를 실행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="d2f9c-159">이렇게 하려면.NET Core 또는.NET Framework 런타임을 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="d2f9c-160">EF Core 모델 프로젝트를.NET Core 또는.NET Framework를 대상으로 하는 경우 EF Core 도구는 프로젝트에서 런타임에 차용 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="d2f9c-161">EF Core 모델은.NET Standard 클래스 라이브러리에 해당 하는 수행할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="d2f9c-162">.NET Standard는 실제.NET 구현이 아닙니다. 이.NET 구현에서 지원 해야 하는 Api 집합의 사양.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="d2f9c-163">따라서.NET Standard 부족 응용 프로그램 코드를 실행 하는 EF Core 도구에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="d2f9c-164">시작 프로젝트로 사용 하기 위해 만든 더미 프로젝트 도구는.NET Standard 클래스 라이브러리를 로드할 수 구체적인 대상 플랫폼을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="d2f9c-165">ASP.NET Core 환경</span><span class="sxs-lookup"><span data-stu-id="d2f9c-165">ASP.NET Core environment</span></span>

<span data-ttu-id="d2f9c-166">ASP.NET Core 프로젝트에 대 한 환경에 지정 하려면 **env:ASPNETCORE_ENVIRONMENT** 명령을 실행 하기 전에 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="d2f9c-167">일반 매개 변수</span><span class="sxs-lookup"><span data-stu-id="d2f9c-167">Common parameters</span></span>

<span data-ttu-id="d2f9c-168">다음 표에서 EF Core 명령은 모두에 공통 되는 매개 변수를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="d2f9c-169">매개 변수</span><span class="sxs-lookup"><span data-stu-id="d2f9c-169">Parameter</span></span>                 | <span data-ttu-id="d2f9c-170">설명</span><span class="sxs-lookup"><span data-stu-id="d2f9c-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d2f9c-171">상황에 맞는 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="d2f9c-171">-Context \<String></span></span>        | <span data-ttu-id="d2f9c-172">`DbContext` 클래스를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-172">The `DbContext` class to use.</span></span> <span data-ttu-id="d2f9c-173">유일한 또는 정규화 된 네임 스페이스의 클래스 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="d2f9c-174">이 매개 변수를 생략 하면 EF Core 컨텍스트 클래스를 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="d2f9c-175">여러 컨텍스트 클래스의 경우이 매개 변수는 필수입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="d2f9c-176">-프로젝트 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="d2f9c-176">-Project \<String></span></span>        | <span data-ttu-id="d2f9c-177">대상 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-177">The target project.</span></span> <span data-ttu-id="d2f9c-178">이 매개 변수를 생략 합니다 **기본 프로젝트** 에 대 한 **패키지 관리자 콘솔** 대상 프로젝트로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="d2f9c-179">-StartupProject \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="d2f9c-179">-StartupProject \<String></span></span> | <span data-ttu-id="d2f9c-180">시작 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-180">The startup project.</span></span> <span data-ttu-id="d2f9c-181">이 매개 변수를 생략 합니다 **시작 프로젝트** 에 **솔루션 속성** 대상 프로젝트로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="d2f9c-182">-Verbose</span><span class="sxs-lookup"><span data-stu-id="d2f9c-182">-Verbose</span></span>                  | <span data-ttu-id="d2f9c-183">자세한 정보 출력을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="d2f9c-184">명령에 대 한 도움말 정보를 표시 하려면 PowerShell의 사용 하 여 `Get-Help` 명령입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="d2f9c-185">상황에 맞는, 프로젝트 및 StartupProject 매개 변수 탭 확장을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="d2f9c-186">추가 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="d2f9c-186">Add-Migration</span></span>

<span data-ttu-id="d2f9c-187">새 마이그레이션을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-187">Adds a new migration.</span></span>

<span data-ttu-id="d2f9c-188">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="d2f9c-188">Parameters:</span></span>

| <span data-ttu-id="d2f9c-189">매개 변수</span><span class="sxs-lookup"><span data-stu-id="d2f9c-189">Parameter</span></span>                         | <span data-ttu-id="d2f9c-190">설명</span><span class="sxs-lookup"><span data-stu-id="d2f9c-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d2f9c-191"><nobr>-Name \<문자열 ><nobr></span><span class="sxs-lookup"><span data-stu-id="d2f9c-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="d2f9c-192">마이그레이션 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-192">The name of the migration.</span></span> <span data-ttu-id="d2f9c-193">위치 매개 변수 이며 필수입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="d2f9c-194"><nobr>-OutputDir \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="d2f9c-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="d2f9c-195">사용 하 여 디렉터리 (및 하위 네임 스페이스).</span><span class="sxs-lookup"><span data-stu-id="d2f9c-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="d2f9c-196">대상 프로젝트 디렉터리를 기준으로 경로입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="d2f9c-197">기본값은 "마이그레이션"입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="d2f9c-198">데이터베이스 삭제</span><span class="sxs-lookup"><span data-stu-id="d2f9c-198">Drop-Database</span></span>

<span data-ttu-id="d2f9c-199">데이터베이스를 삭제합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-199">Drops the database.</span></span>

<span data-ttu-id="d2f9c-200">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="d2f9c-200">Parameters:</span></span>

| <span data-ttu-id="d2f9c-201">매개 변수</span><span class="sxs-lookup"><span data-stu-id="d2f9c-201">Parameter</span></span> | <span data-ttu-id="d2f9c-202">설명</span><span class="sxs-lookup"><span data-stu-id="d2f9c-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="d2f9c-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="d2f9c-203">-WhatIf</span></span>   | <span data-ttu-id="d2f9c-204">데이터베이스는 삭제할 수 있지만 삭제 하지는 마세요 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="d2f9c-205">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="d2f9c-205">Get-DbContext</span></span>

<span data-ttu-id="d2f9c-206">사용 가능한 목록 `DbContext` 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-206">Lists available `DbContext` types.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="d2f9c-207">마이그레이션 제거</span><span class="sxs-lookup"><span data-stu-id="d2f9c-207">Remove-Migration</span></span>

<span data-ttu-id="d2f9c-208">마지막으로 마이그레이션 (마이그레이션에 대해 수행 된 코드 변경 내용을 롤백)를 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="d2f9c-209">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="d2f9c-209">Parameters:</span></span>

| <span data-ttu-id="d2f9c-210">매개 변수</span><span class="sxs-lookup"><span data-stu-id="d2f9c-210">Parameter</span></span> | <span data-ttu-id="d2f9c-211">설명</span><span class="sxs-lookup"><span data-stu-id="d2f9c-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="d2f9c-212">-Force</span><span class="sxs-lookup"><span data-stu-id="d2f9c-212">-Force</span></span>    | <span data-ttu-id="d2f9c-213">마이그레이션 되돌리기 (데이터베이스에 적용 된 변경 내용 롤백).</span><span class="sxs-lookup"><span data-stu-id="d2f9c-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="d2f9c-214">DbContext 스 캐 폴드</span><span class="sxs-lookup"><span data-stu-id="d2f9c-214">Scaffold-DbContext</span></span>

<span data-ttu-id="d2f9c-215">에 대 한 코드 생성을 `DbContext` 및 데이터베이스에 대 한 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-215">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="d2f9c-216">에 대 한 순서로 `Scaffold-DbContext` 엔터티 형식으로 생성 하려면 데이터베이스 테이블에는 기본 키가 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-216">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="d2f9c-217">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="d2f9c-217">Parameters:</span></span>

| <span data-ttu-id="d2f9c-218">매개 변수</span><span class="sxs-lookup"><span data-stu-id="d2f9c-218">Parameter</span></span>                          | <span data-ttu-id="d2f9c-219">설명</span><span class="sxs-lookup"><span data-stu-id="d2f9c-219">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d2f9c-220"><nobr>연결 \<문자열 ></nobr></span><span class="sxs-lookup"><span data-stu-id="d2f9c-220"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="d2f9c-221">데이터베이스에 연결 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-221">The connection string to the database.</span></span> <span data-ttu-id="d2f9c-222">ASP.NET Core 2.x 프로젝트에 대 한 값이 될 수 있습니다 *이름 =\<연결 문자열의 이름 >* 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-222">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="d2f9c-223">이 경우 이름을 프로젝트에 대해 설정 된 구성 소스에서 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-223">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="d2f9c-224">위치 매개 변수 이며 필수입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-224">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="d2f9c-225"><nobr>공급자 \<문자열 ></nobr></span><span class="sxs-lookup"><span data-stu-id="d2f9c-225"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="d2f9c-226">공급자에서 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-226">The provider to use.</span></span> <span data-ttu-id="d2f9c-227">일반적으로이 NuGet 패키지의 이름 예: `Microsoft.EntityFrameworkCore.SqlServer`합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-227">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="d2f9c-228">위치 매개 변수 이며 필수입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-228">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="d2f9c-229">-OutputDir \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="d2f9c-229">-OutputDir \<String></span></span>               | <span data-ttu-id="d2f9c-230">디렉터리에 파일 보관입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-230">The directory to put files in.</span></span> <span data-ttu-id="d2f9c-231">프로젝트 디렉터리를 기준으로 경로입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-231">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="d2f9c-232">-ContextDir \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="d2f9c-232">-ContextDir \<String></span></span>              | <span data-ttu-id="d2f9c-233">적용할 디렉터리는 `DbContext` 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-233">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="d2f9c-234">프로젝트 디렉터리를 기준으로 경로입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="d2f9c-235">상황에 맞는 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="d2f9c-235">-Context \<String></span></span>                 | <span data-ttu-id="d2f9c-236">이름을 합니다 `DbContext` 클래스를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-236">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="d2f9c-237">-스키마 \<String ></span><span class="sxs-lookup"><span data-stu-id="d2f9c-237">-Schemas \<String[]></span></span>               | <span data-ttu-id="d2f9c-238">에 대 한 엔터티 형식을 생성 하는 테이블의 스키마입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-238">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="d2f9c-239">이 매개 변수를 생략 하면 모든 스키마 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-239">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="d2f9c-240">-테이블 \<String ></span><span class="sxs-lookup"><span data-stu-id="d2f9c-240">-Tables \<String[]></span></span>                | <span data-ttu-id="d2f9c-241">에 대 한 엔터티 형식을 생성 하는 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-241">The tables to generate entity types for.</span></span> <span data-ttu-id="d2f9c-242">이 매개 변수를 생략 하면 테이블을 모두 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-242">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="d2f9c-243">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="d2f9c-243">-DataAnnotations</span></span>                   | <span data-ttu-id="d2f9c-244">(가능한 한) 경우 모델을 구성 하려면 특성을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-244">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="d2f9c-245">이 매개 변수를 생략 하면 fluent API만 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-245">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="d2f9c-246">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="d2f9c-246">-UseDatabaseNames</span></span>                  | <span data-ttu-id="d2f9c-247">데이터베이스에 표시 된 대로 테이블 및 열 이름을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-247">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="d2f9c-248">이 매개 변수를 생략 하면 더욱 긴밀 하 게 C# 이름 스타일 규칙에 맞게 데이터베이스 이름이 변경 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-248">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="d2f9c-249">-Force</span><span class="sxs-lookup"><span data-stu-id="d2f9c-249">-Force</span></span>                             | <span data-ttu-id="d2f9c-250">기존 파일을 덮어씁니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-250">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="d2f9c-251">예제:</span><span class="sxs-lookup"><span data-stu-id="d2f9c-251">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="d2f9c-252">지정한 이름 가진 별도 폴더에 컨텍스트를 만들고 선택한 테이블만 스 캐 폴딩 하는 예제:</span><span class="sxs-lookup"><span data-stu-id="d2f9c-252">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="d2f9c-253">스크립트 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="d2f9c-253">Script-Migration</span></span>

<span data-ttu-id="d2f9c-254">에 적용 되는 모든 변경 내용을 하나 선택한 마이그레이션에서 선택한 마이그레이션 다른 SQL 스크립트를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-254">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="d2f9c-255">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="d2f9c-255">Parameters:</span></span>

| <span data-ttu-id="d2f9c-256">매개 변수</span><span class="sxs-lookup"><span data-stu-id="d2f9c-256">Parameter</span></span>                | <span data-ttu-id="d2f9c-257">설명</span><span class="sxs-lookup"><span data-stu-id="d2f9c-257">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d2f9c-258"> \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="d2f9c-258">*-From* \<String></span></span>        | <span data-ttu-id="d2f9c-259">마이그레이션을 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-259">The starting migration.</span></span> <span data-ttu-id="d2f9c-260">마이그레이션은 식별 이름 또는 id입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-260">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="d2f9c-261">숫자 0는 특별 한 의미 *첫 번째 마이그레이션 전에*입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-261">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="d2f9c-262">기본값은 0입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-262">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="d2f9c-263">*-에* \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="d2f9c-263">*-To* \<String></span></span>          | <span data-ttu-id="d2f9c-264">끝 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-264">The ending migration.</span></span> <span data-ttu-id="d2f9c-265">기본값 마지막 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-265">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="d2f9c-266"><nobr>멱 등</nobr></span><span class="sxs-lookup"><span data-stu-id="d2f9c-266"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="d2f9c-267">모든 마이그레이션 데이터베이스에서 사용할 수 있는 스크립트를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-267">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="d2f9c-268">-출력 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="d2f9c-268">-Output \<String></span></span>        | <span data-ttu-id="d2f9c-269">결과를 쓸 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-269">The file to write the result to.</span></span> <span data-ttu-id="d2f9c-270">예를 들어 앱의 런타임 파일이 생성 될 때 파일이 동일한 폴더에 생성 된 이름으로 만들어집니다이 매개 변수를 생략 합니다. */obj/Debug/netcoreapp2.1/ghbkztfz.sql/* 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-270">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="d2f9c-271">및 출력 매개 변수 탭 확장을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-271">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="d2f9c-272">다음 예제에서는 마이그레이션 이름을 사용 하 여 InitialCreate 마이그레이션에 대 한 스크립트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-272">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="d2f9c-273">다음 예제에서는 마이그레이션 ID를 사용 하 여 InitialCreate 마이그레이션 후 모든 마이그레이션에 대 한 스크립트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-273">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="d2f9c-274">데이터베이스 업데이트</span><span class="sxs-lookup"><span data-stu-id="d2f9c-274">Update-Database</span></span>

<span data-ttu-id="d2f9c-275">지정 된 마이그레이션 또는 마지막 마이그레이션을 위해 데이터베이스를 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-275">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="d2f9c-276">매개 변수</span><span class="sxs-lookup"><span data-stu-id="d2f9c-276">Parameter</span></span>                           | <span data-ttu-id="d2f9c-277">설명</span><span class="sxs-lookup"><span data-stu-id="d2f9c-277">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d2f9c-278"><nobr>*마이그레이션* \<문자열 ></nobr></span><span class="sxs-lookup"><span data-stu-id="d2f9c-278"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="d2f9c-279">대상 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-279">The target migration.</span></span> <span data-ttu-id="d2f9c-280">마이그레이션은 식별 이름 또는 id입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-280">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="d2f9c-281">숫자 0는 특별 한 의미 *첫 번째 마이그레이션을 시작 하기 전에* 하 고 모든 마이그레이션에 되돌릴 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-281">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="d2f9c-282">마이그레이션 없음이 지정 된 경우 명령을 기본값은 마지막 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-282">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="d2f9c-283">마이그레이션 매개 변수 탭 확장을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-283">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="d2f9c-284">다음 예제에서는 모든 마이그레이션 되돌립니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-284">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="d2f9c-285">다음 예제에서는 지정 된 마이그레이션에 데이터베이스를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-285">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="d2f9c-286">첫 번째 마이그레이션 이름을 사용 하 고 마이그레이션 ID를 사용 하 여 두 번째 키를 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="d2f9c-286">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a><span data-ttu-id="d2f9c-287">추가 자료</span><span class="sxs-lookup"><span data-stu-id="d2f9c-287">Additional resources</span></span>

* [<span data-ttu-id="d2f9c-288">마이그레이션</span><span class="sxs-lookup"><span data-stu-id="d2f9c-288">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="d2f9c-289">리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="d2f9c-289">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
