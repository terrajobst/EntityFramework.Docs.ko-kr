---
title: 디자이너 엔터티 분할-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: ba1895ae491cec909ff88a8784eea82f1876f595
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415264"
---
# <a name="designer-entity-splitting"></a>디자이너 엔터티 분할
이 연습에서는 Entity Framework Designer (EF Designer)를 사용 하 여 모델을 수정 하 여 엔터티 형식을 두 개의 테이블에 매핑하는 방법을 보여 줍니다. 테이블이 공통 키를 공유하는 경우 한 엔터티를 여러 테이블에 매핑할 수 있습니다. 엔터티 형식을 두 개의 테이블에 매핑하는 데 적용 되는 개념은 엔터티 형식을 세 개 이상의 테이블에 매핑하기 쉽도록 쉽게 확장할 수 있습니다.

다음 그림에서는 EF Designer를 사용할 때 사용 되는 기본 창을 보여 줍니다.

![EF 디자이너](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>필수 조건

Visual Studio 2012 또는 Visual Studio 2010, Ultimate, Premium, Professional 또는 Web Express edition

## <a name="create-the-database"></a>데이터베이스 만들기

Visual Studio와 함께 설치 되는 데이터베이스 서버는 설치한 Visual Studio 버전에 따라 다릅니다.

-   Visual Studio 2012을 사용 하는 경우 LocalDB 데이터베이스를 만듭니다.
-   Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만듭니다.

먼저 단일 엔터티로 결합할 두 개의 테이블이 포함 된 데이터베이스를 만듭니다.

-   Visual Studio를 엽니다.
-   **뷰&gt; 서버 탐색기**
-   데이터 연결을 마우스 오른쪽 단추로 클릭 하 **&gt; 연결 추가** ...를 클릭 합니다.
-   서버 탐색기 데이터베이스에 연결 하지 않은 경우 **Microsoft SQL Server** 를 데이터 원본으로 선택 해야 합니다.
-   설치한 항목에 따라 LocalDB 또는 SQL Express에 연결
-   데이터베이스 이름으로 **EntitySplitting** 을 입력 합니다.
-   **확인** 을 선택 하 고 새 데이터베이스를 만들지 여부를 묻는 메시지가 표시 되 면 **예** 를 선택 합니다.
-   이제 새 데이터베이스가 서버 탐색기에 표시 됩니다.
-   Visual Studio 2012을 사용 하는 경우
    -   서버 탐색기에서 데이터베이스를 마우스 오른쪽 단추로 클릭 하 고 **새 쿼리** 를 선택 합니다.
    -   다음 SQL을 새 쿼리에 복사한 다음 쿼리를 마우스 오른쪽 단추로 클릭 하 고 **실행** 을 선택 합니다.
-   Visual Studio 2010을 사용 하는 경우
    -   **데이터&gt; Transact-sql 편집기-&gt; 새 쿼리 연결** ...을 선택 합니다.
    -   \\SQLEXPRESS를 서버 이름으로 입력 하 고 **확인을** 클릭 **합니다.**
    -   쿼리 편집기 위쪽의 드롭다운에서 **EntitySplitting** 데이터베이스를 선택 합니다.
    -   다음 SQL을 새 쿼리에 복사한 다음 쿼리를 마우스 오른쪽 단추로 클릭 하 고 **Sql 실행** 을 선택 합니다.

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a>프로젝트 만들기

-   **파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트**를 클릭합니다.
-   왼쪽 창에서 **Visual C\#** 을 클릭 한 다음 **콘솔 응용 프로그램** 템플릿을 선택 합니다.
-   프로젝트 이름으로 **MapEntityToTablesSample** 를 입력 하 고 **확인을**클릭 합니다.
-   첫 번째 섹션에서 만든 SQL 쿼리를 저장 하 라는 메시지가 표시 되 면 **아니요** 를 클릭 합니다.

## <a name="create-a-model-based-on-the-database"></a>데이터베이스를 기반으로 모델 만들기

-   솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가**를 가리킨 다음 **새 항목**을 클릭 합니다.
-   왼쪽 메뉴에서 **데이터** 를 선택 하 고 템플릿 창에서 **ADO.NET 엔터티 데이터 모델** 를 선택 합니다.
-   파일 이름에 대해 **Mapentitytonode.js 모델 .edmx** 를 입력 한 다음 **추가**를 클릭 합니다.
-   Model 콘텐츠 선택 대화 상자에서 **데이터베이스에서 생성**을 선택 하 고 다음을 클릭 **합니다.**
-   드롭다운에서 **EntitySplitting** 연결을 선택 하 고 **다음**을 클릭 합니다.
-   데이터베이스 개체 선택 대화 상자에서 **테이블** 노드 옆의 확인란을 선택 합니다.
    그러면 **EntitySplitting** 데이터베이스의 모든 테이블이 모델에 추가 됩니다.
-    **마침**을 클릭 합니다.

모델 편집을 위한 디자인 화면을 제공 하는 Entity Designer 표시 됩니다.

## <a name="map-an-entity-to-two-tables"></a>두 테이블에 엔터티 매핑

이 단계에서는 person 엔터티 형식이 **person** 및 **PersonInfo** 테이블의 데이터를 결합 하도록 **업데이트 합니다.**

-    **PersonInfo **엔터티의 **Email** 및 **Phone** 속성을 선택 하 고 **ctrl + X** 키를 누릅니다.
-    **Person **엔터티를 선택 하 고 **ctrl + V** 키를 누릅니다.
-   디자인 화면에서 **PersonInfo** 엔터티를 선택 하 고 키보드에서 **삭제** 단추를 누릅니다.
-   모델에서 **PersonInfo** 테이블을 제거할지 묻는 메시지가 표시 되 면 **아니요** 를 클릭 합니다. 그러면 **Person** 엔터티에 매핑됩니다.

    ![테이블 삭제](~/ef6/media/deletetables.png)

다음 단계를 수행 하려면 **매핑 정보** 창이 필요 합니다. 이 창이 표시 되지 않으면 디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 **매핑 정보**를 선택 합니다.

-    **Person** 엔터티 유형을 선택 하 고 **매핑 정보** 창에서 ** 테이블 또는 뷰&gt; 추가&lt;** 클릭 합니다.
-   드롭다운 목록에서 **PersonInfo ** 를 선택 합니다.
     **매핑 정보** 창은 기본 열 매핑으로 업데이트 되며이는 시나리오에 적합 합니다.

이제 **person** 엔터티 형식이 **Person** 및 **PersonInfo** 테이블에 매핑됩니다.

![매핑 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a>모델 사용

-   Main 메서드에 다음 코드를 붙여 넣습니다.

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   애플리케이션을 컴파일하고 실행합니다.

다음 T-sql 문은이 응용 프로그램을 실행 한 결과로 데이터베이스에 대해 실행 되었습니다. 

-   다음 두 **INSERT** 문은 컨텍스트를 실행 한 결과로 실행 되었습니다. SaveChanges (). **Person** 엔터티에서 데이터를 가져와 **person** 테이블과 **PersonInfo** 테이블 간에 분할 합니다.

    ![1 삽입](~/ef6/media/insert1.png)

    ![2 삽입](~/ef6/media/insert2.png)
-   데이터베이스에서 사용자를 열거 한 결과로 다음 **SELECT** 가 실행 되었습니다. **Person** 및 **PersonInfo** 테이블의 데이터를 결합 합니다.

    ![선택](~/ef6/media/select.png)
