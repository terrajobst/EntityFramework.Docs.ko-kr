---
title: 디자인 타임 DbContext 생성-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: c36dae150085b1ab509288f6fabfdd8ed7201ca8
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812024"
---
# <a name="design-time-dbcontext-creation"></a>디자인 타임 DbContext 만들기

응용 프로그램의 엔터티 형식에 대 한 세부 정보를 수집 하 고 데이터베이스 스키마에 매핑하는 방법에 대 한 세부 정보를 수집 하기 위해 일부 EF Core 도구 명령 (예: [마이그레이션][1] 명령)에는 디자인 타임에 파생 된 `DbContext` 인스턴스를 만들어야 합니다. 대부분의 경우 생성 되는 `DbContext`을 [런타임에 구성][2]하는 방법과 비슷한 방식으로 구성 하는 것이 좋습니다.

도구에서 `DbContext`를 만드는 데는 여러 가지 방법이 있습니다.

## <a name="from-application-services"></a>응용 프로그램 서비스에서

시작 프로젝트가 [ASP.NET Core 웹 호스트][3] 또는 [.Net Core 일반 호스트][4]를 사용 하는 경우 도구는 응용 프로그램의 서비스 공급자에서 DbContext 개체를 가져오려고 시도 합니다.

도구는 먼저 `Program.CreateHostBuilder()`를 호출 하 고 `Build()`를 호출한 다음 `Services` 속성에 액세스 하 여 서비스 공급자를 가져오려고 시도 합니다.

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
> 새 ASP.NET Core 응용 프로그램을 만드는 경우이 후크는 기본적으로 포함 되어 있습니다.

`DbContext` 자체와 해당 생성자의 모든 종속성을 응용 프로그램의 서비스 공급자에 서비스로 등록 해야 합니다. 이는 `DbContextOptions<TContext>`의 인스턴스를 인수로 사용 하 고 [`AddDbContext<TContext>` 메서드][6]를 사용 하 [는 `DbContext`에 생성자를][5] 사용 하 여 쉽게 달성할 수 있습니다.

## <a name="using-a-constructor-with-no-parameters"></a>매개 변수 없이 생성자 사용

응용 프로그램 서비스 공급자에서 DbContext를 가져올 수 없는 경우 도구는 프로젝트 내에서 파생 `DbContext` 형식을 찾습니다. 그런 다음 매개 변수 없이 생성자를 사용 하 여 인스턴스를 만들려고 시도 합니다. `DbContext` [`OnConfiguring`][7] 메서드를 사용 하 여 구성 된 경우이 생성자가 기본 생성자가 될 수 있습니다.

## <a name="from-a-design-time-factory"></a>디자인 타임 팩터리에서

`IDesignTimeDbContextFactory<TContext>` 인터페이스를 구현 하 여 도구에 DbContext를 만드는 방법을 지시할 수도 있습니다 .이 인터페이스를 구현 하는 클래스가 파생 된 `DbContext`와 동일한 프로젝트 또는 응용 프로그램의 시작 프로젝트에 있는 경우 도구에서 다른 도구를 사용 하지 않습니다. DbContext를 만들고 디자인 타임 팩터리를 대신 사용 하는 방법

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
> `args` 매개 변수는 현재 사용 되지 않습니다. 도구에서 디자인 타임 인수를 지정 하는 기능을 추적 하는 [문제가][8] 있습니다.

디자인 타임 팩터리는 런타임에 `DbContext` 생성자가 추가 매개 변수를 사용 하지 않거나 DI에 등록 되어 있지 않거나, DI를 전혀 사용 하지 않는 경우, 아니면 어떤 이유로 든 지는 경우에 특히 유용할 수 있습니다. ASP.NET Core 응용 프로그램의 `Main` 클래스에 `BuildWebHost` 메서드를 사용 하지 않는 것이 좋습니다.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
