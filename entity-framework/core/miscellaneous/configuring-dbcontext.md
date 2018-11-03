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
# <a name="configuring-a-dbcontext"></a>DbContext 구성

이 문서에서는 구성에 대 한 기본 패턴을 보여 줍니다.는 `DbContext` 를 통해를 `DbContextOptions` 특정 EF Core 공급자 및 선택적 동작을 사용 하 여 데이터베이스에 연결 합니다.

## <a name="design-time-dbcontext-configuration"></a>디자인 타임 DbContext 구성

EF Core 디자인 타임 도구와 같은 [마이그레이션을](xref:core/managing-schemas/migrations/index) 검색 하 고 작업의 인스턴스를 만들 수 있어야를 `DbContext` 응용 프로그램의 엔터티 형식 및 데이터베이스 스키마에 매핑하는 방법에 대 한 세부 정보를 수집 하기 위해 형식입니다. 도구를 쉽게 만들 수 있다면이 프로세스는 자동 수는 `DbContext` 런타임 시 구성 하는 방법을에 비슷하게 구성 수는 방식에서입니다.

모든 패턴에 필요한 구성 정보를 제공 하는 동안 합니다 `DbContext` 실행 시간을 사용 해야 하는 도구에서 작업할 수는 `DbContext` 디자인 타임에만 작업할 수 제한 된 수의 패턴입니다. 더 자세히 살펴보시기 합니다 [디자인 타임에 컨텍스트를 만들지](xref:core/miscellaneous/cli/dbcontext-creation) 섹션입니다.

## <a name="configuring-dbcontextoptions"></a>DbContextOptions 구성

`DbContext` 인스턴스가 있어야 `DbContextOptions` 작업을 수행 하기 위해. `DbContextOptions` 인스턴스와 같은 구성 정보를 전달 합니다.

- 데이터베이스 공급자를 사용 하려면 일반적으로 같은 메서드를 호출 하 여 선택한 `UseSqlServer` 또는 `UseSqlite`합니다. 이러한 확장 메서드는 해당 공급자 패키지를 같은 필요 `Microsoft.EntityFrameworkCore.SqlServer` 또는 `Microsoft.EntityFrameworkCore.Sqlite`합니다. 에 정의 된 메서드는 `Microsoft.EntityFrameworkCore` 네임 스페이스입니다.
- 모든 필수 연결 문자열이 나 데이터베이스 인스턴스의 식별자입니다. 일반적으로 인수로 전달 위에서 언급 한 공급자 선택 방법
- 일반적으로 연결 된 공급자 선택 방법에 대 한 호출 내에서 모든 선택적 동작 공급자 수준의 선택기
- 일반적으로 공급자 선택기 메서드 전이나 후 연결 된 모든 일반 EF Core 동작 선택기

다음 예제에서는 구성 합니다 `DbContextOptions` SQL Server 공급자를 사용 하려면 연결에 포함 합니다 `connectionString` 변수와 공급자 수준의 명령 제한 시간을 실행 하는 모든 쿼리는 EF Core 동작 선택기를를 `DbContext` [비 추적](xref:core/querying/tracking#no-tracking-queries) 기본적으로:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> 공급자 선택기 메서드와 위에서 언급 한 다른 동작 선택기 메서드에 확장 메서드 `DbContextOptions` 또는 공급자별 옵션 클래스입니다. 네임 스페이스를 포함 해야 합니다. 이러한 확장 메서드에 대 한 액세스를 가지려면 (일반적으로 `Microsoft.EntityFrameworkCore`)에서 범위 및 프로젝트에 추가 패키지 종속성을 포함 합니다.

`DbContextOptions` 제공할 수 있습니다 합니다 `DbContext` 재정의 하 여는 `OnConfiguring` 메서드 또는 생성자 인수를 통해 외부에서.

둘 다 사용 하는 경우 `OnConfiguring` 마지막으로 적용 되 고 생성자 인수를 제공 하는 옵션을 덮어쓸 수 있습니다.

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
> DbContext의 기본 생성자의 비 제네릭 버전 받기도 `DbContextOptions`, 있지만 여러 컨텍스트 형식 사용 하 여 응용 프로그램에 대 한 비 제네릭 버전을 사용 하 여 권장 되지 않습니다.

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

상황에 맞는 코드로 `OnConfiguring`:

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

초기화 하는 응용 프로그램 코드를 `DbContext` 를 사용 하는 `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> 이 방법은 편은 아니지 테스트, 테스트 전체 데이터베이스를 대상으로 하지 않는 한 합니다.

### <a name="using-dbcontext-with-dependency-injection"></a>종속성 주입을 사용 하 여 DbContext를 사용 하 여

EF Core를 사용 하 여 지원 `DbContext` 종속성 주입 컨테이너를 사용 합니다. 사용 하 여 서비스 컨테이너에 추가할 수 있습니다 하 여 DbContext 형식을 `AddDbContext<TContext>` 메서드.

`AddDbContext<TContext>` 둘 다 하 여 DbContext 형식을 하 게 `TContext`, 및 해당 `DbContextOptions<TContext>` 주입 서비스 컨테이너에서 사용할 수 있습니다.

참조 [더 많은 읽기](#more-reading) 아래 종속성 주입에 대 한 자세한 내용은 합니다.

추가 된 `Dbcontext` 종속성 주입에:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

이렇게 하려면 추가 [생성자 인수](#constructor-argument) 를 허용 하는 DbContext 형식 `DbContextOptions<TContext>`합니다.

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

응용 프로그램 코드 (ServiceProvider를 덜 일반적을 직접 사용):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a>더 많은 읽기

* 읽기 [ASP.NET Core에서 시작](../get-started/aspnetcore/index.md) EF를 사용 하 여 ASP.NET Core를 사용 하 여 대 한 자세한 내용은 합니다.
* 읽기 [종속성 주입](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) DI를 사용 하는 방법에 대 한 자세한 내용을 보려면.
* 읽기 [테스트](testing/index.md) 자세한 내용은 합니다.
