---
title: EF6에서 EF Core로 이식 - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 826b58bd-77b0-4bbc-bfcd-24d1ed3a8f38
uid: efcore-and-ef6/porting/index
ms.openlocfilehash: 77096b9bffba6b8c2a3d7bfb0c2e41e2d170a7db
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182086"
---
# <a name="porting-from-ef6-to-ef-core"></a><span data-ttu-id="f0cdb-102">EF6에서 EF Core로 이식</span><span class="sxs-lookup"><span data-stu-id="f0cdb-102">Porting from EF6 to EF Core</span></span>

<span data-ttu-id="f0cdb-103">EF Core의 기본적인 변경 내용 때문에, 변경할 중요한 이유가 있는 경우에만 EF6 애플리케이션을 EF Core로 이동하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-103">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span>
<span data-ttu-id="f0cdb-104">EF6에서 EF Core로의 이동은 업그레이드가 아닌 이식으로 이해해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-104">You should view the move from EF6 to EF Core as a port rather than an upgrade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f0cdb-105">포팅 프로세스를 시작하기 전에 EF Core가 애플리케이션에 대한 데이터 액세스 요구 사항을 충족하는지 확인하는 것이 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-105">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="f0cdb-106">누락된 기능</span><span class="sxs-lookup"><span data-stu-id="f0cdb-106">Missing features</span></span>

<span data-ttu-id="f0cdb-107">EF Core에 애플리케이션에서 사용해야 하는 모든 기능이 포함되어 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-107">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="f0cdb-108">EF Core의 기능 집합을 EF6와 비교하는 방법에 대한 자세한 내용은 [기능 비교](xref:efcore-and-ef6/index)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-108">See [Feature Comparison](xref:efcore-and-ef6/index) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="f0cdb-109">필수 기능이 누락된 경우 EF Core로 포팅하기 전에 이러한 기능 부족을 보완할 수 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-109">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="f0cdb-110">동작 변경</span><span class="sxs-lookup"><span data-stu-id="f0cdb-110">Behavior changes</span></span>

<span data-ttu-id="f0cdb-111">이는 EF6와 EF Core 간 동작의 일부 변경 내용에 대한 완전하지 않은 목록입니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-111">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="f0cdb-112">애플리케이션이 동작하는 방식을 변경할 수 있지만 EF Core로 변경한 후에 컴파일 오류로 표시되지 않으므로 애플리케이션의 포팅을 염두에 두는 것이 중요합니다</span><span class="sxs-lookup"><span data-stu-id="f0cdb-112">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="f0cdb-113">DbSet.Add/Attach 및 그래프 동작</span><span class="sxs-lookup"><span data-stu-id="f0cdb-113">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="f0cdb-114">EF6에서 엔터티에 대해 `DbSet.Add()`를 호출하면 해당 탐색 속성에서 참조되는 모든 엔터티에 대한 재귀 검색이 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-114">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="f0cdb-115">검색된 모든 엔터티는 아직 컨텍스트에 의해 추적되지 않으며, 추가된 것으로 표시되기도 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-115">Any entities that are found, and are not already tracked by the context, are also marked as added.</span></span> <span data-ttu-id="f0cdb-116">모든 엔터티가 변경되지 않은 것으로 표시된 경우를 제외하고 `DbSet.Attach()`는 동일하게 동작합니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-116">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="f0cdb-117">**EF Core는 비슷한 재귀 검색을 수행하지만 약간 다른 규칙을 사용합니다.**</span><span class="sxs-lookup"><span data-stu-id="f0cdb-117">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="f0cdb-118">루트 엔터티는 항상 요청된 상태(`DbSet.Add`에 대해 추가되고 `DbSet.Attach`에 대해 변경 되지 않음)입니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-118">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="f0cdb-119">**탐색 속성을 재귀적으로 검색하는 동안 발견된 엔터티:**</span><span class="sxs-lookup"><span data-stu-id="f0cdb-119">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="f0cdb-120">**엔터티의 기본 키가 저장소로 생성된 경우**</span><span class="sxs-lookup"><span data-stu-id="f0cdb-120">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="f0cdb-121">기본 키가 값으로 설정되지 않으면 상태가 추가로 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-121">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="f0cdb-122">기본 키 값은 속성 형식(예: `int`의 `0`, `string`의 `null` 등)에 대한 CLR 기본값이 할당된 경우 "설정되지 않음"으로 간주됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-122">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="f0cdb-123">기본 키가 값으로 설정되면 상태는 변경되지 않음으로 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-123">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="f0cdb-124">기본 키가 데이터베이스에서 생성되지 않은 경우 엔터티는 루트와 동일한 상태에 배치됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-124">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="f0cdb-125">Code First 데이터베이스 초기화</span><span class="sxs-lookup"><span data-stu-id="f0cdb-125">Code First database initialization</span></span>

<span data-ttu-id="f0cdb-126">**EF6에는 데이터베이스 연결을 선택하고 데이터베이스를 초기화하는 데 사용하는 상당한 양의 매직이 있습니다. 이러한 규칙은 다음과 같습니다.**</span><span class="sxs-lookup"><span data-stu-id="f0cdb-126">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="f0cdb-127">구성을 수행하지 않으면 EF6는 SQL Express 또는 LocalDb에서 데이터베이스를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-127">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="f0cdb-128">컨텍스트와 동일한 이름의 연결 문자열이 애플리케이션 `App/Web.config` 파일에 있는 경우 이 연결이 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-128">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="f0cdb-129">데이터베이스가 없으면 해당 데이터베이스가 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-129">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="f0cdb-130">데이터베이스에 모델의 테이블이 없으면 현재 모델에 대한 스키마가 데이터베이스에 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-130">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="f0cdb-131">마이그레이션을 사용하는 경우에는 데이터베이스를 만드는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-131">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="f0cdb-132">데이터베이스가 있고 EF6에서 이전에 스키마를 만든 경우 스키마가 현재 모델과의 호환성을 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-132">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="f0cdb-133">스키마가 만들어진 후 모델이 변경되면 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-133">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="f0cdb-134">**EF Core는 이 매직을 수행하지 않습니다.**</span><span class="sxs-lookup"><span data-stu-id="f0cdb-134">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="f0cdb-135">데이터베이스 연결은 코드에서 명시적으로 구성해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-135">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="f0cdb-136">초기화는 수행되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-136">No initialization is performed.</span></span> <span data-ttu-id="f0cdb-137">`DbContext.Database.Migrate()`를 사용하여 마이그레이션을 적용(또는 `DbContext.Database.EnsureCreated()` 및 `EnsureDeleted()`를 사용하여 마이그레이션 없이 데이터베이스를 생성/삭제)해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-137">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="f0cdb-138">Code First 테이블 명명 규칙</span><span class="sxs-lookup"><span data-stu-id="f0cdb-138">Code First table naming convention</span></span>

<span data-ttu-id="f0cdb-139">EF6는 복수화 서비스를 통해 엔터티 클래스 이름을 실행하여 엔터티가 매핑되는 기본 테이블 이름을 계산합니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-139">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="f0cdb-140">EF Core는 엔터티가 파생 컨텍스트에서 노출되는 `DbSet` 속성의 이름을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-140">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="f0cdb-141">엔터티에 `DbSet` 속성이 없으면 클래스 이름이 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0cdb-141">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
