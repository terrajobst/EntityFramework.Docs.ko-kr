---
title: 마이그레이션 기록 테이블-EF6를 사용자 지정
author: divega
ms.date: 10/23/2016
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: e3faefc4b812ec4bc440ed2bb48747053d8cb1b3
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283695"
---
# <a name="customizing-the-migrations-history-table"></a><span data-ttu-id="b9975-102">마이그레이션 기록 테이블을 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="b9975-102">Customizing the migrations history table</span></span>
> [!NOTE]
> <span data-ttu-id="b9975-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="b9975-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

> [!NOTE]
> <span data-ttu-id="b9975-105">이 문서에서는 기본 시나리오에서 Code First 마이그레이션을 사용 하는 방법을 알고 있다고 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="b9975-106">이렇게 하지 않으면 경우 읽기 해야 [Code First 마이그레이션을](~/ef6/modeling/code-first/migrations/index.md) 계속 하기 전에 합니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="what-is-migrations-history-table"></a><span data-ttu-id="b9975-107">마이그레이션 기록 테이블 이란?</span><span class="sxs-lookup"><span data-stu-id="b9975-107">What is Migrations History Table?</span></span>

<span data-ttu-id="b9975-108">마이그레이션 기록 테이블은 데이터베이스에 적용 되는 마이그레이션에 대 한 정보를 저장 하기 위해 Code First 마이그레이션을 사용 하는 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-108">Migrations history table is a table used by Code First Migrations to store details about migrations applied to the database.</span></span> <span data-ttu-id="b9975-109">기본적으로 데이터베이스에서 테이블의 이름은 \_ \_MigrationHistory 하며 첫 번째 마이그레이션 수행 된 데이터베이스를 적용 하는 경우 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-109">By default the name of the table in the database is \_\_MigrationHistory and it is created when applying the first migration do the database.</span></span> <span data-ttu-id="b9975-110">Entity Framework 5이이 테이블 응용 프로그램이 Microsoft Sql Server 데이터베이스를 사용 하는 경우 시스템 테이블을 했습니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-110">In Entity Framework 5 this table was a system table if the application used Microsoft Sql Server database.</span></span> <span data-ttu-id="b9975-111">그러나 Entity Framework 6에서 변경 된이 및 마이그레이션 기록 테이블의 시스템 테이블을 더 이상 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-111">This has changed in Entity Framework 6 however and the migrations history table is no longer marked a system table.</span></span>

## <a name="why-customize-migrations-history-table"></a><span data-ttu-id="b9975-112">마이그레이션 기록 테이블을 사용자 지정 이유?</span><span class="sxs-lookup"><span data-stu-id="b9975-112">Why customize Migrations History Table?</span></span>

<span data-ttu-id="b9975-113">마이그레이션 기록 테이블 에서만 Code First 마이그레이션을 사용 해야 하 고 수동으로 변경 하면 마이그레이션을 중단할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-113">Migrations history table is supposed to be used solely by Code First Migrations and changing it manually can break migrations.</span></span> <span data-ttu-id="b9975-114">그러나 경우에 따라 기본 구성은 적합 하지 않습니다 및 사용자 지정, 예를 들어 테이블에 필요:</span><span class="sxs-lookup"><span data-stu-id="b9975-114">However sometimes the default configuration is not suitable and the table needs to be customized, for instance:</span></span>

-   <span data-ttu-id="b9975-115">이름 및/또는 3을 사용 하려면 열의 측면을 변경 해야 할<sup>rd</sup> 파티 마이그레이션 공급자</span><span class="sxs-lookup"><span data-stu-id="b9975-115">You need to change names and/or facets of the columns to enable a 3<sup>rd</sup> party Migrations provider</span></span>
-   <span data-ttu-id="b9975-116">테이블의 이름을 변경.</span><span class="sxs-lookup"><span data-stu-id="b9975-116">You want to change the name of the table</span></span>
-   <span data-ttu-id="b9975-117">에 대 한 기본이 아닌 스키마를 사용 해야 합니다 \_ \_MigrationHistory 테이블</span><span class="sxs-lookup"><span data-stu-id="b9975-117">You need to use a non-default schema for the \_\_MigrationHistory table</span></span>
-   <span data-ttu-id="b9975-118">컨텍스트는 지정 된 버전에 대 한 추가 데이터를 저장 해야 하며 테이블에 추가 열을 추가 해야 하므로</span><span class="sxs-lookup"><span data-stu-id="b9975-118">You need to store additional data for a given version of the context and therefore you need to add an additional column to the table</span></span>

## <a name="words-of-precaution"></a><span data-ttu-id="b9975-119">예방 조치로 단어</span><span class="sxs-lookup"><span data-stu-id="b9975-119">Words of precaution</span></span>

<span data-ttu-id="b9975-120">마이그레이션 기록 테이블을 변경 하는 것은 강력 하지만 지나치게 많이 되지 않도록 주의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-120">Changing the migration history table is powerful but you need to be careful to not overdo it.</span></span> <span data-ttu-id="b9975-121">EF 런타임에 현재 확인 하지 여부를 사용자 지정된 마이그레이션 기록 테이블은는 런타임과 호환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-121">EF runtime currently does not check whether the customized migrations history table is compatible with the runtime.</span></span> <span data-ttu-id="b9975-122">응용 프로그램이 없는 경우 런타임 시 중단 또는 예기치 않은 방식으로 동작 하도록 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-122">If it is not your application may break at runtime or behave in unpredictable ways.</span></span> <span data-ttu-id="b9975-123">이 훨씬 더 중요 데이터베이스별 다중 컨텍스트를 사용 하는 경우는 경우 여러 컨텍스트에서 사용할 수 동일한 마이그레이션 기록 테이블의 마이그레이션에 대 한 정보를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-123">This is even more important if you use multiple contexts per database in which case multiple contexts can use the same migration history table to store information about migrations.</span></span>

## <a name="how-to-customize-migrations-history-table"></a><span data-ttu-id="b9975-124">마이그레이션 기록 테이블을 사용자 지정 하는 방법</span><span class="sxs-lookup"><span data-stu-id="b9975-124">How to customize Migrations History Table?</span></span>

<span data-ttu-id="b9975-125">시작 하기 전에 알아야 첫 번째 마이그레이션을 적용 하기 전에만 마이그레이션 기록 테이블 사용자 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-125">Before you start you need to know that you can customize the migrations history table only before you apply the first migration.</span></span> <span data-ttu-id="b9975-126">이제 코드입니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-126">Now, to the code.</span></span>

<span data-ttu-id="b9975-127">첫째, System.Data.Entity.Migrations.History.HistoryContext 클래스에서 파생 된 클래스를 만드는 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-127">First, you will need to create a class derived from System.Data.Entity.Migrations.History.HistoryContext class.</span></span> <span data-ttu-id="b9975-128">마이그레이션 기록 테이블을 구성 하는 것은 매우 유사 fluent API를 사용 하 여 EF 모델을 구성 하므로 HistoryContext 클래스는 DbContext 클래스에서 파생 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-128">The HistoryContext class is derived from the DbContext class so configuring the migrations history table is very similar to configuring EF models with fluent API.</span></span> <span data-ttu-id="b9975-129">OnModelCreating 메서드를 재정의 하 여 fluent API를 사용 하 여 테이블을 구성 하기만 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-129">You just need to override the OnModelCreating method and use fluent API to configure the table.</span></span>

>[!NOTE]
> <span data-ttu-id="b9975-130">일반적으로 EF 모델을 구성한 경우에 기본 호출할 필요가 없습니다. DbContext.OnModelCreating() 이후 OnModelCreating 메서드 재정의에서 onmodelcreating ()의 본문이 비어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-130">Typically when you configure EF models you don’t need to call base.OnModelCreating() from the overridden OnModelCreating method since the DbContext.OnModelCreating() has empty body.</span></span> <span data-ttu-id="b9975-131">마이그레이션 기록 테이블을 구성 하는 경우에 해당 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-131">This is not the case when configuring the migrations history table.</span></span> <span data-ttu-id="b9975-132">이 경우 첫 번째 onmodelcreating () 재정의에서 할 일은 실제로 기본를 호출 하는 것입니다. Onmodelcreating ()입니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-132">In this case the first thing to do in your OnModelCreating() override is to actually call base.OnModelCreating().</span></span> <span data-ttu-id="b9975-133">이렇게 하면 마이그레이션 기록 테이블에서 메서드를 재정의 한 다음 조정 하는 기본 방식으로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-133">This will configure the migrations history table in the default way which you then tweak in the overriding method.</span></span>

<span data-ttu-id="b9975-134">마이그레이션 기록 테이블의 이름을 바꾸고 "관리자" 라는 사용자 지정 스키마를 저장 하려는 경우를 가정해 봅니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-134">Let’s say you want to rename the migrations history table and put it to a custom schema called “admin”.</span></span> <span data-ttu-id="b9975-135">또한 DBA 알려주실 것 마이그레이션 MigrationId 열 이름을 바꾸려면\_id입니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-135">In addition your DBA would like you to rename the MigrationId column to Migration\_ID.</span></span>  <span data-ttu-id="b9975-136">HistoryContext에서 파생 된 다음 클래스를 만들어이 달성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-136">You could achieve this by creating the following class derived from HistoryContext:</span></span>

``` csharp
    using System.Data.Common;
    using System.Data.Entity;
    using System.Data.Entity.Migrations.History;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class MyHistoryContext : HistoryContext
        {
            public MyHistoryContext(DbConnection dbConnection, string defaultSchema)
                : base(dbConnection, defaultSchema)
            {
            }

            protected override void OnModelCreating(DbModelBuilder modelBuilder)
            {
                base.OnModelCreating(modelBuilder);
                modelBuilder.Entity<HistoryRow>().ToTable(tableName: "MigrationHistory", schemaName: "admin");
                modelBuilder.Entity<HistoryRow>().Property(p => p.MigrationId).HasColumnName("Migration_ID");
            }
        }
    }
```

<span data-ttu-id="b9975-137">EF를 통해 등록 하 여 인식 확인 해야 하는 사용자 지정 프로그램 HistoryContext 준비 되 면 [코드 기반 구성](https://msdn.com/data/jj680699):</span><span class="sxs-lookup"><span data-stu-id="b9975-137">Once your custom HistoryContext is ready you need to make EF aware of it by registering it via [code-based configuration](https://msdn.com/data/jj680699):</span></span>

``` csharp
    using System.Data.Entity;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class ModelConfiguration : DbConfiguration
        {
            public ModelConfiguration()
            {
                this.SetHistoryContext("System.Data.SqlClient",
                    (connection, defaultSchema) => new MyHistoryContext(connection, defaultSchema));
            }
        }
    }
```

<span data-ttu-id="b9975-138">이제 거의 완성 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-138">That’s pretty much it.</span></span> <span data-ttu-id="b9975-139">Enable-migrations, 패키지 관리자 콘솔을 이동할 수 있습니다 이제 추가 마이그레이션 및 마지막으로 데이터베이스 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-139">Now you can go to the Package Manager Console, Enable-Migrations, Add-Migration and finally Update-Database.</span></span> <span data-ttu-id="b9975-140">마이그레이션 기록 테이블 HistoryContext 파생 클래스에서 지정 된 세부 정보에 따라 구성 데이터베이스에 추가 그러면 합니다.</span><span class="sxs-lookup"><span data-stu-id="b9975-140">This should result in adding to the database a migrations history table configured according to the details you specified in your HistoryContext derived class.</span></span>

![데이터베이스](~/ef6/media/database.png)
