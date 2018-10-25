---
title: 로깅-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 65501b5ac03ae544c51b7fc1a07fa9eea849f1e3
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022147"
---
# <a name="logging"></a><span data-ttu-id="79949-102">로깅</span><span class="sxs-lookup"><span data-stu-id="79949-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="79949-103">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="79949-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="79949-104">ASP.NET Core 응용 프로그램</span><span class="sxs-lookup"><span data-stu-id="79949-104">ASP.NET Core applications</span></span>

<span data-ttu-id="79949-105">EF Core는 ASP.NET Core의 로깅 메커니즘을 사용 하 여 자동으로 통합 될 때마다 `AddDbContext` 또는 `AddDbContextPool` 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="79949-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="79949-106">따라서 ASP.NET Core를 사용 하는 경우 로깅 구성 해야에 설명 된 대로 합니다 [ASP.NET Core 설명서](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)합니다.</span><span class="sxs-lookup"><span data-stu-id="79949-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="79949-107">다른 응용 프로그램</span><span class="sxs-lookup"><span data-stu-id="79949-107">Other applications</span></span>

<span data-ttu-id="79949-108">현재 로깅 EF Core에는 하나 이상의 ILoggerProvider를 사용 하 여 구성 자체는 ILoggerFactory 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="79949-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="79949-109">일반적인 공급자 같은 패키지에 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="79949-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="79949-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): 간단한 콘솔으로 거를 합니다.</span><span class="sxs-lookup"><span data-stu-id="79949-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="79949-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): 지 원하는 Azure App Services '진단 로그' 및 '로그 스트림' 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="79949-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="79949-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): System.Diagnostics.Debug.WriteLine()를 사용 하 여 디버거 모니터는 로그입니다.</span><span class="sxs-lookup"><span data-stu-id="79949-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="79949-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Windows 이벤트 로그에 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="79949-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="79949-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener를 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="79949-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="79949-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): System.Diagnostics.TraceSource.TraceEvent()를 사용 하 여 추적 수신기에 로그 합니다.</span><span class="sxs-lookup"><span data-stu-id="79949-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

<span data-ttu-id="79949-116">적절 한 패키지를 설치한 후 응용 프로그램을 LoggerFactory의 singleton/전역 인스턴스를 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="79949-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="79949-117">예를 들어 콘솔으로 거를 사용 하 여:</span><span class="sxs-lookup"><span data-stu-id="79949-117">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="79949-118">이 단일/전역 인스턴스 다음에 등록 해야 및 EF Core는 `DbContextOptionsBuilder`합니다.</span><span class="sxs-lookup"><span data-stu-id="79949-118">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="79949-119">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="79949-119">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="79949-120">것이 매우 중요 한 응용 프로그램 각 컨텍스트 인스턴스에 대 한 새 ILoggerFactory 인스턴스를 만들지 마십시오입니다.</span><span class="sxs-lookup"><span data-stu-id="79949-120">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="79949-121">이렇게 성능 저하 및 메모리 누수를 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="79949-121">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="79949-122">로깅되는 내용을 필터링 합니다.</span><span class="sxs-lookup"><span data-stu-id="79949-122">Filtering what is logged</span></span>

<span data-ttu-id="79949-123">로깅되는 내용을 필터링 하는 가장 쉬운 방법은 구성 하는 것은 ILoggerProvider를 등록 하는 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="79949-123">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="79949-124">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="79949-124">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="79949-125">이 예제에서는 로그 메시지에만 반환 하도록 필터링 됩니다.</span><span class="sxs-lookup"><span data-stu-id="79949-125">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="79949-126">'Microsoft.EntityFrameworkCore.Database.Command' 범주에</span><span class="sxs-lookup"><span data-stu-id="79949-126">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="79949-127">'정보' 수준</span><span class="sxs-lookup"><span data-stu-id="79949-127">at the 'Information' level</span></span>

<span data-ttu-id="79949-128">EF Core에 대 한로 거 범주에 정의 된는 `DbLoggerCategory` 간단한 문자열을 쉽게 있지만 범주를 찾을 수 있도록 클래스를 해결 합니다.</span><span class="sxs-lookup"><span data-stu-id="79949-128">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="79949-129">기본 로깅 인프라에 대 한 자세한 내용은에서 찾을 수 있습니다 합니다 [ASP.NET Core 로깅 설명서](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)합니다.</span><span class="sxs-lookup"><span data-stu-id="79949-129">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
