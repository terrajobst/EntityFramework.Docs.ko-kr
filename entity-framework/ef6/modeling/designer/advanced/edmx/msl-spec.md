---
title: MSL 사양-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 8990d1373ea2121ce11337a43dbcdf3b9e1532bd
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182563"
---
# <a name="msl-specification"></a>XML 사양
MSL (매핑 사양 언어)은 Entity Framework 응용 프로그램의 개념적 모델과 저장소 모델 간의 매핑을 설명 하는 XML 기반 언어입니다.

Entity Framework 응용 프로그램에서 매핑 메타 데이터는 빌드 시 msl 파일 (MSL로 작성 됨)에서 로드 됩니다. Entity Framework는 런타임에 매핑 메타 데이터를 사용 하 여 개념적 모델에 대 한 쿼리를 저장소 관련 명령으로 변환 합니다.

Entity Framework Designer (EF Designer)는 디자인 타임에 매핑 정보를 .edmx 파일에 저장 합니다. 빌드 시 Entity Designer는 .edmx 파일의 정보를 사용 하 여 런타임에 Entity Framework에 필요한 msl 파일을 만듭니다.

MSL에서 참조하는 모든 개념적 모델 형식 또는 스토리지 모델 형식의 이름은 해당 네임스페이스 이름으로 한정되어야 합니다. 개념적 모델 네임 스페이스 이름에 대 한 자세한 내용은 [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md)을 참조 하세요. 저장소 모델 네임 스페이스 이름에 대 한 자세한 내용은 [SSDL 사양](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md)을 참조 하십시오.

MSL의 버전은 XML 네임 스페이스로 구분 됩니다.

| MSL 버전 | XML 네임 스페이스                                        |
|:------------|:-----------------------------------------------------|
| MSL v1      | urn: 스키마-microsoft-com: windows: storage: 매핑: CS |
| MSL v2      | https://schemas.microsoft.com/ado/2008/09/mapping/cs |
| MSL v3      | https://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a>Alias 요소(MSL)

MSL (매핑 사양 언어)의 **Alias** 요소는 개념적 모델 및 저장소 모델 네임 스페이스에 대 한 별칭을 정의 하는 데 사용 되는 mapping 요소의 자식입니다. MSL에서 참조하는 모든 개념적 모델 형식 또는 스토리지 모델 형식의 이름은 해당 네임스페이스 이름으로 한정되어야 합니다. 개념적 모델 네임 스페이스 이름에 대 한 자세한 내용은 Schema 요소 (CSDL)를 참조 하세요. 저장소 모델 네임 스페이스 이름에 대 한 자세한 내용은 Schema 요소 (SSDL)를 참조 하세요.

**Alias** 요소에는 자식 요소가 있을 수 없습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Alias** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| **Key**        | 예         | **Value** 특성에 지정 된 네임 스페이스에 대 한 별칭입니다. |
| **Value**      | 예         | **키** 요소의 값이 별칭이 되는 네임 스페이스입니다.     |

### <a name="example"></a>예제

다음 예제에서는 개념적 모델에 정의 된 형식에 대 한 별칭 `c`을 정의 하는 **alias** 요소를 보여 줍니다.

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="associationend-element-msl"></a>AssociationEnd 요소(MSL)

MSL (매핑 사양 언어)의 **AssociationEnd** 요소는 개념적 모델의 엔터티 형식에 대 한 수정 함수가 기본 데이터베이스의 저장 프로시저에 매핑될 때 사용 됩니다. 수정 저장 프로시저가 association 속성에 값을 보유 하는 매개 변수를 사용 하는 경우 **AssociationEnd** 요소는 속성 값을 매개 변수에 매핑합니다. 자세한 내용은 아래 예제를 참조하십시오.

엔터티 형식의 수정 함수를 저장 프로시저에 매핑하는 방법에 대 한 자세한 내용은 ModificationFunctionMapping 요소 (MSL) 및 연습을 참조 하세요. 저장 프로시저에 엔터티 매핑

**AssociationEnd** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.

-   ScalarProperty

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **AssociationEnd** 요소에 적용할 수 있는 특성에 대해 설명 합니다.

| 특성 이름     | 필수 여부 | 값                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **AssociationSet** | 예         | 매핑되는 연결의 이름                                                                                                                                 |
| **From**           | 예         | 매핑되는 연결에 해당 하는 탐색 속성의 **Fromrole** 특성 값입니다. 자세한 내용은 NavigationProperty 요소 (CSDL)를 참조 하세요. |
| **수행할 작업**             | 예         | 매핑되는 연결에 해당 하는 탐색 속성의 **Torole** 특성 값입니다. 자세한 내용은 NavigationProperty 요소 (CSDL)를 참조 하세요.   |

### <a name="example"></a>예제

다음과 같은 개념적 모델 엔터티 형식이 있다고 가정합니다.

``` xml
 <EntityType Name="Course">
   <Key>
     <PropertyRef Name="CourseID" />
   </Key>
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" MaxLength="100"
             FixedLength="false" Unicode="true" />
   <Property Type="Int32" Name="Credits" Nullable="false" />
   <NavigationProperty Name="Department"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Course" ToRole="Department" />
 </EntityType>
```

또한 다음과 같은 저장 프로시저가 있다고 가정합니다.

``` SQL
 CREATE PROCEDURE [dbo].[UpdateCourse]
                                @CourseID int,
                                @Title nvarchar(50),
                                @Credits int,
                                @DepartmentID int
                                AS
                                UPDATE Course SET Title=@Title,
                                                              Credits=@Credits,
                                                              DepartmentID=@DepartmentID
                                WHERE CourseID=@CourseID;
```

@No__t-0 엔터티의 업데이트 함수를이 저장 프로시저에 매핑하려면 **DepartmentID** 매개 변수에 값을 제공 해야 합니다. `DepartmentID`의 값은 엔터티 형식의 속성에 해당하지 않으며, 여기에 표시된 매핑을 사용하는 독립 연결에 포함되어 있습니다.

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
 </AssociationSetMapping>
```

다음 코드에서는 **FK @ no__t-3course @ no__t-4Department** 연결의 **DepartmentID** 속성을 **UpdateCourse** 저장 프로시저에 매핑하는 데 사용 되는 **AssociationEnd** 요소를 보여 줍니다. **강좌** 엔터티 형식이 매핑됨):

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdateCourse">
         <AssociationEnd AssociationSet="FK_Course_Department"
                         From="Course" To="Department">
           <ScalarProperty Name="DepartmentID"
                           ParameterName="DepartmentID"
                           Version="Current" />
         </AssociationEnd>
         <ScalarProperty Name="Credits" ParameterName="Credits"
                         Version="Current" />
         <ScalarProperty Name="Title" ParameterName="Title"
                         Version="Current" />
         <ScalarProperty Name="CourseID" ParameterName="CourseID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="associationsetmapping-element-msl"></a>AssociationSetMapping 요소(MSL)

MSL (매핑 사양 언어)의 **AssociationSetMapping** 요소는 개념적 모델의 연결과 기본 데이터베이스의 테이블 열 간의 매핑을 정의 합니다.

개념적 모델의 연결은 기본 데이터베이스의 기본 및 외래 키 열을 나타내는 속성이 있는 형식입니다. **AssociationSetMapping** 요소는 두 개의 endproperty 요소를 사용 하 여 데이터베이스의 연결 유형 속성과 열 간의 매핑을 정의 합니다. 조건 요소를 사용 하 여 이러한 매핑에 조건을 입력할 수 있습니다. 연결에 대 한 삽입, 업데이트 및 삭제 함수를 ModificationFunctionMapping 요소를 사용 하 여 데이터베이스의 저장 프로시저에 매핑합니다. QueryView 요소의 Entity SQL 문자열을 사용 하 여 연결과 테이블 열 간의 읽기 전용 매핑을 정의 합니다.

> [!NOTE]
> 개념적 모델의 연결에 대해 참조 제약 조건이 정의 된 경우 **AssociationSetMapping** 요소를 사용 하 여 연결을 매핑할 필요가 없습니다. 참조 제약 조건이 있는 연결에 대해 **AssociationSetMapping** 요소가 있으면 **AssociationSetMapping** 요소에 정의 된 매핑이 무시 됩니다. 자세한 내용은 참조를 참조 하세요.

**AssociationSetMapping** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.

-   QueryView (0 개 또는 1 개)
-   EndProperty (0 개 또는 2 개)
-   조건 (0 개 이상)
-   ModificationFunctionMapping (0 개 또는 1 개)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **AssociationSetMapping** 요소에 적용 될 수 있는 특성을 설명 합니다.

| 특성 이름     | 필수 여부 | 값                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| **이름**           | 예         | 매핑되는 개념적 모델 연결 집합의 이름                      |
| **TypeName**       | 아니요          | 매핑되는 개념적 모델 연결 형식의 네임스페이스로 한정된 이름 |
| **StoreEntitySet** | 아니요          | 매핑되는 테이블의 이름                                                 |

### <a name="example"></a>예제

다음 예에서는 개념적 모델에 설정 된 **FK @ no__t-2ab@ no__t-3Department** association이 데이터베이스의 **과정** 테이블에 매핑되는 **AssociationSetMapping** 요소를 보여 줍니다. 연결 형식 속성과 테이블 열 간의 매핑은 자식 **Endproperty** 요소에 지정 됩니다.

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

## <a name="complexproperty-element-msl"></a>ComplexProperty 요소(MSL)

MSL (매핑 사양 언어)의 **complexproperty** 요소는 개념적 모델 엔터티 형식의 복합 형식 속성과 기본 데이터베이스의 테이블 열 간의 매핑을 정의 합니다. 속성-열 매핑은 자식 ScalarProperty 요소에 지정 됩니다.

**ComplexType** property 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.

-   ScalarProperty (0 개 이상)
-   **Complexproperty** (0 개 이상)
-   ComplextTypeMapping (0 개 이상)
-   조건 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Complexproperty** 요소에 적용 되는 특성에 대해 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| **이름**       | 예         | 개념적 모델에서 매핑되는 엔터티 형식의 복합 속성 이름입니다. |
| **TypeName**   | 아니요          | 개념적 모델 속성 형식의 네임스페이스로 한정된 이름입니다.                              |

### <a name="example"></a>예제

다음 예제는 School 모델을 기반으로 합니다. 다음 복합 형식이 개념적 모델에 추가되었습니다.

``` xml
 <ComplexType Name="FullName">
   <Property Type="String" Name="LastName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
   <Property Type="String" Name="FirstName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
 </ComplexType>
```

**Person** 엔터티 형식의 **LastName** 및 **FirstName** 속성은 하나의 복합 속성인 **이름**으로 대체 되었습니다.

``` xml
 <EntityType Name="Person">
   <Key>
     <PropertyRef Name="PersonID" />
   </Key>
   <Property Name="PersonID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="HireDate" Type="DateTime" />
   <Property Name="EnrollmentDate" Type="DateTime" />
   <Property Name="Name" Type="SchoolModel.FullName" Nullable="false" />
 </EntityType>
```

다음 MSL은 **Name** 속성을 기본 데이터베이스의 열에 매핑하는 데 사용 되는 **complexproperty** 요소를 보여 줍니다.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
       <ComplexProperty Name="Name" TypeName="SchoolModel.FullName">
         <ScalarProperty Name="FirstName" ColumnName="FirstName" />
         <ScalarProperty Name="LastName" ColumnName="LastName" />  
       </ComplexProperty>
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="complextypemapping-element-msl"></a>ComplexTypeMapping 요소(MSL)

MSL (매핑 사양 언어)의 **Complextypemapping** 요소는 resultmapping 요소의 자식 이며, 다음과 같은 경우 개념적 모델의 function import와 기본 데이터베이스의 저장 프로시저 간 매핑을 정의 합니다. true입니다.

-   Function Import가 개념적 복합 형식을 반환하는 경우
-   저장 프로시저에서 반환되는 열 이름이 복합 형식의 속성 이름과 정확히 일치하지 않는 경우

기본적으로 저장 프로시저에서 반환되는 열과 복합 형식 간의 매핑은 열 및 속성 이름을 기반으로 합니다. 열 이름이 속성 이름과 정확히 일치 하지 않는 경우에는 **Complextypemapping** 요소를 사용 하 여 매핑을 정의 해야 합니다. 기본 매핑의 예는 FunctionImportMapping 요소 (MSL)를 참조 하세요.

**Complextypemapping** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.

-   ScalarProperty (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Complextypemapping** 요소에 적용할 수 있는 특성에 대해 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| **TypeName**   | 예         | 매핑되는 복합 형식의 네임스페이스로 한정된 이름입니다. |

### <a name="example"></a>예제

다음과 같은 저장 프로시저가 있다고 가정합니다.

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

또한 다음과 같은 개념적 모델 복합 형식이 있다고 가정합니다.

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

이전 복합 형식의 인스턴스를 반환 하는 function import를 만들려면 저장 프로시저에서 반환 되는 열과 엔터티 형식 간의 매핑을 **Complextypemapping** 요소에 정의 해야 합니다.

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <ComplexTypeMapping TypeName="SchoolModel.GradeInfo">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </ComplexTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="condition-element-msl"></a>Condition 요소(MSL)

MSL (매핑 사양 언어)의 **Condition** 요소는 개념적 모델과 기본 데이터베이스 간의 매핑에 대 한 조건을 배치 합니다. XML 노드 내에 정의 된 매핑은 자식 **조건** 요소에 지정 된 모든 조건이 충족 되는 경우 유효 합니다. 그렇지 않으면 매핑이 유효하지 않습니다. 예를 들어 MappingFragment 요소에 하나 이상의 **Condition** 자식 요소가 포함 된 경우 **MappingFragment** 노드 내에 정의 된 매핑은 자식 **조건** 요소의 모든 조건이 충족 되는 경우에만 유효 합니다.

각 조건은 이름 ( **name** 특성으로 지정 된 개념적 모델 엔터티 속성 **의 이름)** 또는 **columnname** ( **columnname** 특성으로 지정 된 데이터베이스의 열 이름) 중 하나에 적용 될 수 있습니다. **Name** 특성이 설정 되 면 엔터티 속성 값을 기준으로 조건이 검사 됩니다. **ColumnName** 특성이 설정 되 면 열 값을 기준으로 조건이 검사 됩니다. **Condition** 요소에는 **Name** 또는 **ColumnName** 특성 중 하나만 지정할 수 있습니다.

> [!NOTE]
> FunctionImportMapping 요소 내에서 **Condition** 요소를 사용 하는 경우에는 **Name** 특성만 적용할 수 없습니다.

**Condition** 요소는 다음 요소의 자식일 수 있습니다.

-   AssociationSetMapping
-   ComplexProperty
-   EntitySetMapping
-   MappingFragment
-   EntityTypeMapping

**Condition** 요소는 자식 요소를 가질 수 없습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Condition** 요소에 적용할 수 있는 특성에 대해 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ColumnName** | 아니요          | 조건을 확인하는 데 사용되는 값이 있는 테이블 열의 이름입니다.                                                                                                                                                                                                                   |
| **IsNull**     | 아니요          | **True** 또는 **False**입니다. 값이 **true** 이 고 열 값이 **null**이거나 값이 **False** 이 고 열 값이 **null**이 아닌 경우 조건은 true입니다. 그렇지 않으면 조건이 false입니다. <br/> **IsNull** 및 **Value** 특성은 동시에 사용할 수 없습니다. |
| **Value**      | 아니요          | 열 값과 비교할 값입니다. 값이 동일하면 조건이 true입니다. 그렇지 않으면 조건이 false입니다. <br/> **IsNull** 및 **Value** 특성은 동시에 사용할 수 없습니다.                                                                       |
| **이름**       | 아니요          | 조건을 확인하는 데 사용되는 값이 있는 개념적 모델 엔터티 속성의 이름입니다. <br/> **Condition** 요소가 FunctionImportMapping 요소 내에서 사용 되는 경우에는이 특성을 적용할 수 없습니다.                                                                           |

### <a name="example"></a>예제

다음 예제에서는 **조건** 요소를 **MappingFragment** 요소의 자식으로 보여 줍니다. **HireDate** 가 null이 고 **EnrollmentDate** 가 null 인 경우 **SchoolModel** 형식과 **Person** 테이블의 **PersonID** 및 **HireDate** 열 간에 데이터가 매핑됩니다. **EnrollmentDate** 가 null이 고 **HireDate** 가 null 인 경우 **SchoolModel** 형식과 **Person** 테이블의 **PersonID** 및 **등록** 열 간에 데이터가 매핑됩니다.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="deletefunction-element-msl"></a>DeleteFunction 요소(MSL)

MSL (매핑 사양 언어)의 **deletefunction** 요소는 개념적 모델의 엔터티 형식 또는 연결에 대 한 삭제 함수를 기본 데이터베이스의 저장 프로시저에 매핑합니다. 수정 함수가 매핑되는 저장 프로시저는 스토리지 모델에서 선언되어야 합니다. 자세한 내용은 Function 요소 (SSDL)를 참조 하세요.

> [!NOTE]
> 엔터티 형식의 삽입, 업데이트 또는 삭제 작업을 모두 저장 프로시저에 매핑하지 않은 경우 런타임에 실행 하면 매핑되지 않은 작업이 실패 하 고 UpdateException이 throw 됩니다.

### <a name="deletefunction-applied-to-entitytypemapping"></a>EntityTypeMapping에 적용되는 DeleteFunction

EntityTypeMapping 요소에 적용 될 때 **Deletefunction** 요소는 개념적 모델의 엔터티 형식에 대 한 삭제 함수를 저장 프로시저에 매핑합니다.

**Deletefunction** 요소는 **entitytypemapping** 요소에 적용 될 때 다음과 같은 자식 요소를 포함할 수 있습니다.

-   AssociationEnd (0 개 이상)
-   ComplexProperty (0 개 이상)
-   ScarlarProperty (0 개 이상)

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Entitytypemapping** 요소에 적용 될 때 **deletefunction** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름            | 필수 여부 | 값                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | 예         | 삭제 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다. 저장 프로시저는 스토리지 모델에서 선언해야 합니다. |
| **RowsAffectedParameter** | 아니요          | 영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.                                                                               |

#### <a name="example"></a>예제

다음 예는 School 모델을 기반으로 하며 **Person** 엔터티 형식의 delete 함수를 **deletefunction** 저장 프로시저에 매핑하는 **deletefunction** 요소를 보여 줍니다. **Deleteperson** 저장 프로시저는 저장소 모델에서 선언 됩니다.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="deletefunction-applied-to-associationsetmapping"></a>AssociationSetMapping에 적용된 DeleteFunction

**Deletefunction** 요소는 AssociationSetMapping 요소에 적용 될 때 개념적 모델에 있는 연결의 delete 함수를 저장 프로시저에 매핑합니다.

**Deletefunction** 요소는 **AssociationSetMapping** 요소에 적용 될 때 다음과 같은 자식 요소를 포함할 수 있습니다.

-   EndProperty

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **AssociationSetMapping** 요소에 적용 될 때 **deletefunction** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름            | 필수 여부 | 값                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | 예         | 삭제 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다. 저장 프로시저는 스토리지 모델에서 선언해야 합니다. |
| **RowsAffectedParameter** | 아니요          | 영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.                                                                               |

#### <a name="example"></a>예제

다음 예는 School 모델을 기반으로 하며 **CourseInstructor** 연결의 delete 함수를 **DeleteCourseInstructor** 저장 프로시저에 매핑하는 데 사용 되는 **deletefunction** 요소를 보여 줍니다. **DeleteCourseInstructor** 저장 프로시저는 저장소 모델에서 선언 됩니다.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="endproperty-element-msl"></a>EndProperty 요소(MSL)

MSL (매핑 사양 언어)의 **Endproperty** 요소는 개념적 모델 연결의 끝 또는 수정 함수와 기본 데이터베이스 간의 매핑을 정의 합니다. 속성-열 매핑은 자식 ScalarProperty 요소에 지정 됩니다.

**Endproperty** 요소를 사용 하 여 개념적 모델 연결의 끝에 대 한 매핑을 정의할 경우이 요소는 AssociationSetMapping 요소의 자식입니다. **Endproperty** 요소를 사용 하 여 개념적 모델 연결의 수정 함수에 대 한 매핑을 정의할 경우이 요소는 insertfunction 요소 또는 deletefunction 요소의 자식입니다.

**Endproperty** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.

-   ScalarProperty (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Endproperty** 요소에 적용 되는 특성에 대해 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                 |
|:---------------|:------------|:------------------------------------------------------|
| 이름           | 예         | 매핑되는 연결 끝의 이름 |

### <a name="example"></a>예제

다음 예에서는 개념적 모델의 **FK @ no__t-2Course; no__t-3Department** 연결이 데이터베이스의 **과정** 테이블에 매핑되는 요소를 보여 줍니다. 연결 형식 속성과 테이블 열 간의 매핑은 자식 **Endproperty** 요소에 지정 됩니다.

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

### <a name="example"></a>예제

다음 예에서는 연결 (**CourseInstructor**)의 insert 및 delete 함수를 기본 데이터베이스의 저장 프로시저에 매핑하는 **endproperty** 요소를 보여 줍니다. 매핑되는 함수는 스토리지 모델에서 선언됩니다.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="entitycontainermapping-element-msl"></a>EntityContainerMapping 요소(MSL)

MSL (매핑 사양 언어)의 **EntityContainerMapping** 요소는 개념적 모델의 엔터티 컨테이너를 저장소 모델의 엔터티 컨테이너에 매핑합니다. **EntityContainerMapping** 요소는 Mapping 요소의 자식입니다.

**EntityContainerMapping** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   EntitySetMapping (0 개 이상)
-   AssociationSetMapping (0 개 이상)
-   FunctionImportMapping (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **EntityContainerMapping** 요소에 적용 될 수 있는 특성을 설명 합니다.

| 특성 이름            | 필수 여부 | 값                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StorageModelContainer** | 예         | 매핑되는 스토리지 모델 엔터티 컨테이너의 이름입니다.                                                                                                                                                                                     |
| **CdmEntityContainer**    | 예         | 매핑되는 개념적 모델 엔터티 컨테이너의 이름입니다.                                                                                                                                                                                  |
| **GenerateUpdateViews**   | 아니요          | **True** 또는 **False**입니다. **False**이면 업데이트 보기가 생성 되지 않습니다. 데이터를 성공적으로 왕복 하지 못할 수 있으므로 읽기 전용 매핑이 잘못 된 경우이 특성을 **False** 로 설정 해야 합니다. <br/> 기본값은 **True**입니다. |

### <a name="example"></a>예제

다음 예제에서는 **SchoolModelEntities** 컨테이너 (개념적 모델 엔터티 컨테이너)를 **SchoolModelStoreContainer** 컨테이너 (저장소 모델 엔터티)에 매핑하는 **EntityContainerMapping** 요소를 보여 줍니다. 컨테이너):

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolModelEntities">
   <EntitySetMapping Name="Courses">
     <EntityTypeMapping TypeName="c.Course">
       <MappingFragment StoreEntitySet="Course">
         <ScalarProperty Name="CourseID" ColumnName="CourseID" />
         <ScalarProperty Name="Title" ColumnName="Title" />
         <ScalarProperty Name="Credits" ColumnName="Credits" />
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments">
     <EntityTypeMapping TypeName="c.Department">
       <MappingFragment StoreEntitySet="Department">
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         <ScalarProperty Name="Name" ColumnName="Name" />
         <ScalarProperty Name="Budget" ColumnName="Budget" />
         <ScalarProperty Name="StartDate" ColumnName="StartDate" />
         <ScalarProperty Name="Administrator" ColumnName="Administrator" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
 </EntityContainerMapping>
```

## <a name="entitysetmapping-element-msl"></a>EntitySetMapping 요소(MSL)

MSL (매핑 사양 언어)의 **EntitySetMapping** 요소는 개념적 모델 엔터티 집합의 모든 형식을 저장소 모델의 엔터티 집합에 매핑합니다. 개념적 모델의 엔터티 집합은 동일한 형식 (및 파생 형식)의 엔터티 인스턴스에 대 한 논리적 컨테이너입니다. 저장소 모델의 엔터티 집합은 기본 데이터베이스의 테이블 또는 뷰를 나타냅니다. 개념적 모델 엔터티 집합은 **EntitySetMapping** 요소의 **Name** 특성 값으로 지정 됩니다. 매핑되는 테이블 또는 뷰는 각 자식 MappingFragment 요소 또는 **EntitySetMapping** 요소 자체의 자동 연결 **entityset** 특성에 의해 지정 됩니다.

**EntitySetMapping** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.

-   EntityTypeMapping (0 개 이상)
-   QueryView (0 개 또는 1 개)
-   MappingFragment (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **EntitySetMapping** 요소에 적용 될 수 있는 특성을 설명 합니다.

| 특성 이름           | 필수 여부 | 값                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**                 | 예         | 매핑되는 개념적 모델 엔터티 집합의 이름입니다.                                                                                                                                                             |
| **TypeName** **1**       | 아니요          | 매핑되는 개념적 모델 엔터티 형식의 이름입니다.                                                                                                                                                            |
| 을 (를) **entityset** **1** | 아니요          | 매핑되는 대상 스토리지 모델 엔터티 집합의 이름입니다.                                                                                                                                                             |
| **MakeColumnsDistinct**  | 아니요          | 고유 행만 반환 되는지 여부에 따라 **True** 또는 **False** 입니다. <br/> 이 특성이 **True**로 설정 된 경우 EntityContainerMapping 요소의 **generateupdateviews** 특성을 **False**로 설정 해야 합니다. |

 

**1** Entitytypemapping 및 MappingFragment 자식 요소 대신 **TypeName** 및 **valentityset** 특성을 사용 하 여 단일 엔터티 형식을 단일 테이블에 매핑할 수 있습니다.

### <a name="example"></a>예제

다음 예에서는 개념적 모델의 **강좌** 엔터티 집합에 있는 세 가지 형식 (기본 형식 및 두 개의 파생 형식)을 기본 데이터베이스에 있는 세 개의 다른 테이블에 매핑하는 **EntitySetMapping** 요소를 보여 줍니다. 테이블은 각 **MappingFragment** 요소의 **entityset** 특성에 의해 지정 됩니다.

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.Course)">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnlineCourse)">
     <MappingFragment StoreEntitySet="OnlineCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="URL" ColumnName="URL" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnsiteCourse)">
     <MappingFragment StoreEntitySet="OnsiteCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Time" ColumnName="Time" />
       <ScalarProperty Name="Days" ColumnName="Days" />
       <ScalarProperty Name="Location" ColumnName="Location" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="entitytypemapping-element-msl"></a>EntityTypeMapping 요소(MSL)

MSL (매핑 사양 언어)의 **Entitytypemapping** 요소는 개념적 모델의 엔터티 형식과 기본 데이터베이스의 테이블 또는 뷰 간의 매핑을 정의 합니다. 개념적 모델 엔터티 형식과 기본 데이터베이스 테이블 또는 뷰에 대 한 자세한 내용은 EntityType 요소 (CSDL) 및 EntitySet 요소 (SSDL)를 참조 하세요. 매핑되는 개념적 모델 엔터티 형식은 **Entitytypemapping** 요소의 **TypeName** 특성에 의해 지정 됩니다. 매핑되는 테이블 또는 뷰는 자식 MappingFragment 요소의 **entityset** 특성에 의해 지정 됩니다.

ModificationFunctionMapping 자식 요소를 사용 하 여 엔터티 형식의 삽입, 업데이트 또는 삭제 함수를 데이터베이스의 저장 프로시저에 매핑할 수 있습니다.

**Entitytypemapping** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.

-   MappingFragment (0 개 이상)
-   ModificationFunctionMapping (0 개 또는 1 개)
-   ScalarProperty
-   조건

> [!NOTE]
> **MappingFragment** 및 **ModificationFunctionMapping** 요소는 **entitytypemapping** 요소의 자식 요소일 수 없습니다.


> [!NOTE]
> **ScalarProperty** 및 **Condition** 요소는 functionimportmapping 요소 내에서 사용 될 때 **entitytypemapping** 요소의 자식 요소일 수 있습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Entitytypemapping** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **TypeName**   | 예         | 매핑되는 개념적 모델 엔터티 형식의 네임스페이스로 한정된 이름입니다. <br/> 해당 형식이 추상 또는 파생 형식이면 이 값은 `IsOfType(Namespace-qualified_type_name)`이어야 합니다. |

### <a name="example"></a>예제

다음 예제에서는 두 개의 자식 **Entitytypemapping** 요소가 있는 EntitySetMapping 요소를 보여 줍니다. 첫 번째 **Entitytypemapping** 요소에서 **SchoolModel** 엔터티 형식은 **person** 테이블에 매핑됩니다. 두 번째 **Entitytypemapping** 요소에서 **SchoolModel** 형식의 업데이트 기능은 데이터베이스의 저장 프로시저인 **updateperson**에 매핑됩니다.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate" ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>예제

다음 예제에서는 루트 형식이 추상인 형식 계층 구조의 매핑을 보여 줍니다. **TypeName** 특성에는 `IsOfType` 구문을 사용 합니다.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="functionimportmapping-element-msl"></a>FunctionImportMapping 요소(MSL)

MSL (매핑 사양 언어)의 **Functionimportmapping** 요소는 개념적 모델의 function import와 기본 데이터베이스의 저장 프로시저 또는 함수 간 매핑을 정의 합니다. Function Import는 개념적 모델에서 선언해야 하고 저장 프로시저는 스토리지 모델에서 선언해야 합니다. 자세한 내용은 FunctionImport 요소 (CSDL) 및 Function 요소 (SSDL)를 참조 하세요.

> [!NOTE]
> 기본적으로, Function Import에서 개념적 모델 엔터티 형식이나 복합 형식을 반환하는 경우에는 기본 저장 프로시저에서 반환된 열 이름이 개념적 모델 형식의 속성 이름과 정확히 일치해야 합니다. 열 이름이 속성 이름과 정확히 일치 하지 않는 경우에는이 매핑을 ResultMapping 요소에 정의 해야 합니다.

**Functionimportmapping** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.

-   ResultMapping (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Functionimportmapping** 요소에 적용할 수 있는 특성에 대해 설명 합니다.

| 특성 이름         | 필수 여부 | 값                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| **FunctionImportName** | 예         | 매핑되는 개념적 모델의 Function Import 이름           |
| **FunctionName**       | 예         | 매핑되는 스토리지 모델 함수의 네임스페이스로 한정된 이름 |

### <a name="example"></a>예제

다음 예제는 School 모델을 기반으로 합니다. 다음은 스토리지 모델의 함수입니다.

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

다음은 개념적 모델의 Function Import입니다.

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

다음 예제에서는 위의 함수와 함수 가져오기를 서로 매핑하는 데 사용 되는 **Functionimportmapping** 요소를 보여 줍니다.

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a>InsertFunction 요소(MSL)

MSL (매핑 사양 언어)의 **insertfunction** 요소는 개념적 모델의 엔터티 형식 또는 연결에 대 한 삽입 함수를 기본 데이터베이스의 저장 프로시저에 매핑합니다. 수정 함수가 매핑되는 저장 프로시저는 스토리지 모델에서 선언되어야 합니다. 자세한 내용은 Function 요소 (SSDL)를 참조 하세요.

> [!NOTE]
> 엔터티 형식의 삽입, 업데이트 또는 삭제 작업을 모두 저장 프로시저에 매핑하지 않은 경우 런타임에 실행 하면 매핑되지 않은 작업이 실패 하 고 UpdateException이 throw 됩니다.

**Insertfunction** 요소는 ModificationFunctionMapping 요소의 자식일 수 있으며 entitytypemapping 요소 또는 AssociationSetMapping 요소에 적용 될 수 있습니다.

### <a name="insertfunction-applied-to-entitytypemapping"></a>EntityTypeMapping에 적용되는 InsertFunction

EntityTypeMapping 요소에 적용 될 때 **Insertfunction** 요소는 개념적 모델의 엔터티 형식에 대 한 삽입 함수를 저장 프로시저에 매핑합니다.

**Insertfunction** 요소는 **entitytypemapping** 요소에 적용 될 때 다음과 같은 자식 요소를 포함할 수 있습니다.

-   AssociationEnd (0 개 이상)
-   ComplexProperty (0 개 이상)
-   ResultBinding (0 개 또는 1 개)
-   ScarlarProperty (0 개 이상)

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Entitytypemapping** 요소에 적용 될 때 **insertfunction** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름            | 필수 여부 | 값                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | 예         | 삽입 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다. 저장 프로시저는 스토리지 모델에서 선언해야 합니다. |
| **RowsAffectedParameter** | 아니요          | 영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.                                                                               |

#### <a name="example"></a>예제

다음 예는 School 모델을 기반으로 하며 Person 엔터티 형식의 삽입 함수를 **Insertfunction** 저장 프로시저에 매핑하는 데 사용 되는 **insertfunction** 요소를 보여 줍니다. **Insertperson** 저장 프로시저는 저장소 모델에서 선언 됩니다.

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
### <a name="insertfunction-applied-to-associationsetmapping"></a>AssociationSetMapping에 적용된 InsertFunction

AssociationSetMapping 요소에 적용 될 때 **insertfunction** 요소는 개념적 모델에 있는 연결의 삽입 함수를 저장 프로시저에 매핑합니다.

**Insertfunction** 요소는 **AssociationSetMapping** 요소에 적용 될 때 다음과 같은 자식 요소를 포함할 수 있습니다.

-   EndProperty

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **AssociationSetMapping** 요소에 적용 될 때 **insertfunction** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름            | 필수 여부 | 값                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | 예         | 삽입 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다. 저장 프로시저는 스토리지 모델에서 선언해야 합니다. |
| **RowsAffectedParameter** | 아니요          | 영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.                                                                               |

#### <a name="example"></a>예제

다음 예는 School 모델을 기반으로 하며 **CourseInstructor** 연결의 insert 함수를 **InsertCourseInstructor** 저장 프로시저에 매핑하는 데 사용 되는 **insertfunction** 요소를 보여 줍니다. **InsertCourseInstructor** 저장 프로시저는 저장소 모델에서 선언 됩니다.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="mapping-element-msl"></a>Mapping 요소(MSL)

MSL (매핑 사양 언어)의 **mapping** 요소에는 개념적 모델에 정의 된 개체를 데이터베이스에 매핑하기 위한 정보 (저장소 모델에 설명 된 대로)가 포함 되어 있습니다. 자세한 내용은 CSDL 사양 및 SSDL 사양을 참조 하세요.

매핑 **요소는** 매핑 사양에 대 한 루트 요소입니다. 매핑 사양에 대 한 XML 네임 스페이스는 https://schemas.microsoft.com/ado/2009/11/mapping/cs 입니다.

Mapping 요소는 다음에 나열된 순서대로 자식 요소를 포함할 수 있습니다.

-   별칭 (0 개 이상)
-   EntityContainerMapping (정확히 1 개)

MSL에서 참조하는 개념적 모델 형식과 스토리지 모델 형식의 이름은 해당 네임스페이스 이름으로 한정되어야 합니다. 개념적 모델 네임 스페이스 이름에 대 한 자세한 내용은 Schema 요소 (CSDL)를 참조 하세요. 저장소 모델 네임 스페이스 이름에 대 한 자세한 내용은 Schema 요소 (SSDL)를 참조 하세요. MSL에서 사용 되는 네임 스페이스에 대 한 별칭은 Alias 요소를 사용 하 여 정의할 수 있습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Mapping** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                 |
|:---------------|:------------|:------------------------------------------------------|
| **스페이스바**      | 예         | **C-S**. 이 값은 고정된 값이므로 변경할 수 없습니다. |

### <a name="example"></a>예제

다음 예에서는 School 모델의 일부를 기반으로 하는 **Mapping** 요소를 보여 줍니다. School 모델에 대 한 자세한 내용은 빠른 시작 (Entity Framework)을 참조 하세요.

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="mappingfragment-element-msl"></a>MappingFragment 요소(MSL)

MSL (매핑 사양 언어)의 **MappingFragment** 요소는 개념적 모델 엔터티 형식의 속성과 데이터베이스의 테이블 또는 뷰 간의 매핑을 정의 합니다. 개념적 모델 엔터티 형식과 기본 데이터베이스 테이블 또는 뷰에 대 한 자세한 내용은 EntityType 요소 (CSDL) 및 EntitySet 요소 (SSDL)를 참조 하세요. **MappingFragment** 는 entitytypemapping 요소 또는 EntitySetMapping 요소의 자식 요소일 수 있습니다.

**MappingFragment** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.

-   ComplexType (0 개 이상)
-   ScalarProperty (0 개 이상)
-   조건 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **MappingFragment** 요소에 적용 될 수 있는 특성을 설명 합니다.

| 특성 이름          | 필수 여부 | 값                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StoreEntitySet**      | 예         | 매핑되는 테이블 또는 뷰의 이름입니다.                                                                                                                                                                           |
| **MakeColumnsDistinct** | 아니요          | 고유 행만 반환 되는지 여부에 따라 **True** 또는 **False** 입니다. <br/> 이 특성이 **True**로 설정 된 경우 EntityContainerMapping 요소의 **generateupdateviews** 특성을 **False**로 설정 해야 합니다. |

### <a name="example"></a>예제

다음 예제에서는 **MappingFragment** 요소를 **entitytypemapping** 요소의 자식으로 보여 줍니다. 이 예제에서 개념적 모델의 **과정** 유형 속성은 데이터베이스의 **과정** 테이블 열에 매핑됩니다.

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>예제

다음 예제에서는 **MappingFragment** 요소를 **EntitySetMapping** 요소의 자식으로 보여 줍니다. 위의 예제와 같이 개념적 모델의 **과정** 유형 속성은 데이터베이스의 **과정** 테이블 열에 매핑됩니다.

``` xml
 <EntitySetMapping Name="Courses" TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
 </EntitySetMapping>
```

## <a name="modificationfunctionmapping-element-msl"></a>ModificationFunctionMapping 요소(MSL)

MSL (매핑 사양 언어)의 **ModificationFunctionMapping** 요소는 개념적 모델 엔터티 형식의 삽입, 업데이트 및 삭제 함수를 기본 데이터베이스의 저장 프로시저에 매핑합니다. **ModificationFunctionMapping** 요소는 개념적 모델의 다 대 다 연결에 대 한 삽입 및 삭제 함수를 기본 데이터베이스의 저장 프로시저에 매핑할 수도 있습니다. 수정 함수가 매핑되는 저장 프로시저는 스토리지 모델에서 선언되어야 합니다. 자세한 내용은 Function 요소 (SSDL)를 참조 하세요.

> [!NOTE]
> 엔터티 형식의 삽입, 업데이트 또는 삭제 작업을 모두 저장 프로시저에 매핑하지 않은 경우 런타임에 실행 하면 매핑되지 않은 작업이 실패 하 고 UpdateException이 throw 됩니다.


> [!NOTE]
> 상속 계층 구조의 한 엔터티에 대한 수정 함수가 저장 프로시저에 매핑된 경우 계층 구조의 모든 형식에 대한 수정 함수가 저장 프로시저에 매핑되어야 합니다.

**ModificationFunctionMapping** 요소는 entitytypemapping 요소 또는 AssociationSetMapping 요소의 자식일 수 있습니다.

**ModificationFunctionMapping** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.

-   DeleteFunction (0 개 또는 1 개)
-   InsertFunction (0 개 또는 1 개)
-   UpdateFunction (0 개 또는 1 개)

**ModificationFunctionMapping** 요소에 적용할 수 있는 특성이 없습니다.

### <a name="example"></a>예제

다음 예제에서는 School 모델의 **인물** 엔터티 집합에 대 한 엔터티 집합 매핑을 보여 줍니다. **Person** 엔터티 형식에 대 한 열 매핑 외에도 **person** 형식의 삽입, 업데이트 및 삭제 함수에 대 한 매핑이 표시 됩니다. 매핑되는 함수는 스토리지 모델에서 선언됩니다.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>예제

다음 예에서는 School 모델의 **CourseInstructor** 연결 집합에 대 한 연결 집합 매핑을 보여 줍니다. **CourseInstructor** 연결에 대 한 열 매핑 외에도 **CourseInstructor** 연결의 insert 및 delete 함수 매핑이 표시 됩니다. 매핑되는 함수는 스토리지 모델에서 선언됩니다.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```
 

 

## <a name="queryview-element-msl"></a>QueryView 요소(MSL)

MSL (매핑 사양 언어)의 **QueryView** 요소는 개념적 모델의 엔터티 형식 또는 연결 및 기본 데이터베이스의 테이블 간에 읽기 전용 매핑을 정의 합니다. 매핑은 저장소 모델에 대해 평가 되는 Entity SQL 쿼리로 정의 되며, 개념적 모델에서 엔터티 또는 연결을 기준으로 결과 집합을 표현 합니다. 쿼리 뷰는 읽기 전용이므로 표준 업데이트 명령을 사용하여 쿼리 뷰에서 정의된 형식을 업데이트할 수 없습니다. 수정 함수를 사용하여 이러한 형식을 업데이트할 수 있습니다. 자세한 내용은 방법: 수정 함수를 저장 프로시저에 매핑합니다.

> [!NOTE]
> **QueryView** 요소에는 **GroupBy**, group 집계 또는 탐색 속성을 포함 하는 Entity SQL 식이 지원 되지 않습니다.

 

**QueryView** 요소는 EntitySetMapping 요소 또는 AssociationSetMapping 요소의 자식일 수 있습니다. 쿼리 뷰는 앞의 자식 요소의 경우 개념적 모델의 엔터티에 대한 읽기 전용 매핑을 정의하고, 뒤의 자식 요소의 경우 개념적 모델의 연결에 대한 읽기 전용 매핑을 정의합니다.

> [!NOTE]
> **AssociationSetMapping** 요소가 참조 제약 조건이 있는 연결에 대 한 것 이면 **AssociationSetMapping** 요소가 무시 됩니다. 자세한 내용은 참조를 참조 하세요.

**QueryView** 요소에는 자식 요소가 있을 수 없습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **QueryView** 요소에 적용 될 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **TypeName**   | 아니요          | 쿼리 뷰에 의해 매핑되는 개념적 모델 형식의 이름입니다. |

### <a name="example"></a>예제

다음 예에서는 **QueryView** 요소를 **EntitySetMapping** 요소의 자식으로 보여 주고 School 모델에서 **부서** 엔터티 형식에 대 한 쿼리 뷰 매핑을 정의 합니다.

``` xml
 <EntitySetMapping Name="Departments" >
   <QueryView>
     SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                         d.Name,
                                         d.Budget,
                                         d.StartDate)
     FROM SchoolModelStoreContainer.Department AS d
     WHERE d.Budget > 150000
   </QueryView>
 </EntitySetMapping>
```

이 쿼리는 저장소 모델에서 **부서** 유형의 멤버 하위 집합만 반환 하므로 School 모델의 **부서** 유형은 다음과 같이이 매핑을 기반으로 수정 되었습니다.

``` xml
 <EntityType Name="Department">
   <Key>
     <PropertyRef Name="DepartmentID" />
   </Key>
   <Property Type="Int32" Name="DepartmentID" Nullable="false" />
   <Property Type="String" Name="Name" Nullable="false"
             MaxLength="50" FixedLength="false" Unicode="true" />
   <Property Type="Decimal" Name="Budget" Nullable="false"
             Precision="19" Scale="4" />
   <Property Type="DateTime" Name="StartDate" Nullable="false" />
   <NavigationProperty Name="Courses"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Department" ToRole="Course" />
 </EntityType>
```

### <a name="example"></a>예제

다음 예제에서는 **QueryView** 요소를 **AssociationSetMapping** 요소의 자식으로 표시 하 고 School 모델에서 `FK_Course_Department` 연결에 대 한 읽기 전용 매핑을 정의 합니다.

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolEntities">
   <EntitySetMapping Name="Courses" >
     <QueryView>
       SELECT VALUE SchoolModel.Course(c.CourseID,
                                       c.Title,
                                       c.Credits)
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments" >
     <QueryView>
       SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                           d.Name,
                                           d.Budget,
                                           d.StartDate)
       FROM SchoolModelStoreContainer.Department AS d
       WHERE d.Budget > 150000
     </QueryView>
   </EntitySetMapping>
   <AssociationSetMapping Name="FK_Course_Department" >
     <QueryView>
       SELECT VALUE SchoolModel.FK_Course_Department(
         CREATEREF(SchoolEntities.Departments, row(c.DepartmentID), SchoolModel.Department),
         CREATEREF(SchoolEntities.Courses, row(c.CourseID)) )
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </AssociationSetMapping>
 </EntityContainerMapping>
```
 
### <a name="comments"></a>주석

다음 시나리오가 가능하도록 쿼리 뷰를 정의할 수 있습니다.

-   스토리지 모델에 있는 엔터티의 모든 속성을 포함하지 않는 엔터티를 개념적 모델에서 정의합니다. 여기에는 기본값이 없고 **null** 값을 지원 하지 않는 속성이 포함 됩니다.
-   스토리지 모델의 계산 열을 개념적 모델의 엔터티 형식 속성에 매핑합니다.
-   개념적 모델의 엔터티를 분할하는 데 사용된 조건이 같음을 기반으로 하지 않는 매핑을 정의합니다. **조건** 요소를 사용 하 여 조건부 매핑을 지정 하는 경우 제공 된 조건은 지정 된 값과 같아야 합니다. 자세한 내용은 Condition 요소 (MSL)를 참조 하세요.
-   스토리지 모델의 같은 열을 개념적 모델의 여러 형식에 매핑합니다.
-   여러 형식을 같은 테이블에 매핑합니다.
-   관계형 스키마에서 외래 키를 기반으로 하지 않는 연결을 개념적 모델에서 정의합니다.
-   사용자 지정 비즈니스 논리를 사용하여 개념적 모델에서 속성 값을 설정합니다. 예를 들어 개념적 모델의 부울 값인 **true**값에 데이터 소스의 문자열 값 "T"를 매핑할 수 있습니다.
-   쿼리 결과에 대한 조건부 필터를 정의합니다.
-   스토리지 모델보다 더 적은 제한을 개념적 모델의 데이터에 대해 적용합니다. 예를 들어 매핑되는 열이 **null**값을 지원 하지 않는 경우에도 개념적 모델에서 속성을 nullable로 설정할 수 있습니다.

엔터티에 대한 쿼리 뷰를 정의할 때는 다음 사항을 고려해야 합니다.

-   쿼리 뷰는 읽기 전용입니다. 수정 함수를 사용하여 엔터티를 업데이트할 수만 있습니다.
-   쿼리 뷰로 엔터티 형식을 정의하는 경우 관련된 모든 엔터티도 쿼리 뷰로 정의해야 합니다.
-   다대다 연결을 관계형 스키마의 링크 테이블을 나타내는 저장소 모델의 엔터티에 매핑하는 경우이 링크 테이블의 **AssociationSetMapping** 요소에 **QueryView** 요소를 정의 해야 합니다.
-   쿼리 뷰는 형식 계층 구조의 모든 형식에 대해 정의되어야 합니다. 다음과 같은 방법으로 이 작업을 수행할 수 있습니다.
-   -   계층 구조에 있는 모든 엔터티 형식의 합집합을 반환 하는 단일 Entity SQL 쿼리를 지정 하는 단일 **QueryView** 요소를 사용 합니다.
    -   CASE 연산자를 사용 하 여 특정 조건에 따라 계층 구조에서 특정 엔터티 형식을 반환 하는 단일 Entity SQL 쿼리를 지정 하는 단일 **QueryView** 요소를 사용 합니다.
    -   계층의 특정 형식에 대 한 추가 **QueryView** 요소 사용. 이 경우 **QueryView** 요소의 **TypeName** 특성을 사용 하 여 각 뷰의 엔터티 형식을 지정 합니다.
-   쿼리 뷰를 정의 하는 경우에는 **EntitySetMapping** 요소에 **StorageSetName** 특성을 지정할 수 없습니다.
-   쿼리 뷰가 정의 된 경우 **EntitySetMapping**요소는 **속성** 매핑을 포함할 수 없습니다.

## <a name="resultbinding-element-msl"></a>ResultBinding 요소(MSL)

MSL (매핑 사양 언어)의 **Resultbinding** 요소는 엔터티 형식 수정 함수가의 저장 프로시저에 매핑될 때 저장 프로시저에 의해 반환 되는 열 값을 개념적 모델의 엔터티 속성에 매핑합니다. 기본 데이터베이스입니다. 예를 들어 insert 저장 프로시저에서 identity 열의 값을 반환 하는 경우 **Resultbinding** 요소는 반환 된 값을 개념적 모델의 엔터티 형식 속성에 매핑합니다.

**Resultbinding** 요소는 insertfunction 요소 또는 updatefunction 요소의 자식일 수 있습니다.

**Resultbinding** 요소에는 자식 요소가 있을 수 없습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Resultbinding** 요소에 적용할 수 있는 특성에 대해 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **이름**       | 예         | 매핑되는 개념적 모델의 엔터티 속성 이름입니다. |
| **ColumnName** | 예         | 매핑되는 열의 이름입니다.                                          |

### <a name="example"></a>예제

다음 예는 School 모델을 기반으로 하며 **Person** 엔터티 형식의 삽입 함수를 **insertfunction** 저장 프로시저에 매핑하는 데 사용 되는 **insertfunction** 요소를 보여 줍니다. **Insertperson** 저장 프로시저는 아래와 같이 저장소 모델에서 선언 됩니다. **Resultbinding** 요소는 저장 프로시저 (**NewPersonID**)에서 반환 되는 열 값을 엔터티 형식 속성 (**PersonID**)에 매핑하는 데 사용 됩니다.

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```

다음 Transact-sql은 **Insertperson** 저장 프로시저에 대해 설명 합니다.

``` SQL
 CREATE PROCEDURE [dbo].[InsertPerson]
                                @LastName nvarchar(50),
                                @FirstName nvarchar(50),
                                @HireDate datetime,
                                @EnrollmentDate datetime
                                AS
                                INSERT INTO dbo.Person (LastName,
                                                                             FirstName,
                                                                             HireDate,
                                                                             EnrollmentDate)
                                VALUES (@LastName,
                                               @FirstName,
                                               @HireDate,
                                               @EnrollmentDate);
                                SELECT SCOPE_IDENTITY() as NewPersonID;
```

## <a name="resultmapping-element-msl"></a>ResultMapping 요소(MSL)

MSL (매핑 사양 언어)의 **Resultmapping** 요소는 다음이 true 일 때 개념적 모델의 function import와 기본 데이터베이스의 저장 프로시저 간 매핑을 정의 합니다.

-   Function Import가 개념적 모델 엔터티 형식 또는 복합 형식을 반환하는 경우
-   저장 프로시저에서 반환되는 열 이름이 엔터티 형식 또는 복합 형식의 속성 이름과 정확히 일치하지 않는 경우

기본적으로 저장 프로시저에서 반환되는 열과 엔터티 형식 또는 복합 형식 간의 매핑은 열 및 속성 이름을 기반으로 합니다. 열 이름이 속성 이름과 정확히 일치 하지 않는 경우 **resultmapping** 요소를 사용 하 여 매핑을 정의 해야 합니다. 기본 매핑의 예는 FunctionImportMapping 요소 (MSL)를 참조 하세요.

**Resultmapping** 요소는 functionimportmapping 요소의 자식 요소입니다.

**Resultmapping** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.

-   EntityTypeMapping (0 개 이상)
-   ComplexTypeMapping

**Resultmapping** 요소에는 특성을 적용할 수 없습니다.

### <a name="example"></a>예제

다음과 같은 저장 프로시저가 있다고 가정합니다.

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

또한 다음과 같은 개념적 모델 엔터티 형식이 있다고 가정합니다.

``` xml
 <EntityType Name="StudentGrade">
   <Key>
     <PropertyRef Name="EnrollmentID" />
   </Key>
   <Property Name="EnrollmentID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="CourseID" Type="Int32" Nullable="false" />
   <Property Name="StudentID" Type="Int32" Nullable="false" />
   <Property Name="Grade" Type="Decimal" Precision="3" Scale="2" />
 </EntityType>
```

이전 엔터티 형식의 인스턴스를 반환 하는 function import를 만들려면 저장 프로시저에서 반환 되는 열과 엔터티 형식 간의 매핑이 **Resultmapping** 요소에 정의 되어 있어야 합니다.

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <EntityTypeMapping TypeName="SchoolModel.StudentGrade">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </EntityTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="scalarproperty-element-msl"></a>ScalarProperty 요소(MSL)

MSL (매핑 사양 언어)의 **ScalarProperty** 요소는 개념적 모델 엔터티 형식, 복합 형식 또는 연결의 속성을 기본 데이터베이스의 테이블 열 또는 저장 프로시저 매개 변수에 매핑합니다.

> [!NOTE]
> 수정 함수가 매핑되는 저장 프로시저는 스토리지 모델에서 선언되어야 합니다. 자세한 내용은 Function 요소 (SSDL)를 참조 하세요.

**ScalarProperty** 요소는 다음 요소의 자식일 수 있습니다.

-   MappingFragment
-   InsertFunction
-   UpdateFunction
-   DeleteFunction
-   EndProperty
-   ComplexProperty
-   ResultMapping

**MappingFragment**, **Complexproperty**또는 **Endproperty** 요소의 자식으로 **ScalarProperty** 요소는 개념적 모델의 속성을 데이터베이스의 열에 매핑합니다. **Insertfunction**, **Updatefunction**또는 **Deletefunction** 요소의 자식으로 **ScalarProperty** 요소는 개념적 모델의 속성을 저장 프로시저 매개 변수에 매핑합니다.

**ScalarProperty** 요소에는 자식 요소가 있을 수 없습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

**ScalarProperty** 요소에 적용 되는 특성은 요소의 역할에 따라 다릅니다.

다음 표에서는 **ScalarProperty** 요소를 사용 하 여 개념적 모델 속성을 데이터베이스의 열에 매핑하는 경우 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **이름**       | 예         | 매핑되는 개념적 모델 속성의 이름입니다. |
| **ColumnName** | 예         | 매핑되는 테이블 열의 이름입니다.              |

다음 표에서는 개념적 모델 속성을 저장 프로시저 매개 변수에 매핑하는 데 사용할 때 **ScalarProperty** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름    | 필수 여부 | 값                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**          | 예         | 매핑되는 개념적 모델 속성의 이름입니다.                                                                                 |
| **ParameterName** | 예         | 매핑되는 매개 변수의 이름입니다.                                                                                                 |
| **버전(Version)**       | 아니요          | 현재 값 또는 속성의 원래 값이 동시성 검사에 사용 되어야 하는지 여부에 따라 **현재** 또는 **원래** 값입니다. |

### <a name="example"></a>예제

다음 예에서는 두 가지 방법으로 사용 되는 **ScalarProperty** 요소를 보여 줍니다.

-   **Person** 엔터티 형식의 속성을 **person**테이블의 열에 매핑합니다.
-   **Person** 엔터티 형식의 속성을 **updateperson** 저장 프로시저의 매개 변수에 매핑합니다. 저장 프로시저는 스토리지 모델에서 선언해야 합니다.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>예제

다음 예에서는 개념적 모델 연결의 insert 및 delete 함수를 데이터베이스의 저장 프로시저에 매핑하는 데 사용 되는 **ScalarProperty** 요소를 보여 줍니다. 저장 프로시저는 스토리지 모델에서 선언해야 합니다.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="updatefunction-element-msl"></a>UpdateFunction 요소(MSL)

MSL (매핑 사양 언어)의 **updatefunction** 요소는 개념적 모델의 엔터티 형식에 대 한 업데이트 함수를 기본 데이터베이스의 저장 프로시저에 매핑합니다. 수정 함수가 매핑되는 저장 프로시저는 스토리지 모델에서 선언되어야 합니다. 자세한 내용은 Function 요소 (SSDL)를 참조 하세요.

> [!NOTE]
>  엔터티 형식의 삽입, 업데이트 또는 삭제 작업을 모두 저장 프로시저에 매핑하지 않은 경우 런타임에 실행 하면 매핑되지 않은 작업이 실패 하 고 UpdateException이 throw 됩니다.

**Updatefunction** 요소는 ModificationFunctionMapping 요소의 자식일 수 있으며 entitytypemapping 요소에 적용 될 수 있습니다.

**Updatefunction** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.

-   AssociationEnd (0 개 이상)
-   ComplexProperty (0 개 이상)
-   ResultBinding (0 개 또는 1 개)
-   ScarlarProperty (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Updatefunction** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름            | 필수 여부 | 값                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | 예         | 업데이트 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다. 저장 프로시저는 스토리지 모델에서 선언해야 합니다. |
| **RowsAffectedParameter** | 아니요          | 영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.                                                                               |

### <a name="example"></a>예제

다음 예는 School 모델을 기반으로 하며 **Person** 엔터티 형식의 업데이트 함수를 **updatefunction** 저장 프로시저에 매핑하는 데 사용 되는 **updatefunction** 요소를 보여 줍니다. **Updateperson** 저장 프로시저는 저장소 모델에서 선언 됩니다.

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
