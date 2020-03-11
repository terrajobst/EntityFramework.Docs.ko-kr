---
title: EF Designer 모델에 대 한 Entity Framework 런타임 버전 선택-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414994"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="52786-102">EF 디자이너 모델에 대 한 Entity Framework 런타임 버전 선택</span><span class="sxs-lookup"><span data-stu-id="52786-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="52786-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="52786-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="52786-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="52786-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="52786-105">EF6부터 다음 화면이 EF 디자이너에 추가 되어 모델을 만들 때 대상으로 지정할 런타임의 버전을 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="52786-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="52786-106">최신 버전의 Entity Framework 프로젝트에 아직 설치 되지 않은 경우 화면이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="52786-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="52786-107">최신 버전이 이미 설치 되어 있는 경우에는 기본적으로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="52786-107">If the latest version is already installed it will just be used by default.</span></span>

![화면](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="52786-109">EF6를 대상으로 지정</span><span class="sxs-lookup"><span data-stu-id="52786-109">Targeting EF6.x</span></span>

<span data-ttu-id="52786-110">' 버전 선택 ' 화면에서 EF6를 선택 하 여 프로젝트에 EF6 런타임을 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="52786-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="52786-111">EF6를 추가 하면 현재 프로젝트에서이 화면이 표시 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="52786-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="52786-112">이전 버전의 EF가 이미 설치 되어 있는 경우에는 EF6가 사용 하지 않도록 설정 됩니다. 동일한 프로젝트에서 여러 버전의 런타임을 대상으로 지정할 수 없기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="52786-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="52786-113">EF6 옵션을 사용 하도록 설정 하지 않은 경우 다음 단계에 따라 프로젝트를 EF6로 업그레이드 합니다.</span><span class="sxs-lookup"><span data-stu-id="52786-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="52786-114">솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리 ...** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="52786-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="52786-115">**업데이트** 선택</span><span class="sxs-lookup"><span data-stu-id="52786-115">Select **Updates**</span></span>
3.  <span data-ttu-id="52786-116">**Entityframework** (원하는 버전으로 업데이트 해야 하는지 확인)를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="52786-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="52786-117">**업데이트**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="52786-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="52786-118">EF5를 대상으로 지정</span><span class="sxs-lookup"><span data-stu-id="52786-118">Targeting EF5.x</span></span>

<span data-ttu-id="52786-119">' 버전 선택 ' 화면에서 EF5를 선택 하 여 프로젝트에 EF5 런타임을 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="52786-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="52786-120">EF5를 추가 하면 EF6 옵션이 비활성화 된 상태로 화면이 계속 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="52786-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="52786-121">EF4 버전이 이미 설치 되어 있는 경우에는 EF5 대신 EF 버전이 화면에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="52786-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="52786-122">이 경우 다음 단계를 사용 하 여 EF5로 업그레이드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="52786-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="52786-123">**도구-&gt; 라이브러리 패키지 관리자-&gt; 패키지 관리자 콘솔을** 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="52786-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="52786-124">**설치-패키지 EntityFramework-버전 5.0.0을** 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="52786-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="52786-125">EF4를 대상으로 지정</span><span class="sxs-lookup"><span data-stu-id="52786-125">Targeting EF4.x</span></span>

<span data-ttu-id="52786-126">다음 단계를 사용 하 여 EF4 런타임을 프로젝트에 설치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="52786-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="52786-127">**도구-&gt; 라이브러리 패키지 관리자-&gt; 패키지 관리자 콘솔을** 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="52786-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="52786-128">**설치-패키지 EntityFramework-버전 4.3.0을** 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="52786-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
