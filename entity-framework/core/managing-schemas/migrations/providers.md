---
title: "여러 공급자-EF 코어와 마이그레이션"
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d950e74ed4cef7d4274aabcf3eda7b0b735574c6
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2018
---
<a name="migrations-with-multiple-providers"></a>여러 공급자와 마이그레이션
==================================
[EF 핵심 도구] [ 1] 만 활성 중인 공급자에 대 한 마이그레이션을 스 캐 폴드 합니다. 그러나 경우에 따라 수 있습니다 하려는 프로그램 DbContext에 둘 이상의 공급자 (예: Microsoft SQL Server 및 SQLite)를 사용 합니다. 마이그레이션과 함께이 처리 하는 방법은 두 가지가 있습니다. 두 집합을 유지 관리할 수 있습니다는 마이그레이션-둘 다에서 작업할 수 있습니다 각 공급자--또는 단일으로 설정 하는 병합에 대 한 합니다.

<a name="two-migration-sets"></a>두 마이그레이션 집합
------------------
첫 번째 접근 방식에서 변경 될 때마다 모델에 대 한 두 마이그레이션 생성할 수 있습니다.

작업을 수행 하는 한 가지 방법은이 통해 각 마이그레이션 집합 배치 [별도 어셈블리에] [ 2] 수동으로 추가 두 마이그레이션 사이의 활성 공급자 (및 마이그레이션 어셈블리)를 전환 합니다.

더 쉽게 도구와 함께 사용할 또 다른 방법은 프로그램 DbContext에서 파생 되 고 활성 공급자를 재정의 하는 새 형식을 만드는 것입니다. 이 형식은 디자인에 사용 됩니다 때 추가 하거나 마이그레이션을 적용 합니다.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> 각 마이그레이션 세트 DbContext 형식 자체를 사용 하므로이 방법은 별도 마이그레이션을 어셈블리를 사용 하 여 필요 하지 않습니다.

새 마이그레이션에 추가할 때는 컨텍스트 형식을 지정 합니다.

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> 마지막 형제 항목으로 작성 되었으므로 후속 마이그레이션에 대 한 출력 디렉터리를 지정할 필요가 없습니다.

<a name="one-migration-set"></a>하나의 마이그레이션 설정
-----------------
두 집합을 마이그레이션의 마음에 들지 않으면 두 공급자 모두에 적용할 수 있는 단일 집합 수동으로 결합할 수 있습니다.

공급자가 인식 하지 못하는 모든 주석을 무시 하므로 주석을 사용할 수 있습니다. 예를 들어 Microsoft SQL Server와 SQLite와 작동 하는 기본 키 열이 같습니다.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

사용 하 여 작업 공급자를 하나에 적용할 수 있습니다 (또는 공급자 간의 다르게 하기가) 하는 경우는 `ActiveProvider` 활성 공급자를 구별 하는 속성입니다.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
