---
title: ".NET core CLI-EF 코어"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 26b5fb326d20575ed2f3c6955c699e0c3757bf57
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="443fa-102">EF 핵심.NET 명령줄 도구</span><span class="sxs-lookup"><span data-stu-id="443fa-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="443fa-103">Entity Framework Core.NET 명령줄 도구는 플랫폼 간에 대 한 확장 **dotnet** 명령에 포함 된의 [.NET Core SDK][2]합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="443fa-104">권장 하는 경우에 Visual Studio를 사용 하는, [PMC 도구] [ 1] 대신 더 통합 된 환경을 제공 하므로 합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="443fa-105">도구 설치</span><span class="sxs-lookup"><span data-stu-id="443fa-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="443fa-106">다음이 단계를 사용 하 여 EF 코어.NET 명령줄 도구를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="443fa-107">프로젝트 파일을 편집 하 고 Microsoft.EntityFrameworkCore.Tools.DotNet DotNetCliToolReference 항목 (아래 참조)로 추가</span><span class="sxs-lookup"><span data-stu-id="443fa-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="443fa-108">다음 명령을 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="443fa-109">결과 프로젝트 코드는 다음과 같아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="443fa-110">사용 하 여 패키지 참조 `PrivateAssets="All"` 의미는 일반적으로 개발 하는 동안에 사용 되는 패키지에 대 한 특히 유용이 프로젝트를 참조 하는 프로젝트에 노출 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="443fa-111">모든 것이 올바르게를 수행한 경우에 명령 프롬프트에서 다음 명령을 실행 할 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="443fa-112">도구를 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="443fa-112">Using the tools</span></span>
---------------
<span data-ttu-id="443fa-113">명령을 실행할 때마다 두 개의 프로젝트가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="443fa-114">대상 프로젝트에는 모든 파일을 추가 하는 경우 (또는 경우에 따라 제거)입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="443fa-115">대상 프로젝트의 프로젝트는 현재 디렉터리에 기본적으로 사용 되지만 사용 하 여 변경할 수는 <nobr> **-프로젝트** </nobr> 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="443fa-116">시작 프로젝트는 프로젝트의 코드를 실행 하는 경우 도구에서 에뮬레이트됩니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="443fa-117">사용 하 여 변경할 수 있지만 것 현재 디렉터리에 프로젝트에 기본적으로 **-시작 프로젝트** 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

<span data-ttu-id="443fa-118">일반 옵션:</span><span class="sxs-lookup"><span data-stu-id="443fa-118">Common options:</span></span>

|    |                                  |                             |
| -- | -------------------------------- | --------------------------- |
|    | <span data-ttu-id="443fa-119">-json</span><span class="sxs-lookup"><span data-stu-id="443fa-119">--json</span></span>                           | <span data-ttu-id="443fa-120">JSON 출력을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-120">Show JSON output.</span></span>           |
| <span data-ttu-id="443fa-121">-c</span><span class="sxs-lookup"><span data-stu-id="443fa-121">-c</span></span> | <span data-ttu-id="443fa-122">-상황에 맞는 \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="443fa-122">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="443fa-123">사용할 DbContext 합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="443fa-124">-P</span><span class="sxs-lookup"><span data-stu-id="443fa-124">-p</span></span> | <span data-ttu-id="443fa-125">-프로젝트 \<프로젝트 ></span><span class="sxs-lookup"><span data-stu-id="443fa-125">--project \<PROJECT></span></span>             | <span data-ttu-id="443fa-126">사용 하는 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-126">The project to use.</span></span>         |
| <span data-ttu-id="443fa-127">-s</span><span class="sxs-lookup"><span data-stu-id="443fa-127">-s</span></span> | <span data-ttu-id="443fa-128">-시작 프로젝트 \<프로젝트 ></span><span class="sxs-lookup"><span data-stu-id="443fa-128">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="443fa-129">시작 프로젝트 사용입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-129">The startup project to use.</span></span> |
|    | <span data-ttu-id="443fa-130">-프레임 워크 \<프레임 워크 ></span><span class="sxs-lookup"><span data-stu-id="443fa-130">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="443fa-131">대상 프레임 워크입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-131">The target framework.</span></span>       |
|    | <span data-ttu-id="443fa-132">-configuration \<구성 ></span><span class="sxs-lookup"><span data-stu-id="443fa-132">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="443fa-133">사용할 구성입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-133">The configuration to use.</span></span>   |
|    | <span data-ttu-id="443fa-134">-런타임 \<식별자 ></span><span class="sxs-lookup"><span data-stu-id="443fa-134">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="443fa-135">사용 하는 런타임입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-135">The runtime to use.</span></span>         |
| <span data-ttu-id="443fa-136">-h</span><span class="sxs-lookup"><span data-stu-id="443fa-136">-h</span></span> | <span data-ttu-id="443fa-137">-도움말</span><span class="sxs-lookup"><span data-stu-id="443fa-137">--help</span></span>                           | <span data-ttu-id="443fa-138">도움말 정보를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-138">Show help information.</span></span>      |
| <span data-ttu-id="443fa-139">-v</span><span class="sxs-lookup"><span data-stu-id="443fa-139">-v</span></span> | <span data-ttu-id="443fa-140">-verbose</span><span class="sxs-lookup"><span data-stu-id="443fa-140">--verbose</span></span>                        | <span data-ttu-id="443fa-141">자세한 출력을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-141">Show verbose output.</span></span>        |
|    | <span data-ttu-id="443fa-142">-색 없음</span><span class="sxs-lookup"><span data-stu-id="443fa-142">--no-color</span></span>                       | <span data-ttu-id="443fa-143">출력 색을 지정 하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="443fa-143">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="443fa-144">-접두사 출력</span><span class="sxs-lookup"><span data-stu-id="443fa-144">--prefix-output</span></span>                  | <span data-ttu-id="443fa-145">수준으로 출력 하는 접두사입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-145">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="443fa-146">ASP.NET Core 환경을 지정 하려면는 **ASPNETCORE_ENVIRONMENT** 실행 하기 전에 환경 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-146">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="443fa-147">명령</span><span class="sxs-lookup"><span data-stu-id="443fa-147">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="443fa-148">dotnet ef 데이터베이스 삭제</span><span class="sxs-lookup"><span data-stu-id="443fa-148">dotnet ef database drop</span></span>

<span data-ttu-id="443fa-149">데이터베이스를 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-149">Drops the database.</span></span>

<span data-ttu-id="443fa-150">옵션:</span><span class="sxs-lookup"><span data-stu-id="443fa-150">Options:</span></span>

|    |           |                                                          |
| -- | --------- | -------------------------------------------------------- |
| <span data-ttu-id="443fa-151">-f</span><span class="sxs-lookup"><span data-stu-id="443fa-151">-f</span></span> | <span data-ttu-id="443fa-152">-force</span><span class="sxs-lookup"><span data-stu-id="443fa-152">--force</span></span>   | <span data-ttu-id="443fa-153">확인 하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="443fa-153">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="443fa-154">--예행</span><span class="sxs-lookup"><span data-stu-id="443fa-154">--dry-run</span></span> | <span data-ttu-id="443fa-155">표시는 데이터베이스는 삭제할 수 없지만 삭제 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-155">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="443fa-156">dotnet ef 데이터베이스 업데이트</span><span class="sxs-lookup"><span data-stu-id="443fa-156">dotnet ef database update</span></span>

<span data-ttu-id="443fa-157">지정 된 마이그레이션에는 데이터베이스를 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-157">Updates the database to a specified migration.</span></span>

<span data-ttu-id="443fa-158">인수:</span><span class="sxs-lookup"><span data-stu-id="443fa-158">Arguments:</span></span>

|              |                                                                                              |
| ------------ | ---------------------------------------------------------------------------------------------|
| <span data-ttu-id="443fa-159">\<마이그레이션 ></span><span class="sxs-lookup"><span data-stu-id="443fa-159">\<MIGRATION></span></span> | <span data-ttu-id="443fa-160">대상 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-160">The target migration.</span></span> <span data-ttu-id="443fa-161">0 인 경우, 모든 마이그레이션 되돌립니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-161">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="443fa-162">마지막으로 마이그레이션을 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-162">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="443fa-163">dotnet ef dbcontext 정보</span><span class="sxs-lookup"><span data-stu-id="443fa-163">dotnet ef dbcontext info</span></span>

<span data-ttu-id="443fa-164">DbContext 형식에 대 한 정보를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-164">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="443fa-165">dotnet ef dbcontext 목록</span><span class="sxs-lookup"><span data-stu-id="443fa-165">dotnet ef dbcontext list</span></span>

<span data-ttu-id="443fa-166">사용 가능한 DbContext 유형을 나열합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-166">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="443fa-167">dotnet ef dbcontext 스 캐 폴딩</span><span class="sxs-lookup"><span data-stu-id="443fa-167">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="443fa-168">스 캐 폴드 데이터베이스에 대 한 DbContext 및 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-168">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="443fa-169">인수:</span><span class="sxs-lookup"><span data-stu-id="443fa-169">Arguments:</span></span>

|               |                                                                     |
| ------------- | ------------------------------------------------------------------- |
| <span data-ttu-id="443fa-170">\<연결 ></span><span class="sxs-lookup"><span data-stu-id="443fa-170">\<CONNECTION></span></span> | <span data-ttu-id="443fa-171">데이터베이스에 연결 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-171">The connection string to the database.</span></span>                              |
| <span data-ttu-id="443fa-172">\<공급자 ></span><span class="sxs-lookup"><span data-stu-id="443fa-172">\<PROVIDER></span></span>   | <span data-ttu-id="443fa-173">사용 하는 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-173">The provider to use.</span></span> <span data-ttu-id="443fa-174">예를 들어</span><span class="sxs-lookup"><span data-stu-id="443fa-174">(E.g.</span></span> <span data-ttu-id="443fa-175">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="443fa-175">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="443fa-176">옵션:</span><span class="sxs-lookup"><span data-stu-id="443fa-176">Options:</span></span>

|                 |                                         |                                                          |
| --------------- | --------------------------------------- | -------------------------------------------------------- |
| <span data-ttu-id="443fa-177"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="443fa-177"><nobr>-d</nobr></span></span> |       <span data-ttu-id="443fa-178">-데이터 주석</span><span class="sxs-lookup"><span data-stu-id="443fa-178">--data-annotations</span></span>                | <span data-ttu-id="443fa-179">(사용 가능한) 위치 모델을 구성 하려면 특성을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-179">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="443fa-180">생략 하면 fluent API만 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-180">If omitted, only the fluent API is used.</span></span> |
|       <span data-ttu-id="443fa-181">-c</span><span class="sxs-lookup"><span data-stu-id="443fa-181">-c</span></span>        |       <span data-ttu-id="443fa-182">-상황에 맞는 \<이름 ></span><span class="sxs-lookup"><span data-stu-id="443fa-182">--context \<NAME></span></span>                 | <span data-ttu-id="443fa-183">DbContext의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-183">The name of the DbContext.</span></span>                               |
|       <span data-ttu-id="443fa-184">-f</span><span class="sxs-lookup"><span data-stu-id="443fa-184">-f</span></span>        |       <span data-ttu-id="443fa-185">-force</span><span class="sxs-lookup"><span data-stu-id="443fa-185">--force</span></span>                           | <span data-ttu-id="443fa-186">기존 파일을 덮어씁니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-186">Overwrite existing files.</span></span>                                |
|       <span data-ttu-id="443fa-187">-o</span><span class="sxs-lookup"><span data-stu-id="443fa-187">-o</span></span>        |       <span data-ttu-id="443fa-188">-출력-dir \<경로 ></span><span class="sxs-lookup"><span data-stu-id="443fa-188">--output-dir \<PATH></span></span>              | <span data-ttu-id="443fa-189">에 파일을 넣을 디렉터리입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-189">The directory to put files in.</span></span> <span data-ttu-id="443fa-190">경로 프로젝트 디렉터리에 상대적입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-190">Paths are relative to the project directory.</span></span> |
|                 | <span data-ttu-id="443fa-191"><nobr>-스키마 \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="443fa-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="443fa-192">에 대 한 엔터티 형식을 생성 하는 테이블의 스키마입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-192">The schemas of tables to generate entity types for.</span></span>      |
|       <span data-ttu-id="443fa-193">-t</span><span class="sxs-lookup"><span data-stu-id="443fa-193">-t</span></span>        |       <span data-ttu-id="443fa-194">-테이블 \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="443fa-194">--table \<TABLE_NAME>...</span></span>          | <span data-ttu-id="443fa-195">엔터티 형식에 대 한 생성을 위한 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-195">The tables to generate entity types for.</span></span>                 |
|                 |       <span data-ttu-id="443fa-196">-이름-데이터베이스-사용</span><span class="sxs-lookup"><span data-stu-id="443fa-196">--use-database-names</span></span>              | <span data-ttu-id="443fa-197">데이터베이스에서 직접 테이블 및 열 이름을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-197">Use table and column names directly from the database.</span></span>   |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="443fa-198">dotnet ef 마이그레이션 추가</span><span class="sxs-lookup"><span data-stu-id="443fa-198">dotnet ef migrations add</span></span>

<span data-ttu-id="443fa-199">새 마이그레이션을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-199">Adds a new migration.</span></span>

<span data-ttu-id="443fa-200">인수:</span><span class="sxs-lookup"><span data-stu-id="443fa-200">Arguments:</span></span>

|         |                            |
| ------- | -------------------------- |
| <span data-ttu-id="443fa-201">\<이름 ></span><span class="sxs-lookup"><span data-stu-id="443fa-201">\<NAME></span></span> | <span data-ttu-id="443fa-202">마이그레이션의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-202">The name of the migration.</span></span> |

<span data-ttu-id="443fa-203">옵션:</span><span class="sxs-lookup"><span data-stu-id="443fa-203">Options:</span></span>

|                 |                                   |                                                                |
| --------------- |---------------------------------- | -------------------------------------------------------------- |
| <span data-ttu-id="443fa-204"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="443fa-204"><nobr>-o</nobr></span></span> | <span data-ttu-id="443fa-205"><nobr>-출력-dir \<경로 ></nobr></span><span class="sxs-lookup"><span data-stu-id="443fa-205"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="443fa-206">사용할 디렉터리 (및 하위 네임 스페이스).</span><span class="sxs-lookup"><span data-stu-id="443fa-206">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="443fa-207">경로 프로젝트 디렉터리에 상대적입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-207">Paths are relative to the project directory.</span></span> <span data-ttu-id="443fa-208">기본값은 "마이그레이션"입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-208">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="443fa-209">dotnet ef 마이그레이션 목록</span><span class="sxs-lookup"><span data-stu-id="443fa-209">dotnet ef migrations list</span></span>

<span data-ttu-id="443fa-210">사용 가능한 마이그레이션을 나열합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-210">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="443fa-211">dotnet ef 마이그레이션 제거</span><span class="sxs-lookup"><span data-stu-id="443fa-211">dotnet ef migrations remove</span></span>

<span data-ttu-id="443fa-212">마지막 마이그레이션을 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-212">Removes the last migration.</span></span>

<span data-ttu-id="443fa-213">옵션:</span><span class="sxs-lookup"><span data-stu-id="443fa-213">Options:</span></span>

|    |         |                                                                       |
| -- | ------- | --------------------------------------------------------------------- |
| <span data-ttu-id="443fa-214">-f</span><span class="sxs-lookup"><span data-stu-id="443fa-214">-f</span></span> | <span data-ttu-id="443fa-215">-force</span><span class="sxs-lookup"><span data-stu-id="443fa-215">--force</span></span> | <span data-ttu-id="443fa-216">마이그레이션 데이터베이스에 적용 된 경우 참조를 확인 하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="443fa-216">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="443fa-217">dotnet ef 마이그레이션 스크립트</span><span class="sxs-lookup"><span data-stu-id="443fa-217">dotnet ef migrations script</span></span>

<span data-ttu-id="443fa-218">마이그레이션의 SQL 스크립트를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-218">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="443fa-219">인수:</span><span class="sxs-lookup"><span data-stu-id="443fa-219">Arguments:</span></span>

|         |                                                               |
| ------- | ------------------------------------------------------------- |
| <span data-ttu-id="443fa-220">\<></span><span class="sxs-lookup"><span data-stu-id="443fa-220">\<FROM></span></span> | <span data-ttu-id="443fa-221">마이그레이션을 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-221">The starting migration.</span></span> <span data-ttu-id="443fa-222">기본값은 0 (초기 데이터베이스)입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-222">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="443fa-223">\<></span><span class="sxs-lookup"><span data-stu-id="443fa-223">\<TO></span></span>   | <span data-ttu-id="443fa-224">끝의 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-224">The ending migration.</span></span> <span data-ttu-id="443fa-225">마지막으로 마이그레이션을 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-225">Defaults to the last migration.</span></span>         |

<span data-ttu-id="443fa-226">옵션:</span><span class="sxs-lookup"><span data-stu-id="443fa-226">Options:</span></span>

|    |                  |                                                                    |
| -- | ---------------- | ------------------------------------------------------------------ |
| <span data-ttu-id="443fa-227">-o</span><span class="sxs-lookup"><span data-stu-id="443fa-227">-o</span></span> | <span data-ttu-id="443fa-228">-출력 \<파일 ></span><span class="sxs-lookup"><span data-stu-id="443fa-228">--output \<FILE></span></span> | <span data-ttu-id="443fa-229">결과를 쓸 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-229">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="443fa-230">-i</span><span class="sxs-lookup"><span data-stu-id="443fa-230">-i</span></span> | <span data-ttu-id="443fa-231">-idempotent</span><span class="sxs-lookup"><span data-stu-id="443fa-231">--idempotent</span></span>     | <span data-ttu-id="443fa-232">모든 마이그레이션에는 데이터베이스에서 사용할 수 있는 스크립트를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="443fa-232">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
