---
title: EF Core 5.0의 새로운 기능
author: ajcvickers
ms.date: 03/15/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 08a93555fd76d8a9f6d3011f59d9a34f76d0b22f
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136252"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="16d3f-102">EF Core 5.0의 새로운 기능</span><span class="sxs-lookup"><span data-stu-id="16d3f-102">What's New in EF Core 5.0</span></span>

<span data-ttu-id="16d3f-103">현재 EF Core 5.0을 개발하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-103">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="16d3f-104">이 페이지에는 각 미리 보기에서 소개된 중요한 변경 내용의 개요가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-104">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="16d3f-105">이 페이지는 [EF Core 5.0의 플랜](plan.md)과 중복되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-105">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="16d3f-106">이 플랜에서는 최종 릴리스를 출시하기 전에 포함할 예정인 모든 항목을 비롯하여 EF Core 5.0의 전체 테마를 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-106">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="16d3f-107">공식 설명서가 게시되면 여기의 링크가 설명서에 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-107">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-1"></a><span data-ttu-id="16d3f-108">미리 보기 1</span><span class="sxs-lookup"><span data-stu-id="16d3f-108">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="16d3f-109">간단한 로깅</span><span class="sxs-lookup"><span data-stu-id="16d3f-109">Simple logging</span></span>

<span data-ttu-id="16d3f-110">이 기능은 EF6의 `Database.Log`와 유사한 기능을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-110">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="16d3f-111">즉, 어떠한 외부 로깅 프레임워크도 구성할 필요 없이 EF Core에서 로그를 가져오는 간단한 방법을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-111">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="16d3f-112">예비 설명서는 [2019년 12월 5일의 EF 주간 상태](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)에 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-112">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="16d3f-113">추가 설명서는 문제 [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-113">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="16d3f-114">SQL을 생성하는 간단한 방법</span><span class="sxs-lookup"><span data-stu-id="16d3f-114">Simple way to get generated SQL</span></span>

<span data-ttu-id="16d3f-115">EF Core 5.0에는 EF Core가 LINQ 쿼리를 실행할 때 생성하는 SQL을 반환하는 `ToQueryString` 확장 메서드가 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-115">EF Core 5.0 introduces the `ToQueryString` extension method which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="16d3f-116">예비 설명서는 [2020년 1월 9일의 EF 주간 상태](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246)에 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-116">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="16d3f-117">추가 설명서는 문제 [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-117">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="16d3f-118">C# 특성을 사용하여 엔터티에 키가 없음을 나타냄</span><span class="sxs-lookup"><span data-stu-id="16d3f-118">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="16d3f-119">이제 새 `KeylessAttribute`를 사용하는 키가 없는 것으로 엔터티 형식을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-119">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span>
<span data-ttu-id="16d3f-120">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="16d3f-120">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="16d3f-121">설명서는 문제 [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-121">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="16d3f-122">초기화된 DbContext에서 연결 또는 연결 문자열을 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-122">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="16d3f-123">이제 연결 문자열 또는 연결 문자열 없이 DbContext 인스턴스를 보다 쉽게 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-123">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="16d3f-124">또한 이제 컨텍스트 인스턴스에서 연결 또는 연결 문자열을 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-124">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="16d3f-125">이에 따라 동일한 컨텍스트 인스턴스를 다른 데이터베이스에 동적으로 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-125">This allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="16d3f-126">설명서는 문제 [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-126">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="16d3f-127">변경 내용 추적 프록시</span><span class="sxs-lookup"><span data-stu-id="16d3f-127">Change-tracking proxies</span></span>

<span data-ttu-id="16d3f-128">이제 EF Core에서 [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) 및 [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1)를 자동으로 구현하는 런타임 프록시를 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-128">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="16d3f-129">이후 엔터티 속성의 값 변경 내용을 EF Core에 직접 보고하므로 변경 내용을 별도로 검사하지 않아도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-129">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="16d3f-130">그러나 프록시에는 자체적으로 제한 사항이 적용되어 있어 일부 사용자에게는 적합하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-130">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="16d3f-131">설명서는 문제 [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-131">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="16d3f-132">향상된 디버그 보기</span><span class="sxs-lookup"><span data-stu-id="16d3f-132">Enhanced debug views</span></span>

<span data-ttu-id="16d3f-133">디버그 보기를 통해 문제를 디버깅할 때 EF Core의 내부를 쉽게 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-133">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="16d3f-134">모델의 디버그 보기가 며칠 전에 구현되었습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-134">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="16d3f-135">EF Core 5.0에서는 모델 보기를 간편하게 읽을 수 있으며, 상태 관리자에서 추적된 엔터티를 확인할 수 있는 새 디버그 보기가 추가되었습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-135">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="16d3f-136">예비 설명서는 [2019년 12월 12일의 EF 주간 상태](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206)에 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-136">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="16d3f-137">추가 설명서는 문제 [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-137">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="16d3f-138">향상된 데이터베이스 null 의미 체계 처리</span><span class="sxs-lookup"><span data-stu-id="16d3f-138">Improved handling of database null semantics</span></span>

<span data-ttu-id="16d3f-139">관계형 데이터베이스는 일반적으로 NULL을 알 수 없는 값으로 처리하므로 다른 NULL과 같지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-139">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="16d3f-140">반면 C#은 null을 다른 null과 동일하게 비교하는 정의된 값으로 처리합니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-140">C#, on the other hand, treats null as a defined value which compares equal to any other null.</span></span>
<span data-ttu-id="16d3f-141">기본적으로 EF Core는 쿼리를 변환하여 C# null 의미 체계를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-141">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="16d3f-142">EF Core 5.0은 이 변환 과정의 효율성을 크게 향상시킵니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-142">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="16d3f-143">설명서는 문제 [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-143">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="16d3f-144">인덱서 속성</span><span class="sxs-lookup"><span data-stu-id="16d3f-144">Indexer properties</span></span>

<span data-ttu-id="16d3f-145">EF Core 5.0는 C# 인덱서 속성의 매핑을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-145">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="16d3f-146">이에 따라 엔터티를 속성 모음으로 사용할 수 있으며 속성 모음의 명명된 속성으로 열이 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-146">This allows entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="16d3f-147">설명서는 문제 [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-147">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="16d3f-148">열거형 매핑의 check 제약 조건 생성</span><span class="sxs-lookup"><span data-stu-id="16d3f-148">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="16d3f-149">이제 EF Core 5.0 마이그레이션을 통해 열거형 속성 매핑의 CHECK 제약 조건을 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-149">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="16d3f-150">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="16d3f-150">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="16d3f-151">설명서는 문제 [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-151">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="16d3f-152">IsRelational</span><span class="sxs-lookup"><span data-stu-id="16d3f-152">IsRelational</span></span>

<span data-ttu-id="16d3f-153">기존의 `IsSqlServer`, `IsSqlite`, `IsInMemory` 외에 새로운 `IsRelational` 메서드가 추가되었습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-153">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span>
<span data-ttu-id="16d3f-154">DbContext가 관계형 데이터베이스 공급자를 사용 중인지 여부를 테스트하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-154">This can be used to test if the DbContext is using any relational database provider.</span></span>
<span data-ttu-id="16d3f-155">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="16d3f-155">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="16d3f-156">설명서는 문제 [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-156">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="16d3f-157">ETag를 사용한 Cosmos 낙관적 동시성</span><span class="sxs-lookup"><span data-stu-id="16d3f-157">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="16d3f-158">Azure Cosmos DB 데이터베이스 공급자는 이제 ETag를 사용하여 낙관적 동시성을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-158">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span>
<span data-ttu-id="16d3f-159">OnModelCreating에서 모델 작성기를 사용하여 ETag를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-159">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="16d3f-160">그런 다음 SaveChanges에서 동시성 충돌 시 `DbUpdateConcurrencyException`을 throw하며, 다시 시도를 구현하기 위해 충돌을 [처리할 수 있습니다](https://docs.microsoft.com/ef/core/saving/concurrency).</span><span class="sxs-lookup"><span data-stu-id="16d3f-160">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](https://docs.microsoft.com/ef/core/saving/concurrency) to implement retries, etc.</span></span>


<span data-ttu-id="16d3f-161">설명서는 문제 [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-161">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="16d3f-162">더 많은 날짜/시간 구문의 쿼리 변환</span><span class="sxs-lookup"><span data-stu-id="16d3f-162">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="16d3f-163">이제 새로운 DateTime 생성을 포함하는 쿼리가 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-163">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="16d3f-164">또한 이제 다음과 같은 SQL Server 함수가 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-164">In addition, the following SQL Server functions are now mapped:</span></span>
* <span data-ttu-id="16d3f-165">DateDiffWeek</span><span class="sxs-lookup"><span data-stu-id="16d3f-165">DateDiffWeek</span></span>
* <span data-ttu-id="16d3f-166">DateFromParts</span><span class="sxs-lookup"><span data-stu-id="16d3f-166">DateFromParts</span></span>

<span data-ttu-id="16d3f-167">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="16d3f-167">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="16d3f-168">설명서는 문제 [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-168">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="16d3f-169">더 많은 바이트 배열 구문의 쿼리 변환</span><span class="sxs-lookup"><span data-stu-id="16d3f-169">Query translations for more byte array constructs</span></span>

<span data-ttu-id="16d3f-170">Contains, Length, SequenceEqual 등을 사용하는 쿼리는 byte[] 속성에서 이제 SQL로 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-170">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="16d3f-171">예비 설명서는 [2019년 12월 5일의 EF 주간 상태](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)에 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-171">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="16d3f-172">추가 설명서는 문제 [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-172">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="16d3f-173">역방향 쿼리 변환</span><span class="sxs-lookup"><span data-stu-id="16d3f-173">Query translation for Reverse</span></span>

<span data-ttu-id="16d3f-174">이제 `Reverse`를 사용하는 쿼리가 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-174">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="16d3f-175">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="16d3f-175">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="16d3f-176">설명서는 문제 [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-176">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="16d3f-177">비트 연산자의 쿼리 변환</span><span class="sxs-lookup"><span data-stu-id="16d3f-177">Query translation for bitwise operators</span></span>

<span data-ttu-id="16d3f-178">비트 연산자를 사용하는 쿼리는 다음과 같이 더 다양한 경우에 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-178">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="16d3f-179">설명서는 문제 [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-179">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="16d3f-180">Cosmos 문자열의 쿼리 변환</span><span class="sxs-lookup"><span data-stu-id="16d3f-180">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="16d3f-181">이제 Azure Cosmos DB 공급자를 사용할 때 Contains, StartsWith 및 EndsWith 문자열 메서드를 사용하는 쿼리가 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-181">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="16d3f-182">설명서는 문제 [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16d3f-182">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
