---
title: 키가 없는 엔터티 형식-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 3dbc2700fc9bb277eb90885dfc2506c250ae21f1
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445937"
---
# <a name="keyless-entity-types"></a><span data-ttu-id="07a7c-102">키 없는 엔터티 형식</span><span class="sxs-lookup"><span data-stu-id="07a7c-102">Keyless Entity Types</span></span>

> [!NOTE]
> <span data-ttu-id="07a7c-103">이 기능은 쿼리 유형 이름 아래 EF Core 2.1에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-103">This feature was added in EF Core 2.1 under the name of query types.</span></span> <span data-ttu-id="07a7c-104">EF Core 3.0에서 개념은 키가 없는 엔터티 형식으로 이름이 변경 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-104">In EF Core 3.0 the concept was renamed to keyless entity types.</span></span>

<span data-ttu-id="07a7c-105">일반 엔터티 형식 외에도 EF Core 모델에는 키 값이 포함 되지 않은 데이터에 대 한 데이터베이스 쿼리를 수행 하는 데 사용할 수 있는 _키가 없는 엔터티 형식이_포함 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-105">In addition to regular entity types, an EF Core model can contain _keyless entity types_, which can be used to carry out database queries against data that doesn't contain key values.</span></span>

## <a name="keyless-entity-types-characteristics"></a><span data-ttu-id="07a7c-106">키가 없는 엔터티 형식 특징</span><span class="sxs-lookup"><span data-stu-id="07a7c-106">Keyless entity types characteristics</span></span>

<span data-ttu-id="07a7c-107">키가 없는 엔터티 형식은 상속 매핑 및 탐색 속성과 같은 일반적인 엔터티 형식과 동일한 매핑 기능을 대부분 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-107">Keyless entity types support many of the same mapping capabilities as regular entity types, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="07a7c-108">관계형 저장소에서 흐름 API 메서드 또는 데이터 주석을 통해 대상 데이터베이스 개체 및 열을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-108">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="07a7c-109">그러나 다음과 같은 점에서 일반 엔터티 형식과는 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-109">However, they are different from regular entity types in that they:</span></span>

- <span data-ttu-id="07a7c-110">키를 정의할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-110">Cannot have a key defined.</span></span>
- <span data-ttu-id="07a7c-111">는 _DbContext_ 의 변경 내용에 대해서는 추적 되지 않으므로 데이터베이스에 삽입, 업데이트 또는 삭제 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-111">Are never tracked for changes in the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="07a7c-112">규칙에 따라 검색 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-112">Are never discovered by convention.</span></span>
- <span data-ttu-id="07a7c-113">는 다음과 같이 탐색 매핑 기능의 하위 집합만 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-113">Only support a subset of navigation mapping capabilities, specifically:</span></span>
  - <span data-ttu-id="07a7c-114">관계의 주 끝으로 작동 하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-114">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="07a7c-115">소유 된 엔터티를 탐색 하지 못할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-115">They may not have navigations to owned entities</span></span>
  - <span data-ttu-id="07a7c-116">일반 엔터티를 가리키는 참조 탐색 속성만 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-116">They can only contain reference navigation properties pointing to regular entities.</span></span>
  - <span data-ttu-id="07a7c-117">엔터티는 키가 없는 엔터티 형식에 대 한 탐색 속성을 포함할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-117">Entities cannot contain navigation properties to keyless entity types.</span></span>
- <span data-ttu-id="07a7c-118">@No__t-0 메서드 호출로 구성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-118">Need to be configured with `.HasNoKey()` method call.</span></span>
- <span data-ttu-id="07a7c-119">_정의 쿼리에_매핑될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-119">May be mapped to a _defining query_.</span></span> <span data-ttu-id="07a7c-120">정의 쿼리는 키가 없는 엔터티 형식에 대 한 데이터 소스 역할을 하는 모델에 선언 된 쿼리입니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-120">A defining query is a query declared in the model that acts as a data source for a keyless entity type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="07a7c-121">사용 시나리오</span><span class="sxs-lookup"><span data-stu-id="07a7c-121">Usage scenarios</span></span>

<span data-ttu-id="07a7c-122">키가 없는 엔터티 형식에 대 한 몇 가지 주요 사용 시나리오는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-122">Some of the main usage scenarios for keyless entity types are:</span></span>

- <span data-ttu-id="07a7c-123">[원시 SQL 쿼리의](xref:core/querying/raw-sql)반환 형식으로 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-123">Serving as the return type for [raw SQL queries](xref:core/querying/raw-sql).</span></span>
- <span data-ttu-id="07a7c-124">기본 키가 포함 되지 않은 데이터베이스 뷰에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-124">Mapping to database views that do not contain a primary key.</span></span>
- <span data-ttu-id="07a7c-125">기본 키가 정의 되어 있지 않은 테이블에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-125">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="07a7c-126">모델에 정의 된 쿼리에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-126">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="07a7c-127">데이터베이스 개체에 매핑</span><span class="sxs-lookup"><span data-stu-id="07a7c-127">Mapping to database objects</span></span>

<span data-ttu-id="07a7c-128">@No__t-0 또는 `ToView` 흐름 API를 사용 하 여 키가 없는 엔터티 형식을 데이터베이스 개체에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-128">Mapping a keyless entity type to a database object is achieved using the `ToTable` or `ToView` fluent API.</span></span> <span data-ttu-id="07a7c-129">EF Core 관점에서이 메서드에 지정 된 데이터베이스 개체는 _뷰가_읽기 전용 쿼리 원본으로 처리 되 고 update, insert 또는 delete 작업의 대상일 수 없음을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-129">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="07a7c-130">그러나 데이터베이스 개체가 실제로 데이터베이스 뷰가 되어야 한다는 의미는 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-130">However, this does not mean that the database object is actually required to be a database view.</span></span> <span data-ttu-id="07a7c-131">또는 읽기 전용으로 처리 되는 데이터베이스 테이블 일 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-131">It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="07a7c-132">반대로, 일반 엔터티 형식의 경우 EF Core는 `ToTable` 메서드에 지정 된 데이터베이스 개체를 _테이블로_처리할 수 있다고 가정 합니다. 즉, 쿼리 원본으로 사용할 수 있지만 업데이트, 삭제 및 삽입 작업의 대상으로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-132">Conversely, for regular entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="07a7c-133">실제로는 `ToTable`에서 데이터베이스 뷰의 이름을 지정할 수 있으며, 데이터베이스에서 뷰를 업데이트할 수 있도록 구성 하는 한 모든 것이 제대로 작동 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-133">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

> [!NOTE]
> <span data-ttu-id="07a7c-134">`ToView`은 개체가 데이터베이스에 이미 존재 하 고 마이그레이션에 의해 생성 되지 않는다고 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-134">`ToView` assumes that the object already exists in the database and it won't be created by migrations.</span></span>

## <a name="example"></a><span data-ttu-id="07a7c-135">예제</span><span class="sxs-lookup"><span data-stu-id="07a7c-135">Example</span></span>

<span data-ttu-id="07a7c-136">다음 예제에서는 키가 없는 엔터티 형식을 사용 하 여 데이터베이스 뷰를 쿼리 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-136">The following example shows how to use keyless entity types to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="07a7c-137">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-137">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) on GitHub.</span></span>

<span data-ttu-id="07a7c-138">먼저 간단한 블로그 및 사후 모델을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-138">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

<span data-ttu-id="07a7c-139">다음으로, 각 블로그의 연결 된 게시물 수를 쿼리 하는 데 사용할 수 있는 간단한 데이터베이스 뷰를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-139">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

<span data-ttu-id="07a7c-140">다음에는 데이터베이스 뷰의 결과를 저장 하는 클래스를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-140">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

<span data-ttu-id="07a7c-141">다음으로 `HasNoKey` API를 사용 하 여 _Onmodelcreating_ 에서 키가 없는 엔터티 형식을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-141">Next, we configure the keyless entity type in _OnModelCreating_ using the `HasNoKey` API.</span></span>
<span data-ttu-id="07a7c-142">흐름 구성 API를 사용 하 여 키가 없는 엔터티 형식에 대 한 매핑을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-142">We use fluent configuration API to configure the mapping for the keyless entity type:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

<span data-ttu-id="07a7c-143">다음으로 `DbSet<T>` @no__t를 포함 하도록 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-143">Next, we configure the `DbContext` to include the `DbSet<T>`:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

<span data-ttu-id="07a7c-144">마지막으로 다음과 같은 표준 방식으로 데이터베이스 뷰를 쿼리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-144">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="07a7c-145">참고로,이 형식에 대 한 쿼리의 루트 역할을 하는 데 사용할 컨텍스트 수준 쿼리 속성 (DbSet)도 정의 했습니다.</span><span class="sxs-lookup"><span data-stu-id="07a7c-145">Note we have also defined a context level query property (DbSet) to act as a root for queries against this type.</span></span>
