---
title: 쿼리 유형-EF Core
author: anpete
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/query-types
ms.openlocfilehash: c023d442b0fa2728bd20694a55ebb3a7b5c0efd1
ms.sourcegitcommit: 87e72899d17602f7526d6ccd22f3c8ee844145df
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69628416"
---
# <a name="query-types"></a><span data-ttu-id="96fe0-102">쿼리 형식</span><span class="sxs-lookup"><span data-stu-id="96fe0-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="96fe0-103">이 기능은 EF Core 2.1의 새로운 기능</span><span class="sxs-lookup"><span data-stu-id="96fe0-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="96fe0-104">엔터티 형식 외에도 EF Core 모델을 포함할 수 있습니다 _쿼리 유형_는 엔터티 형식에 매핑되어 있지 않은 데이터에 대 한 데이터베이스 쿼리를 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

## <a name="compare-query-types-to-entity-types"></a><span data-ttu-id="96fe0-105">엔터티 형식에 쿼리 유형 비교</span><span class="sxs-lookup"><span data-stu-id="96fe0-105">Compare query types to entity types</span></span>

<span data-ttu-id="96fe0-106">쿼리 유형은는 엔터티 형식과 마찬가지로 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-106">Query types are like entity types in that they:</span></span>

- <span data-ttu-id="96fe0-107">하거나 모델에 추가할 수 있습니다에 `OnModelCreating` 또는 파생에 "set" 속성을 통해 _DbContext_합니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-107">Can be added to the model either in `OnModelCreating` or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="96fe0-108">여러 상속 매핑 및 탐색 속성과 같이 동일한 매핑 기능을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-108">Support many of the same mapping capabilities, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="96fe0-109">관계형 저장소에서 대상 데이터베이스 개체와 데이터 주석 또는 fluent API 메서드를 통해 열을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-109">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="96fe0-110">그러나 서로 엔터티에서 해당 하는 형식:</span><span class="sxs-lookup"><span data-stu-id="96fe0-110">However, they are different from entity types in that they:</span></span>

- <span data-ttu-id="96fe0-111">키를 정의할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-111">Do not require a key to be defined.</span></span>
- <span data-ttu-id="96fe0-112">변경 내용을 추적 되지 않습니다는 _DbContext_ 및 따라서 되지 삽입, 업데이트 되거나 삭제 된 데이터베이스에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-112">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="96fe0-113">규칙에 따라 검색 되지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="96fe0-114">특히 탐색 매핑-기능의 하위 집합이 지원:</span><span class="sxs-lookup"><span data-stu-id="96fe0-114">Only support a subset of navigation mapping capabilities - Specifically:</span></span>
  - <span data-ttu-id="96fe0-115">관계의 주 끝으로 작동 하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-115">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="96fe0-116">엔터티를 가리키는 참조 탐색 속성에만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-116">They can only contain reference navigation properties pointing to entities.</span></span>
  - <span data-ttu-id="96fe0-117">엔터티 쿼리 형식에 탐색 속성을 포함할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-117">Entities cannot contain navigation properties to query types.</span></span>
- <span data-ttu-id="96fe0-118">해결 합니다 _ModelBuilder_ 사용 하 여 합니다 `Query` 메서드 대신 `Entity` 메서드.</span><span class="sxs-lookup"><span data-stu-id="96fe0-118">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="96fe0-119">에 매핑되는 _DbContext_ 형식의 속성을 통해 `DbQuery<T>` 대신 `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="96fe0-119">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="96fe0-120">사용 하 여 데이터베이스 개체에 매핑되는 `ToView` 메서드를 대신 `ToTable`합니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-120">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="96fe0-121">매핑될 수를 _쿼리 정의_ -쿼리 형식에 대 한 데이터 원본 사용 되는 모델에서 선언 하는 보조 쿼리는 쿼리를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-121">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="96fe0-122">사용 시나리오</span><span class="sxs-lookup"><span data-stu-id="96fe0-122">Usage scenarios</span></span>

<span data-ttu-id="96fe0-123">쿼리 형식에 대 한 주요 사용 시나리오는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-123">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="96fe0-124">에 대 한 반환 형식으로 제공 하는 임시 `FromSql()` 쿼리 합니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-124">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="96fe0-125">데이터베이스 뷰 매핑.</span><span class="sxs-lookup"><span data-stu-id="96fe0-125">Mapping to database views.</span></span>
- <span data-ttu-id="96fe0-126">기본 키가 정의 되지 않은 테이블에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-126">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="96fe0-127">모델에 정의 된 쿼리에 매핑.</span><span class="sxs-lookup"><span data-stu-id="96fe0-127">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="96fe0-128">데이터베이스 개체에 대 한 매핑</span><span class="sxs-lookup"><span data-stu-id="96fe0-128">Mapping to database objects</span></span>

<span data-ttu-id="96fe0-129">사용 하 여 이루어집니다 쿼리 형식 데이터베이스 개체에 매핑하는 `ToView` fluent API.</span><span class="sxs-lookup"><span data-stu-id="96fe0-129">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="96fe0-130">이 메서드에 지정 된 데이터베이스 개체는 EF Core의 관점에서을 _보기_, 읽기 전용 쿼리 원본으로 처리 됩니다 의미 및 없습니다 업데이트의 대상이 될, 삽입 또는 삭제 작업입니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-130">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="96fe0-131">그러나 데이터베이스 개체가 실제로 데이터베이스 뷰를 사용할 필요가-읽기 전용으로 처리 될 데이터베이스 테이블 또는 수는이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-131">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="96fe0-132">반대로 엔터티 형식에 대 한 EF Core는 데이터베이스 개체에서 지정한으로 가정 합니다 `ToTable` 방법으로 처리 될 수는 _테이블_, 즉 쿼리 원본으로 사용할 수 있지만 업데이트도 대상인 삭제 및 삽입 작업입니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-132">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="96fe0-133">데이터베이스 뷰 이름을 지정할 수는 사실 `ToTable` 뷰는 구성 데이터베이스에서 업데이트할 수 있는으로 정상적으로 작동 해야 모든 합니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-133">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="96fe0-134">예제</span><span class="sxs-lookup"><span data-stu-id="96fe0-134">Example</span></span>

<span data-ttu-id="96fe0-135">다음 예제에서는 쿼리 유형을 사용 하 여 데이터베이스 뷰를 쿼리 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-135">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="96fe0-136">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-136">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="96fe0-137">먼저 간단한 블로그 및 게시물 모델을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-137">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="96fe0-138">다음으로 연결 된 각 블로그 게시물의 수를 쿼리할 수 있는 간단한 데이터베이스 뷰를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-138">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#View)]

<span data-ttu-id="96fe0-139">다음으로, 데이터베이스 보기에서 결과 포함 된 클래스를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-139">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="96fe0-140">다음으로 쿼리 형식을 구성 _OnModelCreating_ 사용 하는 `modelBuilder.Query<T>` API.</span><span class="sxs-lookup"><span data-stu-id="96fe0-140">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="96fe0-141">쿼리 형식에 대 한 매핑을 구성 하려면 표준 fluent 구성 Api를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-141">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="96fe0-142">다음으로를 구성 하 `DbContext` 여를 `DbQuery<T>`포함 하도록를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-142">Next, we configure the `DbContext` to include the `DbQuery<T>`:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#DbQuery)]

<span data-ttu-id="96fe0-143">마지막으로 표준 방식으로 데이터베이스 뷰를 쿼리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-143">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="96fe0-144">또한 상황에 맞는 쿼리 속성 (DbQuery)이이 형식에 대 한 쿼리의 루트 역할을 정의한 note 합니다.</span><span class="sxs-lookup"><span data-stu-id="96fe0-144">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
