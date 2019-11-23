---
title: Tvf (테이블 반환 함수)-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: 35684196dcd7b708a8feeb1eca3096e8d4e555ec
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182529"
---
# <a name="table-valued-functions-tvfs"></a>테이블 반환 함수 (Tvf)
> [!NOTE]
> **EF5** 이상-이 페이지에서 설명 하는 기능, api 등은 Entity Framework 5에서 도입 되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

비디오 및 단계별 연습에서는 Entity Framework Designer를 사용 하 여 Tvf (테이블 반환 함수)를 매핑하는 방법을 보여 줍니다. 또한 LINQ 쿼리에서 TVF를 호출 하는 방법을 보여 줍니다.

Tvf는 현재 Database First 워크플로에서만 지원 됩니다.

TVF 지원은 Entity Framework 버전 5에서 도입 되었습니다. 테이블 반환 함수, 열거형 및 공간 형식과 같은 새 기능을 사용 하려면 .NET Framework 4.5를 대상으로 해야 합니다. Visual Studio 2012는 기본적으로 .NET 4.5를 대상으로 합니다.

Tvf는 저장 프로시저와 매우 유사 합니다. 즉, TVF의 결과는 구성 가능 합니다. 즉, 저장 프로시저의 결과는 불가능 한 경우 TVF의 결과를 LINQ 쿼리에서 사용할 수 있습니다.

## <a name="watch-the-video"></a>비디오 시청

제공한 **사람**: 줄리아 Kornich

[Wmv](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [wmv (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)

## <a name="pre-requisites"></a>필수 구성 요소

이 연습을 완료 하려면 다음을 수행 해야 합니다.

- [School 데이터베이스](~/ef6/resources/school-database.md)를 설치 합니다.

- 최신 버전의 Visual Studio가 있어야 합니다.

## <a name="set-up-the-project"></a>프로젝트 설정

1.  Visual Studio를 엽니다.
2.  **파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트** 를 클릭 합니다.
3.  왼쪽 창에서 **Visual C\#** 을 클릭 한 다음 **콘솔** 템플릿을 선택 합니다.
4.  프로젝트 이름으로 **TVF** 를 입력 하 고 **확인을** 클릭 합니다.

## <a name="add-a-tvf-to-the-database"></a>데이터베이스에 TVF 추가

-   **보기-&gt; 선택 SQL Server 개체 탐색기**
-   LocalDB가 서버 목록에 없는 경우 **SQL Server** 를 마우스 오른쪽 단추로 클릭 하 고 추가를 선택 **SQL Server** 기본 **Windows 인증** 을 사용 하 여 localdb 서버에 연결 합니다.
-   LocalDB 노드 확장
-   데이터베이스 노드에서 School 데이터베이스 노드를 마우스 오른쪽 단추로 클릭 하 고 **새 쿼리** ...를 선택 합니다.
-   T-sql 편집기에서 다음 TVF 정의를 붙여넣습니다.

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

-   T-sql 편집기에서 마우스 오른쪽 단추를 클릭 하 고 **실행** 을 선택 합니다.
-   GetStudentGradesForCourse 함수는 School 데이터베이스에 추가 됩니다.

 

## <a name="create-a-model"></a>모델 만들기

1.  솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가**를 가리킨 다음 **새 항목** 을 클릭 합니다.
2.  왼쪽 메뉴에서 **데이터** 를 선택 하 고 **템플릿** 창에서 **ADO.NET 엔터티 데이터 모델** 를 선택 합니다.
3.  파일 이름에 TVFModel를 입력 한 다음 **추가** 를 클릭 **합니다.**
4.  Model 콘텐츠 선택 대화 상자에서 **데이터베이스에서 생성**을 선택 하 고 **다음** 을 클릭 합니다.
5.  서버 이름 텍스트 상자에서 **새 연결** 입력 **(localdb)\\mssqllocaldb** 를 클릭 하 여 데이터베이스 이름에 대 한 **School** 를 입력 하 고 **확인을** 클릭 합니다.
6.  데이터베이스 개체 선택 대화 상자의 **테이블** 노드에서 **Person**, **StudentGrade**및 **강의** 테이블을 선택 합니다.
7.   **저장 프로시저 및 함수 노드** 아래에 있는 **GetStudentGradesForCourse** 함수를 선택 하 고 Visual Studio 2012부터 시작 하 여 저장 프로시저 및 함수를 일괄적으로 가져올 수 Entity Designer.
8.   **마침** 클릭
9.  모델 편집을 위한 디자인 화면을 제공 하는 Entity Designer 표시 됩니다.  **데이터베이스 개체 선택** 대화 상자에서 선택한 모든 개체가 모델에 추가 됩니다.
10. 기본적으로 가져온 각 저장 프로시저 또는 함수의 결과 셰이프는 자동으로 엔터티 모델에 새 복합 형식이 됩니다. 그러나 GetStudentGradesForCourse 함수의 결과를 StudentGrade 엔터티에 매핑하려면 디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 모델 브라우저에서 **모델 브라우저** 를 선택한 다음 **함수**가져오기를 선택 하 고 함수 가져오기 편집 대화 상자에서 **GetStudentGradesForCourse** 함수를 두 번 클릭 한 다음  **엔터티** 를 선택 하 고 **StudentGrade** 를 선택 합니다.

## <a name="persist-and-retrieve-data"></a>데이터 유지 및 검색

Main 메서드가 정의 된 파일을 엽니다. Main 함수에 다음 코드를 추가 합니다.

다음 코드에서는 테이블 반환 함수를 사용 하는 쿼리를 작성 하는 방법을 보여 줍니다. 이 쿼리는 관련 강좌 제목 및 등급이 3.5 이상인 관련 학생을 포함 하는 무명 형식으로 결과를 프로젝션 합니다.

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

애플리케이션을 컴파일하고 실행합니다. 프로그램에서는 다음이 출력됩니다.

```console
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a>요약

이 연습에서는 Entity Framework Designer를 사용 하 여 Tvf (테이블 반환 함수)를 매핑하는 방법에 대해 살펴보았습니다. 또한 LINQ 쿼리에서 TVF를 호출 하는 방법을 보여 줍니다.
