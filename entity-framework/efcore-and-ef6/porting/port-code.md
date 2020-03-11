---
title: EF6에서 EF Core로 이식 - 코드 기반 모델 이식 - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413860"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="e5fd3-102">EF6 코드 기반 모델을 EF Core로 이식</span><span class="sxs-lookup"><span data-stu-id="e5fd3-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="e5fd3-103">모든 주의 사항을 읽고 포트 준비가 완료되었으면 시작하는 데 도움이 되는 몇 가지 지침을 확인하세요.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="e5fd3-104">EF Core NuGet 패키지 설치</span><span class="sxs-lookup"><span data-stu-id="e5fd3-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="e5fd3-105">EF Core를 사용하려면 사용할 데이터베이스 공급자에 대한 NuGet 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="e5fd3-106">예를 들어 SQL Server를 대상으로 지정하는 경우 `Microsoft.EntityFrameworkCore.SqlServer`를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="e5fd3-107">자세한 내용은 [데이터베이스 공급자](../../core/providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="e5fd3-108">마이그레이션을 사용하려는 경우에는 `Microsoft.EntityFrameworkCore.Tools` 패키지도 설치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="e5fd3-109">EF Core 및 EF6는 동일한 애플리케이션에서 함께 사용할 수 있으므로 EF6 NuGet 패키지(EntityFramework)가 설치하는 것은 괜찮습니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="e5fd3-110">그러나 애플리케이션의 영역에서 EF6를 사용하지 않으려는 경우에는 패키지를 제거하면 주의가 필요한 코드 조각에 컴파일 오류를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="e5fd3-111">네임스페이스 전환</span><span class="sxs-lookup"><span data-stu-id="e5fd3-111">Swap namespaces</span></span>

<span data-ttu-id="e5fd3-112">EF6에서 사용하는 대부분의 API는 `System.Data.Entity` 네임스페이스(및 관련 하위 네임스페이스)에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="e5fd3-113">첫 번째 코드 변경은 `Microsoft.EntityFrameworkCore` 네임스페이스로 전환하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="e5fd3-114">일반적으로 파생 컨텍스트 코드 파일로 시작하여 발생하는 컴파일 오류를 해결하면서 작업을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="e5fd3-115">컨텍스트 구성(연결 등)</span><span class="sxs-lookup"><span data-stu-id="e5fd3-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="e5fd3-116">[EF Core가 애플리케이션에 대해 작동하는지 확인](ensure-requirements.md)에 설명한 대로 EF Core에는 연결할 데이터베이스를 감지하는 기능이 부족합니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="e5fd3-117">파생 컨텍스트에서 `OnConfiguring` 메서드를 재정의하고 데이터베이스 공급자별 API를 사용하여 데이터베이스에 대한 연결을 설정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="e5fd3-118">대부분의 EF6 애플리케이션은 애플리케이션 `App/Web.config` 파일에 연결 문자열을 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="e5fd3-119">EF Core에서는 `ConfigurationManager` API를 사용하여 이 연결 문자열을 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="e5fd3-120">이 API를 사용하려면 `System.Configuration` 프레임워크 어셈블리에 대한 참조를 추가해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a><span data-ttu-id="e5fd3-121">코드 업데이트</span><span class="sxs-lookup"><span data-stu-id="e5fd3-121">Update your code</span></span>

<span data-ttu-id="e5fd3-122">이 시점에서 컴파일 오류를 해결하고 코드를 검토하여 동작 변경이 어떤 영향을 주는지 확인하는 것이 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="e5fd3-123">기존 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="e5fd3-123">Existing migrations</span></span>

<span data-ttu-id="e5fd3-124">기존의 EF6 마이그레이션을 EF Core로 이식하는 것은 사실상 적절한 방법이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="e5fd3-125">가능하면 EF6의 모든 이전 마이그레이션이 데이터베이스에 적용된 후 EF Core를 사용하여 해당 지점에서 스키마 마이그레이션을 시작하는 것이 가장 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="e5fd3-126">이렇게 하려면 모델을 EF Core로 이식할 때 `Add-Migration` 명령을 사용하여 마이그레이션을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="e5fd3-127">그런 다음 스캐폴드 마이그레이션의 `Up` 및 `Down` 메서드에서 모든 코드를 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="e5fd3-128">후속 마이그레이션은 초기 마이그레이션이 스캐폴드되었을 때의 모델과 비교됩니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="e5fd3-129">포트 테스트</span><span class="sxs-lookup"><span data-stu-id="e5fd3-129">Test the port</span></span>

<span data-ttu-id="e5fd3-130">애플리케이션이 컴파일되는 것만으로 EF Core에 성공적으로 이식되었음을 의미하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="e5fd3-131">애플리케이션의 모든 영역을 테스트하여 어떠한 동작 변경도 애플리케이션에 부정적인 영향을 주지 않았음을 확인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5fd3-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
