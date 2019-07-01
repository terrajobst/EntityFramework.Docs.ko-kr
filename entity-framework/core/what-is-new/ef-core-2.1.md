---
title: EF Core 2.1의 새로운 기능 - EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: 16600ccbb1194d584fae15671118d9c046f1f637
ms.sourcegitcommit: 06073f8efde97dd5f540dbfb69f574d8380566fe
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67333853"
---
# <a name="new-features-in-ef-core-21"></a>EF Core 2.1의 새로운 기능

수많은 버그 수정 및 작은 기능 및 성능 개선 사항 외에도 EF Core 2.1에는 몇 가지 놀라운 기능이 있습니다.

## <a name="lazy-loading"></a>지연 로드
이제 EF Core에는 요청 시 해당 탐색 속성을 로드할 수 있는 엔터티 클래스를 작성하는 사용자에게 필요한 구성 요소가 포함됩니다. 또한 Microsoft.EntityFrameworkCore.Proxies라는 새 패키지를 만들었습니다. 여기서는 해당 구성 요소를 활용하여 최소한으로 수정된 엔터티 클래스(예: 가상 탐색 속성이 포함된 클래스)를 기반으로 지연된 로드 프록시 클래스를 생성합니다.

이 항목에 대한 자세한 내용은 [지연된 로드에 대한 섹션](xref:core/querying/related-data#lazy-loading)을 참고하세요.

## <a name="parameters-in-entity-constructors"></a>엔터티 생성자의 매개 변수
지연된 로드의 필수 구성 요소 중 하나로 해당 생성자에서 매개 변수를 사용하는 엔터티를 생성하도록 설정했습니다. 매개 변수를 사용하여 속성 값, 지연된 로드 대리자 및 서비스를 삽입할 수 있습니다.

이 항목에 대한 자세한 내용은 [매개 변수가 포함된 엔터티 생성자에 대한 섹션](xref:core/modeling/constructors)을 참고하세요.

## <a name="value-conversions"></a>값 변환
지금까지 EF Core는 기본 데이터베이스 공급자가 고유하게 지원하는 형식의 속성만을 매핑할 수 있습니다. 값은 변환 없이 열과 속성 간에 복사되었습니다. EF Core 2.1부터 값 변환은 속성에 적용되기 전에 열에서 가져온 값을 변환하는 데 적용하거나 반대로 수행할 수 있습니다. 열과 속성 간의 사용자 지정 변환을 등록하도록 허용하는 명시적 구성 API뿐만 아니라 필요에 따라 다양한 규칙에서 적용할 수 있는 변환이 있습니다. 이 기능의 애플리케이션은 다음과 같습니다.

- 문자열로 열거형 저장
- SQL Server를 사용하여 부호 없는 정수 매핑
- 속성 값의 자동 암호화 및 암호 해독

이 항목에 대한 자세한 내용은 [값 변환에 대한 섹션](xref:core/modeling/value-conversions)을 참고하세요.  

## <a name="linq-groupby-translation"></a>LINQ GroupBy 변환
EF Core 버전 2.1 이전에 메모리에서 GroupBy LINQ 연산자를 평가했습니다. 이제 대부분의 일반적인 경우에 SQL GROUP BY 절로 변환하도록 지원됩니다.

이 예제에서는 다양한 집계 함수를 계산하는 데 사용되는 GroupBy를 포함하는 쿼리를 보여줍니다.

``` csharp
var query = context.Orders
    .GroupBy(o => new { o.CustomerId, o.EmployeeId })
    .Select(g => new
        {
          g.Key.CustomerId,
          g.Key.EmployeeId,
          Sum = g.Sum(o => o.Amount),
          Min = g.Min(o => o.Amount),
          Max = g.Max(o => o.Amount),
          Avg = g.Average(o => o.Amount)
        });
```

해당 SQL 변환은 다음과 같습니다.

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a>데이터 시드
새 릴리스에서 초기 데이터를 제공하여 데이터베이스를 채울 수 있습니다. EF6과 달리 데이터 시드는 모델 구성의 일부로 엔터티 형식에 연결됩니다. 그런 다음, EF Core 마이그레이션은 데이터베이스를 모델의 새 버전으로 업그레이드하는 경우 삽입, 업데이트 또는 삭제 작업을 적용해야 하는 경우를 자동으로 컴퓨팅할 수 있습니다.

예를 들어 이를 사용하여 `OnModelCreating`에서 게시물의 시드 데이터를 구성할 수 있습니다.

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

이 항목에 대한 자세한 내용은 [데이터 시드에 대한 섹션](xref:core/modeling/data-seeding)을 참고하세요.  

## <a name="query-types"></a>쿼리 형식
이제 EF Core 모델에는 쿼리 형식이 포함될 수 있습니다. 엔터티 형식과 달리 쿼리 형식에는 정의된 키가 없고 삽입, 삭제 또는 업데이트될 수 없습니다(즉, 읽기 전용임). 하지만 쿼리에 의해 직접 반환될 수 있습니다. 쿼리 형식의 사용 시나리오는 다음과 같습니다.

- 기본 키가 없이 보기에 매핑
- 기본 키가 없이 테이블에 매핑
- 모델에서 정의된 쿼리에 매핑
- `FromSql()` 쿼리에 대한 반환 형식으로 제공

이 항목에 대한 자세한 내용은 [쿼리 형식에 대한 섹션](xref:core/modeling/query-types)을 참고하세요.

## <a name="include-for-derived-types"></a>파생된 형식에 포함
이제 `Include` 메서드를 작성할 때 파생된 형식에서 정의된 탐색 속성만을 지정할 수 있습니다. 강력한 형식의 `Include` 버전에서 명시적 캐스트 또는 `as` 연산자를 사용하도록 지원합니다. 또한 이제 문자열 `Include` 버전의 파생된 형식에서 정의된 탐색 속성의 이름을 참조하도록 지원합니다.

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

이 항목에 대한 자세한 내용은 [파생된 형식에서 포함된 항목에 대한 섹션](xref:core/querying/related-data#include-on-derived-types)을 참고하세요.

## <a name="systemtransactions-support"></a>System.Transactions 지원
TransactionScope 등의 System.Transactions 기능에서 작동하는 기능을 추가했습니다. 이 기능은 지원하는 데이터베이스 공급자를 사용하는 경우 .NET Core와.NET Framework에서 작동합니다.

이 항목에 대한 자세한 내용은 [System.Transactions에 대한 섹션](xref:core/saving/transactions#using-systemtransactions)을 참고하세요.

## <a name="better-column-ordering-in-initial-migration"></a>초기 마이그레이션에서 열 순서 개선
고객의 의견에 따라 처음부터 클래스에 선언된 속성과 동일한 순서대로 테이블의 열을 생성하도록 마이그레이션을 업데이트했습니다. 초기 테이블 작성 후에 새 멤버를 추가할 경우 EF Core는 순서를 변경할 수 없습니다.

## <a name="optimization-of-correlated-subqueries"></a>상관 관계 있는 하위 쿼리의 최적화
여러 가지 일반적인 시나리오에서 "N + 1" SQL 쿼리를 실행하지 않도록 이 쿼리 변환을 개선했습니다. 여기서는 프로젝션에서 탐색 속성의 사용량에 따라 상관 관계 있는 하위 쿼리의 데이터를 사용하여 루트 쿼리의 데이터를 조인하게 됩니다. 최적화는 하위 쿼리에서 결과의 버퍼링이 필요하고 쿼리를 수정하여 새로운 동작을 옵트인해야 합니다.

예를 들어 다음 쿼리는 정상적으로 고객에 대한 쿼리로 변환됩니다. 또한 N("N"은 반환된 고객의 수)은 순서에 대한 쿼리를 구분합니다.

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

적절한 위치에 `ToList()`를 포함하여 버퍼링이 순서에 적합함을 나타냅니다. 그러면 최적화를 사용할 수 있습니다.

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

이 쿼리는 두 개의 SQL 쿼리로 변환됩니다. 하나는 고객, 다른 하나는 주문에 대한 쿼리입니다.

## <a name="owned-attribute"></a>[Owned] 특성

이제 형식에 `[Owned]`라는 주석을 달고 소유자 엔터티를 모델에 추가하여 [소유한 엔터티 형식](xref:core/modeling/owned-entities)을 구성할 수 있습니다.

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a>.NET Core SDK에 포함된 명령줄 도구 dotnet-ef

_dotnet-ef_ 명령은 이제 .NET Core SDK의 일부이므로, 마이그레이션을 사용하거나 기존 데이터베이스에서 DbContext를 스캐폴딩하기 위해 더 이상 프로젝트에서 DotNetCliToolReference를 사용할 필요가 없습니다.

다른 버전의 .NET Core SDK 및 EF Core에 명령줄 도구를 활성화하는 방법에 대한 자세한 내용은 [도구 설치](xref:core/miscellaneous/cli/dotnet#installing-the-tools)의 섹션을 참조하세요.

## <a name="microsoftentityframeworkcoreabstractions-package"></a>Microsoft.EntityFrameworkCore.Abstractions 패키지
새 패키지에는 전체적으로 EF Core에 의존하지 않고 EF Core 기능을 강화하기 위해 프로젝트에서 사용할 수 있는 특성 및 인터페이스가 포함되어 있습니다. 예를 들어 [Owned] 특성 및 ILazyLoader 인터페이스를 여기에서 확인할 수 있습니다.

## <a name="state-change-events"></a>상태 변경 이벤트

`ChangeTracker`의 새 `Tracked` 및 `StateChanged` 이벤트를 사용하여 DbContext를 입력하거나 상태를 변경하는 엔터티에 대응하는 논리를 작성할 수 있습니다.

## <a name="raw-sql-parameter-analyzer"></a>원시 SQL 매개 변수 분석기

`FromSql` 또는 `ExecuteSqlCommand`와 같이 원시 SQL API의 잠재적으로 안전하지 않은 사용을 발견하는 새 코드 분석기가 EF Core와 함께 포함됩니다. 예를 들어 다음 쿼리의 경우 _minAge_에 매개 변수가 없기 때문에 경고가 표시됩니다.

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a>데이터베이스 공급자 호환성

EF Core 2.1과 작동하도록 업데이트되거나 테스트된 공급자로 EF Core 2.1을 사용하는 것이 좋습니다.

> [!TIP]
> 예상치 않은 호환성 또는 새로운 기능의 문제가 발생하거나 이에 대한 의견이 있는 경우 [문제 추적기](https://github.com/aspnet/EntityFrameworkCore/issues/new)를 사용하여 보고해주세요.
