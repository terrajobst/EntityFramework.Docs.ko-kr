---
title: 마이그레이션 - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: dc0c1ae1a03c98c6f230557dc0bdd4d29ec191dd
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502203"
---
# <a name="migrations"></a>마이그레이션

데이터 모델은 개발 중에 변경되고 데이터베이스와 동기화되지 않습니다. 데이터베이스를 삭제하고 EF가 모델과 일치하는 새 데이터베이스를 만들게 할 수 있지만, 이 절차로 인해 데이터가 손실됩니다. EF Core의 마이그레이션 기능은 데이터베이스 스키마를 증분 방식으로 업데이트하여 애플리케이션 데이터 모델과의 동기 상태를 유지하면서 데이터베이스에 기존 데이터를 보존하는 방법을 제공합니다.

마이그레이션에는 다음 작업에 도움이 되는 명령줄 도구 및 API가 포함됩니다.

* [마이그레이션 만들기](#create-a-migration). 모델 변경 내용 집합과 동기화하도록 데이터베이스를 업데이트할 수 있는 코드를 생성합니다.
* [데이터베이스 업데이트](#update-the-database). 보류 중인 마이그레이션을 적용하여 데이터베이스 스키마를 업데이트합니다.
* [마이그레이션 코드 사용자 지정](#customize-migration-code). 경우에 따라 생성된 코드를 수정하거나 보완해야 합니다.
* [마이그레이션 제거](#remove-a-migration). 생성된 코드를 삭제합니다.
* [마이그레이션 되돌리기](#revert-a-migration). 데이터베이스 변경 내용을 실행 취소합니다.
* [SQL 스크립트 생성](#generate-sql-scripts). 프로덕션 데이터베이스를 업데이트하거나 마이그레이션 코드 문제점을 해결하기 위한 스크립트가 필요할 수 있습니다.
* [런타임에 마이그레이션 적용](#apply-migrations-at-runtime). 디자인 타임 업데이트 및 실행 중인 스크립트가 최상의 옵션이 아닌 경우 `Migrate()` 메서드를 호출합니다.

> [!TIP]
> `DbContext`가 시작 프로젝트와 다른 어셈블리에 있는 경우 [패키지 관리자 콘솔 도구](xref:core/miscellaneous/cli/powershell#target-and-startup-project) 또는 [.NET Core CLI 도구](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project)에서 대상 및 시작 프로젝트를 명시적으로 지정할 수 있습니다.

## <a name="install-the-tools"></a>도구 설치

[명령줄 도구](xref:core/miscellaneous/cli/index)를 설치합니다.

* Visual Studio의 경우 [패키지 관리자 콘솔 도구](xref:core/miscellaneous/cli/powershell)를 사용하는 것이 좋습니다.
* 기타 개발 환경의 경우 [.NET Core CLI 도구](xref:core/miscellaneous/cli/dotnet)를 선택합니다.

## <a name="create-a-migration"></a>마이그레이션 만들기

[초기 모델 정의](xref:core/modeling/index) 후에는 데이터베이스를 만듭니다. 초기 마이그레이션을 추가하려면 다음 명령을 실행합니다.

### <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration InitialCreate
```

***

프로젝트의 **Migrations** 디렉터리 아래 3개 파일이 추가됩니다.

* **XXXXXXXXXXXXXX_InitialCreate.cs** -- 주 마이그레이션 파일. 마이그레이션을 적용하고(`Up()`) 변환하는 데(`Down()`) 필요한 작업을 포함합니다.
* **XXXXXXXXXXXXXX_InitialCreate.Designer.cs** -- 마이그레이션 메타데이터 파일. EF에서 사용하는 정보가 들어 있습니다.
* **MyContextModelSnapshot.cs** - 현재 모델의 스냅숏. 다음 마이그레이션을 추가할 때 변경 항목을 판단하는 데 사용됩니다.

파일 이름의 타임스탬프는 시간순으로 정렬되기 때문에 변경의 진행 상황을 파악할 수 있습니다.

> [!TIP]
> 마이그레이션 파일을 자유롭게 이동하고 네임스페이스를 변경할 수 있습니다. 새 마이그레이션은 지난 번 마이그레이션의 형제 항목으로 만들어집니다.

## <a name="update-the-database"></a>데이터베이스 업데이트

그런 다음 스키마를 만들기 위해 데이터베이스에 마이그레이션을 적용합니다.

### <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Update-Database
```

***

## <a name="customize-migration-code"></a>마이그레이션 코드 사용자 지정

EF Core 모델을 변경한 후에는 데이터베이스 스키마가 동기화되지 않을 수 있습니다. 최신 상태로 하기 위해 다른 마이그레이션을 추가합니다. 마이그레이션 이름은 버전 제어 시스템의 커밋 메시지처럼 사용할 수 있습니다. 예를 들어 변경이 검토를 위한 새 엔터티 클래스인 경우 *AddProductReviews*와 같은 이름을 선택할 수 있습니다.

### <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add AddProductReviews
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration AddProductReviews
```

***

마이그레이션이 스캐폴드되면(마이그레이션을 위한 코드가 생성됨) 코드가 정확한지 검토하고 올바르게 적용하는 데 필요한 모든 작업을 추가, 제거 또는 수정합니다.

예를 들어 마이그레이션에 다음과 같은 작업을 포함할 수 있습니다.

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
> 작업으로 인해 데이터가 손실될 수 있는 경우(예: 열 삭제) 마이그레이션 스캐폴딩 프로세스가 경고를 표시합니다. 해당 경고가 표시되면 특히 마이그레이션 코드가 정확한지 검토해야 합니다.

적합한 명령을 사용하여 데이터베이스에 마이그레이션을 적용합니다.

### <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Update-Database
```

***

### <a name="empty-migrations"></a>빈 마이그레이션

때때로 모델 변경 없이 마이그레이션을 추가하는 것이 유용합니다. 이 경우 새 마이그레이션을 추가하면 빈 클래스가 있는 코드 파일이 만들어집니다. 이 마이그레이션을 사용자 지정하여 EF Core 모델과 직접적인 관련이 없는 작업을 수행할 수 있습니다. 이러한 방식으로 관리하고자 할 수 있는 항목은 다음과 같습니다.

* 전체 텍스트 검색
* 함수
* 저장 프로시저
* 트리거
* 뷰

## <a name="remove-a-migration"></a>마이그레이션 제거

때때로 마이그레이션을 추가하고 적용하기 전에 EF Core 모델에 추가적인 변경이 필요한 경우가 있습니다. 마지막 마이그레이션을 제거하려면다음 명령을 사용합니다.

### <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations remove
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Remove-Migration
```

***

마이그레이션을 제거한 후 추가적인 모델 변경을 수행하고 다시 추가합니다.

## <a name="revert-a-migration"></a>마이그레이션 되돌리기

이미 한 번 이상의 마이그레이션을 데이터베이스에 적용했으나 되돌려야 하는 경우 같은 명령을 사용하여 마이그레이션을 적용할 수 있으나, 롤백하려는 대상 마이그레이션의 이름을 지정합니다.

### <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update LastGoodMigration
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Update-Database LastGoodMigration
```

***

## <a name="generate-sql-scripts"></a>SQL 스크립트 생성

마이그레이션을 디버깅하거나 프로덕션 데이터베이스에 배포할 경우 SQL 스크립트를 생성하는 것이 유용합니다. 그런 다음 스크립트가 정확한지 추가적으로 검토하고 프로덕션 데이터베이스에 맞게 조정합니다. 또한 배포 기술과 함께 스크립트를 사용할 수도 있습니다. 기본 명령은 다음과 같습니다.

### <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations script
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Script-Migration
```

***

이 명령에는 몇 가지 옵션이 있습니다.

**from** 마이그레이션은 스크립트를 실행하기 전에 데이터베이스에 적용된 마지막 마이그레이션이 되어야 합니다. 적용된 마이그레이션이 없으면 `0`(기본값)을 지정합니다.

**to** 마이그레이션을 스크립트를 실행한 후에 데이터베이스에 적용된 마지막 마이그레이션이 되어야 합니다. 기본값은 프로젝트의 마지막 마이그레이션입니다.

**idempotent** 스크립트는 선택적으로 생성할 수 있습니다. 이 스크립트는 기존에 데이터베이스에 적용되지 않은 경우 마이그레이션만 적용합니다. 데이터베이스에 적용된 마지막 마이그레이션을 잘 모르거나 각기 다른 마이그레이션 상태일 수 있는 여러 데이터베이스에 배포할 때 이것이 유용합니다.

## <a name="apply-migrations-at-runtime"></a>런타임에 마이그레이션 적용

일부 앱은 시작이나 최초 실행 중에 런타임에서 마이그레이션을 적용하려 할 수 있습니다. 이 작업은 `Migrate()` 메서드로 수행합니다.

이 메서드는 `IMigrator` 서비스를 기반으로 구성되므로 더 고급 시나리오에 사용할 수 있습니다. `myDbContext.GetInfrastructure().GetService<IMigrator>()`를 사용하여 액세스합니다.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
>
> * 이 방법은 모두에게 적합한 것이 아닙니다. 로컬 데이터베이스가 있는 앱에는 훌륭하나 대부분의 애플리케이션에는 SQL 스크립트 생성 등과 같이 더 강력한 배포 전략이 필요합니다.
> * `Migrate()`에 앞서 `EnsureCreated()`를 호출하지 않습니다. `EnsureCreated()`는 마이그레이션을 무시하고 스키마를 만들어 `Migrate()`에 실패합니다.

## <a name="next-steps"></a>다음 단계

자세한 내용은 <xref:core/miscellaneous/cli/index>를 참조하세요.
