---
title: 복합 형식-EF 디자이너-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: a3c5578acee23688c04772d2dd0a2221779af562
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489910"
---
# <a name="complex-types---ef-designer"></a><span data-ttu-id="0e6fe-102">복합 형식-EF 디자이너</span><span class="sxs-lookup"><span data-stu-id="0e6fe-102">Complex Types - EF Designer</span></span>
<span data-ttu-id="0e6fe-103">이 항목에서는 Entity Framework Designer (EF 디자이너)를 사용 하 여 복합 형식을 매핑하는 방법 및 복합 형식의 속성을 포함 하는 엔터티에 대 한 쿼리 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-103">This topic shows how to map complex types with the Entity Framework Designer (EF Designer) and how to query for entities that contain properties of complex type.</span></span>

<span data-ttu-id="0e6fe-104">다음 이미지에서는 EF 디자이너를 사용 하 여 작업할 때 사용 되는 기본 windows를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-104">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF 디자이너](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="0e6fe-106">개념적 모델을 빌드하면 매핑되지 않은 엔터티 및 연결에 대 한 경고는 오류 목록에 나타날 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-106">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="0e6fe-107">오류 사라집니다을 모델에서 데이터베이스를 생성 하도록 선택 하기 때문에 이러한 경고를 무시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-107">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="what-is-a-complex-type"></a><span data-ttu-id="0e6fe-108">복합 형식 이란</span><span class="sxs-lookup"><span data-stu-id="0e6fe-108">What is a Complex Type</span></span>

<span data-ttu-id="0e6fe-109">복합 형식은 스칼라 속성이 엔터티 내에 구성되도록 하는 엔터티 형식의 비스칼라 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-109">Complex types are non-scalar properties of entity types that enable scalar properties to be organized within entities.</span></span> <span data-ttu-id="0e6fe-110">복합 형식은 엔터티와 같이 스칼라 속성 또는 다른 복합 형식 속성으로 이루어집니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-110">Like entities, complex types consist of scalar properties or other complex type properties.</span></span>

<span data-ttu-id="0e6fe-111">복합 형식을 나타내는 개체를 사용 하 여 작업할 때 다음을 고려해 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-111">When you work with objects that represent complex types, be aware of the following:</span></span>

-   <span data-ttu-id="0e6fe-112">복합 형식은 키 없고 따라서 독립적으로 존재할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-112">Complex types do not have keys and therefore cannot exist independently.</span></span> <span data-ttu-id="0e6fe-113">복합 형식은 엔터티 형식 또는 다른 복합 형식의 속성으로만 존재할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-113">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="0e6fe-114">복합 형식 연결에 참여할 수 없습니다 및 탐색 속성을 포함할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-114">Complex types cannot participate in associations and cannot contain navigation properties.</span></span>
-   <span data-ttu-id="0e6fe-115">복합 형식 속성 일 수 없습니다 **null**합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-115">Complex type properties cannot be **null**.</span></span> <span data-ttu-id="0e6fe-116">\* \* InvalidOperationException \* \* 발생 때 **DbContext.SaveChanges** 라고는 null 복합 개체가 발견 되 고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-116">An \*\*InvalidOperationException \*\*occurs when **DbContext.SaveChanges** is called and a null complex object is encountered.</span></span> <span data-ttu-id="0e6fe-117">복잡 한 개체의 스칼라 속성 일 수 있습니다 **null**합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-117">Scalar properties of complex objects can be **null**.</span></span>
-   <span data-ttu-id="0e6fe-118">복합 형식은 다른 복합 형식에서 상속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-118">Complex types cannot inherit from other complex types.</span></span>
-   <span data-ttu-id="0e6fe-119">복합 유형으로 정의 해야 합니다는 **클래스**합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-119">You must define the complex type as a **class**.</span></span> 
-   <span data-ttu-id="0e6fe-120">EF에서 복합 형식 개체에서 멤버의 변경 내용을 검색 하면 **DbContext.DetectChanges** 라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-120">EF detects changes to members on a complex type object when **DbContext.DetectChanges** is called.</span></span> <span data-ttu-id="0e6fe-121">엔터티 프레임 워크에서 호출 **DetectChanges** 는 다음 멤버가 호출 될 때 자동으로: **DbSet.Find**하십시오 **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**합니다 **DbSet.Attach**를 **DbContext.SaveChanges**를 **DbContext.GetValidationErrors**, **DbContext.Entry**하십시오 **DbChangeTracker.Entries**합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-121">Entity Framework calls **DetectChanges** automatically when the following members are called: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span></span>

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a><span data-ttu-id="0e6fe-122">엔터티의 속성을 새 복합 형식으로 리팩터링</span><span class="sxs-lookup"><span data-stu-id="0e6fe-122">Refactor an Entity’s Properties into New Complex Type</span></span>

<span data-ttu-id="0e6fe-123">개념적 모델의 엔터티를 이미 있는 경우에 일부 속성을 복합 형식 속성으로 리팩터링 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-123">If you already have an entity in your conceptual model you may want to refactor some of the properties into a complex type property.</span></span>

<span data-ttu-id="0e6fe-124">디자이너 화면에서 하나를 선택 하거나 엔터티를 더 많은 속성 (탐색 속성 제외)에서 그런 다음 마우스 오른쪽 단추로 클릭 하 고 선택 **-리팩터링&gt; 새 복합 형식이 이동**합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-124">On the designer surface, select one or more properties (excluding navigation properties) of an entity, then right-click and select **Refactor -&gt; Move to New Complex Type**.</span></span>

![리팩터링](~/ef6/media/refactor.png)

<span data-ttu-id="0e6fe-126">선택한 속성을 사용 하 여 새 복합 형식에 추가 되는 **Model 브라우저**합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-126">A new complex type with the selected properties is added to the **Model Browser**.</span></span> <span data-ttu-id="0e6fe-127">복합 형식에 기본 이름이 지정됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-127">The complex type is given a default name.</span></span>

<span data-ttu-id="0e6fe-128">새로 만든 형식의 복합 속성이 선택된 속성을 대체합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-128">A complex property of the newly created type replaces the selected properties.</span></span> <span data-ttu-id="0e6fe-129">모든 속성 매핑은 유지됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-129">All property mappings are preserved.</span></span>

![2 리팩터링](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a><span data-ttu-id="0e6fe-131">새 복합 형식을 만들려면</span><span class="sxs-lookup"><span data-stu-id="0e6fe-131">Create a New Complex Type</span></span>

<span data-ttu-id="0e6fe-132">또한 기존 엔터티의 속성을 포함 하지 않는 새 복합 형식을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-132">You can also create a new complex type that does not contain properties of an existing entity.</span></span>

<span data-ttu-id="0e6fe-133">마우스 오른쪽 단추로 클릭 합니다 **복합 형식** 폴더 Model 브라우저에서 가리키고 **AddNew 복합 형식...** .</span><span class="sxs-lookup"><span data-stu-id="0e6fe-133">Right-click the **Complex Types** folder in the Model Browser, point to **AddNew Complex Type…**.</span></span> <span data-ttu-id="0e6fe-134">선택할 수 있습니다 합니다 **복합 형식** 누릅니다 폴더를 **삽입** 키보드의 키입니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-134">Alternatively, you can select the **Complex Types** folder and press the **Insert** key on your keyboard.</span></span>

![새 복합 형식 추가](~/ef6/media/addnewcomplextype.png)

<span data-ttu-id="0e6fe-136">새 복합 형식이 기본 이름의 폴더에 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-136">A new complex type is added to the folder with a default name.</span></span> <span data-ttu-id="0e6fe-137">이제 형식에 속성을 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-137">You can now add properties to the type.</span></span>

## <a name="add-properties-to-a-complex-type"></a><span data-ttu-id="0e6fe-138">복합 형식에 속성 추가</span><span class="sxs-lookup"><span data-stu-id="0e6fe-138">Add Properties to a Complex Type</span></span>

<span data-ttu-id="0e6fe-139">복합 형식의 속성은 스칼라 형식 또는 기존 복합 형식일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-139">Properties of a complex type can be scalar types or existing complex types.</span></span> <span data-ttu-id="0e6fe-140">그러나 복합 형식 속성에는 순환 참조가 있을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-140">However, complex type properties cannot have circular references.</span></span> <span data-ttu-id="0e6fe-141">예를 들어, 복합 형식 **OnsiteCourseDetails** 복합 형식의 속성을 사용할 수 없습니다 **OnsiteCourseDetails**합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-141">For example, a complex type **OnsiteCourseDetails** cannot have a property of complex type **OnsiteCourseDetails**.</span></span>

<span data-ttu-id="0e6fe-142">아래에 나열된 방법 중 하나를 사용하여 복합 형식에 속성을 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-142">You can add a property to a complex type in any of the ways listed below.</span></span>

-   <span data-ttu-id="0e6fe-143">Model 브라우저에서 복합 형식을 마우스 오른쪽 **추가**, 가리킨 **스칼라 속성** 하거나 **복합 속성**, desired 속성 형식을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-143">Right-click a complex type in the Model Browser, point to **Add**, then point to **Scalar Property** or **Complex Property**, then select the desired property type.</span></span> <span data-ttu-id="0e6fe-144">복합 형식을 선택 하 고 다음 키를 누릅니다 수 또는 합니다 **삽입** 키보드의 키입니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-144">Alternatively, you can select a complex type and then press the **Insert** key on your keyboard.</span></span>  

    ![복합 형식 속성을 추가 합니다.](~/ef6/media/addpropertiestocomplextype.png)

    <span data-ttu-id="0e6fe-146">새 속성이 기본 이름의 복합 형식에 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-146">A new property is added to the complex type with a default name.</span></span>

- <span data-ttu-id="0e6fe-147">또는</span><span class="sxs-lookup"><span data-stu-id="0e6fe-147">OR -</span></span>

-   <span data-ttu-id="0e6fe-148">엔터티 속성을 마우스 오른쪽 단추로 클릭 합니다 **EF 디자이너** 화면 **복사**에서 복합 형식을 마우스 오른쪽 단추로 클릭 합니다 **Model 브라우저** 선택한  **붙여넣기**합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-148">Right-click an entity property on the **EF  Designer** surface and select **Copy**, then right-click the complex type in the **Model Browser** and select **Paste**.</span></span>

## <a name="rename-a-complex-type"></a><span data-ttu-id="0e6fe-149">복합 형식의 이름을 바꾸려면</span><span class="sxs-lookup"><span data-stu-id="0e6fe-149">Rename a Complex Type</span></span>

<span data-ttu-id="0e6fe-150">복합 형식의 이름을 바꾸면 프로젝트 전체에서 형식에 대한 모든 참조가 업데이트됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-150">When you rename a complex type, all references to the type are updated throughout the project.</span></span>

-   <span data-ttu-id="0e6fe-151">느린에서 복합 형식을 두 번 클릭 합니다 **Model 브라우저**합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-151">Slowly double-click a complex type in the **Model Browser**.</span></span>
    <span data-ttu-id="0e6fe-152">이름이 선택되고 편집 모드가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-152">The name will be selected and in edit mode.</span></span>

- <span data-ttu-id="0e6fe-153">또는</span><span class="sxs-lookup"><span data-stu-id="0e6fe-153">OR -</span></span>

-   <span data-ttu-id="0e6fe-154">복합 형식을 마우스 오른쪽 단추로 클릭 합니다 **Model 브라우저** 선택한 **이름 바꾸기**합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-154">Right-click a complex type in the **Model Browser** and select **Rename**.</span></span>

- <span data-ttu-id="0e6fe-155">또는</span><span class="sxs-lookup"><span data-stu-id="0e6fe-155">OR -</span></span>

-   <span data-ttu-id="0e6fe-156">Model 브라우저에서 복합 형식을 선택하고 F2 키를 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-156">Select a complex type in the Model Browser and press the F2 key.</span></span>

- <span data-ttu-id="0e6fe-157">또는</span><span class="sxs-lookup"><span data-stu-id="0e6fe-157">OR -</span></span>

-   <span data-ttu-id="0e6fe-158">복합 형식을 마우스 오른쪽 단추로 클릭 합니다 **Model 브라우저** 선택한 **속성**합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-158">Right-click a complex type in the **Model Browser** and select **Properties**.</span></span> <span data-ttu-id="0e6fe-159">이름을 편집 합니다 **속성** 창입니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-159">Edit the name in the **Properties** window.</span></span>

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a><span data-ttu-id="0e6fe-160">엔터티에 기존 복합 형식 추가 및 해당 속성을 테이블 열에 매핑</span><span class="sxs-lookup"><span data-stu-id="0e6fe-160">Add an Existing Complex Type to an Entity and Map its Properties to Table Columns</span></span>

1.  <span data-ttu-id="0e6fe-161">엔터티를 마우스 오른쪽 **새로 추가**, 선택한 **복합 속성**합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-161">Right-click an entity, point to **Add New**, and select **Complex Property**.</span></span>
    <span data-ttu-id="0e6fe-162">기본 이름의 복합 형식 속성이 엔터티에 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-162">A complex type property with a default name is added to the entity.</span></span> <span data-ttu-id="0e6fe-163">기존 복합 형식에서 선택한 기본 형식이 속성에 할당됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-163">A default type (chosen from the existing complex types) is assigned to the property.</span></span>
2.  <span data-ttu-id="0e6fe-164">원하는 형식의 속성에 할당 합니다 **속성** 창입니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-164">Assign the desired type to the property in the **Properties** window.</span></span>
    <span data-ttu-id="0e6fe-165">복합 형식 속성을 엔터티에 추가한 후에는 해당 속성을 테이블 열에 매핑해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-165">After adding a complex type property to an entity, you must map its properties to table columns.</span></span>
3.  <span data-ttu-id="0e6fe-166">또는 디자인 화면에서 엔터티 형식을 마우스 오른쪽 단추로 클릭 합니다 **Model 브라우저** 선택한 **테이블 매핑을**합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-166">Right-click an entity type on the design surface or in the **Model Browser** and select **Table Mappings**.</span></span>
    <span data-ttu-id="0e6fe-167">테이블 매핑이 표시 됩니다는 **매핑 정보** 창입니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-167">The table mappings are displayed in the **Mapping Details** window.</span></span>
4.  <span data-ttu-id="0e6fe-168">확장 된 **매핑됩니다 &lt;테이블 이름&gt;**  노드.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-168">Expand the **Maps to &lt;Table Name&gt;** node.</span></span>
    <span data-ttu-id="0e6fe-169">A **열 매핑** 노드가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-169">A **Column Mappings** node appears.</span></span>
5.  <span data-ttu-id="0e6fe-170">확장 된 **열 매핑** 노드.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-170">Expand the **Column Mappings** node.</span></span>
    <span data-ttu-id="0e6fe-171">테이블의 모든 열이 나열된 목록이 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-171">A list of all the columns in the table appears.</span></span> <span data-ttu-id="0e6fe-172">기본 속성 (있는 경우) 열에 매핑하는 아래에 나열 됩니다는 **값/속성** 제목입니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-172">The default properties (if any) to which the columns map are listed under the **Value/Property** heading.</span></span>
6.  <span data-ttu-id="0e6fe-173">에 매핑할 열을 선택한 다음 해당 마우스 오른쪽 단추로 **값/속성** 필드입니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-173">Select the column you want to map, and then right-click the corresponding **Value/Property** field.</span></span>
    <span data-ttu-id="0e6fe-174">모든 스칼라 속성을 나열한 드롭다운 목록이 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-174">A drop-down list of all the scalar properties is displayed.</span></span>
7.  <span data-ttu-id="0e6fe-175">적절한 속성을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-175">Select the appropriate property.</span></span>

    ![복합 형식 매핑](~/ef6/media/mapcomplextype.png)

8.  <span data-ttu-id="0e6fe-177">각 테이블 열별로 6 ~ 7단계를 반복합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-177">Repeat steps 6 and 7 for each table column.</span></span>

>[!NOTE]
> <span data-ttu-id="0e6fe-178">열 매핑을 삭제를 매핑하고, 클릭 하려는 열을 선택 합니다 **값/속성** 필드입니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-178">To delete a column mapping, select the column that you want to map, and then click the **Value/Property** field.</span></span> <span data-ttu-id="0e6fe-179">그런 다음 선택 **삭제** 드롭 다운 목록에서.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-179">Then, select **Delete** from the drop-down list.</span></span>

## <a name="map-a-function-import-to-a-complex-type"></a><span data-ttu-id="0e6fe-180">Function Import를 복합 형식 매핑</span><span class="sxs-lookup"><span data-stu-id="0e6fe-180">Map a Function Import to a Complex Type</span></span>

<span data-ttu-id="0e6fe-181">Function Import는 저장 프로시저를 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-181">Function imports are based on stored procedures.</span></span> <span data-ttu-id="0e6fe-182">Function Import를 복합 형식에 매핑하려면 해당 저장 프로시저에서 반환하는 열의 수와 복합 형식의 속성 수가 일치해야 하고 저장소 형식이 속성 형식과 호환되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-182">To map a function import to a complex type, the columns returned by the corresponding stored procedure must match the properties of the complex type in number and must have storage types that are compatible with the property types.</span></span>

-   <span data-ttu-id="0e6fe-183">복합 형식에 매핑하려는 가져온된 함수가 두 번 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-183">Double-click on an imported function that you want to map to a complex type.</span></span>

    ![함수 가져오기](~/ef6/media/functionimports.png)

-   <span data-ttu-id="0e6fe-185">다음과 같이 새 Function Import에 대한 설정을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-185">Fill in the settings for the new function import, as follows:</span></span>
    -   <span data-ttu-id="0e6fe-186">function import를 만드는 저장된 프로시저를 지정 합니다 **저장 프로시저 이름** 필드입니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-186">Specify the stored procedure for which you are creating a function import in the **Stored Procedure Name** field.</span></span> <span data-ttu-id="0e6fe-187">이 필드는 저장소 모델의 모든 저장 프로시저가 표시되는 드롭다운 목록입니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-187">This field is a drop-down list that displays all the stored procedures in the storage model.</span></span>
    -   <span data-ttu-id="0e6fe-188">function import의 이름을 지정 합니다 **Function Import 이름** 필드입니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-188">Specify the name of the function import in the **Function Import Name** field.</span></span>
    -   <span data-ttu-id="0e6fe-189">선택 **복잡 한** 반환으로 입력 한 다음 드롭다운 목록에서 적절 한 유형을 선택 하 여 특정 복합 반환 형식을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-189">Select **Complex** as the return type and then specify the specific complex return type by choosing the appropriate type from the drop-down list.</span></span>

        ![Function Import 편집](~/ef6/media/editfunctionimport.png)

-   <span data-ttu-id="0e6fe-191">**확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-191">Click **OK**.</span></span>
    <span data-ttu-id="0e6fe-192">Function Import 항목이 개념적 모델에 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-192">The function import entry is created in the conceptual model.</span></span>

### <a name="customize-column-mapping-for-function-import"></a><span data-ttu-id="0e6fe-193">열 매핑 함수 가져오기에 대 한 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="0e6fe-193">Customize Column Mapping for Function Import</span></span>

-   <span data-ttu-id="0e6fe-194">Model 브라우저에서 function import를 마우스 오른쪽 단추로 클릭 **Function Import 매핑**합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-194">Right-click the function import in the Model Browser and select **Function Import Mapping**.</span></span>
    <span data-ttu-id="0e6fe-195">합니다 **매핑 정보** 창이 나타나고 함수 가져오기에 대 한 기본 매핑을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-195">The **Mapping Details** window appears and shows the default mapping for the function import.</span></span> <span data-ttu-id="0e6fe-196">화살표는 열 값과 속성 값 사이의 매핑을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-196">Arrows indicate the mappings between column values and property values.</span></span> <span data-ttu-id="0e6fe-197">기본적으로 열 이름은 복합 형식의 속성 이름과 같다고 간주됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-197">By default, the column names are assumed to be the same as the complex type's property names.</span></span> <span data-ttu-id="0e6fe-198">기본 열 이름은 회색 텍스트로 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-198">The default column names appear in gray text.</span></span>
-   <span data-ttu-id="0e6fe-199">필요한 경우 Function Import에 대응하는 저장 프로시저에서 반환된 열 이름과 일치하도록 열 이름을 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-199">If necessary, change the column names to match the column names that are returned by the stored procedure that corresponds to the function import.</span></span>

## <a name="delete-a-complex-type"></a><span data-ttu-id="0e6fe-200">복합 형식을 삭제합니다</span><span class="sxs-lookup"><span data-stu-id="0e6fe-200">Delete a Complex Type</span></span>

<span data-ttu-id="0e6fe-201">복합 형식을 삭제하면 개념적 모델에서 형식이 삭제되고 형식의 모든 인스턴스에 대한 매핑이 삭제됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-201">When you delete a complex type, the type is deleted from the conceptual model and mappings for all instances of the type are deleted.</span></span> <span data-ttu-id="0e6fe-202">그러나 형식에 대한 참조는 업데이트되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-202">However, references to the type are not updated.</span></span> <span data-ttu-id="0e6fe-203">예를 들어, 엔터티에 형식이 complextype1 인 복합 형식 속성 있고 ComplexType1에서 삭제 되는 **Model 브라우저**, 해당 엔터티 속성이 업데이트 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-203">For example, if an entity has a complex type property of type ComplexType1 and ComplexType1 is deleted in the **Model Browser**, the corresponding entity property is not updated.</span></span> <span data-ttu-id="0e6fe-204">모델 삭제 된 복합 형식을 참조 하는 엔터티를 포함 하기 때문에 유효 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-204">The model will not validate  because it contains an entity that references a deleted complex type.</span></span> <span data-ttu-id="0e6fe-205">Entity Designer를 사용하여 삭제된 복합 형식에 대한 참조를 업데이트하거나 삭제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-205">You can update or delete references to deleted complex types by using the Entity Designer.</span></span>

-   <span data-ttu-id="0e6fe-206">Model 브라우저에서 복합 형식을 마우스 오른쪽 단추로 클릭 **삭제**합니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-206">Right-click a complex type in the Model Browser and select **Delete**.</span></span>

- <span data-ttu-id="0e6fe-207">또는</span><span class="sxs-lookup"><span data-stu-id="0e6fe-207">OR -</span></span>

-   <span data-ttu-id="0e6fe-208">Model 브라우저에서 복합 형식을 선택한 다음 키보드의 Delete 키를 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-208">Select a complex type in the Model Browser and press the Delete key on your keyboard.</span></span>

## <a name="query-for-entities-containing-properties-of-complex-type"></a><span data-ttu-id="0e6fe-209">복합 형식의 속성을 포함 하는 엔터티에 대 한 쿼리</span><span class="sxs-lookup"><span data-stu-id="0e6fe-209">Query for Entities Containing Properties of Complex Type</span></span>

<span data-ttu-id="0e6fe-210">다음 코드는 엔터티 컬렉션을 복합 형식 속성을 포함 하는 형식 개체를 반환 하는 쿼리를 실행 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="0e6fe-210">The following code shows how to execute a query that returns a collection of entity type objects that contain a complex type property.</span></span>

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        var courses =
            from c in context.OnsiteCourses
            order by c.Details.Time
            select c;

        foreach (var c in courses)
        {
            Console.WriteLine("Time: " + c.Details.Time);
            Console.WriteLine("Days: " + c.Details.Days);
            Console.WriteLine("Location: " + c.Details.Location);
        }
    }
```
