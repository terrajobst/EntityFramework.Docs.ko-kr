---
title: 로깅-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: efc78fbada3c59bf9cf2c4cb694835bb5ad60e76
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997007"
---
# <a name="logging"></a>로깅

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging)을 볼 수 있습니다.

## <a name="aspnet-core-applications"></a>ASP.NET Core 응용 프로그램

EF Core는 ASP.NET Core의 로깅 메커니즘을 사용 하 여 자동으로 통합 될 때마다 `AddDbContext` 또는 `AddDbContextPool` 사용 됩니다. 따라서 ASP.NET Core를 사용 하는 경우 로깅 구성 해야에 설명 된 대로 합니다 [ASP.NET Core 설명서](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)합니다.

## <a name="other-applications"></a>다른 응용 프로그램

현재 로깅 EF Core에는 하나 이상의 ILoggerProvider를 사용 하 여 구성 자체는 ILoggerFactory 필요 합니다. 일반적인 공급자 같은 패키지에 제공 됩니다.

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): 간단한 콘솔으로 거를 합니다.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): 지 원하는 Azure App Services '진단 로그' 및 '로그 스트림' 기능입니다.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): System.Diagnostics.Debug.WriteLine()를 사용 하 여 디버거 모니터는 로그입니다.
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Windows 이벤트 로그에 기록 합니다.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener를 지원 합니다.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): System.Diagnostics.TraceSource.TraceEvent()를 사용 하 여 추적 수신기에 로그 합니다.

적절 한 패키지를 설치한 후 응용 프로그램을 LoggerFactory의 singleton/전역 인스턴스를 만들어야 합니다. 예를 들어 콘솔으로 거를 사용 하 여:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

이 단일/전역 인스턴스 다음에 등록 해야 및 EF Core는 `DbContextOptionsBuilder`합니다. 예를 들어:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> 것이 매우 중요 한 응용 프로그램 각 컨텍스트 인스턴스에 대 한 새 ILoggerFactory 인스턴스를 만들지 마십시오입니다. 이렇게 성능 저하 및 메모리 누수를 발생 합니다.

## <a name="filtering-what-is-logged"></a>로깅되는 내용을 필터링 합니다.

로깅되는 내용을 필터링 하는 가장 쉬운 방법은 구성 하는 것은 ILoggerProvider를 등록 하는 경우입니다. 예를 들어:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

이 예제에서는 로그 메시지에만 반환 하도록 필터링 됩니다.
 * 'Microsoft.EntityFrameworkCore.Database.Command' 범주에
 * '정보' 수준

EF Core에 대 한로 거 범주에 정의 된는 `DbLoggerCategory` 간단한 문자열을 쉽게 있지만 범주를 찾을 수 있도록 클래스를 해결 합니다.

기본 로깅 인프라에 대 한 자세한 내용은에서 찾을 수 있습니다 합니다 [ASP.NET Core 로깅 설명서](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)합니다.
