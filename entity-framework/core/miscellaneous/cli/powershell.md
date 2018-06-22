---
title: EF 코어 패키지 관리자 콘솔 (Visual Studio)
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: a53455a78db4bc504c45abafdacf9a15381f608e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812562"
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="65801-102">EF 코어 패키지 관리자 콘솔 도구</span><span class="sxs-lookup"><span data-stu-id="65801-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="65801-103">NuGet의를 사용 하 여 Visual Studio 내에서 EF 코어 패키지 관리자 콘솔 (PMC) 도구 실행 [패키지 관리자 콘솔][2]합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="65801-104">이러한 도구는 .NET Core와 .NET Framework 프로젝트 모두에서 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="65801-105">Visual Studio를 사용 하지?</span><span class="sxs-lookup"><span data-stu-id="65801-105">Not using Visual Studio?</span></span> <span data-ttu-id="65801-106">[EF 핵심 명령줄 도구] [ 1] 크로스 플랫폼 및 명령 프롬프트 내 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="65801-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="65801-107">도구 설치</span><span class="sxs-lookup"><span data-stu-id="65801-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="65801-108">Microsoft.EntityFrameworkCore.Tools NuGet 패키지를 설치 하 여 EF 코어 패키지 관리자 콘솔 도구를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="65801-109">내부 다음 명령을 실행 하 여 설치할 수 있습니다 [패키지 관리자 콘솔][2]합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="65801-110">올바르게 작동 했던이 명령을 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="65801-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="65801-111">시작 프로젝트 대상이.NET 표준 [크로스-지원 되는 프레임 워크를 대상] [ 3] 도구를 사용 하기 전에.</span><span class="sxs-lookup"><span data-stu-id="65801-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="65801-112">사용 중인 경우 **유니버설 Windows** 또는 **Xamarin**, EF 코드는 표준.NET 클래스 라이브러리를 이동 하 고 [크로스-지원 되는 프레임 워크를 대상] [ 3] 도구를 사용 하기 전에.</span><span class="sxs-lookup"><span data-stu-id="65801-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="65801-113">시작 프로젝트로 클래스 라이브러리를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="65801-114">도구를 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="65801-114">Using the tools</span></span>
---------------
<span data-ttu-id="65801-115">명령을 실행할 때마다 두 개의 프로젝트가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="65801-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="65801-116">대상 프로젝트에는 모든 파일이 추가됩니다(또는 일부 경우 제거됨).</span><span class="sxs-lookup"><span data-stu-id="65801-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="65801-117">기본적으로 대상 프로젝트는 **기본 프로젝트** 패키지 관리자 콘솔에서 선택 되었지만 지정할 수도 있습니다를 사용 하 여-프로젝트 매개 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="65801-118">시작 프로젝트는 프로젝트 코드를 실행할 때 도구가 에뮬레이트하는 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="65801-119">기본적으로 하나 **시작 프로젝트로 설정** 솔루션 탐색기에서 합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="65801-120">또한 지정할 수 있습니다-StartupProject 매개 변수를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="65801-121">일반 매개 변수:</span><span class="sxs-lookup"><span data-stu-id="65801-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="65801-122">직접 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="65801-122">-Context \<String></span></span>        | <span data-ttu-id="65801-123">사용할 DbContext 합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="65801-124">-프로젝트 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="65801-124">-Project \<String></span></span>        | <span data-ttu-id="65801-125">사용 하는 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-125">The project to use.</span></span>         |
| <span data-ttu-id="65801-126">-StartupProject \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="65801-126">-StartupProject \<String></span></span> | <span data-ttu-id="65801-127">시작 프로젝트 사용입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-127">The startup project to use.</span></span> |
| <span data-ttu-id="65801-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="65801-128">-Verbose</span></span>                  | <span data-ttu-id="65801-129">자세한 출력을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-129">Show verbose output.</span></span>        |

<span data-ttu-id="65801-130">사용 하 여 PowerShell의 명령에 대 한 도움말 정보를 표시 하려면 `Get-Help` 명령입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="65801-131">컨텍스트, 프로젝트 및 StartupProject 매개 변수 탭 확장을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="65801-132">설정 **env:ASPNETCORE_ENVIRONMENT** ASP.NET Core을 실행 하기 전에.</span><span class="sxs-lookup"><span data-stu-id="65801-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="65801-133">명령</span><span class="sxs-lookup"><span data-stu-id="65801-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="65801-134">추가 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="65801-134">Add-Migration</span></span>

<span data-ttu-id="65801-135">새 마이그레이션을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-135">Adds a new migration.</span></span>

<span data-ttu-id="65801-136">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="65801-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="65801-137">***-이름*** \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="65801-137">***-Name*** \<String></span></span>             | <span data-ttu-id="65801-138">마이그레이션의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="65801-139"><nobr>-OutputDir \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="65801-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="65801-140">사용할 디렉터리 (및 하위 네임 스페이스).</span><span class="sxs-lookup"><span data-stu-id="65801-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="65801-141">경로 프로젝트 디렉터리에 상대적입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="65801-142">기본값은 "마이그레이션"입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="65801-143">매개 변수에서 **b o l** 필요한 및에서 *기울임꼴* 위치는입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="65801-144">데이터베이스를 삭제</span><span class="sxs-lookup"><span data-stu-id="65801-144">Drop-Database</span></span>

<span data-ttu-id="65801-145">데이터베이스를 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-145">Drops the database.</span></span>

<span data-ttu-id="65801-146">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="65801-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="65801-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="65801-147">-WhatIf</span></span> | <span data-ttu-id="65801-148">표시는 데이터베이스는 삭제할 수 없지만 삭제 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="65801-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="65801-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="65801-149">Get-DbContext</span></span>

<span data-ttu-id="65801-150">DbContext 형식에 대 한 정보를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="65801-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="65801-151">제거 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="65801-151">Remove-Migration</span></span>

<span data-ttu-id="65801-152">마지막 마이그레이션을 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-152">Removes the last migration.</span></span>

<span data-ttu-id="65801-153">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="65801-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="65801-154">대입</span><span class="sxs-lookup"><span data-stu-id="65801-154">-Force</span></span> | <span data-ttu-id="65801-155">데이터베이스에 적용 된 경우 마이그레이션을 되돌립니다.</span><span class="sxs-lookup"><span data-stu-id="65801-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="65801-156">스 캐 폴드 DbContext</span><span class="sxs-lookup"><span data-stu-id="65801-156">Scaffold-DbContext</span></span>

<span data-ttu-id="65801-157">스 캐 폴드 데이터베이스에 대 한 DbContext 및 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="65801-158">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="65801-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="65801-159"><nobr>***-연결*** \<문자열 ></nobr></span><span class="sxs-lookup"><span data-stu-id="65801-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="65801-160">데이터베이스에 연결 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="65801-161">***-공급자*** \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="65801-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="65801-162">사용 하는 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-162">The provider to use.</span></span> <span data-ttu-id="65801-163">(예:</span><span class="sxs-lookup"><span data-stu-id="65801-163">(E.g.</span></span> <span data-ttu-id="65801-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="65801-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>                              |
| <span data-ttu-id="65801-165">-출력 디렉터리 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="65801-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="65801-166">에 파일을 넣을 디렉터리입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-166">The directory to put files in.</span></span> <span data-ttu-id="65801-167">경로 프로젝트 디렉터리에 상대적입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-167">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="65801-168">-ContextDir \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="65801-168">-ContextDir \<String></span></span>                    | <span data-ttu-id="65801-169">디렉터리에 DbContext 파일을 넣습니다.</span><span class="sxs-lookup"><span data-stu-id="65801-169">The directory to put DbContext file in.</span></span> <span data-ttu-id="65801-170">경로 프로젝트 디렉터리에 상대적입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-170">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="65801-171">직접 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="65801-171">-Context \<String></span></span>                       | <span data-ttu-id="65801-172">생성할 DbContext의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-172">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="65801-173">-스키마 \<String ></span><span class="sxs-lookup"><span data-stu-id="65801-173">-Schemas \<String[]></span></span>                     | <span data-ttu-id="65801-174">에 대 한 엔터티 형식을 생성 하는 테이블의 스키마입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-174">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="65801-175">-테이블 \<String ></span><span class="sxs-lookup"><span data-stu-id="65801-175">-Tables \<String[]></span></span>                      | <span data-ttu-id="65801-176">엔터티 형식에 대 한 생성을 위한 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-176">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="65801-177">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="65801-177">-DataAnnotations</span></span>                         | <span data-ttu-id="65801-178">(사용 가능한) 위치 모델을 구성 하려면 특성을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-178">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="65801-179">생략 하면 fluent API만 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="65801-179">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="65801-180">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="65801-180">-UseDatabaseNames</span></span>                        | <span data-ttu-id="65801-181">데이터베이스에서 직접 테이블 및 열 이름을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-181">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="65801-182">대입</span><span class="sxs-lookup"><span data-stu-id="65801-182">-Force</span></span>                                   | <span data-ttu-id="65801-183">기존 파일을 덮어씁니다.</span><span class="sxs-lookup"><span data-stu-id="65801-183">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="65801-184">스크립트 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="65801-184">Script-Migration</span></span>

<span data-ttu-id="65801-185">마이그레이션의 SQL 스크립트를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-185">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="65801-186">매개 변수:</span><span class="sxs-lookup"><span data-stu-id="65801-186">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="65801-187"> \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="65801-187">*-From* \<String></span></span> | <span data-ttu-id="65801-188">마이그레이션을 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-188">The starting migration.</span></span> <span data-ttu-id="65801-189">기본값은 0 (초기 데이터베이스)입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-189">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="65801-190">*이것을* \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="65801-190">*-To* \<String></span></span>   | <span data-ttu-id="65801-191">끝의 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-191">The ending migration.</span></span> <span data-ttu-id="65801-192">마지막으로 마이그레이션을 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-192">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="65801-193">-Idempotent</span><span class="sxs-lookup"><span data-stu-id="65801-193">-Idempotent</span></span>       | <span data-ttu-id="65801-194">모든 마이그레이션에는 데이터베이스에서 사용할 수 있는 스크립트를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-194">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="65801-195">-출력 \<문자열 ></span><span class="sxs-lookup"><span data-stu-id="65801-195">-Output \<String></span></span> | <span data-ttu-id="65801-196">결과를 쓸 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-196">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="65801-197">To, From, 출력 매개 변수 탭 확장을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-197">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="65801-198">데이터베이스 업데이트</span><span class="sxs-lookup"><span data-stu-id="65801-198">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="65801-199"><nobr>*-마이그레이션* \<문자열 ></nobr></span><span class="sxs-lookup"><span data-stu-id="65801-199"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="65801-200">대상 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-200">The target migration.</span></span> <span data-ttu-id="65801-201">'0' 인 경우 모든 마이그레이션 되돌립니다.</span><span class="sxs-lookup"><span data-stu-id="65801-201">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="65801-202">마지막으로 마이그레이션을 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="65801-202">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="65801-203">마이그레이션 매개 변수 탭 확장을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="65801-203">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
