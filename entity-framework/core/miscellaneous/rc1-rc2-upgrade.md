---
title: EF Core 1.0 RC1에서 RC2-EF Core로 업그레이드
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 83b98fda5ac9491994b5b3fb333c9951ec01188a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996899"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>EF Core 1.0 RC1에서 1.0 RC2로 업그레이드

이 문서에서는 rc2 RC1 패키지를 사용 하 여 빌드된 응용 프로그램을 이동 하는 것에 대 한 지침을 제공 합니다.

## <a name="package-names-and-versions"></a>패키지 이름 및 버전

RC1 및 RC2 사이 "Entity Framework Core"에서 "Entity Framework 7"로 변경 했습니다. 자세한 내용은 변경 이유에 대 [Scott hanselman이 게시물](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx)합니다. 이러한 변경으로 인해 우리 패키지 이름에서 변경 `EntityFramework.*` 하 `Microsoft.EntityFrameworkCore.*` 에서 버전 `7.0.0-rc1-final` 에 `1.0.0-rc2-final` (또는 `1.0.0-preview1-final` 도구에 대 한).

**완전히 RC1 패키지를 제거한 다음 RC2를 설치 해야 것입니다.** 몇 가지 일반적인 패키지에 대 한 매핑을 다음과 같습니다.

| RC1 패키지                                               | RC2 해당                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final      |
| EntityFramework.SQLite                    7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final      |
| EntityFramework7.Npgsql                   3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres             <to be advised>      |
| EntityFramework.SqlServerCompact35        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final      |
| EntityFramework.InMemory                  7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final      |
| EntityFramework.IBMDataServer             7.0.0-beta1     | RC2 용 아직 사용할 수 없음                                            |
| EntityFramework.Commands                  7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final      |

## <a name="namespaces"></a>네임스페이스

패키지 이름과 함께 네임 스페이스에서 변경할 `Microsoft.Data.Entity.*` 에 `Microsoft.EntityFrameworkCore.*`입니다. 이 변경의 찾기/바꾸기를 사용 하 여 처리할 수 있습니다 `using Microsoft.Data.Entity` 사용 하 여 `using Microsoft.EntityFrameworkCore`입니다.

## <a name="table-naming-convention-changes"></a>테이블 명명 규칙 변경

RC2의 했습니다 중요 한 기능 변경의 이름을 사용 하는 것 이었습니다는 `DbSet<TEntity>` 테이블 이름으로 지정 된 엔터티에 대 한 속성이 매핑될 클래스 이름 대신 합니다. 알아볼 수 있습니다이 변경에 대 한 [관련된 알림 문제](https://github.com/aspnet/Announcements/issues/167)합니다.

기존 RC1 응용 프로그램에 대 한 것이 좋습니다의 시작 부분에 다음 코드를 추가 하면 `OnModelCreating` RC1 명명 전략을 유지 하는 방법.

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

새 명명 전략을 채택 하려는 경우 것이 좋습니다 성공적으로 완료 된 업그레이드 단계 다음 코드를 제거 및 테이블에 적용할 마이그레이션을 만들기의 나머지 부분 이름을 바꿉니다.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>AddDbContext / Startup.cs 변경 (ASP.NET Core 프로젝트에만 해당)

Rc1에서는에서 Entity Framework 서비스는 응용 프로그램 서비스 공급자를 추가 해야 `Startup.ConfigureServices(...)`:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

RC2에서에 대 한 호출을 제거할 수 있습니다 `AddEntityFramework()`, `AddSqlServer()`등.:

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

상황에 맞는 옵션을 사용 하 고 기본 생성자에 전달 하 여 파생 컨텍스트에 생성자를 추가 해야 합니다. 이 백그라운드에서 예에서 약간 scary 마법을 일부 제거 했습니다 때문에 필요 합니다.

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>즉, IServiceProvider 전달

RC1 코드가 전달 하는 경우는 `IServiceProvider` 컨텍스트에이 이제로 이동 되었습니다 `DbContextOptions`, 별도 생성자 매개 변수를 되지 않고 있습니다. 사용 하 여 `DbContextOptionsBuilder.UseInternalServiceProvider(...)` 서비스 공급자를 설정 합니다.

### <a name="testing"></a>테스트

이 작업을 수행 하는 것에 대 한 가장 일반적인 시나리오를 테스트할 때 InMemory 데이터베이스의 범위를 제어 하는 것 이었습니다. 업데이트 된 참조 [테스트](testing/index.md) RC2를 사용 하 여이 작업을 수행 하는 예는 문서입니다.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>응용 프로그램 서비스 공급자 (ASP.NET Core 프로젝트에만 해당)에서 내부 서비스 확인

ASP.NET Core 응용 프로그램을 EF 응용 프로그램 서비스 공급자에서 내부 서비스를 해결할 경우의 오버 로드 `AddDbContext` 구성할 수 있습니다.

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> 응용 프로그램 서비스 공급자를 내부 EF 서비스를 결합 하는 이유가 없다면 내부적으로 자체 서비스를 관리 하는 EF를 허용 하는 것이 좋습니다. 응용 프로그램 서비스 공급자에 게 EF가 내부적으로 사용 하는 서비스를 대체 하는 데이 작업을 수행 하려고 하는 주된 이유는

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>DNX 명령 = >.NET CLI (ASP.NET Core 프로젝트에만 해당)

이전에 사용한 합니다 `dnx ef` ASP.NET 5 프로젝트에 대 한 명령, 이러한 이제 전환 `dotnet ef` 명령입니다. 동일한 명령 구문을 계속 적용 됩니다. 사용할 수 있습니다 `dotnet ef --help` 구문 정보에 대 한 합니다.

명령을 등록 하는 방법은.NET CLI 교체 되 고 DNX 인해 RC2에서 변경 되었습니다. 명령에서 이제 등록 되었습니다.는 `tools` 단원의 `project.json`:

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
> Visual Studio를 사용 하는 경우 (이 지원 되지 않습니다 RC1에서) 하는 ASP.NET Core 프로젝트에 대 한 EF 명령은 실행 하려면 패키지 관리자 콘솔을 이제 사용할 수 있습니다. 명령에 등록 해야 합니다 `tools` 의 섹션 `project.json` 이 작업을 수행 하 합니다.

## <a name="package-manager-commands-require-powershell-5"></a>패키지 관리자 명령을 PowerShell 5 필요

Entity Framework 명령을 사용 하 여 Visual Studio에서 패키지 관리자 콘솔에서 PowerShell 5가 설치 되어 있는지 확인 해야 합니다. 이 다음 릴리스에서 제거 될 임시 요구 사항이 (참조 [#5327 발급](https://github.com/aspnet/EntityFramework/issues/5327) 자세한).

## <a name="using-imports-in-projectjson"></a>Project.json에서 "가져오기"를 사용 하 여

EF Core의 종속성 중 일부를 지원 하지 않습니다.NET Standard 아직. .NET Standard 및.NET Core 프로젝트에서 EF Core는 "imports" 임시 방편으로 project.json에 추가 해야 합니다.

EF에 추가할 때 NuGet 복원에는이 오류 메시지가 표시 됩니다.

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

해결 방법은 수동으로 "이식 가능한 net451 + win8" 이식 가능한 프로필을 가져오는 것입니다. 이 일치 하는이 이진 파일을 처리 하는 NuGet이이 강제로 하지 않은 경우에.NET standard 호환 프레임 워크를 제공 합니다. "이식 가능한 net451 + win8" 하지는 않지만 100% 호환.NET Standard를 사용 하 여.NET 표준 PCL에서 전환 하는 데 충분히 호환 됩니다. Imports EF의 종속성이 결과적으로.NET 표준으로 업그레이드 하는 경우 제거할 수 있습니다.

배열 구문에서 "imports" 여러 프레임 워크를 추가할 수 있습니다. 다른 가져오기 프로젝트에 추가 라이브러리를 추가 하는 경우에 필요할 수 있습니다.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

참조 [#5176 발급](https://github.com/aspnet/EntityFramework/issues/5176)합니다.
