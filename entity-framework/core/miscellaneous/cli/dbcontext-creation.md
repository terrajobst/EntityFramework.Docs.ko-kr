---
title: 디자인 타임 DbContext 만들기-EF 코어
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 8b38d300d31038bdf5cd877aa3c42b7f5f97eabc
ms.sourcegitcommit: 7113e8675f26cbb546200824512078bf360225df
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2018
ms.locfileid: "30202486"
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="5f4cf-102">디자인 타임 DbContext 만들기</span><span class="sxs-lookup"><span data-stu-id="5f4cf-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="5f4cf-103">EF 핵심 도구 명령 중 일부 (예를 들어는 [마이그레이션] [ 1] 명령) 파생 필요 `DbContext` 인스턴스를 응용 프로그램에 대 한 세부 정보를 수집 하려면 쿼리 디자인 타임에 만듭니다 엔터티 형식 및 데이터베이스 스키마에 매핑되는 방식입니다.</span><span class="sxs-lookup"><span data-stu-id="5f4cf-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="5f4cf-104">대부분의 경우 것은 바람직하지 하는 `DbContext` 함으로써 만든 어떻게 하는 것을 유사한 방식으로 구성 [런타임 시 구성][2]합니다.</span><span class="sxs-lookup"><span data-stu-id="5f4cf-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="5f4cf-105">여러 가지 방법으로 만들려고 시도 하는 도구는 `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="5f4cf-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="5f4cf-106">응용 프로그램 서비스</span><span class="sxs-lookup"><span data-stu-id="5f4cf-106">From application services</span></span>
-------------------------
<span data-ttu-id="5f4cf-107">시작 프로젝트는 ASP.NET Core 응용 프로그램, 도구는 응용 프로그램의 서비스 공급자에서 DbContext 개체를 가져올 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="5f4cf-107">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="5f4cf-108">이 도구를 호출 하 여 서비스 공급자를 먼저 시도 `Program.BuildWebHost()` 에 액세스 하 고는 `IWebHost.Services` 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="5f4cf-108">The tool first try to obtain the service provider by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span>

> [!NOTE]
> <span data-ttu-id="5f4cf-109">새 ASP.NET 코어 2.0 응용 프로그램을 만들 때 기본적으로이 후크 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5f4cf-109">When you create a new ASP.NET Core 2.0 application, this hook is included by default.</span></span> <span data-ttu-id="5f4cf-110">EF Core 및 ASP.NET 코어의 이전 버전에서는 도구를 호출 하려고 `Startup.ConfigureServices` 더 이상 응용 프로그램의 서비스 공급자 하지만이 패턴을 얻기 위해 제대로 작동 ASP.NET 코어 2.0 응용 프로그램에서 직접 합니다.</span><span class="sxs-lookup"><span data-stu-id="5f4cf-110">In previous versions of EF Core and ASP.NET Core, the tools try to invoke `Startup.ConfigureServices` directly in order to obtain the application's service provider, but this pattern no longer works correctly in ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="5f4cf-111">ASP.NET Core 1.x 응용 프로그램 2.0 업그레이드 하는 경우 다음을 할 수 있습니다 [수정 프로그램 `Program` 새 패턴을 따르도록 클래스][3]합니다.</span><span class="sxs-lookup"><span data-stu-id="5f4cf-111">If you are upgrading an ASP.NET Core 1.x application to 2.0, you can [modify your `Program` class to follow the new pattern][3].</span></span>

<span data-ttu-id="5f4cf-112">`DbContext` 자체 및 모든 종속성의 생성자에 서비스 응용 프로그램의 서비스 공급자에 등록 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5f4cf-112">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="5f4cf-113">쉽게 액세스가 가능 하도록 하 여 [에 생성자가는 `DbContext` 의 인스턴스를 사용 하 `DbContextOptions<TContext>` 인수로] [ 4] 를 사용 하 고는 [ `AddDbContext<TContext>` 메서드] [5].</span><span class="sxs-lookup"><span data-stu-id="5f4cf-113">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as a argument][4] and using the [`AddDbContext<TContext>` method][5].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="5f4cf-114">매개 변수가 없는 생성자를 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="5f4cf-114">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="5f4cf-115">경우에 DbContext를 응용 프로그램 서비스 공급자 로부터 가져올 수 없으며 도구를 찾습니다는 파생 된 `DbContext` 프로젝트 내부에 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="5f4cf-115">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="5f4cf-116">다음은 매개 변수가 없는 생성자를 사용 하 여 인스턴스를 만들려고 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="5f4cf-116">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="5f4cf-117">이 경우 기본 생성자를 수 있습니다는 `DbContext` 을 사용 하도록 구성 된 [ `OnConfiguring` ] [ 6] 메서드.</span><span class="sxs-lookup"><span data-stu-id="5f4cf-117">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][6] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="5f4cf-118">디자인 타임 공장에서</span><span class="sxs-lookup"><span data-stu-id="5f4cf-118">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="5f4cf-119">알 수 있습니다는 도구를 구현 하 여 프로그램 DbContext를 만드는 방법의 `IDesignTimeDbContextFactory<TContext>` 인터페이스:이 인터페이스를 구현 하는 클래스는 파생 된 동일한 프로젝트 중 하나에서 발견 되 면 `DbContext` 또는 응용 프로그램의 시작 프로젝트에 도구는 다음과 같이 무시 됩니다. 대신 DbContext 및 사용 하 여 디자인 타임 팩터리를 만드는 다른 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="5f4cf-119">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="5f4cf-120">`args` 매개 변수는 현재 사용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5f4cf-120">The `args` parameter is currently unused.</span></span> <span data-ttu-id="5f4cf-121">[문제] [ 7] 도구에서 디자인 타임 인수를 지정 하는 기능을 추적 합니다.</span><span class="sxs-lookup"><span data-stu-id="5f4cf-121">There is [an issue][7] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="5f4cf-122">디자인 타임 팩터리 경우 런타임 시 보다 디자인 타임에 대 한 DbContext를 다르게 구성 해야 하는 경우에 특히 유용할 수 있습니다는 `DbContext` DI를 전혀 사용 하지 않는 경우 추가 매개 변수 DI를에 등록 되지 않은 생성자 하나 또는 일부에 대 한 경우 있어야 하 고 싶지 이유는 `BuildWebHost` ASP.NET Core 응용 프로그램의 메서드</span><span class="sxs-lookup"><span data-stu-id="5f4cf-122">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's</span></span>  
<span data-ttu-id="5f4cf-123">`Main` 클래스.</span><span class="sxs-lookup"><span data-stu-id="5f4cf-123">`Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
