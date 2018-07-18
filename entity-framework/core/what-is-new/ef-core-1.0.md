---
title: EF Core 1.0의 새로운 기능 - EF Core
author: divega
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: af7cf490ef2b04afb02461279fbe67c1c7fa3d95
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949024"
---
# <a name="features-included-in-ef-core-10"></a><span data-ttu-id="8dd09-102">EF Core 1.0에 포함된 기능</span><span class="sxs-lookup"><span data-stu-id="8dd09-102">Features included in EF Core 1.0</span></span>

## <a name="platforms"></a><span data-ttu-id="8dd09-103">플랫폼</span><span class="sxs-lookup"><span data-stu-id="8dd09-103">Platforms</span></span>
### <a name="net-framework-451"></a><span data-ttu-id="8dd09-104">.NET Framework 4.5.1</span><span class="sxs-lookup"><span data-stu-id="8dd09-104">.NET Framework 4.5.1</span></span>
<span data-ttu-id="8dd09-105">콘솔, WPF, WinForms, ASP.NET 4 등을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-105">Includes Console, WPF, WinForms, ASP.NET 4, etc.</span></span>
### <a name="net-standard-13"></a><span data-ttu-id="8dd09-106">.NET Standard 1.3</span><span class="sxs-lookup"><span data-stu-id="8dd09-106">.NET Standard 1.3</span></span>
<span data-ttu-id="8dd09-107">Windows, OSX 및 Linux에서 .NET Framework 및 .NET Core를 둘 다 대상으로 하는 ASP.NET Core를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-107">Including ASP.NET Core targeting both .NET Framework and .NET Core on Windows, OSX, and Linux.</span></span>

## <a name="modelling"></a><span data-ttu-id="8dd09-108">모델링</span><span class="sxs-lookup"><span data-stu-id="8dd09-108">Modelling</span></span>
### <a name="basic-modelling"></a><span data-ttu-id="8dd09-109">기본 모델링</span><span class="sxs-lookup"><span data-stu-id="8dd09-109">Basic modelling</span></span>
<span data-ttu-id="8dd09-110">일반적인 스칼라 형식(`int`, `string` 등)의 get/set 속성이 있는 POCO 엔터티를 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-110">Based on POCO entities with get/set properties of common scalar types (`int`, `string`, etc.).</span></span>
### <a name="relationships-and-navigation-properties"></a><span data-ttu-id="8dd09-111">관계 및 탐색 속성</span><span class="sxs-lookup"><span data-stu-id="8dd09-111">Relationships and navigation properties</span></span>
<span data-ttu-id="8dd09-112">외래 키를 기반으로 모델에서 일 대 다 및 일 대 영/일 관계를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-112">One-to-many and One-to-zero-or-one relationships can be specified in the model based on a foreign key.</span></span> <span data-ttu-id="8dd09-113">단순 컬렉션 또는 참조 형식의 탐색 속성은 이러한 관계와 연결될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-113">Navigation properties of simple collection or reference types can be associated with these relationships.</span></span>
### <a name="built-in-conventions"></a><span data-ttu-id="8dd09-114">기본 제공 규칙</span><span class="sxs-lookup"><span data-stu-id="8dd09-114">Built-in conventions</span></span>
<span data-ttu-id="8dd09-115">이러한 기능은 엔터티 클래스의 모양을 기반으로 초기 모델을 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-115">These build an initial model based on the shape of the entity classes.</span></span>
### <a name="fluent-api"></a><span data-ttu-id="8dd09-116">Fluent API</span><span class="sxs-lookup"><span data-stu-id="8dd09-116">Fluent API</span></span>
<span data-ttu-id="8dd09-117">컨텍스트에서 `OnModelCreating` 메서드를 재정의하여 규칙에 의해 발견된 모델을 추가로 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-117">Allows you to override the `OnModelCreating` method on your context to further configure the model that was discovered by convention.</span></span>
### <a name="data-annotations"></a><span data-ttu-id="8dd09-118">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="8dd09-118">Data annotations</span></span>
<span data-ttu-id="8dd09-119">엔터티 클래스/속성에 추가할 수 있고 EF 모델에 영향을 주는 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-119">Are attributes that can be added to your entity classes/properties and will influence the EF model.</span></span> <span data-ttu-id="8dd09-120">예를 들어 `[Required]`를 추가하면 EF 속성이 필수임을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-120">For example, adding `[Required]` will let EF know that a property is required.</span></span>
### <a name="relational-table-mapping"></a><span data-ttu-id="8dd09-121">관계형 테이블 매핑</span><span class="sxs-lookup"><span data-stu-id="8dd09-121">Relational Table mapping</span></span>
<span data-ttu-id="8dd09-122">엔터티를 테이블/열에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-122">Allows entities to be mapped to tables/columns.</span></span>
### <a name="key-value-generation"></a><span data-ttu-id="8dd09-123">키 값 생성</span><span class="sxs-lookup"><span data-stu-id="8dd09-123">Key value generation</span></span>
<span data-ttu-id="8dd09-124">클라이언트 쪽 생성 및 데이터베이스 생성을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-124">Including client-side generation and database generation.</span></span>
### <a name="database-generated-values"></a><span data-ttu-id="8dd09-125">데이터베이스 생성 값</span><span class="sxs-lookup"><span data-stu-id="8dd09-125">Database generated values</span></span>
<span data-ttu-id="8dd09-126">삽입(기본값) 또는 업데이트(계산된 열)에 대한 값을 데이터베이스에서 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-126">Allows for values to be generated by the database on insert (default values) or update (computed columns).</span></span>
### <a name="sequences-in-sql-server"></a><span data-ttu-id="8dd09-127">SQL Server의 시퀀스</span><span class="sxs-lookup"><span data-stu-id="8dd09-127">Sequences in SQL Server</span></span>
<span data-ttu-id="8dd09-128">시퀀스 개체를 모델에 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-128">Allows for sequence objects to be defined in the model.</span></span>
### <a name="unique-constraints"></a><span data-ttu-id="8dd09-129">Unique 제약 조건</span><span class="sxs-lookup"><span data-stu-id="8dd09-129">Unique constraints</span></span>
<span data-ttu-id="8dd09-130">대체 키 정의 및 해당 키를 대상으로 하는 관계를 정의하는 기능을 허용합니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-130">Allows for the definition of alternate keys and the ability to define relationships that target that key.</span></span>
### <a name="indexes"></a><span data-ttu-id="8dd09-131">인덱스</span><span class="sxs-lookup"><span data-stu-id="8dd09-131">Indexes</span></span>
<span data-ttu-id="8dd09-132">모델의 인덱스를 정의하면 데이터베이스에 인덱스가 자동으로 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-132">Defining indexes in the model automatically introduces indexes in the database.</span></span> <span data-ttu-id="8dd09-133">고유 인덱스도 지원됩니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-133">Unique indexes are also supported.</span></span>
### <a name="shadow-state-properties"></a><span data-ttu-id="8dd09-134">섀도 상태 속성</span><span class="sxs-lookup"><span data-stu-id="8dd09-134">Shadow state properties</span></span>
<span data-ttu-id="8dd09-135">선언되지 않고 .NET 클래스에 저장되지 않았지만 EF Core에서 추적 및 업데이트할 수 있는 속성을 모델에 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-135">Allows for properties to be defined in the model that are not declared and are not stored in the .NET class but can be tracked and updated by EF Core.</span></span> <span data-ttu-id="8dd09-136">외래 키를 개체에 노출하지 않는 것이 좋을 경우 일반적으로 외래 키 속성에 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-136">Commonly used for foreign key properties when exposing these in the object is not desired.</span></span>
### <a name="table-per-hierarchy-inheritance-pattern"></a><span data-ttu-id="8dd09-137">계층별 테이블 상속 패턴</span><span class="sxs-lookup"><span data-stu-id="8dd09-137">Table-Per-Hierarchy inheritance pattern</span></span>
<span data-ttu-id="8dd09-138">데이터베이스의 지정된 레코드에 대한 엔터티 형식을 식별하기 위해 판별자 열을 사용하여 상속 계층 구조의 엔터티를 단일 테이블에 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-138">Allows entities in an inheritance hierarchy to be saved to a single table using a discriminator column to identify they entity type for a given record in the database.</span></span>
### <a name="model-validation"></a><span data-ttu-id="8dd09-139">모델 유효성 검사</span><span class="sxs-lookup"><span data-stu-id="8dd09-139">Model validation</span></span>
<span data-ttu-id="8dd09-140">모델에서 잘못된 패턴을 검색하고 유용한 오류 메시지를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-140">Detects invalid patterns in the model and provides helpful error messages.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="8dd09-141">Change tracking</span><span class="sxs-lookup"><span data-stu-id="8dd09-141">Change tracking</span></span>
### <a name="snapshot-change-tracking"></a><span data-ttu-id="8dd09-142">스냅숏 변경 내용 추적</span><span class="sxs-lookup"><span data-stu-id="8dd09-142">Snapshot change tracking</span></span>
<span data-ttu-id="8dd09-143">현재 상태를 원래 상태의 복사본(스냅숏)과 비교하여 엔터티의 변경 내용을 자동으로 검색할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-143">Allows changes in entities to be detected automatically by comparing current state against a copy (snapshot) of the original state.</span></span>
### <a name="notification-change-tracking"></a><span data-ttu-id="8dd09-144">알림 변경 내용 추적</span><span class="sxs-lookup"><span data-stu-id="8dd09-144">Notification change tracking</span></span>
<span data-ttu-id="8dd09-145">속성 값이 수정될 경우 엔터티가 변경 추적 프로그램에 알릴 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-145">Allows your entities to notify the change tracker when property values are modified.</span></span>
### <a name="accessing-tracked-state"></a><span data-ttu-id="8dd09-146">추적된 상태 액세스</span><span class="sxs-lookup"><span data-stu-id="8dd09-146">Accessing tracked state</span></span>
<span data-ttu-id="8dd09-147">`DbContext.Entry` 및 `DbContext.ChangeTracker`를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-147">Via `DbContext.Entry` and `DbContext.ChangeTracker`.</span></span>
### <a name="attaching-detached-entitiesgraphs"></a><span data-ttu-id="8dd09-148">분리된 엔터티/그래프 연결</span><span class="sxs-lookup"><span data-stu-id="8dd09-148">Attaching detached entities/graphs</span></span>
<span data-ttu-id="8dd09-149">새 `DbContext.AttachGraph` API를 통해 새로운/수정된 엔터티를 저장하기 위해 엔터티를 컨텍스트에 다시 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-149">The new `DbContext.AttachGraph` API helps re-attach entities to a context in order to save new/modified entities.</span></span>

## <a name="saving-data"></a><span data-ttu-id="8dd09-150">데이터 저장</span><span class="sxs-lookup"><span data-stu-id="8dd09-150">Saving data</span></span>
### <a name="basic-save-functionality"></a><span data-ttu-id="8dd09-151">기본 저장 기능</span><span class="sxs-lookup"><span data-stu-id="8dd09-151">Basic save functionality</span></span>
<span data-ttu-id="8dd09-152">엔터티 인스턴스의 변경 내용이 데이터베이스에 지속될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-152">Allows changes to entity instances to be persisted to the database.</span></span>
### <a name="optimistic-concurrency"></a><span data-ttu-id="8dd09-153">낙관적 동시성</span><span class="sxs-lookup"><span data-stu-id="8dd09-153">Optimistic Concurrency</span></span>
<span data-ttu-id="8dd09-154">데이터베이스에서 데이터가 페치된 이후 다른 사용자가 변경한 내용을 덮어쓰지 못하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-154">Protects against overwriting changes made by another user since data was fetched from the database.</span></span>
### <a name="async-savechanges"></a><span data-ttu-id="8dd09-155">비동기 SaveChanges</span><span class="sxs-lookup"><span data-stu-id="8dd09-155">Async SaveChanges</span></span>
<span data-ttu-id="8dd09-156">데이터베이스가 `SaveChanges`에서 실행된 명령을 처리하는 동안 다른 요청을 처리하도록 현재 스레드를 해제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-156">Can free up the current thread to process other requests while the database processes the commands issued from `SaveChanges`.</span></span>
### <a name="database-transactions"></a><span data-ttu-id="8dd09-157">데이터베이스 트랜잭션</span><span class="sxs-lookup"><span data-stu-id="8dd09-157">Database Transactions</span></span>
<span data-ttu-id="8dd09-158">`SaveChanges`는 항상 원자성입니다(완전히 성공하거나 데이터베이스가 변경되지 않음).</span><span class="sxs-lookup"><span data-stu-id="8dd09-158">Means that `SaveChanges` is always atomic (meaning it either completely succeeds, or no changes are made to the database).</span></span> <span data-ttu-id="8dd09-159">컨텍스트 인스턴스 간에 트랜잭션 공유를 허용하는 트랜잭션 관련 API도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-159">There are also transaction related APIs to allow sharing transactions between context instances etc.</span></span>
### <a name="relational-batching-of-statements"></a><span data-ttu-id="8dd09-160">관계형: 문 일괄 처리</span><span class="sxs-lookup"><span data-stu-id="8dd09-160">Relational: Batching of statements</span></span>
<span data-ttu-id="8dd09-161">여러 INSERT/UPDATE/DELETE 명령을 데이터베이스에 대한 단일 왕복으로 일괄 처리하여 성능을 개선합니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-161">Provides better performance by batching up multiple INSERT/UPDATE/DELETE commands into a single roundtrip to the database.</span></span>

## <a name="query"></a><span data-ttu-id="8dd09-162">Query</span><span class="sxs-lookup"><span data-stu-id="8dd09-162">Query</span></span>
### <a name="basic-linq-support"></a><span data-ttu-id="8dd09-163">기본 LINQ 지원</span><span class="sxs-lookup"><span data-stu-id="8dd09-163">Basic LINQ support</span></span>
<span data-ttu-id="8dd09-164">LINQ를 사용하여 데이터베이스에서 데이터를 검색하는 기능을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-164">Provides the ability to use LINQ to retrieve data from the database.</span></span>
### <a name="mixed-clientserver-evaluation"></a><span data-ttu-id="8dd09-165">혼합 클라이언트/서버 평가</span><span class="sxs-lookup"><span data-stu-id="8dd09-165">Mixed client/server evaluation</span></span>
<span data-ttu-id="8dd09-166">데이터베이스에서 평가할 수 없는 논리를 쿼리에 포함할 수 있으므로 데이터가 메모리로 검색된 후 평가되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-166">Enables queries to contain logic that cannot be evaluated in the database, and must therefore be evaluated after the data is retrieved into memory.</span></span>
### <a name="notracking"></a><span data-ttu-id="8dd09-167">NoTracking</span><span class="sxs-lookup"><span data-stu-id="8dd09-167">NoTracking</span></span>
<span data-ttu-id="8dd09-168">쿼리를 사용하면 컨텍스트가 엔터티 인스턴스의 변경 내용을 모니터링할 필요가 없을 경우(결과가 읽기 전용인 경우 유용함) 쿼리 실행이 빨라집니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-168">Queries enables quicker query execution when the context does not need to monitor for changes to the entity instances (this is useful if the results are read-only).</span></span>
### <a name="eager-loading"></a><span data-ttu-id="8dd09-169">즉시 로드</span><span class="sxs-lookup"><span data-stu-id="8dd09-169">Eager loading</span></span>
<span data-ttu-id="8dd09-170">쿼리 시 페치해야하는 관련 데이터를 식별하는 `Include` 및 `ThenInclude` 메서드를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-170">Provides the `Include` and `ThenInclude` methods to identify related data that should also be fetched when querying.</span></span>
### <a name="async-query"></a><span data-ttu-id="8dd09-171">비동기 쿼리</span><span class="sxs-lookup"><span data-stu-id="8dd09-171">Async query</span></span>
<span data-ttu-id="8dd09-172">데이터베이스가 쿼리를 처리하는 동안 다른 요청을 처리하도록 현재 스레드 및 연결된 리소스를 해제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-172">Can free up the current thread (and it's associated resources) to process other requests while the database processes the query.</span></span>
### <a name="raw-sql-queries"></a><span data-ttu-id="8dd09-173">원시 SQL 쿼리</span><span class="sxs-lookup"><span data-stu-id="8dd09-173">Raw SQL queries</span></span>
<span data-ttu-id="8dd09-174">원시 SQL 쿼리를 사용하여 데이터를 페치하는 `DbSet.FromSql` 메서드를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-174">Provides the `DbSet.FromSql` method to use raw SQL queries to fetch data.</span></span> <span data-ttu-id="8dd09-175">이러한 쿼리는 LINQ 사용 시 작성할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-175">These queries can also be composed on using LINQ.</span></span>

## <a name="database-schema-management"></a><span data-ttu-id="8dd09-176">데이터베이스 스키마 관리</span><span class="sxs-lookup"><span data-stu-id="8dd09-176">Database schema management</span></span>       
### <a name="database-creationdeletion-apis"></a><span data-ttu-id="8dd09-177">데이터베이스 만들기/삭제 API</span><span class="sxs-lookup"><span data-stu-id="8dd09-177">Database creation/deletion APIs</span></span>
<span data-ttu-id="8dd09-178">일반적으로 마이그레이션을 사용하지 않고 데이터베이스를 빠르게 생성/삭제하려는 테스트용으로 디자인됩니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-178">Are mostly designed for testing where you want to quickly create/delete the database without using migrations.</span></span>
### <a name="relational-database-migrations"></a><span data-ttu-id="8dd09-179">관계형 데이터베이스 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="8dd09-179">Relational database migrations</span></span>
<span data-ttu-id="8dd09-180">시간이 지나면서 모델이 변경됨에 따라 관계형 데이터베이스 스키마를 진전시킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-180">Allow a relational database schema to evolve overtime as your model changes.</span></span>
### <a name="reverse-engineer-from-database"></a><span data-ttu-id="8dd09-181">데이터베이스의 리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="8dd09-181">Reverse engineer from database</span></span>
<span data-ttu-id="8dd09-182">기존 관계형 데이터베이스 스키마를 기반으로 EF 모델을 스캐폴딩합니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-182">Scaffolds an EF model based on an existing relational database schema.</span></span>

## <a name="database-providers"></a><span data-ttu-id="8dd09-183">데이터베이스 공급자</span><span class="sxs-lookup"><span data-stu-id="8dd09-183">Database providers</span></span>
### <a name="sql-server"></a><span data-ttu-id="8dd09-184">SQL Server</span><span class="sxs-lookup"><span data-stu-id="8dd09-184">SQL Server</span></span>
<span data-ttu-id="8dd09-185">Microsoft SQL Server 2008 이상에 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-185">Connects to Microsoft SQL Server 2008 onwards.</span></span>
### <a name="sqlite"></a><span data-ttu-id="8dd09-186">SQLite</span><span class="sxs-lookup"><span data-stu-id="8dd09-186">SQLite</span></span>
<span data-ttu-id="8dd09-187">SQLite 3 데이터베이스에 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-187">Connects to a SQLite 3 database.</span></span>
### <a name="in-memory"></a><span data-ttu-id="8dd09-188">메모리 내</span><span class="sxs-lookup"><span data-stu-id="8dd09-188">In-Memory</span></span>
<span data-ttu-id="8dd09-189">실제 데이터베이스에 연결하지 않고 쉽게 테스트할 수 있도록 디자인되었습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-189">Is designed to easily enable testing without connecting to a real database.</span></span>
### <a name="3rd-party-providers"></a><span data-ttu-id="8dd09-190">타사 공급자</span><span class="sxs-lookup"><span data-stu-id="8dd09-190">3rd party providers</span></span>
<span data-ttu-id="8dd09-191">다른 데이터베이스 엔진에 대해 여러 공급자를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8dd09-191">Several providers are available for other database engines.</span></span> <span data-ttu-id="8dd09-192">전체 목록은 [데이터베이스 공급자](../providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="8dd09-192">See [Database Providers](../providers/index.md) for a complete list.</span></span>
