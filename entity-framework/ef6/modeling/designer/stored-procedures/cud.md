---
title: Designer CUD 저장 프로시저-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: bdb0df969c33d5ad3f103bfa9af6002c9c2bb9b3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813556"
---
# <a name="designer-cud-stored-procedures"></a>Designer CUD 저장 프로시저

이 단계별 연습에서는 Entity Framework Designer (EF Designer)를 사용 하 여 엔터티 형식의 create\\insert, update 및 delete (CUD) 작업을 저장 프로시저에 매핑하는 방법을 보여 줍니다.  기본적으로 Entity Framework는 CUD 작업에 대 한 SQL 문을 자동으로 생성 하지만 저장 프로시저를 이러한 작업에 매핑할 수도 있습니다.  

Code First은 저장 프로시저나 함수에 대 한 매핑을 지원 하지 않습니다. 그러나 SqlQuery 메서드를 사용 하 여 저장 프로시저 또는 함수를 호출할 수 있습니다. 예를 들면 다음과 같습니다.

``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a>CUD 작업을 저장 프로시저에 매핑할 때의 고려 사항

CUD 작업을 저장 프로시저에 매핑할 때는 다음과 같은 사항을 고려해 야 합니다.

- CUD 작업 중 하나를 저장 프로시저에 매핑하는 경우 모든 작업을 매핑합니다. 세 가지를 모두 매핑하지 않으면 매핑되지 않은 작업이 실행 되 고  **Updateexception** 이 throw 되는 경우 실패 합니다.
- 저장 프로시저의 모든 매개 변수를 엔터티 속성에 매핑해야 합니다.
- 서버에서 삽입 된 행에 대 한 기본 키 값을 생성 하는 경우이 값을 다시 엔터티의 키 속성에 매핑해야 합니다. 다음 예제에서 **Insertperson** 저장 프로시저는 새로 만든 기본 키를 저장 프로시저의 결과 집합의 일부로 반환 합니다. 기본 키는 EF 디자이너의 기능 **&gt;&lt;결과 바인딩 추가** 를 사용 하 여 엔터티 키 (**PersonID**)에 매핑됩니다.
- 저장 프로시저 호출은 개념적 모델의 엔터티와 함께 1:1에 매핑됩니다. 예를 들어 개념적 모델에서 상속 계층 구조를 구현 하 고 **부모** (기본) 및 **자식** (파생 된) 엔터티에 대 한 CUD 저장 프로시저를 매핑하는 경우 **자식** 변경 내용 저장은 **자식의**저장 프로시저만 호출 하 고 **부모의**저장 프로시저 호출을 트리거하지는 않습니다.

## <a name="prerequisites"></a>필수 조건

이 연습을 완료하려면 다음 사항이 필요합니다.

- 최신 버전의 Visual Studio입니다.
- [School 예제 데이터베이스](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>프로젝트 설정

- Visual Studio 2012을 엽니다.
- **파일-&gt; 새로 만들기-&gt; 프로젝트** 를 선택 합니다.
- 왼쪽 창에서 **Visual C\#** 을 클릭 한 다음 **콘솔** 템플릿을 선택 합니다.
- 이름 **으로 을** 입력 합니다.
-  **확인**을 선택합니다.

## <a name="create-a-model"></a>모델 만들기

- 솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가-&gt; 새 항목**을 선택 합니다.
- 왼쪽 메뉴에서 **데이터** 를 선택 하 고 템플릿 창에서 **ADO.NET 엔터티 데이터 모델** 를 선택 합니다.
- 파일 이름 **에 대해 다음** 을 입력 하 고 **추가**를 클릭 합니다.
- Model 콘텐츠 선택 대화 상자에서 **데이터베이스에서 생성**을 선택 하 고 **다음**을 클릭 합니다.
-  **새 연결**을 클릭 합니다. 연결 속성 대화 상자에서 서버 이름 (\\예: **mssqllocaldb**)을 입력 하 고, 인증 방법을 선택 하 고, 데이터베이스 이름으로 **School** 을 입력 한 다음, **확인**을 클릭 합니다.
    데이터 연결 선택 대화 상자가 데이터베이스 연결 설정으로 업데이트 됩니다.
- 데이터베이스 개체 선택 대화 상자의 **테이블** 노드에서 **Person** 테이블을 선택 합니다.
- 또한 **저장 프로시저 및 함수** 노드에서 **deleteperson**, **insertperson**및 **updateperson**의 저장 프로시저를 선택 합니다.
- Visual Studio 2012부터 EF Designer는 저장 프로시저의 대량 가져오기를 지원 합니다. **선택한 저장 프로시저 및 함수를 엔터티 모델로 가져오기** 는 기본적으로 선택 되어 있습니다. 이 예제에서는 엔터티 형식을 삽입, 업데이트 및 삭제 하는 저장 프로시저를 포함 하 고 있으므로이를 가져오지 않고이 확인란의 선택을 취소 합니다.

    ![가져오기 S 프로시저](~/ef6/media/importsprocs.jpg)

-  **마침**을 클릭 합니다.
    모델 편집을 위한 디자인 화면을 제공 하는 EF 디자이너가 표시 됩니다.

## <a name="map-the-person-entity-to-stored-procedures"></a>Person 엔터티를 저장 프로시저에 매핑

-  **Person** 엔터티 형식을 마우스 오른쪽 단추로 클릭 하 고 **저장 프로시저 매핑**을 선택 합니다.
- 저장 프로시저 매핑은 **매핑 정보** 창에 표시 됩니다.
-  **&lt;삽입 함수&gt;** 를 클릭 합니다.
    필드가 스토리지 모델의 저장 프로시저를 포함하는 드롭다운 목록이 됩니다. 이 목록의 저장 프로시저를 개념적 모델의 엔터티 형식에 매핑할 수 있습니다.
    드롭다운 목록에서 **Insertperson** 를 선택 합니다.
- 저장 프로시저 매개 변수와 엔터티 속성 간의 기본 매핑이 나타납니다. 화살표는 매핑 방향을 나타냅니다. 즉, 속성 값이 저장 프로시저 매개 변수에 제공됩니다.
-  **&lt;결과 바인딩 추가&gt;** 를 클릭 합니다.
-  **NewPersonID**를 입력 하 고 **insertperson** 저장 프로시저에서 반환 된 매개 변수의 이름을 입력 합니다. 선행 또는 후행 공백을 입력 하지 않아야 합니다.
-  **Enter**키를 누릅니다.
- 기본적으로 **NewPersonID** 는 **PersonID**엔터티 키에 매핑됩니다. 화살표는 매핑 방향을 나타냅니다. 즉, 결과 열의 값이 속성에 제공됩니다.

    ![매핑 정보](~/ef6/media/mappingdetails.png)

- &lt;클릭 하 여 ** 업데이트 함수&gt;**  를 선택 하 고 결과 드롭다운 목록에서 **updateperson** 을 선택 합니다.
- 저장 프로시저 매개 변수와 엔터티 속성 간의 기본 매핑이 나타납니다.
- &lt;클릭 하 여 ** 함수&gt; 선택** 하 고 결과 드롭다운 목록에서 **deleteperson** 를 선택 합니다.
- 저장 프로시저 매개 변수와 엔터티 속성 간의 기본 매핑이 나타납니다.

 **Person** 엔터티 형식의 삽입, 업데이트 및 삭제 작업이 이제 저장 프로시저에 매핑됩니다.

저장 프로시저를 사용 하 여 엔터티를 업데이트 하거나 삭제할 때 동시성 검사를 사용 하도록 설정 하려면 다음 옵션 중 하나를 사용 합니다.

- **OUTPUT** 매개 변수를 사용 하 여 저장 프로시저에서 영향을 받는 행 수를 반환 하 고 매개 변수 이름 옆의 **영향을 받는 행 매개 변수** 확인란을 선택 합니다. 작업이 호출 될 때 반환 되는 값이 0 이면  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) 이 throw 됩니다.
- 동시성 검사에 사용 하려는 속성 옆의 **원래 값 사용** 확인란을 선택 합니다. 업데이트를 시도 하면 데이터를 데이터베이스에 다시 쓸 때 원래 데이터베이스에서 읽은 속성의 값이 사용 됩니다. 값이 데이터베이스의 값과 일치 하지 않으면 **OptimisticConcurrencyException** 이 throw 됩니다.

## <a name="use-the-model"></a>모델 사용

**Main** 메서드가 정의 된 **Program.cs** 파일을 엽니다. Main 함수에 다음 코드를 추가 합니다.

이 코드는 새 **Person** 개체를 만든 다음 개체를 업데이트 하 고 마지막으로 개체를 삭제 합니다.

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

- 애플리케이션을 컴파일하고 실행합니다. 프로그램은 다음 출력을 생성 합니다. *

> [!NOTE]
> PersonID는 서버에 의해 자동으로 생성 되므로 다른 숫자가 표시 될 가능성이 높습니다. *

``` Output
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

최종 버전의 Visual Studio에서 작업 하는 경우 Intellitrace를 디버거와 함께 사용 하 여 실행 되는 SQL 문을 확인할 수 있습니다.

![Intellitrace](~/ef6/media/intellitrace.png)
