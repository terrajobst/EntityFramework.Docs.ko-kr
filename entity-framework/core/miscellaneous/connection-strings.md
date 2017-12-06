---
title: "연결 문자열-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: b4ed01f0452d74ac49d3fde780caa5f1b25a6e97
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="connection-strings"></a>연결 문자열

대부분의 데이터베이스 공급자 특정 형태의 연결 문자열을 데이터베이스에 연결 해야 합니다. 이 연결 문자열 보호 해야 하는 중요 한 정보를 포함 하는 경우도 있습니다. 와 같은 개발, 테스트 및 프로덕션 환경 간에 응용 프로그램을 이동 하면 연결 문자열을 변경 해야 할 수도 있습니다.

## <a name="net-framework-applications"></a>.NET framework 응용 프로그램

WinForms, WPF, 콘솔 및 ASP.NET 4와 같은.NET framework 응용 프로그램 테스트를 거친 연결 문자열 패턴이 있어야 합니다. 연결 문자열은 응용 프로그램 App.config 파일 (Web.config ASP.NET을 사용 하는 경우)에 추가 되어야 합니다. 사용자 이름 및 암호와 같은 중요 한 정보를 포함 하는 연결 문자열을 사용 하 여 구성 파일의 콘텐츠를 보호할 수 있습니다 [보호 되는 구성](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration)합니다.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>

  <connectionStrings>
    <add name="BloggingDatabase"
         connectionString="Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" />
  </connectionStrings>
</configuration>
```

> [!TIP]  
> `providerName` 설정은 데이터베이스 공급자가 코드를 통해 구성 때문에 App.config에 저장 된 EF 코어 연결 문자열에 필요 하지 않습니다.

그런 다음 사용 하 여 연결 문자열을 읽을 수 있습니다는 `ConfigurationManager` 컨텍스트에 API `OnConfiguring` 메서드. 에 대 한 참조를 추가 해야 할 수는 `System.Configuration` 프레임 워크 어셈블리를이 API를 사용할 수 있습니다.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="universal-windows-platform-uwp"></a>UWP(유니버설 Windows 플랫폼)

UWP 응용 프로그램에서 연결 문자열은 일반적으로 로컬 파일 이름을 지정 하는 SQLite 연결 합니다. 일반적으로 중요 한 정보를 포함 하지 않으며 응용 프로그램이 배포 되는 서 변경 될 필요가 없습니다. 따라서 이러한 연결 문자열 만족 하는 일반적으로 코드에 남겨둘 다음과 같이 합니다. 코드 밖으로 이동 하려는 경우 다음 UWP 설정의 개념, 지원 참조는 [UWP 설명서의 응용 프로그램 설정 섹션](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) 대 한 자세한 내용은 합니다.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
            optionsBuilder.UseSqlite("Data Source=blogging.db");
    }
}
```

## <a name="aspnet-core"></a>ASP.NET Core

ASP.NET Core 구성 시스템은 매우 유연 하 고 연결 문자열에 저장할 수 `appsettings.json`, 환경 변수, 사용자 암호 저장소 또는 다른 구성 소스입니다. 참조는 [ASP.NET Core 설명서의 구성 섹션](https://docs.asp.net/en/latest/fundamentals/configuration.html) 내용을 확인 합니다. 다음 예제에서는 연결 문자열에 저장 된 `appsettings.json`합니다.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

컨텍스트는 일반적으로 구성 `Startup.cs` 구성에서 읽고 있는 연결 문자열을 사용 합니다. 참고는 `GetConnectionString()` 메서드 키가 구성 값에 대 한 찾습니다 `ConnectionStrings:<connection string name>`합니다.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
