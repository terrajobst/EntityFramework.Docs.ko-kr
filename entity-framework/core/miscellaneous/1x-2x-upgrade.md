---
title: 이전 버전에서 EF Core 2-EF Core로 업그레이드
author: divega
ms.date: 08/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: b27c09fdb6210dd7c6aa0c8bc912a8bd183c16b9
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414232"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a><span data-ttu-id="979de-102">이전 버전의 응용 프로그램을 EF Core 2.0로 업그레이드</span><span class="sxs-lookup"><span data-stu-id="979de-102">Upgrading applications from previous versions to EF Core 2.0</span></span>

<span data-ttu-id="979de-103">2\.0에서 기존 Api 및 동작을 크게 구체화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-103">We have taken the opportunity to significantly refine our existing APIs and behaviors in 2.0.</span></span> <span data-ttu-id="979de-104">기존 응용 프로그램 코드를 수정 해야 할 수 있는 몇 가지 개선 사항이 있습니다. 그러나 대부분의 응용 프로그램에는 영향을 거의 없지만 대부분의 경우에는 사용 되지 않는 Api를 대체 하기 위해 다시 컴파일 및 최소한의 단계별 변경이 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-104">There are a few improvements that can require modifying existing application code, although we believe that for the majority of applications the impact will be low, in most cases requiring just recompilation and minimal guided changes to replace obsolete APIs.</span></span>

<span data-ttu-id="979de-105">기존 응용 프로그램을 EF Core 2.0으로 업데이트 하려면 다음이 필요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-105">Updating an existing application to EF Core 2.0 may require:</span></span>

1. <span data-ttu-id="979de-106">.NET Standard 2.0를 지 원하는 응용 프로그램에 대 한 대상 .NET 구현을 업그레이드 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-106">Upgrading the target .NET implementation of the application to one that supports .NET Standard 2.0.</span></span> <span data-ttu-id="979de-107">자세한 내용은 [지원 되는 .Net 구현](xref:core/platforms/index) 을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="979de-107">See [Supported .NET Implementations](xref:core/platforms/index) for more details.</span></span>

2. <span data-ttu-id="979de-108">EF Core 2.0와 호환 되는 대상 데이터베이스에 대 한 공급자를 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-108">Identify a provider for the target database which is compatible with EF Core 2.0.</span></span> <span data-ttu-id="979de-109">[EF Core 2.0을 보려면 아래 2.0 데이터베이스 공급자가 있어야](#ef-core-20-requires-a-20-database-provider) 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-109">See [EF Core 2.0 requires a 2.0 database provider](#ef-core-20-requires-a-20-database-provider) below.</span></span>

3. <span data-ttu-id="979de-110">모든 EF Core 패키지 (런타임 및 도구)를 2.0로 업그레이드 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-110">Upgrading all the EF Core packages (runtime and tooling) to 2.0.</span></span> <span data-ttu-id="979de-111">자세한 내용은 [EF Core 설치](xref:core/get-started/install/index) 를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="979de-111">Refer to [Installing EF Core](xref:core/get-started/install/index) for more details.</span></span>

4. <span data-ttu-id="979de-112">이 문서의 나머지 부분에서 설명 하는 주요 변경 내용을 보정 하기 위해 필요한 코드를 변경 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-112">Make any necessary code changes to compensate for the breaking changes described in the rest of this document.</span></span>

## <a name="aspnet-core-now-includes-ef-core"></a><span data-ttu-id="979de-113">현재 ASP.NET Core에는 EF Core</span><span class="sxs-lookup"><span data-stu-id="979de-113">ASP.NET Core now includes EF Core</span></span>

<span data-ttu-id="979de-114">ASP.NET Core 2.0을 대상으로 하는 애플리케이션은 추가적인 종속성 없이 타사 데이터베이스 공급자와 함께 EF Core 2.0을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-114">Applications targeting ASP.NET Core 2.0 can use EF Core 2.0 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="979de-115">그러나 이전 버전의 ASP.NET Core를 대상으로 하는 응용 프로그램은 EF Core 2.0를 사용 하기 위해 ASP.NET Core 2.0로 업그레이드 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-115">However, applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.0 in order to use EF Core 2.0.</span></span> <span data-ttu-id="979de-116">ASP.NET Core 응용 프로그램을 2.0로 업그레이드 하는 방법에 대 한 자세한 내용은 [주제에 대 한 ASP.NET Core 설명서를](/aspnet/core/migration/1x-to-2x/)참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="979de-116">For more details on upgrading ASP.NET Core applications to 2.0 see [the ASP.NET Core documentation on the subject](/aspnet/core/migration/1x-to-2x/).</span></span>

## <a name="new-way-of-getting-application-services-in-aspnet-core"></a><span data-ttu-id="979de-117">ASP.NET Core에서 응용 프로그램 서비스를 가져오는 새로운 방법</span><span class="sxs-lookup"><span data-stu-id="979de-117">New way of getting application services in ASP.NET Core</span></span>

<span data-ttu-id="979de-118">ASP.NET Core 웹 응용 프로그램에 권장 되는 패턴은 2.0에 사용 되는 디자인 타임 논리 EF Core 중단 된 방식으로에 대해 업데이트 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-118">The recommended pattern for ASP.NET Core web applications has been updated for 2.0 in a way that broke the design-time logic EF Core used in 1.x.</span></span> <span data-ttu-id="979de-119">이전에는 디자인 타임에 EF Core 응용 프로그램의 서비스 공급자에 액세스 하기 위해 `Startup.ConfigureServices`을 직접 호출 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-119">Previously at design-time, EF Core would try to invoke `Startup.ConfigureServices` directly in order to access the application's service provider.</span></span> <span data-ttu-id="979de-120">ASP.NET Core 2.0에서는 구성이 `Startup` 클래스 외부에서 초기화 됩니다.</span><span class="sxs-lookup"><span data-stu-id="979de-120">In ASP.NET Core 2.0, Configuration is initialized outside of the `Startup` class.</span></span> <span data-ttu-id="979de-121">EF Core를 사용 하는 응용 프로그램은 일반적으로 구성에서 연결 문자열에 액세스 하므로 `Startup` 자체가 더 이상 충분 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-121">Applications using EF Core typically access their connection string from Configuration, so `Startup` by itself is no longer sufficient.</span></span> <span data-ttu-id="979de-122">ASP.NET Core 1.x 응용 프로그램을 업그레이드 하는 경우 EF Core 도구를 사용할 때 다음과 같은 오류가 나타날 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-122">If you upgrade an ASP.NET Core 1.x application, you may receive the following error when using the EF Core tools.</span></span>

> <span data-ttu-id="979de-123">' ApplicationContext '에서 매개 변수가 없는 생성자를 찾을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-123">No parameterless constructor was found on 'ApplicationContext'.</span></span> <span data-ttu-id="979de-124">' Applicationcontext '에 매개 변수가 없는 생성자를 추가 하거나 ' IDesignTimeDbContextFactory&lt;ApplicationContext&gt;'의 구현을 ' ApplicationContext '와 같은 어셈블리에 추가 하십시오.</span><span class="sxs-lookup"><span data-stu-id="979de-124">Either add a parameterless constructor to 'ApplicationContext' or add an implementation of 'IDesignTimeDbContextFactory&lt;ApplicationContext&gt;' in the same assembly as 'ApplicationContext'</span></span>

<span data-ttu-id="979de-125">새 디자인 타임 후크가 ASP.NET Core 2.0의 기본 템플릿에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-125">A new design-time hook has been added in ASP.NET Core 2.0's default template.</span></span> <span data-ttu-id="979de-126">정적 `Program.BuildWebHost` 메서드를 사용 하면 EF Core를 통해 디자인 타임에 응용 프로그램의 서비스 공급자에 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-126">The static `Program.BuildWebHost` method enables EF Core to access the application's service provider at design time.</span></span> <span data-ttu-id="979de-127">ASP.NET Core 1.x 응용 프로그램을 업그레이드 하는 경우 `Program` 클래스를 다음과 같이 업데이트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-127">If you are upgrading an ASP.NET Core 1.x application, you will need to update the `Program` class to resemble the following.</span></span>

``` csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;

namespace AspNetCoreDotNetCore2._0App
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
}
```

<span data-ttu-id="979de-128">응용 프로그램을 2.0로 업데이트할 때이 새 패턴을 채택 하는 것이 좋으며 Entity Framework Core 마이그레이션과 같은 제품 기능을 사용 하려면 작업을 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-128">The adoption of this new pattern when updating applications to 2.0 is highly recommended and is required in order for product features like Entity Framework Core Migrations to work.</span></span> <span data-ttu-id="979de-129">다른 일반적인 대안은 [ *Idesigntimedbcontextfactory\<tcontext >* 를 구현](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory)하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="979de-129">The other common alternative is to [implement *IDesignTimeDbContextFactory\<TContext>*](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).</span></span>

## <a name="idbcontextfactory-renamed"></a><span data-ttu-id="979de-130">IDbContextFactory 이름 바꾸기</span><span class="sxs-lookup"><span data-stu-id="979de-130">IDbContextFactory renamed</span></span>

<span data-ttu-id="979de-131">다양 한 응용 프로그램 패턴을 지원 하 고 사용자가 디자인 타임에 `DbContext`를 사용 하는 방식을 더 잘 제어할 수 있도록 `IDbContextFactory<TContext>` 인터페이스가 제공 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-131">In order to support diverse application patterns and give users more control over how their `DbContext` is used at design time, we have, in the past, provided the `IDbContextFactory<TContext>` interface.</span></span> <span data-ttu-id="979de-132">디자인 타임에 EF Core 도구는 프로젝트에서이 인터페이스의 구현을 검색 하 여 `DbContext` 개체를 만드는 데 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-132">At design-time, the EF Core tools will discover implementations of this interface in your project and use it to create `DbContext` objects.</span></span>

<span data-ttu-id="979de-133">이 인터페이스에는 일부 사용자가 다른 `DbContext`만들기 시나리오에 대해 다시 사용 하려고 하지 않는 매우 일반적인 이름이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-133">This interface had a very general name which mislead some users to try re-using it for other `DbContext`-creating scenarios.</span></span> <span data-ttu-id="979de-134">EF 도구는 디자인 타임에 구현을 사용 하려고 시도 하 고 `Update-Database` 또는 `dotnet ef database update` 같은 명령이 실패 하는 경우 가드를 방지 했습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-134">They were caught off guard when the EF Tools then tried to use their implementation at design-time and caused commands like `Update-Database` or `dotnet ef database update` to fail.</span></span>

<span data-ttu-id="979de-135">이 인터페이스의 강력한 디자인 타임 의미 체계를 전달 하기 위해 `IDesignTimeDbContextFactory<TContext>`로 이름을 바꿨습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-135">In order to communicate the strong design-time semantics of this interface, we have renamed it to `IDesignTimeDbContextFactory<TContext>`.</span></span>

<span data-ttu-id="979de-136">2\.0 릴리스는 `IDbContextFactory<TContext>` 있지만 아직 사용 되지 않은 것으로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="979de-136">For the 2.0 release the `IDbContextFactory<TContext>` still exists but is marked as obsolete.</span></span>

## <a name="dbcontextfactoryoptions-removed"></a><span data-ttu-id="979de-137">DbContextFactoryOptions 제거 됨</span><span class="sxs-lookup"><span data-stu-id="979de-137">DbContextFactoryOptions removed</span></span>

<span data-ttu-id="979de-138">위에서 설명한 ASP.NET Core 2.0 변경 사항으로 인해 `DbContextFactoryOptions` 새 `IDesignTimeDbContextFactory<TContext>` 인터페이스에서 더 이상 필요 하지 않은 것을 발견 했습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-138">Because of the ASP.NET Core 2.0 changes described above, we found that `DbContextFactoryOptions` was no longer needed on the new `IDesignTimeDbContextFactory<TContext>` interface.</span></span> <span data-ttu-id="979de-139">대신를 사용 해야 하는 대체 항목은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-139">Here are the alternatives you should be using instead.</span></span>

| <span data-ttu-id="979de-140">DbContextFactoryOptions</span><span class="sxs-lookup"><span data-stu-id="979de-140">DbContextFactoryOptions</span></span> | <span data-ttu-id="979de-141">대체</span><span class="sxs-lookup"><span data-stu-id="979de-141">Alternative</span></span>                                                  |
|:------------------------|:-------------------------------------------------------------|
| <span data-ttu-id="979de-142">ApplicationBasePath</span><span class="sxs-lookup"><span data-stu-id="979de-142">ApplicationBasePath</span></span>     | <span data-ttu-id="979de-143">AppContext.BaseDirectory</span><span class="sxs-lookup"><span data-stu-id="979de-143">AppContext.BaseDirectory</span></span>                                     |
| <span data-ttu-id="979de-144">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="979de-144">ContentRootPath</span></span>         | <span data-ttu-id="979de-145">Directory.GetCurrentDirectory()</span><span class="sxs-lookup"><span data-stu-id="979de-145">Directory.GetCurrentDirectory()</span></span>                              |
| <span data-ttu-id="979de-146">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="979de-146">EnvironmentName</span></span>         | <span data-ttu-id="979de-147">Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT")</span><span class="sxs-lookup"><span data-stu-id="979de-147">Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT")</span></span> |

## <a name="design-time-working-directory-changed"></a><span data-ttu-id="979de-148">디자인 타임 작업 디렉터리가 변경 됨</span><span class="sxs-lookup"><span data-stu-id="979de-148">Design-time working directory changed</span></span>

<span data-ttu-id="979de-149">ASP.NET Core 2.0 변경 내용은 응용 프로그램을 실행할 때 Visual Studio에서 사용 하는 작업 디렉터리에 맞게 `dotnet ef`에서 사용 하는 작업 디렉터리에도 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-149">The ASP.NET Core 2.0 changes also required the working directory used by `dotnet ef` to align with the working directory used by Visual Studio when running your application.</span></span> <span data-ttu-id="979de-150">이에 대 한 한 가지 관찰 가능한 부작용은 이제 SQLite 파일 이름이 프로젝트 디렉터리를 기준으로 하는 것과 같은 출력 디렉터리가 아니라는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="979de-150">One observable side effect of this is that SQLite filenames are now relative to the project directory and not the output directory like they used to be.</span></span>

## <a name="ef-core-20-requires-a-20-database-provider"></a><span data-ttu-id="979de-151">EF Core 2.0에는 2.0 데이터베이스 공급자가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-151">EF Core 2.0 requires a 2.0 database provider</span></span>

<span data-ttu-id="979de-152">EF Core 2.0의 경우 데이터베이스 공급자가 작동 하는 방식에서 많은 단순화 및 향상 된 기능을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-152">For EF Core 2.0 we have made many simplifications and improvements in the way database providers work.</span></span> <span data-ttu-id="979de-153">즉, 1.0. x 및 1.1. x 공급자는 EF Core 2.0에서 작동 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-153">This means that 1.0.x and 1.1.x providers will not work with EF Core 2.0.</span></span>

<span data-ttu-id="979de-154">SQL Server 및 SQLite 공급자는 EF 팀에서 배송 되며 2.0 버전은 2.0 릴리스의 일부로 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="979de-154">The SQL Server and SQLite providers are shipped by the EF team and 2.0 versions will be available as part of the 2.0 release.</span></span> <span data-ttu-id="979de-155">[SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL)및 [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) 에 대 한 오픈 소스 타사 공급자가 2.0에 대해 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="979de-155">The open-source third party providers for [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), and [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) are being updated for 2.0.</span></span> <span data-ttu-id="979de-156">다른 모든 공급자의 경우 공급자 작성자에 게 문의 하세요.</span><span class="sxs-lookup"><span data-stu-id="979de-156">For all other providers, please contact the provider writer.</span></span>

## <a name="logging-and-diagnostics-events-have-changed"></a><span data-ttu-id="979de-157">로깅 및 진단 이벤트가 변경 됨</span><span class="sxs-lookup"><span data-stu-id="979de-157">Logging and Diagnostics events have changed</span></span>

<span data-ttu-id="979de-158">참고: 이러한 변경 내용은 대부분의 응용 프로그램 코드에 영향을 주지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-158">Note: these changes should not impact most application code.</span></span>

<span data-ttu-id="979de-159">[ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) 전송 된 메시지에 대 한 이벤트 id가 2.0에서 변경 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-159">The event IDs for messages sent to an [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) have changed in 2.0.</span></span> <span data-ttu-id="979de-160">이제 이벤트 ID가 EF Core 코드 전체에서 고유합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-160">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="979de-161">또한 이 메시지가 MVC 등에서 사용되는 구조화된 로깅의 표준 패턴을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="979de-161">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="979de-162">로거 범주도 변경되었습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-162">Logger categories have also changed.</span></span> <span data-ttu-id="979de-163">이제 [DbLoggerCategory](https://github.com/aspnet/EntityFrameworkCore/blob/rel/2.0.0/src/EFCore/DbLoggerCategory.cs)를 통해 액세스되는 잘 알려진 범주 집합입니다.</span><span class="sxs-lookup"><span data-stu-id="979de-163">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFrameworkCore/blob/rel/2.0.0/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="979de-164">[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) events는 이제 해당 `ILogger` 메시지와 동일한 이벤트 ID 이름을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-164">[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) events now use the same event ID names as the corresponding `ILogger` messages.</span></span> <span data-ttu-id="979de-165">이벤트 페이로드는 [EventData](/dotnet/api/microsoft.entityframeworkcore.diagnostics.eventdata)에서 파생 된 모든 명목상 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="979de-165">The event payloads are all nominal types derived from [EventData](/dotnet/api/microsoft.entityframeworkcore.diagnostics.eventdata).</span></span>

<span data-ttu-id="979de-166">이벤트 Id, 페이로드 유형 및 범주는 [CoreEventId](/dotnet/api/microsoft.entityframeworkcore.diagnostics.coreeventid) 및 [RelationalEventId](/dotnet/api/microsoft.entityframeworkcore.diagnostics.relationaleventid) 클래스에 설명 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-166">Event IDs, payload types, and categories are documented in the [CoreEventId](/dotnet/api/microsoft.entityframeworkcore.diagnostics.coreeventid) and the [RelationalEventId](/dotnet/api/microsoft.entityframeworkcore.diagnostics.relationaleventid) classes.</span></span>

<span data-ttu-id="979de-167">Id도 Microsoft.entityframeworkcore.tools.dotnet에서 새 Microsoft.entityframeworkcore.tools.dotnet 네임 스페이스로 이동 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-167">IDs have also moved from Microsoft.EntityFrameworkCore.Infrastructure to the new Microsoft.EntityFrameworkCore.Diagnostics namespace.</span></span>

## <a name="ef-core-relational-metadata-api-changes"></a><span data-ttu-id="979de-168">EF Core 관계형 메타 데이터 API 변경 내용</span><span class="sxs-lookup"><span data-stu-id="979de-168">EF Core relational metadata API changes</span></span>

<span data-ttu-id="979de-169">이제 EF Core 2.0은 사용하는 각각의 다른 공급자마다 다른 [IModel](/dotnet/api/microsoft.entityframeworkcore.metadata.imodel)을 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-169">EF Core 2.0 will now build a different [IModel](/dotnet/api/microsoft.entityframeworkcore.metadata.imodel) for each different provider being used.</span></span> <span data-ttu-id="979de-170">일반적으로 애플리케이션에 투명합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-170">This is usually transparent to the application.</span></span> <span data-ttu-id="979de-171">이를 통해 _공통 관계형 메타 데이터 개념_ 에 대 한 액세스는 항상 `.SqlServer`, `.Sqlite`등의 `.Relational`에 대 한 호출을 통해 수행 되도록 낮은 수준의 메타 데이터 api가 간소화 됩니다. 예를 들어, 1.1. x 코드는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-171">This has facilitated a simplification of lower-level metadata APIs such that any access to _common relational metadata concepts_ is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc. For example, 1.1.x code like this:</span></span>

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

<span data-ttu-id="979de-172">이제 다음과 같이 작성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="979de-172">Should now be written like this:</span></span>

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

<span data-ttu-id="979de-173">`ForSqlServerToTable`와 같은 메서드를 사용 하는 대신 확장 메서드를 사용 하 여 현재 사용 중인 공급자에 따라 조건부 코드를 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-173">Instead of using methods like `ForSqlServerToTable`, extension methods are now available to write conditional code based on the current provider in use.</span></span> <span data-ttu-id="979de-174">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-174">For example:</span></span>

```csharp
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

<span data-ttu-id="979de-175">이 변경은 _모든_ 관계형 공급자에 대해 정의 된 api/메타 데이터에만 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="979de-175">Note that this change only applies to APIs/metadata that is defined for _all_ relational providers.</span></span> <span data-ttu-id="979de-176">API와 메타 데이터는 단일 공급자에만 해당 되는 경우 동일 하 게 유지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="979de-176">The API and metadata remains the same when it is specific to only a single provider.</span></span> <span data-ttu-id="979de-177">예를 들어 클러스터형 인덱스는 SQL Server와 관련이 있으므로 `ForSqlServerIsClustered` 및 `.SqlServer().IsClustered()`를 계속 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-177">For example, clustered indexes are specific to SQL Sever, so `ForSqlServerIsClustered` and  `.SqlServer().IsClustered()` must still be used.</span></span>

## <a name="dont-take-control-of-the-ef-service-provider"></a><span data-ttu-id="979de-178">EF 서비스 공급자를 제어 하지 않음</span><span class="sxs-lookup"><span data-stu-id="979de-178">Don’t take control of the EF service provider</span></span>

<span data-ttu-id="979de-179">EF Core는 내부 구현에 대 한 내부 `IServiceProvider` (종속성 주입 컨테이너)를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-179">EF Core uses an internal `IServiceProvider` (a dependency injection container) for its internal implementation.</span></span> <span data-ttu-id="979de-180">응용 프로그램에서는 특수 한 경우를 제외 하 고는 EF Core이 공급자를 만들고 관리할 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-180">Applications should allow EF Core to create and manage this provider except in special cases.</span></span> <span data-ttu-id="979de-181">`UseInternalServiceProvider`에 대 한 모든 호출을 제거 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-181">Strongly consider removing any calls to `UseInternalServiceProvider`.</span></span> <span data-ttu-id="979de-182">응용 프로그램에서 `UseInternalServiceProvider`를 호출 해야 하는 경우에는 시나리오를 처리 하는 다른 방법을 조사할 수 있도록 문제를 처리 하 [는](https://github.com/aspnet/EntityFramework/Issues) 것을 고려 하세요.</span><span class="sxs-lookup"><span data-stu-id="979de-182">If an application does need to call `UseInternalServiceProvider`, then please consider [filing an issue](https://github.com/aspnet/EntityFramework/Issues) so we can investigate other ways to handle your scenario.</span></span>

<span data-ttu-id="979de-183">`UseInternalServiceProvider`를 호출 하지 않으면 `AddEntityFramework`, `AddEntityFrameworkSqlServer`등의 호출은 응용 프로그램 코드에서 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-183">Calling `AddEntityFramework`, `AddEntityFrameworkSqlServer`, etc. is not required by application code unless `UseInternalServiceProvider` is also called.</span></span> <span data-ttu-id="979de-184">`AddEntityFramework` 또는 `AddEntityFrameworkSqlServer`에 대 한 기존 호출을 제거 합니다. `AddDbContext`와 동일한 방식으로 계속 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-184">Remove any existing calls to `AddEntityFramework` or `AddEntityFrameworkSqlServer`, etc. `AddDbContext` should still be used in the same way as before.</span></span>

## <a name="in-memory-databases-must-be-named"></a><span data-ttu-id="979de-185">메모리 내 데이터베이스의 이름을 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-185">In-memory databases must be named</span></span>

<span data-ttu-id="979de-186">명명 되지 않은 전역 메모리 내 데이터베이스가 제거 되었으며 대신 모든 메모리 내 데이터베이스의 이름을 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-186">The global unnamed in-memory database has been removed and instead all in-memory databases must be named.</span></span> <span data-ttu-id="979de-187">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-187">For example:</span></span>

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

<span data-ttu-id="979de-188">그러면 이름이 "MyDatabase" 인 데이터베이스가 생성/사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="979de-188">This creates/uses a database with the name “MyDatabase”.</span></span> <span data-ttu-id="979de-189">동일한 이름을 사용 하 여 `UseInMemoryDatabase`을 다시 호출 하면 동일한 메모리 내 데이터베이스가 사용 되어 여러 컨텍스트 인스턴스에서 공유할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-189">If `UseInMemoryDatabase` is called again with the same name, then the same in-memory database will be used, allowing it to be shared by multiple context instances.</span></span>

## <a name="read-only-api-changes"></a><span data-ttu-id="979de-190">읽기 전용 API 변경</span><span class="sxs-lookup"><span data-stu-id="979de-190">Read-only API changes</span></span>

<span data-ttu-id="979de-191">`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`및 `IsStoreGeneratedAlways`는 사용 되지 않으며 [BeforeSaveBehavior](/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.beforesavebehavior) 및 [aftersavebehavior](/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.aftersavebehavior)로 대체 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-191">`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`, and `IsStoreGeneratedAlways` have been obsoleted and replaced with [BeforeSaveBehavior](/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.beforesavebehavior) and [AfterSaveBehavior](/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.aftersavebehavior).</span></span> <span data-ttu-id="979de-192">이러한 동작은 저장소에 생성 된 속성 뿐만 아니라 모든 속성에 적용 되며, 데이터베이스 행에 삽입 (`BeforeSaveBehavior`) 하거나 기존 데이터베이스 행 (`AfterSaveBehavior`)을 업데이트할 때 속성의 값을 사용 하는 방법을 결정 합니다.</span><span class="sxs-lookup"><span data-stu-id="979de-192">These behaviors apply to any property (not only store-generated properties) and determine how the value of the property should be used when inserting into a database row (`BeforeSaveBehavior`) or when updating an existing database row (`AfterSaveBehavior`).</span></span>

<span data-ttu-id="979de-193">Valuegenerated로 표시 된 속성 (예: 계산 열)은 기본적으로 속성에 설정 된 모든 값을 무시 [합니다.](/dotnet/api/microsoft.entityframeworkcore.metadata.valuegenerated)</span><span class="sxs-lookup"><span data-stu-id="979de-193">Properties marked as [ValueGenerated.OnAddOrUpdate](/dotnet/api/microsoft.entityframeworkcore.metadata.valuegenerated) (for example, for computed columns) will by default ignore any value currently set on the property.</span></span> <span data-ttu-id="979de-194">즉, 추적 된 엔터티에서 값이 설정 또는 수정 되었는지 여부에 관계 없이 항상 저장소에서 생성 된 값을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="979de-194">This means that a store-generated value will always be obtained regardless of whether any value has been set or modified on the tracked entity.</span></span> <span data-ttu-id="979de-195">다른 `Before\AfterSaveBehavior`를 설정 하 여 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-195">This can be changed by setting a different `Before\AfterSaveBehavior`.</span></span>

## <a name="new-clientsetnull-delete-behavior"></a><span data-ttu-id="979de-196">새 ClientSetNull 삭제 동작</span><span class="sxs-lookup"><span data-stu-id="979de-196">New ClientSetNull delete behavior</span></span>

<span data-ttu-id="979de-197">이전 릴리스에서는 [Deletebehavior](/dotnet/api/microsoft.entityframeworkcore.deletebehavior) 와 일치 하는 `SetNull` 의미 체계가 더 폐쇄형 컨텍스트에 의해 추적 되는 엔터티에 대 한 동작을 제한 했습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-197">In previous releases, [DeleteBehavior.Restrict](/dotnet/api/microsoft.entityframeworkcore.deletebehavior) had a behavior for entities tracked by the context that more closed matched `SetNull` semantics.</span></span> <span data-ttu-id="979de-198">EF Core 2.0에서는 선택적 관계에 대 한 기본값으로 새로운 `ClientSetNull` 동작이 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-198">In EF Core 2.0, a new `ClientSetNull` behavior has been introduced as the default for optional relationships.</span></span> <span data-ttu-id="979de-199">이 동작에는 추적 된 엔터티에 대 한 `SetNull` 의미 체계와 EF Core를 사용 하 여 만든 데이터베이스에 대 한 `Restrict` 동작이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-199">This behavior has `SetNull` semantics for tracked entities and `Restrict` behavior for databases created using EF Core.</span></span> <span data-ttu-id="979de-200">경험에 따르면 추적 되는 엔터티와 데이터베이스에 대해 가장 예상 되 고 유용한 동작이 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="979de-200">In our experience, these are the most expected/useful behaviors for tracked entities and the database.</span></span> <span data-ttu-id="979de-201">`DeleteBehavior.Restrict`는 선택적 관계에 대해 설정 된 경우 추적 된 엔터티에 대해 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="979de-201">`DeleteBehavior.Restrict` is now honored for tracked entities when set for optional relationships.</span></span>

## <a name="provider-design-time-packages-removed"></a><span data-ttu-id="979de-202">공급자 디자인 타임 패키지가 제거 됨</span><span class="sxs-lookup"><span data-stu-id="979de-202">Provider design-time packages removed</span></span>

<span data-ttu-id="979de-203">`Microsoft.EntityFrameworkCore.Relational.Design` 패키지가 제거 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-203">The `Microsoft.EntityFrameworkCore.Relational.Design` package has been removed.</span></span> <span data-ttu-id="979de-204">콘텐츠는 `Microsoft.EntityFrameworkCore.Relational` 및 `Microsoft.EntityFrameworkCore.Design`에 통합 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="979de-204">It's contents were consolidated into `Microsoft.EntityFrameworkCore.Relational` and `Microsoft.EntityFrameworkCore.Design`.</span></span>

<span data-ttu-id="979de-205">이는 공급자 디자인 타임 패키지로 전파 됩니다.</span><span class="sxs-lookup"><span data-stu-id="979de-205">This propagates into the provider design-time packages.</span></span> <span data-ttu-id="979de-206">이러한 패키지 (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`등)가 제거 되 고 해당 콘텐츠가 주 공급자 패키지에 통합 됩니다.</span><span class="sxs-lookup"><span data-stu-id="979de-206">Those packages (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`, etc.) were removed and their contents consolidated into the main provider packages.</span></span>

<span data-ttu-id="979de-207">2\.0 EF Core에서 `Scaffold-DbContext` 또는 `dotnet ef dbcontext scaffold`를 사용 하도록 설정 하려면 단일 공급자 패키지를 참조 하기만 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="979de-207">To enable `Scaffold-DbContext` or `dotnet ef dbcontext scaffold` in EF Core 2.0, you only need to reference the single provider package:</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
