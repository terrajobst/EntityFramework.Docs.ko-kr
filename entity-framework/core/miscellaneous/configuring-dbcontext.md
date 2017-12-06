---
title: "DbContext-EF 핵심 구성"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 96abf3b94be3e1d19f833644f1c2f6f13fe0e730
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2017
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="7ace6-102">DbContext를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-102">Configuring a DbContext</span></span>

<span data-ttu-id="7ace6-103">이 문서에서는 구성에 대 한 패턴을 `DbContext` 와 `DbContextOptions`합니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-103">This article shows patterns for configuring a `DbContext` with `DbContextOptions`.</span></span> <span data-ttu-id="7ace6-104">옵션은 선택 하 고 데이터 저장소 구성에서 주로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-104">Options are primarily used to select and configure the data store.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="7ace6-105">DbContextOptions 구성</span><span class="sxs-lookup"><span data-stu-id="7ace6-105">Configuring DbContextOptions</span></span>

<span data-ttu-id="7ace6-106">`DbContext`인스턴스가 있어야 `DbContextOptions` 실행할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-106">`DbContext` must have an instance of `DbContextOptions` in order to execute.</span></span> <span data-ttu-id="7ace6-107">이 재정의 하 여 구성할 수 있습니다 `OnConfiguring`, 또는 생성자 인수를 통해 외부적으로 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-107">This can be configured by overriding `OnConfiguring`, or supplied externally via a constructor argument.</span></span>

<span data-ttu-id="7ace6-108">모두 사용 하는 경우 `OnConfiguring` 이므로 덧셈 및 제공 된 옵션에서 실행 되는 생성자 인수를 제공 하는 옵션을 덮어씁니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-108">If both are used, `OnConfiguring` is executed on the supplied options, meaning it is additive and can overwrite  options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="7ace6-109">생성자 인수</span><span class="sxs-lookup"><span data-stu-id="7ace6-109">Constructor argument</span></span>

<span data-ttu-id="7ace6-110">생성자를 사용 하 여 컨텍스트 코드</span><span class="sxs-lookup"><span data-stu-id="7ace6-110">Context code with constructor</span></span>

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
> <span data-ttu-id="7ace6-111">DbContext의 기본 생성자는 또한의 비 제네릭 버전 허용 `DbContextOptions`합니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-111">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`.</span></span> <span data-ttu-id="7ace6-112">비 제네릭 버전을 사용 하 여 여러 컨텍스트 유형으로 응용 프로그램에 대 한 권장 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-112">Using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="7ace6-113">생성자 인수에서 초기화 하는 응용 프로그램 코드</span><span class="sxs-lookup"><span data-stu-id="7ace6-113">Application code to initialize from constructor argument</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="7ace6-114">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="7ace6-114">OnConfiguring</span></span>

> [!WARNING]  
> <span data-ttu-id="7ace6-115">`OnConfiguring`마지막 발생 하 고 DI 또는 생성자에서 가져온 옵션을 덮어쓸 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-115">`OnConfiguring` occurs last and can overwrite options obtained from DI or the constructor.</span></span> <span data-ttu-id="7ace6-116">이 방법은 (없는 경우 전체 데이터베이스를 대상)를 테스트 자체을 충분 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-116">This approach does not lend itself to testing (unless you target the full database).</span></span>

<span data-ttu-id="7ace6-117">사용 하 여 상황에 맞는 코드 `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="7ace6-117">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="7ace6-118">으로 초기화 하려면 응용 프로그램 코드 `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="7ace6-118">Application code to initialize with `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="7ace6-119">종속성 주입 DbContext 사용</span><span class="sxs-lookup"><span data-stu-id="7ace6-119">Using DbContext with dependency injection</span></span>

<span data-ttu-id="7ace6-120">EF를 사용 하 여 원하는 `DbContext` 종속성 주입 컨테이너를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-120">EF supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="7ace6-121">사용 하 여 서비스 컨테이너에 추가할 수 DbContext 형식 `AddDbContext<TContext>`합니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-121">Your DbContext type can be added to the service container by using `AddDbContext<TContext>`.</span></span>

<span data-ttu-id="7ace6-122">`AddDbContext`두 프로그램 DbContext 형식 하면 `TContext`, 및 `DbContextOptions<TContext>` 서비스 컨테이너에서 삽입에 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-122">`AddDbContext` will make both your DbContext type, `TContext`, and `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="7ace6-123">참조 [더 많은 읽기](#more-reading) 아래 종속성 주입에 대 한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-123">See [more reading](#more-reading) below for information on dependency injection.</span></span>

<span data-ttu-id="7ace6-124">종속성 주입에 dbcontext를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-124">Adding dbcontext to dependency injection</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="7ace6-125">이 위해서는 추가 [생성자 인수](#constructor-argument) 를 허용 하 DbContext 형식으로 `DbContextOptions`합니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-125">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions`.</span></span>

<span data-ttu-id="7ace6-126">상황에 맞는 코드:</span><span class="sxs-lookup"><span data-stu-id="7ace6-126">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="7ace6-127">응용 프로그램 코드 (ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="7ace6-127">Application code (in ASP.NET Core):</span></span>

``` csharp
public MyController(BloggingContext context)
```

<span data-ttu-id="7ace6-128">응용 프로그램 코드 (ServiceProvider를 코드도 직접 사용):</span><span class="sxs-lookup"><span data-stu-id="7ace6-128">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a><span data-ttu-id="7ace6-129">사용 하 여`IDesignTimeDbContextFactory<TContext>`</span><span class="sxs-lookup"><span data-stu-id="7ace6-129">Using `IDesignTimeDbContextFactory<TContext>`</span></span>

<span data-ttu-id="7ace6-130">위의 옵션 대신, 있습니다 수 구현도 제공의 `IDesignTimeDbContextFactory<TContext>`합니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-130">As an alternative to the options above, you may also provide an implementation of `IDesignTimeDbContextFactory<TContext>`.</span></span> <span data-ttu-id="7ace6-131">EF 도구가이 팩터리에서 사용 프로그램 DbContext의 인스턴스를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-131">EF tools can use this factory to create an instance of your DbContext.</span></span> <span data-ttu-id="7ace6-132">이 마이그레이션 등 특정 디자인 타임 환경을 제공 하기 위해 필요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-132">This may be required in order to enable specific design-time experiences such as migrations.</span></span>

<span data-ttu-id="7ace6-133">기본 public 생성자가 없는 컨텍스트 형식에 대 한 디자인 타임 서비스를 사용 하도록 설정 하려면이 인터페이스를 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-133">Implement this interface to enable design-time services for context types that do not have a public default constructor.</span></span> <span data-ttu-id="7ace6-134">디자인 타임 서비스를이 파생 컨텍스트로 동일한 어셈블리에 있는이 인터페이스의 구현 자동으로 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-134">Design-time services will automatically discover implementations of this interface that are in the same assembly as the derived context.</span></span>

<span data-ttu-id="7ace6-135">예제:</span><span class="sxs-lookup"><span data-stu-id="7ace6-135">Example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

## <a name="more-reading"></a><span data-ttu-id="7ace6-136">더 많은 읽기</span><span class="sxs-lookup"><span data-stu-id="7ace6-136">More reading</span></span>

* <span data-ttu-id="7ace6-137">읽기 [ASP.NET 코어에서 시작](../get-started/aspnetcore/index.md) EF를 사용 하 여 ASP.NET 코어 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-137">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="7ace6-138">읽기 [종속성 주입](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) DI를 사용 하는 방법에 대 한 자세한 내용을 보려면 합니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-138">Read [Dependency Injection](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) to learn more about using DI.</span></span>
* <span data-ttu-id="7ace6-139">읽기 [테스트](testing/index.md) 자세한 정보에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="7ace6-139">Read [Testing](testing/index.md) for more information.</span></span>
