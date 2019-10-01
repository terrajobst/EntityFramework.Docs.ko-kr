---
title: 원시 SQL 쿼리 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: ebec5775770c0f1e297eaaf35bf644c605a69afc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197765"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="00666-102">원시 SQL 쿼리</span><span class="sxs-lookup"><span data-stu-id="00666-102">Raw SQL Queries</span></span>

<span data-ttu-id="00666-103">Entity Framework Core를 사용하면 관계형 데이터베이스로 작업할 때 원시 SQL 쿼리로 드롭다운할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="00666-104">이는 수행하려는 쿼리를 LINQ를 사용하여 표현할 수 없거나 LINQ 쿼리를 사용하면 비효율적인 SQL 쿼리가 발생하는 경우에 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="00666-105">원시 SQL 쿼리는 기본 엔터티 형식 또는 모델의 일부인 [키 없는 엔터티 형식](xref:core/modeling/keyless-entity-types)을 반환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-105">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="00666-106">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="00666-107">기본 원시 SQL 쿼리</span><span class="sxs-lookup"><span data-stu-id="00666-107">Basic raw SQL queries</span></span>

<span data-ttu-id="00666-108">`FromSqlRaw` 확장 메서드를 사용하여 원시 SQL 쿼리를 기반으로 한 LINQ 쿼리를 시작할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-108">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="00666-109">원시 SQL 쿼리를 사용하여 저장 프로시저를 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-109">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="00666-110">매개 변수 전달</span><span class="sxs-lookup"><span data-stu-id="00666-110">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="00666-111">**원시 SQL 쿼리에 항상 매개 변수화를 사용하세요.**</span><span class="sxs-lookup"><span data-stu-id="00666-111">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="00666-112">사용자가 제공한 값을 원시 SQL 쿼리에 도입할 때는 SQL 삽입 공격을 방지하기 위해 주의를 기울여야 합니다.</span><span class="sxs-lookup"><span data-stu-id="00666-112">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="00666-113">이러한 값이 잘못된 문자를 포함하지 않은 것을 확인하는 것 외에도 항상 매개 변수화를 사용하여 SQL 텍스트와 별도로 값을 보내야 합니다.</span><span class="sxs-lookup"><span data-stu-id="00666-113">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="00666-114">특히 유효성이 검사되지 않은 사용자 제공 값을 사용하여 연결되거나 보간된 문자열(`$""`)을 `FromSqlRaw` 또는 `ExecuteSqlRaw`로 전달하면 안 됩니다.</span><span class="sxs-lookup"><span data-stu-id="00666-114">In particular, never pass a concatenated or interpolated string (`$""`) with unvalidated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="00666-115">`FromSqlInterpolated` 및`ExecuteSqlInterpolated` 메서드를 사용하면 SQL 삽입 공격으로부터 보호하는 방식으로 문자열 보간 구문을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-115">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="00666-116">다음 예에서는 SQL 쿼리 문자열에 매개 변수 자리 표시자를 포함하고 추가 인수를 제공하여 단일 매개 변수를 저장 프로시저에 전달합니다.</span><span class="sxs-lookup"><span data-stu-id="00666-116">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="00666-117">`String.Format` 구문처럼 보일 수 있지만 제공된 값은 `DbParameter`로 래핑되고 생성된 매개 변수 이름은 `{0}` 자리 표시자가 지정된 위치에 삽입됩니다.</span><span class="sxs-lookup"><span data-stu-id="00666-117">While this may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="00666-118">`FromSqlRaw`대신 문자열 보간을 안전하게 사용할 수 있도록 하는 `FromSqlInterpolated`를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-118">As an alternative to `FromSqlRaw`, you can use `FromSqlInterpolated` which allows the safe use of string interpolation.</span></span> <span data-ttu-id="00666-119">이전 예제와 마찬가지로 값은 `DbParameter`로 변환되므로 SQL 삽입에 취약하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-119">As with the previous example, the value is converted to a `DbParameter` and is therefore not vulnerable to SQL injection:</span></span>

> [!NOTE]
> <span data-ttu-id="00666-120">버전 3.0 이전에는 `FromSqlRaw` 및 `FromSqlInterpolated`가 `FromSql`이라는 두 오버로드였습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-120">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="00666-121">자세한 내용은 [이전 버전 섹션](#previous-versions)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="00666-121">See the [previous versions section](#previous-versions) for more details.</span></span>


<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="00666-122">DbParameter를 생성하고 매개 변수 값으로 제공할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-122">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="00666-123">문자열 자리 표시자가 아닌 일반 SQL 매개 변수 자리 표시자가 사용되므로 `FromSqlRaw`를 안전하게 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-123">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

<span data-ttu-id="00666-124">이렇게 하면 SQL 쿼리 문자열에 명명된 매개 변수를 사용할 수 있으며, 이는 저장 프로시저가 선택적 매개 변수를 포함하는 경우 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="00666-124">This allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="00666-125">LINQ로 작성</span><span class="sxs-lookup"><span data-stu-id="00666-125">Composing with LINQ</span></span>

<span data-ttu-id="00666-126">SQL 쿼리를 데이터베이스에서 작성할 수 있는 경우 LINQ 연산자를 사용하여 초기 원시 SQL 쿼리의 맨 위에 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-126">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="00666-127">SQL 쿼리는 `SELECT` 키워드로 시작하여 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-127">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="00666-128">다음 예제에서는 TVF(테이블 반환 함수)에서 선택한 다음, LINQ로 이를 작성하여 필터링 및 정렬을 수행하는 원시 SQL 쿼리를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="00666-128">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

<span data-ttu-id="00666-129">이것은 다음 SQL 쿼리를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="00666-129">This will produce the following SQL query:</span></span>

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a><span data-ttu-id="00666-130">변경 내용 추적</span><span class="sxs-lookup"><span data-stu-id="00666-130">Change Tracking</span></span>

<span data-ttu-id="00666-131">`FromSql` 메서드를 사용하는 쿼리는 EF Core에서 다른 LINQ 쿼리와 완전히 동일한 변경 내용 추적 규칙을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="00666-131">Queries that use the `FromSql` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="00666-132">예를 들어 쿼리 프로젝트 엔터티 형식의 경우 기본적으로 결과가 추적됩니다.</span><span class="sxs-lookup"><span data-stu-id="00666-132">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="00666-133">다음 예제에서는 TVF(테이블 반환 함수)에서 선택하는 원시 SQL 쿼리를 사용한 이후에 `AsNoTracking`을 호출하여 변경 내용 추적을 사용하지 않도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="00666-133">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a><span data-ttu-id="00666-134">관련 데이터 포함</span><span class="sxs-lookup"><span data-stu-id="00666-134">Including related data</span></span>

<span data-ttu-id="00666-135">`Include` 메서드는 다른 LINQ 쿼리와 마찬가지로 관련 데이터를 포함하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

<span data-ttu-id="00666-136">이를 위해서는 원시 SQL 쿼리를 작성해야 합니다. 이것은 저장 프로시저 호출에서는 작동하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-136">Note that this requires your raw SQL query to be composable; it will notably not work with stored procedure calls.</span></span> <span data-ttu-id="00666-137">[제한 사항](#limitations)에서 작성 가능성에 대한 내용을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="00666-137">See notes on composability under [Limitations](#limitations)).</span></span>

## <a name="limitations"></a><span data-ttu-id="00666-138">제한 사항</span><span class="sxs-lookup"><span data-stu-id="00666-138">Limitations</span></span>

<span data-ttu-id="00666-139">원시 SQL 쿼리를 사용할 때는 몇 가지 제한 사항에 유의해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="00666-139">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="00666-140">SQL 쿼리는 엔터티 형식의 모든 속성에 대해 데이터를 반환해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="00666-140">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="00666-141">결과 집합의 열 이름은 속성이 매핑되는 열 이름과 일치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="00666-141">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="00666-142">속성/열 매핑이 원시 SQL 쿼리에 대해 무시되고 결과 집합 열 이름이 속성 이름과 일치해야 한다는 점에서 EF6와 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="00666-142">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="00666-143">SQL 쿼리에 관련 데이터가 포함될 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-143">The SQL query cannot contain related data.</span></span> <span data-ttu-id="00666-144">그러나 대부분의 경우 쿼리의 맨 위에서 `Include` 연산자를 사용하여 관련 데이터를 반환하도록 작성할 수 있습니다([관련 데이터 포함](#including-related-data) 참조).</span><span class="sxs-lookup"><span data-stu-id="00666-144">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="00666-145">이 메서드에 전달된 `SELECT` 문은 일반적으로 구성 가능해야 합니다. EF Core가 서버에서 추가 쿼리 연산자를 평가해야 하는 경우(예: `FromSql` 메서드 다음에 적용된 LINQ 연산자를 변환해야 하는 경우) 제공된 SQL은 하위 쿼리로 처리됩니다.</span><span class="sxs-lookup"><span data-stu-id="00666-145">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql` methods), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="00666-146">즉, 전달된 SQL에는 다음과 같이 하위 쿼리에서 유효하지 않은 문자나 옵션을 포함할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-146">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="00666-147">후행 세미콜론</span><span class="sxs-lookup"><span data-stu-id="00666-147">A trailing semicolon</span></span>
  * <span data-ttu-id="00666-148">SQL Server의 후행 쿼리 수준 힌트(예: `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="00666-148">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="00666-149">SQL Server에서, `SELECT` 절에서 `OFFSET 0` 또는 `TOP 100 PERCENT`와 함께 제공되지 않는 `ORDER BY` 절</span><span class="sxs-lookup"><span data-stu-id="00666-149">On SQL Server, an `ORDER BY` clause that is not accompanied of `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="00666-150">SQL Server는 저장 프로시저 호출에 대한 작성을 허용하지 않으므로 그러한 호출에 추가 쿼리 연산자를 적용하려고 하면 잘못된 SQL이 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="00666-150">Note that SQL Server does not allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="00666-151">클라이언트 평가를 위해 `AsEnumerable()` 뒤에 쿼리 연산자를 도입할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-151">Query operators may be introduced after `AsEnumerable()` for client evaluation.</span></span>

# <a name="previous-versions"></a><span data-ttu-id="00666-152">이전 버전</span><span class="sxs-lookup"><span data-stu-id="00666-152">Previous versions</span></span>

<span data-ttu-id="00666-153">EF Core 버전 2.2 및 이전 버전에는 최신 `FromSqlRaw` 및 `FromSqlInterpolated`와 동일한 방식으로 작동하는 `FromSql`이라는 두 개의 오버로드가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-153">EF Core version 2.2 and earlier had two overloads named `FromSql` which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="00666-154">이로 인해 보간된 문자열 메서드를 호출하려다 실수로 원시 문자열 메서드를 호출하거나, 반대로 후자를 호출하려다 전자를 호출하는 실수를 저지를 가능성이 매우 높습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-154">This made it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="00666-155">쿼리에는 매개 변수가 있어야 하는데, 이 경우 매개 변수가 없는 쿼리가 나올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00666-155">This could result in queries not being parameterized when they should have been.</span></span>
