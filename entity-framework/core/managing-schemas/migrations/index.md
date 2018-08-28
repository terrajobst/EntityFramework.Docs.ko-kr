---
title: 마이그레이션 - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 4a5d6f3798c7af7597f95cebea1aeb9e5e58d277
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996524"
---
<a name="migrations"></a><span data-ttu-id="d4128-102">마이그레이션</span><span class="sxs-lookup"><span data-stu-id="d4128-102">Migrations</span></span>
==========
<span data-ttu-id="d4128-103">마이그레이션은 데이터베이스에 스키마 변경 내용을 증분 방식으로 적용하여 EF Core 모델과의 동기 상태를 유지하면서 데이터베이스에 기존 데이터를 보존하는 방법을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-103">Migrations provide a way to incrementally apply schema changes to the database to keep it in sync with your EF Core model while preserving existing data in the database.</span></span>

<a name="creating-the-database"></a><span data-ttu-id="d4128-104">데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="d4128-104">Creating the database</span></span>
---------------------
<span data-ttu-id="d4128-105">[초기 모델 정의][1] 후에는 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-105">After you've [defined your initial model][1], it's time to create the database.</span></span> <span data-ttu-id="d4128-106">이를 위해 초기 마이그레이션을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-106">To do this, add an initial migration.</span></span>
<span data-ttu-id="d4128-107">[EF Core 도구][2]를 설치하고 적합한 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-107">Install the [EF Core Tools][2] and run the appropriate command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="d4128-108">프로젝트의 **Migrations** 디렉터리 아래 3개 파일이 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-108">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="d4128-109">**00000000000000_InitialCreate.cs** -- 주 마이그레이션 파일.</span><span class="sxs-lookup"><span data-stu-id="d4128-109">**00000000000000_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="d4128-110">마이그레이션을 적용하고(`Up()`) 변환하는 데(`Down()`) 필요한 작업을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-110">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="d4128-111">**00000000000000_InitialCreate.Designer.cs** -- 마이그레이션 메타데이터 파일.</span><span class="sxs-lookup"><span data-stu-id="d4128-111">**00000000000000_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="d4128-112">EF에서 사용하는 정보가 들어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-112">Contains information used by EF.</span></span>
* <span data-ttu-id="d4128-113">**MyContextModelSnapshot.cs** - 현재 모델의 스냅숏.</span><span class="sxs-lookup"><span data-stu-id="d4128-113">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="d4128-114">다음 마이그레이션을 추가할 때 변경 항목을 판단하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-114">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="d4128-115">파일 이름의 타임스탬프는 시간순으로 정렬되기 때문에 변경의 진행 상황을 파악할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-115">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="d4128-116">마이그레이션 파일을 자유롭게 이동하고 네임스페이스를 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-116">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="d4128-117">새 마이그레이션은 지난 번 마이그레이션의 형제 항목으로 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-117">New migrations are created as siblings of the last migration.</span></span>

<span data-ttu-id="d4128-118">그런 다음 스키마를 만들기 위해 데이터베이스에 마이그레이션을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-118">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a><span data-ttu-id="d4128-119">다른 마이그레이션 추가</span><span class="sxs-lookup"><span data-stu-id="d4128-119">Adding another migration</span></span>
------------------------
<span data-ttu-id="d4128-120">EF Core 모델을 변경한 후에는 데이터베이스 스키마가 동기 상태가 아닙니다.  최신 상태로 하기 위해 다른 마이그레이션을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-120">After making changes to your EF Core model, the database schema will be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="d4128-121">마이그레이션 이름은 버전 제어 시스템의 커밋 메시지처럼 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-121">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="d4128-122">예를 들어 제품의 고객 평가를 변경한 경우 *AddProductReviews*와 같이 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-122">For example, if I made changes to save customer reviews of products, I might choose something like *AddProductReviews*.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="d4128-123">마이그레이션이 스캐폴드되면 정확한지 검토하고 정확한 적용을 위해 필요한 다른 작업을 추가해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-123">Once the migration is scaffolded, you should review it for accuracy and add any additional operations required to apply it correctly.</span></span> <span data-ttu-id="d4128-124">예를 들어 마이그레이션에 다음과 같은 작업을 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-124">For example, your migration might contain the following operations:</span></span>

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

<span data-ttu-id="d4128-125">이러한 작업은 데이터베이스 스키마를 호환되게 하지만 기존 고객 이름은 유지하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-125">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="d4128-126">향상을 위해 다음과 같이 재작성합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-126">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="d4128-127">새 마이그레이션을 추가하면 작업이 스캐폴드되어 데이터가 손실될 수 있다(예: 열 삭제)는 경고가 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-127">Adding a new migration warns when an operation is scaffolded that may result in data loss (like dropping a column).</span></span> <span data-ttu-id="d4128-128">이러한 마이그레이션은 특히 정확한지 검토해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-128">Be sure to especially review these migrations for accuracy.</span></span>

<span data-ttu-id="d4128-129">적합한 명령을 사용하여 데이터베이스에 마이그레이션을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-129">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a><span data-ttu-id="d4128-130">마이그레이션 제거</span><span class="sxs-lookup"><span data-stu-id="d4128-130">Removing a migration</span></span>
--------------------
<span data-ttu-id="d4128-131">때때로 마이그레이션을 추가하고 적용하기 전에 EF Core 모델에 추가적인 변경이 필요한 경우가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-131">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span>
<span data-ttu-id="d4128-132">마지막 마이그레이션을 제거하려면다음 명령을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-132">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="d4128-133">제거한 후 추가적인 모델 변경을 수행하고 다시 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-133">After removing it, you can make the additional model changes and add it again.</span></span>

<a name="reverting-a-migration"></a><span data-ttu-id="d4128-134">마이그레이션 되돌리기</span><span class="sxs-lookup"><span data-stu-id="d4128-134">Reverting a migration</span></span>
---------------------
<span data-ttu-id="d4128-135">이미 한 번 이상의 마이그레이션을 데이터베이스에 적용했으나 되돌려야 하는 경우 같은 명령을 사용하여 마이그레이션을 적용할 수 있으나, 롤백하려는 대상 마이그레이션의 이름을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-135">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a><span data-ttu-id="d4128-136">빈 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="d4128-136">Empty migrations</span></span>
----------------
<span data-ttu-id="d4128-137">때때로 모델 변경 없이 마이그레이션을 추가하는 것이 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-137">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="d4128-138">이 경우 새 마이그레이션을 추가하면 빈 항목이 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-138">In this case, adding a new migration creates an empty one.</span></span> <span data-ttu-id="d4128-139">이 마이그레이션을 사용자 지정하여 EF Core 모델과 직접적인 관련이 없는 작업을 수행할 수 있습니다. </span><span class="sxs-lookup"><span data-stu-id="d4128-139">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span>
<span data-ttu-id="d4128-140">이러한 방식으로 관리하고자 할 수 있는 항목은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-140">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="d4128-141">전체 텍스트 검색</span><span class="sxs-lookup"><span data-stu-id="d4128-141">Full-Text Search</span></span>
* <span data-ttu-id="d4128-142">함수</span><span class="sxs-lookup"><span data-stu-id="d4128-142">Functions</span></span>
* <span data-ttu-id="d4128-143">저장 프로시저</span><span class="sxs-lookup"><span data-stu-id="d4128-143">Stored procedures</span></span>
* <span data-ttu-id="d4128-144">트리거</span><span class="sxs-lookup"><span data-stu-id="d4128-144">Triggers</span></span>
* <span data-ttu-id="d4128-145">보기</span><span class="sxs-lookup"><span data-stu-id="d4128-145">Views</span></span>
* <span data-ttu-id="d4128-146">기타</span><span class="sxs-lookup"><span data-stu-id="d4128-146">etc.</span></span>

<a name="generating-a-sql-script"></a><span data-ttu-id="d4128-147">SQL 스크립트 생성</span><span class="sxs-lookup"><span data-stu-id="d4128-147">Generating a SQL script</span></span>
-----------------------
<span data-ttu-id="d4128-148">마이그레이션을 디버깅하거나 프로덕션 데이터베이스에 배포할 경우 SQL 스크립트를 생성하는 것이 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-148">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="d4128-149">그런 다음 스크립트가 정확한지 추가적으로 검토하고 프로덕션 데이터베이스에 맞게 조정합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-149">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="d4128-150">또한 배포 기술과 함께 스크립트를 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-150">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="d4128-151">기본 명령은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-151">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="d4128-152">이 명령에는 몇 가지 옵션이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-152">There are several options to this command.</span></span>

<span data-ttu-id="d4128-153">**from** 마이그레이션은 스크립트를 실행하기 전에 데이터베이스에 적용된 마지막 마이그레이션이 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-153">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="d4128-154">적용된 마이그레이션이 없으면 `0`(기본값)을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-154">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="d4128-155">**to** 마이그레이션을 스크립트를 실행한 후에 데이터베이스에 적용된 마지막 마이그레이션이 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-155">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="d4128-156">기본값은 프로젝트의 마지막 마이그레이션입니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-156">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="d4128-157">**idempotent** 스크립트는 선택적으로 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-157">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="d4128-158">이 스크립트는 기존에 데이터베이스에 적용되지 않은 경우 마이그레이션만 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-158">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="d4128-159">데이터베이스에 적용된 마지막 마이그레이션을 잘 모르거나 각기 다른 마이그레이션 상태일 수 있는 여러 데이터베이스에 배포할 때 이것이 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-159">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="applying-migrations-at-runtime"></a><span data-ttu-id="d4128-160">런타임 시 마이그레이션 적용</span><span class="sxs-lookup"><span data-stu-id="d4128-160">Applying migrations at runtime</span></span>
------------------------------
<span data-ttu-id="d4128-161">일부 앱은 시작이나 최초 실행 중에 런타임에서 마이그레이션을 적용하려 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-161">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="d4128-162">이 작업은 `Migrate()` 메서드로 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-162">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="d4128-163">주의: 이 방법은 모두에게 적합한 것이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-163">Caution, this approach isn't for everyone.</span></span> <span data-ttu-id="d4128-164">로컬 데이터베이스가 있는 앱에는 훌륭하나 대부분의 응용 프로그램에는 SQL 스크립트 생성 등과 같이 더 강력한 배포 전략이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-164">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> <span data-ttu-id="d4128-165">`Migrate()`에 앞서 `EnsureCreated()`를 호출하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-165">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="d4128-166">`EnsureCreated()`는 마이그레이션을 무시하고 스키마를 만들어 `Migrate()`에 실패합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-166">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

> [!NOTE]
> <span data-ttu-id="d4128-167">이 메서드는 `IMigrator` 서비스를 기반으로 구성되므로 더 고급 시나리오에 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-167">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="d4128-168">`DbContext.GetService<IMigrator>()`를 사용하여 액세스합니다.</span><span class="sxs-lookup"><span data-stu-id="d4128-168">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
