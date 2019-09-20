---
title: EF6에서 EF Core로 포팅 하 여 EDMX 기반 모델 포팅
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: a1c978e141f47a39fff59eb5fc417a63afd37b04
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71148992"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="e4ebe-102">EF Core EF6 EDMX 기반 모델 포팅</span><span class="sxs-lookup"><span data-stu-id="e4ebe-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="e4ebe-103">EF Core는 모델에 대 한 EDMX 파일 형식을 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e4ebe-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="e4ebe-104">이러한 모델을 이식 하는 가장 좋은 옵션은 응용 프로그램의 데이터베이스에서 새 코드 기반 모델을 생성 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="e4ebe-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="e4ebe-105">NuGet 패키지 EF Core 설치</span><span class="sxs-lookup"><span data-stu-id="e4ebe-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="e4ebe-106">`Microsoft.EntityFrameworkCore.Tools` NuGet 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="e4ebe-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="e4ebe-107">모델 다시 생성</span><span class="sxs-lookup"><span data-stu-id="e4ebe-107">Regenerate the model</span></span>

<span data-ttu-id="e4ebe-108">이제 리버스 엔지니어링 기능을 사용 하 여 기존 데이터베이스를 기반으로 모델을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e4ebe-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="e4ebe-109">패키지 관리자 콘솔에서 다음 명령을 실행 합니다 (도구 – > NuGet 패키지 관리자-> 패키지 관리자 콘솔).</span><span class="sxs-lookup"><span data-stu-id="e4ebe-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="e4ebe-110">테이블의 하위 집합을 스 캐 폴드 하는 명령 옵션은 [패키지 관리자 콘솔 (Visual Studio)](../../core/miscellaneous/cli/powershell.md) 을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="e4ebe-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="e4ebe-111">예를 들어 SQL Server LocalDB 인스턴스의 블로그 데이터베이스에서 모델을 스 캐 폴드 하는 명령은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="e4ebe-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="e4ebe-112">EF6 model 제거</span><span class="sxs-lookup"><span data-stu-id="e4ebe-112">Remove EF6 model</span></span>

<span data-ttu-id="e4ebe-113">이제 응용 프로그램에서 EF6 모델을 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="e4ebe-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="e4ebe-114">EF Core 및 EF6는 동일한 응용 프로그램에서 함께 사용할 수 있으므로 EF6 NuGet 패키지 (EntityFramework)를 설치 하는 것은 괜찮습니다.</span><span class="sxs-lookup"><span data-stu-id="e4ebe-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="e4ebe-115">그러나 응용 프로그램의 영역에서 EF6를 사용 하지 않으려는 경우에는 패키지를 제거 하면 주의가 필요한 코드 조각에 컴파일 오류를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e4ebe-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="e4ebe-116">코드 업데이트</span><span class="sxs-lookup"><span data-stu-id="e4ebe-116">Update your code</span></span>

<span data-ttu-id="e4ebe-117">이 시점에서 컴파일 오류를 해결 하 고 코드를 검토 하 여 EF6와 EF Core 간의 동작이 어떻게 영향을 주는지 확인 하는 것이 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="e4ebe-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="e4ebe-118">포트 테스트</span><span class="sxs-lookup"><span data-stu-id="e4ebe-118">Test the port</span></span>

<span data-ttu-id="e4ebe-119">응용 프로그램이 컴파일되는 경우에만가 EF Core에 성공적으로 이식 되었다는 것을 의미 하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e4ebe-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="e4ebe-120">응용 프로그램의 모든 영역을 테스트 하 여 어떤 동작 변경도 응용 프로그램에 부정적인 영향을 주지 않는지 확인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e4ebe-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
