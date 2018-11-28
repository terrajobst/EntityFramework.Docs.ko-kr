---
title: DbContext-EF Core 구성
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: f5a9ae17471391442170d8c40264e4db6922cb08
ms.sourcegitcommit: 39080d38e1adea90db741257e60dc0e7ed08aa82
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "50980004"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="2a47d-102">DbContext 구성</span><span class="sxs-lookup"><span data-stu-id="2a47d-102">Configuring a DbContext</span></span>

<span data-ttu-id="2a47d-103">이 문서에서는 특정 EF Core 공급자 및 선택적 동작을 사용하여 데이터베이스에 연결하기 위해 `DbContextOptions`를 통해 `DbContext`를 구성하는 기본 패턴을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="2a47d-104">디자인 타임 DbContext 구성</span><span class="sxs-lookup"><span data-stu-id="2a47d-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="2a47d-105">[마이그레이션](xref:core/managing-schemas/migrations/index)과 같은 EF Core 디자인 타임 도구는 응용 프로그램의 엔터티 형식과 데이터베이스 스키마에 매핑하는 방법에 대한 세부 정보를 수집하기 위해 `DbContext` 형식의 작동 중인 인스턴스를 찾고 만들 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="2a47d-106">이 프로세스는 런타임에 구성되는 방식과 유사하게 구성되는 방식으로 `DbContext`를 쉽게 만들 수 있는 한 자동으로 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="2a47d-107">`DbContext`에 필요한 설정 정보를 제공하는 패턴은 런타임에 작동할 수 있지만, 디자인 타임에 `DbContext`를 사용해야 하는 툴은 제한된 수의 패턴으로만 작동할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="2a47d-108">이 내용은 [디자인 타임 DbContext 만들기](xref:core/miscellaneous/cli/dbcontext-creation) 섹션에서 보다 자세히 다루고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="2a47d-109">DbContextOptions 구성</span><span class="sxs-lookup"><span data-stu-id="2a47d-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="2a47d-110">`DbContext` 인스턴스가 있어야 `DbContextOptions` 작업을 수행 하기 위해.</span><span class="sxs-lookup"><span data-stu-id="2a47d-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="2a47d-111">`DbContextOptions` 인스턴스와 같은 구성 정보를 전달 합니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="2a47d-112">데이터베이스 공급자를 사용 하려면 일반적으로 같은 메서드를 호출 하 여 선택한 `UseSqlServer` 또는 `UseSqlite`합니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="2a47d-113">이러한 확장 메서드는 해당 공급자 패키지를 같은 필요 `Microsoft.EntityFrameworkCore.SqlServer` 또는 `Microsoft.EntityFrameworkCore.Sqlite`합니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="2a47d-114">에 정의 된 메서드는 `Microsoft.EntityFrameworkCore` 네임 스페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="2a47d-115">모든 필수 연결 문자열이 나 데이터베이스 인스턴스의 식별자입니다. 일반적으로 인수로 전달 위에서 언급 한 공급자 선택 방법</span><span class="sxs-lookup"><span data-stu-id="2a47d-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="2a47d-116">일반적으로 연결 된 공급자 선택 방법에 대 한 호출 내에서 모든 선택적 동작 공급자 수준의 선택기</span><span class="sxs-lookup"><span data-stu-id="2a47d-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="2a47d-117">일반적으로 공급자 선택기 메서드 전이나 후 연결 된 모든 일반 EF Core 동작 선택기</span><span class="sxs-lookup"><span data-stu-id="2a47d-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="2a47d-118">다음 예제에서는 구성 합니다 `DbContextOptions` SQL Server 공급자를 사용 하려면 연결에 포함 합니다 `connectionString` 변수와 공급자 수준의 명령 제한 시간을 실행 하는 모든 쿼리는 EF Core 동작 선택기를를 `DbContext` [비 추적](xref:core/querying/tracking#no-tracking-queries) 기본적으로:</span><span class="sxs-lookup"><span data-stu-id="2a47d-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="2a47d-119">공급자 선택기 메서드와 위에서 언급 한 다른 동작 선택기 메서드에 확장 메서드 `DbContextOptions` 또는 공급자별 옵션 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="2a47d-120">네임 스페이스를 포함 해야 합니다. 이러한 확장 메서드에 대 한 액세스를 가지려면 (일반적으로 `Microsoft.EntityFrameworkCore`)에서 범위 및 프로젝트에 추가 패키지 종속성을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="2a47d-121">`DbContextOptions`는 `OnConfiguring` 메서드를 재정의하거나 또는 외부에서 생성자 인수를 통해 `DbContext`에 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="2a47d-122">둘 다 사용되면 `OnConfiguring`이 마지막에 적용되며 생성자 인수에 제공된 옵션을 덮어쓸 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="2a47d-123">생성자 인수</span><span class="sxs-lookup"><span data-stu-id="2a47d-123">Constructor argument</span></span>

<span data-ttu-id="2a47d-124">생성자를 사용한 컨텍스트 코드:</span><span class="sxs-lookup"><span data-stu-id="2a47d-124">Context code with constructor:</span></span>

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
> <span data-ttu-id="2a47d-125">DbContext의 기본 생성자도 `DbContextOptions`의 비 제네릭 버전을 허용하지만 비 제네릭 버전을 사용하는 것은 다중 컨텍스트 유형이 있는 응용 프로그램에는 권장되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="2a47d-126">생성자 인수에서 초기화 하는 응용 프로그램 코드:</span><span class="sxs-lookup"><span data-stu-id="2a47d-126">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="2a47d-127">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="2a47d-127">OnConfiguring</span></span>

<span data-ttu-id="2a47d-128">`OnConfiguring`을 사용한 컨텍스트 코드:</span><span class="sxs-lookup"><span data-stu-id="2a47d-128">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="2a47d-129">`OnConfiguring`을 사용하여 `DbContext`를 초기화하는 응용 프로그램 코드 :</span><span class="sxs-lookup"><span data-stu-id="2a47d-129">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="2a47d-130">이 방법은 테스트가 전체 데이터베이스를 대상으로하지 않는 한 테스트에 적합하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-130">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="2a47d-131">종속성 주입과 함께 DbContext를 사용하기</span><span class="sxs-lookup"><span data-stu-id="2a47d-131">Using DbContext with dependency injection</span></span>

<span data-ttu-id="2a47d-132">EF Core는 종속성 주입 컨테이너로 `DbContext`를 사용하는 것을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-132">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="2a47d-133">DbContext 형식은 `AddDbContext<TContext>` 메서드를 사용하여 서비스 컨테이너에 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-133">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="2a47d-134">`AddDbContext<TContext>`는 DbContext 형식인 `TContext` 및 해당 `DbContextOptions<TContext>`를 서비스 컨테이너에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-134">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="2a47d-135">종속성 주입에 대한 추가 정보는 아래의 [더 많은 읽기](#more-reading) 자료를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="2a47d-135">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="2a47d-136">종속성 주입에 `Dbcontext` 추가:</span><span class="sxs-lookup"><span data-stu-id="2a47d-136">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="2a47d-137">이렇게 하려면 `DbContextOptions<TContext>`를 허용하는 DbContext 형식에 [생성자 인수](#constructor-argument)를 추가해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2a47d-137">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="2a47d-138">컨텍스트 코드:</span><span class="sxs-lookup"><span data-stu-id="2a47d-138">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="2a47d-139">응용 프로그램 코드 (ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="2a47d-139">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="2a47d-140">응용 프로그램 코드(직접 ServiceProvider 사용, 덜 일반적):</span><span class="sxs-lookup"><span data-stu-id="2a47d-140">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a><span data-ttu-id="2a47d-141">더 많은 읽기</span><span class="sxs-lookup"><span data-stu-id="2a47d-141">More reading</span></span>

* <span data-ttu-id="2a47d-142">ASP.NET Core에서 EF 사용에 대한 자세한 내용은 [ASP.NET Core 시작하기](../get-started/aspnetcore/index.md)를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="2a47d-142">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="2a47d-143">DI 사용에 관해 더 배우려면 [종속성 주입](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection)을 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="2a47d-143">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="2a47d-144">자세한 내용은 [테스트](testing/index.md)를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="2a47d-144">Read [Testing](testing/index.md) for more information.</span></span>
