---
title: "EF 1.0 Core r c 1에서 RC2-EF 코어로 업그레이드"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
ms.technology: entity-framework-core
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: ae5077c30642e3f40f51adee429821978f194460
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="d9f46-102">EF 1.0 Core r c 1에서 1.0 r c 2로 업그레이드</span><span class="sxs-lookup"><span data-stu-id="d9f46-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="d9f46-103">이 문서에서는 r c 2로 RC1 패키지를 사용 하 여 빌드한 응용 프로그램을 이동 하기 위한 지침을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="d9f46-104">패키지 이름 및 버전</span><span class="sxs-lookup"><span data-stu-id="d9f46-104">Package Names and Versions</span></span>

<span data-ttu-id="d9f46-105">RC1와 RC2 사이 "Entity Framework Core"를 "Entity Framework 7"에서 변경 했습니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="d9f46-106">자세한 내용은의 변경에 대 한 용도 대 한 [Scott hanselman이 게시물](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx)합니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="d9f46-107">이러한 변경 때문에 패키지 이름에서 변경 `EntityFramework.*` 를 `Microsoft.EntityFrameworkCore.*` 및 우리의 버전 `7.0.0-rc1-final` 를 `1.0.0-rc2-final` (또는 `1.0.0-preview1-final` 도구에 대 한).</span><span class="sxs-lookup"><span data-stu-id="d9f46-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="d9f46-108">**완전히 RC1 패키지를 제거 하 고 다음 RC2 설치 해야 합니다는 스토리 합니다.**</span><span class="sxs-lookup"><span data-stu-id="d9f46-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="d9f46-109">다음은 몇 가지 일반적인 패키지에 대 한 매핑을입니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="d9f46-110">RC1 패키지</span><span class="sxs-lookup"><span data-stu-id="d9f46-110">RC1 Package</span></span>                                               | <span data-ttu-id="d9f46-111">해당 하는 RC2</span><span class="sxs-lookup"><span data-stu-id="d9f46-111">RC2 Equivalent</span></span>                                                       |
| --------------------------------------------------------- | -------------------------------------------------------------------- |
| <span data-ttu-id="d9f46-112">EntityFramework.MicrosoftSqlServer 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="d9f46-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="d9f46-113">Microsoft.EntityFrameworkCore.SqlServer 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="d9f46-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="d9f46-114">EntityFramework.SQLite 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="d9f46-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="d9f46-115">Microsoft.EntityFrameworkCore.Sqlite 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="d9f46-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="d9f46-116">EntityFramework7.Npgsql 3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="d9f46-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="d9f46-117">NpgSql.EntityFrameworkCore.Postgres<to be advised></span><span class="sxs-lookup"><span data-stu-id="d9f46-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="d9f46-118">EntityFramework.SqlServerCompact35 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="d9f46-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="d9f46-119">EntityFrameworkCore.SqlServerCompact35 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="d9f46-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="d9f46-120">EntityFramework.SqlServerCompact40 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="d9f46-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="d9f46-121">EntityFrameworkCore.SqlServerCompact40 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="d9f46-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="d9f46-122">EntityFramework.InMemory 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="d9f46-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="d9f46-123">Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="d9f46-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="d9f46-124">EntityFramework.IBMDataServer 7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="d9f46-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="d9f46-125">R c 2 용 아직 사용할 수 없음</span><span class="sxs-lookup"><span data-stu-id="d9f46-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="d9f46-126">EntityFramework.Commands 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="d9f46-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="d9f46-127">Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="d9f46-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="d9f46-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="d9f46-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="d9f46-129">Microsoft.EntityFrameworkCore.SqlServer.Design 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="d9f46-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="d9f46-130">네임스페이스</span><span class="sxs-lookup"><span data-stu-id="d9f46-130">Namespaces</span></span>

<span data-ttu-id="d9f46-131">패키지 이름, 함께 네임 스페이스에서 변경 `Microsoft.Data.Entity.*` 를 `Microsoft.EntityFrameworkCore.*`합니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="d9f46-132">이러한 변경의 찾기/바꾸기도 처리할 수 있습니다 `using Microsoft.Data.Entity` 와 `using Microsoft.EntityFrameworkCore`합니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="d9f46-133">테이블 명명 규칙 변경</span><span class="sxs-lookup"><span data-stu-id="d9f46-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="d9f46-134">R c 2의 했습니다는 중요 한 기능 변경의 이름을 사용 하는 것 이었습니다는 `DbSet<TEntity>` 테이블 이름으로 특정된 엔터티에 대해 속성 매핑될 클래스 이름만 대신 합니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="d9f46-135">자세한 내용은 이러한 변경에 대 한 [관련된 알림 문제](https://github.com/aspnet/Announcements/issues/167)합니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="d9f46-136">기존 RC1 응용 프로그램에 대 한 것이 좋습니다의 시작 부분에 다음 코드를 추가 하면 `OnModelCreating` RC1 명명 전략을 유지 하는 메서드:</span><span class="sxs-lookup"><span data-stu-id="d9f46-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="d9f46-137">새 명명 전략을 채택 하려는 경우 것이 좋습니다 성공적으로 업그레이드 단계와 다음 코드를 제거 및 연산자의 테이블을 적용 하려면 마이그레이션 만드는 나머지 완료의 이름을 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="d9f46-138">AddDbContext / Startup.cs 변경 (ASP.NET Core 프로젝트에만 해당)</span><span class="sxs-lookup"><span data-stu-id="d9f46-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="d9f46-139">Entity Framework 서비스는 응용 프로그램 서비스 공급자를 추가 해야 했습니다 r c 1에서는 `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="d9f46-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="d9f46-140">R c 2에서에 대 한 호출을 제거할 수 있습니다 `AddEntityFramework()`, `AddSqlServer()`등.:</span><span class="sxs-lookup"><span data-stu-id="d9f46-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="d9f46-141">상황에 맞는 옵션을 사용 하 여 기본 생성자에 전달 하 여 파생 컨텍스트에 생성자를 추가 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="d9f46-142">이 백그라운드에서 예에서 약간 무시 무시 마법 중 일부를 제거 하기 때문에 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="d9f46-143">IServiceProvider에 전달합니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="d9f46-144">RC1 전달 하는 코드를 있는 경우는 `IServiceProvider` 컨텍스트에이 이제로 이동 되었습니다 `DbContextOptions`, 아니라 별도 생성자 매개 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="d9f46-145">사용 하 여 `DbContextOptionsBuilder.UseInternalServiceProvider(...)` 서비스 공급자를 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="d9f46-146">테스트</span><span class="sxs-lookup"><span data-stu-id="d9f46-146">Testing</span></span>

<span data-ttu-id="d9f46-147">이 작업을 위한 가장 일반적인 시나리오를 테스트할 때 InMemory 데이터베이스의 범위를 제어 하는 것 이었습니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="d9f46-148">업데이트 된 참조 [테스트](testing/index.md) RC2와이 작업에 대 한 예제는 문서입니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="d9f46-149">응용 프로그램 서비스 공급자 (ASP.NET Core 프로젝트에만 해당)에서 내부 서비스 확인</span><span class="sxs-lookup"><span data-stu-id="d9f46-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="d9f46-150">오버 로드 없는 ASP.NET Core 응용 프로그램이 있는 경우 응용 프로그램 서비스 공급자에서 내부 서비스를 확인 하는 EF `AddDbContext` 되이 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> <span data-ttu-id="d9f46-151">응용 프로그램 서비스 공급자에 내부 EF 서비스를 결합 하는 이유가 없다면 내부적으로 자체 서비스를 관리 하는 EF를 허용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="d9f46-152">이 작업을 수행 하려면 하는 주요 이유는 EF 내부적으로 사용 하는 서비스의 이름을 바꾸려면 응용 프로그램 서비스 공급자를 사용 하려면</span><span class="sxs-lookup"><span data-stu-id="d9f46-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="d9f46-153">DNX 명령을 = >.NET CLI (ASP.NET Core 프로젝트에만 해당)</span><span class="sxs-lookup"><span data-stu-id="d9f46-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="d9f46-154">이전에 사용한는 `dnx ef` ASP.NET 5 프로젝트에 대 한 명령,이 이제로 이동 `dotnet ef` 명령입니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="d9f46-155">동일한 명령 구문을 여전히 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-155">The same command syntax still applies.</span></span> <span data-ttu-id="d9f46-156">사용할 수 있습니다 `dotnet ef --help` 구문 정보에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="d9f46-157">명령을 등록 하는 방법은.NET CLI 대체 되 고 DNX 인해 r c 2에서 변경 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="d9f46-158">명령에서 등록 한 `tools` 섹션 `project.json`:</span><span class="sxs-lookup"><span data-stu-id="d9f46-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

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
> <span data-ttu-id="d9f46-159">Visual Studio를 사용 하는 경우 (이 지원 되지 않습니다 RC1) ASP.NET Core 프로젝트에 대 한 EF 명령을 실행 하도록 이제 패키지 관리자 콘솔을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="d9f46-160">명령에 등록 해야 하는 `tools` 섹션 `project.json` 이렇게 하려면.</span><span class="sxs-lookup"><span data-stu-id="d9f46-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="d9f46-161">패키지 관리자 명령은 필요 PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="d9f46-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="d9f46-162">Entity Framework 명령을 사용 하 여 Visual Studio에서 패키지 관리자 콘솔에서 PowerShell 5 설치 되어 있는지 확인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="d9f46-163">이 다음 릴리스에서 제거 될 임시 요구 사항 (참조 [#5327 발급](https://github.com/aspnet/EntityFramework/issues/5327) 자세한 세부 정보에 대 한).</span><span class="sxs-lookup"><span data-stu-id="d9f46-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="d9f46-164">Project.json에서 "가져오기"를 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="d9f46-164">Using "imports" in project.json</span></span>

<span data-ttu-id="d9f46-165">EF 핵심 종속성 중 일부를 지원 하지 않습니다.NET 표준 아직.</span><span class="sxs-lookup"><span data-stu-id="d9f46-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="d9f46-166">표준.NET 및.NET Core 프로젝트의 EF 코어 "로 가져옵니다. 이러한" 임시 방편으로 project.json 추가 필요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="d9f46-167">EF를 추가할 때 NuGet 복원이 오류 메시지가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-167">When adding EF, NuGet restore will display this error message:</span></span>

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

<span data-ttu-id="d9f46-168">이를 해결 하려면 수동으로 "portable net451 + win8" 이식 가능한 프로 파일을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="d9f46-169">이 강제로이 일치 하는이 이진 파일을 처리 하는 NuGet 없는 경우에.NET 표준와 호환 프레임 워크를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="d9f46-170">"Portable net451 + win8" 하지는 않지만 표준.NET 호환 100% 표준.NET에 대 한 PCL 전환에 대해 충분히 호환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="d9f46-171">Imports EF의 종속성 결국.NET 표준으로 업그레이드 하는 경우 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="d9f46-172">배열 구문에서 다양 한 프레임 워크 "가져오기"를 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="d9f46-173">다른 가져오기 프로젝트에 추가 라이브러리를 추가 하는 경우에 필요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="d9f46-174">참조 [#5176 발급](https://github.com/aspnet/EntityFramework/issues/5176)합니다.</span><span class="sxs-lookup"><span data-stu-id="d9f46-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
