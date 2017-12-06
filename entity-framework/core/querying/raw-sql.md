---
title: "원시 SQL 쿼리-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: ddf3a841800684688d50dcf9323f4d83c851222f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="raw-sql-queries"></a><span data-ttu-id="f5a96-102">원시 SQL 쿼리</span><span class="sxs-lookup"><span data-stu-id="f5a96-102">Raw SQL Queries</span></span>

<span data-ttu-id="f5a96-103">Entity Framework Core를 사용 하면 관계형 데이터베이스와 함께 사용할 때 원시 SQL 쿼리로 드롭다운 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="f5a96-104">LINQ를 사용 하 여 수행 하려는 쿼리를 표현할 수 없는 경우 또는 비효율적인 SQL 데이터베이스에 전송 되는 줄여주는 LINQ 쿼리를 사용 하는 경우에 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="f5a96-105">이 문서를 볼 수 있습니다 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub에서 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="f5a96-106">제한 사항</span><span class="sxs-lookup"><span data-stu-id="f5a96-106">Limitations</span></span>

<span data-ttu-id="f5a96-107">원시 SQL 쿼리를 사용 하는 경우 알아두어야 할 몇 가지 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-107">There are a couple of limitations to be aware of when using raw SQL queries:</span></span>
* <span data-ttu-id="f5a96-108">SQL 쿼리 엔터티 형식을 모델의 일부인 반환에 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-108">SQL queries can only be used to return entity types that are part of your model.</span></span> <span data-ttu-id="f5a96-109">향상 된 기능을 통해 백로그에 없는 [원시 SQL 쿼리에서 임시 형식을 반환 하는 활성화](https://github.com/aspnet/EntityFramework/issues/1862)합니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-109">There is an enhancement on our backlog to [enable returning ad-hoc types from raw SQL queries](https://github.com/aspnet/EntityFramework/issues/1862).</span></span>

* <span data-ttu-id="f5a96-110">SQL 쿼리 엔터티 형식의 모든 속성에 대 한 데이터를 반환 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-110">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="f5a96-111">결과 집합의 열 이름 속성에 매핑되는 열 이름이 일치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-111">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="f5a96-112">이 옵션은 원시 SQL 쿼리에 대 한 속성/열 매핑이 무시 되었습니다 여기서 EF6 다릅니다 및 결과 집합 열 이름은 속성 이름과 일치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-112">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="f5a96-113">SQL 쿼리는 관련된 데이터를 포함할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-113">The SQL query cannot contain related data.</span></span> <span data-ttu-id="f5a96-114">그러나 대부분의 경우에서 구성할 수 있습니다를 사용 하 여 쿼리를 기반으로 `Include` 관련된 데이터를 반환 하도록 연산자 (참조 [관련된 데이터를 포함 하 여](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="f5a96-114">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="f5a96-115">기본 원시 SQL 쿼리</span><span class="sxs-lookup"><span data-stu-id="f5a96-115">Basic raw SQL queries</span></span>

<span data-ttu-id="f5a96-116">사용할 수는 *FromSql* 확장 메서드를 원시 SQL 쿼리를 기반으로 하는 LINQ 쿼리를 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-116">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="f5a96-117">저장된 프로시저를 실행할 원시 SQL 쿼리를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-117">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="f5a96-118">매개 변수 전달</span><span class="sxs-lookup"><span data-stu-id="f5a96-118">Passing parameters</span></span>

<span data-ttu-id="f5a96-119">SQL을 허용 하는 API와 마찬가지로 SQL 삽입 공격 으로부터 보호 하기 위해 입력 하는 모든 사용자를 매개 변수화 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-119">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="f5a96-120">SQL 쿼리 문자열에 매개 변수 자리 표시자를 포함할 수 있으며 매개 변수 값으로 추가 인수를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-120">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="f5a96-121">제공한 매개 변수 값으로 자동으로 변환 됩니다는 `DbParameter`합니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-121">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="f5a96-122">다음 예제에서는 저장된 프로시저를 단일 매개 변수를 전달합니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-122">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="f5a96-123">처럼 보일 수이 하는 동안 `String.Format` 구문, 제공 된 값이 요소에 래핑되 생성 된 매개 변수 이름과 매개 변수 삽입 위치는 `{0}` 자리 표시자를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-123">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="f5a96-124">이 동일한 쿼리는 있지만 이상 EF 코어 2.0 버전에서 지원 되는 문자열 보간 구문을 사용 하 여:</span><span class="sxs-lookup"><span data-stu-id="f5a96-124">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="f5a96-125">DbParameter를 생성 하 고 매개 변수 값으로 제공할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="f5a96-126">SQL 쿼리 문자열의 명명 된 매개 변수를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-126">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="f5a96-127">LINQ로 작성</span><span class="sxs-lookup"><span data-stu-id="f5a96-127">Composing with LINQ</span></span>

<span data-ttu-id="f5a96-128">데이터베이스에서에서 SQL 쿼리를 구성할 수, 있으면 LINQ 연산자를 사용 하 여 초기 원시 SQL 쿼리를 기반으로 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-128">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="f5a96-129">사용 중인에 구성할 수 있는 SQL 쿼리는 `SELECT` 키워드입니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-129">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="f5a96-130">다음 예제에서는 LINQ를 사용 하 여 필터링 및 정렬을 수행 하기에 테이블 반환 함수 (TVF)에서 선택 하 고 다음을 구성 하는 원시 SQL 쿼리를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-130">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="f5a96-131">관련된 데이터를 포함 하 여</span><span class="sxs-lookup"><span data-stu-id="f5a96-131">Including related data</span></span>

<span data-ttu-id="f5a96-132">LINQ 연산자가 있는 작성 용도 쿼리에 관련 된 데이터를 포함 하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-132">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="f5a96-133">**항상 원시 SQL 쿼리를 매개 변수화를 사용 하도록:** 원시 SQL 허용 하는 Api와 같은 문자열 `FromSql` 및 `ExecuteSqlCommand` 쉽게 매개 변수로 전달 될 값을 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-133">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="f5a96-134">사용자 입력의 유효성을 검사 하는 것 외에도 항상 원시 SQL 쿼리/명령에서 사용 되는 모든 값에 대 한 매개 변수화를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-134">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="f5a96-135">사용할 경우 문자열 연결을 동적으로 SQL 삽입 공격 으로부터 보호 하기 위해 모든 입력 유효성 검사를 담당 하는 다음 쿼리 문자열의 모든 부분을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5a96-135">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
