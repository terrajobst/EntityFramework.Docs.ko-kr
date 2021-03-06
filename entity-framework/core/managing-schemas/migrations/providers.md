---
title: 여러 공급자를 사용 하 여 마이그레이션-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/08/2017
uid: core/managing-schemas/migrations/providers
ms.openlocfilehash: efe95893f7dbfc8e5c4775e86d58abb32eee3c83
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414256"
---
# <a name="migrations-with-multiple-providers"></a>여러 공급자를 사용 하 여 마이그레이션

[EF Core 도구][1] 는 활성 공급자의 마이그레이션만 스 캐 폴드 합니다. 그러나 경우에 따라 DbContext에서 둘 이상의 공급자 (예: Microsoft SQL Server 및 SQLite)를 사용 하는 것이 좋습니다. 마이그레이션을 사용 하 여이를 처리 하는 방법에는 두 가지가 있습니다. 각 공급자에 대해 하나씩 두 개의 마이그레이션 집합을 유지 관리할 수 있으며 둘 다에서 작동할 수 있는 단일 집합으로 병합할 수 있습니다.

## <a name="two-migration-sets"></a>마이그레이션 집합 2 개

첫 번째 방법에서는 각 모델 변경에 대해 두 개의 마이그레이션을 생성 합니다.

이 작업을 수행 하는 한 가지 방법은 각 마이그레이션 집합을 [별도의 어셈블리에][2] 배치 하 고 두 마이그레이션 추가 간에 활성 공급자 (및 마이그레이션 어셈블리)를 수동으로 전환 하는 것입니다.

도구를 쉽게 사용 하는 또 다른 방법은 DbContext에서 파생 되는 새 형식을 만들고 활성 공급자를 재정의 하는 것입니다. 이 형식은 마이그레이션을 추가 하거나 적용할 때 디자인 타임에 사용 됩니다.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> 각 마이그레이션 집합은 자체 DbContext 유형을 사용 하므로이 방법은 별도의 마이그레이션 어셈블리를 사용할 필요가 없습니다.

새 마이그레이션을 추가할 때 컨텍스트 유형을 지정 합니다.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```

***

> [!TIP]
> 마지막 마이그레이션에 대 한 형제로 만들어지기 때문에 후속 마이그레이션의 출력 디렉터리를 지정할 필요가 없습니다.

## <a name="one-migration-set"></a>마이그레이션 집합 하나

두 개의 마이그레이션 집합이 없는 경우 두 공급자에 모두 적용할 수 있는 단일 집합으로 수동으로 결합할 수 있습니다.

공급자는 인식 되지 않는 주석을 무시 하므로 주석이 공존할 수 있습니다. 예를 들어 Microsoft SQL Server와 SQLite 모두에서 작동 하는 기본 키 열은 다음과 같습니다.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

한 공급자에 대해서만 작업을 적용할 수 있거나 공급자 간에 다르게 적용 될 수 있는 경우에는 `ActiveProvider` 속성을 사용 하 여 활성화 된 공급자를 알려 주십시오.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```

  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
