---
title: 연결 문자열-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: ed89d6d09b15b0dea7fd8bc3ff3e3f631495ecb7
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149109"
---
# <a name="connection-strings"></a>연결 문자열

대부분의 데이터베이스 공급자는 특정 형태의 데이터베이스에 연결할 연결 문자열을 요구합니다. 이 연결 문자열은 보호해야 하는 중요한 정보를 포함하는 경우도 있습니다. 응용 프로그램 개발, 테스트 및 프로덕션 등 환경 간에 이동하면 연결 문자열을 변경해야 합니다.

## <a name="winforms--wpf-applications"></a>WinForms & WPF 응용 프로그램

WinForms, WPF 및 ASP.NET 4 응용 프로그램은 연결 문자열 패턴이 시도 되었고 테스트를 거쳤습니다. 연결 문자열은 응용 프로그램의 App.config 파일 (ASP.NET를 사용 하는 경우 web.config)에 추가 되어야 합니다. 연결 문자열에 사용자 이름 및 암호와 같은 중요 한 정보가 포함 되어 있는 경우 [암호 관리자 도구](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)를 사용 하 여 구성 파일의 내용을 보호할 수 있습니다.

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
> `providerName` 설정을 데이터베이스 공급자 코드를 통해 구성했기 때문에 App.config에 저장된 EF 코어 연결 문자열에 필요하지 않습니다.

그런 다음 컨텍스트의 `OnConfiguring` 메서드에서 `ConfigurationManager` API를 사용하여 연결 문자열을 읽을 수 있습니다. 이 API를 사용하려면 `System.Configuration` 프레임워크 어셈블리에 대한 참조를 추가해야 할 수 있습니다.

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

UWP 응용 프로그램의 연결 문자열은 일반적으로 로컬 파일 이름만 지정하는 SQLite 연결입니다. 일반적으로 민감한 정보는 포함되어 있지 않으므로 응용 프로그램이 배포될 때 변경하지 않아도 됩니다. 따라서 이러한 연결 문자열은 일반적으로 아래와 같이 코드에 남게 됩니다. 코드를 코드 밖으로 이동하려면 UWP가 설정 개념을 지원합니다. 자세한 내용은 [UWP 설명서의 앱 설정 섹션](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)을 참조하십시오.

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

ASP.NET Core에서 구성 시스템은 매우 유연하며 연결 문자열은 `appsettings.json`, 환경 변수, 사용자 비밀 저장소 또는 다른 구성 소스에 저장될 수 있습니다. 자세한 내용은 [ASP.NET 설명서의 구성 섹션](https://docs.asp.net/en/latest/fundamentals/configuration.html)을 참조하십시오. 다음 예는 `appsettings.json`에 저장된 연결 문자열을 보여줍니다.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

컨텍스트는 일반적으로 구성에서 읽는 연결 문자열로 `Startup.cs`에 구성됩니다. `GetConnectionString()` 메서드는 키가 `ConnectionStrings:<connection string name>`인 구성 값을 찾습니다. 이 확장 메서드를 사용 하려면 [Microsoft Extensions. 구성](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) 네임 스페이스를 가져와야 합니다.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
