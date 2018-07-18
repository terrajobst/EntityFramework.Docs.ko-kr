---
title: 테이블 반환 함수 (Tvf)-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
caps.latest.revision: 3
ms.openlocfilehash: 7d652725a2655b691b03aa3f43103753fe72ede7
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122621"
---
# <a name="table-valued-functions-tvfs"></a>테이블 반환 함수 (Tvf)
> [!NOTE]
> **EF5 이상만** -Api 기능 등이이 페이지에 설명 된 Entity Framework 5에서 도입 되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

비디오 및 단계별 연습에는 테이블 반환 함수 (Tvf) Entity Framework 디자이너를 사용 하 여 매핑하는 방법을 보여 줍니다. 또한 LINQ 쿼리에서 TVF를 호출 하는 방법을 보여 줍니다.

Tvf는 현재 Database First 워크플로에서 지원 합니다.

TVF 지원 Entity Framework 5 버전에서에서 도입 되었습니다. 테이블 반환 함수, 열거형 및.NET Framework 4.5를 대상 해야 공간 형식과 같은 새 기능을 사용 하는 note 합니다. Visual Studio 2012는 기본적으로.NET 4.5를 대상으로 합니다.

Tvf는 하나의 주요 차이점이 있지만 저장된 프로시저와 매우 비슷합니다: TVF의 결과 구성 가능 합니다. 즉, 저장된 프로시저의 결과 수 없습니다 되지만 TVF의 결과 ' LINQ 쿼리에서 사용할 수 있습니다.

## <a name="watch-the-video"></a>비디오를 시청 하세요.

**제공한**: Julia Kornich

[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)

## <a name="pre-requisites"></a>필수 조건

이 연습을 완료 하려면:

- 설치 합니다 [School 데이터베이스](~/ef6/resources/school-database.md)합니다.

- Visual Studio의 최신 버전

## <a name="set-up-the-project"></a>프로젝트 설정

1.  Visual Studio를 엽니다.
2.  에 **파일** 메뉴에서 **새로 만들기**를 클릭 하 고 **프로젝트**
3.  왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔** 템플릿
4.  입력 **TVF** 고 프로젝트의 이름으로 **확인**

## <a name="add-a-tvf-to-the-database"></a>TVF를 데이터베이스에 추가

-   선택 **뷰-&gt; SQL Server 개체 탐색기**
-   LocalDB 서버 목록에 없는 경우: 마우스 오른쪽 단추로 클릭 **SQL Server** 선택한 **SQL Server 추가** 기본값을 사용 하 여 **Windows 인증** LocalDB 서버에 연결
-   LocalDB 노드를 확장 합니다.
-   데이터베이스 노드 아래에서 School 데이터베이스 노드를 마우스 오른쪽 단추로 클릭 하 고 선택 **새 쿼리...**
-   T-SQL 편집기에서 다음 TVF 정을 붙여 넣습니다.

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

-   T-SQL 편집기에서 마우스 오른쪽 단추를 클릭 하 고 선택 **실행**
-   GetStudentGradesForCourse 함수가 School 데이터베이스에 추가 됩니다.

 

## <a name="create-a-model"></a>모델 만들기

1.  솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 **추가**를 클릭 하 고 **새 항목**
2.  선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 에 **템플릿** 창
3.  입력 **TVFModel.edmx** 클릭 한 다음 확인 하 고 파일 이름에 대 한 **추가**
4.  Model 콘텐츠 선택 대화 상자에서 선택 **데이터베이스에서 생성**를 클릭 하 고 **다음**
5.  클릭 **새 연결** Enter **(localdb)\\mssqllocaldb** Enter의 입력란에 서버 이름 **학교** 데이터베이스의 이름을 클릭 **확인**
6.  선택에서 하면 데이터베이스 개체 대화 상자에서 **테이블** 노드를 선택 합니다 **Person**를 **StudentGrade**, 및 **과정** 테이블
7.  선택 된 **GetStudentGradesForCourse** 함수 아래에 **저장 프로시저 및 함수** 참고, Visual Studio 2012를 사용 하 여 해당 시작 노드 Entity Designer 사용 하면 일괄 처리 가져올 저장된 프로시저 및 함수를
8.  클릭 **마침**
9.  모델 편집을 위해 디자인 화면을 제공 하는 Entity Designer에 표시 됩니다. 선택한 모든 개체를 **데이터베이스 개체 선택** 대화 상자는 모델에 추가 됩니다.
10. 기본적으로 각 가져온된 저장된 프로시저 또는 함수의 결과 도형은 엔터티 모델에 새 복합 형식을 자동으로 있게 됩니다. StudentGrade 엔터티에 GetStudentGradesForCourse 함수의 결과 매핑할 하고자 하지만: 디자인 화면을 마우스 오른쪽 단추로 클릭 **Model 브라우저** 에서 Model 브라우저 선택 **Function Imports**를 두 번 클릭 합니다 **GetStudentGradesForCourse** 함수에는 Function Import 편집 대화 상자를 선택 **엔터티** 선택한 **StudentGrade**

## <a name="persist-and-retrieve-data"></a>유지 하 고 데이터를 검색 합니다.

Main 메서드에 정의 되어 있는 파일을 엽니다. 다음 코드를 Main 함수에 추가 합니다.

다음 코드는 테이블 반환 함수를 사용 하는 쿼리를 작성 하는 방법을 보여 줍니다. 쿼리 관련된 과정 제목 및 3.5 보다 크거나 등급을 사용 하 여 관련된 학생을 포함 하는 익명 형식으로 결과 프로젝션 합니다.

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

응용 프로그램을 컴파일하고 실행합니다. 프로그램에서는 다음이 출력됩니다.

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a>요약

이 연습에서는 테이블 반환 함수 (Tvf) Entity Framework 디자이너를 사용 하 여 매핑하는 방법에 살펴보았습니다. 또한 LINQ 쿼리에서 TVF를 호출 하는 방법을 보여 줍니다.
