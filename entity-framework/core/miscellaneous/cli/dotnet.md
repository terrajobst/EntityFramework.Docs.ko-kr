---
title: .NET core CLI-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 81427b010f63bdd591ffb9117c1556722313c3fa
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995299"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="a47a5-102">EF Core.NET 명령줄 도구</span><span class="sxs-lookup"><span data-stu-id="a47a5-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="a47a5-103">Entity Framework Core.NET 명령줄 도구는 플랫폼 간 확장 **dotnet** 참가 하는 명령인의 합니다 [.NET Core SDK][2]합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="a47a5-104">경우에 Visual Studio를 사용 하는, 것이 좋습니다 [PMC 도구] [ 1] 대신 더 통합적인된 환경을 제공 하므로 합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="a47a5-105">도구 설치</span><span class="sxs-lookup"><span data-stu-id="a47a5-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="a47a5-106">.NET Core SDK 2.1.300 버전에 최신 포함 **dotnet ef** EF Core 2.0 및 이후 버전과 호환 되는 명령입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="a47a5-107">따라서 최신 버전의 EF Core 런타임 및.NET Core SDK를 사용 하는 경우 설치가 필요 하지 않습니다 및이 섹션의 나머지 부분을 무시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="a47a5-108">다른 한편으로 **dotnet ef** 도구가.NET Core SDK 2.1.300 버전에 포함 하 고 최신 EF Core 버전 1.0 및 1.1 호환 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="a47a5-109">2.1.200 버전도 설치 해야.NET Core SDK 2.1.300 있는 컴퓨터에서 이전 버전의 EF Core를 사용 하는 프로젝트를 작업할 수 있습니다 전이나 설치 또는 이전 버전 SDK의 이전 버전을 사용 하 여 수정 하 여 응용 프로그램을 구성 하 고 해당  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="a47a5-110">이 파일은 일반적으로 솔루션 디렉터리 (프로젝트 위에서 하나)에 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="a47a5-111">그런 다음 아래 installlation 명령으로 진행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="a47a5-112">.NET Core SDK의 이전 버전에서는 이러한 단계를 사용 하 여 EF Core.NET 명령줄 도구를 설치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="a47a5-113">프로젝트 파일을 편집 하 고 Microsoft.EntityFrameworkCore.Tools.DotNet DotNetCliToolReference 항목 (아래 참조)으로 추가</span><span class="sxs-lookup"><span data-stu-id="a47a5-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="a47a5-114">다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="a47a5-115">결과 프로젝트는 코드는 다음과 같아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-115">The resulting project should look something like this:</span></span>

``` xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                      Version="2.0.0"
                      PrivateAssets="All" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                            Version="2.0.0" />
  </ItemGroup>
</Project>
```

> [!NOTE]
> <span data-ttu-id="a47a5-116">패키지 참조를 사용 하 여 `PrivateAssets="All"` 의미는 일반적으로 개발 하는 동안에 사용 되는 패키지 특히 유용이 프로젝트를 참조 하는 프로젝트에 노출 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="a47a5-117">오른쪽에서 모든 작업을 수행한 경우 명령 프롬프트에서 다음 명령을 실행 하려면 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="a47a5-118">도구를 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="a47a5-118">Using the tools</span></span>
---------------
<span data-ttu-id="a47a5-119">명령을 호출할 때마다 두 개의 프로젝트가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="a47a5-120">대상 프로젝트에는 모든 파일이 추가됩니다(또는 일부 경우 제거됨).</span><span class="sxs-lookup"><span data-stu-id="a47a5-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="a47a5-121">대상 프로젝트의 프로젝트는 현재 디렉터리에 기본값만 사용 하 여 변경할 수 있습니다 합니다 <nobr> **-프로젝트** </nobr> 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="a47a5-122">시작 프로젝트는 프로젝트 코드를 실행할 때 도구가 에뮬레이트하는 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="a47a5-123">사용 하 여 변경할 수 있습니다 하지만 기본값은 현재 디렉터리에서 프로젝트를 **-시작 프로젝트** 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="a47a5-124">예를 들어, EF Core는 다른 프로젝트에 설치 되어 있는 웹 응용 프로그램의 데이터베이스를 업데이트 하는 다음과 같습니다: `dotnet ef database update --project {project-path}` (에서 웹 앱 디렉터리)</span><span class="sxs-lookup"><span data-stu-id="a47a5-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="a47a5-125">일반 옵션:</span><span class="sxs-lookup"><span data-stu-id="a47a5-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="a47a5-126">--json</span><span class="sxs-lookup"><span data-stu-id="a47a5-126">--json</span></span>                           | <span data-ttu-id="a47a5-127">JSON 출력을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-127">Show JSON output.</span></span>           |
| <span data-ttu-id="a47a5-128">-c</span><span class="sxs-lookup"><span data-stu-id="a47a5-128">-c</span></span> | <span data-ttu-id="a47a5-129">-상황에 맞는 \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="a47a5-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="a47a5-130">사용 하 여 DbContext 합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="a47a5-131">-p</span><span class="sxs-lookup"><span data-stu-id="a47a5-131">-p</span></span> | <span data-ttu-id="a47a5-132">-프로젝트 \<프로젝트 ></span><span class="sxs-lookup"><span data-stu-id="a47a5-132">--project \<PROJECT></span></span>             | <span data-ttu-id="a47a5-133">프로젝트를 사용 하는입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-133">The project to use.</span></span>         |
| <span data-ttu-id="a47a5-134">-s</span><span class="sxs-lookup"><span data-stu-id="a47a5-134">-s</span></span> | <span data-ttu-id="a47a5-135">-시작 프로젝트 \<프로젝트 ></span><span class="sxs-lookup"><span data-stu-id="a47a5-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="a47a5-136">시작 프로젝트 사용입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="a47a5-137">-framework \<프레임 워크 ></span><span class="sxs-lookup"><span data-stu-id="a47a5-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="a47a5-138">대상 프레임 워크입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-138">The target framework.</span></span>       |
|    | <span data-ttu-id="a47a5-139">-구성 \<구성 ></span><span class="sxs-lookup"><span data-stu-id="a47a5-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="a47a5-140">사용할 구성입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="a47a5-141">-런타임 \<식별자 ></span><span class="sxs-lookup"><span data-stu-id="a47a5-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="a47a5-142">런타임 사용입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-142">The runtime to use.</span></span>         |
| <span data-ttu-id="a47a5-143">-h</span><span class="sxs-lookup"><span data-stu-id="a47a5-143">-h</span></span> | <span data-ttu-id="a47a5-144">-도움말</span><span class="sxs-lookup"><span data-stu-id="a47a5-144">--help</span></span>                           | <span data-ttu-id="a47a5-145">도움말 정보를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-145">Show help information.</span></span>      |
| <span data-ttu-id="a47a5-146">-v</span><span class="sxs-lookup"><span data-stu-id="a47a5-146">-v</span></span> | <span data-ttu-id="a47a5-147">-verbose</span><span class="sxs-lookup"><span data-stu-id="a47a5-147">--verbose</span></span>                        | <span data-ttu-id="a47a5-148">자세한 정보 출력을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="a47a5-149">-색 없음</span><span class="sxs-lookup"><span data-stu-id="a47a5-149">--no-color</span></span>                       | <span data-ttu-id="a47a5-150">출력 색을 지정 하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="a47a5-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="a47a5-151">--prefix-output</span><span class="sxs-lookup"><span data-stu-id="a47a5-151">--prefix-output</span></span>                  | <span data-ttu-id="a47a5-152">수준으로 출력 하는 접두사입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="a47a5-153">ASP.NET Core 환경 지정 하려면 설정 합니다 **ASPNETCORE_ENVIRONMENT** 실행 하기 전에 환경 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="a47a5-154">명령</span><span class="sxs-lookup"><span data-stu-id="a47a5-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="a47a5-155">dotnet ef 데이터베이스 삭제</span><span class="sxs-lookup"><span data-stu-id="a47a5-155">dotnet ef database drop</span></span>

<span data-ttu-id="a47a5-156">데이터베이스를 삭제합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-156">Drops the database.</span></span>

<span data-ttu-id="a47a5-157">옵션:</span><span class="sxs-lookup"><span data-stu-id="a47a5-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="a47a5-158">-f</span><span class="sxs-lookup"><span data-stu-id="a47a5-158">-f</span></span> | <span data-ttu-id="a47a5-159">-force</span><span class="sxs-lookup"><span data-stu-id="a47a5-159">--force</span></span>   | <span data-ttu-id="a47a5-160">확인 하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="a47a5-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="a47a5-161">--dry-run</span><span class="sxs-lookup"><span data-stu-id="a47a5-161">--dry-run</span></span> | <span data-ttu-id="a47a5-162">데이터베이스는 삭제할 수 있지만 삭제 하지는 마세요 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="a47a5-163">dotnet ef 데이터베이스 업데이트</span><span class="sxs-lookup"><span data-stu-id="a47a5-163">dotnet ef database update</span></span>

<span data-ttu-id="a47a5-164">지정 된 마이그레이션에 데이터베이스를 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="a47a5-165">인수:</span><span class="sxs-lookup"><span data-stu-id="a47a5-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="a47a5-166">\<마이그레이션 &GT;</span><span class="sxs-lookup"><span data-stu-id="a47a5-166">\<MIGRATION></span></span> | <span data-ttu-id="a47a5-167">대상 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-167">The target migration.</span></span> <span data-ttu-id="a47a5-168">0 인 경우, 모든 마이그레이션 되돌려집니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="a47a5-169">기본값 마지막 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="a47a5-170">dotnet ef dbcontext 정보</span><span class="sxs-lookup"><span data-stu-id="a47a5-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="a47a5-171">DbContext 유형에 대 한 정보를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="a47a5-172">dotnet ef dbcontext 목록</span><span class="sxs-lookup"><span data-stu-id="a47a5-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="a47a5-173">사용 가능한 DbContext 형식을 나열합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="a47a5-174">dotnet ef dbcontext 스 캐 폴드</span><span class="sxs-lookup"><span data-stu-id="a47a5-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="a47a5-175">데이터베이스는 DbContext와 엔터티 형식을 스 캐 폴딩 합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="a47a5-176">인수:</span><span class="sxs-lookup"><span data-stu-id="a47a5-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="a47a5-177">\<연결 &GT;</span><span class="sxs-lookup"><span data-stu-id="a47a5-177">\<CONNECTION></span></span> | <span data-ttu-id="a47a5-178">데이터베이스에 연결 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="a47a5-179">\<공급자 &GT;</span><span class="sxs-lookup"><span data-stu-id="a47a5-179">\<PROVIDER></span></span>   | <span data-ttu-id="a47a5-180">공급자에서 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-180">The provider to use.</span></span> <span data-ttu-id="a47a5-181">(예: Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="a47a5-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="a47a5-182">옵션:</span><span class="sxs-lookup"><span data-stu-id="a47a5-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a47a5-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="a47a5-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="a47a5-184">-데이터 주석</span><span class="sxs-lookup"><span data-stu-id="a47a5-184">--data-annotations</span></span>                      | <span data-ttu-id="a47a5-185">(가능한 한) 경우 모델을 구성 하려면 특성을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="a47a5-186">생략 하면 fluent API만 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="a47a5-187">-c</span><span class="sxs-lookup"><span data-stu-id="a47a5-187">-c</span></span>              | <span data-ttu-id="a47a5-188">-상황에 맞는 \<이름 ></span><span class="sxs-lookup"><span data-stu-id="a47a5-188">--context \<NAME></span></span>                       | <span data-ttu-id="a47a5-189">DbContext의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="a47a5-190">-상황에 맞는-dir \<경로 ></span><span class="sxs-lookup"><span data-stu-id="a47a5-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="a47a5-191">에 DbContext 파일을 배치할 디렉터리입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="a47a5-192">프로젝트 디렉터리를 기준으로 경로입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="a47a5-193">-f</span><span class="sxs-lookup"><span data-stu-id="a47a5-193">-f</span></span>              | <span data-ttu-id="a47a5-194">-force</span><span class="sxs-lookup"><span data-stu-id="a47a5-194">--force</span></span>                                 | <span data-ttu-id="a47a5-195">기존 파일을 덮어씁니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="a47a5-196">-o</span><span class="sxs-lookup"><span data-stu-id="a47a5-196">-o</span></span>              | <span data-ttu-id="a47a5-197">-출력 dir \<경로 ></span><span class="sxs-lookup"><span data-stu-id="a47a5-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="a47a5-198">디렉터리에 파일 보관입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-198">The directory to put files in.</span></span> <span data-ttu-id="a47a5-199">프로젝트 디렉터리를 기준으로 경로입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="a47a5-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="a47a5-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="a47a5-201">에 대 한 엔터티 형식을 생성 하는 테이블의 스키마입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="a47a5-202">-t</span><span class="sxs-lookup"><span data-stu-id="a47a5-202">-t</span></span>              | <span data-ttu-id="a47a5-203">-테이블 \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="a47a5-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="a47a5-204">에 대 한 엔터티 형식을 생성 하는 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="a47a5-205">-사용 하 여 데이터베이스 이름</span><span class="sxs-lookup"><span data-stu-id="a47a5-205">--use-database-names</span></span>                    | <span data-ttu-id="a47a5-206">데이터베이스에서 직접 테이블 및 열 이름을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="a47a5-207">dotnet ef migrations 추가</span><span class="sxs-lookup"><span data-stu-id="a47a5-207">dotnet ef migrations add</span></span>

<span data-ttu-id="a47a5-208">새 마이그레이션을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-208">Adds a new migration.</span></span>

<span data-ttu-id="a47a5-209">인수:</span><span class="sxs-lookup"><span data-stu-id="a47a5-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="a47a5-210">\<이름 &GT;</span><span class="sxs-lookup"><span data-stu-id="a47a5-210">\<NAME></span></span> | <span data-ttu-id="a47a5-211">마이그레이션 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-211">The name of the migration.</span></span> |

<span data-ttu-id="a47a5-212">옵션:</span><span class="sxs-lookup"><span data-stu-id="a47a5-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a47a5-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="a47a5-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="a47a5-214"><nobr>--output-dir \<PATH></nobr></span><span class="sxs-lookup"><span data-stu-id="a47a5-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="a47a5-215">사용 하 여 디렉터리 (및 하위 네임 스페이스).</span><span class="sxs-lookup"><span data-stu-id="a47a5-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="a47a5-216">프로젝트 디렉터리를 기준으로 경로입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="a47a5-217">기본값은 "마이그레이션"입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="a47a5-218">dotnet ef migrations 목록</span><span class="sxs-lookup"><span data-stu-id="a47a5-218">dotnet ef migrations list</span></span>

<span data-ttu-id="a47a5-219">사용할 수 있는 마이그레이션을 나열합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="a47a5-220">dotnet ef migrations 제거</span><span class="sxs-lookup"><span data-stu-id="a47a5-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="a47a5-221">마지막 마이그레이션을 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-221">Removes the last migration.</span></span>

<span data-ttu-id="a47a5-222">옵션:</span><span class="sxs-lookup"><span data-stu-id="a47a5-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="a47a5-223">-f</span><span class="sxs-lookup"><span data-stu-id="a47a5-223">-f</span></span> | <span data-ttu-id="a47a5-224">-force</span><span class="sxs-lookup"><span data-stu-id="a47a5-224">--force</span></span> | <span data-ttu-id="a47a5-225">데이터베이스에 적용 한 경우 마이그레이션을 되돌립니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="a47a5-226">dotnet ef migrations 스크립트</span><span class="sxs-lookup"><span data-stu-id="a47a5-226">dotnet ef migrations script</span></span>

<span data-ttu-id="a47a5-227">마이그레이션에서 SQL 스크립트를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="a47a5-228">인수:</span><span class="sxs-lookup"><span data-stu-id="a47a5-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="a47a5-229">\<FROM></span><span class="sxs-lookup"><span data-stu-id="a47a5-229">\<FROM></span></span> | <span data-ttu-id="a47a5-230">마이그레이션을 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-230">The starting migration.</span></span> <span data-ttu-id="a47a5-231">기본값은 0 (초기 데이터베이스)입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="a47a5-232">\<TO></span><span class="sxs-lookup"><span data-stu-id="a47a5-232">\<TO></span></span>   | <span data-ttu-id="a47a5-233">끝 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-233">The ending migration.</span></span> <span data-ttu-id="a47a5-234">기본값 마지막 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="a47a5-235">옵션:</span><span class="sxs-lookup"><span data-stu-id="a47a5-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="a47a5-236">-o</span><span class="sxs-lookup"><span data-stu-id="a47a5-236">-o</span></span> | <span data-ttu-id="a47a5-237">-출력 \<파일 ></span><span class="sxs-lookup"><span data-stu-id="a47a5-237">--output \<FILE></span></span> | <span data-ttu-id="a47a5-238">결과를 쓸 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="a47a5-239">-i</span><span class="sxs-lookup"><span data-stu-id="a47a5-239">-i</span></span> | <span data-ttu-id="a47a5-240">-멱 등 원</span><span class="sxs-lookup"><span data-stu-id="a47a5-240">--idempotent</span></span>     | <span data-ttu-id="a47a5-241">모든 마이그레이션 데이터베이스에서 사용할 수 있는 스크립트를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="a47a5-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
