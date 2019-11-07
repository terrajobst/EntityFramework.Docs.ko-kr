---
title: DbContext 구성-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 3ab90d46b7a4476044e5ea38eaf04f995708e7bf
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655796"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="dffb9-102">DbContext 구성</span><span class="sxs-lookup"><span data-stu-id="dffb9-102">Configuring a DbContext</span></span>

<span data-ttu-id="dffb9-103">이 문서에서는 특정 EF Core 공급자 및 선택적 동작을 사용 하 여 데이터베이스에 연결 하기 위해 `DbContextOptions`을 통해 `DbContext`을 구성 하는 기본 패턴을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="dffb9-104">디자인 타임 DbContext 구성</span><span class="sxs-lookup"><span data-stu-id="dffb9-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="dffb9-105">[마이그레이션과](xref:core/managing-schemas/migrations/index) 같은 EF Core 디자인 타임 도구는 응용 프로그램의 엔터티 형식에 대 한 세부 정보 및 데이터베이스 스키마에 매핑되는 방법에 대 한 세부 정보를 수집 하기 위해 `DbContext` 형식의 작업 인스턴스를 검색 하 고 만들 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="dffb9-106">이 프로세스는 도구에서 런타임에 구성 하는 방식과 비슷하게 구성 되는 방식으로 `DbContext`을 쉽게 만들 수 있는 경우에만 자동으로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="dffb9-107">`DbContext`에 필요한 구성 정보를 제공 하는 패턴은 런타임에 작동할 수 있지만 디자인 타임에 `DbContext`를 사용 해야 하는 도구는 제한 된 수의 패턴 에서만 작동할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="dffb9-108">이러한 항목은 [디자인 타임 컨텍스트 생성](xref:core/miscellaneous/cli/dbcontext-creation) 섹션에서 자세히 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="dffb9-109">DbContextOptions 구성</span><span class="sxs-lookup"><span data-stu-id="dffb9-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="dffb9-110">작업을 수행 하려면 `DbContext`에 `DbContextOptions` 인스턴스가 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="dffb9-111">`DbContextOptions` 인스턴스는 다음과 같은 구성 정보를 전달 합니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="dffb9-112">사용할 데이터베이스 공급자입니다. 일반적으로 `UseSqlServer` 또는 `UseSqlite`과 같은 메서드를 호출 하 여 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="dffb9-113">이러한 확장 메서드에는 `Microsoft.EntityFrameworkCore.SqlServer` 또는 `Microsoft.EntityFrameworkCore.Sqlite`과 같은 해당 공급자 패키지가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="dffb9-114">메서드는 `Microsoft.EntityFrameworkCore` 네임 스페이스에 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="dffb9-115">일반적으로 위에서 언급 한 공급자 선택 메서드에 인수로 전달 되는 데이터베이스 인스턴스의 필수 연결 문자열 또는 식별자입니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="dffb9-116">공급자 수준 선택적 동작 선택기 (일반적으로 공급자 선택 메서드 호출 내에도 연결)</span><span class="sxs-lookup"><span data-stu-id="dffb9-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="dffb9-117">일반적으로 공급자 선택기 메서드에 연결 된 일반적인 EF Core 동작 선택기입니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="dffb9-118">다음 예에서는 SQL Server 공급자를 사용 하도록 `DbContextOptions`을 구성 하 고, `connectionString` 변수에 포함 된 연결, 공급자 수준 명령 제한 시간 및 `DbContext`가 [추적 되지](xref:core/querying/tracking#no-tracking-queries) 않는 모든 쿼리를 실행 하는 EF Core 동작 선택기를 구성 합니다. 기본적으로:</span><span class="sxs-lookup"><span data-stu-id="dffb9-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="dffb9-119">위에서 언급 한 공급자 선택기 메서드 및 기타 동작 선택기 메서드는 `DbContextOptions` 또는 공급자별 옵션 클래스의 확장 메서드입니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="dffb9-120">이러한 확장 메서드에 액세스 하기 위해 범위에 네임 스페이스 (일반적으로 `Microsoft.EntityFrameworkCore`)가 있어야 하 고 프로젝트에 추가 패키지 종속성이 포함 되어야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="dffb9-121">`OnConfiguring` 메서드를 재정의 하거나 생성자 인수를 통해 외부에서 `DbContextOptions`를 `DbContext`에 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="dffb9-122">둘 다 사용 하는 경우 `OnConfiguring`이 마지막으로 적용 되 고 생성자 인수에 제공 된 옵션을 덮어쓸 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="dffb9-123">생성자 인수</span><span class="sxs-lookup"><span data-stu-id="dffb9-123">Constructor argument</span></span>

<span data-ttu-id="dffb9-124">생성자는 다음과 같이 `DbContextOptions`을 허용 하기만 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-124">Your constructor can simply accept a `DbContextOptions` as follows:</span></span>

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
> <span data-ttu-id="dffb9-125">DbContext의 기본 생성자는 비 제네릭 버전의 `DbContextOptions`도 허용 하지만, 여러 컨텍스트 형식이 있는 응용 프로그램에는 제네릭이 아닌 버전을 사용 하지 않는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="dffb9-126">이제 응용 프로그램은 다음과 같이 컨텍스트를 인스턴스화할 때 `DbContextOptions`을 전달할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-126">Your application can now pass the `DbContextOptions` when instantiating a context, as follows:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="dffb9-127">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="dffb9-127">OnConfiguring</span></span>

<span data-ttu-id="dffb9-128">컨텍스트 자체 내에서 `DbContextOptions`을 초기화할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-128">You can also initialize the `DbContextOptions` within the context itself.</span></span> <span data-ttu-id="dffb9-129">기본 구성에이 방법을 사용할 수 있지만 일반적으로 데이터베이스 연결 문자열 등의 외부에서 특정 구성 정보를 가져와야 합니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-129">While you can use this technique for basic configuration, you will typically still need to get certain configuration details from the outside, e.g. a database connection string.</span></span> <span data-ttu-id="dffb9-130">구성 API 또는 다른 방법으로이 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-130">This can be done with a configuration API or any other means.</span></span>

<span data-ttu-id="dffb9-131">컨텍스트 내에서 `DbContextOptions`을 초기화 하려면 `OnConfiguring` 메서드를 재정의 하 고 제공 된 `DbContextOptionsBuilder`에 대해 메서드를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-131">To initialize `DbContextOptions` within the context, override the `OnConfiguring` method and call the methods on the provided `DbContextOptionsBuilder`:</span></span>

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

<span data-ttu-id="dffb9-132">응용 프로그램은 해당 생성자에 아무것도 전달 하지 않고 이러한 컨텍스트를 간단히 인스턴스화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-132">An application can simply instantiate such a context without passing anything to its constructor:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="dffb9-133">이 방법은 테스트가 전체 데이터베이스를 대상으로 하지 않는 한 테스트를 위한 것이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-133">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="dffb9-134">종속성 주입으로 DbContext 사용</span><span class="sxs-lookup"><span data-stu-id="dffb9-134">Using DbContext with dependency injection</span></span>

<span data-ttu-id="dffb9-135">EF Core는 종속성 주입 컨테이너와 `DbContext` 사용을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-135">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="dffb9-136">`AddDbContext<TContext>` 메서드를 사용 하 여 DbContext 형식을 서비스 컨테이너에 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-136">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="dffb9-137">`AddDbContext<TContext>`은 DbContext 형식 `TContext`을 사용 하 고 서비스 컨테이너에서 삽입 하는 데 해당 `DbContextOptions<TContext>`를 사용할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-137">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="dffb9-138">종속성 주입에 대 한 자세한 내용은 아래 [읽기](#more-reading) 를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="dffb9-138">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="dffb9-139">종속성 주입에 `DbContext` 추가:</span><span class="sxs-lookup"><span data-stu-id="dffb9-139">Adding the `DbContext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="dffb9-140">이렇게 하려면 `DbContextOptions<TContext>`을 허용 하는 [생성자 인수](#constructor-argument) 를 DbContext 형식에 추가 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-140">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="dffb9-141">컨텍스트 코드:</span><span class="sxs-lookup"><span data-stu-id="dffb9-141">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="dffb9-142">응용 프로그램 코드 (ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="dffb9-142">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="dffb9-143">응용 프로그램 코드 (ServiceProvider 사용, 일반적이 아닌):</span><span class="sxs-lookup"><span data-stu-id="dffb9-143">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="dffb9-144">DbContext 스레딩 문제 방지</span><span class="sxs-lookup"><span data-stu-id="dffb9-144">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="dffb9-145">Entity Framework Core는 동일한 `DbContext` 인스턴스에서 실행 되는 여러 병렬 작업을 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-145">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="dffb9-146">여기에는 비동기 쿼리의 병렬 실행과 여러 스레드에서의 명시적 동시 사용이 모두 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-146">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="dffb9-147">따라서는 항상 비동기 호출을 즉시 `await` 하거나 병렬로 실행 되는 작업에 대해 별도의 `DbContext` 인스턴스를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-147">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="dffb9-148">EF Core에서 `DbContext` 인스턴스를 동시에 사용 하려고 시도 하면 다음과 같은 메시지가 포함 된 `InvalidOperationException`이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-148">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span>

> <span data-ttu-id="dffb9-149">이전 작업이 완료 되기 전에이 컨텍스트에서 시작 된 두 번째 작업입니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-149">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="dffb9-150">이 문제는 일반적으로 DbContext의 동일한 인스턴스를 사용 하는 다른 스레드에 의해 발생 하지만 인스턴스 멤버는 스레드로부터 안전 하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-150">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="dffb9-151">동시 액세스가 검색 되지 않는 경우에는 정의 되지 않은 동작, 응용 프로그램 충돌 및 데이터 손상이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-151">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="dffb9-152">실수로 동일한 `DbContext` 인스턴스에서 동시에 액세스할 수 있는 일반적인 실수가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-152">There are common mistakes that can inadvertently cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="dffb9-153">동일한 DbContext에서 다른 작업을 시작 하기 전에 비동기 작업의 완료를 기다리지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-153">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="dffb9-154">비동기 메서드를 사용 하면 EF Core를 사용 하 여 비차단 방식으로 데이터베이스에 액세스 하는 작업을 시작할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-154">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="dffb9-155">그러나 호출자가 이러한 메서드 중 하나를 완료 하는 것을 기다리지 않고 `DbContext`에 대 한 다른 작업을 수행 하는 경우에는 `DbContext` 상태가 손상 될 수 있습니다 (가능성이 매우 높음).</span><span class="sxs-lookup"><span data-stu-id="dffb9-155">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span>

<span data-ttu-id="dffb9-156">항상 EF Core 비동기 메서드를 즉시 기다립니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-156">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="dffb9-157">종속성 주입을 통해 여러 스레드 간에 DbContext 인스턴스를 암시적으로 공유</span><span class="sxs-lookup"><span data-stu-id="dffb9-157">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="dffb9-158">[`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) 확장 메서드는 기본적으로 범위가 지정 된 [수명](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) 으로 `DbContext` 형식을 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-158">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span>

<span data-ttu-id="dffb9-159">이는 지정 된 시간에 각 클라이언트 요청을 실행 하는 스레드가 하나 뿐이 고 각 요청이 별도의 종속성 주입 범위 (따라서 별도의 `DbContext`)를 가져오기 때문에 ASP.NET Core 응용 프로그램의 동시 액세스 문제와는 안전 합니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-159">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="dffb9-160">그러나 동시에 여러 스레드를 명시적으로 실행 하는 코드는 `DbContext` 인스턴스가 동시에 액세스 되지 않도록 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-160">However any code that explicitly executes multiple threads in parallel should ensure that `DbContext` instances aren't ever accessed concurrently.</span></span>

<span data-ttu-id="dffb9-161">종속성 주입을 사용 하 여이 작업을 수행 하려면 컨텍스트를 범위 지정으로 등록 하 고 각 스레드에 대해 범위 (`IServiceScopeFactory` 사용)를 만들거나 `DbContext`을 임시 (`ServiceLifetime` 매개 변수를 사용 하는 `AddDbContext`의 오버 로드를 사용 하 여)로 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="dffb9-161">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="dffb9-162">추가 정보</span><span class="sxs-lookup"><span data-stu-id="dffb9-162">More reading</span></span>

- <span data-ttu-id="dffb9-163">DI를 사용 하는 방법에 대 한 자세한 내용은 [종속성 주입](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) 을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="dffb9-163">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
- <span data-ttu-id="dffb9-164">자세한 내용은 [테스트](testing/index.md) 를 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="dffb9-164">Read [Testing](testing/index.md) for more information.</span></span>
