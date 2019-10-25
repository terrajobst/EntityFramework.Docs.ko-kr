---
title: 사용자 지정 마이그레이션 기록 테이블-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0db393ff3101564f8d8081d0a57b264c2c459df7
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812078"
---
# <a name="custom-migrations-history-table"></a><span data-ttu-id="118e7-102">사용자 지정 마이그레이션 기록 테이블</span><span class="sxs-lookup"><span data-stu-id="118e7-102">Custom Migrations History Table</span></span>

<span data-ttu-id="118e7-103">기본적으로 EF Core는 데이터베이스에 적용 된 마이그레이션을 `__EFMigrationsHistory`라는 테이블에 기록 하 여 추적 합니다.</span><span class="sxs-lookup"><span data-stu-id="118e7-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="118e7-104">다양 한 이유로이 테이블을 사용자의 요구에 맞게 사용자 지정 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="118e7-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="118e7-105">마이그레이션을 적용 *한 후* 마이그레이션 기록 테이블을 사용자 지정 하는 경우 데이터베이스의 기존 테이블을 업데이트할 책임이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="118e7-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

## <a name="schema-and-table-name"></a><span data-ttu-id="118e7-106">스키마 및 테이블 이름</span><span class="sxs-lookup"><span data-stu-id="118e7-106">Schema and table name</span></span>

<span data-ttu-id="118e7-107">`OnConfiguring()` (또는 ASP.NET Core에서 `ConfigureServices()`)의 `MigrationsHistoryTable()` 메서드를 사용 하 여 스키마 및 테이블 이름을 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="118e7-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="118e7-108">SQL Server EF Core 공급자를 사용 하는 예제는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="118e7-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

## <a name="other-changes"></a><span data-ttu-id="118e7-109">기타 변경 내용</span><span class="sxs-lookup"><span data-stu-id="118e7-109">Other changes</span></span>

<span data-ttu-id="118e7-110">테이블의 추가 측면을 구성 하려면 공급자별 `IHistoryRepository` 서비스를 재정의 하 고 대체 합니다.</span><span class="sxs-lookup"><span data-stu-id="118e7-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="118e7-111">SQL Server에서 MigrationId 열 이름을 *Id* 로 변경 하는 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="118e7-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="118e7-112">`SqlServerHistoryRepository`은 내부 네임 스페이스 내에 있으며 이후 릴리스에서 변경 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="118e7-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
