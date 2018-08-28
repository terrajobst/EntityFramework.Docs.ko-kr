---
title: DbContext-EF Core 구성
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 393349c05ffaf42c6d2520e73abce23def6becc0
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995940"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="46c1e-102">DbContext 구성</span><span class="sxs-lookup"><span data-stu-id="46c1e-102">Configuring a DbContext</span></span>

<span data-ttu-id="46c1e-103">이 문서에서는 구성에 대 한 기본 패턴을 보여 줍니다.는 `DbContext` 를 통해를 `DbContextOptions` 특정 EF Core 공급자 및 선택적 동작을 사용 하 여 데이터베이스에 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="46c1e-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="46c1e-104">디자인 타임 DbContext 구성</span><span class="sxs-lookup"><span data-stu-id="46c1e-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="46c1e-105">EF Core 디자인 타임 도구와 같은 [마이그레이션을](xref:core/managing-schemas/migrations/index) 검색 하 고 작업의 인스턴스를 만들 수 있어야를 `DbContext` 응용 프로그램의 엔터티 형식 및 데이터베이스 스키마에 매핑하는 방법에 대 한 세부 정보를 수집 하기 위해 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="46c1e-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="46c1e-106">도구를 쉽게 만들 수 있다면이 프로세스는 자동 수는 `DbContext` 런타임 시 구성 하는 방법을에 비슷하게 구성 수는 방식에서입니다.</span><span class="sxs-lookup"><span data-stu-id="46c1e-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="46c1e-107">모든 패턴에 필요한 구성 정보를 제공 하는 동안 합니다 `DbContext` 실행 시간을 사용 해야 하는 도구에서 작업할 수는 `DbContext` 디자인 타임에만 작업할 수 제한 된 수의 패턴입니다.</span><span class="sxs-lookup"><span data-stu-id="46c1e-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="46c1e-108">더 자세히 살펴보시기 합니다 [디자인 타임에 컨텍스트를 만들지](xref:core/miscellaneous/cli/dbcontext-creation) 섹션입니다.</span><span class="sxs-lookup"><span data-stu-id="46c1e-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="46c1e-109">DbContextOptions 구성</span><span class="sxs-lookup"><span data-stu-id="46c1e-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="46c1e-110">`DbContext` 인스턴스가 있어야 `DbContextOptions` 작업을 수행 하기 위해.</span><span class="sxs-lookup"><span data-stu-id="46c1e-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="46c1e-111">`DbContextOptions` 인스턴스와 같은 구성 정보를 전달 합니다.</span><span class="sxs-lookup"><span data-stu-id="46c1e-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="46c1e-112">를 사용 하려면 데이터베이스 공급자와 같은 메서드를 호출 하 여 일반적으로 선택한 `UseSqlServer` 또는 `UseSqlite`</span><span class="sxs-lookup"><span data-stu-id="46c1e-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`</span></span>
- <span data-ttu-id="46c1e-113">모든 필수 연결 문자열이 나 데이터베이스 인스턴스의 식별자입니다. 일반적으로 인수로 전달 위에서 언급 한 공급자 선택 방법</span><span class="sxs-lookup"><span data-stu-id="46c1e-113">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="46c1e-114">일반적으로 연결 된 공급자 선택 방법에 대 한 호출 내에서 모든 선택적 동작 공급자 수준의 선택기</span><span class="sxs-lookup"><span data-stu-id="46c1e-114">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="46c1e-115">일반적으로 공급자 선택기 메서드 전이나 후 연결 된 모든 일반 EF Core 동작 선택기</span><span class="sxs-lookup"><span data-stu-id="46c1e-115">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="46c1e-116">다음 예제에서는 구성 합니다 `DbContextOptions` SQL Server 공급자를 사용 하려면 연결에 포함 합니다 `connectionString` 변수와 공급자 수준의 명령 제한 시간을 실행 하는 모든 쿼리는 EF Core 동작 선택기를를 `DbContext` [비 추적](xref:core/querying/tracking#no-tracking-queries) 기본적으로:</span><span class="sxs-lookup"><span data-stu-id="46c1e-116">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="46c1e-117">공급자 선택기 메서드와 위에서 언급 한 다른 동작 선택기 메서드에 확장 메서드 `DbContextOptions` 또는 공급자별 옵션 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="46c1e-117">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="46c1e-118">네임 스페이스를 포함 해야 합니다. 이러한 확장 메서드에 대 한 액세스를 가지려면 (일반적으로 `Microsoft.EntityFrameworkCore`)에서 범위 및 프로젝트에 추가 패키지 종속성을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="46c1e-118">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="46c1e-119">`DbContextOptions` 제공할 수 있습니다 합니다 `DbContext` 재정의 하 여는 `OnConfiguring` 메서드 또는 생성자 인수를 통해 외부에서.</span><span class="sxs-lookup"><span data-stu-id="46c1e-119">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="46c1e-120">둘 다 사용 하는 경우 `OnConfiguring` 마지막으로 적용 되 고 생성자 인수를 제공 하는 옵션을 덮어쓸 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="46c1e-120">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="46c1e-121">생성자 인수</span><span class="sxs-lookup"><span data-stu-id="46c1e-121">Constructor argument</span></span>

<span data-ttu-id="46c1e-122">생성자를 사용 하 여 상황에 맞는 코드:</span><span class="sxs-lookup"><span data-stu-id="46c1e-122">Context code with constructor:</span></span>

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
> <span data-ttu-id="46c1e-123">DbContext의 기본 생성자의 비 제네릭 버전 받기도 `DbContextOptions`, 있지만 여러 컨텍스트 형식 사용 하 여 응용 프로그램에 대 한 비 제네릭 버전을 사용 하 여 권장 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="46c1e-123">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="46c1e-124">생성자 인수에서 초기화 하는 응용 프로그램 코드:</span><span class="sxs-lookup"><span data-stu-id="46c1e-124">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="46c1e-125">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="46c1e-125">OnConfiguring</span></span>

<span data-ttu-id="46c1e-126">상황에 맞는 코드로 `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="46c1e-126">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="46c1e-127">초기화 하는 응용 프로그램 코드를 `DbContext` 를 사용 하는 `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="46c1e-127">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="46c1e-128">이 방법은 편은 아니지 테스트, 테스트 전체 데이터베이스를 대상으로 하지 않는 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="46c1e-128">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="46c1e-129">종속성 주입을 사용 하 여 DbContext를 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="46c1e-129">Using DbContext with dependency injection</span></span>

<span data-ttu-id="46c1e-130">EF Core를 사용 하 여 지원 `DbContext` 종속성 주입 컨테이너를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="46c1e-130">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="46c1e-131">사용 하 여 서비스 컨테이너에 추가할 수 있습니다 하 여 DbContext 형식을 `AddDbContext<TContext>` 메서드.</span><span class="sxs-lookup"><span data-stu-id="46c1e-131">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="46c1e-132">`AddDbContext<TContext>` 둘 다 하 여 DbContext 형식을 하 게 `TContext`, 및 해당 `DbContextOptions<TContext>` 주입 서비스 컨테이너에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="46c1e-132">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="46c1e-133">참조 [더 많은 읽기](#more-reading) 아래 종속성 주입에 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="46c1e-133">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="46c1e-134">추가 된 `Dbcontext` 종속성 주입에:</span><span class="sxs-lookup"><span data-stu-id="46c1e-134">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="46c1e-135">이렇게 하려면 추가 [생성자 인수](#constructor-argument) 를 허용 하는 DbContext 형식 `DbContextOptions<TContext>`합니다.</span><span class="sxs-lookup"><span data-stu-id="46c1e-135">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="46c1e-136">상황에 맞는 코드:</span><span class="sxs-lookup"><span data-stu-id="46c1e-136">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="46c1e-137">응용 프로그램 코드 (ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="46c1e-137">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="46c1e-138">응용 프로그램 코드 (ServiceProvider를 덜 일반적을 직접 사용):</span><span class="sxs-lookup"><span data-stu-id="46c1e-138">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a><span data-ttu-id="46c1e-139">더 많은 읽기</span><span class="sxs-lookup"><span data-stu-id="46c1e-139">More reading</span></span>

* <span data-ttu-id="46c1e-140">읽기 [ASP.NET Core에서 시작](../get-started/aspnetcore/index.md) EF를 사용 하 여 ASP.NET Core를 사용 하 여 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="46c1e-140">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="46c1e-141">읽기 [종속성 주입](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) DI를 사용 하는 방법에 대 한 자세한 내용을 보려면.</span><span class="sxs-lookup"><span data-stu-id="46c1e-141">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="46c1e-142">읽기 [테스트](testing/index.md) 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="46c1e-142">Read [Testing](testing/index.md) for more information.</span></span>
