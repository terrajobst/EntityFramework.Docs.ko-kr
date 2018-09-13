---
title: 여러 공급자-EF Core 사용 하 여 마이그레이션
author: bricelam
ms.author: bricelam
ms.date: 11/08/2017
ms.openlocfilehash: 75c055221609679db3f00016b9cb44c6c8c6e473
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488779"
---
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="9c8d5-102">여러 공급자를 사용 하 여 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="9c8d5-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="9c8d5-103">합니다 [EF Core 도구] [ 1] 만 활성 공급자에 대 한 마이그레이션을 스 캐 폴딩 합니다.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="9c8d5-104">그러나 경우에 따라 하려는 DbContext를 사용 하 여 둘 이상의 공급자 (예: Microsoft SQL Server 및 SQLite)를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="9c8d5-105">마이그레이션을 사용 하 여이 처리 하는 방법은 두 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="9c8d5-106">두 집합을 유지 관리할 수 있습니다-마이그레이션의 다른 각 공급자에는 단일으로 설정 하는 병합 작업 둘 다에서.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="9c8d5-107">두 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="9c8d5-107">Two migration sets</span></span>
------------------
<span data-ttu-id="9c8d5-108">첫 번째 접근 방식에서는 각 모델 변경에 대 한 두 마이그레이션을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="9c8d5-109">작업을 수행 하는 한 가지 방법은이 방법은 각 마이그레이션 마련 [별도 어셈블리에] [ 2] 수동으로 전환할 활성 공급자 (및 마이그레이션 어셈블리) 두 마이그레이션을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="9c8d5-110">도구를 사용 하 여 작업을 더 쉽게 하는 또 다른 방법은 DbContext에서 파생 되 고 활성 공급자를 재정의 하는 새 형식을 만드는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="9c8d5-111">이 형식은 디자인에는 추가 하거나 마이그레이션 적용 시점입니다.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="9c8d5-112">각 마이그레이션 집합 DbContext 형식을 자체를 사용 하므로이 방법은 별도 마이그레이션을 어셈블리를 사용 하 여 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="9c8d5-113">새 마이그레이션에 추가할 때 컨텍스트 형식을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="9c8d5-114">마지막 형제 항목으로 만들어지므로 이후 마이그레이션에 대 한 출력 디렉터리를 지정할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="9c8d5-115">하나의 마이그레이션 설정</span><span class="sxs-lookup"><span data-stu-id="9c8d5-115">One migration set</span></span>
-----------------
<span data-ttu-id="9c8d5-116">두 집합을 마이그레이션의 마음에 들지 않으면 두 공급자 모두에 적용할 수 있는 단일 집합을 수동으로 결합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="9c8d5-117">공급자를 인식 하지 못하는 모든 주석을 무시 하므로 주석 공존할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="9c8d5-118">예를 들어 Microsoft SQL Server 및 SQLite를 사용 하 여 작동 하는 기본 키 열이 같습니다.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="9c8d5-119">사용 하 여 작업 공급자가 하나에 적용할 수 있습니다 (또는 공급자 간에 다르게 들은) 하는 경우는 `ActiveProvider` 는 공급자가 활성 구별 하는 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
