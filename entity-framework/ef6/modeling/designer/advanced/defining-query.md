---
title: 쿼리 정의-EF 디자이너-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: b1589dc12ccb50754c2e950932a2d82bc4869f6b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415534"
---
# <a name="defining-query---ef-designer"></a>쿼리 정의-EF 디자이너
이 연습에서는 EF Designer를 사용 하 여 정의 쿼리와 해당 엔터티 형식을 모델에 추가 하는 방법을 보여 줍니다. 정의 쿼리는 데이터베이스 뷰에서 제공 하는 것과 유사한 기능을 제공 하는 데 일반적으로 사용 되지만, 뷰는 데이터베이스가 아니라 모델에 정의 됩니다. 정의 쿼리를 사용 하면 .edmx 파일의 **DefiningQuery** 요소에 지정 된 SQL 문을 실행할 수 있습니다. 자세한 내용은 **DefiningQuery** In The [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md)를 참조 하세요.

쿼리 정의를 사용 하는 경우 모델에서 엔터티 형식을 정의 해야 합니다. 엔터티 형식은 정의 하는 쿼리에 의해 노출 되는 데이터를 표시 하는 데 사용 됩니다. 이 엔터티 형식을 통해 표시 되는 데이터는 읽기 전용입니다.

매개 변수가 있는 쿼리를 정의 쿼리로 실행할 수 없습니다. 그러나 데이터는 저장 프로시저에 데이터를 표시하는 엔터티 형식의 삽입, 업데이트 및 삭제 함수를 매핑하여 업데이트할 수 있습니다. 자세한 내용은 [저장 프로시저를 사용 하 여 삽입, 업데이트 및 삭제](~/ef6/modeling/designer/stored-procedures/cud.md)를 참조 하세요.

이 항목에서는 다음 작업을 수행 하는 방법을 보여 줍니다.

-   정의 쿼리 추가
-   모델에 엔터티 형식 추가
-   정의 쿼리를 엔터티 형식에 매핑

## <a name="prerequisites"></a>필수 조건

이 연습을 완료하려면 다음 사항이 필요합니다.

- 최신 버전의 Visual Studio입니다.
- [School 예제 데이터베이스](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>프로젝트 설정

이 연습에서는 Visual Studio 2012 이상 버전을 사용 합니다.

-   Visual Studio를 엽니다.
-   **파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트**를 클릭합니다.
-   왼쪽 창에서 **Visual C\#** 을 클릭 한 다음 **콘솔 응용 프로그램** 템플릿을 선택 합니다.
-   프로젝트 이름으로 **DefiningQuerySample** 를 입력 하 고 **확인을**클릭 합니다.

 

## <a name="create-a-model-based-on-the-school-database"></a>School 데이터베이스를 기반으로 모델 만들기

-   솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가**를 가리킨 다음 **새 항목**을 클릭 합니다.
-   왼쪽 메뉴에서 **데이터** 를 선택 하 고 템플릿 창에서 **ADO.NET 엔터티 데이터 모델** 를 선택 합니다.
-   파일 이름에 **DefiningQueryModel** 를 입력 한 다음 **추가**를 클릭 합니다.
-   Model 콘텐츠 선택 대화 상자에서 **데이터베이스에서 생성**을 선택 하 고 **다음**을 클릭 합니다.
-   새 연결을 클릭 합니다. 연결 속성 대화 상자에서 서버 이름 (\\예: **mssqllocaldb**)을 입력 하 고, 인증 방법을 선택 하 고, 데이터베이스 이름으로 **School** 을 입력 한 다음, **확인**을 클릭 합니다.
    데이터 연결 선택 대화 상자가 데이터베이스 연결 설정으로 업데이트 됩니다.
-   데이터베이스 개체 선택 대화 상자에서 **테이블** 노드를 선택 합니다. 이렇게 하면 모든 테이블이 **School** 모델에 추가 됩니다.
-    **마침**을 클릭 합니다.
-   솔루션 탐색기에서 **DefiningQueryModel** 파일을 마우스 오른쪽 단추로 클릭 하 고 **연결 프로그램 ...** 을 선택 합니다.
-   **XML (텍스트) 편집기**를 선택 합니다.

    ![XML 편집기](~/ef6/media/xmleditor.png)

-   다음 메시지가 표시 되 면 **예** 를 클릭 합니다.

    ![경고 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a>정의 쿼리 추가

이 단계에서는 XML 편집기를 사용 하 여 정의 쿼리와 엔터티 형식을 .edmx 파일의 SSDL 섹션에 추가 합니다. 

-   .Edmx 파일의 SSDL 섹션에 **EntitySet** 요소를 추가 합니다 (5 ~ 13 번 줄). 다음 사항을 지정합니다.
    -    **EntitySet** 요소의 **이름** 및 **EntityType** 특성만 지정 됩니다.
    -   엔터티 형식의 정규화 된 이름은 **EntityType** 특성에 사용 됩니다.
    -   실행할 SQL 문은 **DefiningQuery** 요소에 지정 되어 있습니다.

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

-   .Edmx의 SSDL 섹션에 **EntityType** 요소를 추가 합니다. 파일이 아래와 같이 표시 됩니다. 유의 사항은 다음과 같습니다.
    -   **이름** 특성의 값은 위의 **EntitySet** 요소에 있는 **entitytype** 특성의 값에 해당 합니다. 하지만 엔터티 형식의 정규화 된 이름은 **entitytype** 특성에 사용 됩니다.
    -   속성 이름은 **DefiningQuery** 요소의 SQL 문에서 반환 되는 열 이름 (위)에 해당 합니다.
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
> 나중에 **모델 업데이트 마법사** 대화 상자를 실행 하는 경우 쿼리 정의를 비롯 하 여 저장소 모델에 대 한 변경 내용을 덮어씁니다.

 

## <a name="add-an-entity-type-to-the-model"></a>모델에 엔터티 형식 추가

이 단계에서는 EF Designer를 사용 하 여 엔터티 형식을 개념적 모델에 추가 합니다.  다음 사항에 유의 하세요.

-   엔터티의 **이름은** 위의 **EntitySet** 요소에 있는 **EntityType** 특성의 값에 해당 합니다.
-   속성 이름은 위의 **DefiningQuery** 요소에서 SQL 문에 의해 반환 되는 열 이름에 해당 합니다.
-   이 예제에서 엔터티 키는 고유 키 값을 보장하는 세 개의 속성으로 구성되어 있습니다.

EF 디자이너에서 모델을 엽니다.

-   DefiningQueryModel를 두 번 클릭 합니다.
-   **예** 를 들어 다음 메시지를 표시 합니다.

    ![경고 2](~/ef6/media/warning2.png)

 

모델 편집을 위한 디자인 화면을 제공 하는 Entity Designer 표시 됩니다.

-   디자이너 화면을 마우스 오른쪽 단추로 클릭 하 고 **추가 새**-&gt;**엔터티 ...** 를 선택 합니다.
-   엔터티 이름에 **GradeReport** 을 지정 하 고 **키 속성**에 **CourseID** 를 지정 합니다.
-   **GradeReport** 엔터티를 마우스 오른쪽 단추로 클릭 하 고 **Add New**-&gt; **스칼라 속성**을 선택 합니다.
-   속성의 기본 이름을 **FirstName**으로 변경 합니다.
-   다른 스칼라 속성을 추가 하 고 이름에 **LastName** 을 지정 합니다.
-   다른 스칼라 속성을 추가 하 고 이름에 대해 **등급** 을 지정 합니다.
-   **속성** 창에서 **등급**의 **Type** 속성을 **Decimal**로 변경 합니다.
-   **FirstName** 및 **LastName** 속성을 선택 합니다.
-   **속성** 창에서 **EntityKey** 속성 값을 **True**로 변경 합니다.

따라서 다음 요소가 .edmx 파일의 **CSDL** 섹션에 추가 되었습니다.

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a>정의 쿼리를 엔터티 형식에 매핑

이 단계에서는 매핑 정보 창을 사용 하 여 개념 및 저장소 엔터티 형식을 매핑합니다.

-   디자인 화면에서 **GradeReport** 엔터티를 마우스 오른쪽 단추로 클릭 하 고 **테이블 매핑**을 선택 합니다.  
    **매핑 정보** 창이 표시 됩니다.
-   테이블 **또는 뷰&gt;** 드롭다운 목록 ( **테이블**s 아래에 있음)&lt;에서 **GradeReport** 을 선택 합니다.  
    개념 및 저장소 **GradeReport** 엔터티 형식 간의 기본 매핑이 표시 됩니다.  
    ![매핑 Details3](~/ef6/media/mappingdetails.png)

결과적으로 **EntitySetMapping** 요소가 .edmx 파일의 매핑 섹션에 추가 됩니다. 

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

-   애플리케이션을 컴파일합니다.

 

## <a name="call-the-defining-query-in-your-code"></a>코드에서 정의 쿼리를 호출 합니다.

이제 **GradeReport** 엔터티 형식을 사용 하 여 정의 쿼리를 실행할 수 있습니다. 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
