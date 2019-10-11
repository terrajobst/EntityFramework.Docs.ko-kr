---
title: EF6에서 EF Core로 포팅-코드 기반 모델 포팅-EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181224"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="fe2cf-102">EF Core EF6 코드 기반 모델 포팅</span><span class="sxs-lookup"><span data-stu-id="fe2cf-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="fe2cf-103">모든 주의 사항을 읽고 포트 준비가 완료 되 면 시작 하는 데 도움이 되는 몇 가지 지침을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="fe2cf-104">NuGet 패키지 EF Core 설치</span><span class="sxs-lookup"><span data-stu-id="fe2cf-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="fe2cf-105">EF Core를 사용 하려면 사용 하려는 데이터베이스 공급자에 대 한 NuGet 패키지를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="fe2cf-106">예를 들어 SQL Server를 대상으로 지정 하는 경우 `Microsoft.EntityFrameworkCore.SqlServer`을 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="fe2cf-107">자세한 내용은 [데이터베이스 공급자](../../core/providers/index.md) 를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="fe2cf-108">마이그레이션을 사용 하려는 경우 `Microsoft.EntityFrameworkCore.Tools` 패키지도 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="fe2cf-109">EF Core 및 EF6는 동일한 응용 프로그램에서 함께 사용할 수 있으므로 EF6 NuGet 패키지 (EntityFramework)를 설치 하는 것은 괜찮습니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="fe2cf-110">그러나 응용 프로그램의 영역에서 EF6를 사용 하지 않으려는 경우에는 패키지를 제거 하면 주의가 필요한 코드 조각에 컴파일 오류를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="fe2cf-111">네임 스페이스 바꾸기</span><span class="sxs-lookup"><span data-stu-id="fe2cf-111">Swap namespaces</span></span>

<span data-ttu-id="fe2cf-112">EF6에서 사용 하는 대부분의 Api는 `System.Data.Entity` 네임 스페이스와 관련 하위 네임 스페이스에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="fe2cf-113">첫 번째 코드 변경 내용은 `Microsoft.EntityFrameworkCore` 네임 스페이스로 교체 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="fe2cf-114">일반적으로 파생 컨텍스트 코드 파일로 시작 하 여 발생 하는 컴파일 오류를 해결 하는 작업을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="fe2cf-115">컨텍스트 구성 (연결 등)</span><span class="sxs-lookup"><span data-stu-id="fe2cf-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="fe2cf-116">[응용 프로그램에 EF Core 작동 하는지 확인](ensure-requirements.md)에서 설명한 것 처럼 EF Core에 연결할 데이터베이스를 검색 하는 것이 더 나을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="fe2cf-117">파생 된 컨텍스트에서 `OnConfiguring` 메서드를 재정의 하 고 데이터베이스 공급자 특정 API를 사용 하 여 데이터베이스에 대 한 연결을 설정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="fe2cf-118">대부분의 EF6 응용 프로그램은 응용 프로그램 `App/Web.config` 파일에 연결 문자열을 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="fe2cf-119">EF Core에서 `ConfigurationManager` API를 사용 하 여이 연결 문자열을 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="fe2cf-120">이 API를 사용하려면 `System.Configuration` 프레임워크 어셈블리에 대한 참조를 추가해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="fe2cf-121">코드 업데이트</span><span class="sxs-lookup"><span data-stu-id="fe2cf-121">Update your code</span></span>

<span data-ttu-id="fe2cf-122">이 시점에서 컴파일 오류를 해결 하 고 코드를 검토 하 여 동작 변경으로 인해 영향을 주는지 확인 하는 것이 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="fe2cf-123">기존 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="fe2cf-123">Existing migrations</span></span>

<span data-ttu-id="fe2cf-124">기존 EF6 마이그레이션을 EF Core로 이식 하는 것은 사실상 적절 한 방법이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="fe2cf-125">가능 하면 EF6의 모든 이전 마이그레이션이 데이터베이스에 적용 된 후 EF Core를 사용 하 여 해당 지점에서 스키마 마이그레이션을 시작 하는 것이 가장 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="fe2cf-126">이렇게 하려면 모델을 EF Core로 이식 하 고 나면 `Add-Migration` 명령을 사용 하 여 마이그레이션을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="fe2cf-127">그런 다음 스 캐 폴드 마이그레이션의 `Up` 및 `Down` 메서드에서 모든 코드를 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="fe2cf-128">이후 마이그레이션은 초기 마이그레이션이 스 캐 폴드 때의 모델과 비교 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="fe2cf-129">포트 테스트</span><span class="sxs-lookup"><span data-stu-id="fe2cf-129">Test the port</span></span>

<span data-ttu-id="fe2cf-130">응용 프로그램이 컴파일되는 경우에만가 EF Core에 성공적으로 이식 되었다는 것을 의미 하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="fe2cf-131">응용 프로그램의 모든 영역을 테스트 하 여 어떤 동작 변경도 응용 프로그램에 부정적인 영향을 주지 않는지 확인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe2cf-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
