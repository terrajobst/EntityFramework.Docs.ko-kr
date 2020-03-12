---
title: EF Core 2.2의 새로운 기능 - EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: fb9de799753bebd7b4092cd8f4af74703dee3e45
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413554"
---
# <a name="new-features-in-ef-core-22"></a>EF Core 2.2의 새로운 기능

## <a name="spatial-data-support"></a>공간 데이터 지원

개체의 물리적 위치 및 모양을 나타내는 데 공간 데이터를 사용할 수 있습니다.
대부분의 데이터베이스는 기본적으로 공간 데이터를 저장하고 인덱싱하고 쿼리할 수 있습니다.
일반적인 시나리오로는 지정된 거리 내의 개체에 대한 쿼리, 다각형에 지정된 위치가 포함되는지 테스트 등이 있습니다.
EF Core 2.2는 이제 [NTS](https://github.com/NetTopologySuite/NetTopologySuite)(NetTopologySuite) 라이브러리의 형식을 사용하는 다양한 데이터베이스의 공간 데이터 작업을 지원합니다.

공간 데이터 지원은 일련의 공급자별 확장 패키지로 구현됩니다.
이러한 각각의 패키지는 NTS 형식과 메서드에 대한 매핑, 데이터베이스의 해당 공간 형식 및 함수를 제공합니다.
그러한 공급자 확장은 [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/)([Npgsql 프로젝트](https://www.npgsql.org/))에 대해 현재 사용 가능합니다.
공간 형식은 추가 확장 없이 [EF Core 메모리 내 공급자](xref:core/providers/in-memory/index)에서 바로 사용할 수 있습니다.

공급자 확장을 설치하면 지원되는 형식의 속성을 엔터티에 추가할 수 있습니다. 예를 들어:

``` csharp
using NetTopologySuite.Geometries;

namespace MyApp
{
  public class Friend
  {
    [Key]
    public string Name { get; set; }
  
    [Required]
    public Point Location { get; set; }
  }
}
```

그런 다음, 공간 데이터와 함께 엔터티를 지속할 수 있습니다.

``` csharp
using (var context = new MyDbContext())
{
    context.Add(
        new Friend
        {
            Name = "Bill",
            Location = new Point(-122.34877, 47.6233355) {SRID = 4326 }
        });
    context.SaveChanges();
}
```

그리고 공간 데이터 및 작업을 기반으로 데이터베이스 쿼리를 실행할 수 있습니다.

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

이 기능에 대한 자세한 내용은 [공간 형식 설명서](xref:core/modeling/spatial)를 참조하세요.

## <a name="collections-of-owned-entities"></a>소유 엔터티 컬렉션

EF Core 2.0은 일대일 연결에서 소유권을 모델링하는 기능을 추가했습니다.
EF Core 2.2는 일대다 연결에 소유권을 표시하는 기능을 확장합니다.
소유권은 엔터티가 사용되는 방식을 제한하는 데 도움이 됩니다.

예를 들어, 소유 엔터티는 다음과 같습니다.

- 다른 엔터티 형식의 탐색 속성에만 표시될 수 있습니다.
- 자동으로 로드되며, 소유자와 함께 DbContext에 의해서만 추적될 수 있습니다.

관계형 데이터베이스에서 소유 컬렉션은 일반적인 일대다 연결처럼 소유자의 별도의 테이블에 매핑됩니다.
그러나 문서 지향적 데이터베이스에서는 소유 컬렉션 또는 참조의 소유 엔터티를 소유자와 동일한 문서 내에 중첩할 계획입니다.

새 OwnsMany() API를 호출하여 이 기능을 사용할 수 있습니다.

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

자세한 내용은 [업데이트된 소유 엔터티 설명서](xref:core/modeling/owned-entities#collections-of-owned-types)를 참조하세요.

## <a name="query-tags"></a>쿼리 태그

이 기능은 로그에서 캡처한 생성된 SQL 쿼리와 코드의 LINQ 쿼리의 상관 관계를 간소화합니다.

쿼리 태그를 사용하려면 새 TagWith() 메서드를 사용하여 LINQ 쿼리에 주석을 답니다.
이전 예에서 공간 쿼리 사용:

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

이 LINQ 쿼리는 다음 SQL 출력을 생성합니다.

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

자세한 내용은 [쿼리 태그 설명서](xref:core/querying/tags)를 참조하세요.
