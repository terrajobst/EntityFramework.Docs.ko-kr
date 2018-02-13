---
title: "DbContext-EF 핵심 구성"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 6980acd53b0a74055af7a1e04b476f4625c327c9
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/12/2018
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="15fde-102">DbContext를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-102">Configuring a DbContext</span></span>

<span data-ttu-id="15fde-103">이 문서에서는 기본 패턴을 구성 하는 데는 `DbContext` 통해는 `DbContextOptions` 특정 EF 핵심 공급자 및 선택적 동작을 사용 하 여 데이터베이스에 연결 하 합니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="15fde-104">디자인 타임 DbContext 구성</span><span class="sxs-lookup"><span data-stu-id="15fde-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="15fde-105">EF 핵심 디자인 타임와 같은 도구 [마이그레이션](xref:core/managing-schemas/migrations/index) 검색 하 고 작업의 인스턴스를 만들 수 있어야는 `DbContext` 응용 프로그램의 엔터티 형식 및 데이터베이스 스키마에 매핑되는 방식에 대 한 세부 정보를 수집 하려면 쿼리 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="15fde-106">이 프로세스도 가능 자동으로 도구를 쉽게 만들 수는 `DbContext` 것은 구성 하는 것 마찬가지로 실행 시 방법을 구성 하는 방식에서입니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="15fde-107">임의의 패턴에 필요한 구성 정보를 제공 하는 동안는 `DbContext` 런타임에 사용 하 여 해야 하는 도구에서 작업할 수는 `DbContext` 디자인 타임에만 처리할 수 있는 제한 된 수의 패턴입니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="15fde-108">더 자세하게에서 살펴보시기는 [디자인 타임 컨텍스트를 만들지](xref:core/miscellaneous/cli/dbcontext-creation) 섹션.</span><span class="sxs-lookup"><span data-stu-id="15fde-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="15fde-109">DbContextOptions 구성</span><span class="sxs-lookup"><span data-stu-id="15fde-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="15fde-110">`DbContext` 인스턴스가 있어야 `DbContextOptions` 하려면 작업을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="15fde-111">`DbContextOptions` 인스턴스와 같은 구성 정보를 전달 합니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="15fde-112">와 같은 메서드를 호출 하 여 데이터베이스 공급자를 사용 하려면 일반적으로 선택 `UseSqlServer` 또는 `UseSqlite`</span><span class="sxs-lookup"><span data-stu-id="15fde-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`</span></span>
- <span data-ttu-id="15fde-113">모든 필수 연결 문자열이 나 데이터베이스 인스턴스의 식별자입니다. 일반적으로 인수로 전달 위에서 언급 한 공급자 선택 방법</span><span class="sxs-lookup"><span data-stu-id="15fde-113">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="15fde-114">일반적으로 내 공급자 선택 방법에 대 한 호출 또한 연결 된 공급자 수준의 선택적 동작 선택기</span><span class="sxs-lookup"><span data-stu-id="15fde-114">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="15fde-115">일반적으로 공급자 선택기 메서드 전이나 후 연결 된 모든 일반 EF 코어 동작 선택기</span><span class="sxs-lookup"><span data-stu-id="15fde-115">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="15fde-116">다음 예제에서는 구성에서 `DbContextOptions` 에 포함 된 연결을 SQL Server 공급자를 사용 하는 `connectionString` 변수와 공급자 수준의 명령 제한 시간에 실행 되는 모든 쿼리는 EF 코어 동작 선택 기가 있는 `DbContext` [아니요 추적](xref:core/querying/tracking#no-tracking-queries) 기본적으로:</span><span class="sxs-lookup"><span data-stu-id="15fde-116">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="15fde-117">공급자 선택기 메서드 및 위에서 언급 한 다른 동작 선택기 메서드는 확장명 메서드가 `DbContextOptions` 또는 공급자별 옵션 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-117">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="15fde-118">네임 스페이스를 포함 해야 합니다. 이러한 확장 메서드에 대 한 액세스를 가지려면 (일반적으로 `Microsoft.EntityFrameworkCore`)에서 범위가 지정 되 고 프로젝트에 추가 패키지 종속성을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-118">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="15fde-119">`DbContextOptions` 에 제공할 수는 `DbContext` 재정의 하 여는 `OnConfiguring` 메서드 또는 생성자 인수를 통해 외부적으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-119">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="15fde-120">모두 사용 되 면 `OnConfiguring` 마지막에 적용 하 고는 생성자 인수를 제공 하는 옵션을 덮어쓸 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-120">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="15fde-121">생성자 인수</span><span class="sxs-lookup"><span data-stu-id="15fde-121">Constructor argument</span></span>

<span data-ttu-id="15fde-122">생성자를 사용 하 여 상황에 맞는 코드:</span><span class="sxs-lookup"><span data-stu-id="15fde-122">Context code with constructor:</span></span>

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
> <span data-ttu-id="15fde-123">DbContext의 기본 생성자는 또한의 비 제네릭 버전 허용 `DbContextOptions`, 하지만 비 제네릭 버전을 사용 하 여 여러 컨텍스트 유형으로 응용 프로그램에 대 한 권장 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-123">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="15fde-124">생성자 인수에서 초기화 하는 응용 프로그램 코드:</span><span class="sxs-lookup"><span data-stu-id="15fde-124">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="15fde-125">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="15fde-125">OnConfiguring</span></span>

<span data-ttu-id="15fde-126">사용 하 여 상황에 맞는 코드 `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="15fde-126">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="15fde-127">초기화 하는 응용 프로그램 코드는 `DbContext` 사용 하 여 `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="15fde-127">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="15fde-128">이 접근 방식에 적합 하지 않은 자체 테스트를 테스트 전체 데이터베이스를 대상으로 하지 않는 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-128">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="15fde-129">종속성 주입 DbContext 사용</span><span class="sxs-lookup"><span data-stu-id="15fde-129">Using DbContext with dependency injection</span></span>

<span data-ttu-id="15fde-130">EF 코어를 사용 하 여 원하는 `DbContext` 종속성 주입 컨테이너를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-130">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="15fde-131">DbContext 형식을 사용 하 여 서비스 컨테이너에 추가할 수 있습니다는 `AddDbContext<TContext>` 메서드.</span><span class="sxs-lookup"><span data-stu-id="15fde-131">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="15fde-132">`AddDbContext<TContext>` 모두에 DbContext 형식 하면 `TContext`, 및 해당 `DbContextOptions<TContext>` 서비스 컨테이너에서 삽입에 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-132">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="15fde-133">참조 [더 많은 읽기](#more-reading) 아래에 대 한 자세한 내용은 종속성 주입 합니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-133">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="15fde-134">추가 `Dbcontext` 종속성 주입에:</span><span class="sxs-lookup"><span data-stu-id="15fde-134">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="15fde-135">이 위해서는 추가 [생성자 인수](#constructor-argument) 를 허용 하 DbContext 형식으로 `DbContextOptions<TContext>`합니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-135">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="15fde-136">상황에 맞는 코드:</span><span class="sxs-lookup"><span data-stu-id="15fde-136">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="15fde-137">응용 프로그램 코드 (ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="15fde-137">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="15fde-138">응용 프로그램 코드 (ServiceProvider를 코드도 직접 사용):</span><span class="sxs-lookup"><span data-stu-id="15fde-138">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a><span data-ttu-id="15fde-139">더 많은 읽기</span><span class="sxs-lookup"><span data-stu-id="15fde-139">More reading</span></span>

* <span data-ttu-id="15fde-140">읽기 [ASP.NET 코어에서 시작](../get-started/aspnetcore/index.md) EF를 사용 하 여 ASP.NET 코어 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-140">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="15fde-141">읽기 [종속성 주입](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) DI를 사용 하는 방법에 대 한 자세한 내용을 보려면 합니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-141">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="15fde-142">읽기 [테스트](testing/index.md) 자세한 정보에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="15fde-142">Read [Testing](testing/index.md) for more information.</span></span>
