---
title: EF Core 5.0의 새로운 기능
author: ajcvickers
ms.date: 01/29/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 65d7bd43e8a00c77fd6091a74c677635710d03e3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413848"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="5b20d-102">EF Core 5.0의 새로운 기능</span><span class="sxs-lookup"><span data-stu-id="5b20d-102">What's New in EF Core 5.0</span></span>

<span data-ttu-id="5b20d-103">현재 EF Core 5.0을 개발하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-103">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="5b20d-104">이 페이지에는 각 미리 보기에서 소개된 중요한 변경 내용의 개요가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-104">This page will contain an overview of interesting changes introduced in each preview.</span></span>
<span data-ttu-id="5b20d-105">EF Core 5.0의 첫 번째 미리 보기는 2020년 1분기에 공개될 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-105">The first preview of EF Core 5.0 is tentatively expected in in the first quarter of 2020.</span></span>

<span data-ttu-id="5b20d-106">이 페이지는 [EF Core 5.0의 플랜](plan.md)과 중복되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-106">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="5b20d-107">이 플랜에서는 최종 릴리스를 출시하기 전에 포함할 예정인 모든 항목을 비롯하여 EF Core 5.0의 전체 테마를 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="5b20d-108">공식 설명서가 게시되면 여기의 링크가 설명서에 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-1-not-yet-shipped"></a><span data-ttu-id="5b20d-109">미리 보기 1(아직 출시되지 않음)</span><span class="sxs-lookup"><span data-stu-id="5b20d-109">Preview 1 (Not yet shipped)</span></span>

### <a name="simple-logging"></a><span data-ttu-id="5b20d-110">간단한 로깅</span><span class="sxs-lookup"><span data-stu-id="5b20d-110">Simple logging</span></span>

<span data-ttu-id="5b20d-111">이 기능은 EF6의 `Database.Log`와 유사한 기능을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-111">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="5b20d-112">즉, 어떠한 외부 로깅 프레임워크도 구성할 필요 없이 EF Core에서 로그를 가져오는 간단한 방법을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-112">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="5b20d-113">예비 설명서는 [2019년 12월 5일의 EF 주간 상태](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)에 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-113">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="5b20d-114">추가 설명서는 문제 [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-114">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="5b20d-115">SQL을 생성하는 간단한 방법</span><span class="sxs-lookup"><span data-stu-id="5b20d-115">Simple way to get generated SQL</span></span>

<span data-ttu-id="5b20d-116">EF Core 5.0에는 EF Core가 LINQ 쿼리를 실행할 때 생성하는 SQL을 반환하는 `ToQueryString` 확장 메서드가 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-116">EF Core 5.0 introduces the `ToQueryString` extension method which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="5b20d-117">예비 설명서는 [2020년 1월 9일의 EF 주간 상태](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246)에 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-117">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="5b20d-118">추가 설명서는 문제 [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-118">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="5b20d-119">향상된 디버그 보기</span><span class="sxs-lookup"><span data-stu-id="5b20d-119">Enhanced debug views</span></span>

<span data-ttu-id="5b20d-120">디버그 보기를 통해 문제를 디버깅할 때 EF Core의 내부를 쉽게 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-120">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="5b20d-121">모델의 디버그 보기가 며칠 전에 구현되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-121">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="5b20d-122">EF Core 5.0에서는 모델 보기를 간편하게 읽을 수 있으며, 상태 관리자에서 추적된 엔터티를 확인할 수 있는 새 디버그 보기가 추가되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-122">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="5b20d-123">예비 설명서는 [2019년 12월 12일의 EF 주간 상태](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206)에 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-123">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="5b20d-124">추가 설명서는 문제 [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-124">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="5b20d-125">초기화된 DbContext에서 연결 또는 연결 문자열을 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-125">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="5b20d-126">이제 연결 문자열 또는 연결 문자열 없이 DbContext 인스턴스를 보다 쉽게 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-126">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="5b20d-127">또한 이제 컨텍스트 인스턴스에서 연결 또는 연결 문자열을 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-127">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="5b20d-128">이에 따라 동일한 컨텍스트 인스턴스를 다른 데이터베이스에 동적으로 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-128">This allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="5b20d-129">설명서는 문제 [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-129">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="5b20d-130">변경 내용 추적 프록시</span><span class="sxs-lookup"><span data-stu-id="5b20d-130">Change-tracking proxies</span></span>

<span data-ttu-id="5b20d-131">이제 EF Core에서 [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) 및 [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1)를 자동으로 구현하는 런타임 프록시를 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-131">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="5b20d-132">이후 엔터티 속성의 값 변경 내용을 EF Core에 직접 보고하므로 변경 내용을 별도로 검사하지 않아도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-132">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="5b20d-133">그러나 프록시에는 자체적으로 제한 사항이 적용되어 있어 일부 사용자에게는 적합하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-133">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="5b20d-134">설명서는 문제 [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-134">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="5b20d-135">향상된 데이터베이스 null 의미 체계 처리</span><span class="sxs-lookup"><span data-stu-id="5b20d-135">Improved handling of database null semantics</span></span>

<span data-ttu-id="5b20d-136">관계형 데이터베이스는 일반적으로 NULL을 알 수 없는 값으로 처리하므로 다른 NULL과 같지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-136">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="5b20d-137">반면 C#은 null을 다른 null과 동일하게 비교하는 정의된 값으로 처리합니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-137">C#, on the other hand, treats null as a defined value which compares equal to any other null.</span></span>
<span data-ttu-id="5b20d-138">기본적으로 EF Core는 쿼리를 변환하여 C# null 의미 체계를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-138">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="5b20d-139">EF Core 5.0은 이 변환 과정의 효율성을 크게 향상시킵니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-139">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="5b20d-140">설명서는 문제 [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-140">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="5b20d-141">인덱서 속성</span><span class="sxs-lookup"><span data-stu-id="5b20d-141">Indexer properties</span></span>

<span data-ttu-id="5b20d-142">EF Core 5.0는 C# 인덱서 속성의 매핑을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-142">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="5b20d-143">이에 따라 엔터티를 속성 모음으로 사용할 수 있으며 속성 모음의 명명된 속성으로 열이 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-143">This allows entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="5b20d-144">설명서는 문제 [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-144">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="5b20d-145">열거형 매핑의 check 제약 조건 생성</span><span class="sxs-lookup"><span data-stu-id="5b20d-145">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="5b20d-146">이제 EF Core 5.0 마이그레이션을 통해 열거형 속성 매핑의 CHECK 제약 조건을 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-146">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="5b20d-147">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="5b20d-147">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="5b20d-148">설명서는 문제 [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-148">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="5b20d-149">더 많은 날짜/시간 구문의 쿼리 변환</span><span class="sxs-lookup"><span data-stu-id="5b20d-149">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="5b20d-150">이제 새로운 DataTime 생성을 포함하는 쿼리가 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-150">Queries containing new DataTime construction are now translated.</span></span>
<span data-ttu-id="5b20d-151">또한 이제 SQL Server 함수 DateDiffWeek가 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-151">Also, the SQL Server function DateDiffWeek is now mapped.</span></span>

<span data-ttu-id="5b20d-152">설명서는 문제 [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-152">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="5b20d-153">더 많은 바이트 배열 구문의 쿼리 변환</span><span class="sxs-lookup"><span data-stu-id="5b20d-153">Query translations for more byte array constructs</span></span>

<span data-ttu-id="5b20d-154">Contains, Length, SequenceEqual 등을 사용하는 쿼리는 byte[] 속성에서 이제 SQL로 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-154">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="5b20d-155">예비 설명서는 [2019년 12월 5일의 EF 주간 상태](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)에 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-155">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="5b20d-156">추가 설명서는 문제 [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-156">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="5b20d-157">역방향 쿼리 변환</span><span class="sxs-lookup"><span data-stu-id="5b20d-157">Query translation for Reverse</span></span>

<span data-ttu-id="5b20d-158">이제 `Reverse`를 사용하는 쿼리가 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-158">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="5b20d-159">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="5b20d-159">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="5b20d-160">설명서는 문제 [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-160">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="5b20d-161">비트 연산자의 쿼리 변환</span><span class="sxs-lookup"><span data-stu-id="5b20d-161">Query translation for bitwise operators</span></span>

<span data-ttu-id="5b20d-162">비트 연산자를 사용하는 쿼리는 다음과 같이 더 다양한 경우에 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-162">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="5b20d-163">설명서는 문제 [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-163">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="5b20d-164">Cosmos 문자열의 쿼리 변환</span><span class="sxs-lookup"><span data-stu-id="5b20d-164">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="5b20d-165">이제 Azure Cosmos DB 공급자를 사용할 때 Contains, StartsWith 및 EndsWith 문자열 메서드를 사용하는 쿼리가 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-165">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="5b20d-166">설명서는 문제 [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5b20d-166">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
