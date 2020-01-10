---
title: 테이블 분할-EF Core
description: Entity Framework Core를 사용 하 여 테이블 분할을 구성 하는 방법
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 01/03/2020
uid: core/modeling/table-splitting
ms.openlocfilehash: c38d3ee0efa82f84a1051017ae40c9f3fdd57f1f
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781172"
---
# <a name="table-splitting"></a><span data-ttu-id="48778-103">테이블 분할</span><span class="sxs-lookup"><span data-stu-id="48778-103">Table Splitting</span></span>

<span data-ttu-id="48778-104">EF Core를 사용 하 여 둘 이상의 엔터티를 단일 행에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="48778-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="48778-105">이를 _테이블 분할_ 또는 _테이블 공유_라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="48778-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="48778-106">구성</span><span class="sxs-lookup"><span data-stu-id="48778-106">Configuration</span></span>

<span data-ttu-id="48778-107">테이블 분할을 사용 하려면 엔터티 형식을 동일한 테이블에 매핑해야 하며, 기본 키가 동일한 열에 매핑되고, 한 엔터티 형식의 기본 키와 동일한 테이블에 있는 하나 이상의 관계가 구성 되어 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="48778-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="48778-108">테이블 분할에 대 한 일반적인 시나리오는 성능 또는 캡슐화를 향상 하기 위해 테이블의 열 하위 집합만 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="48778-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="48778-109">이 예제 `Order`에서는 `DetailedOrder`의 하위 집합을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="48778-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="48778-110">필요한 구성 외에도 `Property(o => o.Status).HasColumnName("Status")`을 호출 하 여 `DetailedOrder.Status`를 `Order.Status`와 동일한 열에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="48778-110">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting)]

> [!TIP]
> <span data-ttu-id="48778-111">자세한 컨텍스트는 [전체 샘플 프로젝트](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) 를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="48778-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="48778-112">용도</span><span class="sxs-lookup"><span data-stu-id="48778-112">Usage</span></span>

<span data-ttu-id="48778-113">테이블 분할을 사용 하 여 엔터티를 저장 하 고 쿼리 하는 작업은 다른 엔터티와 동일한 방식으로 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="48778-113">Saving and querying entities using table splitting is done in the same way as other entities:</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="optional-dependent-entity"></a><span data-ttu-id="48778-114">선택적 종속 엔터티</span><span class="sxs-lookup"><span data-stu-id="48778-114">Optional dependent entity</span></span>

> [!NOTE]
> <span data-ttu-id="48778-115">이 기능은 EF Core 3.0에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="48778-115">This feature was introduced in EF Core 3.0.</span></span>

<span data-ttu-id="48778-116">종속 엔터티에서 사용 하는 모든 열이 데이터베이스에서 `NULL` 되는 경우 쿼리할 때 해당 열에 대 한 인스턴스가 생성 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="48778-116">If all of the columns used by a dependent entity are `NULL` in the database, then no instance for it will be created when queried.</span></span> <span data-ttu-id="48778-117">이렇게 하면 선택적 종속 엔터티를 모델링할 수 있습니다. 여기에서 주 서버에 대 한 관계 속성은 null입니다.</span><span class="sxs-lookup"><span data-stu-id="48778-117">This allows modeling an optional dependent entity, where the relationship property on the principal would be null.</span></span> <span data-ttu-id="48778-118">이 경우 종속 항목의 모든 속성은 선택 사항이 며 `null`로 설정 됩니다 .이는 예상 되지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="48778-118">Note that This would also happen all of the dependent's properties are optional and set to `null`, which might not be expected.</span></span>

## <a name="concurrency-tokens"></a><span data-ttu-id="48778-119">동시성 토큰</span><span class="sxs-lookup"><span data-stu-id="48778-119">Concurrency tokens</span></span>

<span data-ttu-id="48778-120">테이블을 공유 하는 엔터티 형식에 동시성 토큰이 있는 경우 다른 모든 엔터티 형식에도 포함 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="48778-120">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types as well.</span></span> <span data-ttu-id="48778-121">이는 동일한 테이블에 매핑된 엔터티 중 하나만 업데이트 될 때 부실 동시성 토큰 값을 방지 하기 위해 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="48778-121">This is necessary in order to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="48778-122">동시성 토큰을 사용 하는 코드에 노출 하지 않으려면 하나를 [섀도 속성](xref:core/modeling/shadow-properties)으로 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="48778-122">To avoid exposing the concurrency token to the consuming code, it's possible the create one as a [shadow property](xref:core/modeling/shadow-properties):</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
