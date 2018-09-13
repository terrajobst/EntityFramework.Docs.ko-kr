---
title: 저장 프로시저-EF6 디자이너 쿼리
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 6284b10261e6f3b9bf69d1c15e121988e4976d48
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489949"
---
# <a name="designer-query-stored-procedures"></a><span data-ttu-id="68f3e-102">디자이너 쿼리 저장 프로시저</span><span class="sxs-lookup"><span data-stu-id="68f3e-102">Designer Query Stored Procedures</span></span>
<span data-ttu-id="68f3e-103">이 단계별 연습에는 저장된 프로시저를 가져오려는 Entity Framework Designer (EF 디자이너)를 사용 하 여 모델에 다음 결과 검색 하 고 가져온된 저장된 프로시저를 호출 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-103">This step-by-step walkthrough show how to use the Entity Framework Designer (EF Designer) to import stored procedures into a model and then call the imported stored procedures to retrieve results.</span></span> 

<span data-ttu-id="68f3e-104">Code First에서 지원 하지 않음을 저장 프로시저나 함수에 대 한 매핑 참고 합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-104">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="68f3e-105">그러나 System.Data.Entity.DbSet.SqlQuery 메서드를 사용 하 여 저장된 프로시저 또는 함수를 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-105">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="68f3e-106">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="68f3e-106">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a><span data-ttu-id="68f3e-107">전제 조건</span><span class="sxs-lookup"><span data-stu-id="68f3e-107">Prerequisites</span></span>

<span data-ttu-id="68f3e-108">이 연습을 완료하려면 다음 사항이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-108">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="68f3e-109">Visual Studio의 최신 버전입니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-109">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="68f3e-110">합니다 [School 샘플 데이터베이스](~/ef6/resources/school-database.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-110">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="68f3e-111">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="68f3e-111">Set up the Project</span></span>

-   <span data-ttu-id="68f3e-112">Visual Studio 2012를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-112">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="68f3e-113">선택 **파일-&gt; 새로운 기능-&gt; 프로젝트**</span><span class="sxs-lookup"><span data-stu-id="68f3e-113">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="68f3e-114">왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔** 템플릿.</span><span class="sxs-lookup"><span data-stu-id="68f3e-114">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="68f3e-115">입력 **EFwithSProcsSample** 이름으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-115">Enter **EFwithSProcsSample** as the name.</span></span>
-   <span data-ttu-id="68f3e-116">**확인**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-116">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="68f3e-117">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="68f3e-117">Create a Model</span></span>

-   <span data-ttu-id="68f3e-118">솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 누르고 **추가-&gt; 새 항목**합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-118">Right-click the project in Solution Explorer and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="68f3e-119">선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 템플릿 창에서.</span><span class="sxs-lookup"><span data-stu-id="68f3e-119">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="68f3e-120">입력 **EFwithSProcsModel.edmx** 파일 이름 및 클릭 **추가**합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-120">Enter **EFwithSProcsModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="68f3e-121">Model 콘텐츠 선택 대화 상자에서 선택 **데이터베이스에서 생성**를 클릭 하 고 **다음**합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-121">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="68f3e-122">클릭 **새 연결**합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-122">Click **New Connection**.</span></span>  
    <span data-ttu-id="68f3e-123">연결 속성 대화 상자에서 서버 이름을 입력 합니다 (예를 들어 **(localdb)\\mssqllocaldb**)을 선택 인증 방법, 형식 **학교** 데이터베이스 이름에 대 한 다음 클릭 **확인**합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-123">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>  
    <span data-ttu-id="68f3e-124">데이터 연결 선택 대화 상자는 데이터베이스 연결 설정으로 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-124">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="68f3e-125">데이터베이스 개체 선택 대화 상자에서 확인 합니다 **테이블** 모든 테이블을 선택 하려면 확인란을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-125">In the Choose Your Database Objects dialog box, check the **Tables** checkbox to select all the tables.</span></span>  
    <span data-ttu-id="68f3e-126">또한 아래에 있는 다음 저장된 프로시저를 선택 합니다 **저장 프로시저 및 함수** 노드: **GetStudentGrades** 하 고 **GetDepartmentName**합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-126">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **GetStudentGrades** and **GetDepartmentName**.</span></span> 

    ![가져오기](~/ef6/media/import.jpg)

    <span data-ttu-id="68f3e-128">*Visual Studio 2012를 사용한 EF 디자이너 시작 저장된 프로시저의 대량 가져오기를 지원 합니다. 합니다 **theentity 모델로 저장된 프로시저 및 함수를 선택 하는 가져오기** 기본적으로 선택 됩니다.*</span><span class="sxs-lookup"><span data-stu-id="68f3e-128">*Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures. The **Import selected stored procedures and functions into theentity model** is checked by default.*</span></span>
-   <span data-ttu-id="68f3e-129">**마침**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-129">Click **Finish**.</span></span>

<span data-ttu-id="68f3e-130">기본적으로 각 가져온된 저장된 프로시저 또는 함수의 둘 이상의 열을 반환 하는 결과 도형은 새 복합 형식을 자동으로 있게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-130">By default, the result shape of each imported stored procedure or function that returns more than one column will automatically become a new complex type.</span></span> <span data-ttu-id="68f3e-131">이 예제에서는 매핑할 결과 **GetStudentGrades** 함수를 합니다 **StudentGrade** 엔터티 및 결과 **GetDepartmentName** 에**none** (**none** 기본값).</span><span class="sxs-lookup"><span data-stu-id="68f3e-131">In this example we want to map the results of the **GetStudentGrades** function to the **StudentGrade** entity and the results of the **GetDepartmentName** to **none** (**none** is the default value).</span></span>

<span data-ttu-id="68f3e-132">엔터티 형식을 반환 하는 함수 가져오기에 대 한 해당 저장된 프로시저에서 반환 되는 열 반환 된 엔터티 형식의 스칼라 속성을 정확히 일치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-132">For a function import to return an entity type, the columns returned by the corresponding stored procedure must exactly match the scalar properties of the returned entity type.</span></span> <span data-ttu-id="68f3e-133">Function import는 단순 형식, 복합 형식 또는 값의 컬렉션을 반환할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-133">A function import can also return collections of simple types, complex types, or no value.</span></span>

-   <span data-ttu-id="68f3e-134">디자인 화면을 마우스 오른쪽 단추로 클릭 **Model 브라우저**합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-134">Right-click the design surface and select **Model Browser**.</span></span>
-   <span data-ttu-id="68f3e-135">**Model 브라우저**를 선택 **Function Imports**를 두 번 클릭 하 고는 **GetStudentGrades** 함수.</span><span class="sxs-lookup"><span data-stu-id="68f3e-135">In **Model Browser**, select **Function Imports**, and then double-click the **GetStudentGrades** function.</span></span>
-   <span data-ttu-id="68f3e-136">Function Import 편집 대화 상자에서 선택 **엔터티** 선택한 **StudentGrade**합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-136">In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**.</span></span>  
    <span data-ttu-id="68f3e-137">***Function Import는 구성이 가능 하므로** 맨 위에 있는 확인란을 선택 합니다 **Function Imports** 대화 상자를 사용 하면 구성 가능한 함수에 매핑할 수입니다. 이 상자를 선택 하는 경우에 구성 가능 함수 (테이블 반환 함수)에 표시 됩니다는 **저장 프로시저 / 함수 이름** 드롭 다운 목록. 이 상자를 선택 하지 않을 경우 목록에서 구성 가능 하지 않음 함수만 표시 됩니다.*</span><span class="sxs-lookup"><span data-stu-id="68f3e-137">*The **Function Import is composable** checkbox at the top of the **Function Imports** dialog will let you map to composable functions. If you do check this box, only composable functions (Table-valued Functions) will appear in the **Stored Procedure / Function Name** drop-down list. If you do not check this box, only non-composable functions will be shown in the list.*</span></span>

## <a name="use-the-model"></a><span data-ttu-id="68f3e-138">모델 사용</span><span class="sxs-lookup"><span data-stu-id="68f3e-138">Use the Model</span></span>

<span data-ttu-id="68f3e-139">열기는 **Program.cs** 파일을 **Main** 메서드 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-139">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="68f3e-140">다음 코드를 Main 함수에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-140">Add the following code into the Main function.</span></span>

<span data-ttu-id="68f3e-141">두 개의 저장된 프로시저를 호출 하는 코드: **GetStudentGrades** (반환 **StudentGrades** 지정 된 *StudentId*) 및 **GetDepartmentName** (출력 매개 변수에서 부서 이름을 반환).</span><span class="sxs-lookup"><span data-stu-id="68f3e-141">The code calls two stored procedures: **GetStudentGrades** (returns **StudentGrades** for the specified *StudentId*) and **GetDepartmentName** (returns the name of the department in the output parameter).</span></span>  

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        // Specify the Student ID.
        int studentId = 2;

        // Call GetStudentGrades and iterate through the returned collection.
        foreach (StudentGrade grade in context.GetStudentGrades(studentId))
        {
            Console.WriteLine("StudentID: {0}\tSubject={1}", studentId, grade.Subject);
            Console.WriteLine("Student grade: " + grade.Grade);
        }

        // Call GetDepartmentName.
        // Declare the name variable that will contain the value returned by the output parameter.
        ObjectParameter name = new ObjectParameter("Name", typeof(String));
        context.GetDepartmentName(1, name);
        Console.WriteLine("The department name is {0}", name.Value);

    }
```

<span data-ttu-id="68f3e-142">응용 프로그램을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-142">Compile and run the application.</span></span> <span data-ttu-id="68f3e-143">프로그램에서는 다음이 출력됩니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-143">The program produces the following output:</span></span>

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a><span data-ttu-id="68f3e-144">출력 매개 변수</span><span class="sxs-lookup"><span data-stu-id="68f3e-144">Output Parameters</span></span>
-----------------

<span data-ttu-id="68f3e-145">출력 매개 변수를 사용 하는 경우 결과 완전히 읽을 때까지 해당 값은 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-145">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="68f3e-146">내용은 DbDataReader의 기본 동작으로 인해 이것이 [DataReader를 사용 하 여 데이터 검색](http://go.microsoft.com/fwlink/?LinkID=398589) 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="68f3e-146">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>
