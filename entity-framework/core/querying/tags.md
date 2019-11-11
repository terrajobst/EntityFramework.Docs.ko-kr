---
title: 쿼리 태그 - EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 73C7A627-C8E9-452D-9CD5-AFCC8FEFE395
uid: core/querying/tags
ms.openlocfilehash: e8415b237df45ce652dcd152013f4f12a992aed7
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654825"
---
# <a name="query-tags"></a>쿼리 태그

> [!NOTE]
> 이 기능은 EF Core 2.2의 새로운 기능입니다.

이 기능은 로그에서 캡처한 생성된 SQL 쿼리와 코드의 LINQ 쿼리를 서로 연관 짓는 데 도움이 됩니다.
새 `TagWith()` 메서드를 사용하여 LINQ 쿼리에 주석을 답니다.

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

이 LINQ 쿼리는 다음 SQL 문으로 변환됩니다.

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

동일한 쿼리에서 `TagWith()`를 여러 번 호출할 수 있습니다.
쿼리 태그는 누적됩니다.
예를 들어, 다음과 같은 메서드를 가정합니다.

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

다음 쿼리:

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

다음으로 변환:

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

다중 선 문자열을 쿼리 태그로 사용할 수도 있습니다.
예:

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

다음 SQL을 생성합니다.

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a>알려진 제한 사항

**쿼리 태그는 매개 변수화할 수 없음:** EF Core는 항상 LINQ 쿼리의 쿼리 태그를 생성된 SQL에 포함되는 문자열 리터럴로 처리합니다.
쿼리 태그를 매개 변수로 사용하는 컴파일된 쿼리는 허용되지 않습니다.
