---
title: EF Core 3.0의 새로운 기능 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: cf0d2cf032b9aa319fe706aece5b1ea66a5d6251
ms.sourcegitcommit: a013e243a14f384999ceccaf9c779b8c1ae3b936
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463365"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="f06a5-102">EF Core 3.0에 포함된 새로운 기능(현재 미리 보기 상태)</span><span class="sxs-lookup"><span data-stu-id="f06a5-102">New features included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f06a5-103">이후 릴리스의 기능 집합 및 일정은 항상 변경될 수 있으며 이 페이지를 최신 상태로 유지하려고 노력함에도 불구하고 최신 계획을 항상 반영할 수 없을 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="f06a5-104">다음 목록에는 EF Core 3.0용으로 계획된 주요한 새 기능이 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-104">The following list includes the major new features planned for EF Core 3.0.</span></span>
<span data-ttu-id="f06a5-105">이러한 기능의 대부분은 현재 미리 보기에 포함되어 있지 않지만, RTM을 진행함에 따라 제공될 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-105">Most of these features are not included in the current preview, but will become available as we make progress towards RTM.</span></span>

<span data-ttu-id="f06a5-106">그 이유는 릴리스 초기에 계획된 [호환성이 손상되는 변경](xref:core/what-is-new/ef-core-3.0/breaking-changes)을 구현하는 데 집중하고 있기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-106">The reason is that at the beginning of the release we are focusing on implementing planned [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span></span>
<span data-ttu-id="f06a5-107">이러한 많은 호환성이 손상되는 변경은 EF Core의 개선 사항입니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-107">Many of these breaking changes are improvements to EF Core on their own.</span></span>
<span data-ttu-id="f06a5-108">많은 기타 항목에서 추가 개선 사항을 차단 해제해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-108">Many others are required to unblock further improvements.</span></span> 

<span data-ttu-id="f06a5-109">진행 중인 버그 수정 및 개선 사항의 전체 목록은 [문제 추적기의 이 쿼리](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="f06a5-109">For a complete list of bug fixes and enhancements underway, you can see [this query in our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span></span>

## <a name="linq-improvements"></a><span data-ttu-id="f06a5-110">LINQ 기능 향상</span><span class="sxs-lookup"><span data-stu-id="f06a5-110">LINQ improvements</span></span> 

[<span data-ttu-id="f06a5-111">추적 문제 #12795</span><span class="sxs-lookup"><span data-stu-id="f06a5-111">Tracking Issue #12795</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

<span data-ttu-id="f06a5-112">이 기능에 대한 작업이 시작되었지만 현재 미리 보기에는 포함되어 있지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-112">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="f06a5-113">LINQ를 사용하면 선택한 언어로 데이터베이스 쿼리를 작성할 수 있어서 다양한 종류의 정보를 활용하여 IntelliSense 및 컴파일 시간 형식 검사를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-113">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to get IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="f06a5-114">그러나 LINQ를 사용하면 복잡한 쿼리를 무제한으로 작성할 수도 있습니다. 이는 LINQ 공급자에게 항상 큰 문제였습니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-114">But LINQ also enables you to write an unlimited number of complicated queries, and that has always been a huge challenge for LINQ providers.</span></span>
<span data-ttu-id="f06a5-115">EF Core의 처음 몇 버전에서는 쿼리 중 SQL로 변환할 수 있는 부분을 생각한 다음, 나머지 쿼리가 클라이언트의 메모리에서 실행되도록 허용하여 이 문제를 부분적으로 해결했습니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-115">In the first few versions of EF Core, we solved that in part by figuring out what portions of a query could be translated to SQL, and then by allowing the rest of the query to execute in memory on the client.</span></span>
<span data-ttu-id="f06a5-116">이러한 클라이언트 쪽 실행은 일부 상황에서는 바람직하지만, 다른 많은 경우에는 애플리케이션이 프로덕션에 배포될 때까지 식별되지 않을 수 있는 비효율적인 쿼리가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-116">This client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries that may not be identified until an application is deployed to production.</span></span>
<span data-ttu-id="f06a5-117">EF Core 3.0에서는 LINQ 구현 방식과 테스트 방법을 크게 변화시킬 계획입니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-117">In EF Core 3.0, we're planning to make profound changes to how our LINQ implementation works, and how we test it.</span></span>
<span data-ttu-id="f06a5-118">목표는 패치 릴리스에서 쿼리 중단을 방지하는 등 더욱 강력하게 만들고, 더 많은 식을 SQL로 정확하게 변환하고, 더 많은 경우에 효율적인 쿼리를 생성하며, 비효율적인 쿼리가 검색되지 않는 것을 방지하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-118">The goals are to make it more robust (for example, to avoid breaking queries in patch releases), to enable translating more expressions correctly into SQL, to generate efficient queries in more cases, and to prevent inefficient queries from going undetected.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="f06a5-119">Cosmos DB 지원</span><span class="sxs-lookup"><span data-stu-id="f06a5-119">Cosmos DB support</span></span> 

[<span data-ttu-id="f06a5-120">추적 문제 #8443</span><span class="sxs-lookup"><span data-stu-id="f06a5-120">Tracking Issue #8443</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

<span data-ttu-id="f06a5-121">이 기능은 현재 미리 보기에 포함되어 있지만 아직 완전하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-121">This feature is included in the current preview, but isn't complete yet.</span></span> 

<span data-ttu-id="f06a5-122">Microsoft는 개발자가 EF 프로그래밍 모델에 친숙해져서 애플리케이션 데이터베이스로 Azure Cosmos DB를 쉽게 대상으로 지정할 수 있도록 하기 위해 EF Core의 Cosmos DB 공급자에 공을 들이고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-122">We're working on a Cosmos DB provider for EF Core, to enable developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="f06a5-123">목표는 글로벌 배포, "always on" 가용성, 탄력적인 확장성, 짧은 대기 시간, .NET 개발자에 훨씬 더 많은 액세스 가능성과 같은 몇몇 Cosmos DB의 장점을 활용하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-123">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="f06a5-124">공급자는 Cosmos DB의 SQL API에 대해 자동 변경 내용 추적, LINQ, 값 변환 같은 대부분의 EF Core 기능을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-124">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>
<span data-ttu-id="f06a5-125">Microsoft는 EF Core 2.2 전에 이러한 노력을 시작했으며, [공급자의 몇몇 미리 보기 버전을 제공했습니다](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span><span class="sxs-lookup"><span data-stu-id="f06a5-125">We started this effort before EF Core 2.2, and [we have made some preview versions of the provider available](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span></span>
<span data-ttu-id="f06a5-126">새 계획은 EF Core 3.0과 함께 공급자를 계속 개발하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-126">The new plan is to continue developing the provider alongside EF Core 3.0.</span></span> 

## <a name="c-80-support"></a><span data-ttu-id="f06a5-127">C# 8.0 지원</span><span class="sxs-lookup"><span data-stu-id="f06a5-127">C# 8.0 support</span></span>

<span data-ttu-id="f06a5-128">[추적 문제 #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[추적 문제 #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span><span class="sxs-lookup"><span data-stu-id="f06a5-128">[Tracking Issue #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Tracking Issue #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span></span>

<span data-ttu-id="f06a5-129">이 기능에 대한 작업이 시작되었지만 현재 미리 보기에는 포함되어 있지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-129">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="f06a5-130">고객이 EF Core를 사용하면서 비동기 스트림(`await foreach` 포함) 및 nullable 참조 형식과 같은 [C# 8.0의 새 기능](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/)의 일부를 활용하기를 바랍니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-130">We want our customers to take advantage of some of the [new features coming in C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) like async streams (including `await foreach`) and nullable reference types while using EF Core.</span></span>

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="f06a5-131">데이터베이스 뷰의 리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="f06a5-131">Reverse engineering of database views</span></span>

[<span data-ttu-id="f06a5-132">추적 문제 #1679</span><span class="sxs-lookup"><span data-stu-id="f06a5-132">Tracking Issue #1679</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

<span data-ttu-id="f06a5-133">이 기능은 현재 미리 보기에 포함되어 있지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-133">This feature isn't included in the current preview.</span></span>

<span data-ttu-id="f06a5-134">EF Core 2.1에서 도입되고 EF Core 3.0에서 키가 없는 엔터티 형식으로 간주되는 [쿼리 유형](xref:core/modeling/query-types)은 데이터베이스에서 읽을 수 있지만 업데이트할 수 없는 데이터를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-134">[Query types](xref:core/modeling/query-types), introduced in EF Core 2.1 and considered entity types without keys in EF Core 3.0, represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="f06a5-135">이 특성은 대부분의 시나리오에서 데이터베이스 뷰에 매우 적합하므로 리버스 엔지리어닝 데이터베이스 뷰에서 키 없이 엔터티 형식을 자동화할 계획입니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-135">This characteristic makes them an excellent fit for database views in most scenarios, so we plan to automate the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="property-bag-entities"></a><span data-ttu-id="f06a5-136">속성 모음 엔터티</span><span class="sxs-lookup"><span data-stu-id="f06a5-136">Property bag entities</span></span> 

<span data-ttu-id="f06a5-137">[추적 문제 #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) 및 [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span><span class="sxs-lookup"><span data-stu-id="f06a5-137">[Tracking Issue #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) and [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span></span>

<span data-ttu-id="f06a5-138">이 기능에 대한 작업이 시작되었지만 현재 미리 보기에는 포함되어 있지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-138">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="f06a5-139">이 기능은 일반 속성 대신 인덱싱된 속성에 데이터를 저장하는 엔터티를 사용하고 동일한 .NET 클래스의 인스턴스(잠재적으로 `Dictionary<string, object>`만큼 단순한 인스턴스)를 사용하여 동일한 EF Core 모델에서 여러 엔터티 형식을 나타낼 수 있도록 하는 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-139">This feature is about enabling entities that store data in indexed properties instead of regular properties, and also about being able to use instances of the same .NET class (potentially something as simple as a `Dictionary<string, object>`) to represent different entity types in the same EF Core model.</span></span>
<span data-ttu-id="f06a5-140">이 기능은 EF Core에 대해 가장 요청이 많았던 기능 향상 중 하나인 조인 엔터티가 없는 다 대 다 관계를 지원하는 발판입니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-140">This feature is a stepping stone to support many-to-many relationships without a join entity, which is one of the most requested improvements for EF Core.</span></span>

## <a name="ef-63-on-net-core"></a><span data-ttu-id="f06a5-141">.NET Core의 EF 6.3</span><span class="sxs-lookup"><span data-stu-id="f06a5-141">EF 6.3 on .NET Core</span></span> 

[<span data-ttu-id="f06a5-142">추적 문제 EF6#271</span><span class="sxs-lookup"><span data-stu-id="f06a5-142">Tracking Issue EF6#271</span></span>](https://github.com/aspnet/EntityFramework6/issues/271)

<span data-ttu-id="f06a5-143">이 기능에 대한 작업이 시작되었지만 현재 미리 보기에는 포함되어 있지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-143">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="f06a5-144">많은 기존 애플리케이션이 이전 버전의 EF를 사용하고 단지 .NET Core를 활용하기 위해 이전 버전의 EF를 EF Core로 포팅하는 일은 상당한 노력이 필요할 수 있다는 점을 이해합니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-144">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="f06a5-145">그러한 이유로, Microsoft는 다음 버전의 EF 6가 .NET Core 3.0에서 실행되도록 조정할 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-145">For that reason, we will be adapting the next version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="f06a5-146">이는 최소한의 변경으로 기존 애플리케이션의 포팅을 간단히 수행하기 위해서입니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-146">We are doing this to facilitate porting existing applications with minimal changes.</span></span>
<span data-ttu-id="f06a5-147">몇 가지 제한 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-147">There are going to be some limitations.</span></span> <span data-ttu-id="f06a5-148">예:</span><span class="sxs-lookup"><span data-stu-id="f06a5-148">For example:</span></span>
- <span data-ttu-id="f06a5-149">새로운 공급자는 .NET Core에 포함된 SQL Server 지원 외에 다른 데이터베이스로 작업해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-149">It will require new providers to work with other databases besides the included SQL Server support on .NET Core</span></span>
- <span data-ttu-id="f06a5-150">SQL Server를 사용한 공간 지원은 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-150">Spatial support with SQL Server won't be enabled</span></span>

<span data-ttu-id="f06a5-151">또한 현재 시점에서 EF 6을 위해 계획된 새로운 기능은 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f06a5-151">Note also that there are no new features planned for EF 6 at this point.</span></span>
