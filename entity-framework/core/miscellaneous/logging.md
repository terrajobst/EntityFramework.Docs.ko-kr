---
title: 로깅-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 6a8499f9f0220087e76f2e0b3a75ce551c4ddb80
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197512"
---
# <a name="logging"></a>로깅

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging)을 볼 수 있습니다.

## <a name="aspnet-core-applications"></a>응용 프로그램 ASP.NET Core

EF Core은 또는 `AddDbContext` `AddDbContextPool` 가 사용 될 때마다 ASP.NET Core의 로깅 메커니즘과 자동으로 통합 됩니다. 따라서 ASP.NET Core를 사용 하는 경우 [ASP.NET Core 설명서](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)에 설명 된 대로 로깅을 구성 해야 합니다.

## <a name="other-applications"></a>기타 응용 프로그램

EF Core 로깅에는 하나 이상의 로깅 공급자를 사용 하 여 구성 된 ILoggerFactory가 필요 합니다. 공통 공급자는 다음 패키지에 제공 됩니다.

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): 간단한 콘솔로 거입니다.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Azure 앱 서비스 ' 진단 로그 ' 및 ' 로그 스트림 ' 기능을 지원 합니다.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): 시스템 진단. WriteLine. WriteLine ()을 사용 하 여 디버거 모니터에 기록 합니다.
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Windows 이벤트 로그에 기록 합니다.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener를 지원 합니다.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): 를 사용 하 여 `System.Diagnostics.TraceSource.TraceEvent()`추적 수신기에 기록 합니다.

적절 한 패키지를 설치한 후 응용 프로그램은 Server.loggerfactory의 단일/전역 인스턴스를 만들어야 합니다. 예를 들어 콘솔로 거를 사용 합니다.

# <a name="version-30tabv3"></a>[버전 3.0](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

# <a name="version-2xtabv2"></a>[2.x 버전](#tab/v2)

> [!NOTE]
> 다음 코드 샘플에서는 버전 2.2 `ConsoleLoggerProvider` 에서 사용 되지 않는 생성자를 사용 하 고 3.0에서 대체 했습니다. 2\.2을 사용 하는 경우 경고를 무시 하 고 표시 하지 않는 것이 안전 합니다.

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

그런 다음이 단일/전역 인스턴스를에 EF Core `DbContextOptionsBuilder`등록 해야 합니다. 예:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> 응용 프로그램에서 각 컨텍스트 인스턴스에 대해 새로운 ILoggerFactory 인스턴스를 만들지 않는 것이 매우 중요 합니다. 이렇게 하면 메모리 누수가 발생 하 고 성능이 저하 됩니다.

## <a name="filtering-what-is-logged"></a>기록 되는 내용 필터링

응용 프로그램은 ILoggerProvider에 대 한 필터를 구성 하 여 로깅되는 내용을 제어할 수 있습니다. 예:

# <a name="version-30tabv3"></a>[버전 3.0](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

# <a name="version-2xtabv2"></a>[2.x 버전](#tab/v2)

> [!NOTE]
> 다음 코드 샘플에서는 버전 2.2 `ConsoleLoggerProvider` 에서 사용 되지 않는 생성자를 사용 하 고 3.0에서 대체 했습니다. 2\.2을 사용 하는 경우 경고를 무시 하 고 표시 하지 않는 것이 안전 합니다.

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[]
    {
        new ConsoleLoggerProvider((category, level)
            => category == DbLoggerCategory.Database.Command.Name
               && level == LogLevel.Information, true)
    });
```

***

이 예에서는 로그를 필터링 하 여 메시지만 반환 합니다.
 * ' Microsoft.entityframeworkcore.tools.dotnet ' 범주에 있습니다.
 * ' 정보 ' 수준에서

EF Core에서로 거 범주는 범주를 쉽게 `DbLoggerCategory` 찾을 수 있도록 클래스에 정의 되어 있지만 단순 문자열로 확인 됩니다.

기본 로깅 인프라에 대 한 자세한 내용은 [ASP.NET Core 로깅 설명서](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)에서 찾을 수 있습니다.
