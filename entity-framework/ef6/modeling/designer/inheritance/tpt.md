---
title: 디자이너 TPT 상속-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489455"
---
# <a name="designer-tpt-inheritance"></a>디자이너 TPT 상속
이 단계별 연습에서는 Entity Framework Designer (EF 디자이너)를 사용 하 여 모델에서 형식당 하나의 테이블 (TPT) 상속을 구현 하는 방법을 보여 줍니다. 형식당 하나의 테이블 상속에서는 데이터베이스의 별도 테이블을 사용하여 상속 계층 구조에 있는 각 형식의 상속되지 않는 속성 및 키 속성 데이터를 유지합니다.

이 연습에 매핑하여를 **과정** (기본 형식) **OnlineCourse** (과정에서 파생), 및 **OnsiteCourse** (에서 파생 되 **과정**) 같은 이름의 테이블에 엔터티입니다. 모델을 데이터베이스에서 만들어 TPT 상속을 구현 하는 모델을 변경 합니다.

또한 Model First를 사용 하 여 시작 하 고 모델에서 데이터베이스를 생성할 수 있습니다. EF 디자이너에서는 기본적으로 TPT 전략을 사용 하 고 있으므로 모든 상속 모델에는 별도 테이블에 매핑됩니다.

## <a name="other-inheritance-options"></a>다른 상속 옵션

테이블-당-TPH (계층)는 다른 유형의 상속 계층 구조의 엔터티 형식의 모든 데이터를 유지 하는 하나의 데이터베이스 테이블을 사용 하는 상속 합니다.  Entity Designer를 사용 하 여 계층당 하나의 테이블 상속을 매핑하는 방법에 대 한 정보를 참조 하세요 [EF 디자이너 TPH 상속](~/ef6/modeling/designer/inheritance/tph.md)합니다. 

Note는, 구체적 형식당 테이블 형식 상속 TPC () 및 혼합 된 상속 모델은 Entity Framework 런타임에서 지 원하는 있지만 EF 디자이너에서 지원 되지 않습니다. TPC 또는 혼합된 상속을 사용 하려는 경우 두 가지 옵션이 있습니다: Code First를 사용 하 여 또는 EDMX 파일을 수동으로 편집 합니다. EDMX 파일을 사용 하려는 경우 매핑 세부 정보 창 "안전 모드로" 넣 및 디자이너를 사용 하 여 매핑을 변경할 수 없습니다.

## <a name="prerequisites"></a>전제 조건

이 연습을 완료하려면 다음 사항이 필요합니다.

- Visual Studio의 최신 버전입니다.
- 합니다 [School 샘플 데이터베이스](~/ef6/resources/school-database.md)합니다.

## <a name="set-up-the-project"></a>프로젝트 설정

-   Visual Studio 2012를 엽니다.
-   선택 **파일-&gt; 새로운 기능-&gt; 프로젝트**
-   왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔** 템플릿.
-   입력 **TPTDBFirstSample** 이름으로 합니다.
-   **확인**을 선택합니다.

## <a name="create-a-model"></a>모델 만들기

-   솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 누르고 **추가-&gt; 새 항목**합니다.
-   선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 템플릿 창에서.
-   입력 **TPTModel.edmx** 파일 이름 및 클릭 **추가**합니다.
-   Model 콘텐츠 선택 대화 상자에서 선택 * * 생성에서 데이터베이스 * *을 클릭 한 다음 **다음**합니다.
-   클릭 **새 연결**합니다.
    연결 속성 대화 상자에서 서버 이름을 입력 합니다 (예를 들어 **(localdb)\\mssqllocaldb**)을 선택 인증 방법, 형식 **학교** 데이터베이스 이름에 대 한 다음 클릭 **확인**합니다.
    데이터 연결 선택 대화 상자는 데이터베이스 연결 설정으로 업데이트 됩니다.
-   데이터베이스 개체 선택 대화 상자의 테이블 노드를 선택 합니다 **부서**를 **강의와 OnlineCourse를 OnsiteCourse** 테이블입니다.
-   **마침**을 클릭합니다.

모델 편집을 위해 디자인 화면을 제공 하는 Entity Designer에 표시 됩니다. 데이터베이스 개체 선택 대화 상자에서 선택한 모든 개체는 모델에 추가 됩니다.

## <a name="implement-table-per-type-inheritance"></a>형식당 하나의 테이블 상속 구현

-   디자인 화면에서 마우스 오른쪽 단추로 클릭 합니다 **OnlineCourse** 엔터티 형식 및 선택 **속성**합니다.
-   에 **속성** Base Type 속성을 설정 하는 창 **코스**합니다.
-   마우스 오른쪽 단추로 클릭 합니다 **OnsiteCourse** 엔터티 형식 및 선택 **속성**합니다.
-   에 **속성** Base Type 속성을 설정 하는 창 **코스**합니다.
-   사이의 연결 (선)를 마우스 오른쪽 단추로 클릭 합니다 **OnlineCourse** 하 고 **과정** 엔터티 형식입니다.
    선택 **모델에서 삭제**합니다.
-   간의 연결을 마우스 오른쪽 단추로 클릭 합니다 **OnsiteCourse** 하 고 **과정** 엔터티 형식입니다.
    선택 **모델에서 삭제**합니다.

이제 삭제 합니다 **CourseID** 속성을 **OnlineCourse** 하 고 **OnsiteCourse** 이러한 클래스를 상속 하기 때문에 **CourseID** 에서 합니다 **코스** 기본 형식입니다.

-   마우스 오른쪽 단추로 클릭 합니다 **CourseID** 의 속성을 **OnlineCourse** 엔터티 형식을 선택한 후 **모델에서 삭제**합니다.
-   마우스 오른쪽 단추로 클릭 합니다 **CourseID** 의 속성을 **OnsiteCourse** 엔터티 형식을 선택한 후 **모델에서 삭제**
-   이제 형식당 하나의 테이블 상속이 구현되었습니다.

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a>모델 사용

열기는 **Program.cs** 파일을 **Main** 메서드 정의 됩니다. 다음 코드를 붙여 합니다 **Main** 함수입니다. 코드는 세 가지 쿼리를 실행 합니다. 첫 번째 쿼리 모든 **과정** 지정된 부서와 관련이 있습니다. 두 번째 쿼리에서 **OfType** 반환 하는 방법 **OnlineCourses** 지정된 부서와 관련 된 합니다. 세 번째 쿼리는 반환 **OnsiteCourses**합니다.

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
