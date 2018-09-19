---
title: 모델-EF6 당 여러 다이어그램
author: divega
ms.date: 10/23/2016
ms.assetid: b95db5c8-de8d-43bd-9ccc-5df6a5e25e1b
ms.openlocfilehash: e78b927a147143629ac5b73e23edf439ba6d0264
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283617"
---
# <a name="multiple-diagrams-per-model"></a><span data-ttu-id="559c2-102">모델에 대해 여러 다이어그램</span><span class="sxs-lookup"><span data-stu-id="559c2-102">Multiple Diagrams per Model</span></span>
> [!NOTE]
> <span data-ttu-id="559c2-103">**EF5 이상만** -Api 기능 등이이 페이지에 설명 된 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="559c2-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="559c2-105">이 비디오 및 페이지에는 Entity Framework Designer (EF 디자이너)를 사용 하 여 여러 다이어그램에 모델을 분할 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-105">This video and page shows how to split a model into multiple diagrams using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="559c2-106">모델 보기 또는 편집에 너무 커지면 하는 경우이 기능을 사용 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-106">You might want to use this feature when your model becomes too large to view or edit.</span></span>

<span data-ttu-id="559c2-107">이전 버전의 EF 디자이너에서 EDMX 파일 당 한 다이어그램만 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-107">In earlier versions of the EF Designer you could only have one diagram per the EDMX file.</span></span> <span data-ttu-id="559c2-108">Visual Studio 2012부터 여러 다이어그램에 EDMX 파일을 분할 하는 EF 디자이너를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-108">Starting with Visual Studio 2012, you can use the EF Designer to split your EDMX file into multiple diagrams.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="559c2-109">비디오를 시청 하세요.</span><span class="sxs-lookup"><span data-stu-id="559c2-109">Watch the video</span></span>
<span data-ttu-id="559c2-110">이 비디오에는 Entity Framework Designer (EF 디자이너)를 사용 하 여 여러 다이어그램에 모델을 분할 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-110">This video shows how to split a model into multiple diagrams using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="559c2-111">모델 보기 또는 편집에 너무 커지면 하는 경우이 기능을 사용 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-111">You might want to use this feature when your model becomes too large to view or edit.</span></span>

<span data-ttu-id="559c2-112">**제공한**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="559c2-112">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="559c2-113">**비디오**: [WMV](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv) | [MP4](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)</span><span class="sxs-lookup"><span data-stu-id="559c2-113">**Video**: [WMV](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv) | [MP4](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)</span></span>

## <a name="ef-designer-overview"></a><span data-ttu-id="559c2-114">EF 디자이너 개요</span><span class="sxs-lookup"><span data-stu-id="559c2-114">EF Designer Overview</span></span>

<span data-ttu-id="559c2-115">EF 디자이너의 엔터티 데이터 모델 마법사를 사용 하 여 모델을 만들 때.edmx 파일 생성 되어 솔루션에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-115">When you create a model using the EF Designer’s Entity Data Model Wizard, an .edmx file is created and added to your solution.</span></span> <span data-ttu-id="559c2-116">이 파일에는 엔터티 및 데이터베이스에 매핑하는 방법의 모양을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-116">This file defines the shape of your entities and how they map to the database.</span></span>

<span data-ttu-id="559c2-117">EF 디자이너 다음 구성 요소로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-117">The EF Designer consists of the following components:</span></span>

-   <span data-ttu-id="559c2-118">모델 편집을 위한 시각적 디자인 화면입니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-118">A visual design surface for editing the model.</span></span> <span data-ttu-id="559c2-119">엔터티 및 연결을 만들고, 수정하고, 삭제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-119">You can create, modify, or delete entities and associations.</span></span>
-   <span data-ttu-id="559c2-120">A **Model 브라우저** 모델의 트리 보기를 제공 하는 창입니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-120">A **Model Browser** window that provides tree views of the model.</span></span>  <span data-ttu-id="559c2-121">엔터티 및 해당 연결 아래에 *\[ModelName\]* 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-121">The entities and their associations are located under the *\[ModelName\]* folder.</span></span> <span data-ttu-id="559c2-122">데이터베이스 테이블 및 제약 조건 아래에  *\[ModelName\]* 합니다. 폴더에 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-122">The database tables and constraints are located under the *\[ModelName\]*.Store folder.</span></span>
-   <span data-ttu-id="559c2-123">A **매핑 정보** 매핑 보기 및 편집에 대 한 창.</span><span class="sxs-lookup"><span data-stu-id="559c2-123">A **Mapping Details** window for viewing and editing mappings.</span></span> <span data-ttu-id="559c2-124">엔터티 형식 또는 연결을 데이터베이스 테이블, 열, 저장 프로시저 등에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-124">You can map entity types or associations to database tables, columns, and stored procedures.</span></span> 

<span data-ttu-id="559c2-125">시각적 디자인 화면 창의 엔터티 데이터 모델 마법사가 완료 되 면 자동으로 열립니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-125">The visual design surface window is automatically opened when the Entity Data Model Wizard finishes.</span></span> <span data-ttu-id="559c2-126">모델 브라우저 표시 되지 않으면, 기본 디자인 화면을 마우스 오른쪽 단추로 **Model 브라우저**합니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-126">If the Model Browser is not visible, right-click the main design surface and select **Model Browser**.</span></span>

<span data-ttu-id="559c2-127">다음 스크린샷은 EF 디자이너에서 열린.edmx 파일을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-127">The following screenshot shows an .edmx file opened in the EF Designer.</span></span> <span data-ttu-id="559c2-128">스크린샷은 시각적 디자인 화면 (왼쪽) 및 **Model 브라우저** (오른쪽)에 창입니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-128">The screenshot shows the visual design surface (to the left) and the **Model Browser** window (to the right).</span></span>

![EF 디자이너 2](~/ef6/media/efdesigner2.png)

<span data-ttu-id="559c2-130">EF 디자이너에서 수행할 작업을 실행 취소 하려면 Ctrl + Z를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-130">To undo an operation done in the EF Designer, click Ctrl-Z.</span></span>

## <a name="working-with-diagrams"></a><span data-ttu-id="559c2-131">다이어그램 작업</span><span class="sxs-lookup"><span data-stu-id="559c2-131">Working with Diagrams</span></span>

<span data-ttu-id="559c2-132">기본적으로 EF 디자이너 Diagram1를 호출 하는 한 다이어그램을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-132">By default the EF Designer creates one diagram called Diagram1.</span></span> <span data-ttu-id="559c2-133">많은 수의 엔터티 및 연결을 사용 하 여 다이어그램에 있는 경우는 가장 원하는 논리적으로 분할 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-133">If you have a diagram with a large number of entities and associations, you will most like want to split them up logically.</span></span> <span data-ttu-id="559c2-134">Visual Studio 2012부터 여러 다이어그램에서 개념적 모델을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-134">Starting with Visual Studio 2012, you can view your conceptual model in multiple diagrams.</span></span>   

<span data-ttu-id="559c2-135">새 다이어그램을 추가 하면 다이어그램 폴더 Model 브라우저 창에 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-135">As you add new diagrams, they appear under the Diagrams folder in the Model Browser window.</span></span> <span data-ttu-id="559c2-136">다이어그램의 이름을 바꾸려면: Model 브라우저 창에서 다이어그램을 선택, 이름 뒤에 한 번 클릭 하 고 새 이름을 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-136">To rename a diagram: select the diagram in the Model Browser window, click once on the name, and type the new name.</span></span>  <span data-ttu-id="559c2-137">또한 다이어그램 이름을 마우스 오른쪽 단추로 클릭 하 고 선택할 수 **이름 바꾸기**합니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-137">You can also right-click the diagram name and select **Rename**.</span></span>

<span data-ttu-id="559c2-138">다이어그램 이름은 Visual Studio 편집기에서.edmx 파일 이름 옆에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-138">The diagram name is displayed next to the .edmx file name, in the Visual Studio editor.</span></span> <span data-ttu-id="559c2-139">예를 들어 Model1.edmx\[Diagram1\]합니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-139">For example Model1.edmx\[Diagram1\].</span></span>

![DiagramName](~/ef6/media/diagramname.png)

<span data-ttu-id="559c2-141">다이어그램 콘텐츠 (모양 및 색의 엔터티 및 연결)에 저장 됩니다는. edmx.diagram 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-141">The diagrams content (shape and color of entities and associations) is stored in the .edmx.diagram file.</span></span> <span data-ttu-id="559c2-142">이 파일을 보려면 솔루션 탐색기를 선택 하 고.edmx 파일을 펼칩니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-142">To view this file, select Solution Explorer and unfold the .edmx file.</span></span> 

![DiagramFiles](~/ef6/media/diagramfiles.png)

<span data-ttu-id="559c2-144">편집 하지 마십시오는. edmx.diagram EF 디자이너에서 덮어쓸 수 있습니다이 파일의 콘텐츠 파일을 수동으로.</span><span class="sxs-lookup"><span data-stu-id="559c2-144">You should not edit the .edmx.diagram file manually, the content of this file maybe overwritten by the EF Designer.</span></span>
 
## <a name="splitting-entities-and-associations-into-a-new-diagram"></a><span data-ttu-id="559c2-145">분할 엔터티 및 새 다이어그램에 연결</span><span class="sxs-lookup"><span data-stu-id="559c2-145">Splitting Entities and Associations into a New Diagram</span></span>

<span data-ttu-id="559c2-146">기존 다이어그램 (여러 엔터티를 선택 하려면 shift 키를 길게)에서 엔터티를 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-146">You can select entities on the existing diagram (hold Shift to select multiple entities).</span></span> <span data-ttu-id="559c2-147">마우스 오른쪽 단추를 클릭 하 고 선택 **새 다이어그램으로 이동**합니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-147">Click the right mouse button and select **Move to new Diagram**.</span></span> <span data-ttu-id="559c2-148">새 다이어그램을 만들고 선택한 엔터티 및 해당 연결 다이어그램으로 이동 됩니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-148">The new diagram is created and the selected entities and their associations are moved to the diagram.</span></span>

<span data-ttu-id="559c2-149">Model 브라우저에서 다이어그램 폴더를 마우스 오른쪽 단추로 클릭 하 고 선택할 수 또는 **새 다이어그램을 추가 합니다.**</span><span class="sxs-lookup"><span data-stu-id="559c2-149">Alternatively, you can right-click the Diagrams folder in Model Browser and select **Add new Diagram.**</span></span> <span data-ttu-id="559c2-150">다음 끌어서 하 고 엔터티를 디자인 화면으로 Model 브라우저에서 엔터티 형식을 폴더 삭제 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-150">You can then drag and drop entities from under the Entity Types folder in Model Browser onto the design surface.</span></span>

<span data-ttu-id="559c2-151">수도 잘라내기 또는 복사 (Ctrl + X 또는 Ctrl + C 키를 사용 하 여) 엔터티 한 다이어그램에서 하 다른 (Ctrl + V 키 사용)을 붙여 넣습니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-151">You can also cut or copy entities (using Ctrl-X or Ctrl-C keys) from one diagram and paste (using Ctrl-V key) on the other.</span></span> <span data-ttu-id="559c2-152">이름이 같은 엔터티는 엔터티를 이미 붙여 넣으려는 다이어그램 있으면 새 엔터티 생성 되어 모델에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-152">If the diagram into which you are pasting an entity already contains an entity with the same name, a new entity will be created and added to the model.</span></span>  <span data-ttu-id="559c2-153">예를 들어: Diagram2 부서 엔터티를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-153">For example: Diagram2 contains the Department entity.</span></span> <span data-ttu-id="559c2-154">그런 다음 Diagram2에서 다른 부서를 붙여넣습니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-154">Then, you paste another Department on Diagram2.</span></span> <span data-ttu-id="559c2-155">Department1 엔터티가 생성 되어 개념적 모델에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-155">The Department1 entity is created and added to the conceptual model.</span></span>   

<span data-ttu-id="559c2-156">다이어그램에서 관련된 엔터티를 포함 하려면 rick 원클릭 엔터티 및 선택 **관련 포함**합니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-156">To include related entities in a diagram, rick-click the entity and select **Include Related**.</span></span> <span data-ttu-id="559c2-157">이 지정 된 다이어그램에 관련된 엔터티 및 연결의 복사본 하 게 합니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-157">This will make a copy of the related entities and associations in the specified diagram.</span></span>

## <a name="changing-the-color-of-entities"></a><span data-ttu-id="559c2-158">엔터티는 색 변경</span><span class="sxs-lookup"><span data-stu-id="559c2-158">Changing the Color of Entities</span></span>

<span data-ttu-id="559c2-159">여러 다이어그램에 모델을 분할 하는 것 외에도 엔터티는 색도 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-159">In addition to splitting a model into multiple diagrams, you can also change colors of your entities.</span></span>

<span data-ttu-id="559c2-160">색을 변경 하려면 디자인 화면에서 엔터티 (또는 여러 엔터티)를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-160">To change the color, select an entity (or multiple entities) on the design surface.</span></span> <span data-ttu-id="559c2-161">그런 다음 마우스 오른쪽 단추를 클릭 하 고 선택 **속성**합니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-161">Then, click the right mouse button and select **Properties**.</span></span> <span data-ttu-id="559c2-162">속성 창에서 선택 합니다 **채우기 색** 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-162">In the Properties window, select the **Fill Color** property.</span></span> <span data-ttu-id="559c2-163">올바른 색 이름 (예를 들어 빨간색) 또는 유효한 RGB (예를 들어, 255, 128, 128)를 사용 하 여 색을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-163">Specify the color using either a valid color name (for example, Red) or a valid RGB (for example, 255, 128, 128).</span></span> 

![색](~/ef6/media/color.png)

## <a name="summary"></a><span data-ttu-id="559c2-165">요약</span><span class="sxs-lookup"><span data-stu-id="559c2-165">Summary</span></span>

<span data-ttu-id="559c2-166">이 항목의 여러 다이어그램에 모델을 분할 하는 방법 및 Entity Framework 디자이너를 사용 하 여 엔터티에 대 한 다른 색을 지정 하는 방법을 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="559c2-166">In this topic we looked at how to split a model into multiple diagrams and also how to specify a different color for an entity using the Entity Framework Designer.</span></span> 
