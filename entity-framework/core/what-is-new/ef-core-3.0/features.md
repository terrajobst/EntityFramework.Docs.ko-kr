---
title: EF Core 3.0의 새로운 기능 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 528733d6eec33de2c9538541a6ed5be704b9d433
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005563"
---
# <a name="new-features-included-in-ef-core-30"></a><span data-ttu-id="a41ec-102">EF Core 3.0에 포함된 새로운 기능</span><span class="sxs-lookup"><span data-stu-id="a41ec-102">New features included in EF Core 3.0</span></span>

<span data-ttu-id="a41ec-103">다음 목록에는 EF Core 3.0용으로 계획된 주요한 새 기능이 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-103">The following list includes the major new features planned for EF Core 3.0.</span></span>

<span data-ttu-id="a41ec-104">EF Core 3.0은 주요 릴리스이며, 기존 애플리케이션에 부정적인 영향을 줄 수 있는 API 개선에 해당하는 [주요 변경 사항](xref:core/what-is-new/ef-core-3.0/breaking-changes)이 다수 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-104">EF Core 3.0 is a major release and also contains numerous [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), which are API improvements that may have negative impact on existing applications.</span></span>  

## <a name="linq-improvements"></a><span data-ttu-id="a41ec-105">LINQ 기능 향상</span><span class="sxs-lookup"><span data-stu-id="a41ec-105">LINQ improvements</span></span> 

<span data-ttu-id="a41ec-106">LINQ를 사용하면 선택한 언어로 데이터베이스 쿼리를 작성할 수 있어서 다양한 종류의 정보를 활용하여 IntelliSense 및 컴파일 시간 형식 검사를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-106">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to offer IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="a41ec-107">그러나 LINQ를 사용하면 임의의 식(메서드 호출 또는 작업)을 포함하는 복잡한 쿼리를 무제한으로 작성할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-107">But LINQ also enables you to write an unlimited number of complicated queries containing arbitrary expressions (method calls or operations).</span></span>
<span data-ttu-id="a41ec-108">이러한 모든 조합을 처리하는 것은 LINQ 공급자에게 항상 중요한 도전 과제입니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-108">Handling all those combinations has always been a significant challenge for LINQ providers.</span></span>
<span data-ttu-id="a41ec-109">EF Core 3.0에서는 더 많은 식을 SQL로 변환하고, 더 많은 사례에서 효율적인 쿼리를 생성하며, 비효율적인 쿼리가 검색되지 않도록 하고, 기존 애플리케이션 및 데이터 공급자에 대해 호환성이 손상되는 변경 없이 새 쿼리 기능 및 성능 개선을 점차 더 쉽게 도입할 수 있도록 LINQ 구현을 다시 작성했습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-109">In EF Core 3.0, we've rewritten our LINQ implementation to enable translating more expressions into SQL, to generate efficient queries in more cases, to prevent inefficient queries from going undetected, and to make it easier for us to gradually introduce new query capabilities and performance improvementswithout breaking existing applications and data providers.</span></span>

### <a name="client-evaluation"></a><span data-ttu-id="a41ec-110">클라이언트 평가</span><span class="sxs-lookup"><span data-stu-id="a41ec-110">Client evaluation</span></span>

<span data-ttu-id="a41ec-111">EF Core 3.0의 주요 디자인 변경은 SQL 또는 매개 변수로 변환할 수 없는 LINQ 식을 처리하는 방식과 관련이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-111">The main design change in EF Core 3.0 has to do with how it handles LINQ expressions that it cannot translate to SQL or parameters:</span></span>

<span data-ttu-id="a41ec-112">처음 몇 가지 버전에서 EF Core는 SQL로 변환할 수 있는 쿼리 부분을 파악하여 클라이언트에서 나머지 쿼리를 실행했습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-112">In the first few versions, EF Core simply figured out what portions of a query could be translated to SQL, and executed the rest of the query on the client.</span></span>
<span data-ttu-id="a41ec-113">이러한 클라이언트 쪽 실행은 경우에 따라 바람직할 수 있지만 많은 경우 비효율적인 쿼리가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-113">This type of client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries.</span></span>
<span data-ttu-id="a41ec-114">예를 들어 EF Core 2.2가 `Where()` 호출에서 조건자를 변환할 수 없는 경우에는 필터가 없는 SQL 문을 실행하여 데이터베이스에서 모든 행을 읽은 다음 메모리 내에서 필터링합니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-114">For example, if EF Core 2.2 couldn't translate a predicate in a `Where()` call, it executed a SQL statement without a filter, read all all the rows from the database, and then filtered them in-memory.</span></span>
<span data-ttu-id="a41ec-115">이는 데이터베이스에 적은 수의 행이 포함된 경우에는 허용할 수 있지만 데이터베이스에 많은 수의 행이 포함된 경우에는 애플리케이션 오류 또는 성능 문제가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-115">That may be acceptable if the database contains a small number of rows, but can result in significant performance issues or even application failure if the database contains a large number or rows.</span></span>
<span data-ttu-id="a41ec-116">EF Core 3.0에서는 클라이언트 평가가 최상위 프로젝션(`Select()`에 대한 마지막 호출)에서만 수행되도록 제한했습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-116">In EF Core 3.0 we have restricted client evaluation to only happen on the top-level projection (the last call to `Select()`).</span></span>
<span data-ttu-id="a41ec-117">EF Core 3.0이 쿼리의 다른 위치에서 변환할 수 없는 식을 감지하면 런타임 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-117">When EF Core 3.0 detects expressions that cannot be translated anywhere else in the query, it throws a runtime exception.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="a41ec-118">Cosmos DB 지원</span><span class="sxs-lookup"><span data-stu-id="a41ec-118">Cosmos DB support</span></span> 

<span data-ttu-id="a41ec-119">EF Core의 Cosmos DB 공급자는 EF 프로그래밍 모델에 친숙한 개발자가 애플리케이션 데이터베이스로서 Azure Cosmos DB를 쉽게 대상으로 지정할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-119">The Cosmos DB provider for EF Core enables developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="a41ec-120">목표는 글로벌 배포, "always on" 가용성, 탄력적인 확장성, 짧은 대기 시간, .NET 개발자에 훨씬 더 많은 액세스 가능성과 같은 몇몇 Cosmos DB의 장점을 활용하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-120">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="a41ec-121">공급자는 Cosmos DB의 SQL API에 대해 자동 변경 내용 추적, LINQ, 값 변환 같은 대부분의 EF Core 기능을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-121">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="a41ec-122">C# 8.0 지원</span><span class="sxs-lookup"><span data-stu-id="a41ec-122">C# 8.0 support</span></span>

<span data-ttu-id="a41ec-123">EF Core 3.0은 C# 8.0의 몇 가지 새로운 기능을 활용합니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-123">EF Core 3.0 takes advantage of some of the new features in C# 8.0:</span></span>

### <a name="asynchronous-streams"></a><span data-ttu-id="a41ec-124">비동기 스트림</span><span class="sxs-lookup"><span data-stu-id="a41ec-124">Asynchronous streams</span></span>

<span data-ttu-id="a41ec-125">이제 비동기 쿼리 결과는 새 표준 `IAsyncEnumerable<T>` 인터페이스를 사용하여 노출되며 `await foreach`를 사용하여 이용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-125">Asynchronous query results are now exposed using the new standard `IAsyncEnumerable<T>` interface and can be consumed using `await foreach`.</span></span>

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Proccess(o);
} 
```

### <a name="nullable-reference-types"></a><span data-ttu-id="a41ec-126">nullable 참조 형식</span><span class="sxs-lookup"><span data-stu-id="a41ec-126">Nullable reference types</span></span> 

<span data-ttu-id="a41ec-127">EF Core 코드에서 이 새로운 기능을 사용하는 경우 EF Core는 참조 형식(문자열 또는 탐색 속성과 같은 기본 형식)의 속성에 대한 null 허용 여부를 판단하여 데이터베이스에서 열과 관계의 null 허용 여부를 결정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-127">When this new feature is enabled in your code, EF Core can reason about the nullability of properties of refrence types (either of primitive types like string or navigation properties) to decide the nullability of columns and relationships in the database.</span></span>

## <a name="interception"></a><span data-ttu-id="a41ec-128">Interception</span><span class="sxs-lookup"><span data-stu-id="a41ec-128">Interception</span></span>

<span data-ttu-id="a41ec-129">EF Core 3.0의 새로운 인터셉션 API를 사용하면 연결 열기, 트랜잭션 초기화 및 명령 실행과 같은 일반적인 EF Core 작업의 일부로 발생하는 하위 수준 데이터베이스 작업의 결과를 프로그래밍 방식으로 관찰하고 수정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-129">The new interception API in EF Core 3.0 allows programatically observing and modifying the outcome of low-level database operations that occur as part of the normal operation of EF Core, such as opening connections, initating transactions, and executing commands.</span></span> 

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="a41ec-130">데이터베이스 뷰의 리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="a41ec-130">Reverse engineering of database views</span></span>

<span data-ttu-id="a41ec-131">키가 없는 엔터티 형식(이전에는 [쿼리 형식](xref:core/modeling/query-types)으로 알려짐)은 데이터베이스에서 읽을 수는 있지만 업데이트할 수 없는 데이터를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-131">Entity types without keys (previously known as [query types](xref:core/modeling/query-types)) represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="a41ec-132">이 특성은 대부분의 시나리오에서 데이터베이스 뷰 매핑에 매우 적합하므로 데이터베이스 뷰를 리버스 엔지니어링할 때 키 없이 엔터티 형식을 생성할 수 있도록 자동화했습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-132">This characteristic makes them an excellent fit for mapping database views in most scenarios, so we automated the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="a41ec-133">보안 주체와 테이블을 공유하는 종속 엔터티는 이제 선택 사항입니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-133">Dependent entities sharing the table with the principal are now optional</span></span>

<span data-ttu-id="a41ec-134">EF Core 3.0부터 `OrderDetails`가 `Order`에 소유되거나 같은 테이블에 명시적으로 매핑된 경우 `OrderDetails` 없이 `Order`를 추가할 수 있으며 기본 키를 제외하고 모든 `OrderDetails` 속성이 Null 허용 열에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-134">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="a41ec-135">쿼리 시 EF Core는 해당 필수 속성에 값이 없거나 기본 키 이외의 필수 속성이 없고 모든 속성이 `null`이면 `OrderDetails`를 `null`로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-135">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

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

## <a name="ef-63-on-net-core"></a><span data-ttu-id="a41ec-136">.NET Core의 EF 6.3</span><span class="sxs-lookup"><span data-stu-id="a41ec-136">EF 6.3 on .NET Core</span></span>

<span data-ttu-id="a41ec-137">많은 기존 애플리케이션이 이전 버전의 EF를 사용하고 단지 .NET Core를 활용하기 위해 이전 버전의 EF를 EF Core로 포팅하는 일은 상당한 노력이 필요할 수 있다는 점을 이해합니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-137">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="a41ec-138">그러한 이유로 Microsoft는 .NET Core 3.0에서 최신 버전의 EF 6을 실행할 수 있도록 했습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-138">For that reason, we have enabled the newewst version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="a41ec-139">다음과 같은 몇 가지 제한 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-139">There are some limitations, for example:</span></span>
- <span data-ttu-id="a41ec-140">새 공급자는 .NET Core에서 작업을 수행해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-140">New providers are required to work on .NET Core</span></span>
- <span data-ttu-id="a41ec-141">SQL Server를 사용한 공간 지원은 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-141">Spatial support with SQL Server won't be enabled</span></span>

## <a name="postponed-features"></a><span data-ttu-id="a41ec-142">연기된 기능</span><span class="sxs-lookup"><span data-stu-id="a41ec-142">Postponed features</span></span>

<span data-ttu-id="a41ec-143">원래 EF Core 3.0에 대해 계획되었던 일부 기능은 이후 릴리스로 연기되었습니다.</span><span class="sxs-lookup"><span data-stu-id="a41ec-143">Some features originally planned for EF Core 3.0 were postponed to future releases:</span></span> 

- <span data-ttu-id="a41ec-144">마이그레이션에서 모델의 일부를 무시하는 기능([#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725)로 추적).</span><span class="sxs-lookup"><span data-stu-id="a41ec-144">Ability to ingore parts of a model in migrations, tracked by [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span></span>
- <span data-ttu-id="a41ec-145">두 가지 문제로 추적되는 속성 모음 엔터티(공유 형식 엔터티에 대한 [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) 및 인덱싱된 속성 매핑 지원에 대한 [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610)).</span><span class="sxs-lookup"><span data-stu-id="a41ec-145">Property bag entities, tracked by two separate issues: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) about shared-type entities and [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) about indexed property mapping support.</span></span>
