---
title: EF6에서 EF 디자이너-관계
author: divega
ms.date: 2016-10-23
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: 72efe76956c930a787449e6cce453ab0317adc7c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994650"
---
# <a name="relationships---ef-designer"></a><span data-ttu-id="efb88-102">관계-EF 디자이너</span><span class="sxs-lookup"><span data-stu-id="efb88-102">Relationships - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="efb88-103">이 페이지는 EF 디자이너를 사용 하 여 모델의 관계를 설정 하는 방법에 대 한 정보를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-103">This page provides information about setting up relationships in your model using the EF Designer.</span></span> <span data-ttu-id="efb88-104">EF 및 관계를 사용 하 여 데이터 액세스 및 조작 하는 방법에 관계에 대 한 일반적인 정보를 참조 하세요 [관계 및 탐색 속성](~/ef6/fundamentals/relationships.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>

<span data-ttu-id="efb88-105">연결 모델의 엔터티 형식 간의 관계를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-105">Associations define relationships between entity types in a model.</span></span> <span data-ttu-id="efb88-106">이 항목에서는 Entity Framework Designer (EF 디자이너)를 사용 하 여 연결을 매핑하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-106">This topic shows how to map associations with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="efb88-107">다음 이미지에서는 EF 디자이너를 사용 하 여 작업할 때 사용 되는 기본 windows를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-107">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="efb88-109">개념적 모델을 빌드하면 매핑되지 않은 엔터티 및 연결에 대 한 경고는 오류 목록에 나타날 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-109">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="efb88-110">오류 사라집니다을 모델에서 데이터베이스를 생성 하도록 선택 하기 때문에 이러한 경고를 무시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-110">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="associations-overview"></a><span data-ttu-id="efb88-111">연결 개요</span><span class="sxs-lookup"><span data-stu-id="efb88-111">Associations Overview</span></span>

<span data-ttu-id="efb88-112">EF 디자이너를 사용 하 여 모델을 디자인할 때.edmx 파일 모델을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-112">When you design your model using the EF Designer, an .edmx file represents your model.</span></span> <span data-ttu-id="efb88-113">.Edmx 파일에는 **연결** 요소 두 엔터티 형식 간의 관계를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-113">In the .edmx file, an **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="efb88-114">연결은 관계에 관련된 엔터티 형식과 복합성이라도 하는 관계의 각 End에 있는 가능한 엔터티 형식 수를 지정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-114">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="efb88-115">연결 end의 복합성 값이 1 (1), 0 개 또는 1 (0..1) 또는 여러 열에 있을 수 있습니다 (\*).</span><span class="sxs-lookup"><span data-stu-id="efb88-115">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="efb88-116">이 정보는 두 명의 자식에 지정 된 **최종** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-116">This information is specified in two child **End** elements.</span></span>

<span data-ttu-id="efb88-117">런타임 시 (엔터티의 외래 키를 노출 하려는) 하는 경우 탐색 속성 또는 외래 키를 통해 연결의 한쪽 end에 엔터티 형식 인스턴스를 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-117">At run time, entity type instances at one end of an association can be accessed through navigation properties or foreign keys (if you choose to expose foreign keys in your entities).</span></span> <span data-ttu-id="efb88-118">노출 된 외래 키를 사용 하 여 엔터티 간의 관계를 사용 하 여 관리 되는 **ReferentialConstraint** 요소 (의 자식 요소를 **연결** 요소).</span><span class="sxs-lookup"><span data-stu-id="efb88-118">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element (a child element of the **Association** element).</span></span> <span data-ttu-id="efb88-119">항상 된 엔터티의 관계에 대 한 외래 키를 노출 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-119">It is recommended that you always expose foreign keys for relationships in your entities.</span></span>

> [!NOTE]
> <span data-ttu-id="efb88-120">다 대 다에서 (\*:\*) 엔터티에 외래 키를 추가할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-120">In many-to-many (\*:\*) you cannot add foreign keys to the entities.</span></span> <span data-ttu-id="efb88-121">에 \*:\* 독립 개체를 사용 하 여 관리 되는 관계, 연결 정보입니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-121">In a \*:\* relationship, the association information is managed with an independent object.</span></span>

<span data-ttu-id="efb88-122">CSDL 요소에 대 한 자세한 (**ReferentialConstraint**, **연결**등)을 참조 합니다 [CSDL 사양](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-122">For information about CSDL elements (**ReferentialConstraint**, **Association**, etc.) see the [CSDL specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span>

## <a name="create-and-delete-associations"></a><span data-ttu-id="efb88-123">만들기 및 연결 삭제</span><span class="sxs-lookup"><span data-stu-id="efb88-123">Create and Delete Associations</span></span>

<span data-ttu-id="efb88-124">EF 디자이너 업데이트와 연결 하 여.edmx 파일의 모델 콘텐츠를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-124">Creating an association with the EF Designer updates the model content of the .edmx file.</span></span> <span data-ttu-id="efb88-125">연결을 만든 후 (이 항목의 뒷부분에 설명) 연결에 대 한 매핑을 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-125">After creating an association, you must create the mappings for the association (discussed later in this topic).</span></span>

> [!NOTE]
> <span data-ttu-id="efb88-126">이 섹션에서는 모델 간의 연결을 만들려는 엔터티를 이미 추가 하는 것을 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-126">This section assumes that you already added the entities you wish to create an association between to your model.</span></span>

### <a name="to-create-an-association"></a><span data-ttu-id="efb88-127">연결을 만들려면</span><span class="sxs-lookup"><span data-stu-id="efb88-127">To create an association</span></span>

1.  <span data-ttu-id="efb88-128">디자인 화면의 빈 영역을 마우스 오른쪽 **새로 추가**, 선택한 **연결 하는 중...** .</span><span class="sxs-lookup"><span data-stu-id="efb88-128">Right-click an empty area of the design surface, point to **Add New**, and select **Association…**.</span></span>
2.  <span data-ttu-id="efb88-129">연결에 대 한 설정을 입력 합니다 **연결 추가** 대화 합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-129">Fill in the settings for the association in the **Add Association** dialog.</span></span>

    ![AddAssociation](~/ef6/media/addassociation.png)

    > [!NOTE]
    > <span data-ttu-id="efb88-131">하지 탐색 속성 또는 외래 키 속성에 추가할 연결의 end에서 엔터티 선택을 취소 하 여 선택할 수 있습니다는 * * 탐색 속성 * * 및 * *에 외래 키 속성을 추가 합니다 &lt;엔터티 형식 이름&gt; 엔터티 * * 확인란 합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-131">You can choose to not add navigation properties or foreign key properties to the entities at the ends of the association by clearing the **Navigation Property **and **Add foreign key properties to the &lt;entity type name&gt; Entity **checkboxes.</span></span> <span data-ttu-id="efb88-132">탐색 속성을 하나만 추가하는 경우 연결은 한 방향으로만 통과할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-132">If you add only one navigation property, the association will be traversable in only one direction.</span></span> <span data-ttu-id="efb88-133">탐색 속성을 추가하지 않는 경우 연결 End에 있는 엔터티에 액세스하기 위해 외래 키 속성을 추가하도록 선택해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-133">If you add no navigation properties, you must choose to add foreign key properties in order to access entities at the ends of the association.</span></span>
    
3.  <span data-ttu-id="efb88-134">**확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-134">Click **OK**.</span></span>

### <a name="to-delete-an-association"></a><span data-ttu-id="efb88-135">연결을 삭제하려면</span><span class="sxs-lookup"><span data-stu-id="efb88-135">To delete an association</span></span>

<span data-ttu-id="efb88-136">삭제 하려면 다음 중 하나는 연결 수행:</span><span class="sxs-lookup"><span data-stu-id="efb88-136">To delete an association do one of the following:</span></span>

-   <span data-ttu-id="efb88-137">EF 디자이너 화면에서 연결을 마우스 오른쪽 단추로 클릭 **삭제**합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-137">Right-click the association on the EF Designer surface and select **Delete**.</span></span>

- <span data-ttu-id="efb88-138">또는</span><span class="sxs-lookup"><span data-stu-id="efb88-138">OR -</span></span>

-   <span data-ttu-id="efb88-139">연결을 하나 이상 선택하고 Delete 키를 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-139">Select one or more associations and press the DELETE key.</span></span>

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a><span data-ttu-id="efb88-140">엔터티 (참조 제약 조건)에 외래 키 속성을 포함</span><span class="sxs-lookup"><span data-stu-id="efb88-140">Include Foreign Key Properties in Your Entities (Referential Constraints)</span></span>

<span data-ttu-id="efb88-141">항상 된 엔터티의 관계에 대 한 외래 키를 노출 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-141">It is recommended that you always expose foreign keys for relationships in your entities.</span></span> <span data-ttu-id="efb88-142">Entity Framework 참조 제약 조건이 사용 하 여 속성을 관계의 외래 키 역할도 식별.</span><span class="sxs-lookup"><span data-stu-id="efb88-142">Entity Framework uses a referential constraint to identify that a property acts as the foreign key for a relationship.</span></span>

<span data-ttu-id="efb88-143">선택한 경우에 ***외래 키 속성을 추가 합니다 &lt;엔터티 형식 이름&gt; 엔터티*** 확인란을이 참조 제약 조건 추가 된 관계를 만들 때.</span><span class="sxs-lookup"><span data-stu-id="efb88-143">If you checked the ***Add foreign key properties to the &lt;entity type name&gt; Entity*** checkbox when creating a relationship, this referential constraint was added for you.</span></span>

<span data-ttu-id="efb88-144">EF 디자이너를 사용 하 여 추가 하거나 참조 제약 조건을 편집 하는 경우 EF 디자이너를 추가 하거나이 수정 하는 한 **ReferentialConstraint** .edmx 파일의 CSDL 콘텐츠에서 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-144">When you use the EF Designer to add or edit a referential constraint, the EF Designer adds or modifies a **ReferentialConstraint** element in the CSDL content of the .edmx file.</span></span>

-   <span data-ttu-id="efb88-145">편집할 연결을 두 번 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-145">Double-click the association that you want to edit.</span></span>
    <span data-ttu-id="efb88-146">합니다 **참조 제약 조건의** 대화 상자가 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-146">The **Referential Constraint** dialog box appears.</span></span>
-   <span data-ttu-id="efb88-147">**주** 드롭 다운 목록에서 참조 제약 조건의 주 엔터티를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-147">From the **Principal** drop-down list, select the principal entity in the referential constraint.</span></span>
    <span data-ttu-id="efb88-148">엔터티의 키 속성에 추가 됩니다는 **주체 키** 대화 상자에서 목록입니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-148">The entity's key properties are added to the **Principal Key** list in the dialog box.</span></span>
-   <span data-ttu-id="efb88-149">**종속** 드롭 다운 목록에서 참조 제약 조건의 종속 엔터티를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-149">From the **Dependent** drop-down list, select the dependent entity in the referential constraint.</span></span>
-   <span data-ttu-id="efb88-150">종속 키가 있는 각 주 키에 대해 해당 하는 종속 키에서 드롭 다운 목록에서 선택 합니다 **종속 키** 열입니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-150">For each principal key that has a dependent key, select a corresponding dependent key from the drop-down lists in the **Dependent Key** column.</span></span>

    ![RefConstraint](~/ef6/media/refconstraint.png)

-   <span data-ttu-id="efb88-152">**확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-152">Click **OK**.</span></span>

## <a name="create-and-edit-association-mappings"></a><span data-ttu-id="efb88-153">연결 매핑 만들기 및 편집</span><span class="sxs-lookup"><span data-stu-id="efb88-153">Create and Edit Association Mappings</span></span>

<span data-ttu-id="efb88-154">연결의 데이터베이스에 매핑되는 방식을 지정할 수 있습니다 합니다 **매핑 정보** EF 디자이너의 창.</span><span class="sxs-lookup"><span data-stu-id="efb88-154">You can specify how an association maps to the database in the **Mapping Details** window of the EF Designer.</span></span>

> [!NOTE]
> <span data-ttu-id="efb88-155">참조 제약 조건이 지정 되지 않은 연결에 대 한 세부 정보에만 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-155">You can only map details for the associations that do not have a referential constraint specified.</span></span> <span data-ttu-id="efb88-156">그런 다음 참조 제약 조건이 지정 된 경우 엔터티에 외래 키 속성이 포함 되어 고 외래 키에 매핑되는 열 컨트롤에 엔터티 매핑 정보를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-156">If a referential constraint is specified then a foreign key property is included in the entity and you can use the Mapping Details for the entity to control which column the foreign key maps to.</span></span>

### <a name="create-an-association-mapping"></a><span data-ttu-id="efb88-157">연결 매핑을 만들려면</span><span class="sxs-lookup"><span data-stu-id="efb88-157">Create an association mapping</span></span>

-   <span data-ttu-id="efb88-158">디자인 화면에서 연결을 마우스 오른쪽 단추로 클릭 **테이블 매핑**합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-158">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="efb88-159">에 연결 매핑이 표시 됩니다는 **매핑 정보** 창입니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-159">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="efb88-160">클릭 **테이블이 나 뷰 추가**합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-160">Click **Add a Table or View**.</span></span>
    <span data-ttu-id="efb88-161">저장소 모델에 있는 모든 테이블이 포함된 드롭다운 목록이 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-161">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="efb88-162">연결이 매핑될 테이블을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-162">Select the table to which the association will map.</span></span>
    <span data-ttu-id="efb88-163">합니다 **매핑 정보** 창에서 각 엔터티 형식의 키 속성과 연결의 양쪽에 표시 됩니다 **끝**합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-163">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each **End**.</span></span>
-   <span data-ttu-id="efb88-164">각 키 속성을 클릭 합니다 **열** 필드 및 속성이 매핑될 열을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-164">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

    ![MappingDetails4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a><span data-ttu-id="efb88-166">연결 매핑을 편집합니다</span><span class="sxs-lookup"><span data-stu-id="efb88-166">Edit an association mapping</span></span>

-   <span data-ttu-id="efb88-167">디자인 화면에서 연결을 마우스 오른쪽 단추로 클릭 **테이블 매핑**합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-167">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="efb88-168">에 연결 매핑이 표시 됩니다는 **매핑 정보** 창입니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-168">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="efb88-169">클릭 **매핑됩니다 &lt;테이블 이름&gt;** 합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-169">Click **Maps to &lt;Table Name&gt;**.</span></span>
    <span data-ttu-id="efb88-170">저장소 모델에 있는 모든 테이블이 포함된 드롭다운 목록이 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-170">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="efb88-171">연결이 매핑될 테이블을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-171">Select the table to which the association will map.</span></span>
    <span data-ttu-id="efb88-172">합니다 **매핑 정보** 창 각 End에 있는 엔터티 형식의 키 속성과 연결의 양쪽에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-172">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each End.</span></span>
-   <span data-ttu-id="efb88-173">각 키 속성을 클릭 합니다 **열** 필드 및 속성이 매핑될 열을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-173">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

## <a name="edit-and-delete-navigation-properties"></a><span data-ttu-id="efb88-174">편집 및 탐색 속성 삭제</span><span class="sxs-lookup"><span data-stu-id="efb88-174">Edit and Delete Navigation Properties</span></span>

<span data-ttu-id="efb88-175">탐색 속성은 모델에서 연결의 end에서 엔터티를 찾는 데 사용 되는 바로 가기 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-175">Navigation properties are shortcut properties that are used to locate the entities at the ends of an association in a model.</span></span> <span data-ttu-id="efb88-176">두 엔터티 형식 간 연결을 만들면 탐색 속성을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-176">Navigation properties can be created when you create an association between two entity types.</span></span>

#### <a name="to-edit-navigation-properties"></a><span data-ttu-id="efb88-177">탐색 속성을 편집 하려면</span><span class="sxs-lookup"><span data-stu-id="efb88-177">To edit navigation properties</span></span>

-   <span data-ttu-id="efb88-178">EF 디자이너 화면에서 탐색 속성을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-178">Select a navigation property on the EF Designer surface.</span></span>
    <span data-ttu-id="efb88-179">Visual Studio에는 탐색 속성에 대 한 정보가 표시 됩니다 **속성** 창입니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-179">Information about the navigation property is displayed in the Visual Studio **Properties** window.</span></span>
-   <span data-ttu-id="efb88-180">속성 설정을 변경 합니다 **속성** 창입니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-180">Change the property settings in the **Properties** window.</span></span>

#### <a name="to-delete-navigation-properties"></a><span data-ttu-id="efb88-181">탐색 속성을 삭제 하려면</span><span class="sxs-lookup"><span data-stu-id="efb88-181">To delete navigation properties</span></span>

-   <span data-ttu-id="efb88-182">개념적 모델의 엔터티 형식에서 외래 키가 노출되지 않는 경우 탐색 속성을 삭제하면 해당 연결을 한 방향으로만 통과할 수 있는 연결 또는 전혀 통과할 수 없는 연결로 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-182">If foreign keys are not exposed on entity types in the conceptual model, deleting a navigation property may make the corresponding association traversable in only one direction or not traversable at all.</span></span>
-   <span data-ttu-id="efb88-183">EF 디자이너 화면에 탐색 속성을 마우스 오른쪽 단추로 클릭 **삭제**합니다.</span><span class="sxs-lookup"><span data-stu-id="efb88-183">Right-click a navigation property on the EF Designer surface and select **Delete**.</span></span>
