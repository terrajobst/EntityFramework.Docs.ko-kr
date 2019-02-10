---
title: 로깅-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 0a996403afdbe076b1690c98eeb305b40c4d1f4a
ms.sourcegitcommit: 109a16478de498b65717a6e09be243647e217fb3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/10/2019
ms.locfileid: "55985576"
---
# <a name="logging"></a><span data-ttu-id="ab713-102">로깅</span><span class="sxs-lookup"><span data-stu-id="ab713-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="ab713-103">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="ab713-104">ASP.NET Core 응용 프로그램</span><span class="sxs-lookup"><span data-stu-id="ab713-104">ASP.NET Core applications</span></span>

<span data-ttu-id="ab713-105">EF Core는 ASP.NET Core의 로깅 메커니즘을 사용 하 여 자동으로 통합 될 때마다 `AddDbContext` 또는 `AddDbContextPool` 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="ab713-106">따라서 ASP.NET Core를 사용 하는 경우 로깅 구성 해야에 설명 된 대로 합니다 [ASP.NET Core 설명서](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)합니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="ab713-107">다른 응용 프로그램</span><span class="sxs-lookup"><span data-stu-id="ab713-107">Other applications</span></span>

<span data-ttu-id="ab713-108">현재 로깅 EF Core에는 하나 이상의 ILoggerProvider를 사용 하 여 구성 자체는 ILoggerFactory 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="ab713-109">일반적인 공급자 같은 패키지에 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="ab713-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): 간단한 콘솔으로 거를 합니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="ab713-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Azure App Services '진단 로그' 및 '로그 스트림' 기능을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="ab713-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): 디버거에 로그 System.Diagnostics.Debug.WriteLine()를 사용 하 여 모니터링 합니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="ab713-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Windows 이벤트 로그에 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="ab713-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="ab713-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): System.Diagnostics.TraceSource.TraceEvent()를 사용 하 여 추적 수신기에 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

> [!NOTE]
> <span data-ttu-id="ab713-116">다음 코드 샘플에서는 `ConsoleLoggerProvider` 생성자를 버전 2.2에서에서 사용 되지 않습니다 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-116">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2.</span></span> <span data-ttu-id="ab713-117">사용 되지 않는 로깅 Api에 대 한 적절 한 대체 버전 3.0에서에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-117">Proper replacements for obsolete logging APIs will be available in version 3.0.</span></span> <span data-ttu-id="ab713-118">그동안를 무시 하 고 경고를 표시 하지 않으려면 안전 합니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-118">In the meantime, it is safe to ignore and suppress the warnings.</span></span>

<span data-ttu-id="ab713-119">적절 한 패키지를 설치한 후 응용 프로그램을 LoggerFactory의 singleton/전역 인스턴스를 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-119">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="ab713-120">예를 들어 콘솔으로 거를 사용 하 여:</span><span class="sxs-lookup"><span data-stu-id="ab713-120">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="ab713-121">이 단일/전역 인스턴스 다음에 등록 해야 및 EF Core는 `DbContextOptionsBuilder`합니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-121">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="ab713-122">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="ab713-122">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="ab713-123">것이 매우 중요 한 응용 프로그램 각 컨텍스트 인스턴스에 대 한 새 ILoggerFactory 인스턴스를 만들지 마십시오입니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-123">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="ab713-124">이렇게 성능 저하 및 메모리 누수를 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-124">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="ab713-125">로깅되는 내용을 필터링 합니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-125">Filtering what is logged</span></span>

> [!NOTE]
> <span data-ttu-id="ab713-126">다음 코드 샘플에서는 `ConsoleLoggerProvider` 생성자를 버전 2.2에서에서 사용 되지 않습니다 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-126">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2.</span></span> <span data-ttu-id="ab713-127">사용 되지 않는 로깅 Api에 대 한 적절 한 대체 버전 3.0에서에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-127">Proper replacements for obsolete logging APIs will be available in version 3.0.</span></span> <span data-ttu-id="ab713-128">그동안를 무시 하 고 경고를 표시 하지 않으려면 안전 합니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-128">In the meantime, it is safe to ignore and suppress the warnings.</span></span>

<span data-ttu-id="ab713-129">로깅되는 내용을 필터링 하는 가장 쉬운 방법은 구성 하는 것은 ILoggerProvider를 등록 하는 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-129">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="ab713-130">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="ab713-130">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="ab713-131">이 예제에서는 로그 메시지에만 반환 하도록 필터링 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-131">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="ab713-132">'Microsoft.EntityFrameworkCore.Database.Command' 범주에</span><span class="sxs-lookup"><span data-stu-id="ab713-132">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="ab713-133">'정보' 수준</span><span class="sxs-lookup"><span data-stu-id="ab713-133">at the 'Information' level</span></span>

<span data-ttu-id="ab713-134">EF Core에 대 한로 거 범주에 정의 된는 `DbLoggerCategory` 간단한 문자열을 쉽게 있지만 범주를 찾을 수 있도록 클래스를 해결 합니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-134">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="ab713-135">기본 로깅 인프라에 대 한 자세한 내용은에서 찾을 수 있습니다 합니다 [ASP.NET Core 로깅 설명서](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)합니다.</span><span class="sxs-lookup"><span data-stu-id="ab713-135">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
