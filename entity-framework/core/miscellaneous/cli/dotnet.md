---
title: .NET core CLI-EF 코어
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 396d31c9d0c0f47d299f49e82e557ed29b8420e7
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="1933a-102">EF 핵심.NET 명령줄 도구</span><span class="sxs-lookup"><span data-stu-id="1933a-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="1933a-103">Entity Framework Core.NET 명령줄 도구는 플랫폼 간에 대 한 확장 **dotnet** 명령에 포함 된의 [.NET Core SDK][2]합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="1933a-104">권장 하는 경우에 Visual Studio를 사용 하는, [PMC 도구] [ 1] 대신 더 통합 된 환경을 제공 하므로 합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="1933a-105">도구 설치</span><span class="sxs-lookup"><span data-stu-id="1933a-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="1933a-106">다음이 단계를 사용 하 여 EF 코어.NET 명령줄 도구를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="1933a-107">프로젝트 파일을 편집 하 고 Microsoft.EntityFrameworkCore.Tools.DotNet DotNetCliToolReference 항목 (아래 참조)로 추가</span><span class="sxs-lookup"><span data-stu-id="1933a-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="1933a-108">다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="1933a-109">결과 프로젝트 코드는 다음과 같아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="1933a-110">사용 하 여 패키지 참조 `PrivateAssets="All"` 의미는 일반적으로 개발 하는 동안에 사용 되는 패키지에 대 한 특히 유용이 프로젝트를 참조 하는 프로젝트에 노출 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="1933a-111">모든 것이 올바르게를 수행한 경우에 명령 프롬프트에서 다음 명령을 실행 할 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="1933a-112">도구를 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="1933a-112">Using the tools</span></span>
---------------
<span data-ttu-id="1933a-113">명령을 실행할 때마다 두 개의 프로젝트가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="1933a-114">대상 프로젝트에는 모든 파일이 추가됩니다(또는 일부 경우 제거됨).</span><span class="sxs-lookup"><span data-stu-id="1933a-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="1933a-115">대상 프로젝트의 프로젝트는 현재 디렉터리에 기본적으로 사용 되지만 사용 하 여 변경할 수는 <nobr> **-프로젝트** </nobr> 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="1933a-116">시작 프로젝트는 프로젝트 코드를 실행할 때 도구가 에뮬레이트하는 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="1933a-117">사용 하 여 변경할 수 있지만 것 현재 디렉터리에 프로젝트에 기본적으로 **-시작 프로젝트** 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="1933a-118">예를 들어, EF 코어 다른 프로젝트에 설치 되어 있는 웹 응용 프로그램 데이터베이스를 업데이트 하는 다음과 같습니다: `dotnet ef database update --project {project-path}` (에서 웹 응용 프로그램 디렉터리)</span><span class="sxs-lookup"><span data-stu-id="1933a-118">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="1933a-119">일반 옵션:</span><span class="sxs-lookup"><span data-stu-id="1933a-119">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="1933a-120">-json</span><span class="sxs-lookup"><span data-stu-id="1933a-120">--json</span></span>                           | <span data-ttu-id="1933a-121">JSON 출력을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-121">Show JSON output.</span></span>           |
| <span data-ttu-id="1933a-122">-c</span><span class="sxs-lookup"><span data-stu-id="1933a-122">-c</span></span> | <span data-ttu-id="1933a-123">-상황에 맞는 \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="1933a-123">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="1933a-124">사용할 DbContext 합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-124">The DbContext to use.</span></span>       |
| <span data-ttu-id="1933a-125">-p</span><span class="sxs-lookup"><span data-stu-id="1933a-125">-p</span></span> | <span data-ttu-id="1933a-126">-프로젝트 \<프로젝트 ></span><span class="sxs-lookup"><span data-stu-id="1933a-126">--project \<PROJECT></span></span>             | <span data-ttu-id="1933a-127">사용 하는 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-127">The project to use.</span></span>         |
| <span data-ttu-id="1933a-128">-s</span><span class="sxs-lookup"><span data-stu-id="1933a-128">-s</span></span> | <span data-ttu-id="1933a-129">-시작 프로젝트 \<프로젝트 ></span><span class="sxs-lookup"><span data-stu-id="1933a-129">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="1933a-130">시작 프로젝트 사용입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-130">The startup project to use.</span></span> |
|    | <span data-ttu-id="1933a-131">-프레임 워크 \<프레임 워크 ></span><span class="sxs-lookup"><span data-stu-id="1933a-131">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="1933a-132">대상 프레임 워크입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-132">The target framework.</span></span>       |
|    | <span data-ttu-id="1933a-133">-configuration \<구성 ></span><span class="sxs-lookup"><span data-stu-id="1933a-133">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="1933a-134">사용할 구성입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-134">The configuration to use.</span></span>   |
|    | <span data-ttu-id="1933a-135">-런타임 \<식별자 ></span><span class="sxs-lookup"><span data-stu-id="1933a-135">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="1933a-136">사용 하는 런타임입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-136">The runtime to use.</span></span>         |
| <span data-ttu-id="1933a-137">-h</span><span class="sxs-lookup"><span data-stu-id="1933a-137">-h</span></span> | <span data-ttu-id="1933a-138">-도움말</span><span class="sxs-lookup"><span data-stu-id="1933a-138">--help</span></span>                           | <span data-ttu-id="1933a-139">도움말 정보를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-139">Show help information.</span></span>      |
| <span data-ttu-id="1933a-140">-v</span><span class="sxs-lookup"><span data-stu-id="1933a-140">-v</span></span> | <span data-ttu-id="1933a-141">-verbose</span><span class="sxs-lookup"><span data-stu-id="1933a-141">--verbose</span></span>                        | <span data-ttu-id="1933a-142">자세한 출력을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-142">Show verbose output.</span></span>        |
|    | <span data-ttu-id="1933a-143">-색 없음</span><span class="sxs-lookup"><span data-stu-id="1933a-143">--no-color</span></span>                       | <span data-ttu-id="1933a-144">출력 색을 지정 하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="1933a-144">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="1933a-145">--prefix-output</span><span class="sxs-lookup"><span data-stu-id="1933a-145">--prefix-output</span></span>                  | <span data-ttu-id="1933a-146">수준으로 출력 하는 접두사입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-146">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="1933a-147">ASP.NET Core 환경을 지정 하려면는 **ASPNETCORE_ENVIRONMENT** 실행 하기 전에 환경 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-147">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="1933a-148">명령</span><span class="sxs-lookup"><span data-stu-id="1933a-148">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="1933a-149">dotnet ef 데이터베이스 삭제</span><span class="sxs-lookup"><span data-stu-id="1933a-149">dotnet ef database drop</span></span>

<span data-ttu-id="1933a-150">데이터베이스를 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-150">Drops the database.</span></span>

<span data-ttu-id="1933a-151">옵션:</span><span class="sxs-lookup"><span data-stu-id="1933a-151">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="1933a-152">-f</span><span class="sxs-lookup"><span data-stu-id="1933a-152">-f</span></span> | <span data-ttu-id="1933a-153">-force</span><span class="sxs-lookup"><span data-stu-id="1933a-153">--force</span></span>   | <span data-ttu-id="1933a-154">확인 하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="1933a-154">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="1933a-155">--dry-run</span><span class="sxs-lookup"><span data-stu-id="1933a-155">--dry-run</span></span> | <span data-ttu-id="1933a-156">표시는 데이터베이스는 삭제할 수 없지만 삭제 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-156">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="1933a-157">dotnet ef 데이터베이스 업데이트</span><span class="sxs-lookup"><span data-stu-id="1933a-157">dotnet ef database update</span></span>

<span data-ttu-id="1933a-158">지정 된 마이그레이션에는 데이터베이스를 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-158">Updates the database to a specified migration.</span></span>

<span data-ttu-id="1933a-159">인수:</span><span class="sxs-lookup"><span data-stu-id="1933a-159">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="1933a-160">\<마이그레이션 &GT;</span><span class="sxs-lookup"><span data-stu-id="1933a-160">\<MIGRATION></span></span> | <span data-ttu-id="1933a-161">대상 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-161">The target migration.</span></span> <span data-ttu-id="1933a-162">0 인 경우, 모든 마이그레이션 되돌립니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-162">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="1933a-163">마지막으로 마이그레이션을 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-163">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="1933a-164">dotnet ef dbcontext 정보</span><span class="sxs-lookup"><span data-stu-id="1933a-164">dotnet ef dbcontext info</span></span>

<span data-ttu-id="1933a-165">DbContext 형식에 대 한 정보를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-165">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="1933a-166">dotnet ef dbcontext 목록</span><span class="sxs-lookup"><span data-stu-id="1933a-166">dotnet ef dbcontext list</span></span>

<span data-ttu-id="1933a-167">사용 가능한 DbContext 유형을 나열합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-167">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="1933a-168">dotnet ef dbcontext 스 캐 폴딩</span><span class="sxs-lookup"><span data-stu-id="1933a-168">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="1933a-169">스 캐 폴드 데이터베이스에 대 한 DbContext 및 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-169">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="1933a-170">인수:</span><span class="sxs-lookup"><span data-stu-id="1933a-170">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="1933a-171">\<연결 &GT;</span><span class="sxs-lookup"><span data-stu-id="1933a-171">\<CONNECTION></span></span> | <span data-ttu-id="1933a-172">데이터베이스에 연결 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-172">The connection string to the database.</span></span>                              |
| <span data-ttu-id="1933a-173">\<공급자 &GT;</span><span class="sxs-lookup"><span data-stu-id="1933a-173">\<PROVIDER></span></span>   | <span data-ttu-id="1933a-174">사용 하는 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-174">The provider to use.</span></span> <span data-ttu-id="1933a-175">(예:</span><span class="sxs-lookup"><span data-stu-id="1933a-175">(E.g.</span></span> <span data-ttu-id="1933a-176">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="1933a-176">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="1933a-177">옵션:</span><span class="sxs-lookup"><span data-stu-id="1933a-177">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1933a-178"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="1933a-178"><nobr>-d</nobr></span></span> | <span data-ttu-id="1933a-179">-데이터 주석</span><span class="sxs-lookup"><span data-stu-id="1933a-179">--data-annotations</span></span>                      | <span data-ttu-id="1933a-180">(사용 가능한) 위치 모델을 구성 하려면 특성을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-180">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="1933a-181">생략 하면 fluent API만 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-181">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="1933a-182">-c</span><span class="sxs-lookup"><span data-stu-id="1933a-182">-c</span></span>              | <span data-ttu-id="1933a-183">-상황에 맞는 \<이름 ></span><span class="sxs-lookup"><span data-stu-id="1933a-183">--context \<NAME></span></span>                       | <span data-ttu-id="1933a-184">DbContext의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-184">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="1933a-185">-dir 컨텍스트 \<경로 ></span><span class="sxs-lookup"><span data-stu-id="1933a-185">--context-dir \<PATH></span></span>                   | <span data-ttu-id="1933a-186">디렉터리에 DbContext 파일을 넣습니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-186">The directory to put DbContext file in.</span></span> <span data-ttu-id="1933a-187">경로 프로젝트 디렉터리에 상대적입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-187">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="1933a-188">-f</span><span class="sxs-lookup"><span data-stu-id="1933a-188">-f</span></span>              | <span data-ttu-id="1933a-189">-force</span><span class="sxs-lookup"><span data-stu-id="1933a-189">--force</span></span>                                 | <span data-ttu-id="1933a-190">기존 파일을 덮어씁니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-190">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="1933a-191">-o</span><span class="sxs-lookup"><span data-stu-id="1933a-191">-o</span></span>              | <span data-ttu-id="1933a-192">-출력-dir \<경로 ></span><span class="sxs-lookup"><span data-stu-id="1933a-192">--output-dir \<PATH></span></span>                    | <span data-ttu-id="1933a-193">에 파일을 넣을 디렉터리입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-193">The directory to put files in.</span></span> <span data-ttu-id="1933a-194">경로 프로젝트 디렉터리에 상대적입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-194">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="1933a-195"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="1933a-195"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="1933a-196">에 대 한 엔터티 형식을 생성 하는 테이블의 스키마입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-196">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="1933a-197">-t</span><span class="sxs-lookup"><span data-stu-id="1933a-197">-t</span></span>              | <span data-ttu-id="1933a-198">-테이블 \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="1933a-198">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="1933a-199">엔터티 형식에 대 한 생성을 위한 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-199">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="1933a-200">-이름-데이터베이스-사용</span><span class="sxs-lookup"><span data-stu-id="1933a-200">--use-database-names</span></span>                    | <span data-ttu-id="1933a-201">데이터베이스에서 직접 테이블 및 열 이름을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-201">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="1933a-202">dotnet ef 마이그레이션 추가</span><span class="sxs-lookup"><span data-stu-id="1933a-202">dotnet ef migrations add</span></span>

<span data-ttu-id="1933a-203">새 마이그레이션을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-203">Adds a new migration.</span></span>

<span data-ttu-id="1933a-204">인수:</span><span class="sxs-lookup"><span data-stu-id="1933a-204">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="1933a-205">\<이름 &GT;</span><span class="sxs-lookup"><span data-stu-id="1933a-205">\<NAME></span></span> | <span data-ttu-id="1933a-206">마이그레이션의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-206">The name of the migration.</span></span> |

<span data-ttu-id="1933a-207">옵션:</span><span class="sxs-lookup"><span data-stu-id="1933a-207">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1933a-208"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="1933a-208"><nobr>-o</nobr></span></span> | <span data-ttu-id="1933a-209"><nobr>--output-dir \<PATH></nobr></span><span class="sxs-lookup"><span data-stu-id="1933a-209"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="1933a-210">사용할 디렉터리 (및 하위 네임 스페이스).</span><span class="sxs-lookup"><span data-stu-id="1933a-210">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="1933a-211">경로 프로젝트 디렉터리에 상대적입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-211">Paths are relative to the project directory.</span></span> <span data-ttu-id="1933a-212">기본값은 "마이그레이션"입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-212">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="1933a-213">dotnet ef 마이그레이션 목록</span><span class="sxs-lookup"><span data-stu-id="1933a-213">dotnet ef migrations list</span></span>

<span data-ttu-id="1933a-214">사용 가능한 마이그레이션을 나열합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-214">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="1933a-215">dotnet ef 마이그레이션 제거</span><span class="sxs-lookup"><span data-stu-id="1933a-215">dotnet ef migrations remove</span></span>

<span data-ttu-id="1933a-216">마지막 마이그레이션을 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-216">Removes the last migration.</span></span>

<span data-ttu-id="1933a-217">옵션:</span><span class="sxs-lookup"><span data-stu-id="1933a-217">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="1933a-218">-f</span><span class="sxs-lookup"><span data-stu-id="1933a-218">-f</span></span> | <span data-ttu-id="1933a-219">-force</span><span class="sxs-lookup"><span data-stu-id="1933a-219">--force</span></span> | <span data-ttu-id="1933a-220">데이터베이스에 적용 된 경우 마이그레이션을 되돌립니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-220">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="1933a-221">dotnet ef 마이그레이션 스크립트</span><span class="sxs-lookup"><span data-stu-id="1933a-221">dotnet ef migrations script</span></span>

<span data-ttu-id="1933a-222">마이그레이션의 SQL 스크립트를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-222">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="1933a-223">인수:</span><span class="sxs-lookup"><span data-stu-id="1933a-223">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="1933a-224">\<FROM></span><span class="sxs-lookup"><span data-stu-id="1933a-224">\<FROM></span></span> | <span data-ttu-id="1933a-225">마이그레이션을 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-225">The starting migration.</span></span> <span data-ttu-id="1933a-226">기본값은 0 (초기 데이터베이스)입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-226">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="1933a-227">\<TO></span><span class="sxs-lookup"><span data-stu-id="1933a-227">\<TO></span></span>   | <span data-ttu-id="1933a-228">끝의 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-228">The ending migration.</span></span> <span data-ttu-id="1933a-229">마지막으로 마이그레이션을 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-229">Defaults to the last migration.</span></span>         |

<span data-ttu-id="1933a-230">옵션:</span><span class="sxs-lookup"><span data-stu-id="1933a-230">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="1933a-231">-o</span><span class="sxs-lookup"><span data-stu-id="1933a-231">-o</span></span> | <span data-ttu-id="1933a-232">-출력 \<파일 ></span><span class="sxs-lookup"><span data-stu-id="1933a-232">--output \<FILE></span></span> | <span data-ttu-id="1933a-233">결과를 쓸 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-233">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="1933a-234">-i</span><span class="sxs-lookup"><span data-stu-id="1933a-234">-i</span></span> | <span data-ttu-id="1933a-235">-idempotent</span><span class="sxs-lookup"><span data-stu-id="1933a-235">--idempotent</span></span>     | <span data-ttu-id="1933a-236">모든 마이그레이션에는 데이터베이스에서 사용할 수 있는 스크립트를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="1933a-236">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
