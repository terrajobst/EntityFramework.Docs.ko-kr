---
title: 디자이너 엔터티에 분할 EF6
author: divega
ms.date: 10/23/2016
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: ba1895ae491cec909ff88a8784eea82f1876f595
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490855"
---
# <a name="designer-entity-splitting"></a>디자이너 엔터티 분
이 연습에서는 Entity Framework Designer (EF 디자이너)를 사용 하 여 모델을 수정 하 여 엔터티 형식을 두 테이블에 매핑하는 방법을 보여 줍니다. 테이블이 공통 키를 공유하는 경우 한 엔터티를 여러 테이블에 매핑할 수 있습니다. 두 개의 테이블에 한 엔터티 형식 매핑에 적용되는 개념은 셋 이상의 테이블에 한 엔터티 형식 매핑으로 쉽게 확장됩니다.

다음 이미지에서는 EF 디자이너를 사용 하 여 작업할 때 사용 되는 기본 windows를 보여 줍니다.

![EF 디자이너](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>전제 조건

Visual Studio 2012 또는 Visual Studio 2010, Ultimate, Premium, Professional 또는 Web Express 버전입니다.

## <a name="create-the-database"></a>데이터베이스 만들기

Visual Studio와 함께 설치 되는 데이터베이스 서버 설치한 Visual Studio의 버전에 따라 다릅니다.

-   Visual Studio 2012를 사용 하는 경우 다음 만들려는 LocalDB 데이터베이스입니다.
-   Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만드는 됩니다.

먼저 단일 엔터티로 결합 하겠습니다 두 테이블을 사용 하 여 데이터베이스를 만듭니다.

-   Visual Studio를 엽니다.
-   **보기-&gt; 서버 탐색기**
-   마우스 오른쪽 단추로 클릭 **데이터 연결-&gt; 연결 추가 중...**
-   선택 해야 하기 전에 서버 탐색기에서 데이터베이스에 연결 하지 않았으면 **Microsoft SQL Server** 데이터 원본으로
-   LocalDB 또는 어느에 따라 설치한 SQL Express에 연결
-   입력 **EntitySplitting** 데이터베이스 이름으로
-   선택 **확인** 를 묻는 새 데이터베이스를 만들려는 경우 **예**
-   새 데이터베이스 서버 탐색기에 나타납니다.
-   Visual Studio 2012를 사용 하는 경우
    -   서버 탐색기에서 데이터베이스를 마우스 오른쪽 단추로 클릭 하 고 선택 **새 쿼리**
    -   새 쿼리를 다음과 같은 SQL 복사 후 선택한 쿼리를 마우스 오른쪽 단추로 클릭 **실행**
-   Visual Studio 2010을 사용 하는 경우
    -   선택 **데이터&gt; Transact SQL 편집기-&gt; 새 쿼리 연결 하는 중...**
    -   입력 **.\\ SQLEXPRESS** 서버 이름 및 클릭으로 **확인**
    -   선택 된 **EntitySplitting** 쿼리 편집기의 맨 위에 있는 드롭다운에서 아래로 데이터베이스
    -   새 쿼리를 다음과 같은 SQL 복사 후 선택한 쿼리를 마우스 오른쪽 단추로 클릭 **SQL 실행**

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
-   왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔 응용 프로그램** 템플릿.
-   입력 **MapEntityToTablesSample** 고 프로젝트의 이름으로 **확인**합니다.
-   클릭 **No** 첫 번째 섹션에서 만든 SQL 쿼리를 저장 하 라는 메시지가 표시 되는 경우.

## <a name="create-a-model-based-on-the-database"></a>데이터베이스를 기반으로 모델 만들기

-   솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 **추가**를 클릭 하 고 **새 항목**합니다.
-   선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 템플릿 창에서.
-   입력 **MapEntityToTablesModel.edmx** 파일 이름 및 클릭 **추가**합니다.
-   Model 콘텐츠 선택 대화 상자에서 선택 **데이터베이스에서 생성**를 클릭 하 고 **다음입니다.**
-   선택 된 **EntitySplitting** 드롭다운에서 연결 아래로 및 클릭 **다음**합니다.
-   데이터베이스 개체 선택 대화 상자에서 확인란 옆에 **테이블** 노드.
    이렇게 하면 모든 테이블에 추가 됩니다는 **EntitySplitting** 모델 데이터베이스입니다.
-   **마침**을 클릭합니다.

모델 편집을 위해 디자인 화면을 제공 하는 Entity Designer에 표시 됩니다.

## <a name="map-an-entity-to-two-tables"></a>두 테이블에 엔터티 매핑

이 단계에서는 업데이트를 **Person** 엔터티 형식에서 데이터를 결합할 수는 **Person** 및 **PersonInfo** 테이블.

-   선택는 **전자 메일** 및 **Phone** 의 속성을 * * PersonInfo * * 엔터티 및 키를 누릅니다 **Ctrl + X를 누릅니다** 키.
-   선택 된 * * 사용자 * * 엔터티 및 키를 눌러 **Ctrl + V** 키입니다.
-   디자인 화면에서 선택 합니다 **PersonInfo** 엔터티와 키를 눌러 **삭제** 키보드에서 단추입니다.
-   클릭 **없음** 제거 하려는 경우 메시지가 표시 되 면를 **PersonInfo** 테이블 모델에서 여기에 매핑할 합니다 **Person** 엔터티.

    ![테이블 삭제](~/ef6/media/deletetables.png)

다음 단계는 필요 합니다 **매핑 정보** 창입니다. 이 창에 표시 되지 않으면 디자인 화면을 마우스 오른쪽 단추로 **매핑 정보**합니다.

-   선택는 **Person** 엔터티 형식 및 클릭 **&lt;테이블이 나 뷰를 추가&gt;** 에 **매핑 정보** 창.
-   선택 * * PersonInfo * * 드롭다운 목록에서.
    합니다 **매핑 정보** 창이 기본 열 매핑으로 업데이트 됩니다, 시나리오에 적합 합니다.

**Person** 엔터티 형식이 매핑되는 합니다 **Person** 및 **PersonInfo** 테이블.

![2 매핑](~/ef6/media/mapping2.png)

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

-   응용 프로그램을 컴파일하고 실행합니다.

다음 T-SQL 문은이 응용 프로그램을 실행 한 결과로 데이터베이스에 대해 실행 되었습니다. 

-   다음 두 **삽입** 문이 컨텍스트를 실행 한 결과로 실행 되었습니다. Savechanges ()입니다. 데이터를 차지 합니다 **사용자** 엔터티 간의 분할 및는 **사용자** 및 **PersonInfo** 테이블.

    ![1 삽입](~/ef6/media/insert1.png)

    ![2 삽입](~/ef6/media/insert2.png)
-   다음 **선택** 데이터베이스에 사용자를 열거 하는 결과로 실행 되었습니다. 데이터를 결합 하는 **Person** 및 **PersonInfo** 테이블입니다.

    ![선택](~/ef6/media/select.png)
