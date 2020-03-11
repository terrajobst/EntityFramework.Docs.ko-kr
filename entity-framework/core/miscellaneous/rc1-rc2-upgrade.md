---
title: EF Core 1.0 RC1에서 RC2로 업그레이드-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 887b7cd539b9c0f5a680398f5039757420228710
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414112"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="21556-102">EF Core 1.0 RC1에서 1.0 RC2로 업그레이드</span><span class="sxs-lookup"><span data-stu-id="21556-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="21556-103">이 문서에서는 RC1 패키지를 사용 하 여 빌드된 응용 프로그램을 RC2로 이동 하기 위한 지침을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="21556-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="21556-104">패키지 이름 및 버전</span><span class="sxs-lookup"><span data-stu-id="21556-104">Package Names and Versions</span></span>

<span data-ttu-id="21556-105">RC1과 RC2 사이에서 "Entity Framework 7"을 "Entity Framework Core"로 변경 했습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="21556-106">[Scott Hanselman이 게시물](https://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx)을 변경 하는 이유에 대 한 자세한 내용을 읽어볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-106">You can read more about the reasons for the change in [this post by Scott Hanselman](https://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="21556-107">이러한 변경으로 인해 패키지 이름이 `EntityFramework.*`에서 `Microsoft.EntityFrameworkCore.*`로 변경 되 고 버전은 `7.0.0-rc1-final`에서 `1.0.0-rc2-final` (또는 도구 `1.0.0-preview1-final`)로 변경 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="21556-108">**RC1 패키지를 완전히 제거한 다음 RC2 패키지를 설치 해야 합니다.**</span><span class="sxs-lookup"><span data-stu-id="21556-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="21556-109">다음은 몇 가지 일반적인 패키지에 대 한 매핑입니다.</span><span class="sxs-lookup"><span data-stu-id="21556-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="21556-110">RC1 패키지</span><span class="sxs-lookup"><span data-stu-id="21556-110">RC1 Package</span></span>                                               | <span data-ttu-id="21556-111">RC2 동급</span><span class="sxs-lookup"><span data-stu-id="21556-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="21556-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="21556-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="21556-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="21556-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="21556-114">EntityFramework.SQLite                    7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="21556-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="21556-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="21556-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="21556-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="21556-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="21556-117">NpgSql <to be advised></span><span class="sxs-lookup"><span data-stu-id="21556-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="21556-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="21556-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="21556-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="21556-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="21556-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="21556-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="21556-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="21556-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="21556-122">EntityFramework.InMemory                  7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="21556-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="21556-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="21556-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="21556-124">EntityFramework.IBMDataServer             7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="21556-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="21556-125">RC2에서 아직 사용할 수 없음</span><span class="sxs-lookup"><span data-stu-id="21556-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="21556-126">EntityFramework.Commands                  7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="21556-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="21556-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="21556-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="21556-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="21556-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="21556-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="21556-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="21556-130">네임스페이스</span><span class="sxs-lookup"><span data-stu-id="21556-130">Namespaces</span></span>

<span data-ttu-id="21556-131">패키지 이름과 함께 네임 스페이스는 `Microsoft.Data.Entity.*`에서 `Microsoft.EntityFrameworkCore.*`로 변경 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="21556-132">`using Microsoft.EntityFrameworkCore`와 `using Microsoft.Data.Entity`의 찾기/바꾸기를 사용 하 여이 변경 내용을 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="21556-133">테이블 명명 규칙 변경</span><span class="sxs-lookup"><span data-stu-id="21556-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="21556-134">RC2에서 수행 하는 중요 한 기능 변경은 클래스 이름이 아니라 지정 된 엔터티의 `DbSet<TEntity>` 속성 이름을 매핑되는 테이블 이름으로 사용 하는 것 이었습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="21556-135">[관련 알림 문제](https://github.com/aspnet/Announcements/issues/167)에서이 변경 내용에 대해 자세히 알아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="21556-136">기존 RC1 응용 프로그램의 경우 RC1 명명 전략을 유지 하기 위해 `OnModelCreating` 메서드의 시작 부분에 다음 코드를 추가 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="21556-137">새 명명 전략을 채택 하려는 경우 나머지 업그레이드 단계를 완료 한 다음 코드를 제거 하 고 마이그레이션을 만들어 테이블 이름을 바꾸는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="21556-138">Services.adddbcontext/Startup.cs Changes (ASP.NET Core 프로젝트에만 해당)</span><span class="sxs-lookup"><span data-stu-id="21556-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="21556-139">R c 1에서는 응용 프로그램 서비스 공급자 (`Startup.ConfigureServices(...)`)에 Entity Framework 서비스를 추가 해야 했습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="21556-140">RC2에서 `AddEntityFramework()`, `AddSqlServer()`등에 대 한 호출을 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="21556-141">또한 컨텍스트 옵션을 사용 하 여 기본 생성자에 전달 하는 생성자를 파생 컨텍스트에 추가 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="21556-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="21556-142">이는 내부적으로 snuck 하는 몇 가지 방법 중 일부를 제거 했기 때문에 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="21556-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="21556-143">IServiceProvider 전달</span><span class="sxs-lookup"><span data-stu-id="21556-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="21556-144">컨텍스트에 `IServiceProvider`를 전달 하는 RC1 코드가 있으면이는 별도의 생성자 매개 변수가 아니라 `DbContextOptions`으로 이동 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="21556-145">`DbContextOptionsBuilder.UseInternalServiceProvider(...)`를 사용 하 여 서비스 공급자를 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="21556-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="21556-146">테스트</span><span class="sxs-lookup"><span data-stu-id="21556-146">Testing</span></span>

<span data-ttu-id="21556-147">이 작업을 수행 하는 가장 일반적인 시나리오는 테스트할 때 InMemory 데이터베이스의 범위를 제어 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="21556-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="21556-148">RC2를 사용 하 여이 작업을 수행 하는 예제는 업데이트 된 [테스트](testing/index.md) 문서를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="21556-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="21556-149">응용 프로그램 서비스 공급자에서 내부 서비스 확인 (ASP.NET Core 프로젝트에만 해당)</span><span class="sxs-lookup"><span data-stu-id="21556-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="21556-150">ASP.NET Core 응용 프로그램이 있고 EF에서 응용 프로그램 서비스 공급자의 내부 서비스를 확인 하려는 경우이를 구성할 수 있는 `AddDbContext` 오버 로드가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider));
```

> [!WARNING]  
> <span data-ttu-id="21556-151">내부 EF 서비스를 응용 프로그램 서비스 공급자에 결합 해야 하는 이유가 없다면 EF가 내부적으로 자체 서비스를 관리 하도록 허용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="21556-152">이 작업을 수행 하는 가장 중요 한 이유는 응용 프로그램 서비스 공급자를 사용 하 여 EF에서 내부적으로 사용 하는 서비스를 대체 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="21556-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="21556-153">DNX Commands = > .NET CLI (ASP.NET Core 프로젝트에만 해당)</span><span class="sxs-lookup"><span data-stu-id="21556-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="21556-154">ASP.NET 5 프로젝트에 대 한 `dnx ef` 명령을 이전에 사용한 경우에는 이제 `dotnet ef` 명령으로 이동 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="21556-155">동일한 명령 구문이 여전히 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="21556-155">The same command syntax still applies.</span></span> <span data-ttu-id="21556-156">구문 정보를 `dotnet ef --help` 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="21556-157">DNX가 .NET CLI로 대체 되기 때문에 RC2에서 명령이 등록 되는 방법이 변경 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="21556-158">이제 명령이 `project.json`의 `tools` 섹션에 등록 됩니다.</span><span class="sxs-lookup"><span data-stu-id="21556-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> <span data-ttu-id="21556-159">Visual Studio를 사용 하는 경우 이제 패키지 관리자 콘솔을 사용 하 여 ASP.NET Core 프로젝트에 대 한 EF 명령을 실행할 수 있습니다 (RC1에서는 지원 되지 않음).</span><span class="sxs-lookup"><span data-stu-id="21556-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="21556-160">이 작업을 수행 하려면 `project.json`의 `tools` 섹션에서 명령을 등록 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="21556-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="21556-161">패키지 관리자 명령에 PowerShell 5 필요</span><span class="sxs-lookup"><span data-stu-id="21556-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="21556-162">Visual Studio의 패키지 관리자 콘솔에서 Entity Framework 명령을 사용 하는 경우 PowerShell 5가 설치 되어 있는지 확인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="21556-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="21556-163">다음 릴리스에서 제거 될 임시 요구 사항입니다 (자세한 내용은 [문제 #5327](https://github.com/aspnet/EntityFramework/issues/5327) 참조).</span><span class="sxs-lookup"><span data-stu-id="21556-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="21556-164">Project. json에서 "imports" 사용</span><span class="sxs-lookup"><span data-stu-id="21556-164">Using "imports" in project.json</span></span>

<span data-ttu-id="21556-165">EF Core의 일부 종속성은 아직 .NET Standard를 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="21556-166">.NET Standard 및 .NET Core 프로젝트의 EF Core에는 임시 해결 방법으로 project. json에 "가져오기"를 추가 해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="21556-167">EF를 추가 하는 경우 NuGet 복원은 다음 오류 메시지를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="21556-167">When adding EF, NuGet restore will display this error message:</span></span>

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

<span data-ttu-id="21556-168">해결 방법은 이식 가능한 프로필 "net451 + win8"를 수동으로 가져오는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="21556-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="21556-169">이렇게 하면 NuGet이이 이진 파일과 일치 하는 이진 파일을 .NET Standard와 호환 되는 프레임 워크로 처리 합니다. 단,이는 그렇지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="21556-170">"Net451 + win8"는 .NET Standard와 100% 호환 되지 않지만 PCL에서 .NET Standard로 전환 하는 경우에만 호환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="21556-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="21556-171">EF의 종속성이 궁극적으로 .NET Standard로 업그레이드 되 면 가져오기를 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="21556-172">배열 구문에서 "imports"에 여러 프레임 워크를 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="21556-173">다른 가져오기는 프로젝트에 라이브러리를 추가 하는 경우에 필요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21556-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="21556-174">[문제 #5176](https://github.com/aspnet/EntityFramework/issues/5176)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="21556-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
