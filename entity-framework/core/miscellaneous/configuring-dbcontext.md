---
title: DbContext-EF Core 구성
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 9400fe8ea817b6aca0fb63c1de05ffe1dc997b2f
ms.sourcegitcommit: a8b04050033c5dc46c076b7e21b017749e0967a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58868011"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="b1be4-102">DbContext 구성</span><span class="sxs-lookup"><span data-stu-id="b1be4-102">Configuring a DbContext</span></span>

<span data-ttu-id="b1be4-103">이 문서에서는 특정 EF Core 공급자 및 선택적 동작을 사용하여 데이터베이스에 연결하기 위해 `DbContextOptions`를 통해 `DbContext`를 구성하는 기본 패턴을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="b1be4-104">디자인 타임 DbContext 구성</span><span class="sxs-lookup"><span data-stu-id="b1be4-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="b1be4-105">[마이그레이션](xref:core/managing-schemas/migrations/index)과 같은 EF Core 디자인 타임 도구는 응용 프로그램의 엔터티 형식과 데이터베이스 스키마에 매핑하는 방법에 대한 세부 정보를 수집하기 위해 `DbContext` 형식의 작동 중인 인스턴스를 찾고 만들 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="b1be4-106">이 프로세스는 런타임에 구성되는 방식과 유사하게 구성되는 방식으로 `DbContext`를 쉽게 만들 수 있는 한 자동으로 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="b1be4-107">`DbContext`에 필요한 설정 정보를 제공하는 패턴은 런타임에 작동할 수 있지만, 디자인 타임에 `DbContext`를 사용해야 하는 툴은 제한된 수의 패턴으로만 작동할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="b1be4-108">이 내용은 [디자인 타임 DbContext 만들기](xref:core/miscellaneous/cli/dbcontext-creation) 섹션에서 보다 자세히 다루고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="b1be4-109">DbContextOptions 구성</span><span class="sxs-lookup"><span data-stu-id="b1be4-109">Configuring DbContextOptions</span></span>

`DbContext` <span data-ttu-id="b1be4-110">인스턴스가 있어야 `DbContextOptions` 작업을 수행 하기 위해.</span><span class="sxs-lookup"><span data-stu-id="b1be4-110">must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="b1be4-111">`DbContextOptions` 인스턴스는 다음과 같은 구성 정보를 전달합니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="b1be4-112">데이터베이스 공급자를 사용 하려면 일반적으로 같은 메서드를 호출 하 여 선택한 `UseSqlServer` 또는 `UseSqlite`합니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="b1be4-113">이러한 확장 메서드는 해당 공급자 패키지를 같은 필요 `Microsoft.EntityFrameworkCore.SqlServer` 또는 `Microsoft.EntityFrameworkCore.Sqlite`합니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="b1be4-114">에 정의 된 메서드는 `Microsoft.EntityFrameworkCore` 네임 스페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="b1be4-115">모든 필수 연결 문자열이 나 데이터베이스 인스턴스의 식별자입니다. 일반적으로 인수로 전달 위에서 언급 한 공급자 선택 방법</span><span class="sxs-lookup"><span data-stu-id="b1be4-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="b1be4-116">일반적으로 연결 된 공급자 선택 방법에 대 한 호출 내에서 모든 선택적 동작 공급자 수준의 선택기</span><span class="sxs-lookup"><span data-stu-id="b1be4-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="b1be4-117">일반적으로 공급자 선택기 메서드 전이나 후 연결 된 모든 일반 EF Core 동작 선택기</span><span class="sxs-lookup"><span data-stu-id="b1be4-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="b1be4-118">다음 예제에서는 구성 합니다 `DbContextOptions` SQL Server 공급자를 사용 하려면 연결에 포함 합니다 `connectionString` 변수와 공급자 수준의 명령 제한 시간을 실행 하는 모든 쿼리는 EF Core 동작 선택기를를 `DbContext` [비 추적](xref:core/querying/tracking#no-tracking-queries) 기본적으로:</span><span class="sxs-lookup"><span data-stu-id="b1be4-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="b1be4-119">위에서 설명한 공급자 선택기 메서드 및 기타 동작 선택기 메서드는 `DbContextOptions` 또는 공급자 관련 옵션 클래스의 확장 메서드입니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="b1be4-120">이러한 확장 메서드에 액세스하려면 범위에 네임스페이스(일반적으로 `Microsoft.EntityFrameworkCore`)가 있어야 하고 프로젝트에 추가 패키지 종속성이 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="b1be4-121">`DbContextOptions`는 `OnConfiguring` 메서드를 재정의하거나 또는 외부에서 생성자 인수를 통해 `DbContext`에 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="b1be4-122">둘 다 사용되면 `OnConfiguring`이 마지막에 적용되며 생성자 인수에 제공된 옵션을 덮어쓸 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="b1be4-123">생성자 인수</span><span class="sxs-lookup"><span data-stu-id="b1be4-123">Constructor argument</span></span>

<span data-ttu-id="b1be4-124">생성자를 사용한 컨텍스트 코드:</span><span class="sxs-lookup"><span data-stu-id="b1be4-124">Context code with constructor:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> <span data-ttu-id="b1be4-125">DbContext의 기본 생성자도 `DbContextOptions`의 비 제네릭 버전을 허용하지만 비 제네릭 버전을 사용하는 것은 다중 컨텍스트 유형이 있는 응용 프로그램에는 권장되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="b1be4-126">생성자 인수에서 초기화 하는 응용 프로그램 코드:</span><span class="sxs-lookup"><span data-stu-id="b1be4-126">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="b1be4-127">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="b1be4-127">OnConfiguring</span></span>

<span data-ttu-id="b1be4-128">`OnConfiguring`을 사용한 컨텍스트 코드:</span><span class="sxs-lookup"><span data-stu-id="b1be4-128">Context code with `OnConfiguring`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

<span data-ttu-id="b1be4-129">`OnConfiguring`을 사용하여 `DbContext`를 초기화하는 응용 프로그램 코드 :</span><span class="sxs-lookup"><span data-stu-id="b1be4-129">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="b1be4-130">이 방법은 테스트가 전체 데이터베이스를 대상으로하지 않는 한 테스트에 적합하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-130">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="b1be4-131">종속성 주입과 함께 DbContext를 사용하기</span><span class="sxs-lookup"><span data-stu-id="b1be4-131">Using DbContext with dependency injection</span></span>

<span data-ttu-id="b1be4-132">EF Core는 종속성 주입 컨테이너로 `DbContext`를 사용하는 것을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-132">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="b1be4-133">DbContext 형식은 `AddDbContext<TContext>` 메서드를 사용하여 서비스 컨테이너에 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-133">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

`AddDbContext<TContext>` <span data-ttu-id="b1be4-134">둘 다 하 여 DbContext 형식을 하 게 `TContext`, 및 해당 `DbContextOptions<TContext>` 주입 서비스 컨테이너에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-134">will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="b1be4-135">종속성 주입에 대한 추가 정보는 아래의 [더 많은 읽기](#more-reading) 자료를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="b1be4-135">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="b1be4-136">종속성 주입에 `Dbcontext` 추가:</span><span class="sxs-lookup"><span data-stu-id="b1be4-136">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="b1be4-137">이렇게 하려면 `DbContextOptions<TContext>`를 허용하는 DbContext 형식에 [생성자 인수](#constructor-argument)를 추가해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-137">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="b1be4-138">컨텍스트 코드:</span><span class="sxs-lookup"><span data-stu-id="b1be4-138">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="b1be4-139">응용 프로그램 코드 (ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="b1be4-139">Application code (in ASP.NET Core):</span></span>

``` csharp
public class MyController
{
    private readonly BloggingContext _context;

    public MyController(BloggingContext context)
    {
      _context = context;
    }

    ...
}
```

<span data-ttu-id="b1be4-140">응용 프로그램 코드(직접 ServiceProvider 사용, 덜 일반적):</span><span class="sxs-lookup"><span data-stu-id="b1be4-140">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="b1be4-141">DbContext 스레딩 문제를 방지 합니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-141">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="b1be4-142">Entity Framework Core 동일한 실행 되 고 여러 병렬 작업을 지원 하지 않습니다 `DbContext` 인스턴스.</span><span class="sxs-lookup"><span data-stu-id="b1be4-142">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="b1be4-143">동시 액세스 정의 되지 않은 동작, 응용 프로그램 충돌 및 데이터 손상이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-143">Concurrent access can result in undefined behavior, application crashes and data corruption.</span></span> <span data-ttu-id="b1be4-144">항상 사용 하는 것이 때문에 별도 `DbContext` 병렬로 실행 하는 작업에 대 한 인스턴스.</span><span class="sxs-lookup"><span data-stu-id="b1be4-144">Because of this it's important to always use separate `DbContext` instances for operations that execute in parallel.</span></span> 

<span data-ttu-id="b1be4-145">동일한 inadvernetly 원인은 동시 액세스를 수행할 수 있는 일반적인 실수는 `DbContext` 인스턴스:</span><span class="sxs-lookup"><span data-stu-id="b1be4-145">There are common mistakes that can inadvernetly cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="b1be4-146">동일한 DbContext에 대해 다른 작업을 시작 하기 전에 비동기 작업의 완료를 대기할 무시</span><span class="sxs-lookup"><span data-stu-id="b1be4-146">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="b1be4-147">비동기 메서드는 비차단 방식으로 데이터베이스를 액세스 하는 작업을 시작 하려면 EF Core를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-147">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="b1be4-148">그러나 호출자가 이러한 방법 중 하나를 완료를 기다리지는 않습니다 하 고 다른 작업에서 수행 하는 경우는 `DbContext`의 상태는 `DbContext` 일 수 있습니다 (및 가능성이 됩니다) 손상입니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-148">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span> 

<span data-ttu-id="b1be4-149">EF Core 비동기 메서드를 즉시 await 항상 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-149">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="b1be4-150">종속성 주입을 통해 여러 스레드에서 DbContext 인스턴스를 암시적으로 공유</span><span class="sxs-lookup"><span data-stu-id="b1be4-150">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="b1be4-151">합니다 [ `AddDbContext` ](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) 확장 메서드는 등록 `DbContext` 사용 하 여 형식를 [scoped 수명](https://docs .microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) 기본적으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-151">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs .microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span> 

<span data-ttu-id="b1be4-152">될 ASP.NET Core 응용 프로그램에 대 한 동시 액세스 문제 로부터 안전 하 게 지정된 된 시간에 각 클라이언트 요청을 실행 하는 스레드가 하나만 있기 때문에 각 요청을 별도 종속성 주입 범위를 가져옵니다 (및 따라서 별도 `DbContext` 인스턴스)입니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-152">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="b1be4-153">하지만 명시적으로 병렬로 여러 스레드를 실행 하는 모든 코드 인지를 확인 해야 `DbContext` 인스턴스가 아닌 동시에 accesed 적이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b1be4-153">However any code that explicitly executes multiple threads in paralell should ensure that `DbContext` instances aren't ever accesed concurrently.</span></span>

<span data-ttu-id="b1be4-154">종속성 주입을 사용 하 여 수행할 수 있습니다 하거나를 등록 하 여 컨텍스트 범위 지정 및 만들기 범위로 (사용 하 여 `IServiceScopeFactory`) 각 스레드에 대해 또는 등록 하 여 합니다 `DbContext` 일시적으로 (오버 로드를 사용 하 여 `AddDbContext` 를사용하는`ServiceLifetime` 매개 변수).</span><span class="sxs-lookup"><span data-stu-id="b1be4-154">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="b1be4-155">더 많은 읽기</span><span class="sxs-lookup"><span data-stu-id="b1be4-155">More reading</span></span>

* <span data-ttu-id="b1be4-156">ASP.NET Core에서 EF 사용에 대한 자세한 내용은 [ASP.NET Core 시작하기](../get-started/aspnetcore/index.md)를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="b1be4-156">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="b1be4-157">DI 사용에 관해 더 배우려면 [종속성 주입](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection)을 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="b1be4-157">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="b1be4-158">자세한 내용은 [테스트](testing/index.md)를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="b1be4-158">Read [Testing](testing/index.md) for more information.</span></span>
