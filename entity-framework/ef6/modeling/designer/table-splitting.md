---
title: 디자이너 테이블 분할-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: f5e7532e6c0b473d8ce77cbd11e3e673b0af6cbe
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921786"
---
# <a name="designer-table-splitting"></a>디자이너 테이블 분할
이 연습에서는 Entity Framework Designer (EF Designer)를 사용 하 여 모델을 수정 하 여 여러 엔터티 형식을 단일 테이블에 매핑하는 방법을 보여 줍니다.

테이블 분할을 사용 하는 한 가지 이유는 지연 로드를 사용 하 여 개체를 로드할 때 일부 속성의 로드를 지연 하는 것입니다. 매우 많은 양의 데이터를 포함 하는 속성을 별도의 엔터티로 분리 하 고 필요한 경우에만 로드할 수 있습니다.

다음 그림에서는 EF Designer를 사용할 때 사용 되는 기본 창을 보여 줍니다.

![EF 디자이너](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>전제 조건

이 연습을 완료하려면 다음 사항이 필요합니다.

- 최신 버전의 Visual Studio입니다.
- [School 예제 데이터베이스](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>프로젝트 설정

이 연습에서는 Visual Studio 2012을 사용 합니다.

-   Visual Studio 2012을 엽니다.
-   **파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트**를 클릭합니다.
-   왼쪽 창에서 Visual C\#를 클릭 한 다음 콘솔 응용 프로그램 템플릿을 선택 합니다.
-   프로젝트 이름으로 **표** 를 입력 하 고 **확인**을 클릭 합니다.

## <a name="create-a-model-based-on-the-school-database"></a>School 데이터베이스를 기반으로 모델 만들기

-   솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가**를 가리킨 다음 **새 항목**을 클릭 합니다.
-   왼쪽 메뉴에서 **데이터** 를 선택 하 고 템플릿 창에서 **ADO.NET 엔터티 데이터 모델** 를 선택 합니다.
-   파일 이름에 대해 파일 이름에 대해 파일 이름으로 **.edmx** 를 입력 한 다음 **추가**를 클릭 합니다.
-   Model 콘텐츠 선택 대화 상자에서 **데이터베이스에서 생성**을 선택 하 고 다음을 클릭 **합니다.**
-   새 연결을 클릭 합니다. 연결 속성 대화 상자에서 서버 이름 (예: **(localdb)\\mssqllocaldb**)을 입력 하 고, 인증 방법을 선택 하 고, 데이터베이스 이름으로 **School** 을 입력 한 다음, **확인**을 클릭 합니다.
    데이터 연결 선택 대화 상자가 데이터베이스 연결 설정으로 업데이트 됩니다.
-   데이터베이스 개체 선택 대화 상자에서 **테이블** 노드를 펼침 **Person** 테이블을 선택 합니다. 그러면 지정 된 테이블이 **School** 모델에 추가 됩니다.
-    **마침**을 클릭 합니다.

모델 편집을 위한 디자인 화면을 제공 하는 Entity Designer 표시 됩니다.  **데이터베이스 개체** 선택 대화 상자에서 선택한 모든 개체가 모델에 추가 됩니다.

## <a name="map-two-entities-to-a-single-table"></a>단일 테이블에 두 엔터티 매핑

이 섹션에서는 **Person** 엔터티를 두 엔터티로 분할 한 다음 단일 테이블에 매핑합니다.

> [!NOTE]
> **Person** 엔터티는 많은 양의 데이터를 포함할 수 있는 속성을 포함 하지 않습니다. 이는 예제로만 사용 됩니다.

-   디자인 화면의 빈 영역을 마우스 오른쪽 단추로 클릭 하 고 **새로 추가**를 가리킨 다음 **엔터티**를 클릭 합니다.
     **새 엔터티** 대화 상자가 나타납니다.
-    **엔터티 이름에 대해** PersonID **를 입력 하**고 **키 속성** 이름에 대해을 입력 합니다.  
-    **확인을**클릭 합니다.
-   새 엔터티 형식이 만들어져 디자인 화면에 표시됩니다.
-    ****   **** Person엔터티 형식의 HireDate 속성을 선택 하 고 **ctrl + X** 키를 누릅니다.
-   아웃 **정보** 엔터티를 선택 하 고 **ctrl + V** 키를 누릅니다.
-   **Person** 과 **정보**간의 연결을 만듭니다. 이렇게 하려면 디자인 화면의 빈 영역을 마우스 오른쪽 단추로 클릭 하 고 **새로 추가**를 가리킨 다음 **연결**을 클릭 합니다.
-    **** 연결 추가 대화 상자가 나타납니다. **PersonHireInfo** 이름은 기본적으로 제공 됩니다.
-   관계의 양쪽 끝에 복합성 **1 (1)** 을 지정 합니다.
-   **확인**을 누릅니다.

다음 단계를 수행 하려면 **매핑 정보** 창이 필요 합니다. 이 창이 표시 되지 않으면 디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 **매핑 정보**를 선택 합니다.

-   To **reinfo** 엔터티 유형을 선택 하 고 **매핑 정보**  **&lt;창에서테이블&gt;또는 뷰 추가를**클릭합니다 .
-   **&lt;테이블 또는 뷰&gt;** 필드추가드롭다운목록에서Person을선택 합니다. 목록에는 선택한 엔터티를 매핑할 수 있는 테이블 또는 뷰가 포함 됩니다.
    적절 한 속성은 기본적으로 매핑되어야 합니다.

    ![매핑](~/ef6/media/mapping.png)

-   디자인 화면에서 **PersonHireInfo** 연결을 선택 합니다.
-   디자인 화면에서 연결을 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다.
-   **속성** 창에서 **참조 제약 조건** 속성을 선택 하 고 줄임표 단추를 클릭 합니다.
-   **주** 드롭다운 목록에서 **Person** 을 선택 합니다.
-   **확인**을 누릅니다.

 

## <a name="use-the-model"></a>모델 사용

-   Main 메서드에 다음 코드를 붙여 넣습니다.

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   애플리케이션을 컴파일하고 실행합니다.

다음 T-sql 문은이 응용 프로그램을 실행 한 결과로 **School** 데이터베이스에 대해 실행 되었습니다. 

-   다음 **INSERT** 는 컨텍스트 실행의 결과로 실행 되었습니다. SaveChanges () 및 **Person** **및 개체의 데이터를 결합** 합니다.

    ![Insert](~/ef6/media/insert.png)

-   다음 **SELECT** 는 컨텍스트 실행의 결과로 실행 되었습니다. FirstOrDefault ()를 선택 하 고 **Person** 으로 매핑된 열만 선택 합니다.

    ![1 선택](~/ef6/media/select1.png)

-   다음 **SELECT** 는 탐색 속성 existingperson에 액세스 한 결과로 실행 되었습니다. 강사는 **속성에 매핑된** 열만 선택 합니다.

    ![2 선택](~/ef6/media/select2.png)
