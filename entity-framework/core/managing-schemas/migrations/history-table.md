---
title: 사용자 지정 마이그레이션 기록 테이블-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
ms.openlocfilehash: 1a253972a8f4e410421ec8a77c079e588d368819
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488818"
---
<a name="custom-migrations-history-table"></a><span data-ttu-id="e4dbe-102">사용자 지정 마이그레이션 기록 테이블</span><span class="sxs-lookup"><span data-stu-id="e4dbe-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="e4dbe-103">기본적으로 EF Core는 추적 하는 마이그레이션에 적용 된 데이터베이스 라는 테이블에 기록 하 여 `__EFMigrationsHistory`입니다.</span><span class="sxs-lookup"><span data-stu-id="e4dbe-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="e4dbe-104">다양 한 이유로이 테이블에 필요에 맞게 사용자 지정 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="e4dbe-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e4dbe-105">마이그레이션 기록 테이블을 사용자 지정 하면 *후* 마이그레이션 적용을 담당 하는 데이터베이스의 기존 테이블을 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="e4dbe-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="e4dbe-106">스키마 및 테이블 이름</span><span class="sxs-lookup"><span data-stu-id="e4dbe-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="e4dbe-107">스키마 및 테이블 이름을 사용 하 여 변경할 수 있습니다 합니다 `MigrationsHistoryTable()` 의 메서드 `OnConfiguring()` (또는 `ConfigureServices()` ASP.NET Core에서).</span><span class="sxs-lookup"><span data-stu-id="e4dbe-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="e4dbe-108">SQL Server EF Core 공급자를 사용 하는 예제는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="e4dbe-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="e4dbe-109">기타 변경 내용</span><span class="sxs-lookup"><span data-stu-id="e4dbe-109">Other changes</span></span>
-------------
<span data-ttu-id="e4dbe-110">테이블의 추가 측면을 구성 하려면 재정의 하 고 공급자 관련 대체 `IHistoryRepository` 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="e4dbe-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="e4dbe-111">MigrationId 열 이름을 변경 하는 예로 *Id* SQL server입니다.</span><span class="sxs-lookup"><span data-stu-id="e4dbe-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="e4dbe-112">`SqlServerHistoryRepository` 내부 네임 스페이스 내에서 이며 변경 될 수 있습니다 나중에 릴리스 합니다.</span><span class="sxs-lookup"><span data-stu-id="e4dbe-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
