---
title: 디자이너 테이블 분할 EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: 8b0ca6778a06ed43b1365d2e5969ff15948f8004
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490699"
---
# <a name="designer-table-splitting"></a>디자이너의 테이블 분
이 연습에서는 Entity Framework Designer (EF 디자이너)를 사용 하 여 모델을 수정 하 여 여러 엔터티 형식을 단일 테이블에 매핑하는 방법을 보여 줍니다.

테이블 분할을 사용 하려는 이유 중 하나는 지연에 개체를 로드 하려면 로드를 사용 하는 경우 일부 속성을 로드 하는 지연 되 합니다. 별도 엔터티로 매우 많은 양의 데이터를 포함 하는 수만 필요할 때 로드할 속성을 구분할 수 있습니다.

다음 이미지에서는 EF 디자이너를 사용 하 여 작업할 때 사용 되는 기본 windows를 보여 줍니다.

![EF 디자이너](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>전제 조건

이 연습을 완료하려면 다음 사항이 필요합니다.

- Visual Studio의 최신 버전입니다.
- 합니다 [School 샘플 데이터베이스](~/ef6/resources/school-database.md)합니다.

## <a name="set-up-the-project"></a>프로젝트 설정

이 연습에서는 Visual Studio 2012 사용 중입니다.

-   Visual Studio 2012를 엽니다.
-   **파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트**를 클릭합니다.
-   왼쪽된 창에서 Visual C 클릭\#, 콘솔 응용 프로그램 템플릿을 선택 합니다.
-   입력 **TableSplittingSample** 고 프로젝트의 이름으로 **확인**합니다.

## <a name="create-a-model-based-on-the-school-database"></a>School 데이터베이스를 기반으로 모델 만들기

-   솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 **추가**를 클릭 하 고 **새 항목**합니다.
-   선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 템플릿 창에서.
-   입력 **TableSplittingModel.edmx** 파일 이름 및 클릭 **추가**합니다.
-   Model 콘텐츠 선택 대화 상자에서 선택 **데이터베이스에서 생성**를 클릭 하 고 **다음입니다.**
-   새 연결을 클릭 합니다. 연결 속성 대화 상자에서 서버 이름을 입력 합니다 (예를 들어 **(localdb)\\mssqllocaldb**)을 선택 인증 방법, 형식 **학교** 데이터베이스 이름에 대 한 다음 클릭 **확인**합니다.
    데이터 연결 선택 대화 상자는 데이터베이스 연결 설정으로 업데이트 됩니다.
-   데이터베이스 개체 선택 대화 상자에서 펼침를 **테이블** 노드와 확인 합니다 **Person** 테이블. 이 지정 된 테이블을 추가 합니다 **학교** 모델입니다.
-   **마침**을 클릭합니다.

모델 편집을 위해 디자인 화면을 제공 하는 Entity Designer에 표시 됩니다. 선택한 모든 개체를 **데이터베이스 개체 선택** 대화 상자는 모델에 추가 됩니다.

## <a name="map-two-entities-to-a-single-table"></a>단일 테이블에 두 개의 엔터티 매핑

분할이 섹션의 **Person** 두 엔터티를 엔터티 단일 테이블에 매핑하면 합니다.

> [!NOTE]
> 합니다 **Person** 엔터티에 많은 양의 데이터를 포함할 수 있는 모든 속성을 포함 되지 않는; 예로 사용 됩니다.

-   디자인 화면의 빈 영역을 마우스 오른쪽 **새로 추가**, 클릭 **엔터티**합니다.
    합니다 **새 엔터티** 대화 상자가 나타납니다.
-   형식 **HireInfo** 에 대 한 합니다 **엔터티 이름** 및 **PersonID** 에 대 한 합니다 **키 속성** 이름입니다.
-   **확인**을 클릭합니다.
-   새 엔터티 형식이 만들어져 디자인 화면에 표시됩니다.
-   선택는 **HireDate** 의 속성을 **사용자** 엔터티 형식 및 키를 누릅니다 **Ctrl + X** 키.
-   선택 된 **HireInfo** 엔터티와 키를 눌러 **Ctrl + V** 키입니다.
-   간에 관계를 만들려면 **Person** 하 고 **HireInfo**합니다. 이렇게 하려면 디자인 화면의 빈 영역을 마우스 오른쪽 단추로 클릭, 가리킨 **새로 추가**, 클릭 **연결**합니다.
-   합니다 **연결 추가** 대화 상자가 나타납니다. 합니다 **PersonHireInfo** 기본적으로 이름이 지정 됩니다.
-   복합성을 지정 **1(One)** 관계의 양쪽 끝에 있습니다.
-   키를 눌러 **확인**합니다.

다음 단계에 필요 합니다 **매핑 정보** 창입니다. 이 창에 표시 되지 않으면 디자인 화면을 마우스 오른쪽 단추로 **매핑 정보**합니다.

-   선택 합니다 **HireInfo** 엔터티 형식 및 클릭 **&lt;테이블 또는 뷰 추가&gt;** 에 **매핑 정보** 창.
-   선택 **Person** 에서 **&lt;테이블이 나 뷰 추가&gt;** 필드 드롭 다운 목록. 목록 테이블이 또는 선택한 엔터티에 매핑할 수 있는 뷰.
    기본적으로 적절 한 속성을 매핑해야 합니다.

    ![매핑](~/ef6/media/mapping.png)

-   선택 된 **PersonHireInfo** 디자인 화면에 연결 합니다.
-   디자인 화면에서 연결을 마우스 오른쪽 단추로 클릭 **속성**합니다.
-   에 **속성** 창에서를 **참조 제약 조건** 속성 줄임표 단추를 클릭 합니다.
-   선택 **Person** 에서 합니다 **주체** 드롭 다운 목록.
-   키를 눌러 **확인**합니다.

 

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
-   응용 프로그램을 컴파일하고 실행합니다.

다음 T-SQL 문을 대해 실행 된 합니다 **학교** 이 응용 프로그램을 실행 한 결과로 데이터베이스입니다. 

-   다음 **삽입** 컨텍스트를 실행 한 결과로 실행 되었습니다. savechanges () 및 결합 하 여 데이터를 **Person** 하 고 **HireInfo** 엔터티

    ![Insert](~/ef6/media/insert.png)

-   다음 **선택** 컨텍스트를 실행 한 결과로 실행 되었습니다. 에 매핑된 열만 선택 하 고 People.FirstOrDefault() **사람**

    ![1 선택](~/ef6/media/select1.png)

-   다음 **선택** 탐색 속성 existingPerson.Instructor 액세스의 결과로 실행 되 고에 매핑된 열만 선택 **HireInfo**

    ![2를 선택 합니다.](~/ef6/media/select2.png)
