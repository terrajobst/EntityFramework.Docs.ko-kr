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

이 문서에서는 특정 EF Core 공급자 및 선택적 동작을 사용하여 데이터베이스에 연결하기 위해 `DbContextOptions`를 통해 `DbContext`를 구성하는 기본 패턴을 보여줍니다.

## <a name="design-time-dbcontext-configuration"></a>디자인 타임 DbContext 구성

[마이그레이션](xref:core/managing-schemas/migrations/index)과 같은 EF Core 디자인 타임 도구는 응용 프로그램의 엔터티 형식과 데이터베이스 스키마에 매핑하는 방법에 대한 세부 정보를 수집하기 위해 `DbContext` 형식의 작동중인 인스턴스를 찾고 만들 수 있어야 합니다. 이 프로세스는 런타임에 구성되는 방식과 유사하게 구성되는 방식으로 `DbContext`를 쉽게 만들 수 있는 한 자동으로 수행 할 수 있습니다.

`DbContext`에 필요한 설정 정보를 제공하는 패턴은 런타임에 작동 할 수 있지만, 디자인 타임에 `DbContext`를 사용해야하는 툴은 제한된 수의 패턴으로만 작동 할 수 있습니다. 이것들은 [디자인 타임 DbContext 만들기](xref:core/miscellaneous/cli/dbcontext-creation) 섹션에서 보다 자세히 다루고 있습니다.

## <a name="configuring-dbcontextoptions"></a>DbContextOptions 구성

작업을 수행하려면 `DbContext`에 `DbContextOptions` 인스턴스가 있어야합니다. DbContextOptions 인스턴스는 다음과 같은 구성 정보를 전달합니다.

데이터베이스 공급자를 사용하려면 일반적으로 `UseSqlServer` 또는 `UseSqlite`와 같은 메서드를 호출하여 선택합니다. 이러한 확장 메서드들은 `Microsoft.EntityFrameworkCore.SqlServer` 또는 `Microsoft.EntityFrameworkCore.Sqlite`와 같은 해당 공급자 패키지가 필요합니다. 이 메서드들은 `Microsoft.EntityFrameworkCore` 네임스페이스에 정의되어 있습니다.
- 모든 필수 연결 문자열이나 데이터베이스 인스턴스의 식별자입니다. 일반적으로 위에서 언급한 공급자 선택 메서드에 인수로 전달됩니다.
- 일반적으로 공급자 선택 메서드에 대한 호출 내에서 체인화 된 모든 공급자 수준의 선택적 동작 선택기
- 일반적으로 공급자 선택기 메서드 이후 또는 이전에 연결된 모든 일반 EF Core 동작 선택기

다음 예제에서는 SQL Server 공급자, `connectionString` 변수에 포함된 데이터베이스 연결 문자열, 공급자 수준의 명령 제한 시간 및 기본적으로 실행되는 모든 쿼리를 `DbContext` [비 추적](xref:core/querying/tracking#no-tracking-queries)에서 수행하는 EF Core 동작 선택기를 사용하도록 `DbContextOptions`를 구성합니다.

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> 위에서 설명한 공급자 선택기 메서드 및 기타 동작 선택기 메서드는 `DbContextOptions` 또는 공급자 관련 옵션 클래스의 확장 메서드입니다. 이러한 확장 메서드에 액세스하려면 범위에 네임스페이스(일반적으로 `Microsoft.EntityFrameworkCore`)가 있어야하고 프로젝트에 추가 패키지 종속성이 있어야합니다.

`DbContextOptions`는 `OnConfiguring` 메서드를 재정의하거나 또는 외부에서 생성자 인수를 통해 `DbContext`에 제공할 수 있습니다.

둘 다 사용되면 `OnConfiguring`이 마지막에 적용되며 생성자 인수에 제공된 옵션을 덮어 쓸 수 있습니다.

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
> DbContext의 기본 생성자도 `DbContextOptions`의 비 제네릭 버전을 허용하지만 비 제네릭 버전을 사용하는 것은 다중 컨텍스트 유형이있는 응용 프로그램에는 권장되지 않습니다.

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

EF Core는 종속성 주입 컨테이너로 `DbContext`를 사용하는 것을 지원합니다. DbContext 형식은 `AddDbContext<TContext>` 메서드를 사용하여 서비스 컨테이너에 추가 할 수 있습니다.

`AddDbContext<TContext>`는 DbContext 형식인 `TContext` 및 해당 `DbContextOptions<TContext>`를 서비스 컨테이너에서 사용할 수 있습니다.

종속성 주입에 대한 추가 정보는 아래의 [더 많은 읽기[(#more-reading) 자료를 참조하십시오.

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

응용 프로그램 코드 (직접 ServiceProvider 사용, 덜 일반적):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a>더 많은 읽기

* ASP.NET Core에서 EF 사용에 대한 자세한 내용은 [ASP.NET Core 시작하기](../get-started/aspnetcore/index.md)를 참조하십시오.
* DI 사용에 관해 더 배우려면 [종속성 주입](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection)을 참조하십시오.
* 자세한 내용은 [테스트](testing/index.md)를 참조하십시오. 
