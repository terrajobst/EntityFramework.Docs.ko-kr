---
title: DbContext-EF 핵심 구성
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
ms.locfileid: "29152392"
---
# <a name="configuring-a-dbcontext"></a>DbContext를 구성합니다.

이 문서에서는 기본 패턴을 구성 하는 데는 `DbContext` 통해는 `DbContextOptions` 특정 EF 핵심 공급자 및 선택적 동작을 사용 하 여 데이터베이스에 연결 하 합니다.

## <a name="design-time-dbcontext-configuration"></a>디자인 타임 DbContext 구성

EF 핵심 디자인 타임와 같은 도구 [마이그레이션](xref:core/managing-schemas/migrations/index) 검색 하 고 작업의 인스턴스를 만들 수 있어야는 `DbContext` 응용 프로그램의 엔터티 형식 및 데이터베이스 스키마에 매핑되는 방식에 대 한 세부 정보를 수집 하려면 쿼리 형식입니다. 이 프로세스도 가능 자동으로 도구를 쉽게 만들 수는 `DbContext` 것은 구성 하는 것 마찬가지로 실행 시 방법을 구성 하는 방식에서입니다.

임의의 패턴에 필요한 구성 정보를 제공 하는 동안는 `DbContext` 런타임에 사용 하 여 해야 하는 도구에서 작업할 수는 `DbContext` 디자인 타임에만 처리할 수 있는 제한 된 수의 패턴입니다. 더 자세하게에서 살펴보시기는 [디자인 타임 컨텍스트를 만들지](xref:core/miscellaneous/cli/dbcontext-creation) 섹션.

## <a name="configuring-dbcontextoptions"></a>DbContextOptions 구성

`DbContext` 인스턴스가 있어야 `DbContextOptions` 하려면 작업을 수행 합니다. `DbContextOptions` 인스턴스와 같은 구성 정보를 전달 합니다.

- 와 같은 메서드를 호출 하 여 데이터베이스 공급자를 사용 하려면 일반적으로 선택 `UseSqlServer` 또는 `UseSqlite`
- 모든 필수 연결 문자열이 나 데이터베이스 인스턴스의 식별자입니다. 일반적으로 인수로 전달 위에서 언급 한 공급자 선택 방법
- 일반적으로 내 공급자 선택 방법에 대 한 호출 또한 연결 된 공급자 수준의 선택적 동작 선택기
- 일반적으로 공급자 선택기 메서드 전이나 후 연결 된 모든 일반 EF 코어 동작 선택기

다음 예제에서는 구성에서 `DbContextOptions` 에 포함 된 연결을 SQL Server 공급자를 사용 하는 `connectionString` 변수와 공급자 수준의 명령 제한 시간에 실행 되는 모든 쿼리는 EF 코어 동작 선택 기가 있는 `DbContext` [아니요 추적](xref:core/querying/tracking#no-tracking-queries) 기본적으로:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> 공급자 선택기 메서드 및 위에서 언급 한 다른 동작 선택기 메서드는 확장명 메서드가 `DbContextOptions` 또는 공급자별 옵션 클래스입니다. 네임 스페이스를 포함 해야 합니다. 이러한 확장 메서드에 대 한 액세스를 가지려면 (일반적으로 `Microsoft.EntityFrameworkCore`)에서 범위가 지정 되 고 프로젝트에 추가 패키지 종속성을 포함 합니다.

`DbContextOptions` 에 제공할 수는 `DbContext` 재정의 하 여는 `OnConfiguring` 메서드 또는 생성자 인수를 통해 외부적으로 합니다.

모두 사용 되 면 `OnConfiguring` 마지막에 적용 하 고는 생성자 인수를 제공 하는 옵션을 덮어쓸 수 있습니다.

### <a name="constructor-argument"></a>생성자 인수

생성자를 사용 하 여 상황에 맞는 코드:

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
> DbContext의 기본 생성자는 또한의 비 제네릭 버전 허용 `DbContextOptions`, 하지만 비 제네릭 버전을 사용 하 여 여러 컨텍스트 유형으로 응용 프로그램에 대 한 권장 되지 않습니다.

생성자 인수에서 초기화 하는 응용 프로그램 코드:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

사용 하 여 상황에 맞는 코드 `OnConfiguring`:

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

초기화 하는 응용 프로그램 코드는 `DbContext` 사용 하 여 `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> 이 접근 방식에 적합 하지 않은 자체 테스트를 테스트 전체 데이터베이스를 대상으로 하지 않는 한 합니다.

### <a name="using-dbcontext-with-dependency-injection"></a>종속성 주입 DbContext 사용

EF 코어를 사용 하 여 원하는 `DbContext` 종속성 주입 컨테이너를 사용 합니다. DbContext 형식을 사용 하 여 서비스 컨테이너에 추가할 수 있습니다는 `AddDbContext<TContext>` 메서드.

`AddDbContext<TContext>` 모두에 DbContext 형식 하면 `TContext`, 및 해당 `DbContextOptions<TContext>` 서비스 컨테이너에서 삽입에 사용할 수 있습니다.

참조 [더 많은 읽기](#more-reading) 아래에 대 한 자세한 내용은 종속성 주입 합니다.

추가 `Dbcontext` 종속성 주입에:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

이 위해서는 추가 [생성자 인수](#constructor-argument) 를 허용 하 DbContext 형식으로 `DbContextOptions<TContext>`합니다.

상황에 맞는 코드:

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

응용 프로그램 코드 (ServiceProvider를 코드도 직접 사용):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a>더 많은 읽기

* 읽기 [ASP.NET 코어에서 시작](../get-started/aspnetcore/index.md) EF를 사용 하 여 ASP.NET 코어 대 한 자세한 내용은 합니다.
* 읽기 [종속성 주입](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) DI를 사용 하는 방법에 대 한 자세한 내용을 보려면 합니다.
* 읽기 [테스트](testing/index.md) 자세한 정보에 대 한 합니다.
