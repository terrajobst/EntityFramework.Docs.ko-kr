---
title: 디자이너 쿼리 저장 프로시저-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 2e0092b526278597e8477d47eeb642598647bb91
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415204"
---
# <a name="designer-query-stored-procedures"></a><span data-ttu-id="30880-102">디자이너 쿼리 저장 프로시저</span><span class="sxs-lookup"><span data-stu-id="30880-102">Designer Query Stored Procedures</span></span>
<span data-ttu-id="30880-103">이 단계별 연습에서는 Entity Framework Designer (EF Designer)를 사용 하 여 저장 프로시저를 모델로 가져온 다음 가져온 저장 프로시저를 호출 하 여 결과를 검색 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="30880-103">This step-by-step walkthrough show how to use the Entity Framework Designer (EF Designer) to import stored procedures into a model and then call the imported stored procedures to retrieve results.</span></span> 

<span data-ttu-id="30880-104">Code First은 저장 프로시저나 함수에 대 한 매핑을 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="30880-104">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="30880-105">그러나 SqlQuery 메서드를 사용 하 여 저장 프로시저 또는 함수를 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="30880-105">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="30880-106">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="30880-106">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a><span data-ttu-id="30880-107">필수 조건</span><span class="sxs-lookup"><span data-stu-id="30880-107">Prerequisites</span></span>

<span data-ttu-id="30880-108">이 연습을 완료하려면 다음 사항이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-108">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="30880-109">최신 버전의 Visual Studio입니다.</span><span class="sxs-lookup"><span data-stu-id="30880-109">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="30880-110">[School 예제 데이터베이스](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="30880-110">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="30880-111">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="30880-111">Set up the Project</span></span>

-   <span data-ttu-id="30880-112">Visual Studio 2012을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="30880-112">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="30880-113">**파일-&gt; 새로 만들기-&gt; 프로젝트** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-113">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="30880-114">왼쪽 창에서 **Visual C\#** 을 클릭 한 다음 **콘솔** 템플릿을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-114">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="30880-115">이름으로 **Efwithsprocssample** 을 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-115">Enter **EFwithSProcsSample** as the name.</span></span>
-   <span data-ttu-id="30880-116"> **확인**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-116">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="30880-117">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="30880-117">Create a Model</span></span>

-   <span data-ttu-id="30880-118">솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **추가-&gt; 새 항목**을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-118">Right-click the project in Solution Explorer and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="30880-119">왼쪽 메뉴에서 **데이터** 를 선택 하 고 템플릿 창에서 **ADO.NET 엔터티 데이터 모델** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-119">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="30880-120">파일 이름에 대해 **Efwithsprocsmodel .edmx** 를 입력 한 다음 **추가**를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-120">Enter **EFwithSProcsModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="30880-121">Model 콘텐츠 선택 대화 상자에서 **데이터베이스에서 생성**을 선택 하 고 **다음**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-121">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="30880-122"> **새 연결**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-122">Click **New Connection**.</span></span>  
    <span data-ttu-id="30880-123">연결 속성 대화 상자에서 서버 이름 (\\예: **mssqllocaldb**)을 입력 하 고, 인증 방법을 선택 하 고, 데이터베이스 이름으로 **School** 을 입력 한 다음, **확인**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-123">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>  
    <span data-ttu-id="30880-124">데이터 연결 선택 대화 상자가 데이터베이스 연결 설정으로 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="30880-124">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="30880-125">데이터베이스 개체 선택 대화 상자에서 **테이블** 확인란을 선택 하 여 모든 테이블을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-125">In the Choose Your Database Objects dialog box, check the **Tables** checkbox to select all the tables.</span></span>  
    <span data-ttu-id="30880-126">또한 **저장 프로시저 및 함수** 노드 아래에서 **GetStudentGrades** 및 **GetDepartmentName**저장 프로시저를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-126">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **GetStudentGrades** and **GetDepartmentName**.</span></span> 

    ![가져오기](~/ef6/media/import.jpg)

    <span data-ttu-id="30880-128">*Visual Studio 2012부터 EF Designer는 저장 프로시저의 대량 가져오기를 지원 합니다. **엔터티 모델에 선택한 저장 프로시저 및 함수 가져오기** 는 기본적으로 선택 되어 있습니다.*</span><span class="sxs-lookup"><span data-stu-id="30880-128">*Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures. The **Import selected stored procedures and functions into theentity model** is checked by default.*</span></span>
-   <span data-ttu-id="30880-129"> **마침**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-129">Click **Finish**.</span></span>

<span data-ttu-id="30880-130">기본적으로 두 개 이상의 열을 반환 하는 가져온 저장 프로시저 또는 함수의 결과 셰이프는 자동으로 새 복합 형식이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="30880-130">By default, the result shape of each imported stored procedure or function that returns more than one column will automatically become a new complex type.</span></span> <span data-ttu-id="30880-131">이 예제에서는 **GetStudentGrades** 함수의 결과를 **StudentGrade** 엔터티에 매핑하고 **GetDepartmentName** 의 결과를 **none** 으로 매핑해야 합니다 (기본값**없음** ).</span><span class="sxs-lookup"><span data-stu-id="30880-131">In this example we want to map the results of the **GetStudentGrades** function to the **StudentGrade** entity and the results of the **GetDepartmentName** to **none** (**none** is the default value).</span></span>

<span data-ttu-id="30880-132">Function import에서 엔터티 형식을 반환 하려면 해당 저장 프로시저에서 반환 된 열이 반환 된 엔터티 형식의 스칼라 속성과 정확 하 게 일치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-132">For a function import to return an entity type, the columns returned by the corresponding stored procedure must exactly match the scalar properties of the returned entity type.</span></span> <span data-ttu-id="30880-133">Function import는 단순 형식, 복합 형식 또는 값이 없는 컬렉션을 반환할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="30880-133">A function import can also return collections of simple types, complex types, or no value.</span></span>

-   <span data-ttu-id="30880-134">디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 **모델 브라우저**를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-134">Right-click the design surface and select **Model Browser**.</span></span>
-   <span data-ttu-id="30880-135">**모델 브라우저**에서 **Function Imports**를 선택 하 고 **GetStudentGrades** 함수를 두 번 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-135">In **Model Browser**, select **Function Imports**, and then double-click the **GetStudentGrades** function.</span></span>
-   <span data-ttu-id="30880-136">함수 가져오기 편집 대화 상자에서 **엔터티** 선택 하 고 **StudentGrade**를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-136">In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**.</span></span>  
    <span data-ttu-id="30880-137">*함수 **가져오기** **대화 상자의** 맨 위에 있는 가져오기 가능 확인란은 구성 가능한 함수에 매핑할 수 있습니다. 이 확인란을 선택 하면 구성 가능한 함수 (테이블 반환 함수)만 **저장 프로시저/함수 이름** 드롭다운 목록에 표시 됩니다. 이 확인란을 선택 하지 않으면 구성할 수 없는 함수만 목록에 표시 됩니다.*</span><span class="sxs-lookup"><span data-stu-id="30880-137">*The **Function Import is composable** checkbox at the top of the **Function Imports** dialog will let you map to composable functions. If you do check this box, only composable functions (Table-valued Functions) will appear in the **Stored Procedure / Function Name** drop-down list. If you do not check this box, only non-composable functions will be shown in the list.*</span></span>

## <a name="use-the-model"></a><span data-ttu-id="30880-138">모델 사용</span><span class="sxs-lookup"><span data-stu-id="30880-138">Use the Model</span></span>

<span data-ttu-id="30880-139">**Main** 메서드가 정의 된 **Program.cs** 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="30880-139">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="30880-140">Main 함수에 다음 코드를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-140">Add the following code into the Main function.</span></span>

<span data-ttu-id="30880-141">이 코드는 두 개의 저장 프로시저를 호출 합니다. **GetStudentGrades** (지정 *된 StudentId*에 대 한 **StudentGrades** 를 반환) 및 **GetDepartmentName** (출력 매개 변수의 부서 이름을 반환 합니다.)</span><span class="sxs-lookup"><span data-stu-id="30880-141">The code calls two stored procedures: **GetStudentGrades** (returns **StudentGrades** for the specified *StudentId*) and **GetDepartmentName** (returns the name of the department in the output parameter).</span></span>  

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

<span data-ttu-id="30880-142">애플리케이션을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="30880-142">Compile and run the application.</span></span> <span data-ttu-id="30880-143">프로그램에서는 다음이 출력됩니다.</span><span class="sxs-lookup"><span data-stu-id="30880-143">The program produces the following output:</span></span>

```console
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a><span data-ttu-id="30880-144">출력 매개 변수</span><span class="sxs-lookup"><span data-stu-id="30880-144">Output Parameters</span></span>
-----------------

<span data-ttu-id="30880-145">출력 매개 변수를 사용 하는 경우 결과를 완전히 읽을 때까지 해당 값을 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="30880-145">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="30880-146">이는 DbDataReader의 기본 동작 때문입니다. 자세한 내용은 [DataReader를 사용 하 여 데이터 검색](https://go.microsoft.com/fwlink/?LinkID=398589) 을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="30880-146">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>
