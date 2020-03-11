---
title: EF6에서 EF Core로 이식 - EDMX 기반 모델 이식 - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413530"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="fcee3-102">EF6 EDMX 기반 모델을 EF Core로 이식</span><span class="sxs-lookup"><span data-stu-id="fcee3-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="fcee3-103">EF Core는 모델에 대한 EDMX 파일 형식을 지원하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="fcee3-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="fcee3-104">이러한 모델을 이식하기 위한 최고의 옵션은 애플리케이션의 데이터베이스에서 새 코드 기반 모델을 생성하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="fcee3-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="fcee3-105">EF Core NuGet 패키지 설치</span><span class="sxs-lookup"><span data-stu-id="fcee3-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="fcee3-106">`Microsoft.EntityFrameworkCore.Tools` NuGet 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="fcee3-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="fcee3-107">모델 다시 생성</span><span class="sxs-lookup"><span data-stu-id="fcee3-107">Regenerate the model</span></span>

<span data-ttu-id="fcee3-108">이제 리버스 엔지니어링 기능을 사용하여 기존 데이터베이스를 기반의 모델을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fcee3-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="fcee3-109">패키지 관리자 콘솔(도구 –> NuGet 패키지 관리자 -> 패키지 관리자 콘솔)에서 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="fcee3-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="fcee3-110">테이블의 하위 집합 등을 스캐폴드하기 위한 명령 옵션은 [패키지 관리자 콘솔(Visual Studio)](../../core/miscellaneous/cli/powershell.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="fcee3-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="fcee3-111">예를 들어 SQL Server LocalDB 인스턴스의 블로깅 데이터베이스에서 모델을 스캐폴드하기 위한 명령은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="fcee3-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="fcee3-112">EF6 모델 제거</span><span class="sxs-lookup"><span data-stu-id="fcee3-112">Remove EF6 model</span></span>

<span data-ttu-id="fcee3-113">이제 애플리케이션에서 EF6 모델을 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="fcee3-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="fcee3-114">EF Core 및 EF6는 동일한 애플리케이션에서 함께 사용할 수 있으므로 EF6 NuGet 패키지(EntityFramework)가 설치하는 것은 괜찮습니다.</span><span class="sxs-lookup"><span data-stu-id="fcee3-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="fcee3-115">그러나 애플리케이션의 영역에서 EF6를 사용하지 않으려는 경우에는 패키지를 제거하면 주의가 필요한 코드 조각에 컴파일 오류를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fcee3-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="fcee3-116">코드 업데이트</span><span class="sxs-lookup"><span data-stu-id="fcee3-116">Update your code</span></span>

<span data-ttu-id="fcee3-117">이 시점에서 컴파일 오류를 해결하고 코드를 검토하여 EF6과 EF Core 간의 동작 변경이 어떤 영향을 주는지 확인하는 것이 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="fcee3-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="fcee3-118">포트 테스트</span><span class="sxs-lookup"><span data-stu-id="fcee3-118">Test the port</span></span>

<span data-ttu-id="fcee3-119">애플리케이션이 컴파일되는 것만으로 EF Core에 성공적으로 이식되었음을 의미하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="fcee3-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="fcee3-120">애플리케이션의 모든 영역을 테스트하여 어떠한 동작 변경도 애플리케이션에 부정적인 영향을 주지 않았음을 확인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fcee3-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
