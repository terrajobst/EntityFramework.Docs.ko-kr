---
title: EF Core 2-EF Core에 이전 버전에서 업그레이드
author: divega
ms.author: divega
ms.date: 8/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
ms.technology: entity-framework-core
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 9be2368159fe7ab7b6951cc14a84ee63762ce90c
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39388454"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a><span data-ttu-id="fcff4-102">응용 프로그램을 이전 버전에서 EF Core 2.0으로 업그레이드</span><span class="sxs-lookup"><span data-stu-id="fcff4-102">Upgrading applications from previous versions to EF Core 2.0</span></span>

## <a name="procedures-common-to-all-applications"></a><span data-ttu-id="fcff4-103">모든 응용 프로그램에 공통 된 프로시저</span><span class="sxs-lookup"><span data-stu-id="fcff4-103">Procedures Common to All Applications</span></span>

<span data-ttu-id="fcff4-104">EF Core 2.0으로 기존 응용 프로그램을 업데이트 해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-104">Updating an existing application to EF Core 2.0 may require:</span></span>

1. <span data-ttu-id="fcff4-105">.NET Standard 2.0을 지 원하는 드라이버로 응용 프로그램의 대상.NET 플랫폼을 업그레이드 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-105">Upgrading the target .NET platform of the application to one that supports .NET Standard 2.0.</span></span> <span data-ttu-id="fcff4-106">참조 [지원 되는 플랫폼](../platforms/index.md) 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-106">See [Supported Platforms](../platforms/index.md) for more details.</span></span>

2. <span data-ttu-id="fcff4-107">EF Core 2.0 호환 되는 대상 데이터베이스에 대 한 공급자를 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-107">Identify a provider for the target database which is compatible with EF Core 2.0.</span></span> <span data-ttu-id="fcff4-108">참조 [2.0 데이터베이스 공급자를 필요로 하는 EF Core 2.0](#ef-core-20-requires-a-20-database-provider) 아래.</span><span class="sxs-lookup"><span data-stu-id="fcff4-108">See [EF Core 2.0 requires a 2.0 database provider](#ef-core-20-requires-a-20-database-provider) below.</span></span>

3. <span data-ttu-id="fcff4-109">모든 EF Core 패키지 (런타임 및 도구) 2.0으로 업그레이드 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-109">Upgrading all the EF Core packages (runtime and tooling) to 2.0.</span></span> <span data-ttu-id="fcff4-110">가리킵니다 [EF Core 설치](../get-started/install/index.md) 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-110">Refer to [Installing EF Core](../get-started/install/index.md) for more details.</span></span>

4. <span data-ttu-id="fcff4-111">주요 변경 내용에 대 한 보정을 위해 필요한 코드를 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-111">Make any necessary code changes to compensate for breaking changes.</span></span> <span data-ttu-id="fcff4-112">참조 된 [주요 변경 내용](#breaking-changes) 대 한 자세한 내용은 아래 섹션입니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-112">See the [Breaking Changes](#breaking-changes) section below for more details.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="fcff4-113">ASP.NET Core 응용 프로그램</span><span class="sxs-lookup"><span data-stu-id="fcff4-113">ASP.NET Core applications</span></span>

1. <span data-ttu-id="fcff4-114">특히 참조를 [응용 프로그램의 서비스 공급자를 초기화 하기 위한 새로운 패턴](#new-way-of-getting-application-services) 아래에서 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-114">See in particular the [new pattern for initializing the application's service provider](#new-way-of-getting-application-services) described below.</span></span>

> [!TIP]  
> <span data-ttu-id="fcff4-115">적극 권장 되며 Entity Framework Core 마이그레이션은 같은 제품 기능 작동 하려면에서 반드시 2.0 응용 프로그램을 업데이트 하는 경우이 새 패턴의 도입은 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-115">The adoption of this new pattern when updating applications to 2.0 is highly recommended and is required in order for product features like Entity Framework Core Migrations to work.</span></span> <span data-ttu-id="fcff4-116">다른 일반적인 대체 방법은 [구현 *IDesignTimeDbContextFactory\<TContext >*](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory)합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-116">The other common alternative is to [implement *IDesignTimeDbContextFactory\<TContext>*](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).</span></span>

2. <span data-ttu-id="fcff4-117">ASP.NET Core 2.0을 대상으로 하는 응용 프로그램은 추가적인 종속성 없이 타사 데이터베이스 공급자와 함께 EF Core 2.0을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-117">Applications targeting ASP.NET Core 2.0 can use EF Core 2.0 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="fcff4-118">그러나 이전 버전의 ASP.NET Core를 대상으로 하는 응용 프로그램을 EF Core 2.0을 사용 하려면 ASP.NET Core 2.0으로 업그레이드 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-118">However, applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.0 in order to use EF Core 2.0.</span></span> <span data-ttu-id="fcff4-119">ASP.NET Core 2.0 응용 프로그램 업그레이드에 대 한 자세한 내용은 참조 하세요 [주제에 대 한 ASP.NET Core 설명서](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/)합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-119">For more details on upgrading ASP.NET Core applications to 2.0 see [the ASP.NET Core documentation on the subject](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).</span></span>

## <a name="breaking-changes"></a><span data-ttu-id="fcff4-120">주요 변경 사항</span><span class="sxs-lookup"><span data-stu-id="fcff4-120">Breaking Changes</span></span>

<span data-ttu-id="fcff4-121">기존 Api 및 2.0의 동작이 크게 구체화할 수를 이동 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-121">We have taken the opportunity to significantly refine our existing APIs and behaviors in 2.0.</span></span> <span data-ttu-id="fcff4-122">기존 응용 프로그램 코드 수정 필요할 수 있는 몇 가지 향상 된 기능는 대부분의 응용 프로그램에 대 한 생각 하지만 영향을 다시 컴파일 및 사용 되지 않는 Api를 바꾸려면 단계별된 최소한의 변경만 필요한 대부분의 경우 낮은 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-122">There are a few improvements that can require modifying existing application code, although we believe that for the majority of applications the impact will be low, in most cases requiring just recompilation and minimal guided changes to replace obsolete APIs.</span></span>

### <a name="new-way-of-getting-application-services"></a><span data-ttu-id="fcff4-123">새 응용 프로그램 서비스를 가져오는 방법</span><span class="sxs-lookup"><span data-stu-id="fcff4-123">New way of getting application services</span></span>

<span data-ttu-id="fcff4-124">ASP.NET Core 웹 응용 프로그램에 대 한 권장된 패턴 2.0 EF Core 1.x에서 사용 되는 디자인 타임 논리를 중단 하는 방식에서에 대 한 업데이트 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-124">The recommended pattern for ASP.NET Core web applications has been updated for 2.0 in a way that broke the design-time logic EF Core used in 1.x.</span></span> <span data-ttu-id="fcff4-125">이전에 디자인 타임에 EF Core 하려고 호출 `Startup.ConfigureServices` 응용 프로그램의 서비스 공급자에 액세스 하기 위해 직접.</span><span class="sxs-lookup"><span data-stu-id="fcff4-125">Previously at design-time, EF Core would try to invoke `Startup.ConfigureServices` directly in order to access the application's service provider.</span></span> <span data-ttu-id="fcff4-126">외부의 ASP.NET Core 2.0에서 구성 초기화 되는 `Startup` 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-126">In ASP.NET Core 2.0, Configuration is initialized outside of the `Startup` class.</span></span> <span data-ttu-id="fcff4-127">일반적으로 EF Core를 사용 하 여 응용 프로그램 구성에서 하므로 해당 연결 문자열을 액세스할 `Startup` 자체적으로 충분 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-127">Applications using EF Core typically access their connection string from Configuration, so `Startup` by itself is no longer sufficient.</span></span> <span data-ttu-id="fcff4-128">ASP.NET Core 1.x 응용 프로그램을 업그레이드 하는 경우 EF Core 도구를 사용 하는 경우 다음 오류가 나타날 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-128">If you upgrade an ASP.NET Core 1.x application, you may receive the following error when using the EF Core tools.</span></span>

> <span data-ttu-id="fcff4-129">매개 변수가 없는 생성자가 없습니다 'ApplicationContext'에서 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-129">No parameterless constructor was found on 'ApplicationContext'.</span></span> <span data-ttu-id="fcff4-130">'ApplicationContext' 매개 변수가 없는 생성자를 추가 하거나 구현 추가 ' IDesignTimeDbContextFactory&lt;ApplicationContext&gt;' 'ApplicationContext'와 동일한 어셈블리에서</span><span class="sxs-lookup"><span data-stu-id="fcff4-130">Either add a parameterless constructor to 'ApplicationContext' or add an implementation of 'IDesignTimeDbContextFactory&lt;ApplicationContext&gt;' in the same assembly as 'ApplicationContext'</span></span>

<span data-ttu-id="fcff4-131">새 디자인 타임 후크를 ASP.NET Core 2.0의 기본 서식 파일에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-131">A new design-time hook has been added in ASP.NET Core 2.0's default template.</span></span> <span data-ttu-id="fcff4-132">정적 `Program.BuildWebHost` 메서드는 디자인 타임에 응용 프로그램의 서비스 공급자에 액세스 하는 EF Core를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-132">The static `Program.BuildWebHost` method enables EF Core to access the application's service provider at design time.</span></span> <span data-ttu-id="fcff4-133">ASP.NET Core 1.x 응용 프로그램을 업그레이드 하는 경우 업데이트 해야 합니다 `Program` 클래스는 다음과 유사 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-133">If you are upgrading an ASP.NET Core 1.x application, you will need to update you `Program` class to resemble the following.</span></span>

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

### <a name="idbcontextfactory-renamed"></a><span data-ttu-id="fcff4-134">이름을 바꿀 IDbContextFactory</span><span class="sxs-lookup"><span data-stu-id="fcff4-134">IDbContextFactory renamed</span></span>

<span data-ttu-id="fcff4-135">다양 한 응용 프로그램 패턴을 지원 하 고 사용자에 게는 방법 더 제어를 제공 하기 위해 자신의 `DbContext` 는 디자인 타임에, 이전에 준비 했습니다는 `IDbContextFactory<TContext>` 인터페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-135">In order to support diverse application patterns and give users more control over how their `DbContext` is used at design time, we have, in the past, provided the `IDbContextFactory<TContext>` interface.</span></span> <span data-ttu-id="fcff4-136">디자인 타임에 EF Core 도구는 프로젝트에서이 인터페이스의 구현을 찾아 사용 하 여 만들 `DbContext` 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-136">At design-time, the EF Core tools will discover implementations of this interface in your project and use it to create `DbContext` objects.</span></span>

<span data-ttu-id="fcff4-137">이 인터페이스는 다시 다른를 불러 일부는 매우 일반적인 이름을 `DbContext`-시나리오 만들기.</span><span class="sxs-lookup"><span data-stu-id="fcff4-137">This interface had a very general name which mislead some users to try re-using it for other `DbContext`-creating scenarios.</span></span> <span data-ttu-id="fcff4-138">EF 도구는 다음 디자인 타임에 해당 구현을 사용 하려고 할 때 침착 하 고 같은 명령을 발생 되었습니다 `Update-Database` 또는 `dotnet ef database update` 실패 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-138">They were caught off guard when the EF Tools then tried to use their implementation at design-time and caused commands like `Update-Database` or `dotnet ef database update` to fail.</span></span>

<span data-ttu-id="fcff4-139">이 인터페이스의 강력한 디자인 타임 의미 체계와 통신 하기 위해 하였습니다 되도록 `IDesignTimeDbContextFactory<TContext>`합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-139">In order to communicate the strong design-time semantics of this interface, we have renamed it to `IDesignTimeDbContextFactory<TContext>`.</span></span>

<span data-ttu-id="fcff4-140">2.0 릴리스는 `IDbContextFactory<TContext>` 여전히 존재 하지만 되지 않음으로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-140">For the 2.0 release the `IDbContextFactory<TContext>` still exists but is marked as obsolete.</span></span>

### <a name="dbcontextfactoryoptions-removed"></a><span data-ttu-id="fcff4-141">DbContextFactoryOptions 제거</span><span class="sxs-lookup"><span data-stu-id="fcff4-141">DbContextFactoryOptions removed</span></span>

<span data-ttu-id="fcff4-142">위에서 설명한 ASP.NET Core 2.0 변경으로 인해 알게 하 `DbContextFactoryOptions` 새 필요 없게 된 `IDesignTimeDbContextFactory<TContext>` 인터페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-142">Because of the ASP.NET Core 2.0 changes described above, we found that `DbContextFactoryOptions` was no longer needed on the new `IDesignTimeDbContextFactory<TContext>` interface.</span></span> <span data-ttu-id="fcff4-143">대신 사용 해야 하는 대안을 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-143">Here are the alternatives you should be using instead.</span></span>

| <span data-ttu-id="fcff4-144">DbContextFactoryOptions</span><span class="sxs-lookup"><span data-stu-id="fcff4-144">DbContextFactoryOptions</span></span> | <span data-ttu-id="fcff4-145">대체</span><span class="sxs-lookup"><span data-stu-id="fcff4-145">Alternative</span></span>                                                  |
|:------------------------|:-------------------------------------------------------------|
| <span data-ttu-id="fcff4-146">ApplicationBasePath</span><span class="sxs-lookup"><span data-stu-id="fcff4-146">ApplicationBasePath</span></span>     | <span data-ttu-id="fcff4-147">AppContext.BaseDirectory</span><span class="sxs-lookup"><span data-stu-id="fcff4-147">AppContext.BaseDirectory</span></span>                                     |
| <span data-ttu-id="fcff4-148">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="fcff4-148">ContentRootPath</span></span>         | <span data-ttu-id="fcff4-149">Directory.GetCurrentDirectory()</span><span class="sxs-lookup"><span data-stu-id="fcff4-149">Directory.GetCurrentDirectory()</span></span>                              |
| <span data-ttu-id="fcff4-150">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="fcff4-150">EnvironmentName</span></span>         | <span data-ttu-id="fcff4-151">Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT")</span><span class="sxs-lookup"><span data-stu-id="fcff4-151">Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT")</span></span> |

### <a name="design-time-working-directory-changed"></a><span data-ttu-id="fcff4-152">디자인 타임 작업 디렉터리가 변경 되었음</span><span class="sxs-lookup"><span data-stu-id="fcff4-152">Design-time working directory changed</span></span>

<span data-ttu-id="fcff4-153">또한 필요한 작업 디렉터리에서 사용 하는 ASP.NET Core 2.0 변경 `dotnet ef` 응용 프로그램을 실행 하는 경우에 Visual Studio를 사용한 작업 디렉터리에 맞춰집니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-153">The ASP.NET Core 2.0 changes also required the working directory used by `dotnet ef` to align with the working directory used by Visual Studio when running your application.</span></span> <span data-ttu-id="fcff4-154">이 한 눈에 띄는 부작용은 SQLite 파일은 이제 프로젝트 디렉터리 및 출력 디렉터리가 아니라 수 있었던 같은 경우</span><span class="sxs-lookup"><span data-stu-id="fcff4-154">One observable side effect of this is that SQLite filenames are now relative to the project directory and not the output directory like they used to be.</span></span>

### <a name="ef-core-20-requires-a-20-database-provider"></a><span data-ttu-id="fcff4-155">EF Core 2.0 2.0 데이터베이스 공급자가 필요</span><span class="sxs-lookup"><span data-stu-id="fcff4-155">EF Core 2.0 requires a 2.0 database provider</span></span>

<span data-ttu-id="fcff4-156">EF Core 2.0에 대 한 작동 하는 많은 단순화 형식 및 향상 된 방식으로 데이터베이스 공급자에서 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-156">For EF Core 2.0 we have made many simplifications and improvements in the way database providers work.</span></span> <span data-ttu-id="fcff4-157">즉, EF Core 2.0을 사용 하 여 1.0.x 및 1.1.x 공급자 작동 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-157">This means that 1.0.x and 1.1.x providers will not work with EF Core 2.0.</span></span>

<span data-ttu-id="fcff4-158">SQL Server 및 SQLite 공급자는 EF 팀에서 제공 되 고 2.0 버전을 사용할 수 있습니다 2.0의 일환으로 릴리스 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-158">The SQL Server and SQLite providers are shipped by the EF team and 2.0 versions will be available as part of the 2.0 release.</span></span> <span data-ttu-id="fcff4-159">에 대 한 오픈 소스 타사 공급자 [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact)를 [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), 및 [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) 2.0에 대 한 업데이트 하는 중입니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-159">The open-source third party providers for [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), and [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) are being updated for 2.0.</span></span> <span data-ttu-id="fcff4-160">다른 모든 공급자에 대 한 공급자 작성자에 게 문의 하십시오.</span><span class="sxs-lookup"><span data-stu-id="fcff4-160">For all other providers, please contact the provider writer.</span></span>

### <a name="logging-and-diagnostics-events-have-changed"></a><span data-ttu-id="fcff4-161">로깅 및 진단 이벤트 변경</span><span class="sxs-lookup"><span data-stu-id="fcff4-161">Logging and Diagnostics events have changed</span></span>

<span data-ttu-id="fcff4-162">참고: 이러한 변경 내용은 대부분의 응용 프로그램 코드가 영향을 주지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-162">Note: these changes should not impact most application code.</span></span>

<span data-ttu-id="fcff4-163">전송 된 메시지에 대 한 이벤트 Id는 [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) 2.0에서 변경 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-163">The event IDs for messages sent to an [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) have changed in 2.0.</span></span> <span data-ttu-id="fcff4-164">이제 이벤트 ID가 EF Core 코드 전체에서 고유합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-164">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="fcff4-165">또한 이 메시지가 MVC 등에서 사용되는 구조화된 로깅의 표준 패턴을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-165">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="fcff4-166">로거 범주도 변경되었습니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-166">Logger categories have also changed.</span></span> <span data-ttu-id="fcff4-167">이제 [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs)를 통해 액세스되는 잘 알려진 범주 집합입니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-167">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="fcff4-168">[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) 이벤트가 해당 이벤트 ID 이름과 이제 사용할 `ILogger` 메시지입니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-168">[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) events now use the same event ID names as the corresponding `ILogger` messages.</span></span> <span data-ttu-id="fcff4-169">이벤트 페이로드는에서 파생 된 모든 명목 형식을 [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs)합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-169">The event payloads are all nominal types derived from [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).</span></span>

<span data-ttu-id="fcff4-170">이벤트 Id, 페이로드 형식 및 범주에 설명 되어는 [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) 하며 [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) 클래스.</span><span class="sxs-lookup"><span data-stu-id="fcff4-170">Event IDs, payload types, and categories are documented in the [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) and the [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) classes.</span></span>

<span data-ttu-id="fcff4-171">Id가 새 Microsoft.EntityFrameworkCore.Diagnostics 네임 스페이스에도 Microsoft.EntityFrameworkCore.Infraestructure에서 이동 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-171">IDs have also moved from Microsoft.EntityFrameworkCore.Infraestructure to the new Microsoft.EntityFrameworkCore.Diagnostics namespace.</span></span>

### <a name="ef-core-relational-metadata-api-changes"></a><span data-ttu-id="fcff4-172">EF Core 관계형 메타 데이터 API 변경 내용</span><span class="sxs-lookup"><span data-stu-id="fcff4-172">EF Core relational metadata API changes</span></span>

<span data-ttu-id="fcff4-173">이제 EF Core 2.0은 사용하는 각각의 다른 공급자마다 다른 [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs)을 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-173">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="fcff4-174">일반적으로 응용 프로그램에 투명합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-174">This is usually transparent to the application.</span></span> <span data-ttu-id="fcff4-175">이렇게 하면 하위 수준 메타데이터 API가 간소화되어 _공통 관계형 메타데이터 개념_에 대한 모든 액세스가 항상 `.SqlServer`, `.Sqlite` 등이 아닌 `.Relational` 호출을 통해 이루어집니다. 예를 들어 1.1.x 다음과 같은 코드:</span><span class="sxs-lookup"><span data-stu-id="fcff4-175">This has facilitated a simplification of lower-level metadata APIs such that any access to _common relational metadata concepts_ is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc. For example, 1.1.x code like this:</span></span>

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

<span data-ttu-id="fcff4-176">다음과 같은 이제 작성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-176">Should now be written like this:</span></span>

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

<span data-ttu-id="fcff4-177">와 같은 메서드를 사용 하는 대신 `ForSqlServerToTable`, 확장 메서드는 사용 중인 현재 공급자를 기반으로 조건부 코드를 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-177">Instead of using methods like `ForSqlServerToTable`, extension methods are now available to write conditional code based on the current provider in use.</span></span> <span data-ttu-id="fcff4-178">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="fcff4-178">For example:</span></span>

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

<span data-ttu-id="fcff4-179">이 변경은 Api/메타 데이터에 대해 정의 된에 적용 됩니다 _모든_ 관계형 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-179">Note that this change only applies to APIs/metadata that is defined for _all_ relational providers.</span></span> <span data-ttu-id="fcff4-180">API 및 메타 데이터를 동일 하 게 유지 때만 단일 공급자에만 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-180">The API and metadata remains the same when it is specific to only a single provider.</span></span> <span data-ttu-id="fcff4-181">예를 들어 클러스터형된 인덱스는 SQL Sever 관련이 있으므로 `ForSqlServerIsClustered` 및 `.SqlServer().IsClustered()` 계속 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-181">For example, clustered indexes are specific to SQL Sever, so `ForSqlServerIsClustered` and  `.SqlServer().IsClustered()` must still be used.</span></span>

### <a name="dont-take-control-of-the-ef-service-provider"></a><span data-ttu-id="fcff4-182">EF 서비스 공급자의 컨트롤을 사용 하지</span><span class="sxs-lookup"><span data-stu-id="fcff4-182">Don’t take control of the EF service provider</span></span>

<span data-ttu-id="fcff4-183">EF Core를 사용 하 여 내부 `IServiceProvider` (종속성 주입 컨테이너)의 내부 구현에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-183">EF Core uses an internal `IServiceProvider` (a dependency injection container) for its internal implementation.</span></span> <span data-ttu-id="fcff4-184">응용 프로그램에 EF Core를 제외 하 고 특별 한 경우에이 공급자를 만들고 허용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-184">Applications should allow EF Core to create and manage this provider except in special cases.</span></span> <span data-ttu-id="fcff4-185">에 대 한 모든 호출을 제거 방법을 고려해 야 `UseInternalServiceProvider`합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-185">Strongly consider removing any calls to `UseInternalServiceProvider`.</span></span> <span data-ttu-id="fcff4-186">응용 프로그램에 호출 해야 하는 경우 `UseInternalServiceProvider`, 다음 보세요 [문제를 신고](https://github.com/aspnet/EntityFramework/Issues) 시나리오를 처리 하는 다른 방법을 조사할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-186">If an application does need to call `UseInternalServiceProvider`, then please consider [filing an issue](https://github.com/aspnet/EntityFramework/Issues) so we can investigate other ways to handle your scenario.</span></span>

<span data-ttu-id="fcff4-187">호출 `AddEntityFramework`, `AddEntityFrameworkSqlServer`, 하지 않는 한 응용 프로그램 코드에서 등 필요 하지 않은 `UseInternalServiceProvider` 라고도 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-187">Calling `AddEntityFramework`, `AddEntityFrameworkSqlServer`, etc. is not required by application code unless `UseInternalServiceProvider` is also called.</span></span> <span data-ttu-id="fcff4-188">기존 호출을 제거 `AddEntityFramework` 나 `AddEntityFrameworkSqlServer`등 `AddDbContext` 이전과 동일한 방식으로 사용 계속 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-188">Remove any existing calls to `AddEntityFramework` or `AddEntityFrameworkSqlServer`, etc. `AddDbContext` should still be used in the same way as before.</span></span>

### <a name="in-memory-databases-must-be-named"></a><span data-ttu-id="fcff4-189">메모리 내 데이터베이스의 이름은</span><span class="sxs-lookup"><span data-stu-id="fcff4-189">In-memory databases must be named</span></span>

<span data-ttu-id="fcff4-190">전역 명명 되지 않은 메모리 내 데이터베이스 제거 되 고 대신 모든 메모리 내 데이터베이스를 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-190">The global unnamed in-memory database has been removed and instead all in-memory databases must be named.</span></span> <span data-ttu-id="fcff4-191">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="fcff4-191">For example:</span></span>

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

<span data-ttu-id="fcff4-192">이 만듭니다/에서는 "MyDatabase" 라는 이름의 데이터베이스를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-192">This creates/uses a database with the name “MyDatabase”.</span></span> <span data-ttu-id="fcff4-193">경우 `UseInMemoryDatabase` 동일한 메모리 내 데이터베이스 사용 됩니다, 상황에 맞는 여러 인스턴스에서 공유할 수 있도록 동일한 이름의 다시 호출 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-193">If `UseInMemoryDatabase` is called again with the same name, then the same in-memory database will be used, allowing it to be shared by multiple context instances.</span></span>

### <a name="read-only-api-changes"></a><span data-ttu-id="fcff4-194">읽기 전용 API 변경 내용</span><span class="sxs-lookup"><span data-stu-id="fcff4-194">Read-only API changes</span></span>

<span data-ttu-id="fcff4-195">`IsReadOnlyBeforeSave`를 `IsReadOnlyAfterSave`, 및 `IsStoreGeneratedAlways` 사용 되지 않습니다 및로 대체 되었습니다 [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) 하 고 [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55)합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-195">`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`, and `IsStoreGeneratedAlways` have been obsoleted and replaced with [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) and [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55).</span></span> <span data-ttu-id="fcff4-196">이러한 동작은 모든 속성 (뿐만 아니라 저장소 생성 속성)에 적용 하 고 데이터베이스 행을 삽입 하는 경우 속성의 값을 사용할지 어떻게 결정 (`BeforeSaveBehavior`) 또는 때 행을 데이터베이스 기존 업데이트 (`AfterSaveBehavior`).</span><span class="sxs-lookup"><span data-stu-id="fcff4-196">These behaviors apply to any property (not only store-generated properties) and determine how the value of the property should be used when inserting into a database row (`BeforeSaveBehavior`) or when updating an existing database row (`AfterSaveBehavior`).</span></span>

<span data-ttu-id="fcff4-197">속성으로 표시 [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (예: 계산 된 열)는 기본적으로 모든 값을 무시 현재 속성을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-197">Properties marked as [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (for example, for computed columns) will by default ignore any value currently set on the property.</span></span> <span data-ttu-id="fcff4-198">즉, 저장소 생성 값은 항상 값 설정 또는 추적 된 엔터티의 수정 된 여부에 관계 없이 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-198">This means that a store-generated value will always be obtained regardless of whether any value has been set or modified on the tracked entity.</span></span> <span data-ttu-id="fcff4-199">다른 설정 하 여 변경할 수 있습니다 `Before\AfterSaveBehavior`합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-199">This can be changed by setting a different `Before\AfterSaveBehavior`.</span></span>

### <a name="new-clientsetnull-delete-behavior"></a><span data-ttu-id="fcff4-200">새 ClientSetNull 삭제 동작</span><span class="sxs-lookup"><span data-stu-id="fcff4-200">New ClientSetNull delete behavior</span></span>

<span data-ttu-id="fcff4-201">이전 릴리스에서 [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) 있었습니다 엔터티에 대 한 동작을 컨텍스트에 의해 추적 이상의 닫힌 일치 `SetNull` 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-201">In previous releases, [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) had a behavior for entities tracked by the context that more closed matched `SetNull` semantics.</span></span> <span data-ttu-id="fcff4-202">EF Core 2.0에서 새 `ClientSetNull` 선택적 관계에 대 한 기본 동작을 도입 했습니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-202">In EF Core 2.0, a new `ClientSetNull` behavior has been introduced as the default for optional relationships.</span></span> <span data-ttu-id="fcff4-203">이 동작은 `SetNull` 추적 된 엔터티에 대 한 의미 체계 및 `Restrict` EF Core를 사용 하 여 만든 데이터베이스에 대 한 동작입니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-203">This behavior has `SetNull` semantics for tracked entities and `Restrict` behavior for databases created using EF Core.</span></span> <span data-ttu-id="fcff4-204">경험에 따르면 이들은 추적 된 엔터티 및 데이터베이스에 대 한 예상/유용한 동작입니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-204">In our experience, these are the most expected/useful behaviors for tracked entities and the database.</span></span> <span data-ttu-id="fcff4-205">`DeleteBehavior.Restrict` 이제 선택적 관계에 대 한 설정 된 경우 추적 된 엔터티에 대 한 인식 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-205">`DeleteBehavior.Restrict` is now honored for tracked entities when set for optional relationships.</span></span>

### <a name="provider-design-time-packages-removed"></a><span data-ttu-id="fcff4-206">공급자 설계 시간 패키지 제거</span><span class="sxs-lookup"><span data-stu-id="fcff4-206">Provider design-time packages removed</span></span>

<span data-ttu-id="fcff4-207">`Microsoft.EntityFrameworkCore.Relational.Design` 패키지 제거 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-207">The `Microsoft.EntityFrameworkCore.Relational.Design` package has been removed.</span></span> <span data-ttu-id="fcff4-208">내용에 통합 되었습니다 `Microsoft.EntityFrameworkCore.Relational` 고 `Microsoft.EntityFrameworkCore.Design`입니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-208">It's contents were consolidated into `Microsoft.EntityFrameworkCore.Relational` and `Microsoft.EntityFrameworkCore.Design`.</span></span>

<span data-ttu-id="fcff4-209">이 공급자 설계 시간 패키지를 전파합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-209">This propagates into the provider design-time packages.</span></span> <span data-ttu-id="fcff4-210">해당 패키지 (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`등) 제거 된 내용도 주요 공급자 패키지를 통합 하 고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-210">Those packages (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`, etc.) were removed and their contents consolidated into the main provider packages.</span></span>

<span data-ttu-id="fcff4-211">사용할 수 있도록 `Scaffold-DbContext` 또는 `dotnet ef dbcontext scaffold` EF Core 2.0에서 하기만 하면 단일 공급자 패키지를 참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcff4-211">To enable `Scaffold-DbContext` or `dotnet ef dbcontext scaffold` in EF Core 2.0, you only need to reference the single provider package:</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
