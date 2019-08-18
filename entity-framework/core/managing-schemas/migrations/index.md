---
title: 마이그레이션 - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 7d97551044ae4a8fc42d1676199da884f3e2994d
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565252"
---
<a name="migrations"></a><span data-ttu-id="2daa1-102">마이그레이션</span><span class="sxs-lookup"><span data-stu-id="2daa1-102">Migrations</span></span>
==========

<span data-ttu-id="2daa1-103">데이터 모델은 개발 중에 변경되고 데이터베이스와 동기화되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-103">A data model changes during development and gets out of sync with the database.</span></span> <span data-ttu-id="2daa1-104">데이터베이스를 삭제하고 EF가 모델과 일치하는 새 데이터베이스를 만들게 할 수 있지만, 이 절차로 인해 데이터가 손실됩니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-104">You can drop the database and let EF create a new one that matches the model, but this procedure results in the loss of data.</span></span> <span data-ttu-id="2daa1-105">EF Core의 마이그레이션 기능은 데이터베이스 스키마를 증분 방식으로 업데이트하여 애플리케이션 데이터 모델과의 동기 상태를 유지하면서 데이터베이스에 기존 데이터를 보존하는 방법을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="2daa1-106">마이그레이션에는 다음 작업에 도움이 되는 명령줄 도구 및 API가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-106">Migrations includes command-line tools and APIs that help with the following tasks:</span></span>

* <span data-ttu-id="2daa1-107">[마이그레이션 만들기](#create-a-migration).</span><span class="sxs-lookup"><span data-stu-id="2daa1-107">[Create a migration](#create-a-migration).</span></span> <span data-ttu-id="2daa1-108">모델 변경 내용 집합과 동기화하도록 데이터베이스를 업데이트할 수 있는 코드를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-108">Generate code that can update the database to sync it with a set of model changes.</span></span>
* <span data-ttu-id="2daa1-109">[데이터베이스 업데이트](#update-the-database).</span><span class="sxs-lookup"><span data-stu-id="2daa1-109">[Update the database](#update-the-database).</span></span> <span data-ttu-id="2daa1-110">보류 중인 마이그레이션을 적용하여 데이터베이스 스키마를 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-110">Apply pending migrations to update the database schema.</span></span>
* <span data-ttu-id="2daa1-111">[마이그레이션 코드 사용자 지정](#customize-migration-code).</span><span class="sxs-lookup"><span data-stu-id="2daa1-111">[Customize migration code](#customize-migration-code).</span></span> <span data-ttu-id="2daa1-112">경우에 따라 생성된 코드를 수정하거나 보완해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-112">Sometimes the generated code needs to be modified or supplemented.</span></span>
* <span data-ttu-id="2daa1-113">[마이그레이션 제거](#remove-a-migration).</span><span class="sxs-lookup"><span data-stu-id="2daa1-113">[Remove a migration](#remove-a-migration).</span></span> <span data-ttu-id="2daa1-114">생성된 코드를 삭제합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-114">Delete the generated code.</span></span>
* <span data-ttu-id="2daa1-115">[마이그레이션 되돌리기](#revert-a-migration).</span><span class="sxs-lookup"><span data-stu-id="2daa1-115">[Revert a migration](#revert-a-migration).</span></span> <span data-ttu-id="2daa1-116">데이터베이스 변경 내용을 실행 취소합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-116">Undo the database changes.</span></span>
* <span data-ttu-id="2daa1-117">[SQL 스크립트 생성](#generate-sql-scripts).</span><span class="sxs-lookup"><span data-stu-id="2daa1-117">[Generate SQL scripts](#generate-sql-scripts).</span></span> <span data-ttu-id="2daa1-118">프로덕션 데이터베이스를 업데이트하거나 마이그레이션 코드 문제점을 해결하기 위한 스크립트가 필요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-118">You might need a script to update a production database or to troubleshoot migration code.</span></span>
* <span data-ttu-id="2daa1-119">[런타임에 마이그레이션 적용](#apply-migrations-at-runtime).</span><span class="sxs-lookup"><span data-stu-id="2daa1-119">[Apply migrations at runtime](#apply-migrations-at-runtime).</span></span> <span data-ttu-id="2daa1-120">디자인 타임 업데이트 및 실행 중인 스크립트가 최상의 옵션이 아닌 경우 `Migrate()` 메서드를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-120">When design-time updates and running scripts aren't the best options, call the `Migrate()` method.</span></span>

<a name="install-the-tools"></a><span data-ttu-id="2daa1-121">도구 설치</span><span class="sxs-lookup"><span data-stu-id="2daa1-121">Install the tools</span></span>
-----------------

<span data-ttu-id="2daa1-122">[명령줄 도구](xref:core/miscellaneous/cli/index)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-122">Install the [command-line tools](xref:core/miscellaneous/cli/index):</span></span>
* <span data-ttu-id="2daa1-123">Visual Studio의 경우 [패키지 관리자 콘솔 도구](xref:core/miscellaneous/cli/powershell)를 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-123">For Visual Studio, we recommend the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="2daa1-124">기타 개발 환경의 경우 [.NET Core CLI 도구](xref:core/miscellaneous/cli/dotnet)를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-124">For other development environments, choose the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span>

<a name="create-a-migration"></a><span data-ttu-id="2daa1-125">마이그레이션 만들기</span><span class="sxs-lookup"><span data-stu-id="2daa1-125">Create a migration</span></span>
------------------

<span data-ttu-id="2daa1-126">[초기 모델 정의](xref:core/modeling/index) 후에는 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-126">After you've [defined your initial model](xref:core/modeling/index), it's time to create the database.</span></span> <span data-ttu-id="2daa1-127">초기 마이그레이션을 추가하려면 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-127">To add an initial migration, run the following command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="2daa1-128">프로젝트의 **Migrations** 디렉터리 아래 3개 파일이 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-128">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="2daa1-129">**XXXXXXXXXXXXXX_InitialCreate.cs** -- 주 마이그레이션 파일.</span><span class="sxs-lookup"><span data-stu-id="2daa1-129">**XXXXXXXXXXXXXX_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="2daa1-130">마이그레이션을 적용하고(`Up()`) 변환하는 데(`Down()`) 필요한 작업을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-130">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="2daa1-131">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs** -- 마이그레이션 메타데이터 파일.</span><span class="sxs-lookup"><span data-stu-id="2daa1-131">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="2daa1-132">EF에서 사용하는 정보가 들어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-132">Contains information used by EF.</span></span>
* <span data-ttu-id="2daa1-133">**MyContextModelSnapshot.cs** - 현재 모델의 스냅숏.</span><span class="sxs-lookup"><span data-stu-id="2daa1-133">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="2daa1-134">다음 마이그레이션을 추가할 때 변경 항목을 판단하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-134">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="2daa1-135">파일 이름의 타임스탬프는 시간순으로 정렬되기 때문에 변경의 진행 상황을 파악할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-135">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="2daa1-136">마이그레이션 파일을 자유롭게 이동하고 네임스페이스를 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-136">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="2daa1-137">새 마이그레이션은 지난 번 마이그레이션의 형제 항목으로 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-137">New migrations are created as siblings of the last migration.</span></span>

<a name="update-the-database"></a><span data-ttu-id="2daa1-138">데이터베이스 업데이트</span><span class="sxs-lookup"><span data-stu-id="2daa1-138">Update the database</span></span>
-------------------

<span data-ttu-id="2daa1-139">그런 다음 스키마를 만들기 위해 데이터베이스에 마이그레이션을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-139">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="customize-migration-code"></a><span data-ttu-id="2daa1-140">마이그레이션 코드 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="2daa1-140">Customize migration code</span></span>
------------------------

<span data-ttu-id="2daa1-141">EF Core 모델을 변경한 후에는 데이터베이스 스키마가 동기화되지 않을 수 있습니다. 최신 상태로 하기 위해 다른 마이그레이션을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-141">After making changes to your EF Core model, the database schema might be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="2daa1-142">마이그레이션 이름은 버전 제어 시스템의 커밋 메시지처럼 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-142">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="2daa1-143">예를 들어 변경이 검토를 위한 새 엔터티 클래스인 경우 *AddProductReviews*와 같은 이름을 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-143">For example, you might choose a name like *AddProductReviews* if the change is a new entity class for reviews.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="2daa1-144">마이그레이션이 스캐폴드되면(마이그레이션을 위한 코드가 생성됨) 코드가 정확한지 검토하고 올바르게 적용하는 데 필요한 모든 작업을 추가, 제거 또는 수정합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-144">Once the migration is scaffolded (code generated for it), review the code for accuracy and add, remove or modify any operations required to apply it correctly.</span></span>

<span data-ttu-id="2daa1-145">예를 들어 마이그레이션에 다음과 같은 작업을 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-145">For example, a migration might contain the following operations:</span></span>

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

<span data-ttu-id="2daa1-146">이러한 작업은 데이터베이스 스키마를 호환되게 하지만 기존 고객 이름은 유지하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-146">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="2daa1-147">향상을 위해 다음과 같이 재작성합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-147">To make it better, rewrite it as follows.</span></span>

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

> [!TIP]
> <span data-ttu-id="2daa1-148">작업으로 인해 데이터가 손실될 수 있는 경우(예: 열 삭제) 마이그레이션 스캐폴딩 프로세스가 경고를 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-148">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="2daa1-149">해당 경고가 표시되면 특히 마이그레이션 코드가 정확한지 검토해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-149">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

<span data-ttu-id="2daa1-150">적합한 명령을 사용하여 데이터베이스에 마이그레이션을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-150">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

### <a name="empty-migrations"></a><span data-ttu-id="2daa1-151">빈 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="2daa1-151">Empty migrations</span></span>

<span data-ttu-id="2daa1-152">때때로 모델 변경 없이 마이그레이션을 추가하는 것이 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-152">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="2daa1-153">이 경우 새 마이그레이션을 추가하면 빈 클래스가 있는 코드 파일이 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-153">In this case, adding a new migration creates code files with empty classes.</span></span> <span data-ttu-id="2daa1-154">이 마이그레이션을 사용자 지정하여 EF Core 모델과 직접적인 관련이 없는 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-154">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span> <span data-ttu-id="2daa1-155">이러한 방식으로 관리하고자 할 수 있는 항목은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-155">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="2daa1-156">전체 텍스트 검색</span><span class="sxs-lookup"><span data-stu-id="2daa1-156">Full-Text Search</span></span>
* <span data-ttu-id="2daa1-157">함수</span><span class="sxs-lookup"><span data-stu-id="2daa1-157">Functions</span></span>
* <span data-ttu-id="2daa1-158">저장 프로시저</span><span class="sxs-lookup"><span data-stu-id="2daa1-158">Stored procedures</span></span>
* <span data-ttu-id="2daa1-159">트리거</span><span class="sxs-lookup"><span data-stu-id="2daa1-159">Triggers</span></span>
* <span data-ttu-id="2daa1-160">보기</span><span class="sxs-lookup"><span data-stu-id="2daa1-160">Views</span></span>

<a name="remove-a-migration"></a><span data-ttu-id="2daa1-161">마이그레이션 제거</span><span class="sxs-lookup"><span data-stu-id="2daa1-161">Remove a migration</span></span>
------------------
<span data-ttu-id="2daa1-162">때때로 마이그레이션을 추가하고 적용하기 전에 EF Core 모델에 추가적인 변경이 필요한 경우가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-162">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="2daa1-163">마지막 마이그레이션을 제거하려면다음 명령을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-163">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="2daa1-164">마이그레이션을 제거한 후 추가적인 모델 변경을 수행하고 다시 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-164">After removing the migration, you can make the additional model changes and add it again.</span></span>

<a name="revert-a-migration"></a><span data-ttu-id="2daa1-165">마이그레이션 되돌리기</span><span class="sxs-lookup"><span data-stu-id="2daa1-165">Revert a migration</span></span>
------------------
<span data-ttu-id="2daa1-166">이미 한 번 이상의 마이그레이션을 데이터베이스에 적용했으나 되돌려야 하는 경우 같은 명령을 사용하여 마이그레이션을 적용할 수 있으나, 롤백하려는 대상 마이그레이션의 이름을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-166">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="generate-sql-scripts"></a><span data-ttu-id="2daa1-167">SQL 스크립트 생성</span><span class="sxs-lookup"><span data-stu-id="2daa1-167">Generate SQL scripts</span></span>
--------------------
<span data-ttu-id="2daa1-168">마이그레이션을 디버깅하거나 프로덕션 데이터베이스에 배포할 경우 SQL 스크립트를 생성하는 것이 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-168">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="2daa1-169">그런 다음 스크립트가 정확한지 추가적으로 검토하고 프로덕션 데이터베이스에 맞게 조정합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-169">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="2daa1-170">또한 배포 기술과 함께 스크립트를 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-170">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="2daa1-171">기본 명령은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-171">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="2daa1-172">이 명령에는 몇 가지 옵션이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-172">There are several options to this command.</span></span>

<span data-ttu-id="2daa1-173">**from** 마이그레이션은 스크립트를 실행하기 전에 데이터베이스에 적용된 마지막 마이그레이션이 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-173">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="2daa1-174">적용된 마이그레이션이 없으면 `0`(기본값)을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-174">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="2daa1-175">**to** 마이그레이션을 스크립트를 실행한 후에 데이터베이스에 적용된 마지막 마이그레이션이 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-175">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="2daa1-176">기본값은 프로젝트의 마지막 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-176">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="2daa1-177">**idempotent** 스크립트는 선택적으로 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-177">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="2daa1-178">이 스크립트는 기존에 데이터베이스에 적용되지 않은 경우 마이그레이션만 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-178">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="2daa1-179">데이터베이스에 적용된 마지막 마이그레이션을 잘 모르거나 각기 다른 마이그레이션 상태일 수 있는 여러 데이터베이스에 배포할 때 이것이 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-179">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="apply-migrations-at-runtime"></a><span data-ttu-id="2daa1-180">런타임에 마이그레이션 적용</span><span class="sxs-lookup"><span data-stu-id="2daa1-180">Apply migrations at runtime</span></span>
---------------------------
<span data-ttu-id="2daa1-181">일부 앱은 시작이나 최초 실행 중에 런타임에서 마이그레이션을 적용하려 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-181">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="2daa1-182">이 작업은 `Migrate()` 메서드로 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-182">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="2daa1-183">이 메서드는 `IMigrator` 서비스를 기반으로 구성되므로 더 고급 시나리오에 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-183">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="2daa1-184">`myDbContext.GetInfrastructure().GetService<IMigrator>()`를 사용하여 액세스합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-184">Use `myDbContext.GetInfrastructure().GetService<IMigrator>()` to access it.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> * <span data-ttu-id="2daa1-185">이 방법은 모두에게 적합한 것이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-185">This approach isn't for everyone.</span></span> <span data-ttu-id="2daa1-186">로컬 데이터베이스가 있는 앱에는 훌륭하나 대부분의 애플리케이션에는 SQL 스크립트 생성 등과 같이 더 강력한 배포 전략이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-186">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>
> * <span data-ttu-id="2daa1-187">`Migrate()`에 앞서 `EnsureCreated()`를 호출하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-187">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="2daa1-188">`EnsureCreated()`는 마이그레이션을 무시하고 스키마를 만들어 `Migrate()`에 실패합니다.</span><span class="sxs-lookup"><span data-stu-id="2daa1-188">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

<a name="next-steps"></a><span data-ttu-id="2daa1-189">다음 단계</span><span class="sxs-lookup"><span data-stu-id="2daa1-189">Next steps</span></span>
----------

<span data-ttu-id="2daa1-190">자세한 내용은 <xref:core/miscellaneous/cli/index>을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="2daa1-190">For more information, see <xref:core/miscellaneous/cli/index>.</span></span>
