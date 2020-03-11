---
title: 모델 당 여러 다이어그램-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b95db5c8-de8d-43bd-9ccc-5df6a5e25e1b
ms.openlocfilehash: e78b927a147143629ac5b73e23edf439ba6d0264
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415120"
---
# <a name="multiple-diagrams-per-model"></a><span data-ttu-id="29318-102">모델 당 여러 다이어그램</span><span class="sxs-lookup"><span data-stu-id="29318-102">Multiple Diagrams per Model</span></span>
> [!NOTE]
> <span data-ttu-id="29318-103">**EF5** 이상-이 페이지에서 설명 하는 기능, api 등은 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="29318-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="29318-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="29318-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="29318-105">이 비디오 및 페이지에서는 Entity Framework Designer (EF Designer)를 사용 하 여 모델을 여러 다이어그램으로 분할 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="29318-105">This video and page shows how to split a model into multiple diagrams using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="29318-106">모델이 너무 커서 보거나 편집할 수 없게 되는 경우이 기능을 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="29318-106">You might want to use this feature when your model becomes too large to view or edit.</span></span>

<span data-ttu-id="29318-107">이전 버전의 EF 디자이너에서는 EDMX 파일 마다 하나의 다이어그램만 있을 수 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="29318-107">In earlier versions of the EF Designer you could only have one diagram per the EDMX file.</span></span> <span data-ttu-id="29318-108">Visual Studio 2012부터 EF Designer를 사용 하 여 EDMX 파일을 여러 다이어그램으로 분할할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="29318-108">Starting with Visual Studio 2012, you can use the EF Designer to split your EDMX file into multiple diagrams.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="29318-109">비디오 보기</span><span class="sxs-lookup"><span data-stu-id="29318-109">Watch the video</span></span>
<span data-ttu-id="29318-110">이 비디오에서는 Entity Framework Designer (EF Designer)를 사용 하 여 모델을 여러 다이어그램으로 분할 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="29318-110">This video shows how to split a model into multiple diagrams using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="29318-111">모델이 너무 커서 보거나 편집할 수 없게 되는 경우이 기능을 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="29318-111">You might want to use this feature when your model becomes too large to view or edit.</span></span>

<span data-ttu-id="29318-112">제공한 **사람**: 줄리아 Kornich</span><span class="sxs-lookup"><span data-stu-id="29318-112">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="29318-113">**비디오**: [wmv](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv) | [MP4](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v) | [wmv (ZIP)](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)</span><span class="sxs-lookup"><span data-stu-id="29318-113">**Video**: [WMV](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv) | [MP4](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)</span></span>

## <a name="ef-designer-overview"></a><span data-ttu-id="29318-114">EF 디자이너 개요</span><span class="sxs-lookup"><span data-stu-id="29318-114">EF Designer Overview</span></span>

<span data-ttu-id="29318-115">EF 디자이너의 엔터티 데이터 모델 마법사를 사용 하 여 모델을 만들 때 .edmx 파일이 생성 되어 솔루션에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="29318-115">When you create a model using the EF Designer’s Entity Data Model Wizard, an .edmx file is created and added to your solution.</span></span> <span data-ttu-id="29318-116">이 파일은 엔터티의 모양과 데이터베이스에 매핑되는 방법을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="29318-116">This file defines the shape of your entities and how they map to the database.</span></span>

<span data-ttu-id="29318-117">EF Designer는 다음과 같은 구성 요소로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="29318-117">The EF Designer consists of the following components:</span></span>

-   <span data-ttu-id="29318-118">모델을 편집 하기 위한 시각적 디자인 화면입니다.</span><span class="sxs-lookup"><span data-stu-id="29318-118">A visual design surface for editing the model.</span></span> <span data-ttu-id="29318-119">엔터티 및 연결을 만들고, 수정하고, 삭제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="29318-119">You can create, modify, or delete entities and associations.</span></span>
-   <span data-ttu-id="29318-120">모델의 트리 뷰를 제공 하는 **모델 브라우저** 창입니다.</span><span class="sxs-lookup"><span data-stu-id="29318-120">A **Model Browser** window that provides tree views of the model.</span></span><span data-ttu-id="29318-121">  엔터티와 해당 연결은 *\[ModelName\]* 폴더 아래에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="29318-121">  The entities and their associations are located under the *\[ModelName\]* folder.</span></span> <span data-ttu-id="29318-122">데이터베이스 테이블과 제약 조건은 *\[ModelName\]* 에 있습니다. 저장소 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="29318-122">The database tables and constraints are located under the *\[ModelName\]*.Store folder.</span></span>
-   <span data-ttu-id="29318-123">매핑 보기 및 편집을 위한 **매핑 정보** 창입니다.</span><span class="sxs-lookup"><span data-stu-id="29318-123">A **Mapping Details** window for viewing and editing mappings.</span></span> <span data-ttu-id="29318-124">엔터티 형식 또는 연결을 데이터베이스 테이블, 열, 저장 프로시저 등에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="29318-124">You can map entity types or associations to database tables, columns, and stored procedures.</span></span> 

<span data-ttu-id="29318-125">엔터티 데이터 모델 마법사가 완료 되 면 비주얼 디자인 화면 창이 자동으로 열립니다.</span><span class="sxs-lookup"><span data-stu-id="29318-125">The visual design surface window is automatically opened when the Entity Data Model Wizard finishes.</span></span> <span data-ttu-id="29318-126">모델 브라우저가 표시 되지 않으면 주 디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 **모델 브라우저**를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="29318-126">If the Model Browser is not visible, right-click the main design surface and select **Model Browser**.</span></span>

<span data-ttu-id="29318-127">다음 스크린샷은 EF 디자이너에서 열린 .edmx 파일을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="29318-127">The following screenshot shows an .edmx file opened in the EF Designer.</span></span> <span data-ttu-id="29318-128">스크린 샷에서는 시각적 디자인 화면 (왼쪽) 및 **모델 브라우저** 창 (오른쪽)을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="29318-128">The screenshot shows the visual design surface (to the left) and the **Model Browser** window (to the right).</span></span>

![EF 디자이너 2](~/ef6/media/efdesigner2.png)

<span data-ttu-id="29318-130">EF Designer에서 수행한 작업을 실행 취소 하려면 Ctrl + Z를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="29318-130">To undo an operation done in the EF Designer, click Ctrl-Z.</span></span>

## <a name="working-with-diagrams"></a><span data-ttu-id="29318-131">다이어그램 작업</span><span class="sxs-lookup"><span data-stu-id="29318-131">Working with Diagrams</span></span>

<span data-ttu-id="29318-132">기본적으로 EF Designer는 Diagram1 라는 하나의 다이어그램을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="29318-132">By default the EF Designer creates one diagram called Diagram1.</span></span> <span data-ttu-id="29318-133">엔터티 및 연결 수가 많은 다이어그램이 있는 경우 논리적으로 분할 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="29318-133">If you have a diagram with a large number of entities and associations, you will most like want to split them up logically.</span></span> <span data-ttu-id="29318-134">Visual Studio 2012부터 여러 다이어그램에서 개념적 모델을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="29318-134">Starting with Visual Studio 2012, you can view your conceptual model in multiple diagrams.</span></span>   

<span data-ttu-id="29318-135">새 다이어그램을 추가 하면 모델 브라우저 창의 다이어그램 폴더 아래에 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="29318-135">As you add new diagrams, they appear under the Diagrams folder in the Model Browser window.</span></span> <span data-ttu-id="29318-136">다이어그램의 이름을 바꾸려면 모델 브라우저 창에서 다이어그램을 선택 하 고 이름에서 한 번을 클릭 한 다음 새 이름을 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="29318-136">To rename a diagram: select the diagram in the Model Browser window, click once on the name, and type the new name.</span></span> <span data-ttu-id="29318-137"> 다이어그램 이름을 마우스 오른쪽 단추로 클릭 하 고 **이름 바꾸기**를 선택할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="29318-137"> You can also right-click the diagram name and select **Rename**.</span></span>

<span data-ttu-id="29318-138">Visual Studio 편집기에서 .edmx 파일 이름 옆에 다이어그램 이름이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="29318-138">The diagram name is displayed next to the .edmx file name, in the Visual Studio editor.</span></span> <span data-ttu-id="29318-139">예를 들어 Model1\[Diagram1\]입니다.</span><span class="sxs-lookup"><span data-stu-id="29318-139">For example Model1.edmx\[Diagram1\].</span></span>

![DiagramName](~/ef6/media/diagramname.png)

<span data-ttu-id="29318-141">다이어그램 콘텐츠 (엔터티 및 연결의 모양 및 색)는 .edmx. 다이어그램 파일에 저장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="29318-141">The diagrams content (shape and color of entities and associations) is stored in the .edmx.diagram file.</span></span> <span data-ttu-id="29318-142">이 파일을 보려면 솔루션 탐색기를 선택 하 고 .edmx 파일을 펼침.</span><span class="sxs-lookup"><span data-stu-id="29318-142">To view this file, select Solution Explorer and unfold the .edmx file.</span></span> 

![DiagramFiles](~/ef6/media/diagramfiles.png)

<span data-ttu-id="29318-144">.Edmx 디자이너에서이 파일의 콘텐츠를 덮어쓸 수 있으므로 직접 편집 하면 안 됩니다.</span><span class="sxs-lookup"><span data-stu-id="29318-144">You should not edit the .edmx.diagram file manually, the content of this file maybe overwritten by the EF Designer.</span></span>
 
## <a name="splitting-entities-and-associations-into-a-new-diagram"></a><span data-ttu-id="29318-145">엔터티 및 연결을 새 다이어그램으로 분할</span><span class="sxs-lookup"><span data-stu-id="29318-145">Splitting Entities and Associations into a New Diagram</span></span>

<span data-ttu-id="29318-146">기존 다이어그램에서 엔터티를 선택할 수 있습니다 (Shift 키를 누른 채 여러 엔터티 선택).</span><span class="sxs-lookup"><span data-stu-id="29318-146">You can select entities on the existing diagram (hold Shift to select multiple entities).</span></span> <span data-ttu-id="29318-147">마우스 오른쪽 단추를 클릭 하 고 **새 다이어그램으로 이동을**선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="29318-147">Click the right mouse button and select **Move to new Diagram**.</span></span> <span data-ttu-id="29318-148">새 다이어그램이 만들어지고 선택한 엔터티와 해당 연결이 다이어그램으로 이동 됩니다.</span><span class="sxs-lookup"><span data-stu-id="29318-148">The new diagram is created and the selected entities and their associations are moved to the diagram.</span></span>

<span data-ttu-id="29318-149">또는 모델 브라우저에서 다이어그램 폴더를 마우스 오른쪽 단추로 클릭 하 고 **새 다이어그램 추가** 를 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="29318-149">Alternatively, you can right-click the Diagrams folder in Model Browser and select **Add new Diagram.**</span></span> <span data-ttu-id="29318-150">그런 다음 모델 브라우저의 엔터티 형식 폴더 아래에 있는 엔터티를 디자인 화면으로 끌어다 놓을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="29318-150">You can then drag and drop entities from under the Entity Types folder in Model Browser onto the design surface.</span></span>

<span data-ttu-id="29318-151">또한 한 다이어그램에서 엔터티 (Ctrl + X 또는 Ctrl + C 키 사용)를 잘라내거나 복사 하 여 다른 다이어그램에 붙여 넣을 수 있습니다 (Ctrl + V 키 사용).</span><span class="sxs-lookup"><span data-stu-id="29318-151">You can also cut or copy entities (using Ctrl-X or Ctrl-C keys) from one diagram and paste (using Ctrl-V key) on the other.</span></span> <span data-ttu-id="29318-152">엔터티에 붙여 넣는 다이어그램에 동일한 이름의 엔터티가 이미 포함 되어 있으면 새 엔터티가 생성 되어 모델에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="29318-152">If the diagram into which you are pasting an entity already contains an entity with the same name, a new entity will be created and added to the model.</span></span><span data-ttu-id="29318-153">  예: Diagram2는 부서 엔터티를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="29318-153">  For example: Diagram2 contains the Department entity.</span></span> <span data-ttu-id="29318-154">그런 다음 Diagram2에 다른 부서를 붙여 넣습니다.</span><span class="sxs-lookup"><span data-stu-id="29318-154">Then, you paste another Department on Diagram2.</span></span> <span data-ttu-id="29318-155">Department1 엔터티가 만들어지고 개념적 모델에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="29318-155">The Department1 entity is created and added to the conceptual model.</span></span>   

<span data-ttu-id="29318-156">다이어그램에 관련 엔터티를 포함 하려면 엔터티를 rick 클릭 하 고 관련 항목 **포함**을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="29318-156">To include related entities in a diagram, rick-click the entity and select **Include Related**.</span></span> <span data-ttu-id="29318-157">그러면 지정 된 다이어그램에서 관련 엔터티 및 연결의 복사본이 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="29318-157">This will make a copy of the related entities and associations in the specified diagram.</span></span>

## <a name="changing-the-color-of-entities"></a><span data-ttu-id="29318-158">엔터티 색 변경</span><span class="sxs-lookup"><span data-stu-id="29318-158">Changing the Color of Entities</span></span>

<span data-ttu-id="29318-159">모델을 여러 다이어그램으로 분할 하는 것 외에도 엔터티의 색을 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="29318-159">In addition to splitting a model into multiple diagrams, you can also change colors of your entities.</span></span>

<span data-ttu-id="29318-160">색을 변경 하려면 디자인 화면에서 엔터티 (또는 여러 엔터티)를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="29318-160">To change the color, select an entity (or multiple entities) on the design surface.</span></span> <span data-ttu-id="29318-161">그런 다음 마우스 오른쪽 단추를 클릭 하 고 **속성**을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="29318-161">Then, click the right mouse button and select **Properties**.</span></span> <span data-ttu-id="29318-162">속성 창에서 **채우기 색** 속성을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="29318-162">In the Properties window, select the **Fill Color** property.</span></span> <span data-ttu-id="29318-163">올바른 색 이름 (예: Red) 또는 유효한 RGB (예: 255, 128, 128)를 사용 하 여 색을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="29318-163">Specify the color using either a valid color name (for example, Red) or a valid RGB (for example, 255, 128, 128).</span></span> 

![색](~/ef6/media/color.png)

## <a name="summary"></a><span data-ttu-id="29318-165">요약</span><span class="sxs-lookup"><span data-stu-id="29318-165">Summary</span></span>

<span data-ttu-id="29318-166">이 항목에서는 모델을 여러 다이어그램으로 분할 하는 방법 및 Entity Framework Designer를 사용 하 여 엔터티에 대해 다른 색을 지정 하는 방법을 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="29318-166">In this topic we looked at how to split a model into multiple diagrams and also how to specify a different color for an entity using the Entity Framework Designer.</span></span> 
