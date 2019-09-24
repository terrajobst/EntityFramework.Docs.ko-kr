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
# <a name="logging"></a><span data-ttu-id="b62b0-102">로깅</span><span class="sxs-lookup"><span data-stu-id="b62b0-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="b62b0-103">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="b62b0-104">응용 프로그램 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b62b0-104">ASP.NET Core applications</span></span>

<span data-ttu-id="b62b0-105">EF Core은 또는 `AddDbContext` `AddDbContextPool` 가 사용 될 때마다 ASP.NET Core의 로깅 메커니즘과 자동으로 통합 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="b62b0-106">따라서 ASP.NET Core를 사용 하는 경우 [ASP.NET Core 설명서](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)에 설명 된 대로 로깅을 구성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="b62b0-107">기타 응용 프로그램</span><span class="sxs-lookup"><span data-stu-id="b62b0-107">Other applications</span></span>

<span data-ttu-id="b62b0-108">EF Core 로깅에는 하나 이상의 로깅 공급자를 사용 하 여 구성 된 ILoggerFactory가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-108">EF Core logging requires an ILoggerFactory which is itself configured with one or more logging providers.</span></span> <span data-ttu-id="b62b0-109">공통 공급자는 다음 패키지에 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="b62b0-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): 간단한 콘솔로 거입니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="b62b0-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Azure 앱 서비스 ' 진단 로그 ' 및 ' 로그 스트림 ' 기능을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="b62b0-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): 시스템 진단. WriteLine. WriteLine ()을 사용 하 여 디버거 모니터에 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="b62b0-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Windows 이벤트 로그에 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="b62b0-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener를 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="b62b0-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): 를 사용 하 여 `System.Diagnostics.TraceSource.TraceEvent()`추적 수신기에 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using `System.Diagnostics.TraceSource.TraceEvent()`.</span></span>

<span data-ttu-id="b62b0-116">적절 한 패키지를 설치한 후 응용 프로그램은 Server.loggerfactory의 단일/전역 인스턴스를 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="b62b0-117">예를 들어 콘솔로 거를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-117">For example, using the console logger:</span></span>

# <a name="version-30tabv3"></a>[<span data-ttu-id="b62b0-118">버전 3.0</span><span class="sxs-lookup"><span data-stu-id="b62b0-118">Version 3.0</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

# <a name="version-2xtabv2"></a>[<span data-ttu-id="b62b0-119">2.x 버전</span><span class="sxs-lookup"><span data-stu-id="b62b0-119">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="b62b0-120">다음 코드 샘플에서는 버전 2.2 `ConsoleLoggerProvider` 에서 사용 되지 않는 생성자를 사용 하 고 3.0에서 대체 했습니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-120">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="b62b0-121">2\.2을 사용 하는 경우 경고를 무시 하 고 표시 하지 않는 것이 안전 합니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-121">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

<span data-ttu-id="b62b0-122">그런 다음이 단일/전역 인스턴스를에 EF Core `DbContextOptionsBuilder`등록 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-122">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="b62b0-123">예:</span><span class="sxs-lookup"><span data-stu-id="b62b0-123">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="b62b0-124">응용 프로그램에서 각 컨텍스트 인스턴스에 대해 새로운 ILoggerFactory 인스턴스를 만들지 않는 것이 매우 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-124">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="b62b0-125">이렇게 하면 메모리 누수가 발생 하 고 성능이 저하 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-125">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="b62b0-126">기록 되는 내용 필터링</span><span class="sxs-lookup"><span data-stu-id="b62b0-126">Filtering what is logged</span></span>

<span data-ttu-id="b62b0-127">응용 프로그램은 ILoggerProvider에 대 한 필터를 구성 하 여 로깅되는 내용을 제어할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-127">The application can control what is logged by configuring a filter on the ILoggerProvider.</span></span> <span data-ttu-id="b62b0-128">예:</span><span class="sxs-lookup"><span data-stu-id="b62b0-128">For example:</span></span>

# <a name="version-30tabv3"></a>[<span data-ttu-id="b62b0-129">버전 3.0</span><span class="sxs-lookup"><span data-stu-id="b62b0-129">Version 3.0</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

# <a name="version-2xtabv2"></a>[<span data-ttu-id="b62b0-130">2.x 버전</span><span class="sxs-lookup"><span data-stu-id="b62b0-130">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="b62b0-131">다음 코드 샘플에서는 버전 2.2 `ConsoleLoggerProvider` 에서 사용 되지 않는 생성자를 사용 하 고 3.0에서 대체 했습니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-131">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="b62b0-132">2\.2을 사용 하는 경우 경고를 무시 하 고 표시 하지 않는 것이 안전 합니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-132">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

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

<span data-ttu-id="b62b0-133">이 예에서는 로그를 필터링 하 여 메시지만 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-133">In this example, the log is filtered to only return messages:</span></span>
 * <span data-ttu-id="b62b0-134">' Microsoft.entityframeworkcore.tools.dotnet ' 범주에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-134">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="b62b0-135">' 정보 ' 수준에서</span><span class="sxs-lookup"><span data-stu-id="b62b0-135">at the 'Information' level</span></span>

<span data-ttu-id="b62b0-136">EF Core에서로 거 범주는 범주를 쉽게 `DbLoggerCategory` 찾을 수 있도록 클래스에 정의 되어 있지만 단순 문자열로 확인 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-136">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="b62b0-137">기본 로깅 인프라에 대 한 자세한 내용은 [ASP.NET Core 로깅 설명서](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)에서 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b62b0-137">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
