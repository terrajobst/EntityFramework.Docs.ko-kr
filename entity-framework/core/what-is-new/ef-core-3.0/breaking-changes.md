---
title: EF Core 3.0의 호환성이 손상되는 변경 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 7cc0bd3946be2e63d9fb46a023bf6abe750ae0e3
ms.sourcegitcommit: e90d6cfa3e96f10b8b5275430759a66a0c714ed1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286490"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="059e1-102">EF Core 3.0에 포함된 호환성이 손상되는 변경(현재 미리 보기 상태)</span><span class="sxs-lookup"><span data-stu-id="059e1-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="059e1-103">이후 릴리스의 기능 집합 및 일정은 항상 변경될 수 있으며 이 페이지를 최신 상태로 유지하려고 노력함에도 불구하고 최신 계획을 항상 반영할 수 없을 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="059e1-104">다음 API 및 동작 변경은 3.0.0으로 업그레이드할 때 EF Core 2.2.x용으로 개발된 애플리케이션을 중단시킬 가능성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="059e1-105">데이터베이스 공급자에만 영향을 줄 것으로 예상되는 변경 내용은 [공급자 변경](../../providers/provider-log.md)에 설명되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="059e1-106">3\.0 미리 보기 간의 도입된 새로운 기능의 중단은 여기에 설명되어 있지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="059e1-107">LINQ 쿼리는 더 이상 클라이언트에서 평가되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="059e1-108">[추적 문제 #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[문제 #12795도 참조](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="059e1-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="059e1-109">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-109">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-110">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-110">**Old behavior**</span></span>

<span data-ttu-id="059e1-111">3\.0 이전에는 EF Core가 쿼리의 일부인 식을 SQL 또는 매개 변수로 변환할 수 없었을 때 클라이언트에서 자동으로 식을 계산했습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="059e1-112">기본적으로, 잠재적 비용이 많이 드는 식에 대한 클라이언트 평가는 경고만 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="059e1-113">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-113">**New behavior**</span></span>

<span data-ttu-id="059e1-114">3\.0부터 EF Core는 최상위 수준 프로젝션(쿼리의 마지막 `Select()` 호출)의 식만 클라이언트에서 평가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="059e1-115">쿼리의 다른 부분에서 식을 SQL 또는 매개 변수로 변환할 수 없을 때 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="059e1-116">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-116">**Why**</span></span>

<span data-ttu-id="059e1-117">쿼리의 자동 클라이언트 평가는 중요한 부분을 변환할 수 없어도 많은 쿼리를 실행할 수 있게 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="059e1-118">이 동작으로 인해 프로덕션 환경에서 분명하게 나타날 수 있는 예기치 않은 잠재적 손상을 초래할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="059e1-119">예를 들어 변환할 수 없는 `Where()` 호출의 조건으로 인해 테이블의 모든 행이 데이터베이스 서버에서 전송되고 필터가 클라이언트에 적용될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="059e1-120">이 경우 테이블에 개발 중인 몇 개 행만 포함하지만 애플리케이션이 수백만 개의 행이 포함될 수 있는 프로덕션으로 이동할 때 쉽게 감지되지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="059e1-121">클라이언트 평가 경고도 개발 중에 너무 쉽게 무시됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="059e1-122">이 외에도 자동 클라이언트 평가는 특정 식에 대한 쿼리 변환 기능 개선으로 인해 릴리스 간의 의도하지 않은 호환성이 손상되는 변경이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="059e1-123">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-123">**Mitigations**</span></span>

<span data-ttu-id="059e1-124">쿼리를 완벽하게 변환할 수 없는 경우 변환할 수 있는 형식으로 쿼리를 다시 작성하거나 `AsEnumerable()`, `ToList()` 또는 유사한 형식을 사용하여 명시적으로 데이터를 클라이언트로 가져오면 LINQ to Objects를 사용하여 추가로 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="059e1-125">Entity Framework Core는 더 이상 ASP.NET Core 공유 프레임워크에 포함되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="059e1-126">추적 문제 공지 사항 #325</span><span class="sxs-lookup"><span data-stu-id="059e1-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="059e1-127">이 변경 내용은 ASP.NET Core 3.0 미리 보기 1에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-127">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="059e1-128">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-128">**Old behavior**</span></span>

<span data-ttu-id="059e1-129">ASP.NET Core 3.0 이전에는 `Microsoft.AspNetCore.App` 또는 `Microsoft.AspNetCore.All`에 대한 패키지 참조를 추가하면 EF Core 및 SQL Server 공급자와 같은 일부 EF Core 데이터 공급자가 포함되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="059e1-130">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-130">**New behavior**</span></span>

<span data-ttu-id="059e1-131">3\.0부터 ASP.NET Core 공유 프레임워크에는 EF Core 또는 EF Core 데이터 공급자가 포함되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="059e1-132">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-132">**Why**</span></span>

<span data-ttu-id="059e1-133">이 변경 전에 EF Core를 가져오려면 애플리케이션이 ASP.NET Core 및 SQL Server를 대상으로 했는지 여부에 따라 다른 단계가 필요했습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="059e1-134">또한 ASP.NET Core를 업그레이드 하면 EF Core 및 SQL Server 업그레이드가 강제되었는데, 이는 항상 바람직하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="059e1-135">이 변경으로 인해 EF Core를 가져오는 환경은 모든 공급자, 지원되는 .NET 구현 및 애플리케이션 유형에서 동일합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="059e1-136">또한 개발자는 이제 EF Core 및 EF Core 데이터 공급자의 업그레이드 시기를 정확히 제어할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="059e1-137">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-137">**Mitigations**</span></span>

<span data-ttu-id="059e1-138">ASP.NET Core 3.0 애플리케이션 또는 기타 지원되는 애플리케이션에서 EF Core를 사용하려면 애플리케이션에서 사용할 EF Core 데이터베이스 공급자에 대한 패키지 참조를 명시적으로 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="059e1-139">EF Core 명령줄 도구인 dotnet ef는 더 이상 .NET Core SDK의 일부가 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="059e1-140">추적 문제 #14016</span><span class="sxs-lookup"><span data-stu-id="059e1-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="059e1-141">이 변경 내용은 EF Core 3.0 미리 보기 4 및 .NET Core SDK의 해당 버전에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-141">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="059e1-142">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-142">**Old behavior**</span></span>

<span data-ttu-id="059e1-143">3\.0 이전에는 `dotnet ef` 도구가 .NET Core SDK에 포함되었으며 추가 단계 없이도 모든 프로젝트의 명령줄에서 쉽게 사용할 수 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="059e1-144">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-144">**New behavior**</span></span>

<span data-ttu-id="059e1-145">3\.0부터 .NET SDK에 `dotnet ef` 도구가 포함되어 있지 않으므로 사용하기 전에 명시적으로 로컬 또는 글로벌 도구로 설치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="059e1-146">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-146">**Why**</span></span>

<span data-ttu-id="059e1-147">이 변경으로 인해 `dotnet ef`을 NuGet에서 일반 .NET CLI 도구로 배포 및 업데이트할 수 있으며, EF Core 3.0도 항상 NuGet 패키지로 배포된다는 사실과 일치합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="059e1-148">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-148">**Mitigations**</span></span>

<span data-ttu-id="059e1-149">마이그레이션을 관리하거나 `DbContext`의 스캐폴드를 수행하려면 `dotnet-ef`를 전역 도구로 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="059e1-150">[도구 매니페스트 파일](https://github.com/dotnet/cli/issues/10288)을 사용하여 도구 종속성으로 선언한 프로젝트의 종속성을 복원할 때도 로컬 도구를 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-150">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="059e1-151">FromSql, ExecuteSql, ExecuteSqlAsync의 이름이 변경되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-151">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="059e1-152">추적 문제 #10996</span><span class="sxs-lookup"><span data-stu-id="059e1-152">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="059e1-153">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-153">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-154">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-154">**Old behavior**</span></span>

<span data-ttu-id="059e1-155">EF Core 3.0이 나오기 전에는 이런 메서드 이름이 일반 문자열 또는 SQL 및 매개 변수로 보간해야 하는 문자열에 대해 작동하도록 오버로드되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-155">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="059e1-156">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-156">**New behavior**</span></span>

<span data-ttu-id="059e1-157">EF Core 3.0부터는 `FromSqlRaw`, `ExecuteSqlRaw` 및 `ExecuteSqlRawAsync`를 사용하여 매개 변수를 생성합니다. 여기서 매개 변수는 보간된 쿼리 문자열의 일부로서 전달됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-157">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="059e1-158">예:</span><span class="sxs-lookup"><span data-stu-id="059e1-158">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="059e1-159">`FromSqlInterpolated`, `ExecuteSqlInterpolated` 및 `ExecuteSqlInterpolatedAsync`를 사용하여 매개 변수를 생성합니다. 여기서 매개 변수는 보간된 쿼리 문자열의 일부로서 전달됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-159">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="059e1-160">예:</span><span class="sxs-lookup"><span data-stu-id="059e1-160">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="059e1-161">위의 두 쿼리 모두 같은 SQL 매개 변수를 사용하여 같은 매개 변수가 있는 SQL을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-161">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="059e1-162">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-162">**Why**</span></span>

<span data-ttu-id="059e1-163">이와 같은 메서드 오버로드를 적용할 경우 보간된 문자열 메서드를 호출하려다 실수로 원시 문자열 메서드를 호출하거나, 반대로 후자를 호출하려다 전자를 호출하는 실수를 저지를 가능성이 매우 높습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-163">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="059e1-164">쿼리에는 매개 변수가 있어야 하는데, 이 경우 매개 변수가 없는 쿼리가 나올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-164">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="059e1-165">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-165">**Mitigations**</span></span>

<span data-ttu-id="059e1-166">새 메서드 이름을 사용하도록 전환합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-166">Switch to use the new method names.</span></span>

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="059e1-167">FromSql 메서드는 쿼리 루트에만 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-167">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="059e1-168">추적 이슈 #15704</span><span class="sxs-lookup"><span data-stu-id="059e1-168">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="059e1-169">이 변경 내용은 EF Core 3.0 미리 보기 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-169">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="059e1-170">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-170">**Old behavior**</span></span>

<span data-ttu-id="059e1-171">EF Core 3.0 이전에는 `FromSql` 메서드를 쿼리의 아무 곳에나 지정할 수 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-171">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="059e1-172">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-172">**New behavior**</span></span>

<span data-ttu-id="059e1-173">EF Core 3.0부터는 새로운 `FromSqlRaw` 및 `FromSqlInterpolated` 메서드(`FromSql` 대체)만 쿼리 루트에 지정할 수 있습니다(예: `DbSet<>`에 직접).</span><span class="sxs-lookup"><span data-stu-id="059e1-173">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="059e1-174">다른 위치에 지정하려고 하면 컴파일 오류가 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-174">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="059e1-175">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-175">**Why**</span></span>

<span data-ttu-id="059e1-176">`DbSet`가 아닌 위치에 `FromSql`을 지정하는 것은 추가 의미 또는 추가 가치가 없으므로 특정 시나리오에서 모호성을 발생시킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-176">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="059e1-177">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-177">**Mitigations**</span></span>

<span data-ttu-id="059e1-178">`FromSql` 호출은 이 호출이 적용되는 `DbSet`로 직접 이동해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-178">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

## <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="059e1-179">~~쿼리 실행은 디버그 수준에서 로깅됩니다.~~ 되돌림</span><span class="sxs-lookup"><span data-stu-id="059e1-179">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="059e1-180">추적 문제 #14523</span><span class="sxs-lookup"><span data-stu-id="059e1-180">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="059e1-181">이 변경 내용은 EF Core 3.0 미리 보기 7에서 되돌려집니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-181">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="059e1-182">EF Core 3.0의 새 구성으로 모든 이벤트의 로그 수준을 애플리케이션에서 지정할 수 있기 때문에 이 변경 내용을 되돌렸습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-182">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="059e1-183">예를 들어 SQL의 로깅을 `Debug`로 전환하고 `OnConfiguring` 또는 `AddDbContext`에서 명시적으로 수준을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-183">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="059e1-184">임시 키 값은 더 이상 엔터티 인스턴스에 설정되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-184">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="059e1-185">추적 문제 #12378</span><span class="sxs-lookup"><span data-stu-id="059e1-185">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="059e1-186">이 변경 내용은 EF Core 3.0 미리 보기 2에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-186">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="059e1-187">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-187">**Old behavior**</span></span>

<span data-ttu-id="059e1-188">EF Core 3.0 이전에는 임시 값이 데이터베이스에서 생성된 실제 값을 나중에 가질 수 있는 모든 키 속성에 할당되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-188">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="059e1-189">일반적으로 이러한 임시 값은 큰 음수입니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-189">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="059e1-190">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-190">**New behavior**</span></span>

<span data-ttu-id="059e1-191">3\.0부터 EF Core는 엔터티의 추적 정보의 일부로 임시 키 값을 저장하고, 키 속성 자체는 변경하지 않고 그대로 둡니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-191">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="059e1-192">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-192">**Why**</span></span>

<span data-ttu-id="059e1-193">이 변경은 일부 `DbContext` 인스턴스의 의해 이전에 추적된 엔터티가 다른 `DbContext` 인스턴스로 이동할 때 임시 키 값이 틀리게 영구화되지 않도록 하기 위해 수행되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-193">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="059e1-194">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-194">**Mitigations**</span></span>

<span data-ttu-id="059e1-195">엔터티 간의 연결을 형성하기 위해 외래 키에 기본 키 값을 할당하는 애플리케이션은 기본 키가 저장 생성되고 `Added` 상태의 엔터티에 속하는 경우 이전 동작에 따라 달라질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-195">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="059e1-196">이는 다음과 같은 방법으로 방지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-196">This can be avoided by:</span></span>
* <span data-ttu-id="059e1-197">저장 생성 키를 사용하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-197">Not using store-generated keys.</span></span>
* <span data-ttu-id="059e1-198">외래 키 값을 설정하는 대신 관계를 형성하도록 탐색 속성을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-198">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="059e1-199">엔터티의 추적 정보에서 실제 임시 키 값을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-199">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="059e1-200">예를 들어 `context.Entry(blog).Property(e => e.Id).CurrentValue`는 `blog.Id` 자체가 설정되지 않았더라도 임시 값을 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-200">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="059e1-201">DetectChanges는 저장 생성 키 값을 준수합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-201">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="059e1-202">추적 문제 #14616</span><span class="sxs-lookup"><span data-stu-id="059e1-202">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="059e1-203">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-203">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="059e1-204">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-204">**Old behavior**</span></span>

<span data-ttu-id="059e1-205">EF Core 3.0 이전에는 `DetectChanges`에서 찾은 추적되는 않은 엔터티를 `Added` 상태에서 추적하여 `SaveChanges`가 호출될 때 새로운 행으로 삽입했습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-205">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="059e1-206">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-206">**New behavior**</span></span>

<span data-ttu-id="059e1-207">EF Core 3.0부터 생성된 키 값을 사용하고 일부 키 값을 설정한 경우 `Modified` 상태에서 엔터티를 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-207">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="059e1-208">즉, 엔터티에 대한 행이 있는 것으로 가정하고 `SaveChanges`가 호출될 때 업데이트됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-208">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="059e1-209">키 값이 설정되지 않았거나 엔터티 형식이 생성된 키를 사용하지 않는 경우, 새 엔터티는 이전 버전과 마찬가지로 `Added`로 추적됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-209">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="059e1-210">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-210">**Why**</span></span>

<span data-ttu-id="059e1-211">이 변경으로 인해 저장 생성 키를 사용하는 동안 연결이 분리된 엔터티 그래프를 사용하여 작업을 보다 쉽고 일관되게 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-211">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="059e1-212">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-212">**Mitigations**</span></span>

<span data-ttu-id="059e1-213">엔터티 형식이 생성된 키를 사용하도록 구성되었지만 키 값이 새 인스턴스에 명시적으로 설정된 경우, 이 변경으로 인해 애플리케이션이 중단될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-213">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="059e1-214">해결 방법은 생성된 값을 사용하지 않도록 키 속성을 명시적으로 구성하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-214">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="059e1-215">예를 들어 흐름 API가 있는 경우:</span><span class="sxs-lookup"><span data-stu-id="059e1-215">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="059e1-216">데이터 주석이 있는 경우:</span><span class="sxs-lookup"><span data-stu-id="059e1-216">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="059e1-217">계단식 삭제는 기본적으로 즉시 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-217">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="059e1-218">추적 문제 #10114</span><span class="sxs-lookup"><span data-stu-id="059e1-218">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="059e1-219">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-219">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="059e1-220">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-220">**Old behavior**</span></span>

<span data-ttu-id="059e1-221">3\.0 이전에는 SaveChanges가 호출될 때까지 EF Core에는 연계 작업(필요한 보안 주체가 삭제되거나 필요한 보안 주체에 대한 관계가 끊어질 때 종속 엔터티 삭제)이 적용되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-221">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="059e1-222">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-222">**New behavior**</span></span>

<span data-ttu-id="059e1-223">3\.0부터 EF Core는 트리거 조건이 검색되는 즉시 연계 동작을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-223">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="059e1-224">예를 들어, 보안 주체 엔터티를 삭제하기 위해 `context.Remove()`를 호출하면 추적된 모든 관련 필수 종속 항목도 즉시 `Deleted`로 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-224">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="059e1-225">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-225">**Why**</span></span>

<span data-ttu-id="059e1-226">이 변경으로 인해 `SaveChanges`가 호출되기 _전에_ 삭제될 엔터티를 이해하는 것이 중요한 데이터 바인딩 및 감사 시나리오 환경이 개선되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-226">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="059e1-227">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-227">**Mitigations**</span></span>

<span data-ttu-id="059e1-228">이전 동작은 `context.ChangedTracker`의 설정을 통해 복원할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-228">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="059e1-229">예:</span><span class="sxs-lookup"><span data-stu-id="059e1-229">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="059e1-230">DeleteBehavior.Restrict에는 명확한 의미 체계가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-230">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="059e1-231">추적 문제 #12661</span><span class="sxs-lookup"><span data-stu-id="059e1-231">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="059e1-232">이 변경 내용은 EF Core 3.0 미리 보기 5에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-232">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="059e1-233">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-233">**Old behavior**</span></span>

<span data-ttu-id="059e1-234">3\.0 이전에는 `DeleteBehavior.Restrict`가 `Restrict` 의미 체계를 사용하여 데이터베이스에 외래 키를 만들었지만, 확실치 않은 방식으로 fixup도 변경했습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-234">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="059e1-235">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-235">**New behavior**</span></span>

<span data-ttu-id="059e1-236">3\.0부터 `DeleteBehavior.Restrict`는 EF 내부 fixup에 영향을 주지 않고 `Restrict` 의미 체계(즉, 계단식 없음, 제약 조건 위반을 throw함)로 외부 키를 생성하도록 보장합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-236">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="059e1-237">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-237">**Why**</span></span>

<span data-ttu-id="059e1-238">이 변경은 예기치 않은 부작용 없이 직관적인 방식으로 `DeleteBehavior`를 사용하는 환경을 개선하기 위해 이루어졌습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-238">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="059e1-239">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-239">**Mitigations**</span></span>

<span data-ttu-id="059e1-240">이전 동작은 `DeleteBehavior.ClientNoAction`을 사용하여 복원할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-240">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="059e1-241">쿼리 형식은 엔터티 형식과 통합됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-241">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="059e1-242">추적 문제 #14194</span><span class="sxs-lookup"><span data-stu-id="059e1-242">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="059e1-243">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-243">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="059e1-244">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-244">**Old behavior**</span></span>

<span data-ttu-id="059e1-245">EF Core 3.0 이전에는 [쿼리 형식](xref:core/modeling/query-types)이 기본 키를 구조화된 방법으로 정의하지 않는 데이터를 쿼리하는 수단이었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-245">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="059e1-246">즉, 쿼리 형식은 키 없이 엔터티 형식을 매핑하는데 사용(보기에서 가능할 수도 있지만 테이블에서도 가능한 경우)되는 반면, 일반 엔터티 형식은 키가 사용 가능할 때(테이블에서 가능하지만 보기에서도 가능한 경우) 사용되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-246">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="059e1-247">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-247">**New behavior**</span></span>

<span data-ttu-id="059e1-248">이제 쿼리 형식은 기본 키가 없는 엔터티 형식이 될 뿐입니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-248">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="059e1-249">키 없는 엔터티 형식은 이전 버전의 쿼리 형식과 동일한 기능이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-249">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="059e1-250">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-250">**Why**</span></span>

<span data-ttu-id="059e1-251">이 변경으로 인해 쿼리 형식의 목적에 대한 혼동을 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-251">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="059e1-252">특히 키 없는 엔터티 형식이며 이로 인해 본질적으로 읽기 전용이지만 엔터티 형식이 읽기 전용으로 할 필요가 있다고 해서 사용해서는 안됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-252">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="059e1-253">마찬가지로 보기에 매핑되는 경우가 있지만 이는 보기가 키를 정의하지 않는 경우가 있기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-253">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="059e1-254">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-254">**Mitigations**</span></span>

<span data-ttu-id="059e1-255">API의 다음 부분은 이제 사용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-255">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="059e1-256">**`ModelBuilder.Query<>()`** - 대신 엔터티 형식을 키가 없는 것으로 표시하기 위해 `ModelBuilder.Entity<>().HasNoKey()`를 호출해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-256">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="059e1-257">기본 키가 필요하지만 규칙과 일치하지 않는 경우 잘못된 구성을 피하기 위해 규칙에 따라 구성되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-257">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="059e1-258">**`DbQuery<>`** - 대신 `DbSet<>`를 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-258">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="059e1-259">**`DbContext.Query<>()`** - 대신 `DbContext.Set<>()`를 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-259">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="059e1-260">소유 형식 관계에 대한 구성 API가 변경됨</span><span class="sxs-lookup"><span data-stu-id="059e1-260">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="059e1-261">[추적 문제 #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[추적 문제 #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[추적 문제 #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="059e1-261">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="059e1-262">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-262">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="059e1-263">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-263">**Old behavior**</span></span>

<span data-ttu-id="059e1-264">EF Core 3.0 이전에는 소유 관계의 구성은 `OwnsOne` 또는 `OwnsMany` 호출 직후에 수행되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-264">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="059e1-265">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-265">**New behavior**</span></span>

<span data-ttu-id="059e1-266">EF Core 3.0부터 이제 `WithOwner()`를 사용하여 소유자에게 탐색 속성을 구성하는 흐름 API가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-266">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="059e1-267">예:</span><span class="sxs-lookup"><span data-stu-id="059e1-267">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="059e1-268">이제 소유자와 소유 간의 관계와 관련된 구성이 다른 관계가 구성되는 것과 유사하게 `WithOwner()` 후에 연결되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-268">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="059e1-269">소유 형식 자체에 대한 구성은 `OwnsOne()/OwnsMany()` 이후에도 여전히 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-269">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="059e1-270">예:</span><span class="sxs-lookup"><span data-stu-id="059e1-270">For example:</span></span>

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

<span data-ttu-id="059e1-271">또한 소유 형식 대상으로 `Entity()`, `HasOne()` 또는 `Set()`를 호출하면 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-271">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="059e1-272">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-272">**Why**</span></span>

<span data-ttu-id="059e1-273">이 변경으로 인해 소유 형식 자체의 구성과 소유 형식의 _관계 대상_ 사이를 명확하게 분리하게 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-273">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="059e1-274">이는 `HasForeignKey`와 같은 메서드에 대한 모호성과 혼란을 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-274">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="059e1-275">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-275">**Mitigations**</span></span>

<span data-ttu-id="059e1-276">위의 예와 같이 소유 형식 관계의 구성을 변경하여 새 API 화면을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-276">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="059e1-277">보안 주체와 테이블을 공유하는 종속 엔터티는 이제 선택 사항입니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-277">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="059e1-278">추적 문제 #9005</span><span class="sxs-lookup"><span data-stu-id="059e1-278">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="059e1-279">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-279">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-280">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-280">**Old behavior**</span></span>

<span data-ttu-id="059e1-281">다음 모델을 살펴보세요.</span><span class="sxs-lookup"><span data-stu-id="059e1-281">Consider the following model:</span></span>
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
<span data-ttu-id="059e1-282">EF Core 3.0 전에는 `OrderDetails`를 `Order`가 소유하거나 같은 테이블에 명시적으로 매핑되는 경우 새 `Order`를 추가할 때 `OrderDetails` 인스턴스가 언제나 필수였습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-282">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="059e1-283">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-283">**New behavior**</span></span>

<span data-ttu-id="059e1-284">3\.0부터는 EF Core가 `OrderDetails` 없이 `Order`를 추가할 수 있으며 기본 키를 제외한 모든 `OrderDetails` 속성을 Null 허용 열에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-284">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="059e1-285">EF Core를 쿼리하는 경우 해당 필수 속성에 값이 없거나 기본 키 이외의 필수 속성이 없고 모든 속성이 `null`이면 `OrderDetails`를 `null`로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-285">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="059e1-286">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-286">**Mitigations**</span></span>

<span data-ttu-id="059e1-287">사용하는 모델이 모든 선택적 열에 따라 다르게 공유되는 테이블을 포함하지만 해당 테이블을 가리키는 탐색이 `null`로 예상되지 않는 경우 `null`을 탐색하는 경우를 처리하도록 애플리케이션을 수정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-287">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="059e1-288">이렇게 할 수 없는 경우 엔터티 형식에 필수 속성을 추가하거나 하나 이상의 속성에 `null`이 아닌 값을 할당해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-288">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="059e1-289">동시 토큰 열을 사용하여 테이블을 공유하는 모든 엔터티는 해당 열을 속성에 매핑해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-289">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="059e1-290">추적 문제 #14154</span><span class="sxs-lookup"><span data-stu-id="059e1-290">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="059e1-291">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-291">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-292">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-292">**Old behavior**</span></span>

<span data-ttu-id="059e1-293">다음 모델을 살펴보세요.</span><span class="sxs-lookup"><span data-stu-id="059e1-293">Consider the following model:</span></span>
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
<span data-ttu-id="059e1-294">EF Core 3.0 전에는 `OrderDetails`를 `Order`가 소유하거나 같은 테이블에 명시적으로 매핑하는 경우 `OrderDetails`만 업데이트하면 클라이언트의 `Version` 값이 업데이트되지 않으며 다음 업데이트가 실패합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-294">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="059e1-295">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-295">**New behavior**</span></span>

<span data-ttu-id="059e1-296">3\.0부터 EF Core는 `OrderDetails`를 소유하는 경우 새 `Version` 값을 `Order`에 전파합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-296">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="059e1-297">그렇지 않으면 모델 유효성 검사 중에 예외가 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-297">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="059e1-298">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-298">**Why**</span></span>

<span data-ttu-id="059e1-299">같은 테이블에 매핑된 엔터티 중 한 개만 업데이트하는 경우 부실 동시성 토큰 값을 방지하기 위해 이렇게 변경되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-299">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="059e1-300">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-300">**Mitigations**</span></span>

<span data-ttu-id="059e1-301">테이블을 공유하는 모든 엔터티는 동시성 토큰 열에 매핑되는 속성을 포함해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-301">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="059e1-302">해당 속성을 섀도 상태로 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-302">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="059e1-303">매핑되지 않은 형식에서 상속된 속성은 이제 모든 파생 형식에 대해 단일 열에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-303">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="059e1-304">추적 문제 #13998</span><span class="sxs-lookup"><span data-stu-id="059e1-304">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="059e1-305">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-305">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-306">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-306">**Old behavior**</span></span>

<span data-ttu-id="059e1-307">다음 모델을 살펴보세요.</span><span class="sxs-lookup"><span data-stu-id="059e1-307">Consider the following model:</span></span>
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

<span data-ttu-id="059e1-308">EF Core 3.0 전에는 `ShippingAddress` 속성이 기본적으로 `BulkOrder` 및 `Order`에 대한 별도의 열에 매핑되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-308">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="059e1-309">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-309">**New behavior**</span></span>

<span data-ttu-id="059e1-310">3\.0부터 EF Core는 `ShippingAddress`에 대한 열을 한 개만 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-310">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="059e1-311">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-311">**Why**</span></span>

<span data-ttu-id="059e1-312">이전 동작은 예상할 수 없었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-312">The old behavoir was unexpected.</span></span>

<span data-ttu-id="059e1-313">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-313">**Mitigations**</span></span>

<span data-ttu-id="059e1-314">이 속성을 여전히 파생 형식에 대한 별도의 열에 명시적으로 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-314">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="059e1-315">외래 키 속성 규칙이 더 이상 보안 주체 속성과 동일한 이름을 일치시키지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-315">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="059e1-316">추적 문제 #13274</span><span class="sxs-lookup"><span data-stu-id="059e1-316">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="059e1-317">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-317">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="059e1-318">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-318">**Old behavior**</span></span>

<span data-ttu-id="059e1-319">다음 모델을 살펴보세요.</span><span class="sxs-lookup"><span data-stu-id="059e1-319">Consider the following model:</span></span>
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
<span data-ttu-id="059e1-320">EF Core 3.0 이전에는 규칙에 따라 외래 키에 대해 `CustomerId` 속성이 사용되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-320">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="059e1-321">그러나 `Order`가 소유 형식인 경우 `CustomerId`도 기본 키가 되며 이는 일반적으로 예상되는 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-321">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="059e1-322">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-322">**New behavior**</span></span>

<span data-ttu-id="059e1-323">3\.0부터 EF Core는 보안 주체 속성과 동일한 이름을 가지는 경우 규칙에 따라 외래 키에 대한 속성을 사용하려고 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-323">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="059e1-324">보안 주체 속성 이름과 연결된 보안 주체 유형 이름 및 보안 주체 속성 이름 패턴과 연결된 탐색 이름은 여전히 일치합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-324">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="059e1-325">예:</span><span class="sxs-lookup"><span data-stu-id="059e1-325">For example:</span></span>

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

<span data-ttu-id="059e1-326">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-326">**Why**</span></span>

<span data-ttu-id="059e1-327">이 변경으로 인해 소유 형식에서 기본 키 속성을 잘못 정의하지 않게 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-327">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="059e1-328">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-328">**Mitigations**</span></span>

<span data-ttu-id="059e1-329">속성을 외래 키로 하고, 이에 따라서 기본 키의 일부가 되는 경우 명시적으로 이와 같이 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-329">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="059e1-330">데이터베이스 연결은 이제 TransactionScope가 완료되기 전에 더 이상 사용되지 않으면 닫힙니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-330">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="059e1-331">추적 문제 #14218</span><span class="sxs-lookup"><span data-stu-id="059e1-331">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="059e1-332">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-332">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-333">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-333">**Old behavior**</span></span>

<span data-ttu-id="059e1-334">EF Core 3.0 전에는 컨텍스트가 `TransactionScope` 내에서 연결을 여는 경우 현재 `TransactionScope`가 활성화되어 있는 동안 해당 연결이 열린 상태로 유지됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-334">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="059e1-335">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-335">**New behavior**</span></span>

<span data-ttu-id="059e1-336">3\.0부터 EF Core는 연결의 사용이 완료되면 해당 연결을 즉시 닫습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-336">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="059e1-337">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-337">**Why**</span></span>

<span data-ttu-id="059e1-338">같은 `TransactionScope`에 복수의 컨텍스트를 사용할 수 있도록 이렇게 변경했습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-338">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="059e1-339">새 동작은 EF6과도 일치합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-339">The new behavior also matches EF6.</span></span>

<span data-ttu-id="059e1-340">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-340">**Mitigations**</span></span>

<span data-ttu-id="059e1-341">연결이 열린 상태로 유지되어야 하는 경우 `OpenConnection()`을 명시적으로 호출하여 EF Core가 연결을 조기에 닫지 않도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-341">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="059e1-342">각 속성은 독립적인 메모리 내 정수 키 생성을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-342">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="059e1-343">추적 문제 #6872</span><span class="sxs-lookup"><span data-stu-id="059e1-343">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="059e1-344">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-344">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-345">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-345">**Old behavior**</span></span>

<span data-ttu-id="059e1-346">EF Core 3.0 이전에는 모든 메모리 내 정수 키 속성에 대해 하나의 공유 값 생성기가 사용되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-346">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="059e1-347">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-347">**New behavior**</span></span>

<span data-ttu-id="059e1-348">EF Core 3.0부터 메모리 내 데이터베이스를 사용하는 경우 각 정수 키 속성은 자체 값 생성기를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-348">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="059e1-349">또한 데이터베이스가 삭제되면 모든 테이블에 대해 키 생성이 재설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-349">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="059e1-350">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-350">**Why**</span></span>

<span data-ttu-id="059e1-351">이 변경으로 인해 메모리 내 키 생성을 실제 데이터베이스 키 생성에 보다 가깝게 정렬하고 메모리 내 데이터베이스를 사용할 때 테스트를 서로 격리하는 기능이 향상되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-351">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="059e1-352">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-352">**Mitigations**</span></span>

<span data-ttu-id="059e1-353">이로 인해 특정 메모리 내 키 값을 사용하는 애플리케이션이 중단될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-353">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="059e1-354">대신 특정 키 값을 사용하지 않거나 새 동작과 일치하도록 업데이트하지 않도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-354">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="059e1-355">지원 필드는 기본적으로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-355">Backing fields are used by default</span></span>

[<span data-ttu-id="059e1-356">추적 문제 #12430</span><span class="sxs-lookup"><span data-stu-id="059e1-356">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="059e1-357">이 변경 내용은 EF Core 3.0 미리 보기 2에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-357">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="059e1-358">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-358">**Old behavior**</span></span>

<span data-ttu-id="059e1-359">3\.0 이전에는 속성에 대한 지원 필드가 알려져 있더라도 EF Core는 기본적으로 속성 getter 및 setter 메서드를 사용하여 속성 값을 읽고 작성했습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-359">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="059e1-360">이에 대한 예외는 쿼리 실행으로 알려진 경우 지원 필드가 직접 설정되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-360">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="059e1-361">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-361">**New behavior**</span></span>

<span data-ttu-id="059e1-362">EF Core 3.0부터 속성 지원 필드가 알려진 경우 EF Core는 항상 지원 필드를 사용하여 해당 속성을 읽고 씁니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-362">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="059e1-363">이로 인해 애플리케이션이 getter 또는 setter 메서드로 코딩된 추가 동작을 사용하는 경우 애플리케이션이 중단될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-363">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="059e1-364">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-364">**Why**</span></span>

<span data-ttu-id="059e1-365">이 변경으로 인해 엔터티가 포함된 데이터베이스 작업을 수행할 때 EF Core가 기본적으로 비즈니스 논리를 잘못 트리거하는 것을 방지합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-365">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="059e1-366">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-366">**Mitigations**</span></span>

<span data-ttu-id="059e1-367">`ModelBuilder`에서 속성 액세스 모드의 구성을 통해 3.0 이전 버전의 동작을 복원할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-367">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="059e1-368">예:</span><span class="sxs-lookup"><span data-stu-id="059e1-368">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="059e1-369">호환이 가능한 여러 지원 필드가 발견되면 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-369">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="059e1-370">추적 문제 #12523</span><span class="sxs-lookup"><span data-stu-id="059e1-370">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="059e1-371">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-371">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-372">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-372">**Old behavior**</span></span>

<span data-ttu-id="059e1-373">EF Core 3.0 이전에는 여러 필드가 속성의 지원 필드를 찾기 위한 규칙과 일치하면 우선순위에 따라 하나의 필드가 선택되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-373">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="059e1-374">이로 인해 모호한 경우 잘못된 필드가 사용될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-374">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="059e1-375">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-375">**New behavior**</span></span>

<span data-ttu-id="059e1-376">EF Core 3.0부터 여러 필드가 동일한 속성과 일치하면 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-376">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="059e1-377">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-377">**Why**</span></span>

<span data-ttu-id="059e1-378">이 변경으로 인해 하나의 필드만 정확할 때 다른 필드보다 한 필드만 자동으로 사용하는 것을 방지했습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-378">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="059e1-379">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-379">**Mitigations**</span></span>

<span data-ttu-id="059e1-380">모호한 지원 필드가 있는 속성은 사용할 필드가 명시적으로 지정되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-380">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="059e1-381">예를 들어 흐름 API 사용:</span><span class="sxs-lookup"><span data-stu-id="059e1-381">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="059e1-382">필드 전용 속성 이름은 필드 이름과 일치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-382">Field-only property names should match the field name</span></span>

<span data-ttu-id="059e1-383">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-383">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-384">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-384">**Old behavior**</span></span>

<span data-ttu-id="059e1-385">EF Core 3.0 이전에는 문자열 값으로 속성을 지정할 수 있었고 CLR 형식에서 해당 이름을 가진 속성을 찾을 수 없는 경우, EF Core는 변환 규칙을 사용하여 필드에 일치시키려고 했습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-385">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
```C#
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```
```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="059e1-386">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-386">**New behavior**</span></span>

<span data-ttu-id="059e1-387">EF Core 3.0부터 필드 전용 속성은 필드 이름과 정확히 일치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-387">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="059e1-388">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-388">**Why**</span></span>

<span data-ttu-id="059e1-389">이 변경은 유사한 이름의 두 속성에 대해 동일한 필드를 사용하지 않도록 하기 위해 이루어졌으며, 필드 전용 속성에 대한 일치 규칙을 CLR 속성에 매핑된 속성과 동일하게 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-389">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="059e1-390">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-390">**Mitigations**</span></span>

<span data-ttu-id="059e1-391">필드 전용 속성의 이름은 매핑되는 필드와 동일한 이름을 지정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-391">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="059e1-392">이후 EF Core 3.0 미리 보기에서는 속성 이름과 다른 필드 이름을 명시적으로 다시 사용하도록 설정할 계획입니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-392">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="059e1-393">AddDbContext/AddDbContextPool이 더 이상 AddLogging 및 AddMemoryCache를 호출하지 않음</span><span class="sxs-lookup"><span data-stu-id="059e1-393">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="059e1-394">추적 문제 #14756</span><span class="sxs-lookup"><span data-stu-id="059e1-394">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="059e1-395">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-395">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-396">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-396">**Old behavior**</span></span>

<span data-ttu-id="059e1-397">EF Core 3.0 이전에는 `AddDbContext` 또는 `AddDbContextPool`을 호출하면 [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) 및 [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)에 대한 호출을 통해 D.I를 사용하여 로깅 및 메모리 캐시 서비스도 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-397">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="059e1-398">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-398">**New behavior**</span></span>

<span data-ttu-id="059e1-399">EF Core 3.0부터 `AddDbContext` 및 `AddDbContextPool`은 DI(종속성 주입)를 사용하여 이러한 서비스를 더 이상 등록하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-399">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="059e1-400">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-400">**Why**</span></span>

<span data-ttu-id="059e1-401">EF Core 3.0에서는 이러한 서비스가 애플리케이션의 DI 컨테이너에 있을 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-401">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="059e1-402">그러나 `ILoggerFactory`가 애플리케이션의 DI 컨테이너에 등록된 경우 EF Core에서 여전히 DI를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-402">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="059e1-403">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-403">**Mitigations**</span></span>

<span data-ttu-id="059e1-404">애플리케이션에 이러한 서비스가 필요한 경우 [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) 또는 [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)를 사용하여 DI 컨테이너에 명시적으로 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-404">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="059e1-405">DbContext.Entry는 이제 로컬 DetectChanges를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-405">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="059e1-406">추적 문제 #13552</span><span class="sxs-lookup"><span data-stu-id="059e1-406">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="059e1-407">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-407">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="059e1-408">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-408">**Old behavior**</span></span>

<span data-ttu-id="059e1-409">EF Core 3.0 이전에는 `DbContext.Entry`를 호출하면 추적된 모든 엔터티에 대해 변경 내용이 검색되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-409">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="059e1-410">이로 인해 `EntityEntry`에 노출된 상태가 최신 상태로 유지되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-410">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="059e1-411">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-411">**New behavior**</span></span>

<span data-ttu-id="059e1-412">EF Core 3.0부터 `DbContext.Entry` 호출은 지정된 엔터티와 이와 관련된 추적된 모든 주체 엔터티의 변경 내용만 검색하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-412">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="059e1-413">즉, 이 메서드를 호출하여 다른 곳의 변경 내용을 검색하지 못할 수 있으므로 애플리케이션 상태에 영향을 줄 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-413">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="059e1-414">`ChangeTracker.AutoDetectChangesEnabled`를 `false`로 설정하면 이 로컬 변경 검색도 비활성화됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-414">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="059e1-415">변경 검색(예: `ChangeTracker.Entries` 및 `SaveChanges`)을 유발하는 다른 메서드는 추적된 모든 엔터티의 전체 `DetectChanges`를 유발합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-415">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="059e1-416">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-416">**Why**</span></span>

<span data-ttu-id="059e1-417">이 변경으로 인해 `context.Entry`를 사용하는 기본 성능이 향상되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-417">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="059e1-418">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-418">**Mitigations**</span></span>

<span data-ttu-id="059e1-419">`Entry`를 호출하기 전에 명시적으로 `ChgangeTracker.DetectChanges()`를 호출하여 3.0 이전 버전의 동작을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-419">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="059e1-420">문자열 및 바이트 배열 키는 기본적으로 클라이언트에서 생성되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-420">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="059e1-421">추적 문제 #14617</span><span class="sxs-lookup"><span data-stu-id="059e1-421">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="059e1-422">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-422">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-423">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-423">**Old behavior**</span></span>

<span data-ttu-id="059e1-424">EF Core 3.0 이전에는 null이 아닌 값을 명시적으로 설정하지 않고도 `string` 및 `byte[]` 키 속성을 사용할 수 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-424">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="059e1-425">이 경우 키 값은 클라이언트에서 GUID로 생성되고 `byte[]`의 바이트로 직렬화됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-425">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="059e1-426">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-426">**New behavior**</span></span>

<span data-ttu-id="059e1-427">EF Core 3.0부터 키 값이 설정되지 않았음을 나타내는 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-427">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="059e1-428">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-428">**Why**</span></span>

<span data-ttu-id="059e1-429">클라이언트에서 생성한 `string`/`byte[]` 값은 일반적으로 유용하지 않고 기본 동작으로 인해 생성된 키 값을 일반적인 방식으로 추론하기가 어려웠기 때문에 이 변경이 이루어졌습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-429">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="059e1-430">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-430">**Mitigations**</span></span>

<span data-ttu-id="059e1-431">다른 null이 아닌 값이 설정되지 않은 경우 키 속성이 생성된 값을 사용해야 함을 명시적으로 지정하면 3.0 이전 버전 동작을 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-431">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="059e1-432">예를 들어 흐름 API가 있는 경우:</span><span class="sxs-lookup"><span data-stu-id="059e1-432">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="059e1-433">데이터 주석이 있는 경우:</span><span class="sxs-lookup"><span data-stu-id="059e1-433">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="059e1-434">ILoggerFactory는 이제 범위가 지정된 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-434">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="059e1-435">추적 문제 #14698</span><span class="sxs-lookup"><span data-stu-id="059e1-435">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="059e1-436">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-436">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="059e1-437">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-437">**Old behavior**</span></span>

<span data-ttu-id="059e1-438">EF Core 3.0 이전에는 `ILoggerFactory`가 싱글톤 서비스로 등록되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-438">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="059e1-439">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-439">**New behavior**</span></span>

<span data-ttu-id="059e1-440">EF Core 3.0부터 이제 `ILoggerFactory`는 범위가 지정된 대로 등록됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-440">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="059e1-441">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-441">**Why**</span></span>

<span data-ttu-id="059e1-442">이 변경으로 인해 `DbContext` 인스턴스와 로거를 연결하여 다른 기능을 활성화하고 내부 서비스 공급자의 급증과 같은 병리적인 동작의 일부 사례가 제거됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-442">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="059e1-443">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-443">**Mitigations**</span></span>

<span data-ttu-id="059e1-444">이 변경 내용은 EF Core 내부 서비스 공급자에 사용자 지정 서비스를 등록하고 사용하지 않는 한 애플리케이션 코드에 영향을 미치지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-444">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="059e1-445">이는 일반적이지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-445">This isn't common.</span></span>
<span data-ttu-id="059e1-446">이러한 경우 대부분의 작업은 여전히 작동하지만 `ILoggerFactory`에 종속된 싱글톤 서비스는 `ILoggerFactory`를 얻기 위해 다른 방식으로 변경해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-446">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="059e1-447">이와 같은 상황에 처한 경우 [EF Core GitHub 문제 추적기](https://github.com/aspnet/EntityFrameworkCore/issues)에 문제를 제출하여 향후 이 문제를 해결할 수 있는 방법을 더 잘 이해할 수 있도록 `ILoggerFactory`를 사용하는 방법을 알려주세요.</span><span class="sxs-lookup"><span data-stu-id="059e1-447">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="059e1-448">지연 로드 프록시는 더 이상 탐색 속성이 완전히 로드되었다고 가정하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-448">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="059e1-449">추적 문제 #12780</span><span class="sxs-lookup"><span data-stu-id="059e1-449">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="059e1-450">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-450">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-451">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-451">**Old behavior**</span></span>

<span data-ttu-id="059e1-452">EF Core 3.0 이전에는 `DbContext`가 삭제되면 해당 컨텍스트에서 가져온 엔터티의 지정된 탐색 속성이 완전히 로드되었는지 여부를 알 수 없었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-452">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="059e1-453">대신 프록시는 참조 탐색에 null이 아닌 값이 있으면 로드되고 컬렉션 탐색이 비어 있지 않으면 로드된다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-453">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="059e1-454">이러한 경우 지연 로드를 시도하면 아무런 작업도 수행되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-454">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="059e1-455">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-455">**New behavior**</span></span>

<span data-ttu-id="059e1-456">EF Core 3.0부터 프록시는 탐색 속성이 로드되었는지 여부를 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-456">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="059e1-457">즉, 컨텍스트가 삭제된 후에 로드되는 탐색 속성에 액세스하려고 하면 로드된 탐색이 비어 있거나 null인 경우에도 항상 아무런 작업도 수행되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-457">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="059e1-458">반대로, 로드되지 않은 탐색 속성에 액세스하려고 하면 탐색 속성이 비어 있지 않은 컬렉션이라도 컨텍스트가 삭제되면 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-458">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="059e1-459">이 상황이 발생하는 경우 애플리케이션 코드가 잘못된 시간에 지연 로드를 사용하려고 시도하고 있음을 의미하며, 애플리케이션이 이를 수행하지 않도록 변경해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-459">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="059e1-460">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-460">**Why**</span></span>

<span data-ttu-id="059e1-461">이 변경으로 인해 삭제된 `DbContext` 인스턴스에 지연 로드를 시도할 때 동작을 일관되고 정확하게 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-461">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="059e1-462">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-462">**Mitigations**</span></span>

<span data-ttu-id="059e1-463">애플리케이션을 업데이트하여 삭제된 컨텍스트로 지연 로드를 시도하지 않도록 하거나 예외 메시지에 설명된 대로 작업을 수행하지 않도록 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-463">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="059e1-464">내부 서비스 공급자의 과도한 생성은 이제 기본적으로 오류입니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-464">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="059e1-465">추적 문제 #10236</span><span class="sxs-lookup"><span data-stu-id="059e1-465">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="059e1-466">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-466">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="059e1-467">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-467">**Old behavior**</span></span>

<span data-ttu-id="059e1-468">EF Core 3.0 이전에는 병리적인 수의 내부 서비스 공급자를 만드는 애플리케이션에 경고가 기록되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-468">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="059e1-469">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-469">**New behavior**</span></span>

<span data-ttu-id="059e1-470">EF Core 3.0부터 이제 이 경고가 고려되며 오류와 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-470">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="059e1-471">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-471">**Why**</span></span>

<span data-ttu-id="059e1-472">이 변경으로 인해 병리적인 사례를 더 명시적으로 노출하여 더 나은 애플리케이션 코드를 구동하게 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-472">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="059e1-473">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-473">**Mitigations**</span></span>

<span data-ttu-id="059e1-474">이 오류가 발생했을 때 가장 적절한 조치는 근본 원인을 이해하고 많은 내부 서비스 공급자를 만드는 것을 중단하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-474">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="059e1-475">그러나 오류는 `DbContextOptionsBuilder`의 구성을 통해 경고(또는 무시)로 다시 변환될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-475">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="059e1-476">예:</span><span class="sxs-lookup"><span data-stu-id="059e1-476">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="059e1-477">단일 문자열로 호출되는 HasOne/HasMany의 새 동작</span><span class="sxs-lookup"><span data-stu-id="059e1-477">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="059e1-478">추적 문제 #9171</span><span class="sxs-lookup"><span data-stu-id="059e1-478">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="059e1-479">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-479">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-480">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-480">**Old behavior**</span></span>

<span data-ttu-id="059e1-481">EF Core 3.0 이전에는 단일 문자열을 사용하여 `HasOne` 또는 `HasMany`를 호출하는 코드가 혼란스러운 방식으로 해석되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-481">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="059e1-482">예:</span><span class="sxs-lookup"><span data-stu-id="059e1-482">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="059e1-483">코드는 비공개일 수 있는 `Entrance` 탐색 속성을 사용하여 `Samurai`를 다른 엔터티 형식과 관련시키려는 것 같습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-483">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="059e1-484">실제로 이 코드는 탐색 속성을 사용하지 않고 `Entrance`라고 하는 엔터티 형식과 관계를 생성하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-484">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="059e1-485">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-485">**New behavior**</span></span>

<span data-ttu-id="059e1-486">EF Core 3.0부터 이제 위 코드는 이전에 수행했어야 하는 것처럼 보이는 방식으로 동작합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-486">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="059e1-487">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-487">**Why**</span></span>

<span data-ttu-id="059e1-488">이전 동작은 특히 구성 코드를 읽고 오류를 찾을 때 매우 혼란스럽습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-488">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="059e1-489">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-489">**Mitigations**</span></span>

<span data-ttu-id="059e1-490">새 동작은 형식 이름의 문자열을 사용하고 탐색 속성을 명시적으로 지정하지 않은 채 명시적으로 관계를 구성하는 애플리케이션만 중단됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-490">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="059e1-491">이는 일반적이지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-491">This is not common.</span></span>
<span data-ttu-id="059e1-492">이전 동작은 탐색 속성 이름에 대해 `null`을 전달하여 명시적으로 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-492">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="059e1-493">예:</span><span class="sxs-lookup"><span data-stu-id="059e1-493">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="059e1-494">여러 비동기 메서드의 반환 형식이 작업에서 ValueTask로 변경되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-494">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="059e1-495">추적 문제 #15184</span><span class="sxs-lookup"><span data-stu-id="059e1-495">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="059e1-496">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-496">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-497">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-497">**Old behavior**</span></span>

<span data-ttu-id="059e1-498">다음 비동기 메서드는 이전에 `Task<T>`를 반환했습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-498">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="059e1-499">`ValueGenerator.NextValueAsync()`(및 파생 클래스)</span><span class="sxs-lookup"><span data-stu-id="059e1-499">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="059e1-500">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-500">**New behavior**</span></span>

<span data-ttu-id="059e1-501">앞서 언급한 메서드는 이제 이전과 같은 `T`를 통해 `ValueTask<T>`를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-501">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="059e1-502">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-502">**Why**</span></span>

<span data-ttu-id="059e1-503">이 변경은 해당 메서드를 호출할 때 발생하는 힙 할당 수를 줄여서 일반적인 성능을 개선합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-503">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="059e1-504">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-504">**Mitigations**</span></span>

<span data-ttu-id="059e1-505">단순히 위의 API를 대기 중인 애플리케이션은 다시 컴파일하기만 하면 됩니다. 소스 변경은 필요 없습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-505">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="059e1-506">더 복잡한 사용(예: 반환된 `Task`를 `Task.WhenAny()`에 전달)의 경우 일반적으로 반환된 `ValueTask<T>`에 대해 `AsTask()`를 호출하여 `Task<T>`로 변환해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-506">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="059e1-507">이렇게 하면 이 변경으로 인한 할당 감소가 무효화됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-507">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="059e1-508">관계형:TypeMapping 주석은 이제 TypeMapping일 뿐입니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-508">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="059e1-509">추적 문제 #9913</span><span class="sxs-lookup"><span data-stu-id="059e1-509">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="059e1-510">이 변경 내용은 EF Core 3.0 미리 보기 2에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-510">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="059e1-511">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-511">**Old behavior**</span></span>

<span data-ttu-id="059e1-512">형식 매핑 주석에 대한 주석 이름은 "관계형:TypeMapping"이었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-512">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="059e1-513">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-513">**New behavior**</span></span>

<span data-ttu-id="059e1-514">형식 매핑 주석에 대한 주석 이름은 이제 "TypeMapping"입니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-514">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="059e1-515">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-515">**Why**</span></span>

<span data-ttu-id="059e1-516">이제 형식 매핑은 관계형 데이터베이스 공급자 이상의 용도로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-516">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="059e1-517">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-517">**Mitigations**</span></span>

<span data-ttu-id="059e1-518">이는 일반적이지 않은 주석으로 형식 매핑에 직접 액세스하는 애플리케이션만 중단합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-518">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="059e1-519">수정하기 위한 가장 적절한 작업은 직접 주석을 사용하는 대신 API 화면을 사용하여 형식 매핑에 액세스하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-519">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="059e1-520">파생된 형식의 ToTable에서 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-520">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="059e1-521">추적 문제 #11811</span><span class="sxs-lookup"><span data-stu-id="059e1-521">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="059e1-522">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-522">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="059e1-523">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-523">**Old behavior**</span></span>

<span data-ttu-id="059e1-524">EF Core 3.0 이전에는 유효하지 않은 경우 상속 매핑 전략만 TPH이므로 파생된 형식에 대해 호출된 `ToTable()`은 무시되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-524">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="059e1-525">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-525">**New behavior**</span></span>

<span data-ttu-id="059e1-526">EF Core 3.0부터 시작하여 이후 릴리스에서 TPT 및 TPC 지원을 추가하기 위해 파생 형식에서 호출된 `ToTable()`에서는 나중에 예기치 않은 매핑 변화를 방지하기 위해 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-526">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="059e1-527">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-527">**Why**</span></span>

<span data-ttu-id="059e1-528">현재 파생된 형식을 다른 테이블에 매핑하는 것은 올바르지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-528">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="059e1-529">이 변경으로 인해 할 일이 유효하게 될 때 나중에 중단되는 것을 방지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-529">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="059e1-530">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-530">**Mitigations**</span></span>

<span data-ttu-id="059e1-531">파생된 형식을 다른 테이블에 매핑하려는 시도를 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-531">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="059e1-532">ForSqlServerHasIndex가 HasIndex로 바뀝니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-532">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="059e1-533">추적 문제 #12366</span><span class="sxs-lookup"><span data-stu-id="059e1-533">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="059e1-534">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-534">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="059e1-535">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-535">**Old behavior**</span></span>

<span data-ttu-id="059e1-536">EF Core 3.0 이전에는 `ForSqlServerHasIndex().ForSqlServerInclude()`가 `INCLUDE`와 함께 사용되는 열을 구성하는 방법을 제공했습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-536">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="059e1-537">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-537">**New behavior**</span></span>

<span data-ttu-id="059e1-538">EF Core 3.0부터 인덱스에 `Include`를 사용하여 이제 관계형 수준에서 지원됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-538">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="059e1-539">`HasIndex().ForSqlServerInclude()`을 사용하십시오.</span><span class="sxs-lookup"><span data-stu-id="059e1-539">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="059e1-540">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-540">**Why**</span></span>

<span data-ttu-id="059e1-541">이 변경으로 인해 `Include`가 있는 인덱스의 API를 모든 데이터베이스 공급자에 대해 한 곳으로 통합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-541">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="059e1-542">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-542">**Mitigations**</span></span>

<span data-ttu-id="059e1-543">위에 표시된 것처럼 새 API를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-543">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="059e1-544">메타데이터 API 변경 내용</span><span class="sxs-lookup"><span data-stu-id="059e1-544">Metadata API changes</span></span>

[<span data-ttu-id="059e1-545">추적 문제 #214</span><span class="sxs-lookup"><span data-stu-id="059e1-545">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="059e1-546">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-546">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-547">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-547">**New behavior**</span></span>

<span data-ttu-id="059e1-548">다음 속성이 확장 메서드로 변환되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-548">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="059e1-549">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-549">**Why**</span></span>

<span data-ttu-id="059e1-550">이 변경은 앞서 언급한 인터페이스의 구현을 단순화합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-550">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="059e1-551">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-551">**Mitigations**</span></span>

<span data-ttu-id="059e1-552">새로운 확장 메서드를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-552">Use the new extension methods.</span></span>

## <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="059e1-553">공급자 고유의 메타데이터 API 변경 내용</span><span class="sxs-lookup"><span data-stu-id="059e1-553">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="059e1-554">추적 문제 #214</span><span class="sxs-lookup"><span data-stu-id="059e1-554">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="059e1-555">이 변경 내용은 EF Core 3.0 미리 보기 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-555">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="059e1-556">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-556">**New behavior**</span></span>

<span data-ttu-id="059e1-557">공급자 고유의 확장 메서드가 평면화됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-557">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="059e1-558">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-558">**Why**</span></span>

<span data-ttu-id="059e1-559">이 변경은 앞서 언급한 확장 메서드의 구현을 단순화합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-559">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="059e1-560">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-560">**Mitigations**</span></span>

<span data-ttu-id="059e1-561">새로운 확장 메서드를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-561">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="059e1-562">EF Core는 더 이상 SQLite FK 적용을 위한 pragma를 보내지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-562">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="059e1-563">추적 문제 #12151</span><span class="sxs-lookup"><span data-stu-id="059e1-563">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="059e1-564">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-564">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="059e1-565">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-565">**Old behavior**</span></span>

<span data-ttu-id="059e1-566">EF Core 3.0 이전에는 EF Core가 SQLite에 대한 연결이 열릴 때 `PRAGMA foreign_keys = 1`을 보냈습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-566">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="059e1-567">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-567">**New behavior**</span></span>

<span data-ttu-id="059e1-568">EF Core 3.0부터 EF Core는 더 이상 SQLite에 대한 연결이 열릴 때 `PRAGMA foreign_keys = 1`을 보내지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-568">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="059e1-569">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-569">**Why**</span></span>

<span data-ttu-id="059e1-570">이 변경은 EF Core가 기본적으로 `SQLitePCLRaw.bundle_e_sqlite3`을 사용하기 때문에 이루어졌으며, 이는 FK 적용이 기본적으로 켜져 있고 연결이 열릴 때마다 명시적으로 활성화할 필요가 없음을 의미합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-570">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="059e1-571">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-571">**Mitigations**</span></span>

<span data-ttu-id="059e1-572">외래 키는 기본적으로 EF Core에 사용되는 SQLitePCLRaw.bundle_e_sqlite3에서 기본적으로 활성화됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-572">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="059e1-573">다른 경우에는 연결 문자열에 `Foreign Keys=True`를 지정하여 외래 키를 활성화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-573">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="059e1-574">Microsoft.EntityFrameworkCore.Sqlite는 이제 SQLitePCLRaw.bundle_e_sqlite3에 종속됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-574">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="059e1-575">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-575">**Old behavior**</span></span>

<span data-ttu-id="059e1-576">EF Core 3.0 이전에는 EF Core가 `SQLitePCLRaw.bundle_green`을 사용했습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-576">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="059e1-577">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-577">**New behavior**</span></span>

<span data-ttu-id="059e1-578">EF Core 3.0부터 EF Core가 `SQLitePCLRaw.bundle_e_sqlite3`을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-578">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="059e1-579">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-579">**Why**</span></span>

<span data-ttu-id="059e1-580">이 변경으로 인해 iOS에서 사용되는 SQLite의 버전이 다른 플랫폼과 일치되도록 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-580">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="059e1-581">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-581">**Mitigations**</span></span>

<span data-ttu-id="059e1-582">iOS에서 네이티브 SQLite 버전을 사용하려면 다른 `SQLitePCLRaw` 번들을 사용하도록 `Microsoft.Data.Sqlite`를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-582">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="059e1-583">Guid 값은 이제 SQLite에 텍스트로 저장되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-583">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="059e1-584">추적 문제 #15078</span><span class="sxs-lookup"><span data-stu-id="059e1-584">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="059e1-585">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-585">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-586">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-586">**Old behavior**</span></span>

<span data-ttu-id="059e1-587">Guid 값은 이전에 SQLite에 BLOB 값으로 저장되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-587">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="059e1-588">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-588">**New behavior**</span></span>

<span data-ttu-id="059e1-589">Guid 값은 이제 텍스트로 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-589">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="059e1-590">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-590">**Why**</span></span>

<span data-ttu-id="059e1-591">Guid의 이진 형식은 표준화되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-591">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="059e1-592">값을 텍스트로 저장하면 데이터베이스가 다른 기술과 더 잘 호환됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-592">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="059e1-593">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-593">**Mitigations**</span></span>

<span data-ttu-id="059e1-594">다음과 같이 SQL을 실행하여 기존 데이터베이스를 새 형식으로 마이그레이션할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-594">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="059e1-595">EF Core에서는 이러한 속성에 값 변환기를 구성하여 이전 동작을 계속 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-595">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="059e1-596">Microsoft.Data.Sqlite는 BLOB 및 텍스트 열 모두에서 Guid 값을 읽을 수 있습니다. 하지만 매개 변수 및 상수에 대한 기본 형식이 변경되었으므로 Guid를 포함하는 대부분의 시나리오에서 조치를 취해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-596">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="059e1-597">Char 값은 이제 SQLite에 텍스트로 저장됨</span><span class="sxs-lookup"><span data-stu-id="059e1-597">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="059e1-598">추적 문제 #15020</span><span class="sxs-lookup"><span data-stu-id="059e1-598">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="059e1-599">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-599">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-600">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-600">**Old behavior**</span></span>

<span data-ttu-id="059e1-601">Char 값은 이전에 SQLite에 정수 값으로 저장되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-601">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="059e1-602">예를 들어, *A*의 char 값은 정수 값 65로 저장되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-602">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="059e1-603">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-603">**New behavior**</span></span>

<span data-ttu-id="059e1-604">Char 값은 이제 텍스트로 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-604">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="059e1-605">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-605">**Why**</span></span>

<span data-ttu-id="059e1-606">값을 텍스트로 저장하는 것이 더 자연스럽고 데이터베이스가 다른 기술과 더 잘 호환됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-606">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="059e1-607">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-607">**Mitigations**</span></span>

<span data-ttu-id="059e1-608">다음과 같이 SQL을 실행하여 기존 데이터베이스를 새 형식으로 마이그레이션할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-608">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="059e1-609">EF Core에서는 이러한 속성에 값 변환기를 구성하여 이전 동작을 계속 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-609">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="059e1-610">또한 Microsoft.Data.Sqlite는 정수 및 텍스트 열에서 문자 값을 읽을 수 있으므로 특정 시나리오에서는 아무런 조치도 필요하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-610">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="059e1-611">이제 마이그레이션 ID가 고정 문화권의 달력을 사용하여 생성됨</span><span class="sxs-lookup"><span data-stu-id="059e1-611">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="059e1-612">추적 문제 #12978</span><span class="sxs-lookup"><span data-stu-id="059e1-612">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="059e1-613">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-613">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-614">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-614">**Old behavior**</span></span>

<span data-ttu-id="059e1-615">마이그레이션 ID는 현재 문화권의 달력을 사용하여 의도치 않게 생성되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-615">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="059e1-616">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-616">**New behavior**</span></span>

<span data-ttu-id="059e1-617">이제 마이그레이션 ID는 항상 고정 문화권의 달력(그레고리력)을 사용하여 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-617">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="059e1-618">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-618">**Why**</span></span>

<span data-ttu-id="059e1-619">데이터베이스를 업데이트하거나 병합 충돌을 해결할 때 마이그레이션 순서가 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-619">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="059e1-620">고정 달력을 사용하면 팀 멤버가 서로 다른 시스템 캘린더로 인해 발생할 수 있는 주문 문제를 방지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-620">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="059e1-621">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-621">**Mitigations**</span></span>

<span data-ttu-id="059e1-622">이 변경은 그레고리력(예: 태국 불교식 달력)보다 큰 년도가 있는 그레고리력이 아닌 달력을 사용하는 사용자에게 영향을 줍니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-622">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="059e1-623">기존 마이그레이션 ID를 업데이트하여 기존 마이그레이션 후 새 마이그레이션 순서를 지정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-623">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="059e1-624">마이그레이션 ID는 마이그레이션 디자이너 파일의 마이그레이션 속성에서 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-624">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="059e1-625">마이그레이션 기록 테이블도 업데이트해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-625">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="059e1-626">UseRowNumberForPaging가 제거되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-626">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="059e1-627">추적 문제 #16400</span><span class="sxs-lookup"><span data-stu-id="059e1-627">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="059e1-628">이 변경 내용은 EF Core 3.0 미리 보기 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-628">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="059e1-629">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-629">**Old behavior**</span></span>

<span data-ttu-id="059e1-630">EF Core 3.0 이전에는 `UseRowNumberForPaging`으로 SQL Server 2008과 호환되는 페이징을 위한 SQL을 생성할 수 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-630">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="059e1-631">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-631">**New behavior**</span></span>

<span data-ttu-id="059e1-632">EF Core 3.0부터 EF는 SQL Server 2008 이후 버전하고만 호환되는 페이징을 위한 SQL만 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-632">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="059e1-633">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-633">**Why**</span></span>

<span data-ttu-id="059e1-634">[SQL Server 2008은 더 이상 지원되는 제품이 아니며](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/), EF Core 3.0에서 수행되는 쿼리 변경 내용에 맞게 이 기능을 업데이트하는 것이 중요하므로 이 변경 작업을 수행하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-634">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="059e1-635">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-635">**Mitigations**</span></span>

<span data-ttu-id="059e1-636">생성된 SQL이 지원을 받을 수 있도록 최신 버전의 SQL Server 또는 더 높은 호환성 수준을 사용하여 업데이트하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-636">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="059e1-637">따라서, 이 작업을 수행할 수 없는 경우 세부 정보를 사용하여 [추적 문제에 대해 설명](https://github.com/aspnet/EntityFrameworkCore/issues/16400)해주세요.</span><span class="sxs-lookup"><span data-stu-id="059e1-637">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="059e1-638">사용자 의견에 따라 이 결정을 다시 검토할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-638">We may revisit this decision based on feedback.</span></span>

## <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="059e1-639">확장 정보/메타데이터가 IDbContextOptionsExtension에서 제거되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-639">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="059e1-640">추적 이슈 #16119</span><span class="sxs-lookup"><span data-stu-id="059e1-640">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="059e1-641">이 변경 내용은 EF Core 3.0 미리 보기 7에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-641">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="059e1-642">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-642">**Old behavior**</span></span>

<span data-ttu-id="059e1-643">`IDbContextOptionsExtension`은 확장에 대한 메타데이터를 제공하기 위한 메서드를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-643">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="059e1-644">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-644">**New behavior**</span></span>

<span data-ttu-id="059e1-645">이러한 메서드가 새 `IDbContextOptionsExtension.Info` 속성에서 반환된 새 `DbContextOptionsExtensionInfo` 추상 기본 클래스로 옮겨졌습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-645">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="059e1-646">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-646">**Why**</span></span>

<span data-ttu-id="059e1-647">2\.0부터 3.0까지의 릴리스에서 이러한 메서드를 추가하거나 여러 번 변경해야 했습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-647">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="059e1-648">해당 메서드를 새 추상 기본 클래스로 세분화하면 기존 확장을 중단하지 않고도 이러한 종류의 변경을 더 쉽게 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-648">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="059e1-649">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-649">**Mitigations**</span></span>

<span data-ttu-id="059e1-650">새 패턴에 따라 확장을 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-650">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="059e1-651">EF Core 소스 코드에서 다양한 종류의 확장을 위해 `IDbContextOptionsExtension`을 여러 번 구현한 예가 확인되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-651">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="059e1-652">LogQueryPossibleExceptionWithAggregateOperator 이름이 바뀌었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-652">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="059e1-653">추적 문제 #10985</span><span class="sxs-lookup"><span data-stu-id="059e1-653">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="059e1-654">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-654">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-655">**변경 내용**</span><span class="sxs-lookup"><span data-stu-id="059e1-655">**Change**</span></span>

<span data-ttu-id="059e1-656">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` 이름이 `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`으로 바뀌었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-656">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="059e1-657">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-657">**Why**</span></span>

<span data-ttu-id="059e1-658">이 경고 이벤트의 이름 지정을 다른 모든 경고 이벤트에 맞춥니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-658">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="059e1-659">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-659">**Mitigations**</span></span>

<span data-ttu-id="059e1-660">새 이름을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-660">Use the new name.</span></span> <span data-ttu-id="059e1-661">(이벤트 ID 번호가 변경되지 않았습니다.)</span><span class="sxs-lookup"><span data-stu-id="059e1-661">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="059e1-662">외래 키 제약 조건 이름에 대한 API를 명확히 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-662">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="059e1-663">추적 문제 #10730</span><span class="sxs-lookup"><span data-stu-id="059e1-663">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="059e1-664">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-664">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-665">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-665">**Old behavior**</span></span>

<span data-ttu-id="059e1-666">EF Core 3.0 이전에 외래 키 제약 조건 이름은 "이름"과 같이 간단했습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-666">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="059e1-667">예:</span><span class="sxs-lookup"><span data-stu-id="059e1-667">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="059e1-668">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-668">**New behavior**</span></span>

<span data-ttu-id="059e1-669">이제 EF Core 3.0부터 외래 키 제약 조건 이름은 “제약 조건 이름”이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-669">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="059e1-670">예:</span><span class="sxs-lookup"><span data-stu-id="059e1-670">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="059e1-671">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-671">**Why**</span></span>

<span data-ttu-id="059e1-672">이러한 변경으로 인해 이 영역에서 이름을 일관되게 지정하고, 또한 일관되게 외래 키가 정의된 열 또는 속성 이름이 아닌 외래 키 제약 조건의 이름임을 명확히 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-672">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="059e1-673">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-673">**Mitigations**</span></span>

<span data-ttu-id="059e1-674">새 이름을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-674">Use the new name.</span></span>

## <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="059e1-675">IRelationalDatabaseCreator.HasTables/HasTablesAsync가 공개되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-675">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="059e1-676">추적 이슈 #15997</span><span class="sxs-lookup"><span data-stu-id="059e1-676">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="059e1-677">이 변경 내용은 EF Core 3.0 미리 보기 7에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-677">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="059e1-678">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-678">**Old behavior**</span></span>

<span data-ttu-id="059e1-679">EF Core 3.0 이전에는 이러한 메서드가 비공개였습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-679">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="059e1-680">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-680">**New behavior**</span></span>

<span data-ttu-id="059e1-681">EF Core 3.0부터 이러한 메서드는 공개입니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-681">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="059e1-682">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-682">**Why**</span></span>

<span data-ttu-id="059e1-683">이러한 메서드는 데이터베이스가 생성되었지만 비어 있는지 확인하기 위해 EF에서 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-683">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="059e1-684">이것은 EF 외부에서 마이그레이션을 적용할지 여부를 결정할 때도 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-684">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="059e1-685">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-685">**Mitigations**</span></span>

<span data-ttu-id="059e1-686">재정의의 접근성을 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-686">Change the accessibility of any overrides.</span></span>

## <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="059e1-687">이제 Microsoft.EntityFrameworkCore.Design은 DevelopmentDependency 패키지입니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-687">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="059e1-688">추적 이슈 #11506</span><span class="sxs-lookup"><span data-stu-id="059e1-688">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="059e1-689">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-689">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="059e1-690">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-690">**Old behavior**</span></span>

<span data-ttu-id="059e1-691">EF Core 3.0 이전의 Microsoft.EntityFrameworkCore.Design은 정규 NuGet 패키지였으며, 이 패키지에 종속된 프로젝트에서 해당 어셈블리를 참조할 수 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-691">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="059e1-692">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-692">**New behavior**</span></span>

<span data-ttu-id="059e1-693">EF Core 3.0부터는 DevelopmentDependency 패키지입니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-693">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="059e1-694">따라서 종속성이 다른 프로젝트로 전이되지 않으며 더 이상은 기본적으로 해당 어셈블리를 참조할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-694">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="059e1-695">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-695">**Why**</span></span>

<span data-ttu-id="059e1-696">이 패키지는 디자인 타임에만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-696">This package is only intended to be used at design time.</span></span> <span data-ttu-id="059e1-697">배포된 애플리케이션은 해당 패키지를 참조할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-697">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="059e1-698">패키지를 DevelopmentDependency로 만들면 이 권장 사항이 강화됩니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-698">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="059e1-699">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-699">**Mitigations**</span></span>

<span data-ttu-id="059e1-700">이 패키지를 참조하여 EF Core의 디자인 타임 동작을 재정의해야 하는 경우 프로젝트에서 PackageReference 항목 메타데이터를 업데이트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-700">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="059e1-701">패키지가 Microsoft.EntityFrameworkCore.Tools를 통해 전이적으로 참조되는 경우에는 명시적 PackageReference를 패키지에 추가하여 해당 메타데이터를 변경해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-701">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

## <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="059e1-702">SQLitePCL.raw가 버전 2.0.0으로 업데이트되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-702">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="059e1-703">추적 이슈 #14824</span><span class="sxs-lookup"><span data-stu-id="059e1-703">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="059e1-704">이 변경 내용은 EF Core 3.0 미리 보기 7에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-704">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="059e1-705">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-705">**Old behavior**</span></span>

<span data-ttu-id="059e1-706">이전에는 Microsoft.EntityFrameworkCore.Sqlite가 SQLitePCL.raw의 버전 1.1.12에 의존했습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-706">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="059e1-707">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-707">**New behavior**</span></span>

<span data-ttu-id="059e1-708">버전 2.0.0에 의존하도록 패키지를 업데이트했습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-708">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="059e1-709">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-709">**Why**</span></span>

<span data-ttu-id="059e1-710">SQLitePCL.raw의 버전 2.0.0은 .NET Standard 2.0을 대상으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-710">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="059e1-711">이전에는 .NET Standard 1.1을 대상으로 했기 때문에 전이적 패키지를 대부분 종료해야 작동이 가능했습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-711">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="059e1-712">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-712">**Mitigations**</span></span>

<span data-ttu-id="059e1-713">SQLitePCL.raw 버전 2.0.0에 중대한 변경이 포함되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-713">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="059e1-714">자세한 내용은 [릴리스 정보](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="059e1-714">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>


## <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="059e1-715">NetTopologySuite를 버전 2.0.0으로 업데이트</span><span class="sxs-lookup"><span data-stu-id="059e1-715">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="059e1-716">이슈 추적 #14825</span><span class="sxs-lookup"><span data-stu-id="059e1-716">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="059e1-717">이 변경 내용은 EF Core 3.0 미리 보기 7에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-717">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="059e1-718">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-718">**Old behavior**</span></span>

<span data-ttu-id="059e1-719">이전의 공간 패키지는 NetTopologySuite의 버전 1.15.1을 사용했습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-719">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="059e1-720">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="059e1-720">**New behavior**</span></span>

<span data-ttu-id="059e1-721">버전 2.0.0에 의존하도록 패키지를 업데이트했습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-721">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="059e1-722">**이유**</span><span class="sxs-lookup"><span data-stu-id="059e1-722">**Why**</span></span>

<span data-ttu-id="059e1-723">NetTopologySuite 버전 2.0.0은 EF Core 사용자에게 발생하는 여러 가지 유용성 문제를 해결하는 것이 목적입니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-723">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="059e1-724">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="059e1-724">**Mitigations**</span></span>

<span data-ttu-id="059e1-725">NetTopologySuite 버전 2.0.0에는 호환성이 손상되는 변경이 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="059e1-725">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="059e1-726">자세한 내용은 [릴리스 정보](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="059e1-726">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>
