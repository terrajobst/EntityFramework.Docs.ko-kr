---
title: Entity Framework 가져오기-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 2bdec6a9be228fbe934d0f46aa1bfafdfb2c971c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416098"
---
# <a name="get-entity-framework"></a><span data-ttu-id="436e9-102">Entity Framework 가져오기</span><span class="sxs-lookup"><span data-stu-id="436e9-102">Get Entity Framework</span></span>
<span data-ttu-id="436e9-103">Entity Framework는 Visual Studio 및 EF Runtime 용 EF 도구로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-103">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="436e9-104">Visual Studio 용 EF 도구</span><span class="sxs-lookup"><span data-stu-id="436e9-104">EF Tools for Visual Studio</span></span>

<span data-ttu-id="436e9-105">Visual Studio의 Entity Framework Tools에는 EF Designer와 EF Model 마법사가 포함 되어 있으며, 데이터베이스 first 및 Model first 워크플로에 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-105">The Entity Framework Tools for Visual Studio include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="436e9-106">EF Tools는 모든 최신 버전의 Visual Studio에 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-106">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="436e9-107">Visual Studio의 사용자 지정 설치를 수행 하는 경우 해당 항목을 포함 하는 작업을 선택 하거나 개별 구성 요소로 선택 하 여 "Entity Framework 6 도구" 항목을 선택 했는지 확인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-107">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="436e9-108">일부 이전 버전의 Visual Studio에서는 업데이트 된 EF Tools를 다운로드로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-108">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="436e9-109">Visual Studio 버전에서 사용 가능한 최신 버전의 EF Tools를 가져오는 방법에 대 한 지침은 [Visual Studio 버전](~/ef6/what-is-new/visual-studio.md) 을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="436e9-109">See [Visual Studio Versions](~/ef6/what-is-new/visual-studio.md) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="436e9-110">EF 런타임</span><span class="sxs-lookup"><span data-stu-id="436e9-110">EF Runtime</span></span>

<span data-ttu-id="436e9-111">최신 버전의 Entity Framework는 [Entityframework NuGet 패키지로](https://nuget.org/packages/EntityFramework/)사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-111">The latest version of Entity Framework is available as the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="436e9-112">NuGet 패키지 관리자에 대해 잘 모르는 경우에는 [Nuget 개요](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow)를 참조 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-112">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="436e9-113">EF NuGet 패키지 설치</span><span class="sxs-lookup"><span data-stu-id="436e9-113">Installing the EF NuGet Package</span></span>

<span data-ttu-id="436e9-114">프로젝트의 **참조** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리 ...** 를 선택 하 여 entityframework 패키지를 설치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-114">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![NuGet 패키지 관리](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="436e9-116">패키지 관리자 콘솔에서 설치</span><span class="sxs-lookup"><span data-stu-id="436e9-116">Installing from Package Manager Console</span></span>

<span data-ttu-id="436e9-117">또는 [패키지 관리자 콘솔](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)에서 다음 명령을 실행 하 여 entityframework를 설치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-117">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="436e9-118">EF의 특정 버전 설치</span><span class="sxs-lookup"><span data-stu-id="436e9-118">Installing a specific version of EF</span></span>

<span data-ttu-id="436e9-119">EF 4.1부터 새 버전의 EF 런타임이 [Entityframework NuGet 패키지로](https://www.nuget.org/packages/EntityFramework/)릴리스 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-119">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="436e9-120">Visual Studio의 [패키지 관리자 콘솔](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)에서 다음 명령을 실행 하 여 .NET Framework 기반 프로젝트에 이러한 버전을 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-120">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="436e9-121">`<number>`은 설치할 EF의 특정 버전을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-121">Note that `<number>` represents the specific version of EF to install.</span></span> <span data-ttu-id="436e9-122">예를 들어 6.2.0는 EF 6.2에 대 한 숫자의 버전입니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-122">For example, 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="436e9-123">4\.1 이전의 EF 런타임은 .NET Framework의 일부 이며 별도로 설치할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-123">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="436e9-124">최신 미리 보기 설치</span><span class="sxs-lookup"><span data-stu-id="436e9-124">Installing the Latest Preview</span></span>

<span data-ttu-id="436e9-125">위의 메서드는 Entity Framework의 완전히 지원 되는 최신 릴리스를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-125">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="436e9-126">사용자 의견을 보내 주시면 감사를 제공 하는 데 사용할 수 있는 Entity Framework 시험판 버전이 종종 있습니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-126">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="436e9-127">EntityFramework의 최신 미리 보기를 설치 하려면 NuGet 패키지 관리 창에서 **시험판 포함** 을 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-127">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="436e9-128">시험판 버전을 사용할 수 없는 경우 Entity Framework의 완전히 지원 되는 최신 버전을 자동으로 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-128">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![시험판 포함](~/ef6/media/includeprerelease.png)

<span data-ttu-id="436e9-130">또는 [패키지 관리자 콘솔](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)에서 다음 명령을 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="436e9-130">Alternatively, you can run the following command in the [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```
