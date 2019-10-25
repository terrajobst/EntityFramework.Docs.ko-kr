---
title: 여러 공급자를 사용 하 여 마이그레이션-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/08/2017
uid: core/managing-schemas/migrations/providers
ms.openlocfilehash: c9b1a2563ef548e592374f90a6242b0bd851bc98
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811950"
---
# <a name="migrations-with-multiple-providers"></a><span data-ttu-id="a1d1c-102">여러 공급자를 사용 하 여 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="a1d1c-102">Migrations with Multiple Providers</span></span>

<span data-ttu-id="a1d1c-103">[EF Core 도구][1] 는 활성 공급자의 마이그레이션만 스 캐 폴드 합니다.</span><span class="sxs-lookup"><span data-stu-id="a1d1c-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="a1d1c-104">그러나 경우에 따라 DbContext에서 둘 이상의 공급자 (예: Microsoft SQL Server 및 SQLite)를 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="a1d1c-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="a1d1c-105">마이그레이션을 사용 하 여이를 처리 하는 방법에는 두 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a1d1c-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="a1d1c-106">각 공급자에 대해 하나씩 두 개의 마이그레이션 집합을 유지 관리할 수 있으며 둘 다에서 작동할 수 있는 단일 집합으로 병합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a1d1c-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

## <a name="two-migration-sets"></a><span data-ttu-id="a1d1c-107">마이그레이션 집합 2 개</span><span class="sxs-lookup"><span data-stu-id="a1d1c-107">Two migration sets</span></span>

<span data-ttu-id="a1d1c-108">첫 번째 방법에서는 각 모델 변경에 대해 두 개의 마이그레이션을 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="a1d1c-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="a1d1c-109">이 작업을 수행 하는 한 가지 방법은 각 마이그레이션 집합을 [별도의 어셈블리에][2] 배치 하 고 두 마이그레이션 추가 간에 활성 공급자 (및 마이그레이션 어셈블리)를 수동으로 전환 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="a1d1c-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="a1d1c-110">도구를 쉽게 사용 하는 또 다른 방법은 DbContext에서 파생 되는 새 형식을 만들고 활성 공급자를 재정의 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="a1d1c-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="a1d1c-111">이 형식은 마이그레이션을 추가 하거나 적용할 때 디자인 타임에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a1d1c-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="a1d1c-112">각 마이그레이션 집합은 자체 DbContext 유형을 사용 하므로이 방법은 별도의 마이그레이션 어셈블리를 사용할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="a1d1c-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="a1d1c-113">새 마이그레이션을 추가할 때 컨텍스트 유형을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="a1d1c-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```

``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="a1d1c-114">마지막 마이그레이션에 대 한 형제로 만들어지기 때문에 후속 마이그레이션의 출력 디렉터리를 지정할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="a1d1c-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

## <a name="one-migration-set"></a><span data-ttu-id="a1d1c-115">마이그레이션 집합 하나</span><span class="sxs-lookup"><span data-stu-id="a1d1c-115">One migration set</span></span>

<span data-ttu-id="a1d1c-116">두 개의 마이그레이션 집합이 없는 경우 두 공급자에 모두 적용할 수 있는 단일 집합으로 수동으로 결합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a1d1c-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="a1d1c-117">공급자는 인식 되지 않는 주석을 무시 하므로 주석이 공존할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a1d1c-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="a1d1c-118">예를 들어 Microsoft SQL Server와 SQLite 모두에서 작동 하는 기본 키 열은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="a1d1c-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="a1d1c-119">한 공급자에 대해서만 작업을 적용할 수 있거나 공급자 간에 다르게 적용 될 수 있는 경우에는 `ActiveProvider` 속성을 사용 하 여 활성화 된 공급자를 알려 주십시오.</span><span class="sxs-lookup"><span data-stu-id="a1d1c-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```

  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
