---
title: SSDL 사양-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: b20d1f99f1da9c53a8a164fccc461e07d19c879d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415474"
---
# <a name="ssdl-specification"></a><span data-ttu-id="92de4-102">SSDL 사양</span><span class="sxs-lookup"><span data-stu-id="92de4-102">SSDL Specification</span></span>
<span data-ttu-id="92de4-103">SSDL(스토리지 스키마 정의 언어)은 Entity Framework 애플리케이션의 스토리지 모델을 설명하는 XML 기반 언어입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="92de4-104">Entity Framework 응용 프로그램에서 저장소 모델 메타 데이터는 ssdl로 작성 된 ssdl 파일에서 StoreItemCollection 인스턴스로 로드 되며,의 메서드를 사용 하 여 액세스할 수 있습니다. System.object. c a c.</span><span class="sxs-lookup"><span data-stu-id="92de4-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="92de4-105">Entity Framework는 저장소 모델 메타 데이터를 사용 하 여 개념적 모델에 대 한 쿼리를 저장소 관련 명령으로 변환 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="92de4-106">Entity Framework Designer (EF Designer)는 디자인 타임에 저장소 모델 정보를 .edmx 파일에 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="92de4-107">빌드 시 Entity Designer는 .edmx 파일의 정보를 사용 하 여 런타임에 Entity Framework에 필요한 ssdl 파일을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="92de4-108">SSDL 버전은 XML 네임스페이스로 식별됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="92de4-109">SSDL 버전</span><span class="sxs-lookup"><span data-stu-id="92de4-109">SSDL Version</span></span> | <span data-ttu-id="92de4-110">XML 네임 스페이스</span><span class="sxs-lookup"><span data-stu-id="92de4-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="92de4-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="92de4-111">SSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="92de4-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="92de4-112">SSDL v2</span></span>      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="92de4-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="92de4-113">SSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="92de4-114">Association 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-114">Association Element (SSDL)</span></span>

<span data-ttu-id="92de4-115">SSDL (저장소 스키마 정의 언어)의 **Association** 요소는 기본 데이터베이스의 foreign key 제약 조건에 참여 하는 테이블 열을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="92de4-116">필수 자식 요소인 두 개의 End 요소는 연결의 양쪽 끝에 있는 테이블과 각 끝의 복합성을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="92de4-117">선택적 요소인 ReferentialConstraint 요소는 관련 열뿐 아니라 연결의 주 끝과 종속 끝도 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="92de4-118">AssociationSetMapping 요소 **를** 사용 하는 경우에는 연결에 대 한 열 매핑을 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="92de4-119">**Association** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="92de4-120">설명서 (0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="92de4-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="92de4-121">End (정확히 2 개)</span><span class="sxs-lookup"><span data-stu-id="92de4-121">End (exactly two)</span></span>
-   <span data-ttu-id="92de4-122">(0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="92de4-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="92de4-123">Annotation 요소(0개 이상)</span><span class="sxs-lookup"><span data-stu-id="92de4-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="92de4-124">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="92de4-124">Applicable Attributes</span></span>

<span data-ttu-id="92de4-125">다음 표에서는 **Association** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="92de4-126">특성 이름</span><span class="sxs-lookup"><span data-stu-id="92de4-126">Attribute Name</span></span> | <span data-ttu-id="92de4-127">필수 여부</span><span class="sxs-lookup"><span data-stu-id="92de4-127">Is Required</span></span> | <span data-ttu-id="92de4-128">값</span><span class="sxs-lookup"><span data-stu-id="92de4-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="92de4-129">**Name**</span><span class="sxs-lookup"><span data-stu-id="92de4-129">**Name**</span></span>       | <span data-ttu-id="92de4-130">예</span><span class="sxs-lookup"><span data-stu-id="92de4-130">Yes</span></span>         | <span data-ttu-id="92de4-131">기본 데이터베이스에서 해당하는 외래 키 제약 조건의 이름</span><span class="sxs-lookup"><span data-stu-id="92de4-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="92de4-132">모든 수의 주석 특성 (사용자 지정 XML 특성)이 **Association** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="92de4-133">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="92de4-134">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-135">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-135">Example</span></span>

<span data-ttu-id="92de4-136">다음 예에서는 다음과 같이 외래 키 **제약 조건 외래** 키 제약 조건 **\_** 에 참여 하는 열을 지정 하는 데에는 해당 요소를 사용 하는 **Association** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="associationset-element-ssdl"></a><span data-ttu-id="92de4-137">AssociationSet 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="92de4-138">SSDL (저장소 스키마 정의 언어)의 **AssociationSet** 요소는 기본 데이터베이스의 두 테이블 간 foreign key 제약 조건을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="92de4-139">외래 키 제약 조건에 참여하는 테이블 열은 Association 요소에 지정됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="92de4-140">지정 된 **associationset** 요소에 해당 하는 **Association** 요소는 **associationset** 요소의 **association** 특성에 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="92de4-141">SSDL 연결 집합은 AssociationSetMapping 요소에 의해 CSDL 연결 집합에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="92de4-142">그러나 지정 된 CSDL 연결 집합에 대 한 CSDL 연결이 정의 되어 있는 경우에는 해당 하는 **AssociationSetMapping** 요소가 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="92de4-143">이 경우 **AssociationSetMapping** 요소가 있으면 정의 하는 매핑이 **해당 요소에** 의해 재정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="92de4-144">**AssociationSet** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="92de4-145">설명서 (0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="92de4-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="92de4-146">End(0개 또는 두 개)</span><span class="sxs-lookup"><span data-stu-id="92de4-146">End (zero or two)</span></span>
-   <span data-ttu-id="92de4-147">Annotation 요소(0개 이상)</span><span class="sxs-lookup"><span data-stu-id="92de4-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="92de4-148">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="92de4-148">Applicable Attributes</span></span>

<span data-ttu-id="92de4-149">다음 표에서는 **AssociationSet** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="92de4-150">특성 이름</span><span class="sxs-lookup"><span data-stu-id="92de4-150">Attribute Name</span></span>  | <span data-ttu-id="92de4-151">필수 여부</span><span class="sxs-lookup"><span data-stu-id="92de4-151">Is Required</span></span> | <span data-ttu-id="92de4-152">값</span><span class="sxs-lookup"><span data-stu-id="92de4-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="92de4-153">**Name**</span><span class="sxs-lookup"><span data-stu-id="92de4-153">**Name**</span></span>        | <span data-ttu-id="92de4-154">예</span><span class="sxs-lookup"><span data-stu-id="92de4-154">Yes</span></span>         | <span data-ttu-id="92de4-155">연결 집합이 나타내는 외래 키 제약 조건의 이름</span><span class="sxs-lookup"><span data-stu-id="92de4-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="92de4-156">**연결**</span><span class="sxs-lookup"><span data-stu-id="92de4-156">**Association**</span></span> | <span data-ttu-id="92de4-157">예</span><span class="sxs-lookup"><span data-stu-id="92de4-157">Yes</span></span>         | <span data-ttu-id="92de4-158">외래 키 제약 조건에 참여하는 열을 정의하는 연결의 이름</span><span class="sxs-lookup"><span data-stu-id="92de4-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="92de4-159">임의 개수의 주석 특성 (사용자 지정 XML 특성)이 **AssociationSet** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="92de4-160">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="92de4-161">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-162">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-162">Example</span></span>

<span data-ttu-id="92de4-163">다음 예에서는 기본 데이터베이스의 `FK_CustomerOrders` foreign key 제약 조건을 나타내는 **AssociationSet** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="92de4-164">CollectionType 요소 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="92de4-165">SSDL (저장소 스키마 정의 언어)의 **CollectionType** 요소는 함수의 반환 형식이 컬렉션 임을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="92de4-166">**CollectionType** 요소는 ReturnType 요소의 자식입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="92de4-167">RowType 자식 요소를 사용 하 여 컬렉션의 유형을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="92de4-168">여러 주석 특성 (사용자 지정 XML 특성)을 **CollectionType** 요소에 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="92de4-169">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="92de4-170">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-171">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-171">Example</span></span>

<span data-ttu-id="92de4-172">다음 예에서는 **CollectionType** 요소를 사용 하 여 함수가 행 컬렉션을 반환 하도록 지정 하는 함수를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

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

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="92de4-173">CommandText 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="92de4-174">SSDL (저장소 스키마 정의 언어)의 **CommandText** 요소는 데이터베이스에서 실행 되는 SQL 문을 정의할 수 있게 해 주는 Function 요소의 자식입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="92de4-175">**Commandtext** 요소를 사용 하면 데이터베이스의 저장 프로시저와 유사한 기능을 추가할 수 있지만 저장소 모델에서 **commandtext** 요소를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="92de4-176">**CommandText** 요소에는 자식 요소가 있을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="92de4-177">**CommandText** 요소의 본문은 기본 데이터베이스에 대 한 유효한 SQL 문 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="92de4-178">**CommandText** 요소에 적용할 수 있는 특성이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-179">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-179">Example</span></span>

<span data-ttu-id="92de4-180">다음 예제에서는 자식 **CommandText** 요소를 사용 하는 **Function** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="92de4-181">**UpdateProductInOrder** 함수를 개념적 모델로 가져와 ObjectContext에서 메서드로 노출 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

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

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="92de4-182">DefiningQuery 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="92de4-183">SSDL (저장소 스키마 정의 언어)의 **DefiningQuery** 요소를 사용 하면 기본 데이터베이스에서 직접 SQL 문을 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="92de4-184">**DefiningQuery** 요소는 데이터베이스 뷰와 같이 일반적으로 사용 되지만 뷰는 데이터베이스 대신 저장소 모델에 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="92de4-185">**DefiningQuery** 요소에 정의 된 뷰는 EntitySetMapping 요소를 통해 개념적 모델의 엔터티 형식에 매핑될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="92de4-186">이러한 매핑은 읽기 전용입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-186">These mappings are read-only.</span></span>  

<span data-ttu-id="92de4-187">다음 SSDL 구문은 **EntitySet** 의 선언과 뷰를 검색 하는 데 사용 되는 쿼리를 포함 하는 **DefiningQuery** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

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

<span data-ttu-id="92de4-188">Entity Framework에서 저장 프로시저를 사용 하 여 보기에 대 한 읽기-쓰기 시나리오를 사용 하도록 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span><span data-ttu-id="92de4-189"> 데이터 원본 뷰나 Entity SQL 뷰를 데이터 검색을 위한 기본 테이블로 사용 하거나 저장 프로시저를 사용 하 여 변경 내용을 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-189"> You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="92de4-190">**DefiningQuery** 요소를 사용 하 여 Microsoft SQL Server Compact 3.5를 대상으로 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="92de4-191">SQL Server Compact 3.5는 저장 프로시저를 지원 하지 않지만 **DefiningQuery** 요소를 사용 하 여 비슷한 기능을 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="92de4-192">이 요소는 프로그래밍 언어에 사용되는 데이터 형식과 데이터 소스의 데이터 형식 간에 불일치를 해결하기 위한 저장 프로시저를 만드는 데도 유용하게 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="92de4-193">특정 매개 변수 집합을 사용 하는 **DefiningQuery** 를 작성 한 후 데이터를 삭제 하는 저장 프로시저와 같은 다른 매개 변수 집합을 사용 하 여 저장 프로시저를 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="92de4-194">Dependent 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="92de4-195">SSDL (저장소 스키마 정의 언어)의 **dependent** 요소는 foreign key 제약 조건 (참조 제약 조건이 라고도 함)의 종속 끝을 정의 하는 참조 제약 조건 요소에 대 한 자식 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="92de4-196">**Dependent** 요소는 기본 키 열 (또는 열)을 참조 하는 테이블의 열을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="92de4-197">**Propertyref** 요소는 참조 되는 열을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="92de4-198">Principal 요소는 **종속** 요소에 지정 된 열에서 참조 하는 기본 키 열을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="92de4-199">**종속** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="92de4-200">PropertyRef(0개 이상)</span><span class="sxs-lookup"><span data-stu-id="92de4-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="92de4-201">Annotation 요소(0개 이상)</span><span class="sxs-lookup"><span data-stu-id="92de4-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="92de4-202">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="92de4-202">Applicable Attributes</span></span>

<span data-ttu-id="92de4-203">다음 표에서는 **종속** 요소에 적용할 수 있는 특성에 대해 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="92de4-204">특성 이름</span><span class="sxs-lookup"><span data-stu-id="92de4-204">Attribute Name</span></span> | <span data-ttu-id="92de4-205">필수 여부</span><span class="sxs-lookup"><span data-stu-id="92de4-205">Is Required</span></span> | <span data-ttu-id="92de4-206">값</span><span class="sxs-lookup"><span data-stu-id="92de4-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="92de4-207">**역할**</span><span class="sxs-lookup"><span data-stu-id="92de4-207">**Role**</span></span>       | <span data-ttu-id="92de4-208">예</span><span class="sxs-lookup"><span data-stu-id="92de4-208">Yes</span></span>         | <span data-ttu-id="92de4-209">해당 End 요소의 **Role** 특성 (사용 되는 경우)과 동일한 값입니다. 그렇지 않으면 참조 열이 포함 된 테이블의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="92de4-210">**종속** 요소에는 원하는 수의 주석 특성 (사용자 지정 XML 특성)을 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="92de4-211">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="92de4-212">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-213">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-213">Example</span></span>

<span data-ttu-id="92de4-214">다음 예에서는 **키를 사용** 하 여 **FK\_customerorders** foreign key 제약 조건에 참여 하는 열을 지정 하는 Association 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="92de4-215">**Dependent** 요소는 **Order** 테이블의 **CustomerId** 열을 제약 조건의 종속 끝으로 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

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

## <a name="documentation-element-ssdl"></a><span data-ttu-id="92de4-216">Documentation 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="92de4-217">SSDL (저장소 스키마 정의 언어)의 **문서** 요소를 사용 하 여 부모 요소에 정의 된 개체에 대 한 정보를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="92de4-218">**문서** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="92de4-219">**요약**: 부모 요소에 대 한 간단한 설명입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="92de4-220">(0개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="92de4-220">(zero or one element)</span></span>
-   <span data-ttu-id="92de4-221">이상 **설명**: 부모 요소에 대 한 광범위 한 설명입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="92de4-222">(0개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="92de4-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="92de4-223">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="92de4-223">Applicable Attributes</span></span>

<span data-ttu-id="92de4-224">주석 특성 (사용자 지정 XML 특성)은 **문서** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="92de4-225">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="92de4-226">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-227">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-227">Example</span></span>

<span data-ttu-id="92de4-228">다음 예제에서는 EntityType 요소의 자식 요소인 **문서** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

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

## <a name="end-element-ssdl"></a><span data-ttu-id="92de4-229">End 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-229">End Element (SSDL)</span></span>

<span data-ttu-id="92de4-230">SSDL (저장소 스키마 정의 언어)의 **end** 요소는 기본 데이터베이스에서 외래 키 제약 조건의 한 end에 있는 테이블 및 행 수를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="92de4-231">**End** 요소는 Association 요소 또는 AssociationSet 요소의 자식일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="92de4-232">각 경우에 가능한 자식 요소와 적용 가능한 특성은 서로 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="92de4-233">Association 요소의 자식인 End 요소</span><span class="sxs-lookup"><span data-stu-id="92de4-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="92de4-234">**End** 요소 ( **Association** 요소의 자식)는 각각 **Type** 및 **복합성** 특성을 사용 하 여 foreign key 제약 조건의 끝에 있는 테이블 및 행 수를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="92de4-235">외래 키 제약 조건의 End는 SSDL 연결의 일부로 정의되며 SSDL 연결에는 정확히 두 개의 End가 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="92de4-236">**End** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="92de4-237">설명서 (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="92de4-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="92de4-238">OnDelete (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="92de4-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="92de4-239">Annotation 요소 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="92de4-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="92de4-240">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="92de4-240">Applicable Attributes</span></span>

<span data-ttu-id="92de4-241">다음 표에서는 **End** 요소를 **Association** 요소의 자식일 때 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="92de4-242">특성 이름</span><span class="sxs-lookup"><span data-stu-id="92de4-242">Attribute Name</span></span>   | <span data-ttu-id="92de4-243">필수 여부</span><span class="sxs-lookup"><span data-stu-id="92de4-243">Is Required</span></span> | <span data-ttu-id="92de4-244">값</span><span class="sxs-lookup"><span data-stu-id="92de4-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="92de4-245">**Type**</span><span class="sxs-lookup"><span data-stu-id="92de4-245">**Type**</span></span>         | <span data-ttu-id="92de4-246">예</span><span class="sxs-lookup"><span data-stu-id="92de4-246">Yes</span></span>         | <span data-ttu-id="92de4-247">외래 키 제약 조건의 End에 있는 SSDL 엔터티 집합의 정규화된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="92de4-248">**역할**</span><span class="sxs-lookup"><span data-stu-id="92de4-248">**Role**</span></span>         | <span data-ttu-id="92de4-249">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-249">No</span></span>          | <span data-ttu-id="92de4-250">해당 하는 참조 (사용 되는 경우) 요소의 Principal 또는 Dependent 요소에 있는 **Role** 특성의 값입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="92de4-251">**다중성**</span><span class="sxs-lookup"><span data-stu-id="92de4-251">**Multiplicity**</span></span> | <span data-ttu-id="92de4-252">예</span><span class="sxs-lookup"><span data-stu-id="92de4-252">Yes</span></span>         | <span data-ttu-id="92de4-253">**1**, **0**.1 또는 **\*** foreign key 제약 조건의 끝에 있을 수 있는 행의 수에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="92de4-254">**1** 은 외래 키 제약 조건 끝에 정확히 한 개의 행이 있음을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="92de4-255">**0 .1** 은 foreign key 제약 조건 끝에 0 개 또는 1 개의 행이 있음을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="92de4-256">**\*** 외래 키 제약 조건 끝에 0 개 이상의 행이 존재 함을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="92de4-257">모든 수의 주석 특성 (사용자 지정 XML 특성)은 **끝** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="92de4-258">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="92de4-259">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="92de4-260">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-260">Example</span></span>

<span data-ttu-id="92de4-261">다음 예에서는 **FK\_CustomerOrders** foreign key 제약 조건을 정의 하는 **Association** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="92de4-262">각 **End** 요소에 지정 된 **복합성** 값은 **orders** 테이블의 많은 행이 **customers** 테이블의 행과 연결 될 수 있지만 **customers** 테이블의 한 행만 **orders** 테이블의 행과 연결 될 수 있음을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="92de4-263">또한 **OnDelete** 요소 **는 customers 테이블** 의 행이 삭제 되는 경우 **customers** 테이블의 특정 행을 참조 하는 **Orders** 테이블의 모든 행이 삭제 됨을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="92de4-264">AssociationSet 요소의 자식인 End 요소</span><span class="sxs-lookup"><span data-stu-id="92de4-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="92de4-265">**End** 요소 ( **AssociationSet** 요소의 자식)는 기본 데이터베이스에서 외래 키 제약 조건의 한 end에 있는 테이블을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="92de4-266">**End** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="92de4-267">설명서 (0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="92de4-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="92de4-268">Annotation 요소(0개 이상)</span><span class="sxs-lookup"><span data-stu-id="92de4-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="92de4-269">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="92de4-269">Applicable Attributes</span></span>

<span data-ttu-id="92de4-270">다음 표에서는 **AssociationSet** 요소의 자식인 **End** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="92de4-271">특성 이름</span><span class="sxs-lookup"><span data-stu-id="92de4-271">Attribute Name</span></span> | <span data-ttu-id="92de4-272">필수 여부</span><span class="sxs-lookup"><span data-stu-id="92de4-272">Is Required</span></span> | <span data-ttu-id="92de4-273">값</span><span class="sxs-lookup"><span data-stu-id="92de4-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="92de4-274">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="92de4-274">**EntitySet**</span></span>  | <span data-ttu-id="92de4-275">예</span><span class="sxs-lookup"><span data-stu-id="92de4-275">Yes</span></span>         | <span data-ttu-id="92de4-276">외래 키 제약 조건의 End에 있는 SSDL 엔터티 집합의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="92de4-277">**역할**</span><span class="sxs-lookup"><span data-stu-id="92de4-277">**Role**</span></span>       | <span data-ttu-id="92de4-278">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-278">No</span></span>          | <span data-ttu-id="92de4-279">해당 Association 요소의 한 **End** 요소에 지정 된 **역할** 특성 중 하나의 값입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="92de4-280">모든 수의 주석 특성 (사용자 지정 XML 특성)은 **끝** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="92de4-281">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="92de4-282">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="92de4-283">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-283">Example</span></span>

<span data-ttu-id="92de4-284">다음 예제에서는 두 개의 **End** 요소가 있는 **AssociationSet** 요소를 사용 하 여 **EntityContainer** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

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

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="92de4-285">EntityContainer 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="92de4-286">SSDL (저장소 스키마 정의 언어)의 **EntityContainer** 요소는 Entity Framework 응용 프로그램에서 기본 데이터 소스의 구조를 설명 합니다. ssdl 엔터티 집합 (EntitySet 요소에 정의 됨)은 데이터베이스의 테이블을 나타내고, ssdl 엔터티 형식 (EntityType 요소에 정의 됨)은 테이블의 행을 나타내며, 연결 집합 (AssociationSet 요소에 정의 됨)은 데이터베이스의 foreign key 제약 조건을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="92de4-287">저장소 모델 엔터티 컨테이너는 EntityContainerMapping 요소를 통해 개념적 모델 엔터티 컨테이너에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="92de4-288">**EntityContainer** 요소는 문서 요소를 0 개 또는 1 개 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="92de4-289">**설명서** 요소가 있으면 다른 모든 자식 요소 앞에와 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="92de4-290">**EntityContainer** 요소는 나열 된 순서 대로 다음과 같은 자식 요소를 0 개 이상 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="92de4-291">EntitySet</span><span class="sxs-lookup"><span data-stu-id="92de4-291">EntitySet</span></span>
-   <span data-ttu-id="92de4-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="92de4-292">AssociationSet</span></span>
-   <span data-ttu-id="92de4-293">Annotation 요소</span><span class="sxs-lookup"><span data-stu-id="92de4-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="92de4-294">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="92de4-294">Applicable Attributes</span></span>

<span data-ttu-id="92de4-295">다음 표에서는 **EntityContainer** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="92de4-296">특성 이름</span><span class="sxs-lookup"><span data-stu-id="92de4-296">Attribute Name</span></span> | <span data-ttu-id="92de4-297">필수 여부</span><span class="sxs-lookup"><span data-stu-id="92de4-297">Is Required</span></span> | <span data-ttu-id="92de4-298">값</span><span class="sxs-lookup"><span data-stu-id="92de4-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="92de4-299">**Name**</span><span class="sxs-lookup"><span data-stu-id="92de4-299">**Name**</span></span>       | <span data-ttu-id="92de4-300">예</span><span class="sxs-lookup"><span data-stu-id="92de4-300">Yes</span></span>         | <span data-ttu-id="92de4-301">엔터티 컨테이너의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-301">The name of the entity container.</span></span> <span data-ttu-id="92de4-302">이 이름에는 마침표(.)가 포함될 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="92de4-303">여러 주석 특성 (사용자 지정 XML 특성)을 **EntityContainer** 요소에 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="92de4-304">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="92de4-305">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-306">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-306">Example</span></span>

<span data-ttu-id="92de4-307">다음 예제에서는 두 개의 엔터티 집합과 하나의 연결 집합을 정의 하는 **EntityContainer** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="92de4-308">엔터티 형식 및 연결 형식 이름은 개념적 모델 네임스페이스 이름으로 정규화됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

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

## <a name="entityset-element-ssdl"></a><span data-ttu-id="92de4-309">EntitySet 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="92de4-310">SSDL (저장소 스키마 정의 언어)의 **EntitySet** 요소는 기본 데이터베이스의 테이블 또는 뷰를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="92de4-311">SSDL의 EntityType 요소는 테이블이나 뷰의 행을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="92de4-312">**EntitySet** 요소의 **ENTITYTYPE** 특성은 ssdl 엔터티 집합의 행을 나타내는 특정 ssdl 엔터티 형식을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="92de4-313">CSDL 엔터티 집합과 SSDL 엔터티 집합 간의 매핑은 EntitySetMapping 요소에 지정됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="92de4-314">**EntitySet** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="92de4-315">설명서 (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="92de4-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="92de4-316">DefiningQuery (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="92de4-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="92de4-317">Annotation 요소</span><span class="sxs-lookup"><span data-stu-id="92de4-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="92de4-318">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="92de4-318">Applicable Attributes</span></span>

<span data-ttu-id="92de4-319">다음 표에서는 **EntitySet** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="92de4-320">일부 특성 (여기에 나열 되지 않음)은 **저장소** 별칭으로 정규화 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="92de4-321">이러한 특성은 모델을 업데이트할 때 모델 업데이트 마법사에서 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="92de4-322">특성 이름</span><span class="sxs-lookup"><span data-stu-id="92de4-322">Attribute Name</span></span> | <span data-ttu-id="92de4-323">필수 여부</span><span class="sxs-lookup"><span data-stu-id="92de4-323">Is Required</span></span> | <span data-ttu-id="92de4-324">값</span><span class="sxs-lookup"><span data-stu-id="92de4-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="92de4-325">**Name**</span><span class="sxs-lookup"><span data-stu-id="92de4-325">**Name**</span></span>       | <span data-ttu-id="92de4-326">예</span><span class="sxs-lookup"><span data-stu-id="92de4-326">Yes</span></span>         | <span data-ttu-id="92de4-327">엔터티 집합의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="92de4-328">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="92de4-328">**EntityType**</span></span> | <span data-ttu-id="92de4-329">예</span><span class="sxs-lookup"><span data-stu-id="92de4-329">Yes</span></span>         | <span data-ttu-id="92de4-330">엔터티 집합에 포함되는 인스턴스의 엔터티 형식에 대한 정규화된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="92de4-331">**스키마**</span><span class="sxs-lookup"><span data-stu-id="92de4-331">**Schema**</span></span>     | <span data-ttu-id="92de4-332">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-332">No</span></span>          | <span data-ttu-id="92de4-333">데이터베이스 스키마입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="92de4-334">**Table**</span><span class="sxs-lookup"><span data-stu-id="92de4-334">**Table**</span></span>      | <span data-ttu-id="92de4-335">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-335">No</span></span>          | <span data-ttu-id="92de4-336">데이터베이스 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="92de4-337">임의 개수의 주석 특성 (사용자 지정 XML 특성)은 **EntitySet** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="92de4-338">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="92de4-339">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-340">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-340">Example</span></span>

<span data-ttu-id="92de4-341">다음 예제에서는 두 **EntitySet** 요소와 하나의 **AssociationSet** 요소를 포함 하는 **EntityContainer** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

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

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="92de4-342">EntityType 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="92de4-343">SSDL (저장소 스키마 정의 언어)의 **EntityType** 요소는 기본 데이터베이스의 테이블 또는 뷰에 있는 행을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="92de4-344">SSDL의 EntitySet 요소는 행이 나타나는 테이블 또는 뷰를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="92de4-345">**EntitySet** 요소의 **ENTITYTYPE** 특성은 ssdl 엔터티 집합의 행을 나타내는 특정 ssdl 엔터티 형식을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="92de4-346">SSDL 엔터티 형식과 CSDL 엔터티 형식 간의 매핑은 EntityTypeMapping 요소에서 지정됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="92de4-347">**EntityType** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="92de4-348">설명서 (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="92de4-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="92de4-349">Key(0개 또는 1개)</span><span class="sxs-lookup"><span data-stu-id="92de4-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="92de4-350">Annotation 요소</span><span class="sxs-lookup"><span data-stu-id="92de4-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="92de4-351">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="92de4-351">Applicable Attributes</span></span>

<span data-ttu-id="92de4-352">다음 표에서는 **EntityType** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="92de4-353">특성 이름</span><span class="sxs-lookup"><span data-stu-id="92de4-353">Attribute Name</span></span> | <span data-ttu-id="92de4-354">필수 여부</span><span class="sxs-lookup"><span data-stu-id="92de4-354">Is Required</span></span> | <span data-ttu-id="92de4-355">값</span><span class="sxs-lookup"><span data-stu-id="92de4-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="92de4-356">**Name**</span><span class="sxs-lookup"><span data-stu-id="92de4-356">**Name**</span></span>       | <span data-ttu-id="92de4-357">예</span><span class="sxs-lookup"><span data-stu-id="92de4-357">Yes</span></span>         | <span data-ttu-id="92de4-358">엔터티 형식의 이름.</span><span class="sxs-lookup"><span data-stu-id="92de4-358">The name of the entity type.</span></span> <span data-ttu-id="92de4-359">이 값은 일반적으로 엔터티 형식이 나타내는 행이 있는 테이블의 이름과 동일합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="92de4-360">이 값에는 마침표(.)가 포함될 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="92de4-361">**EntityType** 요소에는 원하는 수의 주석 특성 (사용자 지정 XML 특성)을 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="92de4-362">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="92de4-363">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-364">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-364">Example</span></span>

<span data-ttu-id="92de4-365">다음 예제에서는 두 개의 속성이 있는 **EntityType** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-365">The following example shows an **EntityType** element with two properties:</span></span>

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

## <a name="function-element-ssdl"></a><span data-ttu-id="92de4-366">Function 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-366">Function Element (SSDL)</span></span>

<span data-ttu-id="92de4-367">SSDL (저장소 스키마 정의 언어)의 **Function** 요소는 기본 데이터베이스에 있는 저장 프로시저를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="92de4-368">**Function** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="92de4-369">설명서 (0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="92de4-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="92de4-370">Parameter (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="92de4-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="92de4-371">CommandText (0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="92de4-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="92de4-372">ReturnType (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="92de4-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="92de4-373">Annotation 요소(0개 이상)</span><span class="sxs-lookup"><span data-stu-id="92de4-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="92de4-374">함수의 반환 형식은 **returntype** 요소 또는 **returntype** 특성 (아래 참조) 중 하나를 사용 하 여 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="92de4-375">스토리지 모델에 지정된 저장 프로시저를 애플리케이션의 개념적 모델로 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="92de4-376">자세한 내용은 [저장 프로시저를 사용 하 여 쿼리](~/ef6/modeling/designer/stored-procedures/query.md)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="92de4-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="92de4-377">또한 **함수** 요소는 저장소 모델에서 사용자 지정 함수를 정의 하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="92de4-378">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="92de4-378">Applicable Attributes</span></span>

<span data-ttu-id="92de4-379">다음 표에서는 **Function** 요소에 적용할 수 있는 특성에 대해 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="92de4-380">일부 특성 (여기에 나열 되지 않음)은 **저장소** 별칭으로 정규화 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="92de4-381">이러한 특성은 모델을 업데이트할 때 모델 업데이트 마법사에서 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="92de4-382">특성 이름</span><span class="sxs-lookup"><span data-stu-id="92de4-382">Attribute Name</span></span>             | <span data-ttu-id="92de4-383">필수 여부</span><span class="sxs-lookup"><span data-stu-id="92de4-383">Is Required</span></span> | <span data-ttu-id="92de4-384">값</span><span class="sxs-lookup"><span data-stu-id="92de4-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="92de4-385">**Name**</span><span class="sxs-lookup"><span data-stu-id="92de4-385">**Name**</span></span>                   | <span data-ttu-id="92de4-386">예</span><span class="sxs-lookup"><span data-stu-id="92de4-386">Yes</span></span>         | <span data-ttu-id="92de4-387">저장 프로시저의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="92de4-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="92de4-388">**ReturnType**</span></span>             | <span data-ttu-id="92de4-389">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-389">No</span></span>          | <span data-ttu-id="92de4-390">저장 프로시저의 반환 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="92de4-391">**Aggregate**</span><span class="sxs-lookup"><span data-stu-id="92de4-391">**Aggregate**</span></span>              | <span data-ttu-id="92de4-392">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-392">No</span></span>          | <span data-ttu-id="92de4-393">저장 프로시저에서 집계 값을 반환 하면 **True** 입니다. 그렇지 않으면 **False**입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="92de4-394">**제공**</span><span class="sxs-lookup"><span data-stu-id="92de4-394">**BuiltIn**</span></span>                | <span data-ttu-id="92de4-395">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-395">No</span></span>          | <span data-ttu-id="92de4-396">함수가 기본 제공<sup>1</sup> 함수 이면 **True** 입니다. 그렇지 않으면 **False**입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="92de4-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="92de4-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="92de4-398">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-398">No</span></span>          | <span data-ttu-id="92de4-399">저장 프로시저의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="92de4-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="92de4-400">**NiladicFunction**</span></span>        | <span data-ttu-id="92de4-401">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-401">No</span></span>          | <span data-ttu-id="92de4-402">함수가 무항<sup>2</sup> 함수 이면 **True** 입니다. 그렇지 않으면 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="92de4-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="92de4-403">**IsComposable**</span></span>           | <span data-ttu-id="92de4-404">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-404">No</span></span>          | <span data-ttu-id="92de4-405">함수가 구성 가능한<sup>3</sup> 함수 이면 **True** 입니다. 그렇지 않으면 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="92de4-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="92de4-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="92de4-407">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-407">No</span></span>          | <span data-ttu-id="92de4-408">함수 오버로드를 확인하는 데 사용되는 형식 의미 체계를 정의하는 열거형입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="92de4-409">열거형은 함수 정의별로 공급자 매니페스트에 정의됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="92de4-410">기본값은 **AllowImplicitConversion**입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="92de4-411">**스키마**</span><span class="sxs-lookup"><span data-stu-id="92de4-411">**Schema**</span></span>                 | <span data-ttu-id="92de4-412">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-412">No</span></span>          | <span data-ttu-id="92de4-413">저장 프로시저가 정의된 스키마의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="92de4-414"><sup>1</sup> 기본 제공 함수는 데이터베이스에 정의 된 함수입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="92de4-415">저장소 모델에 정의 된 함수에 대 한 자세한 내용은 CommandText 요소 (SSDL)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="92de4-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="92de4-416"><sup>2</sup> 무항 함수는 매개 변수를 허용 하지 않는 함수 이며, 호출 되는 경우 괄호가 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="92de4-417"><sup>3</sup> 한 함수의 출력이 다른 함수의 입력이 될 수 있는 경우 두 개의 함수를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="92de4-418">원하는 수의 주석 특성 (사용자 지정 XML 특성)이 **Function** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="92de4-419">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="92de4-420">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-421">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-421">Example</span></span>

<span data-ttu-id="92de4-422">다음 예에서는 **Updateorderquantity** 저장 프로시저에 해당 하는 **함수** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="92de4-423">저장 프로시저는 두 매개 변수를 받아들이며 값을 반환하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-423">The stored procedure accepts two parameters and does not return a value.</span></span>

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

## <a name="key-element-ssdl"></a><span data-ttu-id="92de4-424">Key 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-424">Key Element (SSDL)</span></span>

<span data-ttu-id="92de4-425">SSDL (저장소 스키마 정의 언어)의 **키** 요소는 기본 데이터베이스에 있는 테이블의 기본 키를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="92de4-426">**Key** 는 테이블의 행을 나타내는 EntityType 요소의 자식 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="92de4-427">기본 키는 **EntityType** 요소에 정의 된 하나 이상의 속성 요소를 참조 하 여 **키** 요소에 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="92de4-428">**주요** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="92de4-429">PropertyRef(0개 이상)</span><span class="sxs-lookup"><span data-stu-id="92de4-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="92de4-430">Annotation 요소</span><span class="sxs-lookup"><span data-stu-id="92de4-430">Annotation elements</span></span>

<span data-ttu-id="92de4-431">**키** 요소에 적용할 수 있는 특성이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-432">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-432">Example</span></span>

<span data-ttu-id="92de4-433">다음 예제에서는 하나의 속성을 참조 하는 키가 있는 **EntityType** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

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

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="92de4-434">OnDelete 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="92de4-435">SSDL (저장소 스키마 정의 언어)의 **OnDelete** 요소는 foreign key 제약 조건에 참여 하는 행이 삭제 될 때 데이터베이스 동작을 반영 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="92de4-436">작업을 **Cascade**로 설정 하면 삭제 되는 행을 참조 하는 행도 삭제 됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="92de4-437">Action을 **None**으로 설정 하면 삭제 되는 행을 참조 하는 행도 삭제 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="92de4-438">**OnDelete** 요소는 끝 요소의 자식 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="92de4-439">**OnDelete** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="92de4-440">설명서 (0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="92de4-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="92de4-441">Annotation 요소(0개 이상)</span><span class="sxs-lookup"><span data-stu-id="92de4-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="92de4-442">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="92de4-442">Applicable Attributes</span></span>

<span data-ttu-id="92de4-443">다음 표에서는 **OnDelete** 요소에 적용 될 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="92de4-444">특성 이름</span><span class="sxs-lookup"><span data-stu-id="92de4-444">Attribute Name</span></span> | <span data-ttu-id="92de4-445">필수 여부</span><span class="sxs-lookup"><span data-stu-id="92de4-445">Is Required</span></span> | <span data-ttu-id="92de4-446">값</span><span class="sxs-lookup"><span data-stu-id="92de4-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="92de4-447">**작업**</span><span class="sxs-lookup"><span data-stu-id="92de4-447">**Action**</span></span>     | <span data-ttu-id="92de4-448">예</span><span class="sxs-lookup"><span data-stu-id="92de4-448">Yes</span></span>         | <span data-ttu-id="92de4-449">**Cascade** 또는 **None**입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-449">**Cascade** or **None**.</span></span> <span data-ttu-id="92de4-450">( **제한** 된 값은 유효 하지만 동일한 동작을 포함 하지 **않음**)</span><span class="sxs-lookup"><span data-stu-id="92de4-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="92de4-451">**OnDelete** 요소에는 원하는 수의 주석 특성 (사용자 지정 XML 특성)을 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="92de4-452">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="92de4-453">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-454">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-454">Example</span></span>

<span data-ttu-id="92de4-455">다음 예에서는 **FK\_CustomerOrders** foreign key 제약 조건을 정의 하는 **Association** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="92de4-456">**OnDelete** 요소 **는 customers 테이블** 의 행이 삭제 되는 경우 **customers** 테이블의 특정 행을 참조 하는 **Orders** 테이블의 모든 행이 삭제 됨을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

## <a name="parameter-element-ssdl"></a><span data-ttu-id="92de4-457">Parameter 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="92de4-458">SSDL (저장소 스키마 정의 언어)의 **Parameter** 요소는 데이터베이스의 저장 프로시저에 대 한 매개 변수를 지정 하는 Function 요소의 자식입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="92de4-459">**Parameter** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="92de4-460">설명서 (0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="92de4-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="92de4-461">Annotation 요소(0개 이상)</span><span class="sxs-lookup"><span data-stu-id="92de4-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="92de4-462">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="92de4-462">Applicable Attributes</span></span>

<span data-ttu-id="92de4-463">다음 표에서는 **Parameter** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="92de4-464">특성 이름</span><span class="sxs-lookup"><span data-stu-id="92de4-464">Attribute Name</span></span> | <span data-ttu-id="92de4-465">필수 여부</span><span class="sxs-lookup"><span data-stu-id="92de4-465">Is Required</span></span> | <span data-ttu-id="92de4-466">값</span><span class="sxs-lookup"><span data-stu-id="92de4-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="92de4-467">**Name**</span><span class="sxs-lookup"><span data-stu-id="92de4-467">**Name**</span></span>       | <span data-ttu-id="92de4-468">예</span><span class="sxs-lookup"><span data-stu-id="92de4-468">Yes</span></span>         | <span data-ttu-id="92de4-469">매개 변수의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="92de4-470">**Type**</span><span class="sxs-lookup"><span data-stu-id="92de4-470">**Type**</span></span>       | <span data-ttu-id="92de4-471">예</span><span class="sxs-lookup"><span data-stu-id="92de4-471">Yes</span></span>         | <span data-ttu-id="92de4-472">매개 변수 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="92de4-473">**모드**</span><span class="sxs-lookup"><span data-stu-id="92de4-473">**Mode**</span></span>       | <span data-ttu-id="92de4-474">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-474">No</span></span>          | <span data-ttu-id="92de4-475">매개 변수가 입력, 출력 또는 입/출력 매개 변수 인지에 따라 **In**, **Out**또는 **InOut** 입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="92de4-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="92de4-476">**MaxLength**</span></span>  | <span data-ttu-id="92de4-477">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-477">No</span></span>          | <span data-ttu-id="92de4-478">매개 변수의 최대 길이입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="92de4-479">**전체 자릿수**</span><span class="sxs-lookup"><span data-stu-id="92de4-479">**Precision**</span></span>  | <span data-ttu-id="92de4-480">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-480">No</span></span>          | <span data-ttu-id="92de4-481">매개 변수의 전체 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="92de4-482">**배율**</span><span class="sxs-lookup"><span data-stu-id="92de4-482">**Scale**</span></span>      | <span data-ttu-id="92de4-483">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-483">No</span></span>          | <span data-ttu-id="92de4-484">매개 변수의 소수 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="92de4-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="92de4-485">**SRID**</span></span>       | <span data-ttu-id="92de4-486">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-486">No</span></span>          | <span data-ttu-id="92de4-487">공간 시스템 참조 식별자입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="92de4-488">공간 형식의 매개 변수에만 유효 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="92de4-489">자세한 내용은 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="92de4-489">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="92de4-490">원하는 수의 주석 특성 (사용자 지정 XML 특성)이 **Parameter** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="92de4-491">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="92de4-492">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-493">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-493">Example</span></span>

<span data-ttu-id="92de4-494">다음 예제에서는 입력 매개 변수를 지정 하는 두 개의 **매개 변수** 요소를 포함 하는 **Function** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

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

## <a name="principal-element-ssdl"></a><span data-ttu-id="92de4-495">Principal 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="92de4-496">SSDL (저장소 스키마 정의 언어)의 **principal** 요소는 foreign key 제약 조건의 주 끝을 정의 하는 참조 제약 조건 (참조 제약 조건이 라고도 함)의 하위 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="92de4-497">**Principal** 요소는 다른 열 (또는 열)이 참조 하는 테이블의 기본 키 열을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="92de4-498">**Propertyref** 요소는 참조 되는 열을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="92de4-499">Dependent 요소는 **Principal** 요소에 지정 된 기본 키 열을 참조 하는 열을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="92de4-500">**주요** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="92de4-501">PropertyRef(0개 이상)</span><span class="sxs-lookup"><span data-stu-id="92de4-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="92de4-502">Annotation 요소(0개 이상)</span><span class="sxs-lookup"><span data-stu-id="92de4-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="92de4-503">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="92de4-503">Applicable Attributes</span></span>

<span data-ttu-id="92de4-504">다음 표에서는 **Principal** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="92de4-505">특성 이름</span><span class="sxs-lookup"><span data-stu-id="92de4-505">Attribute Name</span></span> | <span data-ttu-id="92de4-506">필수 여부</span><span class="sxs-lookup"><span data-stu-id="92de4-506">Is Required</span></span> | <span data-ttu-id="92de4-507">값</span><span class="sxs-lookup"><span data-stu-id="92de4-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="92de4-508">**역할**</span><span class="sxs-lookup"><span data-stu-id="92de4-508">**Role**</span></span>       | <span data-ttu-id="92de4-509">예</span><span class="sxs-lookup"><span data-stu-id="92de4-509">Yes</span></span>         | <span data-ttu-id="92de4-510">해당 End 요소의 **Role** 특성 (사용 되는 경우)과 동일한 값입니다. 그렇지 않으면 참조 된 열이 포함 된 테이블의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="92de4-511">모든 수의 주석 특성 (사용자 지정 XML 특성)이 **Principal** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="92de4-512">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="92de4-513">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-514">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-514">Example</span></span>

<span data-ttu-id="92de4-515">다음 예에서는 **키를 사용** 하 여 **FK\_customerorders** foreign key 제약 조건에 참여 하는 열을 지정 하는 Association 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="92de4-516">**Principal** 요소는 **Customer** 테이블의 **CustomerId** 열을 제약 조건의 주 끝으로 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

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

## <a name="property-element-ssdl"></a><span data-ttu-id="92de4-517">Property 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-517">Property Element (SSDL)</span></span>

<span data-ttu-id="92de4-518">SSDL (저장소 스키마 정의 언어)의 **Property** 요소는 기본 데이터베이스의 테이블에 있는 열을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="92de4-519">**속성** 요소는 테이블의 행을 나타내는 EntityType 요소의 자식입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="92de4-520">**EntityType** 요소에 정의 된 각 **Property** 요소는 열을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="92de4-521">**속성** 요소에는 자식 요소가 있을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="92de4-522">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="92de4-522">Applicable Attributes</span></span>

<span data-ttu-id="92de4-523">다음 표에서는 **Property** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="92de4-524">특성 이름</span><span class="sxs-lookup"><span data-stu-id="92de4-524">Attribute Name</span></span>            | <span data-ttu-id="92de4-525">필수 여부</span><span class="sxs-lookup"><span data-stu-id="92de4-525">Is Required</span></span> | <span data-ttu-id="92de4-526">값</span><span class="sxs-lookup"><span data-stu-id="92de4-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="92de4-527">**Name**</span><span class="sxs-lookup"><span data-stu-id="92de4-527">**Name**</span></span>                  | <span data-ttu-id="92de4-528">예</span><span class="sxs-lookup"><span data-stu-id="92de4-528">Yes</span></span>         | <span data-ttu-id="92de4-529">해당 열의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="92de4-530">**Type**</span><span class="sxs-lookup"><span data-stu-id="92de4-530">**Type**</span></span>                  | <span data-ttu-id="92de4-531">예</span><span class="sxs-lookup"><span data-stu-id="92de4-531">Yes</span></span>         | <span data-ttu-id="92de4-532">해당 열의 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="92de4-533">**Null 허용**</span><span class="sxs-lookup"><span data-stu-id="92de4-533">**Nullable**</span></span>              | <span data-ttu-id="92de4-534">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-534">No</span></span>          | <span data-ttu-id="92de4-535">해당 열에 null 값을 사용할 수 있는지 여부에 따라 **True** (기본값) 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="92de4-536">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="92de4-536">**DefaultValue**</span></span>          | <span data-ttu-id="92de4-537">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-537">No</span></span>          | <span data-ttu-id="92de4-538">해당 열의 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="92de4-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="92de4-539">**MaxLength**</span></span>             | <span data-ttu-id="92de4-540">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-540">No</span></span>          | <span data-ttu-id="92de4-541">해당 열의 최대 길이입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="92de4-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="92de4-542">**FixedLength**</span></span>           | <span data-ttu-id="92de4-543">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-543">No</span></span>          | <span data-ttu-id="92de4-544">해당 열 값이 고정 길이 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="92de4-545">**전체 자릿수**</span><span class="sxs-lookup"><span data-stu-id="92de4-545">**Precision**</span></span>             | <span data-ttu-id="92de4-546">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-546">No</span></span>          | <span data-ttu-id="92de4-547">해당 열의 전체 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="92de4-548">**배율**</span><span class="sxs-lookup"><span data-stu-id="92de4-548">**Scale**</span></span>                 | <span data-ttu-id="92de4-549">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-549">No</span></span>          | <span data-ttu-id="92de4-550">해당 열의 소수 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="92de4-551">**유니코드**</span><span class="sxs-lookup"><span data-stu-id="92de4-551">**Unicode**</span></span>               | <span data-ttu-id="92de4-552">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-552">No</span></span>          | <span data-ttu-id="92de4-553">해당 열 값이 유니코드 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="92de4-554">**데이터 정렬**</span><span class="sxs-lookup"><span data-stu-id="92de4-554">**Collation**</span></span>             | <span data-ttu-id="92de4-555">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-555">No</span></span>          | <span data-ttu-id="92de4-556">데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="92de4-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="92de4-557">**SRID**</span></span>                  | <span data-ttu-id="92de4-558">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-558">No</span></span>          | <span data-ttu-id="92de4-559">공간 시스템 참조 식별자입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="92de4-560">공간 형식의 속성에 대해서만 유효 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="92de4-561">자세한 내용은 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="92de4-561">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="92de4-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="92de4-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="92de4-563">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-563">No</span></span>          | <span data-ttu-id="92de4-564">**None**, **identity** (해당 열 값이 데이터베이스에서 생성 된 id 인 경우) 또는 **계산** 된 (해당 열 값이 데이터베이스에서 계산 된 경우)입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="92de4-565">RowType 속성에 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="92de4-566">모든 수의 주석 특성 (사용자 지정 XML 특성)은 **Property** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="92de4-567">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="92de4-568">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-569">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-569">Example</span></span>

<span data-ttu-id="92de4-570">다음 예제에서는 두 개의 자식 **속성** 요소를 사용 하는 **EntityType** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

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

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="92de4-571">PropertyRef 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="92de4-572">SSDL (저장소 스키마 정의 언어)의 **Propertyref** 요소는 EntityType 요소에 정의 된 속성을 참조 하 여 속성이 다음 역할 중 하나를 수행 함을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="92de4-573">**EntityType** 이 나타내는 테이블의 기본 키의 일부 여야 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="92de4-574">하나 이상의 **Propertyref** 요소를 사용 하 여 기본 키를 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="92de4-575">자세한 내용은 Key 요소를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="92de4-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="92de4-576">참조 제약 조건의 종속 또는 주 끝.</span><span class="sxs-lookup"><span data-stu-id="92de4-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="92de4-577">자세한 내용은 ReferentialConstraint 요소를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="92de4-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="92de4-578">**Propertyref** 요소에는 다음 자식 요소만 포함 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="92de4-579">설명서 (0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="92de4-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="92de4-580">Annotation 요소</span><span class="sxs-lookup"><span data-stu-id="92de4-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="92de4-581">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="92de4-581">Applicable Attributes</span></span>

<span data-ttu-id="92de4-582">다음 표에서는 **Propertyref** 요소에 적용 될 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="92de4-583">특성 이름</span><span class="sxs-lookup"><span data-stu-id="92de4-583">Attribute Name</span></span> | <span data-ttu-id="92de4-584">필수 여부</span><span class="sxs-lookup"><span data-stu-id="92de4-584">Is Required</span></span> | <span data-ttu-id="92de4-585">값</span><span class="sxs-lookup"><span data-stu-id="92de4-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="92de4-586">**Name**</span><span class="sxs-lookup"><span data-stu-id="92de4-586">**Name**</span></span>       | <span data-ttu-id="92de4-587">예</span><span class="sxs-lookup"><span data-stu-id="92de4-587">Yes</span></span>         | <span data-ttu-id="92de4-588">참조된 속성의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="92de4-589">원하는 수의 주석 특성 (사용자 지정 XML 특성)이 **Propertyref** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="92de4-590">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="92de4-591">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-592">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-592">Example</span></span>

<span data-ttu-id="92de4-593">다음 예에서는 **EntityType** 요소에 정의 된 속성을 참조 하 여 기본 키를 정의 하는 데 사용 되는 **propertyref** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

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

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="92de4-594">ReferentialConstraint 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="92de4-595">SSDL (저장소 스키마 정의 언어)의 참조 **무결성 제약 조건 요소는** 기본 데이터베이스의 foreign key 제약 조건 (참조 무결성 제약 조건이 라고도 함)을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="92de4-596">제약 조건의 주 끝과 종속 끝은 각각 Principal 및 Dependent 자식 요소로 지정됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="92de4-597">주 끝과 종속 끝에 관여하는 열은 PropertyRef 요소를 사용하여 참조됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="92de4-598">이 **요소는** Association 요소의 선택적 자식 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="92de4-599">AssociationSetMapping 요소가 **Association** 요소에 지정 된 foreign key **제약 조건을 매핑하** 는 데 사용 되지 않는 경우에는이 작업을 수행 하는 데 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="92de4-600">이 **요소에는 다음과** 같은 자식 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="92de4-601">설명서 (0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="92de4-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="92de4-602">Principal(1개만)</span><span class="sxs-lookup"><span data-stu-id="92de4-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="92de4-603">종속 (정확히 한 개)</span><span class="sxs-lookup"><span data-stu-id="92de4-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="92de4-604">Annotation 요소(0개 이상)</span><span class="sxs-lookup"><span data-stu-id="92de4-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="92de4-605">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="92de4-605">Applicable Attributes</span></span>

<span data-ttu-id="92de4-606">원하는 수의 주석 특성 (사용자 지정 XML 특성)은 모두 **해당 요소에** 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="92de4-607">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="92de4-608">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-609">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-609">Example</span></span>

<span data-ttu-id="92de4-610">다음 예에서는 다음과 같이 외래 키 **제약 조건 외래** 키 제약 조건 **\_** 에 참여 하는 열을 지정 하는 데에는 해당 요소를 사용 하는 **Association** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="returntype-element-ssdl"></a><span data-ttu-id="92de4-611">ReturnType 요소 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="92de4-612">SSDL (저장소 스키마 정의 언어)의 **ReturnType** 요소는 **함수** 요소에 정의 된 함수에 대 한 반환 형식을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="92de4-613">함수 반환 형식은 **ReturnType** 특성과 함께 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="92de4-614">함수의 반환 형식은 **type** 특성 또는 **ReturnType** 요소를 사용 하 여 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="92de4-615">**ReturnType** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="92de4-616">CollectionType (one)</span><span class="sxs-lookup"><span data-stu-id="92de4-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="92de4-617">모든 수의 주석 특성 (사용자 지정 XML 특성)은 **ReturnType** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="92de4-618">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="92de4-619">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-620">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-620">Example</span></span>

<span data-ttu-id="92de4-621">다음 예에서는 행 컬렉션을 반환 하는 **함수** 를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-621">The following example uses a **Function** that returns a collection of rows.</span></span>

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


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="92de4-622">RowType 요소 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="92de4-623">SSDL (저장소 스키마 정의 언어)의 **RowType** 요소는 명명 되지 않은 구조체를 저장소에 정의 된 함수에 대 한 반환 형식으로 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="92de4-624">**RowType** 요소는 **CollectionType** 요소의 자식 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="92de4-625">**RowType** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="92de4-626">Property(하나 이상)</span><span class="sxs-lookup"><span data-stu-id="92de4-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="92de4-627">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-627">Example</span></span>

<span data-ttu-id="92de4-628">다음 예에서는 **CollectionType** 요소를 사용 하 여 함수가 **RowType** 요소에 지정 된 대로 행 컬렉션을 반환 하도록 지정 하는 저장소 함수를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


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

## <a name="schema-element-ssdl"></a><span data-ttu-id="92de4-629">스키마 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="92de4-630">SSDL (저장소 스키마 정의 언어)의 **Schema** 요소는 저장소 모델 정의의 루트 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="92de4-631">이 요소에는 스토리지 모델을 구성하는 개체, 함수 및 컨테이너에 대한 정의가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="92de4-632">**Schema** 요소는 다음과 같은 자식 요소를 0 개 이상 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="92de4-633">연결</span><span class="sxs-lookup"><span data-stu-id="92de4-633">Association</span></span>
-   <span data-ttu-id="92de4-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="92de4-634">EntityType</span></span>
-   <span data-ttu-id="92de4-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="92de4-635">EntityContainer</span></span>
-   <span data-ttu-id="92de4-636">함수</span><span class="sxs-lookup"><span data-stu-id="92de4-636">Function</span></span>

<span data-ttu-id="92de4-637">**Schema** 요소는 **네임 스페이스** 특성을 사용 하 여 저장소 모델의 엔터티 형식 및 연결 개체에 대 한 네임 스페이스를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="92de4-638">네임스페이스 내에서 두 개체의 이름이 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="92de4-639">저장소 모델 네임 스페이스는 **Schema** 요소의 XML 네임 스페이스와 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="92de4-640">**네임 스페이스** 특성에 의해 정의 된 저장소 모델 네임 스페이스는 엔터티 형식 및 연결 형식에 대 한 논리적 컨테이너입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="92de4-641">**Schema 요소의 XML** 네임 스페이스 ( **xmlns** 특성으로 표시 됨)는 **schema** 요소의 자식 요소 및 특성에 대 한 기본 네임 스페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="92de4-642">https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl 형식의 XML 네임 스페이스 (YYYY 및 MM은 각각 연도와 월을 나타냄)는 SSDL 용으로 예약 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-642">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="92de4-643">사용자 지정 요소 및 특성은 이러한 형식의 네임스페이스에 있을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="92de4-644">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="92de4-644">Applicable Attributes</span></span>

<span data-ttu-id="92de4-645">다음 표에서는 **Schema** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="92de4-646">특성 이름</span><span class="sxs-lookup"><span data-stu-id="92de4-646">Attribute Name</span></span>            | <span data-ttu-id="92de4-647">필수 여부</span><span class="sxs-lookup"><span data-stu-id="92de4-647">Is Required</span></span> | <span data-ttu-id="92de4-648">값</span><span class="sxs-lookup"><span data-stu-id="92de4-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="92de4-649">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="92de4-649">**Namespace**</span></span>             | <span data-ttu-id="92de4-650">예</span><span class="sxs-lookup"><span data-stu-id="92de4-650">Yes</span></span>         | <span data-ttu-id="92de4-651">스토리지 모델의 네임스페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-651">The namespace of the storage model.</span></span> <span data-ttu-id="92de4-652">**네임 스페이스** 특성의 값은 형식의 정규화 된 이름을 구성 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="92de4-653">예를 들어, 이름이 *Customer* 인 **entitytype** 이 examplemodel.store.customer 네임 스페이스에 있는 경우 **Entitytype** 의 정규화 된 이름은 examplemodel.store.customer입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="92de4-654">다음 문자열은 **Namespace** 특성의 값 ( **시스템**, **임시**또는 **Edm**)으로 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="92de4-655">**Namespace** 특성의 값은 CSDL Schema 요소의 **namespace** 특성에 대 한 값과 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="92de4-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="92de4-656">**Alias**</span></span>                 | <span data-ttu-id="92de4-657">아니요</span><span class="sxs-lookup"><span data-stu-id="92de4-657">No</span></span>          | <span data-ttu-id="92de4-658">네임스페이스 이름 대신 사용되는 식별자입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="92de4-659">예를 들어 이름이 *Customer* 인 **EntityType** 이 examplemodel.store.customer 네임 스페이스에 있고 **Alias** 특성의 값이 *storagemodel.customer*인 경우 storagemodel.customer를 **EntityType** 의 정규화 된 이름으로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="92de4-660">**공급 기업**</span><span class="sxs-lookup"><span data-stu-id="92de4-660">**Provider**</span></span>              | <span data-ttu-id="92de4-661">예</span><span class="sxs-lookup"><span data-stu-id="92de4-661">Yes</span></span>         | <span data-ttu-id="92de4-662">데이터 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="92de4-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="92de4-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="92de4-664">예</span><span class="sxs-lookup"><span data-stu-id="92de4-664">Yes</span></span>         | <span data-ttu-id="92de4-665">반환할 공급자 매니페스트를 공급자에게 나타내는 토큰입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="92de4-666">정의된 토큰의 형식은 없으며</span><span class="sxs-lookup"><span data-stu-id="92de4-666">No format for the token is defined.</span></span> <span data-ttu-id="92de4-667">토큰의 값은 공급자가 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="92de4-668">SQL Server 공급자 매니페스트 토큰에 대 한 자세한 내용은 Entity Framework의 SqlClient를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="92de4-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="92de4-669">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-669">Example</span></span>

<span data-ttu-id="92de4-670">다음 예제에서는 **EntityContainer** 요소, 두 개의 **EntityType** 요소 및 하나의 **Association** 요소를 포함 하는 **스키마** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

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

## <a name="annotation-attributes"></a><span data-ttu-id="92de4-671">주석 특성</span><span class="sxs-lookup"><span data-stu-id="92de4-671">Annotation Attributes</span></span>

<span data-ttu-id="92de4-672">SSDL(스토리지 스키마 정의 언어)의 주석 특성은 스토리지 모델의 요소에 대한 추가 메타데이터를 제공하는 스토리지 모델의 사용자 지정 XML 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="92de4-673">주석 특성은 올바른 XML 구조를 가져야 할 뿐 아니라 다음 제약 조건을 충족해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="92de4-674">주석 특성은 SSDL용으로 예약된 XML 네임스페이스에 속하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="92de4-675">두 주석 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="92de4-676">두 개 이상의 주석 특성을 지정된 SSDL 요소에 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="92de4-677">Annotation 요소에 포함 된 메타 데이터는 런타임에 System.object 네임 스페이스의 클래스를 사용 하 여 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-678">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-678">Example</span></span>

<span data-ttu-id="92de4-679">다음 예에서는 **OrderId** 속성에 적용 된 주석 특성이 있는 EntityType 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="92de4-680">또한이 예제에서는 **EntityType** 요소에 추가 된 주석 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-680">The example also show an annotation element added to the **EntityType** element.</span></span>

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

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="92de4-681">Annotation 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="92de4-682">SSDL(스토리지 스키마 정의 언어)의 Annotation 요소는 스토리지 모델에서 스토리지 모델에 대한 추가 메타데이터를 제공하는 사용자 지정 XML 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="92de4-683">Annotation 요소는 올바른 XML 구조를 가져야 함은 물론 다음 제약 조건이 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="92de4-684">Annotation 요소는 SSDL용으로 예약된 XML 네임스페이스에 속하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="92de4-685">두 Annotation 요소의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="92de4-686">Annotation 요소는 제공된 SSDL 요소의 다른 모든 자식 요소 뒤에 나타나야 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="92de4-687">두 개 이상의 Annotation 요소가 제공된 SSDL 요소의 자식이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="92de4-688">.NET Framework 버전 4부터 annotation 요소에 포함 된 메타 데이터는 런타임에 System.object 네임 스페이스의 클래스를 사용 하 여 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="92de4-689">예제</span><span class="sxs-lookup"><span data-stu-id="92de4-689">Example</span></span>

<span data-ttu-id="92de4-690">다음 예에서는 annotation 요소 (**customelement**)가 있는 EntityType 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="92de4-691">또한이 예제에서는 **OrderId** 속성에 적용 된 주석 특성을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

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

## <a name="facets-ssdl"></a><span data-ttu-id="92de4-692">패싯(SSDL)</span><span class="sxs-lookup"><span data-stu-id="92de4-692">Facets (SSDL)</span></span>

<span data-ttu-id="92de4-693">SSDL(저장소 스키마 정의 언어)의 패싯은 Property 요소에 지정된 열 형식에 대한 제약 조건을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="92de4-694">패싯은 **속성** 요소에서 XML 특성으로 구현 됩니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="92de4-695">다음 표에서는 SSDL에서 지원되는 패싯에 대해 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="92de4-696">패싯</span><span class="sxs-lookup"><span data-stu-id="92de4-696">Facet</span></span>           | <span data-ttu-id="92de4-697">설명</span><span class="sxs-lookup"><span data-stu-id="92de4-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="92de4-698">**데이터 정렬**</span><span class="sxs-lookup"><span data-stu-id="92de4-698">**Collation**</span></span>   | <span data-ttu-id="92de4-699">속성 값에 대한 비교 및 순서 지정 작업을 수행할 때 사용할 데이터 정렬 순서 또는 정렬 순서를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="92de4-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="92de4-700">**FixedLength**</span></span> | <span data-ttu-id="92de4-701">열 값 길이가 다양할 수 있는지 여부를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="92de4-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="92de4-702">**MaxLength**</span></span>   | <span data-ttu-id="92de4-703">열 값의 최대 길이를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="92de4-704">**전체 자릿수**</span><span class="sxs-lookup"><span data-stu-id="92de4-704">**Precision**</span></span>   | <span data-ttu-id="92de4-705">**Decimal**형식의 속성에 대해 속성 값에 포함 될 수 있는 자릿수를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="92de4-706">**Time**, **DateTime**및 **DateTimeOffset**형식의 속성에 대해 열 값의 초 소수 부분 자릿수를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="92de4-707">**배율**</span><span class="sxs-lookup"><span data-stu-id="92de4-707">**Scale**</span></span>       | <span data-ttu-id="92de4-708">열 값에 대한 소수점 오른쪽의 자릿수를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="92de4-709">**유니코드**</span><span class="sxs-lookup"><span data-stu-id="92de4-709">**Unicode**</span></span>     | <span data-ttu-id="92de4-710">열 값을 유니코드로 저장할지 여부를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="92de4-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
