---
title: 디자이너 TPH 상속-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 9a546f6450b5aa3b03c062d1ab2c6f9257ba8292
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995006"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="cdfe2-102">디자이너 TPH 상속</span><span class="sxs-lookup"><span data-stu-id="cdfe2-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="cdfe2-103">이 단계별 연습에서는 Entity Framework Designer (EF 디자이너)를 사용 하 여 개념적 모델에서 계층당 하나의 테이블 (TPH) 상속을 구현 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="cdfe2-104">TPH 상속 상속 계층 구조의 엔터티 형식의 모든 데이터를 유지 하기 위해 하나의 데이터베이스 테이블을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="cdfe2-105">이 연습에서는 Person 테이블에 매핑합니다 세 가지 엔터티 형식: Person (기본 형식), (Person에서 파생), 학생과 강사 (Person에서 파생 됨).</span><span class="sxs-lookup"><span data-stu-id="cdfe2-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="cdfe2-106">개념적 모델 (Database First) 데이터베이스에서 만들어 EF 디자이너를 사용 하 여 TPH 상속을 구현 하는 모델을 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="cdfe2-107">Model First를 사용 하 여 TPH 상속을 매핑할 수 있지만 복잡 하는 고유한 데이터베이스 생성 워크플로 작성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="cdfe2-108">이 워크플로를 할당 한 다음 합니다 **Database Generation Workflow** EF 디자이너의 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="cdfe2-109">쉬운 대 안으로 Code First를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="cdfe2-110">다른 상속 옵션</span><span class="sxs-lookup"><span data-stu-id="cdfe2-110">Other Inheritance Options</span></span>

<span data-ttu-id="cdfe2-111">테이블 형식당 TPT ()는 다른 유형의 상속 상속에 참여 하는 엔터티를 별도 데이터베이스의 테이블에에서 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span>  <span data-ttu-id="cdfe2-112">EF 디자이너를 사용 하 여 형식당 하나의 테이블 상속을 매핑하는 방법에 대 한 정보를 참조 하세요 [EF 디자이너 TPT 상속](~/ef6/modeling/designer/inheritance/tpt.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-112">For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="cdfe2-113">구체적 형식당 테이블 형식 상속 TPC () 및 혼합 된 상속 모델에는 Entity Framework 런타임에서 지 있지만 EF 디자이너에서 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="cdfe2-114">TPC 또는 혼합된 상속을 사용 하려는 경우 두 가지 옵션이 있습니다: Code First를 사용 하 여 또는 EDMX 파일을 수동으로 편집 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="cdfe2-115">EDMX 파일을 사용 하려는 경우 매핑 세부 정보 창 "안전 모드로" 넣 및 디자이너를 사용 하 여 매핑을 변경할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cdfe2-116">전제 조건</span><span class="sxs-lookup"><span data-stu-id="cdfe2-116">Prerequisites</span></span>

<span data-ttu-id="cdfe2-117">이 연습을 완료하려면 다음 사항이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="cdfe2-118">Visual Studio의 최신 버전입니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="cdfe2-119">합니다 [School 샘플 데이터베이스](~/ef6/resources/school-database.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="cdfe2-120">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="cdfe2-120">Set up the Project</span></span>

-   <span data-ttu-id="cdfe2-121">Visual Studio 2012를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="cdfe2-122">선택 **파일-&gt; 새로운 기능-&gt; 프로젝트**</span><span class="sxs-lookup"><span data-stu-id="cdfe2-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="cdfe2-123">왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔** 템플릿.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="cdfe2-124">입력 **TPHDBFirstSample** 이름으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="cdfe2-125">**확인**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="cdfe2-126">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="cdfe2-126">Create a Model</span></span>

-   <span data-ttu-id="cdfe2-127">솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 누르고 **추가-&gt; 새 항목**합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="cdfe2-128">선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 템플릿 창에서.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="cdfe2-129">입력 **TPHModel.edmx** 파일 이름 및 클릭 **추가**합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="cdfe2-130">Model 콘텐츠 선택 대화 상자에서 선택 **데이터베이스에서 생성**를 클릭 하 고 **다음**합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="cdfe2-131">클릭 **새 연결**합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-131">Click **New Connection**.</span></span>
    <span data-ttu-id="cdfe2-132">연결 속성 대화 상자에서 서버 이름을 입력 합니다 (예를 들어 **(localdb)\\mssqllocaldb**)을 선택 인증 방법, 형식 **학교** 데이터베이스 이름에 대 한 다음 클릭 **확인**합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="cdfe2-133">데이터 연결 선택 대화 상자는 데이터베이스 연결 설정으로 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="cdfe2-134">데이터베이스 개체 선택 대화 상자의 테이블 노드를 선택 합니다 **Person** 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="cdfe2-135">**마침**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-135">Click **Finish**.</span></span>

<span data-ttu-id="cdfe2-136">모델 편집을 위해 디자인 화면을 제공 하는 Entity Designer에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="cdfe2-137">데이터베이스 개체 선택 대화 상자에서 선택한 모든 개체는 모델에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="cdfe2-138">즉 하는 방법을 **Person** 테이블 데이터베이스에서 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-138">That is how the **Person** table looks in the database.</span></span>

![PersonTable](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="cdfe2-140">계층당 하나의 테이블 상속 구현</span><span class="sxs-lookup"><span data-stu-id="cdfe2-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="cdfe2-141">**Person** 테이블에는 **판별자** 열 두 값 중 하나일 수 있습니다: "Student" 및 "Instructor"입니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="cdfe2-142">값에 따라 합니다 **Person** 테이블에 매핑됩니다 합니다 **학생** 엔터티 또는 **강사** 엔터티.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="cdfe2-143">**Person** 테이블에는 또한 두 열에 **HireDate** 및 **EnrollmentDate**, 이어야 합니다 **nullable** 사람 수 없기 때문에 학생과 강사 동시에 (적어도이 연습의).</span><span class="sxs-lookup"><span data-stu-id="cdfe2-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="cdfe2-144">새 엔터티를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-144">Add new Entities</span></span>

-   <span data-ttu-id="cdfe2-145">새 엔터티를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-145">Add a new entity.</span></span>
    <span data-ttu-id="cdfe2-146">이 위해 Entity Framework Designer의 디자인 화면의 빈 영역을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가-&gt;엔터티**합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="cdfe2-147">형식 **강사** 에 대 한 합니다 **엔터티 이름** 선택한 **Person** 드롭 다운 목록에서를 **기본 형식**합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="cdfe2-148">**확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-148">Click **OK**.</span></span>
-   <span data-ttu-id="cdfe2-149">다른 새 엔터티를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-149">Add another new entity.</span></span> <span data-ttu-id="cdfe2-150">형식 **학생** 에 대 한 합니다 **엔터티 이름** 선택한 **Person** 드롭 다운 목록에서를 **기본 형식**합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="cdfe2-151">두 개의 새 엔터티 형식이 디자인 화면에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="cdfe2-152">화살표는 새 엔터티 형식에서 요소를 **Person** 엔터티 형식에 있음을 나타냅니다 **Person** 새 엔터티 형식에 대 한 기본 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="cdfe2-153">마우스 오른쪽 단추로 클릭 합니다 **HireDate** 의 속성을 **사용자** 엔터티.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="cdfe2-154">선택 **잘라내기** (또는 Ctrl + X 키 사용).</span><span class="sxs-lookup"><span data-stu-id="cdfe2-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="cdfe2-155">마우스 오른쪽 단추로 클릭 합니다 **강사** 엔터티 및 선택 **붙여넣기** (또는 Ctrl + V 키를 사용 하 여).</span><span class="sxs-lookup"><span data-stu-id="cdfe2-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="cdfe2-156">마우스 오른쪽 단추로 클릭 합니다 **HireDate** 속성과 선택 **속성**합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="cdfe2-157">에 **속성** 창에서 설정 합니다 **Nullable** 속성을 **false**합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="cdfe2-158">마우스 오른쪽 단추로 클릭 합니다 **EnrollmentDate** 의 속성을 **사용자** 엔터티.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="cdfe2-159">선택 **잘라내기** (또는 Ctrl + X 키 사용).</span><span class="sxs-lookup"><span data-stu-id="cdfe2-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="cdfe2-160">마우스 오른쪽 단추로 클릭 합니다 **학생** 엔터티 및 선택 **붙여넣기 (또는 Ctrl + V를 사용 하 여 키).**</span><span class="sxs-lookup"><span data-stu-id="cdfe2-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="cdfe2-161">선택 된 **EnrollmentDate** 속성 집합과 합니다 **Nullable** 속성을 **false**합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="cdfe2-162">선택 된 **Person** 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-162">Select the **Person** entity type.</span></span> <span data-ttu-id="cdfe2-163">에 **속성** 창에서 해당 **추상** 속성을 **true**합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="cdfe2-164">삭제 된 **판별자** 속성을 **Person**합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="cdfe2-165">삭제 해야 하는 이유는 다음 섹션에 설명 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="cdfe2-166">엔터티 매핑</span><span class="sxs-lookup"><span data-stu-id="cdfe2-166">Map the entities</span></span>

-   <span data-ttu-id="cdfe2-167">마우스 오른쪽 단추로 클릭 합니다 **강사** 선택한 **테이블 매핑.**</span><span class="sxs-lookup"><span data-stu-id="cdfe2-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="cdfe2-168">Instructor 엔터티 매핑 정보 창에서 선택 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="cdfe2-169">클릭 **&lt;테이블이 나 뷰 추가&gt;** 에 **매핑 정보** 창입니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="cdfe2-170">합니다 **&lt;테이블이 나 뷰 추가&gt;** 필드 테이블의 드롭다운 목록이 됩니다 또는 선택한 엔터티에 매핑할 수 있는 뷰.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="cdfe2-171">선택 **Person** 드롭 다운 목록에서.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="cdfe2-172">합니다 **매핑 정보** 창이 기본 열 매핑과 조건 추가 옵션을 사용 하 여 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="cdfe2-173">클릭할  **&lt;조건 추가&gt;** 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="cdfe2-174">합니다 **&lt;조건 추가&gt;** 필드 조건을 설정할 수 있는 열의 드롭다운 목록이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="cdfe2-175">선택 **판별자** 드롭 다운 목록에서.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="cdfe2-176">에 **연산자** 열의 합니다 **매핑 정보** 창에서 드롭 다운 목록에서 =.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="cdfe2-177">에 **값/속성** 열, 형식 **강사**합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="cdfe2-178">최종 결과 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-178">The end result should look like this:</span></span>

    ![MappingDetails2](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="cdfe2-180">에 대 한 이러한 단계를 반복 합니다 **학생** 엔터티 형식 이지만 같음 조건 확인 **학생** 값입니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="cdfe2-181">*제거 하려는 이유는 **판별자** 속성은 테이블 열을 두 번 매핑할 수 없습니다 때문입니다. 이 열에 사용할 조건부 매핑 속성으로 매핑하기 위한 사용할 수 없습니다. 조건을 사용 하는 경우 둘 다 사용할 수 있습니다 하는 유일한 방법은 **Is Null** 하거나 **Is Not Null** 비교 합니다.*</span><span class="sxs-lookup"><span data-stu-id="cdfe2-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="cdfe2-182">이제 계층당 하나의 테이블 상속이 구현되었습니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![FinalTPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="cdfe2-184">모델 사용</span><span class="sxs-lookup"><span data-stu-id="cdfe2-184">Use the Model</span></span>

<span data-ttu-id="cdfe2-185">열기는 **Program.cs** 파일을 **Main** 메서드 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="cdfe2-186">다음 코드를 붙여 합니다 **Main** 함수입니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="cdfe2-187">코드는 세 가지 쿼리를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-187">The code executes three queries.</span></span> <span data-ttu-id="cdfe2-188">첫 번째 쿼리를 가져오고 모든 **Person** 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="cdfe2-189">두 번째 쿼리에서 **OfType** 를 반환 하는 방법 **강사** 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="cdfe2-190">세 번째 쿼리에서 **OfType** 반환 하는 방법 **학생** 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="cdfe2-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

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
