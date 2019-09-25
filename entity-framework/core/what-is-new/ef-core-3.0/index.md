---
title: Entity Framework Core 3.0의 새로운 기능
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/index
ms.openlocfilehash: ebebbf286d9d8e06492a3c664799e1127c7dc8c0
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197914"
---
# <a name="new-features-in-entity-framework-core-30"></a><span data-ttu-id="547d0-102">Entity Framework Core 3.0의 새로운 기능</span><span class="sxs-lookup"><span data-stu-id="547d0-102">New features in Entity Framework Core 3.0</span></span>

<span data-ttu-id="547d0-103">다음 목록에는 EF Core 3.0의 주요한 새 기능이 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-103">The following list includes the major new features in EF Core 3.0.</span></span>

<span data-ttu-id="547d0-104">EF Core 3.0은 주요 릴리스이며, 기존 애플리케이션에 부정적인 영향을 줄 수 있는 API 개선에 해당하는 [주요 변경 사항](xref:core/what-is-new/ef-core-3.0/breaking-changes)이 다수 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-104">As a major release, EF Core 3.0 also contains several [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), which are API improvements that may have negative impact on existing applications.</span></span>  

## <a name="linq-overhaul"></a><span data-ttu-id="547d0-105">LINQ 정비</span><span class="sxs-lookup"><span data-stu-id="547d0-105">LINQ overhaul</span></span> 

<span data-ttu-id="547d0-106">LINQ를 사용하면 선택한 .NET 언어로 데이터베이스 쿼리를 작성할 수 있어서 다양한 종류의 정보를 활용하여 IntelliSense 및 컴파일 시간 형식 검사를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-106">LINQ enables you to write database queries using your .NET language of choice, taking advantage of rich type information to offer IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="547d0-107">그러나 LINQ를 사용하면 임의의 식(메서드 호출 또는 작업)을 포함하는 복잡한 쿼리를 무제한으로 작성할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-107">But LINQ also enables you to write an unlimited number of complicated queries containing arbitrary expressions (method calls or operations).</span></span>
<span data-ttu-id="547d0-108">이러한 모든 조합을 처리하는 방법은 LINQ 공급자의 주요 과제입니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-108">How to handle all those combinations is the main challenge for LINQ providers.</span></span>

<span data-ttu-id="547d0-109">EF Core 3.0에서는 더 많은 쿼리 패턴을 SQL로 변환하고, 더 많은 사례에서 효율적인 쿼리를 생성하고, 비효율적인 쿼리가 검색되지 않도록 하는 데 사용할 수 있도록 LINQ 공급자를 다시 설계했습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-109">In EF Core 3.0, we rearchitected our LINQ provider to enable translating more query patterns into SQL, generating efficient queries in more cases, and preventing inefficient queries from going undetected.</span></span> <span data-ttu-id="547d0-110">새 LINQ 공급자는 기존 애플리케이션 및 데이터 공급자를 중단시키지 않고 향후 릴리스에서 새로운 쿼리 기능 및 성능 향상을 제공할 수 있는 기반이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-110">The new LINQ provider is the foundation over which we'll be able to offer new query capabilities and performance improvements in future releases, without breaking existing applications and data providers.</span></span>

### <a name="restricted-client-evaluation"></a><span data-ttu-id="547d0-111">클라이언트 평가 제한</span><span class="sxs-lookup"><span data-stu-id="547d0-111">Restricted client evaluation</span></span>
<span data-ttu-id="547d0-112">가장 중요한 설계 변경은 매개 변수로 변환하거나 SQL로 변환할 수 없는 LINQ 식을 처리하는 방법과 관련이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-112">The most important design change has to do with how we handle LINQ expressions that cannot be converted to parameters or translated to SQL.</span></span>

<span data-ttu-id="547d0-113">이전 버전에서 EF Core는 SQL로 변환할 수 있는 쿼리 부분을 파악하여 클라이언트에서 나머지 쿼리를 실행했습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-113">In previous versions, EF Core identified what portions of a query could be translated to SQL, and executed the rest of the query on the client.</span></span>
<span data-ttu-id="547d0-114">이러한 클라이언트 쪽 실행은 경우에 따라 바람직할 수 있지만 많은 경우 비효율적인 쿼리가 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-114">This type of client-side execution is desirable in some situations, but in many other cases it can result in inefficient queries.</span></span>

<span data-ttu-id="547d0-115">예를 들어 EF Core 2.2가 `Where()` 호출에서 조건자를 변환할 수 없는 경우에는 필터가 없는 SQL 문을 실행하여 데이터베이스에서 모든 행을 전송한 다음 메모리 내에서 필터링합니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-115">For example, if EF Core 2.2 couldn't translate a predicate in a `Where()` call, it executed an SQL statement without a filter, transferred all the rows from the database, and then filtered them in-memory:</span></span>

``` csharp
var specialCustomers = 
  context.Customers
    .Where(c => c.Name.StartsWith(n) && IsSpecialCustomer(c));
```

<span data-ttu-id="547d0-116">이는 데이터베이스에 적은 수의 행이 포함된 경우에는 허용할 수 있지만 데이터베이스에 많은 수의 행이 포함된 경우에는 애플리케이션 오류 또는 성능 문제가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-116">That may be acceptable if the database contains a small number of rows but can result in significant performance issues or even application failure if the database contains a large number or rows.</span></span>

<span data-ttu-id="547d0-117">EF Core 3.0에서는 클라이언트 평가가 최상위 프로젝션(`Select()`에 대한 마지막 호출)에서만 수행되도록 제한했습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-117">In EF Core 3.0, we've restricted client evaluation to only happen on the top-level projection (essentially, the last call to `Select()`).</span></span>
<span data-ttu-id="547d0-118">EF Core 3.0이 쿼리의 다른 위치에서 변환할 수 없는 식을 감지하면 런타임 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-118">When EF Core 3.0 detects expressions that can't be translated anywhere else in the query, it throws a runtime exception.</span></span>

<span data-ttu-id="547d0-119">이전 예제와 같이 클라이언트에서 조건자 조건을 평가하려면 개발자는 이제 쿼리 평가를 LINQ to Objects로 명시적으로 전환해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-119">To evaluate a predicate condition on the client as in the previous example, developers now need to explicitly switch evaluation of the query to LINQ to Objects:</span></span> 

``` csharp
var specialCustomers =
  context.Customers
    .Where(c => c.Name.StartsWith(n)) 
    .AsEnumerable() // switches to LINQ to Objects
    .Where(c => IsSpecialCustomer(c));
```

<span data-ttu-id="547d0-120">기존 애플리케이션에 영향을 줄 수 있는 방법에 대한 자세한 내용은[ 주요 변경 사항 설명서](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="547d0-120">See the [breaking changes documentation](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) for more details about how this can affect existing applications.</span></span>

### <a name="single-sql-statement-per-linq-query"></a><span data-ttu-id="547d0-121">LINQ 쿼리당 단일 SQL 문</span><span class="sxs-lookup"><span data-stu-id="547d0-121">Single SQL statement per LINQ query</span></span>

<span data-ttu-id="547d0-122">3\.0에서 변경된 설계의 또 다른 중요한 점은 이제 항상 LINQ 쿼리당 단일 SQL 문을 생성한다는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-122">Another aspect of the design that changed significantly in 3.0 is that we now always generate a single SQL statement per LINQ query.</span></span> <span data-ttu-id="547d0-123">이전 버전에서는 컬렉션 탐색 속성에 대한 `Include()` 호출을 변환하고 하위 쿼리를 사용하여 특정 패턴을 따르는 쿼리를 변환하는 등과 같은 특정 경우에 여러 SQL 문을 생성했습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-123">In previous versions, we used to generate multiple SQL statements in certain cases, like to translate `Include()` calls on collection navigation properties and to translate queries that followed certain patterns with subqueries.</span></span> <span data-ttu-id="547d0-124">이는 어떤 경우에는 편리하고 `Include()`의 경우 네트워크를 통해 중복 데이터를 전송하지 않도록 하는 데 도움이 되지만, 구현이 복잡하고 이로 인해 매우 비효율적인 동작(N+1개의 쿼리)이 발생하며 여러 쿼리에서 반환되는 데이터가 일치하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-124">Although this was in some cases convenient, and for `Include()` it even helped avoid sending redundant data over the wire, the implementation was complex, it resulted in some extremely inefficient behaviors (N+1 queries), and there was situations in which the data returned across multiple queries could be inconsistent.</span></span>

<span data-ttu-id="547d0-125">클라이언트 평가와 마찬가지로 EF Core 3.0이 LINQ 쿼리를 단일 SQL 문으로 변환할 수 없는 경우 런타임 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-125">Similarly to client evaluation, if EF Core 3.0 can't translate a LINQ query into a single SQL statement, it throws a runtime exception.</span></span> <span data-ttu-id="547d0-126">그러나 EF Core는 여러 쿼리를 생성하는 데 사용된 많은 공통 패턴을 JOIN을 통해 단일 쿼리로 변환할 수 있도록 설계되었습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-126">But we made EF Core capable of translating many of the common patterns that used to generate multiple queries to a single query with JOINs.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="547d0-127">Cosmos DB 지원</span><span class="sxs-lookup"><span data-stu-id="547d0-127">Cosmos DB support</span></span> 

<span data-ttu-id="547d0-128">EF Core의 Cosmos DB 공급자는 EF 프로그래밍 모델에 친숙한 개발자가 애플리케이션 데이터베이스로서 Azure Cosmos DB를 쉽게 대상으로 지정할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-128">The Cosmos DB provider for EF Core enables developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span> <span data-ttu-id="547d0-129">목표는 글로벌 배포, "always on" 가용성, 탄력적인 확장성, 짧은 대기 시간, .NET 개발자에 훨씬 더 많은 액세스 가능성과 같은 몇몇 Cosmos DB의 장점을 활용하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-129">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span> <span data-ttu-id="547d0-130">공급자는 Cosmos DB의 SQL API에 대해 자동 변경 내용 추적, LINQ, 값 변환 같은 대부분의 EF Core 기능을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-130">The provider enables most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>

<span data-ttu-id="547d0-131">자세한 내용은 [Cosmos DB 공급자 설명서](xref:core/providers/cosmos/index)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="547d0-131">See the [Cosmos DB provider documentation](xref:core/providers/cosmos/index) for more details.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="547d0-132">C# 8.0 지원</span><span class="sxs-lookup"><span data-stu-id="547d0-132">C# 8.0 support</span></span>

<span data-ttu-id="547d0-133">EF Core 3.0은 [C# 8.0의 몇 가지 새로운 기능](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8)을 활용합니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-133">EF Core 3.0 takes advantage of a couple of the [new features in C# 8.0](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8):</span></span>

### <a name="asynchronous-streams"></a><span data-ttu-id="547d0-134">비동기 스트림</span><span class="sxs-lookup"><span data-stu-id="547d0-134">Asynchronous streams</span></span>

<span data-ttu-id="547d0-135">이제 비동기 쿼리 결과는 새 표준 `IAsyncEnumerable<T>` 인터페이스를 사용하여 노출되며 `await foreach`를 사용하여 이용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-135">Asynchronous query results are now exposed using the new standard `IAsyncEnumerable<T>` interface and can be consumed using `await foreach`.</span></span>

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Process(o);
} 
```

<span data-ttu-id="547d0-136">자세한 내용은 [C# 설명서의 비동기 스트림](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="547d0-136">See the [asynchronous streams in the C# documentation](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams) for more details.</span></span>

### <a name="nullable-reference-types"></a><span data-ttu-id="547d0-137">nullable 참조 형식</span><span class="sxs-lookup"><span data-stu-id="547d0-137">Nullable reference types</span></span> 

<span data-ttu-id="547d0-138">코드에서 이 새로운 기능을 사용하도록 설정하면 EF Core가 참조 형식 속성의 null 허용 여부를 검사하여 데이터베이스의 해당 열과 관계에 적용합니다. nullable이 아닌 참조 형식의 속성은 `[Required]` 데이터 주석 특성인 것처럼 처리됩니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-138">When this new feature is enabled in your code, EF Core examines the nullability of reference type properties and applies it to corresponding columns and relationships in the database: properties of non-nullable references types are treated as if they had the `[Required]` data annotation attribute.</span></span>

<span data-ttu-id="547d0-139">예를 들어 다음 클래스에서 `string?` 형식으로 표시된 속성은 선택 사항으로 구성되고 `string` 형식은 필수적으로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-139">For example, in the following class, properties marked as of type `string?` will be configured as optional, whereas `string` will be configured as required:</span></span>

``` csharp
public class Customer
{
  public int Id { get; set; }
  public string FirstName { get; set; }
  public string LastName { get; set; }
  public string? MiddleName { get; set; }
}
```

<span data-ttu-id="547d0-140">자세한 내용은 EF Core 설명서의 [nullable 참조 형식 작업](xref:core/miscellaneous/nullable-reference-types)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="547d0-140">See [Working with nullable reference types](xref:core/miscellaneous/nullable-reference-types) in the EF Core documentation for more details.</span></span>

## <a name="interception-of-database-operations"></a><span data-ttu-id="547d0-141">데이터베이스 작업 인터셉션</span><span class="sxs-lookup"><span data-stu-id="547d0-141">Interception of database operations</span></span>

<span data-ttu-id="547d0-142">EF Core 3.0의 새로운 인터셉션 API를 통해 하위 수준 데이터베이스 작업이 EF Core 기본 작업의 일부로 발생할 때마다 자동으로 호출되는 사용자 지정 논리를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-142">The new interception API in EF Core 3.0 allows providing custom logic to be invoked automatically whenever low-level database operations occur as part of the normal operation of EF Core.</span></span> <span data-ttu-id="547d0-143">예를 들어 연결을 열거나, 트랜잭션을 커밋하거나, 명령을 실행하는 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-143">For example, when opening connections, committing transactions, or executing commands.</span></span> 

<span data-ttu-id="547d0-144">EF 6에 있던 인터셉션 기능과 마찬가지로 인터셉터를 사용하면 작업을 수행하기 전이나 후에 작업을 가로챌 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-144">Similarly to the interception features that existed in EF 6, interceptors allow you to intercept operations before or after they happen.</span></span> <span data-ttu-id="547d0-145">이러한 이벤트가 발생하기 전에 가로챌 때 실행을 무시하고 인터셉션 논리에서 대체 결과를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-145">When you intercept them before they happen, you are allowed to by-pass execution and supply alternate results from the interception logic.</span></span> 

<span data-ttu-id="547d0-146">예를 들어 명령 텍스트를 조작하기 위해 `IDbCommandInterceptor`를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-146">For example, to manipulate command text, you can create an `IDbCommandInterceptor`:</span></span>

``` csharp 
public class HintCommandInterceptor : DbCommandInterceptor
{
  public override InterceptionResult ReaderExecuting(
    DbCommand command, 
    CommandEventData eventData, 
    InterceptionResult result)
  {
    // Manipulate the command text, etc. here...
    command.CommandText += " OPTION (OPTIMIZE FOR UNKNOWN)";
    return result;
  }
}
``` 

<span data-ttu-id="547d0-147"> `DbContext`를 통해 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-147">And register it with your `DbContext`:</span></span>

``` csharp
services.AddDbContext(b => b
  .UseSqlServer(connectionString)
  .AddInterceptors(new HintCommandInterceptor()));
```

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="547d0-148">데이터베이스 뷰의 리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="547d0-148">Reverse engineering of database views</span></span>

<span data-ttu-id="547d0-149">데이터베이스에서 읽을 수 있지만 업데이트할 수 없는 데이터를 나타내는 쿼리 형식은 [키 없는 엔터티 형식](xref:core/modeling/keyless-entity-types)으로 이름이 변경되었습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-149">Query types, which represent data that can be read from the database but not updated, have been renamed to [keyless entity types](xref:core/modeling/keyless-entity-types).</span></span> <span data-ttu-id="547d0-150">대부분의 시나리오에서 데이터베이스 뷰를 매핑하는 데 매우 적합하므로 EF Core는 이제 데이터베이스를 리버스 엔지니어링할 때 키 없는 엔터티 형식을 자동으로 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-150">As they are an excellent fit for mapping database views in most scenarios, EF Core now automatically creates keyless entity types when reverse engineering database views.</span></span>

<span data-ttu-id="547d0-151">예를 들어 [dotnet ef 명령줄 도구](xref:core/miscellaneous/cli/dotnet)를 사용하여 다음을 입력할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-151">For example, using the [dotnet ef command-line tool](xref:core/miscellaneous/cli/dotnet) you can type:</span></span>

``` console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="547d0-152">그러면 도구는 다음과 같이 키 없이 뷰 및 테이블에 대한 형식을 자동으로 스캐폴드합니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-152">And the tool will now automatically scaffold types for views and tables without keys:</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
  modelBuilder.Entity<Names>(entity =>
  {
    entity.HasNoKey();
    entity.ToView("Names");
  });

  modelBuilder.Entity<Things>(entity =>
  {
    entity.HasNoKey();
  });
}
```

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="547d0-153">보안 주체와 테이블을 공유하는 종속 엔터티는 이제 선택 사항입니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-153">Dependent entities sharing the table with the principal are now optional</span></span>

<span data-ttu-id="547d0-154">EF Core 3.0부터 `OrderDetails`가 `Order`에 소유되거나 같은 테이블에 명시적으로 매핑된 경우 `OrderDetails` 없이 `Order`를 추가할 수 있으며 기본 키를 제외하고 모든 `OrderDetails` 속성이 Null 허용 열에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-154">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="547d0-155">쿼리 시 EF Core는 해당 필수 속성에 값이 없거나 기본 키 이외의 필수 속성이 없고 모든 속성이 `null`이면 `OrderDetails`를 `null`로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-155">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

## <a name="ef-63-on-net-core"></a><span data-ttu-id="547d0-156">.NET Core의 EF 6.3</span><span class="sxs-lookup"><span data-stu-id="547d0-156">EF 6.3 on .NET Core</span></span>

<span data-ttu-id="547d0-157">이는 EF Core 3.0 기능이 아니지만 대부분의 기존 고객에게 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-157">This isn't really an EF Core 3.0 feature, but we think it is important to many of our current customers.</span></span> 

<span data-ttu-id="547d0-158">많은 기존 애플리케이션이 이전 버전의 EF를 사용하고 단지 .NET Core를 활용하기 위해 이전 버전의 EF를 EF Core로 이식하는 일은 상당한 노력이 필요할 수 있다는 점을 이해합니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-158">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can require a significant effort.</span></span> <span data-ttu-id="547d0-159">그러한 이유로 Microsoft는 .NET Core 3.0에서 최신 버전의 EF 6를 포팅하기로 결정했습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-159">For that reason, we decided to port the newest version of EF 6 to run on .NET Core 3.0.</span></span> 

<span data-ttu-id="547d0-160">자세한 내용은 [EF 6의 새로운 기능](xref:ef6/what-is-new/index)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="547d0-160">For more details, see [what's new in EF 6](xref:ef6/what-is-new/index).</span></span>

## <a name="postponed-features"></a><span data-ttu-id="547d0-161">연기된 기능</span><span class="sxs-lookup"><span data-stu-id="547d0-161">Postponed features</span></span>

<span data-ttu-id="547d0-162">원래 EF Core 3.0에 대해 계획되었던 일부 기능은 이후 릴리스로 연기되었습니다.</span><span class="sxs-lookup"><span data-stu-id="547d0-162">Some features originally planned for EF Core 3.0 were postponed to future releases:</span></span>

- <span data-ttu-id="547d0-163">마이그레이션에서 모델의 일부를 무시하는 기능([#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725)로 추적).</span><span class="sxs-lookup"><span data-stu-id="547d0-163">Ability to ignore parts of a model in migrations, tracked as [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span></span>
- <span data-ttu-id="547d0-164">두 가지 문제로 추적되는 속성 모음 엔터티(공유 형식 엔터티에 대한 [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) 및 인덱싱된 속성 매핑 지원에 대한 [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610)).</span><span class="sxs-lookup"><span data-stu-id="547d0-164">Property bag entities, tracked as two separate issues: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) about shared-type entities and [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) about indexed property mapping support.</span></span>
