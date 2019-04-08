---
title: EF Core 3.0의 호환성이 손상되는 변경 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: fd593b2832a5a6ffe27cd4493127b5d405f684ba
ms.sourcegitcommit: ce44f85a5bce32ef2d3d09b7682108d3473511b3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58914129"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="5ce27-102">EF Core 3.0에 포함된 호환성이 손상되는 변경(현재 미리 보기 상태)</span><span class="sxs-lookup"><span data-stu-id="5ce27-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5ce27-103">이후 릴리스의 기능 집합 및 일정은 항상 변경될 수 있으며 이 페이지를 최신 상태로 유지하려고 노력함에도 불구하고 최신 계획을 항상 반영할 수 없을 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="5ce27-104">다음 API 및 동작 변경은 3.0.0으로 업그레이드할 때 EF Core 2.2.x용으로 개발된 애플리케이션을 중단시킬 가능성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="5ce27-105">데이터베이스 공급자에만 영향을 줄 것으로 예상되는 변경 내용은 [공급자 변경](../../providers/provider-log.md)에 설명되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="5ce27-106">3.0 미리 보기 간의 도입된 새로운 기능의 중단은 여기에 설명되어 있지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="5ce27-107">LINQ 쿼리는 더 이상 클라이언트에서 평가되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="5ce27-108">[추적 문제 #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[문제 #12795도 참조](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="5ce27-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="5ce27-109">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="5ce27-110">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-110">Old behavior</span></span>**

<span data-ttu-id="5ce27-111">3.0 이전에는 EF Core가 쿼리의 일부인 식을 SQL 또는 매개 변수로 변환할 수 없었을 때 클라이언트에서 자동으로 식을 계산했습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="5ce27-112">기본적으로, 잠재적 비용이 많이 드는 식에 대한 클라이언트 평가는 경고만 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

**<span data-ttu-id="5ce27-113">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-113">New behavior</span></span>**

<span data-ttu-id="5ce27-114">3.0부터 EF Core는 최상위 수준 프로젝션(쿼리의 마지막 `Select()` 호출)의 식만 클라이언트에서 평가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="5ce27-115">쿼리의 다른 부분에서 식을 SQL 또는 매개 변수로 변환할 수 없을 때 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

**<span data-ttu-id="5ce27-116">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-116">Why</span></span>**

<span data-ttu-id="5ce27-117">쿼리의 자동 클라이언트 평가는 중요한 부분을 변환할 수 없어도 많은 쿼리를 실행할 수 있게 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="5ce27-118">이 동작으로 인해 프로덕션 환경에서 분명하게 나타날 수 있는 예기치 않은 잠재적 손상을 초래할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="5ce27-119">예를 들어 변환할 수 없는 `Where()` 호출의 조건으로 인해 테이블의 모든 행이 데이터베이스 서버에서 전송되고 필터가 클라이언트에 적용될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="5ce27-120">이 경우 테이블에 개발 중인 몇 개 행만 포함하지만 애플리케이션이 수백만 개의 행이 포함될 수 있는 프로덕션으로 이동할 때 쉽게 감지되지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="5ce27-121">클라이언트 평가 경고도 개발 중에 너무 쉽게 무시됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="5ce27-122">이 외에도 자동 클라이언트 평가는 특정 식에 대한 쿼리 변환 기능 개선으로 인해 릴리스 간의 의도하지 않은 호환성이 손상되는 변경이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

**<span data-ttu-id="5ce27-123">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-123">Mitigations</span></span>**

<span data-ttu-id="5ce27-124">쿼리를 완벽하게 변환할 수 없는 경우 변환할 수 있는 형식으로 쿼리를 다시 작성하거나 `AsEnumerable()`, `ToList()` 또는 유사한 형식을 사용하여 명시적으로 데이터를 클라이언트로 가져오면 LINQ to Objects를 사용하여 추가로 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="5ce27-125">Entity Framework Core는 더 이상 ASP.NET Core 공유 프레임워크에 포함되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="5ce27-126">추적 문제 공지 사항 #325</span><span class="sxs-lookup"><span data-stu-id="5ce27-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="5ce27-127">이 변경 내용은 ASP.NET Core 3.0 미리 보기 1에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

**<span data-ttu-id="5ce27-128">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-128">Old behavior</span></span>**

<span data-ttu-id="5ce27-129">ASP.NET Core 3.0 이전에는 `Microsoft.AspNetCore.App` 또는 `Microsoft.AspNetCore.All`에 대한 패키지 참조를 추가하면 EF Core 및 SQL Server 공급자와 같은 일부 EF Core 데이터 공급자가 포함되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

**<span data-ttu-id="5ce27-130">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-130">New behavior</span></span>**

<span data-ttu-id="5ce27-131">3.0부터 ASP.NET Core 공유 프레임워크에는 EF Core 또는 EF Core 데이터 공급자가 포함되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

**<span data-ttu-id="5ce27-132">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-132">Why</span></span>**

<span data-ttu-id="5ce27-133">이 변경 전에 EF Core를 가져오려면 애플리케이션이 ASP.NET Core 및 SQL Server를 대상으로 했는지 여부에 따라 다른 단계가 필요했습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="5ce27-134">또한 ASP.NET Core를 업그레이드 하면 EF Core 및 SQL Server 업그레이드가 강제되었는데, 이는 항상 바람직하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="5ce27-135">이 변경으로 인해 EF Core를 가져오는 환경은 모든 공급자, 지원되는 .NET 구현 및 애플리케이션 유형에서 동일합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="5ce27-136">또한 개발자는 이제 EF Core 및 EF Core 데이터 공급자의 업그레이드 시기를 정확히 제어할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

**<span data-ttu-id="5ce27-137">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-137">Mitigations</span></span>**

<span data-ttu-id="5ce27-138">ASP.NET Core 3.0 애플리케이션 또는 기타 지원되는 애플리케이션에서 EF Core를 사용하려면 애플리케이션에서 사용할 EF Core 데이터베이스 공급자에 대한 패키지 참조를 명시적으로 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="5ce27-139">FromSql, ExecuteSql, ExecuteSqlAsync의 이름이 변경되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-139">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="5ce27-140">추적 문제 #10996</span><span class="sxs-lookup"><span data-stu-id="5ce27-140">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="5ce27-141">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-141">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="5ce27-142">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-142">Old behavior</span></span>**

<span data-ttu-id="5ce27-143">EF Core 3.0이 나오기 전에는 이런 메서드 이름이 일반 문자열 또는 SQL 및 매개 변수로 보간해야 하는 문자열에 대해 작동하도록 오버로드되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-143">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

**<span data-ttu-id="5ce27-144">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-144">New behavior</span></span>**

<span data-ttu-id="5ce27-145">EF Core 3.0부터는 `FromSqlRaw`, `ExecuteSqlRaw` 및 `ExecuteSqlRawAsync`를 사용하여 매개 변수를 생성합니다. 여기서 매개 변수는 보간된 쿼리 문자열의 일부로서 전달됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-145">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="5ce27-146">예:</span><span class="sxs-lookup"><span data-stu-id="5ce27-146">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="5ce27-147">`FromSqlInterpolated`, `ExecuteSqlInterpolated` 및 `ExecuteSqlInterpolatedAsync`를 사용하여 매개 변수를 생성합니다. 여기서 매개 변수는 보간된 쿼리 문자열의 일부로서 전달됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-147">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="5ce27-148">예:</span><span class="sxs-lookup"><span data-stu-id="5ce27-148">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="5ce27-149">위의 두 쿼리 모두 같은 SQL 매개 변수를 사용하여 같은 매개 변수가 있는 SQL을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-149">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

**<span data-ttu-id="5ce27-150">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-150">Why</span></span>**

<span data-ttu-id="5ce27-151">이와 같은 메서드 오버로드를 적용할 경우 보간된 문자열 메서드를 호출하려다 실수로 원시 문자열 메서드를 호출하거나, 반대로 후자를 호출하려다 전자를 호출하는 실수를 저지를 가능성이 매우 높습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-151">Method overloads like this make it very easy to accidentally call the raw srting method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="5ce27-152">쿼리에는 매개 변수가 있어야 하는데, 이 경우 매개 변수가 없는 쿼리가 나올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-152">This could result in queries not being parameterized when they should have been.</span></span>

**<span data-ttu-id="5ce27-153">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-153">Mitigations</span></span>**

<span data-ttu-id="5ce27-154">새 메서드 이름을 사용하도록 전환합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-154">Switch to use the new method names.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="5ce27-155">쿼리 실행이 디버그 수준에서 로깅됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-155">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="5ce27-156">추적 문제 #14523</span><span class="sxs-lookup"><span data-stu-id="5ce27-156">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="5ce27-157">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-157">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="5ce27-158">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-158">Old behavior</span></span>**

<span data-ttu-id="5ce27-159">EF Core 3.0 이전에는 쿼리 및 기타 명령 실행이 `Info` 수준에서 로깅되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-159">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

**<span data-ttu-id="5ce27-160">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-160">New behavior</span></span>**

<span data-ttu-id="5ce27-161">EF Core 3.0 부터 명령/SQL 실행의 로깅은 `Debug` 수준입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-161">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

**<span data-ttu-id="5ce27-162">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-162">Why</span></span>**

<span data-ttu-id="5ce27-163">이 변경은 `Info` 로그 수준에서 노이즈를 줄이기 위해 수행되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-163">This change was made to reduce the noise at the `Info` log level.</span></span>

**<span data-ttu-id="5ce27-164">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-164">Mitigations</span></span>**

<span data-ttu-id="5ce27-165">이 로깅 이벤트는 `RelationalEventId.CommandExecuting`에서 이벤트 ID 20100으로 정의됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-165">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="5ce27-166">`Info` 수준에서 다시 SQL을 로그하려면 `OnConfiguring` 또는 `AddDbContext`에서 수준을 명시적으로 구성하세요.</span><span class="sxs-lookup"><span data-stu-id="5ce27-166">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="5ce27-167">예:</span><span class="sxs-lookup"><span data-stu-id="5ce27-167">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="5ce27-168">임시 키 값은 더 이상 엔터티 인스턴스에 설정되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-168">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="5ce27-169">추적 문제 #12378</span><span class="sxs-lookup"><span data-stu-id="5ce27-169">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="5ce27-170">이 변경 내용은 EF Core 3.0 미리 보기 2에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-170">This change was introduced in EF Core 3.0-preview 2.</span></span>

**<span data-ttu-id="5ce27-171">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-171">Old behavior</span></span>**

<span data-ttu-id="5ce27-172">EF Core 3.0 이전에는 임시 값이 데이터베이스에서 생성된 실제 값을 나중에 가질 수 있는 모든 키 속성에 할당되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-172">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="5ce27-173">일반적으로 이러한 임시 값은 큰 음수입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-173">Usually these temporary values were large negative numbers.</span></span>

**<span data-ttu-id="5ce27-174">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-174">New behavior</span></span>**

<span data-ttu-id="5ce27-175">3.0부터 EF Core는 엔터티의 추적 정보의 일부로 임시 키 값을 저장하고, 키 속성 자체는 변경하지 않고 그대로 둡니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-175">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

**<span data-ttu-id="5ce27-176">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-176">Why</span></span>**

<span data-ttu-id="5ce27-177">이 변경은 일부 `DbContext` 인스턴스의 의해 이전에 추적된 엔터티가 다른 `DbContext` 인스턴스로 이동할 때 임시 키 값이 틀리게 영구화되지 않도록 하기 위해 수행되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-177">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

**<span data-ttu-id="5ce27-178">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-178">Mitigations</span></span>**

<span data-ttu-id="5ce27-179">엔터티 간의 연결을 형성하기 위해 외래 키에 기본 키 값을 할당하는 애플리케이션은 기본 키가 저장 생성되고 `Added` 상태의 엔터티에 속하는 경우 이전 동작에 따라 달라질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-179">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="5ce27-180">이는 다음과 같은 방법으로 방지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-180">This can be avoided by:</span></span>
* <span data-ttu-id="5ce27-181">저장 생성 키를 사용하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-181">Not using store-generated keys.</span></span>
* <span data-ttu-id="5ce27-182">외래 키 값을 설정하는 대신 관계를 형성하도록 탐색 속성을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-182">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="5ce27-183">엔터티의 추적 정보에서 실제 임시 키 값을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-183">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="5ce27-184">예를 들어 `context.Entry(blog).Property(e => e.Id).CurrentValue`는 `blog.Id` 자체가 설정되지 않았더라도 임시 값을 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-184">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="5ce27-185">DetectChanges는 저장 생성 키 값을 준수합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-185">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="5ce27-186">추적 문제 #14616</span><span class="sxs-lookup"><span data-stu-id="5ce27-186">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="5ce27-187">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="5ce27-188">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-188">Old behavior</span></span>**

<span data-ttu-id="5ce27-189">EF Core 3.0 이전에는 `DetectChanges`에서 찾은 추적되는 않은 엔터티를 `Added` 상태에서 추적하여 `SaveChanges`가 호출될 때 새로운 행으로 삽입했습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-189">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

**<span data-ttu-id="5ce27-190">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-190">New behavior</span></span>**

<span data-ttu-id="5ce27-191">EF Core 3.0부터 생성된 키 값을 사용하고 일부 키 값을 설정한 경우 `Modified` 상태에서 엔터티를 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-191">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="5ce27-192">즉, 엔터티에 대한 행이 있는 것으로 가정하고 `SaveChanges`가 호출될 때 업데이트됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-192">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="5ce27-193">키 값이 설정되지 않았거나 엔터티 형식이 생성된 키를 사용하지 않는 경우, 새 엔터티는 이전 버전과 마찬가지로 `Added`로 추적됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-193">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

**<span data-ttu-id="5ce27-194">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-194">Why</span></span>**

<span data-ttu-id="5ce27-195">이 변경으로 인해 저장 생성 키를 사용하는 동안 연결이 분리된 엔터티 그래프를 사용하여 작업을 보다 쉽고 일관되게 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-195">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

**<span data-ttu-id="5ce27-196">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-196">Mitigations</span></span>**

<span data-ttu-id="5ce27-197">엔터티 형식이 생성된 키를 사용하도록 구성되었지만 키 값이 새 인스턴스에 명시적으로 설정된 경우, 이 변경으로 인해 애플리케이션이 중단될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-197">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="5ce27-198">해결 방법은 생성된 값을 사용하지 않도록 키 속성을 명시적으로 구성하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-198">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="5ce27-199">예를 들어 흐름 API가 있는 경우:</span><span class="sxs-lookup"><span data-stu-id="5ce27-199">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="5ce27-200">데이터 주석이 있는 경우:</span><span class="sxs-lookup"><span data-stu-id="5ce27-200">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="5ce27-201">계단식 삭제는 기본적으로 즉시 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-201">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="5ce27-202">추적 문제 #10114</span><span class="sxs-lookup"><span data-stu-id="5ce27-202">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="5ce27-203">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-203">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="5ce27-204">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-204">Old behavior</span></span>**

<span data-ttu-id="5ce27-205">3.0 이전에는 SaveChanges가 호출될 때까지 EF Core에는 연계 작업(필요한 보안 주체가 삭제되거나 필요한 보안 주체에 대한 관계가 끊어질 때 종속 엔터티 삭제)이 적용되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-205">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

**<span data-ttu-id="5ce27-206">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-206">New behavior</span></span>**

<span data-ttu-id="5ce27-207">3.0부터 EF Core는 트리거 조건이 검색되는 즉시 연계 동작을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-207">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="5ce27-208">예를 들어, 보안 주체 엔터티를 삭제하기 위해 `context.Remove()`를 호출하면 추적된 모든 관련 필수 종속 항목도 즉시 `Deleted`로 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-208">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

**<span data-ttu-id="5ce27-209">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-209">Why</span></span>**

<span data-ttu-id="5ce27-210">이 변경으로 인해 `SaveChanges`가 호출되기 _전에_ 삭제될 엔터티를 이해하는 것이 중요한 데이터 바인딩 및 감사 시나리오 환경이 개선되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-210">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

**<span data-ttu-id="5ce27-211">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-211">Mitigations</span></span>**

<span data-ttu-id="5ce27-212">이전 동작은 `context.ChangedTracker`의 설정을 통해 복원할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-212">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="5ce27-213">예:</span><span class="sxs-lookup"><span data-stu-id="5ce27-213">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="5ce27-214">쿼리 형식은 엔터티 형식과 통합됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-214">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="5ce27-215">추적 문제 #14194</span><span class="sxs-lookup"><span data-stu-id="5ce27-215">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="5ce27-216">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-216">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="5ce27-217">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-217">Old behavior</span></span>**

<span data-ttu-id="5ce27-218">EF Core 3.0 이전에는 [쿼리 형식](xref:core/modeling/query-types)이 기본 키를 구조화된 방법으로 정의하지 않는 데이터를 쿼리하는 수단이었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-218">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="5ce27-219">즉, 쿼리 형식은 키 없이 엔터티 형식을 매핑하는데 사용(보기에서 가능할 수도 있지만 테이블에서도 가능한 경우)되는 반면, 일반 엔터티 형식은 키가 사용 가능할 때(테이블에서 가능하지만 보기에서도 가능한 경우) 사용되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-219">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

**<span data-ttu-id="5ce27-220">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-220">New behavior</span></span>**

<span data-ttu-id="5ce27-221">이제 쿼리 형식은 기본 키가 없는 엔터티 형식이 될 뿐입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-221">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="5ce27-222">키 없는 엔터티 형식은 이전 버전의 쿼리 형식과 동일한 기능이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-222">Keyless entity types have the same functionality as query types in previous versions.</span></span>

**<span data-ttu-id="5ce27-223">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-223">Why</span></span>**

<span data-ttu-id="5ce27-224">이 변경으로 인해 쿼리 형식의 목적에 대한 혼동을 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-224">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="5ce27-225">특히 키 없는 엔터티 형식이며 이로 인해 본질적으로 읽기 전용이지만 엔터티 형식이 읽기 전용으로 할 필요가 있다고 해서 사용해서는 안됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-225">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="5ce27-226">마찬가지로 보기에 매핑되는 경우가 있지만 이는 보기가 키를 정의하지 않는 경우가 있기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-226">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

**<span data-ttu-id="5ce27-227">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-227">Mitigations</span></span>**

<span data-ttu-id="5ce27-228">API의 다음 부분은 이제 사용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-228">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="5ce27-229">**`ModelBuilder.Query<>()`** - 대신 엔터티 형식을 키가 없는 것으로 표시하기 위해 `ModelBuilder.Entity<>().HasNoKey()`를 호출해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-229">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="5ce27-230">기본 키가 필요하지만 규칙과 일치하지 않는 경우 잘못된 구성을 피하기 위해 규칙에 따라 구성되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-230">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="5ce27-231">**`DbQuery<>`** - 대신 `DbSet<>`를 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-231">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="5ce27-232">**`DbContext.Query<>()`** - 대신 `DbContext.Set<>()`를 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-232">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="5ce27-233">소유 형식 관계에 대한 구성 API가 변경됨</span><span class="sxs-lookup"><span data-stu-id="5ce27-233">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="5ce27-234">[추적 문제 #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[추적 문제 #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[추적 문제 #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="5ce27-234">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="5ce27-235">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-235">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="5ce27-236">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-236">Old behavior</span></span>**

<span data-ttu-id="5ce27-237">EF Core 3.0 이전에는 소유 관계의 구성은 `OwnsOne` 또는 `OwnsMany` 호출 직후에 수행되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-237">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

**<span data-ttu-id="5ce27-238">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-238">New behavior</span></span>**

<span data-ttu-id="5ce27-239">EF Core 3.0부터 이제 `WithOwner()`를 사용하여 소유자에게 탐색 속성을 구성하는 흐름 API가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-239">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="5ce27-240">예:</span><span class="sxs-lookup"><span data-stu-id="5ce27-240">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="5ce27-241">이제 소유자와 소유 간의 관계와 관련된 구성이 다른 관계가 구성되는 것과 유사하게 `WithOwner()` 후에 연결되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-241">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="5ce27-242">소유 형식 자체에 대한 구성은 `OwnsOne()/OwnsMany()` 이후에도 여전히 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-242">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="5ce27-243">예:</span><span class="sxs-lookup"><span data-stu-id="5ce27-243">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

<span data-ttu-id="5ce27-244">또한 소유 형식 대상으로 `Entity()`, `HasOne()` 또는 `Set()`를 호출하면 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-244">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

**<span data-ttu-id="5ce27-245">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-245">Why</span></span>**

<span data-ttu-id="5ce27-246">이 변경으로 인해 소유 형식 자체의 구성과 소유 형식의 _관계 대상_ 사이를 명확하게 분리하게 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-246">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="5ce27-247">이는 `HasForeignKey`와 같은 메서드에 대한 모호성과 혼란을 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-247">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

**<span data-ttu-id="5ce27-248">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-248">Mitigations</span></span>**

<span data-ttu-id="5ce27-249">위의 예와 같이 소유 형식 관계의 구성을 변경하여 새 API 화면을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-249">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="5ce27-250">보안 주체와 테이블을 공유하는 종속 엔터티는 이제 선택 사항입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-250">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="5ce27-251">추적 문제 #9005</span><span class="sxs-lookup"><span data-stu-id="5ce27-251">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="5ce27-252">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-252">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="5ce27-253">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-253">Old behavior</span></span>**

<span data-ttu-id="5ce27-254">다음 모델을 살펴보세요.</span><span class="sxs-lookup"><span data-stu-id="5ce27-254">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```
<span data-ttu-id="5ce27-255">EF Core 3.0 전에는 `OrderDetails`를 `Order`가 소유하거나 같은 테이블에 명시적으로 매핑되는 경우 새 `Order`를 추가할 때 `OrderDetails` 인스턴스가 언제나 필수였습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-255">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


**<span data-ttu-id="5ce27-256">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-256">New behavior</span></span>**

<span data-ttu-id="5ce27-257">3.0부터는 EF Core가 `OrderDetails` 없이 `Order`를 추가할 수 있으며 기본 키를 제외한 모든 `OrderDetails` 속성을 Null 허용 열에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-257">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="5ce27-258">EF Core를 쿼리하는 경우 해당 필수 속성에 값이 없거나 기본 키 이외의 필수 속성이 없고 모든 속성이 `null`이면 `OrderDetails`를 `null`로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-258">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

**<span data-ttu-id="5ce27-259">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-259">Mitigations</span></span>**

<span data-ttu-id="5ce27-260">사용하는 모델이 모든 선택적 열에 따라 다르게 공유되는 테이블을 포함하지만 해당 테이블을 가리키는 탐색이 `null`로 예상되지 않는 경우 `null`을 탐색하는 경우를 처리하도록 애플리케이션을 수정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-260">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="5ce27-261">이렇게 할 수 없는 경우 엔터티 형식에 필수 속성을 추가하거나 하나 이상의 속성에 `null`이 아닌 값을 할당해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-261">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="5ce27-262">동시 토큰 열을 사용하여 테이블을 공유하는 모든 엔터티는 해당 열을 속성에 매핑해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-262">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="5ce27-263">추적 문제 #14154</span><span class="sxs-lookup"><span data-stu-id="5ce27-263">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="5ce27-264">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-264">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="5ce27-265">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-265">Old behavior</span></span>**

<span data-ttu-id="5ce27-266">다음 모델을 살펴보세요.</span><span class="sxs-lookup"><span data-stu-id="5ce27-266">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
<span data-ttu-id="5ce27-267">EF Core 3.0 전에는 `OrderDetails`를 `Order`가 소유하거나 같은 테이블에 명시적으로 매핑하는 경우 `OrderDetails`만 업데이트하면 클라이언트의 `Version` 값이 업데이트되지 않으며 다음 업데이트가 실패합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-267">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


**<span data-ttu-id="5ce27-268">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-268">New behavior</span></span>**

<span data-ttu-id="5ce27-269">3.0부터 EF Core는 `OrderDetails`를 소유하는 경우 새 `Version` 값을 `Order`에 전파합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-269">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="5ce27-270">그렇지 않으면 모델 유효성 검사 중에 예외가 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-270">Otherwise an exception is thrown during model validation.</span></span>

**<span data-ttu-id="5ce27-271">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-271">Why</span></span>**

<span data-ttu-id="5ce27-272">같은 테이블에 매핑된 엔터티 중 한 개만 업데이트하는 경우 부실 동시성 토큰 값을 방지하기 위해 이렇게 변경되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-272">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

**<span data-ttu-id="5ce27-273">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-273">Mitigations</span></span>**

<span data-ttu-id="5ce27-274">테이블을 공유하는 모든 엔터티는 동시성 토큰 열에 매핑되는 속성을 포함해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-274">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="5ce27-275">해당 속성을 섀도 상태로 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-275">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="5ce27-276">매핑되지 않은 형식에서 상속된 속성은 이제 모든 파생 형식에 대해 단일 열에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-276">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="5ce27-277">추적 문제 #13998</span><span class="sxs-lookup"><span data-stu-id="5ce27-277">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="5ce27-278">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="5ce27-279">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-279">Old behavior</span></span>**

<span data-ttu-id="5ce27-280">다음 모델을 살펴보세요.</span><span class="sxs-lookup"><span data-stu-id="5ce27-280">Consider the following model:</span></span>
```C#
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

<span data-ttu-id="5ce27-281">EF Core 3.0 전에는 `ShippingAddress` 속성이 기본적으로 `BulkOrder` 및 `Order`에 대한 별도의 열에 매핑되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-281">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

**<span data-ttu-id="5ce27-282">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-282">New behavior</span></span>**

<span data-ttu-id="5ce27-283">3.0부터 EF Core는 `ShippingAddress`에 대한 열을 한 개만 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-283">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

**<span data-ttu-id="5ce27-284">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-284">Why</span></span>**

<span data-ttu-id="5ce27-285">이전 동작은 예상할 수 없었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-285">The old behavoir was unexpected.</span></span>

**<span data-ttu-id="5ce27-286">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-286">Mitigations</span></span>**

<span data-ttu-id="5ce27-287">이 속성을 여전히 파생 형식에 대한 별도의 열에 명시적으로 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-287">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="5ce27-288">외래 키 속성 규칙이 더 이상 보안 주체 속성과 동일한 이름을 일치시키지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-288">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="5ce27-289">추적 문제 #13274</span><span class="sxs-lookup"><span data-stu-id="5ce27-289">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="5ce27-290">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-290">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="5ce27-291">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-291">Old behavior</span></span>**

<span data-ttu-id="5ce27-292">다음 모델을 살펴보세요.</span><span class="sxs-lookup"><span data-stu-id="5ce27-292">Consider the following model:</span></span>
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```
<span data-ttu-id="5ce27-293">EF Core 3.0 이전에는 규칙에 따라 외래 키에 대해 `CustomerId` 속성이 사용되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-293">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="5ce27-294">그러나 `Order`가 소유 형식인 경우 `CustomerId`도 기본 키가 되며 이는 일반적으로 예상되는 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-294">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

**<span data-ttu-id="5ce27-295">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-295">New behavior</span></span>**

<span data-ttu-id="5ce27-296">3.0부터 EF Core는 보안 주체 속성과 동일한 이름을 가지는 경우 규칙에 따라 외래 키에 대한 속성을 사용하려고 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-296">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="5ce27-297">보안 주체 속성 이름과 연결된 보안 주체 유형 이름 및 보안 주체 속성 이름 패턴과 연결된 탐색 이름은 여전히 일치합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-297">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="5ce27-298">예:</span><span class="sxs-lookup"><span data-stu-id="5ce27-298">For example:</span></span>

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

**<span data-ttu-id="5ce27-299">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-299">Why</span></span>**

<span data-ttu-id="5ce27-300">이 변경으로 인해 소유 형식에서 기본 키 속성을 잘못 정의하지 않게 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-300">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

**<span data-ttu-id="5ce27-301">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-301">Mitigations</span></span>**

<span data-ttu-id="5ce27-302">속성을 외래 키로 하고, 이에 따라서 기본 키의 일부가 되는 경우 명시적으로 이와 같이 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-302">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="5ce27-303">데이터베이스 연결은 이제 TransactionScope가 완료되기 전에 더 이상 사용되지 않으면 닫힙니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-303">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="5ce27-304">추적 문제 #14218</span><span class="sxs-lookup"><span data-stu-id="5ce27-304">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="5ce27-305">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-305">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="5ce27-306">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-306">Old behavior</span></span>**

<span data-ttu-id="5ce27-307">EF Core 3.0 전에는 컨텍스트가 `TransactionScope` 내에서 연결을 여는 경우 현재 `TransactionScope`가 활성화되어 있는 동안 해당 연결이 열린 상태로 유지됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-307">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point
        
        var categories = context.ProductCategories().ToList();
    }
}
```

**<span data-ttu-id="5ce27-308">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-308">New behavior</span></span>**

<span data-ttu-id="5ce27-309">3.0부터 EF Core는 연결의 사용이 완료되면 해당 연결을 즉시 닫습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-309">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

**<span data-ttu-id="5ce27-310">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-310">Why</span></span>**

<span data-ttu-id="5ce27-311">같은 `TransactionScope`에 복수의 컨텍스트를 사용할 수 있도록 이렇게 변경했습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-311">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="5ce27-312">새 동작 alose는 EF6과 일치합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-312">The new behavior alose matches EF6.</span></span>

**<span data-ttu-id="5ce27-313">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-313">Mitigations</span></span>**

<span data-ttu-id="5ce27-314">연결이 열린 상태로 유지되어야 하는 경우 `OpenConnection()`을 명시적으로 호출하여 EF Core가 연결을 조기에 닫지 않도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-314">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();
        
        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="5ce27-315">각 속성은 독립적인 메모리 내 정수 키 생성을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-315">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="5ce27-316">추적 문제 #6872</span><span class="sxs-lookup"><span data-stu-id="5ce27-316">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="5ce27-317">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-317">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="5ce27-318">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-318">Old behavior</span></span>**

<span data-ttu-id="5ce27-319">EF Core 3.0 이전에는 모든 메모리 내 정수 키 속성에 대해 하나의 공유 값 생성기가 사용되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-319">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

**<span data-ttu-id="5ce27-320">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-320">New behavior</span></span>**

<span data-ttu-id="5ce27-321">EF Core 3.0부터 메모리 내 데이터베이스를 사용하는 경우 각 정수 키 속성은 자체 값 생성기를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-321">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="5ce27-322">또한 데이터베이스가 삭제되면 모든 테이블에 대해 키 생성이 재설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-322">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

**<span data-ttu-id="5ce27-323">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-323">Why</span></span>**

<span data-ttu-id="5ce27-324">이 변경으로 인해 메모리 내 키 생성을 실제 데이터베이스 키 생성에 보다 가깝게 정렬하고 메모리 내 데이터베이스를 사용할 때 테스트를 서로 격리하는 기능이 향상되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-324">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

**<span data-ttu-id="5ce27-325">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-325">Mitigations</span></span>**

<span data-ttu-id="5ce27-326">이로 인해 특정 메모리 내 키 값을 사용하는 애플리케이션이 중단될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-326">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="5ce27-327">대신 특정 키 값을 사용하지 않거나 새 동작과 일치하도록 업데이트하지 않도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-327">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="5ce27-328">지원 필드는 기본적으로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-328">Backing fields are used by default</span></span>

[<span data-ttu-id="5ce27-329">추적 문제 #12430</span><span class="sxs-lookup"><span data-stu-id="5ce27-329">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="5ce27-330">이 변경 내용은 EF Core 3.0 미리 보기 2에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-330">This change was introduced in EF Core 3.0-preview 2.</span></span>

**<span data-ttu-id="5ce27-331">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-331">Old behavior</span></span>**

<span data-ttu-id="5ce27-332">3.0 이전에는 속성에 대한 지원 필드가 알려져 있더라도 EF Core는 기본적으로 속성 getter 및 setter 메서드를 사용하여 속성 값을 읽고 작성했습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-332">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="5ce27-333">이에 대한 예외는 쿼리 실행으로 알려진 경우 지원 필드가 직접 설정되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-333">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

**<span data-ttu-id="5ce27-334">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-334">New behavior</span></span>**

<span data-ttu-id="5ce27-335">EF Core 3.0부터 속성 지원 필드가 알려진 경우 항상 지원 필드를 사용하여 해당 속성을 읽고 씁니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-335">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="5ce27-336">이로 인해 애플리케이션이 getter 또는 setter 메서드로 코딩된 추가 동작을 사용하는 경우 애플리케이션이 중단될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-336">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

**<span data-ttu-id="5ce27-337">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-337">Why</span></span>**

<span data-ttu-id="5ce27-338">이 변경으로 인해 엔터티가 포함된 데이터베이스 작업을 수행할 때 EF Core가 기본적으로 비즈니스 논리를 잘못 트리거하는 것을 방지합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-338">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

**<span data-ttu-id="5ce27-339">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-339">Mitigations</span></span>**

<span data-ttu-id="5ce27-340">modelBuilder 흐름 API에서 속성 액세스 모드의 구성을 통해 3.0 이전 버전의 동작을 복원할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-340">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="5ce27-341">예:</span><span class="sxs-lookup"><span data-stu-id="5ce27-341">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="5ce27-342">호환이 가능한 여러 지원 필드가 발견되면 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-342">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="5ce27-343">추적 문제 #12523</span><span class="sxs-lookup"><span data-stu-id="5ce27-343">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="5ce27-344">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-344">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="5ce27-345">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-345">Old behavior</span></span>**

<span data-ttu-id="5ce27-346">EF Core 3.0 이전에는 여러 필드가 속성의 지원 필드를 찾기 위한 규칙과 일치하면 우선순위에 따라 하나의 필드가 선택되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-346">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="5ce27-347">이로 인해 모호한 경우 잘못된 필드가 사용될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-347">This could cause the wrong field to be used in ambiguous cases.</span></span>

**<span data-ttu-id="5ce27-348">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-348">New behavior</span></span>**

<span data-ttu-id="5ce27-349">EF Core 3.0부터 여러 필드가 동일한 속성과 일치하면 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-349">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

**<span data-ttu-id="5ce27-350">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-350">Why</span></span>**

<span data-ttu-id="5ce27-351">이 변경으로 인해 하나의 필드만 정확할 때 다른 필드보다 한 필드만 자동으로 사용하는 것을 방지했습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-351">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

**<span data-ttu-id="5ce27-352">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-352">Mitigations</span></span>**

<span data-ttu-id="5ce27-353">모호한 지원 필드가 있는 속성은 사용할 필드가 명시적으로 지정되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-353">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="5ce27-354">예를 들어 흐름 API 사용:</span><span class="sxs-lookup"><span data-stu-id="5ce27-354">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="5ce27-355">AddDbContext/AddDbContextPool이 더 이상 AddLogging 및 AddMemoryCache를 호출하지 않음</span><span class="sxs-lookup"><span data-stu-id="5ce27-355">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="5ce27-356">추적 문제 #14756</span><span class="sxs-lookup"><span data-stu-id="5ce27-356">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="5ce27-357">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-357">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="5ce27-358">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-358">Old behavior</span></span>**

<span data-ttu-id="5ce27-359">EF Core 3.0 이전에는 `AddDbContext` 또는 `AddDbContextPool`을 호출하면 [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) 및 [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)에 대한 호출을 통해 D.I를 사용하여 로깅 및 메모리 캐시 서비스도 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-359">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

**<span data-ttu-id="5ce27-360">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-360">New behavior</span></span>**

<span data-ttu-id="5ce27-361">EF Core 3.0부터 `AddDbContext` 및 `AddDbContextPool`은 DI(종속성 주입)를 사용하여 이러한 서비스를 더 이상 등록하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-361">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

**<span data-ttu-id="5ce27-362">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-362">Why</span></span>**

<span data-ttu-id="5ce27-363">EF Core 3.0에서는 이러한 서비스가 애플리케이션의 DI 컨테이너에 있을 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-363">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="5ce27-364">그러나 `ILoggerFactory`가 애플리케이션의 DI 컨테이너에 등록된 경우 EF Core에서 여전히 DI를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-364">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

**<span data-ttu-id="5ce27-365">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-365">Mitigations</span></span>**

<span data-ttu-id="5ce27-366">애플리케이션에 이러한 서비스가 필요한 경우 [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) 또는 [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)를 사용하여 DI 컨테이너에 명시적으로 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-366">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="5ce27-367">DbContext.Entry는 이제 로컬 DetectChanges를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-367">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="5ce27-368">추적 문제 #13552</span><span class="sxs-lookup"><span data-stu-id="5ce27-368">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="5ce27-369">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-369">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="5ce27-370">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-370">Old behavior</span></span>**

<span data-ttu-id="5ce27-371">EF Core 3.0 이전에는 `DbContext.Entry`를 호출하면 추적된 모든 엔터티에 대해 변경 내용이 검색되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-371">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="5ce27-372">이로 인해 `EntityEntry`에 노출된 상태가 최신 상태로 유지되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-372">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

**<span data-ttu-id="5ce27-373">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-373">New behavior</span></span>**

<span data-ttu-id="5ce27-374">EF Core 3.0부터 `DbContext.Entry` 호출은 지정된 엔터티와 이와 관련된 추적된 모든 주체 엔터티의 변경 내용만 검색하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-374">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="5ce27-375">즉, 이 메서드를 호출하여 다른 곳의 변경 내용을 검색하지 못할 수 있으므로 애플리케이션 상태에 영향을 줄 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-375">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="5ce27-376">`ChangeTracker.AutoDetectChangesEnabled`를 `false`로 설정하면 이 로컬 변경 검색도 비활성화됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-376">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="5ce27-377">변경 검색(예: `ChangeTracker.Entries` 및 `SaveChanges`)을 유발하는 다른 메서드는 추적된 모든 엔터티의 전체 `DetectChanges`를 유발합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-377">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

**<span data-ttu-id="5ce27-378">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-378">Why</span></span>**

<span data-ttu-id="5ce27-379">이 변경으로 인해 `context.Entry`를 사용하는 기본 성능이 향상되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-379">This change was made to improve the default performance of using `context.Entry`.</span></span>

**<span data-ttu-id="5ce27-380">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-380">Mitigations</span></span>**

<span data-ttu-id="5ce27-381">`Entry`를 호출하기 전에 명시적으로 `ChgangeTracker.DetectChanges()`를 호출하여 3.0 이전 버전의 동작을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-381">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="5ce27-382">문자열 및 바이트 배열 키는 기본적으로 클라이언트에서 생성되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-382">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="5ce27-383">추적 문제 #14617</span><span class="sxs-lookup"><span data-stu-id="5ce27-383">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="5ce27-384">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-384">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="5ce27-385">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-385">Old behavior</span></span>**

<span data-ttu-id="5ce27-386">EF Core 3.0 이전에는 null이 아닌 값을 명시적으로 설정하지 않고도 `string` 및 `byte[]` 키 속성을 사용할 수 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-386">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="5ce27-387">이 경우 키 값은 클라이언트에서 GUID로 생성되고 `byte[]`의 바이트로 직렬화됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-387">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

**<span data-ttu-id="5ce27-388">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-388">New behavior</span></span>**

<span data-ttu-id="5ce27-389">EF Core 3.0부터 키 값이 설정되지 않았음을 나타내는 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-389">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

**<span data-ttu-id="5ce27-390">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-390">Why</span></span>**

<span data-ttu-id="5ce27-391">클라이언트에서 생성한 `string`/`byte[]` 값은 일반적으로 유용하지 않고 기본 동작으로 인해 생성된 키 값을 일반적인 방식으로 추론하기가 어려웠기 때문에 이 변경이 이루어졌습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-391">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

**<span data-ttu-id="5ce27-392">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-392">Mitigations</span></span>**

<span data-ttu-id="5ce27-393">다른 null이 아닌 값이 설정되지 않은 경우 키 속성이 생성된 값을 사용해야 함을 명시적으로 지정하면 3.0 이전 버전 동작을 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-393">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="5ce27-394">예를 들어 흐름 API가 있는 경우:</span><span class="sxs-lookup"><span data-stu-id="5ce27-394">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="5ce27-395">데이터 주석이 있는 경우:</span><span class="sxs-lookup"><span data-stu-id="5ce27-395">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="5ce27-396">ILoggerFactory는 이제 범위가 지정된 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-396">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="5ce27-397">추적 문제 #14698</span><span class="sxs-lookup"><span data-stu-id="5ce27-397">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="5ce27-398">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-398">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="5ce27-399">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-399">Old behavior</span></span>**

<span data-ttu-id="5ce27-400">EF Core 3.0 이전에는 `ILoggerFactory`가 싱글톤 서비스로 등록되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-400">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

**<span data-ttu-id="5ce27-401">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-401">New behavior</span></span>**

<span data-ttu-id="5ce27-402">EF Core 3.0부터 이제 `ILoggerFactory`는 범위가 지정된 대로 등록됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-402">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

**<span data-ttu-id="5ce27-403">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-403">Why</span></span>**

<span data-ttu-id="5ce27-404">이 변경으로 인해 `DbContext` 인스턴스와 로거를 연결하여 다른 기능을 활성화하고 내부 서비스 공급자의 급증과 같은 병리적인 동작의 일부 사례가 제거됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-404">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

**<span data-ttu-id="5ce27-405">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-405">Mitigations</span></span>**

<span data-ttu-id="5ce27-406">이 변경 내용은 EF Core 내부 서비스 공급자에 사용자 지정 서비스를 등록하고 사용하지 않는 한 애플리케이션 코드에 영향을 미치지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-406">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="5ce27-407">이는 일반적이지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-407">This isn't common.</span></span>
<span data-ttu-id="5ce27-408">이러한 경우 대부분의 작업은 여전히 작동하지만 `ILoggerFactory`에 종속된 싱글톤 서비스는 `ILoggerFactory`를 얻기 위해 다른 방식으로 변경해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-408">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="5ce27-409">이와 같은 상황에 처한 경우 [EF Core GitHub 문제 추적기](https://github.com/aspnet/EntityFrameworkCore/issues)에 문제를 제출하여 향후 이 문제를 해결할 수 있는 방법을 더 잘 이해할 수 있도록 `ILoggerFactory`를 사용하는 방법을 알려주세요.</span><span class="sxs-lookup"><span data-stu-id="5ce27-409">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="5ce27-410">IDbContextOptionsExtensionWithDebugInfo가 IDbContextOptionsExtension에 병합됨</span><span class="sxs-lookup"><span data-stu-id="5ce27-410">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="5ce27-411">추적 문제 #13552</span><span class="sxs-lookup"><span data-stu-id="5ce27-411">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="5ce27-412">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-412">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="5ce27-413">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-413">Old behavior</span></span>**

`IDbContextOptionsExtensionWithDebugInfo` <span data-ttu-id="5ce27-414">은 2.x 릴리스 주기 동안 인터페이스에 대한 호환성이 손상되는 변경을 방지하기 위해 `IDbContextOptionsExtension`에서 확장된 추가 선택적 인터페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-414">was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

**<span data-ttu-id="5ce27-415">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-415">New behavior</span></span>**

<span data-ttu-id="5ce27-416">이제 인터페이스가 `IDbContextOptionsExtension`으로 병합됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-416">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

**<span data-ttu-id="5ce27-417">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-417">Why</span></span>**

<span data-ttu-id="5ce27-418">이 변경은 인터페이스가 개념적으로 하나이기 때문에 이루어졌습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-418">This change was made because the interfaces are conceptually one.</span></span>

**<span data-ttu-id="5ce27-419">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-419">Mitigations</span></span>**

<span data-ttu-id="5ce27-420">새 멤버를 지원하기 위해 `IDbContextOptionsExtension` 구현을 업데이트해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-420">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="5ce27-421">지연 로드 프록시는 더 이상 탐색 속성이 완전히 로드되었다고 가정하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-421">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="5ce27-422">추적 문제 #12780</span><span class="sxs-lookup"><span data-stu-id="5ce27-422">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="5ce27-423">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-423">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="5ce27-424">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-424">Old behavior</span></span>**

<span data-ttu-id="5ce27-425">EF Core 3.0 이전에는 `DbContext`가 삭제되면 해당 컨텍스트에서 가져온 엔터티의 지정된 탐색 속성이 완전히 로드되었는지 여부를 알 수 없었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-425">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="5ce27-426">대신 프록시는 참조 탐색에 null이 아닌 값이 있으면 로드되고 컬렉션 탐색이 비어 있지 않으면 로드된다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-426">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="5ce27-427">이러한 경우 지연 로드를 시도하면 아무런 작업도 수행되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-427">In these cases, attempting to lazy-load would be a no-op.</span></span>

**<span data-ttu-id="5ce27-428">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-428">New behavior</span></span>**

<span data-ttu-id="5ce27-429">EF Core 3.0부터 프록시는 탐색 속성이 로드되었는지 여부를 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-429">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="5ce27-430">즉, 컨텍스트가 삭제된 후에 로드되는 탐색 속성에 액세스하려고 하면 로드된 탐색이 비어 있거나 null인 경우에도 항상 아무런 작업도 수행되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-430">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="5ce27-431">반대로, 로드되지 않은 탐색 속성에 액세스하려고 하면 탐색 속성이 비어 있지 않은 컬렉션이라도 컨텍스트가 삭제되면 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-431">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="5ce27-432">이 상황이 발생하는 경우 애플리케이션 코드가 잘못된 시간에 지연 로드를 사용하려고 시도하고 있음을 의미하며, 애플리케이션이 이를 수행하지 않도록 변경해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-432">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

**<span data-ttu-id="5ce27-433">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-433">Why</span></span>**

<span data-ttu-id="5ce27-434">이 변경으로 인해 삭제된 `DbContext` 인스턴스에 지연 로드를 시도할 때 동작을 일관되고 정확하게 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-434">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

**<span data-ttu-id="5ce27-435">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-435">Mitigations</span></span>**

<span data-ttu-id="5ce27-436">애플리케이션을 업데이트하여 삭제된 컨텍스트로 지연 로드를 시도하지 않도록 하거나 예외 메시지에 설명된 대로 작업을 수행하지 않도록 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-436">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="5ce27-437">내부 서비스 공급자의 과도한 생성은 이제 기본적으로 오류입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-437">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="5ce27-438">추적 문제 #10236</span><span class="sxs-lookup"><span data-stu-id="5ce27-438">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="5ce27-439">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-439">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="5ce27-440">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-440">Old behavior</span></span>**

<span data-ttu-id="5ce27-441">EF Core 3.0 이전에는 병리적인 수의 내부 서비스 공급자를 만드는 애플리케이션에 경고가 기록되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-441">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

**<span data-ttu-id="5ce27-442">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-442">New behavior</span></span>**

<span data-ttu-id="5ce27-443">EF Core 3.0부터 이제 이 경고가 고려되며 오류와 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-443">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

**<span data-ttu-id="5ce27-444">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-444">Why</span></span>**

<span data-ttu-id="5ce27-445">이 변경으로 인해 병리적인 사례를 더 명시적으로 노출하여 더 나은 애플리케이션 코드를 구동하게 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-445">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

**<span data-ttu-id="5ce27-446">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-446">Mitigations</span></span>**

<span data-ttu-id="5ce27-447">이 오류가 발생했을 때 가장 적절한 조치는 근본 원인을 이해하고 많은 내부 서비스 공급자를 만드는 것을 중단하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-447">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="5ce27-448">그러나 오류는 `DbContextOptionsBuilder`의 구성을 통해 경고(또는 무시)로 다시 변환될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-448">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="5ce27-449">예:</span><span class="sxs-lookup"><span data-stu-id="5ce27-449">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="5ce27-450">단일 문자열로 호출되는 HasOne/HasMany의 새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-450">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="5ce27-451">추적 문제 #9171</span><span class="sxs-lookup"><span data-stu-id="5ce27-451">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="5ce27-452">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-452">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="5ce27-453">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-453">Old behavior</span></span>**

<span data-ttu-id="5ce27-454">EF Core 3.0 이전에는 단일 문자열을 사용하여 `HasOne` 또는 `HasMany`를 호출하는 코드가 혼란스러운 방식으로 해석되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-454">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="5ce27-455">예:</span><span class="sxs-lookup"><span data-stu-id="5ce27-455">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="5ce27-456">코드는 비공개일 수 있는 `Entrance` 탐색 속성을 사용하여 `Samurai`를 다른 엔터티 형식과 관련시키려는 것 같습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-456">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="5ce27-457">실제로 이 코드는 탐색 속성을 사용하지 않고 `Entrance`라고 하는 엔터티 형식과 관계를 생성하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-457">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

**<span data-ttu-id="5ce27-458">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-458">New behavior</span></span>**

<span data-ttu-id="5ce27-459">EF Core 3.0부터 이제 위 코드는 이전에 수행했어야 하는 것처럼 보이는 방식으로 동작합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-459">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

**<span data-ttu-id="5ce27-460">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-460">Why</span></span>**

<span data-ttu-id="5ce27-461">이전 동작은 특히 구성 코드를 읽고 오류를 찾을 때 매우 혼란스럽습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-461">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

**<span data-ttu-id="5ce27-462">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-462">Mitigations</span></span>**

<span data-ttu-id="5ce27-463">새 동작은 형식 이름의 문자열을 사용하고 탐색 속성을 명시적으로 지정하지 않은 채 명시적으로 관계를 구성하는 애플리케이션만 중단됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-463">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="5ce27-464">이는 일반적이지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-464">This is not common.</span></span>
<span data-ttu-id="5ce27-465">이전 동작은 탐색 속성 이름에 대해 `null`을 전달하여 명시적으로 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-465">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="5ce27-466">예:</span><span class="sxs-lookup"><span data-stu-id="5ce27-466">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="5ce27-467">여러 비동기 메서드의 반환 형식이 작업에서 ValueTask로 변경되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-467">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="5ce27-468">추적 문제 #15184</span><span class="sxs-lookup"><span data-stu-id="5ce27-468">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="5ce27-469">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-469">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="5ce27-470">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-470">Old behavior</span></span>**

<span data-ttu-id="5ce27-471">다음 비동기 메서드는 이전에 `Task<T>`를 반환했습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-471">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* `ValueGenerator.NextValueAsync()` <span data-ttu-id="5ce27-472">(그리고 파생 클래스)</span><span class="sxs-lookup"><span data-stu-id="5ce27-472">(and deriving classes)</span></span>

**<span data-ttu-id="5ce27-473">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-473">New behavior</span></span>**

<span data-ttu-id="5ce27-474">앞서 언급한 메서드는 이제 이전과 같은 `T`를 통해 `ValueTask<T>`를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-474">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

**<span data-ttu-id="5ce27-475">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-475">Why</span></span>**

<span data-ttu-id="5ce27-476">이 변경은 해당 메서드를 호출할 때 발생하는 힙 할당 수를 줄여서 일반적인 성능을 개선합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-476">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

**<span data-ttu-id="5ce27-477">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-477">Mitigations</span></span>**

<span data-ttu-id="5ce27-478">단순히 위의 API를 대기 중인 애플리케이션은 다시 컴파일하기만 하면 됩니다. 소스 변경은 필요 없습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-478">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="5ce27-479">더 복잡한 사용(예: 반환된 `Task`를 `Task.WhenAny()`에 전달)의 경우 일반적으로 반환된 `ValueTask<T>`에 대해 `AsTask()`를 호출하여 `Task<T>`로 변환해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-479">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="5ce27-480">이렇게 하면 이 변경으로 인한 할당 감소가 무효화됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-480">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="5ce27-481">관계형:TypeMapping 주석은 이제 TypeMapping일 뿐입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-481">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="5ce27-482">추적 문제 #9913</span><span class="sxs-lookup"><span data-stu-id="5ce27-482">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="5ce27-483">이 변경 내용은 EF Core 3.0 미리 보기 2에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-483">This change was introduced in EF Core 3.0-preview 2.</span></span>

**<span data-ttu-id="5ce27-484">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-484">Old behavior</span></span>**

<span data-ttu-id="5ce27-485">형식 매핑 주석에 대한 주석 이름은 "관계형:TypeMapping"이었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-485">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

**<span data-ttu-id="5ce27-486">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-486">New behavior</span></span>**

<span data-ttu-id="5ce27-487">형식 매핑 주석에 대한 주석 이름은 이제 "TypeMapping"입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-487">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

**<span data-ttu-id="5ce27-488">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-488">Why</span></span>**

<span data-ttu-id="5ce27-489">이제 형식 매핑은 관계형 데이터베이스 공급자 이상의 용도로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-489">Type mappings are now used for more than just relational database providers.</span></span>

**<span data-ttu-id="5ce27-490">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-490">Mitigations</span></span>**

<span data-ttu-id="5ce27-491">이는 일반적이지 않은 주석으로 형식 매핑에 직접 액세스하는 애플리케이션만 중단합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-491">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="5ce27-492">수정하기 위한 가장 적절한 작업은 직접 주석을 사용하는 대신 API 화면을 사용하여 형식 매핑에 액세스하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-492">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="5ce27-493">파생된 형식의 ToTable에서 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-493">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="5ce27-494">추적 문제 #11811</span><span class="sxs-lookup"><span data-stu-id="5ce27-494">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="5ce27-495">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-495">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="5ce27-496">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-496">Old behavior</span></span>**

<span data-ttu-id="5ce27-497">EF Core 3.0 이전에는 유효하지 않은 경우 상속 매핑 전략만 TPH이므로 파생된 형식에 대해 호출된 `ToTable()`은 무시되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-497">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

**<span data-ttu-id="5ce27-498">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-498">New behavior</span></span>**

<span data-ttu-id="5ce27-499">EF Core 3.0부터 시작하여 이후 릴리스에서 TPT 및 TPC 지원을 추가하기 위해 파생 형식에서 호출된 `ToTable()`에서는 나중에 예기치 않은 매핑 변화를 방지하기 위해 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-499">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

**<span data-ttu-id="5ce27-500">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-500">Why</span></span>**

<span data-ttu-id="5ce27-501">현재 파생된 형식을 다른 테이블에 매핑하는 것은 올바르지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-501">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="5ce27-502">이 변경으로 인해 할 일이 유효하게 될 때 나중에 중단되는 것을 방지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-502">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

**<span data-ttu-id="5ce27-503">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-503">Mitigations</span></span>**

<span data-ttu-id="5ce27-504">파생된 형식을 다른 테이블에 매핑하려는 시도를 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-504">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="5ce27-505">ForSqlServerHasIndex가 HasIndex로 바뀝니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-505">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="5ce27-506">추적 문제 #12366</span><span class="sxs-lookup"><span data-stu-id="5ce27-506">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="5ce27-507">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-507">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="5ce27-508">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-508">Old behavior</span></span>**

<span data-ttu-id="5ce27-509">EF Core 3.0 이전에는 `ForSqlServerHasIndex().ForSqlServerInclude()`가 `INCLUDE`와 함께 사용되는 열을 구성하는 방법을 제공했습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-509">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

**<span data-ttu-id="5ce27-510">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-510">New behavior</span></span>**

<span data-ttu-id="5ce27-511">EF Core 3.0부터 인덱스에 `Include`를 사용하여 이제 관계형 수준에서 지원됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-511">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="5ce27-512">`HasIndex().ForSqlServerInclude()`을 사용하십시오.</span><span class="sxs-lookup"><span data-stu-id="5ce27-512">Use `HasIndex().ForSqlServerInclude()`.</span></span>

**<span data-ttu-id="5ce27-513">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-513">Why</span></span>**

<span data-ttu-id="5ce27-514">이 변경으로 인해 `Includes`가 있는 인덱스의 API를 모든 데이터베이스 공급자에 대해 한 곳으로 통합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-514">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

**<span data-ttu-id="5ce27-515">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-515">Mitigations</span></span>**

<span data-ttu-id="5ce27-516">위에 표시된 것처럼 새 API를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-516">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="5ce27-517">EF Core는 더 이상 SQLite FK 적용을 위한 pragma를 보내지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-517">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="5ce27-518">추적 문제 #12151</span><span class="sxs-lookup"><span data-stu-id="5ce27-518">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="5ce27-519">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-519">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="5ce27-520">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-520">Old behavior</span></span>**

<span data-ttu-id="5ce27-521">EF Core 3.0 이전에는 EF Core가 SQLite에 대한 연결이 열릴 때 `PRAGMA foreign_keys = 1`을 보냈습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-521">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

**<span data-ttu-id="5ce27-522">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-522">New behavior</span></span>**

<span data-ttu-id="5ce27-523">EF Core 3.0부터 EF Core는 더 이상 SQLite에 대한 연결이 열릴 때 `PRAGMA foreign_keys = 1`을 보내지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-523">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

**<span data-ttu-id="5ce27-524">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-524">Why</span></span>**

<span data-ttu-id="5ce27-525">이 변경은 EF Core가 기본적으로 `SQLitePCLRaw.bundle_e_sqlite3`을 사용하기 때문에 이루어졌으며, 이는 FK 적용이 기본적으로 켜져 있고 연결이 열릴 때마다 명시적으로 활성화할 필요가 없음을 의미합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-525">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

**<span data-ttu-id="5ce27-526">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-526">Mitigations</span></span>**

<span data-ttu-id="5ce27-527">외래 키는 기본적으로 EF Core에 사용되는 SQLitePCLRaw.bundle_e_sqlite3에서 기본적으로 활성화됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-527">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="5ce27-528">다른 경우에는 연결 문자열에 `Foreign Keys=True`를 지정하여 외래 키를 활성화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-528">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="5ce27-529">Microsoft.EntityFrameworkCore.Sqlite는 이제 SQLitePCLRaw.bundle_e_sqlite3에 종속됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-529">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

**<span data-ttu-id="5ce27-530">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-530">Old behavior</span></span>**

<span data-ttu-id="5ce27-531">EF Core 3.0 이전에는 EF Core가 `SQLitePCLRaw.bundle_green`을 사용했습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-531">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

**<span data-ttu-id="5ce27-532">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-532">New behavior</span></span>**

<span data-ttu-id="5ce27-533">EF Core 3.0부터 EF Core가 `SQLitePCLRaw.bundle_e_sqlite3`을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-533">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

**<span data-ttu-id="5ce27-534">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-534">Why</span></span>**

<span data-ttu-id="5ce27-535">이 변경으로 인해 iOS에서 사용되는 SQLite의 버전이 다른 플랫폼과 일치되도록 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-535">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

**<span data-ttu-id="5ce27-536">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-536">Mitigations</span></span>**

<span data-ttu-id="5ce27-537">iOS에서 네이티브 SQLite 버전을 사용하려면 다른 `SQLitePCLRaw` 번들을 사용하도록 `Microsoft.Data.Sqlite`를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-537">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="5ce27-538">Guid 값은 이제 SQLite에 텍스트로 저장되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-538">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="5ce27-539">추적 문제 #15078</span><span class="sxs-lookup"><span data-stu-id="5ce27-539">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="5ce27-540">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-540">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="5ce27-541">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-541">Old behavior</span></span>**

<span data-ttu-id="5ce27-542">Guid 값은 이전에 SQLite에 BLOB 값으로 저장되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-542">Guid values were previously sored as BLOB values on SQLite.</span></span>

**<span data-ttu-id="5ce27-543">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-543">New behavior</span></span>**

<span data-ttu-id="5ce27-544">Guid 값은 이제 텍스트로 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-544">Guid values are now sotred as TEXT.</span></span>

**<span data-ttu-id="5ce27-545">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-545">Why</span></span>**

<span data-ttu-id="5ce27-546">Guid의 이진 형식은 표준화되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-546">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="5ce27-547">값을 텍스트로 저장하면 데이터베이스가 다른 기술과 더 잘 호환됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-547">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

**<span data-ttu-id="5ce27-548">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-548">Mitigations</span></span>**

<span data-ttu-id="5ce27-549">다음과 같이 SQL을 실행하여 기존 데이터베이스를 새 형식으로 마이그레이션할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-549">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

<span data-ttu-id="5ce27-550">EF Core에서는 이러한 속성에 값 변환기를 구성하여 이전 동작을 계속 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-550">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="5ce27-551">Microsoft.Data.Sqlite는 BLOB 및 텍스트 열 모두에서 Guid 값을 읽을 수 있습니다. 하지만 매개 변수 및 상수에 대한 기본 형식이 변경되었으므로 Guid를 포함하는 대부분의 시나리오에서 조치를 취해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-551">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="5ce27-552">Char 값은 이제 SQLite에 텍스트로 저장됨</span><span class="sxs-lookup"><span data-stu-id="5ce27-552">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="5ce27-553">추적 문제 #15020</span><span class="sxs-lookup"><span data-stu-id="5ce27-553">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="5ce27-554">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-554">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="5ce27-555">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-555">Old behavior</span></span>**

<span data-ttu-id="5ce27-556">Char 값은 이전에 SQLite에 정수 값으로 저장되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-556">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="5ce27-557">예를 들어, *A*의 char 값은 정수 값 65로 저장되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-557">For example, a char value of *A* was stored as the integer value 65.</span></span>

**<span data-ttu-id="5ce27-558">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-558">New behavior</span></span>**

<span data-ttu-id="5ce27-559">Char 값은 이제 텍스트로 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-559">Char values are now sotred as TEXT.</span></span>

**<span data-ttu-id="5ce27-560">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-560">Why</span></span>**

<span data-ttu-id="5ce27-561">값을 텍스트로 저장하는 것이 더 자연스럽고 데이터베이스가 다른 기술과 더 잘 호환됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-561">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

**<span data-ttu-id="5ce27-562">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-562">Mitigations</span></span>**

<span data-ttu-id="5ce27-563">다음과 같이 SQL을 실행하여 기존 데이터베이스를 새 형식으로 마이그레이션할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-563">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="5ce27-564">EF Core에서는 이러한 속성에 값 변환기를 구성하여 이전 동작을 계속 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-564">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="5ce27-565">또한 Microsoft.Data.Sqlite는 정수 및 텍스트 열에서 문자 값을 읽을 수 있으므로 특정 시나리오에서는 아무런 조치도 필요하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-565">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="5ce27-566">이제 마이그레이션 ID가 고정 문화권의 달력을 사용하여 생성됨</span><span class="sxs-lookup"><span data-stu-id="5ce27-566">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="5ce27-567">추적 문제 #12978</span><span class="sxs-lookup"><span data-stu-id="5ce27-567">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="5ce27-568">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-568">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="5ce27-569">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-569">Old behavior</span></span>**

<span data-ttu-id="5ce27-570">마이그레이션 ID는 현재 문화권의 달력을 사용하여 의도치 않게 생성되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-570">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

**<span data-ttu-id="5ce27-571">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-571">New behavior</span></span>**

<span data-ttu-id="5ce27-572">이제 마이그레이션 ID는 항상 고정 문화권의 달력(그레고리력)을 사용하여 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-572">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

**<span data-ttu-id="5ce27-573">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-573">Why</span></span>**

<span data-ttu-id="5ce27-574">데이터베이스를 업데이트하거나 병합 충돌을 해결할 때 마이그레이션 순서가 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-574">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="5ce27-575">고정 달력을 사용하면 팀 멤버가 서로 다른 시스템 캘린더로 인해 발생할 수 있는 주문 문제를 방지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-575">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

**<span data-ttu-id="5ce27-576">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-576">Mitigations</span></span>**

<span data-ttu-id="5ce27-577">이 변경은 그레고리력(예: 태국 불교식 달력)보다 큰 년도가 있는 그레고리력이 아닌 달력을 사용하는 사용자에게 영향을 줍니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-577">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="5ce27-578">기존 마이그레이션 ID를 업데이트하여 기존 마이그레이션 후 새 마이그레이션 순서를 지정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-578">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="5ce27-579">마이그레이션 ID는 마이그레이션 디자이너 파일의 마이그레이션 속성에서 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-579">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="5ce27-580">마이그레이션 기록 테이블도 업데이트해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-580">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="5ce27-581">LogQueryPossibleExceptionWithAggregateOperator 이름이 바뀌었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-581">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="5ce27-582">추적 문제 #10985</span><span class="sxs-lookup"><span data-stu-id="5ce27-582">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="5ce27-583">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-583">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="5ce27-584">변경</span><span class="sxs-lookup"><span data-stu-id="5ce27-584">Change</span></span>**

`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` <span data-ttu-id="5ce27-585">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`으로 이름이 변경되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-585">has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

**<span data-ttu-id="5ce27-586">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-586">Why</span></span>**

<span data-ttu-id="5ce27-587">이 경고 이벤트의 이름 지정을 다른 모든 경고 이벤트에 맞춥니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-587">Aligns the naming of this warning event with all other warning events.</span></span>

**<span data-ttu-id="5ce27-588">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-588">Mitigations</span></span>**

<span data-ttu-id="5ce27-589">새 이름을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-589">Use the new name.</span></span> <span data-ttu-id="5ce27-590">(이벤트 ID 번호가 변경되지 않았습니다.)</span><span class="sxs-lookup"><span data-stu-id="5ce27-590">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="5ce27-591">외래 키 제약 조건 이름에 대한 API를 명확히 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-591">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="5ce27-592">추적 문제 #10730</span><span class="sxs-lookup"><span data-stu-id="5ce27-592">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="5ce27-593">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-593">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="5ce27-594">이전 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-594">Old behavior</span></span>**

<span data-ttu-id="5ce27-595">EF Core 3.0 이전에 외래 키 제약 조건 이름은 "이름"과 같이 간단했습니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-595">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="5ce27-596">예:</span><span class="sxs-lookup"><span data-stu-id="5ce27-596">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

**<span data-ttu-id="5ce27-597">새 동작</span><span class="sxs-lookup"><span data-stu-id="5ce27-597">New behavior</span></span>**

<span data-ttu-id="5ce27-598">이제 EF Core 3.0부터 외래 키 제약 조건 이름은 "제약 조건 이름"이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-598">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="5ce27-599">예:</span><span class="sxs-lookup"><span data-stu-id="5ce27-599">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

**<span data-ttu-id="5ce27-600">이유</span><span class="sxs-lookup"><span data-stu-id="5ce27-600">Why</span></span>**

<span data-ttu-id="5ce27-601">이러한 변경으로 인해 이 영역에서 이름을 일관되게 지정하고, 또한 일관되게 외래 키가 정의된 열 또는 속성 이름이 아닌 외래 키 제약 조건의 이름임을 명확히 합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-601">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

**<span data-ttu-id="5ce27-602">완화 방법</span><span class="sxs-lookup"><span data-stu-id="5ce27-602">Mitigations</span></span>**

<span data-ttu-id="5ce27-603">새 이름을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="5ce27-603">Use the new name.</span></span>
