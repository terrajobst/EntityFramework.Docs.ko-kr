---
title: 쿼리 유형-EF 코어
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: f16e3a130f3a4f92b2bf6014f2df0ca4eec56a25
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/14/2018
---
# <a name="query-types"></a><span data-ttu-id="daf81-102">쿼리 유형</span><span class="sxs-lookup"><span data-stu-id="daf81-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="daf81-103">이 기능은 EF 코어 2.1의 새로운 기능</span><span class="sxs-lookup"><span data-stu-id="daf81-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="daf81-104">EF 핵심 모델 엔터티 형식 외에도 포함할 수 있습니다 _쿼리 유형에_, 엔터티 형식에 매핑되어 있지 않은 데이터에 대 한 데이터베이스 쿼리를 수행 하는 데 사용입니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

<span data-ttu-id="daf81-105">쿼리 유형을 유사한 점이 엔터티 형식:</span><span class="sxs-lookup"><span data-stu-id="daf81-105">Query types have many similarities with entity types:</span></span>

- <span data-ttu-id="daf81-106">또한 추가할 수 있습니다 모델 중 하나에서 `OnModelCreating`, 또는 파생에 "설정" 속성을 통해 _DbContext_합니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-106">They can also be added to the model either in `OnModelCreating`, or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="daf81-107">이러한 여러 상속 매핑, 탐색 속성 (제한 사항에 아래 참조)와 같은 및 관계형 저장소를 대상 데이터베이스 개체 및 데이터 주석 또는 fluent API 메서드를 통해 열을 구성 하는 기능에서 동일한 매핑 기능을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-107">They support many of the same mapping capabilities, like inheritance mapping, navigation properties (see limitations below) and, on relational stores, the ability to configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="daf81-108">그러나 엔터티에서 다른 지에 입력 하는 것:</span><span class="sxs-lookup"><span data-stu-id="daf81-108">However they are different from entity types in that they:</span></span>

- <span data-ttu-id="daf81-109">키를 정의할 수는 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-109">Do not require a key to be defined.</span></span>
- <span data-ttu-id="daf81-110">변경 내용을 추적 하지 됩니다는 _DbContext_ 및 따라서는 되지 삽입, 업데이트 또는 삭제할 데이터베이스에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-110">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="daf81-111">규칙으로 검색 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-111">Are never discovered by convention.</span></span>
- <span data-ttu-id="daf81-112">특히 탐색 매핑 기능이-의 일부 지원:</span><span class="sxs-lookup"><span data-stu-id="daf81-112">Only support a subset of navigation mapping capabilities - Specifically:</span></span>
  - <span data-ttu-id="daf81-113">관계의 주 끝으로 작동 하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-113">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="daf81-114">엔터티를 가리키는 참조 탐색 속성에만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-114">They can only contain reference navigation properties pointing to entities.</span></span>
  - <span data-ttu-id="daf81-115">엔터티 쿼리 형식에 탐색 속성을 포함할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-115">Entities cannot contain navigation properties to query types.</span></span>
- <span data-ttu-id="daf81-116">으로 해결 된 _ModelBuilder_ 를 사용 하는 `Query` 메서드 보다는 `Entity` 메서드.</span><span class="sxs-lookup"><span data-stu-id="daf81-116">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="daf81-117">에 매핑되는 _DbContext_ 형식의 속성을 통해 `DbQuery<T>` 보다는 `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="daf81-117">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="daf81-118">사용 하 여 데이터베이스 개체에 매핑되는 `ToView` 메서드를 대신 `ToTable`합니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-118">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="daf81-119">에 매핑할 수 있습니다는 _쿼리 정의_ -쿼리 형식에 대 한 데이터 소스 역할을 하는 모델에 선언 된 보조 쿼리는 쿼리를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-119">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

<span data-ttu-id="daf81-120">쿼리 형식에 대 한 주요 사용 시나리오는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-120">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="daf81-121">에 대 한 반환 형식으로 처리 하는 임시 `FromSql()` 쿼리 합니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-121">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="daf81-122">데이터베이스 뷰 매핑입니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-122">Mapping to database views.</span></span>
- <span data-ttu-id="daf81-123">기본 키가 정의 되지 않은 테이블을 매핑하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-123">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="daf81-124">쿼리는 모델에 정의 된 매핑.</span><span class="sxs-lookup"><span data-stu-id="daf81-124">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="daf81-125">사용 하 여 이루어집니다 쿼리 유형을 데이터베이스 개체에 매핑하는 `ToView` fluent API입니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-125">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="daf81-126">이 메서드에 지정 된 데이터베이스 개체는 EF 코어의 관점에서는 _보기_, 읽기 전용 쿼리 원본으로 처리 되는 의미 및 없습니다 update의 대상이 될, 삽입 또는 삭제 작업입니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-126">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="daf81-127">그러나 데이터베이스 개체는 실제로 데이터베이스 뷰-읽기 전용으로 처리 될 데이터베이스 테이블 또는 것이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-127">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="daf81-128">반대로, 엔터티 형식에 대 한 EF 핵심 가정 데이터베이스 개체가 지정 하는 `ToTable` 메서드도 처리 될 수는 _테이블_, 즉 것 쿼리 원본으로 사용할 수 있지만 업데이트의 대상도 삭제 및 삽입 작업입니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-128">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="daf81-129">데이터베이스 뷰 이름을 지정할 수는 실제로 `ToTable` 뷰는 데이터베이스에서 업데이트할 수 있는 되도록 구성 된 상태로 제대로 작동 해야 모든 항목 및 합니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-129">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="daf81-130">예제</span><span class="sxs-lookup"><span data-stu-id="daf81-130">Example</span></span>

<span data-ttu-id="daf81-131">다음 예제에서는 쿼리 유형을 사용 하 여 데이터베이스 뷰를 쿼리 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-131">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="daf81-132">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-132">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="daf81-133">첫째, 간단한 블로그 및 Post 모델 정의:</span><span class="sxs-lookup"><span data-stu-id="daf81-133">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="daf81-134">다음으로 연결 된 각 블로그 게시물의 수를 쿼리할 수 있도록 간단한 데이터베이스 뷰를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-134">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="daf81-135">다음으로, 데이터베이스 보기에서 결과 보관할 클래스를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-135">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="daf81-136">다음으로에서는 구성에서 쿼리 유형을 _OnModelCreating_ 를 사용 하 여는 `modelBuilder.Query<T>` API입니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-136">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="daf81-137">쿼리 형식에 대 한 매핑을 구성 하려면 표준 fluent 구성 Api를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-137">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="daf81-138">마지막으로, 표준 방식으로 데이터베이스 뷰를 쿼리할 수 했습니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-138">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="daf81-139">Note도 컨텍스트 수준 쿼리 속성 (DbQuery)이이 형식에 대 한 쿼리의 루트 역할을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="daf81-139">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
