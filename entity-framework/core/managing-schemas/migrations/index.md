---
title: 마이그레이션 - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: dd164125c053497af94773011127853ad10d27a6
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754511"
---
<a name="migrations"></a>마이그레이션
==========
마이그레이션은 데이터베이스에 스키마 변경 내용을 증분 방식으로 적용하여 EF Core 모델과의 동기 상태를 유지하면서 데이터베이스에 기존 데이터를 보존하는 방법을 제공합니다.

<a name="creating-the-database"></a>데이터베이스 만들기
---------------------
[초기 모델 정의][1] 후에는 데이터베이스를 만듭니다. 이를 위해 초기 마이그레이션을 추가합니다.
[EF Core 도구][2]를 설치하고 적합한 명령을 실행합니다.

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

프로젝트의 **Migrations** 디렉터리 아래 3개 파일이 추가됩니다.

* **00000000000000_InitialCreate.cs** -- 주 마이그레이션 파일. 마이그레이션을 적용하고(`Up()`) 변환하는 데(`Down()`) 필요한 작업을 포함합니다.
* **00000000000000_InitialCreate.Designer.cs** -- 마이그레이션 메타데이터 파일. EF에서 사용하는 정보가 들어 있습니다.
* **MyContextModelSnapshot.cs** - 현재 모델의 스냅숏. 다음 마이그레이션을 추가할 때 변경 항목을 판단하는 데 사용됩니다.

파일 이름의 타임스탬프는 시간순으로 정렬되기 때문에 변경의 진행 상황을 파악할 수 있습니다.

> [!TIP]
> 마이그레이션 파일을 자유롭게 이동하고 네임스페이스를 변경할 수 있습니다. 새 마이그레이션은 지난 번 마이그레이션의 형제 항목으로 만들어집니다.

그런 다음 스키마를 만들기 위해 데이터베이스에 마이그레이션을 적용합니다.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a>다른 마이그레이션 추가
------------------------
EF Core 모델을 변경한 후에는 데이터베이스 스키마가 동기 상태가 아닙니다.  최신 상태로 하기 위해 다른 마이그레이션을 추가합니다. 마이그레이션 이름은 버전 제어 시스템의 커밋 메시지처럼 사용할 수 있습니다. 예를 들어 제품의 고객 평가를 변경한 경우 *AddProductReviews*와 같이 선택할 수 있습니다.

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

마이그레이션이 스캐폴드되면 정확한지 검토하고 정확한 적용을 위해 필요한 다른 작업을 추가해야 합니다. 예를 들어 마이그레이션에 다음과 같은 작업을 포함할 수 있습니다.

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

이러한 작업은 데이터베이스 스키마를 호환되게 하지만 기존 고객 이름은 유지하지 않습니다. 향상을 위해 다음과 같이 재작성합니다.

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
> 새 마이그레이션을 추가하면 작업이 스캐폴드되어 데이터가 손실될 수 있다(예: 열 삭제)는 경고가 표시됩니다. 이러한 마이그레이션은 특히 정확한지 검토해야 합니다.

적합한 명령을 사용하여 데이터베이스에 마이그레이션을 적용합니다.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a>마이그레이션 제거
--------------------
때때로 마이그레이션을 추가하고 적용하기 전에 EF Core 모델에 추가적인 변경이 필요한 경우가 있습니다.
마지막 마이그레이션을 제거하려면다음 명령을 사용합니다.

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

제거한 후 추가적인 모델 변경을 수행하고 다시 추가합니다.

<a name="reverting-a-migration"></a>마이그레이션 되돌리기
---------------------
이미 한 번 이상의 마이그레이션을 데이터베이스에 적용했으나 되돌려야 하는 경우 같은 명령을 사용하여 마이그레이션을 적용할 수 있으나, 롤백하려는 대상 마이그레이션의 이름을 지정합니다.

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a>빈 마이그레이션
----------------
때때로 모델 변경 없이 마이그레이션을 추가하는 것이 유용합니다. 이 경우 새 마이그레이션을 추가하면 빈 항목이 만들어집니다. 이 마이그레이션을 사용자 지정하여 EF Core 모델과 직접적인 관련이 없는 작업을 수행할 수 있습니다. 
이러한 방식으로 관리하고자 할 수 있는 항목은 다음과 같습니다.

* 전체 텍스트 검색
* 함수
* 저장 프로시저
* 트리거
* 보기
* 기타

<a name="generating-a-sql-script"></a>SQL 스크립트 생성
-----------------------
마이그레이션을 디버깅하거나 프로덕션 데이터베이스에 배포할 경우 SQL 스크립트를 생성하는 것이 유용합니다. 그런 다음 스크립트가 정확한지 추가적으로 검토하고 프로덕션 데이터베이스에 맞게 조정합니다. 또한 배포 기술과 함께 스크립트를 사용할 수도 있습니다. 기본 명령은 다음과 같습니다.

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

이 명령에는 몇 가지 옵션이 있습니다.

**from** 마이그레이션은 스크립트를 실행하기 전에 데이터베이스에 적용된 마지막 마이그레이션이 되어야 합니다. 적용된 마이그레이션이 없으면 `0`(기본값)을 지정합니다.

**to** 마이그레이션을 스크립트를 실행한 후에 데이터베이스에 적용된 마지막 마이그레이션이 되어야 합니다. 기본값은 프로젝트의 마지막 마이그레이션입니다.

**idempotent** 스크립트는 선택적으로 생성할 수 있습니다. 이 스크립트는 기존에 데이터베이스에 적용되지 않은 경우 마이그레이션만 적용합니다. 데이터베이스에 적용된 마지막 마이그레이션을 잘 모르거나 각기 다른 마이그레이션 상태일 수 있는 여러 데이터베이스에 배포할 때 이것이 유용합니다.

<a name="applying-migrations-at-runtime"></a>런타임 시 마이그레이션 적용
------------------------------
일부 앱은 시작이나 최초 실행 중에 런타임에서 마이그레이션을 적용하려 할 수 있습니다. 이 작업은 `Migrate()` 메서드로 수행합니다.

주의: 이 방법은 모두에게 적합한 것이 아닙니다. 로컬 데이터베이스가 있는 앱에는 훌륭하나 대부분의 응용 프로그램에는 SQL 스크립트 생성 등과 같이 더 강력한 배포 전략이 필요합니다.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> `Migrate()`에 앞서 `EnsureCreated()`를 호출하지 않습니다. `EnsureCreated()`는 마이그레이션을 무시하고 스키마를 만들어 `Migrate()`에 실패합니다.

> [!NOTE]
> 이 메서드는 `IMigrator` 서비스를 기반으로 구성되므로 더 고급 시나리오에 사용할 수 있습니다. `DbContext.GetService<IMigrator>()`를 사용하여 액세스합니다.


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
