---
title: 테이블 분할-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: a3a2e5842a6c6b4b490084d205a0d44bb46c17ee
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656035"
---
# <a name="table-splitting"></a><span data-ttu-id="45263-102">테이블 분할</span><span class="sxs-lookup"><span data-stu-id="45263-102">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="45263-103">이 기능은 EF Core 2.0의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="45263-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="45263-104">EF Core를 사용 하 여 둘 이상의 엔터티를 단일 행에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="45263-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="45263-105">이를 _테이블 분할_ 또는 _테이블 공유_라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="45263-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="45263-106">Configuration</span><span class="sxs-lookup"><span data-stu-id="45263-106">Configuration</span></span>

<span data-ttu-id="45263-107">테이블 분할을 사용 하려면 엔터티 형식을 동일한 테이블에 매핑해야 하며, 기본 키가 동일한 열에 매핑되고, 한 엔터티 형식의 기본 키와 동일한 테이블에 있는 하나 이상의 관계가 구성 되어 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="45263-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="45263-108">테이블 분할에 대 한 일반적인 시나리오는 성능 또는 캡슐화를 향상 하기 위해 테이블의 열 하위 집합만 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="45263-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="45263-109">이 예제 `Order`에서는 `DetailedOrder`의 하위 집합을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="45263-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="45263-110">필요한 구성 외에도 `Property(o => o.Status).HasColumnName("Status")`을 호출 하 여 `DetailedOrder.Status`를 `Order.Status`와 동일한 열에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="45263-110">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> <span data-ttu-id="45263-111">자세한 컨텍스트는 [전체 샘플 프로젝트](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) 를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="45263-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="45263-112">사용 현황</span><span class="sxs-lookup"><span data-stu-id="45263-112">Usage</span></span>

<span data-ttu-id="45263-113">테이블 분할을 사용 하 여 엔터티를 저장 하 고 쿼리 하는 작업은 다른 엔터티와 동일한 방식으로 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="45263-113">Saving and querying entities using table splitting is done in the same way as other entities.</span></span> <span data-ttu-id="45263-114">EF Core 3.0부터 종속 엔터티 참조를 `null`수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="45263-114">And starting with EF Core 3.0 the dependent entity reference can be `null`.</span></span> <span data-ttu-id="45263-115">종속 엔터티에서 사용 하는 모든 열이 데이터베이스 `NULL` 이면 쿼리할 때 생성 되는 인스턴스가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="45263-115">If all of the columns used by the dependent entity are `NULL` is the database then no instance for it will be created when queried.</span></span> <span data-ttu-id="45263-116">이는 또한 모든 속성이 선택 사항이 며 `null`로 설정 되어 예상 되지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="45263-116">This would also happen all of the properties are optional and set to `null`, which might not be expected.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="45263-117">동시성 토큰</span><span class="sxs-lookup"><span data-stu-id="45263-117">Concurrency tokens</span></span>

<span data-ttu-id="45263-118">테이블을 공유 하는 엔터티 형식에 동시성 토큰이 있는 경우 동일한 테이블에 매핑된 엔터티 중 하나만 업데이트 되 면 부실 동시성 토큰 값을 방지 하기 위해 다른 모든 엔터티 형식에 포함 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="45263-118">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="45263-119">소비 하는 코드에 노출 되지 않도록 하려면 섀도 상태에서 하나를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="45263-119">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
