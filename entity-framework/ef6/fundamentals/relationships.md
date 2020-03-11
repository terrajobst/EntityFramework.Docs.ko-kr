---
title: 관계, 탐색 속성 및 외래 키-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: 892e872e3cb11ea95084cf6d9ab43c8d500bc0de
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416002"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a><span data-ttu-id="9fc3a-102">관계, 탐색 속성 및 외래 키</span><span class="sxs-lookup"><span data-stu-id="9fc3a-102">Relationships, navigation properties, and foreign keys</span></span>

<span data-ttu-id="9fc3a-103">이 문서에서는 Entity Framework 엔터티 간의 관계를 관리 하는 방법에 대 한 개요를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-103">This article gives an overview of how Entity Framework manages relationships between entities.</span></span> <span data-ttu-id="9fc3a-104">또한 관계를 매핑하고 조작 하는 방법에 대 한 몇 가지 지침을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-104">It also gives some guidance on how to map and manipulate relationships.</span></span>

## <a name="relationships-in-ef"></a><span data-ttu-id="9fc3a-105">EF의 관계</span><span class="sxs-lookup"><span data-stu-id="9fc3a-105">Relationships in EF</span></span>

<span data-ttu-id="9fc3a-106">관계형 데이터베이스에서 테이블 간의 관계 (연결 라고도 함)는 외래 키를 통해 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-106">In relational databases, relationships (also called associations) between tables are defined through foreign keys.</span></span> <span data-ttu-id="9fc3a-107">외래 키(FK)는 두 테이블의 데이터 간 연결을 설정하고 강제 적용하는 데 사용되는 열입니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-107">A foreign key (FK) is a column or combination of columns that is used to establish and enforce a link between the data in two tables.</span></span> <span data-ttu-id="9fc3a-108">일반적으로 일 대 일, 일 대 다 및 다 대 다와 같은 세 가지 유형의 관계가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-108">There are generally three types of relationships: one-to-one, one-to-many, and many-to-many.</span></span> <span data-ttu-id="9fc3a-109">일 대 다 관계에서 외래 키는 관계의 다 쪽을 나타내는 테이블에 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-109">In a one-to-many relationship, the foreign key is defined on the table that represents the many end of the relationship.</span></span> <span data-ttu-id="9fc3a-110">다 대 다 관계에는 기본 키가 두 관련 테이블의 외래 키로 구성 된 세 번째 테이블 (접합 또는 조인 테이블 이라고 함)을 정의 하는 작업이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-110">The many-to-many relationship involves defining a third table (called a junction or join table), whose primary key is composed of the foreign keys from both related tables.</span></span> <span data-ttu-id="9fc3a-111">일 대 일 관계에서는 기본 키가 외래 키로도 적용 되 고 두 테이블에 대 한 별도의 외래 키 열이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-111">In a one-to-one relationship, the primary key acts additionally as a foreign key and there is no separate foreign key column for either table.</span></span>

<span data-ttu-id="9fc3a-112">다음 이미지는 일 대 다 관계에 참여 하는 두 테이블을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-112">The following image shows two tables that participate in one-to-many relationship.</span></span> <span data-ttu-id="9fc3a-113">**과정** 테이블은 **부서** 테이블에 연결 하는 **DepartmentID** 열이 포함 되어 있으므로 종속 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-113">The **Course** table is the dependent table because it contains the **DepartmentID** column that links it to the **Department** table.</span></span>

![부서 및 과정 테이블](~/ef6/media/database2.png)

<span data-ttu-id="9fc3a-115">Entity Framework에서 엔터티는 연결 또는 관계를 통해 다른 엔터티와 관련 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-115">In Entity Framework, an entity can be related to other entities through an association or relationship.</span></span> <span data-ttu-id="9fc3a-116">각 관계에는 엔터티 형식을 설명 하는 두 개의 end와 해당 관계에 있는 두 엔터티에 대 한 형식 (1, 0 또는 그 다)의 복합성이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-116">Each relationship contains two ends that describe the entity type and the multiplicity of the type (one, zero-or-one, or many) for the two entities in that relationship.</span></span> <span data-ttu-id="9fc3a-117">관계는 주 역할이 며 종속 역할인 관계의 end를 설명 하는 참조 제약 조건에 의해 제어 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-117">The relationship may be governed by a referential constraint, which describes which end in the relationship is a principal role and which is a dependent role.</span></span>

<span data-ttu-id="9fc3a-118">탐색 속성은 두 엔터티 형식 간의 연결을 탐색 하는 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-118">Navigation properties provide a way to navigate an association between two entity types.</span></span> <span data-ttu-id="9fc3a-119">모든 개체에는 해당 개체가 참여하는 모든 관계에 대한 탐색 속성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-119">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="9fc3a-120">탐색 속성을 사용 하면 두 방향에서 관계를 탐색 하 고 관리할 수 있습니다. 참조 개체 (복합성이 하나 또는 0 또는 1)를 반환 하거나 복합성이 많은 경우 컬렉션을 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-120">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="9fc3a-121">단방향 탐색을 선택할 수도 있습니다 .이 경우 관계에 참여 하는 형식 중 하나에만 탐색 속성을 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-121">You may also choose to have one-way navigation, in which case you define the navigation property on only one of the types that participates in the relationship and not on both.</span></span>

<span data-ttu-id="9fc3a-122">모델에 데이터베이스의 외래 키에 매핑되는 속성을 포함 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-122">It is recommended to include properties in the model that map to foreign keys in the database.</span></span> <span data-ttu-id="9fc3a-123">외래 키 속성이 포함되어 있으면 종속 개체에서 외래 키 값을 수정하여 관계를 만들거나 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-123">With foreign key properties included, you can create or change a relationship by modifying the foreign key value on a dependent object.</span></span> <span data-ttu-id="9fc3a-124">이러한 연결 종류를 외래 키 연결이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-124">This kind of association is called a foreign key association.</span></span> <span data-ttu-id="9fc3a-125">외래 키를 사용 하는 것은 연결이 끊어진 엔터티로 작업할 때 훨씬 더 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-125">Using foreign keys is even more essential when working with disconnected entities.</span></span> <span data-ttu-id="9fc3a-126">1-1 또는 1-0으로 작업 하는 경우입니다. 1 관계는 별도의 외래 키 열이 없으며 기본 키 속성은 외래 키로 작동 하며 항상 모델에 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-126">Note, that when working with 1-to-1 or 1-to-0..1 relationships, there is no separate foreign key column, the primary key property acts as the foreign key and is always included in the model.</span></span>

<span data-ttu-id="9fc3a-127">모델에 외래 키 열이 포함 되지 않은 경우 연결 정보는 독립 개체로 관리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-127">When foreign key columns are not included in the model, the association information is managed as an independent object.</span></span> <span data-ttu-id="9fc3a-128">관계는 외래 키 속성 대신 개체 참조를 통해 추적 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-128">Relationships are tracked through object references instead of foreign key properties.</span></span> <span data-ttu-id="9fc3a-129">이러한 유형의 연결을 *독립 연결*이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-129">This type of association is called an *independent association*.</span></span> <span data-ttu-id="9fc3a-130">*독립적인 연결* 을 수정 하는 가장 일반적인 방법은 연결에 참여 하는 각 엔터티에 대해 생성 되는 탐색 속성을 수정 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-130">The most common way to modify an *independent association* is to modify the navigation properties that are generated for each entity that participates in the association.</span></span>

<span data-ttu-id="9fc3a-131">모델에서 두 종류의 연결 중 하나 또는 둘 모두를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-131">You can choose to use one or both types of associations in your model.</span></span> <span data-ttu-id="9fc3a-132">그러나 외래 키만 포함 하는 조인 테이블로 연결 되는 순수한 다대다 관계가 있는 경우 EF는 독립 연결을 사용 하 여 이러한 다대다 관계를 관리 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-132">However, if you have a pure many-to-many relationship that is connected by a join table that contains only foreign keys, the EF will use an independent association to manage such many-to-many relationship.</span></span>   

<span data-ttu-id="9fc3a-133">다음 그림은 Entity Framework Designer를 사용 하 여 만든 개념적 모델을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-133">The following image shows a conceptual model that was created with the Entity Framework Designer.</span></span> <span data-ttu-id="9fc3a-134">모델은 일 대 다 관계에 참여 하는 두 개의 엔터티를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-134">The model contains two entities that participate in one-to-many relationship.</span></span> <span data-ttu-id="9fc3a-135">두 엔터티 모두 탐색 속성을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-135">Both entities have navigation properties.</span></span> <span data-ttu-id="9fc3a-136">**강좌** 는 종속 엔터티 이며 **DepartmentID** 외래 키 속성이 정의 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-136">**Course** is the dependent entity and has the **DepartmentID** foreign key property defined.</span></span>

![탐색 속성을 사용 하는 부서 및 과정 테이블](~/ef6/media/relationshipefdesigner.png)

<span data-ttu-id="9fc3a-138">다음 코드 조각에서는 Code First를 사용 하 여 만든 것과 동일한 모델을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-138">The following code snippet shows the same model that was created with Code First.</span></span>

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
     this.Courses = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a><span data-ttu-id="9fc3a-139">관계 구성 또는 매핑</span><span class="sxs-lookup"><span data-stu-id="9fc3a-139">Configuring or mapping relationships</span></span>

<span data-ttu-id="9fc3a-140">이 페이지의 나머지 부분에서는 관계를 사용 하 여 데이터에 액세스 하 고 조작 하는 방법을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-140">The rest of this page covers how to access and manipulate data using relationships.</span></span> <span data-ttu-id="9fc3a-141">모델에서 관계를 설정 하는 방법에 대 한 자세한 내용은 다음 페이지를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-141">For information on setting up relationships in your model, see the following pages.</span></span>

-   <span data-ttu-id="9fc3a-142">Code First에서 관계를 구성 하려면 [데이터 주석](~/ef6/modeling/code-first/data-annotations.md) 및 [흐름 API-관계](~/ef6/modeling/code-first/fluent/relationships.md)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-142">To configure relationships in Code First, see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API – Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>
-   <span data-ttu-id="9fc3a-143">Entity Framework Designer를 사용 하 여 관계를 구성 하려면 [EF Designer와의 관계](~/ef6/modeling/designer/relationships.md)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-143">To configure relationships using the Entity Framework Designer, see [Relationships with the EF Designer](~/ef6/modeling/designer/relationships.md).</span></span>

## <a name="creating-and-modifying-relationships"></a><span data-ttu-id="9fc3a-144">관계 만들기 및 수정</span><span class="sxs-lookup"><span data-stu-id="9fc3a-144">Creating and modifying relationships</span></span>

<span data-ttu-id="9fc3a-145">*외래 키 연결*에서 관계를 변경 하면 `EntityState.Unchanged` 상태의 종속 개체 상태가 `EntityState.Modified`변경 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-145">In a *foreign key association*, when you change the relationship, the state of a dependent object with an `EntityState.Unchanged` state changes to `EntityState.Modified`.</span></span> <span data-ttu-id="9fc3a-146">독립적인 관계에서 관계를 변경 해도 종속 개체의 상태는 업데이트 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-146">In an independent relationship, changing the relationship does not update the state of the dependent object.</span></span>

<span data-ttu-id="9fc3a-147">다음 예에서는 외래 키 속성 및 탐색 속성을 사용 하 여 관련 개체를 연결 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-147">The following examples show how to use the foreign key properties and navigation properties to associate the related objects.</span></span> <span data-ttu-id="9fc3a-148">외래 키 연결을 사용 하는 방법 중 하나를 사용 하 여 관계를 변경, 생성 또는 수정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-148">With foreign key associations, you can use either method to change, create, or modify relationships.</span></span> <span data-ttu-id="9fc3a-149">독립 연결을 사용할 경우에는 외래 키 속성을 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-149">With independent associations, you cannot use the foreign key property.</span></span>

- <span data-ttu-id="9fc3a-150">다음 예제와 같이 외래 키 속성에 새 값을 할당 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-150">By assigning a new value to a foreign key property, as in the following example.</span></span>  
  ``` csharp
  course.DepartmentID = newCourse.DepartmentID;
  ```

- <span data-ttu-id="9fc3a-151">다음 코드는 외래 키를 **null**로 설정 하 여 관계를 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-151">The following code removes a relationship by setting the foreign key to **null**.</span></span> <span data-ttu-id="9fc3a-152">외래 키 속성은 null을 허용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-152">Note, that the foreign key property must be nullable.</span></span>  
  ``` csharp
  course.DepartmentID = null;
  ```

  >[!NOTE]
  > <span data-ttu-id="9fc3a-153">참조가 추가 된 상태 (이 예제에서는 강좌 개체) 인 경우에는 SaveChanges가 호출 될 때까지 참조 탐색 속성이 새 개체의 키 값과 동기화 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-153">If the reference is in the added state (in this example, the course object), the reference navigation property will not be synchronized with the key values of a new object until SaveChanges is called.</span></span> <span data-ttu-id="9fc3a-154">개체 컨텍스트에는 추가된 개체에 대한 영구 키가 포함되어 있지 않으므로 해당 개체가 저장되기 전까지는 동기화가 수행되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-154">Synchronization does not occur because the object context does not contain permanent keys for added objects until they are saved.</span></span> <span data-ttu-id="9fc3a-155">관계를 설정 하는 즉시 새 개체가 완전히 동기화 되어야 하는 경우 다음 방법 중 하나를 사용 합니다. \*</span><span class="sxs-lookup"><span data-stu-id="9fc3a-155">If you must have new objects fully synchronized as soon as you set the relationship, use one of the following methods.\*</span></span>

- <span data-ttu-id="9fc3a-156">탐색 속성에 새 개체를 할당합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-156">By assigning a new object to a navigation property.</span></span> <span data-ttu-id="9fc3a-157">다음 코드는 강좌와 `department`간의 관계를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-157">The following code creates a relationship between a course and a `department`.</span></span> <span data-ttu-id="9fc3a-158">개체를 컨텍스트에 연결 하면 `course` `department.Courses` 컬렉션에도 추가 되 고 `course` 개체의 해당 외래 키 속성이 학과의 키 속성 값으로 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-158">If the objects are attached to the context, the `course` is also added to the `department.Courses` collection, and the corresponding foreign key property on the `course` object is set to the key property value of the department.</span></span>  
  ``` csharp
  course.Department = department;
  ```

- <span data-ttu-id="9fc3a-159">관계를 삭제 하려면 탐색 속성을 `null`로 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-159">To delete the relationship, set the navigation property to `null`.</span></span> <span data-ttu-id="9fc3a-160">.NET 4.0을 기반으로 하는 Entity Framework를 사용 하는 경우에는 관련 end를 먼저 로드 해야 null로 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-160">If you are working with Entity Framework that is based on .NET 4.0, then the related end needs to be loaded before you set it to null.</span></span> <span data-ttu-id="9fc3a-161">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-161">For example:</span></span>   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).Load();
  course.Department = null;
  ```

  <span data-ttu-id="9fc3a-162">.NET 4.5을 기반으로 하는 Entity Framework 5.0부터 관련 end를 로드 하지 않고 관계를 null로 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-162">Starting with Entity Framework 5.0, that is based on .NET 4.5, you can set the relationship to null without loading the related end.</span></span> <span data-ttu-id="9fc3a-163">다음 메서드를 사용 하 여 현재 값을 null로 설정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-163">You can also set the current value to null using the following method.</span></span>   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).CurrentValue = null;
  ```

- <span data-ttu-id="9fc3a-164">엔터티 컬렉션에서 개체를 삭제하거나 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-164">By deleting or adding an object in an entity collection.</span></span> <span data-ttu-id="9fc3a-165">예를 들어 `Course` 형식의 개체를 `department.Courses` 컬렉션에 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-165">For example, you can add an object of type `Course` to the `department.Courses` collection.</span></span> <span data-ttu-id="9fc3a-166">이 작업은 특정 **과정** 및 특정 `department`간의 관계를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-166">This operation creates a relationship between a particular **course** and a particular `department`.</span></span> <span data-ttu-id="9fc3a-167">개체를 컨텍스트에 연결 하면 **과정** 개체의 부서 참조 및 외래 키 속성이 적절 한 `department`설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-167">If the objects are attached to the context, the department reference and the foreign key property on the **course** object will be set to the appropriate `department`.</span></span>  
  ``` csharp
  department.Courses.Add(newCourse);
  ```

- <span data-ttu-id="9fc3a-168">`ChangeRelationshipState` 메서드를 사용 하 여 두 엔터티 개체 간 지정 된 관계의 상태를 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-168">By using the `ChangeRelationshipState` method to change the state of the specified relationship between two entity objects.</span></span> <span data-ttu-id="9fc3a-169">이 메서드는 N 계층 응용 프로그램 및 *독립 연결* 을 사용할 때 가장 일반적으로 사용 됩니다 (외래 키 연결에는 사용할 수 없음).</span><span class="sxs-lookup"><span data-stu-id="9fc3a-169">This method is most commonly used when working with N-Tier applications and an *independent association* (it cannot be used with a foreign key association).</span></span> <span data-ttu-id="9fc3a-170">또한이 방법을 사용 하려면 아래 예제와 같이 `ObjectContext`에 드롭다운 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-170">Also, to use this method you must drop down to `ObjectContext`, as shown in the example below.</span></span>  
<span data-ttu-id="9fc3a-171">다음 예제에는 강사와 강의 사이에 다 대 다 관계가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-171">In the following example, there is a many-to-many relationship between Instructors and Courses.</span></span> <span data-ttu-id="9fc3a-172">`ChangeRelationshipState` 메서드를 호출 하 고 `EntityState.Added` 매개 변수를 전달 하면에서 두 개체 사이에 관계가 추가 되었음을 알 수 `SchoolContext` 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-172">Calling the `ChangeRelationshipState` method and passing the `EntityState.Added` parameter, lets the `SchoolContext` know that a relationship has been added between the two objects:</span></span>
  ``` csharp

  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
  ```

  <span data-ttu-id="9fc3a-173">관계를 추가 하는 것이 아니라 업데이트 하는 경우에는 새 관계를 추가한 후 이전 관계를 삭제 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-173">Note that if you are updating (not just adding) a relationship, you must delete the old relationship after adding the new one:</span></span>

  ``` csharp
  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
  ```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a><span data-ttu-id="9fc3a-174">외래 키와 탐색 속성 간의 변경 내용 동기화</span><span class="sxs-lookup"><span data-stu-id="9fc3a-174">Synchronizing the changes between the foreign keys and navigation properties</span></span>

<span data-ttu-id="9fc3a-175">위에서 설명한 방법 중 하나를 사용 하 여 컨텍스트에 연결 된 개체의 관계를 변경 하는 경우 Entity Framework 외래 키, 참조 및 컬렉션을 동기화 상태로 유지 해야 합니다. Entity Framework는 프록시가 있는 POCO 엔터티에 대해이 동기화 (관계 수정이 라고도 함)를 자동으로 관리 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-175">When you change the relationship of the objects attached to the context by using one of the methods described above, Entity Framework needs to keep foreign keys, references, and collections in sync. Entity Framework automatically manages this synchronization (also known as relationship fix-up) for the POCO entities with proxies.</span></span> <span data-ttu-id="9fc3a-176">자세한 내용은 [프록시 작업](~/ef6/fundamentals/proxies.md)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-176">For more information, see [Working with Proxies](~/ef6/fundamentals/proxies.md).</span></span>

<span data-ttu-id="9fc3a-177">프록시가 없는 POCO 엔터티를 사용 하는 경우 컨텍스트의 관련 개체를 동기화 하기 위해 **DetectChanges** 메서드가 호출 되는지 확인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-177">If you are using POCO entities without proxies, you must make sure that the **DetectChanges** method is called to synchronize the related objects in the context.</span></span> <span data-ttu-id="9fc3a-178">다음 Api는 **DetectChanges** 호출을 자동으로 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-178">Note, that the following APIs automatically trigger a **DetectChanges** call.</span></span>

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
-   <span data-ttu-id="9fc3a-179">`DbSet`에 대해 LINQ 쿼리 실행</span><span class="sxs-lookup"><span data-stu-id="9fc3a-179">Executing a LINQ query against a `DbSet`</span></span>

## <a name="loading-related-objects"></a><span data-ttu-id="9fc3a-180">관련 개체 로드</span><span class="sxs-lookup"><span data-stu-id="9fc3a-180">Loading related objects</span></span>

<span data-ttu-id="9fc3a-181">Entity Framework에서는 일반적으로 탐색 속성을 사용 하 여 정의 된 연결에 의해 반환 된 엔터티와 관련 된 엔터티를 로드 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-181">In Entity Framework you commonly use navigation properties to load entities that are related to the returned entity by the defined association.</span></span> <span data-ttu-id="9fc3a-182">자세한 내용은 [관련 개체 로드](~/ef6/querying/related-data.md)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-182">For more information, see [Loading Related Objects](~/ef6/querying/related-data.md).</span></span>

> [!NOTE]
> <span data-ttu-id="9fc3a-183">외래 키 연결에서 종속 개체의 관련된 끝을 로드하면 현재 메모리에 있는 종속 개체의 외래 키 값에 따라 관련 개체가 로드됩니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-183">In a foreign key association, when you load a related end of a dependent object, the related object will be loaded based on the foreign key value of the dependent that is currently in memory:</span></span>

``` csharp
    // Get the course where currently DepartmentID = 2.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

<span data-ttu-id="9fc3a-184">독립 연결에서는 현재 데이터베이스에 있는 외래 키 값에 따라 종속 개체의 관련된 끝이 쿼리됩니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-184">In an independent association, the related end of a dependent object is queried based on the foreign key value that is currently in the database.</span></span> <span data-ttu-id="9fc3a-185">그러나 관계가 수정 되었으며 종속 개체에 대 한 참조 속성이 개체 컨텍스트에 로드 된 다른 보안 주체 개체를 가리키는 경우 Entity Framework는 클라이언트에 정의 된 대로 관계를 만들려고 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-185">However, if the relationship was modified, and the reference property on the dependent object points to a different principal object that is loaded in the object context, Entity Framework will try to create a relationship as it is defined on the client.</span></span>

## <a name="managing-concurrency"></a><span data-ttu-id="9fc3a-186">동시성 관리</span><span class="sxs-lookup"><span data-stu-id="9fc3a-186">Managing concurrency</span></span>

<span data-ttu-id="9fc3a-187">외래 키와 독립 연결 모두에서 동시성 검사는 모델에 정의 된 엔터티 키와 기타 엔터티 속성을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-187">In both foreign key and independent associations, concurrency checks are based on the entity keys and other entity properties that are defined in the model.</span></span> <span data-ttu-id="9fc3a-188">EF Designer를 사용 하 여 모델을 만드는 경우 `ConcurrencyMode` 특성을 **fixed** 로 설정 하 여 동시성에 대해 속성을 확인 하도록 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-188">When using the EF Designer to create a model, set the `ConcurrencyMode` attribute to **fixed** to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="9fc3a-189">Code First를 사용 하 여 모델을 정의 하는 경우 동시성을 확인 하려는 속성에 `ConcurrencyCheck` 주석을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-189">When using Code First to define a model, use the `ConcurrencyCheck` annotation on properties that you want to be checked for concurrency.</span></span> <span data-ttu-id="9fc3a-190">Code First 작업할 때 `TimeStamp` 주석을 사용 하 여 속성에 동시성을 확인 하도록 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-190">When working with Code First you can also use the `TimeStamp` annotation to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="9fc3a-191">지정 된 클래스에는 타임 스탬프 속성을 하나만 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-191">You can have only one timestamp property in a given class.</span></span> <span data-ttu-id="9fc3a-192">Code First는 데이터베이스의 null을 허용 하지 않는 필드에이 속성을 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-192">Code First maps this property to a non-nullable field in the database.</span></span>

<span data-ttu-id="9fc3a-193">동시성 검사 및 해결에 참여 하는 엔터티를 사용할 때는 항상 외래 키 연결을 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-193">We recommend that you always use the foreign key association when working with entities that participate in concurrency checking and resolution.</span></span>

<span data-ttu-id="9fc3a-194">자세한 내용은 [동시성 충돌 처리](~/ef6/saving/concurrency.md)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-194">For more information, see [Handling Concurrency Conflicts](~/ef6/saving/concurrency.md).</span></span>

## <a name="working-with-overlapping-keys"></a><span data-ttu-id="9fc3a-195">겹치는 키 작업</span><span class="sxs-lookup"><span data-stu-id="9fc3a-195">Working with overlapping Keys</span></span>

<span data-ttu-id="9fc3a-196">겹치는 키는 키의 일부 속성이 엔터티에 있는 다른 키의 일부이기도 한 복합 키입니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-196">Overlapping keys are composite keys where some properties in the key are also part of another key in the entity.</span></span> <span data-ttu-id="9fc3a-197">독립 연결에서는 겹치는 키를 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-197">You cannot have an overlapping key in an independent association.</span></span> <span data-ttu-id="9fc3a-198">겹치는 키가 포함된 외래 키 연결을 변경하려면 개체 참조를 사용하는 대신 외래 키 값을 수정하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="9fc3a-198">To change a foreign key association that includes overlapping keys, we recommend that you modify the foreign key values instead of using the object references.</span></span>
