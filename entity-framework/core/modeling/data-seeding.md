---
title: 데이터 시드-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 5c056c600f696ad1443ddb7b8c95c4b0ead06d21
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414574"
---
# <a name="data-seeding"></a><span data-ttu-id="e52f4-102">데이터 시드</span><span class="sxs-lookup"><span data-stu-id="e52f4-102">Data Seeding</span></span>

<span data-ttu-id="e52f4-103">데이터 시드는 초기 데이터 집합을 사용 하 여 데이터베이스를 채우는 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-103">Data seeding is the process of populating a database with an initial set of data.</span></span>

<span data-ttu-id="e52f4-104">EF Core에서이 작업을 수행할 수 있는 몇 가지 방법이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-104">There are several ways this can be accomplished in EF Core:</span></span>

* <span data-ttu-id="e52f4-105">모델 초기값 데이터</span><span class="sxs-lookup"><span data-stu-id="e52f4-105">Model seed data</span></span>
* <span data-ttu-id="e52f4-106">수동 마이그레이션 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="e52f4-106">Manual migration customization</span></span>
* <span data-ttu-id="e52f4-107">사용자 지정 초기화 논리</span><span class="sxs-lookup"><span data-stu-id="e52f4-107">Custom initialization logic</span></span>

## <a name="model-seed-data"></a><span data-ttu-id="e52f4-108">모델 초기값 데이터</span><span class="sxs-lookup"><span data-stu-id="e52f4-108">Model seed data</span></span>

> [!NOTE]
> <span data-ttu-id="e52f4-109">이 기능은 EF Core 2.1의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-109">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="e52f4-110">EF6와는 달리, EF Core에서 시드 데이터는 모델 구성의 일부로 엔터티 형식에 연결 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-110">Unlike in EF6, in EF Core, seeding data can be associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="e52f4-111">그런 다음 EF Core [마이그레이션은](xref:core/managing-schemas/migrations/index) 데이터베이스를 새 버전의 모델로 업그레이드할 때 적용 해야 하는 삽입, 업데이트 또는 삭제 작업을 자동으로 계산할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-111">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

> [!NOTE]
> <span data-ttu-id="e52f4-112">마이그레이션은 필요한 상태로 시드 데이터를 가져오기 위해 수행 해야 하는 작업을 결정할 때만 모델 변경 사항을 고려 합니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-112">Migrations only considers model changes when determining what operation should be performed to get the seed data into the desired state.</span></span> <span data-ttu-id="e52f4-113">따라서 마이그레이션 외부에서 수행 되는 데이터에 대 한 모든 변경 내용이 손실 되거나 오류가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-113">Thus any changes to the data performed outside of migrations might be lost or cause an error.</span></span>

<span data-ttu-id="e52f4-114">예를 들어 `OnModelCreating`에서 `Blog`에 대 한 초기값 데이터를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-114">As an example, this will configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="e52f4-115">관계가 있는 엔터티를 추가 하려면 외래 키 값을 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-115">To add entities that have a relationship the foreign key values need to be specified:</span></span>

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="e52f4-116">엔터티 형식에 섀도 상태의 속성이 있는 경우 익명 클래스를 사용 하 여 값을 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-116">If the entity type has any properties in shadow state an anonymous class can be used to provide the values:</span></span>

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

<span data-ttu-id="e52f4-117">소유 된 엔터티 형식은 비슷한 방식으로 시드 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-117">Owned entity types can be seeded in a similar fashion:</span></span>

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

<span data-ttu-id="e52f4-118">자세한 컨텍스트는 [전체 샘플 프로젝트](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) 를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="e52f4-118">See the [full sample project](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) for more context.</span></span>

<span data-ttu-id="e52f4-119">모델에 데이터를 추가한 후에는 [마이그레이션을](xref:core/managing-schemas/migrations/index) 사용 하 여 변경 내용을 적용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-119">Once the data has been added to the model, [migrations](xref:core/managing-schemas/migrations/index) should be used to apply the changes.</span></span>

> [!TIP]
> <span data-ttu-id="e52f4-120">자동 배포의 일부로 마이그레이션을 적용 해야 하는 경우 실행 전에 미리 볼 수 있는 [SQL 스크립트를 만들](xref:core/managing-schemas/migrations/index#generate-sql-scripts) 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-120">If you need to apply migrations as part of an automated deployment you can [create a SQL script](xref:core/managing-schemas/migrations/index#generate-sql-scripts) that can be previewed before execution.</span></span>

<span data-ttu-id="e52f4-121">또는 `context.Database.EnsureCreated()`를 사용 하 여 테스트 데이터베이스에 대 한 데이터를 포함 하는 새 데이터베이스를 만들거나 메모리 내 공급자나 관계 없는 데이터베이스를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-121">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider or any non-relation database.</span></span> <span data-ttu-id="e52f4-122">데이터베이스가 이미 있는 경우 `EnsureCreated()`는 데이터베이스의 스키마 및 초기값 데이터를 업데이트 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-122">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor seed data in the database.</span></span> <span data-ttu-id="e52f4-123">관계형 데이터베이스의 경우 마이그레이션을 사용 하려는 경우 `EnsureCreated()`를 호출 하면 안 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-123">For relational databases you shouldn't call `EnsureCreated()` if you plan to use Migrations.</span></span>

### <a name="limitations-of-model-seed-data"></a><span data-ttu-id="e52f4-124">모델 초기값 데이터의 제한 사항</span><span class="sxs-lookup"><span data-stu-id="e52f4-124">Limitations of model seed data</span></span>

<span data-ttu-id="e52f4-125">이러한 유형의 초기값 데이터는 마이그레이션에 의해 관리 되며 데이터베이스에 이미 있는 데이터를 업데이트 하는 스크립트는 데이터베이스에 연결 하지 않고 생성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-125">This type of seed data is managed by migrations and the script to update the data that's already in the database needs to be generated without connecting to the database.</span></span> <span data-ttu-id="e52f4-126">몇 가지 제한 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-126">This imposes some restrictions:</span></span>

* <span data-ttu-id="e52f4-127">기본 키 값은 일반적으로 데이터베이스에서 생성 되는 경우에도 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-127">The primary key value needs to be specified even if it's usually generated by the database.</span></span> <span data-ttu-id="e52f4-128">마이그레이션 간의 데이터 변경 내용을 검색 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-128">It will be used to detect data changes between migrations.</span></span>
* <span data-ttu-id="e52f4-129">기본 키가 변경 되 면 이전에 시드 된 데이터가 제거 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-129">Previously seeded data will be removed if the primary key is changed in any way.</span></span>

<span data-ttu-id="e52f4-130">따라서이 기능은 마이그레이션 외부에서 변경 될 것으로 예상 되지 않고 데이터베이스의 다른 항목 (예: 우편 번호)에 종속 되지 않는 정적 데이터에 가장 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-130">Therefore this feature is most useful for static data that's not expected to change outside of migrations and does not depend on anything else in the database, for example ZIP codes.</span></span>

<span data-ttu-id="e52f4-131">시나리오에 다음 중 하나가 포함 된 경우 마지막 섹션에 설명 된 사용자 지정 초기화 논리를 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-131">If your scenario includes any of the following it is recommended to use custom initialization logic described in the last section:</span></span>

* <span data-ttu-id="e52f4-132">테스트용 임시 데이터</span><span class="sxs-lookup"><span data-stu-id="e52f4-132">Temporary data for testing</span></span>
* <span data-ttu-id="e52f4-133">데이터베이스 상태에 따라 달라 지는 데이터</span><span class="sxs-lookup"><span data-stu-id="e52f4-133">Data that depends on database state</span></span>
* <span data-ttu-id="e52f4-134">대체 키를 id로 사용 하는 엔터티를 포함 하 여 데이터베이스에서 생성 될 키 값이 필요한 데이터입니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-134">Data that needs key values to be generated by the database, including entities that use alternate keys as the identity</span></span>
* <span data-ttu-id="e52f4-135">일부 암호 해시와 같이 사용자 지정 변환이 필요한 데이터 ( [값 변환](xref:core/modeling/value-conversions)에서 처리 되지 않음)</span><span class="sxs-lookup"><span data-stu-id="e52f4-135">Data that requires custom transformation (that is not handled by [value conversions](xref:core/modeling/value-conversions)), such as some password hashing</span></span>
* <span data-ttu-id="e52f4-136">ASP.NET Core Id 역할 및 사용자 만들기와 같은 외부 API를 호출 해야 하는 데이터</span><span class="sxs-lookup"><span data-stu-id="e52f4-136">Data that requires calls to external API, such as ASP.NET Core Identity roles and users creation</span></span>

## <a name="manual-migration-customization"></a><span data-ttu-id="e52f4-137">수동 마이그레이션 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="e52f4-137">Manual migration customization</span></span>

<span data-ttu-id="e52f4-138">마이그레이션을 추가 하면 `HasData`로 지정 된 데이터에 대 한 변경 내용이 `InsertData()`, `UpdateData()`및 `DeleteData()`에 대 한 호출로 변환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-138">When a migration is added the changes to the data specified with `HasData` are transformed to calls to `InsertData()`, `UpdateData()`, and `DeleteData()`.</span></span> <span data-ttu-id="e52f4-139">`HasData`에 대 한 몇 가지 제한 사항을 해결 하는 한 가지 방법은 대신 이러한 호출 또는 [사용자 지정 작업](xref:core/managing-schemas/migrations/operations) 을 마이그레이션에 수동으로 추가 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-139">One way of working around some of the limitations of `HasData` is to manually add these calls or [custom operations](xref:core/managing-schemas/migrations/operations) to the migration instead.</span></span>

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a><span data-ttu-id="e52f4-140">사용자 지정 초기화 논리</span><span class="sxs-lookup"><span data-stu-id="e52f4-140">Custom initialization logic</span></span>

<span data-ttu-id="e52f4-141">간단 하 고 강력한 데이터 시드를 수행 하는 방법은 기본 응용 프로그램 논리 실행을 시작 하기 전에 [`DbContext.SaveChanges()`](xref:core/saving/index) 를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-141">A straightforward and powerful way to perform data seeding is to use [`DbContext.SaveChanges()`](xref:core/saving/index) before the main application logic begins execution.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> <span data-ttu-id="e52f4-142">여러 인스턴스가 실행 중일 때 동시성 문제가 발생 하 고 응용 프로그램에 데이터베이스 스키마를 수정할 수 있는 권한이 있어야 하므로 시드 코드가 일반적인 앱 실행의 일부가 아니어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-142">The seeding code should not be part of the normal app execution as this can cause concurrency issues when multiple instances are running and would also require the app having permission to modify the database schema.</span></span>

<span data-ttu-id="e52f4-143">배포의 제약 조건에 따라 초기화 코드는 여러 가지 방법으로 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-143">Depending on the constraints of your deployment the initialization code can be executed in different ways:</span></span>

* <span data-ttu-id="e52f4-144">로컬로 초기화 앱 실행</span><span class="sxs-lookup"><span data-stu-id="e52f4-144">Running the initialization app locally</span></span>
* <span data-ttu-id="e52f4-145">초기화 루틴을 호출 하 고 초기화 앱을 사용 하지 않도록 설정 하거나 제거 하는 기본 앱을 사용 하 여 초기화 앱을 배포 합니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-145">Deploying the initialization app with the main app, invoking the initialization routine and disabling or removing the initialization app.</span></span>

<span data-ttu-id="e52f4-146">이는 일반적으로 [게시 프로필](/aspnet/core/host-and-deploy/visual-studio-publish-profiles)을 사용 하 여 자동화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e52f4-146">This can usually be automated by using [publish profiles](/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span></span>
