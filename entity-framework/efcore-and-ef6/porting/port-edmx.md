---
title: EF6에서 EF Core로 이식-는 EDMX 기반 모델 이식
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 2c3336ac675a830566001a0ddb3777839f52db18
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997413"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="5c636-102">EF core는 EF6 EDMX 기반 모델 이식</span><span class="sxs-lookup"><span data-stu-id="5c636-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="5c636-103">EF Core 모델에 대 한 EDMX 파일 형식을 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5c636-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="5c636-104">이러한 모델의 경우 포트를 응용 프로그램에 대 한 데이터베이스에서 새 코드 기반 모델을 생성 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="5c636-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="5c636-105">EF Core NuGet 패키지를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="5c636-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="5c636-106">설치는 `Microsoft.EntityFrameworkCore.Tools` NuGet 패키지.</span><span class="sxs-lookup"><span data-stu-id="5c636-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="5c636-107">모델 다시 생성</span><span class="sxs-lookup"><span data-stu-id="5c636-107">Regenerate the model</span></span>

<span data-ttu-id="5c636-108">이제 기존 데이터베이스를 기반으로 모델을 만들려면 리버스 엔지니어링 기능을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5c636-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="5c636-109">패키지 관리자 콘솔에서 다음 명령을 실행 (도구-NuGet 패키지 관리자 > 패키지 관리자 콘솔->).</span><span class="sxs-lookup"><span data-stu-id="5c636-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="5c636-110">참조 [패키지 관리자 콘솔 (Visual Studio)](../../core/miscellaneous/cli/powershell.md) 테이블 등의 하위 집합을 스 캐 폴딩 하려면 명령 옵션에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="5c636-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="5c636-111">예를 들어, 다음은 SQL Server LocalDB 인스턴스에서 Blogging 데이터베이스에서 모델을 스 캐 폴딩 명령입니다.</span><span class="sxs-lookup"><span data-stu-id="5c636-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="5c636-112">EF6 모델 제거</span><span class="sxs-lookup"><span data-stu-id="5c636-112">Remove EF6 model</span></span>

<span data-ttu-id="5c636-113">이제 응용 프로그램에서 EF6 모델로 제거할는 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5c636-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="5c636-114">EF Core 및 EF6 사용된-side-by-side 동일한 응용 프로그램 수 설치 (EntityFramework) EF6 NuGet에서 패키지를 그대로 두는 것이 괜찮습니다.</span><span class="sxs-lookup"><span data-stu-id="5c636-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="5c636-115">그러나 EF6 응용 프로그램의 모든 영역에서 사용 하려는 없습니다, 하는 경우 다음 패키지를 제거는 데 도움이 됩니다 주의가 필요한 코드 부분에 컴파일 오류가 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="5c636-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="5c636-116">코드 업데이트</span><span class="sxs-lookup"><span data-stu-id="5c636-116">Update your code</span></span>

<span data-ttu-id="5c636-117">이 시점에서 주소 지정 컴파일 오류 및 EF6 및 EF Core 간의 동작 변경 내용 수에 영향을 줍니다 있는지 검토 한 후 코드의 문제는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5c636-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="5c636-118">포트를 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="5c636-118">Test the port</span></span>

<span data-ttu-id="5c636-119">응용 프로그램을 컴파일합니다. 단지 EF Core로 이식 성공적으로 의미 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5c636-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="5c636-120">동작 변경 내용이 부정적인 영향을 준 응용 프로그램을 확인 하도록 응용 프로그램의 모든 영역을 테스트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5c636-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>

> [!TIP]
> <span data-ttu-id="5c636-121">참조 [기존 데이터베이스를 사용 하 여 ASP.NET Core에서 EF Core 시작](xref:core/get-started/aspnetcore/existing-db) 기존 데이터베이스를 사용 하는 방법에 대 한 추가 참조에 대 한</span><span class="sxs-lookup"><span data-stu-id="5c636-121">See [Getting Started with EF Core on ASP.NET Core with an Existing Database](xref:core/get-started/aspnetcore/existing-db) for an additional reference on how to work with an existing database,</span></span> 
