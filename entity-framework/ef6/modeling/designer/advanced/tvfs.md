---
title: Tvf (테이블 반환 함수)-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: 35684196dcd7b708a8feeb1eca3096e8d4e555ec
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415450"
---
# <a name="table-valued-functions-tvfs"></a><span data-ttu-id="12d4b-102">테이블 반환 함수 (Tvf)</span><span class="sxs-lookup"><span data-stu-id="12d4b-102">Table-Valued Functions (TVFs)</span></span>
> [!NOTE]
> <span data-ttu-id="12d4b-103">**EF5** 이상-이 페이지에서 설명 하는 기능, api 등은 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="12d4b-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="12d4b-105">비디오 및 단계별 연습에서는 Entity Framework Designer를 사용 하 여 Tvf (테이블 반환 함수)를 매핑하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-105">The video and step-by-step walkthrough shows how to map table-valued functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="12d4b-106">또한 LINQ 쿼리에서 TVF를 호출 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-106">It also demonstrates how to call a TVF from a LINQ query.</span></span>

<span data-ttu-id="12d4b-107">Tvf는 현재 Database First 워크플로에서만 지원 됩니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-107">TVFs are currently only supported in the Database First workflow.</span></span>

<span data-ttu-id="12d4b-108">TVF 지원은 Entity Framework 버전 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-108">TVF support was introduced in Entity Framework version 5.</span></span> <span data-ttu-id="12d4b-109">테이블 반환 함수, 열거형 및 공간 형식과 같은 새 기능을 사용 하려면 .NET Framework 4.5를 대상으로 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-109">Note that to use the new features like table-valued functions, enums, and spatial types you must target .NET Framework 4.5.</span></span> <span data-ttu-id="12d4b-110">Visual Studio 2012는 기본적으로 .NET 4.5를 대상으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="12d4b-111">Tvf는 저장 프로시저와 매우 유사 합니다. 즉, TVF의 결과는 구성 가능 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-111">TVFs are very similar to stored procedures with one key difference: the result of a TVF is composable.</span></span> <span data-ttu-id="12d4b-112">즉, 저장 프로시저의 결과는 불가능 한 경우 TVF의 결과를 LINQ 쿼리에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-112">That means the results from a TVF can be used in a LINQ query while the results of a stored procedure cannot.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="12d4b-113">비디오 보기</span><span class="sxs-lookup"><span data-stu-id="12d4b-113">Watch the video</span></span>

<span data-ttu-id="12d4b-114">제공한 **사람**: 줄리아 Kornich</span><span class="sxs-lookup"><span data-stu-id="12d4b-114">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="12d4b-115">[Wmv](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [wmv (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span><span class="sxs-lookup"><span data-stu-id="12d4b-115">[WMV](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="12d4b-116">필수 구성 요소</span><span class="sxs-lookup"><span data-stu-id="12d4b-116">Pre-Requisites</span></span>

<span data-ttu-id="12d4b-117">이 연습을 완료 하려면 다음을 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-117">To complete this walkthrough, you need to:</span></span>

- <span data-ttu-id="12d4b-118">[School 데이터베이스](~/ef6/resources/school-database.md)를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-118">Install the [School database](~/ef6/resources/school-database.md).</span></span>

- <span data-ttu-id="12d4b-119">최신 버전의 Visual Studio가 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-119">Have a recent version of Visual Studio</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="12d4b-120">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="12d4b-120">Set up the Project</span></span>

1.  <span data-ttu-id="12d4b-121">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-121">Open Visual Studio</span></span>
2.  <span data-ttu-id="12d4b-122">**파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트** 를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-122">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="12d4b-123">왼쪽 창에서 **Visual C\#** 을 클릭 한 다음 **콘솔** 템플릿을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-123">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="12d4b-124">프로젝트 이름으로 **TVF** 를 입력 하 고 **확인을** 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-124">Enter **TVF** as the name of the project and click **OK**</span></span>

## <a name="add-a-tvf-to-the-database"></a><span data-ttu-id="12d4b-125">데이터베이스에 TVF 추가</span><span class="sxs-lookup"><span data-stu-id="12d4b-125">Add a TVF to the Database</span></span>

-   <span data-ttu-id="12d4b-126">**보기-&gt; 선택 SQL Server 개체 탐색기**</span><span class="sxs-lookup"><span data-stu-id="12d4b-126">Select **View -&gt; SQL Server Object Explorer**</span></span>
-   <span data-ttu-id="12d4b-127">LocalDB가 서버 목록에 없는 경우 **SQL Server** 를 마우스 오른쪽 단추로 클릭 하 고 추가를 선택 **SQL Server** 기본 **Windows 인증** 을 사용 하 여 localdb 서버에 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-127">If LocalDB is not in the list of servers: Right-click on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB server</span></span>
-   <span data-ttu-id="12d4b-128">LocalDB 노드 확장</span><span class="sxs-lookup"><span data-stu-id="12d4b-128">Expand the LocalDB node</span></span>
-   <span data-ttu-id="12d4b-129">데이터베이스 노드에서 School 데이터베이스 노드를 마우스 오른쪽 단추로 클릭 하 고 **새 쿼리** ...를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-129">Under the Databases node, right-click the School database node and select **New Query…**</span></span>
-   <span data-ttu-id="12d4b-130">T-sql 편집기에서 다음 TVF 정의를 붙여넣습니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-130">In T-SQL Editor, paste the following TVF definition</span></span>

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

-   <span data-ttu-id="12d4b-131">T-sql 편집기에서 마우스 오른쪽 단추를 클릭 하 고 **실행** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-131">Click the right mouse button on the T-SQL editor and select **Execute**</span></span>
-   <span data-ttu-id="12d4b-132">GetStudentGradesForCourse 함수는 School 데이터베이스에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-132">The GetStudentGradesForCourse function is added to the School database</span></span>

 

## <a name="create-a-model"></a><span data-ttu-id="12d4b-133">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="12d4b-133">Create a Model</span></span>

1.  <span data-ttu-id="12d4b-134">솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가**를 가리킨 다음 **새 항목** 을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-134">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="12d4b-135">왼쪽 메뉴에서 **데이터** 를 선택 하 고 **템플릿** 창에서 **ADO.NET 엔터티 데이터 모델** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-135">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the **Templates** pane</span></span>
3.  <span data-ttu-id="12d4b-136">파일 이름에 TVFModel를 입력 한 다음 **추가** 를 클릭 **합니다.**</span><span class="sxs-lookup"><span data-stu-id="12d4b-136">Enter **TVFModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="12d4b-137">Model 콘텐츠 선택 대화 상자에서 **데이터베이스에서 생성**을 선택 하 고 **다음** 을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-137">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**</span></span>
5.  <span data-ttu-id="12d4b-138">서버 이름 텍스트 상자에서 **새 연결** 입력 **(localdb)\\mssqllocaldb** 를 클릭 하 여 데이터베이스 이름에 대 한 **School** 를 입력 하 고 **확인을** 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-138">Click **New Connection** Enter **(localdb)\\mssqllocaldb** in the Server name text box Enter **School** for the database name Click **OK**</span></span>
6.  <span data-ttu-id="12d4b-139">데이터베이스 개체 선택 대화 상자의 **테이블** 노드에서 **Person**, **StudentGrade**및 **강의** 테이블을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-139">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person**, **StudentGrade**, and **Course** tables</span></span>
7.  <span data-ttu-id="12d4b-140"> **저장 프로시저 및 함수 노드** 아래에 있는 **GetStudentGradesForCourse** 함수를 선택 하 고 Visual Studio 2012부터 시작 하 여 저장 프로시저 및 함수를 일괄적으로 가져올 수 Entity Designer.</span><span class="sxs-lookup"><span data-stu-id="12d4b-140">Select the **GetStudentGradesForCourse** function located under the **Stored Procedures and Functions** node Note, that starting with Visual Studio 2012, the Entity Designer allows you to batch import your Stored Procedures and Functions</span></span>
8.  <span data-ttu-id="12d4b-141"> **마침** 클릭</span><span class="sxs-lookup"><span data-stu-id="12d4b-141">Click **Finish**</span></span>
9.  <span data-ttu-id="12d4b-142">모델 편집을 위한 디자인 화면을 제공 하는 Entity Designer 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-142">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="12d4b-143"> **데이터베이스 개체 선택** 대화 상자에서 선택한 모든 개체가 모델에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-143">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>
10. <span data-ttu-id="12d4b-144">기본적으로 가져온 각 저장 프로시저 또는 함수의 결과 셰이프는 자동으로 엔터티 모델에 새 복합 형식이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-144">By default, the result shape of each imported stored procedure or function will automatically become a new complex type in your entity model.</span></span> <span data-ttu-id="12d4b-145">그러나 GetStudentGradesForCourse 함수의 결과를 StudentGrade 엔터티에 매핑하려면 디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 모델 브라우저에서 **모델 브라우저** 를 선택한 다음 **함수**가져오기를 선택 하 고 함수 가져오기 편집 대화 상자에서 **GetStudentGradesForCourse** 함수를 두 번 클릭 한 다음  **엔터티** 를 선택 하 고 **StudentGrade** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-145">But we want to map the results of the GetStudentGradesForCourse function to the StudentGrade entity: Right-click the design surface and select **Model Browser** In Model Browser, select **Function Imports**, and then double-click the **GetStudentGradesForCourse** function In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="12d4b-146">데이터 유지 및 검색</span><span class="sxs-lookup"><span data-stu-id="12d4b-146">Persist and Retrieve Data</span></span>

<span data-ttu-id="12d4b-147">Main 메서드가 정의 된 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-147">Open the file where the Main method is defined.</span></span> <span data-ttu-id="12d4b-148">Main 함수에 다음 코드를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-148">Add the following code into the Main function.</span></span>

<span data-ttu-id="12d4b-149">다음 코드에서는 테이블 반환 함수를 사용 하는 쿼리를 작성 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-149">The following code demonstrates how to build a query that uses a Table-valued Function.</span></span> <span data-ttu-id="12d4b-150">이 쿼리는 관련 강좌 제목 및 등급이 3.5 이상인 관련 학생을 포함 하는 무명 형식으로 결과를 프로젝션 합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-150">The query projects the results into an anonymous type that contains the related Course title and related students with a grade greater or equal to 3.5.</span></span>

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

<span data-ttu-id="12d4b-151">애플리케이션을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-151">Compile and run the application.</span></span> <span data-ttu-id="12d4b-152">프로그램에서는 다음이 출력됩니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-152">The program produces the following output:</span></span>

```console
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a><span data-ttu-id="12d4b-153">요약</span><span class="sxs-lookup"><span data-stu-id="12d4b-153">Summary</span></span>

<span data-ttu-id="12d4b-154">이 연습에서는 Entity Framework Designer를 사용 하 여 Tvf (테이블 반환 함수)를 매핑하는 방법에 대해 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-154">In this walkthrough we looked at how to map Table-valued Functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="12d4b-155">또한 LINQ 쿼리에서 TVF를 호출 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="12d4b-155">It also demonstrated how to call a TVF from a LINQ query.</span></span>
