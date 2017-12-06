---
title: "디자인 타임 DbContext 만들기-EF 코어"
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 5fcd9e362d76127e7acadd9e552ef3ac90967a37
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="98436-102">디자인 타임 DbContext 만들기</span><span class="sxs-lookup"><span data-stu-id="98436-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="98436-103">명령에는 디자인에 만들려는 DbContext 인스턴스 필요 EF 도구 중 일부 (예를 들어 실행 하는 경우 마이그레이션 명령) 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="98436-103">Some of the EF Tools commands require a DbContext instance to be created at design time (for example, when running Migrations commands).</span></span> <span data-ttu-id="98436-104">여러 가지 방법으로 도구를 만들려고 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="98436-104">There are various ways the tools try to create it.</span></span>

<a name="from-application-services"></a><span data-ttu-id="98436-105">응용 프로그램 서비스</span><span class="sxs-lookup"><span data-stu-id="98436-105">From application services</span></span>
-------------------------
<span data-ttu-id="98436-106">시작 프로젝트는 ASP.NET Core 응용 프로그램, 도구는 응용 프로그램의 서비스 공급자에서 DbContext 개체를 가져올 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="98436-106">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span> <span data-ttu-id="98436-107">호출 하 여 가져올은 `Program.BuildWebHost()` 에 액세스 하 고는 `IWebHost.Services` 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="98436-107">They obtain it by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span> <span data-ttu-id="98436-108">사용 하 여 모든 DbContext 등록 `IServiceCollection.AddDbContext<TContext>()` 찾을 수 있으며 이런 방식이으로 만든 합니다.</span><span class="sxs-lookup"><span data-stu-id="98436-108">Any DbContext registered using `IServiceCollection.AddDbContext<TContext>()` can be found and created this way.</span></span> <span data-ttu-id="98436-109">이 패턴 [ASP.NET 코어 2.0에 도입 된][1]</span><span class="sxs-lookup"><span data-stu-id="98436-109">This pattern was [introduced in ASP.NET Core 2.0][1]</span></span>

<a name="using-the-default-constructor"></a><span data-ttu-id="98436-110">기본 생성자를 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="98436-110">Using the default constructor</span></span>
-----------------------------
<span data-ttu-id="98436-111">DbContext를 응용 프로그램 서비스 공급자 로부터 가져올 수 없으며 도구 프로젝트 내부에 DbContext 형식 찾아보십시오.</span><span class="sxs-lookup"><span data-stu-id="98436-111">If the DbContext can't be obtained from the application service provider, the tools look for the DbContext type inside the project.</span></span> <span data-ttu-id="98436-112">기본 생성자를 사용 하 여 만들 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="98436-112">They try to create it using its default constructor.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="98436-113">디자인 타임 공장에서</span><span class="sxs-lookup"><span data-stu-id="98436-113">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="98436-114">알 수 있습니다는 도구를 구현 하 여 프로그램 DbContext를 만드는 방법을 `IDesignTimeDbContextFactory`합니다.</span><span class="sxs-lookup"><span data-stu-id="98436-114">You can also tell the tools how to create your DbContext by implementing `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="98436-115">이 인터페이스를 구현 하는 클래스는 프로젝트 내부에서 발견 되 면 도구 DbContext를 만드는 다른 방법으로 무시 합니다.</span><span class="sxs-lookup"><span data-stu-id="98436-115">If a class implementing this interface is found inside your project, the tools bypass the other ways of creating the DbContext.</span></span>
<span data-ttu-id="98436-116">팩터리 디자인 타임에 항상 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="98436-116">They always use the factory at design time.</span></span> <span data-ttu-id="98436-117">팩터리는 런타임 시 보다 디자인 타임에 대 한 DbContext를 다르게 구성 해야 하는 경우에 특히 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="98436-117">A factory is especially useful if you need to configure the DbContext differently for design time than at runtime.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
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
> <span data-ttu-id="98436-118">`args` 매개 변수는 현재 사용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="98436-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="98436-119">[문제] [ 2] 도구에서 디자인 타임 인수를 지정 하는 기능을 추적 합니다.</span><span class="sxs-lookup"><span data-stu-id="98436-119">There is [an issue][2] tracking the ability to specify design-time arguments from the tools.</span></span>

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
