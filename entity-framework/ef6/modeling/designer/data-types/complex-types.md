---
title: 복합 형식-EF 디자이너-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: a3c5578acee23688c04772d2dd0a2221779af562
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489910"
---
# <a name="complex-types---ef-designer"></a>복합 형식-EF 디자이너
이 항목에서는 Entity Framework Designer (EF 디자이너)를 사용 하 여 복합 형식을 매핑하는 방법 및 복합 형식의 속성을 포함 하는 엔터티에 대 한 쿼리 하는 방법을 보여 줍니다.

다음 이미지에서는 EF 디자이너를 사용 하 여 작업할 때 사용 되는 기본 windows를 보여 줍니다.

![EF 디자이너](~/ef6/media/efdesigner.png)

> [!NOTE]
> 개념적 모델을 빌드하면 매핑되지 않은 엔터티 및 연결에 대 한 경고는 오류 목록에 나타날 수 있습니다. 오류 사라집니다을 모델에서 데이터베이스를 생성 하도록 선택 하기 때문에 이러한 경고를 무시할 수 있습니다.

## <a name="what-is-a-complex-type"></a>복합 형식 이란

복합 형식은 스칼라 속성이 엔터티 내에 구성되도록 하는 엔터티 형식의 비스칼라 속성입니다. 복합 형식은 엔터티와 같이 스칼라 속성 또는 다른 복합 형식 속성으로 이루어집니다.

복합 형식을 나타내는 개체를 사용 하 여 작업할 때 다음을 고려해 야 합니다.

-   복합 형식은 키 없고 따라서 독립적으로 존재할 수 없습니다. 복합 형식은 엔터티 형식 또는 다른 복합 형식의 속성으로만 존재할 수 있습니다.
-   복합 형식 연결에 참여할 수 없습니다 및 탐색 속성을 포함할 수 없습니다.
-   복합 형식 속성 일 수 없습니다 **null**합니다. \* * InvalidOperationException * * 발생 때 **DbContext.SaveChanges** 라고는 null 복합 개체가 발견 되 고 있습니다. 복잡 한 개체의 스칼라 속성 일 수 있습니다 **null**합니다.
-   복합 형식은 다른 복합 형식에서 상속할 수 없습니다.
-   복합 유형으로 정의 해야 합니다는 **클래스**합니다. 
-   EF에서 복합 형식 개체에서 멤버의 변경 내용을 검색 하면 **DbContext.DetectChanges** 라고 합니다. 엔터티 프레임 워크에서 호출 **DetectChanges** 는 다음 멤버가 호출 될 때 자동으로: **DbSet.Find**하십시오 **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**합니다 **DbSet.Attach**를 **DbContext.SaveChanges**를 **DbContext.GetValidationErrors**, **DbContext.Entry**하십시오 **DbChangeTracker.Entries**합니다.

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a>엔터티의 속성을 새 복합 형식으로 리팩터링

개념적 모델의 엔터티를 이미 있는 경우에 일부 속성을 복합 형식 속성으로 리팩터링 하는 것이 좋습니다.

디자이너 화면에서 하나를 선택 하거나 엔터티를 더 많은 속성 (탐색 속성 제외)에서 그런 다음 마우스 오른쪽 단추로 클릭 하 고 선택 **-리팩터링&gt; 새 복합 형식이 이동**합니다.

![리팩터링](~/ef6/media/refactor.png)

선택한 속성을 사용 하 여 새 복합 형식에 추가 되는 **Model 브라우저**합니다. 복합 형식에 기본 이름이 지정됩니다.

새로 만든 형식의 복합 속성이 선택된 속성을 대체합니다. 모든 속성 매핑은 유지됩니다.

![2 리팩터링](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a>새 복합 형식을 만들려면

또한 기존 엔터티의 속성을 포함 하지 않는 새 복합 형식을 만들 수 있습니다.

마우스 오른쪽 단추로 클릭 합니다 **복합 형식** 폴더 Model 브라우저에서 가리키고 **AddNew 복합 형식...** . 선택할 수 있습니다 합니다 **복합 형식** 누릅니다 폴더를 **삽입** 키보드의 키입니다.

![새 복합 형식 추가](~/ef6/media/addnewcomplextype.png)

새 복합 형식이 기본 이름의 폴더에 추가됩니다. 이제 형식에 속성을 추가할 수 있습니다.

## <a name="add-properties-to-a-complex-type"></a>복합 형식에 속성 추가

복합 형식의 속성은 스칼라 형식 또는 기존 복합 형식일 수 있습니다. 그러나 복합 형식 속성에는 순환 참조가 있을 수 없습니다. 예를 들어, 복합 형식 **OnsiteCourseDetails** 복합 형식의 속성을 사용할 수 없습니다 **OnsiteCourseDetails**합니다.

아래에 나열된 방법 중 하나를 사용하여 복합 형식에 속성을 추가할 수 있습니다.

-   Model 브라우저에서 복합 형식을 마우스 오른쪽 **추가**, 가리킨 **스칼라 속성** 하거나 **복합 속성**, desired 속성 형식을 선택 합니다. 복합 형식을 선택 하 고 다음 키를 누릅니다 수 또는 합니다 **삽입** 키보드의 키입니다.  

    ![복합 형식 속성을 추가 합니다.](~/ef6/media/addpropertiestocomplextype.png)

    새 속성이 기본 이름의 복합 형식에 추가됩니다.

- 또는

-   엔터티 속성을 마우스 오른쪽 단추로 클릭 합니다 **EF 디자이너** 화면 **복사**에서 복합 형식을 마우스 오른쪽 단추로 클릭 합니다 **Model 브라우저** 선택한  **붙여넣기**합니다.

## <a name="rename-a-complex-type"></a>복합 형식의 이름을 바꾸려면

복합 형식의 이름을 바꾸면 프로젝트 전체에서 형식에 대한 모든 참조가 업데이트됩니다.

-   느린에서 복합 형식을 두 번 클릭 합니다 **Model 브라우저**합니다.
    이름이 선택되고 편집 모드가 됩니다.

- 또는

-   복합 형식을 마우스 오른쪽 단추로 클릭 합니다 **Model 브라우저** 선택한 **이름 바꾸기**합니다.

- 또는

-   Model 브라우저에서 복합 형식을 선택하고 F2 키를 누릅니다.

- 또는

-   복합 형식을 마우스 오른쪽 단추로 클릭 합니다 **Model 브라우저** 선택한 **속성**합니다. 이름을 편집 합니다 **속성** 창입니다.

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a>엔터티에 기존 복합 형식 추가 및 해당 속성을 테이블 열에 매핑

1.  엔터티를 마우스 오른쪽 **새로 추가**, 선택한 **복합 속성**합니다.
    기본 이름의 복합 형식 속성이 엔터티에 추가됩니다. 기존 복합 형식에서 선택한 기본 형식이 속성에 할당됩니다.
2.  원하는 형식의 속성에 할당 합니다 **속성** 창입니다.
    복합 형식 속성을 엔터티에 추가한 후에는 해당 속성을 테이블 열에 매핑해야 합니다.
3.  또는 디자인 화면에서 엔터티 형식을 마우스 오른쪽 단추로 클릭 합니다 **Model 브라우저** 선택한 **테이블 매핑을**합니다.
    테이블 매핑이 표시 됩니다는 **매핑 정보** 창입니다.
4.  확장 된 **매핑됩니다 &lt;테이블 이름&gt;**  노드.
    A **열 매핑** 노드가 표시 됩니다.
5.  확장 된 **열 매핑** 노드.
    테이블의 모든 열이 나열된 목록이 나타납니다. 기본 속성 (있는 경우) 열에 매핑하는 아래에 나열 됩니다는 **값/속성** 제목입니다.
6.  에 매핑할 열을 선택한 다음 해당 마우스 오른쪽 단추로 **값/속성** 필드입니다.
    모든 스칼라 속성을 나열한 드롭다운 목록이 표시됩니다.
7.  적절한 속성을 선택합니다.

    ![복합 형식 매핑](~/ef6/media/mapcomplextype.png)

8.  각 테이블 열별로 6 ~ 7단계를 반복합니다.

>[!NOTE]
> 열 매핑을 삭제를 매핑하고, 클릭 하려는 열을 선택 합니다 **값/속성** 필드입니다. 그런 다음 선택 **삭제** 드롭 다운 목록에서.

## <a name="map-a-function-import-to-a-complex-type"></a>Function Import를 복합 형식 매핑

Function Import는 저장 프로시저를 기반으로 합니다. Function Import를 복합 형식에 매핑하려면 해당 저장 프로시저에서 반환하는 열의 수와 복합 형식의 속성 수가 일치해야 하고 스토리지 형식이 속성 형식과 호환되어야 합니다.

-   복합 형식에 매핑하려는 가져온된 함수가 두 번 클릭 합니다.

    ![함수 가져오기](~/ef6/media/functionimports.png)

-   다음과 같이 새 Function Import에 대한 설정을 입력합니다.
    -   function import를 만드는 저장된 프로시저를 지정 합니다 **저장 프로시저 이름** 필드입니다. 이 필드는 스토리지 모델의 모든 저장 프로시저가 표시되는 드롭다운 목록입니다.
    -   function import의 이름을 지정 합니다 **Function Import 이름** 필드입니다.
    -   선택 **복잡 한** 반환으로 입력 한 다음 드롭다운 목록에서 적절 한 유형을 선택 하 여 특정 복합 반환 형식을 지정 합니다.

        ![Function Import 편집](~/ef6/media/editfunctionimport.png)

-   **확인**을 클릭합니다.
    Function Import 항목이 개념적 모델에 만들어집니다.

### <a name="customize-column-mapping-for-function-import"></a>열 매핑 함수 가져오기에 대 한 사용자 지정

-   Model 브라우저에서 function import를 마우스 오른쪽 단추로 클릭 **Function Import 매핑**합니다.
    합니다 **매핑 정보** 창이 나타나고 함수 가져오기에 대 한 기본 매핑을 보여 줍니다. 화살표는 열 값과 속성 값 사이의 매핑을 나타냅니다. 기본적으로 열 이름은 복합 형식의 속성 이름과 같다고 간주됩니다. 기본 열 이름은 회색 텍스트로 나타납니다.
-   필요한 경우 Function Import에 대응하는 저장 프로시저에서 반환된 열 이름과 일치하도록 열 이름을 변경합니다.

## <a name="delete-a-complex-type"></a>복합 형식을 삭제합니다

복합 형식을 삭제하면 개념적 모델에서 형식이 삭제되고 형식의 모든 인스턴스에 대한 매핑이 삭제됩니다. 그러나 형식에 대한 참조는 업데이트되지 않습니다. 예를 들어, 엔터티에 형식이 complextype1 인 복합 형식 속성 있고 ComplexType1에서 삭제 되는 **Model 브라우저**, 해당 엔터티 속성이 업데이트 되지 않습니다. 모델 삭제 된 복합 형식을 참조 하는 엔터티를 포함 하기 때문에 유효 하지 않습니다. Entity Designer를 사용하여 삭제된 복합 형식에 대한 참조를 업데이트하거나 삭제할 수 있습니다.

-   Model 브라우저에서 복합 형식을 마우스 오른쪽 단추로 클릭 **삭제**합니다.

- 또는

-   Model 브라우저에서 복합 형식을 선택한 다음 키보드의 Delete 키를 누릅니다.

## <a name="query-for-entities-containing-properties-of-complex-type"></a>복합 형식의 속성을 포함 하는 엔터티에 대 한 쿼리

다음 코드는 엔터티 컬렉션을 복합 형식 속성을 포함 하는 형식 개체를 반환 하는 쿼리를 실행 하는 방법을 보여 줍니다.

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        var courses =
            from c in context.OnsiteCourses
            order by c.Details.Time
            select c;

        foreach (var c in courses)
        {
            Console.WriteLine("Time: " + c.Details.Time);
            Console.WriteLine("Days: " + c.Details.Days);
            Console.WriteLine("Location: " + c.Details.Location);
        }
    }
```
