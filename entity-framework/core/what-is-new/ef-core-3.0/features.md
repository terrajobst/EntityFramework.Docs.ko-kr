---
title: EF Core 3.0의 새로운 기능 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 528733d6eec33de2c9538541a6ed5be704b9d433
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005563"
---
# <a name="new-features-included-in-ef-core-30"></a>EF Core 3.0에 포함된 새로운 기능

다음 목록에는 EF Core 3.0용으로 계획된 주요한 새 기능이 포함되어 있습니다.

EF Core 3.0은 주요 릴리스이며, 기존 애플리케이션에 부정적인 영향을 줄 수 있는 API 개선에 해당하는 [주요 변경 사항](xref:core/what-is-new/ef-core-3.0/breaking-changes)이 다수 포함되어 있습니다.  

## <a name="linq-improvements"></a>LINQ 기능 향상 

LINQ를 사용하면 선택한 언어로 데이터베이스 쿼리를 작성할 수 있어서 다양한 종류의 정보를 활용하여 IntelliSense 및 컴파일 시간 형식 검사를 수행할 수 있습니다.
그러나 LINQ를 사용하면 임의의 식(메서드 호출 또는 작업)을 포함하는 복잡한 쿼리를 무제한으로 작성할 수도 있습니다.
이러한 모든 조합을 처리하는 것은 LINQ 공급자에게 항상 중요한 도전 과제입니다.
EF Core 3.0에서는 더 많은 식을 SQL로 변환하고, 더 많은 사례에서 효율적인 쿼리를 생성하며, 비효율적인 쿼리가 검색되지 않도록 하고, 기존 애플리케이션 및 데이터 공급자에 대해 호환성이 손상되는 변경 없이 새 쿼리 기능 및 성능 개선을 점차 더 쉽게 도입할 수 있도록 LINQ 구현을 다시 작성했습니다.

### <a name="client-evaluation"></a>클라이언트 평가

EF Core 3.0의 주요 디자인 변경은 SQL 또는 매개 변수로 변환할 수 없는 LINQ 식을 처리하는 방식과 관련이 있습니다.

처음 몇 가지 버전에서 EF Core는 SQL로 변환할 수 있는 쿼리 부분을 파악하여 클라이언트에서 나머지 쿼리를 실행했습니다.
이러한 클라이언트 쪽 실행은 경우에 따라 바람직할 수 있지만 많은 경우 비효율적인 쿼리가 발생할 수 있습니다.
예를 들어 EF Core 2.2가 `Where()` 호출에서 조건자를 변환할 수 없는 경우에는 필터가 없는 SQL 문을 실행하여 데이터베이스에서 모든 행을 읽은 다음 메모리 내에서 필터링합니다.
이는 데이터베이스에 적은 수의 행이 포함된 경우에는 허용할 수 있지만 데이터베이스에 많은 수의 행이 포함된 경우에는 애플리케이션 오류 또는 성능 문제가 발생할 수 있습니다.
EF Core 3.0에서는 클라이언트 평가가 최상위 프로젝션(`Select()`에 대한 마지막 호출)에서만 수행되도록 제한했습니다.
EF Core 3.0이 쿼리의 다른 위치에서 변환할 수 없는 식을 감지하면 런타임 예외가 throw됩니다.

## <a name="cosmos-db-support"></a>Cosmos DB 지원 

EF Core의 Cosmos DB 공급자는 EF 프로그래밍 모델에 친숙한 개발자가 애플리케이션 데이터베이스로서 Azure Cosmos DB를 쉽게 대상으로 지정할 수 있도록 합니다.
목표는 글로벌 배포, "always on" 가용성, 탄력적인 확장성, 짧은 대기 시간, .NET 개발자에 훨씬 더 많은 액세스 가능성과 같은 몇몇 Cosmos DB의 장점을 활용하는 것입니다.
공급자는 Cosmos DB의 SQL API에 대해 자동 변경 내용 추적, LINQ, 값 변환 같은 대부분의 EF Core 기능을 사용할 수 있습니다.

## <a name="c-80-support"></a>C# 8.0 지원

EF Core 3.0은 C# 8.0의 몇 가지 새로운 기능을 활용합니다.

### <a name="asynchronous-streams"></a>비동기 스트림

이제 비동기 쿼리 결과는 새 표준 `IAsyncEnumerable<T>` 인터페이스를 사용하여 노출되며 `await foreach`를 사용하여 이용할 수 있습니다.

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Proccess(o);
} 
```

### <a name="nullable-reference-types"></a>nullable 참조 형식 

EF Core 코드에서 이 새로운 기능을 사용하는 경우 EF Core는 참조 형식(문자열 또는 탐색 속성과 같은 기본 형식)의 속성에 대한 null 허용 여부를 판단하여 데이터베이스에서 열과 관계의 null 허용 여부를 결정할 수 있습니다.

## <a name="interception"></a>Interception

EF Core 3.0의 새로운 인터셉션 API를 사용하면 연결 열기, 트랜잭션 초기화 및 명령 실행과 같은 일반적인 EF Core 작업의 일부로 발생하는 하위 수준 데이터베이스 작업의 결과를 프로그래밍 방식으로 관찰하고 수정할 수 있습니다. 

## <a name="reverse-engineering-of-database-views"></a>데이터베이스 뷰의 리버스 엔지니어링

키가 없는 엔터티 형식(이전에는 [쿼리 형식](xref:core/modeling/query-types)으로 알려짐)은 데이터베이스에서 읽을 수는 있지만 업데이트할 수 없는 데이터를 나타냅니다.
이 특성은 대부분의 시나리오에서 데이터베이스 뷰 매핑에 매우 적합하므로 데이터베이스 뷰를 리버스 엔지니어링할 때 키 없이 엔터티 형식을 생성할 수 있도록 자동화했습니다.

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>보안 주체와 테이블을 공유하는 종속 엔터티는 이제 선택 사항입니다.

EF Core 3.0부터 `OrderDetails`가 `Order`에 소유되거나 같은 테이블에 명시적으로 매핑된 경우 `OrderDetails` 없이 `Order`를 추가할 수 있으며 기본 키를 제외하고 모든 `OrderDetails` 속성이 Null 허용 열에 매핑됩니다.

쿼리 시 EF Core는 해당 필수 속성에 값이 없거나 기본 키 이외의 필수 속성이 없고 모든 속성이 `null`이면 `OrderDetails`를 `null`로 설정합니다.

``` csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

## <a name="ef-63-on-net-core"></a>.NET Core의 EF 6.3

많은 기존 애플리케이션이 이전 버전의 EF를 사용하고 단지 .NET Core를 활용하기 위해 이전 버전의 EF를 EF Core로 포팅하는 일은 상당한 노력이 필요할 수 있다는 점을 이해합니다.
그러한 이유로 Microsoft는 .NET Core 3.0에서 최신 버전의 EF 6을 실행할 수 있도록 했습니다.
다음과 같은 몇 가지 제한 사항이 있습니다.
- 새 공급자는 .NET Core에서 작업을 수행해야 합니다.
- SQL Server를 사용한 공간 지원은 사용할 수 없습니다.

## <a name="postponed-features"></a>연기된 기능

원래 EF Core 3.0에 대해 계획되었던 일부 기능은 이후 릴리스로 연기되었습니다. 

- 마이그레이션에서 모델의 일부를 무시하는 기능([#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725)로 추적).
- 두 가지 문제로 추적되는 속성 모음 엔터티(공유 형식 엔터티에 대한 [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) 및 인덱싱된 속성 매핑 지원에 대한 [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610)).
