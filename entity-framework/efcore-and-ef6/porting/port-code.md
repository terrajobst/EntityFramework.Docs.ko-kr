---
title: EF6에서 EF 코어-코드 기반 모델을 포팅로 이식
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052953"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="4fe22-102">EF6 코드 기반 모델 EF 코어로 이식</span><span class="sxs-lookup"><span data-stu-id="4fe22-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="4fe22-103">모든 주의 사항을 참고 포트 준비가 된 경우 다음 다음은 몇 가지 지침을 시작할 수입니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="4fe22-104">EF Core NuGet 패키지 설치</span><span class="sxs-lookup"><span data-stu-id="4fe22-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="4fe22-105">EF 코어를 사용 하려면 사용 하려는 데이터베이스 공급자에 대 한 NuGet 패키지를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="4fe22-106">예를 들어 SQL Server를 대상으로 하는 경우 설치 하는 것 `Microsoft.EntityFrameworkCore.SqlServer`합니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="4fe22-107">참조 [데이터베이스 공급자](../../core/providers/index.md) 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="4fe22-108">마이그레이션을, 사용 하려는 경우 설치 해야는 `Microsoft.EntityFrameworkCore.Tools` 패키지 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="4fe22-109">EF 코어 및 EF6 사용된-함께 동일한 응용 프로그램 수 그대로 EF6 NuGet 패키지 (EntityFramework)가 설치 하는 것은 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="4fe22-110">그러나 EF6 응용 프로그램의 모든 영역에서 사용 하려는 아닌, 패키지를 설치 제거 할 경우 주의 해야 하는 코드의 부분에 컴파일 오류가 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="4fe22-111">스왑 네임 스페이스</span><span class="sxs-lookup"><span data-stu-id="4fe22-111">Swap namespaces</span></span>

<span data-ttu-id="4fe22-112">EF6에서 사용 하는 대부분의 Api에 있는 `System.Data.Entity` 네임 스페이스 (및 하위 네임 스페이스 관련)입니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="4fe22-113">첫 번째 코드 변경 내용이를 교체 하는 `Microsoft.EntityFrameworkCore` 네임 스페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="4fe22-114">일반적으로 파생 된 컨텍스트 코드 파일에로 시작 하는 다음 하겠지만 여기에서 나타나는 순서 대로 컴파일 오류를 해결 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="4fe22-115">컨텍스트 구성 (연결 등).</span><span class="sxs-lookup"><span data-stu-id="4fe22-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="4fe22-116">에 설명 된 대로 [확인 EF 코어는 작업 응용 프로그램에 대 한](ensure-requirements.md), EF 코어에 덜 매직 주위에 연결할 데이터베이스를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="4fe22-117">재정의 해야 합니다는 `OnConfiguring` 데이터베이스 공급자 특정 API 사용 하 여 데이터베이스에 연결을 설정 하 고 파생된 컨텍스트 메서드.</span><span class="sxs-lookup"><span data-stu-id="4fe22-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="4fe22-118">대부분의 EF6 응용 프로그램의 응용 프로그램에서 연결 문자열을 저장 `App/Web.config` 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="4fe22-119">EF 코어 읽을 있습니다 사용 하 여이 연결 문자열은 `ConfigurationManager` API입니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="4fe22-120">에 대 한 참조를 추가 해야 할 수는 `System.Configuration` 프레임 워크 어셈블리를이 API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="4fe22-121">코드 업데이트</span><span class="sxs-lookup"><span data-stu-id="4fe22-121">Update your code</span></span>

<span data-ttu-id="4fe22-122">이 시점에서 컴파일 오류를 해결 하 고 코드를 참조 하는 경우의 동작 변경 내용은 영향을 검토 하는 것은 것입니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="4fe22-123">기존 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="4fe22-123">Existing migrations</span></span>

<span data-ttu-id="4fe22-124">기존 EF6 마이그레이션 EF 코어로 이식 하는 가능한 방법을 그다지 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="4fe22-125">가능 하면 데이터베이스에 적용 된 모든 이전 마이그레이션이 EF6에서 시작 하는에서 스키마 마이그레이션 가리킨 EF 코어를 사용 하 여 가정 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="4fe22-126">이 작업을 수행 하려면 사용 된 `Add-Migration` 모델 EF 코어로 이식 되 면 마이그레이션 추가할 명령입니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="4fe22-127">다음 그림에서 코드를 모두 제거는 `Up` 및 `Down` 스 캐 폴드 마이그레이션의 메서드.</span><span class="sxs-lookup"><span data-stu-id="4fe22-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="4fe22-128">마이그레이션 이후 해당 초기 마이그레이션을 스 캐 폴드 되었습니다 때 모델에 비교 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="4fe22-129">포트를 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-129">Test the port</span></span>

<span data-ttu-id="4fe22-130">응용 프로그램 컴파일되기 때문에 EF 코어 성공적으로 이식 의미 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="4fe22-131">응용 프로그램 악영향을 동작 변경 되도록 응용 프로그램의 모든 영역을 테스트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4fe22-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
