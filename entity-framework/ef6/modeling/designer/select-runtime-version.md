---
title: EF6에서 EF 디자이너 모델에 대 한 엔터티 프레임 워크 런타임 버전 선택
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488493"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="2ddb6-102">EF 디자이너 모델에 대 한 엔터티 프레임 워크 런타임 버전 선택</span><span class="sxs-lookup"><span data-stu-id="2ddb6-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="2ddb6-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="2ddb6-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="2ddb6-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2ddb6-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="2ddb6-105">EF6부터 다음 화면 모델을 만들 때 대상으로 하려는 런타임 버전을 선택할 수 있도록 EF 디자이너에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="2ddb6-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="2ddb6-106">최신 버전의 Entity Framework 프로젝트에 이미 설치 되어 있지 않으면 화면 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2ddb6-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="2ddb6-107">최신 버전이 이미 설치 되어 있는 경우 기본적으로만 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2ddb6-107">If the latest version is already installed it will just be used by default.</span></span>

![화면](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="2ddb6-109">대상 EF6.x</span><span class="sxs-lookup"><span data-stu-id="2ddb6-109">Targeting EF6.x</span></span>

<span data-ttu-id="2ddb6-110">EF6 EF6 런타임 프로젝트에 추가할 사용자 버전 선택 ' 화면에서 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2ddb6-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="2ddb6-111">EF6를 추가한 후 현재 프로젝트에이 화면이 표시 되 중지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2ddb6-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="2ddb6-112">(때문에 여러 버전의 동일한 프로젝트에서 런타임 대상 할 수 없습니다)를 설치 하는 EF의 이전 버전을 이미 있는 경우 EF6은 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="2ddb6-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="2ddb6-113">EF6 옵션 여기을 사용 하지 않는 경우 EF6에 프로젝트를 업그레이드 하려면 다음이 단계를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ddb6-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="2ddb6-114">솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **NuGet 패키지 관리...**</span><span class="sxs-lookup"><span data-stu-id="2ddb6-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="2ddb6-115">선택 **업데이트**</span><span class="sxs-lookup"><span data-stu-id="2ddb6-115">Select **Updates**</span></span>
3.  <span data-ttu-id="2ddb6-116">선택 **EntityFramework** (원하는 버전으로 업데이트 하려는 있는지 확인)</span><span class="sxs-lookup"><span data-stu-id="2ddb6-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="2ddb6-117">클릭 **업데이트**</span><span class="sxs-lookup"><span data-stu-id="2ddb6-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="2ddb6-118">대상 EF5.x</span><span class="sxs-lookup"><span data-stu-id="2ddb6-118">Targeting EF5.x</span></span>

<span data-ttu-id="2ddb6-119">EF5 EF5 런타임 프로젝트에 추가할 사용자 버전 선택 ' 화면에서 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2ddb6-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="2ddb6-120">EF5를 추가한 후 사용 하지 않도록 설정 하는 EF6 옵션을 사용 하 여 화면을 계속 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2ddb6-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="2ddb6-121">이미 설치 된 런타임의 EF4.x 버전이 있는 경우 표시 됩니다 해당 버전의 EF EF5 보다는 화면에 나열 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ddb6-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="2ddb6-122">다음 단계를 사용 하 여이 이런 EF5로 업그레이드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2ddb6-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="2ddb6-123">선택 **도구-&gt; 라이브러리 패키지 관리자-&gt; 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="2ddb6-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="2ddb6-124">실행 **Install-package EntityFramework-버전 5.0.0**</span><span class="sxs-lookup"><span data-stu-id="2ddb6-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="2ddb6-125">대상 EF4.x</span><span class="sxs-lookup"><span data-stu-id="2ddb6-125">Targeting EF4.x</span></span>

<span data-ttu-id="2ddb6-126">다음 단계를 사용 하 여 프로젝트에 EF4.x runtime을 설치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2ddb6-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="2ddb6-127">선택 **도구-&gt; 라이브러리 패키지 관리자-&gt; 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="2ddb6-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="2ddb6-128">실행 **Install-package EntityFramework-버전 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="2ddb6-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
