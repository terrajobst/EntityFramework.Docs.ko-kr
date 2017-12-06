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
# <a name="configuring-a-dbcontext"></a>DbContext를 구성합니다.

이 문서에서는 구성에 대 한 패턴을 `DbContext` 와 `DbContextOptions`합니다. 옵션은 선택 하 고 데이터 저장소 구성에서 주로 사용 됩니다.

## <a name="configuring-dbcontextoptions"></a>DbContextOptions 구성

`DbContext`인스턴스가 있어야 `DbContextOptions` 실행할 수 있도록 합니다. 이 재정의 하 여 구성할 수 있습니다 `OnConfiguring`, 또는 생성자 인수를 통해 외부적으로 제공 합니다.

모두 사용 하는 경우 `OnConfiguring` 이므로 덧셈 및 제공 된 옵션에서 실행 되는 생성자 인수를 제공 하는 옵션을 덮어씁니다.

### <a name="constructor-argument"></a>생성자 인수

생성자를 사용 하 여 컨텍스트 코드

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
> DbContext의 기본 생성자는 또한의 비 제네릭 버전 허용 `DbContextOptions`합니다. 비 제네릭 버전을 사용 하 여 여러 컨텍스트 유형으로 응용 프로그램에 대 한 권장 되지 않습니다.

생성자 인수에서 초기화 하는 응용 프로그램 코드

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

> [!WARNING]  
> `OnConfiguring`마지막 발생 하 고 DI 또는 생성자에서 가져온 옵션을 덮어쓸 수 있습니다. 이 방법은 (없는 경우 전체 데이터베이스를 대상)를 테스트 자체을 충분 하지 않습니다.

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

으로 초기화 하려면 응용 프로그램 코드 `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a>종속성 주입 DbContext 사용

EF를 사용 하 여 원하는 `DbContext` 종속성 주입 컨테이너를 사용 합니다. 사용 하 여 서비스 컨테이너에 추가할 수 DbContext 형식 `AddDbContext<TContext>`합니다.

`AddDbContext`두 프로그램 DbContext 형식 하면 `TContext`, 및 `DbContextOptions<TContext>` 서비스 컨테이너에서 삽입에 사용할 수 있습니다.

참조 [더 많은 읽기](#more-reading) 아래 종속성 주입에 대 한 내용은 합니다.

종속성 주입에 dbcontext를 추가합니다.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

이 위해서는 추가 [생성자 인수](#constructor-argument) 를 허용 하 DbContext 형식으로 `DbContextOptions`합니다.

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
public MyController(BloggingContext context)
```

응용 프로그램 코드 (ServiceProvider를 코드도 직접 사용):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a>사용 하 여`IDesignTimeDbContextFactory<TContext>`

위의 옵션 대신, 있습니다 수 구현도 제공의 `IDesignTimeDbContextFactory<TContext>`합니다. EF 도구가이 팩터리에서 사용 프로그램 DbContext의 인스턴스를 만들 수 있습니다. 이 마이그레이션 등 특정 디자인 타임 환경을 제공 하기 위해 필요할 수 있습니다.

기본 public 생성자가 없는 컨텍스트 형식에 대 한 디자인 타임 서비스를 사용 하도록 설정 하려면이 인터페이스를 구현 합니다. 디자인 타임 서비스를이 파생 컨텍스트로 동일한 어셈블리에 있는이 인터페이스의 구현 자동으로 검색 합니다.

예제:

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

## <a name="more-reading"></a>더 많은 읽기

* 읽기 [ASP.NET 코어에서 시작](../get-started/aspnetcore/index.md) EF를 사용 하 여 ASP.NET 코어 대 한 자세한 내용은 합니다.
* 읽기 [종속성 주입](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) DI를 사용 하는 방법에 대 한 자세한 내용을 보려면 합니다.
* 읽기 [테스트](testing/index.md) 자세한 정보에 대 한 합니다.
