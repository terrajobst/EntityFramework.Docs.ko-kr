---
title: "로깅-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
ms.technology: entity-framework-core
uid: core/miscellaneous/logging
ms.openlocfilehash: 807560e563eddfb72d4286353b1403a0d2e2a441
ms.sourcegitcommit: 5367516f063cb42804ec92c31cdf76322554f2b5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2017
---
# <a name="logging"></a>로깅

> [!TIP]  
> 이 문서를 볼 수 있습니다 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) GitHub에서 합니다.

## <a name="aspnet-core-applications"></a>ASP.NET Core 응용 프로그램

EF 코어 ASP.NET 코어의 로깅 오류 없이 안전한 메커니즘와 자동으로 통합 될 때마다 `AddDbContext` 또는 `AddDbContextPool` 사용 됩니다. 따라서 ASP.NET Core를 사용할 경우 로깅 해야 구성에 설명 된 대로 [ASP.NET Core 설명서](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)합니다.

## <a name="other-applications"></a>다른 응용 프로그램

현재 로깅 EF 핵심 구성 된 하나 이상의 ILoggerProvider 그 자체가 ILoggerFactory 필요 합니다. 일반 공급자는 다음 패키지에 제공 됩니다.

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): 간단한 콘솔으로 거 합니다.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): 지 원하는 Azure 앱 서비스 '진단 로그' 및 '로그 스트림' 기능입니다.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): System.Diagnostics.Debug.WriteLine()를 사용 하 여 디버거 모니터에 로깅합니다.
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Windows 이벤트 로그에 기록 합니다.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener를 지원 합니다.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): System.Diagnostics.TraceSource.TraceEvent()를 사용 하 여 추적 수신기에는 로그입니다.

적절 한 패키지를 설치한 후 응용 프로그램의 한 LoggerFactory singleton/전역 인스턴스를 만들어야 합니다. 예를 들어 콘솔으로 거를 사용 하 여:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

이 단일 항목/전역 인스턴스 다음에 등록 해야 EF 코어는 `DbContextOptionsBuilder`합니다. 예:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> 응용 프로그램 각 컨텍스트 인스턴스에 대 한 새 ILoggerFactory 인스턴스를 만들지 않는 것이 중요 합니다. 이렇게 메모리 누수 및 성능이 저하 됩니다.

## <a name="filtering-what-is-logged"></a>기록 된 값 필터링

기록 된 값을 필터링 하는 가장 쉬운 방법은 ILoggerProvider 등록할 때이 구성 하는 것입니다. 예:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

이 예제에서는 로그 메시지에만 반환 하도록 필터링:
 * 'Microsoft.EntityFrameworkCore.Database.Command' 범주에
 * '정보' 수준

EF 코어에 대 한로 거 범주에 정의 된는 `DbLoggerCategory` 쉽게 범주 있지만 찾을 수 있도록 클래스 단순한 문자열을 확인 합니다.

기본 로깅 인프라에 대 한 자세한 내용은에서 확인할 수 있습니다는 [ASP.NET Core 로깅 설명서](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)합니다.