---
title: EF Core 1.0 RC1에서 RC2로 업그레이드-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 5300fe459ec2b8ab9bb573c7284b009249071d65
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306461"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>EF Core 1.0 RC1에서 1.0 RC2로 업그레이드

이 문서에서는 RC1 패키지를 사용 하 여 빌드된 응용 프로그램을 RC2로 이동 하기 위한 지침을 제공 합니다.

## <a name="package-names-and-versions"></a>패키지 이름 및 버전

RC1과 RC2 사이에서 "Entity Framework 7"을 "Entity Framework Core"로 변경 했습니다. [Scott Hanselman이 게시물](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx)을 변경 하는 이유에 대 한 자세한 내용을 읽어볼 수 있습니다. 이러한 변경으로 인해 패키지 `EntityFramework.*` 이름이에서 `Microsoft.EntityFrameworkCore.*` 로 변경 되 고 버전이에서 `7.0.0-rc1-final` (또는 `1.0.0-preview1-final` 도구 `1.0.0-rc2-final` )로 변경 되었습니다.

**RC1 패키지를 완전히 제거한 다음 RC2 패키지를 설치 해야 합니다.** 다음은 몇 가지 일반적인 패키지에 대 한 매핑입니다.

| RC1 패키지                                               | RC2 동급                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final      |
| EntityFramework.SQLite                    7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final      |
| EntityFramework7.Npgsql                   3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres             <to be advised>      |
| EntityFramework.SqlServerCompact35        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final      |
| EntityFramework.InMemory                  7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final      |
| EntityFramework.IBMDataServer             7.0.0-beta1     | RC2에서 아직 사용할 수 없음                                            |
| EntityFramework.Commands                  7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final      |

## <a name="namespaces"></a>네임스페이스

패키지 이름과 함께 네임 스페이스는에서 `Microsoft.Data.Entity.*` 로 `Microsoft.EntityFrameworkCore.*`변경 됩니다. `using Microsoft.Data.Entity` 의`using Microsoft.EntityFrameworkCore`찾기/바꾸기를 사용 하 여이 변경 내용을 처리할 수 있습니다.

## <a name="table-naming-convention-changes"></a>테이블 명명 규칙 변경

RC2에서 수행 하는 중요 한 기능 변경은 클래스 이름이 아니라 지정 된 엔터티의 `DbSet<TEntity>` 속성 이름을 매핑되는 테이블 이름으로 사용 하는 것 이었습니다. [관련 알림 문제](https://github.com/aspnet/Announcements/issues/167)에서이 변경 내용에 대해 자세히 알아볼 수 있습니다.

기존 rc1 응용 프로그램의 경우, `OnModelCreating` 메서드 시작 부분에 다음 코드를 추가 하 여 RC1 명명 전략을 유지 하는 것이 좋습니다.

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

새 명명 전략을 채택 하려는 경우 나머지 업그레이드 단계를 완료 한 다음 코드를 제거 하 고 마이그레이션을 만들어 테이블 이름을 바꾸는 것이 좋습니다.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>Services.adddbcontext/Startup.cs Changes (ASP.NET Core 프로젝트에만 해당)

RC1에서는 응용 프로그램 서비스 공급자 `Startup.ConfigureServices(...)`에 Entity Framework 서비스를 추가 해야 했습니다.

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

RC2에서 `AddEntityFramework()`, `AddSqlServer()`등의 호출을 제거할 수 있습니다.

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

또한 컨텍스트 옵션을 사용 하 여 기본 생성자에 전달 하는 생성자를 파생 컨텍스트에 추가 해야 합니다. 이는 내부적으로 snuck 하는 몇 가지 방법 중 일부를 제거 했기 때문에 필요 합니다.

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>IServiceProvider 전달

를 컨텍스트에 전달 `IServiceProvider` 하는 RC1 코드가 있으면이는 별도의 생성자 매개 변수가 아니라로 `DbContextOptions`이동 되었습니다. 를 `DbContextOptionsBuilder.UseInternalServiceProvider(...)` 사용 하 여 서비스 공급자를 설정 합니다.

### <a name="testing"></a>테스트

이 작업을 수행 하는 가장 일반적인 시나리오는 테스트할 때 InMemory 데이터베이스의 범위를 제어 하는 것입니다. RC2를 사용 하 여이 작업을 수행 하는 예제는 업데이트 된 [테스트](testing/index.md) 문서를 참조 하세요.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>응용 프로그램 서비스 공급자에서 내부 서비스 확인 (ASP.NET Core 프로젝트에만 해당)

ASP.NET Core 응용 프로그램이 있고 EF에서 응용 프로그램 서비스 공급자의 내부 서비스를 확인 하려는 경우이를 구성할 수 `AddDbContext` 있는 오버 로드가 있습니다.

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider));
```

> [!WARNING]  
> 내부 EF 서비스를 응용 프로그램 서비스 공급자에 결합 해야 하는 이유가 없다면 EF가 내부적으로 자체 서비스를 관리 하도록 허용 하는 것이 좋습니다. 이 작업을 수행 하는 가장 중요 한 이유는 응용 프로그램 서비스 공급자를 사용 하 여 EF에서 내부적으로 사용 하는 서비스를 대체 하는 것입니다.

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>DNX Commands = > .NET CLI (ASP.NET Core 프로젝트에만 해당)

이전에 ASP.NET 5 프로젝트 `dnx ef` 에 대 한 명령을 사용한 경우 이제 명령으로 `dotnet ef` 이동 되었습니다. 동일한 명령 구문이 여전히 적용 됩니다. 구문 정보에 `dotnet ef --help` 를 사용할 수 있습니다.

DNX가 .NET CLI로 대체 되기 때문에 RC2에서 명령이 등록 되는 방법이 변경 되었습니다. 이제 명령이의 `tools` `project.json`섹션에 등록 되어 있습니다.

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> Visual Studio를 사용 하는 경우 이제 패키지 관리자 콘솔을 사용 하 여 ASP.NET Core 프로젝트에 대 한 EF 명령을 실행할 수 있습니다 (RC1에서는 지원 되지 않음). 이 작업을 수행 하려면 여전히의 `tools` `project.json` 섹션에서 명령을 등록 해야 합니다.

## <a name="package-manager-commands-require-powershell-5"></a>패키지 관리자 명령에 PowerShell 5 필요

Visual Studio의 패키지 관리자 콘솔에서 Entity Framework 명령을 사용 하는 경우 PowerShell 5가 설치 되어 있는지 확인 해야 합니다. 다음 릴리스에서 제거 될 임시 요구 사항입니다 (자세한 내용은 [문제 #5327](https://github.com/aspnet/EntityFramework/issues/5327) 참조).

## <a name="using-imports-in-projectjson"></a>Project. json에서 "imports" 사용

EF Core의 일부 종속성은 아직 .NET Standard를 지원 하지 않습니다. .NET Standard 및 .NET Core 프로젝트의 EF Core에는 임시 해결 방법으로 project. json에 "가져오기"를 추가 해야 할 수 있습니다.

EF를 추가 하는 경우 NuGet 복원은 다음 오류 메시지를 표시 합니다.

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

해결 방법은 이식 가능한 프로필 "net451 + win8"를 수동으로 가져오는 것입니다. 이렇게 하면 NuGet이이 이진 파일과 일치 하는 이진 파일을 .NET Standard와 호환 되는 프레임 워크로 처리 합니다. 단,이는 그렇지 않습니다. "Net451 + win8"는 .NET Standard와 100% 호환 되지 않지만 PCL에서 .NET Standard로 전환 하는 경우에만 호환 됩니다. EF의 종속성이 궁극적으로 .NET Standard로 업그레이드 되 면 가져오기를 제거할 수 있습니다.

배열 구문에서 "imports"에 여러 프레임 워크를 추가할 수 있습니다. 다른 가져오기는 프로젝트에 라이브러리를 추가 하는 경우에 필요할 수 있습니다.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

[문제 #5176](https://github.com/aspnet/EntityFramework/issues/5176)를 참조 하세요.
