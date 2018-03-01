---
title: "EF 코어 패키지 관리자 콘솔 (Visual Studio)"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: aacf8c8564a3966db6202c9ff1c1c02a19a10814
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="6c089-102">EF 코어 패키지 관리자 콘솔 도구</span><span class="sxs-lookup"><span data-stu-id="6c089-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="6c089-103">NuGet의를 사용 하 여 Visual Studio 내에서 EF 코어 패키지 관리자 콘솔 (PMC) 도구 실행 [패키지 관리자 콘솔][2]합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="6c089-104">이러한 도구는 .NET Core와 .NET Framework 프로젝트 모두에서 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="6c089-105">Visual Studio를 사용 하지?</span><span class="sxs-lookup"><span data-stu-id="6c089-105">Not using Visual Studio?</span></span> <span data-ttu-id="6c089-106">[EF 핵심 명령줄 도구] [ 1] 크로스 플랫폼 및 명령 프롬프트 내 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="6c089-107">도구 설치</span><span class="sxs-lookup"><span data-stu-id="6c089-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="6c089-108">Microsoft.EntityFrameworkCore.Tools NuGet 패키지를 설치 하 여 EF 코어 패키지 관리자 콘솔 도구를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="6c089-109">내부 다음 명령을 실행 하 여 설치할 수 있습니다 [패키지 관리자 콘솔][2]합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="6c089-110">올바르게 작동 했던이 명령을 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="6c089-111">시작 프로젝트 대상이.NET 표준 [크로스-지원 되는 프레임 워크를 대상] [ 3] 도구를 사용 하기 전에.</span><span class="sxs-lookup"><span data-stu-id="6c089-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6c089-112">사용 중인 경우 **유니버설 Windows** 또는 **Xamarin**, EF 코드는 표준.NET 클래스 라이브러리를 이동 하 고 [크로스-지원 되는 프레임 워크를 대상] [ 3] 도구를 사용 하기 전에.</span><span class="sxs-lookup"><span data-stu-id="6c089-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="6c089-113">시작 프로젝트로 클래스 라이브러리를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="6c089-114">도구를 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="6c089-114">Using the tools</span></span>
---------------
<span data-ttu-id="6c089-115">명령을 실행할 때마다 두 개의 프로젝트가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="6c089-116">대상 프로젝트에는 모든 파일이 추가됩니다(또는 일부 경우 제거됨).</span><span class="sxs-lookup"><span data-stu-id="6c089-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="6c089-117">기본적으로 대상 프로젝트는 **기본 프로젝트** 패키지 관리자 콘솔에서 선택 되었지만 지정할 수도 있습니다를 사용 하 여-프로젝트 매개 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="6c089-118">시작 프로젝트는 프로젝트 코드를 실행할 때 도구가 에뮬레이트하는 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="6c089-119">기본적으로 하나 **시작 프로젝트로 설정** 솔루션 탐색기에서 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="6c089-120">또한 지정할 수 있습니다-StartupProject 매개 변수를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="6c089-121">일반 매개 변수:</span><span class="sxs-lookup"><span data-stu-id="6c089-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="6c089-122">직접 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="6c089-122">-Context \<String></span></span>        | <span data-ttu-id="6c089-123">사용할 DbContext 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="6c089-124">-프로젝트 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="6c089-124">-Project \<String></span></span>        | <span data-ttu-id="6c089-125">사용 하는 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-125">The project to use.</span></span>         |
| <span data-ttu-id="6c089-126">-StartupProject \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="6c089-126">-StartupProject \<String></span></span> | <span data-ttu-id="6c089-127">시작 프로젝트 사용입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-127">The startup project to use.</span></span> |
| <span data-ttu-id="6c089-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="6c089-128">-Verbose</span></span>                  | <span data-ttu-id="6c089-129">자세한 출력을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-129">Show verbose output.</span></span>        |

<span data-ttu-id="6c089-130">사용 하 여 PowerShell의 명령에 대 한 도움말 정보를 표시 하려면 `Get-Help` 명령입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="6c089-131">컨텍스트, 프로젝트 및 StartupProject 매개 변수 탭 확장을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="6c089-132">설정 **env:ASPNETCORE_ENVIRONMENT** ASP.NET Core을 실행 하기 전에.</span><span class="sxs-lookup"><span data-stu-id="6c089-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="6c089-133">명령</span><span class="sxs-lookup"><span data-stu-id="6c089-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="6c089-134">추가 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="6c089-134">Add-Migration</span></span>

<span data-ttu-id="6c089-135">새 마이그레이션을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-135">Adds a new migration.</span></span>

<span data-ttu-id="6c089-136">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="6c089-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6c089-137">***-이름*** \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="6c089-137">***-Name*** \<String></span></span>             | <span data-ttu-id="6c089-138">마이그레이션의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="6c089-139"><nobr>-OutputDir \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="6c089-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="6c089-140">사용할 디렉터리 (및 하위 네임 스페이스).</span><span class="sxs-lookup"><span data-stu-id="6c089-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="6c089-141">경로 프로젝트 디렉터리에 상대적입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="6c089-142">기본값은 "마이그레이션"입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="6c089-143">매개 변수에서 **b o l** 필요한 및에서 *기울임꼴* 위치는입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="6c089-144">데이터베이스를 삭제</span><span class="sxs-lookup"><span data-stu-id="6c089-144">Drop-Database</span></span>

<span data-ttu-id="6c089-145">데이터베이스를 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-145">Drops the database.</span></span>

<span data-ttu-id="6c089-146">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="6c089-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="6c089-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="6c089-147">-WhatIf</span></span> | <span data-ttu-id="6c089-148">표시는 데이터베이스는 삭제할 수 없지만 삭제 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="6c089-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="6c089-149">Get-DbContext</span></span>

<span data-ttu-id="6c089-150">DbContext 형식에 대 한 정보를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="6c089-151">제거 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="6c089-151">Remove-Migration</span></span>

<span data-ttu-id="6c089-152">마지막 마이그레이션을 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-152">Removes the last migration.</span></span>

<span data-ttu-id="6c089-153">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="6c089-153">Parameters:</span></span>

|        |                                                                       |
|:-------|:----------------------------------------------------------------------|
| <span data-ttu-id="6c089-154">대입</span><span class="sxs-lookup"><span data-stu-id="6c089-154">-Force</span></span> | <span data-ttu-id="6c089-155">마이그레이션 데이터베이스에 적용 된 경우 참조를 확인 하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="6c089-155">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="6c089-156">스 캐 폴드 DbContext</span><span class="sxs-lookup"><span data-stu-id="6c089-156">Scaffold-DbContext</span></span>

<span data-ttu-id="6c089-157">스 캐 폴드 데이터베이스에 대 한 DbContext 및 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="6c089-158">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="6c089-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6c089-159"><nobr>***-연결*** \<문자열 ></nobr></span><span class="sxs-lookup"><span data-stu-id="6c089-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="6c089-160">데이터베이스에 연결 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="6c089-161">***-Provider*** \<String></span><span class="sxs-lookup"><span data-stu-id="6c089-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="6c089-162">사용 하는 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-162">The provider to use.</span></span> <span data-ttu-id="6c089-163">(예:</span><span class="sxs-lookup"><span data-stu-id="6c089-163">(E.g.</span></span> <span data-ttu-id="6c089-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="6c089-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>                              |
| <span data-ttu-id="6c089-165">-출력 디렉터리 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="6c089-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="6c089-166">에 파일을 넣을 디렉터리입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-166">The directory to put files in.</span></span> <span data-ttu-id="6c089-167">경로 프로젝트 디렉터리에 상대적입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-167">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="6c089-168">직접 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="6c089-168">-Context \<String></span></span>                       | <span data-ttu-id="6c089-169">생성할 DbContext의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-169">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="6c089-170">-Schemas \<String[]></span><span class="sxs-lookup"><span data-stu-id="6c089-170">-Schemas \<String[]></span></span>                     | <span data-ttu-id="6c089-171">에 대 한 엔터티 형식을 생성 하는 테이블의 스키마입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-171">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="6c089-172">-테이블 \<String ></span><span class="sxs-lookup"><span data-stu-id="6c089-172">-Tables \<String[]></span></span>                      | <span data-ttu-id="6c089-173">엔터티 형식에 대 한 생성을 위한 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-173">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="6c089-174">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="6c089-174">-DataAnnotations</span></span>                         | <span data-ttu-id="6c089-175">(사용 가능한) 위치 모델을 구성 하려면 특성을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-175">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="6c089-176">생략 하면 fluent API만 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-176">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="6c089-177">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="6c089-177">-UseDatabaseNames</span></span>                        | <span data-ttu-id="6c089-178">데이터베이스에서 직접 테이블 및 열 이름을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-178">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="6c089-179">대입</span><span class="sxs-lookup"><span data-stu-id="6c089-179">-Force</span></span>                                   | <span data-ttu-id="6c089-180">기존 파일을 덮어씁니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-180">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="6c089-181">스크립트 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="6c089-181">Script-Migration</span></span>

<span data-ttu-id="6c089-182">마이그레이션의 SQL 스크립트를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-182">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="6c089-183">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="6c089-183">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="6c089-184">*-From* \<String></span><span class="sxs-lookup"><span data-stu-id="6c089-184">*-From* \<String></span></span> | <span data-ttu-id="6c089-185">마이그레이션을 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-185">The starting migration.</span></span> <span data-ttu-id="6c089-186">기본값은 0 (초기 데이터베이스)입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-186">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="6c089-187">*이것을* \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="6c089-187">*-To* \<String></span></span>   | <span data-ttu-id="6c089-188">끝의 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-188">The ending migration.</span></span> <span data-ttu-id="6c089-189">마지막으로 마이그레이션을 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-189">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="6c089-190">-Idempotent</span><span class="sxs-lookup"><span data-stu-id="6c089-190">-Idempotent</span></span>       | <span data-ttu-id="6c089-191">모든 마이그레이션에는 데이터베이스에서 사용할 수 있는 스크립트를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-191">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="6c089-192">-출력 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="6c089-192">-Output \<String></span></span> | <span data-ttu-id="6c089-193">결과를 쓸 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-193">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="6c089-194">To, From, 출력 매개 변수 탭 확장을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-194">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="6c089-195">데이터베이스 업데이트</span><span class="sxs-lookup"><span data-stu-id="6c089-195">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="6c089-196"><nobr>*-마이그레이션* \<문자열 ></nobr></span><span class="sxs-lookup"><span data-stu-id="6c089-196"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="6c089-197">대상 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-197">The target migration.</span></span> <span data-ttu-id="6c089-198">'0' 인 경우 모든 마이그레이션 되돌립니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-198">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="6c089-199">마지막으로 마이그레이션을 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-199">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="6c089-200">마이그레이션 매개 변수 탭 확장을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="6c089-200">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
