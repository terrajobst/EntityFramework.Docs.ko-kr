---
title: DbContext 구성-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 3ab90d46b7a4476044e5ea38eaf04f995708e7bf
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414160"
---
# <a name="configuring-a-dbcontext"></a>DbContext 구성

이 문서에서는 `DbContextOptions`를 통해 `DbContext`를 구성 하 여 특정 EF Core 공급자 및 선택적 동작을 사용 하 여 데이터베이스에 연결 하는 기본적인 패턴을 보여 줍니다.

## <a name="design-time-dbcontext-configuration"></a>디자인 타임 DbContext 구성

[마이그레이션과](xref:core/managing-schemas/migrations/index) 같은 EF Core 디자인 타임 도구는 응용 프로그램의 엔터티 형식에 대 한 세부 정보 및 데이터베이스 스키마에 매핑되는 방법에 대 한 세부 정보를 수집 하기 위해 `DbContext` 형식의 작업 인스턴스를 검색 하 고 만들 수 있어야 합니다. 이 프로세스는 도구에서 런타임에 구성 하는 방식과 비슷하게 구성 될 수 있는 방식으로 `DbContext`를 쉽게 만들 수 있는 한 자동으로 사용할 수 있습니다.

`DbContext`에 필요한 구성 정보를 제공 하는 패턴은 런타임에 작동할 수 있지만 디자인 타임에 `DbContext`를 사용 해야 하는 도구는 제한 된 수의 패턴 에서만 작동할 수 있습니다. 이러한 항목은 [디자인 타임 컨텍스트 생성](xref:core/miscellaneous/cli/dbcontext-creation) 섹션에서 자세히 설명 합니다.

## <a name="configuring-dbcontextoptions"></a>DbContextOptions 구성

작업을 수행 하려면 `DbContext`에 `DbContextOptions` 인스턴스가 있어야 합니다. `DbContextOptions` 인스턴스는 다음과 같은 구성 정보를 전달 합니다.

- 사용할 데이터베이스 공급자입니다. 일반적으로 `UseSqlServer` 또는 `UseSqlite`와 같은 메서드를 호출 하 여 선택 합니다. 이러한 확장 메서드에는 `Microsoft.EntityFrameworkCore.SqlServer` 또는 `Microsoft.EntityFrameworkCore.Sqlite`와 같은 해당 공급자 패키지가 필요 합니다. 메서드는 `Microsoft.EntityFrameworkCore` 네임 스페이스에 정의 됩니다.
- 일반적으로 위에서 언급 한 공급자 선택 메서드에 인수로 전달 되는 데이터베이스 인스턴스의 필수 연결 문자열 또는 식별자입니다.
- 공급자 수준 선택적 동작 선택기 (일반적으로 공급자 선택 메서드 호출 내에도 연결)
- 일반적으로 공급자 선택기 메서드에 연결 된 일반적인 EF Core 동작 선택기입니다.

다음 예에서는 SQL Server 공급자를 사용 하도록 `DbContextOptions`를 구성 하 고, `connectionString` 변수에 포함 된 연결, 공급자 수준 명령 제한 시간 및 EF Core 동작 선택기를 사용 하 여 기본적으로 `DbContext` [추적 되지 않는](xref:core/querying/tracking#no-tracking-queries) 모든 쿼리를 실행 하도록 합니다.

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> 공급자 선택기 메서드 및 위에서 언급 한 기타 동작 선택기 메서드는 `DbContextOptions` 또는 공급자별 옵션 클래스의 확장 메서드입니다. 이러한 확장 메서드에 액세스 하려면 범위에 네임 스페이스 (일반적으로 `Microsoft.EntityFrameworkCore`)를 포함 하 고 프로젝트에 추가 패키지 종속성을 포함 해야 할 수 있습니다.

`OnConfiguring` 메서드를 재정의 하거나 생성자 인수를 통해 외부에서 `DbContextOptions`를 `DbContext`에 제공할 수 있습니다.

둘 다 사용 하는 경우 `OnConfiguring`은 마지막에 적용 되며 생성자 인수에 제공 된 옵션을 덮어쓸 수 있습니다.

### <a name="constructor-argument"></a>생성자 인수

생성자는 다음과 같이 `DbContextOptions`를 허용 하기만 하면 됩니다.

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
> DbContext의 기본 생성자는 제네릭이 아닌 버전의 `DbContextOptions`허용 하지만, 여러 컨텍스트 형식을 가진 응용 프로그램에는 제네릭이 아닌 버전을 사용 하지 않는 것이 좋습니다.

이제 응용 프로그램은 다음과 같이 컨텍스트를 인스턴스화할 때 `DbContextOptions`를 전달할 수 있습니다.

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

컨텍스트 자체 내에서 `DbContextOptions`를 초기화할 수도 있습니다. 기본 구성에이 방법을 사용할 수 있지만 일반적으로 데이터베이스 연결 문자열 등의 외부에서 특정 구성 정보를 가져와야 합니다. 구성 API 또는 다른 방법으로이 작업을 수행할 수 있습니다.

컨텍스트 내에서 `DbContextOptions`를 초기화 하려면 `OnConfiguring` 메서드를 재정의 하 고 제공 된 `DbContextOptionsBuilder`에서 메서드를 호출 합니다.

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

응용 프로그램은 해당 생성자에 아무것도 전달 하지 않고 이러한 컨텍스트를 간단히 인스턴스화할 수 있습니다.

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> 이 방법은 테스트가 전체 데이터베이스를 대상으로하지 않는 한 테스트에 적합하지 않습니다.

### <a name="using-dbcontext-with-dependency-injection"></a>종속성 주입과 함께 DbContext를 사용하기

EF Core는 종속성 주입 컨테이너와 `DbContext` 사용을 지원 합니다. `AddDbContext<TContext>` 메서드를 사용 하 여 DbContext 형식을 서비스 컨테이너에 추가할 수 있습니다.

`AddDbContext<TContext>`는 DbContext 형식, `TContext`및 해당 `DbContextOptions<TContext>`를 서비스 컨테이너에서 주입에 사용할 수 있도록 합니다.

종속성 주입에 대 한 자세한 내용은 아래 [읽기](#more-reading) 를 참조 하세요.

종속성 주입에 `DbContext` 추가:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

이렇게 하려면 `DbContextOptions<TContext>`를 허용 하는 DbContext 형식에 [생성자 인수](#constructor-argument) 를 추가 해야 합니다.

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

Entity Framework Core는 동일한 `DbContext` 인스턴스에서 실행 되는 여러 병렬 작업을 지원 하지 않습니다. 여기에는 비동기 쿼리의 병렬 실행과 여러 스레드에서의 명시적 동시 사용이 모두 포함 됩니다. 따라서는 항상 비동기 호출을 즉시 `await` 하거나 병렬로 실행 되는 작업에 대해 별도의 `DbContext` 인스턴스를 사용 합니다.

EF Core `DbContext` 인스턴스를 동시에 사용 하려는 시도를 감지 하면 다음과 같은 메시지가 포함 된 `InvalidOperationException` 표시 됩니다.

> 이전 작업이 완료 되기 전에이 컨텍스트에서 시작 된 두 번째 작업입니다. 이 문제는 일반적으로 DbContext의 동일한 인스턴스를 사용 하는 다른 스레드에 의해 발생 하지만 인스턴스 멤버는 스레드로부터 안전 하지 않을 수 있습니다.

동시 액세스가 검색 되지 않는 경우에는 정의 되지 않은 동작, 응용 프로그램 충돌 및 데이터 손상이 발생할 수 있습니다.

실수로 동일한 `DbContext` 인스턴스에서 동시에 액세스할 수 있는 일반적인 실수가 있습니다.

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>동일한 DbContext에서 다른 작업을 시작 하기 전에 비동기 작업의 완료를 기다리지 않습니다.

비동기 메서드를 사용 하면 EF Core를 사용 하 여 비차단 방식으로 데이터베이스에 액세스 하는 작업을 시작할 수 있습니다. 그러나 호출자가 이러한 메서드 중 하나를 완료 하는 것을 기다리지 않고 `DbContext`에 대 한 다른 작업을 수행 하는 경우에는 `DbContext` 상태가 손상 될 수 있습니다 (가능성이 매우 높음).

항상 EF Core 비동기 메서드를 즉시 기다립니다.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>종속성 주입을 통해 여러 스레드 간에 DbContext 인스턴스를 암시적으로 공유

[`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) 확장 메서드는 기본적으로 범위가 지정 된 [수명](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) 으로 `DbContext` 형식을 등록 합니다.

이는 지정 된 시간에 각 클라이언트 요청을 실행 하는 스레드가 하나 뿐 이며 각 요청이 별도의 종속성 주입 범위 (따라서 별도의 `DbContext` 인스턴스)를 가져오기 때문에 ASP.NET Core 응용 프로그램의 동시 액세스 문제와는 안전 합니다.

그러나 동시에 여러 스레드를 명시적으로 실행 하는 코드는 `DbContext` 인스턴스가 동시에 액세스 되지 않도록 해야 합니다.

종속성 주입을 사용 하 여이 작업을 수행 하려면 컨텍스트를 범위 지정으로 등록 하 고 각 스레드에 대해 범위 (`IServiceScopeFactory`사용)를 만들거나 `DbContext`를 임시로 등록 하 여 (`ServiceLifetime` 매개 변수를 사용 하는 `AddDbContext`의 오버 로드를 사용 하 여) 이러한 작업을 수행할 수 있습니다.

## <a name="more-reading"></a>추가 정보

- DI를 사용 하는 방법에 대 한 자세한 내용은 [종속성 주입](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) 을 참조 하세요.
- 자세한 내용은 [테스트](testing/index.md) 를 참조 하십시오.
