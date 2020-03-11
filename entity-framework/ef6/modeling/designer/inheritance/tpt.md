---
title: Designer TPT 상속-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415048"
---
# <a name="designer-tpt-inheritance"></a>디자이너 TPT 상속
이 단계별 연습에서는 Entity Framework Designer (EF Designer)를 사용 하 여 모델에 형식당 하나의 테이블 (TPT) 상속을 구현 하는 방법을 보여 줍니다. 형식당 하나의 테이블 상속에서는 데이터베이스의 별도 테이블을 사용하여 상속 계층 구조에 있는 각 형식의 상속되지 않는 속성 및 키 속성 데이터를 유지합니다.

이 연습에서는 **강좌** (기본 유형), **OnlineCourse** (과정에서 파생 됨) 및 **OnsiteCourse** ( **강의**에서 파생 됨) 엔터티를 같은 이름의 테이블로 매핑합니다. 데이터베이스에서 모델을 만든 다음 모델을 변경 하 여 TPT 상속을 구현 합니다.

Model First 시작 하 여 모델에서 데이터베이스를 생성할 수도 있습니다. EF Designer는 기본적으로 TPT 전략을 사용 하므로 모델의 모든 상속은 개별 테이블에 매핑됩니다.

## <a name="other-inheritance-options"></a>기타 상속 옵션

TPH (계층당 하나의 테이블)은 하나의 데이터베이스 테이블이 상속 계층 구조의 모든 엔터티 형식에 대 한 데이터를 유지 관리 하는 데 사용 되는 또 다른 상속 유형입니다.  Entity Designer를 사용 하 여 계층당 하나의 테이블 상속을 매핑하는 방법에 대 한 자세한 내용은 [EF DESIGNER TPH 상속](~/ef6/modeling/designer/inheritance/tph.md)을 참조 하세요. 

Entity Framework 런타임에서는 TPC (비추상 형식 상속) 및 혼합 된 상속 모델이 지원 되지만 EF 디자이너에서는 지원 되지 않습니다. TPC 또는 mixed 상속을 사용 하려는 경우 두 가지 옵션이 있습니다. Code First를 사용 하거나 EDMX 파일을 수동으로 편집 합니다. EDMX 파일을 사용 하도록 선택 하면 매핑 정보 창이 "안전 모드"로 전환 되 고 디자이너를 사용 하 여 매핑을 변경할 수 없습니다.

## <a name="prerequisites"></a>필수 조건

이 연습을 완료하려면 다음 사항이 필요합니다.

- 최신 버전의 Visual Studio입니다.
- [School 예제 데이터베이스](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>프로젝트 설정

-   Visual Studio 2012을 엽니다.
-   **파일-&gt; 새로 만들기-&gt; 프로젝트** 를 선택 합니다.
-   왼쪽 창에서 **Visual C\#** 을 클릭 한 다음 **콘솔** 템플릿을 선택 합니다.
-   이름으로 **TPTDBFirstSample** 를 입력 합니다.
-    **확인**을 선택합니다.

## <a name="create-a-model"></a>모델 만들기

-   솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **추가-&gt; 새 항목**을 선택 합니다.
-   왼쪽 메뉴에서 **데이터** 를 선택 하 고 템플릿 창에서 **ADO.NET 엔터티 데이터 모델** 를 선택 합니다.
-   파일 이름에 **TPTModel** 를 입력 한 다음 **추가**를 클릭 합니다.
-   Model 콘텐츠 선택 대화 상자에서 **데이터베이스에서 생성**을 선택 하 고 **다음**을 클릭 합니다.
-    **새 연결**을 클릭 합니다.
    연결 속성 대화 상자에서 서버 이름 (\\예: **mssqllocaldb**)을 입력 하 고, 인증 방법을 선택 하 고, 데이터베이스 이름으로 **School** 을 입력 한 다음, **확인**을 클릭 합니다.
    데이터 연결 선택 대화 상자가 데이터베이스 연결 설정으로 업데이트 됩니다.
-   데이터베이스 개체 선택 대화 상자의 테이블 노드에서 **부서**, **과정, OnlineCourse 및 OnsiteCourse** 테이블을 선택 합니다.
-    **마침**을 클릭 합니다.

모델 편집을 위한 디자인 화면을 제공 하는 Entity Designer 표시 됩니다. 데이터베이스 개체 선택 대화 상자에서 선택한 모든 개체가 모델에 추가 됩니다.

## <a name="implement-table-per-type-inheritance"></a>형식당 하나의 테이블 상속 구현

-   디자인 화면에서 **OnlineCourse** 엔터티 형식을 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다.
-   **속성** 창에서 기본 형식 속성을 **과정**으로 설정 합니다.
-   **OnsiteCourse** 엔터티 형식을 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다.
-   **속성** 창에서 기본 형식 속성을 **과정**으로 설정 합니다.
-   **OnlineCourse** 및 **강의** 엔터티 형식 사이의 연결 (선)을 마우스 오른쪽 단추로 클릭 합니다.
    **모델에서 삭제를**선택 합니다.
-   **OnsiteCourse** 및 **강의** 엔터티 형식 간의 연결을 마우스 오른쪽 단추로 클릭 합니다.
    **모델에서 삭제를**선택 합니다.

이제 **OnlineCourse** 및 **OnsiteCourse** 에서 **CourseID** 속성을 삭제 합니다. 이러한 클래스는 **과정** 기본 형식에서 **CourseID** 를 상속 하기 때문입니다.

-   **OnlineCourse** 엔터티 형식의 **CourseID** 속성을 마우스 오른쪽 단추로 클릭 한 다음 **모델에서 삭제**를 선택 합니다.
-   **OnsiteCourse** 엔터티 형식의 **CourseID** 속성을 마우스 오른쪽 단추로 클릭 한 다음 **모델에서 삭제** 를 선택 합니다.
-   이제 형식당 하나의 테이블 상속이 구현되었습니다.

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a>모델 사용

**Main** 메서드가 정의 된 **Program.cs** 파일을 엽니다. **Main** 함수에 다음 코드를 붙여 넣습니다. 이 코드는 세 개의 쿼리를 실행 합니다. 첫 번째 쿼리는 지정 된 학과와 관련 된 모든 **과정** 을 다시 가져옵니다. 두 번째 쿼리에서는 **OfType** 메서드를 사용 하 여 지정 된 학과와 관련 된 **OnlineCourses** 를 반환 합니다. 세 번째 쿼리는 **OnsiteCourses**를 반환 합니다.

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
