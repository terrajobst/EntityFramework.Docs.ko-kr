---
title: 디자이너 TPT 상속-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489455"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="11aaf-102">디자이너 TPT 상속</span><span class="sxs-lookup"><span data-stu-id="11aaf-102">Designer TPT Inheritance</span></span>
<span data-ttu-id="11aaf-103">이 단계별 연습에서는 Entity Framework Designer (EF 디자이너)를 사용 하 여 모델에서 형식당 하나의 테이블 (TPT) 상속을 구현 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-103">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="11aaf-104">형식당 하나의 테이블 상속에서는 데이터베이스의 별도 테이블을 사용하여 상속 계층 구조에 있는 각 형식의 상속되지 않는 속성 및 키 속성 데이터를 유지합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-104">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="11aaf-105">이 연습에 매핑하여를 **과정** (기본 형식) **OnlineCourse** (과정에서 파생), 및 **OnsiteCourse** (에서 파생 되 **과정**) 같은 이름의 테이블에 엔터티입니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-105">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="11aaf-106">모델을 데이터베이스에서 만들어 TPT 상속을 구현 하는 모델을 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-106">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="11aaf-107">또한 Model First를 사용 하 여 시작 하 고 모델에서 데이터베이스를 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-107">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="11aaf-108">EF 디자이너에서는 기본적으로 TPT 전략을 사용 하 고 있으므로 모든 상속 모델에는 별도 테이블에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-108">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="11aaf-109">다른 상속 옵션</span><span class="sxs-lookup"><span data-stu-id="11aaf-109">Other Inheritance Options</span></span>

<span data-ttu-id="11aaf-110">테이블-당-TPH (계층)는 다른 유형의 상속 계층 구조의 엔터티 형식의 모든 데이터를 유지 하는 하나의 데이터베이스 테이블을 사용 하는 상속 합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-110">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span>  <span data-ttu-id="11aaf-111">Entity Designer를 사용 하 여 계층당 하나의 테이블 상속을 매핑하는 방법에 대 한 정보를 참조 하세요 [EF 디자이너 TPH 상속](~/ef6/modeling/designer/inheritance/tph.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-111">For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](~/ef6/modeling/designer/inheritance/tph.md).</span></span> 

<span data-ttu-id="11aaf-112">Note는, 구체적 형식당 테이블 형식 상속 TPC () 및 혼합 된 상속 모델은 Entity Framework 런타임에서 지 원하는 있지만 EF 디자이너에서 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-112">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="11aaf-113">TPC 또는 혼합된 상속을 사용 하려는 경우 두 가지 옵션이 있습니다: Code First를 사용 하 여 또는 EDMX 파일을 수동으로 편집 합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-113">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="11aaf-114">EDMX 파일을 사용 하려는 경우 매핑 세부 정보 창 "안전 모드로" 넣 및 디자이너를 사용 하 여 매핑을 변경할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-114">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="11aaf-115">전제 조건</span><span class="sxs-lookup"><span data-stu-id="11aaf-115">Prerequisites</span></span>

<span data-ttu-id="11aaf-116">이 연습을 완료하려면 다음 사항이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-116">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="11aaf-117">Visual Studio의 최신 버전입니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-117">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="11aaf-118">합니다 [School 샘플 데이터베이스](~/ef6/resources/school-database.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-118">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="11aaf-119">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="11aaf-119">Set up the Project</span></span>

-   <span data-ttu-id="11aaf-120">Visual Studio 2012를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-120">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="11aaf-121">선택 **파일-&gt; 새로운 기능-&gt; 프로젝트**</span><span class="sxs-lookup"><span data-stu-id="11aaf-121">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="11aaf-122">왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔** 템플릿.</span><span class="sxs-lookup"><span data-stu-id="11aaf-122">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="11aaf-123">입력 **TPTDBFirstSample** 이름으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-123">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="11aaf-124">**확인**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-124">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="11aaf-125">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="11aaf-125">Create a Model</span></span>

-   <span data-ttu-id="11aaf-126">솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 누르고 **추가-&gt; 새 항목**합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-126">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="11aaf-127">선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 템플릿 창에서.</span><span class="sxs-lookup"><span data-stu-id="11aaf-127">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="11aaf-128">입력 **TPTModel.edmx** 파일 이름 및 클릭 **추가**합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-128">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="11aaf-129">Model 콘텐츠 선택 대화 상자에서 선택 \* \* 생성에서 데이터베이스 \* \*을 클릭 한 다음 **다음**합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-129">In the Choose Model Contents dialog box, select\*\* Generate from database\*\*, and then click **Next**.</span></span>
-   <span data-ttu-id="11aaf-130">클릭 **새 연결**합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-130">Click **New Connection**.</span></span>
    <span data-ttu-id="11aaf-131">연결 속성 대화 상자에서 서버 이름을 입력 합니다 (예를 들어 **(localdb)\\mssqllocaldb**)을 선택 인증 방법, 형식 **학교** 데이터베이스 이름에 대 한 다음 클릭 **확인**합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-131">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="11aaf-132">데이터 연결 선택 대화 상자는 데이터베이스 연결 설정으로 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-132">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="11aaf-133">데이터베이스 개체 선택 대화 상자의 테이블 노드를 선택 합니다 **부서**를 **강의와 OnlineCourse를 OnsiteCourse** 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-133">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="11aaf-134">**마침**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-134">Click **Finish**.</span></span>

<span data-ttu-id="11aaf-135">모델 편집을 위해 디자인 화면을 제공 하는 Entity Designer에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="11aaf-136">데이터베이스 개체 선택 대화 상자에서 선택한 모든 개체는 모델에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-136">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="11aaf-137">형식당 하나의 테이블 상속 구현</span><span class="sxs-lookup"><span data-stu-id="11aaf-137">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="11aaf-138">디자인 화면에서 마우스 오른쪽 단추로 클릭 합니다 **OnlineCourse** 엔터티 형식 및 선택 **속성**합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-138">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="11aaf-139">에 **속성** Base Type 속성을 설정 하는 창 **코스**합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-139">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="11aaf-140">마우스 오른쪽 단추로 클릭 합니다 **OnsiteCourse** 엔터티 형식 및 선택 **속성**합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-140">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="11aaf-141">에 **속성** Base Type 속성을 설정 하는 창 **코스**합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-141">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="11aaf-142">사이의 연결 (선)를 마우스 오른쪽 단추로 클릭 합니다 **OnlineCourse** 하 고 **과정** 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-142">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="11aaf-143">선택 **모델에서 삭제**합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-143">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="11aaf-144">간의 연결을 마우스 오른쪽 단추로 클릭 합니다 **OnsiteCourse** 하 고 **과정** 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-144">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="11aaf-145">선택 **모델에서 삭제**합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-145">Select **Delete from Model**.</span></span>

<span data-ttu-id="11aaf-146">이제 삭제 합니다 **CourseID** 속성을 **OnlineCourse** 하 고 **OnsiteCourse** 이러한 클래스를 상속 하기 때문에 **CourseID** 에서 합니다 **코스** 기본 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-146">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="11aaf-147">마우스 오른쪽 단추로 클릭 합니다 **CourseID** 의 속성을 **OnlineCourse** 엔터티 형식을 선택한 후 **모델에서 삭제**합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-147">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="11aaf-148">마우스 오른쪽 단추로 클릭 합니다 **CourseID** 의 속성을 **OnsiteCourse** 엔터티 형식을 선택한 후 **모델에서 삭제**</span><span class="sxs-lookup"><span data-stu-id="11aaf-148">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="11aaf-149">이제 형식당 하나의 테이블 상속이 구현되었습니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-149">Table-per-type inheritance is now implemented.</span></span>

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="11aaf-151">모델 사용</span><span class="sxs-lookup"><span data-stu-id="11aaf-151">Use the Model</span></span>

<span data-ttu-id="11aaf-152">열기는 **Program.cs** 파일을 **Main** 메서드 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-152">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="11aaf-153">다음 코드를 붙여 합니다 **Main** 함수입니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-153">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="11aaf-154">코드는 세 가지 쿼리를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-154">The code executes three queries.</span></span> <span data-ttu-id="11aaf-155">첫 번째 쿼리 모든 **과정** 지정된 부서와 관련이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-155">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="11aaf-156">두 번째 쿼리에서 **OfType** 반환 하는 방법 **OnlineCourses** 지정된 부서와 관련 된 합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-156">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="11aaf-157">세 번째 쿼리는 반환 **OnsiteCourses**합니다.</span><span class="sxs-lookup"><span data-stu-id="11aaf-157">The third query returns **OnsiteCourses**.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
