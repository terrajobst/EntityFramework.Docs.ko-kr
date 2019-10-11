---
title: CSDL 사양-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 642e5977ecbbf0c474cac1ceae19d33a135aa875
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182593"
---
# <a name="csdl-specification"></a><span data-ttu-id="e7d59-102">CSDL 사양</span><span class="sxs-lookup"><span data-stu-id="e7d59-102">CSDL Specification</span></span>
<span data-ttu-id="e7d59-103">CSDL(개념 스키마 정의 언어)은 데이터 기반 애플리케이션의 개념적 모델을 구성하는 엔터티, 관계 및 함수를 설명하는 XML 기반 언어입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="e7d59-104">이 개념적 모델은 Entity Framework 또는 WCF Data Services에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="e7d59-105">CSDL로 설명 되는 메타 데이터는 Entity Framework에서 개념적 모델에 정의 된 엔터티 및 관계를 데이터 원본에 매핑하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="e7d59-106">자세한 내용은 [SSDL 사양](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) 및 [MSL 사양](~/ef6/modeling/designer/advanced/edmx/msl-spec.md)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="e7d59-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="e7d59-107">CSDL은 Entity Framework 엔터티 데이터 모델의 구현입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="e7d59-108">Entity Framework 응용 프로그램에서 개념적 모델 메타 데이터는 csdl 파일 (CSDL로 작성 됨)에서 EdmItemCollection 인스턴스로 로드 되며,의 메서드를 사용 하 여 액세스할 수 있습니다. System.object. c a c.</span><span class="sxs-lookup"><span data-stu-id="e7d59-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="e7d59-109">Entity Framework 개념적 모델 메타 데이터를 사용 하 여 개념적 모델에 대 한 쿼리를 데이터 소스 관련 명령으로 변환 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="e7d59-110">EF Designer는 디자인 타임에 개념적 모델 정보를 .edmx 파일에 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="e7d59-111">빌드 시 EF Designer는 .edmx 파일의 정보를 사용 하 여 런타임에 Entity Framework에 필요한 csdl 파일을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="e7d59-112">CSDL 버전은 XML 네임스페이스로 식별됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="e7d59-113">CSDL 버전</span><span class="sxs-lookup"><span data-stu-id="e7d59-113">CSDL Version</span></span> | <span data-ttu-id="e7d59-114">XML 네임 스페이스</span><span class="sxs-lookup"><span data-stu-id="e7d59-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="e7d59-115">CSDL v1</span><span class="sxs-lookup"><span data-stu-id="e7d59-115">CSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="e7d59-116">CSDL v2</span><span class="sxs-lookup"><span data-stu-id="e7d59-116">CSDL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="e7d59-117">CSDL v3</span><span class="sxs-lookup"><span data-stu-id="e7d59-117">CSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="e7d59-118">Association 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-118">Association Element (CSDL)</span></span>

<span data-ttu-id="e7d59-119">**Association** 요소는 두 엔터티 형식 간의 관계를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="e7d59-120">연결은 관계에 관련된 엔터티 형식과 복합성이라도 하는 관계의 각 End에 있는 가능한 엔터티 형식 수를 지정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="e7d59-121">연결 end의 복합성 값은 1 (1), 0 또는 1 (0 .1) 또는 다 수 (\*)가 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="e7d59-122">이 정보는 두 개의 자식 끝 요소에 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="e7d59-123">연결의 한 End에 있는 엔터티 형식 인스턴스는 엔터티 형식에서 노출된 경우 탐색 속성 또는 외래 키를 통해 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="e7d59-124">애플리케이션에서 연결 인스턴스는 엔터티 형식 인스턴스 사이의 특정 연결을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="e7d59-125">연결 인스턴스는 연결 집합에 논리적으로 그룹화됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="e7d59-126">**연결** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-127">설명서 (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="e7d59-128">End (정확히 2 개 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="e7d59-129">(0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="e7d59-130">Annotation 요소 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-131">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-131">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-132">다음 표에서는 **Association** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="e7d59-133">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-133">Attribute Name</span></span> | <span data-ttu-id="e7d59-134">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-134">Is Required</span></span> | <span data-ttu-id="e7d59-135">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="e7d59-136">**이름**</span><span class="sxs-lookup"><span data-stu-id="e7d59-136">**Name**</span></span>       | <span data-ttu-id="e7d59-137">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-137">Yes</span></span>         | <span data-ttu-id="e7d59-138">연결의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="e7d59-139">모든 수의 주석 특성 (사용자 지정 XML 특성)이 **Association** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="e7d59-140">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-141">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-142">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-142">Example</span></span>

<span data-ttu-id="e7d59-143">다음 예제에서는 **고객** 및 **주문** 엔터티 형식에 외래 키가 노출 되지 않은 경우 **customerorders** 연결을 정의 하는 **Association** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="e7d59-144">연결의 각 **End** 에 대 한 **복합성** 값은 여러 **주문이** **고객과**연결 될 수 있지만 한 명의 **고객** 만 **주문과**연결 될 수 있음을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="e7d59-145">또한 **OnDelete** 요소는 **고객이** 삭제 된 경우 특정 **고객과** 관련 되어 있고 ObjectContext로 로드 된 모든 **주문이** 삭제 됨을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="e7d59-146">다음 예제에서는 **고객** 및 **주문** 엔터티 형식에 외래 키가 노출 된 경우 **customerorders** 연결을 정의 하는 **Association** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="e7d59-147">외래 키가 노출 된 상태에서 엔터티 간의 관계는 **키를 사용** 하 여 관리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="e7d59-148">이 연결을 데이터 소스에 매핑할 때는 해당 하는 AssociationSetMapping 요소가 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

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
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="e7d59-149">AssociationSet 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="e7d59-150">CSDL (개념 스키마 정의 언어)의 **AssociationSet** 요소는 동일한 형식의 연결 인스턴스에 대 한 논리적 컨테이너입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="e7d59-151">연결 집합은 연결 인스턴스가 데이터 소스로 매핑될 수 있도록 해당 연결 인스턴스를 그룹화하는 데 필요한 정의를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="e7d59-152">**AssociationSet** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-153">설명서 (0 개 또는 한 개의 요소 허용)</span><span class="sxs-lookup"><span data-stu-id="e7d59-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="e7d59-154">End (정확히 두 개의 요소 필요)</span><span class="sxs-lookup"><span data-stu-id="e7d59-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="e7d59-155">Annotation 요소 (0 개 이상의 요소 허용)</span><span class="sxs-lookup"><span data-stu-id="e7d59-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="e7d59-156">**Association** 특성은 연결 집합이 포함 하는 연결의 유형을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="e7d59-157">연결 집합의 end를 구성 하는 엔터티 집합은 정확히 두 개의 자식 **끝** 요소로 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-158">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-158">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-159">다음 표에서는 **AssociationSet** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="e7d59-160">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-160">Attribute Name</span></span>  | <span data-ttu-id="e7d59-161">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-161">Is Required</span></span> | <span data-ttu-id="e7d59-162">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-163">**이름**</span><span class="sxs-lookup"><span data-stu-id="e7d59-163">**Name**</span></span>        | <span data-ttu-id="e7d59-164">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-164">Yes</span></span>         | <span data-ttu-id="e7d59-165">엔터티 집합의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-165">The name of the entity set.</span></span> <span data-ttu-id="e7d59-166">**Name** 특성의 값은 **Association** 특성의 값과 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="e7d59-167">**연결**</span><span class="sxs-lookup"><span data-stu-id="e7d59-167">**Association**</span></span> | <span data-ttu-id="e7d59-168">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-168">Yes</span></span>         | <span data-ttu-id="e7d59-169">연결 집합에 포함되는 인스턴스의 연결에 대한 정규화된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="e7d59-170">연결은 연결 집합과 동일한 네임스페이스에 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="e7d59-171">임의 개수의 주석 특성 (사용자 지정 XML 특성)이 **AssociationSet** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="e7d59-172">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-173">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-174">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-174">Example</span></span>

<span data-ttu-id="e7d59-175">다음 예에서는 두 개의 **AssociationSet** 요소가 포함 된 **EntityContainer** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

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
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="e7d59-176">CollectionType 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="e7d59-177">CSDL (개념 스키마 정의 언어)의 **CollectionType** 요소는 함수 매개 변수 또는 함수 반환 형식이 컬렉션 임을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="e7d59-178">**CollectionType** 요소는 Parameter 요소 또는 ReturnType (Function) 요소의 자식일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="e7d59-179">**형식** 특성 또는 다음 자식 요소 중 하나를 사용 하 여 컬렉션의 형식을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="e7d59-180">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="e7d59-180">**CollectionType**</span></span>
-   <span data-ttu-id="e7d59-181">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="e7d59-181">ReferenceType</span></span>
-   <span data-ttu-id="e7d59-182">RowType</span><span class="sxs-lookup"><span data-stu-id="e7d59-182">RowType</span></span>
-   <span data-ttu-id="e7d59-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="e7d59-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="e7d59-184">컬렉션의 형식이 **type** 특성과 자식 요소 모두를 사용 하 여 지정 되었는지 여부는 모델에서 유효성을 검사 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-185">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-185">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-186">다음 표에서는 **CollectionType** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="e7d59-187">**DefaultValue**, **MaxLength**, **fixedlength**, **Precision**, **Scale**, **Unicode**및 **Collation** 특성은 **edmsimpletypes**의 컬렉션에만 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="e7d59-188">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-188">Attribute Name</span></span>                                                          | <span data-ttu-id="e7d59-189">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-189">Is Required</span></span> | <span data-ttu-id="e7d59-190">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-191">**형식**</span><span class="sxs-lookup"><span data-stu-id="e7d59-191">**Type**</span></span>                                                                | <span data-ttu-id="e7d59-192">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-192">No</span></span>          | <span data-ttu-id="e7d59-193">컬렉션의 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="e7d59-194">**않는**</span><span class="sxs-lookup"><span data-stu-id="e7d59-194">**Nullable**</span></span>                                                            | <span data-ttu-id="e7d59-195">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-195">No</span></span>          | <span data-ttu-id="e7d59-196">속성에 null 값을 사용할 수 있는지 여부에 따라 **True** (기본값) 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="e7d59-197">CSDL v1에서 복합 형식 속성은 `Nullable="False"` 이어야 합니다. ></span><span class="sxs-lookup"><span data-stu-id="e7d59-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="e7d59-198">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="e7d59-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="e7d59-199">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-199">No</span></span>          | <span data-ttu-id="e7d59-200">속성의 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="e7d59-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="e7d59-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="e7d59-202">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-202">No</span></span>          | <span data-ttu-id="e7d59-203">속성 값의 최대 길이입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="e7d59-204">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="e7d59-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="e7d59-205">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-205">No</span></span>          | <span data-ttu-id="e7d59-206">속성 값이 고정 길이 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="e7d59-207">**전체 자릿수**</span><span class="sxs-lookup"><span data-stu-id="e7d59-207">**Precision**</span></span>                                                           | <span data-ttu-id="e7d59-208">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-208">No</span></span>          | <span data-ttu-id="e7d59-209">속성 값의 전체 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="e7d59-210">**소수 자릿수**</span><span class="sxs-lookup"><span data-stu-id="e7d59-210">**Scale**</span></span>                                                               | <span data-ttu-id="e7d59-211">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-211">No</span></span>          | <span data-ttu-id="e7d59-212">속성 값의 소수 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="e7d59-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="e7d59-213">**SRID**</span></span>                                                                | <span data-ttu-id="e7d59-214">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-214">No</span></span>          | <span data-ttu-id="e7d59-215">공간 시스템 참조 식별자입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="e7d59-216">공간 형식의 속성에 대해서만 유효 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-216">Valid only for properties of spatial types.</span></span><span data-ttu-id="e7d59-217">   자세한 내용은 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) 를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="e7d59-217">   For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="e7d59-218">**유니코드**</span><span class="sxs-lookup"><span data-stu-id="e7d59-218">**Unicode**</span></span>                                                             | <span data-ttu-id="e7d59-219">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-219">No</span></span>          | <span data-ttu-id="e7d59-220">속성 값이 유니코드 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="e7d59-221">**데이터 정렬**</span><span class="sxs-lookup"><span data-stu-id="e7d59-221">**Collation**</span></span>                                                           | <span data-ttu-id="e7d59-222">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-222">No</span></span>          | <span data-ttu-id="e7d59-223">데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="e7d59-224">여러 주석 특성 (사용자 지정 XML 특성)을 **CollectionType** 요소에 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="e7d59-225">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-226">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-227">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-227">Example</span></span>

<span data-ttu-id="e7d59-228">다음 예에서는 **CollectionType** 요소를 사용 하 여 함수가 **Person** 엔터티 형식의 컬렉션을 반환 하도록 지정 하는 모델 정의 함수를 보여 줍니다 ( **ElementType** 특성으로 지정).</span><span class="sxs-lookup"><span data-stu-id="e7d59-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

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
 

<span data-ttu-id="e7d59-229">다음 예에서는 **CollectionType** 요소를 사용 하 여 함수가 **RowType** 요소에 지정 된 대로 행 컬렉션을 반환 하도록 지정 하는 모델 정의 함수를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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
 

<span data-ttu-id="e7d59-230">다음 예에서는 **CollectionType** 요소를 사용 하 여 함수가 **부서** 엔터티 형식의 컬렉션을 매개 변수로 허용 하도록 지정 하는 모델 정의 함수를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

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
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="e7d59-231">ComplexType 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="e7d59-232">**ComplexType** 요소는 **edmsimpletype** 속성 또는 다른 복합 형식으로 구성 된 데이터 구조를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span><span data-ttu-id="e7d59-233">  복합 형식은 엔터티 형식 또는 다른 복합 형식의 속성일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-233">  A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="e7d59-234">복합 형식은 복합 형식에서 데이터를 정의한다는 점에서 엔터티 형식과 유사합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="e7d59-235">그러나 복합 형식과 엔터티 형식 사이에는 약간의 중요한 차이점이 존재합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="e7d59-236">복합 형식은 식별자나 키를 포함하지 않으므로 독립적으로 존재할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="e7d59-237">복합 형식은 엔터티 형식 또는 다른 복합 형식의 속성으로만 존재할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="e7d59-238">복합 형식은 연결에 참여할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="e7d59-239">연결의 끝은 복합 형식이 될 수 없으므로 복합 형식에 대해 탐색 속성을 정의할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="e7d59-240">복합 형식의 스칼라 속성을 각각 null로 설정할 수 있지만 복합 형식 속성의 값은 null일 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="e7d59-241">**ComplexType** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-242">설명서 (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="e7d59-243">Property (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="e7d59-244">Annotation 요소 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="e7d59-245">다음 표에서는 **ComplexType** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="e7d59-246">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="e7d59-247">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-247">Is Required</span></span> | <span data-ttu-id="e7d59-248">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-249">이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-249">Name</span></span>                                                                                                           | <span data-ttu-id="e7d59-250">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-250">Yes</span></span>         | <span data-ttu-id="e7d59-251">복합 형식의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-251">The name of the complex type.</span></span> <span data-ttu-id="e7d59-252">복합 형식의 이름은 모델 범위 내에 있는 다른 복합 형식, 엔터티 형식 또는 연결의 이름과 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="e7d59-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="e7d59-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="e7d59-254">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-254">No</span></span>          | <span data-ttu-id="e7d59-255">정의되는 복합 형식의 기본 형식인 다른 복합 형식의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="e7d59-256">>이 특성은 CSDL v 1에서 적용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="e7d59-257">복합 형식에 대한 상속은 해당 버전에서 지원되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="e7d59-258">이면</span><span class="sxs-lookup"><span data-stu-id="e7d59-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="e7d59-259">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-259">No</span></span>          | <span data-ttu-id="e7d59-260">복합 형식이 추상 형식 인지 여부에 따라 **True** 또는 **False** (기본값)입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="e7d59-261">>이 특성은 CSDL v 1에서 적용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="e7d59-262">해당 버전의 복합 형식은 추상 형식일 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="e7d59-263">모든 수의 주석 특성 (사용자 지정 XML 특성)은 **ComplexType** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="e7d59-264">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-265">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-266">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-266">Example</span></span>

<span data-ttu-id="e7d59-267">다음 예제에서는 **Edmsimpletype** 속성 **StreetAddress**, **City**, **StateOrProvince**, **Country**및 **PostalCode**를 사용 하는 복합 형식 **주소**를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="e7d59-268">복합 형식 **주소** (위)를 엔터티 형식의 속성으로 정의 하려면 엔터티 형식 정의에서 속성 형식을 선언 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="e7d59-269">다음 예제에서는 엔터티 형식 (**게시자**)의 복합 형식인 **Address** 속성을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

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
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="e7d59-270">DefiningExpression 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="e7d59-271">CSDL (개념 스키마 정의 언어)의 **DefiningExpression** 요소에는 개념적 모델에서 함수를 정의 하는 Entity SQL 식이 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="e7d59-272">유효성 검사를 위해 **DefiningExpression** 요소는 임의의 콘텐츠를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="e7d59-273">그러나 **DefiningExpression** 요소가 유효한 Entity SQL를 포함 하지 않는 경우 Entity Framework는 런타임에 예외를 throw 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-274">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-274">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-275">**DefiningExpression** 요소에는 원하는 수의 주석 특성 (사용자 지정 XML 특성)을 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="e7d59-276">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-277">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e7d59-278">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-278">Example</span></span>

<span data-ttu-id="e7d59-279">다음 예에서는 **DefiningExpression** 요소를 사용 하 여 책이 게시 된 이후의 연도 수를 반환 하는 함수를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="e7d59-280">**DefiningExpression** 요소의 내용은 Entity SQL로 작성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="e7d59-281">Dependent 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="e7d59-282">CSDL (개념 스키마 정의 언어)의 **dependent** 요소는 참조 제약 조건의 종속 끝을 정의 하는 참조 제약 조건 요소에 대 한 자식 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="e7d59-283">참조 무결성 제약 **조건** 요소는 관계형 데이터베이스의 참조 무결성 제약 조건과 유사한 기능을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="e7d59-284">데이터베이스 테이블의 열에서 다른 테이블의 기본 키를 참조하는 방식과 동일하게 엔터티 형식의 속성은 다른 엔터티 형식의 엔터티 키를 참조할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="e7d59-285">참조 되는 엔터티 형식을 제약 조건의 *주 끝* 이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="e7d59-286">주 끝을 참조 하는 엔터티 형식을 제약 조건의 *종속 끝* 이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="e7d59-287">**Propertyref** 요소는 주 끝을 참조 하는 키를 지정 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="e7d59-288">**종속** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-289">PropertyRef (하나 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="e7d59-290">Annotation 요소 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-291">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-291">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-292">다음 표에서는 **종속** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="e7d59-293">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-293">Attribute Name</span></span> | <span data-ttu-id="e7d59-294">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-294">Is Required</span></span> | <span data-ttu-id="e7d59-295">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="e7d59-296">**역할**</span><span class="sxs-lookup"><span data-stu-id="e7d59-296">**Role**</span></span>       | <span data-ttu-id="e7d59-297">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-297">Yes</span></span>         | <span data-ttu-id="e7d59-298">연결의 종속 끝에 있는 엔터티 형식의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="e7d59-299">**종속** 요소에는 원하는 수의 주석 특성 (사용자 지정 XML 특성)을 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="e7d59-300">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-301">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-302">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-302">Example</span></span>

<span data-ttu-id="e7d59-303">다음 예제에서는 **PublishedBy** 연결 정의의 일부로 사용 **되는 설명 요소를** 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="e7d59-304">**Book** 엔터티 형식의 **PublisherId** 속성은 참조 제약 조건의 종속 끝을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

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
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="e7d59-305">Documentation 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="e7d59-306">CSDL (개념 스키마 정의 언어)의 **문서** 요소는 부모 요소에 정의 된 개체에 대 한 정보를 제공 하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="e7d59-307">.Edmx 파일에서 **설명서** 요소가 EF 디자이너의 디자인 화면에서 개체로 표시 되는 요소의 자식인 경우 (예: 엔터티, 연결 또는 속성) **설명서** 요소의 내용이 다음에 표시 됩니다. 개체에 대 한 Visual Studio **속성** 창입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="e7d59-308">**문서** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-309">**요약**: 부모 요소에 대 한 간단한 설명입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="e7d59-310">(0개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-310">(zero or one element)</span></span>
-   <span data-ttu-id="e7d59-311">이상 **설명**: 부모 요소에 대 한 광범위 한 설명입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="e7d59-312">(0개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-312">(zero or one element)</span></span>
-   <span data-ttu-id="e7d59-313">Annotation 요소</span><span class="sxs-lookup"><span data-stu-id="e7d59-313">Annotation elements.</span></span> <span data-ttu-id="e7d59-314">(0개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-315">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-315">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-316">주석 특성 (사용자 지정 XML 특성)은 **문서** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="e7d59-317">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-318">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e7d59-319">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-319">Example</span></span>

<span data-ttu-id="e7d59-320">다음 예제에서는 EntityType 요소의 자식 요소인 **문서** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="e7d59-321">아래 코드 조각이 .edmx 파일의 CSDL 콘텐츠에 있었던 경우에는 `Customer` 엔터티 형식을 클릭 하면 **요약** 및 이상 **설명** 요소의 내용이 Visual Studio **속성** 창에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

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
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="e7d59-322">End 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-322">End Element (CSDL)</span></span>

<span data-ttu-id="e7d59-323">CSDL (개념 스키마 정의 언어)의 **End** 요소는 Association 요소 또는 AssociationSet 요소의 자식일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="e7d59-324">각 경우에서 **End** 요소의 역할이 다르고 적용 가능한 특성이 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="e7d59-325">Association 요소의 자식인 End 요소</span><span class="sxs-lookup"><span data-stu-id="e7d59-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="e7d59-326">**End** 요소 ( **association** 요소의 자식)는 연결의 한 end에 있는 엔터티 형식 및 해당 연결의 해당 end에 있을 수 있는 엔터티 형식 인스턴스 수를 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="e7d59-327">연결 End는 연결의 일부로 정의되고 연결에는 정확히 두 개의 연결 End가 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="e7d59-328">연결의 한 End에 있는 엔터티 형식 인스턴스는 엔터티 형식에서 노출된 경우 탐색 속성 또는 외래 키를 통해 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="e7d59-329">**End** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-330">설명서 (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="e7d59-331">OnDelete (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="e7d59-332">Annotation 요소 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-333">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-333">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-334">다음 표에서는 **End** 요소를 **Association** 요소의 자식일 때 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="e7d59-335">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-335">Attribute Name</span></span>   | <span data-ttu-id="e7d59-336">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-336">Is Required</span></span> | <span data-ttu-id="e7d59-337">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-338">**형식**</span><span class="sxs-lookup"><span data-stu-id="e7d59-338">**Type**</span></span>         | <span data-ttu-id="e7d59-339">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-339">Yes</span></span>         | <span data-ttu-id="e7d59-340">연결의 한 End에 있는 엔터티 형식의 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="e7d59-341">**역할**</span><span class="sxs-lookup"><span data-stu-id="e7d59-341">**Role**</span></span>         | <span data-ttu-id="e7d59-342">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-342">No</span></span>          | <span data-ttu-id="e7d59-343">연결 End의 이름.</span><span class="sxs-lookup"><span data-stu-id="e7d59-343">A name for the association end.</span></span> <span data-ttu-id="e7d59-344">이름을 지정하지 않으면 연결 End에 있는 엔터티 형식의 이름이 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="e7d59-345">**복합**</span><span class="sxs-lookup"><span data-stu-id="e7d59-345">**Multiplicity**</span></span> | <span data-ttu-id="e7d59-346">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-346">Yes</span></span>         | <span data-ttu-id="e7d59-347">**1**, **0**.1 또는 **\*** 은 연결의 끝에 있을 수 있는 엔터티 형식 인스턴스 수에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="e7d59-348">**1** 은 정확히 하나의 엔터티 형식 인스턴스가 연결 end에 존재 함을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="e7d59-349">**0 .1** 은 연결 end에 엔터티 형식 인스턴스가 0 개 또는 한 개 존재 함을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="e7d59-350">**\*** 은 연결 end에 엔터티 형식 인스턴스가 0 개 이상 있음을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="e7d59-351">모든 수의 주석 특성 (사용자 지정 XML 특성)은 **끝** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="e7d59-352">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-353">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="e7d59-354">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-354">Example</span></span>

<span data-ttu-id="e7d59-355">다음 예제에서는 **Customerorders** 연결을 정의 하는 **association** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="e7d59-356">연결의 각 **End** 에 대 한 **복합성** 값은 여러 **주문이** **고객과**연결 될 수 있지만 한 명의 **고객** 만 **주문과**연결 될 수 있음을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="e7d59-357">또한 **OnDelete** 요소는 **고객이** 삭제 된 경우 특정 **고객과** 관련 되어 있고 ObjectContext에 로드 된 모든 **주문이** 삭제 됨을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="e7d59-358">AssociationSet 요소의 자식인 End 요소</span><span class="sxs-lookup"><span data-stu-id="e7d59-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="e7d59-359">**End** 요소는 연결 집합의 한쪽 끝을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="e7d59-360">**AssociationSet** 요소는 두 개의 **End** 요소를 포함 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="e7d59-361">**End** 요소에 포함 된 정보는 연결 집합을 데이터 소스에 매핑하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="e7d59-362">**End** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-363">설명서 (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="e7d59-364">Annotation 요소 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="e7d59-365">Annotation 요소는 다른 모든 자식 요소 뒤에 와야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="e7d59-366">Annotation 요소는 CSDL v2 이상 에서만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-367">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-367">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-368">다음 표에서는 **AssociationSet** 요소의 자식인 **End** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="e7d59-369">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-369">Attribute Name</span></span> | <span data-ttu-id="e7d59-370">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-370">Is Required</span></span> | <span data-ttu-id="e7d59-371">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-372">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="e7d59-372">**EntitySet**</span></span>  | <span data-ttu-id="e7d59-373">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-373">Yes</span></span>         | <span data-ttu-id="e7d59-374">부모 **AssociationSet** 요소의 한쪽 끝을 정의 하는 **EntitySet** 요소의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="e7d59-375">**EntitySet** 요소는 부모 **AssociationSet** 요소와 동일한 엔터티 컨테이너에 정의 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="e7d59-376">**역할**</span><span class="sxs-lookup"><span data-stu-id="e7d59-376">**Role**</span></span>       | <span data-ttu-id="e7d59-377">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-377">No</span></span>          | <span data-ttu-id="e7d59-378">연결 집합 End의 이름.</span><span class="sxs-lookup"><span data-stu-id="e7d59-378">The name of the association set end.</span></span> <span data-ttu-id="e7d59-379">**Role** 특성을 사용 하지 않는 경우 연결 집합 끝의 이름은 엔터티 집합의 이름이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="e7d59-380">모든 수의 주석 특성 (사용자 지정 XML 특성)은 **끝** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="e7d59-381">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-382">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="e7d59-383">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-383">Example</span></span>

<span data-ttu-id="e7d59-384">다음 예제에서는 두 개의 **AssociationSet** 요소를 포함 하는 **EntityContainer** 요소를 보여 줍니다. 각 요소에는 두 개의 **End** 요소가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

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
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="e7d59-385">EntityContainer 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="e7d59-386">CSDL (개념 스키마 정의 언어)의 **EntityContainer** 요소는 엔터티 집합, 연결 집합 및 함수 가져오기에 대 한 논리적 컨테이너입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="e7d59-387">개념적 모델 엔터티 컨테이너는 EntityContainerMapping 요소를 통해 저장소 모델 엔터티 컨테이너에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="e7d59-388">스토리지 모델 엔터티 컨테이너는 데이터베이스의 구조를 설명합니다. 엔터티 집합은 데이터베이스의 테이블을 설명하고, 연결 집합은 데이터베이스의 외래 키 제약 조건을 설명하고, 함수 가져오기는 데이터베이스의 저장 프로시저를 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="e7d59-389">**EntityContainer** 요소는 문서 요소를 0 개 또는 1 개 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="e7d59-390">**설명서** 요소가 있으면 모든 **EntitySet**, **AssociationSet**및 **FunctionImport** 요소 앞에와 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="e7d59-391">**EntityContainer** 요소는 나열 된 순서 대로 다음과 같은 자식 요소를 0 개 이상 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-392">EntitySet</span><span class="sxs-lookup"><span data-stu-id="e7d59-392">EntitySet</span></span>
-   <span data-ttu-id="e7d59-393">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="e7d59-393">AssociationSet</span></span>
-   <span data-ttu-id="e7d59-394">FunctionImport</span><span class="sxs-lookup"><span data-stu-id="e7d59-394">FunctionImport</span></span>
-   <span data-ttu-id="e7d59-395">Annotation 요소</span><span class="sxs-lookup"><span data-stu-id="e7d59-395">Annotation elements</span></span>

<span data-ttu-id="e7d59-396">동일한 네임 스페이스 내에 있는 다른 **entitycontainer** 의 콘텐츠를 포함 하도록 **entitycontainer** 요소를 확장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="e7d59-397">다른 **entitycontainer**의 콘텐츠를 포함 하려면 참조 **Entitycontainer** 요소에서 **Extends** 특성의 값을 포함 하려는 **EntityContainer** 요소의 이름으로 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="e7d59-398">포함 된 **entitycontainer** 요소의 모든 자식 요소는 참조 **entitycontainer** 요소의 자식 요소로 처리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-399">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-399">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-400">다음 표에서는 **Using** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="e7d59-401">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-401">Attribute Name</span></span> | <span data-ttu-id="e7d59-402">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-402">Is Required</span></span> | <span data-ttu-id="e7d59-403">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="e7d59-404">**이름**</span><span class="sxs-lookup"><span data-stu-id="e7d59-404">**Name**</span></span>       | <span data-ttu-id="e7d59-405">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-405">Yes</span></span>         | <span data-ttu-id="e7d59-406">엔터티 컨테이너의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="e7d59-407">**제어할**</span><span class="sxs-lookup"><span data-stu-id="e7d59-407">**Extends**</span></span>    | <span data-ttu-id="e7d59-408">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-408">No</span></span>          | <span data-ttu-id="e7d59-409">동일한 네임스페이스 내에 있는 다른 엔터티 컨테이너의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="e7d59-410">여러 주석 특성 (사용자 지정 XML 특성)을 **EntityContainer** 요소에 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="e7d59-411">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-412">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-413">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-413">Example</span></span>

<span data-ttu-id="e7d59-414">다음 예제에서는 세 개의 엔터티 집합과 두 개의 연결 집합을 정의 하는 **EntityContainer** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

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
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="e7d59-415">EntitySet 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="e7d59-416">개념 스키마 정의 언어의 **EntitySet** 요소는 엔터티 형식 및 해당 엔터티 형식에서 파생 된 모든 형식의 인스턴스에 대 한 논리적 컨테이너입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="e7d59-417">엔터티 형식과 엔터티 집합 간의 관계는 관계형 데이터베이스의 행과 테이블 간 관계와 유사합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="e7d59-418">행과 같이 엔터티 형식은 관련 데이터 집합을 정의하고, 테이블과 같이 엔터티 집합은 이러한 정의의 인스턴스를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="e7d59-419">엔터티 집합은 엔터티 형식 인스턴스가 데이터 소스의 관련 데이터 구조에 매핑될 수 있도록 이러한 인스턴스를 그룹화하는 구문을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="e7d59-420">특정 엔터티 형식에 대한 두 개 이상의 엔터티 집합을 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="e7d59-421">EF Designer는 유형별 다중 엔터티 집합이 포함 된 개념적 모델을 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="e7d59-422">**EntitySet** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-423">문서 요소 (0 개 또는 한 개의 요소 허용)</span><span class="sxs-lookup"><span data-stu-id="e7d59-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="e7d59-424">Annotation 요소 (0 개 이상의 요소 허용)</span><span class="sxs-lookup"><span data-stu-id="e7d59-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-425">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-425">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-426">다음 표에서는 **EntitySet** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="e7d59-427">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-427">Attribute Name</span></span> | <span data-ttu-id="e7d59-428">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-428">Is Required</span></span> | <span data-ttu-id="e7d59-429">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-430">**이름**</span><span class="sxs-lookup"><span data-stu-id="e7d59-430">**Name**</span></span>       | <span data-ttu-id="e7d59-431">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-431">Yes</span></span>         | <span data-ttu-id="e7d59-432">엔터티 집합의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="e7d59-433">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="e7d59-433">**EntityType**</span></span> | <span data-ttu-id="e7d59-434">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-434">Yes</span></span>         | <span data-ttu-id="e7d59-435">엔터티 집합에 포함되는 인스턴스의 엔터티 형식에 대한 정규화된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="e7d59-436">임의 개수의 주석 특성 (사용자 지정 XML 특성)은 **EntitySet** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="e7d59-437">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-438">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-439">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-439">Example</span></span>

<span data-ttu-id="e7d59-440">다음 예제에서는 세 개의 **EntitySet** 요소가 포함 된 **EntityContainer** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

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
 

<span data-ttu-id="e7d59-441">형식당 여러 엔터티 집합(MEST)을 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="e7d59-442">다음 예제에서는 **Book** 엔터티 형식에 대 한 두 개의 엔터티 집합이 있는 엔터티 컨테이너를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

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
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="e7d59-443">EntityType 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="e7d59-444">**EntityType** 요소는 개념적 모델에서 고객 또는 주문과 같은 최상위 개념의 구조를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="e7d59-445">엔터티 형식은 애플리케이션에서 엔터티 형식 인스턴스에 대한 템플릿입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="e7d59-446">각 템플릿에는 다음 정보가 들어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="e7d59-447">고유한 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-447">A unique name.</span></span> <span data-ttu-id="e7d59-448">(필수)</span><span class="sxs-lookup"><span data-stu-id="e7d59-448">(Required.)</span></span>
-   <span data-ttu-id="e7d59-449">하나 이상의 속성에 의해 정의되는 엔터티 키</span><span class="sxs-lookup"><span data-stu-id="e7d59-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="e7d59-450">(필수)</span><span class="sxs-lookup"><span data-stu-id="e7d59-450">(Required.)</span></span>
-   <span data-ttu-id="e7d59-451">상위 데이터의 속성</span><span class="sxs-lookup"><span data-stu-id="e7d59-451">Properties for containing data.</span></span> <span data-ttu-id="e7d59-452">필요에 따라</span><span class="sxs-lookup"><span data-stu-id="e7d59-452">(Optional.)</span></span>
-   <span data-ttu-id="e7d59-453">연결의 한 End에서 다른 End로의 탐색을 허용하는 탐색 속성</span><span class="sxs-lookup"><span data-stu-id="e7d59-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="e7d59-454">필요에 따라</span><span class="sxs-lookup"><span data-stu-id="e7d59-454">(Optional.)</span></span>

<span data-ttu-id="e7d59-455">애플리케이션에서 엔터티 형식의 인스턴스는 특정 고객 또는 주문과 같은 특정 개체를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="e7d59-456">엔터티 집합 내에 각 엔터티 형식 인스턴스에 대한 고유한 엔터티 키가 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="e7d59-457">두 엔터티 형식 인스턴스는 형식이 동일하고 해당 엔터티 키 값이 동일한 경우에만 동일한 것으로 간주됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="e7d59-458">**EntityType** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-459">설명서 (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="e7d59-460">Key (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="e7d59-461">Property (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="e7d59-462">NavigationProperty (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="e7d59-463">Annotation 요소 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-464">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-464">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-465">다음 표에서는 **EntityType** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="e7d59-466">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="e7d59-467">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-467">Is Required</span></span> | <span data-ttu-id="e7d59-468">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-469">**이름**</span><span class="sxs-lookup"><span data-stu-id="e7d59-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="e7d59-470">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-470">Yes</span></span>         | <span data-ttu-id="e7d59-471">엔터티 형식의 이름.</span><span class="sxs-lookup"><span data-stu-id="e7d59-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="e7d59-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="e7d59-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="e7d59-473">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-473">No</span></span>          | <span data-ttu-id="e7d59-474">정의되는 엔터티 형식의 기본 형식인 다른 엔터티 형식의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="e7d59-475">**이면**</span><span class="sxs-lookup"><span data-stu-id="e7d59-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="e7d59-476">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-476">No</span></span>          | <span data-ttu-id="e7d59-477">엔터티 형식이 추상 형식 인지 여부에 따라 **True** 또는 **False**입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="e7d59-478">**Open**</span><span class="sxs-lookup"><span data-stu-id="e7d59-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="e7d59-479">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-479">No</span></span>          | <span data-ttu-id="e7d59-480">엔터티 형식이 개방형 엔터티 형식 인지 여부에 따라 **True** 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="e7d59-481">> **OpenType** 특성은 ADO.NET Data Services와 함께 사용 되는 개념적 모델에 정의 된 엔터티 형식에만 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="e7d59-482">**EntityType** 요소에는 원하는 수의 주석 특성 (사용자 지정 XML 특성)을 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="e7d59-483">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-484">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-485">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-485">Example</span></span>

<span data-ttu-id="e7d59-486">다음 예제에서는 세 개의 **속성** 요소와 **NavigationProperty** 요소가 포함 된 **EntityType** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

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
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="e7d59-487">EnumType 요소 (CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="e7d59-488">**EnumType** 요소는 열거 형식을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="e7d59-489">**EnumType** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-490">설명서 (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="e7d59-491">Member (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="e7d59-492">Annotation 요소 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-493">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-493">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-494">다음 표에서는 **EnumType** 요소에 적용 될 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="e7d59-495">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-495">Attribute Name</span></span>     | <span data-ttu-id="e7d59-496">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-496">Is Required</span></span> | <span data-ttu-id="e7d59-497">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-498">**이름**</span><span class="sxs-lookup"><span data-stu-id="e7d59-498">**Name**</span></span>           | <span data-ttu-id="e7d59-499">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-499">Yes</span></span>         | <span data-ttu-id="e7d59-500">엔터티 형식의 이름.</span><span class="sxs-lookup"><span data-stu-id="e7d59-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="e7d59-501">**IsFlags**</span><span class="sxs-lookup"><span data-stu-id="e7d59-501">**IsFlags**</span></span>        | <span data-ttu-id="e7d59-502">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-502">No</span></span>          | <span data-ttu-id="e7d59-503">열거형 형식을 플래그 집합으로 사용할 수 있는지 여부에 따라 **True** 또는 **False**입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="e7d59-504">기본값은 **False입니다**.</span><span class="sxs-lookup"><span data-stu-id="e7d59-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="e7d59-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="e7d59-505">**UnderlyingType**</span></span> | <span data-ttu-id="e7d59-506">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-506">No</span></span>          | <span data-ttu-id="e7d59-507">형식의 값 범위를 정의 하는 **edm.** **Byte**, **edm. Int16** **, edm. i**n t **32입니다.**</span><span class="sxs-lookup"><span data-stu-id="e7d59-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span> <span data-ttu-id="e7d59-508">  열거형 요소의 기본 기본 형식은 **Edm. Int32입니다**.</span><span class="sxs-lookup"><span data-stu-id="e7d59-508">  The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="e7d59-509">**EnumType** 요소에는 원하는 수의 주석 특성 (사용자 지정 XML 특성)을 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="e7d59-510">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-511">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-512">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-512">Example</span></span>

<span data-ttu-id="e7d59-513">다음 예제에서는 세 개의 **멤버** 요소를 포함 하는 **EnumType** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="e7d59-514">Function 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-514">Function Element (CSDL)</span></span>

<span data-ttu-id="e7d59-515">CSDL (개념 스키마 정의 언어)의 **Function** 요소는 개념적 모델에서 함수를 정의 하거나 선언 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="e7d59-516">함수는 DefiningExpression 요소를 사용 하 여 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="e7d59-517">**함수** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-518">설명서 (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="e7d59-519">Parameter (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="e7d59-520">DefiningExpression (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="e7d59-521">ReturnType (함수) (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="e7d59-522">Annotation 요소 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="e7d59-523">함수에 대 한 반환 형식은 **returntype** (function) 요소 또는 **returntype** 특성 (아래 참조) 중 하나를 사용 하 여 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="e7d59-524">EdmSimpleType, 엔터티 형식, 복합 형식, 행 형식 또는 참조 형식(또는 이러한 형식 중 하나의 컬렉션)이 반환 형식이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-525">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-525">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-526">다음 표에서는 **Function** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="e7d59-527">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-527">Attribute Name</span></span> | <span data-ttu-id="e7d59-528">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-528">Is Required</span></span> | <span data-ttu-id="e7d59-529">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="e7d59-530">**이름**</span><span class="sxs-lookup"><span data-stu-id="e7d59-530">**Name**</span></span>       | <span data-ttu-id="e7d59-531">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-531">Yes</span></span>         | <span data-ttu-id="e7d59-532">함수의 이름.</span><span class="sxs-lookup"><span data-stu-id="e7d59-532">The name of the function.</span></span>          |
| <span data-ttu-id="e7d59-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="e7d59-533">**ReturnType**</span></span> | <span data-ttu-id="e7d59-534">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-534">No</span></span>          | <span data-ttu-id="e7d59-535">함수에서 반환하는 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="e7d59-536">원하는 수의 주석 특성 (사용자 지정 XML 특성)이 **Function** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="e7d59-537">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-538">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-539">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-539">Example</span></span>

<span data-ttu-id="e7d59-540">다음 예에서는 **함수** 요소를 사용 하 여 강사가 고용 된 이후 연도 수를 반환 하는 함수를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="e7d59-541">FunctionImport 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="e7d59-542">CSDL (개념 스키마 정의 언어)의 **FunctionImport** 요소는 데이터 소스에 정의 되어 있지만 개념적 모델을 통해 개체에서 사용할 수 있는 함수를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="e7d59-543">예를 들어 저장소 모델의 Function 요소를 사용 하 여 데이터베이스의 저장 프로시저를 나타낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="e7d59-544">개념적 모델의 **FunctionImport** 요소는 Entity Framework 응용 프로그램의 해당 함수를 나타내며 FunctionImportMapping 요소를 사용 하 여 저장소 모델 함수에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="e7d59-545">애플리케이션에서 함수가 호출되면 해당되는 저장 프로시저가 데이터베이스에서 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="e7d59-546">**FunctionImport** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-547">설명서 (0 개 또는 한 개의 요소 허용)</span><span class="sxs-lookup"><span data-stu-id="e7d59-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="e7d59-548">Parameter (0 개 이상의 요소 허용)</span><span class="sxs-lookup"><span data-stu-id="e7d59-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="e7d59-549">Annotation 요소 (0 개 이상의 요소 허용)</span><span class="sxs-lookup"><span data-stu-id="e7d59-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="e7d59-550">ReturnType (FunctionImport) (0 개 이상의 요소 허용)</span><span class="sxs-lookup"><span data-stu-id="e7d59-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="e7d59-551">함수가 허용 하는 각 매개 변수에 대해 하나의 **매개 변수** 요소를 정의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="e7d59-552">함수에 대 한 반환 형식은 **returntype** (FunctionImport) 요소 또는 **returntype** 특성 (아래 참조) 중 하나를 사용 하 여 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="e7d59-553">반환 형식 값은 EdmSimpleType, EntityType 또는 ComplexType의 컬렉션 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-554">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-554">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-555">다음 표에서는 **FunctionImport** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="e7d59-556">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-556">Attribute Name</span></span>   | <span data-ttu-id="e7d59-557">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-557">Is Required</span></span> | <span data-ttu-id="e7d59-558">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-559">**이름**</span><span class="sxs-lookup"><span data-stu-id="e7d59-559">**Name**</span></span>         | <span data-ttu-id="e7d59-560">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-560">Yes</span></span>         | <span data-ttu-id="e7d59-561">가져온 함수의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="e7d59-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="e7d59-562">**ReturnType**</span></span>   | <span data-ttu-id="e7d59-563">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-563">No</span></span>          | <span data-ttu-id="e7d59-564">함수에서 반환하는 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-564">The type that the function returns.</span></span> <span data-ttu-id="e7d59-565">함수에서 값을 반환하지 않으면 이 특성을 사용하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="e7d59-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="e7d59-566">그렇지 않으면 값은 ComplexType, EntityType 또는 EDMSimpleType의 컬렉션 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="e7d59-567">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="e7d59-567">**EntitySet**</span></span>    | <span data-ttu-id="e7d59-568">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-568">No</span></span>          | <span data-ttu-id="e7d59-569">함수가 엔터티 형식의 컬렉션을 반환 하는 경우 **EntitySet** 의 값은 컬렉션이 속한 엔터티 집합 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="e7d59-570">그렇지 않으면 **EntitySet** 특성을 사용 하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="e7d59-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="e7d59-571">**IsComposable**</span></span> | <span data-ttu-id="e7d59-572">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-572">No</span></span>          | <span data-ttu-id="e7d59-573">값이 true로 설정 된 경우 함수는 구성 가능 (테이블 반환 함수) 이며 LINQ 쿼리에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span><span data-ttu-id="e7d59-574">  기본값은 **false**입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-574">  The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="e7d59-575">모든 수의 주석 특성 (사용자 지정 XML 특성)이 **FunctionImport** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="e7d59-576">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-577">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-578">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-578">Example</span></span>

<span data-ttu-id="e7d59-579">다음 예제에서는 하나의 매개 변수를 받아들이고 엔터티 형식의 컬렉션을 반환 하는 **FunctionImport** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="e7d59-580">Key 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-580">Key Element (CSDL)</span></span>

<span data-ttu-id="e7d59-581">**Key** 요소는 EntityType 요소의 자식 요소 이며 *엔터티 키* (id를 확인 하는 엔터티 형식의 속성 또는 속성 집합)를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="e7d59-582">엔터티 키를 구성하는 속성은 디자인 타임에 선택됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="e7d59-583">엔터티 키 속성의 값은 런타임에 엔터티 집합 내에서 엔터티 형식 인스턴스를 고유하게 식별해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="e7d59-584">엔터티 키를 구성하는 속성을 선택하여 엔터티 집합에서 인스턴스의 고유성을 보장해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="e7d59-585">**키** 요소는 엔터티 형식의 속성을 하나 이상 참조 하 여 엔터티 키를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="e7d59-586">**키** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="e7d59-587">PropertyRef (하나 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="e7d59-588">Annotation 요소 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-589">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-589">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-590">**키** 요소에는 원하는 수의 주석 특성 (사용자 지정 XML 특성)을 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="e7d59-591">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-592">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e7d59-593">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-593">Example</span></span>

<span data-ttu-id="e7d59-594">아래 예제에서는 **Book**이라는 엔터티 형식을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="e7d59-595">엔터티 키는 엔터티 형식의 **ISBN** 속성을 참조 하 여 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="e7d59-596">Isbn (국제 표준 책 번호)은 책을 고유 하 게 식별 하기 때문에 **isbn** 속성은 엔터티 키에 적합 한 선택입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="e7d59-597">다음 예제에서는 두 개의 속성 **이름** 및 **주소로**구성 된 엔터티 키가 있는 엔터티 형식 (**Author**)을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

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
 

<span data-ttu-id="e7d59-598">이름 **및** **주소** 를 사용 하는 것은 동일한 이름의 작성자 두 명이 동일한 주소에 존재할 가능성이 없기 때문에 적절 한 선택입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="e7d59-599">그러나 이러한 엔터티 키 선택이 엔터티 집합에서의 고유한 엔터티 키를 절대적으로 보장하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="e7d59-600">이 경우 작성자를 고유 하 게 식별 하는 데 사용할 수 있는 **AuthorId**과 같은 속성을 추가 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="e7d59-601">Member 요소 (CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-601">Member Element (CSDL)</span></span>

<span data-ttu-id="e7d59-602">**Member** 요소는 EnumType 요소의 자식 요소 이며 열거 형식의 멤버를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-603">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-603">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-604">다음 표에서는 **FunctionImport** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="e7d59-605">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-605">Attribute Name</span></span> | <span data-ttu-id="e7d59-606">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-606">Is Required</span></span> | <span data-ttu-id="e7d59-607">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-608">**이름**</span><span class="sxs-lookup"><span data-stu-id="e7d59-608">**Name**</span></span>       | <span data-ttu-id="e7d59-609">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-609">Yes</span></span>         | <span data-ttu-id="e7d59-610">멤버의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="e7d59-611">**Value**</span><span class="sxs-lookup"><span data-stu-id="e7d59-611">**Value**</span></span>      | <span data-ttu-id="e7d59-612">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-612">No</span></span>          | <span data-ttu-id="e7d59-613">멤버의 값입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-613">The value of the member.</span></span> <span data-ttu-id="e7d59-614">기본적으로 첫 번째 멤버의 값은 0이 고 각 연속 열거자의 값은 1 씩 증가 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="e7d59-615">값이 같은 멤버가 여러 개 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="e7d59-616">모든 수의 주석 특성 (사용자 지정 XML 특성)이 **FunctionImport** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="e7d59-617">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-618">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-619">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-619">Example</span></span>

<span data-ttu-id="e7d59-620">다음 예제에서는 세 개의 **멤버** 요소를 포함 하는 **EnumType** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="e7d59-621">NavigationProperty 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="e7d59-622">**NavigationProperty** 요소는 연결의 다른 쪽 end에 대 한 참조를 제공 하는 탐색 속성을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="e7d59-623">속성 요소를 사용 하 여 정의 된 속성과 달리 탐색 속성은 데이터의 모양 및 특징을 정의 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="e7d59-624">이러한 속성은 두 엔터티 형식 사이의 연결을 탐색하는 방법을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="e7d59-625">탐색 속성은 연결의 양쪽 End에 있는 엔터티 형식 모두에 대해 선택적 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="e7d59-626">연결의 End에 있는 한 엔터티 형식에 대해 탐색 속성을 정의하는 경우 연결의 다른 End에 있는 엔터티 형식에 대해서는 탐색 속성을 정의할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="e7d59-627">탐색 속성에 의해 반환된 데이터 형식은 해당 원격 연결 End의 복합성에 의해 결정됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="e7d59-628">예를 들어 Order **Snavprop**탐색 속성이 **customer** 엔터티 형식에 존재 하 고 **고객과** **주문**간에 일 대 다 연결을 탐색 한다고 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="e7d59-629">탐색 속성에 대 한 원격 연결 end의 복합성은 다 수 (\*) 이므로 해당 데이터 형식은 **Order**의 컬렉션입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="e7d59-630">마찬가지로, 탐색 속성인 **CustomerNavProp**가 **Order** 엔터티 형식에 있는 경우 원격 끝의 복합성은 one (1) 이므로 해당 데이터 형식은 **Customer** 가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="e7d59-631">**NavigationProperty** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-632">설명서 (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="e7d59-633">Annotation 요소 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-634">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-634">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-635">다음 표에서는 **NavigationProperty** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="e7d59-636">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-636">Attribute Name</span></span>   | <span data-ttu-id="e7d59-637">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-637">Is Required</span></span> | <span data-ttu-id="e7d59-638">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-639">**이름**</span><span class="sxs-lookup"><span data-stu-id="e7d59-639">**Name**</span></span>         | <span data-ttu-id="e7d59-640">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-640">Yes</span></span>         | <span data-ttu-id="e7d59-641">탐색 속성의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="e7d59-642">**관계만**</span><span class="sxs-lookup"><span data-stu-id="e7d59-642">**Relationship**</span></span> | <span data-ttu-id="e7d59-643">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-643">Yes</span></span>         | <span data-ttu-id="e7d59-644">모델 범위 내에 있는 연결의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="e7d59-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="e7d59-645">**ToRole**</span></span>       | <span data-ttu-id="e7d59-646">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-646">Yes</span></span>         | <span data-ttu-id="e7d59-647">탐색이 끝나는 연결의 End입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="e7d59-648">**Torole** 특성의 값은 association End (AssociationEnd 요소에 정의 됨) 중 하나에 정의 된 **Role** 특성 중 하나의 값과 동일 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="e7d59-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="e7d59-649">**FromRole**</span></span>     | <span data-ttu-id="e7d59-650">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-650">Yes</span></span>         | <span data-ttu-id="e7d59-651">탐색이 시작되는 연결의 End입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="e7d59-652">**Fromrole** 특성의 값은 association End (AssociationEnd 요소에 정의 됨) 중 하나에 정의 된 **Role** 특성 중 하나의 값과 동일 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="e7d59-653">원하는 수의 주석 특성 (사용자 지정 XML 특성)이 **NavigationProperty** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="e7d59-654">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-655">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-656">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-656">Example</span></span>

<span data-ttu-id="e7d59-657">다음 예제에서는 두 개의 탐색 속성 (**PublishedBy** 및 **WrittenBy**)을 사용 하 여 엔터티 형식 (**Book**)을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

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
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="e7d59-658">OnDelete 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="e7d59-659">CSDL (개념 스키마 정의 언어)의 **OnDelete** 요소는 연결과 연결 된 동작을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="e7d59-660">**작업** 특성이 연결의 한쪽 끝에서 **Cascade** 로 설정 된 경우 첫 번째 끝의 엔터티 형식이 삭제 되 면 연결의 반대쪽 끝에 있는 관련 엔터티 형식이 삭제 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="e7d59-661">두 엔터티 형식 간의 연결이 기본 키 대 기본 키 관계인 경우 **OnDelete** 사양에 관계 없이 연결의 다른 쪽 end에 있는 주 개체가 삭제 되 면 로드 된 종속 개체가 삭제 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="e7d59-662">**OnDelete** 요소는 응용 프로그램의 런타임 동작에만 영향을 줍니다. 데이터 원본의 동작에는 영향을 주지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="e7d59-663">데이터 소스에 정의된 동작은 애플리케이션에 정의된 동작과 같아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="e7d59-664">**OnDelete** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-665">설명서 (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="e7d59-666">Annotation 요소 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-667">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-667">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-668">다음 표에서는 **OnDelete** 요소에 적용 될 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="e7d59-669">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-669">Attribute Name</span></span> | <span data-ttu-id="e7d59-670">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-670">Is Required</span></span> | <span data-ttu-id="e7d59-671">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-672">**작업**</span><span class="sxs-lookup"><span data-stu-id="e7d59-672">**Action**</span></span>     | <span data-ttu-id="e7d59-673">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-673">Yes</span></span>         | <span data-ttu-id="e7d59-674">**Cascade** 또는 **None**입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-674">**Cascade** or **None**.</span></span> <span data-ttu-id="e7d59-675">**Cascade**인 경우 주 엔터티 형식이 삭제 되 면 종속 엔터티 형식이 삭제 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="e7d59-676">**None**인 경우 주 엔터티 형식이 삭제 되 면 종속 엔터티 형식이 삭제 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="e7d59-677">모든 수의 주석 특성 (사용자 지정 XML 특성)이 **Association** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="e7d59-678">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-679">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-680">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-680">Example</span></span>

<span data-ttu-id="e7d59-681">다음 예제에서는 **Customerorders** 연결을 정의 하는 **association** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="e7d59-682">**OnDelete** 요소는 **고객이** 삭제 될 때 특정 **고객과** 관련 되어 있고 ObjectContext로 로드 된 모든 **주문이** 삭제 됨을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="e7d59-683">Parameter 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="e7d59-684">CSDL (개념 스키마 정의 언어)의 **Parameter** 요소는 FunctionImport 요소 또는 Function 요소의 자식일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="e7d59-685">FunctionImport 요소 적용</span><span class="sxs-lookup"><span data-stu-id="e7d59-685">FunctionImport Element Application</span></span>

<span data-ttu-id="e7d59-686">**매개 변수** 요소 ( **FunctionImport** 요소의 자식)는 CSDL에 선언 된 함수 가져오기에 대 한 입력 및 출력 매개 변수를 정의 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="e7d59-687">**Parameter** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-688">설명서 (0 개 또는 한 개의 요소 허용)</span><span class="sxs-lookup"><span data-stu-id="e7d59-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="e7d59-689">Annotation 요소 (0 개 이상의 요소 허용)</span><span class="sxs-lookup"><span data-stu-id="e7d59-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-690">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-690">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-691">다음 표에서는 **매개 변수** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="e7d59-692">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-692">Attribute Name</span></span> | <span data-ttu-id="e7d59-693">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-693">Is Required</span></span> | <span data-ttu-id="e7d59-694">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-695">**이름**</span><span class="sxs-lookup"><span data-stu-id="e7d59-695">**Name**</span></span>       | <span data-ttu-id="e7d59-696">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-696">Yes</span></span>         | <span data-ttu-id="e7d59-697">매개 변수의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="e7d59-698">**형식**</span><span class="sxs-lookup"><span data-stu-id="e7d59-698">**Type**</span></span>       | <span data-ttu-id="e7d59-699">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-699">Yes</span></span>         | <span data-ttu-id="e7d59-700">매개 변수 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-700">The parameter type.</span></span> <span data-ttu-id="e7d59-701">값은 모델의 범위 내에 있는 **Edmsimpletype** 또는 복합 유형 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="e7d59-702">**모드**</span><span class="sxs-lookup"><span data-stu-id="e7d59-702">**Mode**</span></span>       | <span data-ttu-id="e7d59-703">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-703">No</span></span>          | <span data-ttu-id="e7d59-704">**In**, **초과**, 또는 **InOut** 매개 변수는 입력, 출력 또는 입출력 매개 변수 인지에 따라 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="e7d59-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="e7d59-705">**MaxLength**</span></span>  | <span data-ttu-id="e7d59-706">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-706">No</span></span>          | <span data-ttu-id="e7d59-707">매개 변수의 최대 허용 길이입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="e7d59-708">**전체 자릿수**</span><span class="sxs-lookup"><span data-stu-id="e7d59-708">**Precision**</span></span>  | <span data-ttu-id="e7d59-709">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-709">No</span></span>          | <span data-ttu-id="e7d59-710">매개 변수의 전체 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="e7d59-711">**소수 자릿수**</span><span class="sxs-lookup"><span data-stu-id="e7d59-711">**Scale**</span></span>      | <span data-ttu-id="e7d59-712">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-712">No</span></span>          | <span data-ttu-id="e7d59-713">매개 변수의 소수 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="e7d59-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="e7d59-714">**SRID**</span></span>       | <span data-ttu-id="e7d59-715">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-715">No</span></span>          | <span data-ttu-id="e7d59-716">공간 시스템 참조 식별자입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="e7d59-717">공간 형식의 매개 변수에만 유효 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="e7d59-718">자세한 내용은 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="e7d59-718">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="e7d59-719">원하는 수의 주석 특성 (사용자 지정 XML 특성)이 **Parameter** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="e7d59-720">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-721">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="e7d59-722">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-722">Example</span></span>

<span data-ttu-id="e7d59-723">다음 예제에서는 하나의 **매개 변수** 자식 요소가 있는 **FunctionImport** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="e7d59-724">함수는 하나의 입력 매개 변수를 받아들이고 엔터티 형식의 컬렉션을 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="e7d59-725">Function 요소 적용</span><span class="sxs-lookup"><span data-stu-id="e7d59-725">Function Element Application</span></span>

<span data-ttu-id="e7d59-726">**Parameter** 요소 ( **Function** 요소의 자식)는 개념적 모델에 정의 되거나 선언 된 함수에 대 한 매개 변수를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="e7d59-727">**Parameter** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-728">설명서 (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="e7d59-729">CollectionType (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="e7d59-730">ReferenceType (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="e7d59-731">RowType (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="e7d59-732">**CollectionType**, **ReferenceType**또는 **RowType** 요소 중 하나만 **속성** 요소의 자식 요소가 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="e7d59-733">Annotation 요소 (0 개 이상의 요소 허용)</span><span class="sxs-lookup"><span data-stu-id="e7d59-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="e7d59-734">Annotation 요소는 다른 모든 자식 요소 뒤에 와야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="e7d59-735">Annotation 요소는 CSDL v2 이상 에서만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-736">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-736">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-737">다음 표에서는 **매개 변수** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="e7d59-738">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-738">Attribute Name</span></span>   | <span data-ttu-id="e7d59-739">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-739">Is Required</span></span> | <span data-ttu-id="e7d59-740">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-741">**이름**</span><span class="sxs-lookup"><span data-stu-id="e7d59-741">**Name**</span></span>         | <span data-ttu-id="e7d59-742">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-742">Yes</span></span>         | <span data-ttu-id="e7d59-743">매개 변수의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="e7d59-744">**형식**</span><span class="sxs-lookup"><span data-stu-id="e7d59-744">**Type**</span></span>         | <span data-ttu-id="e7d59-745">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-745">No</span></span>          | <span data-ttu-id="e7d59-746">매개 변수 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-746">The parameter type.</span></span> <span data-ttu-id="e7d59-747">매개 변수는 다음 형식 또는 이러한 형식의 컬렉션일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="e7d59-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="e7d59-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="e7d59-749">엔터티 형식(entity type)</span><span class="sxs-lookup"><span data-stu-id="e7d59-749">entity type</span></span> <br/> <span data-ttu-id="e7d59-750">복합 형식</span><span class="sxs-lookup"><span data-stu-id="e7d59-750">complex type</span></span> <br/> <span data-ttu-id="e7d59-751">행 형식</span><span class="sxs-lookup"><span data-stu-id="e7d59-751">row type</span></span> <br/> <span data-ttu-id="e7d59-752">참조 형식(reference type)</span><span class="sxs-lookup"><span data-stu-id="e7d59-752">reference type</span></span>                             |
| <span data-ttu-id="e7d59-753">**않는**</span><span class="sxs-lookup"><span data-stu-id="e7d59-753">**Nullable**</span></span>     | <span data-ttu-id="e7d59-754">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-754">No</span></span>          | <span data-ttu-id="e7d59-755">속성에 **null** 값을 사용할 수 있는지 여부에 따라 **True** (기본값) 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="e7d59-756">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="e7d59-756">**DefaultValue**</span></span> | <span data-ttu-id="e7d59-757">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-757">No</span></span>          | <span data-ttu-id="e7d59-758">속성의 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="e7d59-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="e7d59-759">**MaxLength**</span></span>    | <span data-ttu-id="e7d59-760">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-760">No</span></span>          | <span data-ttu-id="e7d59-761">속성 값의 최대 길이입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="e7d59-762">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="e7d59-762">**FixedLength**</span></span>  | <span data-ttu-id="e7d59-763">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-763">No</span></span>          | <span data-ttu-id="e7d59-764">속성 값이 고정 길이 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="e7d59-765">**전체 자릿수**</span><span class="sxs-lookup"><span data-stu-id="e7d59-765">**Precision**</span></span>    | <span data-ttu-id="e7d59-766">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-766">No</span></span>          | <span data-ttu-id="e7d59-767">속성 값의 전체 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="e7d59-768">**소수 자릿수**</span><span class="sxs-lookup"><span data-stu-id="e7d59-768">**Scale**</span></span>        | <span data-ttu-id="e7d59-769">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-769">No</span></span>          | <span data-ttu-id="e7d59-770">속성 값의 소수 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="e7d59-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="e7d59-771">**SRID**</span></span>         | <span data-ttu-id="e7d59-772">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-772">No</span></span>          | <span data-ttu-id="e7d59-773">공간 시스템 참조 식별자입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="e7d59-774">공간 형식의 속성에 대해서만 유효 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="e7d59-775">자세한 내용은 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="e7d59-775">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="e7d59-776">**유니코드**</span><span class="sxs-lookup"><span data-stu-id="e7d59-776">**Unicode**</span></span>      | <span data-ttu-id="e7d59-777">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-777">No</span></span>          | <span data-ttu-id="e7d59-778">속성 값이 유니코드 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="e7d59-779">**데이터 정렬**</span><span class="sxs-lookup"><span data-stu-id="e7d59-779">**Collation**</span></span>    | <span data-ttu-id="e7d59-780">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-780">No</span></span>          | <span data-ttu-id="e7d59-781">데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="e7d59-782">원하는 수의 주석 특성 (사용자 지정 XML 특성)이 **Parameter** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="e7d59-783">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-784">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="e7d59-785">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-785">Example</span></span>

<span data-ttu-id="e7d59-786">다음 예제에서는 하나의 **매개 변수** 자식 요소를 사용 하 여 함수 매개 변수를 정의 하는 **함수** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a><span data-ttu-id="e7d59-787">Principal 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="e7d59-788">CSDL (개념 스키마 정의 언어)의 **principal** 요소는 참조 제약 조건의 주 끝을 정의 하는 참조 제약 조건 요소에 대 한 자식 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="e7d59-789">참조 무결성 제약 **조건** 요소는 관계형 데이터베이스의 참조 무결성 제약 조건과 유사한 기능을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="e7d59-790">데이터베이스 테이블의 열에서 다른 테이블의 기본 키를 참조하는 방식과 동일하게 엔터티 형식의 속성은 다른 엔터티 형식의 엔터티 키를 참조할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="e7d59-791">참조 되는 엔터티 형식을 제약 조건의 *주 끝* 이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="e7d59-792">주 끝을 참조 하는 엔터티 형식을 제약 조건의 *종속 끝* 이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="e7d59-793">**Propertyref** 요소는 종속 end에서 참조 하는 키를 지정 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="e7d59-794">**주요** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-795">PropertyRef (하나 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="e7d59-796">Annotation 요소 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-797">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-797">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-798">다음 표에서는 **Principal** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="e7d59-799">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-799">Attribute Name</span></span> | <span data-ttu-id="e7d59-800">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-800">Is Required</span></span> | <span data-ttu-id="e7d59-801">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="e7d59-802">**역할**</span><span class="sxs-lookup"><span data-stu-id="e7d59-802">**Role**</span></span>       | <span data-ttu-id="e7d59-803">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-803">Yes</span></span>         | <span data-ttu-id="e7d59-804">연결의 주 끝에 있는 엔터티 형식의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="e7d59-805">모든 수의 주석 특성 (사용자 지정 XML 특성)이 **Principal** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="e7d59-806">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-807">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-808">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-808">Example</span></span>

<span data-ttu-id="e7d59-809">다음 예제에서는 **PublishedBy** 연결의 정의에 포함 되는 **설명 요소를** 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="e7d59-810">**게시자** 엔터티 형식의 **Id** 속성은 참조 제약 조건의 주 끝을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

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
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="e7d59-811">Property 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-811">Property Element (CSDL)</span></span>

<span data-ttu-id="e7d59-812">CSDL (개념 스키마 정의 언어)의 **Property** 요소는 EntityType 요소, ComplexType 요소 또는 RowType 요소의 자식일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="e7d59-813">EntityType 및 ComplexType 요소 적용</span><span class="sxs-lookup"><span data-stu-id="e7d59-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="e7d59-814">**속성** 요소 ( **EntityType** 또는 **ComplexType** 요소의 자식)는 엔터티 형식 인스턴스 또는 복합 형식 인스턴스에 포함 될 데이터의 모양 및 특징을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="e7d59-815">개념적 모델의 속성은 클래스에 정의된 속성과 유사합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="e7d59-816">클래스의 속성이 클래스의 모양을 정의하고 개체에 대한 정보를 전달하는 것과 동일한 방식으로, 개념적 모델의 속성은 엔터티 형식의 모양을 정의하고 엔터티 형식 인스턴스에 대한 정보를 전달합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="e7d59-817">**Property** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-818">문서 요소 (0 개 또는 한 개의 요소 허용)</span><span class="sxs-lookup"><span data-stu-id="e7d59-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="e7d59-819">Annotation 요소 (0 개 이상의 요소 허용)</span><span class="sxs-lookup"><span data-stu-id="e7d59-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="e7d59-820">다음 패싯을 **속성** 요소에 적용할 수 있습니다. **Nullable**, **DefaultValue**, **MaxLength**, **fixedlength**, **전체 자릿수**, **소수 자릿수**, **유니코드**, **데이터 정렬**, **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="e7d59-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="e7d59-821">패싯은 속성 값이 데이터 저장소에 저장된 방식에 대한 정보를 제공하는 XML 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="e7d59-822">패싯은 **Edmsimpletype**형식의 속성에만 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-823">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-823">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-824">다음 표에서는 **Property** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="e7d59-825">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-825">Attribute Name</span></span>                                                         | <span data-ttu-id="e7d59-826">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-826">Is Required</span></span> | <span data-ttu-id="e7d59-827">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-828">**이름**</span><span class="sxs-lookup"><span data-stu-id="e7d59-828">**Name**</span></span>                                                               | <span data-ttu-id="e7d59-829">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-829">Yes</span></span>         | <span data-ttu-id="e7d59-830">속성의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="e7d59-831">**형식**</span><span class="sxs-lookup"><span data-stu-id="e7d59-831">**Type**</span></span>                                                               | <span data-ttu-id="e7d59-832">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-832">Yes</span></span>         | <span data-ttu-id="e7d59-833">속성 값의 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-833">The type of the property value.</span></span> <span data-ttu-id="e7d59-834">속성 값 형식은 모델의 범위 내에 있는 **Edmsimpletype** 또는 정규화 된 이름으로 표시 되는 복합 형식 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="e7d59-835">**않는**</span><span class="sxs-lookup"><span data-stu-id="e7d59-835">**Nullable**</span></span>                                                           | <span data-ttu-id="e7d59-836">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-836">No</span></span>          | <span data-ttu-id="e7d59-837">속성에 null 값을 사용할 수 있는지 여부에 따라 **True** (기본값) 또는 <strong>False</strong> 입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="e7d59-838">CSDL v1 복합 형식 속성의 >에는 `Nullable="False"`이 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="e7d59-839">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="e7d59-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="e7d59-840">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-840">No</span></span>          | <span data-ttu-id="e7d59-841">속성의 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="e7d59-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="e7d59-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="e7d59-843">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-843">No</span></span>          | <span data-ttu-id="e7d59-844">속성 값의 최대 길이입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="e7d59-845">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="e7d59-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="e7d59-846">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-846">No</span></span>          | <span data-ttu-id="e7d59-847">속성 값이 고정 길이 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="e7d59-848">**전체 자릿수**</span><span class="sxs-lookup"><span data-stu-id="e7d59-848">**Precision**</span></span>                                                          | <span data-ttu-id="e7d59-849">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-849">No</span></span>          | <span data-ttu-id="e7d59-850">속성 값의 전체 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="e7d59-851">**소수 자릿수**</span><span class="sxs-lookup"><span data-stu-id="e7d59-851">**Scale**</span></span>                                                              | <span data-ttu-id="e7d59-852">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-852">No</span></span>          | <span data-ttu-id="e7d59-853">속성 값의 소수 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="e7d59-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="e7d59-854">**SRID**</span></span>                                                               | <span data-ttu-id="e7d59-855">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-855">No</span></span>          | <span data-ttu-id="e7d59-856">공간 시스템 참조 식별자입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="e7d59-857">공간 형식의 속성에 대해서만 유효 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="e7d59-858">자세한 내용은 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="e7d59-858">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="e7d59-859">**유니코드**</span><span class="sxs-lookup"><span data-stu-id="e7d59-859">**Unicode**</span></span>                                                            | <span data-ttu-id="e7d59-860">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-860">No</span></span>          | <span data-ttu-id="e7d59-861">속성 값이 유니코드 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="e7d59-862">**데이터 정렬**</span><span class="sxs-lookup"><span data-stu-id="e7d59-862">**Collation**</span></span>                                                          | <span data-ttu-id="e7d59-863">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-863">No</span></span>          | <span data-ttu-id="e7d59-864">데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="e7d59-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="e7d59-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="e7d59-866">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-866">No</span></span>          | <span data-ttu-id="e7d59-867">**없음** (기본값) 또는 **고정**.</span><span class="sxs-lookup"><span data-stu-id="e7d59-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="e7d59-868">값을 **Fixed**로 설정 하면 속성 값이 낙관적 동시성 검사에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="e7d59-869">모든 수의 주석 특성 (사용자 지정 XML 특성)은 **Property** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="e7d59-870">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-871">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="e7d59-872">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-872">Example</span></span>

<span data-ttu-id="e7d59-873">다음 예제에서는 세 가지 **속성** 요소를 사용 하는 **EntityType** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

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
 

<span data-ttu-id="e7d59-874">다음 예제에서는 5 개의 **속성** 요소를 포함 하는 **ComplexType** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="e7d59-875">RowType 요소 적용</span><span class="sxs-lookup"><span data-stu-id="e7d59-875">RowType Element Application</span></span>

<span data-ttu-id="e7d59-876">**RowType** 요소의 자식인 **속성** 요소는 모델 정의 함수에 전달 되거나 반환 될 수 있는 데이터의 모양 및 특징을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="e7d59-877">**Property** 요소에는 다음 자식 요소 중 하나만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="e7d59-878">CollectionType</span><span class="sxs-lookup"><span data-stu-id="e7d59-878">CollectionType</span></span>
-   <span data-ttu-id="e7d59-879">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="e7d59-879">ReferenceType</span></span>
-   <span data-ttu-id="e7d59-880">RowType</span><span class="sxs-lookup"><span data-stu-id="e7d59-880">RowType</span></span>

<span data-ttu-id="e7d59-881">**Property** 요소에는 임의 개수의 자식 주석 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="e7d59-882">Annotation 요소는 CSDL v2 이상 에서만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-883">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-883">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-884">다음 표에서는 **Property** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="e7d59-885">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-885">Attribute Name</span></span>                                                     | <span data-ttu-id="e7d59-886">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-886">Is Required</span></span> | <span data-ttu-id="e7d59-887">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-888">**이름**</span><span class="sxs-lookup"><span data-stu-id="e7d59-888">**Name**</span></span>                                                           | <span data-ttu-id="e7d59-889">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-889">Yes</span></span>         | <span data-ttu-id="e7d59-890">속성의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="e7d59-891">**형식**</span><span class="sxs-lookup"><span data-stu-id="e7d59-891">**Type**</span></span>                                                           | <span data-ttu-id="e7d59-892">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-892">Yes</span></span>         | <span data-ttu-id="e7d59-893">속성 값의 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="e7d59-894">**않는**</span><span class="sxs-lookup"><span data-stu-id="e7d59-894">**Nullable**</span></span>                                                       | <span data-ttu-id="e7d59-895">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-895">No</span></span>          | <span data-ttu-id="e7d59-896">속성에 null 값을 사용할 수 있는지 여부에 따라 **True** (기본값) 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="e7d59-897">CSDL v1의 > 복합 형식 속성은 `Nullable="False"` 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="e7d59-898">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="e7d59-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="e7d59-899">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-899">No</span></span>          | <span data-ttu-id="e7d59-900">속성의 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="e7d59-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="e7d59-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="e7d59-902">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-902">No</span></span>          | <span data-ttu-id="e7d59-903">속성 값의 최대 길이입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="e7d59-904">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="e7d59-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="e7d59-905">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-905">No</span></span>          | <span data-ttu-id="e7d59-906">속성 값이 고정 길이 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="e7d59-907">**전체 자릿수**</span><span class="sxs-lookup"><span data-stu-id="e7d59-907">**Precision**</span></span>                                                      | <span data-ttu-id="e7d59-908">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-908">No</span></span>          | <span data-ttu-id="e7d59-909">속성 값의 전체 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="e7d59-910">**소수 자릿수**</span><span class="sxs-lookup"><span data-stu-id="e7d59-910">**Scale**</span></span>                                                          | <span data-ttu-id="e7d59-911">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-911">No</span></span>          | <span data-ttu-id="e7d59-912">속성 값의 소수 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="e7d59-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="e7d59-913">**SRID**</span></span>                                                           | <span data-ttu-id="e7d59-914">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-914">No</span></span>          | <span data-ttu-id="e7d59-915">공간 시스템 참조 식별자입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="e7d59-916">공간 형식의 속성에 대해서만 유효 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="e7d59-917">자세한 내용은 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="e7d59-917">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="e7d59-918">**유니코드**</span><span class="sxs-lookup"><span data-stu-id="e7d59-918">**Unicode**</span></span>                                                        | <span data-ttu-id="e7d59-919">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-919">No</span></span>          | <span data-ttu-id="e7d59-920">속성 값이 유니코드 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="e7d59-921">**데이터 정렬**</span><span class="sxs-lookup"><span data-stu-id="e7d59-921">**Collation**</span></span>                                                      | <span data-ttu-id="e7d59-922">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-922">No</span></span>          | <span data-ttu-id="e7d59-923">데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="e7d59-924">모든 수의 주석 특성 (사용자 지정 XML 특성)은 **Property** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="e7d59-925">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-926">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="e7d59-927">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-927">Example</span></span>

<span data-ttu-id="e7d59-928">다음 예에서는 모델 정의 함수의 반환 형식 셰이프를 정의 하는 데 사용 되는 **속성** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

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
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="e7d59-929">PropertyRef 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="e7d59-930">CSDL (개념 스키마 정의 언어)의 **Propertyref** 요소는 엔터티 형식의 속성을 참조 하 여 속성이 다음 역할 중 하나를 수행 함을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="e7d59-931">엔터티 키(ID를 확인하는 엔터티 형식의 속성 또는 속성 집합)의 일부분.</span><span class="sxs-lookup"><span data-stu-id="e7d59-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="e7d59-932">하나 이상의 **Propertyref** 요소를 사용 하 여 엔터티 키를 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="e7d59-933">참조 제약 조건의 종속 또는 주 끝.</span><span class="sxs-lookup"><span data-stu-id="e7d59-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="e7d59-934">**Propertyref** 요소에는 주석 요소 (0 개 이상)만 자식 요소로 포함 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="e7d59-935">Annotation 요소는 CSDL v2 이상 에서만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-936">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-936">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-937">다음 표에서는 **Propertyref** 요소에 적용 될 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="e7d59-938">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-938">Attribute Name</span></span> | <span data-ttu-id="e7d59-939">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-939">Is Required</span></span> | <span data-ttu-id="e7d59-940">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="e7d59-941">**이름**</span><span class="sxs-lookup"><span data-stu-id="e7d59-941">**Name**</span></span>       | <span data-ttu-id="e7d59-942">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-942">Yes</span></span>         | <span data-ttu-id="e7d59-943">참조된 속성의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="e7d59-944">원하는 수의 주석 특성 (사용자 지정 XML 특성)이 **Propertyref** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="e7d59-945">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-946">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-947">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-947">Example</span></span>

<span data-ttu-id="e7d59-948">아래 예제에서는 엔터티 형식 (**Book**)을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="e7d59-949">엔터티 키는 엔터티 형식의 **ISBN** 속성을 참조 하 여 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="e7d59-950">다음 예제에서는 두 개의 **Propertyref** 요소를 사용 하 여 두 속성 (**Id** 및 **PublisherId**)이 참조 제약 조건의 주 끝과 종속 end 임을 나타내는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

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
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="e7d59-951">ReferenceType 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="e7d59-952">CSDL (개념 스키마 정의 언어)의 **ReferenceType** 요소는 엔터티 형식에 대 한 참조를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="e7d59-953">**ReferenceType** 요소는 다음 요소의 자식일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="e7d59-954">ReturnType (함수)</span><span class="sxs-lookup"><span data-stu-id="e7d59-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="e7d59-955">매개 변수</span><span class="sxs-lookup"><span data-stu-id="e7d59-955">Parameter</span></span>
-   <span data-ttu-id="e7d59-956">CollectionType</span><span class="sxs-lookup"><span data-stu-id="e7d59-956">CollectionType</span></span>

<span data-ttu-id="e7d59-957">**ReferenceType** 요소는 함수에 대 한 매개 변수 또는 반환 형식을 정의할 때 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="e7d59-958">**ReferenceType** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-959">설명서 (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="e7d59-960">Annotation 요소 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-961">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-961">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-962">다음 표에서는 **ReferenceType** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="e7d59-963">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-963">Attribute Name</span></span> | <span data-ttu-id="e7d59-964">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-964">Is Required</span></span> | <span data-ttu-id="e7d59-965">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="e7d59-966">**형식**</span><span class="sxs-lookup"><span data-stu-id="e7d59-966">**Type**</span></span>       | <span data-ttu-id="e7d59-967">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-967">Yes</span></span>         | <span data-ttu-id="e7d59-968">참조되는 엔터티 형식의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="e7d59-969">모든 수의 주석 특성 (사용자 지정 XML 특성)이 **ReferenceType** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="e7d59-970">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-971">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-972">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-972">Example</span></span>

<span data-ttu-id="e7d59-973">다음 예제에서는 **Person** 엔터티 형식에 대 한 참조를 허용 하는 모델 정의 함수에서 **매개 변수** 요소의 자식으로 사용 되는 **ReferenceType** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

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
 

<span data-ttu-id="e7d59-974">다음 예제에서는 **Person** 엔터티 형식에 대 한 참조를 반환 하는 모델 정의 함수에서 **ReturnType** (function) 요소의 자식으로 사용 되는 **ReferenceType** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

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
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="e7d59-975">ReferentialConstraint 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="e7d59-976">CSDL (개념 스키마 정의 언어)의 참조 무결성 제약 **조건** 요소는 관계형 데이터베이스의 참조 무결성 제약 조건과 유사한 기능을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="e7d59-977">데이터베이스 테이블의 열에서 다른 테이블의 기본 키를 참조하는 방식과 동일하게 엔터티 형식의 속성은 다른 엔터티 형식의 엔터티 키를 참조할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="e7d59-978">참조 되는 엔터티 형식을 제약 조건의 *주 끝* 이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="e7d59-979">주 끝을 참조 하는 엔터티 형식을 제약 조건의 *종속 끝* 이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="e7d59-980">한 엔터티 형식에서 노출 되는 외래 키가 다른 엔터티 형식의 속성을 참조 하는 경우 참조 **는 두** 엔터티 형식 사이의 연결을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="e7d59-981">이 요소는 두 엔터티 형식이 관련 된 방법에 대 한 **정보를 제공** 하므로 MSL (매핑 사양 언어)에는 해당 하는 AssociationSetMapping 요소가 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="e7d59-982">노출 된 외래 키가 없는 두 엔터티 형식 간의 연결에는 연결 정보를 데이터 소스에 매핑하기 위해 해당 **AssociationSetMapping** 요소가 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="e7d59-983">엔터티 형식에서 외래 키가 노출 되지 않은 **경우에는 해당 엔터티** 형식과 다른 엔터티 형식 간에 기본 키-기본 키 제약 조건만 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="e7d59-984">이 **요소는** 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-985">설명서 (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="e7d59-986">보안 주체 (정확히 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="e7d59-987">Dependent (정확히 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="e7d59-988">Annotation 요소 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-989">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-989">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-990">이 **요소에는 원하는** 수의 주석 특성 (사용자 지정 XML 특성)이 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="e7d59-991">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-992">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e7d59-993">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-993">Example</span></span>

<span data-ttu-id="e7d59-994">다음 예제에서는 **PublishedBy** 연결 정의의 일부로 사용 **되는 설명 요소를** 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

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
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="e7d59-995">ReturnType (Function) 요소 (CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="e7d59-996">CSDL (개념 스키마 정의 언어)의 **ReturnType** (function) 요소는 함수 요소에 정의 된 함수에 대 한 반환 형식을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="e7d59-997">함수 반환 형식은 **ReturnType** 특성과 함께 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="e7d59-998">반환 형식은 모든 **Edmsimpletype**, 엔터티 형식, 복합 형식, 행 형식, 참조 형식 또는 이러한 형식 중 하나의 컬렉션이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="e7d59-999">함수의 반환 형식은 **ReturnType** (function) 요소의 **type** 특성 또는 다음 자식 요소 중 하나를 사용 하 여 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="e7d59-1000">CollectionType</span><span class="sxs-lookup"><span data-stu-id="e7d59-1000">CollectionType</span></span>
-   <span data-ttu-id="e7d59-1001">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="e7d59-1001">ReferenceType</span></span>
-   <span data-ttu-id="e7d59-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="e7d59-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="e7d59-1003">**ReturnType** (function) 요소의 **type** 특성과 자식 요소 중 하나를 사용 하 여 함수 반환 형식을 지정 하는 경우 모델은 유효성을 검사 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-1004">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-1004">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-1005">다음 표에서는 **ReturnType** (Function) 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="e7d59-1006">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-1006">Attribute Name</span></span> | <span data-ttu-id="e7d59-1007">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-1007">Is Required</span></span> | <span data-ttu-id="e7d59-1008">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="e7d59-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1009">**ReturnType**</span></span> | <span data-ttu-id="e7d59-1010">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1010">No</span></span>          | <span data-ttu-id="e7d59-1011">함수에서 반환하는 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="e7d59-1012">모든 수의 주석 특성 (사용자 지정 XML 특성)은 **ReturnType** (Function) 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="e7d59-1013">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-1014">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-1015">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-1015">Example</span></span>

<span data-ttu-id="e7d59-1016">다음 예에서는 **function** 요소를 사용 하 여 책이 인쇄 된 기간 (년)을 반환 하는 함수를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="e7d59-1017">반환 형식은 **ReturnType** (Function) 요소의 **type** 특성에 의해 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="e7d59-1018">ReturnType (FunctionImport) 요소 (CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="e7d59-1019">CSDL (개념 스키마 정의 언어)의 **ReturnType** (functionimport) 요소는 FunctionImport 요소에 정의 된 함수에 대 한 반환 형식을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="e7d59-1020">함수 반환 형식은 **ReturnType** 특성과 함께 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="e7d59-1021">반환 형식은 엔터티 형식, 복합 형식 또는 **Edmsimpletype**인 모든 컬렉션이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="e7d59-1022">함수의 반환 형식은 **ReturnType** (FunctionImport) 요소의 **type** 특성을 사용 하 여 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-1023">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-1023">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-1024">다음 표에서는 **ReturnType** (FunctionImport) 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="e7d59-1025">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-1025">Attribute Name</span></span> | <span data-ttu-id="e7d59-1026">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-1026">Is Required</span></span> | <span data-ttu-id="e7d59-1027">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-1028">**형식**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1028">**Type**</span></span>       | <span data-ttu-id="e7d59-1029">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1029">No</span></span>          | <span data-ttu-id="e7d59-1030">함수에서 반환하는 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1030">The type that the function returns.</span></span> <span data-ttu-id="e7d59-1031">값은 ComplexType, EntityType 또는 EDMSimpleType의 컬렉션 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="e7d59-1032">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1032">**EntitySet**</span></span>  | <span data-ttu-id="e7d59-1033">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1033">No</span></span>          | <span data-ttu-id="e7d59-1034">함수가 엔터티 형식의 컬렉션을 반환 하는 경우 **EntitySet** 의 값은 컬렉션이 속한 엔터티 집합 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="e7d59-1035">그렇지 않으면 **EntitySet** 특성을 사용 하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="e7d59-1036">모든 수의 주석 특성 (사용자 지정 XML 특성)은 **ReturnType** (FunctionImport) 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="e7d59-1037">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-1038">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-1039">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-1039">Example</span></span>

<span data-ttu-id="e7d59-1040">다음 예에서는 서적과 게시자를 반환 하는 **FunctionImport** 를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="e7d59-1041">함수는 두 개의 결과 집합을 반환 하므로 **ReturnType** (FunctionImport) 요소가 두 개 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="e7d59-1042">RowType 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="e7d59-1043">CSDL (개념 스키마 정의 언어)의 **RowType** 요소는 명명 되지 않은 구조를 개념적 모델에 정의 된 함수에 대 한 매개 변수 또는 반환 형식으로 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="e7d59-1044">**RowType** 요소는 다음 요소의 자식일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="e7d59-1045">CollectionType</span><span class="sxs-lookup"><span data-stu-id="e7d59-1045">CollectionType</span></span>
-   <span data-ttu-id="e7d59-1046">매개 변수</span><span class="sxs-lookup"><span data-stu-id="e7d59-1046">Parameter</span></span>
-   <span data-ttu-id="e7d59-1047">ReturnType (함수)</span><span class="sxs-lookup"><span data-stu-id="e7d59-1047">ReturnType (Function)</span></span>

<span data-ttu-id="e7d59-1048">**RowType** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-1049">속성 (하나 이상)</span><span class="sxs-lookup"><span data-stu-id="e7d59-1049">Property (one or more)</span></span>
-   <span data-ttu-id="e7d59-1050">Annotation 요소 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="e7d59-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-1051">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-1051">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-1052">**RowType** 요소에는 원하는 수의 주석 특성 (사용자 지정 XML 특성)을 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="e7d59-1053">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-1054">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e7d59-1055">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-1055">Example</span></span>

<span data-ttu-id="e7d59-1056">다음 예에서는 **CollectionType** 요소를 사용 하 여 함수가 **RowType** 요소에 지정 된 대로 행 컬렉션을 반환 하도록 지정 하는 모델 정의 함수를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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

## <a name="schema-element-csdl"></a><span data-ttu-id="e7d59-1057">스키마 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="e7d59-1058">**Schema** 요소는 개념적 모델 정의의 루트 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="e7d59-1059">이 요소에는 개념적 모델을 구성하는 개체, 함수 및 컨테이너에 대한 정의가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="e7d59-1060">**Schema** 요소는 다음과 같은 자식 요소를 0 개 이상 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="e7d59-1061">Using</span><span class="sxs-lookup"><span data-stu-id="e7d59-1061">Using</span></span>
-   <span data-ttu-id="e7d59-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="e7d59-1062">EntityContainer</span></span>
-   <span data-ttu-id="e7d59-1063">EntityType</span><span class="sxs-lookup"><span data-stu-id="e7d59-1063">EntityType</span></span>
-   <span data-ttu-id="e7d59-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="e7d59-1064">EnumType</span></span>
-   <span data-ttu-id="e7d59-1065">연결</span><span class="sxs-lookup"><span data-stu-id="e7d59-1065">Association</span></span>
-   <span data-ttu-id="e7d59-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="e7d59-1066">ComplexType</span></span>
-   <span data-ttu-id="e7d59-1067">기능</span><span class="sxs-lookup"><span data-stu-id="e7d59-1067">Function</span></span>

<span data-ttu-id="e7d59-1068">**Schema** 요소에는 0 개 또는 1 개의 주석 요소가 포함 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="e7d59-1069">**함수** 요소 및 주석 요소는 CSDL v2 이상 에서만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="e7d59-1070">**Schema** 요소는 **네임 스페이스** 특성을 사용 하 여 개념적 모델의 엔터티 형식, 복합 형식 및 연결 개체에 대 한 네임 스페이스를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="e7d59-1071">네임스페이스 내에서 두 개체의 이름이 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="e7d59-1072">네임 스페이스는 여러 **스키마** 요소와 여러 개의 csdl 파일에 걸쳐 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="e7d59-1073">개념적 모델 네임 스페이스는 **Schema** 요소의 XML 네임 스페이스와 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="e7d59-1074">**네임 스페이스** 특성에 정의 된 개념적 모델 네임 스페이스는 엔터티 형식, 복합 형식 및 연결 형식에 대 한 논리적 컨테이너입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="e7d59-1075">**Schema 요소의 XML** 네임 스페이스 ( **xmlns** 특성으로 표시 됨)는 **schema** 요소의 자식 요소 및 특성에 대 한 기본 네임 스페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="e7d59-1076">@No__t-0 형식의 XML 네임 스페이스 (YYYY와 MM은 각각 연도와 월을 나타냄)는 CSDL 용으로 예약 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1076">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="e7d59-1077">사용자 지정 요소 및 특성은 이러한 형식의 네임스페이스에 있을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-1078">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-1078">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-1079">다음 표에서는 **Schema** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="e7d59-1080">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-1080">Attribute Name</span></span> | <span data-ttu-id="e7d59-1081">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-1081">Is Required</span></span> | <span data-ttu-id="e7d59-1082">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-1083">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1083">**Namespace**</span></span>  | <span data-ttu-id="e7d59-1084">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-1084">Yes</span></span>         | <span data-ttu-id="e7d59-1085">개념적 모델의 네임스페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="e7d59-1086">**네임 스페이스** 특성의 값은 형식의 정규화 된 이름을 구성 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="e7d59-1087">예를 들어 이름이 *Customer* 인 **entitytype** 이 Simple. Model 네임 스페이스에 있는 경우 **Entitytype** 의 정규화 된 이름은 simpleexamplemodel.customer입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="e7d59-1088">다음 문자열은 **Namespace** 특성에 대 한 값으로 사용할 수 없습니다. **시스템**, **임시**또는 **Edm**입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="e7d59-1089">**Namespace** 특성의 값은 SSDL Schema 요소의 **namespace** 특성에 대 한 값과 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="e7d59-1090">**Alias**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1090">**Alias**</span></span>      | <span data-ttu-id="e7d59-1091">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1091">No</span></span>          | <span data-ttu-id="e7d59-1092">네임스페이스 이름 대신 사용되는 식별자입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="e7d59-1093">예를 들어, 이름이 *Customer* 인 **EntityType** 이 Simple. model 네임 스페이스에 있고 **Alias** 특성의 값이 *model*인 경우에는 **entitytype** 의 정규화 된 이름으로 모델인를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="e7d59-1094">모든 수의 주석 특성 (사용자 지정 XML 특성)은 **Schema** 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="e7d59-1095">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-1096">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-1097">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-1097">Example</span></span>

<span data-ttu-id="e7d59-1098">다음 예제에서는 **EntityContainer** 요소, 두 개의 **EntityType** 요소 및 하나의 **Association** 요소를 포함 하는 **스키마** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

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
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="e7d59-1099">TypeRef 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="e7d59-1100">CSDL (개념 스키마 정의 언어)의 **TypeRef** 요소는 기존 명명 된 형식에 대 한 참조를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="e7d59-1101">**TypeRef** 요소는 함수에 컬렉션을 매개 변수 또는 반환 형식으로 지정 하는 데 사용 되는 CollectionType 요소의 자식일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="e7d59-1102">**TypeRef** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e7d59-1103">설명서 (0 개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="e7d59-1104">Annotation 요소 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="e7d59-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-1105">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-1105">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-1106">다음 표에서는 **TypeRef** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="e7d59-1107">**DefaultValue**, **MaxLength**, **fixedlength**, **Precision**, **Scale**, **Unicode**및 **Collation** 특성은 **edmsimpletypes**에만 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="e7d59-1108">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="e7d59-1109">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-1109">Is Required</span></span> | <span data-ttu-id="e7d59-1110">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-1111">**형식**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1111">**Type**</span></span>                                                           | <span data-ttu-id="e7d59-1112">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1112">No</span></span>          | <span data-ttu-id="e7d59-1113">참조되는 형식의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="e7d59-1114">**않는**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="e7d59-1115">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1115">No</span></span>          | <span data-ttu-id="e7d59-1116">속성에 null 값을 사용할 수 있는지 여부에 따라 **True** (기본값) 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="e7d59-1117">CSDL v1의 > 복합 형식 속성은 `Nullable="False"` 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="e7d59-1118">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="e7d59-1119">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1119">No</span></span>          | <span data-ttu-id="e7d59-1120">속성의 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="e7d59-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="e7d59-1122">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1122">No</span></span>          | <span data-ttu-id="e7d59-1123">속성 값의 최대 길이입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="e7d59-1124">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="e7d59-1125">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1125">No</span></span>          | <span data-ttu-id="e7d59-1126">속성 값이 고정 길이 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="e7d59-1127">**전체 자릿수**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1127">**Precision**</span></span>                                                      | <span data-ttu-id="e7d59-1128">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1128">No</span></span>          | <span data-ttu-id="e7d59-1129">속성 값의 전체 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="e7d59-1130">**소수 자릿수**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1130">**Scale**</span></span>                                                          | <span data-ttu-id="e7d59-1131">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1131">No</span></span>          | <span data-ttu-id="e7d59-1132">속성 값의 소수 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="e7d59-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1133">**SRID**</span></span>                                                           | <span data-ttu-id="e7d59-1134">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1134">No</span></span>          | <span data-ttu-id="e7d59-1135">공간 시스템 참조 식별자입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="e7d59-1136">공간 형식의 속성에 대해서만 유효 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="e7d59-1137">자세한 내용은 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1137">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="e7d59-1138">**유니코드**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="e7d59-1139">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1139">No</span></span>          | <span data-ttu-id="e7d59-1140">속성 값이 유니코드 문자열로 저장 되는지 여부에 따라 **True** 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="e7d59-1141">**데이터 정렬**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1141">**Collation**</span></span>                                                      | <span data-ttu-id="e7d59-1142">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1142">No</span></span>          | <span data-ttu-id="e7d59-1143">데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="e7d59-1144">여러 주석 특성 (사용자 지정 XML 특성)을 **CollectionType** 요소에 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="e7d59-1145">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-1146">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-1147">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-1147">Example</span></span>

<span data-ttu-id="e7d59-1148">다음 예에서는 **TypeRef** 요소 ( **CollectionType** 요소의 자식)를 사용 하 여 함수가 **학과** 엔터티 형식의 컬렉션을 허용 하도록 지정 하는 모델 정의 함수를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

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
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="e7d59-1149">요소 사용(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="e7d59-1150">CSDL (개념 스키마 정의 언어)의 **Using** 요소는 다른 네임 스페이스에 있는 개념적 모델의 콘텐츠를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="e7d59-1151">**네임 스페이스** 특성의 값을 설정 하 여 다른 개념적 모델에 정의 된 엔터티 형식, 복합 형식 및 연결 형식을 참조할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="e7d59-1152">**Using** 요소가 두 개 이상 있는 경우 **Schema** 요소의 자식일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="e7d59-1153">CSDL의 **using** 요소는 프로그래밍 언어에서 **using** 문과 똑같이 작동 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="e7d59-1154">프로그래밍 언어에서 **using 문을 사용** 하 여 네임 스페이스를 가져오면 원래 네임 스페이스의 개체에는 영향을 주지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="e7d59-1155">CSDL의 가져온 네임스페이스에는 원래 네임스페이스의 엔터티 형식에서 파생된 엔터티 형식이 포함될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="e7d59-1156">이는 원래 네임스페이스에 선언된 엔터티 집합에 영향을 줄 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="e7d59-1157">**Using** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="e7d59-1158">설명서 (0 개 또는 한 개의 요소 허용)</span><span class="sxs-lookup"><span data-stu-id="e7d59-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="e7d59-1159">Annotation 요소 (0 개 이상의 요소 허용)</span><span class="sxs-lookup"><span data-stu-id="e7d59-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e7d59-1160">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="e7d59-1160">Applicable Attributes</span></span>

<span data-ttu-id="e7d59-1161">다음 표에서는 **Using** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="e7d59-1162">특성 이름</span><span class="sxs-lookup"><span data-stu-id="e7d59-1162">Attribute Name</span></span> | <span data-ttu-id="e7d59-1163">필수 여부</span><span class="sxs-lookup"><span data-stu-id="e7d59-1163">Is Required</span></span> | <span data-ttu-id="e7d59-1164">값</span><span class="sxs-lookup"><span data-stu-id="e7d59-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-1165">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1165">**Namespace**</span></span>  | <span data-ttu-id="e7d59-1166">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-1166">Yes</span></span>         | <span data-ttu-id="e7d59-1167">가져온 네임스페이스의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="e7d59-1168">**Alias**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1168">**Alias**</span></span>      | <span data-ttu-id="e7d59-1169">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-1169">Yes</span></span>         | <span data-ttu-id="e7d59-1170">네임스페이스 이름 대신 사용되는 식별자입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="e7d59-1171">이 특성은 필요하지만 개체 이름을 정규화하기 위해 네임스페이스 이름 대신 사용해야 할 필요는 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="e7d59-1172">**Using** 요소에 주석 특성 (사용자 지정 XML 특성)을 여러 개 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="e7d59-1173">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e7d59-1174">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="e7d59-1175">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-1175">Example</span></span>

<span data-ttu-id="e7d59-1176">다음 예제에서는 다른 곳에서 정의 된 네임 스페이스를 가져오는 데 사용 되는 **Using** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="e7d59-1177">표시 된 **Schema** 요소의 네임 스페이스는 `BooksModel`입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="e7d59-1178">@No__t-1**EntityType** 의 `Address` 속성은 `ExtendedBooksModel` 네임 스페이스 ( **Using** 요소로 가져옴)에 정의 된 복합 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

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
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="e7d59-1179">주석 특성(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="e7d59-1180">CSDL(개념 스키마 정의 언어)의 주석 특성은 개념적 모델의 사용자 지정 XML 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="e7d59-1181">주석 특성은 올바른 XML 구조를 가져야 함은 물론 다음 조건을 충족해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="e7d59-1182">주석 특성은 CSDL용으로 예약된 XML 네임스페이스에 속하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="e7d59-1183">두 개 이상의 주석 특성을 제공된 CSDL 요소에 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="e7d59-1184">두 주석 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="e7d59-1185">주석 특성을 사용하여 개념적 모델의 요소에 대한 추가 메타데이터를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="e7d59-1186">Annotation 요소에 포함 된 메타 데이터는 런타임에 System.object 네임 스페이스의 클래스를 사용 하 여 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="e7d59-1187">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-1187">Example</span></span>

<span data-ttu-id="e7d59-1188">다음 예제에서는 주석 특성 (**CustomAttribute**)이 포함 된 **EntityType** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="e7d59-1189">이 예제에서는 엔터티 형식 요소에 적용된 Annotation 요소도 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1189">The example also shows an annotation element applied to the entity type element.</span></span>

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
 

<span data-ttu-id="e7d59-1190">다음 코드에서는 주석 특성의 메타데이터를 검색하여 콘솔에 씁니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

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
 

<span data-ttu-id="e7d59-1191">이 코드에서는 `School.csdl` 파일이 프로젝트의 출력 디렉터리에 있고 프로젝트에 다음의 `Imports` 및 `Using` 문을 추가했다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="e7d59-1192">Annotation 요소(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="e7d59-1193">CSDL(개념 스키마 정의 언어)의 Annotation 요소는 개념적 모델의 사용자 지정 XML 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="e7d59-1194">Annotation 요소는 올바른 XML 구조를 가져야 함은 물론 다음 조건을 충족해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="e7d59-1195">Annotation 요소는 CSDL용으로 예약된 XML 네임스페이스에 속하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="e7d59-1196">두 개 이상의 Annotation 요소가 제공된 CSDL 요소의 자식이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="e7d59-1197">두 Annotation 요소의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="e7d59-1198">Annotation 요소는 제공된 CSDL 요소의 다른 모든 자식 요소 뒤에 나타나야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="e7d59-1199">Annotation 요소를 사용하여 개념적 모델의 요소에 대한 추가 메타데이터를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="e7d59-1200">.NET Framework 버전 4부터 annotation 요소에 포함 된 메타 데이터는 런타임에 System.object 네임 스페이스의 클래스를 사용 하 여 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="e7d59-1201">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-1201">Example</span></span>

<span data-ttu-id="e7d59-1202">다음 예에서는 annotation 요소 (**customelement**)가 있는 **EntityType** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="e7d59-1203">다음 예제에서는 엔터티 형식 요소에 적용된 주석 특성도 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

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
 

<span data-ttu-id="e7d59-1204">다음 코드에서는 Annotation 요소의 메타데이터를 검색하여 콘솔에 작성합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

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
 

<span data-ttu-id="e7d59-1205">위의 코드에서는 School.csdl 파일이 프로젝트의 출력 디렉터리에 있고 다음 `Imports` 및 `Using` 문을 프로젝트에 추가했다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="e7d59-1206">개념적 모델 형식(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="e7d59-1207">CSDL (개념 스키마 정의 언어)은 개념적 모델에서 속성을 정의 하는 **Edmsimpletypes**이라는 추상 기본 데이터 형식 집합을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="e7d59-1208">**Edmsimpletypes** 은 저장소 또는 호스팅 환경에서 지원 되는 기본 데이터 형식에 대 한 프록시입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="e7d59-1209">다음 표에서는 CSDL에서 지원되는 기본 데이터 형식을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="e7d59-1210">테이블에는 각 **Edmsimpletype**에 적용 될 수 있는 패싯이 나열 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="e7d59-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="e7d59-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="e7d59-1212">설명</span><span class="sxs-lookup"><span data-stu-id="e7d59-1212">Description</span></span>                                                | <span data-ttu-id="e7d59-1213">적용 가능한 패싯</span><span class="sxs-lookup"><span data-stu-id="e7d59-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="e7d59-1214">**Edm. Binary**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="e7d59-1215">이진 데이터를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="e7d59-1216">MaxLength, FixedLength, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="e7d59-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="e7d59-1217">**Edm. 부울**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="e7d59-1218">**True** 또는 **false**값을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="e7d59-1219">Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="e7d59-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="e7d59-1220">**Edm. 바이트**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="e7d59-1221">부호 없는 8비트 정수 값을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="e7d59-1222">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="e7d59-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="e7d59-1223">**Edm. DateTime**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="e7d59-1224">날짜 및 시간을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="e7d59-1225">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="e7d59-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="e7d59-1226">**Edm DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="e7d59-1227">날짜 및 시간을 GMT에서의 오프셋(분)으로 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="e7d59-1228">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="e7d59-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="e7d59-1229">**Edm. Decimal**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="e7d59-1230">고정 전체 자릿수와 소수 자릿수가 있는 숫자 값을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="e7d59-1231">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="e7d59-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="e7d59-1232">**Edm. Double**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="e7d59-1233">15 자리의 전체 자릿수를 포함 하는 부동 소수점 숫자를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="e7d59-1234">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="e7d59-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="e7d59-1235">**Edm. Float**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="e7d59-1236">전체 자릿수가 7자리인 부동 소수점 숫자를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="e7d59-1237">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="e7d59-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="e7d59-1238">**Edm. Guid**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="e7d59-1239">16바이트 고유 식별자를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="e7d59-1240">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="e7d59-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="e7d59-1241">**Edm. Int16**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="e7d59-1242">부호 있는 16비트 정수 값을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="e7d59-1243">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="e7d59-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="e7d59-1244">**Edm. Int32**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="e7d59-1245">부호 있는 32비트 정수 값을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="e7d59-1246">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="e7d59-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="e7d59-1247">**Edm. Int64**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="e7d59-1248">부호 있는 64비트 정수 값을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="e7d59-1249">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="e7d59-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="e7d59-1250">**Edm. SByte**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="e7d59-1251">부호 있는 8비트 정수 값을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="e7d59-1252">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="e7d59-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="e7d59-1253">**Edm. 문자열**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1253">**Edm.String**</span></span>                   | <span data-ttu-id="e7d59-1254">문자 데이터를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1254">Contains character data.</span></span>                                   | <span data-ttu-id="e7d59-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="e7d59-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="e7d59-1256">**Edm. 시간**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="e7d59-1257">시간을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="e7d59-1258">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="e7d59-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="e7d59-1259">**Edm. 지리**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="e7d59-1260">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="e7d59-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="e7d59-1261">**GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="e7d59-1262">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="e7d59-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="e7d59-1263">**GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="e7d59-1264">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="e7d59-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="e7d59-1265">**GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="e7d59-1266">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="e7d59-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="e7d59-1267">**Edm. GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="e7d59-1268">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="e7d59-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="e7d59-1269">**반환할 geographymultilinestring**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="e7d59-1270">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="e7d59-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="e7d59-1271">**Edm. GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="e7d59-1272">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="e7d59-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="e7d59-1273">**Edm .Geographycollection**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="e7d59-1274">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="e7d59-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="e7d59-1275">**Edm. Geometry**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="e7d59-1276">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="e7d59-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="e7d59-1277">**반환할 geometrypoint**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="e7d59-1278">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="e7d59-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="e7d59-1279">**GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="e7d59-1280">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="e7d59-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="e7d59-1281">**GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="e7d59-1282">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="e7d59-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="e7d59-1283">**GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="e7d59-1284">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="e7d59-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="e7d59-1285">**GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="e7d59-1286">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="e7d59-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="e7d59-1287">**GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="e7d59-1288">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="e7d59-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="e7d59-1289">**GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="e7d59-1290">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="e7d59-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="e7d59-1291">패싯(CSDL)</span><span class="sxs-lookup"><span data-stu-id="e7d59-1291">Facets (CSDL)</span></span>

<span data-ttu-id="e7d59-1292">CSDL(개념 스키마 정의 언어)의 패싯은 엔터티 형식 및 복합 형식 속성에 대한 제약 조건을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="e7d59-1293">패싯은 다음 CSDL 요소에서 XML 특성으로 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="e7d59-1294">속성</span><span class="sxs-lookup"><span data-stu-id="e7d59-1294">Property</span></span>
-   <span data-ttu-id="e7d59-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="e7d59-1295">TypeRef</span></span>
-   <span data-ttu-id="e7d59-1296">매개 변수</span><span class="sxs-lookup"><span data-stu-id="e7d59-1296">Parameter</span></span>

<span data-ttu-id="e7d59-1297">다음 표에서는 CSDL에서 지원되는 패싯에 대해 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="e7d59-1298">모든 패싯은 선택적 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1298">All facets are optional.</span></span> <span data-ttu-id="e7d59-1299">아래에 나열 된 일부 패싯은 개념적 모델에서 데이터베이스를 생성할 때 Entity Framework에서 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="e7d59-1300">개념적 모델의 데이터 형식에 대 한 자세한 내용은 개념적 모델 형식 (CSDL)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="e7d59-1301">패싯</span><span class="sxs-lookup"><span data-stu-id="e7d59-1301">Facet</span></span>               | <span data-ttu-id="e7d59-1302">설명</span><span class="sxs-lookup"><span data-stu-id="e7d59-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="e7d59-1303">적용 대상</span><span class="sxs-lookup"><span data-stu-id="e7d59-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="e7d59-1304">데이터베이스 생성에 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1304">Used for the database generation</span></span> | <span data-ttu-id="e7d59-1305">런타임에서 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="e7d59-1306">**데이터 정렬**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1306">**Collation**</span></span>       | <span data-ttu-id="e7d59-1307">속성 값에 대한 비교 및 순서 지정 작업을 수행할 때 사용할 데이터 정렬 순서 또는 정렬 순서를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="e7d59-1308">**Edm. 문자열**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="e7d59-1309">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-1309">Yes</span></span>                              | <span data-ttu-id="e7d59-1310">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1310">No</span></span>                  |
| <span data-ttu-id="e7d59-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="e7d59-1312">속성 값을 낙관적 동시성 검사에 사용하도록 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="e7d59-1313">모든 **Edmsimpletype** 속성</span><span class="sxs-lookup"><span data-stu-id="e7d59-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="e7d59-1314">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1314">No</span></span>                               | <span data-ttu-id="e7d59-1315">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-1315">Yes</span></span>                 |
| <span data-ttu-id="e7d59-1316">**Default**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1316">**Default**</span></span>         | <span data-ttu-id="e7d59-1317">인스턴스화 시 값이 제공되지 않는 경우 속성의 기본값을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="e7d59-1318">모든 **Edmsimpletype** 속성</span><span class="sxs-lookup"><span data-stu-id="e7d59-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="e7d59-1319">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-1319">Yes</span></span>                              | <span data-ttu-id="e7d59-1320">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-1320">Yes</span></span>                 |
| <span data-ttu-id="e7d59-1321">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1321">**FixedLength**</span></span>     | <span data-ttu-id="e7d59-1322">속성 값 길이가 다양할 수 있는지 여부를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="e7d59-1323">**Edm. Binary**, **edm. String**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="e7d59-1324">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-1324">Yes</span></span>                              | <span data-ttu-id="e7d59-1325">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1325">No</span></span>                  |
| <span data-ttu-id="e7d59-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1326">**MaxLength**</span></span>       | <span data-ttu-id="e7d59-1327">속성 값의 최대 길이를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="e7d59-1328">**Edm. Binary**, **edm. String**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="e7d59-1329">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-1329">Yes</span></span>                              | <span data-ttu-id="e7d59-1330">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1330">No</span></span>                  |
| <span data-ttu-id="e7d59-1331">**않는**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1331">**Nullable**</span></span>        | <span data-ttu-id="e7d59-1332">속성에 **null** 값을 사용할 수 있는지 여부를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="e7d59-1333">모든 **Edmsimpletype** 속성</span><span class="sxs-lookup"><span data-stu-id="e7d59-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="e7d59-1334">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-1334">Yes</span></span>                              | <span data-ttu-id="e7d59-1335">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-1335">Yes</span></span>                 |
| <span data-ttu-id="e7d59-1336">**전체 자릿수**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1336">**Precision**</span></span>       | <span data-ttu-id="e7d59-1337">**Decimal**형식의 속성에 대해 속성 값에 포함 될 수 있는 자릿수를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="e7d59-1338">**Time**, **DateTime**및 **DateTimeOffset**형식의 속성에 대해 속성 값의 초 소수 부분 자릿수를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="e7d59-1339">**Edm. DateTime**, **edm DateTimeOffset**, **edm. 10**, **edm. 시간**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="e7d59-1340">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-1340">Yes</span></span>                              | <span data-ttu-id="e7d59-1341">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1341">No</span></span>                  |
| <span data-ttu-id="e7d59-1342">**소수 자릿수**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1342">**Scale**</span></span>           | <span data-ttu-id="e7d59-1343">속성 값에 대한 소수점 오른쪽의 자릿수를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="e7d59-1344">**Edm. Decimal**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="e7d59-1345">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-1345">Yes</span></span>                              | <span data-ttu-id="e7d59-1346">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1346">No</span></span>                  |
| <span data-ttu-id="e7d59-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1347">**SRID**</span></span>            | <span data-ttu-id="e7d59-1348">공간 시스템 참조 시스템 ID를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="e7d59-1349">자세한 내용은 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1349">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="e7d59-1350">**GeographyPoint, GeographyLineString, GeographyPolygon,, Edm. GeographyMultiPoint,, 반환할 geographymultilinestring, Edm. GeographyMultiPolygon, edm. Geographymultipoint, edm. Geometry, 반환할 geometrypoint, GeometryLineString, GeometryPolygon, GeometryMultiPoint, GeometryMultiLineString, GeometryMultiPolygon, GeometryCollection,**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="e7d59-1351">아니요</span><span class="sxs-lookup"><span data-stu-id="e7d59-1351">No</span></span>                               | <span data-ttu-id="e7d59-1352">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-1352">Yes</span></span>                 |
| <span data-ttu-id="e7d59-1353">**유니코드**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1353">**Unicode**</span></span>         | <span data-ttu-id="e7d59-1354">속성 값을 유니코드로 저장할지 여부를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="e7d59-1355">**Edm. 문자열**</span><span class="sxs-lookup"><span data-stu-id="e7d59-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="e7d59-1356">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-1356">Yes</span></span>                              | <span data-ttu-id="e7d59-1357">예</span><span class="sxs-lookup"><span data-stu-id="e7d59-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="e7d59-1358">개념적 모델에서 데이터베이스를 생성 하는 경우 데이터베이스 생성 마법사가 다음 네임 스페이스에 있는 경우 **속성** 요소에 대해 **이 특성 값** 을 인식 합니다. https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="e7d59-1359">특성에 대해 지원 되는 값은 **id** 및 **계산**된 값입니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="e7d59-1360">**Identity** 값은 데이터베이스에서 생성 된 id 값을 사용 하 여 데이터베이스 열을 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="e7d59-1361">**계산** 된 값은 데이터베이스에서 계산 된 값이 있는 열을 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="e7d59-1362">예제</span><span class="sxs-lookup"><span data-stu-id="e7d59-1362">Example</span></span>

<span data-ttu-id="e7d59-1363">다음 예제에서는 엔터티 형식의 속성에 적용된 패싯을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7d59-1363">The following example shows facets applied to the properties of an entity type:</span></span>

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
