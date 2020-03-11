---
title: MSL 사양-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 8990d1373ea2121ce11337a43dbcdf3b9e1532bd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415480"
---
# <a name="msl-specification"></a><span data-ttu-id="445cd-102">XML 사양</span><span class="sxs-lookup"><span data-stu-id="445cd-102">MSL Specification</span></span>
<span data-ttu-id="445cd-103">MSL (매핑 사양 언어)은 Entity Framework 응용 프로그램의 개념적 모델과 저장소 모델 간의 매핑을 설명 하는 XML 기반 언어입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="445cd-104">Entity Framework 응용 프로그램에서 매핑 메타 데이터는 빌드 시 msl 파일 (MSL로 작성 됨)에서 로드 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="445cd-105">Entity Framework는 런타임에 매핑 메타 데이터를 사용 하 여 개념적 모델에 대 한 쿼리를 저장소 관련 명령으로 변환 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="445cd-106">Entity Framework Designer (EF Designer)는 디자인 타임에 매핑 정보를 .edmx 파일에 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="445cd-107">빌드 시 Entity Designer는 .edmx 파일의 정보를 사용 하 여 런타임에 Entity Framework에 필요한 msl 파일을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="445cd-108">MSL에서 참조하는 모든 개념적 모델 형식 또는 스토리지 모델 형식의 이름은 해당 네임스페이스 이름으로 한정되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="445cd-109">개념적 모델 네임 스페이스 이름에 대 한 자세한 내용은 [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="445cd-110">저장소 모델 네임 스페이스 이름에 대 한 자세한 내용은 [SSDL 사양](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md)을 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="445cd-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="445cd-111">MSL의 버전은 XML 네임 스페이스로 구분 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="445cd-112">MSL 버전</span><span class="sxs-lookup"><span data-stu-id="445cd-112">MSL Version</span></span> | <span data-ttu-id="445cd-113">XML 네임 스페이스</span><span class="sxs-lookup"><span data-stu-id="445cd-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="445cd-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="445cd-114">MSL v1</span></span>      | <span data-ttu-id="445cd-115">urn: 스키마-microsoft-com: windows: storage: 매핑: CS</span><span class="sxs-lookup"><span data-stu-id="445cd-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="445cd-116">MSL v2</span><span class="sxs-lookup"><span data-stu-id="445cd-116">MSL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/mapping/cs |
| <span data-ttu-id="445cd-117">MSL v3</span><span class="sxs-lookup"><span data-stu-id="445cd-117">MSL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="445cd-118">Alias 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-118">Alias Element (MSL)</span></span>

<span data-ttu-id="445cd-119">MSL (매핑 사양 언어)의 **Alias** 요소는 개념적 모델 및 저장소 모델 네임 스페이스에 대 한 별칭을 정의 하는 데 사용 되는 mapping 요소의 자식입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="445cd-120">MSL에서 참조하는 모든 개념적 모델 형식 또는 스토리지 모델 형식의 이름은 해당 네임스페이스 이름으로 한정되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="445cd-121">개념적 모델 네임 스페이스 이름에 대 한 자세한 내용은 Schema 요소 (CSDL)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="445cd-122">저장소 모델 네임 스페이스 이름에 대 한 자세한 내용은 Schema 요소 (SSDL)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="445cd-123">**Alias** 요소에는 자식 요소가 있을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="445cd-124">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-124">Applicable Attributes</span></span>

<span data-ttu-id="445cd-125">다음 표에서는 **Alias** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="445cd-126">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-126">Attribute Name</span></span> | <span data-ttu-id="445cd-127">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-127">Is Required</span></span> | <span data-ttu-id="445cd-128">값</span><span class="sxs-lookup"><span data-stu-id="445cd-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="445cd-129">**키**</span><span class="sxs-lookup"><span data-stu-id="445cd-129">**Key**</span></span>        | <span data-ttu-id="445cd-130">예</span><span class="sxs-lookup"><span data-stu-id="445cd-130">Yes</span></span>         | <span data-ttu-id="445cd-131">**Value** 특성에 지정 된 네임 스페이스에 대 한 별칭입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="445cd-132">**Value**</span><span class="sxs-lookup"><span data-stu-id="445cd-132">**Value**</span></span>      | <span data-ttu-id="445cd-133">예</span><span class="sxs-lookup"><span data-stu-id="445cd-133">Yes</span></span>         | <span data-ttu-id="445cd-134">**키** 요소의 값이 별칭이 되는 네임 스페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="445cd-135">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-135">Example</span></span>

<span data-ttu-id="445cd-136">다음 예제에서는 개념적 모델에 정의 된 형식에 대 한 `c`별칭을 정의 하는 **alias** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

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

## <a name="associationend-element-msl"></a><span data-ttu-id="445cd-137">AssociationEnd 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="445cd-138">MSL (매핑 사양 언어)의 **AssociationEnd** 요소는 개념적 모델의 엔터티 형식에 대 한 수정 함수가 기본 데이터베이스의 저장 프로시저에 매핑될 때 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="445cd-139">수정 저장 프로시저가 association 속성에 값을 보유 하는 매개 변수를 사용 하는 경우 **AssociationEnd** 요소는 속성 값을 매개 변수에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="445cd-140">자세한 내용은 아래 예제를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="445cd-140">For more information, see the example below.</span></span>

<span data-ttu-id="445cd-141">엔터티 형식의 수정 함수를 저장 프로시저에 매핑하는 방법에 대 한 자세한 내용은 ModificationFunctionMapping 요소 (MSL) 및 연습: 저장 프로시저에 엔터티 매핑을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="445cd-142">**AssociationEnd** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="445cd-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="445cd-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="445cd-144">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-144">Applicable Attributes</span></span>

<span data-ttu-id="445cd-145">다음 표에서는 **AssociationEnd** 요소에 적용할 수 있는 특성에 대해 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="445cd-146">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-146">Attribute Name</span></span>     | <span data-ttu-id="445cd-147">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-147">Is Required</span></span> | <span data-ttu-id="445cd-148">값</span><span class="sxs-lookup"><span data-stu-id="445cd-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="445cd-149">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="445cd-149">**AssociationSet**</span></span> | <span data-ttu-id="445cd-150">예</span><span class="sxs-lookup"><span data-stu-id="445cd-150">Yes</span></span>         | <span data-ttu-id="445cd-151">매핑되는 연결의 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="445cd-152">**From**</span><span class="sxs-lookup"><span data-stu-id="445cd-152">**From**</span></span>           | <span data-ttu-id="445cd-153">예</span><span class="sxs-lookup"><span data-stu-id="445cd-153">Yes</span></span>         | <span data-ttu-id="445cd-154">매핑되는 연결에 해당 하는 탐색 속성의 **Fromrole** 특성 값입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="445cd-155">자세한 내용은 NavigationProperty 요소 (CSDL)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="445cd-156">**대상**</span><span class="sxs-lookup"><span data-stu-id="445cd-156">**To**</span></span>             | <span data-ttu-id="445cd-157">예</span><span class="sxs-lookup"><span data-stu-id="445cd-157">Yes</span></span>         | <span data-ttu-id="445cd-158">매핑되는 연결에 해당 하는 탐색 속성의 **Torole** 특성 값입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="445cd-159">자세한 내용은 NavigationProperty 요소 (CSDL)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="445cd-160">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-160">Example</span></span>

<span data-ttu-id="445cd-161">다음과 같은 개념적 모델 엔터티 형식이 있다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-161">Consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="445cd-162">또한 다음과 같은 저장 프로시저가 있다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-162">Also consider the following stored procedure:</span></span>

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

<span data-ttu-id="445cd-163">`Course` 엔터티의 업데이트 함수를이 저장 프로시저에 매핑하려면 **DepartmentID** 매개 변수에 값을 제공 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="445cd-164">`DepartmentID`의 값은 엔터티 형식의 속성에 해당하지 않으며, 여기에 표시된 매핑을 사용하는 독립 연결에 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

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

<span data-ttu-id="445cd-165">다음 코드에서는 **FK\_과정\_학과** 연결의 **DepartmentID** 속성을 **UpdateCourse** 저장 프로시저에 매핑하는 데 사용 되는 **AssociationEnd** 요소를 보여 줍니다 ( **과정** 엔터티 형식의 업데이트 기능이 매핑되는).</span><span class="sxs-lookup"><span data-stu-id="445cd-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

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

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="445cd-166">AssociationSetMapping 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="445cd-167">MSL (매핑 사양 언어)의 **AssociationSetMapping** 요소는 개념적 모델의 연결과 기본 데이터베이스의 테이블 열 간의 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="445cd-168">개념적 모델의 연결은 기본 데이터베이스의 기본 및 외래 키 열을 나타내는 속성이 있는 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="445cd-169">**AssociationSetMapping** 요소는 두 개의 endproperty 요소를 사용 하 여 데이터베이스의 연결 유형 속성과 열 간의 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="445cd-170">Condition 요소를 사용하여 이러한 매핑에 조건을 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="445cd-171">ModificationFunctionMapping 요소를 사용하여 연결에 대한 삽입, 업데이트 및 삭제 함수를 데이터베이스의 저장 프로시저에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="445cd-172">QueryView 요소의 Entity SQL 문자열을 사용 하 여 연결과 테이블 열 간의 읽기 전용 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="445cd-173">개념적 모델의 연결에 대해 참조 제약 조건이 정의 된 경우 **AssociationSetMapping** 요소를 사용 하 여 연결을 매핑할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="445cd-174">참조 제약 조건이 있는 연결에 대해 **AssociationSetMapping** 요소가 있으면 **AssociationSetMapping** 요소에 정의 된 매핑이 무시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="445cd-175">자세한 내용은 참조를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="445cd-176">**AssociationSetMapping** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="445cd-177">QueryView (0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="445cd-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="445cd-178">EndProperty(0개 또는 2개)</span><span class="sxs-lookup"><span data-stu-id="445cd-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="445cd-179">조건 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="445cd-180">ModificationFunctionMapping (0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="445cd-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="445cd-181">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-181">Applicable Attributes</span></span>

<span data-ttu-id="445cd-182">다음 표에서는 **AssociationSetMapping** 요소에 적용 될 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="445cd-183">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-183">Attribute Name</span></span>     | <span data-ttu-id="445cd-184">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-184">Is Required</span></span> | <span data-ttu-id="445cd-185">값</span><span class="sxs-lookup"><span data-stu-id="445cd-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="445cd-186">**Name**</span><span class="sxs-lookup"><span data-stu-id="445cd-186">**Name**</span></span>           | <span data-ttu-id="445cd-187">예</span><span class="sxs-lookup"><span data-stu-id="445cd-187">Yes</span></span>         | <span data-ttu-id="445cd-188">매핑되는 개념적 모델 연결 집합의 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="445cd-189">**T**</span><span class="sxs-lookup"><span data-stu-id="445cd-189">**TypeName**</span></span>       | <span data-ttu-id="445cd-190">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-190">No</span></span>          | <span data-ttu-id="445cd-191">매핑되는 개념적 모델 연결 형식의 네임스페이스로 한정된 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="445cd-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="445cd-192">**StoreEntitySet**</span></span> | <span data-ttu-id="445cd-193">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-193">No</span></span>          | <span data-ttu-id="445cd-194">매핑되는 테이블의 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="445cd-195">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-195">Example</span></span>

<span data-ttu-id="445cd-196">다음 예에서는 개념적 모델에 설정 된 **FK\_과정\_부서** 연결이 데이터베이스의 **과정** 테이블에 매핑되는 **AssociationSetMapping** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="445cd-197">연결 형식 속성과 테이블 열 간의 매핑은 자식 **Endproperty** 요소에 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

## <a name="complexproperty-element-msl"></a><span data-ttu-id="445cd-198">ComplexProperty 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="445cd-199">MSL (매핑 사양 언어)의 **complexproperty** 요소는 개념적 모델 엔터티 형식의 복합 형식 속성과 기본 데이터베이스의 테이블 열 간의 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="445cd-200">속성-열 매핑은 자식 ScalarProperty 요소에 지정됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="445cd-201">**ComplexType** property 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="445cd-202">ScalarProperty (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="445cd-203">**Complexproperty** (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="445cd-204">ComplextTypeMapping (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="445cd-205">조건 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="445cd-206">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-206">Applicable Attributes</span></span>

<span data-ttu-id="445cd-207">다음 표에서는 **Complexproperty** 요소에 적용 되는 특성에 대해 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="445cd-208">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-208">Attribute Name</span></span> | <span data-ttu-id="445cd-209">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-209">Is Required</span></span> | <span data-ttu-id="445cd-210">값</span><span class="sxs-lookup"><span data-stu-id="445cd-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="445cd-211">**Name**</span><span class="sxs-lookup"><span data-stu-id="445cd-211">**Name**</span></span>       | <span data-ttu-id="445cd-212">예</span><span class="sxs-lookup"><span data-stu-id="445cd-212">Yes</span></span>         | <span data-ttu-id="445cd-213">개념적 모델에서 매핑되는 엔터티 형식의 복합 속성 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="445cd-214">**T**</span><span class="sxs-lookup"><span data-stu-id="445cd-214">**TypeName**</span></span>   | <span data-ttu-id="445cd-215">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-215">No</span></span>          | <span data-ttu-id="445cd-216">개념적 모델 속성 형식의 네임스페이스로 한정된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="445cd-217">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-217">Example</span></span>

<span data-ttu-id="445cd-218">다음 예제는 School 모델을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-218">The following example is based on the School model.</span></span> <span data-ttu-id="445cd-219">다음 복합 형식이 개념적 모델에 추가되었습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-219">The following complex type has been added to the conceptual model:</span></span>

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

<span data-ttu-id="445cd-220">**Person** 엔터티 형식의 **LastName** 및 **FirstName** 속성은 하나의 복합 속성인 **이름**으로 대체 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

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

<span data-ttu-id="445cd-221">다음 MSL은 **Name** 속성을 기본 데이터베이스의 열에 매핑하는 데 사용 되는 **complexproperty** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

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

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="445cd-222">ComplexTypeMapping 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="445cd-223">MSL (매핑 사양 언어)의 **Complextypemapping** 요소는 resultmapping 요소의 자식 이며, 다음과 같은 경우 개념적 모델의 function import와 기본 데이터베이스의 저장 프로시저 간 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="445cd-224">Function Import가 개념적 복합 형식을 반환하는 경우</span><span class="sxs-lookup"><span data-stu-id="445cd-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="445cd-225">저장 프로시저에서 반환되는 열 이름이 복합 형식의 속성 이름과 정확히 일치하지 않는 경우</span><span class="sxs-lookup"><span data-stu-id="445cd-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="445cd-226">기본적으로 저장 프로시저에서 반환되는 열과 복합 형식 간의 매핑은 열 및 속성 이름을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="445cd-227">열 이름이 속성 이름과 정확히 일치 하지 않는 경우에는 **Complextypemapping** 요소를 사용 하 여 매핑을 정의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="445cd-228">기본 매핑의 예는 FunctionImportMapping 요소 (MSL)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="445cd-229">**Complextypemapping** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="445cd-230">ScalarProperty (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="445cd-231">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-231">Applicable Attributes</span></span>

<span data-ttu-id="445cd-232">다음 표에서는 **Complextypemapping** 요소에 적용할 수 있는 특성에 대해 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="445cd-233">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-233">Attribute Name</span></span> | <span data-ttu-id="445cd-234">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-234">Is Required</span></span> | <span data-ttu-id="445cd-235">값</span><span class="sxs-lookup"><span data-stu-id="445cd-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="445cd-236">**T**</span><span class="sxs-lookup"><span data-stu-id="445cd-236">**TypeName**</span></span>   | <span data-ttu-id="445cd-237">예</span><span class="sxs-lookup"><span data-stu-id="445cd-237">Yes</span></span>         | <span data-ttu-id="445cd-238">매핑되는 복합 형식의 네임스페이스로 한정된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="445cd-239">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-239">Example</span></span>

<span data-ttu-id="445cd-240">다음과 같은 저장 프로시저가 있다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-240">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="445cd-241">또한 다음과 같은 개념적 모델 복합 형식이 있다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="445cd-242">이전 복합 형식의 인스턴스를 반환 하는 function import를 만들려면 저장 프로시저에서 반환 되는 열과 엔터티 형식 간의 매핑을 **Complextypemapping** 요소에 정의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

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

## <a name="condition-element-msl"></a><span data-ttu-id="445cd-243">Condition 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-243">Condition Element (MSL)</span></span>

<span data-ttu-id="445cd-244">MSL (매핑 사양 언어)의 **Condition** 요소는 개념적 모델과 기본 데이터베이스 간의 매핑에 대 한 조건을 배치 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="445cd-245">XML 노드 내에 정의 된 매핑은 자식 **조건** 요소에 지정 된 모든 조건이 충족 되는 경우 유효 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="445cd-246">그렇지 않으면 매핑이 유효하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="445cd-247">예를 들어 MappingFragment 요소에 하나 이상의 **Condition** 자식 요소가 포함 된 경우 **MappingFragment** 노드 내에 정의 된 매핑은 자식 **조건** 요소의 모든 조건이 충족 되는 경우에만 유효 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="445cd-248">각 조건은 이름 ( **name** 특성으로 지정 된 개념적 모델 엔터티 속성 **의 이름)** 또는 **columnname** ( **columnname** 특성으로 지정 된 데이터베이스의 열 이름) 중 하나에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="445cd-249">**Name** 특성이 설정 되 면 엔터티 속성 값을 기준으로 조건이 검사 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="445cd-250">**ColumnName** 특성이 설정 되 면 열 값을 기준으로 조건이 검사 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="445cd-251">**Condition** 요소에는 **Name** 또는 **ColumnName** 특성 중 하나만 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="445cd-252">FunctionImportMapping 요소 내에서 **Condition** 요소를 사용 하는 경우에는 **Name** 특성만 적용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="445cd-253">**Condition** 요소는 다음 요소의 자식일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="445cd-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="445cd-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="445cd-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="445cd-255">ComplexProperty</span></span>
-   <span data-ttu-id="445cd-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="445cd-256">EntitySetMapping</span></span>
-   <span data-ttu-id="445cd-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="445cd-257">MappingFragment</span></span>
-   <span data-ttu-id="445cd-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="445cd-258">EntityTypeMapping</span></span>

<span data-ttu-id="445cd-259">**Condition** 요소는 자식 요소를 가질 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="445cd-260">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-260">Applicable Attributes</span></span>

<span data-ttu-id="445cd-261">다음 표에서는 **Condition** 요소에 적용할 수 있는 특성에 대해 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="445cd-262">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-262">Attribute Name</span></span> | <span data-ttu-id="445cd-263">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-263">Is Required</span></span> | <span data-ttu-id="445cd-264">값</span><span class="sxs-lookup"><span data-stu-id="445cd-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="445cd-265">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="445cd-265">**ColumnName**</span></span> | <span data-ttu-id="445cd-266">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-266">No</span></span>          | <span data-ttu-id="445cd-267">조건을 확인하는 데 사용되는 값이 있는 테이블 열의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="445cd-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="445cd-268">**IsNull**</span></span>     | <span data-ttu-id="445cd-269">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-269">No</span></span>          | <span data-ttu-id="445cd-270">**True** 또는 **False**입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-270">**True** or **False**.</span></span> <span data-ttu-id="445cd-271">값이 **true** 이 고 열 값이 **null**이거나 값이 **False** 이 고 열 값이 **null**이 아닌 경우 조건은 true입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="445cd-272">그렇지 않으면 조건이 false입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="445cd-273">**IsNull** 및 **Value** 특성은 동시에 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="445cd-274">**Value**</span><span class="sxs-lookup"><span data-stu-id="445cd-274">**Value**</span></span>      | <span data-ttu-id="445cd-275">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-275">No</span></span>          | <span data-ttu-id="445cd-276">열 값과 비교할 값입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-276">The value with which the column value is compared.</span></span> <span data-ttu-id="445cd-277">값이 동일하면 조건이 true입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="445cd-278">그렇지 않으면 조건이 false입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="445cd-279">**IsNull** 및 **Value** 특성은 동시에 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="445cd-280">**Name**</span><span class="sxs-lookup"><span data-stu-id="445cd-280">**Name**</span></span>       | <span data-ttu-id="445cd-281">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-281">No</span></span>          | <span data-ttu-id="445cd-282">조건을 확인하는 데 사용되는 값이 있는 개념적 모델 엔터티 속성의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="445cd-283">**Condition** 요소가 FunctionImportMapping 요소 내에서 사용 되는 경우에는이 특성을 적용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="445cd-284">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-284">Example</span></span>

<span data-ttu-id="445cd-285">다음 예제에서는 **조건** 요소를 **MappingFragment** 요소의 자식으로 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="445cd-286">**HireDate** 가 null이 고 **EnrollmentDate** 가 null 인 경우 **SchoolModel** 형식과 **Person** 테이블의 **PersonID** 및 **HireDate** 열 간에 데이터가 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="445cd-287">**EnrollmentDate** 가 null이 고 **HireDate** 가 null 인 경우 **SchoolModel** 형식과 **Person** 테이블의 **PersonID** 및 **등록** 열 간에 데이터가 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

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

## <a name="deletefunction-element-msl"></a><span data-ttu-id="445cd-288">DeleteFunction 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="445cd-289">MSL (매핑 사양 언어)의 **deletefunction** 요소는 개념적 모델의 엔터티 형식 또는 연결에 대 한 삭제 함수를 기본 데이터베이스의 저장 프로시저에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="445cd-290">수정 함수가 매핑되는 저장 프로시저는 스토리지 모델에서 선언되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="445cd-291">자세한 내용은 Function 요소 (SSDL)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="445cd-292">엔터티 형식의 삽입, 업데이트 또는 삭제 작업을 모두 저장 프로시저에 매핑하지 않은 경우 런타임에 실행 하면 매핑되지 않은 작업이 실패 하 고 UpdateException이 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="445cd-293">EntityTypeMapping에 적용되는 DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="445cd-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="445cd-294">EntityTypeMapping 요소에 적용 될 때 **Deletefunction** 요소는 개념적 모델의 엔터티 형식에 대 한 삭제 함수를 저장 프로시저에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="445cd-295">**Deletefunction** 요소는 **entitytypemapping** 요소에 적용 될 때 다음과 같은 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="445cd-296">AssociationEnd (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="445cd-297">ComplexProperty(0개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="445cd-298">ScarlarProperty (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="445cd-299">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-299">Applicable Attributes</span></span>

<span data-ttu-id="445cd-300">다음 표에서는 **Entitytypemapping** 요소에 적용 될 때 **deletefunction** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="445cd-301">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-301">Attribute Name</span></span>            | <span data-ttu-id="445cd-302">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-302">Is Required</span></span> | <span data-ttu-id="445cd-303">값</span><span class="sxs-lookup"><span data-stu-id="445cd-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="445cd-304">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="445cd-304">**FunctionName**</span></span>          | <span data-ttu-id="445cd-305">예</span><span class="sxs-lookup"><span data-stu-id="445cd-305">Yes</span></span>         | <span data-ttu-id="445cd-306">삭제 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="445cd-307">저장 프로시저는 스토리지 모델에서 선언해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="445cd-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="445cd-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="445cd-309">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-309">No</span></span>          | <span data-ttu-id="445cd-310">영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="445cd-311">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-311">Example</span></span>

<span data-ttu-id="445cd-312">다음 예는 School 모델을 기반으로 하며 **Person** 엔터티 형식의 delete 함수를 **deletefunction** 저장 프로시저에 매핑하는 **deletefunction** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="445cd-313">**Deleteperson** 저장 프로시저는 저장소 모델에서 선언 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

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

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="445cd-314">AssociationSetMapping에 적용된 DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="445cd-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="445cd-315">**Deletefunction** 요소는 AssociationSetMapping 요소에 적용 될 때 개념적 모델에 있는 연결의 delete 함수를 저장 프로시저에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="445cd-316">**Deletefunction** 요소는 **AssociationSetMapping** 요소에 적용 될 때 다음과 같은 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="445cd-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="445cd-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="445cd-318">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-318">Applicable Attributes</span></span>

<span data-ttu-id="445cd-319">다음 표에서는 **AssociationSetMapping** 요소에 적용 될 때 **deletefunction** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="445cd-320">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-320">Attribute Name</span></span>            | <span data-ttu-id="445cd-321">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-321">Is Required</span></span> | <span data-ttu-id="445cd-322">값</span><span class="sxs-lookup"><span data-stu-id="445cd-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="445cd-323">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="445cd-323">**FunctionName**</span></span>          | <span data-ttu-id="445cd-324">예</span><span class="sxs-lookup"><span data-stu-id="445cd-324">Yes</span></span>         | <span data-ttu-id="445cd-325">삭제 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="445cd-326">저장 프로시저는 스토리지 모델에서 선언해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="445cd-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="445cd-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="445cd-328">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-328">No</span></span>          | <span data-ttu-id="445cd-329">영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="445cd-330">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-330">Example</span></span>

<span data-ttu-id="445cd-331">다음 예는 School 모델을 기반으로 하며 **CourseInstructor** 연결의 delete 함수를 **DeleteCourseInstructor** 저장 프로시저에 매핑하는 데 사용 되는 **deletefunction** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="445cd-332">**DeleteCourseInstructor** 저장 프로시저는 저장소 모델에서 선언 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="endproperty-element-msl"></a><span data-ttu-id="445cd-333">EndProperty 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="445cd-334">MSL (매핑 사양 언어)의 **Endproperty** 요소는 개념적 모델 연결의 끝 또는 수정 함수와 기본 데이터베이스 간의 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="445cd-335">속성-열 매핑은 자식 ScalarProperty 요소에서 지정됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="445cd-336">**Endproperty** 요소를 사용 하 여 개념적 모델 연결의 끝에 대 한 매핑을 정의할 경우이 요소는 AssociationSetMapping 요소의 자식입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="445cd-337">**Endproperty** 요소를 사용 하 여 개념적 모델 연결의 수정 함수에 대 한 매핑을 정의할 경우이 요소는 insertfunction 요소 또는 deletefunction 요소의 자식입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="445cd-338">**Endproperty** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="445cd-339">ScalarProperty (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="445cd-340">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-340">Applicable Attributes</span></span>

<span data-ttu-id="445cd-341">다음 표에서는 **Endproperty** 요소에 적용 되는 특성에 대해 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="445cd-342">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-342">Attribute Name</span></span> | <span data-ttu-id="445cd-343">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-343">Is Required</span></span> | <span data-ttu-id="445cd-344">값</span><span class="sxs-lookup"><span data-stu-id="445cd-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="445cd-345">이름</span><span class="sxs-lookup"><span data-stu-id="445cd-345">Name</span></span>           | <span data-ttu-id="445cd-346">예</span><span class="sxs-lookup"><span data-stu-id="445cd-346">Yes</span></span>         | <span data-ttu-id="445cd-347">매핑되는 연결 끝의 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="445cd-348">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-348">Example</span></span>

<span data-ttu-id="445cd-349">다음 예에서는 개념적 모델의 **FK\_과정\_부서** 연결이 데이터베이스의 **과정** 테이블에 매핑되는 **AssociationSetMapping** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="445cd-350">연결 형식 속성과 테이블 열 간의 매핑은 자식 **Endproperty** 요소에 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

### <a name="example"></a><span data-ttu-id="445cd-351">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-351">Example</span></span>

<span data-ttu-id="445cd-352">다음 예에서는 연결 (**CourseInstructor**)의 insert 및 delete 함수를 기본 데이터베이스의 저장 프로시저에 매핑하는 **endproperty** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="445cd-353">매핑되는 함수는 스토리지 모델에서 선언됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-353">The functions that are mapped to are declared in the storage model.</span></span>

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

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="445cd-354">EntityContainerMapping 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="445cd-355">MSL (매핑 사양 언어)의 **EntityContainerMapping** 요소는 개념적 모델의 엔터티 컨테이너를 저장소 모델의 엔터티 컨테이너에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="445cd-356">**EntityContainerMapping** 요소는 Mapping 요소의 자식입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="445cd-357">**EntityContainerMapping** 요소는 다음과 같은 자식 요소를 나열 된 순서 대로 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="445cd-358">EntitySetMapping(0개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="445cd-359">AssociationSetMapping (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="445cd-360">FunctionImportMapping (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="445cd-361">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-361">Applicable Attributes</span></span>

<span data-ttu-id="445cd-362">다음 표에서는 **EntityContainerMapping** 요소에 적용 될 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="445cd-363">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-363">Attribute Name</span></span>            | <span data-ttu-id="445cd-364">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-364">Is Required</span></span> | <span data-ttu-id="445cd-365">값</span><span class="sxs-lookup"><span data-stu-id="445cd-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="445cd-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="445cd-366">**StorageModelContainer**</span></span> | <span data-ttu-id="445cd-367">예</span><span class="sxs-lookup"><span data-stu-id="445cd-367">Yes</span></span>         | <span data-ttu-id="445cd-368">매핑되는 스토리지 모델 엔터티 컨테이너의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="445cd-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="445cd-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="445cd-370">예</span><span class="sxs-lookup"><span data-stu-id="445cd-370">Yes</span></span>         | <span data-ttu-id="445cd-371">매핑되는 개념적 모델 엔터티 컨테이너의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="445cd-372">**GenerateUpdateViews**</span><span class="sxs-lookup"><span data-stu-id="445cd-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="445cd-373">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-373">No</span></span>          | <span data-ttu-id="445cd-374">**True** 또는 **False**입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-374">**True** or **False**.</span></span> <span data-ttu-id="445cd-375">**False**이면 업데이트 보기가 생성 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="445cd-376">데이터를 성공적으로 왕복 하지 못할 수 있으므로 읽기 전용 매핑이 잘못 된 경우이 특성을 **False** 로 설정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="445cd-377">기본값은 **True**입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="445cd-378">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-378">Example</span></span>

<span data-ttu-id="445cd-379">다음 예제에서는 **SchoolModelEntities** 컨테이너 (개념적 모델 엔터티 컨테이너)를 **SchoolModelStoreContainer** 컨테이너 (저장소 모델 엔터티 컨테이너)에 매핑하는 **EntityContainerMapping** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

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

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="445cd-380">EntitySetMapping 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="445cd-381">MSL (매핑 사양 언어)의 **EntitySetMapping** 요소는 개념적 모델 엔터티 집합의 모든 형식을 저장소 모델의 엔터티 집합에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="445cd-382">개념적 모델의 엔터티 집합은 동일한 형식 및 파생된 형식을 갖는 엔터티 인스턴스의 논리적 컨테이너입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="445cd-383">저장소 모델의 엔터티 집합은 기본 데이터베이스의 테이블 또는 뷰를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="445cd-384">개념적 모델 엔터티 집합은 **EntitySetMapping** 요소의 **Name** 특성 값으로 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="445cd-385">매핑되는 테이블 또는 뷰는 각 자식 MappingFragment 요소 또는 **EntitySetMapping** 요소 자체의 자동 연결 **entityset** 특성에 의해 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="445cd-386">**EntitySetMapping** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="445cd-387">EntityTypeMapping(0개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="445cd-388">QueryView (0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="445cd-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="445cd-389">MappingFragment (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="445cd-390">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-390">Applicable Attributes</span></span>

<span data-ttu-id="445cd-391">다음 표에서는 **EntitySetMapping** 요소에 적용 될 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="445cd-392">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-392">Attribute Name</span></span>           | <span data-ttu-id="445cd-393">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-393">Is Required</span></span> | <span data-ttu-id="445cd-394">값</span><span class="sxs-lookup"><span data-stu-id="445cd-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="445cd-395">**Name**</span><span class="sxs-lookup"><span data-stu-id="445cd-395">**Name**</span></span>                 | <span data-ttu-id="445cd-396">예</span><span class="sxs-lookup"><span data-stu-id="445cd-396">Yes</span></span>         | <span data-ttu-id="445cd-397">매핑되는 개념적 모델 엔터티 집합의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="445cd-398">**TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="445cd-398">**TypeName** **1**</span></span>       | <span data-ttu-id="445cd-399">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-399">No</span></span>          | <span data-ttu-id="445cd-400">매핑되는 개념적 모델 엔터티 형식의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="445cd-401">을 (를) **entityset** **1**</span><span class="sxs-lookup"><span data-stu-id="445cd-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="445cd-402">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-402">No</span></span>          | <span data-ttu-id="445cd-403">매핑되는 대상 스토리지 모델 엔터티 집합의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="445cd-404">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="445cd-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="445cd-405">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-405">No</span></span>          | <span data-ttu-id="445cd-406">고유 행만 반환 되는지 여부에 따라 **True** 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="445cd-407">이 특성이 **True**로 설정 된 경우 EntityContainerMapping 요소의 **generateupdateviews** 특성을 **False**로 설정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="445cd-408">**1** Entitytypemapping 및 MappingFragment 자식 요소 대신 **TypeName** 및 **valentityset** 특성을 사용 하 여 단일 엔터티 형식을 단일 테이블에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="445cd-409">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-409">Example</span></span>

<span data-ttu-id="445cd-410">다음 예에서는 개념적 모델의 **강좌** 엔터티 집합에 있는 세 가지 형식 (기본 형식 및 두 개의 파생 형식)을 기본 데이터베이스에 있는 세 개의 다른 테이블에 매핑하는 **EntitySetMapping** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="445cd-411">테이블은 각 **MappingFragment** 요소의 **entityset** 특성에 의해 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

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

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="445cd-412">EntityTypeMapping 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="445cd-413">MSL (매핑 사양 언어)의 **Entitytypemapping** 요소는 개념적 모델의 엔터티 형식과 기본 데이터베이스의 테이블 또는 뷰 간의 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="445cd-414">개념적 모델 엔터티 형식과 기본 데이터베이스 테이블 또는 뷰에 대한 자세한 내용은 EntityType 요소(CSDL) 및 EntitySet 요소(SSDL)를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="445cd-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="445cd-415">매핑되는 개념적 모델 엔터티 형식은 **Entitytypemapping** 요소의 **TypeName** 특성에 의해 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="445cd-416">매핑되는 테이블 또는 뷰는 자식 MappingFragment 요소의 **entityset** 특성에 의해 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="445cd-417">ModificationFunctionMapping 자식 요소를 사용하여 엔터티 형식의 삽입, 업데이트 또는 삭제 함수를 데이터베이스의 저장 프로시저에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="445cd-418">**Entitytypemapping** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="445cd-419">MappingFragment (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="445cd-420">ModificationFunctionMapping (0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="445cd-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="445cd-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="445cd-421">ScalarProperty</span></span>
-   <span data-ttu-id="445cd-422">조건</span><span class="sxs-lookup"><span data-stu-id="445cd-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="445cd-423">**MappingFragment** 및 **ModificationFunctionMapping** 요소는 **entitytypemapping** 요소의 자식 요소일 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="445cd-424">**ScalarProperty** 및 **Condition** 요소는 functionimportmapping 요소 내에서 사용 될 때 **entitytypemapping** 요소의 자식 요소일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="445cd-425">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-425">Applicable Attributes</span></span>

<span data-ttu-id="445cd-426">다음 표에서는 **Entitytypemapping** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="445cd-427">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-427">Attribute Name</span></span> | <span data-ttu-id="445cd-428">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-428">Is Required</span></span> | <span data-ttu-id="445cd-429">값</span><span class="sxs-lookup"><span data-stu-id="445cd-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="445cd-430">**T**</span><span class="sxs-lookup"><span data-stu-id="445cd-430">**TypeName**</span></span>   | <span data-ttu-id="445cd-431">예</span><span class="sxs-lookup"><span data-stu-id="445cd-431">Yes</span></span>         | <span data-ttu-id="445cd-432">매핑되는 개념적 모델 엔터티 형식의 네임스페이스로 한정된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="445cd-433">해당 형식이 추상 또는 파생 형식이면 이 값은 `IsOfType(Namespace-qualified_type_name)`이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="445cd-434">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-434">Example</span></span>

<span data-ttu-id="445cd-435">다음 예제에서는 두 개의 자식 **Entitytypemapping** 요소가 있는 EntitySetMapping 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="445cd-436">첫 번째 **Entitytypemapping** 요소에서 **SchoolModel** 엔터티 형식은 **person** 테이블에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="445cd-437">두 번째 **Entitytypemapping** 요소에서 **SchoolModel** 형식의 업데이트 기능은 데이터베이스의 저장 프로시저인 **updateperson**에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="445cd-438">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-438">Example</span></span>

<span data-ttu-id="445cd-439">다음 예제에서는 루트 형식이 추상인 형식 계층 구조의 매핑을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="445cd-440">**TypeName** 특성에는 `IsOfType` 구문을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

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

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="445cd-441">FunctionImportMapping 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="445cd-442">MSL (매핑 사양 언어)의 **Functionimportmapping** 요소는 개념적 모델의 function import와 기본 데이터베이스의 저장 프로시저 또는 함수 간 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="445cd-443">Function Import는 개념적 모델에서 선언해야 하고 저장 프로시저는 스토리지 모델에서 선언해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="445cd-444">자세한 내용은 FunctionImport 요소 (CSDL) 및 Function 요소 (SSDL)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="445cd-445">기본적으로, Function Import에서 개념적 모델 엔터티 형식이나 복합 형식을 반환하는 경우에는 기본 저장 프로시저에서 반환된 열 이름이 개념적 모델 형식의 속성 이름과 정확히 일치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="445cd-446">열 이름이 속성 이름과 정확히 일치 하지 않는 경우에는이 매핑을 ResultMapping 요소에 정의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="445cd-447">**Functionimportmapping** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="445cd-448">ResultMapping (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="445cd-449">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-449">Applicable Attributes</span></span>

<span data-ttu-id="445cd-450">다음 표에서는 **Functionimportmapping** 요소에 적용할 수 있는 특성에 대해 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="445cd-451">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-451">Attribute Name</span></span>         | <span data-ttu-id="445cd-452">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-452">Is Required</span></span> | <span data-ttu-id="445cd-453">값</span><span class="sxs-lookup"><span data-stu-id="445cd-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="445cd-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="445cd-454">**FunctionImportName**</span></span> | <span data-ttu-id="445cd-455">예</span><span class="sxs-lookup"><span data-stu-id="445cd-455">Yes</span></span>         | <span data-ttu-id="445cd-456">매핑되는 개념적 모델의 Function Import 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="445cd-457">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="445cd-457">**FunctionName**</span></span>       | <span data-ttu-id="445cd-458">예</span><span class="sxs-lookup"><span data-stu-id="445cd-458">Yes</span></span>         | <span data-ttu-id="445cd-459">매핑되는 스토리지 모델 함수의 네임스페이스로 한정된 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="445cd-460">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-460">Example</span></span>

<span data-ttu-id="445cd-461">다음 예제는 School 모델을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-461">The following example is based on the School model.</span></span> <span data-ttu-id="445cd-462">다음은 스토리지 모델의 함수입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="445cd-463">다음은 개념적 모델의 Function Import입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="445cd-464">다음 예제에서는 위의 함수와 함수 가져오기를 서로 매핑하는 데 사용 되는 **Functionimportmapping** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="445cd-465">InsertFunction 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="445cd-466">MSL (매핑 사양 언어)의 **insertfunction** 요소는 개념적 모델의 엔터티 형식 또는 연결에 대 한 삽입 함수를 기본 데이터베이스의 저장 프로시저에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="445cd-467">수정 함수가 매핑되는 저장 프로시저는 스토리지 모델에서 선언되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="445cd-468">자세한 내용은 Function 요소 (SSDL)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="445cd-469">엔터티 형식의 삽입, 업데이트 또는 삭제 작업을 모두 저장 프로시저에 매핑하지 않은 경우 런타임에 실행 하면 매핑되지 않은 작업이 실패 하 고 UpdateException이 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="445cd-470">**Insertfunction** 요소는 ModificationFunctionMapping 요소의 자식일 수 있으며 entitytypemapping 요소 또는 AssociationSetMapping 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="445cd-471">EntityTypeMapping에 적용되는 InsertFunction</span><span class="sxs-lookup"><span data-stu-id="445cd-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="445cd-472">EntityTypeMapping 요소에 적용 될 때 **Insertfunction** 요소는 개념적 모델의 엔터티 형식에 대 한 삽입 함수를 저장 프로시저에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="445cd-473">**Insertfunction** 요소는 **entitytypemapping** 요소에 적용 될 때 다음과 같은 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="445cd-474">AssociationEnd (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="445cd-475">ComplexProperty(0개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="445cd-476">ResultBinding (0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="445cd-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="445cd-477">ScarlarProperty (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="445cd-478">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-478">Applicable Attributes</span></span>

<span data-ttu-id="445cd-479">다음 표에서는 **Entitytypemapping** 요소에 적용 될 때 **insertfunction** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="445cd-480">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-480">Attribute Name</span></span>            | <span data-ttu-id="445cd-481">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-481">Is Required</span></span> | <span data-ttu-id="445cd-482">값</span><span class="sxs-lookup"><span data-stu-id="445cd-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="445cd-483">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="445cd-483">**FunctionName**</span></span>          | <span data-ttu-id="445cd-484">예</span><span class="sxs-lookup"><span data-stu-id="445cd-484">Yes</span></span>         | <span data-ttu-id="445cd-485">삽입 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="445cd-486">저장 프로시저는 스토리지 모델에서 선언해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="445cd-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="445cd-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="445cd-488">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-488">No</span></span>          | <span data-ttu-id="445cd-489">영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="445cd-490">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-490">Example</span></span>

<span data-ttu-id="445cd-491">다음 예는 School 모델을 기반으로 하며 Person 엔터티 형식의 삽입 함수를 **Insertfunction** 저장 프로시저에 매핑하는 데 사용 되는 **insertfunction** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="445cd-492">**Insertperson** 저장 프로시저는 저장소 모델에서 선언 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

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
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="445cd-493">AssociationSetMapping에 적용된 InsertFunction</span><span class="sxs-lookup"><span data-stu-id="445cd-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="445cd-494">AssociationSetMapping 요소에 적용 될 때 **insertfunction** 요소는 개념적 모델에 있는 연결의 삽입 함수를 저장 프로시저에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="445cd-495">**Insertfunction** 요소는 **AssociationSetMapping** 요소에 적용 될 때 다음과 같은 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="445cd-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="445cd-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="445cd-497">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-497">Applicable Attributes</span></span>

<span data-ttu-id="445cd-498">다음 표에서는 **AssociationSetMapping** 요소에 적용 될 때 **insertfunction** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="445cd-499">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-499">Attribute Name</span></span>            | <span data-ttu-id="445cd-500">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-500">Is Required</span></span> | <span data-ttu-id="445cd-501">값</span><span class="sxs-lookup"><span data-stu-id="445cd-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="445cd-502">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="445cd-502">**FunctionName**</span></span>          | <span data-ttu-id="445cd-503">예</span><span class="sxs-lookup"><span data-stu-id="445cd-503">Yes</span></span>         | <span data-ttu-id="445cd-504">삽입 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="445cd-505">저장 프로시저는 스토리지 모델에서 선언해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="445cd-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="445cd-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="445cd-507">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-507">No</span></span>          | <span data-ttu-id="445cd-508">영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="445cd-509">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-509">Example</span></span>

<span data-ttu-id="445cd-510">다음 예는 School 모델을 기반으로 하며 **CourseInstructor** 연결의 insert 함수를 **InsertCourseInstructor** 저장 프로시저에 매핑하는 데 사용 되는 **insertfunction** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="445cd-511">**InsertCourseInstructor** 저장 프로시저는 저장소 모델에서 선언 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="mapping-element-msl"></a><span data-ttu-id="445cd-512">Mapping 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="445cd-513">MSL (매핑 사양 언어)의 **mapping** 요소에는 개념적 모델에 정의 된 개체를 데이터베이스에 매핑하기 위한 정보 (저장소 모델에 설명 된 대로)가 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="445cd-514">자세한 내용은 CSDL 사양 및 SSDL 사양을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="445cd-515">매핑 **요소는** 매핑 사양에 대 한 루트 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="445cd-516">매핑 사양에 대 한 XML 네임 스페이스를 https://schemas.microsoft.com/ado/2009/11/mapping/cs합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-516">The XML namespace for mapping specifications is https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="445cd-517">Mapping 요소는 다음에 나열된 순서대로 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="445cd-518">별칭 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="445cd-519">EntityContainerMapping (정확히 1 개)</span><span class="sxs-lookup"><span data-stu-id="445cd-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="445cd-520">MSL에서 참조하는 개념적 모델 형식과 스토리지 모델 형식의 이름은 해당 네임스페이스 이름으로 한정되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="445cd-521">개념적 모델 네임 스페이스 이름에 대 한 자세한 내용은 Schema 요소 (CSDL)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="445cd-522">저장소 모델 네임 스페이스 이름에 대 한 자세한 내용은 Schema 요소 (SSDL)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="445cd-523">MSL에서 사용되는 네임스페이스의 별칭은 Alias 요소를 사용하여 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="445cd-524">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-524">Applicable Attributes</span></span>

<span data-ttu-id="445cd-525">다음 표에서는 **Mapping** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="445cd-526">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-526">Attribute Name</span></span> | <span data-ttu-id="445cd-527">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-527">Is Required</span></span> | <span data-ttu-id="445cd-528">값</span><span class="sxs-lookup"><span data-stu-id="445cd-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="445cd-529">**스페이스바**</span><span class="sxs-lookup"><span data-stu-id="445cd-529">**Space**</span></span>      | <span data-ttu-id="445cd-530">예</span><span class="sxs-lookup"><span data-stu-id="445cd-530">Yes</span></span>         | <span data-ttu-id="445cd-531">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="445cd-531">**C-S**.</span></span> <span data-ttu-id="445cd-532">이 값은 고정된 값이므로 변경할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="445cd-533">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-533">Example</span></span>

<span data-ttu-id="445cd-534">다음 예에서는 School 모델의 일부를 기반으로 하는 **Mapping** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="445cd-535">School 모델에 대 한 자세한 내용은 빠른 시작 (Entity Framework)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

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

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="445cd-536">MappingFragment 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="445cd-537">MSL (매핑 사양 언어)의 **MappingFragment** 요소는 개념적 모델 엔터티 형식의 속성과 데이터베이스의 테이블 또는 뷰 간의 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="445cd-538">개념적 모델 엔터티 형식과 기본 데이터베이스 테이블 또는 뷰에 대한 자세한 내용은 EntityType 요소(CSDL) 및 EntitySet 요소(SSDL)를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="445cd-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="445cd-539">**MappingFragment** 는 entitytypemapping 요소 또는 EntitySetMapping 요소의 자식 요소일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="445cd-540">**MappingFragment** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="445cd-541">ComplexType (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="445cd-542">ScalarProperty (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="445cd-543">조건 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="445cd-544">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-544">Applicable Attributes</span></span>

<span data-ttu-id="445cd-545">다음 표에서는 **MappingFragment** 요소에 적용 될 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="445cd-546">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-546">Attribute Name</span></span>          | <span data-ttu-id="445cd-547">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-547">Is Required</span></span> | <span data-ttu-id="445cd-548">값</span><span class="sxs-lookup"><span data-stu-id="445cd-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="445cd-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="445cd-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="445cd-550">예</span><span class="sxs-lookup"><span data-stu-id="445cd-550">Yes</span></span>         | <span data-ttu-id="445cd-551">매핑되는 테이블 또는 뷰의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="445cd-552">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="445cd-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="445cd-553">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-553">No</span></span>          | <span data-ttu-id="445cd-554">고유 행만 반환 되는지 여부에 따라 **True** 또는 **False** 입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="445cd-555">이 특성이 **True**로 설정 된 경우 EntityContainerMapping 요소의 **generateupdateviews** 특성을 **False**로 설정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="445cd-556">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-556">Example</span></span>

<span data-ttu-id="445cd-557">다음 예제에서는 **MappingFragment** 요소를 **entitytypemapping** 요소의 자식으로 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="445cd-558">이 예제에서 개념적 모델의 **과정** 유형 속성은 데이터베이스의 **과정** 테이블 열에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="445cd-559">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-559">Example</span></span>

<span data-ttu-id="445cd-560">다음 예제에서는 **MappingFragment** 요소를 **EntitySetMapping** 요소의 자식으로 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="445cd-561">위의 예제와 같이 개념적 모델의 **과정** 유형 속성은 데이터베이스의 **과정** 테이블 열에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="445cd-562">ModificationFunctionMapping 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="445cd-563">MSL (매핑 사양 언어)의 **ModificationFunctionMapping** 요소는 개념적 모델 엔터티 형식의 삽입, 업데이트 및 삭제 함수를 기본 데이터베이스의 저장 프로시저에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="445cd-564">**ModificationFunctionMapping** 요소는 개념적 모델의 다 대 다 연결에 대 한 삽입 및 삭제 함수를 기본 데이터베이스의 저장 프로시저에 매핑할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="445cd-565">수정 함수가 매핑되는 저장 프로시저는 스토리지 모델에서 선언되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="445cd-566">자세한 내용은 Function 요소 (SSDL)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="445cd-567">엔터티 형식의 삽입, 업데이트 또는 삭제 작업을 모두 저장 프로시저에 매핑하지 않은 경우 런타임에 실행 하면 매핑되지 않은 작업이 실패 하 고 UpdateException이 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="445cd-568">상속 계층 구조의 한 엔터티에 대한 수정 함수가 저장 프로시저에 매핑된 경우 계층 구조의 모든 형식에 대한 수정 함수가 저장 프로시저에 매핑되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="445cd-569">**ModificationFunctionMapping** 요소는 entitytypemapping 요소 또는 AssociationSetMapping 요소의 자식일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="445cd-570">**ModificationFunctionMapping** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="445cd-571">DeleteFunction (0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="445cd-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="445cd-572">InsertFunction (0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="445cd-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="445cd-573">UpdateFunction (0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="445cd-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="445cd-574">**ModificationFunctionMapping** 요소에 적용할 수 있는 특성이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="445cd-575">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-575">Example</span></span>

<span data-ttu-id="445cd-576">다음 예제에서는 School 모델의 **인물** 엔터티 집합에 대 한 엔터티 집합 매핑을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="445cd-577">**Person** 엔터티 형식에 대 한 열 매핑 외에도 **person** 형식의 삽입, 업데이트 및 삭제 함수에 대 한 매핑이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="445cd-578">매핑되는 함수는 스토리지 모델에서 선언됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-578">The functions that are mapped to are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="445cd-579">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-579">Example</span></span>

<span data-ttu-id="445cd-580">다음 예에서는 School 모델의 **CourseInstructor** 연결 집합에 대 한 연결 집합 매핑을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="445cd-581">**CourseInstructor** 연결에 대 한 열 매핑 외에도 **CourseInstructor** 연결의 insert 및 delete 함수 매핑이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="445cd-582">매핑되는 함수는 스토리지 모델에서 선언됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-582">The functions that are mapped to are declared in the storage model.</span></span>

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
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="445cd-583">QueryView 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="445cd-584">MSL (매핑 사양 언어)의 **QueryView** 요소는 개념적 모델의 엔터티 형식 또는 연결 및 기본 데이터베이스의 테이블 간에 읽기 전용 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="445cd-585">매핑은 저장소 모델에 대해 평가 되는 Entity SQL 쿼리로 정의 되며, 개념적 모델에서 엔터티 또는 연결을 기준으로 결과 집합을 표현 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="445cd-586">쿼리 뷰는 읽기 전용이므로 표준 업데이트 명령을 사용하여 쿼리 뷰에서 정의된 형식을 업데이트할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="445cd-587">수정 함수를 사용하여 이러한 형식을 업데이트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="445cd-588">자세한 내용은 방법: 저장 프로시저에 수정 함수 매핑을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="445cd-589">**QueryView** 요소에는 **GroupBy**, group 집계 또는 탐색 속성을 포함 하는 Entity SQL 식이 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="445cd-590">**QueryView** 요소는 EntitySetMapping 요소 또는 AssociationSetMapping 요소의 자식일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="445cd-591">쿼리 뷰는 앞의 자식 요소의 경우 개념적 모델의 엔터티에 대한 읽기 전용 매핑을 정의하고,</span><span class="sxs-lookup"><span data-stu-id="445cd-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="445cd-592">뒤의 자식 요소의 경우 개념적 모델의 연결에 대한 읽기 전용 매핑을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="445cd-593">**AssociationSetMapping** 요소가 참조 제약 조건이 있는 연결에 대 한 것 이면 **AssociationSetMapping** 요소가 무시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="445cd-594">자세한 내용은 참조를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="445cd-595">**QueryView** 요소에는 자식 요소가 있을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="445cd-596">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-596">Applicable Attributes</span></span>

<span data-ttu-id="445cd-597">다음 표에서는 **QueryView** 요소에 적용 될 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="445cd-598">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-598">Attribute Name</span></span> | <span data-ttu-id="445cd-599">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-599">Is Required</span></span> | <span data-ttu-id="445cd-600">값</span><span class="sxs-lookup"><span data-stu-id="445cd-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="445cd-601">**T**</span><span class="sxs-lookup"><span data-stu-id="445cd-601">**TypeName**</span></span>   | <span data-ttu-id="445cd-602">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-602">No</span></span>          | <span data-ttu-id="445cd-603">쿼리 뷰에 의해 매핑되는 개념적 모델 형식의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="445cd-604">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-604">Example</span></span>

<span data-ttu-id="445cd-605">다음 예에서는 **QueryView** 요소를 **EntitySetMapping** 요소의 자식으로 보여 주고 School 모델에서 **부서** 엔터티 형식에 대 한 쿼리 뷰 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

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

<span data-ttu-id="445cd-606">이 쿼리는 저장소 모델에서 **부서** 유형의 멤버 하위 집합만 반환 하므로 School 모델의 **부서** 유형은 다음과 같이이 매핑을 기반으로 수정 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

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

### <a name="example"></a><span data-ttu-id="445cd-607">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-607">Example</span></span>

<span data-ttu-id="445cd-608">다음 예제에서는 **QueryView** 요소를 **AssociationSetMapping** 요소의 자식으로 보여 주고 School 모델에서 `FK_Course_Department` 연결에 대 한 읽기 전용 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

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
 
### <a name="comments"></a><span data-ttu-id="445cd-609">설명</span><span class="sxs-lookup"><span data-stu-id="445cd-609">Comments</span></span>

<span data-ttu-id="445cd-610">다음 시나리오가 가능하도록 쿼리 뷰를 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="445cd-611">스토리지 모델에 있는 엔터티의 모든 속성을 포함하지 않는 엔터티를 개념적 모델에서 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="445cd-612">여기에는 기본값이 없고 **null** 값을 지원 하지 않는 속성이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="445cd-613">스토리지 모델의 계산 열을 개념적 모델의 엔터티 형식 속성에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="445cd-614">개념적 모델의 엔터티를 분할하는 데 사용된 조건이 같음을 기반으로 하지 않는 매핑을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="445cd-615">**조건** 요소를 사용 하 여 조건부 매핑을 지정 하는 경우 제공 된 조건은 지정 된 값과 같아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="445cd-616">자세한 내용은 Condition 요소 (MSL)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="445cd-617">스토리지 모델의 같은 열을 개념적 모델의 여러 형식에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="445cd-618">여러 형식을 같은 테이블에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="445cd-619">관계형 스키마에서 외래 키를 기반으로 하지 않는 연결을 개념적 모델에서 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="445cd-620">사용자 지정 비즈니스 논리를 사용하여 개념적 모델에서 속성 값을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="445cd-621">예를 들어 개념적 모델의 부울 값인 **true**값에 데이터 소스의 문자열 값 "T"를 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="445cd-622">쿼리 결과에 대한 조건부 필터를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="445cd-623">스토리지 모델보다 더 적은 제한을 개념적 모델의 데이터에 대해 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="445cd-624">예를 들어 매핑되는 열이 **null**값을 지원 하지 않는 경우에도 개념적 모델에서 속성을 nullable로 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="445cd-625">엔터티에 대한 쿼리 뷰를 정의할 때는 다음 사항을 고려해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="445cd-626">쿼리 뷰는 읽기 전용입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-626">Query views are read-only.</span></span> <span data-ttu-id="445cd-627">수정 함수를 사용하여 엔터티를 업데이트할 수만 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="445cd-628">쿼리 뷰로 엔터티 형식을 정의하는 경우 관련된 모든 엔터티도 쿼리 뷰로 정의해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="445cd-629">다대다 연결을 관계형 스키마의 링크 테이블을 나타내는 저장소 모델의 엔터티에 매핑하는 경우이 링크 테이블의 **AssociationSetMapping** 요소에 **QueryView** 요소를 정의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="445cd-630">쿼리 뷰는 형식 계층 구조의 모든 형식에 대해 정의되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="445cd-631">다음과 같은 방법으로 이 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="445cd-632">계층 구조에 있는 모든 엔터티 형식의 합집합을 반환 하는 단일 Entity SQL 쿼리를 지정 하는 단일 **QueryView** 요소를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="445cd-633">CASE 연산자를 사용 하 여 특정 조건에 따라 계층 구조에서 특정 엔터티 형식을 반환 하는 단일 Entity SQL 쿼리를 지정 하는 단일 **QueryView** 요소를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="445cd-634">계층의 특정 형식에 대 한 추가 **QueryView** 요소 사용.</span><span class="sxs-lookup"><span data-stu-id="445cd-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="445cd-635">이 경우 **QueryView** 요소의 **TypeName** 특성을 사용 하 여 각 뷰의 엔터티 형식을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="445cd-636">쿼리 뷰를 정의 하는 경우에는 **EntitySetMapping** 요소에 **StorageSetName** 특성을 지정할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="445cd-637">쿼리 뷰가 정의 된 경우 **EntitySetMapping**요소는 **속성** 매핑을 포함할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="445cd-638">ResultBinding 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="445cd-639">MSL (매핑 사양 언어)의 **Resultbinding** 요소는 엔터티 형식 수정 함수를 기본 데이터베이스의 저장 프로시저에 매핑할 때 저장 프로시저에서 반환 되는 열 값을 개념적 모델의 엔터티 속성에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="445cd-640">예를 들어 insert 저장 프로시저에서 identity 열의 값을 반환 하는 경우 **Resultbinding** 요소는 반환 된 값을 개념적 모델의 엔터티 형식 속성에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="445cd-641">**Resultbinding** 요소는 insertfunction 요소 또는 updatefunction 요소의 자식일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="445cd-642">**Resultbinding** 요소에는 자식 요소가 있을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="445cd-643">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-643">Applicable Attributes</span></span>

<span data-ttu-id="445cd-644">다음 표에서는 **Resultbinding** 요소에 적용할 수 있는 특성에 대해 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="445cd-645">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-645">Attribute Name</span></span> | <span data-ttu-id="445cd-646">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-646">Is Required</span></span> | <span data-ttu-id="445cd-647">값</span><span class="sxs-lookup"><span data-stu-id="445cd-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="445cd-648">**Name**</span><span class="sxs-lookup"><span data-stu-id="445cd-648">**Name**</span></span>       | <span data-ttu-id="445cd-649">예</span><span class="sxs-lookup"><span data-stu-id="445cd-649">Yes</span></span>         | <span data-ttu-id="445cd-650">매핑되는 개념적 모델의 엔터티 속성 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="445cd-651">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="445cd-651">**ColumnName**</span></span> | <span data-ttu-id="445cd-652">예</span><span class="sxs-lookup"><span data-stu-id="445cd-652">Yes</span></span>         | <span data-ttu-id="445cd-653">매핑되는 열의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="445cd-654">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-654">Example</span></span>

<span data-ttu-id="445cd-655">다음 예는 School 모델을 기반으로 하며 **Person** 엔터티 형식의 삽입 함수를 **insertfunction** 저장 프로시저에 매핑하는 데 사용 되는 **insertfunction** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="445cd-656">**Insertperson** 저장 프로시저는 아래와 같이 저장소 모델에서 선언 됩니다. **Resultbinding** 요소는 저장 프로시저 (**NewPersonID**)에서 반환 되는 열 값을 엔터티 형식 속성 (**PersonID**)에 매핑하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

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

<span data-ttu-id="445cd-657">다음 Transact-sql은 **Insertperson** 저장 프로시저에 대해 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

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

## <a name="resultmapping-element-msl"></a><span data-ttu-id="445cd-658">ResultMapping 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="445cd-659">MSL (매핑 사양 언어)의 **Resultmapping** 요소는 다음이 true 일 때 개념적 모델의 function import와 기본 데이터베이스의 저장 프로시저 간 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="445cd-660">Function Import가 개념적 모델 엔터티 형식 또는 복합 형식을 반환하는 경우</span><span class="sxs-lookup"><span data-stu-id="445cd-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="445cd-661">저장 프로시저에서 반환되는 열 이름이 엔터티 형식 또는 복합 형식의 속성 이름과 정확히 일치하지 않는 경우</span><span class="sxs-lookup"><span data-stu-id="445cd-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="445cd-662">기본적으로 저장 프로시저에서 반환되는 열과 엔터티 형식 또는 복합 형식 간의 매핑은 열 및 속성 이름을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="445cd-663">열 이름이 속성 이름과 정확히 일치 하지 않는 경우 **resultmapping** 요소를 사용 하 여 매핑을 정의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="445cd-664">기본 매핑의 예는 FunctionImportMapping 요소 (MSL)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="445cd-665">**Resultmapping** 요소는 functionimportmapping 요소의 자식 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="445cd-666">**Resultmapping** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="445cd-667">EntityTypeMapping(0개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="445cd-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="445cd-668">ComplexTypeMapping</span></span>

<span data-ttu-id="445cd-669">**Resultmapping** 요소에는 특성을 적용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="445cd-670">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-670">Example</span></span>

<span data-ttu-id="445cd-671">다음과 같은 저장 프로시저가 있다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-671">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="445cd-672">또한 다음과 같은 개념적 모델 엔터티 형식이 있다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-672">Also consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="445cd-673">이전 엔터티 형식의 인스턴스를 반환 하는 function import를 만들려면 저장 프로시저에서 반환 되는 열과 엔터티 형식 간의 매핑이 **Resultmapping** 요소에 정의 되어 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

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

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="445cd-674">ScalarProperty 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="445cd-675">MSL (매핑 사양 언어)의 **ScalarProperty** 요소는 개념적 모델 엔터티 형식, 복합 형식 또는 연결의 속성을 기본 데이터베이스의 테이블 열 또는 저장 프로시저 매개 변수에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="445cd-676">수정 함수가 매핑되는 저장 프로시저는 스토리지 모델에서 선언되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="445cd-677">자세한 내용은 Function 요소 (SSDL)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="445cd-678">**ScalarProperty** 요소는 다음 요소의 자식일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="445cd-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="445cd-679">MappingFragment</span></span>
-   <span data-ttu-id="445cd-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="445cd-680">InsertFunction</span></span>
-   <span data-ttu-id="445cd-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="445cd-681">UpdateFunction</span></span>
-   <span data-ttu-id="445cd-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="445cd-682">DeleteFunction</span></span>
-   <span data-ttu-id="445cd-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="445cd-683">EndProperty</span></span>
-   <span data-ttu-id="445cd-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="445cd-684">ComplexProperty</span></span>
-   <span data-ttu-id="445cd-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="445cd-685">ResultMapping</span></span>

<span data-ttu-id="445cd-686">**MappingFragment**, **Complexproperty**또는 **Endproperty** 요소의 자식으로 **ScalarProperty** 요소는 개념적 모델의 속성을 데이터베이스의 열에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="445cd-687">**Insertfunction**, **Updatefunction**또는 **Deletefunction** 요소의 자식으로 **ScalarProperty** 요소는 개념적 모델의 속성을 저장 프로시저 매개 변수에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="445cd-688">**ScalarProperty** 요소에는 자식 요소가 있을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="445cd-689">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-689">Applicable Attributes</span></span>

<span data-ttu-id="445cd-690">**ScalarProperty** 요소에 적용 되는 특성은 요소의 역할에 따라 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="445cd-691">다음 표에서는 **ScalarProperty** 요소를 사용 하 여 개념적 모델 속성을 데이터베이스의 열에 매핑하는 경우 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="445cd-692">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-692">Attribute Name</span></span> | <span data-ttu-id="445cd-693">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-693">Is Required</span></span> | <span data-ttu-id="445cd-694">값</span><span class="sxs-lookup"><span data-stu-id="445cd-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="445cd-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="445cd-695">**Name**</span></span>       | <span data-ttu-id="445cd-696">예</span><span class="sxs-lookup"><span data-stu-id="445cd-696">Yes</span></span>         | <span data-ttu-id="445cd-697">매핑되는 개념적 모델 속성의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="445cd-698">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="445cd-698">**ColumnName**</span></span> | <span data-ttu-id="445cd-699">예</span><span class="sxs-lookup"><span data-stu-id="445cd-699">Yes</span></span>         | <span data-ttu-id="445cd-700">매핑되는 테이블 열의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="445cd-701">다음 표에서는 개념적 모델 속성을 저장 프로시저 매개 변수에 매핑하는 데 사용할 때 **ScalarProperty** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="445cd-702">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-702">Attribute Name</span></span>    | <span data-ttu-id="445cd-703">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-703">Is Required</span></span> | <span data-ttu-id="445cd-704">값</span><span class="sxs-lookup"><span data-stu-id="445cd-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="445cd-705">**Name**</span><span class="sxs-lookup"><span data-stu-id="445cd-705">**Name**</span></span>          | <span data-ttu-id="445cd-706">예</span><span class="sxs-lookup"><span data-stu-id="445cd-706">Yes</span></span>         | <span data-ttu-id="445cd-707">매핑되는 개념적 모델 속성의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="445cd-708">**ParameterName**</span><span class="sxs-lookup"><span data-stu-id="445cd-708">**ParameterName**</span></span> | <span data-ttu-id="445cd-709">예</span><span class="sxs-lookup"><span data-stu-id="445cd-709">Yes</span></span>         | <span data-ttu-id="445cd-710">매핑되는 매개 변수의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="445cd-711">**버전**</span><span class="sxs-lookup"><span data-stu-id="445cd-711">**Version**</span></span>       | <span data-ttu-id="445cd-712">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-712">No</span></span>          | <span data-ttu-id="445cd-713">현재 값 또는 속성의 원래 값이 동시성 검사에 사용 되어야 하는지 여부에 따라 **현재** 또는 **원래** 값입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="445cd-714">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-714">Example</span></span>

<span data-ttu-id="445cd-715">다음 예에서는 두 가지 방법으로 사용 되는 **ScalarProperty** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="445cd-716">**Person** 엔터티 형식의 속성을 **person**테이블의 열에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="445cd-717">**Person** 엔터티 형식의 속성을 **updateperson** 저장 프로시저의 매개 변수에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="445cd-718">저장 프로시저는 스토리지 모델에서 선언해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-718">The stored procedures are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="445cd-719">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-719">Example</span></span>

<span data-ttu-id="445cd-720">다음 예에서는 개념적 모델 연결의 insert 및 delete 함수를 데이터베이스의 저장 프로시저에 매핑하는 데 사용 되는 **ScalarProperty** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="445cd-721">저장 프로시저는 스토리지 모델에서 선언해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-721">The stored procedures are declared in the storage model.</span></span>

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

## <a name="updatefunction-element-msl"></a><span data-ttu-id="445cd-722">UpdateFunction 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="445cd-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="445cd-723">MSL (매핑 사양 언어)의 **updatefunction** 요소는 개념적 모델의 엔터티 형식에 대 한 업데이트 함수를 기본 데이터베이스의 저장 프로시저에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="445cd-724">수정 함수가 매핑되는 저장 프로시저는 스토리지 모델에서 선언되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="445cd-725">자세한 내용은 Function 요소 (SSDL)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="445cd-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="445cd-726">엔터티 형식의 삽입, 업데이트 또는 삭제 작업을 모두 저장 프로시저에 매핑하지 않은 경우 런타임에 실행 하면 매핑되지 않은 작업이 실패 하 고 UpdateException이 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="445cd-727">**Updatefunction** 요소는 ModificationFunctionMapping 요소의 자식일 수 있으며 entitytypemapping 요소에 적용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="445cd-728">**Updatefunction** 요소에는 다음과 같은 자식 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="445cd-729">AssociationEnd (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="445cd-730">ComplexProperty(0개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="445cd-731">ResultBinding (0 개 또는 1 개)</span><span class="sxs-lookup"><span data-stu-id="445cd-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="445cd-732">ScarlarProperty (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="445cd-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="445cd-733">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="445cd-733">Applicable Attributes</span></span>

<span data-ttu-id="445cd-734">다음 표에서는 **Updatefunction** 요소에 적용할 수 있는 특성을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="445cd-735">특성 이름</span><span class="sxs-lookup"><span data-stu-id="445cd-735">Attribute Name</span></span>            | <span data-ttu-id="445cd-736">필수 여부</span><span class="sxs-lookup"><span data-stu-id="445cd-736">Is Required</span></span> | <span data-ttu-id="445cd-737">값</span><span class="sxs-lookup"><span data-stu-id="445cd-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="445cd-738">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="445cd-738">**FunctionName**</span></span>          | <span data-ttu-id="445cd-739">예</span><span class="sxs-lookup"><span data-stu-id="445cd-739">Yes</span></span>         | <span data-ttu-id="445cd-740">업데이트 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="445cd-741">저장 프로시저는 스토리지 모델에서 선언해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="445cd-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="445cd-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="445cd-743">아니요</span><span class="sxs-lookup"><span data-stu-id="445cd-743">No</span></span>          | <span data-ttu-id="445cd-744">영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="445cd-745">예제</span><span class="sxs-lookup"><span data-stu-id="445cd-745">Example</span></span>

<span data-ttu-id="445cd-746">다음 예는 School 모델을 기반으로 하며 **Person** 엔터티 형식의 업데이트 함수를 **updatefunction** 저장 프로시저에 매핑하는 데 사용 되는 **updatefunction** 요소를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="445cd-747">**Updateperson** 저장 프로시저는 저장소 모델에서 선언 됩니다.</span><span class="sxs-lookup"><span data-stu-id="445cd-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

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
