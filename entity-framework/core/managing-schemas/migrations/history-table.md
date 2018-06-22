---
title: 사용자 지정 마이그레이션 기록 테이블-EF 코어
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cb9892241f3d7f1fae6293bd60a8a5c3e7120969
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
ms.locfileid: "26053813"
---
<a name="custom-migrations-history-table"></a>사용자 지정 마이그레이션 기록 테이블
===============================
기본적으로 EF 코어는 추적 하는 마이그레이션에 적용 된 데이터베이스 라는 테이블에 기록 하 여 `__EFMigrationsHistory`합니다. 여러 가지 이유로이 테이블을 필요에 따라 사용자 지정 하는 것이 좋습니다.

> [!IMPORTANT]
> 마이그레이션 기록 테이블을 사용자 지정한 경우 *후* 마이그레이션 적용을 담당 하는 데이터베이스의 기존 테이블을 업데이트 합니다.

<a name="schema-and-table-name"></a>스키마 및 테이블 이름
----------------------
스키마 및 사용 하 여 테이블 이름을 변경할 수 있습니다는 `MigrationsHistoryTable()` 메서드에서 `OnConfiguring()` (또는 `ConfigureServices()` ASP.NET Core에). SQL Server EF 코어 공급자를 사용 하는 예제는 다음과 같습니다.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>기타 변경 내용
-------------
테이블의 추가 측면을 구성 하려면 재정의 및 공급자 관련 바꾸기 `IHistoryRepository` 서비스입니다. MigrationId 열 이름을 변경의 예로 *Id* SQL Server에 있습니다.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository`내부 네임 스페이스 내 한 수는 이후 릴리스에서 변경 합니다.

``` csharp
class MyHistoryRepository : SqlServerHistoryRepository
{
    public MyHistoryRepository(HistoryRepositoryDependencies dependencies)
        : base(dependencies)
    {
    }

    protected override void ConfigureTable(EntityTypeBuilder<HistoryRow> history)
    {
        base.ConfigureTable(history);

        history.Property(h => h.MigrationId).HasColumnName("Id");
    }
}
```
