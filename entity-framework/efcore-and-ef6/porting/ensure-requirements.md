---
title: EF6에서 EF Core로 포팅-요구 사항 유효성 검사
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 07caa39e8a56db71e493331ea9f0286309abc52a
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565348"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="0011f-102">EF6에서 EF Core로 포팅 하기 전: 응용 프로그램의 요구 사항 확인</span><span class="sxs-lookup"><span data-stu-id="0011f-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="0011f-103">포팅 프로세스를 시작 하기 전에 EF Core 응용 프로그램에 대 한 데이터 액세스 요구 사항을 충족 하는지 확인 하는 것이 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="0011f-104">누락 된 기능</span><span class="sxs-lookup"><span data-stu-id="0011f-104">Missing features</span></span>

<span data-ttu-id="0011f-105">EF Core에 응용 프로그램에서 사용 해야 하는 모든 기능이 포함 되어 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="0011f-106">EF Core의 기능 집합을 EF6와 비교 하는 방법에 대 한 자세한 내용은 [기능 비교](../features.md) 를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="0011f-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="0011f-107">필요한 기능이 누락 된 경우 EF Core으로 포팅 하기 전에 이러한 기능이 없다는 것을 보완할 수 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="0011f-108">동작 변경</span><span class="sxs-lookup"><span data-stu-id="0011f-108">Behavior changes</span></span>

<span data-ttu-id="0011f-109">EF6와 EF Core 간의 동작에 대 한 일부 변경 내용에 대 한 완전 하지 않은 목록입니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="0011f-110">응용 프로그램이 동작 하는 방식을 변경할 수 있지만 EF Core로 바꾼 후에 컴파일 오류로 표시 되지 않는 응용 프로그램의 포트를 염두에 두는 것이 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="0011f-111">DbSet. 추가/연결 및 그래프 동작</span><span class="sxs-lookup"><span data-stu-id="0011f-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="0011f-112">EF6에서는 엔터티에 대해 `DbSet.Add()` 를 호출 하면 해당 탐색 속성에서 참조 되는 모든 엔터티에 대 한 재귀 검색이 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="0011f-113">검색 된 모든 엔터티는 아직 컨텍스트에 의해 추적 되지 않으며 추가 된 것으로 표시 되기도 합니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-113">Any entities that are found, and are not already tracked by the context, are also marked as added.</span></span> <span data-ttu-id="0011f-114">`DbSet.Attach()`모든 엔터티가 변경 되지 않은 것으로 표시 된 경우를 제외 하 고는 동일 하 게 동작 합니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="0011f-115">**EF Core는 비슷한 재귀 검색을 수행 하지만 약간 다른 규칙을 사용 합니다.**</span><span class="sxs-lookup"><span data-stu-id="0011f-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="0011f-116">루트 엔터티는 항상 요청 된 상태 (에 대해 `DbSet.Add` 추가 되 고 `DbSet.Attach`변경 되지 않음)입니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="0011f-117">**탐색 속성을 재귀적으로 검색 하는 동안 발견 된 엔터티:**</span><span class="sxs-lookup"><span data-stu-id="0011f-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="0011f-118">**엔터티의 기본 키가 저장소로 생성 된 경우**</span><span class="sxs-lookup"><span data-stu-id="0011f-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="0011f-119">기본 키가 값으로 설정 되지 않으면 상태가 추가로 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="0011f-120">기본 키 값은 속성 `0` 형식 (예: `int` `null` for, for `string`, 등)에 대 한 CLR 기본값이 할당 된 경우 "설정 되지 않음"으로 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="0011f-121">기본 키가 값으로 설정 된 경우 상태는 변경 되지 않음으로 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="0011f-122">기본 키가 데이터베이스에서 생성 되지 않은 경우 엔터티는 루트와 동일한 상태에 배치 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="0011f-123">Code First 데이터베이스 초기화</span><span class="sxs-lookup"><span data-stu-id="0011f-123">Code First database initialization</span></span>

<span data-ttu-id="0011f-124">**EF6는 데이터베이스 연결을 선택 하 고 데이터베이스를 초기화 하는 데 상당한 양의 매직을 수행 합니다. 이러한 규칙 중 일부는 다음과 같습니다.**</span><span class="sxs-lookup"><span data-stu-id="0011f-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="0011f-125">구성을 수행 하지 않으면 EF6는 SQL Express 또는 LocalDb에서 데이터베이스를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="0011f-126">컨텍스트와 이름이 같은 연결 문자열이 응용 프로그램 `App/Web.config` 파일에 있는 경우이 연결이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="0011f-127">데이터베이스가 존재 하지 않는 경우 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="0011f-128">데이터베이스에 모델의 테이블이 없으면 현재 모델에 대 한 스키마가 데이터베이스에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="0011f-129">마이그레이션을 사용 하는 경우에는 데이터베이스를 만드는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="0011f-130">데이터베이스가 존재 하 고 EF6에서 이전에 스키마를 만든 경우 스키마가 현재 모델과의 호환성을 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="0011f-131">스키마가 만들어진 후 모델이 변경 되 면 예외가 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="0011f-132">**EF Core는이 매직을 수행 하지 않습니다.**</span><span class="sxs-lookup"><span data-stu-id="0011f-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="0011f-133">데이터베이스 연결은 코드에서 명시적으로 구성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="0011f-134">초기화가 수행 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-134">No initialization is performed.</span></span> <span data-ttu-id="0011f-135">를 사용 하 `DbContext.Database.Migrate()` 여 마이그레이션을 적용 하거나 `DbContext.Database.EnsureCreated()` , `EnsureDeleted()` 마이그레이션을 사용 하지 않고 데이터베이스를 만들거나 삭제 하려면를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="0011f-136">Code First 테이블 명명 규칙</span><span class="sxs-lookup"><span data-stu-id="0011f-136">Code First table naming convention</span></span>

<span data-ttu-id="0011f-137">EF6는 복수화 서비스를 통해 엔터티 클래스 이름을 실행 하 여 엔터티가 매핑되는 기본 테이블 이름을 계산 합니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="0011f-138">EF Core는 엔터티가 파생 컨텍스트에서 노출 `DbSet` 되는 속성의 이름을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="0011f-139">엔터티에 `DbSet` 속성이 없으면 클래스 이름이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0011f-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
