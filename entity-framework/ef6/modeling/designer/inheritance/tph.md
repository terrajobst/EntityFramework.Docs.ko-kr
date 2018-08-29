---
title: 디자이너 TPH 상속-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 9a546f6450b5aa3b03c062d1ab2c6f9257ba8292
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995006"
---
# <a name="designer-tph-inheritance"></a>디자이너 TPH 상속
이 단계별 연습에서는 Entity Framework Designer (EF 디자이너)를 사용 하 여 개념적 모델에서 계층당 하나의 테이블 (TPH) 상속을 구현 하는 방법을 보여 줍니다. TPH 상속 상속 계층 구조의 엔터티 형식의 모든 데이터를 유지 하기 위해 하나의 데이터베이스 테이블을 사용 합니다.

이 연습에서는 Person 테이블에 매핑합니다 세 가지 엔터티 형식: Person (기본 형식), (Person에서 파생), 학생과 강사 (Person에서 파생 됨). 개념적 모델 (Database First) 데이터베이스에서 만들어 EF 디자이너를 사용 하 여 TPH 상속을 구현 하는 모델을 변경 합니다.

Model First를 사용 하 여 TPH 상속을 매핑할 수 있지만 복잡 하는 고유한 데이터베이스 생성 워크플로 작성 해야 합니다. 이 워크플로를 할당 한 다음 합니다 **Database Generation Workflow** EF 디자이너의 속성입니다. 쉬운 대 안으로 Code First를 사용 하는 것입니다.

## <a name="other-inheritance-options"></a>다른 상속 옵션

테이블 형식당 TPT ()는 다른 유형의 상속 상속에 참여 하는 엔터티를 별도 데이터베이스의 테이블에에서 매핑됩니다.  EF 디자이너를 사용 하 여 형식당 하나의 테이블 상속을 매핑하는 방법에 대 한 정보를 참조 하세요 [EF 디자이너 TPT 상속](~/ef6/modeling/designer/inheritance/tpt.md)합니다.

구체적 형식당 테이블 형식 상속 TPC () 및 혼합 된 상속 모델에는 Entity Framework 런타임에서 지 있지만 EF 디자이너에서 지원 되지 않습니다. TPC 또는 혼합된 상속을 사용 하려는 경우 두 가지 옵션이 있습니다: Code First를 사용 하 여 또는 EDMX 파일을 수동으로 편집 합니다. EDMX 파일을 사용 하려는 경우 매핑 세부 정보 창 "안전 모드로" 넣 및 디자이너를 사용 하 여 매핑을 변경할 수 없습니다.

## <a name="prerequisites"></a>전제 조건

이 연습을 완료하려면 다음 사항이 필요합니다.

- Visual Studio의 최신 버전입니다.
- 합니다 [School 샘플 데이터베이스](~/ef6/resources/school-database.md)합니다.

## <a name="set-up-the-project"></a>프로젝트 설정

-   Visual Studio 2012를 엽니다.
-   선택 **파일-&gt; 새로운 기능-&gt; 프로젝트**
-   왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔** 템플릿.
-   입력 **TPHDBFirstSample** 이름으로 합니다.
-   **확인**을 선택합니다.

## <a name="create-a-model"></a>모델 만들기

-   솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 누르고 **추가-&gt; 새 항목**합니다.
-   선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 템플릿 창에서.
-   입력 **TPHModel.edmx** 파일 이름 및 클릭 **추가**합니다.
-   Model 콘텐츠 선택 대화 상자에서 선택 **데이터베이스에서 생성**를 클릭 하 고 **다음**합니다.
-   클릭 **새 연결**합니다.
    연결 속성 대화 상자에서 서버 이름을 입력 합니다 (예를 들어 **(localdb)\\mssqllocaldb**)을 선택 인증 방법, 형식 **학교** 데이터베이스 이름에 대 한 다음 클릭 **확인**합니다.
    데이터 연결 선택 대화 상자는 데이터베이스 연결 설정으로 업데이트 됩니다.
-   데이터베이스 개체 선택 대화 상자의 테이블 노드를 선택 합니다 **Person** 테이블입니다.
-   **마침**을 클릭합니다.

모델 편집을 위해 디자인 화면을 제공 하는 Entity Designer에 표시 됩니다. 데이터베이스 개체 선택 대화 상자에서 선택한 모든 개체는 모델에 추가 됩니다.

즉 하는 방법을 **Person** 테이블 데이터베이스에서 검색 합니다.

![PersonTable](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a>계층당 하나의 테이블 상속 구현

**Person** 테이블에는 **판별자** 열 두 값 중 하나일 수 있습니다: "Student" 및 "Instructor"입니다. 값에 따라 합니다 **Person** 테이블에 매핑됩니다 합니다 **학생** 엔터티 또는 **강사** 엔터티. **Person** 테이블에는 또한 두 열에 **HireDate** 및 **EnrollmentDate**, 이어야 합니다 **nullable** 사람 수 없기 때문에 학생과 강사 동시에 (적어도이 연습의).

### <a name="add-new-entities"></a>새 엔터티를 추가 합니다.

-   새 엔터티를 추가 합니다.
    이 위해 Entity Framework Designer의 디자인 화면의 빈 영역을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가-&gt;엔터티**합니다.
-   형식 **강사** 에 대 한 합니다 **엔터티 이름** 선택한 **Person** 드롭 다운 목록에서를 **기본 형식**합니다.
-   **확인**을 클릭합니다.
-   다른 새 엔터티를 추가 합니다. 형식 **학생** 에 대 한 합니다 **엔터티 이름** 선택한 **Person** 드롭 다운 목록에서를 **기본 형식**합니다.

두 개의 새 엔터티 형식이 디자인 화면에 추가 되었습니다. 화살표는 새 엔터티 형식에서 요소를 **Person** 엔터티 형식에 있음을 나타냅니다 **Person** 새 엔터티 형식에 대 한 기본 형식입니다.

-   마우스 오른쪽 단추로 클릭 합니다 **HireDate** 의 속성을 **사용자** 엔터티. 선택 **잘라내기** (또는 Ctrl + X 키 사용).
-   마우스 오른쪽 단추로 클릭 합니다 **강사** 엔터티 및 선택 **붙여넣기** (또는 Ctrl + V 키를 사용 하 여).
-   마우스 오른쪽 단추로 클릭 합니다 **HireDate** 속성과 선택 **속성**합니다.
-   에 **속성** 창에서 설정 합니다 **Nullable** 속성을 **false**합니다.
-   마우스 오른쪽 단추로 클릭 합니다 **EnrollmentDate** 의 속성을 **사용자** 엔터티. 선택 **잘라내기** (또는 Ctrl + X 키 사용).
-   마우스 오른쪽 단추로 클릭 합니다 **학생** 엔터티 및 선택 **붙여넣기 (또는 Ctrl + V를 사용 하 여 키).**
-   선택 된 **EnrollmentDate** 속성 집합과 합니다 **Nullable** 속성을 **false**합니다.
-   선택 된 **Person** 엔터티 형식입니다. 에 **속성** 창에서 해당 **추상** 속성을 **true**합니다.
-   삭제 된 **판별자** 속성을 **Person**합니다. 삭제 해야 하는 이유는 다음 섹션에 설명 되어 있습니다.

### <a name="map-the-entities"></a>엔터티 매핑

-   마우스 오른쪽 단추로 클릭 합니다 **강사** 선택한 **테이블 매핑.**
    Instructor 엔터티 매핑 정보 창에서 선택 됩니다.
-   클릭 **&lt;테이블이 나 뷰 추가&gt;** 에 **매핑 정보** 창입니다.
    합니다 **&lt;테이블이 나 뷰 추가&gt;** 필드 테이블의 드롭다운 목록이 됩니다 또는 선택한 엔터티에 매핑할 수 있는 뷰.
-   선택 **Person** 드롭 다운 목록에서.
-   합니다 **매핑 정보** 창이 기본 열 매핑과 조건 추가 옵션을 사용 하 여 업데이트 됩니다.
-   클릭할  **&lt;조건 추가&gt;** 합니다.
    합니다 **&lt;조건 추가&gt;** 필드 조건을 설정할 수 있는 열의 드롭다운 목록이 됩니다.
-   선택 **판별자** 드롭 다운 목록에서.
-   에 **연산자** 열의 합니다 **매핑 정보** 창에서 드롭 다운 목록에서 =.
-   에 **값/속성** 열, 형식 **강사**합니다. 최종 결과 다음과 같습니다.

    ![MappingDetails2](~/ef6/media/mappingdetails2.png)

-   에 대 한 이러한 단계를 반복 합니다 **학생** 엔터티 형식 이지만 같음 조건 확인 **학생** 값입니다.  
    *제거 하려는 이유는 **판별자** 속성은 테이블 열을 두 번 매핑할 수 없습니다 때문입니다. 이 열에 사용할 조건부 매핑 속성으로 매핑하기 위한 사용할 수 없습니다. 조건을 사용 하는 경우 둘 다 사용할 수 있습니다 하는 유일한 방법은 **Is Null** 하거나 **Is Not Null** 비교 합니다.*

이제 계층당 하나의 테이블 상속이 구현되었습니다.

![FinalTPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a>모델 사용

열기는 **Program.cs** 파일을 **Main** 메서드 정의 됩니다. 다음 코드를 붙여 합니다 **Main** 함수입니다. 코드는 세 가지 쿼리를 실행 합니다. 첫 번째 쿼리를 가져오고 모든 **Person** 개체입니다. 두 번째 쿼리에서 **OfType** 를 반환 하는 방법 **강사** 개체입니다. 세 번째 쿼리에서 **OfType** 반환 하는 방법 **학생** 개체입니다.

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
