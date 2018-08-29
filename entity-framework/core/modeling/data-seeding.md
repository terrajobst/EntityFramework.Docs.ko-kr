---
title: 데이터 시드-EF Core
author: AndriySvyryd
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 48ba2389de4b57dbe4c2b2124911c71440d45556
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994481"
---
# <a name="data-seeding"></a><span data-ttu-id="eac36-102">데이터 시드</span><span class="sxs-lookup"><span data-stu-id="eac36-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="eac36-103">이 기능은 EF Core 2.1의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="eac36-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="eac36-104">데이터 시드는 데이터베이스를 채우는 초기 데이터를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="eac36-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="eac36-105">와 달리 ef6에서 EF Core에서 데이터를 시드 연관 된 모델 구성의 일부로 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="eac36-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="eac36-106">그런 다음 EF Core [마이그레이션을](xref:core/managing-schemas/migrations/index) 삽입, 업데이트 또는 삭제할 모델의 새 버전으로 데이터베이스를 업그레이드 하는 경우 적용 해야 하는 작업 항목에 자동으로 계산할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="eac36-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="eac36-107">예를 들어 사용할 수 있습니다에 대 한 시드 데이터를 구성 하는 `Blog` 에서 `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="eac36-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="eac36-108">외래 키 값 관계가 있는 엔터티를 추가 하려면를 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="eac36-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="eac36-109">자주 외래 키 속성을 되므로 섀도 상태의 값을 설정 하는 익명 클래스 수를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="eac36-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="eac36-110">사용 하도록 권장 되는 엔터티를 추가한 후 [마이그레이션을](xref:core/managing-schemas/migrations/index) 변경 내용을 적용 합니다.</span><span class="sxs-lookup"><span data-stu-id="eac36-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="eac36-111">사용할 수 있습니다 `context.Database.EnsureCreated()` 메모리 내 공급자를 사용 하는 경우 또는 예를 들어 테스트 데이터베이스에 대 한 시드 데이터를 포함 하는 새 데이터베이스를 만들려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="eac36-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="eac36-112">해당 경우 데이터베이스가 이미 `EnsureCreated()` 스키마 또는 데이터베이스에 시드 데이터 업데이트 하지 것입니다.</span><span class="sxs-lookup"><span data-stu-id="eac36-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
