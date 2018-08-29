---
title: 테이블 반환 함수 (Tvf)-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: f4b8c1bd442bac67a06584bd23c040381374f303
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993249"
---
# <a name="table-valued-functions-tvfs"></a><span data-ttu-id="cefa6-102">테이블 반환 함수 (Tvf)</span><span class="sxs-lookup"><span data-stu-id="cefa6-102">Table-Valued Functions (TVFs)</span></span>
> [!NOTE]
> <span data-ttu-id="cefa6-103">**EF5 이상만** -Api 기능 등이이 페이지에 설명 된 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="cefa6-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="cefa6-105">비디오 및 단계별 연습에는 테이블 반환 함수 (Tvf) Entity Framework 디자이너를 사용 하 여 매핑하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-105">The video and step-by-step walkthrough shows how to map table-valued functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="cefa6-106">또한 LINQ 쿼리에서 TVF를 호출 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-106">It also demonstrates how to call a TVF from a LINQ query.</span></span>

<span data-ttu-id="cefa6-107">Tvf는 현재 Database First 워크플로에서 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-107">TVFs are currently only supported in the Database First workflow.</span></span>

<span data-ttu-id="cefa6-108">TVF 지원 Entity Framework 5 버전에서에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-108">TVF support was introduced in Entity Framework version 5.</span></span> <span data-ttu-id="cefa6-109">테이블 반환 함수, 열거형 및.NET Framework 4.5를 대상 해야 공간 형식과 같은 새 기능을 사용 하는 note 합니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-109">Note that to use the new features like table-valued functions, enums, and spatial types you must target .NET Framework 4.5.</span></span> <span data-ttu-id="cefa6-110">Visual Studio 2012는 기본적으로.NET 4.5를 대상으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="cefa6-111">Tvf는 하나의 주요 차이점이 있지만 저장된 프로시저와 매우 비슷합니다: TVF의 결과 구성 가능 합니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-111">TVFs are very similar to stored procedures with one key difference: the result of a TVF is composable.</span></span> <span data-ttu-id="cefa6-112">즉, 저장된 프로시저의 결과 수 없습니다 되지만 TVF의 결과 ' LINQ 쿼리에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-112">That means the results from a TVF can be used in a LINQ query while the results of a stored procedure cannot.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="cefa6-113">비디오를 시청 하세요.</span><span class="sxs-lookup"><span data-stu-id="cefa6-113">Watch the video</span></span>

<span data-ttu-id="cefa6-114">**제공한**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="cefa6-114">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="cefa6-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span><span class="sxs-lookup"><span data-stu-id="cefa6-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="cefa6-116">필수 조건</span><span class="sxs-lookup"><span data-stu-id="cefa6-116">Pre-Requisites</span></span>

<span data-ttu-id="cefa6-117">이 연습을 완료 하려면:</span><span class="sxs-lookup"><span data-stu-id="cefa6-117">To complete this walkthrough, you need to:</span></span>

- <span data-ttu-id="cefa6-118">설치 합니다 [School 데이터베이스](~/ef6/resources/school-database.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-118">Install the [School database](~/ef6/resources/school-database.md).</span></span>

- <span data-ttu-id="cefa6-119">Visual Studio의 최신 버전</span><span class="sxs-lookup"><span data-stu-id="cefa6-119">Have a recent version of Visual Studio</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="cefa6-120">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="cefa6-120">Set up the Project</span></span>

1.  <span data-ttu-id="cefa6-121">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-121">Open Visual Studio</span></span>
2.  <span data-ttu-id="cefa6-122">에 **파일** 메뉴에서 **새로 만들기**를 클릭 하 고 **프로젝트**</span><span class="sxs-lookup"><span data-stu-id="cefa6-122">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="cefa6-123">왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔** 템플릿</span><span class="sxs-lookup"><span data-stu-id="cefa6-123">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="cefa6-124">입력 **TVF** 고 프로젝트의 이름으로 **확인**</span><span class="sxs-lookup"><span data-stu-id="cefa6-124">Enter **TVF** as the name of the project and click **OK**</span></span>

## <a name="add-a-tvf-to-the-database"></a><span data-ttu-id="cefa6-125">TVF를 데이터베이스에 추가</span><span class="sxs-lookup"><span data-stu-id="cefa6-125">Add a TVF to the Database</span></span>

-   <span data-ttu-id="cefa6-126">선택 **뷰-&gt; SQL Server 개체 탐색기**</span><span class="sxs-lookup"><span data-stu-id="cefa6-126">Select **View -&gt; SQL Server Object Explorer**</span></span>
-   <span data-ttu-id="cefa6-127">LocalDB 서버 목록에 없는 경우: 마우스 오른쪽 단추로 클릭 **SQL Server** 선택한 **SQL Server 추가** 기본값을 사용 하 여 **Windows 인증** LocalDB 서버에 연결</span><span class="sxs-lookup"><span data-stu-id="cefa6-127">If LocalDB is not in the list of servers: Right-click on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB server</span></span>
-   <span data-ttu-id="cefa6-128">LocalDB 노드를 확장 합니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-128">Expand the LocalDB node</span></span>
-   <span data-ttu-id="cefa6-129">데이터베이스 노드 아래에서 School 데이터베이스 노드를 마우스 오른쪽 단추로 클릭 하 고 선택 **새 쿼리...**</span><span class="sxs-lookup"><span data-stu-id="cefa6-129">Under the Databases node, right-click the School database node and select **New Query…**</span></span>
-   <span data-ttu-id="cefa6-130">T-SQL 편집기에서 다음 TVF 정을 붙여 넣습니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-130">In T-SQL Editor, paste the following TVF definition</span></span>

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   <span data-ttu-id="cefa6-131">T-SQL 편집기에서 마우스 오른쪽 단추를 클릭 하 고 선택 **실행**</span><span class="sxs-lookup"><span data-stu-id="cefa6-131">Click the right mouse button on the T-SQL editor and select **Execute**</span></span>
-   <span data-ttu-id="cefa6-132">GetStudentGradesForCourse 함수가 School 데이터베이스에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-132">The GetStudentGradesForCourse function is added to the School database</span></span>

 

## <a name="create-a-model"></a><span data-ttu-id="cefa6-133">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="cefa6-133">Create a Model</span></span>

1.  <span data-ttu-id="cefa6-134">솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 **추가**를 클릭 하 고 **새 항목**</span><span class="sxs-lookup"><span data-stu-id="cefa6-134">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="cefa6-135">선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 에 **템플릿** 창</span><span class="sxs-lookup"><span data-stu-id="cefa6-135">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the **Templates** pane</span></span>
3.  <span data-ttu-id="cefa6-136">입력 **TVFModel.edmx** 클릭 한 다음 확인 하 고 파일 이름에 대 한 **추가**</span><span class="sxs-lookup"><span data-stu-id="cefa6-136">Enter **TVFModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="cefa6-137">Model 콘텐츠 선택 대화 상자에서 선택 **데이터베이스에서 생성**를 클릭 하 고 **다음**</span><span class="sxs-lookup"><span data-stu-id="cefa6-137">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**</span></span>
5.  <span data-ttu-id="cefa6-138">클릭 **새 연결** Enter **(localdb)\\mssqllocaldb** Enter의 입력란에 서버 이름 **학교** 데이터베이스의 이름을 클릭 **확인**</span><span class="sxs-lookup"><span data-stu-id="cefa6-138">Click **New Connection** Enter **(localdb)\\mssqllocaldb** in the Server name text box Enter **School** for the database name Click **OK**</span></span>
6.  <span data-ttu-id="cefa6-139">선택에서 하면 데이터베이스 개체 대화 상자에서 **테이블** 노드를 선택 합니다 **Person**를 **StudentGrade**, 및 **과정** 테이블</span><span class="sxs-lookup"><span data-stu-id="cefa6-139">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person**, **StudentGrade**, and **Course** tables</span></span>
7.  <span data-ttu-id="cefa6-140">선택 된 **GetStudentGradesForCourse** 함수 아래에 **저장 프로시저 및 함수** 참고, Visual Studio 2012를 사용 하 여 해당 시작 노드 Entity Designer 사용 하면 일괄 처리 가져올 저장된 프로시저 및 함수를</span><span class="sxs-lookup"><span data-stu-id="cefa6-140">Select the **GetStudentGradesForCourse** function located under the **Stored Procedures and Functions** node Note, that starting with Visual Studio 2012, the Entity Designer allows you to batch import your Stored Procedures and Functions</span></span>
8.  <span data-ttu-id="cefa6-141">클릭 **마침**</span><span class="sxs-lookup"><span data-stu-id="cefa6-141">Click **Finish**</span></span>
9.  <span data-ttu-id="cefa6-142">모델 편집을 위해 디자인 화면을 제공 하는 Entity Designer에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-142">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="cefa6-143">선택한 모든 개체를 **데이터베이스 개체 선택** 대화 상자는 모델에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-143">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>
10. <span data-ttu-id="cefa6-144">기본적으로 각 가져온된 저장된 프로시저 또는 함수의 결과 도형은 엔터티 모델에 새 복합 형식을 자동으로 있게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-144">By default, the result shape of each imported stored procedure or function will automatically become a new complex type in your entity model.</span></span> <span data-ttu-id="cefa6-145">StudentGrade 엔터티에 GetStudentGradesForCourse 함수의 결과 매핑할 하고자 하지만: 디자인 화면을 마우스 오른쪽 단추로 클릭 **Model 브라우저** 에서 Model 브라우저 선택 **Function Imports**를 두 번 클릭 합니다 **GetStudentGradesForCourse** 함수에는 Function Import 편집 대화 상자를 선택 **엔터티** 선택한 **StudentGrade**</span><span class="sxs-lookup"><span data-stu-id="cefa6-145">But we want to map the results of the GetStudentGradesForCourse function to the StudentGrade entity: Right-click the design surface and select **Model Browser** In Model Browser, select **Function Imports**, and then double-click the **GetStudentGradesForCourse** function In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="cefa6-146">유지 하 고 데이터를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-146">Persist and Retrieve Data</span></span>

<span data-ttu-id="cefa6-147">Main 메서드에 정의 되어 있는 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-147">Open the file where the Main method is defined.</span></span> <span data-ttu-id="cefa6-148">다음 코드를 Main 함수에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-148">Add the following code into the Main function.</span></span>

<span data-ttu-id="cefa6-149">다음 코드는 테이블 반환 함수를 사용 하는 쿼리를 작성 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-149">The following code demonstrates how to build a query that uses a Table-valued Function.</span></span> <span data-ttu-id="cefa6-150">쿼리 관련된 과정 제목 및 3.5 보다 크거나 등급을 사용 하 여 관련된 학생을 포함 하는 익명 형식으로 결과 프로젝션 합니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-150">The query projects the results into an anonymous type that contains the related Course title and related students with a grade greater or equal to 3.5.</span></span>

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

<span data-ttu-id="cefa6-151">응용 프로그램을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-151">Compile and run the application.</span></span> <span data-ttu-id="cefa6-152">프로그램에서는 다음이 출력됩니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-152">The program produces the following output:</span></span>

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a><span data-ttu-id="cefa6-153">요약</span><span class="sxs-lookup"><span data-stu-id="cefa6-153">Summary</span></span>

<span data-ttu-id="cefa6-154">이 연습에서는 테이블 반환 함수 (Tvf) Entity Framework 디자이너를 사용 하 여 매핑하는 방법에 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-154">In this walkthrough we looked at how to map Table-valued Functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="cefa6-155">또한 LINQ 쿼리에서 TVF를 호출 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="cefa6-155">It also demonstrated how to call a TVF from a LINQ query.</span></span>
