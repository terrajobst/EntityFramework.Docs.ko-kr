---
title: 테이블 분할-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 4a0bfaf017106a0bfdff084b1c472bdc17459a89
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562593"
---
# <a name="table-splitting"></a><span data-ttu-id="0d0e2-102">테이블 분</span><span class="sxs-lookup"><span data-stu-id="0d0e2-102">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="0d0e2-103">이 기능은 EF Core 2.0의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="0d0e2-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="0d0e2-104">EF Core는 단일 행에 두 개 이상의 엔터티를 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0d0e2-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="0d0e2-105">이 이라고 _분할 테이블_ 하거나 _테이블 공유_합니다.</span><span class="sxs-lookup"><span data-stu-id="0d0e2-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="0d0e2-106">구성</span><span class="sxs-lookup"><span data-stu-id="0d0e2-106">Configuration</span></span>

<span data-ttu-id="0d0e2-107">테이블 엔터티 유형을 동일한 테이블에 매핑할 수 해야 분할을 사용 하려면 동일한 열에 매핑된 기본 키 및 하나 이상의 관계를 엔터티 형식의 기본 키와 동일한 테이블의 다른 사이 구성 된 경우</span><span class="sxs-lookup"><span data-stu-id="0d0e2-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="0d0e2-108">테이블 분할에 대 한 일반적인 시나리오에서에서 사용 하는 열의 하위 집합만 테이블 성능 크거나 캡슐화 합니다.</span><span class="sxs-lookup"><span data-stu-id="0d0e2-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="0d0e2-109">이 예제의 `Order` 의 하위 집합을 나타내는 `DetailedOrder`합니다.</span><span class="sxs-lookup"><span data-stu-id="0d0e2-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="0d0e2-110">필수 구성 외에도 호출 `HasBaseType((string)null)` 매핑을 사용 하지 않으려면 `DetailedOrder` 와 동일한 계층에 `Order`.</span><span class="sxs-lookup"><span data-stu-id="0d0e2-110">In addition to the required configuration we call `HasBaseType((string)null)` to avoid mapping `DetailedOrder` in the same hierarchy as `Order`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

<span data-ttu-id="0d0e2-111">참조 된 [전체 샘플 프로젝트](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) 자세한 컨텍스트.</span><span class="sxs-lookup"><span data-stu-id="0d0e2-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="0d0e2-112">사용법</span><span class="sxs-lookup"><span data-stu-id="0d0e2-112">Usage</span></span>

<span data-ttu-id="0d0e2-113">저장 하 고 테이블 분할을 사용 하 여 엔터티 쿼리 다른 엔터티는 동일한 방식으로 수행 됩니다, 그리고 유일한 차이점은 삽입에 대 한 행을 공유 하는 모든 엔터티를 추적할 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0d0e2-113">Saving and querying entities using table splitting is done in the same way as other entities, the only difference is that all entities sharing a row must be tracked for the insert.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="0d0e2-114">동시성 토큰</span><span class="sxs-lookup"><span data-stu-id="0d0e2-114">Concurrency tokens</span></span>

<span data-ttu-id="0d0e2-115">테이블 공유 엔터티 형식의 동시성 토큰 있으면 다음이 포함 되어야 합니다는 같은 테이블에 매핑된 엔터티 중 하나에이 업데이트 될 때 오래 된 동시성 토큰 값을 방지 하려면 다른 모든 엔터티 형식에.</span><span class="sxs-lookup"><span data-stu-id="0d0e2-115">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="0d0e2-116">사용 하는 코드에 노출 하지 않는 것이 가능한 섀도 상태의 하나 만드세요.</span><span class="sxs-lookup"><span data-stu-id="0d0e2-116">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]