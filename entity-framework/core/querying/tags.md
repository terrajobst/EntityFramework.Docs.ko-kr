---
title: 쿼리 태그 - EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 73C7A627-C8E9-452D-9CD5-AFCC8FEFE395
uid: core/querying/tags
ms.openlocfilehash: 3a4d516cab5836c659e42d825c4f1bf89355d671
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688756"
---
# <a name="query-tags"></a><span data-ttu-id="037ab-102">쿼리 태그</span><span class="sxs-lookup"><span data-stu-id="037ab-102">Query tags</span></span>
> [!NOTE]
> <span data-ttu-id="037ab-103">이 기능은 EF Core 2.2의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="037ab-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="037ab-104">이 기능은 로그에서 캡처한 생성된 SQL 쿼리와 코드의 LINQ 쿼리를 서로 연관 짓는 데 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="037ab-104">This feature helps correlate LINQ queries in code with generated SQL queries captured in logs.</span></span>
<span data-ttu-id="037ab-105">새 `TagWith()` 메서드를 사용하여 LINQ 쿼리에 주석을 답니다.</span><span class="sxs-lookup"><span data-stu-id="037ab-105">You annotate a LINQ query using the new `TagWith()` method:</span></span> 

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="037ab-106">이 LINQ 쿼리는 다음 SQL 문으로 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="037ab-106">This LINQ query is translated to the following SQL statement:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="037ab-107">동일한 쿼리에서 `TagWith()`를 여러 번 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="037ab-107">It's possible to call `TagWith()` many times on the same query.</span></span>
<span data-ttu-id="037ab-108">쿼리 태그는 누적됩니다.</span><span class="sxs-lookup"><span data-stu-id="037ab-108">Query tags are cumulative.</span></span>
<span data-ttu-id="037ab-109">예를 들어, 다음과 같은 메서드를 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="037ab-109">For example, given the following methods:</span></span>

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

<span data-ttu-id="037ab-110">다음 쿼리:</span><span class="sxs-lookup"><span data-stu-id="037ab-110">The following query:</span></span>   

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

<span data-ttu-id="037ab-111">다음으로 변환:</span><span class="sxs-lookup"><span data-stu-id="037ab-111">Translates to:</span></span>

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="037ab-112">다중 선 문자열을 쿼리 태그로 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="037ab-112">It's also possible to use multi-line strings as query tags.</span></span>
<span data-ttu-id="037ab-113">예:</span><span class="sxs-lookup"><span data-stu-id="037ab-113">For example:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

<span data-ttu-id="037ab-114">다음 SQL을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="037ab-114">Produces the following SQL:</span></span>

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a><span data-ttu-id="037ab-115">알려진 제한 사항</span><span class="sxs-lookup"><span data-stu-id="037ab-115">Known limitations</span></span>
<span data-ttu-id="037ab-116">**쿼리 태그는 매개 변수화할 수 없음:** EF Core는 항상 LINQ 쿼리의 쿼리 태그를 생성된 SQL에 포함되는 문자열 리터럴로 처리합니다.</span><span class="sxs-lookup"><span data-stu-id="037ab-116">**Query tags aren't parameterizable:** EF Core always treats query tags in the LINQ query as string literals that are included in the generated SQL.</span></span>
<span data-ttu-id="037ab-117">쿼리 태그를 매개 변수로 사용하는 컴파일된 쿼리는 허용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="037ab-117">Compiled queries that take query tags as parameters aren't allowed.</span></span>