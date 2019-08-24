---
title: EF6-MSL 사양
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 6bff1f5407bc0546e60b5bee1178be9aa4748bd8
ms.sourcegitcommit: 29f928a6116771fe78f306846e6f2d45cbe8d1f4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460139"
---
# <a name="msl-specification"></a>XML 사양
매핑 사양 언어 (MSL)은 개념적 모델 및 Entity Framework 응용 프로그램의 저장소 모델 간의 매핑을 설명 하는 XML 기반 언어입니다.

Entity Framework 응용 프로그램에서는 빌드 시 매핑 메타 데이터 (MSL로 작성).msl 파일에서 로드 됩니다. Entity Framework 런타임에 매핑 메타 데이터를 사용 하 여 저장소 관련 명령 개념적 모델에 대 한 쿼리를 변환 합니다.

Entity Framework Designer (EF 디자이너)는 디자인 타임에.edmx 파일에 매핑 정보를 저장합니다. 빌드 시 Entity Designer 정보 사용 하 여.edmx 파일에는 필요한 Entity Framework에서 런타임에.msl 파일을 만들려면

MSL에서 참조하는 모든 개념적 모델 형식 또는 스토리지 모델 형식의 이름은 해당 네임스페이스 이름으로 한정되어야 합니다. 개념적 모델 네임 스페이스 이름에 대 한 정보를 참조 하세요 [CSDL 사양](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md)합니다. 저장소 모델 네임 스페이스 이름에 대 한 정보를 참조 하세요 [SSDL 사양](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md)합니다.

MSL의 버전은 XML 네임 스페이스로 식별 됩니다.

| MSL 버전 | XML Namespace                                        |
|:------------|:-----------------------------------------------------|
| MSL v1      | urn: 스키마-microsoft-com:windows:storage:mapping:CS |
| MSL v2      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| MSL v3      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a>Alias 요소(MSL)

합니다 **별칭** MSL (매핑 사양 언어)에서 요소는 개념적 모델과 저장소 모델 네임 스페이스에 대 한 별칭을 정의 하는 데 사용 되는 매핑 요소의 자식입니다. MSL에서 참조하는 모든 개념적 모델 형식 또는 스토리지 모델 형식의 이름은 해당 네임스페이스 이름으로 한정되어야 합니다. 개념적 모델 네임 스페이스 이름에 대 한 내용은 스키마 요소 (CSDL)을 참조 하세요. 저장소 모델 네임 스페이스 이름에 대 한 내용은 스키마 요소 (SSDL)을 참조 하세요.

합니다 **별칭** 요소는 자식 요소를 포함할 수 없습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **별칭** 요소.

| 특성 이름 | 필수 여부 | 값                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| **키**        | 예         | 지정 된 네임 스페이스에 대 한 별칭을 **값** 특성입니다. |
| **값**      | 예         | 네임 스페이스의 값을 **키** 요소는 별칭입니다.     |

### <a name="example"></a>예제

다음 예제는 **별칭** 별칭을 정의 하는 요소 `c`, 개념적 모델에 정의 된 형식에 대 한 합니다.

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

합니다 **AssociationEnd** MSL (매핑 사양 언어)에서 요소는 개념적 모델의 엔터티 형식 수정 함수가 기본 데이터베이스의 저장된 프로시저에 매핑된 경우 사용 합니다. 값은 연결 속성에 저장 된 매개 변수를 사용 하는 저장된 프로시저를 수정 하는 경우는 **AssociationEnd** 요소 매개 변수 속성 값을 매핑합니다. 자세한 내용은 아래 예제를 참조하십시오.

저장된 프로시저에 엔터티 형식의 수정 함수 매핑 하는 방법에 대 한 자세한 내용은 참조 ModificationFunctionMapping 요소 (MSL) 및 연습: 저장 프로시저에 엔터티 매핑.

합니다 **AssociationEnd** 요소는 다음 자식 요소를 포함할 수 있습니다.

-   ScalarProperty

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 해당 하는 특성을 설명 합니다 **AssociationEnd** 요소입니다.

| 특성 이름     | 필수 여부 | 값                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **AssociationSet** | 예         | 매핑되는 연결의 이름                                                                                                                                 |
| **From**           | 예         | 값을 **FromRole** 매핑되는 연결에 해당 하는 탐색 속성의 특성입니다. 자세한 내용은 NavigationProperty 요소 (CSDL)을 참조 하세요. |
| **대상**             | 예         | 값을 **ToRole** 매핑되는 연결에 해당 하는 탐색 속성의 특성입니다. 자세한 내용은 NavigationProperty 요소 (CSDL)을 참조 하세요.   |

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

업데이트 함수 이름을 매핑하려면 합니다 `Course` 이 저장된 프로시저에 엔터티, 값을 제공 해야 합니다는 **DepartmentID** 매개 변수입니다. `DepartmentID`의 값은 엔터티 형식의 속성에 해당하지 않으며, 여기에 표시된 매핑을 사용하는 독립 연결에 포함되어 있습니다.

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

다음 코드에서는 합니다 **AssociationEnd** 매핑하는 데 사용 되는 요소는 **DepartmentID** 속성을 **FK\_과정\_부서** 연결 합니다 **UpdateCourse** 저장 프로시저 (할 업데이트 함수를 **과정** 엔터티 형식이 매핑되는):

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

합니다 **AssociationSetMapping** MSL (매핑 사양 언어)에서 요소는 기본 데이터베이스에서 개념적 모델 및 테이블 열에 있는 연결 간의 매핑을 정의 합니다.

개념적 모델의 연결은 기본 데이터베이스의 기본 및 외래 키 열을 나타내는 속성이 있는 형식입니다. 합니다 **AssociationSetMapping** 요소 두 EndProperty 요소를 사용 하 여 데이터베이스의 연결 형식 속성과 열 간의 매핑을 정의 합니다. Condition 요소를 사용 하 여 이러한 매핑에 조건을 적용할 수 있습니다. ModificationFunctionMapping 요소를 사용 하 여 데이터베이스에서 저장된 프로시저에 삽입, 업데이트 및 연결에 대 한 삭제 함수를 매핑하십시오. QueryView 요소에서 Entity SQL 문자열을 사용 하 여 연결과 테이블 열 간의 읽기 전용 매핑을 정의 합니다.

> [!NOTE]
> 연결을 사용 하 여 매핑할 필요가 없습니다 개념적 모델의 연결에 대 한 참조 제약 조건이 정의 된 경우는 **AssociationSetMapping** 요소입니다. 경우는 **AssociationSetMapping** 요소가 있는 참조 제약 조건이 있는 연결의 매핑을 정의 합니다 **AssociationSetMapping** 요소는 무시 됩니다. 자세한 내용은 ReferentialConstraint 요소 (CSDL)을 참조 하세요.

합니다 **AssociationSetMapping** 요소는 다음 자식 요소를 포함할 수 있습니다

-   QueryView (0 또는 1)
-   EndProperty (0 또는 2)
-   조건 (0 개 이상)
-   ModificationFunctionMapping (0 또는 1)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **AssociationSetMapping** 요소입니다.

| 특성 이름     | 필수 여부 | 값                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| **이름**           | 예         | 매핑되는 개념적 모델 연결 집합의 이름                      |
| **TypeName**       | 아니요          | 매핑되는 개념적 모델 연결 형식의 네임스페이스로 한정된 이름 |
| **StoreEntitySet** | 아니요          | 매핑되는 테이블의 이름                                                 |

### <a name="example"></a>예제

다음 예제는 **AssociationSetMapping** 요소를 **FK\_과정\_부서** 개념적 모델의 연결 집합은 매핑되 **과정** 데이터베이스의 테이블입니다. 자식에서 연결 형식 속성과 테이블 열 간의 매핑을 지정 된 **EndProperty** 요소입니다.

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

A **ComplexProperty** MSL (매핑 사양 언어)의 요소에는 기본 데이터베이스에서 개념적 모델 엔터티 형식과 테이블 열에 복합 형식 속성 간의 매핑을 정의 합니다. 속성-열 매핑은 자식 ScalarProperty 요소에 지정 됩니다.

합니다 **ComplexType** 속성 요소에는 다음 자식 요소를 포함할 수 있습니다.

-   ScalarProperty (0 개 이상)
-   **ComplexProperty** (0 개 이상)
-   ComplextTypeMapping (0 개 이상)
-   조건 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 해당 하는 특성을 설명 합니다 **ComplexProperty** 요소:

| 특성 이름 | 필수 여부 | 값                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| **이름**       | 예         | 개념적 모델에서 매핑되는 엔터티 형식의 복합 속성 이름입니다. |
| **TypeName**   | 아니요          | 개념적 모델 속성 형식의 네임스페이스로 한정된 이름입니다.                              |

### <a name="example"></a>예제

다음 예제에서는 School 모델을 기반으로 합니다. 다음 복합 형식이 개념적 모델에 추가되었습니다.

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

**LastName** 및 **FirstName** 의 속성을 **Person** 하나의 복합 속성을 사용 하 여 엔터티 형식의 이름이 바뀌었습니다. **이름**:

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

에서는 다음 MSL 합니다 **ComplexProperty** 매핑하는 데 사용 되는 요소는 **이름** 기본 데이터베이스의 열에 속성:

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

합니다 **ComplexTypeMapping** MSL (매핑 사양 언어)에서 요소 ResultMapping 요소의 자식인 기본 개념적 모델의 function import와 저장된 프로시저 간의 매핑을 정의 하 고 데이터베이스는 다음과 같은 경우:

-   Function Import가 개념적 복합 형식을 반환하는 경우
-   저장 프로시저에서 반환되는 열 이름이 복합 형식의 속성 이름과 정확히 일치하지 않는 경우

기본적으로 저장 프로시저에서 반환되는 열과 복합 형식 간의 매핑은 열 및 속성 이름을 기반으로 합니다. 열 이름이 정확히 일치 하지 않는 속성 이름, 경우에 사용 해야 합니다 **ComplexTypeMapping** 매핑을 정의 하는 요소입니다. 기본 매핑의 예 FunctionImportMapping 요소 (MSL)을 참조 하세요.

합니다 **ComplexTypeMapping** 요소는 다음 자식 요소를 포함할 수 있습니다.

-   ScalarProperty (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 해당 하는 특성을 설명 합니다 **ComplexTypeMapping** 요소입니다.

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

이전 복합 형식의 인스턴스를 반환 하는 function import를 만들기 위해 열 간의 매핑이 저장된 프로시저에서 반환 하 고 엔터티 형식에 정의 되어야 합니다는 **ComplexTypeMapping** 요소:

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

합니다 **조건을** MSL (매핑 사양 언어)에서 요소 개념적 모델과 기본 데이터베이스 간의 매핑에 대 한 조건을 적용 합니다. XML 노드 내에 정의 된 매핑을 사용 하는 모든 유효한 조건에 지정 된 자식 **조건을** 요소를 충족 합니다. 그렇지 않으면 매핑이 유효하지 않습니다. 예를 들어 MappingFragment 요소를 하나 이상 포함 **조건을** 내에서 정의 된 매핑은 자식 요소를 **MappingFragment** 노드 모든 경우에 적용 됩니다 자식조건 **조건** 요소 충족 됩니다.

각 조건 중 하나에 적용할 수는 **이름** (지정 된 개념적 모델 엔터티 속성의 이름을 합니다 **이름** 특성), 또는 **ColumnName** (이름에 있는 열의 지정한 데이터베이스를 **ColumnName** 특성). 경우는 **이름을** 특성이 설정 되어, 엔터티 속성 값에 대해 조건이 확인 됩니다. 경우는 **ColumnName** 특성을 설정 하면 열 값에 대해 조건이 확인 됩니다. 중 하나만 합니다 **이름** 또는 **ColumnName** 특성을 지정할 수 있습니다를 **조건** 요소입니다.

> [!NOTE]
> 때를 **조건** 만 FunctionImportMapping 요소 내에서 요소를 사용 합니다 **이름** 특성 적용 되지 않습니다.

합니다 **조건을** 요소에는 다음 요소의 자식일 수 있습니다.

-   AssociationSetMapping
-   ComplexProperty
-   EntitySetMapping
-   MappingFragment
-   EntityTypeMapping

합니다 **조건을** 요소는 자식 요소가 있을 수 있습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 해당 하는 특성을 설명 합니다 **조건을** 요소:

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ColumnName** | 아니요          | 조건을 확인하는 데 사용되는 값이 있는 테이블 열의 이름입니다.                                                                                                                                                                                                                   |
| **IsNull**     | 아니요          | **True 이면** 나 **False**합니다. 값이 **True** 이 고 열 값이 **null**, 또는 값이 **False** 고 열 값이 없는 **null**, 조건이 true 인 . 그렇지 않으면 조건이 false입니다. <br/> 합니다 **IsNull** 하 고 **값** 동시에 특성을 사용할 수 없습니다. |
| **값**      | 아니요          | 열 값과 비교할 값입니다. 값이 동일하면 조건이 true입니다. 그렇지 않으면 조건이 false입니다. <br/> 합니다 **IsNull** 하 고 **값** 동시에 특성을 사용할 수 없습니다.                                                                       |
| **이름**       | 아니요          | 조건을 확인하는 데 사용되는 값이 있는 개념적 모델 엔터티 속성의 이름입니다. <br/> 이 특성을 적용할 수 없는 경우는 **조건을** FunctionImportMapping 요소 내의 요소를 사용 합니다.                                                                           |

### <a name="example"></a>예제

다음 예와 **조건을** 요소의 자식인 **MappingFragment** 요소입니다. 때 **HireDate** isnotnull 및 **EnrollmentDate** 는 null 간에 데이터가 매핑되는 합니다 **SchoolModel.Instructor** 형식 및 **PersonID**하 고 **HireDate** 열의 합니다 **Person** 테이블. 때 **EnrollmentDate** isnotnull 및 **HireDate** 는 null 간에 데이터가 매핑되는 합니다 **SchoolModel.Student** 형식 및 **PersonID** 및 **등록** 의 열을 **Person** 테이블입니다.

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

합니다 **DeleteFunction** MSL (매핑 사양 언어)에서 요소는 기본 데이터베이스의 저장된 프로시저에 엔터티 형식 또는 연결 개념적 모델에 대 한 삭제 함수를 매핑합니다. 수정 함수가 매핑되는 저장 프로시저는 스토리지 모델에서 선언되어야 합니다. 자세한 내용은 함수 요소 (SSDL)을 참조 하세요.

> [!NOTE]
> 매핑되지 않는 경우 모든 세 개의 삽입, 업데이트 또는 삭제 저장된 프로시저에 엔터티 형식의 작업, 경우 런타임에 실행 하면 매핑되지 않은 작업이 실패 합니다 및는 UpdateException throw 됩니다.

### <a name="deletefunction-applied-to-entitytypemapping"></a>EntityTypeMapping에 적용되는 DeleteFunction

EntityTypeMapping 요소에 적용할 경우 합니다 **DeleteFunction** 요소 저장된 프로시저를 개념적 모델의 엔터티 형식에 대 한 삭제 함수를 매핑합니다.

합니다 **DeleteFunction** 요소에 적용 하는 경우 다음 자식 요소를 포함할 수 있습니다는 **EntityTypeMapping** 요소:

-   AssociationEnd (0 개 이상)
-   ComplexProperty (0 개 이상)
-   ScarlarProperty (0 개 이상)

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 특성을 적용할 수 있습니다는 **DeleteFunction** 요소에 적용 되는 경우는 **EntityTypeMapping** 요소입니다.

| 특성 이름            | 필수 여부 | 값                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | 예         | 삭제 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다. 저장 프로시저는 스토리지 모델에서 선언해야 합니다. |
| **RowsAffectedParameter** | 아니요          | 영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.                                                                               |

#### <a name="example"></a>예제

다음 예제에서는 School 모델을 기반으로 하며 표시를 **DeleteFunction** 대 한 삭제 함수 매핑 요소를 **Person** 엔터티 형식을 **DeletePerson** 저장된 프로시저입니다. 합니다 **DeletePerson** 저장된 프로시저는 저장소 모델에서 선언 됩니다.

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

AssociationSetMapping 요소에 적용할 경우 합니다 **DeleteFunction** 요소 저장된 프로시저를 개념적 모델의 연결에 대 한 삭제 함수를 매핑합니다.

합니다 **DeleteFunction** 요소에 적용 하는 경우 다음 자식 요소를 포함할 수 있습니다 합니다 **AssociationSetMapping** 요소:

-   EndProperty

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 특성을 적용할 수 있습니다는 **DeleteFunction** 요소에 적용 되는 경우를 **AssociationSetMapping** 요소입니다.

| 특성 이름            | 필수 여부 | 값                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | 예         | 삭제 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다. 저장 프로시저는 스토리지 모델에서 선언해야 합니다. |
| **RowsAffectedParameter** | 아니요          | 영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.                                                                               |

#### <a name="example"></a>예제

다음 예제에서는 School 모델을 기반으로 하며 표시를 **DeleteFunction** 대 한 삭제 함수를 매핑하는 데 사용 되는 요소는 **CourseInstructor** 연결을  **DeleteCourseInstructor** 저장 프로시저입니다. 합니다 **DeleteCourseInstructor** 저장된 프로시저는 저장소 모델에서 선언 됩니다.

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

합니다 **EndProperty** MSL (매핑 사양 언어)에서 요소 끝 또는 수정 함수를 개념적 모델 연결 및 기본 데이터베이스 간의 매핑을 정의 합니다. 속성-열 매핑은 자식 ScalarProperty 요소에 지정 됩니다.

경우는 **EndProperty** AssociationSetMapping 요소 자식, 요소는 개념적 모델 연결의 끝에 대 한 매핑을 정의 하는 데 사용 됩니다. 경우는 **EndProperty** 요소는 개념적 모델 연결의 수정 함수에 대 한 매핑을 정의 하는, 자식인 InsertFunction 요소 또는 DeleteFunction 요소입니다.

합니다 **EndProperty** 요소는 다음 자식 요소를 포함할 수 있습니다.

-   ScalarProperty (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 해당 하는 특성을 설명 합니다 **EndProperty** 요소:

| 특성 이름 | 필수 여부 | 값                                                 |
|:---------------|:------------|:------------------------------------------------------|
| 이름           | 예         | 매핑되는 연결 끝의 이름 |

### <a name="example"></a>예제

에서는 다음 예제는 **AssociationSetMapping** 요소를 **FK\_과정\_부서** 합니다 매핑되는개념적모델의연결**과정** 데이터베이스의 테이블입니다. 자식에서 연결 형식 속성과 테이블 열 간의 매핑을 지정 된 **EndProperty** 요소입니다.

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

다음 예제에서는 합니다 **EndProperty** 요소는 연결의 삽입 및 삭제 함수 매핑 (**CourseInstructor**) 기본 데이터베이스의 저장된 프로시저에 합니다. 매핑되는 함수는 스토리지 모델에서 선언됩니다.

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

합니다 **EntityContainerMapping** MSL (매핑 사양 언어)에서 요소는 저장소 모델의 엔터티 컨테이너에 개념적 모델의 엔터티 컨테이너를 매핑합니다. 합니다 **EntityContainerMapping** 매핑 요소의 자식 요소입니다.

합니다 **EntityContainerMapping** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   EntitySetMapping (0 개 이상)
-   AssociationSetMapping (0 개 이상)
-   FunctionImportMapping (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **EntityContainerMapping** 요소입니다.

| 특성 이름            | 필수 여부 | 값                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StorageModelContainer** | 예         | 매핑되는 스토리지 모델 엔터티 컨테이너의 이름입니다.                                                                                                                                                                                     |
| **CdmEntityContainer**    | 예         | 매핑되는 개념적 모델 엔터티 컨테이너의 이름입니다.                                                                                                                                                                                  |
| **GenerateUpdateViews**   | 아니요          | **True 이면** 나 **False**합니다. 하는 경우 **False**, 업데이트 보기가 생성 됩니다. 이 특성에 설정할 **False** 는 유효 하지 않게 데이터가 올바르게 라운드트립되지 않기 때문에 읽기 전용 매핑이 있는 경우. <br/> 기본값은 **True**합니다. |

### <a name="example"></a>예제

에서는 다음 예제는 **EntityContainerMapping** 매핑되는 요소는 **SchoolModelEntities** 컨테이너 (개념적 모델 엔터티 컨테이너)를  **SchoolModelStoreContainer** 컨테이너 (저장소 모델 엔터티 컨테이너):

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

합니다 **EntitySetMapping** 저장소 모델의 매핑 사양 언어 (MSL) 맵 엔터티를 개념적 모델의 모든 형식 집합에 요소를 설정 합니다. 개념적 모델의 엔터티 집합에 대 한 논리적 컨테이너는 동일한 형식 (및 파생된 형식)의 엔터티의 인스턴스. 저장소 모델의 엔터티 집합을 테이블 또는 기본 데이터베이스의 뷰를 나타냅니다. 개념적 모델 엔터티 집합의 값으로 지정 된 합니다 **이름** 특성을 **EntitySetMapping** 요소. 매핑 대상 테이블 또는 보기에서 지정 된 합니다 **StoreEntitySet** 각 자식 MappingFragment 요소 또는 특성을 **EntitySetMapping** 요소 자체.

합니다 **EntitySetMapping** 요소는 다음 자식 요소를 포함할 수 있습니다.

-   EntityTypeMapping (0 개 이상)
-   QueryView (0 또는 1)
-   MappingFragment (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **EntitySetMapping** 요소입니다.

| 특성 이름           | 필수 여부 | 값                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**                 | 예         | 매핑되는 개념적 모델 엔터티 집합의 이름입니다.                                                                                                                                                             |
| **TypeName** **1**       | 아니요          | 매핑되는 개념적 모델 엔터티 형식의 이름입니다.                                                                                                                                                            |
| **StoreEntitySet** **1** | 아니요          | 매핑되는 대상 스토리지 모델 엔터티 집합의 이름입니다.                                                                                                                                                             |
| **MakeColumnsDistinct**  | 아니요          | **True 이면** 나 **False** 고유한 행만 반환 되는지 여부에 따라 합니다. <br/> 이 특성이로 설정 된 경우 **True**서 **GenerateUpdateViews** EntityContainerMapping 요소의 특성으로 설정 되어 있어야 **False**. |

 

**1** 는 **TypeName** 하 고 **StoreEntitySet** 특성 단일 엔터티 형식을 단일 테이블에 매핑할 EntityTypeMapping 및 MappingFragment 자식 요소를 대신 사용할 수 있습니다.

### <a name="example"></a>예제

다음 예제와 **EntitySetMapping** 의 세 가지 형식 (기본 형식 및 두 개의 파생된 형식)를 매핑하는 요소는 **과정** 개념적 모델에서 서로 다른 세 테이블의 엔터티 집합을 기본 데이터베이스입니다. 테이블에서 지정 된 된 **StoreEntitySet** 각각의 특성 **MappingFragment** 요소입니다.

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

합니다 **EntityTypeMapping** MSL (매핑 사양 언어)에서 요소 내부 데이터베이스에서 개념적 모델 및 테이블의 엔터티 형식 또는 뷰 간의 매핑을 정의 합니다. 개념적 모델 엔터티 형식과 기본 데이터베이스 테이블 또는 뷰에 대 한 자세한 내용은 EntityType 요소 (CSDL) 및 EntitySet 요소 (SSDL)를 참조 하십시오. 매핑되는 개념적 모델 엔터티 형식을 지정 하 여는 **TypeName** 특성을 **EntityTypeMapping** 요소입니다. 매핑되는 뷰나 테이블에서 지정 합니다 **StoreEntitySet** 자식 MappingFragment 요소의 특성입니다.

자식 요소를 사용 하 여 삽입, 매핑할 수 있습니다 ModificationFunctionMapping 업데이트 또는 삭제 함수를 데이터베이스에서 저장된 프로시저에 엔터티 형식입니다.

합니다 **EntityTypeMapping** 요소는 다음 자식 요소를 포함할 수 있습니다.

-   MappingFragment (0 개 이상)
-   ModificationFunctionMapping (0 또는 1)
-   ScalarProperty
-   조건

> [!NOTE]
> **MappingFragment** 하 고 **ModificationFunctionMapping** 요소는 자식 요소의 수 없습니다는 **EntityTypeMapping** 동시 요소입니다.


> [!NOTE]
> **ScalarProperty** 및 **조건** 요소는 자식 요소의 수만 합니다 **EntityTypeMapping** 요소 FunctionImportMapping 요소 내에서 사용 하는 경우.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **EntityTypeMapping** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **TypeName**   | 예         | 매핑되는 개념적 모델 엔터티 형식의 네임스페이스로 한정된 이름입니다. <br/> 해당 형식이 추상 또는 파생 형식이면 이 값은 `IsOfType(Namespace-qualified_type_name)`이어야 합니다. |

### <a name="example"></a>예제

다음 예제에서는 두 개의 자식 EntitySetMapping 요소를 보여 줍니다 **EntityTypeMapping** 요소입니다. 첫 번째에서 **EntityTypeMapping** 요소를 **SchoolModel.Person** 엔터티 형식에 매핑되는 **Person** 테이블입니다. 두 번째에서 **EntityTypeMapping** 요소, 업데이트 기능을 **SchoolModel.Person** 형식이 저장된 프로시저에 매핑되는 **UpdatePerson**, 데이터베이스에서 .

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

다음 예제에서는 루트 형식이 추상인 형식 계층 구조의 매핑을 보여 줍니다. 사용 된 `IsOfType` 구문에 대 한는 **TypeName** 특성.

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

합니다 **FunctionImportMapping** MSL (매핑 사양 언어)에서 요소 내부 데이터베이스에서 function import에서 개념적 모델 및 저장된 프로시저 또는 함수 간에 매핑을 정의 합니다. Function Import는 개념적 모델에서 선언해야 하고 저장 프로시저는 스토리지 모델에서 선언해야 합니다. 자세한 내용은 FunctionImport 요소 (CSDL) 및 함수 요소 (SSDL)를 참조 하십시오.

> [!NOTE]
> 기본적으로, Function Import에서 개념적 모델 엔터티 형식이나 복합 형식을 반환하는 경우에는 기본 저장 프로시저에서 반환된 열 이름이 개념적 모델 형식의 속성 이름과 정확히 일치해야 합니다. 열 이름이 정확히 일치 하지 않는 속성 이름, ResultMapping 요소에서 매핑을 정의 해야 합니다.

합니다 **FunctionImportMapping** 요소는 다음 자식 요소를 포함할 수 있습니다.

-   ResultMapping (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 해당 하는 특성을 설명 합니다 **FunctionImportMapping** 요소:

| 특성 이름         | 필수 여부 | 값                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| **FunctionImportName** | 예         | 매핑되는 개념적 모델의 Function Import 이름           |
| **FunctionName**       | 예         | 매핑되는 스토리지 모델 함수의 네임스페이스로 한정된 이름 |

### <a name="example"></a>예제

다음 예제에서는 School 모델을 기반으로 합니다. 다음은 스토리지 모델의 함수입니다.

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

다음 예제에서는 표시 된 **FunctionImportMapping** 함수 매핑하고 서로 위에 가져오기 작동 하는 데 사용 되는 요소:

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a>InsertFunction 요소(MSL)

합니다 **InsertFunction** MSL (매핑 사양 언어)에서 요소는 기본 데이터베이스의 저장된 프로시저에 엔터티 형식 또는 연결 개념적 모델에 대 한 삽입 함수를 매핑합니다. 수정 함수가 매핑되는 저장 프로시저는 스토리지 모델에서 선언되어야 합니다. 자세한 내용은 함수 요소 (SSDL)을 참조 하세요.

> [!NOTE]
> 매핑되지 않는 경우 모든 세 개의 삽입, 업데이트 또는 삭제 저장된 프로시저에 엔터티 형식의 작업, 경우 런타임에 실행 하면 매핑되지 않은 작업이 실패 합니다 및는 UpdateException throw 됩니다.

합니다 **InsertFunction** 요소 ModificationFunctionMapping 요소의 자식이 될 수 있으며 EntityTypeMapping 요소 또는 AssociationSetMapping 요소에 적용 합니다.

### <a name="insertfunction-applied-to-entitytypemapping"></a>EntityTypeMapping에 적용되는 InsertFunction

EntityTypeMapping 요소에 적용할 경우 합니다 **InsertFunction** 요소 저장된 프로시저를 개념적 모델에서 엔터티 형식의 삽입 함수를 매핑합니다.

합니다 **InsertFunction** 요소에 적용 하는 경우 다음 자식 요소를 포함할 수 있습니다는 **EntityTypeMapping** 요소:

-   AssociationEnd (0 개 이상)
-   ComplexProperty (0 개 이상)
-   ResultBinding (0 또는 1)
-   ScarlarProperty (0 개 이상)

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 테이블에 적용할 수 있는 특성에 설명 합니다 **InsertFunction** 요소에 적용 될 때를 **EntityTypeMapping** 요소.

| 특성 이름            | 필수 여부 | 값                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | 예         | 삽입 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다. 저장 프로시저는 스토리지 모델에서 선언해야 합니다. |
| **RowsAffectedParameter** | 아니요          | 영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.                                                                               |

#### <a name="example"></a>예제

다음 예제에서는 School 모델을 기반으로 하며 표시 합니다 **InsertFunction** 에 Person 엔터티 형식의 삽입 함수를 매핑하는 데 사용 되는 요소는 **InsertPerson** 저장 프로시저입니다. 합니다 **InsertPerson** 저장된 프로시저는 저장소 모델에서 선언 됩니다.

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

AssociationSetMapping 요소에 적용할 경우 합니다 **InsertFunction** 요소 저장된 프로시저를 개념적 모델의 연결에 대 한 삽입 함수를 매핑합니다.

합니다 **InsertFunction** 요소에 적용 하는 경우 다음 자식 요소를 포함할 수 있습니다 합니다 **AssociationSetMapping** 요소:

-   EndProperty

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 특성을 적용할 수 있습니다는 **InsertFunction** 요소에 적용 되는 경우를 **AssociationSetMapping** 요소입니다.

| 특성 이름            | 필수 여부 | 값                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | 예         | 삽입 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다. 저장 프로시저는 스토리지 모델에서 선언해야 합니다. |
| **RowsAffectedParameter** | 아니요          | 영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.                                                                               |

#### <a name="example"></a>예제

다음 예제에서는 School 모델을 기반으로 하며 표시를 **InsertFunction** 대 한 삽입 함수를 매핑하는 데 사용 되는 요소는 **CourseInstructor** 연결을  **InsertCourseInstructor** 저장 프로시저입니다. 합니다 **InsertCourseInstructor** 저장된 프로시저는 저장소 모델에서 선언 됩니다.

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

합니다 **매핑** 요소 MSL (매핑 사양 언어)에 매핑 (저장소 모델에서 설명)로 데이터베이스에 개념적 모델에서 정의 된 개체에 대 한 정보를 포함 합니다. 자세한 내용은 CSDL 사양 및 SSDL 사양을 참조 하세요.

합니다 **매핑** 요소는 매핑 사양의 루트 요소입니다. 매핑 사양에 대 한 XML 네임 스페이스는 http://schemas.microsoft.com/ado/2009/11/mapping/cs 합니다.

Mapping 요소는 다음에 나열된 순서대로 자식 요소를 포함할 수 있습니다.

-   별칭 (0 개 이상)
-   EntityContainerMapping (1 개만)

MSL에서 참조하는 개념적 모델 형식과 스토리지 모델 형식의 이름은 해당 네임스페이스 이름으로 한정되어야 합니다. 개념적 모델 네임 스페이스 이름에 대 한 내용은 스키마 요소 (CSDL)을 참조 하세요. 저장소 모델 네임 스페이스 이름에 대 한 내용은 스키마 요소 (SSDL)을 참조 하세요. Alias 요소를 사용 하 여 MSL에서 사용 되는 네임 스페이스에 대 한 별칭을 정의할 수 있습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **매핑** 요소.

| 특성 이름 | 필수 여부 | 값                                                 |
|:---------------|:------------|:------------------------------------------------------|
| **스페이스바**      | 예         | **C-S**합니다. 이 값은 고정된 값이므로 변경할 수 없습니다. |

### <a name="example"></a>예제

다음 예제는 **매핑** School 모델의 일부를 기반으로 하는 요소입니다. School 모델에 대 한 자세한 내용은 빠른 시작 (Entity Framework)를 참조 하세요.

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

합니다 **MappingFragment** MSL (매핑 사양 언어)에서 요소는 개념적 모델 엔터티 형식 및 테이블 또는 데이터베이스에서 뷰 속성 간의 매핑을 정의 합니다. 개념적 모델 엔터티 형식과 기본 데이터베이스 테이블 또는 뷰에 대 한 자세한 내용은 EntityType 요소 (CSDL) 및 EntitySet 요소 (SSDL)를 참조 하십시오. 합니다 **MappingFragment** EntityTypeMapping 요소 또는 EntitySetMapping 요소는 자식 요소가 될 수 있습니다.

합니다 **MappingFragment** 요소는 다음 자식 요소를 포함할 수 있습니다.

-   ComplexType (0 개 이상)
-   ScalarProperty (0 개 이상)
-   조건 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **MappingFragment** 요소입니다.

| 특성 이름          | 필수 여부 | 값                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StoreEntitySet**      | 예         | 매핑되는 테이블 또는 뷰의 이름입니다.                                                                                                                                                                           |
| **MakeColumnsDistinct** | 아니요          | **True 이면** 나 **False** 고유한 행만 반환 되는지 여부에 따라 합니다. <br/> 이 특성이로 설정 된 경우 **True**서 **GenerateUpdateViews** EntityContainerMapping 요소의 특성으로 설정 되어 있어야 **False**. |

### <a name="example"></a>예제

다음 예제와 **MappingFragment** 의 자식 요소를 **EntityTypeMapping** 요소입니다. 이 예제에서는 속성을를 **과정** 개념적 모델의 열에 매핑되는 **과정** 데이터베이스의 테이블.

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

다음 예제와 **MappingFragment** 의 자식 요소를 **EntitySetMapping** 요소입니다. 속성을 위의 예제와 같이 **과정** 개념적 모델의 열에 매핑되는 **과정** 데이터베이스의 테이블입니다.

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

합니다 **ModificationFunctionMapping** MSL (매핑 사양 언어)에서 요소 매핑합니다 삽입, 업데이트 및 기본 데이터베이스의 저장된 프로시저를 개념적 모델 엔터티 형식의 함수를 삭제 합니다. 합니다 **ModificationFunctionMapping** 요소 삽입 매핑할 고 기본 데이터베이스의 저장된 프로시저를 개념적 모델의 다 대 다 연결에 대 한 함수를 삭제할 수도 있습니다. 수정 함수가 매핑되는 저장 프로시저는 스토리지 모델에서 선언되어야 합니다. 자세한 내용은 함수 요소 (SSDL)을 참조 하세요.

> [!NOTE]
> 매핑되지 않는 경우 모든 세 개의 삽입, 업데이트 또는 삭제 저장된 프로시저에 엔터티 형식의 작업, 경우 런타임에 실행 하면 매핑되지 않은 작업이 실패 합니다 및는 UpdateException throw 됩니다.


> [!NOTE]
> 상속 계층 구조의 한 엔터티에 대한 수정 함수가 저장 프로시저에 매핑된 경우 계층 구조의 모든 형식에 대한 수정 함수가 저장 프로시저에 매핑되어야 합니다.

합니다 **ModificationFunctionMapping** EntityTypeMapping 요소 또는 AssociationSetMapping 요소의 자식 요소일 수 있습니다.

합니다 **ModificationFunctionMapping** 요소는 다음 자식 요소를 포함할 수 있습니다.

-   DeleteFunction (0 또는 1)
-   InsertFunction (0 또는 1)
-   UpdateFunction (0 또는 1)

적용할 수 없는 특성을 **ModificationFunctionMapping** 요소입니다.

### <a name="example"></a>예제

다음 예제에서는 엔터티 집합에 대 한 매핑을 합니다 **사람** School 모델의 엔터티 집합입니다. 에 대 한 열 매핑 외에도 합니다 **Person** 엔터티 형식 매핑의 삽입, 업데이트 및 삭제 함수를 **사용자** 형식이 표시 됩니다. 매핑되는 함수는 스토리지 모델에서 선언됩니다.

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

다음 예제에서는 연결에 대 한 매핑 집합을 **CourseInstructor** School 모델의 연결 집합입니다. 에 대 한 열 매핑 외에도 합니다 **CourseInstructor** 연결의 삽입 및 삭제 함수 매핑 합니다 **CourseInstructor** 연결 표시 됩니다. 매핑되는 함수는 스토리지 모델에서 선언됩니다.

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

합니다 **QueryView** 요소 MSL (매핑 사양 언어)에서 개념적 모델과 기본 데이터베이스의 테이블에서 엔터티 형식 또는 연결 간의 읽기 전용 매핑을 정의 합니다. 매핑은 저장소 모델에 대해 평가 되는 Entity SQL 쿼리를 사용 하 여 정의 됩니다 하 고 엔터티 또는 개념적 모델의 연결을 기준으로 결과 집합을 표시 합니다. 쿼리 뷰는 읽기 전용이므로 표준 업데이트 명령을 사용하여 쿼리 뷰에서 정의된 형식을 업데이트할 수 없습니다. 수정 함수를 사용하여 이러한 형식을 업데이트할 수 있습니다. 자세한 내용은 방법: 저장 프로시저에 수정 함수를 맵.

> [!NOTE]
> 에 **QueryView** 요소를 포함 하는 Entity SQL 식은 **GroupBy**, 그룹 집계 또는 탐색 속성이 지원 되지 않습니다.

 

합니다 **QueryView** EntitySetMapping 요소 또는 AssociationSetMapping 요소의 자식 요소일 수 있습니다. 쿼리 뷰는 앞의 자식 요소의 경우 개념적 모델의 엔터티에 대한 읽기 전용 매핑을 정의하고, 뒤의 자식 요소의 경우 개념적 모델의 연결에 대한 읽기 전용 매핑을 정의합니다.

> [!NOTE]
> 경우는 **AssociationSetMapping** 요소는 참조 제약 조건 사용 하 여 연결 합니다 **AssociationSetMapping** 요소는 무시 됩니다. 자세한 내용은 ReferentialConstraint 요소 (CSDL)을 참조 하세요.

합니다 **QueryView** 요소는 모든 자식 요소를 포함할 수 없습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **QueryView** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **TypeName**   | 아니요          | 쿼리 뷰에 의해 매핑되는 개념적 모델 형식의 이름입니다. |

### <a name="example"></a>예제

다음 예제와 **QueryView** 의 자식 요소로 합니다 **EntitySetMapping** 요소에 대 한 쿼리 뷰 매핑을 정의 하 고는 **부서** 엔터티 형식에는 School 모델입니다.

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

쿼리만 포함 된 멤버의 하위 집합을 반환 하기 때문에 **부서** 저장소 모델의 형식에에서는 **부서** School 모델의 형식이 다음과 같이이 매핑을 기반으로 수정 되었습니다.

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

다음 예제에 나와 있는 **QueryView** 의 자식 요소로 **AssociationSetMapping** 요소에 대 한 읽기 전용 매핑을 정의 하 고는 `FK_Course_Department` School 모델에 연결 합니다.

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
 
### <a name="comments"></a>설명

다음 시나리오가 가능하도록 쿼리 뷰를 정의할 수 있습니다.

-   스토리지 모델에 있는 엔터티의 모든 속성을 포함하지 않는 엔터티를 개념적 모델에서 정의합니다. 여기에 기본값이 없습니다를 지원 하지 않는 속성 **null** 값입니다.
-   스토리지 모델의 계산 열을 개념적 모델의 엔터티 형식 속성에 매핑합니다.
-   개념적 모델의 엔터티를 분할하는 데 사용된 조건이 같음을 기반으로 하지 않는 매핑을 정의합니다. 사용 하 여 조건부 매핑을 지정 하면 합니다 **조건** 요소에 제공된 된 조건을 지정 된 값과 같아야 합니다. 자세한 내용은 조건 요소 (MSL)을 참조 하세요.
-   스토리지 모델의 같은 열을 개념적 모델의 여러 형식에 매핑합니다.
-   여러 형식을 같은 테이블에 매핑합니다.
-   관계형 스키마에서 외래 키를 기반으로 하지 않는 연결을 개념적 모델에서 정의합니다.
-   사용자 지정 비즈니스 논리를 사용하여 개념적 모델에서 속성 값을 설정합니다. 예를 들어의 문자열 값 "T" 값으로 데이터 원본에 매핑할 수 있습니다 **true**, 개념적 모델에는 부울 값입니다.
-   쿼리 결과에 대한 조건부 필터를 정의합니다.
-   스토리지 모델보다 더 적은 제한을 개념적 모델의 데이터에 대해 적용합니다. 예를 들어, 할 수 있습니다 속성 개념적 모델의 null 허용 열이 매핑되는 지원 하지 않는 경우에 **null**값입니다.

엔터티에 대한 쿼리 뷰를 정의할 때는 다음 사항을 고려해야 합니다.

-   쿼리 뷰는 읽기 전용입니다. 수정 함수를 사용하여 엔터티를 업데이트할 수만 있습니다.
-   쿼리 뷰로 엔터티 형식을 정의하는 경우 관련된 모든 엔터티도 쿼리 뷰로 정의해야 합니다.
-   다 대 다 연결을 관계형 스키마의 링크 테이블을 나타내는 저장소 모델의 엔터티에 매핑되는 경우 정의 해야 합니다는 **QueryView** 요소에는 **AssociationSetMapping** 이 링크 테이블에 대 한 요소입니다.
-   쿼리 뷰는 형식 계층 구조의 모든 형식에 대해 정의되어야 합니다. 다음과 같은 방법으로 이 작업을 수행할 수 있습니다.
-   -   단일 **QueryView** 계층의 모든 엔터티 형식의 합집합을 반환 하는 단일 Entity SQL 쿼리를 지정 하는 요소입니다.
    -   단일 **QueryView** CASE 연산자를 사용 하 여 계층 구조에서 특정 엔터티 형식을 반환 하는 단일 Entity SQL 쿼리를 지정 하는 요소는 특정 조건에 기반 합니다.
    -   추가 사용 하 여 **QueryView** 계층 구조의 특정 형식에 대 한 요소입니다. 이 예에서 사용 하 여를 **TypeName** 특성을 **QueryView** 각 뷰의 엔터티 형식을 지정 하는 요소입니다.
-   쿼리 뷰를 정의할 때 지정할 수 없습니다는 **StorageSetName** 특성을 **EntitySetMapping** 요소입니다.
-   쿼리 뷰를 정의할 때 합니다 **EntitySetMapping**요소를 포함할 수 없습니다 **속성** 매핑.

## <a name="resultbinding-element-msl"></a>ResultBinding 요소(MSL)

합니다 **ResultBinding** MSL (매핑 사양 언어)의 요소에서 반환 되는 저장된 프로시저를 개념적 모델의 엔터티 속성 엔터티 형식 수정 함수가 매핑되는 경우에 저장 된 열 값을 매핑됩니다 기본 데이터베이스에 대 한 절차입니다. 예를 들어 id 열 값을 삽입 하 여 반환 될 때 저장 프로시저는 **ResultBinding** 요소 개념적 모델의 엔터티 형식 속성에 반환 되는 값을 매핑합니다.

합니다 **ResultBinding** InsertFunction 요소 또는 UpdateFunction 요소의 자식 요소일 수 있습니다.

합니다 **ResultBinding** 요소는 모든 자식 요소를 포함할 수 없습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 해당 하는 특성을 설명 합니다 **ResultBinding** 요소:

| 특성 이름 | 필수 여부 | 값                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **이름**       | 예         | 매핑되는 개념적 모델의 엔터티 속성 이름입니다. |
| **ColumnName** | 예         | 매핑되는 열의 이름입니다.                                          |

### <a name="example"></a>예제

다음 예제에서는 School 모델을 기반으로 하며 표시를 **InsertFunction** 대 한 삽입 함수를 매핑하는 데 사용 되는 요소는 **Person** 엔터티 형식을 **InsertPerson** 저장된 프로시저입니다. (합니다 **InsertPerson** 저장된 프로시저는 아래 및 저장소 모델에서 선언 됩니다.) A **ResultBinding** 요소는 저장된 프로시저에서 반환 되는 열 값에 매핑할 때 사용 됩니다 (**NewPersonID**)을 엔터티 형식 속성 (**PersonID**).

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

다음 TRANSACT-SQL에 설명 합니다 **InsertPerson** 저장 프로시저:

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

합니다 **ResultMapping** MSL (매핑 사양 언어)에서 요소 다음에 해당할 때 기본 데이터베이스에서 개념적 모델의 function import와 저장된 프로시저 간의 매핑을 정의 합니다.

-   Function Import가 개념적 모델 엔터티 형식 또는 복합 형식을 반환하는 경우
-   저장 프로시저에서 반환되는 열 이름이 엔터티 형식 또는 복합 형식의 속성 이름과 정확히 일치하지 않는 경우

기본적으로 저장 프로시저에서 반환되는 열과 엔터티 형식 또는 복합 형식 간의 매핑은 열 및 속성 이름을 기반으로 합니다. 열 이름이 정확히 일치 하지 않는 속성 이름, 경우에 사용 해야 합니다 **ResultMapping** 매핑을 정의 하는 요소입니다. 기본 매핑의 예 FunctionImportMapping 요소 (MSL)을 참조 하세요.

합니다 **ResultMapping** FunctionImportMapping 요소 자식 요소입니다.

합니다 **ResultMapping** 요소는 다음 자식 요소를 포함할 수 있습니다.

-   EntityTypeMapping (0 개 이상)
-   ComplexTypeMapping

적용할 수 없는 특성을 **ResultMapping** 요소입니다.

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

이전 엔터티 형식의 인스턴스를 반환 하는 function import를 만들기 위해 열 간의 매핑이 저장된 프로시저에서 반환 하 고 엔터티 형식에 정의 되어야 합니다는 **ResultMapping** 요소:

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

합니다 **ScalarProperty** MSL (매핑 사양 언어)의 요소는 개념적 모델 엔터티 형식, 복합 형식 또는 연결의 속성을 테이블 열 또는 기본 데이터베이스의 저장된 프로시저 매개 변수를에 매핑됩니다.

> [!NOTE]
> 수정 함수가 매핑되는 저장 프로시저는 스토리지 모델에서 선언되어야 합니다. 자세한 내용은 함수 요소 (SSDL)을 참조 하세요.

합니다 **ScalarProperty** 요소에는 다음 요소의 자식일 수 있습니다.

-   MappingFragment
-   InsertFunction
-   UpdateFunction
-   DeleteFunction
-   EndProperty
-   ComplexProperty
-   ResultMapping

자식으로는 **MappingFragment**를 **ComplexProperty**, 또는 **EndProperty** 요소를 **ScalarProperty** 요소 속성을 매핑합니다 데이터베이스에서 열을 개념적 모델. 자식으로는 **InsertFunction**를 **UpdateFunction**, 또는 **DeleteFunction** 요소를 **ScalarProperty** 요소 속성을 매핑합니다 저장된 프로시저 매개 변수에 개념적 모델.

합니다 **ScalarProperty** 요소는 모든 자식 요소를 포함할 수 없습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

에 적용 되는 특성을 **ScalarProperty** 요소는 해당 요소의 역할에 따라 달라 집니다.

다음 표에서 경우 적용할 수 있는 특성을 설명 합니다 **ScalarProperty** 요소는 개념적 모델 속성 데이터베이스의 열에 매핑하는 데 사용:

| 특성 이름 | 필수 여부 | 값                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **이름**       | 예         | 매핑되는 개념적 모델 속성의 이름입니다. |
| **ColumnName** | 예         | 매핑되는 테이블 열의 이름입니다.              |

다음 표에서 해당 하는 특성을 설명 합니다 **ScalarProperty** 요소 개념적 모델 속성을 저장된 프로시저 매개 변수에 매핑할 사용 하는 경우:

| 특성 이름    | 필수 여부 | 값                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**          | 예         | 매핑되는 개념적 모델 속성의 이름입니다.                                                                                 |
| **ParameterName** | 예         | 매핑되는 매개 변수의 이름입니다.                                                                                                 |
| **Version**       | 아니요          | **현재** 나 **원래** 동시성 검사에 대 한 현재 값 또는 속성의 원래 값은 사용 하는 여부에 따라 합니다. |

### <a name="example"></a>예제

다음 예제는 **ScalarProperty** 두 가지 방법으로 사용 되는 요소:

-   속성을 매핑할를 **Person** 의 열에 엔터티 형식 합니다 **사용자**테이블.
-   속성을 매핑할를 **Person** 엔터티 형식 매개 변수에 **UpdatePerson** 저장 프로시저. 저장 프로시저는 스토리지 모델에서 선언해야 합니다.

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

에서는 다음 예제는 **ScalarProperty** 삽입 매핑 및 데이터베이스의 저장된 프로시저를 개념적 모델 연결의 함수를 삭제 하는 데 사용 되는 요소입니다. 저장 프로시저는 스토리지 모델에서 선언해야 합니다.

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

합니다 **UpdateFunction** MSL (매핑 사양 언어)에서 요소는 기본 데이터베이스의 저장된 프로시저를 개념적 모델에서 엔터티 형식의 업데이트 함수를 매핑합니다. 수정 함수가 매핑되는 저장 프로시저는 스토리지 모델에서 선언되어야 합니다. 자세한 내용은 함수 요소 (SSDL)을 참조 하세요.

> [!NOTE]
>  매핑되지 않는 경우 모든 세 개의 삽입, 업데이트 또는 삭제 저장된 프로시저에 엔터티 형식의 작업, 경우 런타임에 실행 하면 매핑되지 않은 작업이 실패 합니다 및는 UpdateException throw 됩니다.

합니다 **UpdateFunction** 요소 ModificationFunctionMapping 요소의 자식일 수 및 EntityTypeMapping 요소에 적용 합니다.

합니다 **UpdateFunction** 요소는 다음 자식 요소를 포함할 수 있습니다.

-   AssociationEnd (0 개 이상)
-   ComplexProperty (0 개 이상)
-   ResultBinding (0 또는 1)
-   ScarlarProperty (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **UpdateFunction** 요소입니다.

| 특성 이름            | 필수 여부 | 값                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | 예         | 업데이트 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다. 저장 프로시저는 스토리지 모델에서 선언해야 합니다. |
| **RowsAffectedParameter** | 아니요          | 영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.                                                                               |

### <a name="example"></a>예제

다음 예제에서는 School 모델을 기반으로 하며 표시를 **UpdateFunction** 업데이트 함수를 매핑하는 데 사용 되는 요소는 **Person** 엔터티 형식을 **UpdatePerson** 저장된 프로시저입니다. 합니다 **UpdatePerson** 저장된 프로시저는 저장소 모델에서 선언 됩니다.

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
