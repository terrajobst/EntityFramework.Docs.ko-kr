---
title: EF6-MSL 사양
author: divega
ms.date: 2016-10-23
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 77dc7072c70b104188cd23974f32308960daebb6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996033"
---
# <a name="msl-specification"></a><span data-ttu-id="53964-102">XML 사양</span><span class="sxs-lookup"><span data-stu-id="53964-102">MSL Specification</span></span>
<span data-ttu-id="53964-103">매핑 사양 언어 (MSL)은 개념적 모델 및 Entity Framework 응용 프로그램의 저장소 모델 간의 매핑을 설명 하는 XML 기반 언어입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="53964-104">Entity Framework 응용 프로그램에서는 빌드 시 매핑 메타 데이터 (MSL로 작성).msl 파일에서 로드 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="53964-105">Entity Framework 런타임에 매핑 메타 데이터를 사용 하 여 저장소 관련 명령 개념적 모델에 대 한 쿼리를 변환 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="53964-106">Entity Framework Designer (EF 디자이너)는 디자인 타임에.edmx 파일에 매핑 정보를 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="53964-107">빌드 시 Entity Designer 정보 사용 하 여.edmx 파일에는 필요한 Entity Framework에서 런타임에.msl 파일을 만들려면</span><span class="sxs-lookup"><span data-stu-id="53964-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="53964-108">MSL에서 참조하는 모든 개념적 모델 형식 또는 저장소 모델 형식의 이름은 해당 네임스페이스 이름으로 한정되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="53964-109">개념적 모델 네임 스페이스 이름에 대 한 정보를 참조 하세요 [CSDL 사양](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="53964-110">저장소 모델 네임 스페이스 이름에 대 한 정보를 참조 하세요 [SSDL 사양](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="53964-111">MSL의 버전은 XML 네임 스페이스로 식별 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="53964-112">MSL 버전</span><span class="sxs-lookup"><span data-stu-id="53964-112">MSL Version</span></span> | <span data-ttu-id="53964-113">XML Namespace</span><span class="sxs-lookup"><span data-stu-id="53964-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="53964-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="53964-114">MSL v1</span></span>      | <span data-ttu-id="53964-115">urn: 스키마-microsoft-com:windows:storage:mapping:CS</span><span class="sxs-lookup"><span data-stu-id="53964-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="53964-116">MSL v2</span><span class="sxs-lookup"><span data-stu-id="53964-116">MSL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| <span data-ttu-id="53964-117">MSL v3</span><span class="sxs-lookup"><span data-stu-id="53964-117">MSL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="53964-118">Alias 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-118">Alias Element (MSL)</span></span>

<span data-ttu-id="53964-119">합니다 **별칭** MSL (매핑 사양 언어)에서 요소는 개념적 모델과 저장소 모델 네임 스페이스에 대 한 별칭을 정의 하는 데 사용 되는 매핑 요소의 자식입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="53964-120">MSL에서 참조하는 모든 개념적 모델 형식 또는 저장소 모델 형식의 이름은 해당 네임스페이스 이름으로 한정되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="53964-121">개념적 모델 네임 스페이스 이름에 대 한 내용은 스키마 요소 (CSDL)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="53964-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="53964-122">저장소 모델 네임 스페이스 이름에 대 한 내용은 스키마 요소 (SSDL)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="53964-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="53964-123">합니다 **별칭** 요소는 자식 요소를 포함할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="53964-124">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-124">Applicable Attributes</span></span>

<span data-ttu-id="53964-125">아래 표에서 설명에 적용할 수 있는 특성을 **별칭** 요소.</span><span class="sxs-lookup"><span data-stu-id="53964-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="53964-126">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-126">Attribute Name</span></span> | <span data-ttu-id="53964-127">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-127">Is Required</span></span> | <span data-ttu-id="53964-128">값</span><span class="sxs-lookup"><span data-stu-id="53964-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="53964-129">**키**</span><span class="sxs-lookup"><span data-stu-id="53964-129">**Key**</span></span>        | <span data-ttu-id="53964-130">예</span><span class="sxs-lookup"><span data-stu-id="53964-130">Yes</span></span>         | <span data-ttu-id="53964-131">지정 된 네임 스페이스에 대 한 별칭을 **값** 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="53964-132">**값**</span><span class="sxs-lookup"><span data-stu-id="53964-132">**Value**</span></span>      | <span data-ttu-id="53964-133">예</span><span class="sxs-lookup"><span data-stu-id="53964-133">Yes</span></span>         | <span data-ttu-id="53964-134">네임 스페이스의 값을 **키** 요소는 별칭입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="53964-135">예제</span><span class="sxs-lookup"><span data-stu-id="53964-135">Example</span></span>

<span data-ttu-id="53964-136">다음 예제는 **별칭** 별칭을 정의 하는 요소 `c`, 개념적 모델에 정의 된 형식에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

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

## <a name="associationend-element-msl"></a><span data-ttu-id="53964-137">AssociationEnd 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="53964-138">합니다 **AssociationEnd** MSL (매핑 사양 언어)에서 요소는 개념적 모델의 엔터티 형식 수정 함수가 기본 데이터베이스의 저장된 프로시저에 매핑된 경우 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="53964-139">값은 연결 속성에 저장 된 매개 변수를 사용 하는 저장된 프로시저를 수정 하는 경우는 **AssociationEnd** 요소 매개 변수 속성 값을 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="53964-140">자세한 내용은 아래 예제를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="53964-140">For more information, see the example below.</span></span>

<span data-ttu-id="53964-141">저장된 프로시저에 엔터티 형식의 수정 함수 매핑 하는 방법에 대 한 자세한 내용은 참조 ModificationFunctionMapping 요소 (MSL) 및 연습: 저장 프로시저에 엔터티 매핑.</span><span class="sxs-lookup"><span data-stu-id="53964-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="53964-142">합니다 **AssociationEnd** 요소는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="53964-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="53964-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="53964-144">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-144">Applicable Attributes</span></span>

<span data-ttu-id="53964-145">다음 표에서 해당 하는 특성을 설명 합니다 **AssociationEnd** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="53964-146">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-146">Attribute Name</span></span>     | <span data-ttu-id="53964-147">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-147">Is Required</span></span> | <span data-ttu-id="53964-148">값</span><span class="sxs-lookup"><span data-stu-id="53964-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="53964-149">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="53964-149">**AssociationSet**</span></span> | <span data-ttu-id="53964-150">예</span><span class="sxs-lookup"><span data-stu-id="53964-150">Yes</span></span>         | <span data-ttu-id="53964-151">매핑되는 연결의 이름</span><span class="sxs-lookup"><span data-stu-id="53964-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="53964-152">**From**</span><span class="sxs-lookup"><span data-stu-id="53964-152">**From**</span></span>           | <span data-ttu-id="53964-153">예</span><span class="sxs-lookup"><span data-stu-id="53964-153">Yes</span></span>         | <span data-ttu-id="53964-154">값을 **FromRole** 매핑되는 연결에 해당 하는 탐색 속성의 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="53964-155">자세한 내용은 NavigationProperty 요소 (CSDL)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="53964-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="53964-156">**대상**</span><span class="sxs-lookup"><span data-stu-id="53964-156">**To**</span></span>             | <span data-ttu-id="53964-157">예</span><span class="sxs-lookup"><span data-stu-id="53964-157">Yes</span></span>         | <span data-ttu-id="53964-158">값을 **ToRole** 매핑되는 연결에 해당 하는 탐색 속성의 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="53964-159">자세한 내용은 NavigationProperty 요소 (CSDL)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="53964-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="53964-160">예제</span><span class="sxs-lookup"><span data-stu-id="53964-160">Example</span></span>

<span data-ttu-id="53964-161">다음과 같은 개념적 모델 엔터티 형식이 있다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-161">Consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="53964-162">또한 다음과 같은 저장 프로시저가 있다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-162">Also consider the following stored procedure:</span></span>

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

<span data-ttu-id="53964-163">업데이트 함수 이름을 매핑하려면 합니다 `Course` 이 저장된 프로시저에 엔터티, 값을 제공 해야 합니다는 **DepartmentID** 매개 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="53964-164">`DepartmentID`의 값은 엔터티 형식의 속성에 해당하지 않으며, 여기에 표시된 매핑을 사용하는 독립 연결에 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

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

<span data-ttu-id="53964-165">다음 코드에서는 합니다 **AssociationEnd** 매핑하는 데 사용 되는 요소는 **DepartmentID** 속성을 **FK\_과정\_부서** 연결 합니다 **UpdateCourse** 저장 프로시저 (할 업데이트 함수를 **과정** 엔터티 형식이 매핑되는):</span><span class="sxs-lookup"><span data-stu-id="53964-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

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

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="53964-166">AssociationSetMapping 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="53964-167">합니다 **AssociationSetMapping** MSL (매핑 사양 언어)에서 요소는 기본 데이터베이스에서 개념적 모델 및 테이블 열에 있는 연결 간의 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="53964-168">개념적 모델의 연결은 기본 데이터베이스의 기본 및 외래 키 열을 나타내는 속성이 있는 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="53964-169">합니다 **AssociationSetMapping** 요소 두 EndProperty 요소를 사용 하 여 데이터베이스의 연결 형식 속성과 열 간의 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="53964-170">Condition 요소를 사용 하 여 이러한 매핑에 조건을 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="53964-171">ModificationFunctionMapping 요소를 사용 하 여 데이터베이스에서 저장된 프로시저에 삽입, 업데이트 및 연결에 대 한 삭제 함수를 매핑하십시오.</span><span class="sxs-lookup"><span data-stu-id="53964-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="53964-172">QueryView 요소에서 Entity SQL 문자열을 사용 하 여 연결과 테이블 열 간의 읽기 전용 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="53964-173">연결을 사용 하 여 매핑할 필요가 없습니다 개념적 모델의 연결에 대 한 참조 제약 조건이 정의 된 경우는 **AssociationSetMapping** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="53964-174">경우는 **AssociationSetMapping** 요소가 있는 참조 제약 조건이 있는 연결의 매핑을 정의 합니다 **AssociationSetMapping** 요소는 무시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="53964-175">자세한 내용은 ReferentialConstraint 요소 (CSDL)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="53964-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="53964-176">합니다 **AssociationSetMapping** 요소는 다음 자식 요소를 포함할 수 있습니다</span><span class="sxs-lookup"><span data-stu-id="53964-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="53964-177">QueryView (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="53964-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="53964-178">EndProperty (0 또는 2)</span><span class="sxs-lookup"><span data-stu-id="53964-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="53964-179">조건 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="53964-180">ModificationFunctionMapping (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="53964-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="53964-181">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-181">Applicable Attributes</span></span>

<span data-ttu-id="53964-182">다음 표에서 설명에 적용할 수 있는 특성을 **AssociationSetMapping** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="53964-183">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-183">Attribute Name</span></span>     | <span data-ttu-id="53964-184">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-184">Is Required</span></span> | <span data-ttu-id="53964-185">값</span><span class="sxs-lookup"><span data-stu-id="53964-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="53964-186">**이름**</span><span class="sxs-lookup"><span data-stu-id="53964-186">**Name**</span></span>           | <span data-ttu-id="53964-187">예</span><span class="sxs-lookup"><span data-stu-id="53964-187">Yes</span></span>         | <span data-ttu-id="53964-188">매핑되는 개념적 모델 연결 집합의 이름</span><span class="sxs-lookup"><span data-stu-id="53964-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="53964-189">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="53964-189">**TypeName**</span></span>       | <span data-ttu-id="53964-190">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-190">No</span></span>          | <span data-ttu-id="53964-191">매핑되는 개념적 모델 연결 형식의 네임스페이스로 한정된 이름</span><span class="sxs-lookup"><span data-stu-id="53964-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="53964-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="53964-192">**StoreEntitySet**</span></span> | <span data-ttu-id="53964-193">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-193">No</span></span>          | <span data-ttu-id="53964-194">매핑되는 테이블의 이름</span><span class="sxs-lookup"><span data-stu-id="53964-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="53964-195">예제</span><span class="sxs-lookup"><span data-stu-id="53964-195">Example</span></span>

<span data-ttu-id="53964-196">다음 예제는 **AssociationSetMapping** 요소를 **FK\_과정\_부서** 개념적 모델의 연결 집합은 매핑되 **과정** 데이터베이스의 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="53964-197">자식에서 연결 형식 속성과 테이블 열 간의 매핑을 지정 된 **EndProperty** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

## <a name="complexproperty-element-msl"></a><span data-ttu-id="53964-198">ComplexProperty 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="53964-199">A **ComplexProperty** MSL (매핑 사양 언어)의 요소에는 기본 데이터베이스에서 개념적 모델 엔터티 형식과 테이블 열에 복합 형식 속성 간의 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="53964-200">속성-열 매핑은 자식 ScalarProperty 요소에 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="53964-201">합니다 **ComplexType** 속성 요소에는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="53964-202">ScalarProperty (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="53964-203">**ComplexProperty** (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="53964-204">ComplextTypeMapping (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="53964-205">조건 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="53964-206">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-206">Applicable Attributes</span></span>

<span data-ttu-id="53964-207">다음 표에서 해당 하는 특성을 설명 합니다 **ComplexProperty** 요소:</span><span class="sxs-lookup"><span data-stu-id="53964-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="53964-208">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-208">Attribute Name</span></span> | <span data-ttu-id="53964-209">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-209">Is Required</span></span> | <span data-ttu-id="53964-210">값</span><span class="sxs-lookup"><span data-stu-id="53964-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="53964-211">**이름**</span><span class="sxs-lookup"><span data-stu-id="53964-211">**Name**</span></span>       | <span data-ttu-id="53964-212">예</span><span class="sxs-lookup"><span data-stu-id="53964-212">Yes</span></span>         | <span data-ttu-id="53964-213">개념적 모델에서 매핑되는 엔터티 형식의 복합 속성 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="53964-214">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="53964-214">**TypeName**</span></span>   | <span data-ttu-id="53964-215">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-215">No</span></span>          | <span data-ttu-id="53964-216">개념적 모델 속성 형식의 네임스페이스로 한정된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="53964-217">예제</span><span class="sxs-lookup"><span data-stu-id="53964-217">Example</span></span>

<span data-ttu-id="53964-218">다음 예제에서는 School 모델을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-218">The following example is based on the School model.</span></span> <span data-ttu-id="53964-219">다음 복합 형식이 개념적 모델에 추가되었습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-219">The following complex type has been added to the conceptual model:</span></span>

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

<span data-ttu-id="53964-220">**LastName** 및 **FirstName** 의 속성을 **Person** 하나의 복합 속성을 사용 하 여 엔터티 형식의 이름이 바뀌었습니다. **이름**:</span><span class="sxs-lookup"><span data-stu-id="53964-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

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

<span data-ttu-id="53964-221">에서는 다음 MSL 합니다 **ComplexProperty** 매핑하는 데 사용 되는 요소는 **이름** 기본 데이터베이스의 열에 속성:</span><span class="sxs-lookup"><span data-stu-id="53964-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

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

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="53964-222">ComplexTypeMapping 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="53964-223">합니다 **ComplexTypeMapping** MSL (매핑 사양 언어)에서 요소 ResultMapping 요소의 자식인 기본 개념적 모델의 function import와 저장된 프로시저 간의 매핑을 정의 하 고 데이터베이스는 다음과 같은 경우:</span><span class="sxs-lookup"><span data-stu-id="53964-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="53964-224">Function Import가 개념적 복합 형식을 반환하는 경우</span><span class="sxs-lookup"><span data-stu-id="53964-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="53964-225">저장 프로시저에서 반환되는 열 이름이 복합 형식의 속성 이름과 정확히 일치하지 않는 경우</span><span class="sxs-lookup"><span data-stu-id="53964-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="53964-226">기본적으로 저장 프로시저에서 반환되는 열과 복합 형식 간의 매핑은 열 및 속성 이름을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="53964-227">열 이름이 정확히 일치 하지 않는 속성 이름, 경우에 사용 해야 합니다 **ComplexTypeMapping** 매핑을 정의 하는 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="53964-228">기본 매핑의 예 FunctionImportMapping 요소 (MSL)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="53964-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="53964-229">합니다 **ComplexTypeMapping** 요소는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="53964-230">ScalarProperty (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="53964-231">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-231">Applicable Attributes</span></span>

<span data-ttu-id="53964-232">다음 표에서 해당 하는 특성을 설명 합니다 **ComplexTypeMapping** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="53964-233">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-233">Attribute Name</span></span> | <span data-ttu-id="53964-234">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-234">Is Required</span></span> | <span data-ttu-id="53964-235">값</span><span class="sxs-lookup"><span data-stu-id="53964-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="53964-236">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="53964-236">**TypeName**</span></span>   | <span data-ttu-id="53964-237">예</span><span class="sxs-lookup"><span data-stu-id="53964-237">Yes</span></span>         | <span data-ttu-id="53964-238">매핑되는 복합 형식의 네임스페이스로 한정된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="53964-239">예제</span><span class="sxs-lookup"><span data-stu-id="53964-239">Example</span></span>

<span data-ttu-id="53964-240">다음과 같은 저장 프로시저가 있다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-240">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="53964-241">또한 다음과 같은 개념적 모델 복합 형식이 있다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="53964-242">이전 복합 형식의 인스턴스를 반환 하는 function import를 만들기 위해 열 간의 매핑이 저장된 프로시저에서 반환 하 고 엔터티 형식에 정의 되어야 합니다는 **ComplexTypeMapping** 요소:</span><span class="sxs-lookup"><span data-stu-id="53964-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

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

## <a name="condition-element-msl"></a><span data-ttu-id="53964-243">Condition 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-243">Condition Element (MSL)</span></span>

<span data-ttu-id="53964-244">합니다 **조건을** MSL (매핑 사양 언어)에서 요소 개념적 모델과 기본 데이터베이스 간의 매핑에 대 한 조건을 적용 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="53964-245">XML 노드 내에 정의 된 매핑을 사용 하는 모든 유효한 조건에 지정 된 자식 **조건을** 요소를 충족 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="53964-246">그렇지 않으면 매핑이 유효하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="53964-247">예를 들어 MappingFragment 요소를 하나 이상 포함 **조건을** 내에서 정의 된 매핑은 자식 요소를 **MappingFragment** 노드 모든 경우에 적용 됩니다 자식조건 **조건** 요소 충족 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="53964-248">각 조건 중 하나에 적용할 수는 **이름** (지정 된 개념적 모델 엔터티 속성의 이름을 합니다 **이름** 특성), 또는 **ColumnName** (이름에 있는 열의 지정한 데이터베이스를 **ColumnName** 특성).</span><span class="sxs-lookup"><span data-stu-id="53964-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="53964-249">경우는 **이름을** 특성이 설정 되어, 엔터티 속성 값에 대해 조건이 확인 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="53964-250">경우는 **ColumnName** 특성을 설정 하면 열 값에 대해 조건이 확인 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="53964-251">중 하나만 합니다 **이름** 또는 **ColumnName** 특성을 지정할 수 있습니다를 **조건** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="53964-252">때를 **조건** 만 FunctionImportMapping 요소 내에서 요소를 사용 합니다 **이름** 특성 적용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="53964-253">합니다 **조건을** 요소에는 다음 요소의 자식일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="53964-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="53964-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="53964-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="53964-255">ComplexProperty</span></span>
-   <span data-ttu-id="53964-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="53964-256">EntitySetMapping</span></span>
-   <span data-ttu-id="53964-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="53964-257">MappingFragment</span></span>
-   <span data-ttu-id="53964-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="53964-258">EntityTypeMapping</span></span>

<span data-ttu-id="53964-259">합니다 **조건을** 요소는 자식 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="53964-260">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-260">Applicable Attributes</span></span>

<span data-ttu-id="53964-261">다음 표에서 해당 하는 특성을 설명 합니다 **조건을** 요소:</span><span class="sxs-lookup"><span data-stu-id="53964-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="53964-262">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-262">Attribute Name</span></span> | <span data-ttu-id="53964-263">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-263">Is Required</span></span> | <span data-ttu-id="53964-264">값</span><span class="sxs-lookup"><span data-stu-id="53964-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="53964-265">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="53964-265">**ColumnName**</span></span> | <span data-ttu-id="53964-266">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-266">No</span></span>          | <span data-ttu-id="53964-267">조건을 확인하는 데 사용되는 값이 있는 테이블 열의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="53964-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="53964-268">**IsNull**</span></span>     | <span data-ttu-id="53964-269">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-269">No</span></span>          | <span data-ttu-id="53964-270">**True 이면** 나 **False**합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-270">**True** or **False**.</span></span> <span data-ttu-id="53964-271">값이 **True** 이 고 열 값이 **null**, 또는 값이 **False** 고 열 값이 없는 **null**, 조건이 true 인 .</span><span class="sxs-lookup"><span data-stu-id="53964-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="53964-272">그렇지 않으면 조건이 false입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="53964-273">합니다 **IsNull** 하 고 **값** 동시에 특성을 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="53964-274">**값**</span><span class="sxs-lookup"><span data-stu-id="53964-274">**Value**</span></span>      | <span data-ttu-id="53964-275">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-275">No</span></span>          | <span data-ttu-id="53964-276">열 값과 비교할 값입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-276">The value with which the column value is compared.</span></span> <span data-ttu-id="53964-277">값이 동일하면 조건이 true입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="53964-278">그렇지 않으면 조건이 false입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="53964-279">합니다 **IsNull** 하 고 **값** 동시에 특성을 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="53964-280">**이름**</span><span class="sxs-lookup"><span data-stu-id="53964-280">**Name**</span></span>       | <span data-ttu-id="53964-281">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-281">No</span></span>          | <span data-ttu-id="53964-282">조건을 확인하는 데 사용되는 값이 있는 개념적 모델 엔터티 속성의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="53964-283">이 특성을 적용할 수 없는 경우는 **조건을** FunctionImportMapping 요소 내의 요소를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="53964-284">예제</span><span class="sxs-lookup"><span data-stu-id="53964-284">Example</span></span>

<span data-ttu-id="53964-285">다음 예와 **조건을** 요소의 자식인 **MappingFragment** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="53964-286">때 **HireDate** isnotnull 및 **EnrollmentDate** 는 null 간에 데이터가 매핑되는 합니다 **SchoolModel.Instructor** 형식 및 **PersonID**하 고 **HireDate** 열의 합니다 **Person** 테이블.</span><span class="sxs-lookup"><span data-stu-id="53964-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="53964-287">때 **EnrollmentDate** isnotnull 및 **HireDate** 는 null 간에 데이터가 매핑되는 합니다 **SchoolModel.Student** 형식 및 **PersonID** 및 **등록** 의 열을 **Person** 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

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

## <a name="deletefunction-element-msl"></a><span data-ttu-id="53964-288">DeleteFunction 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="53964-289">합니다 **DeleteFunction** MSL (매핑 사양 언어)에서 요소는 기본 데이터베이스의 저장된 프로시저에 엔터티 형식 또는 연결 개념적 모델에 대 한 삭제 함수를 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="53964-290">수정 함수가 매핑되는 저장 프로시저는 저장소 모델에서 선언되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="53964-291">자세한 내용은 함수 요소 (SSDL)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="53964-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="53964-292">매핑되지 않는 경우 모든 세 개의 삽입, 업데이트 또는 삭제 저장된 프로시저에 엔터티 형식의 작업, 경우 런타임에 실행 하면 매핑되지 않은 작업이 실패 합니다 및는 UpdateException throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="53964-293">EntityTypeMapping에 적용되는 DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="53964-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="53964-294">EntityTypeMapping 요소에 적용할 경우 합니다 **DeleteFunction** 요소 저장된 프로시저를 개념적 모델의 엔터티 형식에 대 한 삭제 함수를 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="53964-295">합니다 **DeleteFunction** 요소에 적용 하는 경우 다음 자식 요소를 포함할 수 있습니다는 **EntityTypeMapping** 요소:</span><span class="sxs-lookup"><span data-stu-id="53964-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="53964-296">AssociationEnd (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="53964-297">ComplexProperty (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="53964-298">ScarlarProperty (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="53964-299">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-299">Applicable Attributes</span></span>

<span data-ttu-id="53964-300">다음 표에서 특성을 적용할 수 있습니다는 **DeleteFunction** 요소에 적용 되는 경우는 **EntityTypeMapping** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="53964-301">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-301">Attribute Name</span></span>            | <span data-ttu-id="53964-302">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-302">Is Required</span></span> | <span data-ttu-id="53964-303">값</span><span class="sxs-lookup"><span data-stu-id="53964-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="53964-304">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="53964-304">**FunctionName**</span></span>          | <span data-ttu-id="53964-305">예</span><span class="sxs-lookup"><span data-stu-id="53964-305">Yes</span></span>         | <span data-ttu-id="53964-306">삭제 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="53964-307">저장 프로시저는 저장소 모델에서 선언해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="53964-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="53964-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="53964-309">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-309">No</span></span>          | <span data-ttu-id="53964-310">영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="53964-311">예제</span><span class="sxs-lookup"><span data-stu-id="53964-311">Example</span></span>

<span data-ttu-id="53964-312">다음 예제에서는 School 모델을 기반으로 하며 표시를 **DeleteFunction** 대 한 삭제 함수 매핑 요소를 **Person** 엔터티 형식을 **DeletePerson** 저장된 프로시저입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="53964-313">합니다 **DeletePerson** 저장된 프로시저는 저장소 모델에서 선언 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

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

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="53964-314">AssociationSetMapping에 적용된 DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="53964-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="53964-315">AssociationSetMapping 요소에 적용할 경우 합니다 **DeleteFunction** 요소 저장된 프로시저를 개념적 모델의 연결에 대 한 삭제 함수를 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="53964-316">합니다 **DeleteFunction** 요소에 적용 하는 경우 다음 자식 요소를 포함할 수 있습니다 합니다 **AssociationSetMapping** 요소:</span><span class="sxs-lookup"><span data-stu-id="53964-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="53964-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="53964-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="53964-318">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-318">Applicable Attributes</span></span>

<span data-ttu-id="53964-319">다음 표에서 특성을 적용할 수 있습니다는 **DeleteFunction** 요소에 적용 되는 경우를 **AssociationSetMapping** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="53964-320">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-320">Attribute Name</span></span>            | <span data-ttu-id="53964-321">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-321">Is Required</span></span> | <span data-ttu-id="53964-322">값</span><span class="sxs-lookup"><span data-stu-id="53964-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="53964-323">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="53964-323">**FunctionName**</span></span>          | <span data-ttu-id="53964-324">예</span><span class="sxs-lookup"><span data-stu-id="53964-324">Yes</span></span>         | <span data-ttu-id="53964-325">삭제 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="53964-326">저장 프로시저는 저장소 모델에서 선언해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="53964-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="53964-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="53964-328">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-328">No</span></span>          | <span data-ttu-id="53964-329">영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="53964-330">예제</span><span class="sxs-lookup"><span data-stu-id="53964-330">Example</span></span>

<span data-ttu-id="53964-331">다음 예제에서는 School 모델을 기반으로 하며 표시를 **DeleteFunction** 대 한 삭제 함수를 매핑하는 데 사용 되는 요소는 **CourseInstructor** 연결을  **DeleteCourseInstructor** 저장 프로시저입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="53964-332">합니다 **DeleteCourseInstructor** 저장된 프로시저는 저장소 모델에서 선언 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="endproperty-element-msl"></a><span data-ttu-id="53964-333">EndProperty 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="53964-334">합니다 **EndProperty** MSL (매핑 사양 언어)에서 요소 끝 또는 수정 함수를 개념적 모델 연결 및 기본 데이터베이스 간의 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="53964-335">속성-열 매핑은 자식 ScalarProperty 요소에 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="53964-336">경우는 **EndProperty** AssociationSetMapping 요소 자식, 요소는 개념적 모델 연결의 끝에 대 한 매핑을 정의 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="53964-337">경우는 **EndProperty** 요소는 개념적 모델 연결의 수정 함수에 대 한 매핑을 정의 하는, 자식인 InsertFunction 요소 또는 DeleteFunction 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="53964-338">합니다 **EndProperty** 요소는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="53964-339">ScalarProperty (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="53964-340">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-340">Applicable Attributes</span></span>

<span data-ttu-id="53964-341">다음 표에서 해당 하는 특성을 설명 합니다 **EndProperty** 요소:</span><span class="sxs-lookup"><span data-stu-id="53964-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="53964-342">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-342">Attribute Name</span></span> | <span data-ttu-id="53964-343">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-343">Is Required</span></span> | <span data-ttu-id="53964-344">값</span><span class="sxs-lookup"><span data-stu-id="53964-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="53964-345">name</span><span class="sxs-lookup"><span data-stu-id="53964-345">Name</span></span>           | <span data-ttu-id="53964-346">예</span><span class="sxs-lookup"><span data-stu-id="53964-346">Yes</span></span>         | <span data-ttu-id="53964-347">매핑되는 연결 끝의 이름</span><span class="sxs-lookup"><span data-stu-id="53964-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="53964-348">예제</span><span class="sxs-lookup"><span data-stu-id="53964-348">Example</span></span>

<span data-ttu-id="53964-349">에서는 다음 예제는 **AssociationSetMapping** 요소를 **FK\_과정\_부서** 합니다 매핑되는개념적모델의연결**과정** 데이터베이스의 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="53964-350">자식에서 연결 형식 속성과 테이블 열 간의 매핑을 지정 된 **EndProperty** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

### <a name="example"></a><span data-ttu-id="53964-351">예제</span><span class="sxs-lookup"><span data-stu-id="53964-351">Example</span></span>

<span data-ttu-id="53964-352">다음 예제에서는 합니다 **EndProperty** 요소는 연결의 삽입 및 삭제 함수 매핑 (**CourseInstructor**) 기본 데이터베이스의 저장된 프로시저에 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="53964-353">매핑되는 함수는 저장소 모델에서 선언됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-353">The functions that are mapped to are declared in the storage model.</span></span>

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

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="53964-354">EntityContainerMapping 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="53964-355">합니다 **EntityContainerMapping** MSL (매핑 사양 언어)에서 요소는 저장소 모델의 엔터티 컨테이너에 개념적 모델의 엔터티 컨테이너를 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="53964-356">합니다 **EntityContainerMapping** 매핑 요소의 자식 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="53964-357">합니다 **EntityContainerMapping** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="53964-358">EntitySetMapping (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="53964-359">AssociationSetMapping (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="53964-360">FunctionImportMapping (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="53964-361">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-361">Applicable Attributes</span></span>

<span data-ttu-id="53964-362">다음 표에서 설명에 적용할 수 있는 특성을 **EntityContainerMapping** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="53964-363">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-363">Attribute Name</span></span>            | <span data-ttu-id="53964-364">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-364">Is Required</span></span> | <span data-ttu-id="53964-365">값</span><span class="sxs-lookup"><span data-stu-id="53964-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="53964-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="53964-366">**StorageModelContainer**</span></span> | <span data-ttu-id="53964-367">예</span><span class="sxs-lookup"><span data-stu-id="53964-367">Yes</span></span>         | <span data-ttu-id="53964-368">매핑되는 저장소 모델 엔터티 컨테이너의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="53964-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="53964-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="53964-370">예</span><span class="sxs-lookup"><span data-stu-id="53964-370">Yes</span></span>         | <span data-ttu-id="53964-371">매핑되는 개념적 모델 엔터티 컨테이너의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="53964-372">**GenerateUpdateViews**</span><span class="sxs-lookup"><span data-stu-id="53964-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="53964-373">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-373">No</span></span>          | <span data-ttu-id="53964-374">**True 이면** 나 **False**합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-374">**True** or **False**.</span></span> <span data-ttu-id="53964-375">하는 경우 **False**, 업데이트 보기가 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="53964-376">이 특성에 설정할 **False** 는 유효 하지 않게 데이터가 올바르게 라운드트립되지 않기 때문에 읽기 전용 매핑이 있는 경우.</span><span class="sxs-lookup"><span data-stu-id="53964-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="53964-377">기본값은 **True**합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="53964-378">예제</span><span class="sxs-lookup"><span data-stu-id="53964-378">Example</span></span>

<span data-ttu-id="53964-379">에서는 다음 예제는 **EntityContainerMapping** 매핑되는 요소는 **SchoolModelEntities** 컨테이너 (개념적 모델 엔터티 컨테이너)를  **SchoolModelStoreContainer** 컨테이너 (저장소 모델 엔터티 컨테이너):</span><span class="sxs-lookup"><span data-stu-id="53964-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

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

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="53964-380">EntitySetMapping 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="53964-381">합니다 **EntitySetMapping** 저장소 모델의 매핑 사양 언어 (MSL) 맵 엔터티를 개념적 모델의 모든 형식 집합에 요소를 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="53964-382">개념적 모델의 엔터티 집합에 대 한 논리적 컨테이너는 동일한 형식 (및 파생된 형식)의 엔터티의 인스턴스.</span><span class="sxs-lookup"><span data-stu-id="53964-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="53964-383">저장소 모델의 엔터티 집합을 테이블 또는 기본 데이터베이스의 뷰를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="53964-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="53964-384">개념적 모델 엔터티 집합의 값으로 지정 된 합니다 **이름** 특성을 **EntitySetMapping** 요소.</span><span class="sxs-lookup"><span data-stu-id="53964-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="53964-385">매핑 대상 테이블 또는 보기에서 지정 된 합니다 **StoreEntitySet** 각 자식 MappingFragment 요소 또는 특성을 **EntitySetMapping** 요소 자체.</span><span class="sxs-lookup"><span data-stu-id="53964-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="53964-386">합니다 **EntitySetMapping** 요소는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="53964-387">EntityTypeMapping (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="53964-388">QueryView (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="53964-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="53964-389">MappingFragment (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="53964-390">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-390">Applicable Attributes</span></span>

<span data-ttu-id="53964-391">다음 표에서 설명에 적용할 수 있는 특성을 **EntitySetMapping** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="53964-392">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-392">Attribute Name</span></span>           | <span data-ttu-id="53964-393">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-393">Is Required</span></span> | <span data-ttu-id="53964-394">값</span><span class="sxs-lookup"><span data-stu-id="53964-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="53964-395">**이름**</span><span class="sxs-lookup"><span data-stu-id="53964-395">**Name**</span></span>                 | <span data-ttu-id="53964-396">예</span><span class="sxs-lookup"><span data-stu-id="53964-396">Yes</span></span>         | <span data-ttu-id="53964-397">매핑되는 개념적 모델 엔터티 집합의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="53964-398">**TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="53964-398">**TypeName** **1**</span></span>       | <span data-ttu-id="53964-399">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-399">No</span></span>          | <span data-ttu-id="53964-400">매핑되는 개념적 모델 엔터티 형식의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="53964-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="53964-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="53964-402">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-402">No</span></span>          | <span data-ttu-id="53964-403">매핑되는 대상 저장소 모델 엔터티 집합의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="53964-404">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="53964-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="53964-405">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-405">No</span></span>          | <span data-ttu-id="53964-406">**True 이면** 나 **False** 고유한 행만 반환 되는지 여부에 따라 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="53964-407">이 특성이로 설정 된 경우 **True**서 **GenerateUpdateViews** EntityContainerMapping 요소의 특성으로 설정 되어 있어야 **False**.</span><span class="sxs-lookup"><span data-stu-id="53964-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="53964-408">**1** 는 **TypeName** 하 고 **StoreEntitySet** 특성 단일 엔터티 형식을 단일 테이블에 매핑할 EntityTypeMapping 및 MappingFragment 자식 요소를 대신 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="53964-409">예제</span><span class="sxs-lookup"><span data-stu-id="53964-409">Example</span></span>

<span data-ttu-id="53964-410">다음 예제와 **EntitySetMapping** 의 세 가지 형식 (기본 형식 및 두 개의 파생된 형식)를 매핑하는 요소는 **과정** 개념적 모델에서 서로 다른 세 테이블의 엔터티 집합을 기본 데이터베이스입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="53964-411">테이블에서 지정 된 된 **StoreEntitySet** 각각의 특성 **MappingFragment** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

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

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="53964-412">EntityTypeMapping 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="53964-413">합니다 **EntityTypeMapping** MSL (매핑 사양 언어)에서 요소 내부 데이터베이스에서 개념적 모델 및 테이블의 엔터티 형식 또는 뷰 간의 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="53964-414">개념적 모델 엔터티 형식과 기본 데이터베이스 테이블 또는 뷰에 대 한 자세한 내용은 EntityType 요소 (CSDL) 및 EntitySet 요소 (SSDL)를 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="53964-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="53964-415">매핑되는 개념적 모델 엔터티 형식을 지정 하 여는 **TypeName** 특성을 **EntityTypeMapping** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="53964-416">매핑되는 뷰나 테이블에서 지정 합니다 **StoreEntitySet** 자식 MappingFragment 요소의 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="53964-417">자식 요소를 사용 하 여 삽입, 매핑할 수 있습니다 ModificationFunctionMapping 업데이트 또는 삭제 함수를 데이터베이스에서 저장된 프로시저에 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="53964-418">합니다 **EntityTypeMapping** 요소는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="53964-419">MappingFragment (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="53964-420">ModificationFunctionMapping (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="53964-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="53964-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="53964-421">ScalarProperty</span></span>
-   <span data-ttu-id="53964-422">조건</span><span class="sxs-lookup"><span data-stu-id="53964-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="53964-423">**MappingFragment** 하 고 **ModificationFunctionMapping** 요소는 자식 요소의 수 없습니다는 **EntityTypeMapping** 동시 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="53964-424">**ScalarProperty** 및 **조건** 요소는 자식 요소의 수만 합니다 **EntityTypeMapping** 요소 FunctionImportMapping 요소 내에서 사용 하는 경우.</span><span class="sxs-lookup"><span data-stu-id="53964-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="53964-425">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-425">Applicable Attributes</span></span>

<span data-ttu-id="53964-426">다음 표에서 설명에 적용할 수 있는 특성을 **EntityTypeMapping** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="53964-427">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-427">Attribute Name</span></span> | <span data-ttu-id="53964-428">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-428">Is Required</span></span> | <span data-ttu-id="53964-429">값</span><span class="sxs-lookup"><span data-stu-id="53964-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="53964-430">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="53964-430">**TypeName**</span></span>   | <span data-ttu-id="53964-431">예</span><span class="sxs-lookup"><span data-stu-id="53964-431">Yes</span></span>         | <span data-ttu-id="53964-432">매핑되는 개념적 모델 엔터티 형식의 네임스페이스로 한정된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="53964-433">해당 형식이 추상 또는 파생 형식이면 이 값은 `IsOfType(Namespace-qualified_type_name)`이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="53964-434">예제</span><span class="sxs-lookup"><span data-stu-id="53964-434">Example</span></span>

<span data-ttu-id="53964-435">다음 예제에서는 두 개의 자식 EntitySetMapping 요소를 보여 줍니다 **EntityTypeMapping** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="53964-436">첫 번째에서 **EntityTypeMapping** 요소를 **SchoolModel.Person** 엔터티 형식에 매핑되는 **Person** 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="53964-437">두 번째에서 **EntityTypeMapping** 요소, 업데이트 기능을 **SchoolModel.Person** 형식이 저장된 프로시저에 매핑되는 **UpdatePerson**, 데이터베이스에서 .</span><span class="sxs-lookup"><span data-stu-id="53964-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="53964-438">예제</span><span class="sxs-lookup"><span data-stu-id="53964-438">Example</span></span>

<span data-ttu-id="53964-439">다음 예제에서는 루트 형식이 추상인 형식 계층 구조의 매핑을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="53964-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="53964-440">사용 된 `IsOfType` 구문에 대 한는 **TypeName** 특성.</span><span class="sxs-lookup"><span data-stu-id="53964-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

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

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="53964-441">FunctionImportMapping 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="53964-442">합니다 **FunctionImportMapping** MSL (매핑 사양 언어)에서 요소 내부 데이터베이스에서 function import에서 개념적 모델 및 저장된 프로시저 또는 함수 간에 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="53964-443">Function Import는 개념적 모델에서 선언해야 하고 저장 프로시저는 저장소 모델에서 선언해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="53964-444">자세한 내용은 FunctionImport 요소 (CSDL) 및 함수 요소 (SSDL)를 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="53964-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="53964-445">기본적으로, Function Import에서 개념적 모델 엔터티 형식이나 복합 형식을 반환하는 경우에는 기본 저장 프로시저에서 반환된 열 이름이 개념적 모델 형식의 속성 이름과 정확히 일치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="53964-446">열 이름이 정확히 일치 하지 않는 속성 이름, ResultMapping 요소에서 매핑을 정의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="53964-447">합니다 **FunctionImportMapping** 요소는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="53964-448">ResultMapping (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="53964-449">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-449">Applicable Attributes</span></span>

<span data-ttu-id="53964-450">다음 표에서 해당 하는 특성을 설명 합니다 **FunctionImportMapping** 요소:</span><span class="sxs-lookup"><span data-stu-id="53964-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="53964-451">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-451">Attribute Name</span></span>         | <span data-ttu-id="53964-452">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-452">Is Required</span></span> | <span data-ttu-id="53964-453">값</span><span class="sxs-lookup"><span data-stu-id="53964-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="53964-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="53964-454">**FunctionImportName**</span></span> | <span data-ttu-id="53964-455">예</span><span class="sxs-lookup"><span data-stu-id="53964-455">Yes</span></span>         | <span data-ttu-id="53964-456">매핑되는 개념적 모델의 Function Import 이름</span><span class="sxs-lookup"><span data-stu-id="53964-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="53964-457">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="53964-457">**FunctionName**</span></span>       | <span data-ttu-id="53964-458">예</span><span class="sxs-lookup"><span data-stu-id="53964-458">Yes</span></span>         | <span data-ttu-id="53964-459">매핑되는 저장소 모델 함수의 네임스페이스로 한정된 이름</span><span class="sxs-lookup"><span data-stu-id="53964-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="53964-460">예제</span><span class="sxs-lookup"><span data-stu-id="53964-460">Example</span></span>

<span data-ttu-id="53964-461">다음 예제에서는 School 모델을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-461">The following example is based on the School model.</span></span> <span data-ttu-id="53964-462">다음은 저장소 모델의 함수입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="53964-463">다음은 개념적 모델의 Function Import입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="53964-464">다음 예제에서는 표시 된 **FunctionImportMapping** 함수 매핑하고 서로 위에 가져오기 작동 하는 데 사용 되는 요소:</span><span class="sxs-lookup"><span data-stu-id="53964-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="53964-465">InsertFunction 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="53964-466">합니다 **InsertFunction** MSL (매핑 사양 언어)에서 요소는 기본 데이터베이스의 저장된 프로시저에 엔터티 형식 또는 연결 개념적 모델에 대 한 삽입 함수를 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="53964-467">수정 함수가 매핑되는 저장 프로시저는 저장소 모델에서 선언되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="53964-468">자세한 내용은 함수 요소 (SSDL)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="53964-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="53964-469">매핑되지 않는 경우 모든 세 개의 삽입, 업데이트 또는 삭제 저장된 프로시저에 엔터티 형식의 작업, 경우 런타임에 실행 하면 매핑되지 않은 작업이 실패 합니다 및는 UpdateException throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="53964-470">합니다 **InsertFunction** 요소 ModificationFunctionMapping 요소의 자식이 될 수 있으며 EntityTypeMapping 요소 또는 AssociationSetMapping 요소에 적용 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="53964-471">EntityTypeMapping에 적용되는 InsertFunction</span><span class="sxs-lookup"><span data-stu-id="53964-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="53964-472">EntityTypeMapping 요소에 적용할 경우 합니다 **InsertFunction** 요소 저장된 프로시저를 개념적 모델에서 엔터티 형식의 삽입 함수를 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="53964-473">합니다 **InsertFunction** 요소에 적용 하는 경우 다음 자식 요소를 포함할 수 있습니다는 **EntityTypeMapping** 요소:</span><span class="sxs-lookup"><span data-stu-id="53964-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="53964-474">AssociationEnd (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="53964-475">ComplexProperty (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="53964-476">ResultBinding (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="53964-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="53964-477">ScarlarProperty (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="53964-478">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-478">Applicable Attributes</span></span>

<span data-ttu-id="53964-479">다음 테이블에 적용할 수 있는 특성에 설명 합니다 **InsertFunction** 요소에 적용 될 때를 **EntityTypeMapping** 요소.</span><span class="sxs-lookup"><span data-stu-id="53964-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="53964-480">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-480">Attribute Name</span></span>            | <span data-ttu-id="53964-481">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-481">Is Required</span></span> | <span data-ttu-id="53964-482">값</span><span class="sxs-lookup"><span data-stu-id="53964-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="53964-483">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="53964-483">**FunctionName**</span></span>          | <span data-ttu-id="53964-484">예</span><span class="sxs-lookup"><span data-stu-id="53964-484">Yes</span></span>         | <span data-ttu-id="53964-485">삽입 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="53964-486">저장 프로시저는 저장소 모델에서 선언해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="53964-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="53964-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="53964-488">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-488">No</span></span>          | <span data-ttu-id="53964-489">영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="53964-490">예제</span><span class="sxs-lookup"><span data-stu-id="53964-490">Example</span></span>

<span data-ttu-id="53964-491">다음 예제에서는 School 모델을 기반으로 하며 표시 합니다 **InsertFunction** 에 Person 엔터티 형식의 삽입 함수를 매핑하는 데 사용 되는 요소는 **InsertPerson** 저장 프로시저입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="53964-492">합니다 **InsertPerson** 저장된 프로시저는 저장소 모델에서 선언 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

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
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="53964-493">AssociationSetMapping에 적용된 InsertFunction</span><span class="sxs-lookup"><span data-stu-id="53964-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="53964-494">AssociationSetMapping 요소에 적용할 경우 합니다 **InsertFunction** 요소 저장된 프로시저를 개념적 모델의 연결에 대 한 삽입 함수를 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="53964-495">합니다 **InsertFunction** 요소에 적용 하는 경우 다음 자식 요소를 포함할 수 있습니다 합니다 **AssociationSetMapping** 요소:</span><span class="sxs-lookup"><span data-stu-id="53964-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="53964-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="53964-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="53964-497">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-497">Applicable Attributes</span></span>

<span data-ttu-id="53964-498">다음 표에서 특성을 적용할 수 있습니다는 **InsertFunction** 요소에 적용 되는 경우를 **AssociationSetMapping** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="53964-499">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-499">Attribute Name</span></span>            | <span data-ttu-id="53964-500">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-500">Is Required</span></span> | <span data-ttu-id="53964-501">값</span><span class="sxs-lookup"><span data-stu-id="53964-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="53964-502">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="53964-502">**FunctionName**</span></span>          | <span data-ttu-id="53964-503">예</span><span class="sxs-lookup"><span data-stu-id="53964-503">Yes</span></span>         | <span data-ttu-id="53964-504">삽입 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="53964-505">저장 프로시저는 저장소 모델에서 선언해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="53964-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="53964-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="53964-507">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-507">No</span></span>          | <span data-ttu-id="53964-508">영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="53964-509">예제</span><span class="sxs-lookup"><span data-stu-id="53964-509">Example</span></span>

<span data-ttu-id="53964-510">다음 예제에서는 School 모델을 기반으로 하며 표시를 **InsertFunction** 대 한 삽입 함수를 매핑하는 데 사용 되는 요소는 **CourseInstructor** 연결을  **InsertCourseInstructor** 저장 프로시저입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="53964-511">합니다 **InsertCourseInstructor** 저장된 프로시저는 저장소 모델에서 선언 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="mapping-element-msl"></a><span data-ttu-id="53964-512">Mapping 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="53964-513">합니다 **매핑** 요소 MSL (매핑 사양 언어)에 매핑 (저장소 모델에서 설명)로 데이터베이스에 개념적 모델에서 정의 된 개체에 대 한 정보를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="53964-514">자세한 내용은 CSDL 사양 및 SSDL 사양을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="53964-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="53964-515">합니다 **매핑** 요소는 매핑 사양의 루트 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="53964-516">매핑 사양에 대 한 XML 네임 스페이스는 http://schemas.microsoft.com/ado/2009/11/mapping/cs합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-516">The XML namespace for mapping specifications is http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="53964-517">Mapping 요소는 다음에 나열된 순서대로 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="53964-518">별칭 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="53964-519">EntityContainerMapping (1 개만)</span><span class="sxs-lookup"><span data-stu-id="53964-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="53964-520">MSL에서 참조하는 개념적 모델 형식과 저장소 모델 형식의 이름은 해당 네임스페이스 이름으로 한정되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="53964-521">개념적 모델 네임 스페이스 이름에 대 한 내용은 스키마 요소 (CSDL)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="53964-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="53964-522">저장소 모델 네임 스페이스 이름에 대 한 내용은 스키마 요소 (SSDL)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="53964-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="53964-523">Alias 요소를 사용 하 여 MSL에서 사용 되는 네임 스페이스에 대 한 별칭을 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="53964-524">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-524">Applicable Attributes</span></span>

<span data-ttu-id="53964-525">아래 표에서 설명에 적용할 수 있는 특성을 **매핑** 요소.</span><span class="sxs-lookup"><span data-stu-id="53964-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="53964-526">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-526">Attribute Name</span></span> | <span data-ttu-id="53964-527">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-527">Is Required</span></span> | <span data-ttu-id="53964-528">값</span><span class="sxs-lookup"><span data-stu-id="53964-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="53964-529">**스페이스바**</span><span class="sxs-lookup"><span data-stu-id="53964-529">**Space**</span></span>      | <span data-ttu-id="53964-530">예</span><span class="sxs-lookup"><span data-stu-id="53964-530">Yes</span></span>         | <span data-ttu-id="53964-531">**C-S**합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-531">**C-S**.</span></span> <span data-ttu-id="53964-532">이 값은 고정된 값이므로 변경할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="53964-533">예제</span><span class="sxs-lookup"><span data-stu-id="53964-533">Example</span></span>

<span data-ttu-id="53964-534">다음 예제는 **매핑** School 모델의 일부를 기반으로 하는 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="53964-535">School 모델에 대 한 자세한 내용은 빠른 시작 (Entity Framework)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="53964-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

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

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="53964-536">MappingFragment 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="53964-537">합니다 **MappingFragment** MSL (매핑 사양 언어)에서 요소는 개념적 모델 엔터티 형식 및 테이블 또는 데이터베이스에서 뷰 속성 간의 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="53964-538">개념적 모델 엔터티 형식과 기본 데이터베이스 테이블 또는 뷰에 대 한 자세한 내용은 EntityType 요소 (CSDL) 및 EntitySet 요소 (SSDL)를 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="53964-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="53964-539">합니다 **MappingFragment** EntityTypeMapping 요소 또는 EntitySetMapping 요소는 자식 요소가 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="53964-540">합니다 **MappingFragment** 요소는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="53964-541">ComplexType (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="53964-542">ScalarProperty (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="53964-543">조건 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="53964-544">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-544">Applicable Attributes</span></span>

<span data-ttu-id="53964-545">다음 표에서 설명에 적용할 수 있는 특성을 **MappingFragment** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="53964-546">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-546">Attribute Name</span></span>          | <span data-ttu-id="53964-547">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-547">Is Required</span></span> | <span data-ttu-id="53964-548">값</span><span class="sxs-lookup"><span data-stu-id="53964-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="53964-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="53964-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="53964-550">예</span><span class="sxs-lookup"><span data-stu-id="53964-550">Yes</span></span>         | <span data-ttu-id="53964-551">매핑되는 테이블 또는 뷰의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="53964-552">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="53964-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="53964-553">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-553">No</span></span>          | <span data-ttu-id="53964-554">**True 이면** 나 **False** 고유한 행만 반환 되는지 여부에 따라 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="53964-555">이 특성이로 설정 된 경우 **True**서 **GenerateUpdateViews** EntityContainerMapping 요소의 특성으로 설정 되어 있어야 **False**.</span><span class="sxs-lookup"><span data-stu-id="53964-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="53964-556">예제</span><span class="sxs-lookup"><span data-stu-id="53964-556">Example</span></span>

<span data-ttu-id="53964-557">다음 예제와 **MappingFragment** 의 자식 요소를 **EntityTypeMapping** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="53964-558">이 예제에서는 속성을를 **과정** 개념적 모델의 열에 매핑되는 **과정** 데이터베이스의 테이블.</span><span class="sxs-lookup"><span data-stu-id="53964-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="53964-559">예제</span><span class="sxs-lookup"><span data-stu-id="53964-559">Example</span></span>

<span data-ttu-id="53964-560">다음 예제와 **MappingFragment** 의 자식 요소를 **EntitySetMapping** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="53964-561">속성을 위의 예제와 같이 **과정** 개념적 모델의 열에 매핑되는 **과정** 데이터베이스의 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="53964-562">ModificationFunctionMapping 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="53964-563">합니다 **ModificationFunctionMapping** MSL (매핑 사양 언어)에서 요소 매핑합니다 삽입, 업데이트 및 기본 데이터베이스의 저장된 프로시저를 개념적 모델 엔터티 형식의 함수를 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="53964-564">합니다 **ModificationFunctionMapping** 요소 삽입 매핑할 고 기본 데이터베이스의 저장된 프로시저를 개념적 모델의 다 대 다 연결에 대 한 함수를 삭제할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="53964-565">수정 함수가 매핑되는 저장 프로시저는 저장소 모델에서 선언되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="53964-566">자세한 내용은 함수 요소 (SSDL)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="53964-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="53964-567">매핑되지 않는 경우 모든 세 개의 삽입, 업데이트 또는 삭제 저장된 프로시저에 엔터티 형식의 작업, 경우 런타임에 실행 하면 매핑되지 않은 작업이 실패 합니다 및는 UpdateException throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="53964-568">상속 계층 구조의 한 엔터티에 대한 수정 함수가 저장 프로시저에 매핑된 경우 계층 구조의 모든 형식에 대한 수정 함수가 저장 프로시저에 매핑되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="53964-569">합니다 **ModificationFunctionMapping** EntityTypeMapping 요소 또는 AssociationSetMapping 요소의 자식 요소일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="53964-570">합니다 **ModificationFunctionMapping** 요소는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="53964-571">DeleteFunction (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="53964-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="53964-572">InsertFunction (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="53964-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="53964-573">UpdateFunction (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="53964-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="53964-574">적용할 수 없는 특성을 **ModificationFunctionMapping** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="53964-575">예제</span><span class="sxs-lookup"><span data-stu-id="53964-575">Example</span></span>

<span data-ttu-id="53964-576">다음 예제에서는 엔터티 집합에 대 한 매핑을 합니다 **사람** School 모델의 엔터티 집합입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="53964-577">에 대 한 열 매핑 외에도 합니다 **Person** 엔터티 형식 매핑의 삽입, 업데이트 및 삭제 함수를 **사용자** 형식이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="53964-578">매핑되는 함수는 저장소 모델에서 선언됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-578">The functions that are mapped to are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="53964-579">예제</span><span class="sxs-lookup"><span data-stu-id="53964-579">Example</span></span>

<span data-ttu-id="53964-580">다음 예제에서는 연결에 대 한 매핑 집합을 **CourseInstructor** School 모델의 연결 집합입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="53964-581">에 대 한 열 매핑 외에도 합니다 **CourseInstructor** 연결의 삽입 및 삭제 함수 매핑 합니다 **CourseInstructor** 연결 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="53964-582">매핑되는 함수는 저장소 모델에서 선언됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-582">The functions that are mapped to are declared in the storage model.</span></span>

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
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="53964-583">QueryView 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="53964-584">합니다 **QueryView** 요소 MSL (매핑 사양 언어)에서 개념적 모델과 기본 데이터베이스의 테이블에서 엔터티 형식 또는 연결 간의 읽기 전용 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="53964-585">매핑은 저장소 모델에 대해 평가 되는 Entity SQL 쿼리를 사용 하 여 정의 됩니다 하 고 엔터티 또는 개념적 모델의 연결을 기준으로 결과 집합을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="53964-586">쿼리 뷰는 읽기 전용이므로 표준 업데이트 명령을 사용하여 쿼리 뷰에서 정의된 형식을 업데이트할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="53964-587">수정 함수를 사용하여 이러한 형식을 업데이트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="53964-588">자세한 내용은 방법: 저장 프로시저에 수정 함수를 맵.</span><span class="sxs-lookup"><span data-stu-id="53964-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="53964-589">에 **QueryView** 요소를 포함 하는 Entity SQL 식은 **GroupBy**, 그룹 집계 또는 탐색 속성이 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="53964-590">합니다 **QueryView** EntitySetMapping 요소 또는 AssociationSetMapping 요소의 자식 요소일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="53964-591">쿼리 뷰는 앞의 자식 요소의 경우 개념적 모델의 엔터티에 대한 읽기 전용 매핑을 정의하고,</span><span class="sxs-lookup"><span data-stu-id="53964-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="53964-592">뒤의 자식 요소의 경우 개념적 모델의 연결에 대한 읽기 전용 매핑을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="53964-593">경우는 **AssociationSetMapping** 요소는 참조 제약 조건 사용 하 여 연결 합니다 **AssociationSetMapping** 요소는 무시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="53964-594">자세한 내용은 ReferentialConstraint 요소 (CSDL)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="53964-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="53964-595">합니다 **QueryView** 요소는 모든 자식 요소를 포함할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="53964-596">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-596">Applicable Attributes</span></span>

<span data-ttu-id="53964-597">다음 표에서 설명에 적용할 수 있는 특성을 **QueryView** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="53964-598">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-598">Attribute Name</span></span> | <span data-ttu-id="53964-599">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-599">Is Required</span></span> | <span data-ttu-id="53964-600">값</span><span class="sxs-lookup"><span data-stu-id="53964-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="53964-601">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="53964-601">**TypeName**</span></span>   | <span data-ttu-id="53964-602">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-602">No</span></span>          | <span data-ttu-id="53964-603">쿼리 뷰에 의해 매핑되는 개념적 모델 형식의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="53964-604">예제</span><span class="sxs-lookup"><span data-stu-id="53964-604">Example</span></span>

<span data-ttu-id="53964-605">다음 예제와 **QueryView** 의 자식 요소로 합니다 **EntitySetMapping** 요소에 대 한 쿼리 뷰 매핑을 정의 하 고는 **부서** 엔터티 형식에는 School 모델입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

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

<span data-ttu-id="53964-606">쿼리만 포함 된 멤버의 하위 집합을 반환 하기 때문에 **부서** 저장소 모델의 형식에에서는 **부서** School 모델의 형식이 다음과 같이이 매핑을 기반으로 수정 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

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

### <a name="example"></a><span data-ttu-id="53964-607">예제</span><span class="sxs-lookup"><span data-stu-id="53964-607">Example</span></span>

<span data-ttu-id="53964-608">다음 예제에 나와 있는 **QueryView** 의 자식 요소로 **AssociationSetMapping** 요소에 대 한 읽기 전용 매핑을 정의 하 고는 `FK_Course_Department` School 모델에 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

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
 
### <a name="comments"></a><span data-ttu-id="53964-609">설명</span><span class="sxs-lookup"><span data-stu-id="53964-609">Comments</span></span>

<span data-ttu-id="53964-610">다음 시나리오가 가능하도록 쿼리 뷰를 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="53964-611">저장소 모델에 있는 엔터티의 모든 속성을 포함하지 않는 엔터티를 개념적 모델에서 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="53964-612">여기에 기본값이 없습니다를 지원 하지 않는 속성 **null** 값입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="53964-613">저장소 모델의 계산 열을 개념적 모델의 엔터티 형식 속성에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="53964-614">개념적 모델의 엔터티를 분할하는 데 사용된 조건이 같음을 기반으로 하지 않는 매핑을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="53964-615">사용 하 여 조건부 매핑을 지정 하면 합니다 **조건** 요소에 제공된 된 조건을 지정 된 값과 같아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="53964-616">자세한 내용은 조건 요소 (MSL)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="53964-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="53964-617">저장소 모델의 같은 열을 개념적 모델의 여러 형식에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="53964-618">여러 형식을 같은 테이블에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="53964-619">관계형 스키마에서 외래 키를 기반으로 하지 않는 연결을 개념적 모델에서 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="53964-620">사용자 지정 비즈니스 논리를 사용하여 개념적 모델에서 속성 값을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="53964-621">예를 들어의 문자열 값 "T" 값으로 데이터 원본에 매핑할 수 있습니다 **true**, 개념적 모델에는 부울 값입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="53964-622">쿼리 결과에 대한 조건부 필터를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="53964-623">저장소 모델보다 더 적은 제한을 개념적 모델의 데이터에 대해 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="53964-624">예를 들어, 할 수 있습니다 속성 개념적 모델의 null 허용 열이 매핑되는 지원 하지 않는 경우에 **null**값입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="53964-625">엔터티에 대한 쿼리 뷰를 정의할 때는 다음 사항을 고려해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="53964-626">쿼리 뷰는 읽기 전용입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-626">Query views are read-only.</span></span> <span data-ttu-id="53964-627">수정 함수를 사용하여 엔터티를 업데이트할 수만 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="53964-628">쿼리 뷰로 엔터티 형식을 정의하는 경우 관련된 모든 엔터티도 쿼리 뷰로 정의해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="53964-629">다 대 다 연결을 관계형 스키마의 링크 테이블을 나타내는 저장소 모델의 엔터티에 매핑되는 경우 정의 해야 합니다는 **QueryView** 요소에는 **AssociationSetMapping** 이 링크 테이블에 대 한 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="53964-630">쿼리 뷰는 형식 계층 구조의 모든 형식에 대해 정의되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="53964-631">다음과 같은 방법으로 이 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="53964-632">단일 **QueryView** 계층의 모든 엔터티 형식의 합집합을 반환 하는 단일 Entity SQL 쿼리를 지정 하는 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="53964-633">단일 **QueryView** CASE 연산자를 사용 하 여 계층 구조에서 특정 엔터티 형식을 반환 하는 단일 Entity SQL 쿼리를 지정 하는 요소는 특정 조건에 기반 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="53964-634">추가 사용 하 여 **QueryView** 계층 구조의 특정 형식에 대 한 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="53964-635">이 예에서 사용 하 여를 **TypeName** 특성을 **QueryView** 각 뷰의 엔터티 형식을 지정 하는 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="53964-636">쿼리 뷰를 정의할 때 지정할 수 없습니다는 **StorageSetName** 특성을 **EntitySetMapping** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="53964-637">쿼리 뷰를 정의할 때 합니다 **EntitySetMapping**요소를 포함할 수 없습니다 **속성** 매핑.</span><span class="sxs-lookup"><span data-stu-id="53964-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="53964-638">ResultBinding 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="53964-639">합니다 **ResultBinding** MSL (매핑 사양 언어)의 요소에서 반환 되는 저장된 프로시저를 개념적 모델의 엔터티 속성 엔터티 형식 수정 함수가 매핑되는 경우에 저장 된 열 값을 매핑됩니다 기본 데이터베이스에 대 한 절차입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="53964-640">예를 들어 id 열 값을 삽입 하 여 반환 될 때 저장 프로시저는 **ResultBinding** 요소 개념적 모델의 엔터티 형식 속성에 반환 되는 값을 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="53964-641">합니다 **ResultBinding** InsertFunction 요소 또는 UpdateFunction 요소의 자식 요소일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="53964-642">합니다 **ResultBinding** 요소는 모든 자식 요소를 포함할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="53964-643">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-643">Applicable Attributes</span></span>

<span data-ttu-id="53964-644">다음 표에서 해당 하는 특성을 설명 합니다 **ResultBinding** 요소:</span><span class="sxs-lookup"><span data-stu-id="53964-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="53964-645">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-645">Attribute Name</span></span> | <span data-ttu-id="53964-646">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-646">Is Required</span></span> | <span data-ttu-id="53964-647">값</span><span class="sxs-lookup"><span data-stu-id="53964-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="53964-648">**이름**</span><span class="sxs-lookup"><span data-stu-id="53964-648">**Name**</span></span>       | <span data-ttu-id="53964-649">예</span><span class="sxs-lookup"><span data-stu-id="53964-649">Yes</span></span>         | <span data-ttu-id="53964-650">매핑되는 개념적 모델의 엔터티 속성 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="53964-651">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="53964-651">**ColumnName**</span></span> | <span data-ttu-id="53964-652">예</span><span class="sxs-lookup"><span data-stu-id="53964-652">Yes</span></span>         | <span data-ttu-id="53964-653">매핑되는 열의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="53964-654">예제</span><span class="sxs-lookup"><span data-stu-id="53964-654">Example</span></span>

<span data-ttu-id="53964-655">다음 예제에서는 School 모델을 기반으로 하며 표시를 **InsertFunction** 대 한 삽입 함수를 매핑하는 데 사용 되는 요소는 **Person** 엔터티 형식을 **InsertPerson** 저장된 프로시저입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="53964-656">(합니다 **InsertPerson** 저장된 프로시저는 아래 및 저장소 모델에서 선언 됩니다.) A **ResultBinding** 요소는 저장된 프로시저에서 반환 되는 열 값에 매핑할 때 사용 됩니다 (**NewPersonID**)을 엔터티 형식 속성 (**PersonID**).</span><span class="sxs-lookup"><span data-stu-id="53964-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

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

<span data-ttu-id="53964-657">다음 TRANSACT-SQL에 설명 합니다 **InsertPerson** 저장 프로시저:</span><span class="sxs-lookup"><span data-stu-id="53964-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

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

## <a name="resultmapping-element-msl"></a><span data-ttu-id="53964-658">ResultMapping 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="53964-659">합니다 **ResultMapping** MSL (매핑 사양 언어)에서 요소 다음에 해당할 때 기본 데이터베이스에서 개념적 모델의 function import와 저장된 프로시저 간의 매핑을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="53964-660">Function Import가 개념적 모델 엔터티 형식 또는 복합 형식을 반환하는 경우</span><span class="sxs-lookup"><span data-stu-id="53964-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="53964-661">저장 프로시저에서 반환되는 열 이름이 엔터티 형식 또는 복합 형식의 속성 이름과 정확히 일치하지 않는 경우</span><span class="sxs-lookup"><span data-stu-id="53964-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="53964-662">기본적으로 저장 프로시저에서 반환되는 열과 엔터티 형식 또는 복합 형식 간의 매핑은 열 및 속성 이름을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="53964-663">열 이름이 정확히 일치 하지 않는 속성 이름, 경우에 사용 해야 합니다 **ResultMapping** 매핑을 정의 하는 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="53964-664">기본 매핑의 예 FunctionImportMapping 요소 (MSL)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="53964-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="53964-665">합니다 **ResultMapping** FunctionImportMapping 요소 자식 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="53964-666">합니다 **ResultMapping** 요소는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="53964-667">EntityTypeMapping (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="53964-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="53964-668">ComplexTypeMapping</span></span>

<span data-ttu-id="53964-669">적용할 수 없는 특성을 **ResultMapping** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="53964-670">예제</span><span class="sxs-lookup"><span data-stu-id="53964-670">Example</span></span>

<span data-ttu-id="53964-671">다음과 같은 저장 프로시저가 있다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-671">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="53964-672">또한 다음과 같은 개념적 모델 엔터티 형식이 있다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-672">Also consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="53964-673">이전 엔터티 형식의 인스턴스를 반환 하는 function import를 만들기 위해 열 간의 매핑이 저장된 프로시저에서 반환 하 고 엔터티 형식에 정의 되어야 합니다는 **ResultMapping** 요소:</span><span class="sxs-lookup"><span data-stu-id="53964-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

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

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="53964-674">ScalarProperty 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="53964-675">합니다 **ScalarProperty** MSL (매핑 사양 언어)의 요소는 개념적 모델 엔터티 형식, 복합 형식 또는 연결의 속성을 테이블 열 또는 기본 데이터베이스의 저장된 프로시저 매개 변수를에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="53964-676">수정 함수가 매핑되는 저장 프로시저는 저장소 모델에서 선언되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="53964-677">자세한 내용은 함수 요소 (SSDL)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="53964-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="53964-678">합니다 **ScalarProperty** 요소에는 다음 요소의 자식일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="53964-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="53964-679">MappingFragment</span></span>
-   <span data-ttu-id="53964-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="53964-680">InsertFunction</span></span>
-   <span data-ttu-id="53964-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="53964-681">UpdateFunction</span></span>
-   <span data-ttu-id="53964-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="53964-682">DeleteFunction</span></span>
-   <span data-ttu-id="53964-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="53964-683">EndProperty</span></span>
-   <span data-ttu-id="53964-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="53964-684">ComplexProperty</span></span>
-   <span data-ttu-id="53964-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="53964-685">ResultMapping</span></span>

<span data-ttu-id="53964-686">자식으로는 **MappingFragment**를 **ComplexProperty**, 또는 **EndProperty** 요소를 **ScalarProperty** 요소 속성을 매핑합니다 데이터베이스에서 열을 개념적 모델.</span><span class="sxs-lookup"><span data-stu-id="53964-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="53964-687">자식으로는 **InsertFunction**를 **UpdateFunction**, 또는 **DeleteFunction** 요소를 **ScalarProperty** 요소 속성을 매핑합니다 저장된 프로시저 매개 변수에 개념적 모델.</span><span class="sxs-lookup"><span data-stu-id="53964-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="53964-688">합니다 **ScalarProperty** 요소는 모든 자식 요소를 포함할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="53964-689">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-689">Applicable Attributes</span></span>

<span data-ttu-id="53964-690">에 적용 되는 특성을 **ScalarProperty** 요소는 해당 요소의 역할에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="53964-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="53964-691">다음 표에서 경우 적용할 수 있는 특성을 설명 합니다 **ScalarProperty** 요소는 개념적 모델 속성 데이터베이스의 열에 매핑하는 데 사용:</span><span class="sxs-lookup"><span data-stu-id="53964-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="53964-692">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-692">Attribute Name</span></span> | <span data-ttu-id="53964-693">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-693">Is Required</span></span> | <span data-ttu-id="53964-694">값</span><span class="sxs-lookup"><span data-stu-id="53964-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="53964-695">**이름**</span><span class="sxs-lookup"><span data-stu-id="53964-695">**Name**</span></span>       | <span data-ttu-id="53964-696">예</span><span class="sxs-lookup"><span data-stu-id="53964-696">Yes</span></span>         | <span data-ttu-id="53964-697">매핑되는 개념적 모델 속성의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="53964-698">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="53964-698">**ColumnName**</span></span> | <span data-ttu-id="53964-699">예</span><span class="sxs-lookup"><span data-stu-id="53964-699">Yes</span></span>         | <span data-ttu-id="53964-700">매핑되는 테이블 열의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="53964-701">다음 표에서 해당 하는 특성을 설명 합니다 **ScalarProperty** 요소 개념적 모델 속성을 저장된 프로시저 매개 변수에 매핑할 사용 하는 경우:</span><span class="sxs-lookup"><span data-stu-id="53964-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="53964-702">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-702">Attribute Name</span></span>    | <span data-ttu-id="53964-703">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-703">Is Required</span></span> | <span data-ttu-id="53964-704">값</span><span class="sxs-lookup"><span data-stu-id="53964-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="53964-705">**이름**</span><span class="sxs-lookup"><span data-stu-id="53964-705">**Name**</span></span>          | <span data-ttu-id="53964-706">예</span><span class="sxs-lookup"><span data-stu-id="53964-706">Yes</span></span>         | <span data-ttu-id="53964-707">매핑되는 개념적 모델 속성의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="53964-708">**ParameterName**</span><span class="sxs-lookup"><span data-stu-id="53964-708">**ParameterName**</span></span> | <span data-ttu-id="53964-709">예</span><span class="sxs-lookup"><span data-stu-id="53964-709">Yes</span></span>         | <span data-ttu-id="53964-710">매핑되는 매개 변수의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="53964-711">**Version**</span><span class="sxs-lookup"><span data-stu-id="53964-711">**Version**</span></span>       | <span data-ttu-id="53964-712">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-712">No</span></span>          | <span data-ttu-id="53964-713">**현재** 나 **원래** 동시성 검사에 대 한 현재 값 또는 속성의 원래 값은 사용 하는 여부에 따라 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="53964-714">예제</span><span class="sxs-lookup"><span data-stu-id="53964-714">Example</span></span>

<span data-ttu-id="53964-715">다음 예제는 **ScalarProperty** 두 가지 방법으로 사용 되는 요소:</span><span class="sxs-lookup"><span data-stu-id="53964-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="53964-716">속성을 매핑할를 **Person** 의 열에 엔터티 형식 합니다 **사용자**테이블.</span><span class="sxs-lookup"><span data-stu-id="53964-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="53964-717">속성을 매핑할를 **Person** 엔터티 형식 매개 변수에 **UpdatePerson** 저장 프로시저.</span><span class="sxs-lookup"><span data-stu-id="53964-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="53964-718">저장 프로시저는 저장소 모델에서 선언해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-718">The stored procedures are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="53964-719">예제</span><span class="sxs-lookup"><span data-stu-id="53964-719">Example</span></span>

<span data-ttu-id="53964-720">에서는 다음 예제는 **ScalarProperty** 삽입 매핑 및 데이터베이스의 저장된 프로시저를 개념적 모델 연결의 함수를 삭제 하는 데 사용 되는 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="53964-721">저장 프로시저는 저장소 모델에서 선언해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-721">The stored procedures are declared in the storage model.</span></span>

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

## <a name="updatefunction-element-msl"></a><span data-ttu-id="53964-722">UpdateFunction 요소(MSL)</span><span class="sxs-lookup"><span data-stu-id="53964-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="53964-723">합니다 **UpdateFunction** MSL (매핑 사양 언어)에서 요소는 기본 데이터베이스의 저장된 프로시저를 개념적 모델에서 엔터티 형식의 업데이트 함수를 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="53964-724">수정 함수가 매핑되는 저장 프로시저는 저장소 모델에서 선언되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="53964-725">자세한 내용은 함수 요소 (SSDL)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="53964-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="53964-726">매핑되지 않는 경우 모든 세 개의 삽입, 업데이트 또는 삭제 저장된 프로시저에 엔터티 형식의 작업, 경우 런타임에 실행 하면 매핑되지 않은 작업이 실패 합니다 및는 UpdateException throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="53964-727">합니다 **UpdateFunction** 요소 ModificationFunctionMapping 요소의 자식일 수 및 EntityTypeMapping 요소에 적용 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="53964-728">합니다 **UpdateFunction** 요소는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53964-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="53964-729">AssociationEnd (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="53964-730">ComplexProperty (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="53964-731">ResultBinding (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="53964-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="53964-732">ScarlarProperty (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="53964-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="53964-733">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="53964-733">Applicable Attributes</span></span>

<span data-ttu-id="53964-734">다음 표에서 설명에 적용할 수 있는 특성을 **UpdateFunction** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="53964-735">특성 이름</span><span class="sxs-lookup"><span data-stu-id="53964-735">Attribute Name</span></span>            | <span data-ttu-id="53964-736">필수 여부</span><span class="sxs-lookup"><span data-stu-id="53964-736">Is Required</span></span> | <span data-ttu-id="53964-737">값</span><span class="sxs-lookup"><span data-stu-id="53964-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="53964-738">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="53964-738">**FunctionName**</span></span>          | <span data-ttu-id="53964-739">예</span><span class="sxs-lookup"><span data-stu-id="53964-739">Yes</span></span>         | <span data-ttu-id="53964-740">업데이트 함수가 매핑되는 저장 프로시저의 네임스페이스로 한정된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="53964-741">저장 프로시저는 저장소 모델에서 선언해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="53964-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="53964-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="53964-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="53964-743">아니요</span><span class="sxs-lookup"><span data-stu-id="53964-743">No</span></span>          | <span data-ttu-id="53964-744">영향을 받는 행 수를 반환하는 출력 매개 변수의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="53964-745">예제</span><span class="sxs-lookup"><span data-stu-id="53964-745">Example</span></span>

<span data-ttu-id="53964-746">다음 예제에서는 School 모델을 기반으로 하며 표시를 **UpdateFunction** 업데이트 함수를 매핑하는 데 사용 되는 요소는 **Person** 엔터티 형식을 **UpdatePerson** 저장된 프로시저입니다.</span><span class="sxs-lookup"><span data-stu-id="53964-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="53964-747">합니다 **UpdatePerson** 저장된 프로시저는 저장소 모델에서 선언 됩니다.</span><span class="sxs-lookup"><span data-stu-id="53964-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

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
