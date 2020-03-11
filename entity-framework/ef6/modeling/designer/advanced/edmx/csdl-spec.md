---
title: CSDL 사양-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 642e5977ecbbf0c474cac1ceae19d33a135aa875
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415516"
---
# <a name="csdl-specification"></a>CSDL 사양
CSDL(개념 스키마 정의 언어)은 데이터 기반 애플리케이션의 개념적 모델을 구성하는 엔터티, 관계 및 함수를 설명하는 XML 기반 언어입니다. 이 개념적 모델은 Entity Framework 또는 WCF Data Services에서 사용할 수 있습니다. CSDL로 설명 되는 메타 데이터는 Entity Framework에서 개념적 모델에 정의 된 엔터티 및 관계를 데이터 원본에 매핑하는 데 사용 됩니다. 자세한 내용은 [SSDL 사양](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) 및 [MSL 사양](~/ef6/modeling/designer/advanced/edmx/msl-spec.md)을 참조 하세요.

CSDL은 Entity Framework 엔터티 데이터 모델의 구현입니다.

Entity Framework 응용 프로그램에서 개념적 모델 메타 데이터는 csdl 파일 (CSDL로 작성 됨)에서 EdmItemCollection 인스턴스로 로드 되며,의 메서드를 사용 하 여 액세스할 수 있습니다. System.object. c a c. Entity Framework 개념적 모델 메타 데이터를 사용 하 여 개념적 모델에 대 한 쿼리를 데이터 소스 관련 명령으로 변환 합니다.

EF Designer는 디자인 타임에 개념적 모델 정보를 .edmx 파일에 저장 합니다. 빌드 시 EF Designer는 .edmx 파일의 정보를 사용 하 여 런타임에 Entity Framework에 필요한 csdl 파일을 만듭니다.

CSDL 버전은 XML 네임스페이스로 식별됩니다.

| CSDL 버전 | XML 네임 스페이스                                |
|:-------------|:---------------------------------------------|
| CSDL v1      | https://schemas.microsoft.com/ado/2006/04/edm |
| CSDL v2      | https://schemas.microsoft.com/ado/2008/09/edm |
| CSDL v3      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a>Association 요소(CSDL)

**Association** 요소는 두 엔터티 형식 간의 관계를 정의 합니다. 연결은 관계에 관련된 엔터티 형식과 복합성이라도 하는 관계의 각 End에 있는 가능한 엔터티 형식 수를 지정해야 합니다. 연결 end의 복합성 값은 1 (1), 0 또는 1 (0 .1) 또는 다 수 (\*) 일 수 있습니다. 이 정보는 두 자식 End 요소에 지정되어 있습니다.

연결의 한 End에 있는 엔터티 형식 인스턴스는 엔터티 형식에서 노출된 경우 탐색 속성 또는 외래 키를 통해 액세스할 수 있습니다.

애플리케이션에서 연결 인스턴스는 엔터티 형식 인스턴스 사이의 특정 연결을 나타냅니다. 연결 인스턴스는 연결 집합에 논리적으로 그룹화됩니다.

**연결** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소)
-   End (정확히 2 개 요소)
-   (0 개 또는 한 개의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Association** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                        |
|:---------------|:------------|:-----------------------------|
| **Name**       | 예         | 연결의 이름입니다. |

 

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)이 **Association** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 **고객** 및 **주문** 엔터티 형식에 외래 키가 노출 되지 않은 경우 **customerorders** 연결을 정의 하는 **Association** 요소를 보여 줍니다. 연결의 각 **End** 에 대 한 **복합성** 값은 여러 **주문이** **고객과**연결 될 수 있지만 한 명의 **고객** 만 **주문과**연결 될 수 있음을 의미 합니다. 또한 **OnDelete** 요소는 **고객이** 삭제 된 경우 특정 **고객과** 관련 되어 있고 ObjectContext로 로드 된 모든 **주문이** 삭제 됨을 나타냅니다.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

다음 예제에서는 **고객** 및 **주문** 엔터티 형식에 외래 키가 노출 된 경우 **customerorders** 연결을 정의 하는 **Association** 요소를 보여 줍니다. 외래 키가 노출 된 상태에서 엔터티 간의 관계는 **키를 사용** 하 여 관리 됩니다. 이 연결을 데이터 소스에 매핑하는 데 해당되는 AssociationSetMapping 요소가 필요하지 않습니다.

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

CSDL (개념 스키마 정의 언어)의 **AssociationSet** 요소는 동일한 형식의 연결 인스턴스에 대 한 논리적 컨테이너입니다. 연결 집합은 연결 인스턴스가 데이터 소스로 매핑될 수 있도록 해당 연결 인스턴스를 그룹화하는 데 필요한 정의를 제공합니다.  

**AssociationSet** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소 허용)
-   End (정확히 두 개의 요소 필요)
-   Annotation 요소 (0 개 이상의 요소 허용)

**Association** 특성은 연결 집합이 포함 하는 연결의 유형을 지정 합니다. 연결 집합의 end를 구성 하는 엔터티 집합은 정확히 두 개의 자식 **끝** 요소로 지정 됩니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **AssociationSet** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름  | 필수 여부 | 값                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**        | 예         | 엔터티 집합의 이름입니다. **Name** 특성의 값은 **Association** 특성의 값과 같을 수 없습니다.                                 |
| **연결** | 예         | 연결 집합에 포함되는 인스턴스의 연결에 대한 정규화된 이름입니다. 연결은 연결 집합과 동일한 네임스페이스에 있어야 합니다. |

 

> [!NOTE]
> 임의 개수의 주석 특성 (사용자 지정 XML 특성)이 **AssociationSet** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예에서는 두 개의 **AssociationSet** 요소가 포함 된 **EntityContainer** 요소를 보여 줍니다.

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

CSDL (개념 스키마 정의 언어)의 **CollectionType** 요소는 함수 매개 변수 또는 함수 반환 형식이 컬렉션 임을 지정 합니다. **CollectionType** 요소는 Parameter 요소 또는 ReturnType (Function) 요소의 자식일 수 있습니다. **형식** 특성 또는 다음 자식 요소 중 하나를 사용 하 여 컬렉션의 형식을 지정할 수 있습니다.

-   **CollectionType**
-   ReferenceType
-   RowType
-   TypeRef

> [!NOTE]
> 컬렉션의 형식이 **type** 특성과 자식 요소 모두를 사용 하 여 지정 되었는지 여부는 모델에서 유효성을 검사 하지 않습니다.

 

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **CollectionType** 요소에 적용할 수 있는 특성을 설명 합니다. **DefaultValue**, **MaxLength**, **fixedlength**, **Precision**, **Scale**, **Unicode**및 **Collation** 특성은 **edmsimpletypes**의 컬렉션에만 적용 됩니다.

| 특성 이름                                                          | 필수 여부 | 값                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**                                                                | 아니요          | 컬렉션의 형식입니다.                                                                                                                                                                                                      |
| **Null 허용**                                                            | 아니요          | 속성이 null 값을 가질 수 있는지 여부에 따라 **True**(기본값) 또는 **False**입니다. <br/> [!NOTE]                                                                                                                 |
| CSDL v1에 > 복합 형식 속성에 `Nullable="False"`있어야 합니다. |             |                                                                                                                                                                                                                                  |
| **DefaultValue**                                                        | 아니요          | 속성의 기본값입니다.                                                                                                                                                                                               |
| **MaxLength**                                                           | 아니요          | 속성 값의 최대 길이입니다.                                                                                                                                                                                        |
| **FixedLength**                                                         | 아니요          | 속성 값이 고정 길이 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.                                                                                                                           |
| **전체 자릿수**                                                           | 아니요          | 속성 값의 전체 자릿수입니다.                                                                                                                                                                                             |
| **배율**                                                               | 아니요          | 속성 값의 소수 자릿수입니다.                                                                                                                                                                                                 |
| **SRID**                                                                | 아니요          | 공간 시스템 참조 식별자입니다. 공간 형식의 속성에 대해서만 유효 합니다.   자세한 내용은 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) 를 참조 하세요. |
| **유니코드**                                                             | 아니요          | 속성 값이 유니코드 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.                                                                                                                                |
| **데이터 정렬**                                                           | 아니요          | 데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.                                                                                                                                                    |

 

> [!NOTE]
> 여러 주석 특성 (사용자 지정 XML 특성)을 **CollectionType** 요소에 적용할 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예에서는 **CollectionType** 요소를 사용 하 여 함수가 **Person** 엔터티 형식의 컬렉션을 반환 하도록 지정 하는 모델 정의 함수를 보여 줍니다 ( **ElementType** 특성으로 지정).

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
 

다음 예에서는 **CollectionType** 요소를 사용 하 여 함수가 **RowType** 요소에 지정 된 대로 행 컬렉션을 반환 하도록 지정 하는 모델 정의 함수를 보여 줍니다.

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
 

다음 예에서는 **CollectionType** 요소를 사용 하 여 함수가 **부서** 엔터티 형식의 컬렉션을 매개 변수로 허용 하도록 지정 하는 모델 정의 함수를 보여 줍니다.

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

**ComplexType** 요소는 **edmsimpletype** 속성 또는 다른 복합 형식으로 구성 된 데이터 구조를 정의 합니다.  복합 형식은 엔터티 형식 또는 다른 복합 형식의 속성 일 수 있습니다. 복합 형식은 복합 형식에서 데이터를 정의한다는 점에서 엔터티 형식과 유사합니다. 그러나 복합 형식과 엔터티 형식 사이에는 약간의 중요한 차이점이 존재합니다.

-   복합 형식은 식별자나 키를 포함하지 않으므로 독립적으로 존재할 수 없습니다. 복합 형식은 엔터티 형식 또는 다른 복합 형식의 속성으로만 존재할 수 있습니다.
-   복합 형식은 연결에 참여할 수 없습니다. 연결의 어느 End도 복합 형식이 될 수 없으므로 복합 형식에 대해 탐색 속성을 정의할 수 없습니다.
-   복합 형식의 스칼라 속성을 각각 null로 설정할 수 있지만 복합 형식 속성의 값은 null일 수 없습니다.

**ComplexType** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소)
-   Property(0개 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

다음 표에서는 **ComplexType** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름                                                                                                 | 필수 여부 | 값                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 이름                                                                                                           | 예         | 복합 형식의 이름입니다. 복합 형식의 이름은 모델 범위 내에 있는 다른 복합 형식, 엔터티 형식 또는 연결의 이름과 같을 수 없습니다. |
| BaseType                                                                                                       | 아니요          | 정의되는 복합 형식의 기본 형식인 다른 복합 형식의 이름입니다. <br/> [!NOTE]                                                                     |
| >이 특성은 CSDL v 1에서 적용 되지 않습니다. 복합 형식에 대한 상속은 해당 버전에서 지원되지 않습니다. |             |                                                                                                                                                                                     |
| 요약                                                                                                       | 아니요          | 복합 형식이 추상 형식 인지 여부에 따라 **True** 또는 **False** (기본값)입니다. <br/> [!NOTE]                                                                  |
| >이 특성은 CSDL v 1에서 적용 되지 않습니다. 해당 버전의 복합 형식은 추상 형식일 수 없습니다.         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)은 **ComplexType** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 **Edmsimpletype** 속성 **StreetAddress**, **City**, **StateOrProvince**, **Country**및 **PostalCode**를 사용 하는 복합 형식 **주소**를 보여 줍니다.

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

복합 형식 **주소** (위)를 엔터티 형식의 속성으로 정의 하려면 엔터티 형식 정의에서 속성 형식을 선언 해야 합니다. 다음 예제에서는 엔터티 형식 (**게시자**)의 복합 형식인 **Address** 속성을 보여 줍니다.

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

CSDL (개념 스키마 정의 언어)의 **DefiningExpression** 요소에는 개념적 모델에서 함수를 정의 하는 Entity SQL 식이 포함 되어 있습니다.  

> [!NOTE]
> 유효성 검사를 위해 **DefiningExpression** 요소는 임의의 콘텐츠를 포함할 수 있습니다. 그러나 **DefiningExpression** 요소가 유효한 Entity SQL를 포함 하지 않는 경우 Entity Framework는 런타임에 예외를 throw 합니다.

 

### <a name="applicable-attributes"></a>적용 가능한 특성

**DefiningExpression** 요소에는 원하는 수의 주석 특성 (사용자 지정 XML 특성)을 적용할 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예에서는 **DefiningExpression** 요소를 사용 하 여 책이 게시 된 이후의 연도 수를 반환 하는 함수를 정의 합니다. **DefiningExpression** 요소의 내용은 Entity SQL로 작성 됩니다.

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a>Dependent 요소(CSDL)

CSDL (개념 스키마 정의 언어)의 **dependent** 요소는 참조 제약 조건의 종속 끝을 정의 하는 참조 제약 조건 요소에 대 한 자식 요소입니다. 참조 무결성 제약 **조건** 요소는 관계형 데이터베이스의 참조 무결성 제약 조건과 유사한 기능을 정의 합니다. 데이터베이스 테이블의 열에서 다른 테이블의 기본 키를 참조하는 방식과 동일하게 엔터티 형식의 속성은 다른 엔터티 형식의 엔터티 키를 참조할 수 있습니다. 참조 되는 엔터티 형식을 제약 조건의 *주 끝* 이라고 합니다. 주 끝을 참조 하는 엔터티 형식을 제약 조건의 *종속 끝* 이라고 합니다. **Propertyref** 요소는 주 끝을 참조 하는 키를 지정 하는 데 사용 됩니다.

**종속** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   PropertyRef (하나 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **종속** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **역할**       | 예         | 연결의 종속 끝에 있는 엔터티 형식의 이름입니다. |

 

> [!NOTE]
> **종속** 요소에는 원하는 수의 주석 특성 (사용자 지정 XML 특성)을 적용할 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 **PublishedBy** 연결 정의의 일부로 사용 **되는 설명 요소를** 보여 줍니다. **Book** 엔터티 형식의 **PublisherId** 속성은 참조 제약 조건의 종속 끝을 구성 합니다.

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

CSDL (개념 스키마 정의 언어)의 **문서** 요소는 부모 요소에 정의 된 개체에 대 한 정보를 제공 하는 데 사용할 수 있습니다. .Edmx 파일에서 **설명서** 요소가 EF 디자이너의 디자인 화면에서 개체로 표시 되는 요소의 자식인 경우 (예: 엔터티, 연결 또는 속성), **설명서** 요소의 내용이 개체에 대 한 Visual Studio **속성** 창에 표시 됩니다.

**문서** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   **요약**: 부모 요소에 대 한 간단한 설명입니다. (0개 또는 한 개의 요소)
-   이상 **설명**: 부모 요소에 대 한 광범위 한 설명입니다. (0개 또는 한 개의 요소)
-   Annotation 요소 (0개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

주석 특성 (사용자 지정 XML 특성)은 **문서** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제에서는 EntityType 요소의 자식 요소인 **문서** 요소를 보여 줍니다. 아래 코드 조각이 .edmx 파일의 CSDL 콘텐츠에 있었던 경우에는 `Customer` 엔터티 형식을 클릭 하면 **Summary** 및 **Valdescription** 요소의 내용이 Visual Studio **속성** 창에 표시 됩니다.

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

CSDL (개념 스키마 정의 언어)의 **End** 요소는 Association 요소 또는 AssociationSet 요소의 자식일 수 있습니다. 각 경우에서 **End** 요소의 역할이 다르고 적용 가능한 특성이 다릅니다.

### <a name="end-element-as-a-child-of-the-association-element"></a>Association 요소의 자식인 End 요소

**End** 요소 ( **association** 요소의 자식)는 연결의 한 end에 있는 엔터티 형식 및 해당 연결의 해당 end에 있을 수 있는 엔터티 형식 인스턴스 수를 식별 합니다. 연결 End는 연결의 일부로 정의되고 연결에는 정확히 두 개의 연결 End가 있어야 합니다. 연결의 한 End에 있는 엔터티 형식 인스턴스는 엔터티 형식에서 노출된 경우 탐색 속성 또는 외래 키를 통해 액세스할 수 있습니다.  

**End** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소)
-   OnDelete (0 개 또는 한 개의 요소)
-   Annotation 요소 (0 개 이상의 요소)

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **End** 요소를 **Association** 요소의 자식일 때 적용할 수 있는 특성을 설명 합니다.

| 특성 이름   | 필수 여부 | 값                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**         | 예         | 연결의 한 End에 있는 엔터티 형식의 이름                                                                                                                                                                                                                                                                                                                                                         |
| **역할**         | 아니요          | 연결 End의 이름. 이름을 지정하지 않으면 연결 End에 있는 엔터티 형식의 이름이 사용됩니다.                                                                                                                                                                                                                                                                                           |
| **다중성** | 예         | **1**, **0**.1 또는 **\*** 연결의 끝에 있을 수 있는 엔터티 형식 인스턴스 수에 따라 달라 집니다. <br/> **1** 은 정확히 하나의 엔터티 형식 인스턴스가 연결 end에 존재 함을 나타냅니다. <br/> **0 .1** 은 연결 end에 엔터티 형식 인스턴스가 0 개 또는 한 개 존재 함을 나타냅니다. <br/> **\*** 는 연결 end에 엔터티 형식 인스턴스가 0 개 이상 있음을 나타냅니다. |

 

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)은 **끝** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

#### <a name="example"></a>예제

다음 예제에서는 **Customerorders** 연결을 정의 하는 **association** 요소를 보여 줍니다. 연결의 각 **End** 에 대 한 **복합성** 값은 여러 **주문이** **고객과**연결 될 수 있지만 한 명의 **고객** 만 **주문과**연결 될 수 있음을 의미 합니다. 또한 **OnDelete** 요소는 **고객이** 삭제 된 경우 특정 **고객과** 관련 되어 있고 ObjectContext에 로드 된 모든 **주문이** 삭제 됨을 나타냅니다.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a>AssociationSet 요소의 자식인 End 요소

**End** 요소는 연결 집합의 한쪽 끝을 지정 합니다. **AssociationSet** 요소는 두 개의 **End** 요소를 포함 해야 합니다. **End** 요소에 포함 된 정보는 연결 집합을 데이터 소스에 매핑하는 데 사용 됩니다.

**End** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소)
-   Annotation 요소 (0 개 이상의 요소)

> [!NOTE]
> Annotation 요소는 다른 모든 자식 요소 뒤에 와야 합니다. Annotation 요소는 CSDL v2 이상 에서만 사용할 수 있습니다.

 

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **AssociationSet** 요소의 자식인 **End** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EntitySet**  | 예         | 부모 **AssociationSet** 요소의 한쪽 끝을 정의 하는 **EntitySet** 요소의 이름입니다. **EntitySet** 요소는 부모 **AssociationSet** 요소와 동일한 엔터티 컨테이너에 정의 되어야 합니다. |
| **역할**       | 아니요          | 연결 집합 End의 이름. **Role** 특성을 사용 하지 않는 경우 연결 집합 끝의 이름은 엔터티 집합의 이름이 됩니다.                                                                   |

 

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)은 **끝** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

#### <a name="example"></a>예제

다음 예제에서는 두 개의 **AssociationSet** 요소를 포함 하는 **EntityContainer** 요소를 보여 줍니다. 각 요소에는 두 개의 **End** 요소가 있습니다.

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

CSDL (개념 스키마 정의 언어)의 **EntityContainer** 요소는 엔터티 집합, 연결 집합 및 함수 가져오기에 대 한 논리적 컨테이너입니다. 개념적 모델 엔터티 컨테이너는 EntityContainerMapping 요소를 통해 저장소 모델 엔터티 컨테이너로 매핑됩니다. 스토리지 모델 엔터티 컨테이너는 데이터베이스의 구조를 설명합니다. 엔터티 집합은 데이터베이스의 테이블을 설명하고, 연결 집합은 데이터베이스의 외래 키 제약 조건을 설명하고, 함수 가져오기는 데이터베이스의 저장 프로시저를 설명합니다.

**EntityContainer** 요소는 문서 요소를 0 개 또는 1 개 포함할 수 있습니다. **설명서** 요소가 있으면 모든 **EntitySet**, **AssociationSet**및 **FunctionImport** 요소 앞에와 야 합니다.

**EntityContainer** 요소는 나열 된 순서 대로 다음과 같은 자식 요소를 0 개 이상 포함할 수 있습니다.

-   EntitySet
-   AssociationSet
-   FunctionImport
-   Annotation 요소

동일한 네임 스페이스 내에 있는 다른 **entitycontainer** 의 콘텐츠를 포함 하도록 **entitycontainer** 요소를 확장할 수 있습니다. 다른 **entitycontainer**의 콘텐츠를 포함 하려면 참조 **Entitycontainer** 요소에서 **Extends** 특성의 값을 포함 하려는 **EntityContainer** 요소의 이름으로 설정 합니다. 포함 된 **entitycontainer** 요소의 모든 자식 요소는 참조 **entitycontainer** 요소의 자식 요소로 처리 됩니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Using** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Name**       | 예         | 엔터티 컨테이너의 이름입니다.                               |
| **제어할**    | 아니요          | 동일한 네임스페이스 내에 있는 다른 엔터티 컨테이너의 이름입니다. |

 

> [!NOTE]
> 여러 주석 특성 (사용자 지정 XML 특성)을 **EntityContainer** 요소에 적용할 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 세 개의 엔터티 집합과 두 개의 연결 집합을 정의 하는 **EntityContainer** 요소를 보여 줍니다.

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

개념 스키마 정의 언어의 **EntitySet** 요소는 엔터티 형식 및 해당 엔터티 형식에서 파생 된 모든 형식의 인스턴스에 대 한 논리적 컨테이너입니다. 엔터티 형식과 엔터티 집합 간의 관계는 관계형 데이터베이스의 행과 테이블 간 관계와 유사합니다. 행과 같이 엔터티 형식은 관련 데이터 집합을 정의하고, 테이블과 같이 엔터티 집합은 이러한 정의의 인스턴스를 포함합니다. 엔터티 집합은 엔터티 형식 인스턴스가 데이터 소스의 관련 데이터 구조에 매핑될 수 있도록 이러한 인스턴스를 그룹화하는 구문을 제공합니다.  

특정 엔터티 형식에 대한 두 개 이상의 엔터티 집합을 정의할 수 있습니다.

> [!NOTE]
> EF Designer는 유형별 다중 엔터티 집합이 포함 된 개념적 모델을 지원 하지 않습니다.

 

**EntitySet** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   문서 요소 (0 개 또는 한 개의 요소 허용)
-   Annotation 요소 (0 개 이상의 요소 허용)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **EntitySet** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Name**       | 예         | 엔터티 집합의 이름입니다.                                                              |
| **EntityType** | 예         | 엔터티 집합에 포함되는 인스턴스의 엔터티 형식에 대한 정규화된 이름입니다. |

 

> [!NOTE]
> 임의 개수의 주석 특성 (사용자 지정 XML 특성)은 **EntitySet** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 세 개의 **EntitySet** 요소가 포함 된 **EntityContainer** 요소를 보여 줍니다.

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
 

형식당 여러 엔터티 집합(MEST)을 정의할 수 있습니다. 다음 예제에서는 **Book** 엔터티 형식에 대 한 두 개의 엔터티 집합이 있는 엔터티 컨테이너를 정의 합니다.

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

**EntityType** 요소는 개념적 모델에서 고객 또는 주문과 같은 최상위 개념의 구조를 나타냅니다. 엔터티 형식은 애플리케이션에서 엔터티 형식 인스턴스에 대한 템플릿입니다. 각 템플릿에는 다음 정보가 들어 있습니다.

-   고유한 이름 (필수)
-   하나 이상의 속성에 의해 정의되는 엔터티 키 (필수)
-   상위 데이터의 속성 필요에 따라
-   연결의 한 End에서 다른 End로의 탐색을 허용하는 탐색 속성 필요에 따라

애플리케이션에서 엔터티 형식의 인스턴스는 특정 고객 또는 주문과 같은 특정 개체를 나타냅니다. 엔터티 집합 내에 각 엔터티 형식 인스턴스에 대한 고유한 엔터티 키가 있어야 합니다.

두 엔터티 형식 인스턴스는 형식이 동일하고 해당 엔터티 키 값이 동일한 경우에만 동일한 것으로 간주됩니다.

**EntityType** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소)
-   Key(0개 또는 1개)
-   Property(0개 이상의 요소)
-   NavigationProperty(0개 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **EntityType** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름                                                                                                                                  | 필수 여부 | 값                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Name**                                                                                                                                        | 예         | 엔터티 형식의 이름.                                                                     |
| **BaseType**                                                                                                                                    | 아니요          | 정의되는 엔터티 형식의 기본 형식인 다른 엔터티 형식의 이름입니다.  |
| **이면**                                                                                                                                    | 아니요          | 엔터티 형식이 추상 형식 인지 여부에 따라 **True** 또는 **False**입니다.                 |
| **Open**                                                                                                                                    | 아니요          | 엔터티 형식이 개방형 엔터티 형식 인지 여부에 따라 **True** 또는 **False** 입니다. <br/> [!NOTE] |
| > **OpenType** 특성은 ADO.NET Data Services와 함께 사용 되는 개념적 모델에 정의 된 엔터티 형식에만 적용할 수 있습니다. |             |                                                                                                  |

 

> [!NOTE]
> **EntityType** 요소에는 원하는 수의 주석 특성 (사용자 지정 XML 특성)을 적용할 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 세 개의 **속성** 요소와 **NavigationProperty** 요소가 포함 된 **EntityType** 요소를 보여 줍니다.

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

**EnumType** 요소는 열거 형식을 나타냅니다.

**EnumType** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소)
-   Member (0 개 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **EnumType** 요소에 적용 될 수 있는 특성을 설명 합니다.

| 특성 이름     | 필수 여부 | 값                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**           | 예         | 엔터티 형식의 이름.                                                                                                                                                                  |
| **IsFlags**        | 아니요          | 열거형 형식을 플래그 집합으로 사용할 수 있는지 여부에 따라 **True** 또는 **False**입니다. 기본값은 **False입니다**.                                                                     |
| **UnderlyingType** | 아니요          | 형식의 값 범위를 정의 하는 **edm.** **Byte**, **edm. Int16** **, edm. i**n t **32입니다.**   열거형 요소의 기본 기본 형식은 **Edm. Int32입니다**. |

 

> [!NOTE]
> **EnumType** 요소에는 원하는 수의 주석 특성 (사용자 지정 XML 특성)을 적용할 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 세 개의 **멤버** 요소를 포함 하는 **EnumType** 요소를 보여 줍니다.

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a>Function 요소(CSDL)

CSDL (개념 스키마 정의 언어)의 **Function** 요소는 개념적 모델에서 함수를 정의 하거나 선언 하는 데 사용 됩니다. 함수는 DefiningExpression 요소를 사용하여 정의됩니다.  

**함수** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소)
-   Parameter(0개 이상의 요소)
-   DefiningExpression(0개 또는 한 개의 요소)
-   ReturnType (함수) (0 개 또는 한 개의 요소)
-   Annotation 요소 (0 개 이상의 요소)

함수에 대 한 반환 형식은 **returntype** (function) 요소 또는 **returntype** 특성 (아래 참조) 중 하나를 사용 하 여 지정 해야 합니다. EdmSimpleType, 엔터티 형식, 복합 형식, 행 형식 또는 참조 형식(또는 이러한 형식 중 하나의 컬렉션)이 반환 형식이 될 수 있습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Function** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                              |
|:---------------|:------------|:-----------------------------------|
| **Name**       | 예         | 함수의 이름.          |
| **ReturnType** | 아니요          | 함수에서 반환하는 형식입니다. |

 

> [!NOTE]
> 원하는 수의 주석 특성 (사용자 지정 XML 특성)이 **Function** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예에서는 **함수** 요소를 사용 하 여 강사가 고용 된 이후 연도 수를 반환 하는 함수를 정의 합니다.

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a>FunctionImport 요소(CSDL)

CSDL (개념 스키마 정의 언어)의 **FunctionImport** 요소는 데이터 소스에 정의 되어 있지만 개념적 모델을 통해 개체에서 사용할 수 있는 함수를 나타냅니다. 예를 들어, 저장소 모델에 Function 요소를 사용하여 데이터베이스의 저장 프로시저를 나타낼 수 있습니다. 개념적 모델의 **FunctionImport** 요소는 Entity Framework 응용 프로그램의 해당 함수를 나타내며 FunctionImportMapping 요소를 사용 하 여 저장소 모델 함수에 매핑됩니다. 애플리케이션에서 함수가 호출되면 해당되는 저장 프로시저가 데이터베이스에서 실행됩니다.

**FunctionImport** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소 허용)
-   Parameter(0개 이상의 요소 허용)
-   Annotation 요소 (0 개 이상의 요소 허용)
-   ReturnType (FunctionImport) (0 개 이상의 요소 허용)

함수가 허용 하는 각 매개 변수에 대해 하나의 **매개 변수** 요소를 정의 해야 합니다.

함수에 대 한 반환 형식은 **returntype** (FunctionImport) 요소 또는 **returntype** 특성 (아래 참조) 중 하나를 사용 하 여 지정 해야 합니다. 반환 형식 값은 EdmSimpleType, EntityType 또는 ComplexType의 컬렉션 이어야 합니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **FunctionImport** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름   | 필수 여부 | 값                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**         | 예         | 가져온 함수의 이름입니다.                                                                                                                                                                    |
| **ReturnType**   | 아니요          | 함수에서 반환하는 형식입니다. 함수에서 값을 반환하지 않으면 이 특성을 사용하지 마십시오. 그렇지 않으면 값은 ComplexType, EntityType 또는 EDMSimpleType의 컬렉션 이어야 합니다.        |
| **EntitySet**    | 아니요          | 함수가 엔터티 형식의 컬렉션을 반환 하는 경우 **EntitySet** 의 값은 컬렉션이 속한 엔터티 집합 이어야 합니다. 그렇지 않으면 **EntitySet** 특성을 사용할 수 없습니다. |
| **IsComposable** | 아니요          | 값이 true로 설정 된 경우 함수는 구성 가능 (테이블 반환 함수) 이며 LINQ 쿼리에서 사용할 수 있습니다.  기본값은 **false**입니다.                                                           |

 

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)이 **FunctionImport** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 하나의 매개 변수를 받아들이고 엔터티 형식의 컬렉션을 반환 하는 **FunctionImport** 요소를 보여 줍니다.

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a>Key 요소(CSDL)

**Key** 요소는 EntityType 요소의 자식 요소 이며 *엔터티 키* (id를 확인 하는 엔터티 형식의 속성 또는 속성 집합)를 정의 합니다. 엔터티 키를 구성하는 속성은 디자인 타임에 선택됩니다. 엔터티 키 속성의 값은 런타임에 엔터티 집합 내에서 엔터티 형식 인스턴스를 고유하게 식별해야 합니다. 엔터티 키를 구성하는 속성을 선택하여 엔터티 집합에서 인스턴스의 고유성을 보장해야 합니다. **키** 요소는 엔터티 형식의 속성을 하나 이상 참조 하 여 엔터티 키를 정의 합니다.

**키** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.

-   PropertyRef (하나 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

**키** 요소에는 원하는 수의 주석 특성 (사용자 지정 XML 특성)을 적용할 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

아래 예제에서는 **Book**이라는 엔터티 형식을 정의 합니다. 엔터티 키는 엔터티 형식의 **ISBN** 속성을 참조 하 여 정의 됩니다.

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
 

Isbn (국제 표준 책 번호)은 책을 고유 하 게 식별 하기 때문에 **isbn** 속성은 엔터티 키에 적합 한 선택입니다.

다음 예제에서는 두 개의 속성 **이름** 및 **주소로**구성 된 엔터티 키가 있는 엔터티 형식 (**Author**)을 보여 줍니다.

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
 

이름 **및** **주소** 를 사용 하는 것은 동일한 이름의 작성자 두 명이 동일한 주소에 존재할 가능성이 없기 때문에 적절 한 선택입니다. 그러나 이러한 엔터티 키 선택이 엔터티 집합에서의 고유한 엔터티 키를 절대적으로 보장하지는 않습니다. 이 경우 작성자를 고유 하 게 식별 하는 데 사용할 수 있는 **AuthorId**과 같은 속성을 추가 하는 것이 좋습니다.

 

## <a name="member-element-csdl"></a>Member 요소 (CSDL)

**Member** 요소는 EnumType 요소의 자식 요소 이며 열거 형식의 멤버를 정의 합니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **FunctionImport** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | 예         | 멤버의 이름입니다.                                                                                                                                                                  |
| **Value**      | 아니요          | 멤버의 값입니다. 기본적으로 첫 번째 멤버의 값은 0이 고 각 연속 열거자의 값은 1 씩 증가 합니다. 값이 같은 멤버가 여러 개 있을 수 있습니다. |

 

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)이 **FunctionImport** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 세 개의 **멤버** 요소를 포함 하는 **EnumType** 요소를 보여 줍니다.

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a>NavigationProperty 요소(CSDL)

**NavigationProperty** 요소는 연결의 다른 쪽 end에 대 한 참조를 제공 하는 탐색 속성을 정의 합니다. Property 요소를 사용하여 정의된 속성과 달리 탐색 속성은 데이터의 모양 및 특징을 정의하지 않습니다. 이러한 속성은 두 엔터티 형식 사이의 연결을 탐색하는 방법을 제공합니다.

탐색 속성은 연결의 양쪽 End에 있는 엔터티 형식 모두에 대해 선택적 요소입니다. 연결의 End에 있는 한 엔터티 형식에 대해 탐색 속성을 정의하는 경우 연결의 다른 End에 있는 엔터티 형식에 대해서는 탐색 속성을 정의할 필요가 없습니다.

탐색 속성에 의해 반환된 데이터 형식은 해당 원격 연결 End의 복합성에 의해 결정됩니다. 예를 들어 Order **Snavprop**탐색 속성이 **customer** 엔터티 형식에 존재 하 고 **고객과** **주문**간에 일 대 다 연결을 탐색 한다고 가정 합니다. 탐색 속성에 대 한 원격 연결 end의 복합성은 다 수 (\*) 이므로 해당 데이터 형식은 **Order**의 컬렉션입니다. 마찬가지로, 탐색 속성인 **CustomerNavProp**가 **Order** 엔터티 형식에 있는 경우 원격 끝의 복합성은 one (1) 이므로 해당 데이터 형식은 **Customer** 가 됩니다.

**NavigationProperty** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **NavigationProperty** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름   | 필수 여부 | 값                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**         | 예         | 탐색 속성의 이름입니다.                                                                                                                                                                                                             |
| **관계만** | 예         | 모델 범위 내에 있는 연결의 이름입니다.                                                                                                                                                                                |
| **ToRole**       | 예         | 탐색이 끝나는 연결의 End입니다. **Torole** 특성의 값은 association End (AssociationEnd 요소에 정의 됨) 중 하나에 정의 된 **Role** 특성 중 하나의 값과 동일 해야 합니다.       |
| **FromRole**     | 예         | 탐색이 시작되는 연결의 End입니다. **Fromrole** 특성의 값은 association End (AssociationEnd 요소에 정의 됨) 중 하나에 정의 된 **Role** 특성 중 하나의 값과 동일 해야 합니다. |

 

> [!NOTE]
> 원하는 수의 주석 특성 (사용자 지정 XML 특성)이 **NavigationProperty** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 두 개의 탐색 속성 (**PublishedBy** 및 **WrittenBy**)을 사용 하 여 엔터티 형식 (**Book**)을 정의 합니다.

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

CSDL (개념 스키마 정의 언어)의 **OnDelete** 요소는 연결과 연결 된 동작을 정의 합니다. **작업** 특성이 연결의 한쪽 끝에서 **Cascade** 로 설정 된 경우 첫 번째 끝의 엔터티 형식이 삭제 되 면 연결의 반대쪽 끝에 있는 관련 엔터티 형식이 삭제 됩니다. 두 엔터티 형식 간의 연결이 기본 키 대 기본 키 관계인 경우 **OnDelete** 사양에 관계 없이 연결의 다른 쪽 end에 있는 주 개체가 삭제 되 면 로드 된 종속 개체가 삭제 됩니다.  

> [!NOTE]
> **OnDelete** 요소는 응용 프로그램의 런타임 동작에만 영향을 줍니다. 데이터 원본의 동작에는 영향을 주지 않습니다. 데이터 소스에 정의된 동작은 애플리케이션에 정의된 동작과 같아야 합니다.

 

**OnDelete** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **OnDelete** 요소에 적용 될 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **작업**     | 예         | **Cascade** 또는 **None**입니다. **Cascade**인 경우 주 엔터티 형식이 삭제 되 면 종속 엔터티 형식이 삭제 됩니다. **None**인 경우 주 엔터티 형식이 삭제 되 면 종속 엔터티 형식이 삭제 되지 않습니다. |

 

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)이 **Association** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 **Customerorders** 연결을 정의 하는 **association** 요소를 보여 줍니다. **OnDelete** 요소는 **고객이** 삭제 될 때 특정 **고객과** 관련 되어 있고 ObjectContext로 로드 된 모든 **주문이** 삭제 됨을 나타냅니다.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a>Parameter 요소(CSDL)

CSDL (개념 스키마 정의 언어)의 **Parameter** 요소는 FunctionImport 요소 또는 Function 요소의 자식일 수 있습니다.

### <a name="functionimport-element-application"></a>FunctionImport 요소 적용

**매개 변수** 요소 ( **FunctionImport** 요소의 자식)는 CSDL에 선언 된 함수 가져오기에 대 한 입력 및 출력 매개 변수를 정의 하는 데 사용 됩니다.

**Parameter** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소 허용)
-   Annotation 요소 (0 개 이상의 요소 허용)

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **매개 변수** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | 예         | 매개 변수의 이름입니다.                                                                                                                                                                                                      |
| **Type**       | 예         | 매개 변수 형식입니다. 값이 **EDMSimpleType** 또는 모델 범위에 포함되는 복합 형식이어야 합니다.                                                                                                             |
| **모드**       | 아니요          | 매개 변수가 입력, 출력 또는 입/출력 매개 변수 인지에 따라 **In**, **Out**또는 **InOut** 입니다.                                                                                                                |
| **MaxLength**  | 아니요          | 매개 변수의 최대 허용 길이입니다.                                                                                                                                                                                    |
| **전체 자릿수**  | 아니요          | 매개 변수의 전체 자릿수입니다.                                                                                                                                                                                                 |
| **배율**      | 아니요          | 매개 변수의 소수 자릿수입니다.                                                                                                                                                                                                     |
| **SRID**       | 아니요          | 공간 시스템 참조 식별자입니다. 공간 형식의 매개 변수에만 유효 합니다. 자세한 내용은 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)를 참조 하세요. |

 

> [!NOTE]
> 원하는 수의 주석 특성 (사용자 지정 XML 특성)이 **Parameter** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

#### <a name="example"></a>예제

다음 예제에서는 하나의 **매개 변수** 자식 요소가 있는 **FunctionImport** 요소를 보여 줍니다. 함수는 하나의 입력 매개 변수를 받아들이고 엔터티 형식의 컬렉션을 반환합니다.

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a>Function 요소 적용

**Parameter** 요소 ( **Function** 요소의 자식)는 개념적 모델에 정의 되거나 선언 된 함수에 대 한 매개 변수를 정의 합니다.

**Parameter** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소)
-   CollectionType (0 개 또는 한 개의 요소)
-   ReferenceType (0 개 또는 한 개의 요소)
-   RowType (0 개 또는 한 개의 요소)

> [!NOTE]
> **CollectionType**, **ReferenceType**또는 **RowType** 요소 중 하나만 **속성** 요소의 자식 요소가 될 수 있습니다.

 

-   Annotation 요소 (0 개 이상의 요소 허용)

> [!NOTE]
> Annotation 요소는 다른 모든 자식 요소 뒤에 와야 합니다. Annotation 요소는 CSDL v2 이상 에서만 사용할 수 있습니다.

 

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **매개 변수** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름   | 필수 여부 | 값                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**         | 예         | 매개 변수의 이름입니다.                                                                                                                                                                                                      |
| **Type**         | 아니요          | 매개 변수 형식입니다. 매개 변수는 다음 형식 또는 이러한 형식의 컬렉션일 수 있습니다. <br/> **EdmSimpleType** <br/> 엔터티 형식(entity type) <br/> 복합 형식 <br/> 행 형식 <br/> 참조 형식(reference type)                             |
| **Null 허용**     | 아니요          | 속성에 **null** 값을 사용할 수 있는지 여부에 따라 **True** (기본값) 또는 **False** 입니다.                                                                                                                          |
| **DefaultValue** | 아니요          | 속성의 기본값입니다.                                                                                                                                                                                              |
| **MaxLength**    | 아니요          | 속성 값의 최대 길이입니다.                                                                                                                                                                                       |
| **FixedLength**  | 아니요          | 속성 값이 고정 길이 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.                                                                                                                          |
| **전체 자릿수**    | 아니요          | 속성 값의 전체 자릿수입니다.                                                                                                                                                                                            |
| **배율**        | 아니요          | 속성 값의 소수 자릿수입니다.                                                                                                                                                                                                |
| **SRID**         | 아니요          | 공간 시스템 참조 식별자입니다. 공간 형식의 속성에 대해서만 유효 합니다. 자세한 내용은 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)를 참조 하세요. |
| **유니코드**      | 아니요          | 속성 값이 유니코드 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.                                                                                                                               |
| **데이터 정렬**    | 아니요          | 데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.                                                                                                                                                   |

 

> [!NOTE]
> 원하는 수의 주석 특성 (사용자 지정 XML 특성)이 **Parameter** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

#### <a name="example"></a>예제

다음 예제에서는 하나의 **매개 변수** 자식 요소를 사용 하 여 함수 매개 변수를 정의 하는 **함수** 요소를 보여 줍니다.

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a>Principal 요소(CSDL)

CSDL (개념 스키마 정의 언어)의 **principal** 요소는 참조 제약 조건의 주 끝을 정의 하는 참조 제약 조건 요소에 대 한 자식 요소입니다. 참조 무결성 제약 **조건** 요소는 관계형 데이터베이스의 참조 무결성 제약 조건과 유사한 기능을 정의 합니다. 데이터베이스 테이블의 열에서 다른 테이블의 기본 키를 참조하는 방식과 동일하게 엔터티 형식의 속성은 다른 엔터티 형식의 엔터티 키를 참조할 수 있습니다. 참조 되는 엔터티 형식을 제약 조건의 *주 끝* 이라고 합니다. 주 끝을 참조 하는 엔터티 형식을 제약 조건의 *종속 끝* 이라고 합니다. **Propertyref** 요소는 종속 end에서 참조 하는 키를 지정 하는 데 사용 됩니다.

**주요** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   PropertyRef (하나 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Principal** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **역할**       | 예         | 연결의 주 끝에 있는 엔터티 형식의 이름입니다. |

 

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)이 **Principal** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 **PublishedBy** 연결의 정의에 포함 되는 **설명 요소를** 보여 줍니다. **게시자** 엔터티 형식의 **Id** 속성은 참조 제약 조건의 주 끝을 구성 합니다.

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

CSDL (개념 스키마 정의 언어)의 **Property** 요소는 EntityType 요소, ComplexType 요소 또는 RowType 요소의 자식일 수 있습니다.

### <a name="entitytype-and-complextype-element-applications"></a>EntityType 및 ComplexType 요소 적용

**속성** 요소 ( **EntityType** 또는 **ComplexType** 요소의 자식)는 엔터티 형식 인스턴스 또는 복합 형식 인스턴스에 포함 될 데이터의 모양 및 특징을 정의 합니다. 개념적 모델의 속성은 클래스에 정의된 속성과 유사합니다. 클래스의 속성이 클래스의 모양을 정의하고 개체에 대한 정보를 전달하는 것과 동일한 방식으로, 개념적 모델의 속성은 엔터티 형식의 모양을 정의하고 엔터티 형식 인스턴스에 대한 정보를 전달합니다.

**Property** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   문서 요소 (0 개 또는 한 개의 요소 허용)
-   Annotation 요소 (0 개 이상의 요소 허용)

다음 패싯을 **Nullable**, **DefaultValue**, **MaxLength**, **fixedlength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**등의 **속성** 요소에 적용할 수 있습니다. 패싯은 속성 값이 데이터 저장소에 저장된 방식에 대한 정보를 제공하는 XML 특성입니다.

> [!NOTE]
> 패싯은 **Edmsimpletype**형식의 속성에만 적용할 수 있습니다.

 

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Property** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름                                                         | 필수 여부 | 값                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                                                               | 예         | 속성의 이름입니다.                                                                                                                                                                                                       |
| **Type**                                                               | 예         | 속성 값의 형식입니다. 속성 값 유형은 **EDMSimpleType** 또는 모델 범위에 포함되는 복합 유형(정규화된 이름으로 표시)이어야 합니다.                                                 |
| **Null 허용**                                                           | 아니요          | 속성이 null 값을 가질 수 있는지 여부에 따라 **True**(기본값) 또는 <strong>False</strong>입니다. <br/> [!NOTE]                                                                                                   |
| CSDL v1 복합 형식 속성의 >에는 `Nullable="False"`있어야 합니다. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                       | 아니요          | 속성의 기본값입니다.                                                                                                                                                                                              |
| **MaxLength**                                                          | 아니요          | 속성 값의 최대 길이입니다.                                                                                                                                                                                       |
| **FixedLength**                                                        | 아니요          | 속성 값이 고정 길이 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.                                                                                                                          |
| **전체 자릿수**                                                          | 아니요          | 속성 값의 전체 자릿수입니다.                                                                                                                                                                                            |
| **배율**                                                              | 아니요          | 속성 값의 소수 자릿수입니다.                                                                                                                                                                                                |
| **SRID**                                                               | 아니요          | 공간 시스템 참조 식별자입니다. 공간 형식의 속성에 대해서만 유효 합니다. 자세한 내용은 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)를 참조 하세요. |
| **유니코드**                                                            | 아니요          | 속성 값이 유니코드 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.                                                                                                                               |
| **데이터 정렬**                                                          | 아니요          | 데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.                                                                                                                                                   |
| **ConcurrencyMode**                                                    | 아니요          | **없음**(기본값) 또는 **고정**입니다. 이 값을 **고정**으로 설정하면 속성 값이 낙관적 동시성 검사에 사용됩니다.                                                                                  |

 

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)은 **Property** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

#### <a name="example"></a>예제

다음 예제에서는 세 가지 **속성** 요소를 사용 하는 **EntityType** 요소를 보여 줍니다.

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
 

다음 예제에서는 5 개의 **속성** 요소를 포함 하는 **ComplexType** 요소를 보여 줍니다.

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

**RowType** 요소의 자식인 **속성** 요소는 모델 정의 함수에 전달 되거나 반환 될 수 있는 데이터의 모양 및 특징을 정의 합니다.  

**Property** 요소에는 다음 자식 요소 중 하나만 사용할 수 있습니다.

-   CollectionType
-   ReferenceType
-   RowType

**Property** 요소에는 임의 개수의 자식 주석 요소가 있을 수 있습니다.

> [!NOTE]
> Annotation 요소는 CSDL v2 이상 에서만 사용할 수 있습니다.

 

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Property** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름                                                     | 필수 여부 | 값                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                                                           | 예         | 속성의 이름입니다.                                                                                                                                                                                                       |
| **Type**                                                           | 예         | 속성 값의 형식입니다.                                                                                                                                                                                                 |
| **Null 허용**                                                       | 아니요          | 속성이 null 값을 가질 수 있는지 여부에 따라 **True**(기본값) 또는 **False**입니다. <br/> [!NOTE]                                                                                                                |
| CSDL v1의 > 복합 형식 속성에는 `Nullable="False"`있어야 합니다. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                   | 아니요          | 속성의 기본값입니다.                                                                                                                                                                                              |
| **MaxLength**                                                      | 아니요          | 속성 값의 최대 길이입니다.                                                                                                                                                                                       |
| **FixedLength**                                                    | 아니요          | 속성 값이 고정 길이 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.                                                                                                                          |
| **전체 자릿수**                                                      | 아니요          | 속성 값의 전체 자릿수입니다.                                                                                                                                                                                            |
| **배율**                                                          | 아니요          | 속성 값의 소수 자릿수입니다.                                                                                                                                                                                                |
| **SRID**                                                           | 아니요          | 공간 시스템 참조 식별자입니다. 공간 형식의 속성에 대해서만 유효 합니다. 자세한 내용은 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)를 참조 하세요. |
| **유니코드**                                                        | 아니요          | 속성 값이 유니코드 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.                                                                                                                               |
| **데이터 정렬**                                                      | 아니요          | 데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.                                                                                                                                                   |

 

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)은 **Property** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

#### <a name="example"></a>예제

다음 예에서는 모델 정의 함수의 반환 형식 셰이프를 정의 하는 데 사용 되는 **속성** 요소를 보여 줍니다.

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

CSDL (개념 스키마 정의 언어)의 **Propertyref** 요소는 엔터티 형식의 속성을 참조 하 여 속성이 다음 역할 중 하나를 수행 함을 표시 합니다.

-   엔터티 키(ID를 확인하는 엔터티 형식의 속성 또는 속성 집합)의 일부분. 하나 이상의 **Propertyref** 요소를 사용 하 여 엔터티 키를 정의할 수 있습니다.
-   참조 제약 조건의 종속 또는 주 끝.

**Propertyref** 요소에는 주석 요소 (0 개 이상)만 자식 요소로 포함 될 수 있습니다.

> [!NOTE]
> Annotation 요소는 CSDL v2 이상 에서만 사용할 수 있습니다.

 

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Propertyref** 요소에 적용 될 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                |
|:---------------|:------------|:-------------------------------------|
| **Name**       | 예         | 참조된 속성의 이름입니다. |

 

> [!NOTE]
> 원하는 수의 주석 특성 (사용자 지정 XML 특성)이 **Propertyref** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

아래 예제에서는 엔터티 형식 (**Book**)을 정의 합니다. 엔터티 키는 엔터티 형식의 **ISBN** 속성을 참조 하 여 정의 됩니다.

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
 

다음 예제에서는 두 개의 **Propertyref** 요소를 사용 하 여 두 속성 (**Id** 및 **PublisherId**)이 참조 제약 조건의 주 끝과 종속 end 임을 나타내는 데 사용 됩니다.

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

CSDL (개념 스키마 정의 언어)의 **ReferenceType** 요소는 엔터티 형식에 대 한 참조를 지정 합니다. **ReferenceType** 요소는 다음 요소의 자식일 수 있습니다.

-   ReturnType (함수)
-   매개 변수
-   CollectionType

**ReferenceType** 요소는 함수에 대 한 매개 변수 또는 반환 형식을 정의할 때 사용 됩니다.

**ReferenceType** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **ReferenceType** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                         |
|:---------------|:------------|:----------------------------------------------|
| **Type**       | 예         | 참조되는 엔터티 형식의 이름입니다. |

 

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)이 **ReferenceType** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 **Person** 엔터티 형식에 대 한 참조를 허용 하는 모델 정의 함수에서 **매개 변수** 요소의 자식으로 사용 되는 **ReferenceType** 요소를 보여 줍니다.

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
 

다음 예제에서는 **Person** 엔터티 형식에 대 한 참조를 반환 하는 모델 정의 함수에서 **ReturnType** (function) 요소의 자식으로 사용 되는 **ReferenceType** 요소를 보여 줍니다.

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

CSDL (개념 스키마 정의 언어)의 참조 무결성 제약 **조건** 요소는 관계형 데이터베이스의 참조 무결성 제약 조건과 유사한 기능을 정의 합니다. 데이터베이스 테이블의 열에서 다른 테이블의 기본 키를 참조하는 방식과 동일하게 엔터티 형식의 속성은 다른 엔터티 형식의 엔터티 키를 참조할 수 있습니다. 참조 되는 엔터티 형식을 제약 조건의 *주 끝* 이라고 합니다. 주 끝을 참조 하는 엔터티 형식을 제약 조건의 *종속 끝* 이라고 합니다.

한 엔터티 형식에서 노출 되는 외래 키가 다른 엔터티 형식의 속성을 참조 하는 경우 참조 **는 두** 엔터티 형식 사이의 연결을 정의 합니다. 이 요소는 두 엔터티 형식이 관련 된 방법에 대 한 **정보를 제공** 하므로 MSL (매핑 사양 언어)에는 해당 하는 AssociationSetMapping 요소가 필요 하지 않습니다. 노출 된 외래 키가 없는 두 엔터티 형식 간의 연결에는 연결 정보를 데이터 소스에 매핑하기 위해 해당 **AssociationSetMapping** 요소가 있어야 합니다.

엔터티 형식에서 외래 키가 노출 되지 않은 **경우에는 해당 엔터티** 형식과 다른 엔터티 형식 간에 기본 키-기본 키 제약 조건만 정의할 수 있습니다.

이 **요소는** 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소)
-   보안 주체 (정확히 한 개의 요소)
-   Dependent(정확히 한 개의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

이 **요소에는 원하는** 수의 주석 특성 (사용자 지정 XML 특성)이 있을 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제에서는 **PublishedBy** 연결 정의의 일부로 사용 **되는 설명 요소를** 보여 줍니다.

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
 

 

## <a name="returntype-function-element-csdl"></a>ReturnType (Function) 요소 (CSDL)

CSDL (개념 스키마 정의 언어)의 **ReturnType** (function) 요소는 함수 요소에 정의 된 함수에 대 한 반환 형식을 지정 합니다. 함수 반환 형식은 **ReturnType** 특성과 함께 지정할 수도 있습니다.

반환 형식은 모든 **Edmsimpletype**, 엔터티 형식, 복합 형식, 행 형식, 참조 형식 또는 이러한 형식 중 하나의 컬렉션이 될 수 있습니다.

함수의 반환 형식은 **ReturnType** (function) 요소의 **type** 특성 또는 다음 자식 요소 중 하나를 사용 하 여 지정할 수 있습니다.

-   CollectionType
-   ReferenceType
-   RowType

> [!NOTE]
> **ReturnType** (function) 요소의 **type** 특성과 자식 요소 중 하나를 사용 하 여 함수 반환 형식을 지정 하는 경우 모델은 유효성을 검사 하지 않습니다.

 

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **ReturnType** (Function) 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                              |
|:---------------|:------------|:-----------------------------------|
| **ReturnType** | 아니요          | 함수에서 반환하는 형식입니다. |

 

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)은 **ReturnType** (Function) 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예에서는 **function** 요소를 사용 하 여 책이 인쇄 된 기간 (년)을 반환 하는 함수를 정의 합니다. 반환 형식은 **ReturnType** (Function) 요소의 **type** 특성에 의해 지정 됩니다.

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a>ReturnType (FunctionImport) 요소 (CSDL)

CSDL (개념 스키마 정의 언어)의 **ReturnType** (functionimport) 요소는 FunctionImport 요소에 정의 된 함수에 대 한 반환 형식을 지정 합니다. 함수 반환 형식은 **ReturnType** 특성과 함께 지정할 수도 있습니다.

반환 형식은 엔터티 형식, 복합 형식 또는 **Edmsimpletype**인 모든 컬렉션이 될 수 있습니다.

함수의 반환 형식은 **ReturnType** (FunctionImport) 요소의 **type** 특성을 사용 하 여 지정 됩니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **ReturnType** (FunctionImport) 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**       | 아니요          | 함수에서 반환하는 형식입니다. 값은 ComplexType, EntityType 또는 EDMSimpleType의 컬렉션 이어야 합니다.                                                                                      |
| **EntitySet**  | 아니요          | 함수가 엔터티 형식의 컬렉션을 반환 하는 경우 **EntitySet** 의 값은 컬렉션이 속한 엔터티 집합 이어야 합니다. 그렇지 않으면 **EntitySet** 특성을 사용할 수 없습니다. |

 

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)은 **ReturnType** (FunctionImport) 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예에서는 서적과 게시자를 반환 하는 **FunctionImport** 를 사용 합니다. 함수는 두 개의 결과 집합을 반환 하므로 **ReturnType** (FunctionImport) 요소가 두 개 지정 됩니다.

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a>RowType 요소(CSDL)

CSDL (개념 스키마 정의 언어)의 **RowType** 요소는 명명 되지 않은 구조를 개념적 모델에 정의 된 함수에 대 한 매개 변수 또는 반환 형식으로 정의 합니다.

**RowType** 요소는 다음 요소의 자식일 수 있습니다.

-   CollectionType
-   매개 변수
-   ReturnType (함수)

**RowType** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   Property(하나 이상)
-   Annotation 요소(0개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

**RowType** 요소에는 원하는 수의 주석 특성 (사용자 지정 XML 특성)을 적용할 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예에서는 **CollectionType** 요소를 사용 하 여 함수가 **RowType** 요소에 지정 된 대로 행 컬렉션을 반환 하도록 지정 하는 모델 정의 함수를 보여 줍니다.

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

**Schema** 요소는 개념적 모델 정의의 루트 요소입니다. 이 요소에는 개념적 모델을 구성하는 개체, 함수 및 컨테이너에 대한 정의가 포함되어 있습니다.

**Schema** 요소는 다음과 같은 자식 요소를 0 개 이상 포함할 수 있습니다.

-   Using
-   EntityContainer
-   EntityType
-   EnumType
-   연결
-   ComplexType
-   함수

**Schema** 요소에는 0 개 또는 1 개의 주석 요소가 포함 될 수 있습니다.

> [!NOTE]
> **함수** 요소 및 주석 요소는 CSDL v2 이상 에서만 사용할 수 있습니다.

 

**Schema** 요소는 **네임 스페이스** 특성을 사용 하 여 개념적 모델의 엔터티 형식, 복합 형식 및 연결 개체에 대 한 네임 스페이스를 정의 합니다. 네임스페이스 내에서 두 개체의 이름이 서로 같을 수 없습니다. 네임 스페이스는 여러 **스키마** 요소와 여러 개의 csdl 파일에 걸쳐 있을 수 있습니다.

개념적 모델 네임 스페이스는 **Schema** 요소의 XML 네임 스페이스와 다릅니다. **네임 스페이스** 특성에 정의 된 개념적 모델 네임 스페이스는 엔터티 형식, 복합 형식 및 연결 형식에 대 한 논리적 컨테이너입니다. **Schema 요소의 XML** 네임 스페이스 ( **xmlns** 특성으로 표시 됨)는 **schema** 요소의 자식 요소 및 특성에 대 한 기본 네임 스페이스입니다. https://schemas.microsoft.com/ado/YYYY/MM/edm 형식의 XML 네임 스페이스 (YYYY 및 MM은 각각 연도와 월을 나타냄)는 CSDL 용으로 예약 되어 있습니다. 사용자 지정 요소 및 특성은 이러한 형식의 네임스페이스에 있을 수 없습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Schema** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | 예         | 개념적 모델의 네임스페이스입니다. **네임 스페이스** 특성의 값은 형식의 정규화 된 이름을 구성 하는 데 사용 됩니다. 예를 들어 이름이 *Customer* 인 **entitytype** 이 Simple. Model 네임 스페이스에 있는 경우 **Entitytype** 의 정규화 된 이름은 simpleexamplemodel.customer입니다. <br/> 다음 문자열은 **Namespace** 특성의 값 ( **시스템**, **임시**또는 **Edm**)으로 사용할 수 없습니다. **Namespace** 특성의 값은 SSDL Schema 요소의 **namespace** 특성에 대 한 값과 같을 수 없습니다. |
| **Alias**      | 아니요          | 네임스페이스 이름 대신 사용되는 식별자입니다. 예를 들어, 이름이 *Customer* 인 **EntityType** 이 Simple. model 네임 스페이스에 있고 **Alias** 특성의 값이 *model*인 경우에는 **entitytype** 의 정규화 된 이름으로 모델인를 사용할 수 있습니다.                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)은 **Schema** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 **EntityContainer** 요소, 두 개의 **EntityType** 요소 및 하나의 **Association** 요소를 포함 하는 **스키마** 요소를 보여 줍니다.

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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

CSDL (개념 스키마 정의 언어)의 **TypeRef** 요소는 기존 명명 된 형식에 대 한 참조를 제공 합니다. **TypeRef** 요소는 함수에 컬렉션을 매개 변수 또는 반환 형식으로 지정 하는 데 사용 되는 CollectionType 요소의 자식일 수 있습니다.

**TypeRef** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소)
-   Annotation 요소 (0 개 이상의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **TypeRef** 요소에 적용할 수 있는 특성을 설명 합니다. **DefaultValue**, **MaxLength**, **fixedlength**, **Precision**, **Scale**, **Unicode**및 **Collation** 특성은 **edmsimpletypes**에만 적용 됩니다.

| 특성 이름                                                     | 필수 여부 | 값                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**                                                           | 아니요          | 참조되는 형식의 이름입니다.                                                                                                                                                                                          |
| **Null 허용**                                                       | 아니요          | 속성이 null 값을 가질 수 있는지 여부에 따라 **True**(기본값) 또는 **False**입니다. <br/> [!NOTE]                                                                                                                |
| CSDL v1의 > 복합 형식 속성에는 `Nullable="False"`있어야 합니다. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                   | 아니요          | 속성의 기본값입니다.                                                                                                                                                                                              |
| **MaxLength**                                                      | 아니요          | 속성 값의 최대 길이입니다.                                                                                                                                                                                       |
| **FixedLength**                                                    | 아니요          | 속성 값이 고정 길이 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.                                                                                                                          |
| **전체 자릿수**                                                      | 아니요          | 속성 값의 전체 자릿수입니다.                                                                                                                                                                                            |
| **배율**                                                          | 아니요          | 속성 값의 소수 자릿수입니다.                                                                                                                                                                                                |
| **SRID**                                                           | 아니요          | 공간 시스템 참조 식별자입니다. 공간 형식의 속성에 대해서만 유효 합니다. 자세한 내용은 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)를 참조 하세요. |
| **유니코드**                                                        | 아니요          | 속성 값이 유니코드 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.                                                                                                                               |
| **데이터 정렬**                                                      | 아니요          | 데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.                                                                                                                                                   |

 

> [!NOTE]
> 여러 주석 특성 (사용자 지정 XML 특성)을 **CollectionType** 요소에 적용할 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예에서는 **TypeRef** 요소 ( **CollectionType** 요소의 자식)를 사용 하 여 함수가 **학과** 엔터티 형식의 컬렉션을 허용 하도록 지정 하는 모델 정의 함수를 보여 줍니다.

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

CSDL (개념 스키마 정의 언어)의 **Using** 요소는 다른 네임 스페이스에 있는 개념적 모델의 콘텐츠를 가져옵니다. **네임 스페이스** 특성의 값을 설정 하 여 다른 개념적 모델에 정의 된 엔터티 형식, 복합 형식 및 연결 형식을 참조할 수 있습니다. **Using** 요소가 두 개 이상 있는 경우 **Schema** 요소의 자식일 수 있습니다.

> [!NOTE]
> CSDL의 **using** 요소는 프로그래밍 언어에서 **using** 문과 똑같이 작동 하지 않습니다. 프로그래밍 언어에서 **using 문을 사용** 하 여 네임 스페이스를 가져오면 원래 네임 스페이스의 개체에는 영향을 주지 않습니다. CSDL의 가져온 네임스페이스에는 원래 네임스페이스의 엔터티 형식에서 파생된 엔터티 형식이 포함될 수 있습니다. 이는 원래 네임스페이스에 선언된 엔터티 집합에 영향을 줄 수 있습니다.

 

**Using** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소 허용)
-   Annotation 요소 (0 개 이상의 요소 허용)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Using** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | 예         | 가져온 네임스페이스의 이름입니다.                                                                                                                                                |
| **Alias**      | 예         | 네임스페이스 이름 대신 사용되는 식별자입니다. 이 특성은 필요하지만 개체 이름을 정규화하기 위해 네임스페이스 이름 대신 사용해야 할 필요는 없습니다. |

 

> [!NOTE]
> **Using** 요소에 주석 특성 (사용자 지정 XML 특성)을 여러 개 적용할 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

 

### <a name="example"></a>예제

다음 예제에서는 다른 곳에서 정의 된 네임 스페이스를 가져오는 데 사용 되는 **Using** 요소를 보여 줍니다. 표시 된 **Schema** 요소의 네임 스페이스는 `BooksModel`됩니다. `Publisher`**EntityType** 의 `Address` 속성은 `ExtendedBooksModel` 네임 스페이스 ( **Using** 요소로 가져옴)에 정의 된 복합 형식입니다.

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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

주석 특성을 사용하여 개념적 모델의 요소에 대한 추가 메타데이터를 제공할 수 있습니다. Annotation 요소에 포함 된 메타 데이터는 런타임에 System.object 네임 스페이스의 클래스를 사용 하 여 액세스할 수 있습니다.

### <a name="example"></a>예제

다음 예제에서는 주석 특성 (**CustomAttribute**)이 포함 된 **EntityType** 요소를 보여 줍니다. 이 예제에서는 엔터티 형식 요소에 적용된 Annotation 요소도 보여 줍니다.

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
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

Annotation 요소를 사용하여 개념적 모델의 요소에 대한 추가 메타데이터를 제공할 수 있습니다. .NET Framework 버전 4부터 annotation 요소에 포함 된 메타 데이터는 런타임에 System.object 네임 스페이스의 클래스를 사용 하 여 액세스할 수 있습니다.

### <a name="example"></a>예제

다음 예에서는 annotation 요소 (**customelement**)가 있는 **EntityType** 요소를 보여 줍니다. 다음 예제에서는 엔터티 형식 요소에 적용된 주석 특성도 보여 줍니다.

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
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

CSDL (개념 스키마 정의 언어)은 개념적 모델에서 속성을 정의 하는 **Edmsimpletypes**이라는 추상 기본 데이터 형식 집합을 지원 합니다. **Edmsimpletypes** 은 저장소 또는 호스팅 환경에서 지원 되는 기본 데이터 형식에 대 한 프록시입니다.

다음 표에서는 CSDL에서 지원되는 기본 데이터 형식을 보여 줍니다. 테이블에는 각 **Edmsimpletype**에 적용 될 수 있는 패싯이 나열 됩니다.

| EDMSimpleType                    | 설명                                                | 적용 가능한 패싯                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| **Edm. Binary**                   | 이진 데이터를 포함합니다.                                      | MaxLength, FixedLength, Nullable, Default                                |
| **Edm.Boolean**                  | **True** 또는 **false**값을 포함 합니다.                  | Nullable, Default                                                        |
| **Edm. 바이트**                     | 부호 없는 8비트 정수 값을 포함합니다.                  | Precision, Nullable, Default                                             |
| **Edm. DateTime**                 | 날짜 및 시간을 나타냅니다.                                | Precision, Nullable, Default                                             |
| **Edm.DateTimeOffset**           | 날짜와 시간(GMT 기준 분 단위 오프셋)을 포함합니다. | Precision, Nullable, Default                                             |
| **Edm. Decimal**                  | 고정 전체 자릿수와 소수 자릿수가 있는 숫자 값을 포함합니다.   | Precision, Nullable, Default                                             |
| **Edm.Double**                   | 15 자리의 전체 자릿수를 포함 하는 부동 소수점 숫자를 포함 합니다.   | Precision, Nullable, Default                                             |
| **Edm. Float**                    | 전체 자릿수가 7자리인 부동 소수점 숫자를 포함합니다.   | Precision, Nullable, Default                                             |
| **Edm. Guid**                     | 16바이트 고유 식별자를 포함합니다.                      | Precision, Nullable, Default                                             |
| **Edm. Int16**                    | 부호 있는 16비트 정수 값을 포함합니다.                    | Precision, Nullable, Default                                             |
| **Edm.Int32**                    | 부호 있는 32비트 정수 값을 포함합니다.                    | Precision, Nullable, Default                                             |
| **Edm.Int64**                    | 부호 있는 64비트 정수 값을 포함합니다.                    | Precision, Nullable, Default                                             |
| **Edm. SByte**                    | 부호 있는 8비트 정수 값을 포함합니다.                     | Precision, Nullable, Default                                             |
| **Edm.String**                   | 문자 데이터를 포함합니다.                                   | Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default |
| **Edm. 시간**                     | 시간을 포함합니다.                                    | Precision, Nullable, Default                                             |
| **Edm. 지리**                |                                                            | Nullable, Default, SRID                                                  |
| **Edm.GeographyPoint**           |                                                            | Nullable, Default, SRID                                                  |
| **GeographyLineString**      |                                                            | Nullable, Default, SRID                                                  |
| **GeographyPolygon**         |                                                            | Nullable, Default, SRID                                                  |
| **Edm. GeographyMultiPoint**      |                                                            | Nullable, Default, SRID                                                  |
| **반환할 geographymultilinestring** |                                                            | Nullable, Default, SRID                                                  |
| **Edm. GeographyMultiPolygon**    |                                                            | Nullable, Default, SRID                                                  |
| **Edm .Geographycollection**      |                                                            | Nullable, Default, SRID                                                  |
| **Edm. Geometry**                 |                                                            | Nullable, Default, SRID                                                  |
| **반환할 geometrypoint**            |                                                            | Nullable, Default, SRID                                                  |
| **GeometryLineString**       |                                                            | Nullable, Default, SRID                                                  |
| **GeometryPolygon**          |                                                            | Nullable, Default, SRID                                                  |
| **GeometryMultiPoint**       |                                                            | Nullable, Default, SRID                                                  |
| **GeometryMultiLineString**  |                                                            | Nullable, Default, SRID                                                  |
| **GeometryMultiPolygon**     |                                                            | Nullable, Default, SRID                                                  |
| **GeometryCollection**       |                                                            | Nullable, Default, SRID                                                  |

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
| **ConcurrencyMode** | 속성 값을 낙관적 동시성 검사에 사용하도록 지정합니다.                                                                                                                                                                    | 모든 **Edmsimpletype** 속성                                                                                                                                                                                                                                                                                                                                                     | 아니요                               | 예                 |
| **Shared**         | 인스턴스화 시 값이 제공되지 않는 경우 속성의 기본값을 지정합니다.                                                                                                                                                                       | 모든 **Edmsimpletype** 속성                                                                                                                                                                                                                                                                                                                                                     | 예                              | 예                 |
| **FixedLength**     | 속성 값 길이가 다양할 수 있는지 여부를 지정합니다.                                                                                                                                                                                                  | **Edm. Binary**, **edm. String**                                                                                                                                                                                                                                                                                                                                                       | 예                              | 아니요                  |
| **MaxLength**       | 속성 값의 최대 길이를 지정합니다.                                                                                                                                                                                                           | **Edm. Binary**, **edm. String**                                                                                                                                                                                                                                                                                                                                                       | 예                              | 아니요                  |
| **Null 허용**        | 속성에 **null** 값을 사용할 수 있는지 여부를 지정 합니다.                                                                                                                                                                                                     | 모든 **Edmsimpletype** 속성                                                                                                                                                                                                                                                                                                                                                     | 예                              | 예                 |
| **전체 자릿수**       | **Decimal**형식의 속성에 대해 속성 값에 포함 될 수 있는 자릿수를 지정 합니다. **Time**, **DateTime**및 **DateTimeOffset**형식의 속성에 대해 속성 값의 초 소수 부분 자릿수를 지정 합니다. | **Edm. DateTime**, **edm DateTimeOffset**, **edm. 10**, **edm. 시간**                                                                                                                                                                                                                                                                                                              | 예                              | 아니요                  |
| **배율**           | 속성 값에 대한 소수점 오른쪽의 자릿수를 지정합니다.                                                                                                                                                                      | **Edm. Decimal**                                                                                                                                                                                                                                                                                                                                                                      | 예                              | 아니요                  |
| **SRID**            | 공간 시스템 참조 시스템 ID를 지정 합니다. 자세한 내용은 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)를 참조 하세요.                                                              | **GeographyPoint, GeographyLineString, GeographyPolygon,, Edm. GeographyMultiPoint,, 반환할 geographymultilinestring, Edm. GeographyMultiPolygon, edm. Geographymultipoint, edm. Geometry, 반환할 geometrypoint, GeometryLineString, GeometryPolygon, GeometryMultiPoint, GeometryMultiLineString, GeometryMultiPolygon, GeometryCollection,** | 아니요                               | 예                 |
| **유니코드**         | 속성 값을 유니코드로 저장할지 여부를 나타냅니다.                                                                                                                                                                                                    | **Edm.String**                                                                                                                                                                                                                                                                                                                                                                       | 예                              | 예                 |

>[!NOTE]
> 개념적 모델에서 데이터베이스를 생성 하는 경우 데이터베이스 생성 마법사가 다음 네임 스페이스에 있는 경우 **속성** 요소에 대해 **이 특성 값** 을 인식 합니다. https://schemas.microsoft.com/ado/2009/02/edm/annotation. 특성에 대해 지원 되는 값은 **id** 및 **계산**된 값입니다. **Identity** 값은 데이터베이스에서 생성 된 id 값을 사용 하 여 데이터베이스 열을 생성 합니다. **계산** 된 값은 데이터베이스에서 계산 된 값이 있는 열을 생성 합니다.

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
    xmlns:a="https://schemas.microsoft.com/ado/2009/02/edm/annotation" />
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
