---
title: SSDL 사양-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: b20d1f99f1da9c53a8a164fccc461e07d19c879d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182539"
---
# <a name="ssdl-specification"></a>SSDL 사양
SSDL(스토리지 스키마 정의 언어)은 Entity Framework 애플리케이션의 스토리지 모델을 설명하는 XML 기반 언어입니다.

Entity Framework 응용 프로그램에서 저장소 모델 메타 데이터는 ssdl로 작성 된 ssdl 파일에서 StoreItemCollection 인스턴스로 로드 되며,의 메서드를 사용 하 여 액세스할 수 있습니다. System.object. c a c. Entity Framework는 저장소 모델 메타 데이터를 사용 하 여 개념적 모델에 대 한 쿼리를 저장소 관련 명령으로 변환 합니다.

Entity Framework Designer (EF Designer)는 디자인 타임에 저장소 모델 정보를 .edmx 파일에 저장 합니다. 빌드 시 Entity Designer는 .edmx 파일의 정보를 사용 하 여 런타임에 Entity Framework에 필요한 ssdl 파일을 만듭니다.

SSDL 버전은 XML 네임스페이스로 식별됩니다.

| SSDL 버전 | XML 네임 스페이스                                     |
|:-------------|:--------------------------------------------------|
| SSDL v1      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| SSDL v2      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| SSDL v3      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a>Association 요소(SSDL)

SSDL (저장소 스키마 정의 언어)의 **Association** 요소는 기본 데이터베이스의 foreign key 제약 조건에 참여 하는 테이블 열을 지정 합니다. 두 개의 필수 자식 끝 요소는 연결의 끝에 있는 테이블과 각 끝의 복합성을 지정 합니다. 선택적 상호 관련 Alconstraint 요소는 연결의 주 및 종속 end와 참여 하는 열을 지정 합니다. AssociationSetMapping 요소 **를** 사용 하는 경우에는 연결에 대 한 열 매핑을 지정 해야 합니다.

**Association** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 1 개)
-   End (정확히 2 개)
-   (0 개 또는 1 개)
-   Annotation 요소 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Association** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| **이름**       | 예         | 기본 데이터베이스에서 해당하는 외래 키 제약 조건의 이름 |

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)이 **Association** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예에서는 **no__t-3CustomerOrders** foreign key 제약 조건에 참여 하는 열을 지정 하는 데 **해당 요소를** 사용 하는 **Association** 요소를 보여 줍니다.

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

SSDL (저장소 스키마 정의 언어)의 **AssociationSet** 요소는 기본 데이터베이스의 두 테이블 간 foreign key 제약 조건을 나타냅니다. Foreign key 제약 조건에 참여 하는 테이블 열은 Association 요소에 지정 됩니다. 지정 된 **associationset** 요소에 해당 하는 **Association** 요소는 **associationset** 요소의 **association** 특성에 지정 됩니다.

SSDL 연결 집합은 AssociationSetMapping 요소에 의해 CSDL 연결 집합에 매핑됩니다. 그러나 지정 된 CSDL 연결 집합에 대 한 CSDL 연결이 정의 되어 있는 경우에는 해당 하는 **AssociationSetMapping** 요소가 필요 하지 않습니다. 이 경우 **AssociationSetMapping** 요소가 있으면 정의 하는 매핑이 **해당 요소에** 의해 재정의 됩니다.

**AssociationSet** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 1 개)
-   End (0 개 또는 2 개)
-   Annotation 요소 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **AssociationSet** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름  | 필수 여부 | 값                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| **이름**        | 예         | 연결 집합이 나타내는 외래 키 제약 조건의 이름                          |
| **연결** | 예         | 외래 키 제약 조건에 참여하는 열을 정의하는 연결의 이름 |

> [!NOTE]
> 임의 개수의 주석 특성 (사용자 지정 XML 특성)이 **AssociationSet** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예에서는 기본 데이터베이스에서 `FK_CustomerOrders` foreign key 제약 조건을 나타내는 **AssociationSet** 요소를 보여 줍니다.

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a>CollectionType 요소 (SSDL)

SSDL (저장소 스키마 정의 언어)의 **CollectionType** 요소는 함수의 반환 형식이 컬렉션 임을 지정 합니다. **CollectionType** 요소는 ReturnType 요소의 자식입니다. RowType 자식 요소를 사용 하 여 컬렉션의 유형을 지정 합니다.

> [!NOTE]
> 여러 주석 특성 (사용자 지정 XML 특성)을 **CollectionType** 요소에 적용할 수 있습니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예에서는 **CollectionType** 요소를 사용 하 여 함수가 행 컬렉션을 반환 하도록 지정 하는 함수를 보여 줍니다.

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

SSDL (저장소 스키마 정의 언어)의 **CommandText** 요소는 데이터베이스에서 실행 되는 SQL 문을 정의할 수 있게 해 주는 Function 요소의 자식입니다. **Commandtext** 요소를 사용 하면 데이터베이스의 저장 프로시저와 유사한 기능을 추가할 수 있지만 저장소 모델에서 **commandtext** 요소를 정의 합니다.

**CommandText** 요소에는 자식 요소가 있을 수 없습니다. **CommandText** 요소의 본문은 기본 데이터베이스에 대 한 유효한 SQL 문 이어야 합니다.

**CommandText** 요소에 적용할 수 있는 특성이 없습니다.

### <a name="example"></a>예제

다음 예제에서는 자식 **CommandText** 요소를 사용 하는 **Function** 요소를 보여 줍니다. **UpdateProductInOrder** 함수를 개념적 모델로 가져와 ObjectContext에서 메서드로 노출 합니다.  

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

SSDL (저장소 스키마 정의 언어)의 **DefiningQuery** 요소를 사용 하면 기본 데이터베이스에서 직접 SQL 문을 실행할 수 있습니다. **DefiningQuery** 요소는 데이터베이스 뷰와 같이 일반적으로 사용 되지만 뷰는 데이터베이스 대신 저장소 모델에 정의 됩니다. **DefiningQuery** 요소에 정의 된 뷰는 EntitySetMapping 요소를 통해 개념적 모델의 엔터티 형식에 매핑될 수 있습니다. 이러한 매핑은 읽기 전용입니다.  

다음 SSDL 구문은 **EntitySet** 의 선언과 뷰를 검색 하는 데 사용 되는 쿼리를 포함 하는 **DefiningQuery** 요소를 보여 줍니다.

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

Entity Framework에서 저장 프로시저를 사용 하 여 보기에 대 한 읽기-쓰기 시나리오를 사용 하도록 설정할 수 있습니다. 데이터 원본 뷰나 Entity SQL 뷰를 데이터 검색을 위한 기본 테이블로 사용 하거나 저장 프로시저를 사용 하 여 변경 내용을 처리할 수 있습니다.

**DefiningQuery** 요소를 사용 하 여 Microsoft SQL Server Compact 3.5를 대상으로 지정할 수 있습니다. SQL Server Compact 3.5는 저장 프로시저를 지원 하지 않지만 **DefiningQuery** 요소를 사용 하 여 비슷한 기능을 구현할 수 있습니다. 이 요소는 프로그래밍 언어에 사용되는 데이터 형식과 데이터 소스의 데이터 형식 간에 불일치를 해결하기 위한 저장 프로시저를 만드는 데도 유용하게 사용할 수 있습니다. 특정 매개 변수 집합을 사용 하는 **DefiningQuery** 를 작성 한 후 데이터를 삭제 하는 저장 프로시저와 같은 다른 매개 변수 집합을 사용 하 여 저장 프로시저를 호출할 수 있습니다.

## <a name="dependent-element-ssdl"></a>Dependent 요소(SSDL)

SSDL (저장소 스키마 정의 언어)의 **dependent** 요소는 foreign key 제약 조건 (참조 제약 조건이 라고도 함)의 종속 끝을 정의 하는 참조 제약 조건 요소에 대 한 자식 요소입니다. **Dependent** 요소는 기본 키 열 (또는 열)을 참조 하는 테이블의 열을 지정 합니다. **Propertyref** 요소는 참조 되는 열을 지정 합니다. Principal 요소는 **종속** 요소에 지정 된 열에서 참조 하는 기본 키 열을 지정 합니다.

**종속** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   PropertyRef (하나 이상)
-   Annotation 요소 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **종속** 요소에 적용할 수 있는 특성에 대해 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **역할**       | 예         | 해당 End 요소의 **Role** 특성 (사용 되는 경우)과 동일한 값입니다. 그렇지 않으면 참조 열이 포함 된 테이블의 이름입니다. |

> [!NOTE]
> **종속** 요소에는 원하는 수의 주석 특성 (사용자 지정 XML 특성)을 적용할 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예에서는 **no__t-2CustomerOrders** foreign key 제약 조건에 참여 하는 열을 지정 하기 위해 **Referentialconstraint** 요소를 사용 하는 Association 요소를 보여 줍니다. **Dependent** 요소는 **Order** 테이블의 **CustomerId** 열을 제약 조건의 종속 끝으로 지정 합니다.

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

SSDL (저장소 스키마 정의 언어)의 **문서** 요소를 사용 하 여 부모 요소에 정의 된 개체에 대 한 정보를 제공할 수 있습니다.

**문서** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   **요약**: 부모 요소에 대 한 간단한 설명입니다. (0개 또는 한 개의 요소)
-   이상 **설명**: 부모 요소에 대 한 광범위 한 설명입니다. (0개 또는 한 개의 요소)

### <a name="applicable-attributes"></a>적용 가능한 특성

주석 특성 (사용자 지정 XML 특성)은 **문서** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제에서는 EntityType 요소의 자식 요소인 **문서** 요소를 보여 줍니다.

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

SSDL (저장소 스키마 정의 언어)의 **end** 요소는 기본 데이터베이스에서 외래 키 제약 조건의 한 end에 있는 테이블 및 행 수를 지정 합니다. **End** 요소는 Association 요소 또는 AssociationSet 요소의 자식일 수 있습니다. 각 경우에 가능한 자식 요소와 적용 가능한 특성은 서로 다릅니다.

### <a name="end-element-as-a-child-of-the-association-element"></a>Association 요소의 자식인 End 요소

**End** 요소 ( **Association** 요소의 자식)는 각각 **Type** 및 **복합성** 특성을 사용 하 여 foreign key 제약 조건의 끝에 있는 테이블 및 행 수를 지정 합니다. 외래 키 제약 조건의 End는 SSDL 연결의 일부로 정의되며 SSDL 연결에는 정확히 두 개의 End가 있어야 합니다.

**End** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소)
-   OnDelete (0 개 또는 한 개의 요소)
-   Annotation 요소 (0 개 이상의 요소)

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **End** 요소를 **Association** 요소의 자식일 때 적용할 수 있는 특성을 설명 합니다.

| 특성 이름   | 필수 여부 | 값                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **형식**         | 예         | Foreign key 제약 조건의 끝에 있는 SSDL 엔터티 집합의 정규화 된 이름입니다.                                                                                                                                                                                                                                                                                          |
| **역할**         | 아니요          | 해당 하는 참조 (사용 되는 경우) 요소의 Principal 또는 Dependent 요소에 있는 **Role** 특성의 값입니다.                                                                                                                                                                                                                                             |
| **복합** | 예         | **1**, **0**.1 또는 **\*** 은 foreign key 제약 조건의 끝에 있을 수 있는 행의 수에 따라 달라 집니다. <br/> **1** 은 외래 키 제약 조건 끝에 정확히 한 개의 행이 있음을 나타냅니다. <br/> **0 .1** 은 foreign key 제약 조건 끝에 0 개 또는 1 개의 행이 있음을 나타냅니다. <br/> **\*** 은 foreign key 제약 조건 끝에 0 개, 1 개 또는 그 이상의 행이 존재 함을 나타냅니다. |

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)은 **끝** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

#### <a name="example"></a>예제

다음 예에서는 **FK @ no__t-2CustomerOrders** foreign key 제약 조건을 정의 하는 **Association** 요소를 보여 줍니다. 각 **End** 요소에 지정 된 **복합성** 값은 **Orders** 테이블의 많은 행이 **customers** 테이블의 행과 연결 될 수 있지만 **customers** 테이블의 한 행만 행과 연결 될 수 있음을 의미 합니다. **Orders** 테이블에 있습니다. 또한 **OnDelete** 요소 **는 customers 테이블** 의 행이 삭제 되는 경우 **customers** 테이블의 특정 행을 참조 하는 **Orders** 테이블의 모든 행이 삭제 됨을 나타냅니다.

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

**End** 요소 ( **AssociationSet** 요소의 자식)는 기본 데이터베이스에서 외래 키 제약 조건의 한 end에 있는 테이블을 지정 합니다.

**End** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 1 개)
-   Annotation 요소 (0 개 이상)

#### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **AssociationSet** 요소의 자식인 **End** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| **EntitySet**  | 예         | Foreign key 제약 조건의 끝에 있는 SSDL 엔터티 집합의 이름입니다.                                      |
| **역할**       | 아니요          | 해당 Association 요소의 한 **End** 요소에 지정 된 **역할** 특성 중 하나의 값입니다. |

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)은 **끝** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

#### <a name="example"></a>예제

다음 예제에서는 두 개의 **End** 요소가 있는 **AssociationSet** 요소를 사용 하 여 **EntityContainer** 요소를 보여 줍니다.

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

SSDL (저장소 스키마 정의 언어)의 **EntityContainer** 요소는 Entity Framework 응용 프로그램에서 기본 데이터 원본의 구조를 설명 합니다. SSDL 요소에 정의 된 SSDL 엔터티 집합 (EntitySet 요소에 정의 됨)은 데이터베이스의 테이블을 나타내고, SSDL 엔터티 형식 (EntityType 요소에 정의 됨)은 테이블의 행을 나타내며, 연결 집합 (AssociationSet 요소에 정의 됨)은에서 외래 키 제약 조건을 나타냅니다. 데이터. 저장소 모델 엔터티 컨테이너는 EntityContainerMapping 요소를 통해 개념적 모델 엔터티 컨테이너에 매핑됩니다.

**EntityContainer** 요소는 문서 요소를 0 개 또는 1 개 포함할 수 있습니다. **설명서** 요소가 있으면 다른 모든 자식 요소 앞에와 야 합니다.

**EntityContainer** 요소는 나열 된 순서 대로 다음과 같은 자식 요소를 0 개 이상 포함할 수 있습니다.

-   EntitySet
-   AssociationSet
-   Annotation 요소

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **EntityContainer** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| **이름**       | 예         | 엔터티 컨테이너의 이름입니다. 이 이름에는 마침표(.)가 포함될 수 없습니다. |

> [!NOTE]
> 여러 주석 특성 (사용자 지정 XML 특성)을 **EntityContainer** 요소에 적용할 수 있습니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제에서는 두 개의 엔터티 집합과 하나의 연결 집합을 정의 하는 **EntityContainer** 요소를 보여 줍니다. 엔터티 형식 및 연결 형식 이름은 개념적 모델 네임스페이스 이름으로 정규화됩니다.

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

SSDL (저장소 스키마 정의 언어)의 **EntitySet** 요소는 기본 데이터베이스의 테이블 또는 뷰를 나타냅니다. SSDL의 EntityType 요소는 테이블이 나 뷰의 행을 나타냅니다. **EntitySet** 요소의 **ENTITYTYPE** 특성은 ssdl 엔터티 집합의 행을 나타내는 특정 ssdl 엔터티 형식을 지정 합니다. CSDL 엔터티 집합과 SSDL 엔터티 집합 간의 매핑은 EntitySetMapping 요소에 지정 됩니다.

**EntitySet** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소)
-   DefiningQuery (0 개 또는 한 개의 요소)
-   Annotation 요소

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **EntitySet** 요소에 적용할 수 있는 특성을 설명 합니다.

> [!NOTE]
> 일부 특성 (여기에 나열 되지 않음)은 **저장소** 별칭으로 정규화 될 수 있습니다. 이러한 특성은 모델을 업데이트할 때 모델 업데이트 마법사에서 사용 됩니다.

| 특성 이름 | 필수 여부 | 값                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **이름**       | 예         | 엔터티 집합의 이름입니다.                                                              |
| **EntityType** | 예         | 엔터티 집합에 포함되는 인스턴스의 엔터티 형식에 대한 정규화된 이름입니다. |
| **스키마**     | 아니요          | 데이터베이스 스키마입니다.                                                                     |
| **테이블**      | 아니요          | 데이터베이스 테이블입니다.                                                                      |

> [!NOTE]
> 임의 개수의 주석 특성 (사용자 지정 XML 특성)은 **EntitySet** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제에서는 두 **EntitySet** 요소와 하나의 **AssociationSet** 요소를 포함 하는 **EntityContainer** 요소를 보여 줍니다.

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

SSDL (저장소 스키마 정의 언어)의 **EntityType** 요소는 기본 데이터베이스의 테이블 또는 뷰에 있는 행을 나타냅니다. SSDL의 EntitySet 요소는 행이 발생 하는 테이블 또는 뷰를 나타냅니다. **EntitySet** 요소의 **ENTITYTYPE** 특성은 ssdl 엔터티 집합의 행을 나타내는 특정 ssdl 엔터티 형식을 지정 합니다. SSDL 엔터티 형식과 CSDL 엔터티 형식 간의 매핑은 EntityTypeMapping 요소에 지정 됩니다.

**EntityType** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 한 개의 요소)
-   Key (0 개 또는 한 개의 요소)
-   Annotation 요소

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **EntityType** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**       | 예         | 엔터티 형식의 이름. 이 값은 일반적으로 엔터티 형식이 나타내는 행이 있는 테이블의 이름과 동일합니다. 이 값에는 마침표(.)가 포함될 수 없습니다. |

> [!NOTE]
> **EntityType** 요소에는 원하는 수의 주석 특성 (사용자 지정 XML 특성)을 적용할 수 있습니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제에서는 두 개의 속성이 있는 **EntityType** 요소를 보여 줍니다.

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

SSDL (저장소 스키마 정의 언어)의 **Function** 요소는 기본 데이터베이스에 있는 저장 프로시저를 지정 합니다.

**Function** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 1 개)
-   Parameter (0 개 이상)
-   CommandText (0 개 또는 1 개)
-   ReturnType (0 개 이상)
-   Annotation 요소 (0 개 이상)

함수의 반환 형식은 **returntype** 요소 또는 **returntype** 특성 (아래 참조) 중 하나를 사용 하 여 지정 해야 합니다.

스토리지 모델에 지정된 저장 프로시저를 애플리케이션의 개념적 모델로 가져올 수 있습니다. 자세한 내용은 [저장 프로시저를 사용 하 여 쿼리](~/ef6/modeling/designer/stored-procedures/query.md)를 참조 하세요. 또한 **함수** 요소는 저장소 모델에서 사용자 지정 함수를 정의 하는 데 사용할 수 있습니다.  

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Function** 요소에 적용할 수 있는 특성에 대해 설명 합니다.

> [!NOTE]
> 일부 특성 (여기에 나열 되지 않음)은 **저장소** 별칭으로 정규화 될 수 있습니다. 이러한 특성은 모델을 업데이트할 때 모델 업데이트 마법사에서 사용 됩니다.

| 특성 이름             | 필수 여부 | 값                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**                   | 예         | 저장 프로시저의 이름입니다.                                                                                                                                                                                  |
| **ReturnType**             | 아니요          | 저장 프로시저의 반환 형식입니다.                                                                                                                                                                           |
| **집계**              | 아니요          | 저장 프로시저에서 집계 값을 반환 하면 **True** 입니다. 그렇지 않으면 **False**입니다.                                                                                                                                  |
| **제공**                | 아니요          | 함수가 기본 제공<sup>1</sup> 함수 이면 **True** 입니다. 그렇지 않으면 **False**입니다.                                                                                                                                  |
| **StoreFunctionName**      | 아니요          | 저장 프로시저의 이름입니다.                                                                                                                                                                                  |
| **NiladicFunction**        | 아니요          | 함수가 무항<sup>2</sup> 함수 이면 **True** 입니다. 그렇지 않으면 **False** 입니다.                                                                                                                                   |
| **IsComposable**           | 아니요          | 함수가 구성 가능한<sup>3</sup> 함수 이면 **True** 입니다. 그렇지 않으면 **False** 입니다.                                                                                                                                |
| **ParameterTypeSemantics** | 아니요          | 함수 오버로드를 확인하는 데 사용되는 형식 의미 체계를 정의하는 열거형입니다. 열거형은 함수 정의별로 공급자 매니페스트에 정의됩니다. 기본값은 **AllowImplicitConversion**입니다. |
| **스키마**                 | 아니요          | 저장 프로시저가 정의된 스키마의 이름입니다.                                                                                                                                                   |

<sup>1</sup> 기본 제공 함수는 데이터베이스에 정의 된 함수입니다. 저장소 모델에 정의 된 함수에 대 한 자세한 내용은 CommandText 요소 (SSDL)를 참조 하세요.

<sup>2</sup> 무항 함수는 매개 변수를 허용 하지 않는 함수 이며, 호출 되는 경우 괄호가 필요 하지 않습니다.

<sup>3</sup> 한 함수의 출력이 다른 함수의 입력이 될 수 있는 경우 두 개의 함수를 구성할 수 있습니다.

> [!NOTE]
> 원하는 수의 주석 특성 (사용자 지정 XML 특성)이 **Function** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예에서는 **Updateorderquantity** 저장 프로시저에 해당 하는 **함수** 요소를 보여 줍니다. 저장 프로시저는 두 매개 변수를 받아들이며 값을 반환하지 않습니다.

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

SSDL (저장소 스키마 정의 언어)의 **키** 요소는 기본 데이터베이스에 있는 테이블의 기본 키를 나타냅니다. **Key** 는 테이블의 행을 나타내는 EntityType 요소의 자식 요소입니다. 기본 키는 **EntityType** 요소에 정의 된 하나 이상의 속성 요소를 참조 하 여 **키** 요소에 정의 됩니다.

**주요** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   PropertyRef (하나 이상)
-   Annotation 요소

**키** 요소에 적용할 수 있는 특성이 없습니다.

### <a name="example"></a>예제

다음 예제에서는 하나의 속성을 참조 하는 키가 있는 **EntityType** 요소를 보여 줍니다.

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

SSDL (저장소 스키마 정의 언어)의 **OnDelete** 요소는 foreign key 제약 조건에 참여 하는 행이 삭제 될 때 데이터베이스 동작을 반영 합니다. 작업을 **Cascade**로 설정 하면 삭제 되는 행을 참조 하는 행도 삭제 됩니다. Action을 **None**으로 설정 하면 삭제 되는 행을 참조 하는 행도 삭제 되지 않습니다. **OnDelete** 요소는 끝 요소의 자식 요소입니다.

**OnDelete** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 1 개)
-   Annotation 요소 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **OnDelete** 요소에 적용 될 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| **작업**     | 예         | **Cascade** 또는 **None**입니다. ( **제한** 된 값은 유효 하지만 동일한 동작을 포함 하지 **않음**) |

> [!NOTE]
> **OnDelete** 요소에는 원하는 수의 주석 특성 (사용자 지정 XML 특성)을 적용할 수 있습니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예에서는 **FK @ no__t-2CustomerOrders** foreign key 제약 조건을 정의 하는 **Association** 요소를 보여 줍니다. **OnDelete** 요소 **는 customers 테이블** 의 행이 삭제 되는 경우 **customers** 테이블의 특정 행을 참조 하는 **Orders** 테이블의 모든 행이 삭제 됨을 나타냅니다.

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

SSDL (저장소 스키마 정의 언어)의 **Parameter** 요소는 데이터베이스의 저장 프로시저에 대 한 매개 변수를 지정 하는 Function 요소의 자식입니다.

**Parameter** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   설명서 (0 개 또는 1 개)
-   Annotation 요소 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Parameter** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**       | 예         | 매개 변수의 이름입니다.                                                                                                                                                                                                      |
| **형식**       | 예         | 매개 변수 형식입니다.                                                                                                                                                                                                             |
| **모드**       | 아니요          | **In**, **초과**, 또는 **InOut** 매개 변수는 입력, 출력 또는 입출력 매개 변수 인지에 따라 합니다.                                                                                                                |
| **MaxLength**  | 아니요          | 매개 변수의 최대 길이입니다.                                                                                                                                                                                            |
| **전체 자릿수**  | 아니요          | 매개 변수의 전체 자릿수입니다.                                                                                                                                                                                                 |
| **소수 자릿수**      | 아니요          | 매개 변수의 소수 자릿수입니다.                                                                                                                                                                                                     |
| **SRID**       | 아니요          | 공간 시스템 참조 식별자입니다. 공간 형식의 매개 변수에만 유효 합니다. 자세한 내용은 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)를 참조 하세요. |

> [!NOTE]
> 원하는 수의 주석 특성 (사용자 지정 XML 특성)이 **Parameter** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제에서는 입력 매개 변수를 지정 하는 두 개의 **매개 변수** 요소를 포함 하는 **Function** 요소를 보여 줍니다.

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

SSDL (저장소 스키마 정의 언어)의 **principal** 요소는 foreign key 제약 조건의 주 끝을 정의 하는 참조 제약 조건 (참조 제약 조건이 라고도 함)의 하위 요소입니다. **Principal** 요소는 다른 열 (또는 열)이 참조 하는 테이블의 기본 키 열을 지정 합니다. **Propertyref** 요소는 참조 되는 열을 지정 합니다. Dependent 요소는 **Principal** 요소에 지정 된 기본 키 열을 참조 하는 열을 지정 합니다.

**주요** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.

-   PropertyRef (하나 이상)
-   Annotation 요소 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Principal** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **역할**       | 예         | 해당 End 요소의 **Role** 특성 (사용 되는 경우)과 동일한 값입니다. 그렇지 않으면 참조 된 열이 포함 된 테이블의 이름입니다. |

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)이 **Principal** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예에서는 **no__t-2CustomerOrders** foreign key 제약 조건에 참여 하는 열을 지정 하기 위해 **Referentialconstraint** 요소를 사용 하는 Association 요소를 보여 줍니다. **Principal** 요소는 **Customer** 테이블의 **CustomerId** 열을 제약 조건의 주 끝으로 지정 합니다.

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

SSDL (저장소 스키마 정의 언어)의 **Property** 요소는 기본 데이터베이스의 테이블에 있는 열을 나타냅니다. **속성** 요소는 테이블의 행을 나타내는 EntityType 요소의 자식입니다. **EntityType** 요소에 정의 된 각 **Property** 요소는 열을 나타냅니다.

**속성** 요소에는 자식 요소가 있을 수 없습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Property** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름            | 필수 여부 | 값                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **이름**                  | 예         | 해당 열의 이름입니다.                                                                                                                                                                                           |
| **형식**                  | 예         | 해당 열의 형식입니다.                                                                                                                                                                                           |
| **않는**              | 아니요          | 해당 열에 null 값을 사용할 수 있는지 여부에 따라 **True** (기본값) 또는 **False** 입니다.                                                                                                                  |
| **DefaultValue**          | 아니요          | 해당 열의 기본값입니다.                                                                                                                                                                                  |
| **MaxLength**             | 아니요          | 해당 열의 최대 길이입니다.                                                                                                                                                                                 |
| **FixedLength**           | 아니요          | 해당 열 값이 고정 길이 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.                                                                                                              |
| **전체 자릿수**             | 아니요          | 해당 열의 전체 자릿수입니다.                                                                                                                                                                                      |
| **소수 자릿수**                 | 아니요          | 해당 열의 소수 자릿수입니다.                                                                                                                                                                                          |
| **유니코드**               | 아니요          | 해당 열 값이 유니코드 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.                                                                                                                   |
| **데이터 정렬**             | 아니요          | 데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.                                                                                                                                                   |
| **SRID**                  | 아니요          | 공간 시스템 참조 식별자입니다. 공간 형식의 속성에 대해서만 유효 합니다. 자세한 내용은 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)를 참조 하세요. |
| **StoreGeneratedPattern** | 아니요          | **None**, **identity** (해당 열 값이 데이터베이스에서 생성 된 id 인 경우) 또는 **계산** 된 (해당 열 값이 데이터베이스에서 계산 된 경우)입니다. RowType 속성에 사용할 수 없습니다. |

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)은 **Property** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예제에서는 두 개의 자식 **속성** 요소를 사용 하는 **EntityType** 요소를 보여 줍니다.

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

SSDL (저장소 스키마 정의 언어)의 **Propertyref** 요소는 EntityType 요소에 정의 된 속성을 참조 하 여 속성이 다음 역할 중 하나를 수행 함을 표시 합니다.

-   **EntityType** 이 나타내는 테이블의 기본 키의 일부 여야 합니다. 하나 이상의 **Propertyref** 요소를 사용 하 여 기본 키를 정의할 수 있습니다. 자세한 내용은 Key 요소를 참조 하세요.
-   참조 제약 조건의 종속 또는 주 끝. 자세한 내용은 참조 참조 요소를 참조 하세요.

**Propertyref** 요소에는 다음 자식 요소만 포함 될 수 있습니다.

-   설명서 (0 개 또는 1 개)
-   Annotation 요소

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Propertyref** 요소에 적용 될 수 있는 특성을 설명 합니다.

| 특성 이름 | 필수 여부 | 값                                |
|:---------------|:------------|:-------------------------------------|
| **이름**       | 예         | 참조된 속성의 이름입니다. |

> [!NOTE]
> 원하는 수의 주석 특성 (사용자 지정 XML 특성)이 **Propertyref** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예에서는 **EntityType** 요소에 정의 된 속성을 참조 하 여 기본 키를 정의 하는 데 사용 되는 **propertyref** 요소를 보여 줍니다.

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

SSDL (저장소 스키마 정의 언어)의 참조 **무결성 제약 조건 요소는** 기본 데이터베이스의 foreign key 제약 조건 (참조 무결성 제약 조건이 라고도 함)을 나타냅니다. 제약 조건의 주 끝과 종속 끝은 각각 주 및 종속 자식 요소에 의해 지정 됩니다. 주 서버와 종속 end에 참여 하는 열은 PropertyRef 요소를 사용 하 여 참조 됩니다.

이 **요소는** Association 요소의 선택적 자식 요소입니다. AssociationSetMapping 요소가 **Association** 요소에 지정 된 foreign key **제약 조건을 매핑하** 는 데 사용 되지 않는 경우에는이 작업을 수행 하는 데 사용 해야 합니다.

이 **요소에는 다음과** 같은 자식 요소가 있을 수 있습니다.

-   설명서 (0 개 또는 1 개)
-   보안 주체 (정확히 한 개)
-   종속 (정확히 한 개)
-   Annotation 요소 (0 개 이상)

### <a name="applicable-attributes"></a>적용 가능한 특성

원하는 수의 주석 특성 (사용자 지정 XML 특성)은 모두 **해당 요소에** 적용 될 수 있습니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예에서는 **no__t-3CustomerOrders** foreign key 제약 조건에 참여 하는 열을 지정 하는 데 **해당 요소를** 사용 하는 **Association** 요소를 보여 줍니다.

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

SSDL (저장소 스키마 정의 언어)의 **ReturnType** 요소는 **함수** 요소에 정의 된 함수에 대 한 반환 형식을 지정 합니다. 함수 반환 형식은 **ReturnType** 특성과 함께 지정할 수도 있습니다.

함수의 반환 형식은 **type** 특성 또는 **ReturnType** 요소를 사용 하 여 지정 됩니다.

**ReturnType** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.

- CollectionType (one)  

> [!NOTE]
> 모든 수의 주석 특성 (사용자 지정 XML 특성)은 **ReturnType** 요소에 적용 될 수 있습니다. 그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다. 두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.

### <a name="example"></a>예제

다음 예에서는 행 컬렉션을 반환 하는 **함수** 를 사용 합니다.

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

SSDL (저장소 스키마 정의 언어)의 **RowType** 요소는 명명 되지 않은 구조체를 저장소에 정의 된 함수에 대 한 반환 형식으로 정의 합니다.

**RowType** 요소는 **CollectionType** 요소의 자식 요소입니다.

**RowType** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.

- 속성 (하나 이상)  

### <a name="example"></a>예제

다음 예에서는 **CollectionType** 요소를 사용 하 여 함수가 **RowType** 요소에 지정 된 대로 행 컬렉션을 반환 하도록 지정 하는 저장소 함수를 보여 줍니다.


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

SSDL (저장소 스키마 정의 언어)의 **Schema** 요소는 저장소 모델 정의의 루트 요소입니다. 이 요소에는 스토리지 모델을 구성하는 개체, 함수 및 컨테이너에 대한 정의가 포함되어 있습니다.

**Schema** 요소는 다음과 같은 자식 요소를 0 개 이상 포함할 수 있습니다.

-   연결
-   EntityType
-   EntityContainer
-   기능

**Schema** 요소는 **네임 스페이스** 특성을 사용 하 여 저장소 모델의 엔터티 형식 및 연결 개체에 대 한 네임 스페이스를 정의 합니다. 네임스페이스 내에서 두 개체의 이름이 서로 같을 수 없습니다.

저장소 모델 네임 스페이스는 **Schema** 요소의 XML 네임 스페이스와 다릅니다. **네임 스페이스** 특성에 의해 정의 된 저장소 모델 네임 스페이스는 엔터티 형식 및 연결 형식에 대 한 논리적 컨테이너입니다. **Schema 요소의 XML** 네임 스페이스 ( **xmlns** 특성으로 표시 됨)는 **schema** 요소의 자식 요소 및 특성에 대 한 기본 네임 스페이스입니다. @No__t-0 형식의 XML 네임 스페이스 (YYYY와 MM은 각각 연도와 월을 나타냄)는 SSDL 용으로 예약 되어 있습니다. 사용자 지정 요소 및 특성은 이러한 형식의 네임스페이스에 있을 수 없습니다.

### <a name="applicable-attributes"></a>적용 가능한 특성

다음 표에서는 **Schema** 요소에 적용할 수 있는 특성을 설명 합니다.

| 특성 이름            | 필수 여부 | 값                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**             | 예         | 스토리지 모델의 네임스페이스입니다. **네임 스페이스** 특성의 값은 형식의 정규화 된 이름을 구성 하는 데 사용 됩니다. 예를 들어, 이름이 *Customer* 인 **entitytype** 이 examplemodel.store.customer 네임 스페이스에 있는 경우 **Entitytype** 의 정규화 된 이름은 examplemodel.store.customer입니다. <br/> 다음 문자열은 **Namespace** 특성에 대 한 값으로 사용할 수 없습니다. **시스템**, **임시**또는 **Edm**입니다. **Namespace** 특성의 값은 CSDL Schema 요소의 **namespace** 특성에 대 한 값과 같을 수 없습니다. |
| **Alias**                 | 아니요          | 네임스페이스 이름 대신 사용되는 식별자입니다. 예를 들어 이름이 *Customer* 인 **EntityType** 이 examplemodel.store.customer 네임 스페이스에 있고 **Alias** 특성의 값이 *storagemodel.customer*인 경우 storagemodel.customer를의 **정규화 된 이름으로 사용할 수 있습니다. EntityType.**                                                                                                                                                                                                                                                                                    |
| **공급자**              | 예         | 데이터 공급자입니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| **ProviderManifestToken** | 예         | 반환할 공급자 매니페스트를 공급자에게 나타내는 토큰입니다. 정의된 토큰의 형식은 없으며 토큰의 값은 공급자가 정의합니다. SQL Server 공급자 매니페스트 토큰에 대 한 자세한 내용은 Entity Framework의 SqlClient를 참조 하세요.                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a>예제

다음 예제에서는 **EntityContainer** 요소, 두 개의 **EntityType** 요소 및 하나의 **Association** 요소를 포함 하는 **스키마** 요소를 보여 줍니다.

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="https://schemas.microsoft.com/ado/2009/11/edm/ssdl">
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

SSDL(스토리지 스키마 정의 언어)의 주석 특성은 스토리지 모델의 요소에 대한 추가 메타데이터를 제공하는 스토리지 모델의 사용자 지정 XML 특성입니다. 주석 특성은 올바른 XML 구조를 가져야 할 뿐 아니라 다음 제약 조건을 충족해야 합니다.

-   주석 특성은 SSDL용으로 예약된 XML 네임스페이스에 속하지 않아야 합니다.
-   두 주석 특성의 정규화된 이름은 서로 같을 수 없습니다.

두 개 이상의 주석 특성을 지정된 SSDL 요소에 적용할 수 있습니다. Annotation 요소에 포함 된 메타 데이터는 런타임에 System.object 네임 스페이스의 클래스를 사용 하 여 액세스할 수 있습니다.

### <a name="example"></a>예제

다음 예에서는 **OrderId** 속성에 적용 된 주석 특성이 있는 EntityType 요소를 보여 줍니다. 또한이 예제에서는 **EntityType** 요소에 추가 된 주석 요소를 보여 줍니다.

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

SSDL(스토리지 스키마 정의 언어)의 Annotation 요소는 스토리지 모델에서 스토리지 모델에 대한 추가 메타데이터를 제공하는 사용자 지정 XML 요소입니다. Annotation 요소는 올바른 XML 구조를 가져야 함은 물론 다음 제약 조건이 적용됩니다.

-   Annotation 요소는 SSDL용으로 예약된 XML 네임스페이스에 속하지 않아야 합니다.
-   두 Annotation 요소의 정규화된 이름은 서로 같을 수 없습니다.
-   Annotation 요소는 제공된 SSDL 요소의 다른 모든 자식 요소 뒤에 나타나야 합니다.

두 개 이상의 Annotation 요소가 제공된 SSDL 요소의 자식이 될 수 있습니다. .NET Framework 버전 4부터 annotation 요소에 포함 된 메타 데이터는 런타임에 System.object 네임 스페이스의 클래스를 사용 하 여 액세스할 수 있습니다.

### <a name="example"></a>예제

다음 예에서는 annotation 요소 (**customelement**)가 있는 EntityType 요소를 보여 줍니다. 또한이 예제에서는 **OrderId** 속성에 적용 된 주석 특성을 보여 줍니다.

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

SSDL (저장소 스키마 정의 언어)의 패싯은 속성 요소에 지정 된 열 유형에 대 한 제약 조건을 나타냅니다. 패싯은 **속성** 요소에서 XML 특성으로 구현 됩니다.

다음 표에서는 SSDL에서 지원되는 패싯에 대해 설명합니다.

| 패싯           | 설명                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **데이터 정렬**   | 속성 값에 대한 비교 및 순서 지정 작업을 수행할 때 사용할 데이터 정렬 순서 또는 정렬 순서를 지정합니다.                                                                                                             |
| **FixedLength** | 열 값 길이가 다양할 수 있는지 여부를 지정합니다.                                                                                                                                                                                                  |
| **MaxLength**   | 열 값의 최대 길이를 지정합니다.                                                                                                                                                                                                           |
| **전체 자릿수**   | **Decimal**형식의 속성에 대해 속성 값에 포함 될 수 있는 자릿수를 지정 합니다. **Time**, **DateTime**및 **DateTimeOffset**형식의 속성에 대해 열 값의 초 소수 부분 자릿수를 지정 합니다. |
| **소수 자릿수**       | 열 값에 대한 소수점 오른쪽의 자릿수를 지정합니다.                                                                                                                                                                      |
| **유니코드**     | 열 값을 유니코드로 저장할지 여부를 나타냅니다.                                                                                                                                                                                                    |
