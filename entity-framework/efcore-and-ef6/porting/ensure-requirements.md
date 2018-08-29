---
title: EF6에서 EF Core-로 이식 요구 사항 유효성 검사
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: fd378c72a3c8de4a441861b3c52b94eba5f7e5b4
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993442"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="eff2e-102">EF6에서 EF Core로 이식 하기 전에: 응용 프로그램의 요구 사항 유효성 검사</span><span class="sxs-lookup"><span data-stu-id="eff2e-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="eff2e-103">이식 프로세스를 시작 하기 전에 EF Core 응용 프로그램에 대 한 데이터 액세스 요구 사항을 충족 하는지 유효성을 검사 하는 것이 반드시 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="eff2e-104">누락 된 기능</span><span class="sxs-lookup"><span data-stu-id="eff2e-104">Missing features</span></span>

<span data-ttu-id="eff2e-105">EF Core 응용 프로그램에서 사용 하는 데 필요한 모든 기능에 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="eff2e-106">참조 [기능 비교](../features.md) EF6에 EF Core에서 설정 하는 기능을 비교 하는 하는 방법을 자세히 비교 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="eff2e-107">모든 필수 기능이 누락 되었는지, EF Core로 이식 하기 전에 기능이 부족 한 보완할 수 있는지를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="eff2e-108">동작 변경 내용</span><span class="sxs-lookup"><span data-stu-id="eff2e-108">Behavior changes</span></span>

<span data-ttu-id="eff2e-109">EF6 및 EF Core 간의 동작 변경 목록이 완전 하지 않은 경우</span><span class="sxs-lookup"><span data-stu-id="eff2e-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="eff2e-110">유념 하 포트로 응용 프로그램으로 서 응용 프로그램의 동작 방식, 하지만 표시 되지 것입니다 컴파일 오류로 바꾼 후 EF Core로 변경할 수 있지만 두는 것이 반드시 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="eff2e-111">DbSet.Add/Attach 및 그래프 동작</span><span class="sxs-lookup"><span data-stu-id="eff2e-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="eff2e-112">Ef6과 호출 `DbSet.Add()` 해당 탐색 속성에서 참조 된 모든 엔터티를 재귀적으로 검색에는 엔터티 결과에서 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="eff2e-113">검색 되 고, 이미 컨텍스트에 의해 추적 되지 않습니다 하는 모든 엔터티도 추가 하는 것으로 표시 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-113">Any entities that are found, and are not already tracked by the context, are also be marked as added.</span></span> <span data-ttu-id="eff2e-114">`DbSet.Attach()` 모든 엔터티가 표시 됩니다 점을 제외 하 고는 동일 하 게 동작으로 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="eff2e-115">**EF Core는 비슷한 재귀 검색을 수행 하지만 약간 다른 몇 가지 규칙이 포함 됩니다.**</span><span class="sxs-lookup"><span data-stu-id="eff2e-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="eff2e-116">루트 엔터티는 항상 요청 된 상태 (에 대 한 추가 `DbSet.Add` 에 대 한 채 `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="eff2e-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="eff2e-117">**탐색 속성의 재귀 검색 중에 발견 된 엔터티:**</span><span class="sxs-lookup"><span data-stu-id="eff2e-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="eff2e-118">**엔터티의 기본 키 저장소가 생성 된 경우**</span><span class="sxs-lookup"><span data-stu-id="eff2e-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="eff2e-119">기본 키 값으로 설정 하지 않으면, 상태에 추가 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="eff2e-120">기본 키 값 경우 것으로 간주 "설정 안 함" 속성 형식에 대 한 CLR 기본 값이 할당 됩니다 (예를 들어 `0` 에 대 한 `int`를 `null` 에 대 한 `string`등.).</span><span class="sxs-lookup"><span data-stu-id="eff2e-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="eff2e-121">기본 키를 값으로 설정 되 면 상태는 unchanged로 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="eff2e-122">기본 키가 데이터베이스에서 생성 된, 엔터티 루트로 동일한 상태로 전환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="eff2e-123">첫 번째 데이터베이스 초기화 코드</span><span class="sxs-lookup"><span data-stu-id="eff2e-123">Code First database initialization</span></span>

<span data-ttu-id="eff2e-124">**EF6에 상당한 양의 데이터베이스 연결을 선택 하 고 데이터베이스를 초기화할 주위 수행 하는 기능이 있습니다. 이러한 규칙 중 일부는 다음과 같습니다.**</span><span class="sxs-lookup"><span data-stu-id="eff2e-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="eff2e-125">구성 되어 EF6는 SQL Express 또는 LocalDb 데이터베이스를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="eff2e-126">동일한 이름으로 컨텍스트를 사용 하 여 연결 문자열은 응용 프로그램의 경우 `App/Web.config` 파일을이 연결을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="eff2e-127">데이터베이스 존재 하지 않는 경우 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="eff2e-128">모델에서 테이블이 데이터베이스에 있으면 현재 모델에 대 한 스키마는 데이터베이스에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="eff2e-129">마이그레이션을 사용 하는 경우 다음 사용할 데이터베이스를 만들려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="eff2e-130">데이터베이스가 존재 하 고 EF6 이전에 스키마를 만들었으며, 경우에 현재 모델을 사용 하 여 호환성에 대 한 스키마 확인 됩니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="eff2e-131">스키마 만들어진 후 모델이 변경 된 경우 예외가 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="eff2e-132">**EF Core이 매직이 중 하나를 수행 하지 않습니다.**</span><span class="sxs-lookup"><span data-stu-id="eff2e-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="eff2e-133">코드에서 데이터베이스 연결을 명시적으로 구성 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="eff2e-134">초기화가 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-134">No initialization is performed.</span></span> <span data-ttu-id="eff2e-135">사용 해야 합니다 `DbContext.Database.Migrate()` 마이그레이션을 적용할 (또는 `DbContext.Database.EnsureCreated()` 및 `EnsureDeleted()` 마이그레이션을 사용 하지 않고 데이터베이스 만들기/삭제).</span><span class="sxs-lookup"><span data-stu-id="eff2e-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="eff2e-136">코드 첫 번째 테이블 명명 규칙</span><span class="sxs-lookup"><span data-stu-id="eff2e-136">Code First table naming convention</span></span>

<span data-ttu-id="eff2e-137">EF6은 엔터티에 매핑되는 기본 테이블 이름을 계산 하는 복수 적용 서비스를 통해 엔터티 클래스 이름을 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="eff2e-138">EF Core의 이름을 사용 하 여 `DbSet` 엔터티는 파생 컨텍스트의 제공 되는 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="eff2e-139">엔터티가 없는 경우는 `DbSet` 속성을 클래스 이름이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="eff2e-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
