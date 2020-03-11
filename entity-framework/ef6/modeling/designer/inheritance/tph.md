---
title: 디자이너 TPH 상속-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415234"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="dddf8-102">디자이너 TPH 상속</span><span class="sxs-lookup"><span data-stu-id="dddf8-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="dddf8-103">이 단계별 연습에서는 Entity Framework Designer (EF Designer)를 사용 하 여 개념적 모델에서 TPH (계층당 하나의 테이블) 상속을 구현 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="dddf8-104">TPH 상속은 하나의 데이터베이스 테이블을 사용 하 여 상속 계층 구조의 모든 엔터티 형식에 대 한 데이터를 유지 관리 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="dddf8-105">이 연습에서는 person 테이블을 Person (기본 형식), 학생 (Person에서 파생) 및 강사 (Person에서 파생 됨)의 세 가지 엔터티 형식으로 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="dddf8-106">데이터베이스 (Database First)에서 개념적 모델을 만든 다음 EF Designer를 사용 하 여 TPH 상속을 구현 하도록 모델을 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="dddf8-107">Model First를 사용 하 여 TPH 상속에 매핑할 수 있지만 복잡 한 데이터베이스 생성 워크플로를 직접 작성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="dddf8-108">그런 다음이 워크플로를 EF 디자이너의 **데이터베이스 생성 워크플로** 속성에 할당 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="dddf8-109">보다 쉬운 방법은 Code First를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="dddf8-110">기타 상속 옵션</span><span class="sxs-lookup"><span data-stu-id="dddf8-110">Other Inheritance Options</span></span>

<span data-ttu-id="dddf8-111">형식당 하나의 테이블 (TPT)은 데이터베이스의 개별 테이블이 상속에 참여 하는 엔터티에 매핑되는 또 다른 상속 유형입니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span> <span data-ttu-id="dddf8-112"> EF Designer를 사용 하 여 형식당 하나의 테이블 상속을 매핑하는 방법에 대 한 자세한 내용은 [Ef DESIGNER TPT 상속](~/ef6/modeling/designer/inheritance/tpt.md)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="dddf8-112"> For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="dddf8-113">TPC (비추상 형식 상속) 및 혼합 된 상속 모델은 Entity Framework 런타임에서 지원 되지만 EF 디자이너에서는 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="dddf8-114">TPC 또는 mixed 상속을 사용 하려는 경우 두 가지 옵션이 있습니다. Code First를 사용 하거나 EDMX 파일을 수동으로 편집 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="dddf8-115">EDMX 파일을 사용 하도록 선택 하면 매핑 정보 창이 "안전 모드"로 전환 되 고 디자이너를 사용 하 여 매핑을 변경할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dddf8-116">필수 조건</span><span class="sxs-lookup"><span data-stu-id="dddf8-116">Prerequisites</span></span>

<span data-ttu-id="dddf8-117">이 연습을 완료하려면 다음 사항이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="dddf8-118">최신 버전의 Visual Studio입니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="dddf8-119">[School 예제 데이터베이스](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="dddf8-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="dddf8-120">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="dddf8-120">Set up the Project</span></span>

-   <span data-ttu-id="dddf8-121">Visual Studio 2012을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="dddf8-122">**파일-&gt; 새로 만들기-&gt; 프로젝트** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="dddf8-123">왼쪽 창에서 **Visual C\#** 을 클릭 한 다음 **콘솔** 템플릿을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="dddf8-124">이름으로 **Tphdbfirstsample** 을 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="dddf8-125"> **확인**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="dddf8-126">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="dddf8-126">Create a Model</span></span>

-   <span data-ttu-id="dddf8-127">솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가-&gt; 새 항목**을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="dddf8-128">왼쪽 메뉴에서 **데이터** 를 선택 하 고 템플릿 창에서 **ADO.NET 엔터티 데이터 모델** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="dddf8-129">파일 이름에 대해 **Tphmodel .edmx** 를 입력 한 다음 **추가**를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="dddf8-130">Model 콘텐츠 선택 대화 상자에서 **데이터베이스에서 생성**을 선택 하 고 **다음**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="dddf8-131"> **새 연결**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-131">Click **New Connection**.</span></span>
    <span data-ttu-id="dddf8-132">연결 속성 대화 상자에서 서버 이름 (\\예: **mssqllocaldb**)을 입력 하 고, 인증 방법을 선택 하 고, 데이터베이스 이름으로 **School** 을 입력 한 다음, **확인**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="dddf8-133">데이터 연결 선택 대화 상자가 데이터베이스 연결 설정으로 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="dddf8-134">데이터베이스 개체 선택 대화 상자의 테이블 노드에서 **Person** 테이블을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="dddf8-135"> **마침**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-135">Click **Finish**.</span></span>

<span data-ttu-id="dddf8-136">모델 편집을 위한 디자인 화면을 제공 하는 Entity Designer 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="dddf8-137">데이터베이스 개체 선택 대화 상자에서 선택한 모든 개체가 모델에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="dddf8-138">이는 **Person** 테이블이 데이터베이스에서 표시 되는 방식입니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-138">That is how the **Person** table looks in the database.</span></span>

![Person 테이블](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="dddf8-140">계층당 하나의 테이블 상속 구현</span><span class="sxs-lookup"><span data-stu-id="dddf8-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="dddf8-141"> **Person** 테이블에는 "Student" 및 "강사"의 두 값 중 하나를 가질 수 있는 **판별자** 열이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="dddf8-142">값에 따라 **Person** 테이블은 **Student** 엔터티 또는 **강사** 엔터티에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="dddf8-143"> **Person** 테이블에는 **HireDate** 및 **EnrollmentDate**라는 두 개의 열이 있습니다 .이 열은 한 사람이 동시에 학생 및 강사 일 수 없으므로 **null을 허용** 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="dddf8-144">새 엔터티 추가</span><span class="sxs-lookup"><span data-stu-id="dddf8-144">Add new Entities</span></span>

-   <span data-ttu-id="dddf8-145">새 엔터티를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-145">Add a new entity.</span></span>
    <span data-ttu-id="dddf8-146">이렇게 하려면 Entity Framework Designer 디자인 화면의 빈 공간을 마우스 오른쪽 단추로 클릭 하 고 **추가&gt;엔터티**를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="dddf8-147"> **엔터티 이름** 에 대해 **강사** 를 입력 하 고 **기본 형식의**드롭다운 목록에서 **Person** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="dddf8-148"> **확인을**클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-148">Click **OK**.</span></span>
-   <span data-ttu-id="dddf8-149">다른 새 엔터티를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-149">Add another new entity.</span></span> <span data-ttu-id="dddf8-150"> **엔터티 이름** 에 **Student** 를 입력 하 고 **기본 형식의**드롭다운 목록에서 **Person** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="dddf8-151">두 개의 새 엔터티 형식이 디자인 화면에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="dddf8-152">새 엔터티 형식에서 **Person** 엔터티 형식으로 가리키는 화살표입니다. 이는 **Person** 새 엔터티 형식에 대 한 기본 형식 임을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="dddf8-153"> **Person** 엔터티의 **HireDate** 속성을 마우스 오른쪽 단추로 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="dddf8-154"> **잘라내기** 를 선택 합니다 (또는 Ctrl + X 키 사용).</span><span class="sxs-lookup"><span data-stu-id="dddf8-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="dddf8-155"> **강사** 엔터티를 마우스 오른쪽 단추로 클릭 하 고 **붙여넣기** 를 선택 합니다 (또는 Ctrl + V 키 사용).</span><span class="sxs-lookup"><span data-stu-id="dddf8-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="dddf8-156"> **HireDate** 속성을 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="dddf8-157"> **속성** 창에서 **Nullable** 속성을 **false**로 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="dddf8-158"> **Person** 엔터티의 **EnrollmentDate** 속성을 마우스 오른쪽 단추로 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="dddf8-159"> **잘라내기** 를 선택 합니다 (또는 Ctrl + X 키 사용).</span><span class="sxs-lookup"><span data-stu-id="dddf8-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="dddf8-160"> **학생** 엔터티를 마우스 오른쪽 단추로 클릭 하 고 **붙여넣기를 선택 합니다 (또는 Ctrl + V 키 사용).**</span><span class="sxs-lookup"><span data-stu-id="dddf8-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="dddf8-161"> **EnrollmentDate** 속성을 선택 하 고 **Nullable** 속성을 **false**로 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="dddf8-162"> **Person** 엔터티 형식을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-162">Select the **Person** entity type.</span></span> <span data-ttu-id="dddf8-163"> **속성** 창에서 **추상** 속성을 **true**로 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="dddf8-164">**Person**에서 **판별자** 속성을 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="dddf8-165">삭제 해야 하는 이유는 다음 섹션에 설명 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="dddf8-166">엔터티 매핑</span><span class="sxs-lookup"><span data-stu-id="dddf8-166">Map the entities</span></span>

-   <span data-ttu-id="dddf8-167"> **강사** 를 마우스 오른쪽 단추로 클릭 하 고 **테이블 매핑을 선택 합니다.**</span><span class="sxs-lookup"><span data-stu-id="dddf8-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="dddf8-168">매핑 정보 창에서 강사가 엔터티를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="dddf8-169"> **매핑 정보** 창에서\ \** 테이블 또는 뷰&gt; 추가&lt\;\** 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="dddf8-170"> **테이블 또는 뷰&gt;**  필드를 추가&lt;선택한 엔터티를 매핑할 수 있는 테이블이 나 뷰의 드롭다운 목록이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="dddf8-171">드롭다운 목록에서 **Person** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="dddf8-172"> **매핑 정보** 창은 기본 열 매핑과 조건을 추가 하는 옵션으로 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="dddf8-173">\ \**&lt;&gt;조건 추가\ \**를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="dddf8-174"> **조건을&lt;&gt;조건을 추가** 하면 조건을 설정할 수 있는 열의 드롭다운 목록이 필드가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="dddf8-175">드롭다운 목록에서 **판별자** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="dddf8-176"> **매핑 정보** 창의 **연산자** 열에서 드롭다운 목록에서 =를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="dddf8-177"> **값/속성** 열에 **강사**를 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="dddf8-178">최종 결과는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-178">The end result should look like this:</span></span>

    ![매핑 정보](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="dddf8-180"> **학생** 엔터티 형식에 대해 이러한 단계를 반복 하지만 조건이 **학생** 값과 동일 하 게 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="dddf8-181">***판별자** 속성을 제거 하려는 이유는 테이블 열을 두 번 이상 매핑할 수 없기 때문입니다. 이 열은 조건부 매핑에 사용 되므로 속성 매핑에도 사용할 수 없습니다. 조건에서 \*\*Is null*\* 을 사용 하는 경우에만 사용할 수 있는 유일한 방법은 이 고, 그렇지 않으면 **null** 비교 인 경우입니다.\*</span><span class="sxs-lookup"><span data-stu-id="dddf8-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="dddf8-182">이제 계층당 하나의 테이블 상속이 구현되었습니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![최종 TPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="dddf8-184">모델 사용</span><span class="sxs-lookup"><span data-stu-id="dddf8-184">Use the Model</span></span>

<span data-ttu-id="dddf8-185">**Main** 메서드가 정의 된 **Program.cs** 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="dddf8-186">**Main** 함수에 다음 코드를 붙여 넣습니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="dddf8-187">이 코드는 세 개의 쿼리를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-187">The code executes three queries.</span></span> <span data-ttu-id="dddf8-188">첫 번째 쿼리는 모든 **Person** 개체를 다시 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="dddf8-189">두 번째 쿼리에서는 **OfType** 메서드를 사용 하 여 **강사** 개체를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="dddf8-190">세 번째 쿼리는 **OfType** 메서드를 사용 하 여 **Student** 개체를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="dddf8-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
