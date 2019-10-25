---
title: EF Core 도구 참조 (패키지 관리자 콘솔)-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: a9ce6d5b5f36a72e3715a9de787f1f00e989a58c
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811907"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="9d4bd-102">Entity Framework Core 도구 참조-Visual Studio의 패키지 관리자 콘솔</span><span class="sxs-lookup"><span data-stu-id="9d4bd-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="9d4bd-103">Entity Framework Core 용 패키지 관리자 콘솔 (PMC) 도구는 디자인 타임 개발 작업을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="9d4bd-104">예를 들어, [마이그레이션을](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0)만들고, 마이그레이션을 적용 하 고, 기존 데이터베이스를 기반으로 하는 모델에 대 한 코드를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="9d4bd-105">명령은 [패키지 관리자 콘솔](/nuget/tools/package-manager-console)을 사용 하 여 Visual Studio 내부에서 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="9d4bd-106">이러한 도구는 .NET Core와 .NET Framework 프로젝트 모두에서 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="9d4bd-107">Visual Studio를 사용 하지 않는 경우 [EF Core 명령줄 도구](dotnet.md) 를 대신 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="9d4bd-108">CLI 도구는 플랫폼 간 이며 명령 프롬프트 내에서 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="9d4bd-109">도구 설치</span><span class="sxs-lookup"><span data-stu-id="9d4bd-109">Installing the tools</span></span>

<span data-ttu-id="9d4bd-110">도구를 설치 하 고 업데이트 하는 절차는 ASP.NET Core 2.1 이상 버전과 이전 버전 또는 다른 프로젝트 형식 간에 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="9d4bd-111">ASP.NET Core 버전 2.1 이상</span><span class="sxs-lookup"><span data-stu-id="9d4bd-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="9d4bd-112">`Microsoft.EntityFrameworkCore.Tools` 패키지가 [AspNetCore 메타 패키지](/aspnet/core/fundamentals/metapackage-app)에 포함 되어 있으므로 도구는 ASP.NET Core 2.1 이상 프로젝트에 자동으로 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="9d4bd-113">따라서 도구를 설치 하기 위해 아무것도 수행할 필요가 없지만 다음을 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>

* <span data-ttu-id="9d4bd-114">새 프로젝트에서 도구를 사용 하기 전에 패키지를 복원 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="9d4bd-115">패키지를 설치 하 여 최신 버전으로 도구를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="9d4bd-116">최신 버전의 도구를 사용 하 고 있는지 확인 하려면 다음 단계를 수행 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="9d4bd-117">*.Csproj* 파일을 편집 하 고 최신 버전의 [microsoft.entityframeworkcore.tools.dotnet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) 패키지를 지정 하는 줄을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="9d4bd-118">예를 들어 *.csproj* 파일에는 다음과 같은 `ItemGroup` 포함 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="9d4bd-119">다음 예제와 같은 메시지가 표시 되 면 도구를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="9d4bd-120">EF Core 도구 버전 ' 2.1.1-30846 '은 (는) 런타임 ' 2.1.3-32065 ' 보다 이전 버전입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="9d4bd-121">최신 기능 및 버그 수정에 대 한 도구를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="9d4bd-122">도구를 업데이트 하려면:</span><span class="sxs-lookup"><span data-stu-id="9d4bd-122">To update the tools:</span></span>

* <span data-ttu-id="9d4bd-123">최신 .NET Core SDK를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="9d4bd-124">Visual Studio를 최신 버전으로 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="9d4bd-125">위와 같이 최신 도구 패키지에 대 한 패키지 참조를 포함 하도록 *.csproj* 파일을 편집 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="9d4bd-126">기타 버전 및 프로젝트 형식</span><span class="sxs-lookup"><span data-stu-id="9d4bd-126">Other versions and project types</span></span>

<span data-ttu-id="9d4bd-127">**패키지 관리자**콘솔에서 다음 명령을 실행 하 여 패키지 관리자 콘솔 도구를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="9d4bd-128">**패키지 관리자 콘솔**에서 다음 명령을 실행 하 여 도구를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="9d4bd-129">설치 확인</span><span class="sxs-lookup"><span data-stu-id="9d4bd-129">Verify the installation</span></span>

<span data-ttu-id="9d4bd-130">다음 명령을 실행 하 여 도구가 설치 되어 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="9d4bd-131">출력은 다음과 같이 표시 됩니다. 사용 중인 도구의 버전을 알 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

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

## <a name="using-the-tools"></a><span data-ttu-id="9d4bd-132">도구 사용</span><span class="sxs-lookup"><span data-stu-id="9d4bd-132">Using the tools</span></span>

<span data-ttu-id="9d4bd-133">도구를 사용 하기 전에:</span><span class="sxs-lookup"><span data-stu-id="9d4bd-133">Before using the tools:</span></span>

* <span data-ttu-id="9d4bd-134">대상과 시작 프로젝트의 차이점을 이해 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="9d4bd-135">.NET Standard 클래스 라이브러리와 함께 도구를 사용 하는 방법을 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="9d4bd-136">ASP.NET Core 프로젝트의 경우 환경을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="9d4bd-137">대상 및 시작 프로젝트</span><span class="sxs-lookup"><span data-stu-id="9d4bd-137">Target and startup project</span></span>

<span data-ttu-id="9d4bd-138">명령은 *프로젝트* 와 *시작 프로젝트*를 참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="9d4bd-139">*프로젝트* 는 명령이 파일을 추가 하거나 제거 하는 위치 이기 때문에 *대상 프로젝트가* 라고도 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="9d4bd-140">기본적으로 **패키지 관리자 콘솔** 에서 선택한 **기본 프로젝트** 는 대상 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="9d4bd-141"><nobr>`--project`</nobr> 옵션을 사용 하 여 다른 프로젝트를 대상 프로젝트로 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="9d4bd-142">*시작 프로젝트* 는 도구가 빌드하고 실행 하는 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="9d4bd-143">도구는 디자인 타임에 응용 프로그램 코드를 실행 하 여 데이터베이스 연결 문자열 및 모델 구성과 같은 프로젝트에 대 한 정보를 가져와야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="9d4bd-144">기본적으로 **솔루션 탐색기** 의 **시작 프로젝트** 는 시작 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="9d4bd-145"><nobr>`--startup-project`</nobr> 옵션을 사용 하 여 다른 프로젝트를 시작 프로젝트로 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="9d4bd-146">시작 프로젝트와 대상 프로젝트는 대개 동일한 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="9d4bd-147">개별 프로젝트의 일반적인 시나리오는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="9d4bd-148">EF Core 컨텍스트 및 엔터티 클래스는 .NET Core 클래스 라이브러리에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="9d4bd-149">.NET Core 콘솔 앱 또는 웹 앱은 클래스 라이브러리를 참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="9d4bd-150">또한 [EF Core 컨텍스트와 별도의 클래스 라이브러리에 마이그레이션 코드](xref:core/managing-schemas/migrations/projects)를 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="9d4bd-151">기타 대상 프레임 워크</span><span class="sxs-lookup"><span data-stu-id="9d4bd-151">Other target frameworks</span></span>

<span data-ttu-id="9d4bd-152">패키지 관리자 콘솔 도구는 .NET Core 또는 .NET Framework 프로젝트에서 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="9d4bd-153">.NET Standard 클래스 라이브러리에 EF Core 모델을 포함 하는 앱에는 .NET Core 또는 .NET Framework 프로젝트가 없을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="9d4bd-154">예를 들어이는 Xamarin 및 유니버설 Windows 플랫폼 apps에서 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="9d4bd-155">이러한 경우 도구에 대 한 시작 프로젝트 역할을 하는 용도로만 사용할 수 있는 .NET Core 또는 .NET Framework 콘솔 앱 프로젝트를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="9d4bd-156">프로젝트는 실제 코드가 없는 더미 프로젝트인 경우 도구에 대 한 대상을 제공 하기만 하면 됩니다 &mdash;.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="9d4bd-157">더미 프로젝트가 필요한 이유는 무엇 인가요?</span><span class="sxs-lookup"><span data-stu-id="9d4bd-157">Why is a dummy project required?</span></span> <span data-ttu-id="9d4bd-158">앞에서 설명한 대로 도구는 디자인 타임에 응용 프로그램 코드를 실행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="9d4bd-159">이렇게 하려면 .NET Core 또는 .NET Framework 런타임을 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="9d4bd-160">EF Core 모델이 .NET Core 또는 .NET Framework을 대상으로 하는 프로젝트에 있는 경우 EF Core 도구는 프로젝트에서 런타임을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="9d4bd-161">EF Core 모델이 .NET Standard 클래스 라이브러리에 있는 경우에는이 작업을 수행할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="9d4bd-162">.NET Standard은 실제 .NET 구현이 아닙니다. .NET 구현에서 지원 해야 하는 Api 집합의 사양입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="9d4bd-163">따라서 .NET Standard EF Core 도구를 통해 응용 프로그램 코드를 실행할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="9d4bd-164">시작 프로젝트로 사용 하기 위해 만드는 더미 프로젝트는 도구가 .NET Standard 클래스 라이브러리를 로드 하는 데 사용할 수 있는 구체적인 대상 플랫폼을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="9d4bd-165">ASP.NET Core 환경</span><span class="sxs-lookup"><span data-stu-id="9d4bd-165">ASP.NET Core environment</span></span>

<span data-ttu-id="9d4bd-166">ASP.NET Core 프로젝트에 대 한 환경을 지정 하려면 명령을 실행 하기 전에 **env: ASPNETCORE_ENVIRONMENT** 를 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="9d4bd-167">일반 매개 변수</span><span class="sxs-lookup"><span data-stu-id="9d4bd-167">Common parameters</span></span>

<span data-ttu-id="9d4bd-168">다음 표에서는 모든 EF Core 명령에 공통적인 매개 변수를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="9d4bd-169">매개 변수</span><span class="sxs-lookup"><span data-stu-id="9d4bd-169">Parameter</span></span>                 | <span data-ttu-id="9d4bd-170">설명</span><span class="sxs-lookup"><span data-stu-id="9d4bd-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9d4bd-171">-Context \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="9d4bd-171">-Context \<String></span></span>        | <span data-ttu-id="9d4bd-172">사용할 `DbContext` 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-172">The `DbContext` class to use.</span></span> <span data-ttu-id="9d4bd-173">네임 스페이스를 포함 하거나 정규화 된 클래스 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="9d4bd-174">이 매개 변수를 생략 하면 EF Core 컨텍스트 클래스를 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="9d4bd-175">컨텍스트 클래스가 여러 개인 경우이 매개 변수는 필수입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="9d4bd-176">-Project \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="9d4bd-176">-Project \<String></span></span>        | <span data-ttu-id="9d4bd-177">대상 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-177">The target project.</span></span> <span data-ttu-id="9d4bd-178">이 매개 변수를 생략 하면 **패키지 관리자 콘솔** 의 **기본 프로젝트가** 대상 프로젝트로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="9d4bd-179">-StartupProject \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="9d4bd-179">-StartupProject \<String></span></span> | <span data-ttu-id="9d4bd-180">시작 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-180">The startup project.</span></span> <span data-ttu-id="9d4bd-181">이 매개 변수를 생략 하면 **솔루션 속성** 의 **시작 프로젝트가** 대상 프로젝트로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="9d4bd-182">-자세한 정보</span><span class="sxs-lookup"><span data-stu-id="9d4bd-182">-Verbose</span></span>                  | <span data-ttu-id="9d4bd-183">자세한 정보 출력을 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="9d4bd-184">명령에 대 한 도움말 정보를 표시 하려면 PowerShell의 `Get-Help` 명령을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="9d4bd-185">Context, Project 및 StartupProject 매개 변수는 탭 확장을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="9d4bd-186">추가-마이그레이션</span><span class="sxs-lookup"><span data-stu-id="9d4bd-186">Add-Migration</span></span>

<span data-ttu-id="9d4bd-187">새 마이그레이션을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-187">Adds a new migration.</span></span>

<span data-ttu-id="9d4bd-188">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="9d4bd-188">Parameters:</span></span>

| <span data-ttu-id="9d4bd-189">매개 변수</span><span class="sxs-lookup"><span data-stu-id="9d4bd-189">Parameter</span></span>                         | <span data-ttu-id="9d4bd-190">설명</span><span class="sxs-lookup"><span data-stu-id="9d4bd-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9d4bd-191"><nobr>-Name \<문자열 ><nobr></span><span class="sxs-lookup"><span data-stu-id="9d4bd-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="9d4bd-192">마이그레이션의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-192">The name of the migration.</span></span> <span data-ttu-id="9d4bd-193">이 매개 변수는 위치 매개 변수 이며 필수입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="9d4bd-194"><nobr>-OutputDir \<문자열 ></nobr></span><span class="sxs-lookup"><span data-stu-id="9d4bd-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="9d4bd-195">사용할 디렉터리 및 하위 네임 스페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="9d4bd-196">경로는 대상 프로젝트 디렉터리를 기준으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="9d4bd-197">기본값은 "migration"입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="9d4bd-198">Drop Database</span><span class="sxs-lookup"><span data-stu-id="9d4bd-198">Drop-Database</span></span>

<span data-ttu-id="9d4bd-199">데이터베이스를 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-199">Drops the database.</span></span>

<span data-ttu-id="9d4bd-200">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="9d4bd-200">Parameters:</span></span>

| <span data-ttu-id="9d4bd-201">매개 변수</span><span class="sxs-lookup"><span data-stu-id="9d4bd-201">Parameter</span></span> | <span data-ttu-id="9d4bd-202">설명</span><span class="sxs-lookup"><span data-stu-id="9d4bd-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="9d4bd-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="9d4bd-203">-WhatIf</span></span>   | <span data-ttu-id="9d4bd-204">삭제할 데이터베이스를 표시 하지만 삭제할 데이터베이스를 표시 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="9d4bd-205">DbContext</span><span class="sxs-lookup"><span data-stu-id="9d4bd-205">Get-DbContext</span></span>

<span data-ttu-id="9d4bd-206">`DbContext` 형식에 대 한 정보를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-206">Gets information about a `DbContext` type.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="9d4bd-207">마이그레이션 제거</span><span class="sxs-lookup"><span data-stu-id="9d4bd-207">Remove-Migration</span></span>

<span data-ttu-id="9d4bd-208">마이그레이션에 대해 수행 된 코드 변경 내용을 롤백하는 마지막 마이그레이션을 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="9d4bd-209">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="9d4bd-209">Parameters:</span></span>

| <span data-ttu-id="9d4bd-210">매개 변수</span><span class="sxs-lookup"><span data-stu-id="9d4bd-210">Parameter</span></span> | <span data-ttu-id="9d4bd-211">설명</span><span class="sxs-lookup"><span data-stu-id="9d4bd-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="9d4bd-212">-Force</span><span class="sxs-lookup"><span data-stu-id="9d4bd-212">-Force</span></span>    | <span data-ttu-id="9d4bd-213">마이그레이션을 되돌립니다 (데이터베이스에 적용 된 변경 내용 롤백).</span><span class="sxs-lookup"><span data-stu-id="9d4bd-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="9d4bd-214">스 캐 폴드-DbContext</span><span class="sxs-lookup"><span data-stu-id="9d4bd-214">Scaffold-DbContext</span></span>

<span data-ttu-id="9d4bd-215">데이터베이스에 대 한 `DbContext` 및 엔터티 형식에 대 한 코드를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-215">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="9d4bd-216">`Scaffold-DbContext`에서 엔터티 형식을 생성 하려면 데이터베이스 테이블에 기본 키가 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-216">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="9d4bd-217">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="9d4bd-217">Parameters:</span></span>

| <span data-ttu-id="9d4bd-218">매개 변수</span><span class="sxs-lookup"><span data-stu-id="9d4bd-218">Parameter</span></span>                          | <span data-ttu-id="9d4bd-219">설명</span><span class="sxs-lookup"><span data-stu-id="9d4bd-219">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9d4bd-220"><nobr>-연결 \<문자열 ></nobr></span><span class="sxs-lookup"><span data-stu-id="9d4bd-220"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="9d4bd-221">데이터베이스에 대 한 연결 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-221">The connection string to the database.</span></span> <span data-ttu-id="9d4bd-222">ASP.NET Core 2.x 프로젝트의 경우 값은 *이름 =\<연결 문자열 > 이름일*수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-222">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="9d4bd-223">이 경우 프로젝트에 대해 설정 된 구성 소스에서 이름이 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-223">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="9d4bd-224">이 매개 변수는 위치 매개 변수 이며 필수입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-224">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="9d4bd-225"><nobr>-공급자 \<문자열 ></nobr></span><span class="sxs-lookup"><span data-stu-id="9d4bd-225"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="9d4bd-226">사용할 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-226">The provider to use.</span></span> <span data-ttu-id="9d4bd-227">일반적으로 NuGet 패키지의 이름입니다 (예: `Microsoft.EntityFrameworkCore.SqlServer`).</span><span class="sxs-lookup"><span data-stu-id="9d4bd-227">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="9d4bd-228">이 매개 변수는 위치 매개 변수 이며 필수입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-228">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="9d4bd-229">-OutputDir \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="9d4bd-229">-OutputDir \<String></span></span>               | <span data-ttu-id="9d4bd-230">파일을 저장할 디렉터리입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-230">The directory to put files in.</span></span> <span data-ttu-id="9d4bd-231">경로는 프로젝트 디렉터리에 상대적입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-231">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="9d4bd-232">-ContextDir \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="9d4bd-232">-ContextDir \<String></span></span>              | <span data-ttu-id="9d4bd-233">`DbContext` 파일을 저장할 디렉터리입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-233">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="9d4bd-234">경로는 프로젝트 디렉터리에 상대적입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="9d4bd-235">-Context \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="9d4bd-235">-Context \<String></span></span>                 | <span data-ttu-id="9d4bd-236">생성할 `DbContext` 클래스의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-236">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="9d4bd-237">-스키마 \<String [] ></span><span class="sxs-lookup"><span data-stu-id="9d4bd-237">-Schemas \<String[]></span></span>               | <span data-ttu-id="9d4bd-238">엔터티 형식을 생성할 테이블의 스키마입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-238">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="9d4bd-239">이 매개 변수를 생략 하면 모든 스키마가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-239">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="9d4bd-240">-Tables \<String [] ></span><span class="sxs-lookup"><span data-stu-id="9d4bd-240">-Tables \<String[]></span></span>                | <span data-ttu-id="9d4bd-241">엔터티 형식을 생성할 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-241">The tables to generate entity types for.</span></span> <span data-ttu-id="9d4bd-242">이 매개 변수를 생략 하면 모든 테이블이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-242">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="9d4bd-243">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="9d4bd-243">-DataAnnotations</span></span>                   | <span data-ttu-id="9d4bd-244">특성을 사용 하 여 모델을 구성 합니다 (가능한 경우).</span><span class="sxs-lookup"><span data-stu-id="9d4bd-244">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="9d4bd-245">이 매개 변수를 생략 하면 흐름 API만 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-245">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="9d4bd-246">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="9d4bd-246">-UseDatabaseNames</span></span>                  | <span data-ttu-id="9d4bd-247">테이블 및 열 이름은 데이터베이스에 표시 된 대로 정확 하 게 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-247">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="9d4bd-248">이 매개 변수를 생략 하는 경우 데이터베이스 이름이 이름 스타일 규칙을 C# 더욱 잘 준수 하도록 변경 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-248">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="9d4bd-249">-Force</span><span class="sxs-lookup"><span data-stu-id="9d4bd-249">-Force</span></span>                             | <span data-ttu-id="9d4bd-250">기존 파일을 덮어씁니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-250">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="9d4bd-251">예제:</span><span class="sxs-lookup"><span data-stu-id="9d4bd-251">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="9d4bd-252">예를 들어 선택한 테이블만 스 캐 폴드 지정 된 이름의 개별 폴더에 컨텍스트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-252">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="9d4bd-253">스크립트-마이그레이션</span><span class="sxs-lookup"><span data-stu-id="9d4bd-253">Script-Migration</span></span>

<span data-ttu-id="9d4bd-254">선택한 마이그레이션의 모든 변경 내용을 선택한 다른 마이그레이션에 적용 하는 SQL 스크립트를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-254">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="9d4bd-255">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="9d4bd-255">Parameters:</span></span>

| <span data-ttu-id="9d4bd-256">매개 변수</span><span class="sxs-lookup"><span data-stu-id="9d4bd-256">Parameter</span></span>                | <span data-ttu-id="9d4bd-257">설명</span><span class="sxs-lookup"><span data-stu-id="9d4bd-257">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9d4bd-258">*-* \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="9d4bd-258">*-From* \<String></span></span>        | <span data-ttu-id="9d4bd-259">마이그레이션을 시작 하는 중입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-259">The starting migration.</span></span> <span data-ttu-id="9d4bd-260">마이그레이션은 이름 또는 ID로 식별할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-260">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="9d4bd-261">숫자 0은 *첫 번째 마이그레이션 이전*을 의미 하는 특수 한 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-261">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="9d4bd-262">기본값은 0입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-262">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="9d4bd-263">*-* \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="9d4bd-263">*-To* \<String></span></span>          | <span data-ttu-id="9d4bd-264">종료 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-264">The ending migration.</span></span> <span data-ttu-id="9d4bd-265">마지막 마이그레이션에 대 한 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-265">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="9d4bd-266"><nobr>-Idempotent</nobr></span><span class="sxs-lookup"><span data-stu-id="9d4bd-266"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="9d4bd-267">마이그레이션할 때 데이터베이스에서 사용할 수 있는 스크립트를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-267">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="9d4bd-268">-출력 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="9d4bd-268">-Output \<String></span></span>        | <span data-ttu-id="9d4bd-269">결과를 쓸 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-269">The file to write the result to.</span></span> <span data-ttu-id="9d4bd-270">이 매개 변수를 생략 하면 응용 프로그램의 런타임 파일이 생성 되는 폴더와 동일한 폴더에 생성 된 이름으로 파일이 생성 됩니다 (예: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/* ).</span><span class="sxs-lookup"><span data-stu-id="9d4bd-270">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="9d4bd-271">To, From 및 Output 매개 변수는 탭 확장을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-271">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="9d4bd-272">다음 예에서는 마이그레이션 이름을 사용 하 여 InitialCreate migration에 대 한 스크립트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-272">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="9d4bd-273">다음 예에서는 마이그레이션 ID를 사용 하 여 InitialCreate migration 이후의 모든 마이그레이션에 대 한 스크립트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-273">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="9d4bd-274">업데이트-데이터베이스</span><span class="sxs-lookup"><span data-stu-id="9d4bd-274">Update-Database</span></span>

<span data-ttu-id="9d4bd-275">데이터베이스를 마지막 마이그레이션 또는 지정 된 마이그레이션으로 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-275">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="9d4bd-276">매개 변수</span><span class="sxs-lookup"><span data-stu-id="9d4bd-276">Parameter</span></span>                           | <span data-ttu-id="9d4bd-277">설명</span><span class="sxs-lookup"><span data-stu-id="9d4bd-277">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9d4bd-278"><nobr> *-Migration* \<문자열 ></nobr></span><span class="sxs-lookup"><span data-stu-id="9d4bd-278"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="9d4bd-279">대상 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-279">The target migration.</span></span> <span data-ttu-id="9d4bd-280">마이그레이션은 이름 또는 ID로 식별할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-280">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="9d4bd-281">숫자 0은 *첫 번째 마이그레이션 이전* 을 의미 하는 특별 한 경우로, 모든 마이그레이션이 되돌려집니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-281">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="9d4bd-282">마이그레이션이 지정 되지 않은 경우이 명령은 기본적으로 마지막 마이그레이션을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-282">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="9d4bd-283">Migration 매개 변수는 탭 확장을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-283">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="9d4bd-284">다음 예에서는 모든 마이그레이션을 되돌립니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-284">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="9d4bd-285">다음 예에서는 데이터베이스를 지정 된 마이그레이션으로 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-285">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="9d4bd-286">첫 번째는 마이그레이션 이름을 사용 하 고 두 번째는 마이그레이션 ID를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="9d4bd-286">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a><span data-ttu-id="9d4bd-287">추가 자료</span><span class="sxs-lookup"><span data-stu-id="9d4bd-287">Additional resources</span></span>

* [<span data-ttu-id="9d4bd-288">마이그레이션</span><span class="sxs-lookup"><span data-stu-id="9d4bd-288">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="9d4bd-289">리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="9d4bd-289">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
