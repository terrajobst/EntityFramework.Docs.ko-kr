---
title: EF Core 2.0의 새로운 기능 - EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: 83f6b819409d502dba17a678d44a0746a4a77f4b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413578"
---
# <a name="new-features-in-ef-core-20"></a>EF Core 2.0의 새로운 기능

## <a name="net-standard-20"></a>.NET Standard 2.0

이제 EF Core가 .NET Standard 2.0을 대상으로 하므로 .NET Core 2.0, .NET Framework 4.6.1 및 .NET Standard 2.0을 구현하는 기타 라이브러리에서 작동할 수 있습니다.
지원 항목에 대한 자세한 내용은 [지원되는 .NET 구현](../platforms/index.md)을 참조하세요.

## <a name="modeling"></a>모델링

### <a name="table-splitting"></a>테이블 분할

이제 기본 키 열을 공유하는 동일한 테이블에 둘 이상의 엔터티 형식을 매핑할 수 있으며 각각의 행은 둘 이상의 엔터티에 해당하게 됩니다.

테이블 분할을 사용하려면 테이블을 공유하는 모든 엔터티 형식 사이에 식별 관계(외래 키 속성이 기본 키 형성)가 구성되어야 합니다.

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

이 기능에 대한 자세한 내용은 [테이블 분할에 대한 섹션](xref:core/modeling/table-splitting)을 참조하세요.

### <a name="owned-types"></a>소유된 형식

소유 엔터티 형식이 동일한 .NET 형식을 다른 소유 엔터티 형식과 공유할 수 있으나, .NET 형식만으로는 식별할 수 없으므로 다른 엔터티 형식으로부터의 탐색이 있어야 합니다. 정의 탐색을 포함하는 엔터티가 그 소유자입니다. 소유자를 쿼리할 때 소유된 형식은 기본적으로 포함됩니다.

규칙으로, 소유된 형식에 대해 섀도 기본 키가 만들어지며 테이블 분할을 사용하여 소유자와 동일한 테이블에 매핑됩니다. 따라서 EF6에서 복합 형식이 사용되는 방식과 유사하게 소유된 형식을 사용할 수 있습니다.

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, cb =>
    {
        cb.OwnsOne(c => c.BillingAddress);
        cb.OwnsOne(c => c.ShippingAddress);
    });

public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

이 기능에 대한 자세한 내용은 [소유한 엔터티 형식에 대한 섹션](xref:core/modeling/owned-entities)을 참고하세요.

### <a name="model-level-query-filters"></a>모델 수준 쿼리 필터

EF Core 2.0에는 모델 수준 쿼리 필터라고 하는 새 기능이 포함됩니다. 이 기능을 사용하면 LINQ 쿼리 조건자(일반적으로 쿼리 연산자가 있는 LINQ에 전달된 부울 식)을 메타데이터 모델의 엔터티 형식(보통은 OnModelCreating)에 직접 정의할 수 있습니다. 이러한 필터는 Include 사용이나 직접 탐색 속성 참조 등의 간접 참조되는 엔터티 형식 등을 포함하여 해당 엔터티 형식과 관련한 모든 LINQ 쿼리에 자동으로 적용됩니다. 이 기능의 몇 가지 일반적인 용도는 다음과 같습니다.

- 일시 삭제 - 엔터티 형식이 IsDeleted 속성을 정의합니다.
- 다중 테넌트 - 엔터티 형식이 TenantId 속성을 정의합니다.

위 목록의 두 시나리오에서 기능을 보여 주는 간단한 예제는 다음과 같습니다.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    public int TenantId { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>().HasQueryFilter(
            p => !p.IsDeleted
            && p.TenantId == this.TenantId);
    }
}
```

`Post` 엔터티 형식의 인스턴스에 대해 멀티 테넌트 및 일시 삭제를 구현하는 모델 수준 필터를 정의합니다. `DbContext` 인스턴스 수준 속성`TenantId` 사용: 모델 수준 필터는 올바른 컨텍스트 인스턴스(즉, 쿼리를 실행하는 컨텍스트 인스턴스)의 값을 사용합니다.

IgnoreQueryFilters() 연산자를 사용하여 개별 LINQ 쿼리에 대해 필터를 사용하지 않게 설정할 수 있습니다.

#### <a name="limitations"></a>제한 사항

- 탐색 참조는 허용되지 않습니다. 이 기능은 의견에 따라 추가할 수 있습니다.
- 계층 구조의 루트 엔터티 형식에서만 필터를 정의할 수 있습니다.

### <a name="database-scalar-function-mapping"></a>데이터베이스 스칼라 함수 매핑

EF Core 2.0에는 데이터베이스 스칼라 함수를 메서드 스텁에 매핑하여 LINQ 쿼리에서 사용하고 SQL로 변환할 수 있게 하는 [Paul Middleton](https://github.com/pmiddleton)이 개발한 중요한 기능이 포함되었습니다.

이 기능을 사용하는 방법은 간략히 다음과 같습니다.

`DbContext`에서 정적 메서드를 선언하고 `DbFunctionAttribute`를 사용하여 주석 처리합니다.

``` csharp
public class BloggingContext : DbContext
{
    [DbFunction]
    public static int PostReadCount(int blogId)
    {
        throw new NotImplementedException();
    }
}
```

이러한 메서드는 자동으로 등록됩니다. 등록된 후에 LINQ 쿼리에서 메서드를 호출하면 SQL에서 함수 호출로 변환될 수 있습니다.

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

몇 가지 참고

- 규칙에 따라, SQL을 생성할 때 메서드 이름은 함수 이름으로 사용되나(이 경우 사용자 정의 함수) 메서드 이름 중에 이 이름과 스키마를 재정의할 수 있습니다.
- 현재 스칼라 함수만 지원됩니다.
- 데이터베이스에 매핑된 함수를 만들어야 합니다. EF Core 마이그레이션을 생성할 필요가 없습니다.

### <a name="self-contained-type-configuration-for-code-first"></a>코드 우선에 대해 자체 포함된 형식 구성

EF6에서는 *EntityTypeConfiguration*에서 파생하여 특정 엔터티 형식의 코드 우선 구성을 캡슐화할 수 있었습니다. EF Core 2.0에서는 이 패턴을 다시 사용합니다.

``` csharp
class CustomerConfiguration : IEntityTypeConfiguration<Customer>
{
    public void Configure(EntityTypeBuilder<Customer> builder)
    {
        builder.HasKey(c => c.AlternateKey);
        builder.Property(c => c.Name).HasMaxLength(200);
    }
}

...
// OnModelCreating
builder.ApplyConfiguration(new CustomerConfiguration());
```

## <a name="high-performance"></a>고성능

### <a name="dbcontext-pooling"></a>DbContext 풀링

일반적으로 ASP.NET Core 애플리케이션에서 EF Core 사용을 위한 기본 패턴에는 사용자 지정 DbContext 형식을 종속성 주입 시스템에 등록하고 나중에 컨트롤러의 생성자 매개 변수를 통해 해당 형식의 인스턴스를 구하는 것이 관련됩니다. 즉 각 요청에 대해 DbContext의 새 인스턴스가 만들어집니다.

버전 2.0에서는 투명하게 재사용 가능한 DbContext 인스턴스의 풀을 도입하는 종속성 주입에서 사용자 지정 DbContext 형식을 등록하는 새로운 방법이 도입됩니다. DbContext 풀링을 사용하려면 서비스 등록 중에 `AddDbContext` 대신 `AddDbContextPool`을 사용합니다.

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

이 메서드를 사용하면 컨트롤러가 DbContext 인스턴스를 요청하는 시점에 먼저 풀에서 사용 가능한 인스턴스가 있는지 확인하게 됩니다. 요청 처리가 완료되면 인스턴스의 모든 상태가 재설정되고 인스턴스 자체가 풀로 반환됩니다.

이 방식은 개념적으로 ADO.NET 공급자에서 연결 풀링이 작동하는 방식과 유사하며 DbContext 인스턴스 초기화의 비용을 다소 절감하는 장점이 있습니다.

### <a name="limitations"></a>제한 사항

새 메서드에서 DbContext의 `OnConfiguring()` 메서드에서 수행할 수 있는 작업에 몇 가지 제한이 있습니다.

> [!WARNING]  
> 요청 간에 공유해야 하는 파생된 DbContext 클래스에서 자체 상태(예: 개인 필드)를 유지 관리하는 경우 DbContext 풀링을 사용하지 않습니다. EF Core는 DbContext 인스턴스를 풀에 추가하기 전에 인지한 상태만 재설정합니다.

### <a name="explicitly-compiled-queries"></a>명시적으로 컴파일된 쿼리

대규모 시나리오에서 장점을 발휘하도록 설계된 두 번째 선택 성능 기능입니다.

수동 또는 명시적으로 컴파일된 쿼리 API는 이전 EF 버전에서 제공되었으며, LINQ-SQL에서도 한 번의 계산으로 여러 번 실행하기 위해 애플리케이션의 쿼리 전환 캐시에 사용되었습니다.

일반 EF Core는 쿼리 식의 해시된 표현에 따라 자동으로 쿼리를 컴파일하여 캐시할 수 있으나, 해시 및 캐시 조회 계산을 무시하여 애플리케이션이 대리자 호출을 통해 기존에 컴파일된 쿼리를 사용하도록 함으로써 약간의 성능 향상을 얻는 데 이 메커니즘을 사용할 수 있습니다.

``` csharp
// Create an explicitly compiled query
private static Func<CustomerContext, int, Customer> _customerById =
    EF.CompileQuery((CustomerContext db, int id) =>
        db.Customers
            .Include(c => c.Address)
            .Single(c => c.Id == id));

// Use the compiled query by invoking it
using (var db = new CustomerContext())
{
   var customer = _customerById(db, 147);
}
```

## <a name="change-tracking"></a>변경 내용 추적

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a>연결은 새 및 기존 엔터티의 그래프를 추적할 수 있습니다.

EF Core는 다양한 메커니즘을 통해 키 값의 자동 생성을 지원합니다. 이 기능을 사용할 경우 키 속성이 CLR 기본값(보통 0 또는 null)이면 값이 생성됩니다. 즉 엔터티의 그래프가 `DbContext.Attach` 또는 `DbSet.Attach`로 전달되며 EF Core는 키가 이미 설정된 엔터티는 `Unchanged`, 키 설정이 없는 엔터티는 `Added`로 표시합니다. 이렇게 하면 생성된 키를 사용할 때 새 엔터티와 기존 엔터티의 혼합 그래프를 간편하게 연결할 수 있습니다. `DbContext.Update` 및 `DbSet.Update`는 키 설정이 있는 엔터티가 `Unchanged` 대신 `Modified`로 표시된다는 점을 제외하고는 동일한 방식으로 작동합니다.

## <a name="query"></a>쿼리

### <a name="improved-linq-translation"></a>향상된 LINQ 변환

데이터베이스에서 더 많은 논리를 평가하고(메모리에서보다) 데이터베이스로부터의 불필요한 데이터 검색을 줄이면서 더 많은 쿼리를 성공적으로 실행할 수 있습니다.

### <a name="groupjoin-improvements"></a>GroupJoin 향상

이 작업은 그룹 조인에 대해 생성된 SQL을 개선합니다. 그룹 조인은 선택적 탐색 속성에 대한 하위 쿼리의 결과인 경우가 많습니다.

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a>FromSql 및 ExecuteSqlCommand의 문자열 보간

C# 6에는 C# 식을 직접 문자열 리터럴에 포함할 수 있게 하여 런타임에서 문자열을 빌드할 수 있는 훌륭한 방법을 제공하는 문자열 보간이 도입되었습니다. EF Core 2.0에서는 원시 SQL 문자열 `FromSql` 및 `ExecuteSqlCommand`를 허용하는 두 기본 API에 보간 문자열에 대한 특수 지원을 추가했습니다. 이 새로운 지원을 통해 C# 문자열 보간을 "안전한" 방법으로 사용할 수 있습니다. 즉 동적으로 런타임 시 SQL을 생성할 때 발생할 수 있는 일반적인 SQL 주입 실수로부터 보호할 수 있습니다.

예를 들면 다음과 같습니다.

``` csharp
var city = "London";
var contactTitle = "Sales Representative";

using (var context = CreateContext())
{
    context.Set<Customer>()
        .FromSql($@"
            SELECT *
            FROM ""Customers""
            WHERE ""City"" = {city} AND
                ""ContactTitle"" = {contactTitle}")
            .ToArray();
  }
```

이 예제에서는 SQL 형식 문자열에 두 가지 변수가 포함됩니다. EF Core가 다음 SQL을 생성합니다.

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a>EF.Functions.Like()

EF Core 또는 공급자가 데이터베이스 함수나 연산자에 매핑되는 메서드를 정의하여 LINQ 쿼리에서 호출할 수 있게 하는 데 사용할 수 있는 EF.Functions 속성이 추가되었습니다. 이러한 메서드의 첫 번째 예는 Like()입니다.

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

Like()는 메모리 구현이 다르므로, 메모리 내 데이터베이스에 대해 작업할 경우나 조건자 평가가 클라이언트 측에서 발생해야 하는 경우에 유용합니다.

## <a name="database-management"></a>데이터베이스 관리

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a>DbContext 스캐폴딩에 대한 복수 적용 후크

EF Core 2.0에는 엔터티 형식 이름을 단수화하고 DbSet 이름은 복수화하는 새로운 *IPluralizer* 서비스가 도입되었습니다. 기본 구현은 작업 없음이므로 포그가 자체 복수 처리기에 간편하게 플러그인할 수 있는 후크에 불과합니다.

개발자가 자체 복수 처리기에 연결하는 방법은 다음과 같습니다.

``` csharp
public class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
    {
        services.AddSingleton<IPluralizer, MyPluralizer>();
    }
}

public class MyPluralizer : IPluralizer
{
    public string Pluralize(string name)
    {
        return Inflector.Inflector.Pluralize(name) ?? name;
    }

    public string Singularize(string name)
    {
        return Inflector.Inflector.Singularize(name) ?? name;
    }
}
```

## <a name="others"></a>Others

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a>ADO.NET SQLite 공급자를 SQLitePCL.raw로 이동

여기서는 다양한 플랫폼에서 원시 SQLite 바이너리를 배포하기 위한 Microsoft.Data.Sqlite의 더 강력한 솔루션을 제공합니다.

### <a name="only-one-provider-per-model"></a>모델당 단일 공급자

공급자가 모델과 상호 작용하는 방식을 보완하고 규칙, 주석 및 복합 API가 다양한 공급자에서 작동하는 방식을 간소화합니다.

이제 EF Core 2.0은 사용하는 각각의 다른 공급자마다 다른 [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs)을 빌드합니다. 일반적으로 애플리케이션에 투명합니다. 이렇게 하면 하위 수준 메타데이터 API가 간소화되어 *공통 관계형 메타데이터 개념*에 대한 모든 액세스가 항상 `.SqlServer`, `.Sqlite` 등이 아닌 `.Relational` 호출을 통해 이루어집니다.

### <a name="consolidated-logging-and-diagnostics"></a>통합된 로깅 및 진단

(ILogger에 기반) 로깅 및 (DiagnosticSource 기반) 진단 메커니즘에서 이제 더 많은 코드를 공유합니다.

2\.0에서는 ILogger에 전송된 메시지의 이벤트 ID가 변경되었습니다. 이제 이벤트 ID가 EF Core 코드 전체에서 고유합니다. 또한 이 메시지가 MVC 등에서 사용되는 구조화된 로깅의 표준 패턴을 따릅니다.

로거 범주도 변경되었습니다. 이제 [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs)를 통해 액세스되는 잘 알려진 범주 집합입니다.

이제 DiagnosticSource 이벤트가 `ILogger` 메시지와 동일한 이벤트 ID 이름을 사용합니다.
