---
title: 디자인 타임 DbContext 생성-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f83d4b16227d114a1cac1514667484a908fea4ac
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197569"
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="962fe-102">디자인 타임 DbContext 만들기</span><span class="sxs-lookup"><span data-stu-id="962fe-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="962fe-103">응용 프로그램의 엔터티 형식에 대한 세부 정보를 수집하고 데이터베이스 스키마에 매핑하는 방법에 대한 세부 정보를 수집하기 위해 일부 EF Core 도구 명령(예: [마이그레이션][1] 명령)에는 디자인 타임에 파생 `DbContext` 인스턴스를 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="962fe-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="962fe-104">대부분의 경우 생성 되는는 `DbContext` [런타임에 구성][2]되는 방법과 비슷한 방식으로 구성 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="962fe-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="962fe-105">도구에서를 `DbContext`만드는 데는 여러 가지 방법이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="962fe-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="962fe-106">응용 프로그램 서비스에서</span><span class="sxs-lookup"><span data-stu-id="962fe-106">From application services</span></span>
-------------------------
<span data-ttu-id="962fe-107">시작 프로젝트가 [ASP.NET Core 웹 호스트][3] 또는 [.Net Core 일반 호스트][4]를 사용 하는 경우 도구는 응용 프로그램의 서비스 공급자에서 DbContext 개체를 가져오려고 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="962fe-107">If your startup project uses the [ASP.NET Core Web Host][3] or [.NET Core Generic Host][4], the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="962fe-108">도구는 먼저 `Program.CreateHostBuilder()`를 호출 하 고를 호출한 `Build()`다음 `Services` 속성에 액세스 하 여 서비스 공급자를 가져오려고 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="962fe-108">The tools first try to obtain the service provider by invoking `Program.CreateHostBuilder()`, calling `Build()`, then accessing the `Services` property.</span></span>

``` csharp
public class Program
{
    public static void Main(string[] args)
        => CreateHostBuilder(args).Build().Run();

    // EF Core uses this method at design time to access the DbContext
    public static IHostBuilder CreateHostBuilder(string[] args)
        => Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(
                webBuilder => webBuilder.UseStartup<Startup>());
}

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
        => services.AddDbContext<ApplicationDbContext>();
}

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

> [!NOTE]
> <span data-ttu-id="962fe-109">새 ASP.NET Core 응용 프로그램을 만드는 경우이 후크는 기본적으로 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="962fe-109">When you create a new ASP.NET Core application, this hook is included by default.</span></span>

<span data-ttu-id="962fe-110">`DbContext` 자체와 해당 생성자의 모든 종속성을 응용 프로그램의 서비스 공급자에 서비스로 등록 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="962fe-110">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="962fe-111">이 작업 [은 `DbContext` 의 `DbContextOptions<TContext>` 인스턴스를 인수로][5] 사용 하 고 [ `AddDbContext<TContext>` 메서드][6]를 사용 하는에 생성자를 사용 하 여 쉽게 달성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="962fe-111">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][5] and using the [`AddDbContext<TContext>` method][6].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="962fe-112">매개 변수 없이 생성자 사용</span><span class="sxs-lookup"><span data-stu-id="962fe-112">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="962fe-113">응용 프로그램 서비스 공급자에서 DbContext를 가져올 수 없는 경우 도구는 프로젝트 내에서 파생 `DbContext` 된 형식을 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="962fe-113">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="962fe-114">그런 다음 매개 변수 없이 생성자를 사용 하 여 인스턴스를 만들려고 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="962fe-114">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="962fe-115">`DbContext` 가 메서드를 [`OnConfiguring`][7] 사용 하 여 구성 된 경우이 생성자는 기본 생성자 일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="962fe-115">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][7] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="962fe-116">디자인 타임 팩터리에서</span><span class="sxs-lookup"><span data-stu-id="962fe-116">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="962fe-117">인터페이스를 `IDesignTimeDbContextFactory<TContext>` 구현 하 여 DbContext를 만드는 방법을 도구에 지시할 수도 있습니다. 이 인터페이스를 구현 하는 클래스가 파생 `DbContext` 된 프로젝트 또는 응용 프로그램의 시작 프로젝트에 있는 경우 도구는 DbContext를 만드는 다른 방법을 우회 하 고 대신 디자인 타임 팩터리를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="962fe-117">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
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

> [!NOTE]
> <span data-ttu-id="962fe-118">`args` 매개 변수가 현재 사용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="962fe-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="962fe-119">도구에서 디자인 타임 인수를 지정 하는 기능을 추적 하는 [문제가][8] 있습니다.</span><span class="sxs-lookup"><span data-stu-id="962fe-119">There is [an issue][8] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="962fe-120">디자인 타임 팩터리는 런타임에 보다 디자인 시간에 대해 DbContext를 다르게 구성 해야 하는 경우, `DbContext` 추가 매개 변수가 di에 등록 되지 않거나, di를 사용 하지 않거나, 일부 경우에는를 사용 하는 경우에 특히 유용할 수 있습니다. ASP.NET Core 응용 프로그램의 `BuildWebHost` `Main` 클래스에 메서드를 사용 하지 않는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="962fe-120">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
