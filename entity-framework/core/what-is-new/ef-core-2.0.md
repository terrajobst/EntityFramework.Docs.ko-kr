---
title: EF Core 2.0의 새로운 기능 - EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: b52b1fe6b2d5a585f4d55b0299891f61cbc968a3
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997572"
---
# <a name="new-features-in-ef-core-20"></a><span data-ttu-id="0c18a-102">EF Core 2.0의 새로운 기능</span><span class="sxs-lookup"><span data-stu-id="0c18a-102">New features in EF Core 2.0</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="0c18a-103">.NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="0c18a-103">.NET Standard 2.0</span></span>
<span data-ttu-id="0c18a-104">이제 EF Core가 .NET Standard 2.0을 대상으로 하므로 .NET Core 2.0, .NET Framework 4.6.1 및 .NET Standard 2.0을 구현하는 기타 라이브러리에서 작동할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-104">EF Core now targets .NET Standard 2.0, which means it can work with .NET Core 2.0, .NET Framework 4.6.1, and other libraries that implement .NET Standard 2.0.</span></span>
<span data-ttu-id="0c18a-105">지원 항목에 대한 자세한 내용은 [지원되는 .NET 구현](../platforms/index.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="0c18a-105">See [Supported .NET Implementations](../platforms/index.md) for more details on what is supported.</span></span>

## <a name="modeling"></a><span data-ttu-id="0c18a-106">모델링</span><span class="sxs-lookup"><span data-stu-id="0c18a-106">Modeling</span></span>

### <a name="table-splitting"></a><span data-ttu-id="0c18a-107">테이블 분할</span><span class="sxs-lookup"><span data-stu-id="0c18a-107">Table splitting</span></span>

<span data-ttu-id="0c18a-108">이제 기본 키 열을 공유하는 동일한 테이블에 둘 이상의 엔터티 형식을 매핑할 수 있으며 각각의 행은 둘 이상의 엔터티에 해당하게 됩니다. </span><span class="sxs-lookup"><span data-stu-id="0c18a-108">It is now possible to map two or more entity types to the same table where the primary key column(s) will be shared and each row will correspond to two or more entities.</span></span>

<span data-ttu-id="0c18a-109">테이블 분할을 사용하려면 테이블을 공유하는 모든 엔터티 형식 사이에 식별 관계(외래 키 속성이 기본 키 형성)가 구성되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-109">To use table splitting an identifying relationship (where foreign key properties form the primary key) must be configured between all of the entity types sharing the table:</span></span>

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

### <a name="owned-types"></a><span data-ttu-id="0c18a-110">소유된 형식</span><span class="sxs-lookup"><span data-stu-id="0c18a-110">Owned types</span></span>

<span data-ttu-id="0c18a-111">소유된 엔터티 형식이 동일한 CLR 형식을 다른 소유된 엔터티 형식과 공유할 수 있으나, CLR 형식만으로는 식별할 수 없으므로 다른 엔터티 형식으로부터의 탐색이 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-111">An owned entity type can share the same CLR type with another owned entity type, but since it cannot be identified just by the CLR type there must be a navigation to it from another entity type.</span></span> <span data-ttu-id="0c18a-112">정의 탐색을 포함하는 엔터티가 그 소유자입니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-112">The entity containing the defining navigation is the owner.</span></span> <span data-ttu-id="0c18a-113">소유자를 쿼리할 때 소유된 형식은 기본적으로 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-113">When querying the owner the owned types will be included by default.</span></span>

<span data-ttu-id="0c18a-114">규칙으로, 소유된 형식에 대해 섀도 기본 키가 만들어지며 테이블 분할을 사용하여 소유자와 동일한 테이블에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-114">By convention a shadow primary key will be created for the owned type and it will be mapped to the same table as the owner by using table splitting.</span></span> <span data-ttu-id="0c18a-115">따라서 EF6에서 복합 형식이 사용되는 방식과 유사하게 소유된 형식을 사용할 수 있습니다. </span><span class="sxs-lookup"><span data-stu-id="0c18a-115">This allows to use owned types similarly to how complex types are used in EF6:</span></span>

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
<span data-ttu-id="0c18a-116">이 기능에 대한 자세한 내용은 [소유한 엔터티 형식에 대한 섹션](xref:core/modeling/owned-entities)을 참고하세요.</span><span class="sxs-lookup"><span data-stu-id="0c18a-116">Read the [section on owned entity types](xref:core/modeling/owned-entities) for more information on this feature.</span></span>

### <a name="model-level-query-filters"></a><span data-ttu-id="0c18a-117">모델 수준 쿼리 필터</span><span class="sxs-lookup"><span data-stu-id="0c18a-117">Model-level query filters</span></span>

<span data-ttu-id="0c18a-118">EF Core 2.0에는 모델 수준 쿼리 필터라고 하는 새 기능이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-118">EF Core 2.0 includes a new feature we call Model-level query filters.</span></span> <span data-ttu-id="0c18a-119">이 기능을 사용하면 LINQ 쿼리 조건자(일반적으로 쿼리 연산자가 있는 LINQ에 전달된 부울 식)을 메타데이터 모델의 엔터티 형식(보통은 OnModelCreating)에 직접 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-119">This feature allows LINQ query predicates (a boolean expression typically passed to the LINQ Where query operator) to be defined directly on Entity Types in the metadata model (usually in OnModelCreating).</span></span> <span data-ttu-id="0c18a-120">이러한 필터는 Include 사용이나 직접 탐색 속성 참조 등의 간접 참조되는 엔터티 형식 등을 포함하여 해당 엔터티 형식과 관련한 모든 LINQ 쿼리에 자동으로 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-120">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="0c18a-121">이 기능의 몇 가지 일반적인 용도는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-121">Some common applications of this feature are:</span></span>

- <span data-ttu-id="0c18a-122">일시 삭제 - 엔터티 형식이 IsDeleted 속성을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-122">Soft delete - An Entity Types defines an IsDeleted property.</span></span>
- <span data-ttu-id="0c18a-123">다중 테넌트 - 엔터티 형식이 TenantId 속성을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-123">Multi-tenancy - An Entity Type defines a TenantId property.</span></span>

<span data-ttu-id="0c18a-124">위 목록의 두 시나리오에서 기능을 보여 주는 간단한 예제는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-124">Here is a simple example demonstrating the feature for the two scenarios listed above:</span></span>

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
            && p.TenantId == this.TenantId );
    }
}
```
<span data-ttu-id="0c18a-125">```Post``` 엔터티 형식의 인스턴스에 대해 멀티 테넌트 및 일시 삭제를 구현하는 모델 수준 필터를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-125">We define a model-level filter that implements multi-tenancy and soft-delete for instances of the ```Post``` Entity Type.</span></span> <span data-ttu-id="0c18a-126">DbContext 인스턴스 수준 속성 ```TenantId```의 사용을 살펴봅니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-126">Note the use of a DbContext instance level property: ```TenantId```.</span></span> <span data-ttu-id="0c18a-127">모델 수준 필터는 올바른 컨텍스트 인스턴스(즉, 쿼리를 실행하는 컨텍스트 인스턴스)의 값을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-127">Model-level filters will use the value from the correct context instance (that is, the context instance that is executing the query).</span></span>

<span data-ttu-id="0c18a-128">IgnoreQueryFilters() 연산자를 사용하여 개별 LINQ 쿼리에 대해 필터를 사용하지 않게 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-128">Filters may be disabled for individual LINQ queries using the IgnoreQueryFilters() operator.</span></span>

#### <a name="limitations"></a><span data-ttu-id="0c18a-129">제한 사항</span><span class="sxs-lookup"><span data-stu-id="0c18a-129">Limitations</span></span>

- <span data-ttu-id="0c18a-130">탐색 참조는 허용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-130">Navigation references are not allowed.</span></span> <span data-ttu-id="0c18a-131">이 기능은 의견에 따라 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-131">This feature may be added based on feedback.</span></span>
- <span data-ttu-id="0c18a-132">계층 구조의 루트 엔터티 형식에서만 필터를 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-132">Filters can only be defined on the root Entity Type of a hierarchy.</span></span>

### <a name="database-scalar-function-mapping"></a><span data-ttu-id="0c18a-133">데이터베이스 스칼라 함수 매핑</span><span class="sxs-lookup"><span data-stu-id="0c18a-133">Database scalar function mapping</span></span>

<span data-ttu-id="0c18a-134">EF Core 2.0에는 데이터베이스 스칼라 함수를 메서드 스텁에 매핑하여 LINQ 쿼리에서 사용하고 SQL로 변환할 수 있게 하는 [Paul Middleton](https://github.com/pmiddleton)이 개발한 중요한 기능이 포함되었습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-134">EF Core 2.0 includes an important contribution from [Paul Middleton](https://github.com/pmiddleton) which enables mapping database scalar functions to method stubs so that they can be used in LINQ queries and translated to SQL.</span></span>

<span data-ttu-id="0c18a-135">이 기능을 사용하는 방법은 간략히 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-135">Here is a brief description of how the feature can be used:</span></span>

<span data-ttu-id="0c18a-136">`DbContext`에서 정적 메서드를 선언하고 `DbFunctionAttribute`를 사용하여 주석 처리합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-136">Declare a static method on your `DbContext` and annotate it with `DbFunctionAttribute`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    [DbFunction]
    public static int PostReadCount(int blogId)
    {
        throw new Exception();
    }
}
```

<span data-ttu-id="0c18a-137">이러한 메서드는 자동으로 등록됩니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-137">Methods like this are automatically registered.</span></span> <span data-ttu-id="0c18a-138">등록된 후에 LINQ 쿼리에서 메서드를 호출하면 SQL에서 함수 호출로 변환될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-138">Once registered, calls to the method in a LINQ query can be translated to function calls in SQL:</span></span>

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

<span data-ttu-id="0c18a-139">몇 가지 참고</span><span class="sxs-lookup"><span data-stu-id="0c18a-139">A few things to note:</span></span>

- <span data-ttu-id="0c18a-140">규칙에 따라, SQL을 생성할 때 메서드 이름은 함수 이름으로 사용되나(이 경우 사용자 정의 함수) 메서드 이름 중에 이 이름과 스키마를 재정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-140">By convention the name of the method is used as the name of a function (in this case a user defined function) when generating the SQL, but you can override the name and schema during method registration</span></span>
- <span data-ttu-id="0c18a-141">현재 스칼라 함수만 지원됩니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-141">Currently only scalar functions are supported</span></span>
- <span data-ttu-id="0c18a-142">데이터베이스에 매핑된 함수를 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-142">You must create the mapped function in the database.</span></span> <span data-ttu-id="0c18a-143">EF Core 마이그레이션을 생성할 필요가 없음</span><span class="sxs-lookup"><span data-stu-id="0c18a-143">EF Core migrations will not take care of creating it</span></span>

### <a name="self-contained-type-configuration-for-code-first"></a><span data-ttu-id="0c18a-144">코드 우선에 대해 자체 포함된 형식 구성</span><span class="sxs-lookup"><span data-stu-id="0c18a-144">Self-contained type configuration for code first</span></span>

<span data-ttu-id="0c18a-145">EF6에서는 *EntityTypeConfiguration*에서 파생하여 특정 엔터티 형식의 코드 우선 구성을 캡슐화할 수 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-145">In EF6 it was possible to encapsulate the code first configuration of a specific entity type by deriving from *EntityTypeConfiguration*.</span></span> <span data-ttu-id="0c18a-146">EF Core 2.0에서는 이 패턴을 다시 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-146">In EF Core 2.0 we are bringing this pattern back:</span></span>

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

## <a name="high-performance"></a><span data-ttu-id="0c18a-147">고성능</span><span class="sxs-lookup"><span data-stu-id="0c18a-147">High Performance</span></span>

### <a name="dbcontext-pooling"></a><span data-ttu-id="0c18a-148">DbContext 풀링</span><span class="sxs-lookup"><span data-stu-id="0c18a-148">DbContext pooling</span></span>

<span data-ttu-id="0c18a-149">일반적으로 ASP.NET Core 응용 프로그램에서 EF Core 사용을 위한 기본 패턴에는 사용자 지정 DbContext 형식을 종속성 주입 시스템에 등록하고 나중에 컨트롤러의 생성자 매개 변수를 통해 해당 형식의 인스턴스를 구하는 것이 관련됩니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-149">The basic pattern for using EF Core in an ASP.NET Core application usually involves registering a custom DbContext type into the dependency injection system and later obtaining instances of that type through constructor parameters in controllers.</span></span> <span data-ttu-id="0c18a-150">즉 각 요청에 대해 DbContext의 새 인스턴스가 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-150">This means a new instance of the DbContext is created for each requests.</span></span>

<span data-ttu-id="0c18a-151">버전 2.0에서는 투명하게 재사용 가능한 DbContext 인스턴스의 풀을 도입하는 종속성 주입에서 사용자 지정 DbContext 형식을 등록하는 새로운 방법이 도입됩니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-151">In version 2.0 we are introducing a new way to register custom DbContext types in dependency injection which transparently introduces a pool of reusable DbContext instances.</span></span> <span data-ttu-id="0c18a-152">DbContext 풀링을 사용하려면 서비스 등록 중에 ```AddDbContext``` 대신 ```AddDbContextPool```을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-152">To use DbContext pooling, use the ```AddDbContextPool``` instead of ```AddDbContext``` during service registration:</span></span>

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

<span data-ttu-id="0c18a-153">이 메서드를 사용하면 컨트롤러가 DbContext 인스턴스를 요청하는 시점에 먼저 풀에서 사용 가능한 인스턴스가 있는지 확인하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-153">If this method is used, at the time a DbContext instance is requested by a controller we will first check if there is an instance available in the pool.</span></span> <span data-ttu-id="0c18a-154">요청 처리가 완료되면 인스턴스의 모든 상태가 재설정되고 인스턴스 자체가 풀로 반환됩니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-154">Once the request processing finalizes, any state on the instance is reset and the instance is itself returned to the pool.</span></span>

<span data-ttu-id="0c18a-155">이 방식은 개념적으로 ADO.NET 공급자에서 연결 풀링이 작동하는 방식과 유사하며 DbContext 인스턴스 초기화의 비용을 다소 절감하는 장점이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-155">This is conceptually similar to how connection pooling operates in ADO.NET providers and has the advantage of saving some of the cost of initialization of DbContext instance.</span></span>

### <a name="limitations"></a><span data-ttu-id="0c18a-156">제한 사항</span><span class="sxs-lookup"><span data-stu-id="0c18a-156">Limitations</span></span>

<span data-ttu-id="0c18a-157">새 메서드에서 DbContext의 ```OnConfiguring()``` 메서드에서 수행할 수 있는 작업에 몇 가지 제한이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-157">The new method introduces a few limitations on what can be done in the ```OnConfiguring()``` method of the DbContext.</span></span>

> [!WARNING]  
> <span data-ttu-id="0c18a-158">요청 간에 공유해야 하는 파생된 DbContext 클래스에서 자체 상태(예: 개인 필드)를 유지 관리하는 경우 DbContext 풀링을 사용하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-158">Avoid using DbContext Pooling if you maintain your own state (for example, private fields) in your derived DbContext class that should not be shared across requests.</span></span> <span data-ttu-id="0c18a-159">EF Core는 DbContext 인스턴스를 풀에 추가하기 전에 인지한 상태만 재설정합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-159">EF Core will only reset the state that is aware of before adding a DbContext instance to the pool.</span></span>

### <a name="explicitly-compiled-queries"></a><span data-ttu-id="0c18a-160">명시적으로 컴파일된 쿼리</span><span class="sxs-lookup"><span data-stu-id="0c18a-160">Explicitly compiled queries</span></span>

<span data-ttu-id="0c18a-161">대규모 시나리오에서 장점을 발휘하도록 설계된 두 번째 선택 성능 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-161">This is the second opt-in performance features designed to offer benefits in high-scale scenarios.</span></span>

<span data-ttu-id="0c18a-162">수동 또는 명시적으로 컴파일된 쿼리 API는 이전 EF 버전에서 제공되었으며, LINQ-SQL에서도 한 번의 계산으로 여러 번 실행하기 위해 응용 프로그램의 쿼리 전환 캐시에 사용되었습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-162">Manual or explicitly compiled query APIs have been available in previous versions of EF and also in LINQ to SQL to allow applications to cache the translation of queries so that they can be computed only once and executed many times.</span></span>

<span data-ttu-id="0c18a-163">일반 EF Core는 쿼리 식의 해시된 표현에 따라 자동으로 쿼리를 컴파일하여 캐시할 수 있으나, 해시 및 캐시 조회 계산을 무시하여 응용 프로그램이 대리자 호출을 통해 기존에 컴파일된 쿼리를 사용하도록 함으로써 약간의 성능 향상을 얻는 데 이 메커니즘을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-163">Although in general EF Core can automatically compile and cache queries based on a hashed representation of the query expressions, this mechanism can be used to obtain a small performance gain by bypassing the computation of the hash and the cache lookup, allowing the application to use an already compiled query through the invocation of a delegate.</span></span>

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

## <a name="change-tracking"></a><span data-ttu-id="0c18a-164">변경 내용 추적</span><span class="sxs-lookup"><span data-stu-id="0c18a-164">Change Tracking</span></span>

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a><span data-ttu-id="0c18a-165">연결은 새 및 기존 엔터티의 그래프를 추적할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-165">Attach can track a graph of new and existing entities</span></span>

<span data-ttu-id="0c18a-166">EF Core는 다양한 메커니즘을 통해 키 값의 자동 생성을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-166">EF Core supports automatic generation of key values through a variety of mechanisms.</span></span> <span data-ttu-id="0c18a-167">이 기능을 사용할 경우 키 속성이 CLR 기본값(보통 0 또는 null)이면 값이 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-167">When using this feature, a value is generated if the key property is the CLR default--usually zero or null.</span></span> <span data-ttu-id="0c18a-168">즉 엔터티의 그래프가 `DbContext.Attach` 또는 `DbSet.Attach`로 전달되며 EF Core는 키가 이미 설정된 엔터티는 `Unchanged`, 키 설정이 없는 엔터티는 `Added`로 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-168">This means that a graph of entities can be passed to `DbContext.Attach` or `DbSet.Attach` and EF Core will mark those entities that have a key already set as `Unchanged` while those entities that do not have a key set will be marked as `Added`.</span></span> <span data-ttu-id="0c18a-169">이렇게 하면 생성된 키를 사용할 때 새 엔터티와 기존 엔터티의 혼합 그래프를 간편하게 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-169">This makes it easy to attach a graph of mixed new and existing entities when using generated keys.</span></span> <span data-ttu-id="0c18a-170">`DbContext.Update` 및 `DbSet.Update`는 키 설정이 있는 엔터티가 `Unchanged` 대신 `Modified`로 표시된다는 점을 제외하고는 동일한 방식으로 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-170">`DbContext.Update` and `DbSet.Update` work in the same way, except that entities with a key set are marked as `Modified` instead of `Unchanged`.</span></span>

## <a name="query"></a><span data-ttu-id="0c18a-171">Query</span><span class="sxs-lookup"><span data-stu-id="0c18a-171">Query</span></span>

### <a name="improved-linq-translation"></a><span data-ttu-id="0c18a-172">향상된 LINQ 변환</span><span class="sxs-lookup"><span data-stu-id="0c18a-172">Improved LINQ Translation</span></span>

<span data-ttu-id="0c18a-173">데이터베이스에서 더 많은 논리를 평가하고(메모리에서보다) 데이터베이스로부터의 불필요한 데이터 검색을 줄이면서 더 많은 쿼리를 성공적으로 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-173">Enables more queries to successfully execute, with more logic being evaluated in the database (rather than in-memory) and less data unnecessarily being retrieved from the database.</span></span>

### <a name="groupjoin-improvements"></a><span data-ttu-id="0c18a-174">GroupJoin 향상</span><span class="sxs-lookup"><span data-stu-id="0c18a-174">GroupJoin improvements</span></span>

<span data-ttu-id="0c18a-175">이 작업은 그룹 조인에 대해 생성된 SQL을 개선합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-175">This work improves the SQL that is generated for group joins.</span></span> <span data-ttu-id="0c18a-176">그룹 조인은 선택적 탐색 속성에 대한 하위 쿼리의 결과인 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-176">Group joins are most often a result of sub-queries on optional navigation properties.</span></span>

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a><span data-ttu-id="0c18a-177">FromSql 및 ExecuteSqlCommand의 문자열 보간 </span><span class="sxs-lookup"><span data-stu-id="0c18a-177">String interpolation in FromSql and ExecuteSqlCommand</span></span>

<span data-ttu-id="0c18a-178">C# 6에는 C# 식을 직접 문자열 리터럴에 포함할 수 있게 하여 런타임에서 문자열을 빌드할 수 있는 훌륭한 방법을 제공하는 문자열 보간이 도입되었습니다. </span><span class="sxs-lookup"><span data-stu-id="0c18a-178">C# 6 introduced String Interpolation, a feature that allows C# expressions to be directly embedded in string literals, providing a nice way of building strings at runtime.</span></span> <span data-ttu-id="0c18a-179">EF Core 2.0에서는 원시 SQL 문자열 ```FromSql``` 및 ```ExecuteSqlCommand```를 허용하는 두 기본 API에 보간 문자열에 대한 특수 지원을 추가했습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-179">In EF Core 2.0 we added special support for interpolated strings to our two primary APIs that accept raw SQL strings: ```FromSql``` and ```ExecuteSqlCommand```.</span></span> <span data-ttu-id="0c18a-180">이 새로운 지원을 통해 C# 문자열 보간을 '안전한' 방법으로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-180">This new support allows C# string interpolation to be used in a 'safe' manner.</span></span> <span data-ttu-id="0c18a-181">즉 동적으로 런타임 시 SQL을 생성할 때 발생할 수 있는 일반적인 SQL 주입 실수로부터 보호할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-181">That is, in a way that protects against common SQL injection mistakes that can occur when dynamically constructing SQL at runtime.</span></span>

<span data-ttu-id="0c18a-182">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-182">Here is an example:</span></span>

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

<span data-ttu-id="0c18a-183">이 예제에서는 SQL 형식 문자열에 두 가지 변수가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-183">In this example, there are two variables embedded in the SQL format string.</span></span> <span data-ttu-id="0c18a-184">EF Core가 다음 SQL을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-184">EF Core will produce the following SQL:</span></span>

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a><span data-ttu-id="0c18a-185">EF.Functions.Like()</span><span class="sxs-lookup"><span data-stu-id="0c18a-185">EF.Functions.Like()</span></span>

<span data-ttu-id="0c18a-186">EF Core 또는 공급자가 데이터베이스 함수나 연산자에 매핑되는 메서드를 정의하여 LINQ 쿼리에서 호출할 수 있게 하는 데 사용할 수 있는 EF.Functions 속성이 추가되었습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-186">We have added the EF.Functions property which can be used by EF Core or providers to define methods that map to database functions or operators so that those can be invoked in LINQ queries.</span></span> <span data-ttu-id="0c18a-187">이러한 메서드의 첫 번째 예는 Like()입니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-187">The first example of such a method is Like():</span></span>

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

<span data-ttu-id="0c18a-188">Like()는 메모리 구현이 다르므로, 메모리 내 데이터베이스에 대해 작업할 경우나 조건자 평가가 클라이언트 측에서 발생해야 하는 경우에 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-188">Note that Like() comes with an in-memory implementation, which can be handy when working against an in-memory database or when evaluation of the predicate needs to occur on the client side.</span></span>

## <a name="database-management"></a><span data-ttu-id="0c18a-189">데이터베이스 관리</span><span class="sxs-lookup"><span data-stu-id="0c18a-189">Database management</span></span>

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a><span data-ttu-id="0c18a-190">DbContext 스캐폴딩에 대한 복수 적용 후크</span><span class="sxs-lookup"><span data-stu-id="0c18a-190">Pluralization hook for DbContext Scaffolding</span></span>

<span data-ttu-id="0c18a-191">EF Core 2.0에는 엔터티 형식 이름을 단수화하고 DbSet 이름은 복수화하는 새로운 *IPluralizer* 서비스가 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-191">EF Core 2.0 introduces a new *IPluralizer* service that is used to singularize entity type names and pluralize DbSet names.</span></span> <span data-ttu-id="0c18a-192">기본 구현은 작업 없음이므로 포그가 자체 복수 처리기에 간편하게 플러그인할 수 있는 후크에 불과합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-192">The default implementation is a no-op, so this is just a hook where folks can easily plug in their own pluralizer.</span></span>

<span data-ttu-id="0c18a-193">개발자가 자체 복수 처리기에 연결하는 방법은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-193">Here is what it looks like for a developer to hook in their own pluralizer:</span></span>

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

## <a name="others"></a><span data-ttu-id="0c18a-194">Others</span><span class="sxs-lookup"><span data-stu-id="0c18a-194">Others</span></span>

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a><span data-ttu-id="0c18a-195">ADO.NET SQLite 공급자를 SQLitePCL.raw로 이동</span><span class="sxs-lookup"><span data-stu-id="0c18a-195">Move ADO.NET SQLite provider to SQLitePCL.raw</span></span>
<span data-ttu-id="0c18a-196">여기서는 다양한 플랫폼에서 원시 SQLite 바이너리를 배포하기 위한 Microsoft.Data.Sqlite의 더 강력한 솔루션을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-196">This gives us a more robust solution in Microsoft.Data.Sqlite for distributing native SQLite binaries on different platforms.</span></span>

### <a name="only-one-provider-per-model"></a><span data-ttu-id="0c18a-197">모델당 단일 공급자</span><span class="sxs-lookup"><span data-stu-id="0c18a-197">Only one provider per model</span></span>
<span data-ttu-id="0c18a-198">공급자가 모델과 상호 작용하는 방식을 보완하고 규칙, 주석 및 복합 API가 다양한 공급자에서 작동하는 방식을 간소화합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-198">Significantly augments how providers can interact with the model and simplifies how conventions, annotations and fluent APIs work with different providers.</span></span>

<span data-ttu-id="0c18a-199">이제 EF Core 2.0은 사용하는 각각의 다른 공급자마다 다른 [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs)을 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-199">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="0c18a-200">일반적으로 응용 프로그램에 투명합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-200">This is usually transparent to the application.</span></span> <span data-ttu-id="0c18a-201">이렇게 하면 하위 수준 메타데이터 API가 간소화되어 *공통 관계형 메타데이터 개념*에 대한 모든 액세스가 항상 `.SqlServer`, `.Sqlite` 등이 아닌 `.Relational` 호출을 통해 이루어집니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-201">This has facilitated a simplification of lower-level metadata APIs such that any access to *common relational metadata concepts* is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc.</span></span>

### <a name="consolidated-logging-and-diagnostics"></a><span data-ttu-id="0c18a-202">통합된 로깅 및 진단</span><span class="sxs-lookup"><span data-stu-id="0c18a-202">Consolidated Logging and Diagnostics</span></span>

<span data-ttu-id="0c18a-203">(ILogger에 기반) 로깅 및 (DiagnosticSource 기반) 진단 메커니즘에서 이제 더 많은 코드를 공유합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-203">Logging (based on ILogger) and Diagnostics (based on DiagnosticSource) mechanisms now share more code.</span></span>

<span data-ttu-id="0c18a-204">2.0에서는 ILogger에 전송된 메시지의 이벤트 ID가 변경되었습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-204">The event IDs for messages sent to an ILogger have changed in 2.0.</span></span> <span data-ttu-id="0c18a-205">이제 이벤트 ID가 EF Core 코드 전체에서 고유합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-205">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="0c18a-206">또한 이 메시지가 MVC 등에서 사용되는 구조화된 로깅의 표준 패턴을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-206">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="0c18a-207">로거 범주도 변경되었습니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-207">Logger categories have also changed.</span></span> <span data-ttu-id="0c18a-208">이제 [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs)를 통해 액세스되는 잘 알려진 범주 집합입니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-208">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="0c18a-209">이제 DiagnosticSource 이벤트가 `ILogger` 메시지와 동일한 이벤트 ID 이름을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="0c18a-209">DiagnosticSource events now use the same event ID names as the corresponding `ILogger` messages.</span></span>
