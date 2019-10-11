---
title: 디자이너 쿼리 저장 프로시저-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 2e0092b526278597e8477d47eeb642598647bb91
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182478"
---
# <a name="designer-query-stored-procedures"></a>디자이너 쿼리 저장 프로시저
이 단계별 연습에서는 Entity Framework Designer (EF Designer)를 사용 하 여 저장 프로시저를 모델로 가져온 다음 가져온 저장 프로시저를 호출 하 여 결과를 검색 하는 방법을 보여 줍니다. 

Code First은 저장 프로시저나 함수에 대 한 매핑을 지원 하지 않습니다. 그러나 SqlQuery 메서드를 사용 하 여 저장 프로시저 또는 함수를 호출할 수 있습니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a>사전 요구 사항

이 연습을 완료하려면 다음 사항이 필요합니다.

- 최신 버전의 Visual Studio입니다.
- [School 예제 데이터베이스](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>프로젝트 설정

-   Visual Studio 2012을 엽니다.
-   **파일-&gt; 새 &gt; 프로젝트를** 선택 합니다.
-   왼쪽 창에서 **Visual C @ no__t-1**을 클릭 한 다음 **콘솔** 템플릿을 선택 합니다.
-   이름으로 **Efwithsprocssample** 을 입력 합니다.
-    **확인**을 선택합니다.

## <a name="create-a-model"></a>모델 만들기

-   솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **추가-&gt; 새 항목**을 선택 합니다.
-   왼쪽 메뉴에서 **데이터** 를 선택 하 고 템플릿 창에서 **ADO.NET 엔터티 데이터 모델** 를 선택 합니다.
-   파일 이름에 대해 **Efwithsprocsmodel .edmx** 를 입력 한 다음 **추가**를 클릭 합니다.
-   Model 콘텐츠 선택 대화 상자에서 **데이터베이스에서 생성**을 선택 하 고 **다음**을 클릭 합니다.
-    **새 연결**을 클릭 합니다.  
    연결 속성 대화 상자에서 서버 이름 (@no__t 예: **1mssqllocaldb**)을 입력 하 고, 인증 방법을 선택 하 고, 데이터베이스 이름으로 **School** for 입력 한 다음, **확인**을 클릭 합니다.  
    데이터 연결 선택 대화 상자가 데이터베이스 연결 설정으로 업데이트 됩니다.
-   데이터베이스 개체 선택 대화 상자에서 테이블  checkbox 확인란을 선택 하 여 모든 테이블을 **선택 합니다.**  
    또한 **저장 프로시저 및 함수** 노드에서 다음 저장 프로시저를 선택 합니다. **GetStudentGrades** 및 **GetDepartmentName**. 

    ![가져오기](~/ef6/media/import.jpg)

    *Visual Studio 2012부터 EF Designer는 저장 프로시저의 대량 가져오기를 지원 합니다. **엔터티 모델에 선택한 저장 프로시저 및 함수 가져오기** 는 기본적으로 선택 되어 있습니다.*
-    **마침**을 클릭 합니다.

기본적으로 두 개 이상의 열을 반환 하는 가져온 저장 프로시저 또는 함수의 결과 셰이프는 자동으로 새 복합 형식이 됩니다. 이 예제에서는 **GetStudentGrades** 함수의 결과를 **StudentGrade** 엔터티에 매핑하고 **GetDepartmentName** 의 결과를 **none** 으로 매핑해야 합니다 (기본값**없음** ).

Function import에서 엔터티 형식을 반환 하려면 해당 저장 프로시저에서 반환 된 열이 반환 된 엔터티 형식의 스칼라 속성과 정확 하 게 일치 해야 합니다. Function import는 단순 형식, 복합 형식 또는 값이 없는 컬렉션을 반환할 수도 있습니다.

-   디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 **모델 브라우저**를 선택 합니다.
-   **모델 브라우저**에서 **Function Imports**를 선택 하 고 **GetStudentGrades** 함수를 두 번 클릭 합니다.
-   함수 가져오기 편집 대화 상자에서 **엔터티** 을 선택 하 고 **StudentGrade**를 선택 합니다.  
    **함수** 가져오기 대화 상자의 맨 위에 있는 **함수 가져오기** 구성 가능 확인란을 선택 하면 구성 가능한 함수에 매핑할 수 있습니다. @no__t 이 확인란을 선택 하면 구성 가능한 함수 (테이블 반환 함수)만 **저장 프로시저/함수 이름** 드롭다운 목록에 표시 됩니다. 이 확인란을 선택 하지 않으면 구성할 수 없는 함수만 목록에 표시 됩니다. *

## <a name="use-the-model"></a>모델 사용

**Main** 메서드가 정의 된 **Program.cs** 파일을 엽니다. Main 함수에 다음 코드를 추가 합니다.

이 코드는 두 개의 저장 프로시저를 호출 합니다. **GetStudentGrades** (지정 된 *StudentId*에 대 한 **StudentGrades** 를 반환) 및 **GetDepartmentName** (출력 매개 변수의 부서 이름을 반환)  

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

애플리케이션을 컴파일하고 실행합니다. 프로그램에서는 다음이 출력됩니다.

```console
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a>출력 매개 변수
-----------------

출력 매개 변수를 사용 하는 경우 결과를 완전히 읽을 때까지 해당 값을 사용할 수 없습니다. 이는 DbDataReader의 기본 동작 때문입니다. 자세한 내용은 [DataReader를 사용 하 여 데이터 검색](https://go.microsoft.com/fwlink/?LinkID=398589) 을 참조 하세요.
