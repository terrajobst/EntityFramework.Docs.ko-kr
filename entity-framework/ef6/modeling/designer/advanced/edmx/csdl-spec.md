---
title: CSDL 사양-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 438af83b8a1ad51ee8414341181412e950d0e117
ms.sourcegitcommit: 29f928a6116771fe78f306846e6f2d45cbe8d1f4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460152"
---
# <a name="csdl-specification"></a>CSDL 사양
CSDL(개념 스키마 정의 언어)은 데이터 기반 애플리케이션의 개념적 모델을 구성하는 엔터티, 관계 및 함수를 설명하는 XML 기반 언어입니다. 이 개념적 모델은 Entity Framework 또는 WCF Data Services에서 사용할 수 있습니다. CSDL을 사용 하 여 설명 하는 메타 데이터 엔터티 및 데이터 원본에 개념적 모델에 정의 된 관계를 매핑할 Entity Framework에서 사용 됩니다. 자세한 내용은 [SSDL 사양](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) 하 고 [MSL 사양](~/ef6/modeling/designer/advanced/edmx/msl-spec.md)합니다.

CSDL은 엔터티 데이터 모델의 엔터티 프레임 워크의 구현입니다.

Entity Framework 응용 프로그램에서 개념적 모델 메타 데이터는 System.Data.Metadata.Edm.EdmItemCollection 인스턴스로 (CSDL로 작성).csdl 파일에서 로드 되 고에서 메서드를 사용 하 여 액세스할 수 합니다 System.Data.Metadata.Edm.MetadataWorkspace 클래스입니다. Entity Framework 개념적 모델 메타 데이터를 사용 하 여 개념적 모델을 데이터 소스 관련 명령에 대 한 쿼리를 변환 합니다.

EF 디자이너는.edmx 파일의 디자인 타임에 개념적 모델 정보를 저장합니다. 빌드 시 EF 디자이너는.edmx 파일의 정보는 필요한 Entity Framework에서 런타임에.csdl 파일을 만들려면 사용 합니다.

CSDL 버전은 XML 네임스페이스로 식별됩니다.

| CSDL 버전 | XML Namespace                                |
|:-------------|:---------------------------------------------|
| CSDL v1      | http://schemas.microsoft.com/ado/2006/04/edm |
| CSDL v2      | http://schemas.microsoft.com/ado/2008/09/edm |
| CSDL v3      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a>Association 요소(CSDL)

**연결** 요소 두 엔터티 형식 간의 관계를 정의 합니다. 연결은 관계에 관련된 엔터티 형식과 복합성이라도 하는 관계의 각 End에 있는 가능한 엔터티 형식 수를 지정해야 합니다. 연결 end의 복합성 값이 1 (1), 0 개 또는 1 (0..1) 또는 여러 열에 있을 수 있습니다 (\*). 이 정보는 두 개의 자식 End 요소에 지정 됩니다.

연결의 한 End에 있는 엔터티 형식 인스턴스는 엔터티 형식에서 노출된 경우 탐색 속성 또는 외래 키를 통해 액세스할 수 있습니다.

애플리케이션에서 연결 인스턴스는 엔터티 형식 인스턴스 사이의 특정 연결을 나타냅니다. 연결 인스턴스는 연결 집합에 논리적으로 그룹화됩니다.

**연결** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소)
-   끝 (정확히 두 개의 요소)
-   ReferentialConstraint (0 개 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **연결** 요소.

| 특성 이름 | 필수 여부 | 값                        |
|:---------------|:------------|:-----------------------------|
| **이름**       | 예         | 연결의 이름입니다. |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **연결** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제와 **연결** 정의 하는 요소는 **CustomerOrders** 외래 키에서 노출 되지 않는 하는 경우 연결을 **고객** 및  **순서** 엔터티 형식입니다. **복합성** 각각에 대 한 값 **끝** 나타내려면 연결의 많은 **주문** 에 연결할 수 있는 **고객**, 하지만 하나만 **고객** 와 연결 될 수는 **순서**합니다. 또한 합니다 **OnDelete** 요소에서 나타내는 모든 **주문** 특정 관련 된 **고객** 로 로드 된 및 ObjectContext 삭제할 경우는 **고객** 삭제 됩니다.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

다음 예제와 **연결** 정의 하는 요소를 **CustomerOrders** 외래 키에서 노출 되 면 연결을 **고객** 및  **순서** 엔터티 형식입니다. 노출 된 외래 키를 사용 하 여 엔터티 간의 관계를 사용 하 여 관리 되는 **ReferentialConstraint** 요소입니다. AssociationSetMapping 요소를 해당 데이터 원본에이 연결을 매핑할 필요는 없습니다.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
   <ReferentialConstraint>
        <Principal Role="Customer">
            <PropertyRef Name="Id" />
        </Principal>
        <Dependent Role="Order">
             <PropertyRef Name="CustomerId" />
         </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="associationset-element-csdl"></a>AssociationSet 요소(CSDL)

합니다 **AssociationSet** 요소 (CSDL) 개념 스키마 정의 언어에는 같은 형식의 연결 인스턴스에 대 한 논리적 컨테이너입니다. 연결 집합은 연결 인스턴스가 데이터 소스로 매핑될 수 있도록 해당 연결 인스턴스를 그룹화하는 데 필요한 정의를 제공합니다.  

합니다 **AssociationSet** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소 허용)
-   끝 (정확히 두 개의 요소 필요)
-   Annotation 요소 (0 개 이상의 요소 허용)

합니다 **연결** 특성 연결 집합을 포함 하는 연결의 형식을 지정 합니다. 연결 집합의 끝을 구성 하는 엔터티 집합은 정확히 두 명의 자식으로 지정 됩니다 **최종** 요소입니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **AssociationSet** 요소입니다.

| 특성 이름  | 필수 여부 | 값                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**        | 예         | 엔터티 집합의 이름입니다. 값을 **이름을** 특성의 값과 같을 수 없습니다는 **연결** 특성입니다.                                 |
| **연결** | 예         | 연결 집합에 포함되는 인스턴스의 연결에 대한 정규화된 이름입니다. 연결은 연결 집합과 동일한 네임스페이스에 있어야 합니다. |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **AssociationSet** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제는 **EntityContainer** 요소 두 개가 **AssociationSet** 요소:

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="collectiontype-element-csdl"></a>CollectionType 요소(CSDL)

합니다 **CollectionType** 요소 (CSDL) 개념 스키마 정의 언어의 함수 매개 변수 또는 함수 반환 형식이 컬렉션이 지정 합니다. 합니다 **CollectionType** 매개 변수 요소 또는 ReturnType (함수) 요소의 자식 요소일 수 있습니다. 컬렉션의 형식 중 하나를 사용 하 여 지정할 수 있습니다 합니다 **형식** 특성 또는 다음 자식 요소 중 하나:

-   **CollectionType**
-   ReferenceType
-   RowType
-   TypeRef

> [!NOTE]
> 컬렉션의 형식을 모두 사용 하 여 지정 된 모델을 검사 하지 않으며를 **형식** 특성 및 자식 요소입니다.

 

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **CollectionType** 요소입니다. 합니다 **DefaultValue**, **MaxLength**를 **FixedLength**, **정밀도**를 **확장**,  **유니코드**, 및 **데이터 정렬을** 특성은 해당 컬렉션에만 **EDMSimpleTypes**합니다.

| 특성 이름                                                          | 필수 여부 | 값                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**                                                                | 아니요          | 컬렉션의 형식입니다.                                                                                                                                                                                                      |
| **Null 허용**                                                            | 아니요          | **True 이면** (기본값) 또는 **False** 속성에 null 값을 사용할 수 있는지 여부에 따라 합니다. <br/> [!NOTE]                                                                                                                 |
| > 복합 형식 속성 CSDL v1에서 있어야 `Nullable="False"`합니다. |             |                                                                                                                                                                                                                                  |
| **DefaultValue**                                                        | 아니요          | 속성의 기본값입니다.                                                                                                                                                                                               |
| **MaxLength**                                                           | 아니요          | 속성 값의 최대 길이입니다.                                                                                                                                                                                        |
| **FixedLength**                                                         | 아니요          | **True 이면** 나 **False** 고정된 길이 문자열로 속성 값이 저장될지 여부에 따라 합니다.                                                                                                                           |
| **전체 자릿수**                                                           | 아니요          | 속성 값의 전체 자릿수입니다.                                                                                                                                                                                             |
| **배율**                                                               | 아니요          | 속성 값의 소수 자릿수입니다.                                                                                                                                                                                                 |
| **SRID**                                                                | 아니요          | 시스템 공간 참조 식별자입니다. 공간 형식의 속성에 대해서만 유효 합니다.   자세한 내용은 [SRID](http://en.wikipedia.org/wiki/SRID) 고 [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) |
| **유니코드**                                                             | 아니요          | **True 이면** 나 **False** 유니코드 문자열로 속성 값이 저장될지 여부에 따라 합니다.                                                                                                                                |
| **데이터 정렬**                                                           | 아니요          | 데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.                                                                                                                                                    |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **CollectionType** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제를 사용 하는 모델 정의 함수를 보여 줍니다.는 **CollectionType** 함수의 컬렉션을 반환 하는지 지정 하는 요소 **Person** 엔터티 형식 (합니다 를사용하여지정된대로**ElementType** 특성).

``` xml
 <Function Name="LastNamesAfter">
        <Parameter Name="someString" Type="Edm.String"/>
        <ReturnType>
             <CollectionType  ElementType="SchoolModel.Person"/>
        </ReturnType>
        <DefiningExpression>
             SELECT VALUE p
             FROM SchoolEntities.People AS p
             WHERE p.LastName >= someString
        </DefiningExpression>
 </Function>
```
 

다음 예제에서는 사용 하는 모델 정의 함수를 **CollectionType** 는 반환 행 컬렉션을 지정 하는 요소 (에 지정 된 대로 합니다 **RowType** 요소).

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

다음 예제에서는 사용 하는 모델 정의 함수는 **CollectionType** 함수가 허용의 컬렉션에 매개 변수로 지정 하는 요소 **부서** 엔터티 형식입니다.

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="complextype-element-csdl"></a>ComplexType 요소(CSDL)

A **ComplexType** 이루어진 데이터 구조를 정의 하는 요소 **EdmSimpleType** 속성 또는 다른 복합 형식입니다.  복합 형식은 엔터티 형식 또는 다른 복합 형식의 속성일 수 있습니다. 복합 형식은 복합 형식에서 데이터를 정의한다는 점에서 엔터티 형식과 유사합니다. 그러나 복합 형식과 엔터티 형식 사이에는 약간의 중요한 차이점이 존재합니다.

-   복합 형식은 식별자나 키를 포함하지 않으므로 독립적으로 존재할 수 없습니다. 복합 형식은 엔터티 형식 또는 다른 복합 형식의 속성으로만 존재할 수 있습니다.
-   복합 형식은 연결에 참여할 수 없습니다. 모두 연결의 end도 복합 형식이 수 및 복합 형식에 대 한 탐색 속성을 정의할 수 없습니다 때문입니다.
-   복합 형식의 스칼라 속성을 각각 null로 설정할 수 있지만 복합 형식 속성의 값은 null일 수 없습니다.

A **ComplexType** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소)
-   속성 (0 개 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

아래 표에서 설명에 적용할 수 있는 특성을 **ComplexType** 요소입니다.

| 특성 이름                                                                                                 | 필수 여부 | 값                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 이름                                                                                                           | 예         | 복합 형식의 이름입니다. 복합 형식의 이름은 모델 범위 내에 있는 다른 복합 형식, 엔터티 형식 또는 연결의 이름과 같을 수 없습니다. |
| BaseType                                                                                                       | 아니요          | 정의되는 복합 형식의 기본 형식인 다른 복합 형식의 이름입니다. <br/> [!NOTE]                                                                     |
| >이 특성은 CSDL v1에 적용할 수 없습니다. 복합 형식에 대한 상속은 해당 버전에서 지원되지 않습니다. |             |                                                                                                                                                                                     |
| 추상                                                                                                       | 아니요          | **True 이면** 나 **False** (기본값) 복합 형식이 추상 형식 인지에 따라 합니다. <br/> [!NOTE]                                                                  |
| >이 특성은 CSDL v1에 적용할 수 없습니다. 해당 버전의 복합 형식은 추상 형식일 수 없습니다.         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **ComplexType** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 복합 형식을 보여 줍니다 **주소**를 사용 하 여 합니다 **EdmSimpleType** 속성 **StreetAddress**, **City**,  **StateOrProvince**하십시오 **국가**, 및 **PostalCode**합니다.

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

복합 형식 정의 **주소** (위 참조)는 엔터티 형식의 속성으로 선언 해야 엔터티 형식 정의의 속성 형식입니다. 다음 예제는 **주소** 엔터티 형식의 복합 형식으로 속성 (**게시자**):

``` xml
 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BooksModel.Address" Name="Address" Nullable="false" />
       <NavigationProperty Name="Books" Relationship="BooksModel.PublishedBy"
                           FromRole="Publisher" ToRole="Book" />
     </EntityType>
```
 

 

## <a name="definingexpression-element-csdl"></a>DefiningExpression 요소(CSDL)

합니다 **DefiningExpression** (CSDL) 개념 스키마 정의 언어에서 요소는 개념적 모델의 함수를 정의 하는 Entity SQL 식을 포함 합니다.  

> [!NOTE]
> 유효성 검사를 위해 한 **DefiningExpression** 요소에는 임의의 콘텐츠가 포함 될 수 있습니다. 그러나 Entity Framework는 경우 예외를 throw 런타임에 **DefiningExpression** 요소에 유효한 Entity SQL이 없습니다.

 

### <a name="applicable-attributes"></a>적용 가능한 특성

주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **DefiningExpression** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제에서는 한 **DefiningExpression** 출판 된 이후 지난 연도 수를 반환 하는 함수를 정의 하는 요소입니다. 콘텐츠를 **DefiningExpression** Entity SQL에서 요소가 기록 됩니다.

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a>Dependent 요소(CSDL)

합니다 **종속** 요소 (CSDL) 개념 스키마 정의 언어에서 ReferentialConstraint 요소에 자식 요소 및 참조 제약 조건의 종속 끝을 정의 합니다. A **ReferentialConstraint** 요소는 관계형 데이터베이스의 참조 무결성 제약 조건과 유사한 기능을 정의 합니다. 데이터베이스 테이블의 열에서 다른 테이블의 기본 키를 참조하는 방식과 동일하게 엔터티 형식의 속성은 다른 엔터티 형식의 엔터티 키를 참조할 수 있습니다. 참조 되는 엔터티 형식 이라고 합니다 *주 끝* 제약 조건입니다. 주 끝을 참조 하는 엔터티 형식 이라고 합니다 *종속 끝* 제약 조건입니다. **PropertyRef** 요소는 주 끝을 참조 하는 키를 지정 하는 데 사용 됩니다.

합니다 **종속** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   PropertyRef (하나 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **종속** 요소.

| 특성 이름 | 필수 여부 | 값                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **역할**       | 예         | 연결의 종속 끝에 있는 엔터티 형식의 이름입니다. |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **종속** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

에서는 다음 예제는 **ReferentialConstraint** 요소 정의의 일부로 사용 되는 **PublishedBy** 연결 합니다. **PublisherId** 의 속성을 **책** 엔터티 형식 참조 제약 조건의 종속 끝 이루어집니다.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="documentation-element-csdl"></a>Documentation 요소(CSDL)

합니다 **설명서** 부모 요소에 정의 된 개체에 대 한 정보를 제공 하는 개념 스키마 정의 언어 (CSDL)는 요소를 사용할 수 있습니다. .Edmx 파일의 경우는 **설명서** 요소 내용의 (예: 엔터티, 연결 또는 속성), EF 디자이너의 디자인 화면에 개체로 나타나는 요소의 자식인는 **설명서**  요소는 Visual Studio에 표시 됩니다 **속성** 창 개체에 대 한 합니다.

합니다 **설명서** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   **요약**: 부모 요소에 대 한 간략 한 설명입니다. (0개 또는 한 개의 요소)
-   **LongDescription**: 부모 요소에 대 한 자세한 설명입니다. (0개 또는 한 개의 요소)
-   Annotation 요소입니다. (0개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **설명서** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제는 **설명서** EntityType 요소의 자식 요소로 요소입니다. 아래 코드 조각에에서 있던 경우 CSDL 콘텐츠를.edmx 파일의 내용을 합니다 **요약** 하 고 **LongDescription** 요소는 Visual Studio에 나타납니다 **속성** 클릭할 때 창이 `Customer` 엔터티 형식입니다.

``` xml
 <EntityType Name="Customer">
    <Documentation>
      <Summary>Summary here.</Summary>
      <LongDescription>Long description here.</LongDescription>
    </Documentation>
    <Key>
      <PropertyRef Name="CustomerId" />
    </Key>
    <Property Type="Int32" Name="CustomerId" Nullable="false" />
    <Property Type="String" Name="Name" Nullable="false" />
 </EntityType>
```
 

 

## <a name="end-element-csdl"></a>End 요소(CSDL)

합니다 **최종** 요소 (CSDL) 개념 스키마 정의 언어의 Association 요소 또는 AssociationSet 요소의 자식일 수 있습니다. 각각의 경우 역할을 합니다 **최종** 요소는 다른 및 해당 특성은 서로 다릅니다.

### <a name="end-element-as-a-child-of-the-association-element"></a>Association 요소의 자식인 End 요소

**끝** 요소 (자식으로는 **연결** 요소) 연결의 해당 end에 있을 수 있는 엔터티 형식 인스턴스 수는 연결의 end에 엔터티 형식을 식별 합니다. 연결 End는 연결의 일부로 정의되고 연결에는 정확히 두 개의 연결 End가 있어야 합니다. 연결의 한 End에 있는 엔터티 형식 인스턴스는 엔터티 형식에서 노출된 경우 탐색 속성 또는 외래 키를 통해 액세스할 수 있습니다.  

**최종** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소)
-   OnDelete (0 개 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 특성을 적용할 수 있습니다는 **끝** 의 자식인 경우 요소는 **연결** 요소입니다.

| 특성 이름   | 필수 여부 | 값                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**         | 예         | 연결의 한 End에 있는 엔터티 형식의 이름                                                                                                                                                                                                                                                                                                                                                         |
| **역할**         | 아니요          | 연결 End의 이름. 이름을 지정하지 않으면 연결 End에 있는 엔터티 형식의 이름이 사용됩니다.                                                                                                                                                                                                                                                                                           |
| **다중성** | 예         | **1**하십시오 **0..1**, 또는 **\*** 연결의 end에 있을 수 있는 엔터티 형식 인스턴스 수에 따라 합니다. <br/> **1** 연결 end에 정확히 하나의 엔터티 형식 인스턴스가 있음을 나타냅니다. <br/> **0..1** 연결 end에 0 개 이상의 엔터티 형식 인스턴스가 있음을 나타냅니다. <br/> **\*** 연결 end에 0, 1 또는 더 많은 엔터티 형식 인스턴스가 있음을 나타냅니다. |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **최종** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

#### <a name="example"></a>예제

에서는 다음 예제는 **연결** 정의 하는 요소는 **CustomerOrders** 연결 합니다. **복합성** 각각에 대 한 값 **끝** 나타내려면 연결의 많은 **주문** 에 연결할 수 있는 **고객**, 하지만 하나만 **고객** 와 연결 될 수는 **순서**합니다. 또한 합니다 **OnDelete** 요소에서 나타내는 모든 **주문** 관련 된 특정 **고객** 는 로드 된 ObjectContext 됩니다 삭제 된 경우에는 **고객** 삭제 됩니다.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a>AssociationSet 요소의 자식인 End 요소

합니다 **최종** 요소는 연결 집합의 한 end를 지정 합니다. **AssociationSet** 요소 두 개 있어야 **끝** 요소입니다. 에 포함 된 정보는 **최종** 요소 연결 데이터 원본 집합을 매핑할 때 사용 됩니다.

**최종** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

> [!NOTE]
> Annotation 요소는 다른 모든 자식 요소 뒤에 와야 합니다. Annotation 요소는 CSDL v2 이상에 허용 됩니다.

 

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 특성을 적용할 수 있습니다는 **끝** 의 자식인 경우 요소는 **AssociationSet** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EntitySet**  | 예         | 이름을 합니다 **EntitySet** 부모의 한쪽 끝을 정의 하는 요소 **AssociationSet** 요소입니다. 합니다 **EntitySet** 부모와 동일한 엔터티 컨테이너에 요소를 정의 해야 **AssociationSet** 요소입니다. |
| **역할**       | 아니요          | 연결 집합 End의 이름. 경우는 **역할** 특성이 사용 되지 않습니다, 연결 집합 end의 이름에는 엔터티 집합의 이름이 됩니다.                                                                   |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **최종** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

#### <a name="example"></a>예제

에서는 다음 예제는 **EntityContainer** 요소 두 개가 **AssociationSet** 요소 두 개의 각 **끝** 요소:

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitycontainer-element-csdl"></a>EntityContainer 요소(CSDL)

합니다 **EntityContainer** (CSDL) 개념 스키마 정의 언어에서 요소는 엔터티 집합, 연결 집합 및 함수 가져오기에 대 한 논리적 컨테이너입니다. 개념적 모델 엔터티 컨테이너 EntityContainerMapping 요소를 통해 저장소 모델 엔터티 컨테이너에 매핑됩니다. 스토리지 모델 엔터티 컨테이너는 데이터베이스의 구조를 설명합니다. 엔터티 집합은 데이터베이스의 테이블을 설명하고, 연결 집합은 데이터베이스의 외래 키 제약 조건을 설명하고, 함수 가져오기는 데이터베이스의 저장 프로시저를 설명합니다.

**EntityContainer** 요소 0 개 이상의 Documentation 요소를 포함할 수 있습니다. 경우는 **설명서** 요소가 있는지, 모든 나와야 **EntitySet**, **AssociationSet**, 및 **FunctionImport** 요소.

**EntityContainer** 요소 (나열 된 순서로) 0 개 이상의 자식 요소를 포함할 수 있습니다.

-   EntitySet
-   AssociationSet
-   FunctionImport
-   Annotation 요소

확장할 수 있습니다는 **EntityContainer** 의 다른 내용을 포함 하는 요소 **EntityContainer** 동일한 네임 스페이스입니다. 다른 콘텐츠를 포함 하려면 **EntityContainer**를 참조 **EntityContainer** 값을 설정 하는 요소는 **확장** 는의이름으로특성 **EntityContainer** 포함 하려는 요소입니다. 포함 된 모든 자식 요소 **EntityContainer** 요소를 참조 하는 자식 요소로 간주 됩니다 **EntityContainer** 요소입니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 테이블에 적용할 수 있는 특성을 설명 합니다.는 **사용 하 여** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **이름**       | 예         | 엔터티 컨테이너의 이름입니다.                               |
| **확장**    | 아니요          | 동일한 네임스페이스 내에 있는 다른 엔터티 컨테이너의 이름입니다. |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **EntityContainer** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제는 **EntityContainer** 세 개의 엔터티 집합과 두 개의 연결 집합을 정의 하는 요소입니다.

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entityset-element-csdl"></a>EntitySet 요소(CSDL)

합니다 **EntitySet** 개념 스키마 정의 언어 요소는 엔터티 형식의 인스턴스 및 해당 엔터티 형식에서 파생 된 형식의 인스턴스에 대 한 논리적 컨테이너입니다. 엔터티 형식과 엔터티 집합 간의 관계는 관계형 데이터베이스의 행과 테이블 간 관계와 유사합니다. 행과 같이 엔터티 형식은 관련 데이터 집합을 정의하고, 테이블과 같이 엔터티 집합은 이러한 정의의 인스턴스를 포함합니다. 엔터티 집합은 엔터티 형식 인스턴스가 데이터 소스의 관련 데이터 구조에 매핑될 수 있도록 이러한 인스턴스를 그룹화하는 구문을 제공합니다.  

특정 엔터티 형식에 대한 두 개 이상의 엔터티 집합을 정의할 수 있습니다.

> [!NOTE]
> EF 디자이너 형식별 다중 엔터티 집합을 포함 하는 개념적 모델을 지원 하지 않습니다.

 

합니다 **EntitySet** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   Documentation 요소 (0 개 이상의 요소 허용)
-   Annotation 요소 (0 개 이상의 요소 허용)

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **EntitySet** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **이름**       | 예         | 엔터티 집합의 이름입니다.                                                              |
| **EntityType** | 예         | 엔터티 집합에 포함되는 인스턴스의 엔터티 형식에 대한 정규화된 이름입니다. |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **EntitySet** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제는 **EntityContainer** 3 개 요소 **EntitySet** 요소:

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

형식당 여러 엔터티 집합(MEST)을 정의할 수 있습니다. 다음 예제에서는 두 개의 엔터티 집합이 있는 엔터티 컨테이너를 정의 합니다 **책** 엔터티 형식:

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="FictionBooks" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="BookAuthor" Association="BooksModel.BookAuthor">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitytype-element-csdl"></a>EntityType 요소(CSDL)

합니다 **EntityType** 요소, 고객 또는 개념적 모델의 순서와 같은 최상위 개념의 구조를 나타냅니다. 엔터티 형식은 애플리케이션에서 엔터티 형식 인스턴스에 대한 템플릿입니다. 각 템플릿에는 다음 정보가 들어 있습니다.

-   고유한 이름 (필수)
-   하나 이상의 속성에 의해 정의되는 엔터티 키 (필수)
-   상위 데이터의 속성 (선택적 요소)
-   연결의 한 End에서 다른 End로의 탐색을 허용하는 탐색 속성 (선택적 요소)

애플리케이션에서 엔터티 형식의 인스턴스는 특정 고객 또는 주문과 같은 특정 개체를 나타냅니다. 엔터티 집합 내에 각 엔터티 형식 인스턴스에 대한 고유한 엔터티 키가 있어야 합니다.

두 엔터티 형식 인스턴스는 형식이 동일하고 해당 엔터티 키 값이 동일한 경우에만 동일한 것으로 간주됩니다.

**EntityType** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소)
-   키 (0 개 이상의 요소)
-   속성 (0 개 이상의 요소)
-   NavigationProperty (0 개 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **EntityType** 요소입니다.

| 특성 이름                                                                                                                                  | 필수 여부 | 값                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| **이름**                                                                                                                                        | 예         | 엔터티 형식의 이름.                                                                     |
| **BaseType**                                                                                                                                    | 아니요          | 정의되는 엔터티 형식의 기본 형식인 다른 엔터티 형식의 이름입니다.  |
| **추상**                                                                                                                                    | 아니요          | **True 이면** 나 **False**엔터티 형식이 추상 형식 인지에 따라 합니다.                 |
| **OpenType**                                                                                                                                    | 아니요          | **True 이면** 나 **False** 엔터티 형식 인지 열린 엔터티 형식에 따라 합니다. <br/> [!NOTE] |
| >은 **OpenType** 특성은 ADO.NET Data Services를 사용 하 여 사용 되는 개념적 모델에 정의 된 엔터티 형식에 적용할 수만 있습니다. |             |                                                                                                  |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **EntityType** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제는 **EntityType** 3 개 요소 **속성** 요소 및 두 개의 **NavigationProperty** 요소:

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="enumtype-element-csdl"></a>EnumType 요소 (CSDL)

합니다 **EnumType** 요소는 열거 형식을 나타냅니다.

**EnumType** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소)
-   멤버 (0 개 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **EnumType** 요소입니다.

| 특성 이름     | 필수 여부 | 값                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**           | 예         | 엔터티 형식의 이름.                                                                                                                                                                  |
| **IsFlags**        | 아니요          | **True 이면** 나 **False**플래그 집합으로 열거형 형식을 사용할 수 있는지 여부에 따라 합니다. 기본값은 **False.** 합니다.                                                                     |
| **UnderlyingType** | 아니요          | **Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** 하거나 **Edm.SByte** 형식 값의 범위를 정의 합니다.   유형의 열거형 요소의 기본적인 기본 됩니다 **Edm.Int32.** 합니다. |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **EnumType** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제는 **EnumType** 3 개 요소 **멤버** 요소:

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a>Function 요소(CSDL)

합니다 **함수** 개념 스키마 정의 언어 (CSDL) 요소가 정의 하거나 개념적 모델의 함수 선언에 사용 됩니다. DefiningExpression 요소를 사용 하 여 함수가 정의 됩니다.  

A **함수** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소)
-   매개 변수 (0 개 이상의 요소)
-   DefiningExpression (0 개 이상의 요소)
-   ReturnType (Function) (0 개 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

반환 함수에 대 한 형식을 사용 하 여 지정 해야 합니다 **ReturnType** (Function) 요소 또는 **ReturnType** 특성 (아래 참조) 중 하나만 합니다. EdmSimpleType, 엔터티 형식, 복합 형식, 행 형식 또는 참조 형식(또는 이러한 형식 중 하나의 컬렉션)이 반환 형식이 될 수 있습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **함수** 요소.

| 특성 이름 | 필수 여부 | 값                              |
|:---------------|:------------|:-----------------------------------|
| **이름**       | 예         | 함수의 이름.          |
| **ReturnType** | 아니요          | 함수에서 반환하는 형식입니다. |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **함수** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 한 **함수** 강사가 고용 된 이후 지난 연도 수를 반환 하는 함수를 정의 하는 요소입니다.

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a>FunctionImport 요소(CSDL)

합니다 **FunctionImport** 요소 (CSDL) 개념 스키마 정의 언어에서에 데이터 원본에 정의 되어 있지만 개념적 모델을 통해 개체에 사용할 수 있는 함수를 나타냅니다. 예를 들어, 저장소 모델의 Function 요소는 데이터베이스의 저장된 프로시저를 나타내는 데 사용할 수 있습니다. A **FunctionImport** 요소는 개념적 모델의 Entity Framework 응용 프로그램에 해당 하는 함수를 나타내며 FunctionImportMapping 요소를 사용 하 여 저장소 모델 함수로 매핑됩니다. 애플리케이션에서 함수가 호출되면 해당되는 저장 프로시저가 데이터베이스에서 실행됩니다.

합니다 **FunctionImport** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소 허용)
-   매개 변수 (0 개 이상의 요소 허용)
-   Annotation 요소 (0 개 이상의 요소 허용)
-   ReturnType (FunctionImport) (0 개 이상의 요소 허용)

하나의 **매개 변수** 함수가 받아들이는 각 매개 변수에 대 한 요소를 정의 해야 합니다.

반환 함수에 대 한 형식을 사용 하 여 지정 해야 합니다 **ReturnType** (FunctionImport) 요소 또는 **ReturnType** 특성 (아래 참조) 중 하나만 있습니다. 반환 형식이 값 EdmSimpleType, EntityType 또는 ComplexType 컬렉션인 이어야 합니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **FunctionImport** 요소입니다.

| 특성 이름   | 필수 여부 | 값                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**         | 예         | 가져온 함수의 이름입니다.                                                                                                                                                                    |
| **ReturnType**   | 아니요          | 함수에서 반환하는 형식입니다. 함수에서 값을 반환하지 않으면 이 특성을 사용하지 마십시오. 그렇지 않으면 값 ComplexType, EntityType 또는 EDMSimpleType 컬렉션 이어야 합니다.        |
| **EntitySet**    | 아니요          | 엔터티 컬렉션을 반환 하는 경우 형식의 경우 값을 **EntitySet** 엔터티로 변경 해야은 컬렉션이 소속 된 합니다. 그렇지 않으면 합니다 **EntitySet** 특성을 사용할 수 없습니다. |
| **IsComposable** | 아니요          | 값 설정 된 경우 true 이면 함수는 구성 가능 (테이블 반환 함수) 및 LINQ 쿼리에서 사용할 수 있습니다.  기본값은 **false**입니다.                                                           |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **FunctionImport** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제는 **FunctionImport** 하나의 매개 변수를 받아들이고 엔터티 형식의 컬렉션을 반환 하는 요소:

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a>Key 요소(CSDL)

합니다 **키** 요소는 EntityType 요소의 자식 요소 및 정의 *엔터티 키* (속성 또는 id를 확인 하는 엔터티 형식의 속성 집합). 엔터티 키를 구성하는 속성은 디자인 타임에 선택됩니다. 엔터티 키 속성의 값은 런타임에 엔터티 집합 내에서 엔터티 형식 인스턴스를 고유하게 식별해야 합니다. 엔터티 키를 구성하는 속성을 선택하여 엔터티 집합에서 인스턴스의 고유성을 보장해야 합니다. 합니다 **키** 요소는 엔터티 형식의 속성 중 하나 이상을 참조 하 여 엔터티 키를 정의 합니다.

합니다 **키** 요소는 다음 자식 요소를 포함할 수 있습니다.

-   PropertyRef (하나 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **키** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

아래 예제에서는 명명 된 엔터티 형식을 정의 **책**합니다. 엔터티 키가 참조 하 여 정의 된 **ISBN** 엔터티 형식의 속성입니다.

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

합니다 **ISBN** 속성 이므로 엔터티 키에 적합 한 책을 고유 하 게 식별 하는 국제 표준 책 수 (ISBN).

다음 예제에서는 엔터티 형식 (**작성자**) 두 가지 속성으로 구성 되는 엔터티 키를 가진 **이름** 하 고 **주소**합니다.

``` xml
 <EntityType Name="Author">
   <Key>
     <PropertyRef Name="Name" />
     <PropertyRef Name="Address" />
   </Key>
   <Property Type="String" Name="Name" Nullable="false" />
   <Property Type="String" Name="Address" Nullable="false" />
   <NavigationProperty Name="Books" Relationship="BooksModel.WrittenBy"
                       FromRole="Author" ToRole="Book" />
 </EntityType>
```
 

사용 하 여 **이름을** 하 고 **주소** 엔터티에 대 한 키가 적절 한 선택 하는 이름이 같은 두 저자가 같은 주소 지에서 거주할 가능성 없기 때문에 있습니다. 그러나 이러한 엔터티 키 선택이 엔터티 집합에서의 고유한 엔터티 키를 절대적으로 보장하지는 않습니다. 와 같은 속성을 추가 **AuthorId**를 고유 하 게 식별에 사용할 수 있는 작성자는이 경우에 권장 합니다.

 

## <a name="member-element-csdl"></a>Member 요소 (CSDL)

합니다 **멤버** 요소 EnumType 요소의 자식 요소 및 열거형 형식의 멤버를 정의 합니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **FunctionImport** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**       | 예         | 멤버의 이름입니다.                                                                                                                                                                  |
| **값**      | 아니요          | 멤버의 값입니다. 기본적으로 첫 번째 멤버의 값이 0 및 각 후속 열거자의 값이 1 씩 증가 합니다. 값이 같은 여러 멤버가 있을 수 있습니다. |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **FunctionImport** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제는 **EnumType** 3 개 요소 **멤버** 요소:

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a>NavigationProperty 요소(CSDL)

A **NavigationProperty** 요소가 연결의 반대쪽 끝에 대 한 참조를 제공 하는 탐색 속성을 정의 합니다. Property 요소를 사용 하 여 정의 된 속성과 달리 탐색 속성 모양 및 데이터의 특징을 정의 하지 않습니다. 이러한 속성은 두 엔터티 형식 사이의 연결을 탐색하는 방법을 제공합니다.

탐색 속성은 연결의 양쪽 End에 있는 엔터티 형식 모두에 대해 선택적 요소입니다. 연결의 End에 있는 한 엔터티 형식에 대해 탐색 속성을 정의하는 경우 연결의 다른 End에 있는 엔터티 형식에 대해서는 탐색 속성을 정의할 필요가 없습니다.

탐색 속성에 의해 반환된 데이터 형식은 해당 원격 연결 End의 복합성에 의해 결정됩니다. 예를 들어 탐색 속성을 **OrdersNavProp**에 **고객** 엔터티 형식 사이 일 대 다 연결을 탐색 하 고 **고객** 및  **순서**합니다. 탐색 속성에 대 한 원격 연결 end 복합성이 다 때문에 (\*), 해당 데이터 형식이 컬렉션이 (의 **순서**). 마찬가지로 경우 탐색 속성을 **CustomerNavProp**에 존재 합니다 **순서** 엔터티 형식에 해당 데이터 형식을 것 **고객** 원격 end의 복합성 이므로 하나 (1)입니다.

A **NavigationProperty** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **NavigationProperty** 요소.

| 특성 이름   | 필수 여부 | 값                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**         | 예         | 탐색 속성의 이름입니다.                                                                                                                                                                                                             |
| **관계** | 예         | 모델 범위 내에 있는 연결의 이름입니다.                                                                                                                                                                                |
| **ToRole**       | 예         | 탐색이 끝나는 연결의 End입니다. 값을 **ToRole** 특성 중 하나의 값과 동일 해야 합니다 **역할** (AssociationEnd 요소에 정의 됨) 연결 end 중 하나에 정의 된 특성입니다.       |
| **FromRole**     | 예         | 탐색이 시작되는 연결의 End입니다. 값을 **FromRole** 특성 중 하나의 값과 동일 해야 합니다 **역할** (AssociationEnd 요소에 정의 됨) 연결 end 중 하나에 정의 된 특성입니다. |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **NavigationProperty** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 엔터티 형식 정의 (**Book**) 두 개의 탐색 속성 (**PublishedBy** 하 고 **WrittenBy**):

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="ondelete-element-csdl"></a>OnDelete 요소(CSDL)

합니다 **OnDelete** 요소 (CSDL) 개념 스키마 정의 언어에 연결을 사용 하 여 연결 된 동작을 정의 합니다. 경우는 **동작** 특성이로 설정 된 **Cascade** 관련 연결의 한쪽 끝에서 첫 번째 끝 엔터티 형식이 삭제 되 면 연결의 반대쪽 끝에서 엔터티 형식을 삭제 됩니다. 두 엔터티 형식 간의 연결이 기본 키 대 기본 키 관계를 경우 연결의 한쪽 끝에 보안 주체 개체에 관계 없이 삭제 될 때 로드 된 종속 개체를 삭제 되는 **OnDelete** 사양입니다.  

> [!NOTE]
> 합니다 **OnDelete** 요소는 응용 프로그램의 런타임 동작에만 영향을 줍니다; 데이터 원본에 대 한 동작에 영향을 주지 않습니다. 데이터 소스에 정의된 동작은 애플리케이션에 정의된 동작과 같아야 합니다.

 

**OnDelete** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **OnDelete** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **작업**     | 예         | **Cascade** 나 **None**합니다. 하는 경우 **Cascade**, 주 엔터티 형식이 삭제 되 면 종속 엔터티 형식도 삭제 됩니다. 하는 경우 **None**, 주 엔터티 형식이 삭제 되 면 종속 엔터티 형식도 삭제 되지 것입니다. |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **연결** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

에서는 다음 예제는 **연결** 정의 하는 요소는 **CustomerOrders** 연결 합니다. **OnDelete** 요소에서 나타내는 모든 **주문** 관련 된 특정 **고객** ObjectContext에 로드 되어 및 삭제 될 때 합니다  **고객** 삭제 됩니다.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a>Parameter 요소(CSDL)

합니다 **매개 변수** 요소 (CSDL) 개념 스키마 정의 언어에서 FunctionImport 요소 또는 Function 요소의 자식일 수 있습니다.

### <a name="functionimport-element-application"></a>FunctionImport 요소 적용

A **매개 변수** 요소 (자식으로는 **FunctionImport** 요소) csdl로 선언 된 함수 가져오기에 대 한 입력 및 출력 매개 변수를 정의 하는 데 사용 됩니다.

합니다 **매개 변수** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소 허용)
-   Annotation 요소 (0 개 이상의 요소 허용)

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **매개 변수** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**       | 예         | 매개 변수의 이름입니다.                                                                                                                                                                                                      |
| **Type**       | 예         | 매개 변수 형식입니다. 값 이어야 합니다는 **EDMSimpleType** 또는 모델 범위 내에 있는 복합 형식입니다.                                                                                                             |
| **모드**       | 아니요          | **In**, **초과**, 또는 **InOut** 매개 변수는 입력, 출력 또는 입출력 매개 변수 인지에 따라 합니다.                                                                                                                |
| **MaxLength**  | 아니요          | 매개 변수의 최대 허용 길이입니다.                                                                                                                                                                                    |
| **전체 자릿수**  | 아니요          | 매개 변수의 전체 자릿수입니다.                                                                                                                                                                                                 |
| **배율**      | 아니요          | 매개 변수의 소수 자릿수입니다.                                                                                                                                                                                                     |
| **SRID**       | 아니요          | 시스템 공간 참조 식별자입니다. 공간 형식 매개 변수에 유효 합니다. 자세한 내용은 [SRID](http://en.wikipedia.org/wiki/SRID) 하 고 [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)합니다. |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **매개 변수** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

#### <a name="example"></a>예제

에서는 다음 예제는 **FunctionImport** 하나를 사용 하 여 요소 **매개 변수** 자식 요소입니다. 함수는 하나의 입력 매개 변수를 받아들이고 엔터티 형식의 컬렉션을 반환합니다.

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a>Function 요소 적용

A **매개 변수** 요소 (자식으로는 **함수** 요소) 개념적 모델에서 정의 되거나 선언 된 함수에 대 한 매개 변수를 정의 합니다.

합니다 **매개 변수** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소)
-   CollectionType (0 개 이상의 요소)
-   ReferenceType (0 개 이상의 요소)
-   RowType (0 개 이상의 요소)

> [!NOTE]
> 중 하나에만 합니다 **CollectionType**, **ReferenceType**, 또는 **RowType** 요소는 자식 요소의 수를 **속성** 요소입니다.

 

-   Annotation 요소 (0 개 이상의 요소 허용)

> [!NOTE]
> Annotation 요소는 다른 모든 자식 요소 뒤에 와야 합니다. Annotation 요소는 CSDL v2 이상에 허용 됩니다.

 

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **매개 변수** 요소입니다.

| 특성 이름   | 필수 여부 | 값                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**         | 예         | 매개 변수의 이름입니다.                                                                                                                                                                                                      |
| **Type**         | 아니요          | 매개 변수 형식입니다. 매개 변수는 다음 형식 또는 이러한 형식의 컬렉션일 수 있습니다. <br/> **EdmSimpleType** <br/> 엔터티 형식(entity type) <br/> 복합 형식 <br/> 행 형식 <br/> 참조 형식(reference type)                             |
| **Null 허용**     | 아니요          | **True** (기본값) 또는 **False** 속성을 사용할 수 있는지 여부에 따라 한 **null** 값입니다.                                                                                                                          |
| **DefaultValue** | 아니요          | 속성의 기본값입니다.                                                                                                                                                                                              |
| **MaxLength**    | 아니요          | 속성 값의 최대 길이입니다.                                                                                                                                                                                       |
| **FixedLength**  | 아니요          | **True 이면** 나 **False** 고정된 길이 문자열로 속성 값이 저장될지 여부에 따라 합니다.                                                                                                                          |
| **전체 자릿수**    | 아니요          | 속성 값의 전체 자릿수입니다.                                                                                                                                                                                            |
| **배율**        | 아니요          | 속성 값의 소수 자릿수입니다.                                                                                                                                                                                                |
| **SRID**         | 아니요          | 시스템 공간 참조 식별자입니다. 공간 형식의 속성에 대해서만 유효 합니다. 자세한 내용은 [SRID](http://en.wikipedia.org/wiki/SRID) 하 고 [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)합니다. |
| **유니코드**      | 아니요          | **True 이면** 나 **False** 유니코드 문자열로 속성 값이 저장될지 여부에 따라 합니다.                                                                                                                               |
| **데이터 정렬**    | 아니요          | 데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.                                                                                                                                                   |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **매개 변수** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

#### <a name="example"></a>예제

에서는 다음 예제는 **함수** 하나를 사용 하는 요소 **매개 변수** 자식 요소를 함수 매개 변수를 정의 합니다.

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a>Principal 요소(CSDL)

합니다 **주** 개념 스키마 정의 언어 (CSDL)에서 참조 제약 조건의 주 끝을 정의 하는 ReferentialConstraint 요소에 자식 요소입니다. A **ReferentialConstraint** 요소는 관계형 데이터베이스의 참조 무결성 제약 조건과 유사한 기능을 정의 합니다. 데이터베이스 테이블의 열에서 다른 테이블의 기본 키를 참조하는 방식과 동일하게 엔터티 형식의 속성은 다른 엔터티 형식의 엔터티 키를 참조할 수 있습니다. 참조 되는 엔터티 형식 이라고 합니다 *주 끝* 제약 조건입니다. 주 끝을 참조 하는 엔터티 형식 이라고 합니다 *종속 끝* 제약 조건입니다. **PropertyRef** 요소는 종속 끝에서 참조 하는 키를 지정 하는 데 사용 됩니다.

합니다 **주** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   PropertyRef (하나 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 테이블에 적용할 수 있는 특성을 설명 합니다.는 **주** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **역할**       | 예         | 연결의 주 끝에 있는 엔터티 형식의 이름입니다. |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **주** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

에서는 다음 예제는 **ReferentialConstraint** 정의의 일부인 요소를 **PublishedBy** 연결 합니다. **Id** 의 속성을 **게시자** 엔터티 형식 참조 제약 조건의 주 끝 이루어집니다.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="property-element-csdl"></a>Property 요소(CSDL)

합니다 **속성** 요소 (CSDL) 개념 스키마 정의 언어의 EntityType 요소, ComplexType 요소 또는 RowType 요소의 자식일 수 있습니다.

### <a name="entitytype-and-complextype-element-applications"></a>EntityType 및 ComplexType 요소 적용

**속성** 요소 (자식인 **EntityType** 또는 **ComplexType** 요소) 모양 및 엔터티 형식 인스턴스 또는 복합 형식 인스턴스에 포함 될 데이터의 특징을 정의 합니다. . 개념적 모델의 속성은 클래스에 정의된 속성과 유사합니다. 클래스의 속성이 클래스의 모양을 정의하고 개체에 대한 정보를 전달하는 것과 동일한 방식으로, 개념적 모델의 속성은 엔터티 형식의 모양을 정의하고 엔터티 형식 인스턴스에 대한 정보를 전달합니다.

합니다 **속성** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   Documentation 요소 (0 개 이상의 요소 허용)
-   Annotation 요소 (0 개 이상의 요소 허용)

다음 패싯을 적용할 수는 **속성** 요소: **Nullable**를 **DefaultValue**를 **MaxLength**,  **FixedLength**, **정밀도**, **규모**를 **유니코드**를 **데이터 정렬**,  **ConcurrencyMode**합니다. 패싯은 속성 값이 데이터 저장소에 저장된 방식에 대한 정보를 제공하는 XML 특성입니다.

> [!NOTE]
> 패싯에 형식의 속성에만 적용할 수 있습니다 **EDMSimpleType**합니다.

 

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **속성** 요소입니다.

| 특성 이름                                                         | 필수 여부 | 값                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**                                                               | 예         | 속성의 이름입니다.                                                                                                                                                                                                       |
| **Type**                                                               | 예         | 속성 값의 형식입니다. 속성 값 형식 이어야 합니다는 **EDMSimpleType** 또는 모델 범위 내에 있는 복합 형식 (정규화 된 이름으로 표시 됨).                                                 |
| **Null 허용**                                                           | 아니요          | **True 이면** (기본값) 또는 <strong>False</strong> 속성에 null 값을 사용할 수 있는지 여부에 따라 합니다. <br/> [!NOTE]                                                                                                   |
| > CSDL v1의 복합 형식 속성이 있어야 `Nullable="False"`합니다. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                       | 아니요          | 속성의 기본값입니다.                                                                                                                                                                                              |
| **MaxLength**                                                          | 아니요          | 속성 값의 최대 길이입니다.                                                                                                                                                                                       |
| **FixedLength**                                                        | 아니요          | **True 이면** 나 **False** 고정된 길이 문자열로 속성 값이 저장될지 여부에 따라 합니다.                                                                                                                          |
| **전체 자릿수**                                                          | 아니요          | 속성 값의 전체 자릿수입니다.                                                                                                                                                                                            |
| **배율**                                                              | 아니요          | 속성 값의 소수 자릿수입니다.                                                                                                                                                                                                |
| **SRID**                                                               | 아니요          | 시스템 공간 참조 식별자입니다. 공간 형식의 속성에 대해서만 유효 합니다. 자세한 내용은 [SRID](http://en.wikipedia.org/wiki/SRID) 하 고 [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)합니다. |
| **유니코드**                                                            | 아니요          | **True 이면** 나 **False** 유니코드 문자열로 속성 값이 저장될지 여부에 따라 합니다.                                                                                                                               |
| **데이터 정렬**                                                          | 아니요          | 데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.                                                                                                                                                   |
| **ConcurrencyMode**                                                    | 아니요          | **None** (기본값) 또는 **Fixed**합니다. 값 설정 된 경우 **Fixed**, 속성 값이 낙관적 동시성 검사에 사용 됩니다.                                                                                  |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **속성** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

#### <a name="example"></a>예제

다음 예제는 **EntityType** 3 개 요소 **속성** 요소:

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

다음 예제는 **ComplexType** 5를 사용 하 여 요소 **속성** 요소:

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a>RowType 요소 적용

**속성** 요소 (자식으로는 **RowType** 요소) 모양 및 전달 되거나 모델 정의 함수에서 반환 될 수 있는 데이터의 특징을 정의 합니다.  

합니다 **속성** 요소는 다음 자식 요소 중 하나만 포함할 수 있습니다.

-   CollectionType
-   ReferenceType
-   RowType

합니다 **속성** 요소 번호 자식 주석 요소를 포함할 수 있습니다.

> [!NOTE]
> Annotation 요소는 CSDL v2 이상에 허용 됩니다.

 

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **속성** 요소입니다.

| 특성 이름                                                     | 필수 여부 | 값                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**                                                           | 예         | 속성의 이름입니다.                                                                                                                                                                                                       |
| **Type**                                                           | 예         | 속성 값의 형식입니다.                                                                                                                                                                                                 |
| **Null 허용**                                                       | 아니요          | **True 이면** (기본값) 또는 **False** 속성에 null 값을 사용할 수 있는지 여부에 따라 합니다. <br/> [!NOTE]                                                                                                                |
| > 복합 형식 속성 CSDL v1의 있어야 `Nullable="False"`합니다. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                   | 아니요          | 속성의 기본값입니다.                                                                                                                                                                                              |
| **MaxLength**                                                      | 아니요          | 속성 값의 최대 길이입니다.                                                                                                                                                                                       |
| **FixedLength**                                                    | 아니요          | **True 이면** 나 **False** 고정된 길이 문자열로 속성 값이 저장될지 여부에 따라 합니다.                                                                                                                          |
| **전체 자릿수**                                                      | 아니요          | 속성 값의 전체 자릿수입니다.                                                                                                                                                                                            |
| **배율**                                                          | 아니요          | 속성 값의 소수 자릿수입니다.                                                                                                                                                                                                |
| **SRID**                                                           | 아니요          | 시스템 공간 참조 식별자입니다. 공간 형식의 속성에 대해서만 유효 합니다. 자세한 내용은 [SRID](http://en.wikipedia.org/wiki/SRID) 하 고 [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)합니다. |
| **유니코드**                                                        | 아니요          | **True 이면** 나 **False** 유니코드 문자열로 속성 값이 저장될지 여부에 따라 합니다.                                                                                                                               |
| **데이터 정렬**                                                      | 아니요          | 데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.                                                                                                                                                   |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **속성** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

#### <a name="example"></a>예제

다음 예와 **속성** 모델 정의 함수 반환 형식의 모양을 정의 하는 데 사용 되는 요소입니다.

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

 

## <a name="propertyref-element-csdl"></a>PropertyRef 요소(CSDL)

합니다 **PropertyRef** 요소 (CSDL) 개념 스키마 정의 언어에서를 나타내는 속성이 다음 역할 중 하나가 수행 되는 엔터티 형식의 속성을 참조 합니다.

-   엔터티 키(ID를 확인하는 엔터티 형식의 속성 또는 속성 집합)의 일부분. 하나 이상의 **PropertyRef** 엔터티 키를 정의 하는 요소를 사용할 수 있습니다.
-   참조 제약 조건의 종속 또는 주 끝.

합니다 **PropertyRef** 요소의 자식 요소로 annotation 요소 (0 개 이상)를 하나만 사용할 수 있습니다.

> [!NOTE]
> Annotation 요소는 CSDL v2 이상에 허용 됩니다.

 

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **PropertyRef** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                |
|:---------------|:------------|:-------------------------------------|
| **이름**       | 예         | 참조된 속성의 이름입니다. |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **PropertyRef** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

아래 예제에서는 엔터티 형식 정의 (**책**). 엔터티 키가 참조 하 여 정의 된 **ISBN** 엔터티 형식의 속성입니다.

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

다음 예제에서는 두 개의 **PropertyRef** 요소는 두 개의 나타내는 데 속성 (**Id** 하 고 **PublisherId**)는 참조의 주 끝 및 종속 끝은 제약 조건입니다.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="referencetype-element-csdl"></a>ReferenceType 요소(CSDL)

합니다 **ReferenceType** 요소 (CSDL) 개념 스키마 정의 언어에서 엔터티 형식에 대 한 참조를 지정 합니다. 합니다 **ReferenceType** 요소에는 다음 요소의 자식일 수 있습니다.

-   ReturnType (Function)
-   매개 변수
-   CollectionType

합니다 **ReferenceType** 요소는 함수에 대 한 매개 변수 또는 반환 형식을 정의 하는 경우 사용 합니다.

A **ReferenceType** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **ReferenceType** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                         |
|:---------------|:------------|:----------------------------------------------|
| **Type**       | 예         | 참조되는 엔터티 형식의 이름입니다. |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **ReferenceType** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제와 **ReferenceType** 의 자식으로 사용 되는 요소는 **매개 변수** 요소에 대 한 참조를 받아들이는 모델 정의 함수에는 **Person** 엔터티 형식:

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
   <Parameter Name="instructor">
     <ReferenceType Type="SchoolModel.Person" />
   </Parameter>
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

다음 예제와 합니다 **ReferenceType** 의 자식으로 사용 되는 요소를 **ReturnType** (함수) 요소에 대 한 참조를 반환 하는 모델 정의 함수에는 **Person**엔터티 형식:

``` xml
 <Function Name="GetPersonReference">
     <Parameter Name="p" Type="SchoolModel.Person" />
     <ReturnType>
         <ReferenceType Type="SchoolModel.Person" />
     </ReturnType>
     <DefiningExpression>
           REF(p)
     </DefiningExpression>
 </Function>
```
 

 

## <a name="referentialconstraint-element-csdl"></a>ReferentialConstraint 요소(CSDL)

A **ReferentialConstraint** 요소 (CSDL) 개념 스키마 정의 언어에서는 관계형 데이터베이스의 참조 무결성 제약 조건과 유사한 기능을 정의 합니다. 데이터베이스 테이블의 열에서 다른 테이블의 기본 키를 참조하는 방식과 동일하게 엔터티 형식의 속성은 다른 엔터티 형식의 엔터티 키를 참조할 수 있습니다. 참조 되는 엔터티 형식 이라고 합니다 *주 끝* 제약 조건입니다. 주 끝을 참조 하는 엔터티 형식 이라고 합니다 *종속 끝* 제약 조건입니다.

하나의 엔터티 형식에서 노출 되는 외래 키에서 다른 엔터티 형식의 속성을 참조 하는 경우는 **ReferentialConstraint** 요소는 두 엔터티 형식 간의 연결을 정의 합니다. 때문에 합니다 **ReferentialConstraint** 두 엔터티 형식에 대 한 내용은 해당 AssociationSetMapping 요소는 MSL (매핑 사양 언어)에서 반드시 관련 된 요소를 제공 합니다. 노출 된 외래 키를 갖지 않는 두 엔터티 형식 간의 연결을 해당 있어야 **AssociationSetMapping** 데이터 원본에 연결 정보를 매핑하려면 요소입니다.

외래 키가 엔터티 형식에서 노출 되지 않는 경우는 **ReferentialConstraint** 요소는 엔터티 형식과 다른 엔터티 형식 간의 기본 키 대 기본 키 제약 정의할 수 있습니다.

A **ReferentialConstraint** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소)
-   보안 주체 (정확히 한 개의 요소)
-   종속 (정확히 한 개의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

합니다 **ReferentialConstraint** 요소는 원하는 수의 주석 특성 (사용자 지정 XML 특성)를 포함할 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

에서는 다음 예제는 **ReferentialConstraint** 요소 정의의 일부로 사용 되는 **PublishedBy** 연결 합니다.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="returntype-function-element-csdl"></a>(함수) ReturnType 요소 (CSDL)

합니다 **ReturnType** (Function) 요소 (CSDL) 개념 스키마 정의 언어에서 Function 요소에 정의 된 함수에 대 한 반환 형식을 지정 합니다. 함수 반환 형식으로 지정할 수도 있습니다는 **ReturnType** 특성입니다.

반환 형식이 될 수 있습니다 **EdmSimpleType**, 엔터티 형식, 복합 형식, 행 형식, 참조 형식 또는 이러한 형식 중 하나의 컬렉션입니다.

함수의 반환 형식 중 하나를 사용 하 여 지정할 수 있습니다 합니다 **형식** 특성을 **ReturnType** (Function) 요소 또는 다음 자식 요소 중 하나를 사용 하 여:

-   CollectionType
-   ReferenceType
-   RowType

> [!NOTE]
> 함수 반환 형식을 모두 지정 하는 경우에 모델을 검사 하지 것입니다는 **형식** 특성을 **ReturnType** (함수) 요소와 자식 요소 중 하나입니다.

 

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **ReturnType** (Function) 요소.

| 특성 이름 | 필수 여부 | 값                              |
|:---------------|:------------|:-----------------------------------|
| **ReturnType** | 아니요          | 함수에서 반환하는 형식입니다. |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **ReturnType** (Function) 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 한 **함수** 책이 인쇄 된 연수를 반환 하는 함수를 정의 하는 요소입니다. 반환 형식으로 지정 된 참고 합니다 **형식** 특성을 **ReturnType** (Function) 요소.

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a>(FunctionImport) ReturnType 요소 (CSDL)

합니다 **ReturnType** (FunctionImport) 요소 (CSDL) 개념 스키마 정의 언어에서 FunctionImport 요소에 정의 된 함수에 대 한 반환 형식을 지정 합니다. 함수 반환 형식으로 지정할 수도 있습니다는 **ReturnType** 특성입니다.

형식 복합 형식 엔터티 형식의 컬렉션 일 수를 반환 하거나 **EdmSimpleType**,

함수의 반환 형식이 지정 된 합니다 **형식** 특성을 **ReturnType** (FunctionImport) 요소입니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **ReturnType** (FunctionImport) 요소입니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**       | 아니요          | 함수에서 반환하는 형식입니다. 값은 ComplexType, EntityType 또는 EDMSimpleType 컬렉션 이어야 합니다.                                                                                      |
| **EntitySet**  | 아니요          | 엔터티 컬렉션을 반환 하는 경우 형식의 경우 값을 **EntitySet** 엔터티로 변경 해야은 컬렉션이 소속 된 합니다. 그렇지 않으면 합니다 **EntitySet** 특성을 사용할 수 없습니다. |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **ReturnType** (FunctionImport) 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 한 **FunctionImport** 서적과 게시자를 반환 하는 합니다. 함수는 두 개의 결과 집합 및 두 개의 반환 **ReturnType** (FunctionImport) 요소가 지정 되어 있습니다.

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a>RowType 요소(CSDL)

A **RowType** 요소 (CSDL) 개념 스키마 정의 언어에서 매개 변수 또는 반환 형식은 개념적 모델에 정의 된 함수에 대 한 명명 되지 않은 구조를 정의 합니다.

A **RowType** 다음 요소의 자식 요소일 수 있습니다.

-   CollectionType
-   매개 변수
-   ReturnType (Function)

A **RowType** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   속성 (하나 이상)
-   Annotation 요소 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **RowType** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제에서는 사용 하는 모델 정의 함수를 **CollectionType** 는 반환 행 컬렉션을 지정 하는 요소 (에 지정 된 대로 합니다 **RowType** 요소).

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```

## <a name="schema-element-csdl"></a>스키마 요소(CSDL)

합니다 **스키마** 요소는 개념적 모델 정의의 루트 요소입니다. 이 요소에는 개념적 모델을 구성하는 개체, 함수 및 컨테이너에 대한 정의가 포함되어 있습니다.

합니다 **스키마** 요소 0 개 이상의 자식 요소를 포함할 수 있습니다.

-   Using
-   EntityContainer
-   EntityType
-   EnumType
-   형식 연결
-   ComplexType
-   기능

A **스키마** 요소 0 개 이상의 Annotation 요소를 포함할 수 있습니다.

> [!NOTE]
> 합니다 **함수** CSDL v2 이상 요소 및 annotation 요소 에서만 허용 됩니다.

 

합니다 **스키마** 요소를 사용 합니다 **Namespace** 개념적 모델의 엔터티 형식, 복합 형식 및 연결 개체에 대 한 네임 스페이스를 정의 하는 특성입니다. 네임스페이스 내에서 두 개체의 이름이 서로 같을 수 없습니다. 네임 스페이스에는 여러 걸쳐 있을 수 있습니다 **스키마** 요소 및 여러.csdl 파일입니다.

개념적 모델 네임 스페이스의 XML 네임 스페이스와에서 다릅니다. 합니다 **스키마** 요소입니다. 개념적 모델 네임 스페이스 (정의 된 대로 합니다 **Namespace** 특성)은 엔터티 형식, 복합 형식 및 연결 형식에 대 한 논리적 컨테이너입니다. XML 네임 스페이스 (나타난 합니다 **xmlns** 특성)의 **스키마** 요소는 자식 요소 및 특성에 대 한 기본 네임 스페이스를 **스키마** 요소입니다. 폼의 XML 네임 스페이스 http://schemas.microsoft.com/ado/YYYY/MM/edm (여기서 YYYY, MM은 연도 및 월 각각)는 CSDL 용으로 예약 되어 있습니다. 사용자 지정 요소 및 특성은 이러한 형식의 네임스페이스에 있을 수 없습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 특성을 설명에 적용할 수는 **스키마** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **네임스페이스**  | 예         | 개념적 모델의 네임스페이스입니다. 값을 **Namespace** 특성 형식의 정규화 된 이름을 만드는 데 사용 됩니다. 예를 들어 경우는 **EntityType** 라는 *고객* Simple.Example.Model 네임 스페이스의 정규화 된 이름을를 **EntityType** 는 SimpleExampleModel.Customer입니다. <br/> 다음 문자열 값으로 사용할 수 없습니다는 **Namespace** 특성: **System**를 **일시적인**, 또는 **Edm**합니다. 값을 **Namespace** 특성에 대 한 값과 같을 수 없습니다는 **Namespace** SSDL 스키마 요소의 특성입니다. |
| **Alias**      | 아니요          | 네임스페이스 이름 대신 사용되는 식별자입니다. 예를 들어 경우는 **EntityType** 라는 *고객* Simple.Example.Model 네임 스페이스 및 값에는 **별칭** 특성이 *모델*, Model.Customer를 사용 하 여 정규화 된 이름으로는 **EntityType입니다.**                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **스키마** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제와 **스키마** 포함 하는 요소는 **EntityContainer** 요소, 두 **EntityType** 요소와 **연결** 요소입니다.

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
       Namespace="ExampleModel" Alias="Self">
         <EntityContainer Name="ExampleModelContainer">
           <EntitySet Name="Customers"
                      EntityType="ExampleModel.Customer" />
           <EntitySet Name="Orders" EntityType="ExampleModel.Order" />
           <AssociationSet
                       Name="CustomerOrder"
                       Association="ExampleModel.CustomerOrders">
             <End Role="Customer" EntitySet="Customers" />
             <End Role="Order" EntitySet="Orders" />
           </AssociationSet>
         </EntityContainer>
         <EntityType Name="Customer">
           <Key>
             <PropertyRef Name="CustomerId" />
           </Key>
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
           <Property Type="String" Name="Name" Nullable="false" />
           <NavigationProperty
                    Name="Orders"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Customer" ToRole="Order" />
         </EntityType>
         <EntityType Name="Order">
           <Key>
             <PropertyRef Name="OrderId" />
           </Key>
           <Property Type="Int32" Name="OrderId" Nullable="false" />
           <Property Type="Int32" Name="ProductId" Nullable="false" />
           <Property Type="Int32" Name="Quantity" Nullable="false" />
           <NavigationProperty
                    Name="Customer"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Order" ToRole="Customer" />
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
         </EntityType>
         <Association Name="CustomerOrders">
           <End Type="ExampleModel.Customer"
                Role="Customer" Multiplicity="1" />
           <End Type="ExampleModel.Order"
                Role="Order" Multiplicity="*" />
           <ReferentialConstraint>
             <Principal Role="Customer">
               <PropertyRef Name="CustomerId" />
             </Principal>
             <Dependent Role="Order">
               <PropertyRef Name="CustomerId" />
             </Dependent>
           </ReferentialConstraint>
         </Association>
       </Schema>
```
 

 

## <a name="typeref-element-csdl"></a>TypeRef 요소(CSDL)

합니다 **TypeRef** (CSDL) 개념 스키마 정의 언어 요소는 기존 명명 된 형식에 대 한 참조를 제공 합니다. 합니다 **TypeRef** 함수 매개 변수 또는 반환 형식으로 컬렉션에 있는지를 지정 하는 데 사용 되는 CollectionType 요소의 자식 요소일 수 있습니다.

A **TypeRef** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **TypeRef** 요소입니다. 합니다 **DefaultValue**, **MaxLength**를 **FixedLength**, **정밀도**를 **확장**,  **유니코드**, 및 **데이터 정렬을** 특성에만 적용 됩니다 **EDMSimpleTypes**합니다.

| 특성 이름                                                     | 필수 여부 | 값                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**                                                           | 아니요          | 참조되는 형식의 이름입니다.                                                                                                                                                                                          |
| **Null 허용**                                                       | 아니요          | **True 이면** (기본값) 또는 **False** 속성에 null 값을 사용할 수 있는지 여부에 따라 합니다. <br/> [!NOTE]                                                                                                                |
| > 복합 형식 속성 CSDL v1의 있어야 `Nullable="False"`합니다. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                   | 아니요          | 속성의 기본값입니다.                                                                                                                                                                                              |
| **MaxLength**                                                      | 아니요          | 속성 값의 최대 길이입니다.                                                                                                                                                                                       |
| **FixedLength**                                                    | 아니요          | **True 이면** 나 **False** 고정된 길이 문자열로 속성 값이 저장될지 여부에 따라 합니다.                                                                                                                          |
| **전체 자릿수**                                                      | 아니요          | 속성 값의 전체 자릿수입니다.                                                                                                                                                                                            |
| **배율**                                                          | 아니요          | 속성 값의 소수 자릿수입니다.                                                                                                                                                                                                |
| **SRID**                                                           | 아니요          | 시스템 공간 참조 식별자입니다. 공간 형식의 속성에 대해서만 유효 합니다. 자세한 내용은 [SRID](http://en.wikipedia.org/wiki/SRID) 하 고 [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)합니다. |
| **유니코드**                                                        | 아니요          | **True 이면** 나 **False** 유니코드 문자열로 속성 값이 저장될지 여부에 따라 합니다.                                                                                                                               |
| **데이터 정렬**                                                      | 아니요          | 데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.                                                                                                                                                   |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **CollectionType** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 사용 하는 모델 정의 함수를 **TypeRef** 요소 (자식으로는 **CollectionType** 요소) 함수 컬렉션을 허용 하는지 지정  **부서** 엔터티 형식입니다.

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="using-element-csdl"></a>요소 사용(CSDL)

합니다 **사용 하 여** 요소 (CSDL) 개념 스키마 정의 언어에 다른 네임 스페이스에 존재 하는 개념적 모델의 내용을 가져옵니다. 값을 설정 하 여 합니다 **Namespace** 특성, 엔터티 형식, 복합 형식 및 다른 개념적 모델에 정의 된 형식 연결을 참조할 수 있습니다. 둘 이상의 **사용 하 여** 의 자식 요소일 수 있습니다는 **스키마** 요소입니다.

> [!NOTE]
> 합니다 **사용 하 여** CSDL의 요소와 똑같이 작동 하지 않습니다는 **사용 하 여** 프로그래밍 언어로 문을 합니다. 사용 하 여 네임 스페이스를 가져오는 **를 사용 하 여** 문은 원래 네임 스페이스의 개체를 주지 않습니다 프로그래밍 언어에서 합니다. CSDL의 가져온 네임스페이스에는 원래 네임스페이스의 엔터티 형식에서 파생된 엔터티 형식이 포함될 수 있습니다. 이는 원래 네임스페이스에 선언된 엔터티 집합에 영향을 줄 수 있습니다.

 

합니다 **사용 하 여** 요소는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소 허용)
-   Annotation 요소 (0 개 이상의 요소 허용)

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 특성을 설명에 적용할 수는 **사용 하 여** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **네임스페이스**  | 예         | 가져온 네임스페이스의 이름입니다.                                                                                                                                                |
| **Alias**      | 예         | 네임스페이스 이름 대신 사용되는 식별자입니다. 이 특성은 필요하지만 개체 이름을 정규화하기 위해 네임스페이스 이름 대신 사용해야 할 필요는 없습니다. |

 

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **사용 하 여** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제는 **사용 하 여** 다른 곳에 정의 된 네임 스페이스를 가져오는 데 사용 되는 요소입니다. 네임 스페이스는 **스키마** 표시 된 요소는 `BooksModel`합니다. `Address` 속성에는 `Publisher` **EntityType** 에 정의 된 복합 형식인는 `ExtendedBooksModel` 네임 스페이스 (사용 하 여 가져온를 **사용 하 여** 요소).

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
           Namespace="BooksModel" Alias="Self">

     <Using Namespace="BooksModel.Extended" Alias="BMExt" />

 <EntityContainer Name="BooksContainer" >
       <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
     </EntityContainer>

 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BMExt.Address" Name="Address" Nullable="false" />
     </EntityType>

 </Schema>
```
 

 

## <a name="annotation-attributes-csdl"></a>주석 특성(CSDL)

CSDL(개념 스키마 정의 언어)의 주석 특성은 개념적 모델의 사용자 지정 XML 특성입니다. 주석 특성은 올바른 XML 구조를 가져야 함은 물론 다음 조건을 충족해야 합니다.

-   주석 특성은 CSDL용으로 예약된 XML 네임스페이스에 속하지 않아야 합니다.
-   두 개 이상의 주석 특성을 제공된 CSDL 요소에 적용할 수 있습니다.
-   두 주석 특성의 정규화된 이름은 서로 같을 수 없습니다.

주석 특성을 사용하여 개념적 모델의 요소에 대한 추가 메타데이터를 제공할 수 있습니다. Annotation 요소에 포함 된 메타 데이터 System.Data.Metadata.Edm 네임 스페이스의 클래스를 사용 하 여 런타임에 액세스할 수 있습니다.

### <a name="example"></a>예제

다음 예제는 **EntityType** 주석 특성을 사용 하 여 요소 (**CustomAttribute**). 이 예제에서는 엔터티 형식 요소에 적용된 Annotation 요소도 보여 줍니다.

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

다음 코드에서는 주석 특성의 메타데이터를 검색하여 콘솔에 씁니다.

``` xml
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomAttribute"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomAttribute"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

이 코드에서는 `School.csdl` 파일이 프로젝트의 출력 디렉터리에 있고 프로젝트에 다음의 `Imports` 및 `Using` 문을 추가했다고 가정합니다.

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a>Annotation 요소(CSDL)

CSDL(개념 스키마 정의 언어)의 Annotation 요소는 개념적 모델의 사용자 지정 XML 요소입니다. Annotation 요소는 올바른 XML 구조를 가져야 함은 물론 다음 조건을 충족해야 합니다.

-   Annotation 요소는 CSDL용으로 예약된 XML 네임스페이스에 속하지 않아야 합니다.
-   두 개 이상의 Annotation 요소가 제공된 CSDL 요소의 자식이 될 수 있습니다.
-   두 Annotation 요소의 정규화된 이름은 서로 같을 수 없습니다.
-   Annotation 요소는 제공된 CSDL 요소의 다른 모든 자식 요소 뒤에 나타나야 합니다.

Annotation 요소를 사용하여 개념적 모델의 요소에 대한 추가 메타데이터를 제공할 수 있습니다. .NET Framework 버전 4부터 annotation 요소에 포함 된 메타 데이터에 액세스할 수 있습니다 런타임 System.Data.Metadata.Edm 네임 스페이스의 클래스를 사용 하 여 합니다.

### <a name="example"></a>예제

다음 예제는 **EntityType** 주석 요소를 사용 하 여 요소 (**CustomElement**). 다음 예제에서는 엔터티 형식 요소에 적용된 주석 특성도 보여 줍니다.

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

다음 코드에서는 Annotation 요소의 메타데이터를 검색하여 콘솔에 작성합니다.

``` csharp
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomElement"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomElement"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

위의 코드에서는 School.csdl 파일이 프로젝트의 출력 디렉터리에 있고 다음 `Imports` 및 `Using` 문을 프로젝트에 추가했다고 가정합니다.

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a>개념적 모델 형식(CSDL)

개념 스키마 정의 언어 (CSDL) 라는 추상 기본 데이터 형식의 집합을 지 원하는 **EDMSimpleTypes**, 개념적 모델의 속성을 정의 하는 합니다. **EDMSimpleTypes** 는 저장소 또는 호스팅 환경에서 지원 되는 기본 데이터 형식에 대 한 프록시입니다.

다음 표에서는 CSDL에서 지원되는 기본 데이터 형식을 보여 줍니다. 또한 표에서 각에 적용할 수 있는 패싯 **EDMSimpleType**합니다.

| EDMSimpleType                    | 설명                                                | 적용 가능한 패싯                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| **Edm.Binary**                   | 이진 데이터를 포함합니다.                                      | MaxLength, FixedLength, Nullable, Default                                |
| **Edm.Boolean**                  | 값을 포함 **true** 하거나 **false**합니다.                  | Nullable, Default                                                        |
| **Edm.Byte**                     | 부호 없는 8비트 정수 값을 포함합니다.                  | Precision, Nullable, Default                                             |
| **Edm.DateTime**                 | 날짜 및 시간을 나타냅니다.                                | Precision, Nullable, Default                                             |
| **Edm.DateTimeOffset**           | 날짜 및 시간을 GMT에서의 오프셋(분)으로 포함합니다. | Precision, Nullable, Default                                             |
| **Edm.Decimal**                  | 고정 전체 자릿수와 소수 자릿수가 있는 숫자 값을 포함합니다.   | Precision, Nullable, Default                                             |
| **Edm.Double**                   | 부동 소수점 전체 자릿수가 15 자리인 숫자를 포함합니다.   | Precision, Nullable, Default                                             |
| **Edm.Float**                    | 전체 자릿수가 7자리인 부동 소수점 숫자를 포함합니다.   | Precision, Nullable, Default                                             |
| **Edm.Guid**                     | 16바이트 고유 식별자를 포함합니다.                      | Precision, Nullable, Default                                             |
| **Edm.Int16**                    | 부호 있는 16비트 정수 값을 포함합니다.                    | Precision, Nullable, Default                                             |
| **Edm.Int32**                    | 부호 있는 32비트 정수 값을 포함합니다.                    | Precision, Nullable, Default                                             |
| **Edm.Int64**                    | 부호 있는 64비트 정수 값을 포함합니다.                    | Precision, Nullable, Default                                             |
| **Edm.SByte**                    | 부호 있는 8비트 정수 값을 포함합니다.                     | Precision, Nullable, Default                                             |
| **Edm.String**                   | 문자 데이터를 포함합니다.                                   | Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default |
| **Edm.Time**                     | 시간을 포함합니다.                                    | Precision, Nullable, Default                                             |
| **Edm.Geography**                |                                                            | Null을 허용 기본적 SRID                                                  |
| **Edm.GeographyPoint**           |                                                            | Null을 허용 기본적 SRID                                                  |
| **Edm.GeographyLineString**      |                                                            | Null을 허용 기본적 SRID                                                  |
| **Edm.GeographyPolygon**         |                                                            | Null을 허용 기본적 SRID                                                  |
| **Edm.GeographyMultiPoint**      |                                                            | Null을 허용 기본적 SRID                                                  |
| **Edm.GeographyMultiLineString** |                                                            | Null을 허용 기본적 SRID                                                  |
| **Edm.GeographyMultiPolygon**    |                                                            | Null을 허용 기본적 SRID                                                  |
| **Edm.GeographyCollection**      |                                                            | Null을 허용 기본적 SRID                                                  |
| **Edm.Geometry**                 |                                                            | Null을 허용 기본적 SRID                                                  |
| **Edm.GeometryPoint**            |                                                            | Null을 허용 기본적 SRID                                                  |
| **Edm.GeometryLineString**       |                                                            | Null을 허용 기본적 SRID                                                  |
| **Edm.GeometryPolygon**          |                                                            | Null을 허용 기본적 SRID                                                  |
| **Edm.GeometryMultiPoint**       |                                                            | Null을 허용 기본적 SRID                                                  |
| **Edm.GeometryMultiLineString**  |                                                            | Null을 허용 기본적 SRID                                                  |
| **Edm.GeometryMultiPolygon**     |                                                            | Null을 허용 기본적 SRID                                                  |
| **Edm.GeometryCollection**       |                                                            | Null을 허용 기본적 SRID                                                  |

## <a name="facets-csdl"></a>패싯(CSDL)

CSDL(개념 스키마 정의 언어)의 패싯은 엔터티 형식 및 복합 형식 속성에 대한 제약 조건을 나타냅니다. 패싯은 다음 CSDL 요소에서 XML 특성으로 나타납니다.

-   속성
-   TypeRef
-   매개 변수

다음 표에서는 CSDL에서 지원되는 패싯에 대해 설명합니다. 모든 패싯은 선택적 요소입니다. 아래에 나열 된 일부 패싯은 개념적 모델에서 데이터베이스를 생성할 때 Entity Framework에서 사용 됩니다.

> [!NOTE]
> 개념적 모델의 데이터 형식에 대 한 자세한 내용은 개념적 모델 형식 (CSDL)을 참조 하세요.

| 패싯               | 설명                                                                                                                                                                                                                                                   | 적용 대상                                                                                                                                                                                                                                                                                                                                                                           | 데이터베이스 생성에 사용됩니다. | 런타임에서 사용됩니다. |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| **데이터 정렬**       | 속성 값에 대한 비교 및 순서 지정 작업을 수행할 때 사용할 데이터 정렬 순서 또는 정렬 순서를 지정합니다.                                                                                                               | **Edm.String**                                                                                                                                                                                                                                                                                                                                                                       | 예                              | 아니요                  |
| **ConcurrencyMode** | 속성 값을 낙관적 동시성 검사에 사용하도록 지정합니다.                                                                                                                                                                    | 모든 **EDMSimpleType** 속성                                                                                                                                                                                                                                                                                                                                                     | 아니요                               | 예                 |
| **기본**         | 인스턴스화 시 값이 제공되지 않는 경우 속성의 기본값을 지정합니다.                                                                                                                                                                       | 모든 **EDMSimpleType** 속성                                                                                                                                                                                                                                                                                                                                                     | 예                              | 예                 |
| **FixedLength**     | 속성 값 길이가 다양할 수 있는지 여부를 지정합니다.                                                                                                                                                                                                  | **Edm.Binary**, **Edm.String**                                                                                                                                                                                                                                                                                                                                                       | 예                              | 아니요                  |
| **MaxLength**       | 속성 값의 최대 길이를 지정합니다.                                                                                                                                                                                                           | **Edm.Binary**, **Edm.String**                                                                                                                                                                                                                                                                                                                                                       | 예                              | 아니요                  |
| **Null 허용**        | 속성을 사용할 수 있는지 여부를 지정 된 **null** 값입니다.                                                                                                                                                                                                     | 모든 **EDMSimpleType** 속성                                                                                                                                                                                                                                                                                                                                                     | 예                              | 예                 |
| **전체 자릿수**       | 형식의 속성에 대 한 **10 진수**, 속성 값을 가질 수 있습니다 자릿수를 지정 합니다. 형식의 속성에 대 한 **시간**를 **DateTime**, 및 **DateTimeOffset**, 속성 값의 초의 소수 부분 자릿수를 지정 합니다. | **Edm.DateTime**하십시오 **Edm.DateTimeOffset**하십시오 **Edm.Decimal**, **Edm.Time**                                                                                                                                                                                                                                                                                                              | 예                              | 아니요                  |
| **배율**           | 속성 값에 대한 소수점 오른쪽의 자릿수를 지정합니다.                                                                                                                                                                      | **Edm.Decimal**                                                                                                                                                                                                                                                                                                                                                                      | 예                              | 아니요                  |
| **SRID**            | 시스템 공간 참조 시스템 ID를 지정합니다. 자세한 내용은 [SRID](http://en.wikipedia.org/wiki/SRID) 하 고 [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)합니다.                                                              | **Edm.Geography Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection** | 아니요                               | 예                 |
| **유니코드**         | 속성 값을 유니코드로 저장할지 여부를 나타냅니다.                                                                                                                                                                                                    | **Edm.String**                                                                                                                                                                                                                                                                                                                                                                       | 예                              | 예                 |

>[!NOTE]
> 개념적 모델에서 데이터베이스를 생성 하는 경우 데이터베이스 생성 마법사의 값을 인식 합니다 **StoreGeneratedPattern** 특성을 **속성** 요소 다음의 경우 네임 스페이스: http://schemas.microsoft.com/ado/2009/02/edm/annotation 합니다. 특성에 대해 지원 되는 값은 **Identity** 하 고 **계산 됨**합니다. 값이 **Identity** 는 데이터베이스에서 생성 된 id 값을 사용 하 여 데이터베이스 열을 만듭니다. 값이 **계산 됨** 는 데이터베이스에서 계산 되는 값을 사용 하 여 열을 만듭니다.

### <a name="example"></a>예제

다음 예제에서는 엔터티 형식의 속성에 적용된 패싯을 보여 줍니다.

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="http://schemas.microsoft.com/ado/2009/02/edm/annotation" />
   <Property Type="String"
             Name="ProductName"
             Nullable="false"
             MaxLength="50" />
   <Property Type="String"
             Name="Location"
             Nullable="true"
             MaxLength="25" />
 </EntityType>
```
