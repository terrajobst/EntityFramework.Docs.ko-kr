---
title: Entity Framework Core 3.0의 새로운 기능
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/index
ms.openlocfilehash: ebebbf286d9d8e06492a3c664799e1127c7dc8c0
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197914"
---
# <a name="new-features-in-entity-framework-core-30"></a>Entity Framework Core 3.0의 새로운 기능

다음 목록에는 EF Core 3.0의 주요한 새 기능이 포함되어 있습니다.

EF Core 3.0은 주요 릴리스이며, 기존 애플리케이션에 부정적인 영향을 줄 수 있는 API 개선에 해당하는 [주요 변경 사항](xref:core/what-is-new/ef-core-3.0/breaking-changes)이 다수 포함되어 있습니다.  

## <a name="linq-overhaul"></a>LINQ 정비 

LINQ를 사용하면 선택한 .NET 언어로 데이터베이스 쿼리를 작성할 수 있어서 다양한 종류의 정보를 활용하여 IntelliSense 및 컴파일 시간 형식 검사를 수행할 수 있습니다.
그러나 LINQ를 사용하면 임의의 식(메서드 호출 또는 작업)을 포함하는 복잡한 쿼리를 무제한으로 작성할 수도 있습니다.
이러한 모든 조합을 처리하는 방법은 LINQ 공급자의 주요 과제입니다.

EF Core 3.0에서는 더 많은 쿼리 패턴을 SQL로 변환하고, 더 많은 사례에서 효율적인 쿼리를 생성하고, 비효율적인 쿼리가 검색되지 않도록 하는 데 사용할 수 있도록 LINQ 공급자를 다시 설계했습니다. 새 LINQ 공급자는 기존 애플리케이션 및 데이터 공급자를 중단시키지 않고 향후 릴리스에서 새로운 쿼리 기능 및 성능 향상을 제공할 수 있는 기반이 됩니다.

### <a name="restricted-client-evaluation"></a>클라이언트 평가 제한
가장 중요한 설계 변경은 매개 변수로 변환하거나 SQL로 변환할 수 없는 LINQ 식을 처리하는 방법과 관련이 있습니다.

이전 버전에서 EF Core는 SQL로 변환할 수 있는 쿼리 부분을 파악하여 클라이언트에서 나머지 쿼리를 실행했습니다.
이러한 클라이언트 쪽 실행은 경우에 따라 바람직할 수 있지만 많은 경우 비효율적인 쿼리가 발생합니다.

예를 들어 EF Core 2.2가 `Where()` 호출에서 조건자를 변환할 수 없는 경우에는 필터가 없는 SQL 문을 실행하여 데이터베이스에서 모든 행을 전송한 다음 메모리 내에서 필터링합니다.

``` csharp
var specialCustomers = 
  context.Customers
    .Where(c => c.Name.StartsWith(n) && IsSpecialCustomer(c));
```

이는 데이터베이스에 적은 수의 행이 포함된 경우에는 허용할 수 있지만 데이터베이스에 많은 수의 행이 포함된 경우에는 애플리케이션 오류 또는 성능 문제가 발생할 수 있습니다.

EF Core 3.0에서는 클라이언트 평가가 최상위 프로젝션(`Select()`에 대한 마지막 호출)에서만 수행되도록 제한했습니다.
EF Core 3.0이 쿼리의 다른 위치에서 변환할 수 없는 식을 감지하면 런타임 예외가 throw됩니다.

이전 예제와 같이 클라이언트에서 조건자 조건을 평가하려면 개발자는 이제 쿼리 평가를 LINQ to Objects로 명시적으로 전환해야 합니다. 

``` csharp
var specialCustomers =
  context.Customers
    .Where(c => c.Name.StartsWith(n)) 
    .AsEnumerable() // switches to LINQ to Objects
    .Where(c => IsSpecialCustomer(c));
```

기존 애플리케이션에 영향을 줄 수 있는 방법에 대한 자세한 내용은[ 주요 변경 사항 설명서](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client)를 참조하세요.

### <a name="single-sql-statement-per-linq-query"></a>LINQ 쿼리당 단일 SQL 문

3\.0에서 변경된 설계의 또 다른 중요한 점은 이제 항상 LINQ 쿼리당 단일 SQL 문을 생성한다는 것입니다. 이전 버전에서는 컬렉션 탐색 속성에 대한 `Include()` 호출을 변환하고 하위 쿼리를 사용하여 특정 패턴을 따르는 쿼리를 변환하는 등과 같은 특정 경우에 여러 SQL 문을 생성했습니다. 이는 어떤 경우에는 편리하고 `Include()`의 경우 네트워크를 통해 중복 데이터를 전송하지 않도록 하는 데 도움이 되지만, 구현이 복잡하고 이로 인해 매우 비효율적인 동작(N+1개의 쿼리)이 발생하며 여러 쿼리에서 반환되는 데이터가 일치하지 않을 수 있습니다.

클라이언트 평가와 마찬가지로 EF Core 3.0이 LINQ 쿼리를 단일 SQL 문으로 변환할 수 없는 경우 런타임 예외가 throw됩니다. 그러나 EF Core는 여러 쿼리를 생성하는 데 사용된 많은 공통 패턴을 JOIN을 통해 단일 쿼리로 변환할 수 있도록 설계되었습니다.

## <a name="cosmos-db-support"></a>Cosmos DB 지원 

EF Core의 Cosmos DB 공급자는 EF 프로그래밍 모델에 친숙한 개발자가 애플리케이션 데이터베이스로서 Azure Cosmos DB를 쉽게 대상으로 지정할 수 있도록 합니다. 목표는 글로벌 배포, "always on" 가용성, 탄력적인 확장성, 짧은 대기 시간, .NET 개발자에 훨씬 더 많은 액세스 가능성과 같은 몇몇 Cosmos DB의 장점을 활용하는 것입니다. 공급자는 Cosmos DB의 SQL API에 대해 자동 변경 내용 추적, LINQ, 값 변환 같은 대부분의 EF Core 기능을 사용할 수 있습니다.

자세한 내용은 [Cosmos DB 공급자 설명서](xref:core/providers/cosmos/index)를 참조하세요.

## <a name="c-80-support"></a>C# 8.0 지원

EF Core 3.0은 [C# 8.0의 몇 가지 새로운 기능](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8)을 활용합니다.

### <a name="asynchronous-streams"></a>비동기 스트림

이제 비동기 쿼리 결과는 새 표준 `IAsyncEnumerable<T>` 인터페이스를 사용하여 노출되며 `await foreach`를 사용하여 이용할 수 있습니다.

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Process(o);
} 
```

자세한 내용은 [C# 설명서의 비동기 스트림](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams)을 참조하세요.

### <a name="nullable-reference-types"></a>nullable 참조 형식 

코드에서 이 새로운 기능을 사용하도록 설정하면 EF Core가 참조 형식 속성의 null 허용 여부를 검사하여 데이터베이스의 해당 열과 관계에 적용합니다. nullable이 아닌 참조 형식의 속성은 `[Required]` 데이터 주석 특성인 것처럼 처리됩니다.

예를 들어 다음 클래스에서 `string?` 형식으로 표시된 속성은 선택 사항으로 구성되고 `string` 형식은 필수적으로 구성됩니다.

``` csharp
public class Customer
{
  public int Id { get; set; }
  public string FirstName { get; set; }
  public string LastName { get; set; }
  public string? MiddleName { get; set; }
}
```

자세한 내용은 EF Core 설명서의 [nullable 참조 형식 작업](xref:core/miscellaneous/nullable-reference-types)을 참조하세요.

## <a name="interception-of-database-operations"></a>데이터베이스 작업 인터셉션

EF Core 3.0의 새로운 인터셉션 API를 통해 하위 수준 데이터베이스 작업이 EF Core 기본 작업의 일부로 발생할 때마다 자동으로 호출되는 사용자 지정 논리를 제공할 수 있습니다. 예를 들어 연결을 열거나, 트랜잭션을 커밋하거나, 명령을 실행하는 경우입니다. 

EF 6에 있던 인터셉션 기능과 마찬가지로 인터셉터를 사용하면 작업을 수행하기 전이나 후에 작업을 가로챌 수 있습니다. 이러한 이벤트가 발생하기 전에 가로챌 때 실행을 무시하고 인터셉션 논리에서 대체 결과를 제공할 수 있습니다. 

예를 들어 명령 텍스트를 조작하기 위해 `IDbCommandInterceptor`를 만들 수 있습니다.

``` csharp 
public class HintCommandInterceptor : DbCommandInterceptor
{
  public override InterceptionResult ReaderExecuting(
    DbCommand command, 
    CommandEventData eventData, 
    InterceptionResult result)
  {
    // Manipulate the command text, etc. here...
    command.CommandText += " OPTION (OPTIMIZE FOR UNKNOWN)";
    return result;
  }
}
``` 

 `DbContext`를 통해 등록합니다.

``` csharp
services.AddDbContext(b => b
  .UseSqlServer(connectionString)
  .AddInterceptors(new HintCommandInterceptor()));
```

## <a name="reverse-engineering-of-database-views"></a>데이터베이스 뷰의 리버스 엔지니어링

데이터베이스에서 읽을 수 있지만 업데이트할 수 없는 데이터를 나타내는 쿼리 형식은 [키 없는 엔터티 형식](xref:core/modeling/keyless-entity-types)으로 이름이 변경되었습니다. 대부분의 시나리오에서 데이터베이스 뷰를 매핑하는 데 매우 적합하므로 EF Core는 이제 데이터베이스를 리버스 엔지니어링할 때 키 없는 엔터티 형식을 자동으로 생성합니다.

예를 들어 [dotnet ef 명령줄 도구](xref:core/miscellaneous/cli/dotnet)를 사용하여 다음을 입력할 수 있습니다.

``` console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

그러면 도구는 다음과 같이 키 없이 뷰 및 테이블에 대한 형식을 자동으로 스캐폴드합니다.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
  modelBuilder.Entity<Names>(entity =>
  {
    entity.HasNoKey();
    entity.ToView("Names");
  });

  modelBuilder.Entity<Things>(entity =>
  {
    entity.HasNoKey();
  });
}
```

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

이는 EF Core 3.0 기능이 아니지만 대부분의 기존 고객에게 중요합니다. 

많은 기존 애플리케이션이 이전 버전의 EF를 사용하고 단지 .NET Core를 활용하기 위해 이전 버전의 EF를 EF Core로 이식하는 일은 상당한 노력이 필요할 수 있다는 점을 이해합니다. 그러한 이유로 Microsoft는 .NET Core 3.0에서 최신 버전의 EF 6를 포팅하기로 결정했습니다. 

자세한 내용은 [EF 6의 새로운 기능](xref:ef6/what-is-new/index)을 참조하세요.

## <a name="postponed-features"></a>연기된 기능

원래 EF Core 3.0에 대해 계획되었던 일부 기능은 이후 릴리스로 연기되었습니다.

- 마이그레이션에서 모델의 일부를 무시하는 기능([#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725)로 추적).
- 두 가지 문제로 추적되는 속성 모음 엔터티(공유 형식 엔터티에 대한 [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) 및 인덱싱된 속성 매핑 지원에 대한 [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610)).
