---
title: EF6에서 EF Core로 이식-코드 기반 모델 이식
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997051"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="b6e25-102">EF core는 EF6 코드 기반 모델 이식</span><span class="sxs-lookup"><span data-stu-id="b6e25-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="b6e25-103">모든 주의 읽은 경우 포트 준비가 다음 다음은 시작 하는 데 도움이 되는 몇 가지 지침입니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="b6e25-104">EF Core NuGet 패키지를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="b6e25-105">EF Core를 사용 하려면 사용 하려는 데이터베이스 공급자에 대 한 NuGet 패키지를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="b6e25-106">예를 들어, SQL Server를 대상으로 하는 경우 설치 하는 것 `Microsoft.EntityFrameworkCore.SqlServer`입니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="b6e25-107">참조 [데이터베이스 공급자](../../core/providers/index.md) 세부 정보에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="b6e25-108">마이그레이션을 사용 하려는 경우도 설치 해야 합니다 `Microsoft.EntityFrameworkCore.Tools` 패키지 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="b6e25-109">EF Core 및 EF6 사용된-side-by-side 동일한 응용 프로그램 수 설치 (EntityFramework) EF6 NuGet에서 패키지를 그대로 두는 것이 괜찮습니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="b6e25-110">그러나 EF6 응용 프로그램의 모든 영역에서 사용 하려는 없습니다, 하는 경우 다음 패키지를 제거는 데 도움이 됩니다 주의가 필요한 코드 부분에 컴파일 오류가 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="b6e25-111">네임 스페이스를 교환 합니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-111">Swap namespaces</span></span>

<span data-ttu-id="b6e25-112">EF6에서 사용 하는 대부분의 Api에는 `System.Data.Entity` 네임 스페이스 (및 하위 네임 스페이스 관련)입니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="b6e25-113">첫 번째 코드 변경을 교환 하는 것은 `Microsoft.EntityFrameworkCore` 네임 스페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="b6e25-114">일반적으로 파생된 컨텍스트 코드 파일을 시작 하는 작업 해야 여기에서 발생 하는 컴파일 오류를 해결 합니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="b6e25-115">상황에 맞는 구성 (연결 등.)</span><span class="sxs-lookup"><span data-stu-id="b6e25-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="b6e25-116">에 설명 된 대로 [보장 EF Core는 작업 응용 프로그램에 대 한](ensure-requirements.md), EF Core가 덜 magic 주위에 연결할 데이터베이스를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="b6e25-117">재정의 해야 합니다는 `OnConfiguring` 데이터베이스 공급자 특정 API 사용 하 여 데이터베이스로 연결을 설정 하 고 파생된 컨텍스트 메서드.</span><span class="sxs-lookup"><span data-stu-id="b6e25-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="b6e25-118">응용 프로그램에서 연결 문자열을 저장 하는 대부분의 EF6 응용 프로그램 `App/Web.config` 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="b6e25-119">EF Core에서 읽을 있습니다 사용 하 여이 연결 문자열을 `ConfigurationManager` API.</span><span class="sxs-lookup"><span data-stu-id="b6e25-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="b6e25-120">에 대 한 참조를 추가 해야 합니다 `System.Configuration` 프레임 워크 어셈블리를이 API를 사용 하는 일을 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="b6e25-121">코드 업데이트</span><span class="sxs-lookup"><span data-stu-id="b6e25-121">Update your code</span></span>

<span data-ttu-id="b6e25-122">이 시점에서 컴파일 오류를 해결 하는 경우 변경 된 동작은 영향을 확인 하려면 코드를 검토 한 문제는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="b6e25-123">기존 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="b6e25-123">Existing migrations</span></span>

<span data-ttu-id="b6e25-124">기존 EF6 마이그레이션 EF Core로 이식 가능 하면 실제로 없습니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="b6e25-125">가능한 경우 EF6의 모든 이전 마이그레이션 데이터베이스에 적용 된 스키마는 마이그레이션 시작 가리킨 EF Core를 사용 하 여 가정 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="b6e25-126">이 작업을 수행 하려면 사용 된 `Add-Migration` 모델 EF Core로 이식 되 면 마이그레이션을 추가 하는 명령입니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="b6e25-127">다음의 모든 코드를 제거 합니다 `Up` 및 `Down` 스 캐 폴드 된 마이그레이션 메서드.</span><span class="sxs-lookup"><span data-stu-id="b6e25-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="b6e25-128">이후 마이그레이션 해당 초기 마이그레이션을 스 캐 폴드 된 경우 모델을 비교 합니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="b6e25-129">포트를 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-129">Test the port</span></span>

<span data-ttu-id="b6e25-130">응용 프로그램을 컴파일합니다. 단지 EF Core로 이식 성공적으로 의미 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="b6e25-131">동작 변경 내용이 부정적인 영향을 준 응용 프로그램을 확인 하도록 응용 프로그램의 모든 영역을 테스트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b6e25-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
