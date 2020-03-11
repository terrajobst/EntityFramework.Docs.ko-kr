---
title: 인덱스-EF Core
author: roji
ms.date: 12/16/2019
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 810fccc0c6b035f515107601b245811f7b4118a6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413938"
---
# <a name="indexes"></a><span data-ttu-id="fda45-102">인덱스</span><span class="sxs-lookup"><span data-stu-id="fda45-102">Indexes</span></span>

<span data-ttu-id="fda45-103">인덱스는 많은 데이터 저장소에서 일반적인 개념입니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="fda45-104">데이터 저장소에서의 구현은 다를 수 있지만 열 (또는 열 집합)을 기반으로 조회를 보다 효율적으로 만드는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

<span data-ttu-id="fda45-105">데이터 주석을 사용 하 여 인덱스를 만들 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-105">Indexes cannot be created using data annotations.</span></span> <span data-ttu-id="fda45-106">흐름 API를 사용 하 여 다음과 같이 단일 열에 대 한 인덱스를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-106">You can use the Fluent API to specify an index on a single column as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=4)]

<span data-ttu-id="fda45-107">또한 두 개 이상의 열에 대해 인덱스를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-107">You can also specify an index over more than one column:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=4)]

> [!NOTE]
> <span data-ttu-id="fda45-108">규칙에 따라 외래 키로 사용 되는 각 속성 (또는 속성 집합)에 인덱스가 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-108">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>
>
> <span data-ttu-id="fda45-109">EF Core는 속성의 고유 집합 마다 하나의 인덱스만 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-109">EF Core only supports one index per distinct set of properties.</span></span> <span data-ttu-id="fda45-110">흐름 API를 사용 하 여 인덱스를 이미 정의 하는 속성 집합에서 규칙 또는 이전 구성으로 인덱스를 구성 하는 경우 해당 인덱스의 정의가 변경 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-110">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="fda45-111">규칙으로 만들어진 인덱스를 추가로 구성 하려는 경우에 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-111">This is useful if you want to further configure an index that was created by convention.</span></span>

## <a name="index-uniqueness"></a><span data-ttu-id="fda45-112">인덱스 고유성</span><span class="sxs-lookup"><span data-stu-id="fda45-112">Index uniqueness</span></span>

<span data-ttu-id="fda45-113">기본적으로 인덱스는 고유 하지 않습니다. 여러 행이 인덱스의 열 집합에 대해 동일한 값을 가질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-113">By default, indexes aren't unique: multiple rows are allowed to have the same value(s) for the index's column set.</span></span> <span data-ttu-id="fda45-114">다음과 같이 인덱스를 고유 하 게 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-114">You can make an index unique as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=IndexUnique&highlight=5)]

<span data-ttu-id="fda45-115">인덱스의 열 집합에 대해 동일한 값을 사용 하 여 둘 이상의 엔터티를 삽입 하려고 하면 예외가 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-115">Attempting to insert more than one entity with the same values for the index's column set will cause an exception to be thrown.</span></span>

## <a name="index-name"></a><span data-ttu-id="fda45-116">인덱스 이름</span><span class="sxs-lookup"><span data-stu-id="fda45-116">Index name</span></span>

<span data-ttu-id="fda45-117">규칙에 따라 관계형 데이터베이스에서 만든 인덱스는 `IX_<type name>_<property name>`이름이 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-117">By convention, indexes created in a relational database are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="fda45-118">복합 인덱스의 경우 `<property name>`은 속성 이름의 밑줄로 구분 된 목록입니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-118">For composite indexes, `<property name>` becomes an underscore separated list of property names.</span></span>

<span data-ttu-id="fda45-119">흐름 API를 사용 하 여 데이터베이스에서 만든 인덱스의 이름을 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-119">You can use the Fluent API to set the name of the index created in the database:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexName.cs?name=IndexName&highlight=5)]

## <a name="index-filter"></a><span data-ttu-id="fda45-120">인덱스 필터</span><span class="sxs-lookup"><span data-stu-id="fda45-120">Index filter</span></span>

<span data-ttu-id="fda45-121">일부 관계형 데이터베이스에서는 필터링 된 인덱스 또는 부분 인덱스를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-121">Some relational databases allow you to specify a filtered or partial index.</span></span> <span data-ttu-id="fda45-122">이렇게 하면 열 값의 하위 집합만 인덱싱할 수 있으므로 인덱스의 크기를 줄이고 성능과 디스크 공간 사용을 모두 향상 시킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-122">This allows you to index only a subset of a column's values, reducing the index's size and improving both performance and disk space usage.</span></span> <span data-ttu-id="fda45-123">필터링 된 인덱스 SQL Server에 대 한 자세한 내용은 [설명서를 참조](https://docs.microsoft.com/sql/relational-databases/indexes/create-filtered-indexes)하십시오.</span><span class="sxs-lookup"><span data-stu-id="fda45-123">For more information on SQL Server filtered indexes, [see the documentation](https://docs.microsoft.com/sql/relational-databases/indexes/create-filtered-indexes).</span></span>

<span data-ttu-id="fda45-124">흐름 API를 사용 하 여 SQL 식으로 제공 되는 인덱스에 대 한 필터를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-124">You can use the Fluent API to specify a filter on an index, provided as a SQL expression:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexFilter.cs?name=IndexFilter&highlight=5)]

<span data-ttu-id="fda45-125">SQL Server 공급자를 사용 하는 경우 EF는 고유 인덱스의 일부인 모든 nullable 열에 대해 `'IS NOT NULL'` 필터를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-125">When using the SQL Server provider EF adds an `'IS NOT NULL'` filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="fda45-126">이 규칙을 재정의 하려면 `null` 값을 제공 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-126">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexNoFilter.cs?name=IndexNoFilter&highlight=6)]

## <a name="included-columns"></a><span data-ttu-id="fda45-127">포괄 열</span><span class="sxs-lookup"><span data-stu-id="fda45-127">Included columns</span></span>

<span data-ttu-id="fda45-128">일부 관계형 데이터베이스를 사용 하면 인덱스에 포함 되지만 "키"의 일부가 아닌 열 집합을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-128">Some relational databases allow you to configure a set of columns which get included in the index, but aren't part of its "key".</span></span> <span data-ttu-id="fda45-129">이렇게 하면 테이블 자체에 액세스할 필요가 없으므로 쿼리의 모든 열이 키 또는 키가 아닌 열로 인덱스에 포함 될 때 쿼리 성능이 크게 향상 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-129">This can significantly improve query performance when all columns in the query are included in the index either as key or nonkey columns, as the table itself doesn't need to be accessed.</span></span> <span data-ttu-id="fda45-130">포괄 열 SQL Server에 대 한 자세한 내용은 [설명서를 참조](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns)하십시오.</span><span class="sxs-lookup"><span data-stu-id="fda45-130">For more information on SQL Server included columns, [see the documentation](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns).</span></span>

<span data-ttu-id="fda45-131">다음 예에서는 `Url` 열이 인덱스 키의 일부 이므로 해당 열에 대 한 모든 쿼리 필터링에서 인덱스를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-131">In the following example, the `Url` column is part of the index key, so any query filtering on that column can use the index.</span></span> <span data-ttu-id="fda45-132">또한 `Title` 및 `PublishedOn` 열에만 액세스 하는 쿼리는 테이블에 액세스할 필요가 없으며 더 효율적으로 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fda45-132">But in addition, queries accessing only the `Title` and `PublishedOn` columns will not need to access the table and will run more efficiently:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexInclude.cs?name=IndexInclude&highlight=5-9)]
