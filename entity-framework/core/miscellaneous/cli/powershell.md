---
title: 패키지 관리자 콘솔 (Visual Studio)-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: 3d57a1665da2c94c55981d17e041b0ef74282496
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490854"
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="93516-102">EF Core 패키지 관리자 콘솔 도구</span><span class="sxs-lookup"><span data-stu-id="93516-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="93516-103">NuGet의를 사용 하 여 Visual Studio 내에서 EF Core 패키지 관리자 콘솔 (PMC) 도구를 실행할 [패키지 관리자 콘솔][2]합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="93516-104">이러한 도구는 .NET Core와 .NET Framework 프로젝트 모두에서 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="93516-105">Visual Studio를 사용 하지 않나요?</span><span class="sxs-lookup"><span data-stu-id="93516-105">Not using Visual Studio?</span></span> <span data-ttu-id="93516-106">합니다 [EF Core 명령줄 도구] [ 1] 크로스 플랫폼 및 명령 프롬프트 내에서 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="93516-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="93516-107">도구 설치</span><span class="sxs-lookup"><span data-stu-id="93516-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="93516-108">Microsoft.EntityFrameworkCore.Tools NuGet 패키지를 설치 하 여 EF Core 패키지 관리자 콘솔 도구를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="93516-109">내에서 다음 명령을 실행 하 여 설치할 수 있습니다 [패키지 관리자 콘솔][2]합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="93516-110">모든 항목이 제대로 작동 하는 경우이 명령을 실행 하려면 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="93516-111">시작 프로젝트를.NET Standard를 대상으로 하는 경우 [교차 대상을 지원 되는 프레임 워크] [ 3] 도구를 사용 하기 전에 합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="93516-112">사용 중인 경우 **유니버설 Windows** 또는 **Xamarin**, EF 코드를.NET Standard 클래스 라이브러리를 이동 하 고 [교차 대상을 지원 되는 프레임 워크] [ 3] 도구를 사용 하기 전에 합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="93516-113">시작 프로젝트로 클래스 라이브러리를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="93516-114">도구를 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="93516-114">Using the tools</span></span>
---------------
<span data-ttu-id="93516-115">명령을 호출할 때마다 두 개의 프로젝트가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="93516-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="93516-116">대상 프로젝트에는 모든 파일이 추가됩니다(또는 일부 경우 제거됨).</span><span class="sxs-lookup"><span data-stu-id="93516-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="93516-117">기본값은 대상 프로젝트를 **기본 프로젝트** 패키지 관리자 콘솔에서 선택 되었지만 사용 하 여 지정할 수 있습니다-프로젝트 매개 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="93516-118">시작 프로젝트는 프로젝트 코드를 실행할 때 도구가 에뮬레이트하는 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="93516-119">하나 기본값으로 **시작 프로젝트로 설정** 솔루션 탐색기에서.</span><span class="sxs-lookup"><span data-stu-id="93516-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="93516-120">도 지정할 수 있습니다-StartupProject 매개 변수를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="93516-121">일반 매개 변수:</span><span class="sxs-lookup"><span data-stu-id="93516-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="93516-122">상황에 맞는 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="93516-122">-Context \<String></span></span>        | <span data-ttu-id="93516-123">사용 하 여 DbContext 합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="93516-124">-프로젝트 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="93516-124">-Project \<String></span></span>        | <span data-ttu-id="93516-125">프로젝트를 사용 하는입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-125">The project to use.</span></span>         |
| <span data-ttu-id="93516-126">-StartupProject \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="93516-126">-StartupProject \<String></span></span> | <span data-ttu-id="93516-127">시작 프로젝트 사용입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-127">The startup project to use.</span></span> |
| <span data-ttu-id="93516-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="93516-128">-Verbose</span></span>                  | <span data-ttu-id="93516-129">자세한 정보 출력을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-129">Show verbose output.</span></span>        |

<span data-ttu-id="93516-130">명령에 대 한 도움말 정보를 표시 하려면 PowerShell의 사용 하 여 `Get-Help` 명령입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="93516-131">상황에 맞는, 프로젝트 및 StartupProject 매개 변수 탭 확장을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="93516-132">설정할 **env:ASPNETCORE_ENVIRONMENT** ASP.NET Core 환경을 지정을 실행 하기 전에 합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="93516-133">명령</span><span class="sxs-lookup"><span data-stu-id="93516-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="93516-134">추가 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="93516-134">Add-Migration</span></span>

<span data-ttu-id="93516-135">새 마이그레이션을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-135">Adds a new migration.</span></span>

<span data-ttu-id="93516-136">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="93516-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="93516-137">***-Name*** \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="93516-137">***-Name*** \<String></span></span>             | <span data-ttu-id="93516-138">마이그레이션 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="93516-139"><nobr>-OutputDir \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="93516-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="93516-140">사용 하 여 디렉터리 (및 하위 네임 스페이스).</span><span class="sxs-lookup"><span data-stu-id="93516-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="93516-141">프로젝트 디렉터리를 기준으로 경로입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="93516-142">기본값은 "마이그레이션"입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="93516-143">매개 변수 **굵게** 필요에 및 *기울임꼴* 위치는입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="93516-144">데이터베이스 삭제</span><span class="sxs-lookup"><span data-stu-id="93516-144">Drop-Database</span></span>

<span data-ttu-id="93516-145">데이터베이스를 삭제합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-145">Drops the database.</span></span>

<span data-ttu-id="93516-146">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="93516-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="93516-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="93516-147">-WhatIf</span></span> | <span data-ttu-id="93516-148">데이터베이스는 삭제할 수 있지만 삭제 하지는 마세요 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="93516-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="93516-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="93516-149">Get-DbContext</span></span>

<span data-ttu-id="93516-150">DbContext 유형에 대 한 정보를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="93516-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="93516-151">마이그레이션 제거</span><span class="sxs-lookup"><span data-stu-id="93516-151">Remove-Migration</span></span>

<span data-ttu-id="93516-152">마지막 마이그레이션을 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-152">Removes the last migration.</span></span>

<span data-ttu-id="93516-153">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="93516-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="93516-154">-Force</span><span class="sxs-lookup"><span data-stu-id="93516-154">-Force</span></span> | <span data-ttu-id="93516-155">데이터베이스에 적용 한 경우 마이그레이션을 되돌립니다.</span><span class="sxs-lookup"><span data-stu-id="93516-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="93516-156">DbContext 스 캐 폴드</span><span class="sxs-lookup"><span data-stu-id="93516-156">Scaffold-DbContext</span></span>

<span data-ttu-id="93516-157">데이터베이스는 DbContext와 엔터티 형식을 스 캐 폴딩 합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="93516-158">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="93516-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="93516-159"><nobr>***-연결*** \<문자열 ></nobr></span><span class="sxs-lookup"><span data-stu-id="93516-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="93516-160">데이터베이스에 연결 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="93516-161">***공급자*** \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="93516-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="93516-162">공급자에서 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-162">The provider to use.</span></span> <span data-ttu-id="93516-163">(예: Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="93516-163">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span>                      |
| <span data-ttu-id="93516-164">-OutputDir \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="93516-164">-OutputDir \<String></span></span>                     | <span data-ttu-id="93516-165">디렉터리에 파일 보관입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-165">The directory to put files in.</span></span> <span data-ttu-id="93516-166">프로젝트 디렉터리를 기준으로 경로입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-166">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="93516-167">-ContextDir \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="93516-167">-ContextDir \<String></span></span>                    | <span data-ttu-id="93516-168">에 DbContext 파일을 배치할 디렉터리입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-168">The directory to put DbContext file in.</span></span> <span data-ttu-id="93516-169">프로젝트 디렉터리를 기준으로 경로입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-169">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="93516-170">상황에 맞는 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="93516-170">-Context \<String></span></span>                       | <span data-ttu-id="93516-171">생성할 DbContext의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-171">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="93516-172">-스키마 \<String ></span><span class="sxs-lookup"><span data-stu-id="93516-172">-Schemas \<String[]></span></span>                     | <span data-ttu-id="93516-173">에 대 한 엔터티 형식을 생성 하는 테이블의 스키마입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-173">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="93516-174">-테이블 \<String ></span><span class="sxs-lookup"><span data-stu-id="93516-174">-Tables \<String[]></span></span>                      | <span data-ttu-id="93516-175">에 대 한 엔터티 형식을 생성 하는 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-175">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="93516-176">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="93516-176">-DataAnnotations</span></span>                         | <span data-ttu-id="93516-177">(가능한 한) 경우 모델을 구성 하려면 특성을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-177">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="93516-178">생략 하면 fluent API만 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="93516-178">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="93516-179">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="93516-179">-UseDatabaseNames</span></span>                        | <span data-ttu-id="93516-180">데이터베이스에서 직접 테이블 및 열 이름을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-180">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="93516-181">-Force</span><span class="sxs-lookup"><span data-stu-id="93516-181">-Force</span></span>                                   | <span data-ttu-id="93516-182">기존 파일을 덮어씁니다.</span><span class="sxs-lookup"><span data-stu-id="93516-182">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="93516-183">스크립트 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="93516-183">Script-Migration</span></span>

<span data-ttu-id="93516-184">마이그레이션에서 SQL 스크립트를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-184">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="93516-185">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="93516-185">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="93516-186"> \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="93516-186">*-From* \<String></span></span> | <span data-ttu-id="93516-187">마이그레이션을 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-187">The starting migration.</span></span> <span data-ttu-id="93516-188">기본값은 0 (초기 데이터베이스)입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-188">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="93516-189">*-에* \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="93516-189">*-To* \<String></span></span>   | <span data-ttu-id="93516-190">끝 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-190">The ending migration.</span></span> <span data-ttu-id="93516-191">기본값 마지막 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-191">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="93516-192">멱 등</span><span class="sxs-lookup"><span data-stu-id="93516-192">-Idempotent</span></span>       | <span data-ttu-id="93516-193">모든 마이그레이션 데이터베이스에서 사용할 수 있는 스크립트를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-193">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="93516-194">-출력 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="93516-194">-Output \<String></span></span> | <span data-ttu-id="93516-195">결과를 쓸 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-195">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="93516-196">및 출력 매개 변수 탭 확장을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-196">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="93516-197">데이터베이스 업데이트</span><span class="sxs-lookup"><span data-stu-id="93516-197">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="93516-198"><nobr>*마이그레이션* \<문자열 ></nobr></span><span class="sxs-lookup"><span data-stu-id="93516-198"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="93516-199">대상 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-199">The target migration.</span></span> <span data-ttu-id="93516-200">'0' 인 경우 모든 마이그레이션 되돌려집니다.</span><span class="sxs-lookup"><span data-stu-id="93516-200">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="93516-201">기본값 마지막 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="93516-201">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="93516-202">마이그레이션 매개 변수 탭 확장을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="93516-202">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
