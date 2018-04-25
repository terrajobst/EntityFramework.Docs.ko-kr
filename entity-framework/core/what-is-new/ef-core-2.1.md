---
title: EF Core 2.1의 새로운 기능 - EF Core
author: divega
ms.author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: db1648095aa4d612af53f4e10a30be36edc40da5
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2018
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="efa50-102">EF Core 2.1의 새로운 기능</span><span class="sxs-lookup"><span data-stu-id="efa50-102">New features in EF Core 2.1</span></span>
> [!NOTE]  
> <span data-ttu-id="efa50-103">이 릴리스는 아직 미리 보기 상태입니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-103">This release is still in preview.</span></span>

<span data-ttu-id="efa50-104">수많은 버그 수정 및 작은 기능 및 성능 개선 사항 외에도 EF Core 2.1에는 몇 가지 놀라운 기능이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-104">Besides numerous bug fixes and small functional and performance enhancements, EF Core 2.1 includes some compelling new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="efa50-105">지연 로드</span><span class="sxs-lookup"><span data-stu-id="efa50-105">Lazy loading</span></span>
<span data-ttu-id="efa50-106">이제 EF Core에는 요청 시 해당 탐색 속성을 로드할 수 있는 엔터티 클래스를 작성하는 사용자에게 필요한 구성 요소가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-106">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="efa50-107">또한 Microsoft.EntityFrameworkCore.Proxies라는 새 패키지를 만들었습니다. 여기서는 해당 구성 요소를 활용하여 최소한으로 수정된 엔터티 클래스(예: 가상 탐색 속성이 포함된 클래스)를 기반으로 지연된 로드 프록시 클래스를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-107">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (e.g. classes with virtual navigation properties).</span></span>

<span data-ttu-id="efa50-108">이 항목에 대한 자세한 내용은 [지연된 로드에 대한 섹션](xref:core/querying/related-data#lazy-loading)을 참고하세요.</span><span class="sxs-lookup"><span data-stu-id="efa50-108">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="efa50-109">엔터티 생성자의 매개 변수</span><span class="sxs-lookup"><span data-stu-id="efa50-109">Parameters in entity constructors</span></span>
<span data-ttu-id="efa50-110">지연된 로드의 필수 구성 요소 중 하나로 해당 생성자에서 매개 변수를 사용하는 엔터티를 생성하도록 설정했습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-110">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="efa50-111">매개 변수를 사용하여 속성 값, 지연된 로드 대리자 및 서비스를 삽입할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-111">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="efa50-112">이 항목에 대한 자세한 내용은 [매개 변수가 포함된 엔터티 생성자에 대한 섹션](xref:core/modeling/constructors)을 참고하세요.</span><span class="sxs-lookup"><span data-stu-id="efa50-112">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="efa50-113">값 변환</span><span class="sxs-lookup"><span data-stu-id="efa50-113">Value conversions</span></span>
<span data-ttu-id="efa50-114">지금까지 EF Core는 기본 데이터베이스 공급자가 고유하게 지원하는 형식의 속성만을 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-114">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="efa50-115">값은 변환 없이 열과 속성 간에 복사되었습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-115">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="efa50-116">EF Core 2.1부터 값 변환은 속성에 적용되기 전에 열에서 가져온 값을 변환하는 데 적용하거나 반대로 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-116">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="efa50-117">열과 속성 간의 사용자 지정 변환을 등록하도록 허용하는 명시적 구성 API뿐만 아니라 필요에 따라 다양한 규칙에서 적용할 수 있는 변환이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-117">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="efa50-118">이 기능의 응용 프로그램은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-118">Some of the application of this feature are:</span></span>

- <span data-ttu-id="efa50-119">문자열로 열거형 저장</span><span class="sxs-lookup"><span data-stu-id="efa50-119">Storing enums as strings</span></span>
- <span data-ttu-id="efa50-120">SQL Server를 사용하여 부호 없는 정수 매핑</span><span class="sxs-lookup"><span data-stu-id="efa50-120">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="efa50-121">속성 값의 자동 암호화 및 암호 해독</span><span class="sxs-lookup"><span data-stu-id="efa50-121">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="efa50-122">이 항목에 대한 자세한 내용은 [값 변환에 대한 섹션](xref:core/modeling/value-conversions)을 참고하세요.</span><span class="sxs-lookup"><span data-stu-id="efa50-122">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="efa50-123">LINQ GroupBy 변환</span><span class="sxs-lookup"><span data-stu-id="efa50-123">LINQ GroupBy translation</span></span>
<span data-ttu-id="efa50-124">EF Core 버전 2.1 이전에 메모리에서 GroupBy LINQ 연산자를 평가할 수 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-124">Before version 2.1, in EF Core the GroupBy LINQ operator was always be evaluated in memory.</span></span> <span data-ttu-id="efa50-125">이제 대부분의 일반적인 경우에 SQL GROUP BY 절로 변환하도록 지원됩니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-125">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="efa50-126">이 예제에서는 다양한 집계 함수를 계산하는 데 사용되는 GroupBy를 포함하는 쿼리를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-126">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

``` csharp
var query = context.Orders
    .GroupBy(o => new { o.CustomerId, o.EmployeeId })
    .Select(g => new
        {
          g.Key.CustomerId,
          g.Key.EmployeeId,
          Sum = g.Sum(o => o.Amount),
          Min = g.Min(o => o.Amount),
          Max = g.Max(o => o.Amount),
          Avg = g.Average(o => Amount)
        });
```

<span data-ttu-id="efa50-127">해당 SQL 변환은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-127">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="efa50-128">데이터 시드</span><span class="sxs-lookup"><span data-stu-id="efa50-128">Data Seeding</span></span>
<span data-ttu-id="efa50-129">새 릴리스에서 초기 데이터를 제공하여 데이터베이스를 채울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-129">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="efa50-130">EF6과 달리 데이터 시드는 모델 구성의 일부로 엔터티 형식에 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-130">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="efa50-131">그런 다음, EF Core 마이그레이션은 데이터베이스를 모델의 새 버전으로 업그레이드하는 경우 삽입, 업데이트 또는 삭제 작업을 적용해야 하는 경우를 자동으로 계산할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-131">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="efa50-132">예를 들어 이를 사용하여 `OnModelCreating`에서 게시물의 시드 데이터를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-132">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="efa50-133">이 항목에 대한 자세한 내용은 [데이터 시드에 대한 섹션](xref:core/modeling/data-seeding)을 참고하세요.</span><span class="sxs-lookup"><span data-stu-id="efa50-133">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="efa50-134">쿼리 형식</span><span class="sxs-lookup"><span data-stu-id="efa50-134">Query types</span></span>
<span data-ttu-id="efa50-135">이제 EF Core 모델에는 쿼리 형식이 포함될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-135">An EF Core model can now include query types.</span></span> <span data-ttu-id="efa50-136">엔터티 형식과 달리 쿼리 형식에는 정의된 키가 없고, 삽입, 삭제 또는 업데이트될 수 없습니다(즉, 읽기 전용임). 하지만 쿼리에 의해 직접 반환될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-136">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (i.e. they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="efa50-137">쿼리 형식의 사용 시나리오는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-137">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="efa50-138">기본 키가 없이 보기에 매핑</span><span class="sxs-lookup"><span data-stu-id="efa50-138">Mapping to views without primary keys</span></span>
- <span data-ttu-id="efa50-139">기본 키가 없이 테이블에 매핑</span><span class="sxs-lookup"><span data-stu-id="efa50-139">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="efa50-140">모델에서 정의된 쿼리에 매핑</span><span class="sxs-lookup"><span data-stu-id="efa50-140">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="efa50-141">`FromSql()` 쿼리에 대한 반환 형식으로 제공</span><span class="sxs-lookup"><span data-stu-id="efa50-141">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="efa50-142">이 항목에 대한 자세한 내용은 [쿼리 형식에 대한 섹션](xref:core/modeling/query-types)을 참고하세요.</span><span class="sxs-lookup"><span data-stu-id="efa50-142">Read the [section on query types](xref:core/modeling/query-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="efa50-143">파생된 형식에 포함</span><span class="sxs-lookup"><span data-stu-id="efa50-143">Include for derived types</span></span>
<span data-ttu-id="efa50-144">이제 `Include` 메서드를 작성할 때 파생된 형식에서 정의된 탐색 속성만을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-144">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="efa50-145">강력한 형식의 `Include` 버전에서 명시적 캐스트 또는 `as` 연산자를 사용하도록 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-145">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="efa50-146">또한 이제 문자열 `Include` 버전의 파생된 형식에서 정의된 탐색 속성의 이름을 참조하도록 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-146">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="efa50-147">이 항목에 대한 자세한 내용은 [파생된 형식에서 포함된 항목에 대한 섹션](xref:core/querying/related-data#include-on-derived-types)을 참고하세요.</span><span class="sxs-lookup"><span data-stu-id="efa50-147">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="efa50-148">System.Transactions 지원</span><span class="sxs-lookup"><span data-stu-id="efa50-148">System.Transactions support</span></span>
<span data-ttu-id="efa50-149">TransactionScope 등의 System.Transactions 기능에서 작동하는 기능을 추가했습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-149">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="efa50-150">이 기능은 지원하는 데이터베이스 공급자를 사용하는 경우 .NET Core와.NET Framework에서 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-150">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="efa50-151">이 항목에 대한 자세한 내용은 [System.Transactions에 대한 섹션](xref:core/saving/transactions#using-systemtransactions)을 참고하세요.</span><span class="sxs-lookup"><span data-stu-id="efa50-151">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="efa50-152">초기 마이그레이션에서 열 순서 개선</span><span class="sxs-lookup"><span data-stu-id="efa50-152">Better column ordering in initial migration</span></span>
<span data-ttu-id="efa50-153">고객의 의견에 따라 처음부터 클래스에 선언된 속성과 동일한 순서대로 테이블의 열을 생성하도록 마이그레이션을 업데이트했습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-153">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="efa50-154">초기 테이블 작성 후에 새 멤버를 추가할 경우 EF Core는 순서를 변경할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-154">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="efa50-155">상관 관계 있는 하위 쿼리의 최적화</span><span class="sxs-lookup"><span data-stu-id="efa50-155">Optimization of correlated subqueries</span></span>
<span data-ttu-id="efa50-156">여러 가지 일반적인 시나리오에서 "N + 1" SQL 쿼리를 실행하지 않도록 이 쿼리 변환을 개선했습니다. 여기서는 프로젝션에서 탐색 속성의 사용량에 따라 상관 관계 있는 하위 쿼리의 데이터를 사용하여 루트 쿼리의 데이터를 조인하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-156">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="efa50-157">최적화에는 결과가 하위 쿼리를 형성하는 버퍼링이 필요하고 쿼리를 수정하여 새로운 동작을 옵트인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-157">The optimization requires buffering the results form the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="efa50-158">예를 들어 다음 쿼리는 정상적으로 고객에 대한 쿼리로 변환됩니다. 또한 N("N"은 반환된 고객의 수)은 순서에 대한 쿼리를 구분합니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-158">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="efa50-159">적절한 위치에 `ToList()`를 포함하여 버퍼링이 순서에 적합함을 나타냅니다. 그러면 최적화를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-159">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="efa50-160">이 쿼리를 두 개의 SQL 쿼리로 변환할 수 있습니다. 하나는 고객, 다른 하나는 순서로 변환합니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-160">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

## <a name="ownedattribute"></a><span data-ttu-id="efa50-161">OwnedAttribute</span><span class="sxs-lookup"><span data-stu-id="efa50-161">OwnedAttribute</span></span>

<span data-ttu-id="efa50-162">이제 형식에 `[Owned]`라는 주석을 달고 소유자 엔터티를 모델에 추가하여 [소유한 엔터티 형식](xref:core/modeling/owned-entities)을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-162">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="new-dotnet-ef-global-tool"></a><span data-ttu-id="efa50-163">새 dotnet-ef 전역 도구</span><span class="sxs-lookup"><span data-stu-id="efa50-163">New dotnet-ef global tool</span></span>

<span data-ttu-id="efa50-164">_dotnet-ef_ 명령은 .NET CLI 전역 도구로 변환되었으므로, 마이그레이션을 사용하거나 기존 데이터베이스에서 DbContext를 스캐폴딩하기 위해 더 이상 프로젝트에서 DotNetCliToolReference를 사용할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-164">The _dotnet-ef_ commands have been converted to a .NET CLI global tool, so it will no longer be necessary to use DotNetCliToolReference in the project to be able to use migrations or to scaffold a DbContext from an existing database.</span></span>

## <a name="microsoftentityframeworkcoreabstractions-package"></a><span data-ttu-id="efa50-165">Microsoft.EntityFrameworkCore.Abstractions 패키지</span><span class="sxs-lookup"><span data-stu-id="efa50-165">Microsoft.EntityFrameworkCore.Abstractions package</span></span>
<span data-ttu-id="efa50-166">새 패키지에는 전체적으로 EF Core에 의존하지 않고 EF Core 기능을 강화하기 위해 프로젝트에서 사용할 수 있는 특성 및 인터페이스가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-166">The new package contains attributes and interfaces that you can use in your projects to light up EF Core features without taking a dependency on EF Core as a whole.</span></span> <span data-ttu-id="efa50-167">예:</span><span class="sxs-lookup"><span data-stu-id="efa50-167">E.g.</span></span> <span data-ttu-id="efa50-168">미리 보기 1에 도입된 [Owned] 특성이 여기로 이동되었습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-168">the [Owned] attribute introduced in Preview 1 was moved here.</span></span>

## <a name="state-change-events"></a><span data-ttu-id="efa50-169">상태 변경 이벤트</span><span class="sxs-lookup"><span data-stu-id="efa50-169">State change events</span></span>

<span data-ttu-id="efa50-170">`ChangeTracker`의 새 `Tracked` 및 `StateChanged` 이벤트를 사용하여 DbContext를 입력하거나 상태를 변경하는 엔터티에 대응하는 논리를 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-170">New `Tracked` And `StateChanged` events on `ChangeTracker` can be used to write logic that reacts to entities entering the DbContext or changing their state.</span></span>

## <a name="raw-sql-parameter-analyzer"></a><span data-ttu-id="efa50-171">원시 SQL 매개 변수 분석기</span><span class="sxs-lookup"><span data-stu-id="efa50-171">Raw SQL parameter analyzer</span></span>

<span data-ttu-id="efa50-172">`FromSql` 또는 `ExecuteSqlCommand`와 같이 원시 SQL API의 잠재적으로 안전하지 않은 사용을 발견하는 새 코드 분석기가 EF Core와 함께 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-172">A new code analyzer is included with EF Core that detects potentially unsafe usages of our raw-SQL APIs, like `FromSql` or `ExecuteSqlCommand`.</span></span> <span data-ttu-id="efa50-173">예:</span><span class="sxs-lookup"><span data-stu-id="efa50-173">E.g.</span></span> <span data-ttu-id="efa50-174">다음 쿼리의 경우 _minAge_에 매개 변수가 없기 때문에 경고가 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-174">for the following query, you will see a warning because _minAge_ is not parameterized:</span></span>

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a><span data-ttu-id="efa50-175">데이터베이스 공급자 호환성</span><span class="sxs-lookup"><span data-stu-id="efa50-175">Database provider compatibility</span></span>

<span data-ttu-id="efa50-176">EF Core 2.1은 EF Core 2.0에 만든 데이터베이스 공급자와 호환되거나 최소한으로 변경되도록 디자인되었습니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-176">EF Core 2.1 was designed to be compatible with database providers created for EF Core 2.0, or at least require minimal changes.</span></span> <span data-ttu-id="efa50-177">위에서 설명한 일부 기능(예: 값 변환)에 업데이트된 공급자가 필요하지만 다른 기능(예: 지연된 로드)은 기존 공급자를 사용하여 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="efa50-177">While some of the features described above (e.g. value conversions) require an updated provider, others (e.g. lazy loading) will light up with existing providers.</span></span>

> [!TIP]
> <span data-ttu-id="efa50-178">예상치 않은 호환성 또는 새로운 기능의 문제가 발생하거나 이에 대한 의견이 있는 경우 [문제 추적기](https://github.com/aspnet/EntityFrameworkCore/issues/new)를 사용하여 보고해주세요.</span><span class="sxs-lookup"><span data-stu-id="efa50-178">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
