---
title: 저장 프로시저-EF6 디자이너 CUD
author: divega
ms.date: 2016-10-23
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: 36c9b97b77fec30136cba1d850a0259c689e69ae
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250922"
---
# <a name="designer-cud-stored-procedures"></a>디자이너 CUD 저장 프로시저
이 단계별 연습 만들기를 매핑하는 방법을 보여 줍니다\\삽입, 업데이트 및 삭제 작업 (CUD) Entity Framework Designer (EF 디자이너)를 사용 하 여 저장된 프로시저에 엔터티 형식입니다.  기본적으로 Entity Framework에 CUD 작업에 대 한 SQL 문을 자동으로 생성 되지만 이러한 작업에 저장된 프로시저도 매핑할 수 있습니다.  

Code First에서 지원 하지 않음을 저장 프로시저나 함수에 대 한 매핑 참고 합니다. 그러나 System.Data.Entity.DbSet.SqlQuery 메서드를 사용 하 여 저장된 프로시저 또는 함수를 호출할 수 있습니다. 예를 들어:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a>저장 프로시저를 CUD 작업을 매핑할 때의 고려 사항

CUD 작업 저장된 프로시저를 매핑할 경우 다음 사항을 고려해 야 합니다. 

-   CUD 작업 중 하나는 저장된 프로시저에 매핑하는, 하는 경우 모두를 매핑하십시오. 실행 하는 경우 매핑되지 않은 작업이 모든 3으로 매핑되지 않습니다 못합니다 **UpdateException** throw 됩니다.
-   엔터티 속성에 저장된 프로시저의 모든 매개 변수에 매핑되어야 합니다.
-   삽입 된 행에 대 한 기본 키 값을 생성 하는 서버,이 값이 엔터티의 키 속성에 다시 매핑해야 합니다. 나오는 예제는 **InsertPerson** 저장된 프로시저는 저장된 프로시저의 결과 집합의 일부로 새로 만든된 기본 키를 반환 합니다. 기본 키가 엔터티 키에 매핑된 (**PersonID**)를 사용 하는 **&lt;Binding 추가&gt;** EF 디자이너의 기능입니다.
-   저장된 프로시저 호출은 개념적 모델의 엔터티를 사용 하 여 매핑된 1:1입니다. 예를 들어, 한 다음 맵 고 개념적 모델의 상속 계층 구조를 구현 하는 경우는 CUD 저장 프로시저에 대 한 합니다 **부모** (기본) 및 **자식** (파생 된) 엔터티를 저장 합니다 **자식** 변경만 호출 됩니다는 **자식**저장 프로시저의 트리거되지 않습니다 합니다 **부모**저장 된 프로시저 호출의 합니다.

## <a name="prerequisites"></a>전제 조건

이 연습을 완료하려면 다음 사항이 필요합니다.

- Visual Studio의 최신 버전입니다.
- 합니다 [School 샘플 데이터베이스](~/ef6/resources/school-database.md)합니다.

## <a name="set-up-the-project"></a>프로젝트 설정

-   Visual Studio 2012를 엽니다.
-   선택 **파일-&gt; 새로운 기능-&gt; 프로젝트**
-   왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔** 템플릿.
-   입력 **CUDSProcsSample** 이름으로 합니다.
-   **확인**을 선택합니다.

## <a name="create-a-model"></a>모델 만들기

-   솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 누르고 **추가-&gt; 새 항목**합니다.
-   선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 템플릿 창에서.
-   입력 **CUDSProcs.edmx** 파일 이름 및 클릭 **추가**합니다.
-   Model 콘텐츠 선택 대화 상자에서 선택 **데이터베이스에서 생성**를 클릭 하 고 **다음**합니다.
-   클릭 **새 연결**합니다. 연결 속성 대화 상자에서 서버 이름을 입력 합니다 (예를 들어 **(localdb)\\mssqllocaldb**)을 선택 인증 방법, 형식 **학교** 데이터베이스 이름에 대 한 다음 클릭 **확인**합니다.
    데이터 연결 선택 대화 상자는 데이터베이스 연결 설정으로 업데이트 됩니다.
-   선택에서 하면 데이터베이스 개체 대화 상자에서 **테이블** 노드를 선택 합니다 **Person** 테이블입니다.
-   또한 아래에 있는 다음 저장된 프로시저를 선택 합니다 **저장 프로시저 및 함수** 노드: **DeletePerson**합니다 **InsertPerson**, 및 **UpdatePerson** . 
-   Visual Studio 2012를 사용한 EF 디자이너 시작 저장된 프로시저의 대량 가져오기를 지원 합니다. 합니다 **엔터티 모델에 저장된 프로시저 및 함수를 선택 하는 가져오기** 기본적으로 선택 됩니다. 이 예제에서 삽입, 업데이트 및 엔터티 형식을 삭제 하는 프로시저 저장를 있으므로 가져와야 하지 않으려는 하 고이 확인란의 선택을 취소 합니다. 

    ![S 프로시저 가져오기](~/ef6/media/importsprocs.jpg)

-   **마침**을 클릭합니다.
    모델 편집을 위해 디자인 화면을 제공 하는 EF 디자이너 표시 됩니다.

## <a name="map-the-person-entity-to-stored-procedures"></a>저장된 프로시저에 Person 엔터티 매핑

-   마우스 오른쪽 단추로 클릭 합니다 **Person** 엔터티 형식 및 선택 **저장 프로시저 매핑**합니다.
-   저장된 프로시저 매핑에 표시 합니다 **매핑 정보** 창입니다.
-   클릭  **&lt;선택 함수 삽입&gt;** 합니다.
    필드가 저장소 모델의 저장 프로시저를 포함하는 드롭다운 목록이 됩니다. 이 목록의 저장 프로시저를 개념적 모델의 엔터티 형식에 매핑할 수 있습니다.
    선택 **InsertPerson** 드롭 다운 목록에서.
-   저장 프로시저 매개 변수와 엔터티 속성 간의 기본 매핑이 나타납니다. 화살표는 매핑 방향을 나타냅니다. 즉, 속성 값이 저장 프로시저 매개 변수에 제공됩니다.
-   클릭  **&lt;; Result Binding 추가&gt;** 합니다.
-   형식 **NewPersonID**에서 반환한 매개 변수의 이름을 합니다 **InsertPerson** 저장 프로시저. 선행 또는 후행 공백을 입력할 필요가 있는지 확인 합니다.
-   **Enter** 키를 누릅니다.
-   기본적으로 **NewPersonID** 엔터티 키에 매핑되어 **PersonID**합니다. 화살표는 매핑 방향을 나타냅니다. 즉, 결과 열의 값이 속성에 제공됩니다.

    ![매핑 정보](~/ef6/media/mappingdetails.png)

-   클릭 **&lt;Update Function 선택&gt;** 선택한 **UpdatePerson** 결과 드롭다운 목록에서.
-   저장 프로시저 매개 변수와 엔터티 속성 간의 기본 매핑이 나타납니다.
-   클릭 **&lt;Delete Function 선택&gt;** 선택한 **DeletePerson** 결과 드롭다운 목록에서.
-   저장 프로시저 매개 변수와 엔터티 속성 간의 기본 매핑이 나타납니다.

삽입, 업데이트 및 삭제 작업을 **Person** 엔터티 형식에 이제 저장된 프로시저에 매핑됩니다.

업데이트 하거나 저장된 프로시저를 사용 하 여 엔터티를 삭제할 때 확인 하는 동시성을 사용 하도록 설정 하려는 경우 다음 옵션 중 하나를 사용 합니다.

-   사용 하 여는 **출력** 매개 변수 수가 저장된 프로시저 및 검사에서 행에 영향을 합니다 **매개 변수에 영향을 받는 행** 매개 변수 이름 옆의 확인란을 선택 합니다. 작업이 호출 될 때 반환 되는 값이 0을 [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) throw 됩니다.
-   확인 합니다 **Original Value 사용** 동시성 검사에 사용 하려는 속성 옆의 확인란을 선택 합니다. 원래 데이터베이스에서 읽은 속성의 값을 업데이트 하려고 하는 경우 데이터 쓰기 데이터베이스에 백업 하는 경우 사용 됩니다. 값을 데이터베이스에서 값을 일치 하지 않는 경우는 **OptimisticConcurrencyException** throw 됩니다.

## <a name="use-the-model"></a>모델 사용

열기는 **Program.cs** 파일을 **Main** 메서드 정의 됩니다. 다음 코드를 Main 함수에 추가 합니다.

코드를 만듭니다 **Person** 개체 다음 개체를 업데이트 하 고 마지막으로 개체를 삭제 합니다.         

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

-   응용 프로그램을 컴파일하고 실행합니다. 다음 출력을 생성 하는 프로그램 *
    >[!NOTE]
> PersonID 이므로 서버에서 자동으로 생성 하면 표시 될 가능성이 다른 번호 *

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

Visual Studio Ultimate 버전을 사용 하는 경우에 실행 되는 SQL 문을 보려면 디버거를 사용 하 여 Intellitrace를 사용할 수 있습니다.

![Intellitrace](~/ef6/media/intellitrace.png)
