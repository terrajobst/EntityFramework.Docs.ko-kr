---
title: 저장 프로시저-EF6 디자이너 쿼리
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
caps.latest.revision: 3
ms.openlocfilehash: a08c1afc02266b35372a49fca1e829963e4785b2
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122309"
---
# <a name="designer-query-stored-procedures"></a>디자이너 쿼리 저장 프로시저
이 단계별 연습에는 저장된 프로시저를 가져오려는 Entity Framework Designer (EF 디자이너)를 사용 하 여 모델에 다음 결과 검색 하 고 가져온된 저장된 프로시저를 호출 하는 방법을 보여 줍니다. 

Code First에서 지원 하지 않음을 저장 프로시저나 함수에 대 한 매핑 참고 합니다. 그러나 System.Data.Entity.DbSet.SqlQuery 메서드를 사용 하 여 저장된 프로시저 또는 함수를 호출할 수 있습니다. 예를 들어:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a>전제 조건

이 연습을 완료하려면 다음 사항이 필요합니다.

- Visual Studio의 최신 버전입니다.
- 합니다 [School 샘플 데이터베이스](~/ef6/resources/school-database.md)합니다.

## <a name="set-up-the-project"></a>프로젝트 설정

-   Visual Studio 2012를 엽니다.
-   선택 **파일-&gt; 새로운 기능-&gt; 프로젝트**
-   왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔** 템플릿.
-   입력 **EFwithSProcsSample** 이름으로 합니다.
-   **확인**을 선택합니다.

## <a name="create-a-model"></a>모델 만들기

-   솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 누르고 **추가-&gt; 새 항목**합니다.
-   선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 템플릿 창에서.
-   입력 **EFwithSProcsModel.edmx** 파일 이름 및 클릭 **추가**합니다.
-   Model 콘텐츠 선택 대화 상자에서 선택 **데이터베이스에서 생성**를 클릭 하 고 **다음**합니다.
-   클릭 **새 연결**합니다.  
    연결 속성 대화 상자에서 서버 이름을 입력 합니다 (예를 들어 **(localdb)\\mssqllocaldb**)을 선택 인증 방법, 형식 **학교** 데이터베이스 이름에 대 한 다음 클릭 **확인**합니다.  
    데이터 연결 선택 대화 상자는 데이터베이스 연결 설정으로 업데이트 됩니다.
-   데이터베이스 개체 선택 대화 상자에서 확인 합니다 **테이블** 모든 테이블을 선택 하려면 확인란을 선택 합니다.  
    또한 아래에 있는 다음 저장된 프로시저를 선택 합니다 **저장 프로시저 및 함수** 노드: **GetStudentGrades** 하 고 **GetDepartmentName**합니다. 

    ![가져오기](~/ef6/media/import.jpg)

    *Visual Studio 2012를 사용한 EF 디자이너 시작 저장된 프로시저의 대량 가져오기를 지원 합니다. 합니다 **theentity 모델로 저장된 프로시저 및 함수를 선택 하는 가져오기** 기본적으로 선택 됩니다.*
-   **마침**을 클릭합니다.

기본적으로 각 가져온된 저장된 프로시저 또는 함수의 둘 이상의 열을 반환 하는 결과 도형은 새 복합 형식을 자동으로 있게 됩니다. 이 예제에서는 매핑할 결과 **GetStudentGrades** 함수를 합니다 **StudentGrade** 엔터티 및 결과 **GetDepartmentName** 에**none** (**none** 기본값).

엔터티 형식을 반환 하는 함수 가져오기에 대 한 해당 저장된 프로시저에서 반환 되는 열 반환 된 엔터티 형식의 스칼라 속성을 정확히 일치 해야 합니다. Function import는 단순 형식, 복합 형식 또는 값의 컬렉션을 반환할 수도 있습니다.

-   디자인 화면을 마우스 오른쪽 단추로 클릭 **Model 브라우저**합니다.
-   **Model 브라우저**를 선택 **Function Imports**를 두 번 클릭 하 고는 **GetStudentGrades** 함수.
-   Function Import 편집 대화 상자에서 선택 **엔터티** 선택한 **StudentGrade**합니다.  
    ***Function Import는 구성이 가능 하므로** 맨 위에 있는 확인란을 선택 합니다 **Function Imports** 대화 상자를 사용 하면 구성 가능한 함수에 매핑할 수입니다. 이 상자를 선택 하는 경우에 구성 가능 함수 (테이블 반환 함수)에 표시 됩니다는 **저장 프로시저 / 함수 이름** 드롭 다운 목록. 이 상자를 선택 하지 않을 경우 목록에서 구성 가능 하지 않음 함수만 표시 됩니다.*

## <a name="use-the-model"></a>모델 사용

열기는 **Program.cs** 파일을 **Main** 메서드 정의 됩니다. 다음 코드를 Main 함수에 추가 합니다.

두 개의 저장된 프로시저를 호출 하는 코드: **GetStudentGrades** (반환 **StudentGrades** 지정 된 *StudentId*) 및 **GetDepartmentName** (출력 매개 변수에서 부서 이름을 반환).  

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

응용 프로그램을 컴파일하고 실행합니다. 프로그램에서는 다음이 출력됩니다.

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a>출력 매개 변수
-----------------

출력 매개 변수를 사용 하는 경우 결과 완전히 읽을 때까지 해당 값은 사용할 수 없습니다. 내용은 DbDataReader의 기본 동작으로 인해 이것이 [DataReader를 사용 하 여 데이터 검색](http://go.microsoft.com/fwlink/?LinkID=398589) 대 한 자세한 내용은 합니다.
