---
title: DbContext 구성-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: ddabf825ef23c2ec07efcde390df7d0cf48db33c
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306505"
---
# <a name="configuring-a-dbcontext"></a>DbContext 구성

이 문서에서는 특정 EF Core 공급자 및 선택적 동작을 사용하여 데이터베이스에 연결하기 위해 `DbContextOptions`를 통해 `DbContext`를 구성하는 기본 패턴을 보여줍니다.

## <a name="design-time-dbcontext-configuration"></a>디자인 타임 DbContext 구성

[마이그레이션](xref:core/managing-schemas/migrations/index)과 같은 EF Core 디자인 타임 도구는 응용 프로그램의 엔터티 형식과 데이터베이스 스키마에 매핑하는 방법에 대한 세부 정보를 수집하기 위해 `DbContext` 형식의 작동 중인 인스턴스를 찾고 만들 수 있어야 합니다. 이 프로세스는 런타임에 구성되는 방식과 유사하게 구성되는 방식으로 `DbContext`를 쉽게 만들 수 있는 한 자동으로 수행할 수 있습니다.

`DbContext`에 필요한 설정 정보를 제공하는 패턴은 런타임에 작동할 수 있지만, 디자인 타임에 `DbContext`를 사용해야 하는 툴은 제한된 수의 패턴으로만 작동할 수 있습니다. 이 내용은 [디자인 타임 DbContext 만들기](xref:core/miscellaneous/cli/dbcontext-creation) 섹션에서 보다 자세히 다루고 있습니다.

## <a name="configuring-dbcontextoptions"></a>DbContextOptions 구성

작업을 수행하려면 `DbContext`에 `DbContextOptions` 인스턴스가 있어야 합니다. `DbContextOptions` 인스턴스는 다음과 같은 구성 정보를 전달합니다.

- 사용할 데이터베이스 공급자입니다. 일반적으로 또는 `UseSqlServer` `UseSqlite`와 같은 메서드를 호출 하 여 선택 합니다. 이러한 확장 메서드에는 또는 `Microsoft.EntityFrameworkCore.SqlServer` `Microsoft.EntityFrameworkCore.Sqlite`와 같은 해당 공급자 패키지가 필요 합니다. 메서드는 `Microsoft.EntityFrameworkCore` 네임 스페이스에서 정의 됩니다.
- 일반적으로 위에서 언급 한 공급자 선택 메서드에 인수로 전달 되는 데이터베이스 인스턴스의 필수 연결 문자열 또는 식별자입니다.
- 공급자 수준 선택적 동작 선택기 (일반적으로 공급자 선택 메서드 호출 내에도 연결)
- 일반적으로 공급자 선택기 메서드에 연결 된 일반적인 EF Core 동작 선택기입니다.

다음 예제에서는 SQL Server 공급자 `DbContextOptions` 를 사용 하도록를 구성 하 고, `connectionString` 변수에 포함 된 연결, 공급자 수준 명령 시간 제한을 사용 하 고, EF Core 동작 선택기를 사용 하 여 `DbContext` 모든 쿼리를 실행 합니다. 기본적으로 [추적 안 함](xref:core/querying/tracking#no-tracking-queries) :

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> 위에서 설명한 공급자 선택기 메서드 및 기타 동작 선택기 메서드는 `DbContextOptions` 또는 공급자 관련 옵션 클래스의 확장 메서드입니다. 이러한 확장 메서드에 액세스하려면 범위에 네임스페이스(일반적으로 `Microsoft.EntityFrameworkCore`)가 있어야 하고 프로젝트에 추가 패키지 종속성이 있어야 합니다.

`DbContextOptions`는 `OnConfiguring` 메서드를 재정의하거나 또는 외부에서 생성자 인수를 통해 `DbContext`에 제공할 수 있습니다.

둘 다 사용되면 `OnConfiguring`이 마지막에 적용되며 생성자 인수에 제공된 옵션을 덮어쓸 수 있습니다.

### <a name="constructor-argument"></a>생성자 인수

생성자를 사용한 컨텍스트 코드:

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
> DbContext의 기본 생성자도 `DbContextOptions`의 비 제네릭 버전을 허용하지만 비 제네릭 버전을 사용하는 것은 다중 컨텍스트 유형이 있는 응용 프로그램에는 권장되지 않습니다.

생성자 인수에서 초기화할 응용 프로그램 코드:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

`OnConfiguring`을 사용한 컨텍스트 코드:

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

`OnConfiguring`을 사용하여 `DbContext`를 초기화하는 응용 프로그램 코드 :

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> 이 방법은 테스트가 전체 데이터베이스를 대상으로하지 않는 한 테스트에 적합하지 않습니다.

### <a name="using-dbcontext-with-dependency-injection"></a>종속성 주입과 함께 DbContext를 사용하기

EF Core는 종속성 주입 컨테이너로 `DbContext`를 사용하는 것을 지원합니다. DbContext 형식은 `AddDbContext<TContext>` 메서드를 사용하여 서비스 컨테이너에 추가할 수 있습니다.

`AddDbContext<TContext>`는 DbContext 형식인 `TContext` 및 해당 `DbContextOptions<TContext>`를 서비스 컨테이너에서 사용할 수 있습니다.

종속성 주입에 대한 추가 정보는 아래의 [더 많은 읽기](#more-reading) 자료를 참조하십시오.

종속성 주입에 `DbContext` 추가:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

이렇게 하려면 `DbContextOptions<TContext>`를 허용하는 DbContext 형식에 [생성자 인수](#constructor-argument)를 추가해야 합니다.

컨텍스트 코드:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

응용 프로그램 코드 (ASP.NET Core):

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

응용 프로그램 코드(직접 ServiceProvider 사용, 덜 일반적):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a>DbContext 스레딩 문제 방지

Entity Framework Core는 동일한 `DbContext` 인스턴스에서 실행 되는 여러 병렬 작업을 지원 하지 않습니다. 여기에는 비동기 쿼리의 병렬 실행과 여러 스레드에서의 명시적 동시 사용이 모두 포함 됩니다. 따라서는 항상 `await` 비동기를 호출 하거나 병렬로 실행 되 `DbContext` 는 작업에 대해 별도의 인스턴스를 사용 합니다.

EF Core에서 `DbContext` 인스턴스를 동시에 사용 하려는 시도를 감지 하면 다음과 같은 메시지가 `InvalidOperationException` 표시 됩니다. 

> 이전 작업이 완료 되기 전에이 컨텍스트에서 시작 된 두 번째 작업입니다. 이 문제는 일반적으로 DbContext의 동일한 인스턴스를 사용 하는 다른 스레드에 의해 발생 하지만 인스턴스 멤버는 스레드로부터 안전 하지 않을 수 있습니다.

동시 액세스가 검색 되지 않는 경우에는 정의 되지 않은 동작, 응용 프로그램 충돌 및 데이터 손상이 발생할 수 있습니다.

실수로 동일한 `DbContext` 인스턴스에서 동시에 액세스할 수 있는 일반적인 실수가 있습니다.

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>동일한 DbContext에서 다른 작업을 시작 하기 전에 비동기 작업의 완료를 기다리지 않습니다.

비동기 메서드를 사용 하면 EF Core를 사용 하 여 비차단 방식으로 데이터베이스에 액세스 하는 작업을 시작할 수 있습니다. 그러나 호출자가 이러한 메서드 중 하나를 완료 하는 것을 기다리지 않고에서 다른 작업 `DbContext`을 수행 하기 위해를 진행 하는 경우의 상태 `DbContext` 는이 될 수 있으며,이 경우에는 손상 될 가능성이 높습니다. 

항상 EF Core 비동기 메서드를 즉시 기다립니다.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>종속성 주입을 통해 여러 스레드 간에 DbContext 인스턴스를 암시적으로 공유

확장 [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) 메서드는 기본적 `DbContext` 으로 범위가 지정 된 [수명](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) 으로 형식을 등록 합니다. 

이 ASP.NET Core는 지정 된 시간에 각 클라이언트 요청을 실행 하는 스레드가 하나 뿐 이며, 각 요청이 별도의 종속성 주입 범위 (따라서 `DbContext` 별도의 인스턴스).

그러나 여러 스레드를 명시적으로 병렬로 실행 하는 코드는 `DbContext` 인스턴스가 동시에 액세스 되지 않도록 해야 합니다.

종속성 주입을 사용 하 여이 작업을 수행 `IServiceScopeFactory` `DbContext` `AddDbContext` 하려면컨텍스트를범위가지정된로등록하고각스레드에대해범위(사용)를만들거나를임시로등록합니다.(의오버로드를사용하여를사용합니다.`ServiceLifetime` 매개 변수).

## <a name="more-reading"></a>추가 정보

* ASP.NET Core에서 EF 사용에 대한 자세한 내용은 [ASP.NET Core 시작하기](../get-started/aspnetcore/index.md)를 참조하십시오.
* DI 사용에 관해 더 배우려면 [종속성 주입](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection)을 참조하십시오.
* 자세한 내용은 [테스트](testing/index.md)를 참조하십시오.
