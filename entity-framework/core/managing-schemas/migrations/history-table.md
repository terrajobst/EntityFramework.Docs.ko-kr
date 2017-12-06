---
title: "사용자 지정 마이그레이션 기록 테이블-EF 코어"
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cb9892241f3d7f1fae6293bd60a8a5c3e7120969
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
<a name="custom-migrations-history-table"></a><span data-ttu-id="281a3-102">사용자 지정 마이그레이션 기록 테이블</span><span class="sxs-lookup"><span data-stu-id="281a3-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="281a3-103">기본적으로 EF 코어는 추적 하는 마이그레이션에 적용 된 데이터베이스 라는 테이블에 기록 하 여 `__EFMigrationsHistory`합니다.</span><span class="sxs-lookup"><span data-stu-id="281a3-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="281a3-104">여러 가지 이유로이 테이블을 필요에 따라 사용자 지정 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="281a3-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="281a3-105">마이그레이션 기록 테이블을 사용자 지정한 경우 *후* 마이그레이션 적용을 담당 하는 데이터베이스의 기존 테이블을 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="281a3-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="281a3-106">스키마 및 테이블 이름</span><span class="sxs-lookup"><span data-stu-id="281a3-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="281a3-107">스키마 및 사용 하 여 테이블 이름을 변경할 수 있습니다는 `MigrationsHistoryTable()` 메서드에서 `OnConfiguring()` (또는 `ConfigureServices()` ASP.NET Core에).</span><span class="sxs-lookup"><span data-stu-id="281a3-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="281a3-108">SQL Server EF 코어 공급자를 사용 하는 예제는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="281a3-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="281a3-109">기타 변경 내용</span><span class="sxs-lookup"><span data-stu-id="281a3-109">Other changes</span></span>
-------------
<span data-ttu-id="281a3-110">테이블의 추가 측면을 구성 하려면 재정의 및 공급자 관련 바꾸기 `IHistoryRepository` 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="281a3-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="281a3-111">MigrationId 열 이름을 변경의 예로 *Id* SQL Server에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="281a3-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="281a3-112">`SqlServerHistoryRepository`내부 네임 스페이스 내 한 수는 이후 릴리스에서 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="281a3-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
