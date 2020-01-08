---
title: 로깅-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 1a3863ee5f508c1fd393d4ec2c25c46ab8634f00
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502099"
---
# <a name="logging"></a>로깅

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging)을 볼 수 있습니다.

## <a name="aspnet-core-applications"></a>응용 프로그램 ASP.NET Core

EF Core은 `AddDbContext` 또는 `AddDbContextPool`를 사용할 때마다 ASP.NET Core의 로깅 메커니즘과 자동으로 통합 됩니다. 따라서 ASP.NET Core를 사용 하는 경우 [ASP.NET Core 설명서](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)에 설명 된 대로 로깅을 구성 해야 합니다.

## <a name="other-applications"></a>다른 애플리케이션

EF Core 로깅에는 하나 이상의 로깅 공급자를 사용 하 여 구성 된 ILoggerFactory가 필요 합니다. 공통 공급자는 다음 패키지에 제공 됩니다.

* [Microsoft. extension. console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): 간단한 콘솔로 거입니다.
* [Microsoft 확장명. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Azure 앱 서비스 ' 진단 로그 ' 및 ' 로그 스트림 ' 기능을 지원 합니다.
* [Microsoft. extension. debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): System.web. Debug. WriteLine ()을 사용 하 여 디버거 모니터에 기록 합니다.
* [Microsoft. 확장명: EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Windows 이벤트 로그에 기록 합니다.
* [Microsoft 확장명. eventsource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Eventsource/eventlistener를 지원 합니다.
* [TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): `System.Diagnostics.TraceSource.TraceEvent()`을 사용 하 여 추적 수신기에 기록 합니다.

적절 한 패키지를 설치한 후 응용 프로그램은 Server.loggerfactory의 단일/전역 인스턴스를 만들어야 합니다. 예를 들어 콘솔로 거를 사용 합니다.

### <a name="version-30tabv3"></a>[버전 3.0](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

### <a name="version-2xtabv2"></a>[버전 2.x](#tab/v2)

> [!NOTE]
> 다음 코드 샘플에서는 2.2 버전에서 사용 되지 않는 `ConsoleLoggerProvider` 생성자를 사용 하 고 3.0에서 대체 했습니다. 2\.2을 사용 하는 경우 경고를 무시 하 고 표시 하지 않는 것이 안전 합니다.

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

그런 다음이 단일/전역 인스턴스를 `DbContextOptionsBuilder`EF Core에 등록 해야 합니다. 예를 들면 다음과 같습니다.:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> 응용 프로그램에서 각 컨텍스트 인스턴스에 대해 새로운 ILoggerFactory 인스턴스를 만들지 않는 것이 매우 중요 합니다. 이렇게 하면 메모리 누수가 발생 하 고 성능이 저하 됩니다.

## <a name="filtering-what-is-logged"></a>기록 되는 내용 필터링

응용 프로그램은 ILoggerProvider에 대 한 필터를 구성 하 여 로깅되는 내용을 제어할 수 있습니다. 예를 들면 다음과 같습니다.:

### <a name="version-30tabv3"></a>[버전 3.0](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

### <a name="version-2xtabv2"></a>[버전 2.x](#tab/v2)

> [!NOTE]
> 다음 코드 샘플에서는 2.2 버전에서 사용 되지 않는 `ConsoleLoggerProvider` 생성자를 사용 하 고 3.0에서 대체 했습니다. 2\.2을 사용 하는 경우 경고를 무시 하 고 표시 하지 않는 것이 안전 합니다.

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

EF Core에 대 한로 거 범주는 범주를 쉽게 찾을 수 있도록 `DbLoggerCategory` 클래스에서 정의 되지만 단순 문자열로 확인 됩니다.

기본 로깅 인프라에 대 한 자세한 내용은 [ASP.NET Core 로깅 설명서](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)에서 찾을 수 있습니다.
