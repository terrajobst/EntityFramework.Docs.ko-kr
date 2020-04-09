---
title: EF Core를 사용하여 구성 요소 테스트 - EF Core
description: EF Core를 사용하는 애플리케이션을 테스트하는 다양한 방법
author: ajcvickers
ms.date: 03/23/2020
uid: core/miscellaneous/testing/index
ms.openlocfilehash: b1ab37ebb0a3aae09d5d5b225f746cf83dfba170
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634247"
---
# <a name="testing-code-that-uses-ef-core"></a><span data-ttu-id="26735-103">EF Core를 사용하는 테스트 코드</span><span class="sxs-lookup"><span data-stu-id="26735-103">Testing code that uses EF Core</span></span>

<span data-ttu-id="26735-104">데이터베이스에 액세스하는 코드를 테스트하려면 다음이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="26735-104">Testing code that accesses a database requires either:</span></span>
* <span data-ttu-id="26735-105">프로덕션에 사용되는 동일한 데이터베이스 시스템에 대해 쿼리 및 업데이트 실행</span><span class="sxs-lookup"><span data-stu-id="26735-105">Running queries and updates against the same database system used in production.</span></span>
* <span data-ttu-id="26735-106">관리하기가 좀 더 용이한 다른 데이터베이스 시스템에 대해 쿼리 및 업데이트 실행</span><span class="sxs-lookup"><span data-stu-id="26735-106">Running queries and updates against some other easier to manage database system.</span></span>
* <span data-ttu-id="26735-107">이중 테스트 또는 데이터베이스를 사용하지 않는 기타 메커니즘 사용</span><span class="sxs-lookup"><span data-stu-id="26735-107">Using test doubles or some other mechanism to avoid using a database at all.</span></span>

<span data-ttu-id="26735-108">이 문서에서는 이러한 각 선택 옵션에 포함된 장단점을 간략하게 설명하고 각 접근 방식에 EF Core를 사용하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="26735-108">This document outlines the trade offs involved in each of these choices and shows how EF Core can be used with each approach.</span></span>  

## <a name="all-database-providers-are-not-equal"></a><span data-ttu-id="26735-109">모든 데이터베이스 공급자가 같은 것은 아님</span><span class="sxs-lookup"><span data-stu-id="26735-109">All database providers are not equal</span></span>

<span data-ttu-id="26735-110">EF Core가 기본 데이터베이스 시스템의 모든 측면을 추상화하도록 디자인되지는 않았다는 사실을 이해하는 것은 매우 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="26735-110">It is very important to understand that EF Core is not designed to abstract every aspect of the underlying database system.</span></span>
<span data-ttu-id="26735-111">대신, EF Core는 모든 데이터베이스 시스템에서 사용할 수 있는 일반적인 패턴 및 개념 세트입니다.</span><span class="sxs-lookup"><span data-stu-id="26735-111">Instead, EF Core is a common set of patterns and concepts that can be used with any database system.</span></span>
<span data-ttu-id="26735-112">EF Core 데이터베이스 공급자는 이 공통 프레임워크에 대한 데이터베이스 관련 동작 및 기능을 계층화합니다.</span><span class="sxs-lookup"><span data-stu-id="26735-112">EF Core database providers then layer database-specific behavior and functionality over this common framework.</span></span>
<span data-ttu-id="26735-113">이를 통해 적절한 경우 각 데이터베이스 시스템은 다른 데이터베이스 시스템과 공통된 측면을 유지하면서 최상의 작업 결과를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-113">This allows each database system to do what it does best while still maintaining commonality, where appropriate, with other database systems.</span></span> 

<span data-ttu-id="26735-114">기본적으로 데이터베이스 공급자를 전환할 경우 EF Core 동작이 변경되며 애플리케이션은 동작의 모든 차이점을 명확히 고려하지 못한다면 제대로 작동하지 못할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-114">Fundamentally, this means that switching out the database provider will change EF Core behavior and the application can't be expected to function correctly unless it explicitly accounts for all differences in behavior.</span></span>
<span data-ttu-id="26735-115">즉, 대부분의 경우 여러 관계형 데이터베이스가 상당한 공통점을 공유하므로 이러한 측면이 가능해집니다.</span><span class="sxs-lookup"><span data-stu-id="26735-115">That being said, in many cases doing this will work because there is a high degree of commonality amongst relational databases.</span></span>
<span data-ttu-id="26735-116">이러한 측면에는 장단점이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-116">This is good and bad.</span></span>
<span data-ttu-id="26735-117">데이터베이스 간을 전환하기가 비교적 쉽다는 장점이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-117">Good because moving between databases can be relatively easy.</span></span>
<span data-ttu-id="26735-118">단점은 애플리케이션이 새 데이터베이스 시스템에 대해 완전히 테스트되지 않은 경우 잘못된 보안 인식을 제공할 수 있다는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="26735-118">Bad because it can give a false sense of security if the application is not fully tested against the new database system.</span></span>  

## <a name="approach-1-production-database-system"></a><span data-ttu-id="26735-119">방법 1: 프로덕션 데이터베이스 시스템</span><span class="sxs-lookup"><span data-stu-id="26735-119">Approach 1: Production database system</span></span>

<span data-ttu-id="26735-120">이전 섹션에서 설명한 것처럼 프로덕션 환경에서 실행되는 작업을 테스트하는 유일한 방법은 동일한 데이터베이스 시스템을 사용하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="26735-120">As described in the previous section, the only way to be sure you are testing what runs in production is to use the same database system.</span></span>
<span data-ttu-id="26735-121">예를 들어 배포된 애플리케이션이 SQL Azure를 사용하는 경우 SQL Azure에 대해서도 테스트를 수행해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="26735-121">For example, if the deployed application uses SQL Azure, then testing should also be done against SQL Azure.</span></span>

<span data-ttu-id="26735-122">그러나 모든 개발자가 실제로 코드 작업을 수행하면서 SQL Azure에 대해 테스트를 실행하도록 하면 작업이 느려지고 비용이 많이 들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-122">However, having every developer run tests against SQL Azure while actively working on the code would be both slow and expensive.</span></span>
<span data-ttu-id="26735-123">이러한 사실은 이러한 방식 전반에 나타나는 주요 장단점을 나타냅니다. 테스트 효율성을 향상하기 위해 프로덕션 데이터베이스 시스템에서 언제 벗어나는 것이 적절할까요?</span><span class="sxs-lookup"><span data-stu-id="26735-123">This illustrates the main trade off involved throughout these approaches: when is it appropriate to deviate from the production database system so as to improve test efficiency?</span></span>

<span data-ttu-id="26735-124">다행히 이 경우에는 답변하기 쉽습니다. 개발자 테스트를 위해 로컬 또는 온-프레미스 SQL Server를 사용하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="26735-124">Luckily, in this case the answer is quite easy: use local or on-premises SQL Server for developer testing.</span></span>
<span data-ttu-id="26735-125">SQL Azure 및 SQL Server는 매우 유사하므로 일반적으로 SQL Server에 대해 테스트하는 것이 적절한 절충안입니다.</span><span class="sxs-lookup"><span data-stu-id="26735-125">SQL Azure and SQL Server are extremely similar, so testing against SQL Server is usually a reasonable trade off.</span></span>
<span data-ttu-id="26735-126">즉, 프로덕션으로 전환하기 전에 SQL Azure 자체에 대해 테스트를 실행하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-126">That being said, it is still wise to run tests against SQL Azure itself before going into production.</span></span>
 
### <a name="localdb"></a><span data-ttu-id="26735-127">LocalDb</span><span class="sxs-lookup"><span data-stu-id="26735-127">LocalDb</span></span> 

<span data-ttu-id="26735-128">모든 주요 데이터베이스 시스템에는 로컬 테스트를 위한 일종의 “Developer Edition”이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-128">All the major database systems have some form of "Developer Edition" for local testing.</span></span>
<span data-ttu-id="26735-129">또한 SQL Server에는 [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15)라는 기능이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-129">SQL Server also also has a feature called [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).</span></span>
<span data-ttu-id="26735-130">LocalDb의 주요 이점은 필요할 때 데이터베이스 인스턴스를 실행한다는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="26735-130">The primary advantage of LocalDb is that it spins up the database instance on demand.</span></span>
<span data-ttu-id="26735-131">이렇게 하면 테스트를 실행하지 않는 경우에도 머신에서 데이터베이스 서비스를 계속 실행해야 하는 경우를 방지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-131">This avoids having a database service running on your machine even when you're not running tests.</span></span>

<span data-ttu-id="26735-132">LocalDb에 문제가 없는 것도 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="26735-132">LocalDb is not without it's issues:</span></span>
* <span data-ttu-id="26735-133">[SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15)이 지원하는 모든 기능을 지원하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-133">It doesn't support everything that [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15) does.</span></span>
* <span data-ttu-id="26735-134">Linux에서는 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-134">It isn't available on Linux.</span></span>
* <span data-ttu-id="26735-135">서비스가 실행될 때 첫 번째 테스트 실행에서 지연이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-135">It can cause lag on first test run as the service is spun up.</span></span>

<span data-ttu-id="26735-136">개인적으로 데이터베이스 서비스를 개발 머신에서 실행할 때 문제가 확인된 적은 없으며 Developer Edition을 대신 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-136">Personally, I've never found it a problem having a database service running on my dev machine and I would generally recommend using Developer Edition instead.</span></span>
<span data-ttu-id="26735-137">그러나 일부 사용자의 경우 덜 강력한 개발 머신에서 작업하는 것이 적절할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-137">However, it may be appropriate for some people, especially on less powerful dev machines.</span></span>  

## <a name="approach-2-sqlite"></a><span data-ttu-id="26735-138">방법 2: SQLite</span><span class="sxs-lookup"><span data-stu-id="26735-138">Approach 2: SQLite</span></span>

<span data-ttu-id="26735-139">EF Core는 주로 로컬 SQL Server 인스턴스에서 실행하여 여 SQL Server 공급자를 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="26735-139">EF Core tests the SQL Server provider primarily by running it against a local SQL Server instance.</span></span>
<span data-ttu-id="26735-140">이러한 테스트는 고속 머신에서 몇 분 이내에 수만 개의 쿼리를 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="26735-140">These tests run tens of thousands of queries in a couple of minutes on a fast machine.</span></span>
<span data-ttu-id="26735-141">이러한 테스트는 실제 데이터베이스 시스템을 사용하는 것이 보다 나은 성능을 제공하는 솔루션이라는 것을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="26735-141">This illustrates that using the real database system can be a performant solution.</span></span>
<span data-ttu-id="26735-142">좀 더 경량의 데이터베이스를 사용하는 것이 테스트를 신속하게 실행하는 유일한 방법이라는 근거는 없습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-142">It is a myth that using some lighter-weight database is the only way to run tests quickly.</span></span>

<span data-ttu-id="26735-143">즉, 프로덕션 데이터베이스 시스템 가까이에 있는 항목에 대해 테스트를 실행할 수 없다면 어떻게 할까요?</span><span class="sxs-lookup"><span data-stu-id="26735-143">That being said, what if for whatever reason you can't run tests against something close to your production database system?</span></span>
<span data-ttu-id="26735-144">다음 조건은 비슷한 기능을 가진 항목을 사용하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="26735-144">The next best choice is to use something with similar functionality.</span></span>
<span data-ttu-id="26735-145">일반적으로 다른 관계형 데이터베이스를 의미하므로 [SQLite](https://sqlite.org/index.html)를 선택하는 것이 적절합니다.</span><span class="sxs-lookup"><span data-stu-id="26735-145">This usually means another relational database, for which [SQLite](https://sqlite.org/index.html) is the obvious choice.</span></span>

<span data-ttu-id="26735-146">SQLite는 다음과 같은 이유로 적합합니다.</span><span class="sxs-lookup"><span data-stu-id="26735-146">SQLite is a good choice because:</span></span>
* <span data-ttu-id="26735-147">애플리케이션에서 In-Process로 실행되므로 오버헤드가 낮습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-147">It runs in-process with your application and so has low overhead.</span></span>
* <span data-ttu-id="26735-148">데이터베이스에 대해 자동으로 생성되는 간단한 파일을 사용하므로 데이터베이스를 관리할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-148">It uses simple, automatically created files for databases, and so doesn't require database management.</span></span>
* <span data-ttu-id="26735-149">파일 생성도 방지하는 메모리 내 모드를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="26735-149">It has an in-memory mode that avoids even the file creation.</span></span>

<span data-ttu-id="26735-150">그러나 다음 사항에 주의하세요.</span><span class="sxs-lookup"><span data-stu-id="26735-150">However, remember that:</span></span>
* <span data-ttu-id="26735-151">SQLite가 프로덕션 데이터베이스 시스템에서 수행하는 모든 작업을 반드시 지원하는 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="26735-151">SQLite inevitability doesn't support everything that your production database system does.</span></span>
* <span data-ttu-id="26735-152">SQLite는 일부 쿼리에 대해 프로덕션 데이터베이스 시스템과 다르게 동작합니다.</span><span class="sxs-lookup"><span data-stu-id="26735-152">SQLite will behave differently than your production database system for some queries.</span></span>

<span data-ttu-id="26735-153">따라서 일부 테스트에 SQLite를 사용하는 경우 실제 데이터베이스 시스템에 대해서도 테스트를 수행해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="26735-153">So if you do use SQLite for some testing, make sure to also test against your real database system.</span></span>

<span data-ttu-id="26735-154">EF Core 특정 지침은 [SQLite 테스트](xref:core/miscellaneous/testing/sqlite)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="26735-154">See [Testing with SQLite](xref:core/miscellaneous/testing/sqlite) for EF Core specific guidance.</span></span> 

## <a name="approach-3-the-ef-core-in-memory-database"></a><span data-ttu-id="26735-155">방법 3: EF Core 메모리 내 데이터베이스</span><span class="sxs-lookup"><span data-stu-id="26735-155">Approach 3: The EF Core in-memory database</span></span>

<span data-ttu-id="26735-156">EF Core는 EF Core 자체의 내부 테스트에 사용되는 메모리 내 데이터베이스와 함께 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="26735-156">EF Core comes with an in-memory database that we use for internal testing of EF Core itself.</span></span>
<span data-ttu-id="26735-157">이 데이터베이스는 일반적으로 **EF Core를 사용하는 테스트 애플리케이션을 대신하기에는 적합하지 않습니다**.</span><span class="sxs-lookup"><span data-stu-id="26735-157">This database is in general **not suitable as a substitute for testing applications that use EF Core**.</span></span> <span data-ttu-id="26735-158">구체적으로는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-158">Specifically:</span></span>
* <span data-ttu-id="26735-159">관계형 데이터베이스가 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="26735-159">It is not a relational database</span></span>
* <span data-ttu-id="26735-160">트랜잭션을 지원하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-160">It doesn't support transactions</span></span>
* <span data-ttu-id="26735-161">성능에 대해 최적화되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-161">It is not optimized for performance</span></span>

<span data-ttu-id="26735-162">특히 데이터베이스가 테스트와 관련이 없는 경우 사용하기 때문에 EF Core 내부 테스트에는 별로 중요하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-162">None of this is very important when testing EF Core internals because we use it specifically where the database is irrelevant to the test.</span></span>
<span data-ttu-id="26735-163">반면, EF Core를 사용하는 애플리케이션을 테스트할 때는 매우 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="26735-163">On the other hand, these things tend to be very important when testing an application that uses EF Core.</span></span>

## <a name="unit-testing"></a><span data-ttu-id="26735-164">단위 테스트</span><span class="sxs-lookup"><span data-stu-id="26735-164">Unit testing</span></span>

<span data-ttu-id="26735-165">데이터베이스의 일부 데이터를 사용해야 할 수 있지만 기본적으로 데이터베이스 상호 작용을 테스트하지 않는 비즈니스 논리를 테스트하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-165">Consider testing a piece of business logic that might need to use some data from a database, but is not inherently testing the database interactions.</span></span>
<span data-ttu-id="26735-166">한 가지 옵션은 모의 또는 가짜와 같은 [이중 테스트](https://en.wikipedia.org/wiki/Test_double)를 사용하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="26735-166">One option is to use a [test double](https://en.wikipedia.org/wiki/Test_double) such as a mock or fake.</span></span>

<span data-ttu-id="26735-167">EF Core의 내부 테스트를 위해 이중 테스트를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="26735-167">We use test doubles for internal testing of EF Core.</span></span>
<span data-ttu-id="26735-168">그러나 DbContext 또는 IQueryable을 모의로 테스트해서는 안 됩니다.</span><span class="sxs-lookup"><span data-stu-id="26735-168">However, we never try to mock DbContext or IQueryable.</span></span>
<span data-ttu-id="26735-169">이 작업을 수행하는 것은 어렵고 번거로우며 허술할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="26735-169">Doing so is difficult, cumbersome, and fragile.</span></span>
<span data-ttu-id="26735-170">**따라서 피해야 합니다.**</span><span class="sxs-lookup"><span data-stu-id="26735-170">**Don't do it.**</span></span>

<span data-ttu-id="26735-171">대신 DbContext를 사용하여 단위 테스트를 수행할 때 메모리 내 데이터베이스를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="26735-171">Instead we use the in-memory database when unit testing something that uses DbContext.</span></span>
<span data-ttu-id="26735-172">이 경우 테스트가 데이터베이스 동작에 따라 좌우되지 않으므로 메모리 내 데이터베이스를 사용하는 것이 적합합니다.</span><span class="sxs-lookup"><span data-stu-id="26735-172">In this case using the in-memory database is appropriate because the test is not dependent on database behavior.</span></span>
<span data-ttu-id="26735-173">이 테스트를 통해 실제 데이터베이스 쿼리나 업데이트를 테스트하지는 마세요.</span><span class="sxs-lookup"><span data-stu-id="26735-173">Just don't do this to test actual database queries or updates.</span></span>   

<span data-ttu-id="26735-174">단위 테스트에 메모리 내 데이터베이스를 사용하는 방법에 대한 EF Core 관련 지침은 [메모리 내 공급자 테스트](xref:core/miscellaneous/testing/in-memory)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="26735-174">See [Testing with the in-memory provider](xref:core/miscellaneous/testing/in-memory) for EF Core specific guidance on using the in-memory database for unit testing.</span></span>
