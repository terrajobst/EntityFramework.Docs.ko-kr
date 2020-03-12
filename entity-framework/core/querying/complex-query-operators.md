---
title: 복합 쿼리 연산자-EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 2e187a2a-4072-4198-9040-aaad68e424fd
uid: core/querying/complex-query-operators
ms.openlocfilehash: 44c2695ea003da043925740a52596fd27da638f8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413776"
---
# <a name="complex-query-operators"></a><span data-ttu-id="33ffe-102">복합 쿼리 연산자</span><span class="sxs-lookup"><span data-stu-id="33ffe-102">Complex Query Operators</span></span>

<span data-ttu-id="33ffe-103">LINQ(통합 언어 쿼리)에는 여러 데이터 원본을 결합하거나 복잡한 처리를 수행하는 여러 복합 연산자가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-103">Language Integrated Query (LINQ) contains many complex operators, which combine multiple data sources or does complex processing.</span></span> <span data-ttu-id="33ffe-104">모든 LINQ 연산자가 서버 쪽에 적절한 변환을 가지고 있는 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-104">Not all LINQ operators have suitable translations on the server side.</span></span> <span data-ttu-id="33ffe-105">쿼리가 특정 형식에서는 서버에서 변환되지만 다른 형식에서는 결과가 동일하더라도 변환되지 않는 경우가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-105">Sometimes, a query in one form translates to the server but if written in a different form doesn't translate even if the result is the same.</span></span> <span data-ttu-id="33ffe-106">이 페이지에서는 몇 가지 복합 연산자와 지원되는 변형에 대해 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-106">This page describes some of the complex operators and their supported variations.</span></span> <span data-ttu-id="33ffe-107">이후 릴리스에서는 더 많은 패턴을 인식하고 해당 변환을 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-107">In future releases, we may recognize more patterns and add their corresponding translations.</span></span> <span data-ttu-id="33ffe-108">변환 지원은 공급자 간에 다를 수 있다는 점을 염두에 두어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-108">It's also important to keep in mind that translation support varies between providers.</span></span> <span data-ttu-id="33ffe-109">SqlServer에서는 변환되는 쿼리가 SQLite 데이터베이스에서는 작동하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-109">A particular query, which is translated in SqlServer, may not work for SQLite databases.</span></span>

> [!TIP]
> <span data-ttu-id="33ffe-110">GitHub에서 이 문서의 [샘플](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-110">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="join"></a><span data-ttu-id="33ffe-111">Join</span><span class="sxs-lookup"><span data-stu-id="33ffe-111">Join</span></span>

<span data-ttu-id="33ffe-112">LINQ Join 연산자를 사용하면 각 소스의 키 선택기에 따라 두 데이터 원본을 연결하여 키가 일치하면 값 튜플을 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-112">The LINQ Join operator allows you to connect two data sources based on the key selector for each source, generating a tuple of values when the key matches.</span></span> <span data-ttu-id="33ffe-113">Join은 관계형 데이터베이스에서 `INNER JOIN`으로 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-113">It naturally translates to `INNER JOIN` on relational databases.</span></span> <span data-ttu-id="33ffe-114">LINQ Join에는 외부 및 내부 키 선택기가 있지만 데이터베이스에는 하나의 조인 조건만 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-114">While the LINQ Join has outer and inner key selectors, the database requires a single join condition.</span></span> <span data-ttu-id="33ffe-115">따라서 EF Core는 외부 키 선택기와 내부 키 선택기를 비교하고 동일성을 확인하여 조인 조건을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-115">So EF Core generates a join condition by comparing the outer key selector to the inner key selector for equality.</span></span> <span data-ttu-id="33ffe-116">키 선택기가 무명 형식인 경우, EF Core는 동일성을 구성 요소별로 비교하는 조인 조건을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-116">Further, if the key selectors are anonymous types, EF Core generates a join condition to compare equality component wise.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a><span data-ttu-id="33ffe-117">GroupJoin</span><span class="sxs-lookup"><span data-stu-id="33ffe-117">GroupJoin</span></span>

<span data-ttu-id="33ffe-118">LINQ GroupJoin 연산자를 사용하면 Join과 비슷하게 두 데이터 원본을 연결할 수 있는데, 이때 일치하는 외부 요소에 대한 내부 값 그룹이 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-118">The LINQ GroupJoin operator allows you to connect two data sources similar to Join, but it creates a group of inner values for matching outer elements.</span></span> <span data-ttu-id="33ffe-119">다음 예와 같이 쿼리를 실행하면 `Blog` & `IEnumerable<Post>` 결과가 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-119">Executing a query like the following example generates a result of `Blog` & `IEnumerable<Post>`.</span></span> <span data-ttu-id="33ffe-120">데이터베이스(특히 관계형 데이터베이스)에는 클라이언트 쪽 개체를 나타낼 방법이 없으므로 GroupJoin은 많은 경우에 서버 쪽으로 변환되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-120">Since databases (especially relational databases) don't have a way to represent a collection of client-side objects, GroupJoin doesn't translate to the server in many cases.</span></span> <span data-ttu-id="33ffe-121">특수 선택기 없이 GroupJoin을 수행하려면 서버에서 모든 데이터를 가져와야 합니다(아래의 첫 번째 쿼리).</span><span class="sxs-lookup"><span data-stu-id="33ffe-121">It requires you to get all of the data from the server to do GroupJoin without a special selector (first query below).</span></span> <span data-ttu-id="33ffe-122">그러나 선택기가 선택되는 데이터를 제한하는 경우에는 서버에서 모든 데이터를 가져오는 것이 성능 문제를 야기할 수 있습니다(아래의 두 번째 쿼리).</span><span class="sxs-lookup"><span data-stu-id="33ffe-122">But if the selector is limiting data being selected then fetching all of the data from the server may cause performance issues (second query below).</span></span> <span data-ttu-id="33ffe-123">따라서 EF Core는 GroupJoin을 변환하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-123">That's why EF Core doesn't translate GroupJoin.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a><span data-ttu-id="33ffe-124">SelectMany</span><span class="sxs-lookup"><span data-stu-id="33ffe-124">SelectMany</span></span>

<span data-ttu-id="33ffe-125">LINQ SelectMany 연산자를 사용하면 각 외부 요소에 대해 컬렉션 선택기를 기준으로 열거하고 각 데이터 원본으로부터 값 튜플을 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-125">The LINQ SelectMany operator allows you to enumerate over a collection selector for each outer element and generate tuples of values from each data source.</span></span> <span data-ttu-id="33ffe-126">어떤 면에서는 Join과 비슷하지만 조건이 없으므로 모든 외부 요소가 컬렉션 소스의 요소에 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-126">In a way, it's a join but without any condition so every outer element is connected with an element from the collection source.</span></span> <span data-ttu-id="33ffe-127">컬렉션 선택기가 외부 데이터 원본과 어떤 관계를 맺고 있는지에 따라 SelectMany는 서버 쪽에서 각종 쿼리로 변환될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-127">Depending on how the collection selector is related to the outer data source, SelectMany can translate into various different queries on the server side.</span></span>

### <a name="collection-selector-doesnt-reference-outer"></a><span data-ttu-id="33ffe-128">컬렉션 선택기는 외부를 참조하지 않음</span><span class="sxs-lookup"><span data-stu-id="33ffe-128">Collection selector doesn't reference outer</span></span>

<span data-ttu-id="33ffe-129">컬렉션 선택기가 외부 소스의 무엇도 참조하지 않을 경우, 결과는 두 데이터 원본의 카티전 곱이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-129">When the collection selector isn't referencing anything from the outer source, the result is a cartesian product of both data sources.</span></span> <span data-ttu-id="33ffe-130">이는 관계형 데이터베이스에서 `CROSS JOIN`으로 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-130">It translates to `CROSS JOIN` in relational databases.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a><span data-ttu-id="33ffe-131">컬렉션 선택기는 where 절에서 외부를 참조함</span><span class="sxs-lookup"><span data-stu-id="33ffe-131">Collection selector references outer in a where clause</span></span>

<span data-ttu-id="33ffe-132">컬렉션 선택기에 외부 요소를 참조하는 where 절이 있는 경우 EF Core는 이를 데이터베이스 조인으로 변환하고 조건자를 조인 조건으로 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-132">When the collection selector has a where clause, which references the outer element, then EF Core translates it to a database join and uses the predicate as the join condition.</span></span> <span data-ttu-id="33ffe-133">일반적으로 이 사례는 외부 요소에 대한 컬렉션 탐색을 컬렉션 선택기로 사용하는 경우에 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-133">Normally this case arises when using collection navigation on the outer element as the collection selector.</span></span> <span data-ttu-id="33ffe-134">외부 요소에 대한 컬렉션이 비어 있으면 해당 외부 요소에 대해 결과가 생성되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-134">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="33ffe-135">그러나 컬렉션 선택기에 `DefaultIfEmpty`가 적용된 경우에는 외부 요소가 내부 요소의 기본값에 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-135">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="33ffe-136">이러한 차이 때문에 이 유형의 쿼리는 `DefaultIfEmpty`가 적용되지 않은 경우 `INNER JOIN`으로 변환되고 `DefaultIfEmpty`가 적용된 경우 `LEFT JOIN`으로 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-136">Because of this distinction, this kind of queries translates to `INNER JOIN` in the absence of `DefaultIfEmpty` and `LEFT JOIN` when `DefaultIfEmpty` is applied.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a><span data-ttu-id="33ffe-137">where가 아닌 케이스에서 컬렉션 선택기가 외부를 참조함</span><span class="sxs-lookup"><span data-stu-id="33ffe-137">Collection selector references outer in a non-where case</span></span>

<span data-ttu-id="33ffe-138">컬렉션 선택기가 위 사례와 달리 where 절에 있지 않은 외부 요소를 참조하는 경우에는 데이터베이스 조인으로 변환되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-138">When the collection selector references the outer element, which isn't in a where clause (as the case above), it doesn't translate to a database join.</span></span> <span data-ttu-id="33ffe-139">따라서 각 외부 요소에 대해 컬렉션 선택기를 평가해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-139">That's why we need to evaluate the collection selector for each outer element.</span></span> <span data-ttu-id="33ffe-140">많은 관계형 데이터베이스에서 이는 `APPLY` 연산으로 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-140">It translates to `APPLY` operations in many relational databases.</span></span> <span data-ttu-id="33ffe-141">외부 요소에 대한 컬렉션이 비어 있으면 해당 외부 요소에 대해 결과가 생성되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-141">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="33ffe-142">그러나 컬렉션 선택기에 `DefaultIfEmpty`가 적용된 경우에는 외부 요소가 내부 요소의 기본값에 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-142">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="33ffe-143">이러한 차이 때문에 이 유형의 쿼리는 `DefaultIfEmpty`가 적용되지 않은 경우 `CROSS APPLY`로 변환되고 `DefaultIfEmpty`가 적용된 경우 `OUTER APPLY`로 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-143">Because of this distinction, this kind of queries translates to `CROSS APPLY` in the absence of `DefaultIfEmpty` and `OUTER APPLY` when `DefaultIfEmpty` is applied.</span></span> <span data-ttu-id="33ffe-144">SQLite와 같은 특정 데이터베이스에서는 `APPLY` 연산자를 지원하지 않으므로 이러한 유형의 쿼리가 변환되지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-144">Certain databases like SQLite don't support `APPLY` operators so this kind of query may not be translated.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a><span data-ttu-id="33ffe-145">GroupBy</span><span class="sxs-lookup"><span data-stu-id="33ffe-145">GroupBy</span></span>

<span data-ttu-id="33ffe-146">LINQ GroupBy 연산자는 `IGrouping<TKey, TElement>` 유형의 결과를 만듭니다. 여기서 `TKey`와 `TElement`는 어떠한 임의의 유형도 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-146">LINQ GroupBy operators create a result of type `IGrouping<TKey, TElement>` where `TKey` and `TElement` could be any arbitrary type.</span></span> <span data-ttu-id="33ffe-147">이에 더해, `IGrouping`은 `IEnumerable<TElement>`를 구현합니다. 즉, 그룹화 뒤에 모든 LINQ 연산자를 사용하여 이에 대해 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-147">Furthermore, `IGrouping` implements `IEnumerable<TElement>`, which means you can compose over it using any LINQ operator after the grouping.</span></span> <span data-ttu-id="33ffe-148">`IGrouping`을 나타낼 수 있는 데이터베이스 구조는 없으므로 GroupBy 연산자는 대부분의 경우 변환이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-148">Since no database structure can represent an `IGrouping`, GroupBy operators have no translation in most cases.</span></span> <span data-ttu-id="33ffe-149">각 그룹에 스칼라를 반환하는 집계 연산자가 적용되면 관계형 데이터베이스에서 SQL `GROUP BY`로 변환될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-149">When an aggregate operator is applied to each group, which returns a scalar, it can be translated to SQL `GROUP BY` in relational databases.</span></span> <span data-ttu-id="33ffe-150">SQL `GROUP BY`도 제한적입니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-150">The SQL `GROUP BY` is restrictive too.</span></span> <span data-ttu-id="33ffe-151">이것을 사용하려면 스칼라 값으로만 그룹화해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-151">It requires you to group only by scalar values.</span></span> <span data-ttu-id="33ffe-152">프로젝션은 그룹화 키 열 또는 열에 대해 적용된 집계만 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-152">The projection can only contain grouping key columns or any aggregate applied over a column.</span></span> <span data-ttu-id="33ffe-153">EF Core는 이 패턴을 식별한 후 다음 예제와 같이 서버로 변환합니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-153">EF Core identifies this pattern and translates it to the server, as in the following example:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

<span data-ttu-id="33ffe-154">EF Core는 그룹화에 대한 집계 연산자가 Where 또는 OrderBy(또는 여타 정렬) LINQ 연산자에 나타나는 쿼리도 변환합니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-154">EF Core also translates queries where an aggregate operator on the grouping appears in a Where or OrderBy (or other ordering) LINQ operator.</span></span> <span data-ttu-id="33ffe-155">EF Core는 where 절에 대해 SQL의 `HAVING` 절을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-155">It uses `HAVING` clause in SQL for the where clause.</span></span> <span data-ttu-id="33ffe-156">GroupBy 연산자를 적용하기 전에 오는 쿼리 부분은 서버로 변환될 수만 있다면 어떠한 복합 쿼리도 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-156">The part of the query before applying the GroupBy operator can be any complex query as long as it can be translated to server.</span></span> <span data-ttu-id="33ffe-157">그뿐만 아니라, 결과 원본에서 그룹화를 제거하기 위해 그룹화 쿼리에 집계 연산자를 적용한 후에는 이 위에 다른 쿼리처럼 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-157">Furthermore, once you apply aggregate operators on a grouping query to remove groupings from the resulting source, you can compose on top of it like any other query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

<span data-ttu-id="33ffe-158">EF Core에서 지원하는 집계 연산자는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-158">The aggregate operators EF Core supports are as follows</span></span>

- <span data-ttu-id="33ffe-159">평균</span><span class="sxs-lookup"><span data-stu-id="33ffe-159">Average</span></span>
- <span data-ttu-id="33ffe-160">개수</span><span class="sxs-lookup"><span data-stu-id="33ffe-160">Count</span></span>
- <span data-ttu-id="33ffe-161">LongCount</span><span class="sxs-lookup"><span data-stu-id="33ffe-161">LongCount</span></span>
- <span data-ttu-id="33ffe-162">최대</span><span class="sxs-lookup"><span data-stu-id="33ffe-162">Max</span></span>
- <span data-ttu-id="33ffe-163">최소</span><span class="sxs-lookup"><span data-stu-id="33ffe-163">Min</span></span>
- <span data-ttu-id="33ffe-164">Sum</span><span class="sxs-lookup"><span data-stu-id="33ffe-164">Sum</span></span>

## <a name="left-join"></a><span data-ttu-id="33ffe-165">왼쪽 조인</span><span class="sxs-lookup"><span data-stu-id="33ffe-165">Left Join</span></span>

<span data-ttu-id="33ffe-166">왼쪽 조인은 LINQ 연산자는 아니지만, 관계형 데이터베이스에는 쿼리에서 자주 사용되는 왼쪽 조인이라는 개념이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-166">While Left Join isn't a LINQ operator, relational databases have the concept of a Left Join which is frequently used in queries.</span></span> <span data-ttu-id="33ffe-167">LINQ 쿼리의 특정 패턴이 서버에서 `LEFT JOIN`과 같은 결과를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-167">A particular pattern in LINQ queries gives the same result as a `LEFT JOIN` on the server.</span></span> <span data-ttu-id="33ffe-168">EF Core는 이러한 패턴을 식별하고 서버 쪽에서 여기에 해당하는 `LEFT JOIN`을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-168">EF Core identifies such patterns and generates the equivalent `LEFT JOIN` on the server side.</span></span> <span data-ttu-id="33ffe-169">패턴에는 데이터 원본 간에 GroupJoin을 만든 다음 내부에 관련 요소가 없는 경우 NULL과 일치하도록 그룹화 소스에 DefaultIfEmpty를 적용한 상태로 SelectMany를 연산자를 사용하여 그룹화를 평면화하는 과정이 수반됩니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-169">The pattern involves creating a GroupJoin between both the data sources and then flattening out the grouping by using the SelectMany operator with DefaultIfEmpty on the grouping source to match null when the inner doesn't have a related element.</span></span> <span data-ttu-id="33ffe-170">다음 예에서는 패턴이 어떤 모양이며 무엇을 생성하는지 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-170">The following example shows what that pattern looks like and what it generates.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

<span data-ttu-id="33ffe-171">위 패턴은 식 트리에서 복합 구조체를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-171">The above pattern creates a complex structure in the expression tree.</span></span> <span data-ttu-id="33ffe-172">그렇기 때문에 EF Core는 연산자 직후에 오는 단계에서 GroupJoin 연산자의 그룹화 결과를 평면화할 것을 요구합니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-172">Because of that, EF Core requires you to flatten out the grouping results of the GroupJoin operator in a step immediately following the operator.</span></span> <span data-ttu-id="33ffe-173">GroupJoin-DefaultIfEmpty-SelectMany가 다른 패턴에서 사용되더라도 이를 왼쪽 조인으로 식별하지 못할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="33ffe-173">Even if the GroupJoin-DefaultIfEmpty-SelectMany is used but in a different pattern, we may not identify it as a Left Join.</span></span>
