---
title: SSDL 사양-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: 35c560d88e5078a7fc4c07b76020f3ad7d0735e1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995281"
---
# <a name="ssdl-specification"></a>SSDL 사양
SSDL(저장소 스키마 정의 언어)은 Entity Framework 응용 프로그램의 저장소 모델을 설명하는 XML 기반 언어입니다.

Entity Framework 응용 프로그램에서 저장소 모델 메타 데이터는 System.Data.Metadata.Edm.StoreItemCollection 인스턴스로 (SSDL로 작성).ssdl 파일에서 로드 되 고에서 메서드를 사용 하 여 액세스할 수 합니다 System.Data.Metadata.Edm.MetadataWorkspace 클래스입니다. Entity Framework 저장소 모델 메타 데이터를 사용 하 여 저장소 관련 명령 개념적 모델에 대 한 쿼리를 변환 합니다.

Entity Framework Designer (EF 디자이너)는 디자인 타임에는.edmx 파일의 저장소 모델 정보를 저장합니다. 빌드 시 Entity Designer는.edmx 파일의 정보는 필요한 Entity Framework에서 런타임에.ssdl 파일을 만들려면 사용 합니다.

SSDL 버전은 XML 네임스페이스로 식별됩니다.

| SSDL 버전 | XML Namespace                                     |
|:-------------|:--------------------------------------------------|
| SSDL v1      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| SSDL v2      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| SSDL v3      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a>Association 요소(SSDL)

**연결** 요소 (SSDL) 저장소 스키마 정의 언어에서 기본 데이터베이스의 외래 키 제약 조건에 참여 하는 테이블 열을 지정 합니다. 필수 자식 End 요소가 두 연결의 end에 있는 테이블과 각 끝의 복합성을 지정합니다. 선택적 ReferentialConstraint 요소 참여 하는 열 뿐만 아니라 연결의 주 끝 및 종속 끝을 지정합니다. 없으면 **ReferentialConstraint** 요소가, AssociationSetMapping 요소를 사용 하 여 연결에 대 한 열 매핑을 지정 해야 합니다.

합니다 **연결** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 또는 1)
-   끝 (정확히 2 개)
-   ReferentialConstraint (0 또는 1)
-   Annotation 요소 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **연결** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| **이름**       | 예         | 기본 데이터베이스에서 해당하는 외래 키 제약 조건의 이름 |

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **연결** 요소입니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제와 **연결** 사용 하는 요소를 **ReferentialConstraint** 참여 하는 열을 지정 하는 요소는 **FK\_CustomerOrders**  foreign key 제약 조건:

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="associationset-element-ssdl"></a>AssociationSet 요소(SSDL)

합니다 **AssociationSet** 요소 (SSDL) 저장소 스키마 정의 언어에 기본 데이터베이스에서 두 테이블 간의 외래 키 제약 조건을 나타냅니다. 외래 키 제약 조건에 관여 하는 테이블 열을 Association 요소에 지정 됩니다. **연결** 해당 하는 요소를 지정 **AssociationSet** 요소에 지정 된를 **연결** 특성을 **AssociationSet**  요소입니다.

SSDL 연결 집합 AssociationSetMapping 요소 CSDL 연결 집합에 매핑됩니다. 그러나 지정 된 CSDL 연결에 대 한 CSDL 연결 집합 정의 된 경우 해당 ReferentialConstraint 요소를 사용 하 여 **AssociationSetMapping** 요소가 필요 합니다. 이 경우 경우는 **AssociationSetMapping** 요소가 있는지를 정의 하는 매핑이 재정의 합니다 **ReferentialConstraint** 요소입니다.

합니다 **AssociationSet** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 또는 1)
-   끝 (0 개 또는 2)
-   Annotation 요소 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **AssociationSet** 요소입니다.

| 특성 이름  | 필수 여부 | 값                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| **이름**        | 예         | 연결 집합이 나타내는 외래 키 제약 조건의 이름                          |
| **연결** | 예         | 외래 키 제약 조건에 참여하는 열을 정의하는 연결의 이름 |

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **AssociationSet** 요소입니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

에서는 다음 예제는 **AssociationSet** 나타내는 요소는 `FK_CustomerOrders` 기본 데이터베이스의 외래 키 제약 조건:

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a>CollectionType 요소 (SSDL)

합니다 **CollectionType** 요소 (SSDL) 저장소 스키마 정의 언어에서 함수의 반환 형식이 컬렉션 임을 지정 합니다. 합니다 **CollectionType** ReturnType 요소의 자식 요소입니다. 컬렉션 형식의 RowType 자식 요소를 사용 하 여 지정 됩니다.

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **CollectionType** 요소입니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제에서는 사용 하는 함수를 **CollectionType** 함수 행 컬렉션을 반환 하는지 지정 하는 요소입니다.

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="commandtext-element-ssdl"></a>CommandText 요소(SSDL)

합니다 **CommandText** 요소 (SSDL) 저장소 스키마 정의 언어에서는 데이터베이스에서 실행 되는 SQL 문을 정의할 수 있는 Function 요소의 자식입니다. 합니다 **CommandText** 요소를 사용 하면 데이터베이스에서 저장된 프로시저에 유사한 기능을 추가할 수 있지만 정의 **CommandText** 저장소 모델의 요소입니다.

합니다 **CommandText** 요소는 자식 요소를 포함할 수 없습니다. 본문을 **CommandText** 요소는 기본 데이터베이스에 대 한 유효한 SQL 문 이어야 합니다.

적용할 수 없는 특성을 **CommandText** 요소입니다.

### <a name="example"></a>예제

에서는 다음 예제는 **함수** 자식 요소 **CommandText** 요소입니다. 노출 된 **UpdateProductInOrder** ObjectContext의 메서드를 개념적 모델로 가져와서 역할도 합니다.  

``` xml
 <Function Name="UpdateProductInOrder" IsComposable="false">
   <CommandText>
     UPDATE Orders
     SET ProductId = @productId
     WHERE OrderId = @orderId;
   </CommandText>
   <Parameter Name="productId"
              Mode="In"
              Type="int"/>
   <Parameter Name="orderId"
              Mode="In"
              Type="int"/>
 </Function>
```

## <a name="definingquery-element-ssdl"></a>DefiningQuery 요소(SSDL)

합니다 **DefiningQuery** 요소 (SSDL) 저장소 스키마 정의 언어에서를 사용 하면 기본 데이터베이스에서 직접 SQL 문을 실행할 수 있습니다. 합니다 **DefiningQuery** 요소는 일반적으로 데이터베이스 뷰처럼 사용 되지만 뷰는 데이터베이스 대신 저장소 모델에서 정의 됩니다. 에 정의 된 뷰를 **DefiningQuery** 요소 EntitySetMapping 요소를 통해 개념적 모델의 엔터티 형식에 매핑할 수 있습니다. 이러한 매핑은 읽기 전용입니다.  

다음 SSDL 구문의 선언을 보여 줍니다.는 **EntitySet** 뒤에 **DefiningQuery** 에 뷰를 검색 하는 데 사용 하는 쿼리를 포함 하는 요소입니다.

``` xml
 <Schema>
     <EntitySet Name="Tables" EntityType="Self.STable">
         <DefiningQuery>
           SELECT  TABLE_CATALOG,
                   'test' as TABLE_SCHEMA,
                   TABLE_NAME
           FROM    INFORMATION_SCHEMA.TABLES
         </DefiningQuery>
     </EntitySet>
 </Schema>
```

뷰를 통한 읽기 / 쓰기 시나리오를 사용 하도록 설정 하려면 Entity Framework에서 저장된 프로시저를 사용할 수 있습니다. 데이터를 검색 하 고 저장된 프로시저에서 처리 하는 변경에 대 한 기본 테이블로 데이터 원본 뷰 또는 Entity SQL 뷰를 사용할 수 있습니다.

사용할 수는 **DefiningQuery** Microsoft SQL Server Compact 3.5를 대상으로 하는 요소입니다. SQL Server Compact 3.5는 저장된 프로시저를 지원 하지 않습니다, 유사한 기능을 구현할 수 있습니다 합니다 **DefiningQuery** 요소입니다. 이 요소는 프로그래밍 언어에 사용되는 데이터 형식과 데이터 소스의 데이터 형식 간에 불일치를 해결하기 위한 저장 프로시저를 만드는 데도 유용하게 사용할 수 있습니다. 작성할 수는 **DefiningQuery** 매개 변수는 특정 집합을 사용 하 고 다음 데이터를 삭제 하는 저장된 프로시저 예를 들어, 매개 변수를 다른 집합을 사용 하 여 저장된 프로시저를 호출 합니다.

## <a name="dependent-element-ssdl"></a>Dependent 요소(SSDL)

합니다 **종속** 저장소 스키마 정의 언어 (SSDL)에 참조 제약 조건이 라고도 하는 외래 키 제약 조건의 종속 끝을 정의 하는 ReferentialConstraint 요소에 자식 요소입니다. 합니다 **종속** 요소는 기본 키 열 (또는 열)을 참조 하는 테이블의 열 (또는 열)을 지정 합니다. **PropertyRef** 요소 참조 되는 열을 지정 합니다. 보안 주체에 지정 된 열에서 참조 하는 기본 키 열을 지정 하는 요소는 **종속** 요소입니다.

합니다 **종속** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   PropertyRef (하나 이상)
-   Annotation 요소 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **종속** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **역할**       | 예         | 같은 값을 **역할** (사용) 하는 경우 해당 End 요소의 특성, 그렇지 않으면 테이블의 이름을 포함 하는 참조 하는 열입니다. |

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **종속** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제에서는 사용 하는 연결 요소를 **ReferentialConstraint** 참여 하는 열을 지정 하는 요소는 **FK\_CustomerOrders** 외래 키 제약 조건입니다. **종속** 요소를 지정 합니다 **CustomerId** 열의 합니다 **순서** 테이블 제약 조건의 종속 끝으로.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="documentation-element-ssdl"></a>Documentation 요소(SSDL)

합니다 **설명서** 부모 요소에 정의 된 개체에 대 한 정보를 제공 하는 저장소 스키마 정의 언어 (SSDL)에 요소를 사용할 수 있습니다.

합니다 **설명서** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   **요약**: 부모 요소에 대 한 간략 한 설명입니다. (0개 또는 한 개의 요소)
-   **LongDescription**: 부모 요소에 대 한 자세한 설명입니다. (0개 또는 한 개의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **설명서** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제는 **설명서** EntityType 요소의 자식 요소로 요소입니다.

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="end-element-ssdl"></a>End 요소(SSDL)

합니다 **최종** 기본 데이터베이스의 테이블과 외래 키 제약 조건의 한 end에 있는 행의 수를 지정 하는 저장소 스키마 정의 언어 (SSDL)에 요소입니다. 합니다 **최종** Association 요소 또는 AssociationSet 요소의 자식 요소일 수 있습니다. 각 경우에 가능한 자식 요소와 적용 가능한 특성은 서로 다릅니다.

### <a name="end-element-as-a-child-of-the-association-element"></a>Association 요소의 자식인 End 요소

**끝** 요소 (자식으로는 **연결** 요소) 테이블 및 사용 하 여 외래 키 제약 조건의 end에 있는 행의 수를 지정 합니다 **형식** 및 **복합성** 각각 특성입니다. 외래 키 제약 조건의 End는 SSDL 연결의 일부로 정의되며 SSDL 연결에는 정확히 두 개의 End가 있어야 합니다.

**최종** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소)
-   OnDelete (0 개 이상의 요소)
-   Annotation 요소 (0 개 이상의 요소)

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 특성을 적용할 수 있습니다는 **끝** 의 자식인 경우 요소는 **연결** 요소입니다.

| 특성 이름   | 필수 여부 | 값                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**         | 예         | 외래 키 제약 조건의 end에 있는 SSDL 엔터티 집합의 정규화 된 이름입니다.                                                                                                                                                                                                                                                                                          |
| **역할**         | 아니요          | 값을 **역할** (사용) 하는 경우 해당 ReferentialConstraint 요소 또는 종속 요소의 특성입니다.                                                                                                                                                                                                                                             |
| **다중성** | 예         | **1**하십시오 **0..1**, 또는 **\*** 외래 키 제약 조건의 end에 있을 수 있는 행의 수에 따라 합니다. <br/> **1** 외래 키 제약 조건 end에는 정확히 하나의 행이 있음을 나타냅니다. <br/> **0..1** 나타내는 외래 키 제약 조건 end에 0 개 이상의 행이 존재 합니다. <br/> **\*** 외래 키 제약 조건 end에 0, 1 또는 행이 더 있는지를 나타냅니다. |

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **최종** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

#### <a name="example"></a>예제

에서는 다음 예제는 **연결** 정의 하는 요소는 **FK\_CustomerOrders** foreign key 제약 조건. **복합성** 각각에 지정 된 값 **끝** 요소에는 많은 행을 나타내는 **주문** 테이블의 행과 연결할 수 있습니다는 **고객**  테이블에 있지만 하나의 행에는 **고객** 테이블의 행과 연결할 수 있습니다는 **주문** 테이블. 또한 합니다 **OnDelete** 요소에서 모든 행을 나타냅니다는 **주문** 테이블에서 특정 행을 참조 하는 **고객** 경우 테이블은 삭제 됩니다의 행을 합니다 **고객** 테이블이 삭제 됩니다.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

### <a name="end-element-as-a-child-of-the-associationset-element"></a>AssociationSet 요소의 자식인 End 요소

합니다 **끝** 요소 (자식으로는 **AssociationSet** 요소) 기본 데이터베이스의 외래 키 제약 조건의 한 end에 있는 테이블을 지정 합니다.

**최종** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 또는 1)
-   Annotation 요소 (0 개 이상)

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 특성을 적용할 수 있습니다는 **끝** 의 자식인 경우 요소는 **AssociationSet** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| **EntitySet**  | 예         | 외래 키 제약 조건의 end에 있는 SSDL 엔터티 집합의 이름입니다.                                      |
| **역할**       | 아니요          | 값 중 하나는 **역할** 하나에 지정 된 특성 **끝** 의 해당 Association 요소는 요소입니다. |

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **최종** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

#### <a name="example"></a>예제

에서는 다음 예제는 **EntityContainer** 요소를 **AssociationSet** 요소 두 개가 **끝** 요소:

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitycontainer-element-ssdl"></a>EntityContainer 요소(SSDL)

**EntityContainer** 요소 (SSDL) 저장소 스키마 정의 언어에서 Entity Framework 응용 프로그램에서 기본 데이터 원본의 구조를 설명 합니다: SSDL 엔터티 집합 (EntitySet 요소에 정의 됨)에서 테이블을 나타내는 데이터베이스에 SSDL 엔터티 형식 (EntityType 요소에 정의 됨) 테이블의 행을 나타내고 연결 집합 (AssociationSet 요소에 정의 됨) 데이터베이스에서 foreign key 제약 조건입니다. EntityContainerMapping 요소를 통해 개념적 모델 엔터티 컨테이너를 저장소 모델 엔터티 컨테이너를 매핑합니다.

**EntityContainer** 요소 0 개 이상의 Documentation 요소를 포함할 수 있습니다. 경우는 **설명서** 요소가 있는지, 다른 모든 자식 요소 앞에 야 합니다.

**EntityContainer** 요소 (나열 된 순서로) 0 개 이상의 자식 요소를 포함할 수 있습니다.

-   EntitySet
-   AssociationSet
-   Annotation 요소

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **EntityContainer** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| **이름**       | 예         | 엔터티 컨테이너의 이름입니다. 이 이름에는 마침표(.)가 포함될 수 없습니다. |

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **EntityContainer** 요소입니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제는 **EntityContainer** 두 엔터티 집합 및 하나의 연결 집합을 정의 하는 요소입니다. 엔터티 형식 및 연결 형식 이름은 개념적 모델 네임스페이스 이름으로 정규화됩니다.

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entityset-element-ssdl"></a>EntitySet 요소(SSDL)

**EntitySet** 테이블 또는 뷰의 원본 데이터베이스의 저장소 스키마 정의 언어 (SSDL) 요소를 나타냅니다. SSDL에서 EntityType 요소는 테이블 또는 뷰의 행을 나타냅니다. **EntityType** 특성을 **EntitySet** 요소는 SSDL 엔터티 집합의 행을 나타내는 특정 SSDL 엔터티 형식을 지정 합니다. EntitySetMapping 요소는 CSDL 엔터티 집합 및 SSDL 엔터티 집합 간의 매핑을 지정 됩니다.

합니다 **EntitySet** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소)
-   DefiningQuery (0 개 이상의 요소)
-   Annotation 요소

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **EntitySet** 요소입니다.

> [!NOTE]
> (여기에 나열 되지) 일부 특성을 사용 하 여 정규화 할 수 있습니다 합니다 **저장할** 별칭입니다. 이러한 특성은 모델을 업데이트 하는 경우 모델 업데이트 마법사에서 사용 됩니다.

| 특성 이름 | 필수 여부 | 값                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **이름**       | 예         | 엔터티 집합의 이름입니다.                                                              |
| **EntityType** | 예         | 엔터티 집합에 포함되는 인스턴스의 엔터티 형식에 대한 정규화된 이름입니다. |
| **스키마**     | 아니요          | 데이터베이스 스키마입니다.                                                                     |
| **Table**      | 아니요          | 데이터베이스 테이블입니다.                                                                      |

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **EntitySet** 요소입니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

에서는 다음 예제는 **EntityContainer** 를 가진 두 요소가 **EntitySet** 요소와 하나 **AssociationSet** 요소:

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitytype-element-ssdl"></a>EntityType 요소(SSDL)

**EntityType** 요소 (SSDL) 저장소 스키마 정의 언어에서 기본 데이터베이스의 뷰나 테이블의 행을 나타냅니다. 테이블 또는 뷰의 행 발생 하는 SSDL의 EntitySet 요소를 나타냅니다. **EntityType** 특성을 **EntitySet** 요소는 SSDL 엔터티 집합의 행을 나타내는 특정 SSDL 엔터티 형식을 지정 합니다. SSDL 엔터티 형식과 CSDL 엔터티 형식 간의 매핑은 EntityTypeMapping 요소에 지정 됩니다.

합니다 **EntityType** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 개 이상의 요소)
-   키 (0 개 이상의 요소)
-   Annotation 요소

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **EntityType** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**       | 예         | 엔터티 형식의 이름. 이 값은 일반적으로 엔터티 형식이 나타내는 행이 있는 테이블의 이름과 동일합니다. 이 값에는 마침표(.)가 포함될 수 없습니다. |

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **EntityType** 요소입니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제는 **EntityType** 두 속성이 있는 요소:

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="function-element-ssdl"></a>Function 요소(SSDL)

합니다 **함수** 요소 (SSDL) 저장소 스키마 정의 언어에서 기본 데이터베이스에 존재 하는 저장된 프로시저를 지정 합니다.

합니다 **함수** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 또는 1)
-   매개 변수 (0 개 이상)
-   CommandText (0 또는 1)
-   ReturnType (0 개 이상)
-   Annotation 요소 (0 개 이상)

반환 함수에 대 한 형식을 사용 하 여 지정 해야 합니다 **ReturnType** 요소 또는 **ReturnType** 특성 (아래 참조) 중 하나만 있습니다.

저장소 모델에 지정된 저장 프로시저를 응용 프로그램의 개념적 모델로 가져올 수 있습니다. 자세한 내용은 [저장 프로시저를 사용 하 여 쿼리](~/ef6/modeling/designer/stored-procedures/query.md)합니다. 합니다 **함수** 요소는 저장소 모델에서 사용자 지정 함수 정의를 사용할 수도 있습니다.  

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **함수** 요소입니다.

> [!NOTE]
> (여기에 나열 되지) 일부 특성을 사용 하 여 정규화 할 수 있습니다 합니다 **저장할** 별칭입니다. 이러한 특성은 모델을 업데이트 하는 경우 모델 업데이트 마법사에서 사용 됩니다.

| 특성 이름             | 필수 여부 | 값                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**                   | 예         | 저장 프로시저의 이름입니다.                                                                                                                                                                                  |
| **ReturnType**             | 아니요          | 저장 프로시저의 반환 형식입니다.                                                                                                                                                                           |
| **Aggregate**              | 아니요          | **True 이면** 저장된 프로시저는 집계 값을 반환 하는 경우이 고, 그렇지 **False**합니다.                                                                                                                                  |
| **기본 제공**                | 아니요          | **True** 함수는 기본 제공 하는 경우<sup>1</sup> 함수를 그렇지 않으면 **False**합니다.                                                                                                                                  |
| **StoreFunctionName**      | 아니요          | 저장 프로시저의 이름입니다.                                                                                                                                                                                  |
| **NiladicFunction**        | 아니요          | **True 이면** 함수는 무항 이면<sup>2</sup> ; 함수 **False** 그렇지 않은 경우.                                                                                                                                   |
| **IsComposable**           | 아니요          | **True** 기본 구성 가능 함수 이면<sup>3</sup> ; 함수 **False** 그렇지 않은 경우.                                                                                                                                |
| **ParameterTypeSemantics** | 아니요          | 함수 오버로드를 확인하는 데 사용되는 형식 의미 체계를 정의하는 열거형입니다. 열거형은 함수 정의별로 공급자 매니페스트에 정의됩니다. 기본값은 **AllowImplicitConversion**합니다. |
| **스키마**                 | 아니요          | 저장 프로시저가 정의된 스키마의 이름입니다.                                                                                                                                                   |

<sup>1</sup> 기본 제공 함수는 데이터베이스에 정의 된 함수입니다. 저장소 모델에 정의 된 함수에 대 한 내용은 CommandText 요소 (SSDL)을 참조 하세요.

<sup>2</sup> 무항 함수는 매개 변수를 수락 하 고 호출 하는 경우 괄호가 필요 하지 않습니다는 함수입니다.

<sup>3</sup> 두 함수는 한 함수의 출력이 다른 함수의 입력 수 있으면 구성 가능 합니다.

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **함수** 요소입니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

에서는 다음 예제는 **함수** 해당 하는 요소는 **UpdateOrderQuantity** 저장 프로시저. 저장 프로시저는 두 매개 변수를 받아들이며 값을 반환하지 않습니다.

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="key-element-ssdl"></a>Key 요소(SSDL)

합니다 **키** 요소 (SSDL) 저장소 스키마 정의 언어에서 기본 데이터베이스에서 테이블의 기본 키를 나타냅니다. **키** 테이블의 행을 나타내는 EntityType 요소의 자식 요소입니다. 에 기본 키가 정의 합니다 **키** 에 정의 된 하나 이상의 속성 요소를 참조 하 여 요소를 **EntityType** 요소입니다.

합니다 **키** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   PropertyRef (하나 이상)
-   Annotation 요소

적용할 수 없는 특성을 **키** 요소입니다.

### <a name="example"></a>예제

다음 예제는 **EntityType** 요소 하나의 속성을 참조 하는 키를 사용 하 여:

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="ondelete-element-ssdl"></a>OnDelete 요소(SSDL)

합니다 **OnDelete** 요소 (SSDL) 저장소 스키마 정의 언어에서 외래 키 제약 조건에 관여 하는 행이 삭제 될 때 데이터베이스 동작을 나타냅니다. 작업 설정 된 경우 **Cascade**, 삭제 되는 행을 참조 하는 행도 삭제 됩니다. 작업 설정 된 경우 **None**, 후 삭제 되는 행을 참조 하는 행도 삭제 되지 않습니다. **OnDelete** 최종 요소의 자식 요소입니다.

**OnDelete** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 또는 1)
-   Annotation 요소 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **OnDelete** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| **작업**     | 예         | **Cascade** 나 **None**합니다. (값 **Restricted** 유효 하지만 동일한 동작 **None**.) |

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **OnDelete** 요소입니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

에서는 다음 예제는 **연결** 정의 하는 요소는 **FK\_CustomerOrders** foreign key 제약 조건. **OnDelete** 요소에서 모든 행을 나타냅니다는 **주문** 테이블에서 특정 행을 참조 하는 합니다 **고객** 경우 테이블은 삭제 됩니다 합니다 행**고객** 테이블이 삭제 됩니다.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="parameter-element-ssdl"></a>Parameter 요소(SSDL)

합니다 **매개 변수** (SSDL) 저장소 스키마 정의 언어에서 요소는 데이터베이스의 저장된 프로시저에 대 한 매개 변수를 지정 하는 Function 요소의 자식입니다.

합니다 **매개 변수** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 또는 1)
-   Annotation 요소 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **매개 변수** 요소.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**       | 예         | 매개 변수의 이름입니다.                                                                                                                                                                                                      |
| **Type**       | 예         | 매개 변수 형식입니다.                                                                                                                                                                                                             |
| **모드**       | 아니요          | **In**, **초과**, 또는 **InOut** 매개 변수는 입력, 출력 또는 입출력 매개 변수 인지에 따라 합니다.                                                                                                                |
| **MaxLength**  | 아니요          | 매개 변수의 최대 길이입니다.                                                                                                                                                                                            |
| **전체 자릿수**  | 아니요          | 매개 변수의 전체 자릿수입니다.                                                                                                                                                                                                 |
| **배율**      | 아니요          | 매개 변수의 소수 자릿수입니다.                                                                                                                                                                                                     |
| **SRID**       | 아니요          | 시스템 공간 참조 식별자입니다. 공간 형식 매개 변수에 유효 합니다. 자세한 내용은 [SRID](http://en.wikipedia.org/wiki/SRID) 하 고 [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)합니다. |

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **매개 변수** 요소입니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

에서는 다음 예제는 **함수** 를 가진 두 요소가 **매개 변수** 입력된 매개 변수를 지정 하는 요소:

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="principal-element-ssdl"></a>Principal 요소(SSDL)

합니다 **주** 저장소 스키마 정의 언어 (SSDL)에 참조 제약 조건이 라고도 하는 외래 키 제약 조건의 주 끝을 정의 하는 ReferentialConstraint 요소에 자식 요소입니다. 합니다 **주** 다른 열 (또는 열)에서 참조 되는 테이블의 기본 키 열 (또는 열)를 지정 하는 요소입니다. **PropertyRef** 요소 참조 되는 열을 지정 합니다. Dependent 요소에 지정 된 기본 키 열을 참조 하는 열을 지정 합니다 **주** 요소입니다.

합니다 **주** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.

-   PropertyRef (하나 이상)
-   Annotation 요소 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **주** 요소.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **역할**       | 예         | 같은 값을 **역할** (사용) 하는 경우 해당 End 요소의 특성, 그렇지 않으면 테이블의 이름을 포함 하는 참조 된 열입니다. |

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **주** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제에서는 사용 하는 연결 요소를 **ReferentialConstraint** 참여 하는 열을 지정 하는 요소는 **FK\_CustomerOrders** 외래 키 제약 조건입니다. **주** 요소를 지정 합니다 **CustomerId** 열의 **고객** 테이블 제약 조건의 주 끝으로 합니다.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="property-element-ssdl"></a>Property 요소(SSDL)

합니다 **속성** 요소 (SSDL) 저장소 스키마 정의 언어에서 기본 데이터베이스의 테이블에 열을 나타냅니다. **속성** 요소가 테이블의 행을 나타내는 EntityType 요소의 자식입니다. 각 **속성** 에 정의 된 요소를 **EntityType** 요소는 열을 나타냅니다.

A **속성** 요소는 모든 자식 요소를 포함할 수 없습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서 설명에 적용할 수 있는 특성을 **속성** 요소입니다.

| 특성 이름            | 필수 여부 | 값                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**                  | 예         | 해당 열의 이름입니다.                                                                                                                                                                                           |
| **Type**                  | 예         | 해당 열의 형식입니다.                                                                                                                                                                                           |
| **Null 허용**              | 아니요          | **True 이면** (기본값) 또는 **False** 해당 열에 null 값을 사용할 수 있는지 여부에 따라 합니다.                                                                                                                  |
| **DefaultValue**          | 아니요          | 해당 열의 기본값입니다.                                                                                                                                                                                  |
| **MaxLength**             | 아니요          | 해당 열의 최대 길이입니다.                                                                                                                                                                                 |
| **FixedLength**           | 아니요          | **True 이면** 나 **False** 고정된 길이 문자열로 해당 열 값이 저장될지 여부에 따라 합니다.                                                                                                              |
| **전체 자릿수**             | 아니요          | 해당 열의 전체 자릿수입니다.                                                                                                                                                                                      |
| **배율**                 | 아니요          | 해당 열의 소수 자릿수입니다.                                                                                                                                                                                          |
| **유니코드**               | 아니요          | **True 이면** 나 **False** 유니코드 문자열로 해당 열 값이 저장될지 여부에 따라 합니다.                                                                                                                   |
| **데이터 정렬**             | 아니요          | 데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.                                                                                                                                                   |
| **SRID**                  | 아니요          | 시스템 공간 참조 식별자입니다. 공간 형식의 속성에 대해서만 유효 합니다. 자세한 내용은 [SRID](http://en.wikipedia.org/wiki/SRID) 하 고 [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)합니다. |
| **StoreGeneratedPattern** | 아니요          | **None**, **Identity** (하는 경우 해당 열 값이 데이터베이스에서 생성 된 id), 또는 **계산 됨** (하는 경우 해당 열 값은 데이터베이스에서 계산). RowType 속성에 대해 유효 하지 않습니다. |

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **속성** 요소입니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제는 **EntityType** 두 개의 자식 **속성** 요소:

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="propertyref-element-ssdl"></a>PropertyRef 요소(SSDL)

합니다 **PropertyRef** 요소 (SSDL) 저장소 스키마 정의 언어에서를 나타내는 속성이 다음 역할 중 하나가 수행 되는 EntityType 요소에 정의 된 속성을 참조 합니다.

-   테이블의 기본 키의 일부 여야 하는 **EntityType** 나타냅니다. 하나 이상의 **PropertyRef** 기본 키를 정의 하는 요소를 사용할 수 있습니다. 자세한 내용은 키 요소를 참조 하세요.
-   참조 제약 조건의 종속 또는 주 끝. 자세한 내용은 ReferentialConstraint 요소를 참조 하세요.

합니다 **PropertyRef** 요소는 다음과 같은 자식 요소를 하나만 사용할 수 있습니다.

-   설명서 (0 또는 1)
-   Annotation 요소

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 설명에 적용할 수 있는 특성을 **PropertyRef** 요소입니다.

| 특성 이름 | 필수 여부 | 값                                |
|:---------------|:------------|:-------------------------------------|
| **이름**       | 예         | 참조된 속성의 이름입니다. |

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **PropertyRef** 요소입니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

에서는 다음 예제는 **PropertyRef** 에 정의 된 속성을 참조 하 여 기본 키를 정의 하는 데 사용 되는 요소는 **EntityType** 요소.

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="referentialconstraint-element-ssdl"></a>ReferentialConstraint 요소(SSDL)

합니다 **ReferentialConstraint** 요소 (SSDL) 저장소 스키마 정의 언어에 기본 데이터베이스에는 참조 무결성 제약 조건이 라고도 하는 외래 키 제약을 나타냅니다. 제약 조건의 주 끝 및 종속 끝 각각 보안 주체 및 종속 자식 요소에 의해 지정 됩니다. 주 끝과 종속 끝에 참여 하는 열은 PropertyRef 요소를 사용 하 여 참조 됩니다.

합니다 **ReferentialConstraint** 요소는 Association 요소의 선택적 자식 요소입니다. 경우는 **ReferentialConstraint** 요소에 지정 된 외래 키 제약 조건에 매핑할 사용 하지 않으면 합니다 **연결** 이 작업을 수행 하려면 요소를 사용 해야 하는 AssociationSetMapping 요소입니다.

합니다 **ReferentialConstraint** 요소는 다음 자식 요소를 포함할 수 있습니다.

-   설명서 (0 또는 1)
-   보안 주체 (1 개만)
-   종속 (1 개만)
-   Annotation 요소 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **ReferentialConstraint** 요소입니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제와 **연결** 사용 하는 요소를 **ReferentialConstraint** 참여 하는 열을 지정 하는 요소는 **FK\_CustomerOrders**  foreign key 제약 조건:

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="returntype-element-ssdl"></a>ReturnType 요소 (SSDL)

합니다 **ReturnType** (SSDL) 저장소 스키마 정의 언어에서 요소에 정의 된 함수에 대 한 반환 형식을 지정 하는 **함수** 요소입니다. 함수 반환 형식으로 지정할 수도 있습니다는 **ReturnType** 특성입니다.

함수의 반환 형식이 지정 된 된 **형식** 특성 또는 **ReturnType** 요소입니다.

합니다 **ReturnType** 요소는 다음 자식 요소를 포함할 수 있습니다.

- CollectionType (1 개)  

> [!NOTE]
> 주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **ReturnType** 요소입니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예에서는 **함수** 는 행의 컬렉션을 반환 합니다.

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```


## <a name="rowtype-element-ssdl"></a>RowType 요소 (SSDL)

A **RowType** 반환는 명명 되지 않은 구조를 정의 하는 저장소 스키마 정의 언어 (SSDL)의 요소 형식 저장소에 정의 된 함수에 대 한 합니다.

A **RowType** 요소는 자식 요소입니다 **CollectionType** 요소:

A **RowType** 요소는 다음 자식 요소를 포함할 수 있습니다.

- 속성 (하나 이상)  

### <a name="example"></a>예제

다음 예제에서는 사용 하는 저장소 함수를 **CollectionType** 는 반환 행 컬렉션을 지정 하는 요소 (에 지정 된 대로 합니다 **RowType** 요소).


``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="schema-element-ssdl"></a>스키마 요소(SSDL)

합니다 **스키마** (SSDL) 저장소 스키마 정의 언어에서 요소는 저장소 모델 정의의 루트 요소입니다. 이 요소에는 저장소 모델을 구성하는 개체, 함수 및 컨테이너에 대한 정의가 포함되어 있습니다.

합니다 **스키마** 요소 0 개 이상의 자식 요소를 포함할 수 있습니다.

-   형식 연결
-   EntityType
-   EntityContainer
-   기능

합니다 **스키마** 요소를 사용 합니다 **Namespace** 저장소 모델의 엔터티 형식 및 연결 개체에 대 한 네임 스페이스를 정의 하는 특성입니다. 네임스페이스 내에서 두 개체의 이름이 서로 같을 수 없습니다.

저장소 모델 네임 스페이스의 XML 네임 스페이스와에서 다릅니다. 합니다 **스키마** 요소입니다. 저장소 모델 네임 스페이스 (정의 된 대로 합니다 **Namespace** 특성)은 엔터티 형식 및 연결 형식에 대 한 논리적 컨테이너입니다. XML 네임 스페이스 (나타난 합니다 **xmlns** 특성)의 **스키마** 요소는 자식 요소 및 특성에 대 한 기본 네임 스페이스를 **스키마** 요소입니다. 폼의 XML 네임 스페이스 http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (여기서 YYYY, MM은 연도 및 월 각각)는 SSDL 용으로 예약 되어 있습니다. 사용자 지정 요소 및 특성은 이러한 형식의 네임스페이스에 있을 수 없습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

아래 표에서 특성을 설명에 적용할 수는 **스키마** 요소입니다.

| 특성 이름            | 필수 여부 | 값                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **네임스페이스**             | 예         | 저장소 모델의 네임스페이스입니다. 값을 **Namespace** 특성 형식의 정규화 된 이름을 만드는 데 사용 됩니다. 예를 들어 경우는 **EntityType** 라는 *고객* ExampleModel.Store 네임 스페이스의 정규화 된 이름을를 **EntityType** 는 ExampleModel.Store.Customer입니다. <br/> 다음 문자열 값으로 사용할 수 없습니다는 **Namespace** 특성: **System**를 **일시적인**, 또는 **Edm**합니다. 값을 **Namespace** 특성에 대 한 값과 같을 수 없습니다는 **Namespace** CSDL 스키마 요소의 특성입니다. |
| **Alias**                 | 아니요          | 네임스페이스 이름 대신 사용되는 식별자입니다. 예를 들어 경우는 **EntityType** 라는 *고객* ExampleModel.Store 네임 스페이스 및 값에는 **별칭** 특성이 *StorageModel*, StorageModel.Customer를 사용 하 여 정규화 된 이름으로는 **EntityType입니다.**                                                                                                                                                                                                                                                                                    |
| **공급자**              | 예         | 데이터 공급자입니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| **ProviderManifestToken** | 예         | 반환할 공급자 매니페스트를 공급자에게 나타내는 토큰입니다. 정의된 토큰의 형식은 없으며 토큰의 값은 공급자가 정의합니다. SQL Server 공급자 매니페스트 토큰에 대 한 자세한 내용은 Entity Framework 용 SqlClient를 참조 하세요.                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a>예제

다음 예제와 **스키마** 포함 하는 요소는 **EntityContainer** 요소, 두 **EntityType** 요소와 **연결** 요소입니다.

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
   <EntityContainer Name="ExampleModelStoreContainer">
     <EntitySet Name="Customers"
                EntityType="ExampleModel.Store.Customers"
                Schema="dbo" />
     <EntitySet Name="Orders"
                EntityType="ExampleModel.Store.Orders"
                Schema="dbo" />
     <AssociationSet Name="FK_CustomerOrders"
                     Association="ExampleModel.Store.FK_CustomerOrders">
       <End Role="Customers" EntitySet="Customers" />
       <End Role="Orders" EntitySet="Orders" />
     </AssociationSet>
   </EntityContainer>
   <EntityType Name="Customers">
     <Documentation>
       <Summary>Summary here.</Summary>
       <LongDescription>Long description here.</LongDescription>
     </Documentation>
     <Key>
       <PropertyRef Name="CustomerId" />
     </Key>
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
   </EntityType>
   <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
     <Key>
       <PropertyRef Name="OrderId" />
     </Key>
     <Property Name="OrderId" Type="int" Nullable="false"
               c:CustomAttribute="someValue"/>
     <Property Name="ProductId" Type="int" Nullable="false" />
     <Property Name="Quantity" Type="int" Nullable="false" />
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <c:CustomElement>
       Custom data here.
     </c:CustomElement>
   </EntityType>
   <Association Name="FK_CustomerOrders">
     <End Role="Customers"
          Type="ExampleModel.Store.Customers" Multiplicity="1">
       <OnDelete Action="Cascade" />
     </End>
     <End Role="Orders"
          Type="ExampleModel.Store.Orders" Multiplicity="*" />
     <ReferentialConstraint>
       <Principal Role="Customers">
         <PropertyRef Name="CustomerId" />
       </Principal>
       <Dependent Role="Orders">
         <PropertyRef Name="CustomerId" />
       </Dependent>
     </ReferentialConstraint>
   </Association>
   <Function Name="UpdateOrderQuantity"
             Aggregate="false"
             BuiltIn="false"
             NiladicFunction="false"
             IsComposable="false"
             ParameterTypeSemantics="AllowImplicitConversion"
             Schema="dbo">
     <Parameter Name="orderId" Type="int" Mode="In" />
     <Parameter Name="newQuantity" Type="int" Mode="In" />
   </Function>
   <Function Name="UpdateProductInOrder" IsComposable="false">
     <CommandText>
       UPDATE Orders
       SET ProductId = @productId
       WHERE OrderId = @orderId;
     </CommandText>
     <Parameter Name="productId"
                Mode="In"
                Type="int"/>
     <Parameter Name="orderId"
                Mode="In"
                Type="int"/>
   </Function>
 </Schema>
```

## <a name="annotation-attributes"></a>주석 특성

SSDL(저장소 스키마 정의 언어)의 주석 특성은 저장소 모델의 요소에 대한 추가 메타데이터를 제공하는 저장소 모델의 사용자 지정 XML 특성입니다. 주석 특성은 올바른 XML 구조를 가져야 할 뿐 아니라 다음 제약 조건을 충족해야 합니다.

-   주석 특성은 SSDL용으로 예약된 XML 네임스페이스에 속하지 않아야 합니다.
-   두 주석 특성의 정규화된 이름은 서로 같을 수 없습니다.

두 개 이상의 주석 특성을 지정된 SSDL 요소에 적용할 수 있습니다. Annotation 요소에 포함 된 메타 데이터 System.Data.Metadata.Edm 네임 스페이스의 클래스를 사용 하 여 런타임에 액세스할 수 있습니다.

### <a name="example"></a>예제

다음 예제에서는 주석 특성이 적용 하는 EntityType 요소는 **OrderId** 속성입니다. 예제도 추가 되는 주석 요소를 표시 합니다 **EntityType** 요소입니다.

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="annotation-elements-ssdl"></a>Annotation 요소(SSDL)

SSDL(저장소 스키마 정의 언어)의 Annotation 요소는 저장소 모델에서 저장소 모델에 대한 추가 메타데이터를 제공하는 사용자 지정 XML 요소입니다. Annotation 요소는 올바른 XML 구조를 가져야 함은 물론 다음 제약 조건이 적용됩니다.

-   Annotation 요소는 SSDL용으로 예약된 XML 네임스페이스에 속하지 않아야 합니다.
-   두 Annotation 요소의 정규화된 이름은 서로 같을 수 없습니다.
-   Annotation 요소는 제공된 SSDL 요소의 다른 모든 자식 요소 뒤에 나타나야 합니다.

두 개 이상의 Annotation 요소가 제공된 SSDL 요소의 자식이 될 수 있습니다. .NET Framework 버전 4부터 annotation 요소에 포함 된 메타 데이터에 액세스할 수 있습니다 런타임 System.Data.Metadata.Edm 네임 스페이스의 클래스를 사용 하 여 합니다.

### <a name="example"></a>예제

다음 예제에서는 주석 요소에는 EntityType 요소를 보여 줍니다 (**CustomElement**). 또한이 예제에서는 주석 특성을 적용 합니다 **OrderId** 속성입니다.

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="facets-ssdl"></a>패싯(SSDL)

저장소 스키마 정의 언어 (SSDL)의 패싯은 속성 요소에 지정 된 열 형식에 제약 조건을 나타냅니다. 패싯의 XML 특성으로 구현 됩니다 **속성** 요소입니다.

다음 표에서는 SSDL에서 지원되는 패싯에 대해 설명합니다.

| 패싯           | 설명                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **데이터 정렬**   | 속성 값에 대한 비교 및 순서 지정 작업을 수행할 때 사용할 데이터 정렬 순서 또는 정렬 순서를 지정합니다.                                                                                                             |
| **FixedLength** | 열 값 길이가 다양할 수 있는지 여부를 지정합니다.                                                                                                                                                                                                  |
| **MaxLength**   | 열 값의 최대 길이를 지정합니다.                                                                                                                                                                                                           |
| **전체 자릿수**   | 형식의 속성에 대 한 **10 진수**, 속성 값을 가질 수 있습니다 자릿수를 지정 합니다. 형식의 속성에 대 한 **시간**를 **DateTime**, 및 **DateTimeOffset**, 열 값의 초의 소수 부분 자릿수를 지정 합니다. |
| **배율**       | 열 값에 대한 소수점 오른쪽의 자릿수를 지정합니다.                                                                                                                                                                      |
| **유니코드**     | 열 값을 유니코드로 저장할지 여부를 나타냅니다.                                                                                                                                                                                                    |
