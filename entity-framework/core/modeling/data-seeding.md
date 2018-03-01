---
title: "데이터 시드는-EF 코어"
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 693ffe44e247a79e01ac7c98a36472bf2c68d37f
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="data-seeding"></a><span data-ttu-id="b1063-102">데이터 시드</span><span class="sxs-lookup"><span data-stu-id="b1063-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="b1063-103">이 기능은 EF 코어 2.1의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="b1063-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="b1063-104">데이터 시드는 데이터베이스를 채우는 초기 데이터를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b1063-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="b1063-105">와 달리 EF6 EF 코어에서의 데이터 시드 연관 된 모델 구성의 일부로 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="b1063-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="b1063-106">그런 다음 EF 코어 마이그레이션 삽입, 업데이트 또는 삭제 작업을 데이터베이스 모델의 새 버전으로 업그레이드 하는 경우 적용할 필요가 내용에 자동으로 계산할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b1063-106">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="b1063-107">예를 들어, 사용할 수 있습니다에 대 한 데이터를 구성 하는 `Blog` 에 `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="b1063-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="b1063-108">외래 키 값 관계가 있는 엔터티를 추가 하려면 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b1063-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="b1063-109">자주 외래 키 속성은 그림자 상태 이므로 익명 클래스 값을 설정 하려면 사용할 수 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b1063-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
