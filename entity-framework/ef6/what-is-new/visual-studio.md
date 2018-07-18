---
title: Visual Studio 릴리스-EF6
author: divega
ms.date: 2018-07-05
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 028FF890-4EDB-4F03-AE53-72F9C33EC92F
caps.latest.revision: 3
ms.openlocfilehash: 7bd08a46b1d6acc5a565952e834f01546a5262c8
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121845"
---
# <a name="visual-studio-releases"></a><span data-ttu-id="3a86f-102">Visual Studio 릴리스</span><span class="sxs-lookup"><span data-stu-id="3a86f-102">Visual Studio Releases</span></span>

<span data-ttu-id="3a86f-103">항상 최신 버전의 Visual Studio를 사용 하 여.NET, NuGet 및 Entity Framework에 대 한 최신 도구를 포함 하기 때문에 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-103">We recommend to always use the latest version of Visual Studio because it contains the latest tools for .NET, NuGet, and Entity Framework.</span></span>
<span data-ttu-id="3a86f-104">사실, 다양 한 샘플 및 Entity Framework 설명서에서 연습에는 최신 버전의 Visual Studio를 사용 하는 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-104">In fact, the various samples and walkthroughs across the Entity Framework documentation assume that you are using a recent version of Visual Studio.</span></span>

<span data-ttu-id="3a86f-105">사용 하 여 다른 버전의 Entity Framework를 사용 하 여 이전 버전의 Visual Studio 야으로 계정에 몇 가지 차이점이 있지만 불가능 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-105">It is possible however to use older versions of Visual Studio with different versions of Entity Framework as long as you take into account some differences:</span></span>

## <a name="visual-studio-2017-157-and-newer"></a><span data-ttu-id="3a86f-106">Visual Studio 2017 15.7 이상</span><span class="sxs-lookup"><span data-stu-id="3a86f-106">Visual Studio 2017 15.7 and newer</span></span>

- <span data-ttu-id="3a86f-107">이 버전의 Visual Studio 최신 버전의 Entity Framework 도구 및 EF 6.2 런타임에서 포함 하 고 추가 설치 단계가 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-107">This version of Visual Studio includes the latest release of Entity Framework tools and the EF 6.2 runtime, and does not require additional setup steps.</span></span>
<span data-ttu-id="3a86f-108">참조 [What's New](~/ef6/what-is-new/index.md) 이러한 릴리스에 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-108">See [What's New](~/ef6/what-is-new/index.md) for more details on these releases.</span></span>
- <span data-ttu-id="3a86f-109">Entity Framework EF 도구를 사용 하 여 새 프로젝트 추가 EF 6.2 NuGet 패키지를 자동으로 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-109">Adding Entity Framework to new projects using the EF tools will automatically add the EF 6.2 NuGet package.</span></span>
<span data-ttu-id="3a86f-110">수동으로 설치 하거나 온라인에서 사용할 수 있는 모든 EF NuGet 패키지를 업그레이드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-110">You can manually install or upgrade to any EF NuGet package available online.</span></span>
- <span data-ttu-id="3a86f-111">기본적으로이 버전의 Visual Studio를 사용 하 여 사용 가능한 SQL Server 인스턴스는 호출 MSSQLLocalDB LocalDB 인스턴스입니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-111">By default, the SQL Server instance available with this version of Visual Studio is a LocalDB instance called MSSQLLocalDB.</span></span>
<span data-ttu-id="3a86f-112">사용 해야 하는 연결 문자열의 서버 섹션은 "(localdb)\\MSSQLLocalDB"입니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-112">The server section of connection string you should use is "(localdb)\\MSSQLLocalDB".</span></span>
<span data-ttu-id="3a86f-113">접두사로 축 자 문자열을 사용 해야 `@` 백슬래시를 두 번 또는 "\\\\" C# 코드에서 연결 문자열을 지정 하는 경우.</span><span class="sxs-lookup"><span data-stu-id="3a86f-113">Remember to use a verbatim string prefixed with `@` or double back-slashes "\\\\" when specifying a connection string in C# code.</span></span>  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a><span data-ttu-id="3a86f-114">Visual Studio 2015와 Visual Studio 2017 15.6</span><span class="sxs-lookup"><span data-stu-id="3a86f-114">Visual Studio 2015 to Visual Studio 2017 15.6</span></span>

- <span data-ttu-id="3a86f-115">이러한 버전의 Visual Studio는 Entity Framework 도구 및 런타임 6.1.3에 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-115">These versions of Visual Studio include Entity Framework tools and runtime 6.1.3.</span></span>
<span data-ttu-id="3a86f-116">참조 [지난 해제](~/ef6/what-is-new/past-releases.md#ef-613) 이러한 릴리스에 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-116">See [Past Releases](~/ef6/what-is-new/past-releases.md#ef-613) for more details on these releases.</span></span>
- <span data-ttu-id="3a86f-117">Entity Framework EF 도구를 사용 하 여 새 프로젝트에는 자동으로 추가 EF 6.1.3 NuGet 패키지.</span><span class="sxs-lookup"><span data-stu-id="3a86f-117">Adding Entity Framework to new projects using the EF tools will automatically add the EF 6.1.3 NuGet package.</span></span>
<span data-ttu-id="3a86f-118">수동으로 설치 하거나 온라인에서 사용할 수 있는 모든 EF NuGet 패키지를 업그레이드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-118">You can manually install or upgrade to any EF NuGet package available online.</span></span>
- <span data-ttu-id="3a86f-119">기본적으로이 버전의 Visual Studio를 사용 하 여 사용 가능한 SQL Server 인스턴스는 호출 MSSQLLocalDB LocalDB 인스턴스입니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-119">By default, the SQL Server instance available with this version of Visual Studio is a LocalDB instance called MSSQLLocalDB.</span></span>
<span data-ttu-id="3a86f-120">사용 해야 하는 연결 문자열의 서버 섹션은 "(localdb)\\MSSQLLocalDB"입니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-120">The server section of connection string you should use is "(localdb)\\MSSQLLocalDB".</span></span>
<span data-ttu-id="3a86f-121">접두사로 축 자 문자열을 사용 해야 `@` 백슬래시를 두 번 또는 "\\\\" C# 코드에서 연결 문자열을 지정 하는 경우.</span><span class="sxs-lookup"><span data-stu-id="3a86f-121">Remember to use a verbatim string prefixed with `@` or double back-slashes "\\\\" when specifying a connection string in C# code.</span></span>  


## <a name="visual-studio-2013"></a><span data-ttu-id="3a86f-122">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3a86f-122">Visual Studio 2013</span></span>
- <span data-ttu-id="3a86f-123">이 버전의 Visual Studio를 포함 하 고 이전 버전의 Entity Framework 도구 및 런타임입니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-123">This version of Visual Studio includes and older version of Entity Framework tools and runtime.</span></span>
<span data-ttu-id="3a86f-124">6.1.3, Entity Framework 도구를 업그레이드 하는 것이 좋습니다.를 사용 하 여 [설치 관리자](https://www.microsoft.com/en-us/download/details.aspx?id=40762) Microsoft 다운로드 센터에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-124">It is recommended that you upgrade to Entity Framework Tools 6.1.3, using [the installer](https://www.microsoft.com/en-us/download/details.aspx?id=40762) available in the Microsoft Download Center.</span></span>
<span data-ttu-id="3a86f-125">참조 [지난 해제](~/ef6/what-is-new/past-releases.md#ef-613) 이러한 릴리스에 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-125">See [Past Releases](~/ef6/what-is-new/past-releases.md#ef-613) for more details on these releases.</span></span>
- <span data-ttu-id="3a86f-126">EF 6.1.3 자동으로 추가 업그레이드 EF 도구를 사용 하 여 새 프로젝트에서 Entity Framework 추가 NuGet 패키지.</span><span class="sxs-lookup"><span data-stu-id="3a86f-126">Adding Entity Framework to new projects using the upgraded EF tools will automatically add the EF 6.1.3 NuGet package.</span></span>
<span data-ttu-id="3a86f-127">수동으로 설치 하거나 온라인에서 사용할 수 있는 모든 EF NuGet 패키지를 업그레이드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-127">You can manually install or upgrade to any EF NuGet package available online.</span></span>
- <span data-ttu-id="3a86f-128">기본적으로이 버전의 Visual Studio를 사용 하 여 사용 가능한 SQL Server 인스턴스는 호출 MSSQLLocalDB LocalDB 인스턴스입니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-128">By default, the SQL Server instance available with this version of Visual Studio is a LocalDB instance called MSSQLLocalDB.</span></span>
<span data-ttu-id="3a86f-129">사용 해야 하는 연결 문자열의 서버 섹션은 "(localdb)\\MSSQLLocalDB"입니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-129">The server section of connection string you should use is "(localdb)\\MSSQLLocalDB".</span></span>
<span data-ttu-id="3a86f-130">접두사로 축 자 문자열을 사용 해야 `@` 백슬래시를 두 번 또는 "\\\\" C# 코드에서 연결 문자열을 지정 하는 경우.</span><span class="sxs-lookup"><span data-stu-id="3a86f-130">Remember to use a verbatim string prefixed with `@` or double back-slashes "\\\\" when specifying a connection string in C# code.</span></span>  

## <a name="visual-studio-2012"></a><span data-ttu-id="3a86f-131">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="3a86f-131">Visual Studio 2012</span></span>

- <span data-ttu-id="3a86f-132">이 버전의 Visual Studio를 포함 하 고 이전 버전의 Entity Framework 도구 및 런타임입니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-132">This version of Visual Studio includes and older version of Entity Framework tools and runtime.</span></span>
<span data-ttu-id="3a86f-133">6.1.3, Entity Framework 도구를 업그레이드 하는 것이 좋습니다.를 사용 하 여 [설치 관리자](https://www.microsoft.com/en-us/download/details.aspx?id=40762) Microsoft 다운로드 센터에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-133">It is recommended that you upgrade to Entity Framework Tools 6.1.3, using [the installer](https://www.microsoft.com/en-us/download/details.aspx?id=40762) available in the Microsoft Download Center.</span></span>
<span data-ttu-id="3a86f-134">참조 [지난 해제](~/ef6/what-is-new/past-releases.md#ef-613) 이러한 릴리스에 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-134">See [Past Releases](~/ef6/what-is-new/past-releases.md#ef-613) for more details on these releases.</span></span>
- <span data-ttu-id="3a86f-135">EF 6.1.3 자동으로 추가 업그레이드 EF 도구를 사용 하 여 새 프로젝트에서 Entity Framework 추가 NuGet 패키지.</span><span class="sxs-lookup"><span data-stu-id="3a86f-135">Adding Entity Framework to new projects using the upgraded EF tools will automatically add the EF 6.1.3 NuGet package.</span></span>
<span data-ttu-id="3a86f-136">수동으로 설치 하거나 온라인에서 사용할 수 있는 모든 EF NuGet 패키지를 업그레이드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-136">You can manually install or upgrade to any EF NuGet package available online.</span></span>
- <span data-ttu-id="3a86f-137">기본적으로이 버전의 Visual Studio를 사용 하 여 사용 가능한 SQL Server 인스턴스는 호출 v11.0 LocalDB 인스턴스입니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-137">By default, the SQL Server instance available with this version of Visual Studio is a LocalDB instance called v11.0.</span></span>
<span data-ttu-id="3a86f-138">사용 해야 하는 연결 문자열의 서버 섹션은 "(localdb)\\v11.0"입니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-138">The server section of connection string you should use is "(localdb)\\v11.0".</span></span>
<span data-ttu-id="3a86f-139">접두사로 축 자 문자열을 사용 해야 `@` 백슬래시를 두 번 또는 "\\\\" C# 코드에서 연결 문자열을 지정 하는 경우.</span><span class="sxs-lookup"><span data-stu-id="3a86f-139">Remember to use a verbatim string prefixed with `@` or double back-slashes "\\\\" when specifying a connection string in C# code.</span></span>  

## <a name="visual-studio-2010"></a><span data-ttu-id="3a86f-140">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="3a86f-140">Visual Studio 2010</span></span>

- <span data-ttu-id="3a86f-141">이 버전의 Visual Studio를 사용 하 여 Entity Framework 도구를 사용할 수 있는 버전이 Entity Framework 6 런타임과 호환 되지 않으며 업그레이드할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-141">The version of Entity Framework Tools available with this version of Visual Studio is not compatible with the Entity Framework 6 runtime and cannot be upgraded.</span></span>
- <span data-ttu-id="3a86f-142">기본적으로 Entity Framework 도구는 프로젝트에 Entity Framework 4.0을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-142">By default, the Entity Framework tools will add Entity Framework 4.0 to your projects.</span></span>
<span data-ttu-id="3a86f-143">모든 최신 버전의 EF 사용 하 여 응용 프로그램을 만들기 위해를 먼저 설치 해야 합니다 [NuGet 패키지 관리자 확장](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager)합니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-143">In order to create applications using any newer versions of EF, you will first need to install the [NuGet Package Manager extension](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager).</span></span>
- <span data-ttu-id="3a86f-144">기본적으로 EF 도구 버전에서 모든 코드 생성은 EntityObject 및 Entity Framework 4 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-144">By default, all code generation in the version of EF tools is based on EntityObject and Entity Framework 4.</span></span>
<span data-ttu-id="3a86f-145">코드 생성에 대 한 DbContext 코드 생성 템플릿을 설치 하 여 DbContext 및 Entity Framework 5를 기반으로 전환 하는 것이 좋습니다 [C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) 하거나 [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET)합니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-145">We recommend that you switch the code generation to be based on DbContext and Entity Framework 5, by installing the DbContext code generation templates for [C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) or [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET).</span></span>
- <span data-ttu-id="3a86f-146">NuGet 패키지 관리자 확장을 설치한 후 수동으로 설치 또는 온라인에서 사용할 수 있는 모든 EF NuGet 패키지를 업그레이드할를 EF6을 사용 하 여 Code First로, 디자이너를 사용할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-146">Once you have installed the NuGet Package Manager extensions, you can manually install or upgrade to any EF NuGet package available online and use EF6 with Code First, which does not require a designer.</span></span>
- <span data-ttu-id="3a86f-147">기본적으로이 버전의 Visual Studio를 사용 하 여 사용 가능한 SQL Server 인스턴스를 SQLEXPRESS 라는 SQL Server Express입니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-147">By default, the SQL Server instance available with this version of Visual Studio is SQL Server Express named SQLEXPRESS.</span></span>
<span data-ttu-id="3a86f-148">사용 해야 하는 연결 문자열의 서버 섹션은 ". \\SQLEXPRESS "입니다.</span><span class="sxs-lookup"><span data-stu-id="3a86f-148">The server section of connection string you should use is ".\\SQLEXPRESS".</span></span>
<span data-ttu-id="3a86f-149">접두사로 축 자 문자열을 사용 해야 `@` 백슬래시를 두 번 또는 "\\\\" C# 코드에서 연결 문자열을 지정 하는 경우.</span><span class="sxs-lookup"><span data-stu-id="3a86f-149">Remember to use a verbatim string prefixed with `@` or double back-slashes "\\\\" when specifying a connection string in C# code.</span></span>
