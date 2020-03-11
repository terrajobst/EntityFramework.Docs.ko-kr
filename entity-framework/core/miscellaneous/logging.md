---
title: 로깅-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: e8adc39ec01ff75112b03446a488df6199cc7041
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414088"
---
# <a name="logging"></a><span data-ttu-id="47a57-102">로깅</span><span class="sxs-lookup"><span data-stu-id="47a57-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="47a57-103">GitHub에서 이 문서의 [샘플](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-103">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="47a57-104">응용 프로그램 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="47a57-104">ASP.NET Core applications</span></span>

<span data-ttu-id="47a57-105">EF Core은 `AddDbContext` 또는 `AddDbContextPool`를 사용할 때마다 ASP.NET Core의 로깅 메커니즘과 자동으로 통합 됩니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="47a57-106">따라서 ASP.NET Core를 사용 하는 경우 [ASP.NET Core 설명서](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)에 설명 된 대로 로깅을 구성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="47a57-107">다른 애플리케이션</span><span class="sxs-lookup"><span data-stu-id="47a57-107">Other applications</span></span>

<span data-ttu-id="47a57-108">EF Core 로깅에는 하나 이상의 로깅 공급자를 사용 하 여 구성 된 ILoggerFactory가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-108">EF Core logging requires an ILoggerFactory which is itself configured with one or more logging providers.</span></span> <span data-ttu-id="47a57-109">공통 공급자는 다음 패키지에 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="47a57-110">[Microsoft. extension. console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): 간단한 콘솔로 거입니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="47a57-111">[Microsoft 확장명. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Azure 앱 서비스 ' 진단 로그 ' 및 ' 로그 스트림 ' 기능을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="47a57-112">[Microsoft. extension. debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): System.web. Debug. WriteLine ()을 사용 하 여 디버거 모니터에 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="47a57-113">[Microsoft. 확장명: EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Windows 이벤트 로그에 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="47a57-114">[Microsoft 확장명. eventsource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Eventsource/eventlistener를 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="47a57-115">[TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): `System.Diagnostics.TraceSource.TraceEvent()`을 사용 하 여 추적 수신기에 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using `System.Diagnostics.TraceSource.TraceEvent()`.</span></span>

<span data-ttu-id="47a57-116">적절 한 패키지를 설치한 후 응용 프로그램은 Server.loggerfactory의 단일/전역 인스턴스를 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="47a57-117">예를 들어 콘솔로 거를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-117">For example, using the console logger:</span></span>

### <a name="version-3x"></a>[<span data-ttu-id="47a57-118">버전 3.x</span><span class="sxs-lookup"><span data-stu-id="47a57-118">Version 3.x</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[<span data-ttu-id="47a57-119">버전 2.x</span><span class="sxs-lookup"><span data-stu-id="47a57-119">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="47a57-120">다음 코드 샘플에서는 2.2 버전에서 사용 되지 않는 `ConsoleLoggerProvider` 생성자를 사용 하 고 3.0에서 대체 했습니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-120">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="47a57-121">2\.2을 사용 하는 경우 경고를 무시 하 고 표시 하지 않는 것이 안전 합니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-121">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

<span data-ttu-id="47a57-122">그런 다음이 단일/전역 인스턴스를 `DbContextOptionsBuilder`EF Core에 등록 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-122">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="47a57-123">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-123">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="47a57-124">응용 프로그램에서 각 컨텍스트 인스턴스에 대해 새로운 ILoggerFactory 인스턴스를 만들지 않는 것이 매우 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-124">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="47a57-125">이렇게 하면 메모리 누수가 발생 하 고 성능이 저하 됩니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-125">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="47a57-126">기록 되는 내용 필터링</span><span class="sxs-lookup"><span data-stu-id="47a57-126">Filtering what is logged</span></span>

<span data-ttu-id="47a57-127">응용 프로그램은 ILoggerProvider에 대 한 필터를 구성 하 여 로깅되는 내용을 제어할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-127">The application can control what is logged by configuring a filter on the ILoggerProvider.</span></span> <span data-ttu-id="47a57-128">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-128">For example:</span></span>

### <a name="version-3x"></a>[<span data-ttu-id="47a57-129">버전 3.x</span><span class="sxs-lookup"><span data-stu-id="47a57-129">Version 3.x</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[<span data-ttu-id="47a57-130">버전 2.x</span><span class="sxs-lookup"><span data-stu-id="47a57-130">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="47a57-131">다음 코드 샘플에서는 2.2 버전에서 사용 되지 않는 `ConsoleLoggerProvider` 생성자를 사용 하 고 3.0에서 대체 했습니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-131">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="47a57-132">2\.2을 사용 하는 경우 경고를 무시 하 고 표시 하지 않는 것이 안전 합니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-132">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

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

<span data-ttu-id="47a57-133">이 예에서는 로그를 필터링 하 여 메시지만 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-133">In this example, the log is filtered to only return messages:</span></span>

* <span data-ttu-id="47a57-134">' Microsoft.entityframeworkcore.tools.dotnet ' 범주에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-134">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
* <span data-ttu-id="47a57-135">' 정보 ' 수준에서</span><span class="sxs-lookup"><span data-stu-id="47a57-135">at the 'Information' level</span></span>

<span data-ttu-id="47a57-136">EF Core에 대 한로 거 범주는 범주를 쉽게 찾을 수 있도록 `DbLoggerCategory` 클래스에서 정의 되지만 단순 문자열로 확인 됩니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-136">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="47a57-137">기본 로깅 인프라에 대 한 자세한 내용은 [ASP.NET Core 로깅 설명서](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)에서 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="47a57-137">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
