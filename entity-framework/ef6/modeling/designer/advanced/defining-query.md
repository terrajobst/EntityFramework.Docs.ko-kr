---
title: 쿼리-EF 디자이너-EF6를 정의합니다.
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
caps.latest.revision: 3
ms.openlocfilehash: 593fb9925a7a0b59a69b8c8dc4846640627756aa
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122282"
---
# <a name="defining-query---ef-designer"></a>정의 쿼리-EF 디자이너
이 연습에서는 정의 추가 하는 방법을 보여 줍니다. 쿼리 및 해당 엔터티를 EF 디자이너를 사용 하 여 모델에 입력 합니다. 정의 쿼리는 일반적으로 데이터베이스 뷰를 제공 하는 유사한 기능을 제공 하는 데 사용 됩니다 있지만 뷰는 데이터베이스가 아닌 모델에서 정의 됩니다. 정의 쿼리를 사용 하면 지정 된 SQL 문을 실행 하는 **DefiningQuery** .edmx 파일의 요소입니다. 자세한 내용은 **DefiningQuery** 에 [SSDL 사양](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md)합니다.

정의 쿼리를 사용 하는 경우 모델에 엔터티 형식을 정의할 수도 있습니다. 엔터티 형식 정의 쿼리에 의해 노출 된 데이터를 노출 됩니다. 참고가 엔터티 형식을 통해 표시 되는 데이터는 읽기 전용입니다.

매개 변수가 있는 쿼리를 정의 쿼리로 실행할 수 없습니다. 그러나 데이터는 저장 프로시저에 데이터를 표시하는 엔터티 형식의 삽입, 업데이트 및 삭제 함수를 매핑하여 업데이트할 수 있습니다. 자세한 내용은 [삽입, 업데이트 및 삭제 저장 프로시저를 사용 하 여](~/ef6/modeling/designer/stored-procedures/cud.md)입니다.

이 항목에서는 다음 작업을 수행 하는 방법을 보여 줍니다.

-   정의 쿼리 추가
-   모델에 엔터티 형식 추가
-   맵 엔터티 형식으로 정의 쿼리

## <a name="prerequisites"></a>전제 조건

이 연습을 완료하려면 다음 사항이 필요합니다.

- Visual Studio의 최신 버전입니다.
- 합니다 [School 샘플 데이터베이스](~/ef6/resources/school-database.md)합니다.

## <a name="set-up-the-project"></a>프로젝트 설정

이 연습에서는 Visual Studio 2012 이상 사용 중입니다.

-   Visual Studio를 엽니다.
-   **파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트**를 클릭합니다.
-   왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔 응용 프로그램** 템플릿.
-   입력 **DefiningQuerySample** 고 프로젝트의 이름으로 **확인**합니다.

 

## <a name="create-a-model-based-on-the-school-database"></a>School 데이터베이스를 기반으로 모델 만들기

-   솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 **추가**를 클릭 하 고 **새 항목**합니다.
-   선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 템플릿 창에서.
-   입력 **DefiningQueryModel.edmx** 파일 이름 및 클릭 **추가**합니다.
-   Model 콘텐츠 선택 대화 상자에서 선택 **데이터베이스에서 생성**를 클릭 하 고 **다음**합니다.
-   새 연결을 클릭 합니다. 연결 속성 대화 상자에서 서버 이름을 입력 합니다 (예를 들어 **(localdb)\\mssqllocaldb**)을 선택 인증 방법, 형식 **학교** 데이터베이스 이름에 대 한 다음 클릭 **확인**합니다.
    데이터 연결 선택 대화 상자는 데이터베이스 연결 설정으로 업데이트 됩니다.
-   데이터베이스 개체 선택 대화 상자에서 확인 합니다 **테이블** 노드. 이 테이블을 모두 추가 합니다 **학교** 모델입니다.
-   **마침**을 클릭합니다.
-   솔루션 탐색기에서 마우스 오른쪽 단추로 클릭 합니다 **DefiningQueryModel.edmx** 파일을 선택 **연결 프로그램...** .
-   선택 **XML (텍스트) 편집기**합니다.

    ![XMLEditor](~/ef6/media/xmleditor.png)

-   클릭 **예** 다음 메시지와 함께 메시지가 표시 되 면:

    ![Warning2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a>정의 쿼리 추가

정의 추가 하려면 XML 편집기를 사용 하 여이 단계에서는 쿼리 및.edmx 파일의 SSDL 섹션에는 엔터티 형식입니다. 

-   추가 된 **EntitySet** (13 년에서 5 줄).edmx 파일의 SSDL 섹션에는 요소입니다. 다음을 지정 합니다.
    -   만 **이름** 및 **EntityType** 의 특성을 **EntitySet** 요소를 지정 합니다.
    -   엔터티 형식의 정규화 된 이름에 사용 되는 **EntityType** 특성입니다.
    -   에 지정 된 SQL 문을 실행 합니다 **DefiningQuery** 요소입니다.

``` xml
    <!-- SSDL content -->
    <edmx:StorageModels>
      <Schema Namespace="SchoolModel.Store" Alias="Self" Provider="System.Data.SqlClient" ProviderManifestToken="2008" xmlns:store="http://schemas.microsoft.com/ado/2007/12/edm/EntityStoreSchemaGenerator" xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
        <EntityContainer Name="SchoolModelStoreContainer">
           <EntitySet Name="GradeReport" EntityType="SchoolModel.Store.GradeReport">
              <DefiningQuery>
                SELECT CourseID, Grade, FirstName, LastName
                FROM StudentGrade
                JOIN
                (SELECT * FROM Person WHERE EnrollmentDate IS NOT NULL) AS p
                ON StudentID = p.PersonID
              </DefiningQuery>
          </EntitySet>
          <EntitySet Name="Course" EntityType="SchoolModel.Store.Course" store:Type="Tables" Schema="dbo" />
```

-   추가 된 **EntityType** 요소를.edmx 파일의 SSDL 섹션에 있습니다. 표시 된 것 처럼 아래 파일입니다. 다음 사항에 유의하십시오.
    -   값을 **이름** 특성의 값에 해당는 **EntityType** 특성를 **EntitySet** 요소 위의 있지만의 정규화 된 이름을 엔터티 형식에 사용 되는 **EntityType** 특성입니다.
    -   속성 이름은 SQL 문에서 반환 되는 열 이름에 해당 합니다 **DefiningQuery** 요소 (위의 설명 참조).
    -   이 예제에서 엔터티 키는 고유 키 값을 보장하는 세 개의 속성으로 구성되어 있습니다.

``` xml
    <EntityType Name="GradeReport">
      <Key>
        <PropertyRef Name="CourseID" />
        <PropertyRef Name="FirstName" />
        <PropertyRef Name="LastName" />
      </Key>
      <Property Name="CourseID"
                Type="int"
                Nullable="false" />
      <Property Name="Grade"
                Type="decimal"
                Precision="3"
                Scale="2" />
      <Property Name="FirstName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
      <Property Name="LastName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
    </EntityType>
```

>[!NOTE]
> 나중에 실행 하는 경우는 **모델 업데이트 마법사** 대화 상자에서 정의 쿼리를 포함 하 여 저장소 모델에 대 한 변경 내용을 덮어쓰게 됩니다.

 

## <a name="add-an-entity-type-to-the-model"></a>모델에 엔터티 형식 추가

이 단계에서는 EF 디자이너를 사용 하 여 개념적 모델에 엔터티 형식을 추가 합니다.  다음 사항에 유의하십시오.

-   **이름** 엔터티의 값에 해당 하는 **EntityType** 특성을 **EntitySet** 위의 요소.
-   속성 이름은 SQL 문에서 반환 되는 열 이름에 해당 합니다 **DefiningQuery** 위의 요소.
-   이 예제에서 엔터티 키는 고유 키 값을 보장하는 세 개의 속성으로 구성되어 있습니다.

EF 디자이너에서 모델을 엽니다.

-   DefiningQueryModel.edmx를 두 번 클릭 합니다.
-   예를 들어 **예** 다음 메시지:

    ![Warning2](~/ef6/media/warning2.png)

 

모델 편집을 위해 디자인 화면을 제공 하는 Entity Designer에 표시 됩니다.

-   화면 디자이너를 마우스 오른쪽 단추로 클릭 **새로 추가**-&gt;**엔터티 중...** .
-   지정할 **GradeReport** 엔터티 이름에 대 한 및 **CourseID** 에 대 한 합니다 **키 속성**합니다.
-   마우스 오른쪽 단추로 클릭 합니다 **GradeReport** 엔터티 및 선택 **새로 추가** - &gt; **스칼라 속성**합니다.
-   속성의 기본 이름을 변경할 **FirstName**합니다.
-   다른 스칼라 속성을 추가 하 고 지정할 **LastName** 이름입니다.
-   다른 스칼라 속성을 추가 하 고 지정할 **등급** 이름입니다.
-   에 **속성** 창에서 변경 합니다 **등급**의 **형식** 속성을 **10 진수**.
-   선택 된 **FirstName** 하 고 **LastName** 속성입니다.
-   에 **속성** 창에서 변경 합니다 **EntityKey** 속성 값을 **True**합니다.

결과적으로, 다음 요소를 추가한 합니다 **CSDL** .edmx 파일의 섹션입니다.

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a>맵 엔터티 형식으로 정의 쿼리

이 단계에서는 개념 매핑할 매핑 정보 창 및 저장소 엔터티 형식을 사용 합니다.

-   마우스 오른쪽 단추로 클릭 합니다 **GradeReport** 디자인 화면에서 엔터티 **테이블 매핑**합니다.  
    합니다 **매핑 정보** 창이 표시 됩니다.
-   선택 **GradeReport** 에서 합니다 **&lt;테이블이 나 뷰 추가&gt;** 드롭다운 목록 (아래에 있는 **테이블**s).  
    기본 개념 간의 매핑 및 저장소 **GradeReport** 엔터티 형식을 표시 합니다.  
    ![MappingDetails3](~/ef6/media/mappingdetails.png)

결과적으로 **EntitySetMapping** 요소가.edmx 파일의 매핑 섹션에 추가 됩니다. 

``` xml
    <EntitySetMapping Name="GradeReports">
      <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.GradeReport)">
        <MappingFragment StoreEntitySet="GradeReport">
          <ScalarProperty Name="LastName" ColumnName="LastName" />
          <ScalarProperty Name="FirstName" ColumnName="FirstName" />
          <ScalarProperty Name="Grade" ColumnName="Grade" />
          <ScalarProperty Name="CourseID" ColumnName="CourseID" />
        </MappingFragment>
      </EntityTypeMapping>
    </EntitySetMapping>
```

-   응용 프로그램을 컴파일합니다.

 

## <a name="call-the-defining-query-in-your-code"></a>코드에서 정의 쿼리를 호출 합니다.

사용 하 여 이제 정의 쿼리를 실행할 수 있습니다 합니다 **GradeReport** 엔터티 형식입니다. 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
