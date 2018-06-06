---
title: .NET core CLI-EF 코어
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 721235b07e695efd8df43294e1f4e90c28ae83d7
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754498"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="2c370-102">EF 핵심.NET 명령줄 도구</span><span class="sxs-lookup"><span data-stu-id="2c370-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="2c370-103">Entity Framework Core.NET 명령줄 도구는 플랫폼 간에 대 한 확장 **dotnet** 명령에 포함 된의 [.NET Core SDK][2]합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="2c370-104">권장 하는 경우에 Visual Studio를 사용 하는, [PMC 도구] [ 1] 대신 더 통합 된 환경을 제공 하므로 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="2c370-105">도구 설치</span><span class="sxs-lookup"><span data-stu-id="2c370-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="2c370-106">.NET Core SDK 버전 2.1.300 최신 작성과 **dotnet ef** EF 코어 2.0 및 이상 버전을 호환 되는 명령입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="2c370-107">따라서 최신 버전의 EF 핵심 런타임 및.NET Core SDK를 사용 하는 경우 설치가 필요 하지 않습니다 및이 섹션의 나머지 부분을 무시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="2c370-108">반면에 **dotnet ef** 2.1.300.NET Core SDK 버전에 포함 되 고 최신 도구 EF 핵심 버전 1.0 및 1.1와 호환 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="2c370-109">.NET Core SDK 2.1.300 있는 컴퓨터에 이전 버전의 EF 코어를 사용 하는 프로젝트를 사용 하 여 작업할 수 또는 그 이상 버전이 설치 전에 2.1.200 버전 설치 해야는 SDK의 이전 또는 수정 하 여 이전 버전에 사용할 응용 프로그램을 구성 하 고 해당  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="2c370-110">이 파일은 일반적으로 솔루션 디렉터리 (프로젝트 위에서 하나)에 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="2c370-111">그런 다음 아래 installlation 명령으로 진행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="2c370-112">.NET Core SDK의 이전 버전에서는 다음이 단계를 사용 하 여 EF 코어.NET 명령줄 도구를 설치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="2c370-113">프로젝트 파일을 편집 하 고 Microsoft.EntityFrameworkCore.Tools.DotNet DotNetCliToolReference 항목 (아래 참조)로 추가</span><span class="sxs-lookup"><span data-stu-id="2c370-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="2c370-114">다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="2c370-115">결과 프로젝트 코드는 다음과 같아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="2c370-116">사용 하 여 패키지 참조 `PrivateAssets="All"` 의미는 일반적으로 개발 하는 동안에 사용 되는 패키지에 대 한 특히 유용이 프로젝트를 참조 하는 프로젝트에 노출 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="2c370-117">모든 것이 올바르게를 수행한 경우에 명령 프롬프트에서 다음 명령을 실행 할 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="2c370-118">도구를 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="2c370-118">Using the tools</span></span>
---------------
<span data-ttu-id="2c370-119">명령을 실행할 때마다 두 개의 프로젝트가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="2c370-120">대상 프로젝트에는 모든 파일이 추가됩니다(또는 일부 경우 제거됨).</span><span class="sxs-lookup"><span data-stu-id="2c370-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="2c370-121">대상 프로젝트의 프로젝트는 현재 디렉터리에 기본적으로 사용 되지만 사용 하 여 변경할 수는 <nobr> **-프로젝트** </nobr> 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="2c370-122">시작 프로젝트는 프로젝트 코드를 실행할 때 도구가 에뮬레이트하는 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="2c370-123">사용 하 여 변경할 수 있지만 것 현재 디렉터리에 프로젝트에 기본적으로 **-시작 프로젝트** 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="2c370-124">예를 들어, EF 코어 다른 프로젝트에 설치 되어 있는 웹 응용 프로그램 데이터베이스를 업데이트 하는 다음과 같습니다: `dotnet ef database update --project {project-path}` (에서 웹 응용 프로그램 디렉터리)</span><span class="sxs-lookup"><span data-stu-id="2c370-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="2c370-125">일반 옵션:</span><span class="sxs-lookup"><span data-stu-id="2c370-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="2c370-126">-json</span><span class="sxs-lookup"><span data-stu-id="2c370-126">--json</span></span>                           | <span data-ttu-id="2c370-127">JSON 출력을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-127">Show JSON output.</span></span>           |
| <span data-ttu-id="2c370-128">-c</span><span class="sxs-lookup"><span data-stu-id="2c370-128">-c</span></span> | <span data-ttu-id="2c370-129">-상황에 맞는 \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="2c370-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="2c370-130">사용할 DbContext 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="2c370-131">-p</span><span class="sxs-lookup"><span data-stu-id="2c370-131">-p</span></span> | <span data-ttu-id="2c370-132">-프로젝트 \<프로젝트 ></span><span class="sxs-lookup"><span data-stu-id="2c370-132">--project \<PROJECT></span></span>             | <span data-ttu-id="2c370-133">사용 하는 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-133">The project to use.</span></span>         |
| <span data-ttu-id="2c370-134">-s</span><span class="sxs-lookup"><span data-stu-id="2c370-134">-s</span></span> | <span data-ttu-id="2c370-135">-시작 프로젝트 \<프로젝트 ></span><span class="sxs-lookup"><span data-stu-id="2c370-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="2c370-136">시작 프로젝트 사용입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="2c370-137">-프레임 워크 \<프레임 워크 ></span><span class="sxs-lookup"><span data-stu-id="2c370-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="2c370-138">대상 프레임 워크입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-138">The target framework.</span></span>       |
|    | <span data-ttu-id="2c370-139">-configuration \<구성 ></span><span class="sxs-lookup"><span data-stu-id="2c370-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="2c370-140">사용할 구성입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="2c370-141">-런타임 \<식별자 ></span><span class="sxs-lookup"><span data-stu-id="2c370-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="2c370-142">사용 하는 런타임입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-142">The runtime to use.</span></span>         |
| <span data-ttu-id="2c370-143">-h</span><span class="sxs-lookup"><span data-stu-id="2c370-143">-h</span></span> | <span data-ttu-id="2c370-144">-도움말</span><span class="sxs-lookup"><span data-stu-id="2c370-144">--help</span></span>                           | <span data-ttu-id="2c370-145">도움말 정보를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-145">Show help information.</span></span>      |
| <span data-ttu-id="2c370-146">-v</span><span class="sxs-lookup"><span data-stu-id="2c370-146">-v</span></span> | <span data-ttu-id="2c370-147">-verbose</span><span class="sxs-lookup"><span data-stu-id="2c370-147">--verbose</span></span>                        | <span data-ttu-id="2c370-148">자세한 출력을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="2c370-149">-색 없음</span><span class="sxs-lookup"><span data-stu-id="2c370-149">--no-color</span></span>                       | <span data-ttu-id="2c370-150">출력 색을 지정 하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="2c370-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="2c370-151">--prefix-output</span><span class="sxs-lookup"><span data-stu-id="2c370-151">--prefix-output</span></span>                  | <span data-ttu-id="2c370-152">수준으로 출력 하는 접두사입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="2c370-153">ASP.NET Core 환경을 지정 하려면는 **ASPNETCORE_ENVIRONMENT** 실행 하기 전에 환경 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="2c370-154">명령</span><span class="sxs-lookup"><span data-stu-id="2c370-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="2c370-155">dotnet ef 데이터베이스 삭제</span><span class="sxs-lookup"><span data-stu-id="2c370-155">dotnet ef database drop</span></span>

<span data-ttu-id="2c370-156">데이터베이스를 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-156">Drops the database.</span></span>

<span data-ttu-id="2c370-157">옵션:</span><span class="sxs-lookup"><span data-stu-id="2c370-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="2c370-158">-f</span><span class="sxs-lookup"><span data-stu-id="2c370-158">-f</span></span> | <span data-ttu-id="2c370-159">-force</span><span class="sxs-lookup"><span data-stu-id="2c370-159">--force</span></span>   | <span data-ttu-id="2c370-160">확인 하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="2c370-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="2c370-161">--dry-run</span><span class="sxs-lookup"><span data-stu-id="2c370-161">--dry-run</span></span> | <span data-ttu-id="2c370-162">표시는 데이터베이스는 삭제할 수 없지만 삭제 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="2c370-163">dotnet ef 데이터베이스 업데이트</span><span class="sxs-lookup"><span data-stu-id="2c370-163">dotnet ef database update</span></span>

<span data-ttu-id="2c370-164">지정 된 마이그레이션에는 데이터베이스를 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="2c370-165">인수:</span><span class="sxs-lookup"><span data-stu-id="2c370-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="2c370-166">\<마이그레이션 &GT;</span><span class="sxs-lookup"><span data-stu-id="2c370-166">\<MIGRATION></span></span> | <span data-ttu-id="2c370-167">대상 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-167">The target migration.</span></span> <span data-ttu-id="2c370-168">0 인 경우, 모든 마이그레이션 되돌립니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="2c370-169">마지막으로 마이그레이션을 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="2c370-170">dotnet ef dbcontext 정보</span><span class="sxs-lookup"><span data-stu-id="2c370-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="2c370-171">DbContext 형식에 대 한 정보를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="2c370-172">dotnet ef dbcontext 목록</span><span class="sxs-lookup"><span data-stu-id="2c370-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="2c370-173">사용 가능한 DbContext 유형을 나열합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="2c370-174">dotnet ef dbcontext 스 캐 폴딩</span><span class="sxs-lookup"><span data-stu-id="2c370-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="2c370-175">스 캐 폴드 데이터베이스에 대 한 DbContext 및 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="2c370-176">인수:</span><span class="sxs-lookup"><span data-stu-id="2c370-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="2c370-177">\<연결 &GT;</span><span class="sxs-lookup"><span data-stu-id="2c370-177">\<CONNECTION></span></span> | <span data-ttu-id="2c370-178">데이터베이스에 연결 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="2c370-179">\<공급자 &GT;</span><span class="sxs-lookup"><span data-stu-id="2c370-179">\<PROVIDER></span></span>   | <span data-ttu-id="2c370-180">사용 하는 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-180">The provider to use.</span></span> <span data-ttu-id="2c370-181">(예: Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="2c370-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="2c370-182">옵션:</span><span class="sxs-lookup"><span data-stu-id="2c370-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2c370-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="2c370-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="2c370-184">-데이터 주석</span><span class="sxs-lookup"><span data-stu-id="2c370-184">--data-annotations</span></span>                      | <span data-ttu-id="2c370-185">(사용 가능한) 위치 모델을 구성 하려면 특성을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="2c370-186">생략 하면 fluent API만 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="2c370-187">-c</span><span class="sxs-lookup"><span data-stu-id="2c370-187">-c</span></span>              | <span data-ttu-id="2c370-188">-상황에 맞는 \<이름 ></span><span class="sxs-lookup"><span data-stu-id="2c370-188">--context \<NAME></span></span>                       | <span data-ttu-id="2c370-189">DbContext의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="2c370-190">-dir 컨텍스트 \<경로 ></span><span class="sxs-lookup"><span data-stu-id="2c370-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="2c370-191">디렉터리에 DbContext 파일을 넣습니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="2c370-192">경로 프로젝트 디렉터리에 상대적입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="2c370-193">-f</span><span class="sxs-lookup"><span data-stu-id="2c370-193">-f</span></span>              | <span data-ttu-id="2c370-194">-force</span><span class="sxs-lookup"><span data-stu-id="2c370-194">--force</span></span>                                 | <span data-ttu-id="2c370-195">기존 파일을 덮어씁니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="2c370-196">-o</span><span class="sxs-lookup"><span data-stu-id="2c370-196">-o</span></span>              | <span data-ttu-id="2c370-197">-출력-dir \<경로 ></span><span class="sxs-lookup"><span data-stu-id="2c370-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="2c370-198">에 파일을 넣을 디렉터리입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-198">The directory to put files in.</span></span> <span data-ttu-id="2c370-199">경로 프로젝트 디렉터리에 상대적입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="2c370-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="2c370-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="2c370-201">에 대 한 엔터티 형식을 생성 하는 테이블의 스키마입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="2c370-202">-t</span><span class="sxs-lookup"><span data-stu-id="2c370-202">-t</span></span>              | <span data-ttu-id="2c370-203">-테이블 \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="2c370-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="2c370-204">엔터티 형식에 대 한 생성을 위한 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="2c370-205">-이름-데이터베이스-사용</span><span class="sxs-lookup"><span data-stu-id="2c370-205">--use-database-names</span></span>                    | <span data-ttu-id="2c370-206">데이터베이스에서 직접 테이블 및 열 이름을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="2c370-207">dotnet ef 마이그레이션 추가</span><span class="sxs-lookup"><span data-stu-id="2c370-207">dotnet ef migrations add</span></span>

<span data-ttu-id="2c370-208">새 마이그레이션을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-208">Adds a new migration.</span></span>

<span data-ttu-id="2c370-209">인수:</span><span class="sxs-lookup"><span data-stu-id="2c370-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="2c370-210">\<이름 &GT;</span><span class="sxs-lookup"><span data-stu-id="2c370-210">\<NAME></span></span> | <span data-ttu-id="2c370-211">마이그레이션의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-211">The name of the migration.</span></span> |

<span data-ttu-id="2c370-212">옵션:</span><span class="sxs-lookup"><span data-stu-id="2c370-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2c370-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="2c370-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="2c370-214"><nobr>--output-dir \<PATH></nobr></span><span class="sxs-lookup"><span data-stu-id="2c370-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="2c370-215">사용할 디렉터리 (및 하위 네임 스페이스).</span><span class="sxs-lookup"><span data-stu-id="2c370-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="2c370-216">경로 프로젝트 디렉터리에 상대적입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="2c370-217">기본값은 "마이그레이션"입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="2c370-218">dotnet ef 마이그레이션 목록</span><span class="sxs-lookup"><span data-stu-id="2c370-218">dotnet ef migrations list</span></span>

<span data-ttu-id="2c370-219">사용 가능한 마이그레이션을 나열합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="2c370-220">dotnet ef 마이그레이션 제거</span><span class="sxs-lookup"><span data-stu-id="2c370-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="2c370-221">마지막 마이그레이션을 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-221">Removes the last migration.</span></span>

<span data-ttu-id="2c370-222">옵션:</span><span class="sxs-lookup"><span data-stu-id="2c370-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="2c370-223">-f</span><span class="sxs-lookup"><span data-stu-id="2c370-223">-f</span></span> | <span data-ttu-id="2c370-224">-force</span><span class="sxs-lookup"><span data-stu-id="2c370-224">--force</span></span> | <span data-ttu-id="2c370-225">데이터베이스에 적용 된 경우 마이그레이션을 되돌립니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="2c370-226">dotnet ef 마이그레이션 스크립트</span><span class="sxs-lookup"><span data-stu-id="2c370-226">dotnet ef migrations script</span></span>

<span data-ttu-id="2c370-227">마이그레이션의 SQL 스크립트를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="2c370-228">인수:</span><span class="sxs-lookup"><span data-stu-id="2c370-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="2c370-229">\<FROM></span><span class="sxs-lookup"><span data-stu-id="2c370-229">\<FROM></span></span> | <span data-ttu-id="2c370-230">마이그레이션을 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-230">The starting migration.</span></span> <span data-ttu-id="2c370-231">기본값은 0 (초기 데이터베이스)입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="2c370-232">\<TO></span><span class="sxs-lookup"><span data-stu-id="2c370-232">\<TO></span></span>   | <span data-ttu-id="2c370-233">끝의 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-233">The ending migration.</span></span> <span data-ttu-id="2c370-234">마지막으로 마이그레이션을 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="2c370-235">옵션:</span><span class="sxs-lookup"><span data-stu-id="2c370-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="2c370-236">-o</span><span class="sxs-lookup"><span data-stu-id="2c370-236">-o</span></span> | <span data-ttu-id="2c370-237">-출력 \<파일 ></span><span class="sxs-lookup"><span data-stu-id="2c370-237">--output \<FILE></span></span> | <span data-ttu-id="2c370-238">결과를 쓸 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="2c370-239">-i</span><span class="sxs-lookup"><span data-stu-id="2c370-239">-i</span></span> | <span data-ttu-id="2c370-240">-idempotent</span><span class="sxs-lookup"><span data-stu-id="2c370-240">--idempotent</span></span>     | <span data-ttu-id="2c370-241">모든 마이그레이션에는 데이터베이스에서 사용할 수 있는 스크립트를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c370-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
