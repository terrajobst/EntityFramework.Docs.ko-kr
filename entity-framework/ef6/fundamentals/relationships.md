---
title: 관계, 탐색 속성 및 EF6 외래 키
author: divega
ms.date: 2016-10-23
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: c1d48f18a7dd25a6a48537f0de5379f861bf447a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998003"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a><span data-ttu-id="f63d7-102">관계, 탐색 속성 및 외래 키</span><span class="sxs-lookup"><span data-stu-id="f63d7-102">Relationships, navigation properties and foreign keys</span></span>
<span data-ttu-id="f63d7-103">이 항목에서는 Entity Framework는 엔터티 간의 관계를 관리 하는 방법의 개요를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-103">This topic gives an overview of how Entity Framework manages relationships between entities.</span></span> <span data-ttu-id="f63d7-104">또한 매핑 관계를 조작 하는 방법에 대 한 지침을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-104">It also gives some guidance on how to map and manipulate relationships.</span></span>

## <a name="relationships-in-ef"></a><span data-ttu-id="f63d7-105">EF의 관계</span><span class="sxs-lookup"><span data-stu-id="f63d7-105">Relationships in EF</span></span>

<span data-ttu-id="f63d7-106">관계형 데이터베이스에서 테이블 간의 관계 (연결이 라고도 함)는 외래 키를 통해 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-106">In relational databases, relationships (also called associations) between tables are defined through foreign keys.</span></span> <span data-ttu-id="f63d7-107">외래 키 (FK)는 열 또는 설정 하 고 두 테이블의 데이터 간 연결을 강제 적용 하는 데 사용 되는 열 조합.</span><span class="sxs-lookup"><span data-stu-id="f63d7-107">A foreign key (FK) is a column or combination of columns that is used to establish and enforce a link between the data in two tables.</span></span> <span data-ttu-id="f63d7-108">세 가지 유형의 관계는 일반적으로: 한 일, 1 대 다 및 다 대 다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-108">There are generally three types of relationships: one-to-one, one-to-many, and many-to-many.</span></span> <span data-ttu-id="f63d7-109">1 대 다 관계에서 외래 키 관계의 다 쪽에 있는 테이블에 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-109">In a one-to-many relationship, the foreign key is defined on the table that represents the many end of the relationship.</span></span> <span data-ttu-id="f63d7-110">다 대 다 관계는 두 관련된 테이블의 외래 키의 기본 키가 포함 구성 됩니다 (병합 또는 조인 테이블 이라고 함), 세 번째 테이블 정의 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-110">The many-to-many relationship involves defining a third table (called a junction or join table), whose primary key is composed of the foreign keys from both related tables.</span></span> <span data-ttu-id="f63d7-111">한 일 관계에서 기본 키 또한 외래 키로 작동 하 고 두 테이블 중 하나에 대 한 별도 외래 키 열이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-111">In a one-to-one relationship, the primary key acts additionally as a foreign key and there is no separate foreign key column for either table.</span></span>

<span data-ttu-id="f63d7-112">다음 이미지에 일 대 다 관계에 참여 하는 두 테이블을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-112">The following image shows two tables that participate in one-to-many relationship.</span></span> <span data-ttu-id="f63d7-113">**과정** 있기 때문에 테이블은 종속 테이블 합니다 **DepartmentID** 에 연결 하는 열을 **부서** 테이블.</span><span class="sxs-lookup"><span data-stu-id="f63d7-113">The **Course** table is the dependent table because it contains the **DepartmentID** column that links it to the **Department** table.</span></span>

![Database2](~/ef6/media/database2.png)

<span data-ttu-id="f63d7-115">Entity Framework의 엔터티 연결 또는 관계를 통해 다른 엔터티와 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-115">In Entity Framework, an entity can be related to other entities through an association or relationship.</span></span> <span data-ttu-id="f63d7-116">각 관계는 엔터티 형식 및 해당 관계의 두 엔터티 형식 (1, 0 또는 1 인 이상의)의 다중성에 설명 하는 두 개의 끝을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-116">Each relationship contains two ends that describe the entity type and the multiplicity of the type (one, zero-or-one, or many) for the two entities in that relationship.</span></span> <span data-ttu-id="f63d7-117">관계는 주 역할 관계의 끝을 설명 하는 참조 제약 조건에 의해 관리 되기도 종속 역할 인지 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-117">The relationship may be governed by a referential constraint, which describes which end in the relationship is a principal role and which is a dependent role.</span></span>

<span data-ttu-id="f63d7-118">탐색 속성에는 두 엔터티 형식 간의 연결을 탐색 하는 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-118">Navigation properties provide a way to navigate an association between two entity types.</span></span> <span data-ttu-id="f63d7-119">모든 개체에는 해당 개체가 참여하는 모든 관계에 대한 탐색 속성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-119">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="f63d7-120">탐색 속성을 사용 하면 탐색 하 고 참조 개체를 반환 합니다. 양방향에서 관계 관리 (복합성이 중 하나인 경우 또는 0 또는 1) 또는 컬렉션 (복합성이 많은) 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-120">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="f63d7-121">수도 있습니다 단방향 탐색 하는 경우에 탐색 속성 관계에 참여 하는 형식 중 하나 에서만 및 정의한 둘 다에 없는 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-121">You may also choose to have one-way navigation, in which case you define the navigation property on only one of the types that participates in the relationship and not on both.</span></span>

<span data-ttu-id="f63d7-122">데이터베이스의 외래 키에 매핑되는 모델의 속성을 포함 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-122">It is recommended to include properties in the model that map to foreign keys in the database.</span></span> <span data-ttu-id="f63d7-123">외래 키 속성이 포함되어 있으면 종속 개체에서 외래 키 값을 수정하여 관계를 만들거나 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-123">With foreign key properties included, you can create or change a relationship by modifying the foreign key value on a dependent object.</span></span> <span data-ttu-id="f63d7-124">이러한 연결 종류를 외래 키 연결이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-124">This kind of association is called a foreign key association.</span></span> <span data-ttu-id="f63d7-125">외래 키를 사용 하 여 연결이 끊어진된 엔터티를 사용 하 여 작업 하는 경우 훨씬 더 반드시 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-125">Using foreign keys is even more essential when working with disconnected entities.</span></span> <span data-ttu-id="f63d7-126">참고로 때 1-0 또는 1-1을 사용 하 여 작업... 1 관계를 별도 외래 키 열이 없습니다 하 고 기본 키 속성의 외래 키로 모델에 항상 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-126">Note, that when working with 1-to-1 or 1-to-0..1 relationships, there is no separate foreign key column, the primary key property acts as the foreign key and is always included in the model.</span></span>

<span data-ttu-id="f63d7-127">모델에 외래 키 열 포함 되지 않습니다, 경우 연결 정보는 독립적 개체로 관리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-127">When foreign key columns are not included in the model, the association information is managed as an independent object.</span></span> <span data-ttu-id="f63d7-128">관계는 외래 키 속성 대신 개체 참조를 통해 추적 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-128">Relationships are tracked through object references instead of foreign key properties.</span></span> <span data-ttu-id="f63d7-129">이 유형의 연결 호출 되는 *독립 연결*합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-129">This type of association is called an *independent association*.</span></span> <span data-ttu-id="f63d7-130">수정 하는 가장 일반적인 방법은 *독립 연결* 연결에 참여 하는 각 엔터티에 대해 생성 되는 탐색 속성을 수정 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-130">The most common way to modify an *independent association* is to modify the navigation properties that are generated for each entity that participates in the association.</span></span>

<span data-ttu-id="f63d7-131">모델에서 두 종류의 연결 중 하나 또는 둘 모두를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-131">You can choose to use one or both types of associations in your model.</span></span> <span data-ttu-id="f63d7-132">그러나 순수 다 대 다 관계에서 외래 키만 포함 하는 조인 테이블이 연결 된 경우는 EF는 이러한 다 대 다 관계를 관리 하는 독립 연결을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-132">However, if you have a pure many-to-many relationship that is connected by a join table that contains only foreign keys, the EF will use an independent association to manage such many-to-many relationship.</span></span>   

<span data-ttu-id="f63d7-133">다음 이미지에서는 Entity Framework 디자이너를 사용 하 여 생성 된 개념적 모델을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-133">The following image shows a conceptual model that was created with the Entity Framework Designer.</span></span> <span data-ttu-id="f63d7-134">모델에 일 대 다 관계에 참여 하는 두 개의 엔터티가 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-134">The model contains two entities that participate in one-to-many relationship.</span></span> <span data-ttu-id="f63d7-135">두 엔터티는 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-135">Both entities have navigation properties.</span></span> <span data-ttu-id="f63d7-136">**코스** 종속 엔터티가 있고 합니다 **DepartmentID** 외래 키 속성을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-136">**Course** is the depend entity and has the **DepartmentID** foreign key property defined.</span></span>

![RelationshipEFDesigner](~/ef6/media/relationshipefdesigner.png)

<span data-ttu-id="f63d7-138">다음 코드 조각은 Code First를 사용 하 여 만든 동일한 모델을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-138">The following code snippet shows the same model that was created with Code First.</span></span>

``` csharp
public class Course
{
  public int CourseID { get; set; }
  public string Title { get; set; }
  public int Credits { get; set; }
  public int DepartmentID { get; set; }
  public virtual Department Department { get; set; }
}

public class Department
{
   public Department()
   {
     this.Course = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a><span data-ttu-id="f63d7-139">구성 또는 관계 매핑</span><span class="sxs-lookup"><span data-stu-id="f63d7-139">Configuring or mapping relationships</span></span>

<span data-ttu-id="f63d7-140">이 페이지의 나머지 부분에서는 관계를 사용 하 여 데이터 액세스 및 조작 하는 방법을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-140">The rest of this page covers how to access and manipulate data using relationships.</span></span> <span data-ttu-id="f63d7-141">모델의 관계 설정에 대 한 내용은 다음 페이지를 참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-141">For information on setting up relationships in your model, see the following pages.</span></span>

-   <span data-ttu-id="f63d7-142">Code First에서 관계를 구성 하려면 [데이터 주석](~/ef6/modeling/code-first/data-annotations.md) 하 고 [Fluent API-관계](~/ef6/modeling/code-first/fluent/relationships.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-142">To configure relationships in Code First, see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API – Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>
-   <span data-ttu-id="f63d7-143">Entity Framework 디자이너를 사용 하 여 관계를 구성 하려면 참조 [EF 디자이너를 사용 하 여 관계](~/ef6/modeling/designer/relationships.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-143">To configure relationships using the Entity Framework Designer, see [Relationships with the EF Designer](~/ef6/modeling/designer/relationships.md).</span></span>

## <a name="creating-and-modifying-relationships"></a><span data-ttu-id="f63d7-144">관계 만들기 및 수정</span><span class="sxs-lookup"><span data-stu-id="f63d7-144">Creating and modifying relationships</span></span>

<span data-ttu-id="f63d7-145">에 *외래 키 연결*관계를 상태가 EntityState.Modified로 변경 하는 EntityState.Unchanged 종속 개체의 상태를 변경 하는 경우.</span><span class="sxs-lookup"><span data-stu-id="f63d7-145">In a *foreign key association*, when you change the relationship, the state of a dependent object with an EntityState.Unchanged state changes to EntityState.Modified.</span></span> <span data-ttu-id="f63d7-146">독립적 관계에 관계 변경는 종속 개체의 상태를 업데이트 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-146">In an independent relationship, changing the relationship does not update the state of the dependent object.</span></span>

<span data-ttu-id="f63d7-147">다음 예제에서는 관련된 개체를 연결 하는 외래 키 속성과 탐색 속성을 사용 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-147">The following examples show how to use the foreign key properties and navigation properties to associate the related objects.</span></span> <span data-ttu-id="f63d7-148">외래 키 연결을 사용 하 여 변경, 생성 또는 관계를 수정 하려면 두 방법 중 하나를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-148">With foreign key associations, you can use either method to change, create, or modify relationships.</span></span> <span data-ttu-id="f63d7-149">독립 연결을 사용할 경우에는 외래 키 속성을 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-149">With independent associations, you cannot use the foreign key property.</span></span>

-   <span data-ttu-id="f63d7-150">다음 예제와 같이 외래 키 속성에 새 값을 할당 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-150">By assigning a new value to a foreign key property, as in the following example.</span></span>  
    ``` csharp
    course.DepartmentID = newCourse.DepartmentID;
    ```

-   <span data-ttu-id="f63d7-151">다음 코드는 외래 키를 설정 하 여 관계를 제거 **null**합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-151">The following code removes a relationship by setting the foreign key to **null**.</span></span> <span data-ttu-id="f63d7-152">참고, 외래 키 속성이 null을 허용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-152">Note, that the foreign key property must be nullable.</span></span>  
    ``` csharp
    course.DepartmentID = null;
    ```  
    >[!NOTE]
    > <span data-ttu-id="f63d7-153">(이 예에서는 과정 개체)에 추가 된 상태에 대 한 참조 인 경우 참조 탐색 속성이 동기화 되지 않습니다는 새 개체의 키 값을 사용 하 여 SaveChanges가 호출 될 때까지 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-153">If the reference is in the added state (in this example, the course object), the reference navigation property will not be synchronized with the key values of a new object until SaveChanges is called.</span></span> <span data-ttu-id="f63d7-154">개체 컨텍스트에는 추가된 개체에 대한 영구 키가 포함되어 있지 않으므로 해당 개체가 저장되기 전까지는 동기화가 수행되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-154">Synchronization does not occur because the object context does not contain permanent keys for added objects until they are saved.</span></span> <span data-ttu-id="f63d7-155">관계를 설정 하는 즉시 완전 하 게 동기화 하는 새 개체를 해야 하는 경우 다음 methods.\* 중 하나 사용</span><span class="sxs-lookup"><span data-stu-id="f63d7-155">If you must have new objects fully synchronized as soon as you set the relationship, use one of the following methods.\*</span></span>

-   <span data-ttu-id="f63d7-156">탐색 속성에 새 개체를 할당합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-156">By assigning a new object to a navigation property.</span></span> <span data-ttu-id="f63d7-157">다음 코드는 과정을 사이의 관계를 만듭니다 및 `department`합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-157">The following code creates a relationship between a course and a `department`.</span></span> <span data-ttu-id="f63d7-158">개체 컨텍스트에 연결 하는 경우는 `course` 에 추가 됩니다는 `department.Courses` 컬렉션 및 해당 외래 키 속성에는 `course` 개체의 키 속성 값을로 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-158">If the objects are attached to the context, the `course` is also added to the `department.Courses` collection, and the corresponding foreign key property on the `course` object is set to the key property value of the department.</span></span>  
    ``` csharp
    course.Department = department;
    ```

 -   <span data-ttu-id="f63d7-159">관계를 삭제 하려면 탐색 속성을 설정 `null`합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-159">To delete the relationship, set the navigation property to `null`.</span></span> <span data-ttu-id="f63d7-160">.NET 4.0을 기반으로 하는 Entity Framework를 사용 하 여 작업 하는 경우 관련된 end를 null로 설정 하기 전에 로드 되도록 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-160">If you are working with Entity Framework that is based on .NET 4.0, then the related end needs to be loaded before you set it to null.</span></span> <span data-ttu-id="f63d7-161">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="f63d7-161">For example:</span></span>  
    ``` chsarp
    context.Entry(course).Reference(c => c.Department).Load();  
    course.Department = null;
    ```  
    <span data-ttu-id="f63d7-162">.NET 4.5를 기반으로 하는 Entity Framework 5.0부터 관련된 end를 로드 하지 않고 null로 관계를 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-162">Starting with Entity Framework 5.0, that is based on .NET 4.5, you can set the relationship to null without loading the related end.</span></span> <span data-ttu-id="f63d7-163">또한 다음 메서드를 사용 하 여 null로 현재 값을 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-163">You can also set the current value to null using the following method.</span></span>  
    ``` csharp
    context.Entry(course).Reference(c => c.Department).CurrentValue = null;
    ```

-   <span data-ttu-id="f63d7-164">엔터티 컬렉션에서 개체를 삭제하거나 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-164">By deleting or adding an object in an entity collection.</span></span> <span data-ttu-id="f63d7-165">예를 들어 형식의 개체를 추가할 수 있습니다 `Course` 에 `department.Courses` 컬렉션입니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-165">For example, you can add an object of type `Course` to the `department.Courses` collection.</span></span> <span data-ttu-id="f63d7-166">이 작업은 특정 간의 관계를 만듭니다 **코스** 및 특정 `department`합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-166">This operation creates a relationship between a particular **course** and a particular `department`.</span></span> <span data-ttu-id="f63d7-167">개체 컨텍스트, 부서 참조 및 외래 키 속성에 연결 되어 있으면 합니다 **코스** 적절 한 개체를 설정할 `department`합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-167">If the objects are attached to the context, the department reference and the foreign key property on the **course** object will be set to the appropriate `department`.</span></span>  
    ``` csharp
    department.Courses.Add(newCourse);
    ```

- <span data-ttu-id="f63d7-168">사용 하 여는 `ChangeRelationshipState` 두 엔터티 개체 간 관계의 상태를 변경 하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-168">By using the `ChangeRelationshipState` method to change the state of the specified relationship between two entity objects.</span></span> <span data-ttu-id="f63d7-169">N 계층 응용 프로그램을 사용 하 여 작업 하는 경우에 가장 일반적으로이 메서드는 및 *독립 연결* (사용할 수 없습니다는 외래 키 연결을 사용 하 여).</span><span class="sxs-lookup"><span data-stu-id="f63d7-169">This method is most commonly used when working with N-Tier applications and an *independent association* (it cannot be used with a foreign key association).</span></span> <span data-ttu-id="f63d7-170">또한이 방법을 사용 하려면 삭제 해야 합니다 아래로 `ObjectContext`아래 예제에 나와 있는 것 처럼 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-170">Also, to use this method you must drop down to `ObjectContext`, as shown in the example below.</span></span>  
<span data-ttu-id="f63d7-171">다음 예제에서는 강사 및 강좌 다 대 다 관계가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-171">In the following example, there is a many-to-many relationship between Instructors and Courses.</span></span> <span data-ttu-id="f63d7-172">호출을 `ChangeRelationshipState` 메서드와 전달을 `EntityState.Added` 매개 변수 수는 `SchoolContext` 두 개체 간의 관계 추가 된 것을 알고:</span><span class="sxs-lookup"><span data-stu-id="f63d7-172">Calling the `ChangeRelationshipState` method and passing the `EntityState.Added` parameter, lets the `SchoolContext` know that a relationship has been added between the two objects:</span></span>

``` csharp

       ((IObjectContextAdapter)context).ObjectContext.
                 ObjectStateManager.
                  ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
```

    Note that if you are updating (not just adding) a relationship, you must delete the old relationship after adding the new one:

``` csharp
       ((IObjectContextAdapter)context).ObjectContext.
                  ObjectStateManager.
                  ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a><span data-ttu-id="f63d7-173">외래 키 속성과 탐색 간에 변경 내용 동기화</span><span class="sxs-lookup"><span data-stu-id="f63d7-173">Synchronizing the changes between the foreign keys and navigation properties</span></span>

<span data-ttu-id="f63d7-174">위에서 설명한 방법 중 하나를 사용 하 여 컨텍스트에 연결 된 개체의 관계를 변경 하면 Entity Framework에서는 외래 키, 참조 및 컬렉션에 동기화를 유지 해야 합니다. Entity Framework는 자동으로 프록시를 사용 하 여 POCO 엔터티에 대 한 (라고도 관계 픽스업)이이 동기화를 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-174">When you change the relationship of the objects attached to the context by using one of the methods described above, Entity Framework needs to keep foreign keys, references, and collections in sync. Entity Framework automatically manages this synchronization (also known as relationship fix-up) for the POCO entities with proxies.</span></span> <span data-ttu-id="f63d7-175">자세한 내용은 [프록시를 사용 하 여 작업](~/ef6/fundamentals/proxies.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-175">For more information, see [Working with Proxies](~/ef6/fundamentals/proxies.md).</span></span>

<span data-ttu-id="f63d7-176">프록시가 없는 POCO 엔터티를 사용 하는 경우 해야 하는 **DetectChanges** 메서드를 호출 하는 컨텍스트에서 관련된 개체를 동기화 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-176">If you are using POCO entities without proxies, you must make sure that the **DetectChanges** method is called to synchronize the related objects in the context.</span></span> <span data-ttu-id="f63d7-177">이때 다음과 같은 Api를 자동으로 트리거하는 **DetectChanges** 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-177">Note, that the following APIs automatically trigger a **DetectChanges** call.</span></span>

-   `DbSet.Add`
-   `DbSet.AddRange`
-   `DbSet.Remove`
-   `DbSet.RemoveRange`
-   `DbSet.Find`
-   `DbSet.Local`
-   `DbContext.SaveChanges`
-   `DbSet.Attach`
-   `DbContext.GetValidationErrors`
-   `DbContext.Entry`
-   `DbChangeTracker.Entries`
-   <span data-ttu-id="f63d7-178">에 대 한 쿼리는 LINQ를 실행 한 `DbSet`</span><span class="sxs-lookup"><span data-stu-id="f63d7-178">Executing a LINQ query against a `DbSet`</span></span>

## <a name="loading-related-objects"></a><span data-ttu-id="f63d7-179">관련 개체 로드</span><span class="sxs-lookup"><span data-stu-id="f63d7-179">Loading related objects</span></span>

<span data-ttu-id="f63d7-180">가장 일반적으로 사용 하면 Entity Framework에서 탐색 속성을 사용 하 여 정의 된 연결에 의해 반환된 된 엔터티를 관련 된 엔터티를 로드 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-180">In Entity Framework you use most commonly use the navigation properties to load entities that are related to the returned entity by the defined association.</span></span> <span data-ttu-id="f63d7-181">자세한 내용은 [관련 개체 로드](~/ef6/querying/related-data.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-181">For more information, see [Loading Related Objects](~/ef6/querying/related-data.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f63d7-182">외래 키 연결에서 종속 개체의 관련된 끝을 로드하면 현재 메모리에 있는 종속 개체의 외래 키 값에 따라 관련 개체가 로드됩니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-182">In a foreign key association, when you load a related end of a dependent object, the related object will be loaded based on the foreign key value of the dependent that is currently in memory:</span></span>

``` csharp
    // Get the course where currently DepartmentID = 1.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

<span data-ttu-id="f63d7-183">독립 연결에서는 현재 데이터베이스에 있는 외래 키 값에 따라 종속 개체의 관련된 끝이 쿼리됩니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-183">In an independent association, the related end of a dependent object is queried based on the foreign key value that is currently in the database.</span></span> <span data-ttu-id="f63d7-184">그러나 관계가 수정 되었고 및 Entity Framework 개체 컨텍스트에 로드 되는 다른 보안 주체 개체에 종속 개체 요소에 대해 참조 속성으로 관계를 만들려고 시도 하는 경우 클라이언트에서 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-184">However, if the relationship was modified, and the reference property on the dependent object points to a different principal object that is loaded in the object context, Entity Framework will try to create a relationship as it is defined on the client.</span></span>

## <a name="managing-concurrency"></a><span data-ttu-id="f63d7-185">동시성 관리</span><span class="sxs-lookup"><span data-stu-id="f63d7-185">Managing concurrency</span></span>

<span data-ttu-id="f63d7-186">외래 키 연결과 독립 연결 모두에서 동시성 검사는 엔터티 키 및 모델에 정의 된 다른 엔터티 속성을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-186">In both foreign key and independent associations, concurrency checks are based on the entity keys and other entity properties that are defined in the model.</span></span> <span data-ttu-id="f63d7-187">EF 디자이너를 사용 하 여 모델 만들기를 설정 합니다 `ConcurrencyMode` 특성을 **고정** 동시성 속성을 검사 해야 않도록 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-187">When using the EF Designer to create a model, set the `ConcurrencyMode` attribute to **fixed** to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="f63d7-188">사용 하 여 Code First 모델 정의 사용 하 여는 `ConcurrencyCheck` 속성을 검사할 동시성에 대 한 주석입니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-188">When using Code First to define a model, use the `ConcurrencyCheck` annotation on properties that you want to be checked for concurrency.</span></span> <span data-ttu-id="f63d7-189">Code First를 사용 하 여 작업할 때 사용할 수도 있습니다는 `TimeStamp` 주석 동시성에 대 한 속성을 검사 해야 않도록 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-189">When working with Code First you can also use the `TimeStamp` annotation to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="f63d7-190">지정된 된 클래스의 타임 스탬프 속성을 하나만 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-190">You can have only one timestamp property in a given class.</span></span> <span data-ttu-id="f63d7-191">코드는 먼저 데이터베이스에서 null이 아닌 필드에이 속성을 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-191">Code First maps this property to a non-nullable field in the database.</span></span>

<span data-ttu-id="f63d7-192">동시성 검사 및 확인에 참여 하는 엔터티를 사용 하 여 작업할 때 외래 키 연결을 항상 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-192">We recommend that you always use the foreign key association when working with entities that participate in concurrency checking and resolution.</span></span>

<span data-ttu-id="f63d7-193">자세한 내용은 [동시성 충돌 처리](~/ef6/saving/concurrency.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-193">For more information, see [Handling Concurrency Conflicts](~/ef6/saving/concurrency.md).</span></span>

## <a name="working-with-overlapping-keys"></a><span data-ttu-id="f63d7-194">겹치는 키 사용</span><span class="sxs-lookup"><span data-stu-id="f63d7-194">Working with overlapping Keys</span></span>

<span data-ttu-id="f63d7-195">겹치는 키는 키의 일부 속성이 엔터티에 있는 다른 키의 일부이기도 한 복합 키입니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-195">Overlapping keys are composite keys where some properties in the key are also part of another key in the entity.</span></span> <span data-ttu-id="f63d7-196">독립 연결에서는 겹치는 키를 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-196">You cannot have an overlapping key in an independent association.</span></span> <span data-ttu-id="f63d7-197">겹치는 키가 포함된 외래 키 연결을 변경하려면 개체 참조를 사용하는 대신 외래 키 값을 수정하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="f63d7-197">To change a foreign key association that includes overlapping keys, we recommend that you modify the foreign key values instead of using the object references.</span></span>
