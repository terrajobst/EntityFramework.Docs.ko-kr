---
title: 디자이너 TPH 상속-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415234"
---
# <a name="designer-tph-inheritance"></a>디자이너 TPH 상속
이 단계별 연습에서는 Entity Framework Designer (EF Designer)를 사용 하 여 개념적 모델에서 TPH (계층당 하나의 테이블) 상속을 구현 하는 방법을 보여 줍니다. TPH 상속은 하나의 데이터베이스 테이블을 사용 하 여 상속 계층 구조의 모든 엔터티 형식에 대 한 데이터를 유지 관리 합니다.

이 연습에서는 person 테이블을 Person (기본 형식), 학생 (Person에서 파생) 및 강사 (Person에서 파생 됨)의 세 가지 엔터티 형식으로 매핑합니다. 데이터베이스 (Database First)에서 개념적 모델을 만든 다음 EF Designer를 사용 하 여 TPH 상속을 구현 하도록 모델을 변경 합니다.

Model First를 사용 하 여 TPH 상속에 매핑할 수 있지만 복잡 한 데이터베이스 생성 워크플로를 직접 작성 해야 합니다. 그런 다음이 워크플로를 EF 디자이너의 **데이터베이스 생성 워크플로** 속성에 할당 합니다. 보다 쉬운 방법은 Code First를 사용 하는 것입니다.

## <a name="other-inheritance-options"></a>기타 상속 옵션

형식당 하나의 테이블 (TPT)은 데이터베이스의 개별 테이블이 상속에 참여 하는 엔터티에 매핑되는 또 다른 상속 유형입니다.  EF Designer를 사용 하 여 형식당 하나의 테이블 상속을 매핑하는 방법에 대 한 자세한 내용은 [Ef DESIGNER TPT 상속](~/ef6/modeling/designer/inheritance/tpt.md)을 참조 하세요.

TPC (비추상 형식 상속) 및 혼합 된 상속 모델은 Entity Framework 런타임에서 지원 되지만 EF 디자이너에서는 지원 되지 않습니다. TPC 또는 mixed 상속을 사용 하려는 경우 두 가지 옵션이 있습니다. Code First를 사용 하거나 EDMX 파일을 수동으로 편집 합니다. EDMX 파일을 사용 하도록 선택 하면 매핑 정보 창이 "안전 모드"로 전환 되 고 디자이너를 사용 하 여 매핑을 변경할 수 없습니다.

## <a name="prerequisites"></a>필수 조건

이 연습을 완료하려면 다음 사항이 필요합니다.

- 최신 버전의 Visual Studio입니다.
- [School 예제 데이터베이스](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>프로젝트 설정

-   Visual Studio 2012을 엽니다.
-   **파일-&gt; 새로 만들기-&gt; 프로젝트** 를 선택 합니다.
-   왼쪽 창에서 **Visual C\#** 을 클릭 한 다음 **콘솔** 템플릿을 선택 합니다.
-   이름으로 **Tphdbfirstsample** 을 입력 합니다.
-    **확인**을 선택합니다.

## <a name="create-a-model"></a>모델 만들기

-   솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가-&gt; 새 항목**을 선택 합니다.
-   왼쪽 메뉴에서 **데이터** 를 선택 하 고 템플릿 창에서 **ADO.NET 엔터티 데이터 모델** 를 선택 합니다.
-   파일 이름에 대해 **Tphmodel .edmx** 를 입력 한 다음 **추가**를 클릭 합니다.
-   Model 콘텐츠 선택 대화 상자에서 **데이터베이스에서 생성**을 선택 하 고 **다음**을 클릭 합니다.
-    **새 연결**을 클릭 합니다.
    연결 속성 대화 상자에서 서버 이름 (\\예: **mssqllocaldb**)을 입력 하 고, 인증 방법을 선택 하 고, 데이터베이스 이름으로 **School** 을 입력 한 다음, **확인**을 클릭 합니다.
    데이터 연결 선택 대화 상자가 데이터베이스 연결 설정으로 업데이트 됩니다.
-   데이터베이스 개체 선택 대화 상자의 테이블 노드에서 **Person** 테이블을 선택 합니다.
-    **마침**을 클릭 합니다.

모델 편집을 위한 디자인 화면을 제공 하는 Entity Designer 표시 됩니다. 데이터베이스 개체 선택 대화 상자에서 선택한 모든 개체가 모델에 추가 됩니다.

이는 **Person** 테이블이 데이터베이스에서 표시 되는 방식입니다.

![Person 테이블](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a>계층당 하나의 테이블 상속 구현

 **Person** 테이블에는 "Student" 및 "강사"의 두 값 중 하나를 가질 수 있는 **판별자** 열이 있습니다. 값에 따라 **Person** 테이블은 **Student** 엔터티 또는 **강사** 엔터티에 매핑됩니다.  **Person** 테이블에는 **HireDate** 및 **EnrollmentDate**라는 두 개의 열이 있습니다 .이 열은 한 사람이 동시에 학생 및 강사 일 수 없으므로 **null을 허용** 해야 합니다.

### <a name="add-new-entities"></a>새 엔터티 추가

-   새 엔터티를 추가 합니다.
    이렇게 하려면 Entity Framework Designer 디자인 화면의 빈 공간을 마우스 오른쪽 단추로 클릭 하 고 **추가&gt;엔터티**를 선택 합니다.
-    **엔터티 이름** 에 대해 **강사** 를 입력 하 고 **기본 형식의**드롭다운 목록에서 **Person** 을 선택 합니다.
-    **확인을**클릭 합니다.
-   다른 새 엔터티를 추가 합니다.  **엔터티 이름** 에 **Student** 를 입력 하 고 **기본 형식의**드롭다운 목록에서 **Person** 을 선택 합니다.

두 개의 새 엔터티 형식이 디자인 화면에 추가 되었습니다. 새 엔터티 형식에서 **Person** 엔터티 형식으로 가리키는 화살표입니다. 이는 **Person** 새 엔터티 형식에 대 한 기본 형식 임을 나타냅니다.

-    **Person** 엔터티의 **HireDate** 속성을 마우스 오른쪽 단추로 클릭 합니다.  **잘라내기** 를 선택 합니다 (또는 Ctrl + X 키 사용).
-    **강사** 엔터티를 마우스 오른쪽 단추로 클릭 하 고 **붙여넣기** 를 선택 합니다 (또는 Ctrl + V 키 사용).
-    **HireDate** 속성을 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다.
-    **속성** 창에서 **Nullable** 속성을 **false**로 설정 합니다.
-    **Person** 엔터티의 **EnrollmentDate** 속성을 마우스 오른쪽 단추로 클릭 합니다.  **잘라내기** 를 선택 합니다 (또는 Ctrl + X 키 사용).
-    **학생** 엔터티를 마우스 오른쪽 단추로 클릭 하 고 **붙여넣기를 선택 합니다 (또는 Ctrl + V 키 사용).**
-    **EnrollmentDate** 속성을 선택 하 고 **Nullable** 속성을 **false**로 설정 합니다.
-    **Person** 엔터티 형식을 선택 합니다.  **속성** 창에서 **추상** 속성을 **true**로 설정 합니다.
-   **Person**에서 **판별자** 속성을 삭제 합니다. 삭제 해야 하는 이유는 다음 섹션에 설명 되어 있습니다.

### <a name="map-the-entities"></a>엔터티 매핑

-    **강사** 를 마우스 오른쪽 단추로 클릭 하 고 **테이블 매핑을 선택 합니다.**
    매핑 정보 창에서 강사가 엔터티를 선택 합니다.
-    **매핑 정보** 창에서 ** 테이블 또는 뷰&gt; 추가&lt;** 클릭 합니다.
     **테이블 또는 뷰&gt;**  필드를 추가&lt;선택한 엔터티를 매핑할 수 있는 테이블이 나 뷰의 드롭다운 목록이 됩니다.
-   드롭다운 목록에서 **Person** 를 선택 합니다.
-    **매핑 정보** 창은 기본 열 매핑과 조건을 추가 하는 옵션으로 업데이트 됩니다.
-    **&lt;&gt;조건 추가 **를 클릭 합니다.
     **조건을&lt;&gt;조건을 추가** 하면 조건을 설정할 수 있는 열의 드롭다운 목록이 필드가 됩니다.
-   드롭다운 목록에서 **판별자** 를 선택 합니다.
-    **매핑 정보** 창의 **연산자** 열에서 드롭다운 목록에서 =를 선택 합니다.
-    **값/속성** 열에 **강사**를 입력 합니다. 최종 결과는 다음과 같습니다.

    ![매핑 정보](~/ef6/media/mappingdetails2.png)

-    **학생** 엔터티 형식에 대해 이러한 단계를 반복 하지만 조건이 **학생** 값과 동일 하 게 만듭니다.  
    ***판별자** 속성을 제거 하려는 이유는 테이블 열을 두 번 이상 매핑할 수 없기 때문입니다. 이 열은 조건부 매핑에 사용 되므로 속성 매핑에도 사용할 수 없습니다. 조건에서 **Is null** 을 사용 하는 경우에만 사용할 수 있는 유일한 방법은 이 고, 그렇지 않으면 **null** 비교 인 경우입니다.*

이제 계층당 하나의 테이블 상속이 구현되었습니다.

![최종 TPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a>모델 사용

**Main** 메서드가 정의 된 **Program.cs** 파일을 엽니다. **Main** 함수에 다음 코드를 붙여 넣습니다. 이 코드는 세 개의 쿼리를 실행 합니다. 첫 번째 쿼리는 모든 **Person** 개체를 다시 가져옵니다. 두 번째 쿼리에서는 **OfType** 메서드를 사용 하 여 **강사** 개체를 반환 합니다. 세 번째 쿼리는 **OfType** 메서드를 사용 하 여 **Student** 개체를 반환 합니다.

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
