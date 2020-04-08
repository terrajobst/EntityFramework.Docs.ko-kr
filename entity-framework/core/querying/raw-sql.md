---
title: 원시 SQL 쿼리 - EF Core
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: a54bb67c0fce9d621382f6372e70fe4cdca48a20
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413716"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="81d99-102">원시 SQL 쿼리</span><span class="sxs-lookup"><span data-stu-id="81d99-102">Raw SQL Queries</span></span>

<span data-ttu-id="81d99-103">Entity Framework Core를 사용하면 관계형 데이터베이스로 작업할 때 원시 SQL 쿼리로 드롭다운할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="81d99-104">원시 SQL 쿼리는 LINQ를 사용하여 원하는 쿼리를 표현할 수 없는 경우에 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-104">Raw SQL queries are useful if the query you want can't be expressed using LINQ.</span></span> <span data-ttu-id="81d99-105">원시 SQL 쿼리는 LINQ 쿼리를 사용하면 비효율적인 SQL 쿼리가 발생하는 경우에도 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-105">Raw SQL queries are also used if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="81d99-106">원시 SQL 쿼리는 기본 엔터티 형식 또는 모델의 일부인 [키 없는 엔터티 형식](xref:core/modeling/keyless-entity-types)을 반환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-106">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="81d99-107">GitHub에서 이 문서의 [샘플](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-107">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="81d99-108">기본 원시 SQL 쿼리</span><span class="sxs-lookup"><span data-stu-id="81d99-108">Basic raw SQL queries</span></span>

<span data-ttu-id="81d99-109">`FromSqlRaw` 확장 메서드를 사용하여 원시 SQL 쿼리를 기반으로 한 LINQ 쿼리를 시작할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-109">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span> <span data-ttu-id="81d99-110">`FromSqlRaw`는 쿼리 루트에만 사용할 수 있습니다. 즉, `DbSet<>`에 직접 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-110">`FromSqlRaw` can only be used on query roots, that is directly on the `DbSet<>`.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

<span data-ttu-id="81d99-111">원시 SQL 쿼리를 사용하여 저장 프로시저를 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-111">Raw SQL queries can be used to execute a stored procedure.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a><span data-ttu-id="81d99-112">매개 변수 전달</span><span class="sxs-lookup"><span data-stu-id="81d99-112">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="81d99-113">**원시 SQL 쿼리에 항상 매개 변수화를 사용하세요.**</span><span class="sxs-lookup"><span data-stu-id="81d99-113">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="81d99-114">사용자가 제공한 값을 원시 SQL 쿼리에 도입할 때는 SQL 삽입 공격을 방지하기 위해 주의를 기울여야 합니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-114">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="81d99-115">이러한 값이 잘못된 문자를 포함하지 않은 것을 확인하는 것 외에도 항상 매개 변수화를 사용하여 SQL 텍스트와 별도로 값을 보내야 합니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-115">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="81d99-116">특히 유효성이 검사되지 않은 사용자 제공 값을 사용하여 연결되거나 보간된 문자열(`$""`)을 `FromSqlRaw` 또는 `ExecuteSqlRaw`로 전달하면 안 됩니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-116">In particular, never pass a concatenated or interpolated string (`$""`) with non-validated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="81d99-117">`FromSqlInterpolated` 및`ExecuteSqlInterpolated` 메서드를 사용하면 SQL 삽입 공격으로부터 보호하는 방식으로 문자열 보간 구문을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-117">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="81d99-118">다음 예에서는 SQL 쿼리 문자열에 매개 변수 자리 표시자를 포함하고 추가 인수를 제공하여 단일 매개 변수를 저장 프로시저에 전달합니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-118">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="81d99-119">이 구문은 `String.Format` 구문처럼 보일 수 있지만 제공된 값은 `DbParameter`로 래핑되고 생성된 매개 변수 이름은 `{0}` 자리 표시자가 지정된 위치에 삽입됩니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-119">While this syntax may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

<span data-ttu-id="81d99-120">`FromSqlInterpolated`는 `FromSqlRaw`와 비슷하지만, 이를 사용하여 문자열 보간 구문을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-120">`FromSqlInterpolated` is similar to `FromSqlRaw` but allows you to use string interpolation syntax.</span></span> <span data-ttu-id="81d99-121">`FromSqlRaw`와 마찬가지로 `FromSqlInterpolated`는 쿼리 루트에만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-121">Just like `FromSqlRaw`, `FromSqlInterpolated` can only be used on query roots.</span></span> <span data-ttu-id="81d99-122">이전 예제와 마찬가지로 값은 `DbParameter`로 변환되므로 SQL 삽입에 취약하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-122">As with the previous example, the value is converted to a `DbParameter` and isn't vulnerable to SQL injection.</span></span>

> [!NOTE]
> <span data-ttu-id="81d99-123">버전 3.0 이전에는 `FromSqlRaw` 및 `FromSqlInterpolated`가 `FromSql`이라는 두 오버로드였습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-123">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="81d99-124">자세한 내용은 [이전 버전 섹션](#previous-versions)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="81d99-124">For more information, see the [previous versions section](#previous-versions).</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

<span data-ttu-id="81d99-125">DbParameter를 생성하고 매개 변수 값으로 제공할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="81d99-126">문자열 자리 표시자가 아닌 일반 SQL 매개 변수 자리 표시자가 사용되므로 `FromSqlRaw`를 안전하게 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-126">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

<span data-ttu-id="81d99-127">`FromSqlRaw`를 사용하면 SQL 쿼리 문자열에 명명된 매개 변수를 사용할 수 있으며, 이는 저장 프로시저가 선택적 매개 변수를 포함하는 경우 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-127">`FromSqlRaw` allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a><span data-ttu-id="81d99-128">LINQ로 작성</span><span class="sxs-lookup"><span data-stu-id="81d99-128">Composing with LINQ</span></span>

<span data-ttu-id="81d99-129">LINQ 연산자를 사용하여 초기 원시 SQL 쿼리의 맨 위에 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-129">You can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="81d99-130">EF Core는 이를 하위 쿼리로 취급하고 데이터베이스에서 이에 대해 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-130">EF Core will treat it as subquery and compose over it in the database.</span></span> <span data-ttu-id="81d99-131">다음 예제에서는 TVF(테이블 반환 함수)에서 선택하는 원시 SQL 쿼리를 사용한 후</span><span class="sxs-lookup"><span data-stu-id="81d99-131">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF).</span></span> <span data-ttu-id="81d99-132">LINQ를 사용하여 이에 대해 구성하여 필터링과 정렬을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-132">And then composes on it using LINQ to do filtering and sorting.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

<span data-ttu-id="81d99-133">위 쿼리는 다음 SQL을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-133">Above query generates following SQL:</span></span>

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a><span data-ttu-id="81d99-134">관련 데이터 포함</span><span class="sxs-lookup"><span data-stu-id="81d99-134">Including related data</span></span>

<span data-ttu-id="81d99-135">`Include` 메서드는 다른 LINQ 쿼리와 마찬가지로 관련 데이터를 포함하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

<span data-ttu-id="81d99-136">LINQ를 사용하여 구성하려면 원시 SQL 쿼리가 구성 가능해야 합니다. EF Core가 제공된 SQL을 하위 쿼리로 취급하기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-136">Composing with LINQ requires your raw SQL query to be composable since EF Core will treat the supplied SQL as a subquery.</span></span> <span data-ttu-id="81d99-137">SQL 쿼리는 `SELECT` 키워드로 시작하여 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-137">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span> <span data-ttu-id="81d99-138">또한 전달된 SQL에는 다음과 같이 하위 쿼리에서 유효하지 않은 문자나 옵션을 포함할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-138">Further, SQL passed shouldn't contain any characters or options that aren't valid on a subquery, such as:</span></span>

- <span data-ttu-id="81d99-139">후행 세미콜론</span><span class="sxs-lookup"><span data-stu-id="81d99-139">A trailing semicolon</span></span>
- <span data-ttu-id="81d99-140">SQL Server의 후행 쿼리 수준 힌트(예: `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="81d99-140">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
- <span data-ttu-id="81d99-141">SQL Server에서, `ORDER BY` 절에서 `OFFSET 0` 또는 `TOP 100 PERCENT`와 함께 사용되지 않는 `SELECT` 절</span><span class="sxs-lookup"><span data-stu-id="81d99-141">On SQL Server, an `ORDER BY` clause that isn't used with `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

<span data-ttu-id="81d99-142">SQL Server는 저장 프로시저 호출에 대한 구성을 허용하지 않으므로 그러한 호출에 추가 쿼리 연산자를 적용하려고 하면 잘못된 SQL이 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-142">SQL Server doesn't allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="81d99-143">EF Core가 저장 프로시저에 대해 구성을 시도하지 않도록 `AsEnumerable` 또는 `AsAsyncEnumerable` 메서드 직후에 `FromSqlRaw` 또는 `FromSqlInterpolated` 메서드를 사용하세요.</span><span class="sxs-lookup"><span data-stu-id="81d99-143">Use `AsEnumerable` or `AsAsyncEnumerable` method right after `FromSqlRaw` or `FromSqlInterpolated` methods to make sure that EF Core doesn't try to compose over a stored procedure.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="81d99-144">변경 내용 추적</span><span class="sxs-lookup"><span data-stu-id="81d99-144">Change Tracking</span></span>

<span data-ttu-id="81d99-145">`FromSqlRaw` 또는 `FromSqlInterpolated` 메서드를 사용하는 쿼리는 EF Core에서 다른 LINQ 쿼리와 완전히 동일한 변경 내용 추적 규칙을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-145">Queries that use the `FromSqlRaw` or `FromSqlInterpolated` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="81d99-146">예를 들어 쿼리 프로젝트 엔터티 형식의 경우 기본적으로 결과가 추적됩니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-146">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="81d99-147">다음 예제에서는 TVF(테이블 반환 함수)에서 선택하는 원시 SQL 쿼리를 사용한 이후에 `AsNoTracking`을 호출하여 변경 내용 추적을 사용하지 않도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-147">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a><span data-ttu-id="81d99-148">제한 사항</span><span class="sxs-lookup"><span data-stu-id="81d99-148">Limitations</span></span>

<span data-ttu-id="81d99-149">원시 SQL 쿼리를 사용할 때는 몇 가지 제한 사항에 유의해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-149">There are a few limitations to be aware of when using raw SQL queries:</span></span>

- <span data-ttu-id="81d99-150">SQL 쿼리는 엔터티 형식의 모든 속성에 대해 데이터를 반환해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-150">The SQL query must return data for all properties of the entity type.</span></span>
- <span data-ttu-id="81d99-151">결과 집합의 열 이름은 속성이 매핑되는 열 이름과 일치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-151">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="81d99-152">이 동작은 EF6와 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-152">Note this behavior is different from EF6.</span></span> <span data-ttu-id="81d99-153">EF6은 속성/열 매핑을 원시 SQL 쿼리에 대해 무시했고 결과 집합 열 이름이 속성 이름과 일치해야 했습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-153">EF6 ignored property to column mapping for raw SQL queries and result set column names had to match the property names.</span></span>
- <span data-ttu-id="81d99-154">SQL 쿼리는 관련 데이터를 포함할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-154">The SQL query can't contain related data.</span></span> <span data-ttu-id="81d99-155">그러나 대부분의 경우 쿼리의 맨 위에서 `Include` 연산자를 사용하여 관련 데이터를 반환하도록 작성할 수 있습니다([관련 데이터 포함](#including-related-data) 참조).</span><span class="sxs-lookup"><span data-stu-id="81d99-155">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="previous-versions"></a><span data-ttu-id="81d99-156">이전 버전</span><span class="sxs-lookup"><span data-stu-id="81d99-156">Previous versions</span></span>

<span data-ttu-id="81d99-157">EF Core 버전 2.2 및 이전 버전에는 최신 `FromSql` 및 `FromSqlRaw`와 동일한 방식으로 작동하는 `FromSqlInterpolated` 메서드라는 두 개의 오버로드가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-157">EF Core version 2.2 and earlier had two overloads of method named `FromSql`, which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="81d99-158">이로 인해 보간된 문자열 메서드를 호출하려다 실수로 원시 문자열 메서드를 호출하거나, 반대로 후자를 호출하려다 전자를 호출하는 실수를 저지를 가능성이 매우 높습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-158">It was easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="81d99-159">실수로 잘못된 오버로드를 호출하면 매개 변수가 있어야 하는 쿼리에 매개 변수가 있지 않게 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="81d99-159">Calling wrong overload accidentally could result in queries not being parameterized when they should have been.</span></span>
