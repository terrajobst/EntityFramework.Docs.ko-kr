---
title: "여러 공급자-EF 코어와 마이그레이션"
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 6b278a5ae270b6a84269dffd72eeca609b168cdd
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/28/2017
---
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="fb415-102">여러 공급자와 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="fb415-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="fb415-103">[EF 핵심 도구] [ 1] 만 활성 중인 공급자에 대 한 마이그레이션을 스 캐 폴드 합니다.</span><span class="sxs-lookup"><span data-stu-id="fb415-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="fb415-104">그러나 경우에 따라 수 있습니다 하려는 프로그램 DbContext에 둘 이상의 공급자 (예: Microsoft SQL Server 및 SQLite)를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="fb415-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="fb415-105">마이그레이션과 함께이 처리 하는 방법은 두 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fb415-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="fb415-106">두 집합을 유지 관리할 수 있습니다는 마이그레이션-둘 다에서 작업할 수 있습니다 각 공급자--또는 단일으로 설정 하는 병합에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="fb415-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="fb415-107">두 마이그레이션 집합</span><span class="sxs-lookup"><span data-stu-id="fb415-107">Two migration sets</span></span>
------------------
<span data-ttu-id="fb415-108">첫 번째 접근 방식에서 변경 될 때마다 모델에 대 한 두 마이그레이션 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fb415-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="fb415-109">작업을 수행 하는 한 가지 방법은이 통해 각 마이그레이션 집합 배치 [별도 어셈블리에] [ 2] 수동으로 추가 두 마이그레이션 사이의 활성 공급자 (및 마이그레이션 어셈블리)를 전환 합니다.</span><span class="sxs-lookup"><span data-stu-id="fb415-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="fb415-110">새 형식을 만드는 쉽게 도구와 함께 작업 임을 또 다른 방법은 프로그램 DbContext에서 파생 되 고 활성 공급자를 재정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="fb415-110">Another approach that makes working with the tools easier is to create a new type derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="fb415-111">이 형식은 디자인에 사용 됩니다 때 추가 하거나 마이그레이션을 적용 합니다.</span><span class="sxs-lookup"><span data-stu-id="fb415-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="fb415-112">각 마이그레이션 세트 DbContext 형식 자체를 사용 하므로이 방법은 별도 마이그레이션을 어셈블리를 사용 하 여 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="fb415-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="fb415-113">새 마이그레이션에 추가할 때는 컨텍스트 형식을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="fb415-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="fb415-114">마지막 형제 항목으로 작성 되었으므로 후속 마이그레이션에 대 한 출력 디렉터리를 지정할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="fb415-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="fb415-115">하나의 마이그레이션 설정</span><span class="sxs-lookup"><span data-stu-id="fb415-115">One migration set</span></span>
-----------------
<span data-ttu-id="fb415-116">두 집합을 마이그레이션의 마음에 들지 않으면 두 공급자 모두에 적용할 수 있는 단일 집합 수동으로 결합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fb415-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="fb415-117">공급자가 인식 하지 못하는 모든 주석을 무시 하므로 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fb415-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="fb415-118">예를 들어 Microsoft SQL Server와 SQLite와 작동 하는 기본 키 열이 같습니다.</span><span class="sxs-lookup"><span data-stu-id="fb415-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="fb415-119">사용 하 여 작업 공급자를 하나에 적용할 수 있습니다 (또는 공급자 간의 다르게 하기가) 하는 경우는 `ActiveProvider` 활성 공급자를 구별 하는 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="fb415-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
