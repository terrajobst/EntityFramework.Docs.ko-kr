---
title: 데이터 시드-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 791f7afff36aac52fe2ffdc16ab580db22011b99
ms.sourcegitcommit: 082946dcaa1ee5174e692dbfe53adeed40609c6a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51028098"
---
# <a name="data-seeding"></a><span data-ttu-id="2ca1f-102">데이터 시드</span><span class="sxs-lookup"><span data-stu-id="2ca1f-102">Data Seeding</span></span>

<span data-ttu-id="2ca1f-103">데이터 시드는 초기 데이터 집합을 사용 하 여 데이터베이스를 채우는 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-103">Data seeding is the process of populating a database with an initial set of data.</span></span>

<span data-ttu-id="2ca1f-104">여러 가지 방법으로 EF Core에서이 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-104">There are several ways this can be accomplished in EF Core:</span></span>
* <span data-ttu-id="2ca1f-105">모델 시드 데이터</span><span class="sxs-lookup"><span data-stu-id="2ca1f-105">Model seed data</span></span>
* <span data-ttu-id="2ca1f-106">수동 마이그레이션 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="2ca1f-106">Manual migration customization</span></span>
* <span data-ttu-id="2ca1f-107">사용자 지정 초기화 논리</span><span class="sxs-lookup"><span data-stu-id="2ca1f-107">Custom initialization logic</span></span>

## <a name="model-seed-data"></a><span data-ttu-id="2ca1f-108">모델 시드 데이터</span><span class="sxs-lookup"><span data-stu-id="2ca1f-108">Model seed data</span></span>

> [!NOTE]
> <span data-ttu-id="2ca1f-109">이 기능은 EF Core 2.1의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-109">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="2ca1f-110">달리 ef6에서 EF Core에서 데이터 시드 수 모델 구성의 일부로 엔터티 형식과 사용 하 여 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-110">Unlike in EF6, in EF Core, seeding data can be associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="2ca1f-111">그런 다음 EF Core [마이그레이션을](xref:core/managing-schemas/migrations/index) 삽입, 업데이트 또는 삭제할 모델의 새 버전으로 데이터베이스를 업그레이드 하는 경우 적용 해야 하는 작업 항목에 자동으로 계산할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-111">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

> [!NOTE]
> <span data-ttu-id="2ca1f-112">마이그레이션은 원하는 상태에 시드 데이터를 가져올 수 있는 작업을 수행 하도록 결정 하는 경우 모델 변경 내용을 고려 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-112">Migrations only considers model changes when determining what operation should be performed to get the seed data into the desired state.</span></span> <span data-ttu-id="2ca1f-113">따라서 마이그레이션 외부에서 수행 하는 데이터 변경 사항이 있으면 손실 하거나 오류가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-113">Thus any changes to the data performed outside of migrations might be lost or cause an error.</span></span>

<span data-ttu-id="2ca1f-114">예를 들어에 대 한 시드 데이터를 구성 합니다이 `Blog` 에서 `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="2ca1f-114">As an example, this will configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="2ca1f-115">외래 키 값 관계가 있는 엔터티를 추가 하려면 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-115">To add entities that have a relationship the foreign key values need to be specified:</span></span>

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="2ca1f-116">엔터티 형식에 있는 경우 모든 속성 섀도 상태 값을 제공 하는 익명 클래스를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-116">If the entity type has any properties in shadow state an anonymous class can be used to provide the values:</span></span>

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

<span data-ttu-id="2ca1f-117">소유 된 엔터티 형식이 비슷한 방식으로 시드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-117">Owned entity types can be seeded in a similar fashion:</span></span>

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

<span data-ttu-id="2ca1f-118">참조 된 [전체 샘플 프로젝트](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) 자세한 컨텍스트.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-118">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) for more context.</span></span>

<span data-ttu-id="2ca1f-119">데이터 모델에 추가 되 면 [마이그레이션을](xref:core/managing-schemas/migrations/index) 변경 내용을 적용 하려면 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-119">Once the data has been added to the model, [migrations](xref:core/managing-schemas/migrations/index) should be used to apply the changes.</span></span>

> [!TIP]
> <span data-ttu-id="2ca1f-120">자동화 된 배포의 일환으로 마이그레이션을 적용 해야 할 수 있습니다 [SQL 스크립트를 만듭니다](xref:core/managing-schemas/migrations/index#generate-sql-scripts) 실행 하기 전에 미리 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-120">If you need to apply migrations as part of an automated deployment you can [create a SQL script](xref:core/managing-schemas/migrations/index#generate-sql-scripts) that can be previewed before execution.</span></span>

<span data-ttu-id="2ca1f-121">사용할 수 있습니다 `context.Database.EnsureCreated()` 메모리 내 공급자 또는 비-관계 데이터베이스를 사용 하는 경우 또는 예를 들어 테스트 데이터베이스에 대 한 시드 데이터를 포함 하는 새 데이터베이스를 만들려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-121">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider or any non-relation database.</span></span> <span data-ttu-id="2ca1f-122">해당 경우 데이터베이스가 이미 `EnsureCreated()` 스키마 또는 데이터베이스에 시드 데이터 업데이트 하지 것입니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-122">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span> <span data-ttu-id="2ca1f-123">관계형 데이터베이스에 대 한 호출 하지 않아야 하면 `EnsureCreated()` 마이그레이션을 사용 하려는 경우.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-123">For relational databases you shouldn't call `EnsureCreated()` if you plan to use Migrations.</span></span>

<span data-ttu-id="2ca1f-124">데이터베이스에 연결 하지 않고도 생성 해야 하는 데이터베이스에 이미 있는 데이터를 업데이트 하는 스크립트와 이러한 종류의 시드 데이터 마이그레이션을 하 여 관리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-124">This type of seed data is managed by migrations and the script to update the data that's already in the database needs to be generated without connecting to the database.</span></span> <span data-ttu-id="2ca1f-125">이 인해 몇 가지 제한 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-125">This imposes some restrictions:</span></span>
* <span data-ttu-id="2ca1f-126">기본 키 값을 데이터베이스에서 일반적으로 생성 되는 경우에 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-126">The primary key value needs to be specified even if it's usually generated by the database.</span></span> <span data-ttu-id="2ca1f-127">마이그레이션 간에 데이터 변경 내용을 검색 하려면 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-127">It will be used to detect data changes between migrations.</span></span>
* <span data-ttu-id="2ca1f-128">이전에 시드 된 데이터를 어떤 방식으로 기본 키 변경 되 면 제거 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-128">Previously seeded data will be removed if the primary key is changed in any way.</span></span>

<span data-ttu-id="2ca1f-129">따라서이 기능은 정적 데이터 마이그레이션을 외부 변경 되지는 않을 종속 되지 않는 데이터베이스, 예를 들어 우편 번호의에서 다른 부분에 대 한 가장 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-129">Therefore this feature is most useful for static data that's not expected to change outside of migrations and does not depend on anything else in the database, for example ZIP codes.</span></span>

<span data-ttu-id="2ca1f-130">시나리오 중 하나를 포함 하는 경우 마지막 섹션에서 설명 하는 사용자 지정 초기화 논리를 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-130">If your scenario includes any of the following it is recommended to use custom initialization logic described in the last section:</span></span>
* <span data-ttu-id="2ca1f-131">테스트에 대 한 임시 데이터</span><span class="sxs-lookup"><span data-stu-id="2ca1f-131">Temporary data for testing</span></span>
* <span data-ttu-id="2ca1f-132">데이터베이스 상태에 따라 데이터를</span><span class="sxs-lookup"><span data-stu-id="2ca1f-132">Data that depends on database state</span></span>
* <span data-ttu-id="2ca1f-133">키 값을 id로 대체 키를 사용 하는 엔터티를 포함 하 여 데이터베이스에서 생성할 수는 데이터</span><span class="sxs-lookup"><span data-stu-id="2ca1f-133">Data that needs key values to be generated by the database, including entities that use alternate keys as the identity</span></span>
* <span data-ttu-id="2ca1f-134">사용자 지정 변환 해야 하는 데이터 (에서 처리 되지 않은 [변환 값](xref:core/modeling/value-conversions)), 같은 일부 암호 해시</span><span class="sxs-lookup"><span data-stu-id="2ca1f-134">Data that requires custom transformation (that is not handled by [value conversions](xref:core/modeling/value-conversions)), such as some password hashing</span></span>
* <span data-ttu-id="2ca1f-135">ASP.NET Core Id 역할 및 사용자 만들기와 같은 외부 API에 대 한 호출을 필요로 하는 데이터</span><span class="sxs-lookup"><span data-stu-id="2ca1f-135">Data that requires calls to external API, such as ASP.NET Core Identity roles and users creation</span></span>

## <a name="manual-migration-customization"></a><span data-ttu-id="2ca1f-136">수동 마이그레이션 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="2ca1f-136">Manual migration customization</span></span>

<span data-ttu-id="2ca1f-137">마이그레이션을 사용 하 여 지정 된 데이터는 변경 내용이 추가 될 때 `HasData` 에 대 한 호출으로 변환 됩니다 `InsertData()`하십시오 `UpdateData()`, 및 `DeleteData()`합니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-137">When a migration is added the changes to the data specified with `HasData` are transformed to calls to `InsertData()`, `UpdateData()`, and `DeleteData()`.</span></span> <span data-ttu-id="2ca1f-138">제한 중 일부를 해결 하기는 한 가지 방법은 `HasData` 수동으로 이러한 호출을 추가 하거나 [사용자 지정 작업](xref:core/managing-schemas/migrations/operations) 마이그레이션을 위해 대신 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-138">One way of working around some of the limitations of `HasData` is to manually add these calls or [custom operations](xref:core/managing-schemas/migrations/operations) to the migration instead.</span></span>

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a><span data-ttu-id="2ca1f-139">사용자 지정 초기화 논리</span><span class="sxs-lookup"><span data-stu-id="2ca1f-139">Custom initialization logic</span></span>

<span data-ttu-id="2ca1f-140">데이터 시드 작업을 수행 하는 간단 하 고 강력한 방법을 사용 하는 것 [ `DbContext.SaveChanges()` ](xref:core/saving/index) 논리를 주 응용 프로그램 전에 실행을 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-140">A straightforward and powerful way to perform data seeding is to use [`DbContext.SaveChanges()`](xref:core/saving/index) before the main application logic begins execution.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> <span data-ttu-id="2ca1f-141">시드 코드가 여러 인스턴스가 실행 되 고 데이터베이스 스키마를 수정할 수 있는 권한을 가진 앱도 해야 하는 경우 동시성 문제를 발생할 수 있습니다이 일반 앱 실행에 포함 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-141">The seeding code should not be part of the normal app execution as this can cause concurrency issues when multiple instances are running and would also require the app having permission to modify the database schema.</span></span>

<span data-ttu-id="2ca1f-142">배포의 제약 조건에 따라 다른 방법으로 초기화 코드를 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-142">Depending on the constraints of your deployment the initialization code can be executed in different ways:</span></span>
* <span data-ttu-id="2ca1f-143">초기화 앱을 로컬로 실행</span><span class="sxs-lookup"><span data-stu-id="2ca1f-143">Running the initialization app locally</span></span>
* <span data-ttu-id="2ca1f-144">기본 앱 초기화 루틴을 호출 하 고 사용 하지 않도록 설정 또는 초기화 앱 제거를 사용 하 여 초기화 앱을 배포 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-144">Deploying the initialization app with the main app, invoking the initialization routine and disabling or removing the initialization app.</span></span>

<span data-ttu-id="2ca1f-145">일반적으로 사용 하 여 자동화할 수 있습니다 [게시 프로필](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles)합니다.</span><span class="sxs-lookup"><span data-stu-id="2ca1f-145">This can usually be automated by using [publish profiles](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span></span>
