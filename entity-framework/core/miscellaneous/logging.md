---
title: 로깅-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
ms.technology: entity-framework-core
uid: core/miscellaneous/logging
ms.openlocfilehash: 60d76bf3360eb47cdd9836494c1f135d1005a215
ms.sourcegitcommit: 3adf1267be92effc3c9daa893906a7f36834204f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35232138"
---
# <a name="logging"></a><span data-ttu-id="c33e0-102">로깅</span><span class="sxs-lookup"><span data-stu-id="c33e0-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="c33e0-103">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c33e0-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="c33e0-104">ASP.NET Core 응용 프로그램</span><span class="sxs-lookup"><span data-stu-id="c33e0-104">ASP.NET Core applications</span></span>

<span data-ttu-id="c33e0-105">EF 코어 ASP.NET 코어의 로깅 메커니즘와 자동으로 통합 될 때마다 `AddDbContext` 또는 `AddDbContextPool` 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c33e0-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="c33e0-106">따라서 ASP.NET Core를 사용할 경우 로깅 해야 구성에 설명 된 대로 [ASP.NET Core 설명서](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)합니다.</span><span class="sxs-lookup"><span data-stu-id="c33e0-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="c33e0-107">다른 응용 프로그램</span><span class="sxs-lookup"><span data-stu-id="c33e0-107">Other applications</span></span>

<span data-ttu-id="c33e0-108">현재 로깅 EF 핵심 구성 된 하나 이상의 ILoggerProvider 그 자체가 ILoggerFactory 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="c33e0-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="c33e0-109">일반 공급자는 다음 패키지에 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c33e0-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="c33e0-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): 간단한 콘솔으로 거 합니다.</span><span class="sxs-lookup"><span data-stu-id="c33e0-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="c33e0-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): 지 원하는 Azure 앱 서비스 '진단 로그' 및 '로그 스트림' 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="c33e0-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="c33e0-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): System.Diagnostics.Debug.WriteLine()를 사용 하 여 디버거 모니터에 로깅합니다.</span><span class="sxs-lookup"><span data-stu-id="c33e0-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="c33e0-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Windows 이벤트 로그에 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="c33e0-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="c33e0-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener를 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="c33e0-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="c33e0-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): System.Diagnostics.TraceSource.TraceEvent()를 사용 하 여 추적 수신기에는 로그입니다.</span><span class="sxs-lookup"><span data-stu-id="c33e0-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

<span data-ttu-id="c33e0-116">적절 한 패키지를 설치한 후 응용 프로그램의 한 LoggerFactory singleton/전역 인스턴스를 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c33e0-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="c33e0-117">예를 들어 콘솔으로 거를 사용 하 여:</span><span class="sxs-lookup"><span data-stu-id="c33e0-117">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="c33e0-118">이 단일 항목/전역 인스턴스 다음에 등록 해야 EF 코어는 `DbContextOptionsBuilder`합니다.</span><span class="sxs-lookup"><span data-stu-id="c33e0-118">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="c33e0-119">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="c33e0-119">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="c33e0-120">응용 프로그램 각 컨텍스트 인스턴스에 대 한 새 ILoggerFactory 인스턴스를 만들지 않는 것이 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="c33e0-120">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="c33e0-121">이렇게 메모리 누수 및 성능이 저하 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c33e0-121">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="c33e0-122">기록 된 값 필터링</span><span class="sxs-lookup"><span data-stu-id="c33e0-122">Filtering what is logged</span></span>

<span data-ttu-id="c33e0-123">기록 된 값을 필터링 하는 가장 쉬운 방법은 ILoggerProvider 등록할 때이 구성 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c33e0-123">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="c33e0-124">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="c33e0-124">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="c33e0-125">이 예제에서는 로그 메시지에만 반환 하도록 필터링:</span><span class="sxs-lookup"><span data-stu-id="c33e0-125">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="c33e0-126">'Microsoft.EntityFrameworkCore.Database.Command' 범주에</span><span class="sxs-lookup"><span data-stu-id="c33e0-126">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="c33e0-127">'정보' 수준</span><span class="sxs-lookup"><span data-stu-id="c33e0-127">at the 'Information' level</span></span>

<span data-ttu-id="c33e0-128">EF 코어에 대 한로 거 범주에 정의 된는 `DbLoggerCategory` 쉽게 범주 있지만 찾을 수 있도록 클래스 단순한 문자열을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="c33e0-128">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="c33e0-129">기본 로깅 인프라에 대 한 자세한 내용은에서 확인할 수 있습니다는 [ASP.NET Core 로깅 설명서](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)합니다.</span><span class="sxs-lookup"><span data-stu-id="c33e0-129">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
