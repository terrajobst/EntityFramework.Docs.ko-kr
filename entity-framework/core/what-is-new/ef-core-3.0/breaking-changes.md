---
title: EF Core 3.0의 호환성이 손상되는 변경 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: a7e1a03bf1131cd53123f5cc39b07bed94619b44
ms.sourcegitcommit: a013e243a14f384999ceccaf9c779b8c1ae3b936
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463384"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="ca36e-102">EF Core 3.0에 포함된 호환성이 손상되는 변경(현재 미리 보기 상태)</span><span class="sxs-lookup"><span data-stu-id="ca36e-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ca36e-103">이후 릴리스의 기능 집합 및 일정은 항상 변경될 수 있으며 이 페이지를 최신 상태로 유지하려고 노력함에도 불구하고 최신 계획을 항상 반영할 수 없을 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="ca36e-104">다음 API 및 동작 변경은 3.0.0으로 업그레이드할 때 EF Core 2.2.x용으로 개발된 애플리케이션을 중단시킬 가능성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="ca36e-105">데이터베이스 공급자에만 영향을 줄 것으로 예상되는 변경 내용은 [공급자 변경](../../providers/provider-log.md)에 설명되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="ca36e-106">3.0 미리 보기 간의 도입된 새로운 기능의 중단은 여기에 설명되어 있지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="ca36e-107">LINQ 쿼리는 더 이상 클라이언트에서 평가되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-107">LINQ queries are no longer evaluated on the client</span></span>

[<span data-ttu-id="ca36e-108">추적 문제 #12795</span><span class="sxs-lookup"><span data-stu-id="ca36e-108">Tracking Issue #12795</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

> [!IMPORTANT]
> <span data-ttu-id="ca36e-109">이 줄 바꿈으로 미리 알려드립니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-109">We are pre-announcing this break.</span></span>
<span data-ttu-id="ca36e-110">3.0 미리 보기에서는 아직 제공되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-110">It has not yet shipped in any 3.0 preview.</span></span>

<span data-ttu-id="ca36e-111">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-111">**Old behavior**</span></span>

<span data-ttu-id="ca36e-112">3.0 이전에는 EF Core가 쿼리의 일부인 식을 SQL 또는 매개 변수로 변환할 수 없었을 때 클라이언트에서 자동으로 식을 계산했습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-112">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="ca36e-113">기본적으로, 잠재적 비용이 많이 드는 식에 대한 클라이언트 평가는 경고만 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-113">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="ca36e-114">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-114">**New behavior**</span></span>

<span data-ttu-id="ca36e-115">3.0부터 EF Core는 최상위 수준 프로젝션(쿼리의 마지막 `Select()` 호출)의 식만 클라이언트에서 평가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-115">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="ca36e-116">쿼리의 다른 부분에서 식을 SQL 또는 매개 변수로 변환할 수 없을 때 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-116">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="ca36e-117">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-117">**Why**</span></span>

<span data-ttu-id="ca36e-118">쿼리의 자동 클라이언트 평가는 중요한 부분을 변환할 수 없어도 많은 쿼리를 실행할 수 있게 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-118">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="ca36e-119">이 동작으로 인해 프로덕션 환경에서 분명하게 나타날 수 있는 예기치 않은 잠재적 손상을 초래할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-119">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="ca36e-120">예를 들어 변환할 수 없는 `Where()` 호출의 조건으로 인해 테이블의 모든 행이 데이터베이스 서버에서 전송되고 필터가 클라이언트에 적용될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-120">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="ca36e-121">이 경우 테이블에 개발 중인 몇 개 행만 포함하지만 애플리케이션이 수백만 개의 행이 포함될 수 있는 프로덕션으로 이동할 때 쉽게 감지되지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-121">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="ca36e-122">클라이언트 평가 경고도 개발 중에 너무 쉽게 무시됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-122">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="ca36e-123">이 외에도 자동 클라이언트 평가는 특정 식에 대한 쿼리 변환 기능 개선으로 인해 릴리스 간의 의도하지 않은 호환성이 손상되는 변경이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-123">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="ca36e-124">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-124">**Mitigations**</span></span>

<span data-ttu-id="ca36e-125">쿼리를 완벽하게 변환할 수 없는 경우 변환할 수 있는 형식으로 쿼리를 다시 작성하거나 `AsEnumerable()`, `ToList()` 또는 유사한 형식을 사용하여 명시적으로 데이터를 클라이언트로 가져오면 LINQ to Objects를 사용하여 추가로 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-125">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="ca36e-126">Entity Framework Core는 더 이상 ASP.NET Core 공유 프레임워크에 포함되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-126">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="ca36e-127">추적 문제 공지 사항 #325</span><span class="sxs-lookup"><span data-stu-id="ca36e-127">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="ca36e-128">이 변경 내용은 ASP.NET Core 3.0 미리 보기 1에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-128">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="ca36e-129">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-129">**Old behavior**</span></span>

<span data-ttu-id="ca36e-130">ASP.NET Core 3.0 이전에는 `Microsoft.AspNetCore.App` 또는 `Microsoft.AspNetCore.All`에 대한 패키지 참조를 추가하면 EF Core 및 SQL Server 공급자와 같은 일부 EF Core 데이터 공급자가 포함되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-130">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="ca36e-131">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-131">**New behavior**</span></span>

<span data-ttu-id="ca36e-132">3.0부터 ASP.NET Core 공유 프레임워크에는 EF Core 또는 EF Core 데이터 공급자가 포함되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-132">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="ca36e-133">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-133">**Why**</span></span>

<span data-ttu-id="ca36e-134">이 변경 전에 EF Core를 가져오려면 애플리케이션이 ASP.NET Core 및 SQL Server를 대상으로 했는지 여부에 따라 다른 단계가 필요했습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-134">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="ca36e-135">또한 ASP.NET Core를 업그레이드 하면 EF Core 및 SQL Server 업그레이드가 강제되었는데, 이는 항상 바람직하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-135">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="ca36e-136">이 변경으로 인해 EF Core를 가져오는 환경은 모든 공급자, 지원되는 .NET 구현 및 애플리케이션 유형에서 동일합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-136">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="ca36e-137">또한 개발자는 이제 EF Core 및 EF Core 데이터 공급자의 업그레이드 시기를 정확히 제어할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-137">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="ca36e-138">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-138">**Mitigations**</span></span>

<span data-ttu-id="ca36e-139">ASP.NET Core 3.0 애플리케이션 또는 기타 지원되는 애플리케이션에서 EF Core를 사용하려면 애플리케이션에서 사용할 EF Core 데이터베이스 공급자에 대한 패키지 참조를 명시적으로 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-139">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="ca36e-140">쿼리 실행이 디버그 수준에서 로깅됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-140">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="ca36e-141">추적 문제 #14523</span><span class="sxs-lookup"><span data-stu-id="ca36e-141">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="ca36e-142">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-142">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ca36e-143">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-143">**Old behavior**</span></span>

<span data-ttu-id="ca36e-144">EF Core 3.0 이전에는 쿼리 및 기타 명령 실행이 `Info` 수준에서 로깅되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-144">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="ca36e-145">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-145">**New behavior**</span></span>

<span data-ttu-id="ca36e-146">EF Core 3.0 부터 명령/SQL 실행의 로깅은 `Debug` 수준입니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-146">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="ca36e-147">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-147">**Why**</span></span>

<span data-ttu-id="ca36e-148">이 변경은 `Info` 로그 수준에서 노이즈를 줄이기 위해 수행되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-148">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="ca36e-149">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-149">**Mitigations**</span></span>

<span data-ttu-id="ca36e-150">이 로깅 이벤트는 `RelationalEventId.CommandExecuting`에서 이벤트 ID 20100으로 정의됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-150">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="ca36e-151">`Info` 수준에서 SQL을 다시 로그인하려면 `Debug` 수준에서 로깅을 켜고 이 이벤트에 대해서만 필터링합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-151">To log SQL at the `Info` level again, switch on logging at the `Debug` level and filter to just this event.</span></span>


## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="ca36e-152">임시 키 값은 더 이상 엔터티 인스턴스에 설정되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-152">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="ca36e-153">추적 문제 #12378</span><span class="sxs-lookup"><span data-stu-id="ca36e-153">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="ca36e-154">이 변경 내용은 EF Core 3.0 미리 보기 2에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-154">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="ca36e-155">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-155">**Old behavior**</span></span>

<span data-ttu-id="ca36e-156">EF Core 3.0 이전에는 임시 값이 데이터베이스에서 생성된 실제 값을 나중에 가질 수 있는 모든 키 속성에 할당되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-156">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="ca36e-157">일반적으로 이러한 임시 값은 큰 음수입니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-157">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="ca36e-158">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-158">**New behavior**</span></span>

<span data-ttu-id="ca36e-159">3.0부터 EF Core는 엔터티의 추적 정보의 일부로 임시 키 값을 저장하고, 키 속성 자체는 변경하지 않고 그대로 둡니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-159">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="ca36e-160">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-160">**Why**</span></span>

<span data-ttu-id="ca36e-161">이 변경은 일부 `DbContext` 인스턴스의 의해 이전에 추적된 엔터티가 다른 `DbContext` 인스턴스로 이동할 때 임시 키 값이 틀리게 영구화되지 않도록 하기 위해 수행되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-161">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="ca36e-162">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-162">**Mitigations**</span></span>

<span data-ttu-id="ca36e-163">엔터티 간의 연결을 형성하기 위해 외래 키에 기본 키 값을 할당하는 애플리케이션은 기본 키가 저장 생성되고 `Added` 상태의 엔터티에 속하는 경우 이전 동작에 따라 달라질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-163">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="ca36e-164">이는 다음과 같은 방법으로 방지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-164">This can be avoided by:</span></span>
* <span data-ttu-id="ca36e-165">저장 생성 키를 사용하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-165">Not using store-generated keys.</span></span>
* <span data-ttu-id="ca36e-166">외래 키 값을 설정하는 대신 관계를 형성하도록 탐색 속성을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-166">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="ca36e-167">엔터티의 추적 정보에서 실제 임시 키 값을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-167">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="ca36e-168">예를 들어 `context.Entry(blog).Property(e => e.Id).CurrentValue`는 `blog.Id` 자체가 설정되지 않았더라도 임시 값을 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-168">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="ca36e-169">DetectChanges는 저장 생성 키 값을 준수합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-169">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="ca36e-170">추적 문제 #14616</span><span class="sxs-lookup"><span data-stu-id="ca36e-170">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="ca36e-171">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-171">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ca36e-172">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-172">**Old behavior**</span></span>

<span data-ttu-id="ca36e-173">EF Core 3.0 이전에는 `DetectChanges`에서 찾은 추적되는 않은 엔터티를 `Added` 상태에서 추적하여 `SaveChanges`가 호출될 때 새로운 행으로 삽입했습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-173">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="ca36e-174">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-174">**New behavior**</span></span>

<span data-ttu-id="ca36e-175">EF Core 3.0부터 생성된 키 값을 사용하고 일부 키 값을 설정한 경우 `Modified` 상태에서 엔터티를 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-175">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="ca36e-176">즉, 엔터티에 대한 행이 있는 것으로 가정하고 `SaveChanges`가 호출될 때 업데이트됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-176">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="ca36e-177">키 값이 설정되지 않았거나 엔터티 형식이 생성된 키를 사용하지 않는 경우, 새 엔터티는 이전 버전과 마찬가지로 `Added`로 추적됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-177">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="ca36e-178">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-178">**Why**</span></span>

<span data-ttu-id="ca36e-179">이 변경으로 인해 저장 생성 키를 사용하는 동안 연결이 분리된 엔터티 그래프를 사용하여 작업을 보다 쉽고 일관되게 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-179">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="ca36e-180">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-180">**Mitigations**</span></span>

<span data-ttu-id="ca36e-181">엔터티 형식이 생성된 키를 사용하도록 구성되었지만 키 값이 새 인스턴스에 명시적으로 설정된 경우, 이 변경으로 인해 애플리케이션이 중단될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-181">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="ca36e-182">해결 방법은 생성된 값을 사용하지 않도록 키 속성을 명시적으로 구성하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-182">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="ca36e-183">예를 들어 흐름 API가 있는 경우:</span><span class="sxs-lookup"><span data-stu-id="ca36e-183">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="ca36e-184">데이터 주석이 있는 경우:</span><span class="sxs-lookup"><span data-stu-id="ca36e-184">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="ca36e-185">계단식 삭제는 기본적으로 즉시 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-185">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="ca36e-186">추적 문제 #10114</span><span class="sxs-lookup"><span data-stu-id="ca36e-186">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="ca36e-187">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ca36e-188">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-188">**Old behavior**</span></span>

<span data-ttu-id="ca36e-189">3.0 이전에는 SaveChanges가 호출될 때까지 EF Core에는 연계 작업(필요한 보안 주체가 삭제되거나 필요한 보안 주체에 대한 관계가 끊어질 때 종속 엔터티 삭제)이 적용되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-189">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="ca36e-190">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-190">**New behavior**</span></span>

<span data-ttu-id="ca36e-191">3.0부터 EF Core는 트리거 조건이 검색되는 즉시 연계 동작을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-191">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="ca36e-192">예를 들어, 보안 주체 엔터티를 삭제하기 위해 `context.Remove()`를 호출하면 추적된 모든 관련 필수 종속 항목도 즉시 `Deleted`로 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-192">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="ca36e-193">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-193">**Why**</span></span>

<span data-ttu-id="ca36e-194">이 변경으로 인해 `SaveChanges`가 호출되기 _전에_ 삭제될 엔터티를 이해하는 것이 중요한 데이터 바인딩 및 감사 시나리오 환경이 개선되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-194">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="ca36e-195">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-195">**Mitigations**</span></span>

<span data-ttu-id="ca36e-196">이전 동작은 `context.ChangedTracker`의 설정을 통해 복원할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-196">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="ca36e-197">예:</span><span class="sxs-lookup"><span data-stu-id="ca36e-197">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="ca36e-198">쿼리 형식은 엔터티 형식과 통합됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-198">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="ca36e-199">추적 문제 #14194</span><span class="sxs-lookup"><span data-stu-id="ca36e-199">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="ca36e-200">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-200">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ca36e-201">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-201">**Old behavior**</span></span>

<span data-ttu-id="ca36e-202">EF Core 3.0 이전에는 [쿼리 형식](xref:core/modeling/query-types)이 기본 키를 구조화된 방법으로 정의하지 않는 데이터를 쿼리하는 수단이었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-202">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="ca36e-203">즉, 쿼리 형식은 키 없이 엔터티 형식을 매핑하는데 사용(보기에서 가능할 수도 있지만 테이블에서도 가능한 경우)되는 반면, 일반 엔터티 형식은 키가 사용 가능할 때(테이블에서 가능하지만 보기에서도 가능한 경우) 사용되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-203">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="ca36e-204">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-204">**New behavior**</span></span>

<span data-ttu-id="ca36e-205">이제 쿼리 형식은 기본 키가 없는 엔터티 형식이 될 뿐입니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-205">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="ca36e-206">키 없는 엔터티 형식은 이전 버전의 쿼리 형식과 동일한 기능이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-206">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="ca36e-207">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-207">**Why**</span></span>

<span data-ttu-id="ca36e-208">이 변경으로 인해 쿼리 형식의 목적에 대한 혼동을 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-208">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="ca36e-209">특히 키 없는 엔터티 형식이며 이로 인해 본질적으로 읽기 전용이지만 엔터티 형식이 읽기 전용으로 할 필요가 있다고 해서 사용해서는 안됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-209">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="ca36e-210">마찬가지로 보기에 매핑되는 경우가 있지만 이는 보기가 키를 정의하지 않는 경우가 있기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-210">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="ca36e-211">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-211">**Mitigations**</span></span>

<span data-ttu-id="ca36e-212">API의 다음 부분은 이제 사용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-212">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="ca36e-213">**`ModelBuilder.Query<>()`** - 대신 엔터티 형식을 키가 없는 것으로 표시하기 위해 `ModelBuilder.Entity<>().HasNoKey()`를 호출해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-213">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="ca36e-214">기본 키가 필요하지만 규칙과 일치하지 않는 경우 잘못된 구성을 피하기 위해 규칙에 따라 구성되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-214">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="ca36e-215">**`DbQuery<>`** - 대신 `DbSet<>`를 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-215">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="ca36e-216">**`DbContext.Query<>()`** - 대신 `DbContext.Set<>()`를 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-216">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="ca36e-217">소유 형식 관계에 대한 구성 API가 변경됨</span><span class="sxs-lookup"><span data-stu-id="ca36e-217">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="ca36e-218">[추적 문제 #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[추적 문제 #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[추적 문제 #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="ca36e-218">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="ca36e-219">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-219">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ca36e-220">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-220">**Old behavior**</span></span>

<span data-ttu-id="ca36e-221">EF Core 3.0 이전에는 소유 관계의 구성은 `OwnsOne` 또는 `OwnsMany` 호출 직후에 수행되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-221">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="ca36e-222">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-222">**New behavior**</span></span>

<span data-ttu-id="ca36e-223">EF Core 3.0부터 이제 `WithOwner()`를 사용하여 소유자에게 탐색 속성을 구성하는 흐름 API가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-223">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="ca36e-224">예:</span><span class="sxs-lookup"><span data-stu-id="ca36e-224">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="ca36e-225">이제 소유자와 소유 간의 관계와 관련된 구성이 다른 관계가 구성되는 것과 유사하게 `WithOwner()` 후에 연결되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-225">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="ca36e-226">소유 형식 자체에 대한 구성은 `OwnsOne()/OwnsMany()` 이후에도 여전히 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-226">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="ca36e-227">예:</span><span class="sxs-lookup"><span data-stu-id="ca36e-227">For example:</span></span>

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

<span data-ttu-id="ca36e-228">또한 소유 형식 대상으로 `Entity()`, `HasOne()` 또는 `Set()`를 호출하면 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-228">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="ca36e-229">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-229">**Why**</span></span>

<span data-ttu-id="ca36e-230">이 변경으로 인해 소유 형식 자체의 구성과 소유 형식의 _관계 대상_ 사이를 명확하게 분리하게 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-230">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="ca36e-231">이는 `HasForeignKey`와 같은 메서드에 대한 모호성과 혼란을 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-231">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="ca36e-232">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-232">**Mitigations**</span></span>

<span data-ttu-id="ca36e-233">위의 예와 같이 소유 형식 관계의 구성을 변경하여 새 API 화면을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-233">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="ca36e-234">외래 키 속성 규칙이 더 이상 보안 주체 속성과 동일한 이름을 일치시키지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-234">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="ca36e-235">추적 문제 #13274</span><span class="sxs-lookup"><span data-stu-id="ca36e-235">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="ca36e-236">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-236">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ca36e-237">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-237">**Old behavior**</span></span>

<span data-ttu-id="ca36e-238">다음 모델을 살펴보세요.</span><span class="sxs-lookup"><span data-stu-id="ca36e-238">Consider the following model:</span></span>
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
<span data-ttu-id="ca36e-239">EF Core 3.0 이전에는 규칙에 따라 외래 키에 대해 `CustomerId` 속성이 사용되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-239">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="ca36e-240">그러나 `Order`가 소유 형식인 경우 `CustomerId`도 기본 키가 되며 이는 일반적으로 예상되는 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-240">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="ca36e-241">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-241">**New behavior**</span></span>

<span data-ttu-id="ca36e-242">3.0부터 EF Core는 보안 주체 속성과 동일한 이름을 가지는 경우 규칙에 따라 외래 키에 대한 속성을 사용하려고 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-242">Starting with 3.0, EF Core won't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="ca36e-243">보안 주체 속성 이름과 연결된 보안 주체 유형 이름 및 보안 주체 속성 이름 패턴과 연결된 탐색 이름은 여전히 일치합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-243">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="ca36e-244">예:</span><span class="sxs-lookup"><span data-stu-id="ca36e-244">For example:</span></span>

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

<span data-ttu-id="ca36e-245">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-245">**Why**</span></span>

<span data-ttu-id="ca36e-246">이 변경으로 인해 소유 형식에서 기본 키 속성을 잘못 정의하지 않게 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-246">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="ca36e-247">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-247">**Mitigations**</span></span>

<span data-ttu-id="ca36e-248">속성을 외래 키로 하고, 이에 따라서 기본 키의 일부가 되는 경우 명시적으로 이와 같이 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-248">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="ca36e-249">각 속성은 독립적인 메모리 내 정수 키 생성을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-249">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="ca36e-250">추적 문제 #6872</span><span class="sxs-lookup"><span data-stu-id="ca36e-250">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="ca36e-251">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-251">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ca36e-252">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-252">**Old behavior**</span></span>

<span data-ttu-id="ca36e-253">EF Core 3.0 이전에는 모든 메모리 내 정수 키 속성에 대해 하나의 공유 값 생성기가 사용되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-253">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="ca36e-254">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-254">**New behavior**</span></span>

<span data-ttu-id="ca36e-255">EF Core 3.0부터 메모리 내 데이터베이스를 사용하는 경우 각 정수 키 속성은 자체 값 생성기를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-255">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="ca36e-256">또한 데이터베이스가 삭제되면 모든 테이블에 대해 키 생성이 재설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-256">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="ca36e-257">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-257">**Why**</span></span>

<span data-ttu-id="ca36e-258">이 변경으로 인해 메모리 내 키 생성을 실제 데이터베이스 키 생성에 보다 가깝게 정렬하고 메모리 내 데이터베이스를 사용할 때 테스트를 서로 격리하는 기능이 향상되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-258">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="ca36e-259">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-259">**Mitigations**</span></span>

<span data-ttu-id="ca36e-260">이로 인해 특정 메모리 내 키 값을 사용하는 애플리케이션이 중단될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-260">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="ca36e-261">대신 특정 키 값을 사용하지 않거나 새 동작과 일치하도록 업데이트하지 않도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-261">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="ca36e-262">지원 필드는 기본적으로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-262">Backing fields are used by default</span></span>

[<span data-ttu-id="ca36e-263">추적 문제 #12430</span><span class="sxs-lookup"><span data-stu-id="ca36e-263">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="ca36e-264">이 변경 내용은 EF Core 3.0 미리 보기 2에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-264">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="ca36e-265">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-265">**Old behavior**</span></span>

<span data-ttu-id="ca36e-266">3.0 이전에는 속성에 대한 지원 필드가 알려져 있더라도 EF Core는 기본적으로 속성 getter 및 setter 메서드를 사용하여 속성 값을 읽고 작성했습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-266">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="ca36e-267">이에 대한 예외는 쿼리 실행으로 알려진 경우 지원 필드가 직접 설정되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-267">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="ca36e-268">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-268">**New behavior**</span></span>

<span data-ttu-id="ca36e-269">EF Core 3.0부터 속성 지원 필드가 알려진 경우 항상 지원 필드를 사용하여 해당 속성을 읽고 씁니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-269">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="ca36e-270">이로 인해 애플리케이션이 getter 또는 setter 메서드로 코딩된 추가 동작을 사용하는 경우 애플리케이션이 중단될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-270">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="ca36e-271">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-271">**Why**</span></span>

<span data-ttu-id="ca36e-272">이 변경으로 인해 엔터티가 포함된 데이터베이스 작업을 수행할 때 EF Core가 기본적으로 비즈니스 논리를 잘못 트리거하는 것을 방지합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-272">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="ca36e-273">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-273">**Mitigations**</span></span>

<span data-ttu-id="ca36e-274">modelBuilder 흐름 API에서 속성 액세스 모드의 구성을 통해 3.0 이전 버전의 동작을 복원할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-274">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="ca36e-275">예:</span><span class="sxs-lookup"><span data-stu-id="ca36e-275">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="ca36e-276">호환이 가능한 여러 지원 필드가 발견되면 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-276">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="ca36e-277">추적 문제 #12523</span><span class="sxs-lookup"><span data-stu-id="ca36e-277">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="ca36e-278">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ca36e-279">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-279">**Old behavior**</span></span>

<span data-ttu-id="ca36e-280">EF Core 3.0 이전에는 여러 필드가 속성의 지원 필드를 찾기 위한 규칙과 일치하면 우선순위에 따라 하나의 필드가 선택되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-280">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="ca36e-281">이로 인해 모호한 경우 잘못된 필드가 사용될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-281">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="ca36e-282">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-282">**New behavior**</span></span>

<span data-ttu-id="ca36e-283">EF Core 3.0부터 여러 필드가 동일한 속성과 일치하면 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-283">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="ca36e-284">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-284">**Why**</span></span>

<span data-ttu-id="ca36e-285">이 변경으로 인해 하나의 필드만 정확할 때 다른 필드보다 한 필드만 자동으로 사용하는 것을 방지했습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-285">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="ca36e-286">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-286">**Mitigations**</span></span>

<span data-ttu-id="ca36e-287">모호한 지원 필드가 있는 속성은 사용할 필드가 명시적으로 지정되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-287">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="ca36e-288">예를 들어 흐름 API 사용:</span><span class="sxs-lookup"><span data-stu-id="ca36e-288">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="ca36e-289">DbContext.Entry는 이제 로컬 DetectChanges를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-289">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="ca36e-290">추적 문제 #13552</span><span class="sxs-lookup"><span data-stu-id="ca36e-290">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="ca36e-291">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-291">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ca36e-292">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-292">**Old behavior**</span></span>

<span data-ttu-id="ca36e-293">EF Core 3.0 이전에는 `DbContext.Entry`를 호출하면 추적된 모든 엔터티에 대해 변경 내용이 검색되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-293">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="ca36e-294">이로 인해 `EntityEntry`에 노출된 상태가 최신 상태로 유지되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-294">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="ca36e-295">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-295">**New behavior**</span></span>

<span data-ttu-id="ca36e-296">EF Core 3.0부터 `DbContext.Entry` 호출은 지정된 엔터티와 이와 관련된 추적된 모든 주체 엔터티의 변경 내용만 검색하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-296">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="ca36e-297">즉, 이 메서드를 호출하여 다른 곳의 변경 내용을 검색하지 못할 수 있으므로 애플리케이션 상태에 영향을 줄 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-297">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="ca36e-298">`ChangeTracker.AutoDetectChangesEnabled`를 `false`로 설정하면 이 로컬 변경 검색도 비활성화됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-298">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="ca36e-299">변경 검색(예: `ChangeTracker.Entries` 및 `SaveChanges`)을 유발하는 다른 메서드는 추적된 모든 엔터티의 전체 `DetectChanges`를 유발합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-299">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="ca36e-300">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-300">**Why**</span></span>

<span data-ttu-id="ca36e-301">이 변경으로 인해 `context.Entry`를 사용하는 기본 성능이 향상되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-301">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="ca36e-302">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-302">**Mitigations**</span></span>

<span data-ttu-id="ca36e-303">`Entry`를 호출하기 전에 명시적으로 `ChgangeTracker.DetectChanges()`를 호출하여 3.0 이전 버전의 동작을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-303">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="ca36e-304">문자열 및 바이트 배열 키는 기본적으로 클라이언트에서 생성되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-304">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="ca36e-305">추적 문제 #14617</span><span class="sxs-lookup"><span data-stu-id="ca36e-305">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="ca36e-306">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-306">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ca36e-307">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-307">**Old behavior**</span></span>

<span data-ttu-id="ca36e-308">EF Core 3.0 이전에는 null이 아닌 값을 명시적으로 설정하지 않고도 `string` 및 `byte[]` 키 속성을 사용할 수 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-308">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="ca36e-309">이 경우 키 값은 클라이언트에서 GUID로 생성되고 `byte[]`의 바이트로 직렬화됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-309">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="ca36e-310">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-310">**New behavior**</span></span>

<span data-ttu-id="ca36e-311">EF Core 3.0부터 키 값이 설정되지 않았음을 나타내는 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-311">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="ca36e-312">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-312">**Why**</span></span>

<span data-ttu-id="ca36e-313">클라이언트에서 생성한 `string`/`byte[]` 값은 일반적으로 유용하지 않고 기본 동작으로 인해 생성된 키 값을 일반적인 방식으로 추론하기가 어려웠기 때문에 이 변경이 이루어졌습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-313">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="ca36e-314">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-314">**Mitigations**</span></span>

<span data-ttu-id="ca36e-315">다른 null이 아닌 값이 설정되지 않은 경우 키 속성이 생성된 값을 사용해야 함을 명시적으로 지정하면 3.0 이전 버전 동작을 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-315">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="ca36e-316">예를 들어 흐름 API가 있는 경우:</span><span class="sxs-lookup"><span data-stu-id="ca36e-316">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="ca36e-317">데이터 주석이 있는 경우:</span><span class="sxs-lookup"><span data-stu-id="ca36e-317">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="ca36e-318">ILoggerFactory는 이제 범위가 지정된 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-318">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="ca36e-319">추적 문제 #14698</span><span class="sxs-lookup"><span data-stu-id="ca36e-319">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="ca36e-320">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-320">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ca36e-321">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-321">**Old behavior**</span></span>

<span data-ttu-id="ca36e-322">EF Core 3.0 이전에는 `ILoggerFactory`가 싱글톤 서비스로 등록되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-322">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="ca36e-323">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-323">**New behavior**</span></span>

<span data-ttu-id="ca36e-324">EF Core 3.0부터 이제 `ILoggerFactory`는 범위가 지정된 대로 등록됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-324">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="ca36e-325">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-325">**Why**</span></span>

<span data-ttu-id="ca36e-326">이 변경으로 인해 `DbContext` 인스턴스와 로거를 연결하여 다른 기능을 활성화하고 내부 서비스 공급자의 급증과 같은 병리적인 동작의 일부 사례가 제거됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-326">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="ca36e-327">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-327">**Mitigations**</span></span>

<span data-ttu-id="ca36e-328">이 변경 내용은 EF Core 내부 서비스 공급자에 사용자 지정 서비스를 등록하고 사용하지 않는 한 애플리케이션 코드에 영향을 미치지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-328">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="ca36e-329">이는 일반적이지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-329">This isn't common.</span></span>
<span data-ttu-id="ca36e-330">이러한 경우 대부분의 작업은 여전히 작동하지만 `ILoggerFactory`에 종속된 싱글톤 서비스는 `ILoggerFactory`를 얻기 위해 다른 방식으로 변경해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-330">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="ca36e-331">이와 같은 상황에 처한 경우 [EF Core GitHub 문제 추적기](https://github.com/aspnet/EntityFrameworkCore/issues)에 문제를 제출하여 향후 이 문제를 해결할 수 있는 방법을 더 잘 이해할 수 있도록 `ILoggerFactory`를 사용하는 방법을 알려주세요.</span><span class="sxs-lookup"><span data-stu-id="ca36e-331">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="ca36e-332">IDbContextOptionsExtensionWithDebugInfo가 IDbContextOptionsExtension에 병합됨</span><span class="sxs-lookup"><span data-stu-id="ca36e-332">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="ca36e-333">추적 문제 #13552</span><span class="sxs-lookup"><span data-stu-id="ca36e-333">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="ca36e-334">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-334">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ca36e-335">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-335">**Old behavior**</span></span>

<span data-ttu-id="ca36e-336">`IDbContextOptionsExtensionWithDebugInfo`는 2.x 릴리스 주기 동안 인터페이스에 대한 호환성이 손상되는 변경을 방지하기 위해 `IDbContextOptionsExtension`에서 확장된 추가 선택적 인터페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-336">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="ca36e-337">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-337">**New behavior**</span></span>

<span data-ttu-id="ca36e-338">이제 인터페이스가 `IDbContextOptionsExtension`으로 병합됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-338">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="ca36e-339">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-339">**Why**</span></span>

<span data-ttu-id="ca36e-340">이 변경은 인터페이스가 개념적으로 하나이기 때문에 이루어졌습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-340">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="ca36e-341">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-341">**Mitigations**</span></span>

<span data-ttu-id="ca36e-342">새 멤버를 지원하기 위해 `IDbContextOptionsExtension` 구현을 업데이트해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-342">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="ca36e-343">지연 로드 프록시는 더 이상 탐색 속성이 완전히 로드되었다고 가정하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-343">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="ca36e-344">추적 문제 #12780</span><span class="sxs-lookup"><span data-stu-id="ca36e-344">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="ca36e-345">이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-345">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ca36e-346">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-346">**Old behavior**</span></span>

<span data-ttu-id="ca36e-347">EF Core 3.0 이전에는 `DbContext`가 삭제되면 해당 컨텍스트에서 가져온 엔터티의 지정된 탐색 속성이 완전히 로드되었는지 여부를 알 수 없었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-347">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="ca36e-348">대신 프록시는 참조 탐색에 null이 아닌 값이 있으면 로드되고 컬렉션 탐색이 비어 있지 않으면 로드된다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-348">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="ca36e-349">이러한 경우 지연 로드를 시도하면 아무런 작업도 수행되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-349">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="ca36e-350">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-350">**New behavior**</span></span>

<span data-ttu-id="ca36e-351">EF Core 3.0부터 프록시는 탐색 속성이 로드되었는지 여부를 추적합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-351">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="ca36e-352">즉, 컨텍스트가 삭제된 후에 로드되는 탐색 속성에 액세스하려고 하면 로드된 탐색이 비어 있거나 null인 경우에도 항상 아무런 작업도 수행되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-352">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="ca36e-353">반대로, 로드되지 않은 탐색 속성에 액세스하려고 하면 탐색 속성이 비어 있지 않은 컬렉션이라도 컨텍스트가 삭제되면 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-353">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="ca36e-354">이 상황이 발생하는 경우 애플리케이션 코드가 잘못된 시간에 지연 로드를 사용하려고 시도하고 있음을 의미하며, 애플리케이션이 이를 수행하지 않도록 변경해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-354">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="ca36e-355">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-355">**Why**</span></span>

<span data-ttu-id="ca36e-356">이 변경으로 인해 삭제된 `DbContext` 인스턴스에 지연 로드를 시도할 때 동작을 일관되고 정확하게 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-356">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="ca36e-357">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-357">**Mitigations**</span></span>

<span data-ttu-id="ca36e-358">애플리케이션을 업데이트하여 삭제된 컨텍스트로 지연 로드를 시도하지 않도록 하거나 예외 메시지에 설명된 대로 작업을 수행하지 않도록 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-358">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="ca36e-359">내부 서비스 공급자의 과도한 생성은 이제 기본적으로 오류입니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-359">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="ca36e-360">추적 문제 #10236</span><span class="sxs-lookup"><span data-stu-id="ca36e-360">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="ca36e-361">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-361">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ca36e-362">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-362">**Old behavior**</span></span>

<span data-ttu-id="ca36e-363">EF Core 3.0 이전에는 병리적인 수의 내부 서비스 공급자를 만드는 애플리케이션에 경고가 기록되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-363">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="ca36e-364">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-364">**New behavior**</span></span>

<span data-ttu-id="ca36e-365">EF Core 3.0부터 이제 이 경고가 고려되며 오류와 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-365">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="ca36e-366">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-366">**Why**</span></span>

<span data-ttu-id="ca36e-367">이 변경으로 인해 병리적인 사례를 더 명시적으로 노출하여 더 나은 애플리케이션 코드를 구동하게 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-367">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="ca36e-368">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-368">**Mitigations**</span></span>

<span data-ttu-id="ca36e-369">이 오류가 발생했을 때 가장 적절한 조치는 근본 원인을 이해하고 많은 내부 서비스 공급자를 만드는 것을 중단하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-369">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="ca36e-370">그러나 오류는 `DbContextOptionsBuilder`의 구성을 통해 경고(또는 무시)로 다시 변환될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-370">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="ca36e-371">예:</span><span class="sxs-lookup"><span data-stu-id="ca36e-371">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="ca36e-372">관계형:TypeMapping 주석은 이제 TypeMapping일 뿐입니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-372">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="ca36e-373">추적 문제 #9913</span><span class="sxs-lookup"><span data-stu-id="ca36e-373">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="ca36e-374">이 변경 내용은 EF Core 3.0 미리 보기 2에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-374">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="ca36e-375">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-375">**Old behavior**</span></span>

<span data-ttu-id="ca36e-376">형식 매핑 주석에 대한 주석 이름은 "관계형:TypeMapping"이었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-376">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="ca36e-377">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-377">**New behavior**</span></span>

<span data-ttu-id="ca36e-378">형식 매핑 주석에 대한 주석 이름은 이제 "TypeMapping"입니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-378">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="ca36e-379">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-379">**Why**</span></span>

<span data-ttu-id="ca36e-380">이제 형식 매핑은 관계형 데이터베이스 공급자 이상의 용도로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-380">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="ca36e-381">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-381">**Mitigations**</span></span>

<span data-ttu-id="ca36e-382">이는 일반적이지 않은 주석으로 형식 매핑에 직접 액세스하는 애플리케이션만 중단합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-382">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="ca36e-383">수정하기 위한 가장 적절한 작업은 직접 주석을 사용하는 대신 API 화면을 사용하여 형식 매핑에 액세스하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-383">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="ca36e-384">파생된 형식의 ToTable에서 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-384">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="ca36e-385">추적 문제 #11811</span><span class="sxs-lookup"><span data-stu-id="ca36e-385">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="ca36e-386">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-386">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ca36e-387">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-387">**Old behavior**</span></span>

<span data-ttu-id="ca36e-388">EF Core 3.0 이전에는 유효하지 않은 경우 상속 매핑 전략만 TPH이므로 파생된 형식에 대해 호출된 `ToTable()`은 무시되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-388">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="ca36e-389">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-389">**New behavior**</span></span>

<span data-ttu-id="ca36e-390">EF Core 3.0부터 시작하여 이후 릴리스에서 TPT 및 TPC 지원을 추가하기 위해 파생 형식에서 호출된 `ToTable()`에서는 나중에 예기치 않은 매핑 변화를 방지하기 위해 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-390">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="ca36e-391">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-391">**Why**</span></span>

<span data-ttu-id="ca36e-392">현재 파생된 형식을 다른 테이블에 매핑하는 것은 올바르지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-392">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="ca36e-393">이 변경으로 인해 할 일이 유효하게 될 때 나중에 중단되는 것을 방지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-393">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="ca36e-394">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-394">**Mitigations**</span></span>

<span data-ttu-id="ca36e-395">파생된 형식을 다른 테이블에 매핑하려는 시도를 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-395">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="ca36e-396">ForSqlServerHasIndex가 HasIndex로 바뀝니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-396">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="ca36e-397">추적 문제 #12366</span><span class="sxs-lookup"><span data-stu-id="ca36e-397">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="ca36e-398">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-398">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ca36e-399">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-399">**Old behavior**</span></span>

<span data-ttu-id="ca36e-400">EF Core 3.0 이전에는 `ForSqlServerHasIndex().ForSqlServerInclude()`가 `INCLUDE`와 함께 사용되는 열을 구성하는 방법을 제공했습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-400">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="ca36e-401">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-401">**New behavior**</span></span>

<span data-ttu-id="ca36e-402">EF Core 3.0부터 인덱스에 `Include`를 사용하여 이제 관계형 수준에서 지원됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-402">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="ca36e-403">`HasIndex().ForSqlServerInclude()`을 사용하십시오.</span><span class="sxs-lookup"><span data-stu-id="ca36e-403">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="ca36e-404">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-404">**Why**</span></span>

<span data-ttu-id="ca36e-405">이 변경으로 인해 `Includes`가 있는 인덱스의 API를 모든 데이터베이스 공급자에 대해 한 곳으로 통합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-405">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

<span data-ttu-id="ca36e-406">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-406">**Mitigations**</span></span>

<span data-ttu-id="ca36e-407">위에 표시된 것처럼 새 API를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-407">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="ca36e-408">EF Core는 더 이상 SQLite FK 적용을 위한 pragma를 보내지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-408">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="ca36e-409">추적 문제 #12151</span><span class="sxs-lookup"><span data-stu-id="ca36e-409">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="ca36e-410">이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-410">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ca36e-411">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-411">**Old behavior**</span></span>

<span data-ttu-id="ca36e-412">EF Core 3.0 이전에는 EF Core가 SQLite에 대한 연결이 열릴 때 `PRAGMA foreign_keys = 1`을 보냈습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-412">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="ca36e-413">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-413">**New behavior**</span></span>

<span data-ttu-id="ca36e-414">EF Core 3.0부터 EF Core는 더 이상 SQLite에 대한 연결이 열릴 때 `PRAGMA foreign_keys = 1`을 보내지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-414">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="ca36e-415">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-415">**Why**</span></span>

<span data-ttu-id="ca36e-416">이 변경은 EF Core가 기본적으로 `SQLitePCLRaw.bundle_e_sqlite3`을 사용하기 때문에 이루어졌으며, 이는 FK 적용이 기본적으로 켜져 있고 연결이 열릴 때마다 명시적으로 활성화할 필요가 없음을 의미합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-416">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="ca36e-417">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-417">**Mitigations**</span></span>

<span data-ttu-id="ca36e-418">외래 키는 기본적으로 EF Core에 사용되는 SQLitePCLRaw.bundle_e_sqlite3에서 기본적으로 활성화됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-418">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="ca36e-419">다른 경우에는 연결 문자열에 `Foreign Keys=True`를 지정하여 외래 키를 활성화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-419">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="ca36e-420">Microsoft.EntityFrameworkCore.Sqlite는 이제 SQLitePCLRaw.bundle_e_sqlite3에 종속됩니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-420">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="ca36e-421">**이전 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-421">**Old behavior**</span></span>

<span data-ttu-id="ca36e-422">EF Core 3.0 이전에는 EF Core가 `SQLitePCLRaw.bundle_green`을 사용했습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-422">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="ca36e-423">**새 동작**</span><span class="sxs-lookup"><span data-stu-id="ca36e-423">**New behavior**</span></span>

<span data-ttu-id="ca36e-424">EF Core 3.0부터 EF Core가 `SQLitePCLRaw.bundle_e_sqlite3`을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-424">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="ca36e-425">**이유**</span><span class="sxs-lookup"><span data-stu-id="ca36e-425">**Why**</span></span>

<span data-ttu-id="ca36e-426">이 변경으로 인해 iOS에서 사용되는 SQLite의 버전이 다른 플랫폼과 일치되도록 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-426">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="ca36e-427">**완화 방법**</span><span class="sxs-lookup"><span data-stu-id="ca36e-427">**Mitigations**</span></span>

<span data-ttu-id="ca36e-428">iOS에서 네이티브 SQLite 버전을 사용하려면 다른 `SQLitePCLRaw` 번들을 사용하도록 `Microsoft.Data.Sqlite`를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="ca36e-428">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>
