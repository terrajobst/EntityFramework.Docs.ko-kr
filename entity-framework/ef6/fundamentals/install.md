---
title: Entity Framework-EF6 가져오기
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 7f840a4f9e437ec12f699184339e386976e1528b
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490658"
---
# <a name="get-entity-framework"></a><span data-ttu-id="49210-102">Entity Framework 가져오기</span><span class="sxs-lookup"><span data-stu-id="49210-102">Get Entity Framework</span></span>
<span data-ttu-id="49210-103">Entity Framework는 Visual Studio 및 EF 런타임에 대 한 EF 도구 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="49210-103">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="49210-104">Visual Studio 용 EF 도구</span><span class="sxs-lookup"><span data-stu-id="49210-104">EF Tools for Visual Studio</span></span>

<span data-ttu-id="49210-105">Entity Framework Tools for Visual Studio는 먼저 데이터베이스에 필요한 고 첫 번째 워크플로 모델 EF 디자이너 및 EF 모델 마법사를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="49210-105">The Entity Framework Tools for Visual Studio include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="49210-106">EF 도구는 모든 최신 버전의 Visual Studio에 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="49210-106">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="49210-107">항목이 있는지 확인 해야 하는 Visual Studio의 사용자 지정 설치를 수행 하는 경우 "Entity Framework 6 도구" 하거나 포함 하는 작업을 선택 하 여 선택 하거나 개별 구성 요소로 선택 됩니다.</span><span class="sxs-lookup"><span data-stu-id="49210-107">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="49210-108">일부 이전 버전의 Visual Studio에 대 한 업데이트 된 EF 도구는 다운로드로 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="49210-108">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="49210-109">참조 [Visual Studio 버전](~/ef6/what-is-new/visual-studio.md) Visual Studio의 버전에 대 한 EF 도구를 사용할 수 있는 최신 버전을 가져오는 방법에 대 한 지침에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="49210-109">See [Visual Studio Versions](~/ef6/what-is-new/visual-studio.md) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="49210-110">EF 런타임</span><span class="sxs-lookup"><span data-stu-id="49210-110">EF Runtime</span></span>

<span data-ttu-id="49210-111">Entity Framework의 최신 버전은 제공 된 [EntityFramework NuGet 패키지](http://nuget.org/packages/EntityFramework/)합니다.</span><span class="sxs-lookup"><span data-stu-id="49210-111">The latest version of Entity Framework is available as the [EntityFramework NuGet package](http://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="49210-112">NuGet 패키지 관리자를 사용 하 여 잘 모르는 경우 의견을 교환하실 수 읽기를 [NuGet 개요](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow)합니다.</span><span class="sxs-lookup"><span data-stu-id="49210-112">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="49210-113">EF NuGet 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="49210-113">Installing the EF NuGet Package</span></span>

<span data-ttu-id="49210-114">마우스 오른쪽 단추로 클릭 하 여 EntityFramework 패키지를 설치할 수 있습니다 합니다 **참조가** 프로젝트의 폴더를 선택 하 고 **NuGet 패키지 관리...**</span><span class="sxs-lookup"><span data-stu-id="49210-114">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![NuGet 패키지 관리](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="49210-116">패키지 관리자 콘솔에서 설치</span><span class="sxs-lookup"><span data-stu-id="49210-116">Installing from Package Manager Console</span></span>

<span data-ttu-id="49210-117">EntityFramework에서 다음 명령을 실행 하 여 설치할 수 있습니다 또는 합니다 [패키지 관리자 콘솔](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)합니다.</span><span class="sxs-lookup"><span data-stu-id="49210-117">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="49210-118">EF의 특정 버전 설치</span><span class="sxs-lookup"><span data-stu-id="49210-118">Installing a specific version of EF</span></span>

<span data-ttu-id="49210-119">EF 4.1 이상에서 새 버전의 EF 런타임이 보기로 출시 되었으며 합니다 [EntityFramework NuGet 패키지](https://www.nuget.org/packages/EntityFramework/)합니다.</span><span class="sxs-lookup"><span data-stu-id="49210-119">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="49210-120">Visual Studio에서 다음 명령을 실행 하 여.NET Framework 기반 프로젝트에 추가할 수 있습니다 이러한 버전 중 하나 [패키지 관리자 콘솔](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span><span class="sxs-lookup"><span data-stu-id="49210-120">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="49210-121">`<number>` 를 설치 하는 EF의 특정 버전을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="49210-121">Note that `<number>` represents the specific version of EF to install.</span></span> <span data-ttu-id="49210-122">예를 들어 6.2.0 EF 6.2에 대 한 수의 버전입니다.</span><span class="sxs-lookup"><span data-stu-id="49210-122">For example, 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="49210-123">EF 런타임 전에 4.1.NET Framework의 일부인 것을 별도로 설치할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="49210-123">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="49210-124">최신 미리 보기 설치</span><span class="sxs-lookup"><span data-stu-id="49210-124">Installing the Latest Preview</span></span>

<span data-ttu-id="49210-125">위의 방법이 표시 됩니다 최신 릴리스의 Entity Framework를 완벽 하 게 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="49210-125">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="49210-126">시험판 버전의 Entity Framework는 환영 사용해에 대 한 의견을 사용할 수 있는 많습니다.</span><span class="sxs-lookup"><span data-stu-id="49210-126">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="49210-127">EntityFramework 선택할 수 있습니다의 최신 미리 보기를 설치 하려면 **시험판 포함** NuGet 패키지 관리 창에서.</span><span class="sxs-lookup"><span data-stu-id="49210-127">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="49210-128">자동으로 최신 시험판 버전을 사용할 수 있는 경우 받습니다 완벽 하 게 지원 되는 Entity Framework의 버전입니다.</span><span class="sxs-lookup"><span data-stu-id="49210-128">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![시험판 포함](~/ef6/media/includeprerelease.png)

<span data-ttu-id="49210-130">다음 명령을 실행할 수는 또는 [패키지 관리자 콘솔](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)합니다.</span><span class="sxs-lookup"><span data-stu-id="49210-130">Alternatively, you can run the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```
