---
title: 쿼리 유형-EF Core
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 5a2cd451da8833daf2c315419559eb4a2c705b13
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39388483"
---
# <a name="query-types"></a><span data-ttu-id="4003a-102">쿼리 형식</span><span class="sxs-lookup"><span data-stu-id="4003a-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="4003a-103">이 기능은 EF Core 2.1의 새로운 기능</span><span class="sxs-lookup"><span data-stu-id="4003a-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="4003a-104">엔터티 형식 외에도 EF Core 모델을 포함할 수 있습니다 _쿼리 유형_는 엔터티 형식에 매핑되어 있지 않은 데이터에 대 한 데이터베이스 쿼리를 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

<span data-ttu-id="4003a-105">쿼리 형식에 많은 유사점이 엔터티 유형</span><span class="sxs-lookup"><span data-stu-id="4003a-105">Query types have many similarities with entity types:</span></span>

- <span data-ttu-id="4003a-106">또한 추가할 수 있습니다 모델 중 하나에 `OnModelCreating`, 또는 파생에 "set" 속성을 통해 _DbContext_합니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-106">They can also be added to the model either in `OnModelCreating`, or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="4003a-107">다양 한 동일한 매핑 기능을 상속 매핑, 탐색 속성 (아래의 제한 사항 참조)와 같은 한, 관계형 저장소를 대상 데이터베이스 개체 및 데이터 주석 또는 fluent API 메서드를 통해 열을 구성 하는 기능을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-107">They support many of the same mapping capabilities, like inheritance mapping, navigation properties (see limitations below) and, on relational stores, the ability to configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="4003a-108">그러나 엔터티에서 다른 형식으로:</span><span class="sxs-lookup"><span data-stu-id="4003a-108">However they are different from entity types in that they:</span></span>

- <span data-ttu-id="4003a-109">키를 정의할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-109">Do not require a key to be defined.</span></span>
- <span data-ttu-id="4003a-110">변경 내용을 추적 되지 않습니다는 _DbContext_ 및 따라서 되지 삽입, 업데이트 되거나 삭제 된 데이터베이스에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-110">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="4003a-111">규칙에 따라 검색 되지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-111">Are never discovered by convention.</span></span>
- <span data-ttu-id="4003a-112">특히 탐색 매핑-기능의 하위 집합이 지원:</span><span class="sxs-lookup"><span data-stu-id="4003a-112">Only support a subset of navigation mapping capabilities - Specifically:</span></span>
  - <span data-ttu-id="4003a-113">관계의 주 끝으로 작동 하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-113">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="4003a-114">엔터티를 가리키는 참조 탐색 속성에만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-114">They can only contain reference navigation properties pointing to entities.</span></span>
  - <span data-ttu-id="4003a-115">엔터티 쿼리 형식에 탐색 속성을 포함할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-115">Entities cannot contain navigation properties to query types.</span></span>
- <span data-ttu-id="4003a-116">해결 합니다 _ModelBuilder_ 사용 하 여 합니다 `Query` 메서드 대신 `Entity` 메서드.</span><span class="sxs-lookup"><span data-stu-id="4003a-116">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="4003a-117">에 매핑되는 _DbContext_ 형식의 속성을 통해 `DbQuery<T>` 대신 `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="4003a-117">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="4003a-118">사용 하 여 데이터베이스 개체에 매핑되는 `ToView` 메서드를 대신 `ToTable`합니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-118">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="4003a-119">매핑될 수를 _쿼리 정의_ -쿼리 형식에 대 한 데이터 원본 사용 되는 모델에서 선언 하는 보조 쿼리는 쿼리를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-119">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

<span data-ttu-id="4003a-120">쿼리 형식에 대 한 주요 사용 시나리오는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-120">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="4003a-121">에 대 한 반환 형식으로 제공 하는 임시 `FromSql()` 쿼리 합니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-121">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="4003a-122">데이터베이스 뷰 매핑.</span><span class="sxs-lookup"><span data-stu-id="4003a-122">Mapping to database views.</span></span>
- <span data-ttu-id="4003a-123">기본 키가 정의 되지 않은 테이블에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-123">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="4003a-124">모델에 정의 된 쿼리에 매핑.</span><span class="sxs-lookup"><span data-stu-id="4003a-124">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="4003a-125">사용 하 여 이루어집니다 쿼리 형식 데이터베이스 개체에 매핑하는 `ToView` fluent API.</span><span class="sxs-lookup"><span data-stu-id="4003a-125">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="4003a-126">이 메서드에 지정 된 데이터베이스 개체는 EF Core의 관점에서을 _보기_, 읽기 전용 쿼리 원본으로 처리 됩니다 의미 및 없습니다 업데이트의 대상이 될, 삽입 또는 삭제 작업입니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-126">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="4003a-127">그러나 데이터베이스 개체가 실제로 데이터베이스 뷰를 사용할 필요가-읽기 전용으로 처리 될 데이터베이스 테이블 또는 수는이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-127">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="4003a-128">반대로 엔터티 형식에 대 한 EF Core는 데이터베이스 개체에서 지정한으로 가정 합니다 `ToTable` 방법으로 처리 될 수는 _테이블_, 즉 쿼리 원본으로 사용할 수 있지만 업데이트도 대상인 삭제 및 삽입 작업입니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-128">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="4003a-129">데이터베이스 뷰 이름을 지정할 수는 사실 `ToTable` 뷰는 구성 데이터베이스에서 업데이트할 수 있는으로 정상적으로 작동 해야 모든 합니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-129">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="4003a-130">예</span><span class="sxs-lookup"><span data-stu-id="4003a-130">Example</span></span>

<span data-ttu-id="4003a-131">다음 예제에서는 쿼리 유형을 사용 하 여 데이터베이스 뷰를 쿼리 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-131">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="4003a-132">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryTypes)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-132">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="4003a-133">먼저 간단한 블로그 및 게시물 모델을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-133">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="4003a-134">다음으로 연결 된 각 블로그 게시물의 수를 쿼리할 수 있는 간단한 데이터베이스 뷰를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-134">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="4003a-135">다음으로, 데이터베이스 보기에서 결과 포함 된 클래스를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-135">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="4003a-136">다음으로 쿼리 형식을 구성 _OnModelCreating_ 사용 하는 `modelBuilder.Query<T>` API.</span><span class="sxs-lookup"><span data-stu-id="4003a-136">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="4003a-137">쿼리 형식에 대 한 매핑을 구성 하려면 표준 fluent 구성 Api를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-137">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="4003a-138">마지막으로 표준 방식으로 데이터베이스 뷰를 쿼리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-138">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="4003a-139">또한 상황에 맞는 쿼리 속성 (DbQuery)이이 형식에 대 한 쿼리의 루트 역할을 정의한 note 합니다.</span><span class="sxs-lookup"><span data-stu-id="4003a-139">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
