---
title: 첫 번째 규칙-EF6 코드
author: divega
ms.date: 2016-10-23
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
ms.openlocfilehash: c5fa580879a4b53fed34d94b737988875f38c62c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995528"
---
# <a name="code-first-conventions"></a><span data-ttu-id="d3113-102">코드의 첫 번째 규칙</span><span class="sxs-lookup"><span data-stu-id="d3113-102">Code First Conventions</span></span>
<span data-ttu-id="d3113-103">먼저 코드를 사용 하면 C# 또는 Visual Basic.NET 클래스를 사용 하 여 모델을 설명할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-103">Code First enables you to describe a model by using C# or Visual Basic .NET classes.</span></span> <span data-ttu-id="d3113-104">모델의 기본 셰이프 규칙을 사용 하 여 검색 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-104">The basic shape of the model is detected by using conventions.</span></span> <span data-ttu-id="d3113-105">규칙은 자동으로 Code First를 사용 하 여 작업 하는 경우 클래스 정의 기반으로 개념적 모델을 구성 하는 데 사용 되는 규칙의 집합입니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-105">Conventions are sets of rules that are used to automatically configure a conceptual model based on class definitions when working with Code First.</span></span> <span data-ttu-id="d3113-106">규칙은 System.Data.Entity.ModelConfiguration.Conventions 네임 스페이스에 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-106">The conventions are defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>  

<span data-ttu-id="d3113-107">데이터 주석 또는 fluent API를 사용 하 여 모델을 추가로 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-107">You can further configure your model by using data annotations or the fluent API.</span></span> <span data-ttu-id="d3113-108">데이터 주석 및 규칙 뒤에 흐름 API 통해 구성에 우선 순위가 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-108">Precedence is given to configuration through the fluent API followed by data annotations and then conventions.</span></span> <span data-ttu-id="d3113-109">자세한 내용은 참조 하세요. [데이터 주석](~/ef6/modeling/code-first/data-annotations.md), [Fluent API-관계](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API-형식 및 속성](~/ef6/modeling/code-first/fluent/types-and-properties.md) 고 [VB.NET를사용하여FluentAPI](~/ef6/modeling/code-first/fluent/vb.md).</span><span class="sxs-lookup"><span data-stu-id="d3113-109">For more information see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - Types & Properties](~/ef6/modeling/code-first/fluent/types-and-properties.md) and [Fluent API with VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span></span>  

<span data-ttu-id="d3113-110">Code First 규칙의 세부 목록을의 수를 [API 설명서](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx)합니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-110">A detailed list of Code First conventions is available in the [API Documentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span> <span data-ttu-id="d3113-111">이 항목에서는 Code First에서 사용 된 규칙의 개요를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-111">This topic provides an overview of the conventions used by Code First.</span></span>  

## <a name="type-discovery"></a><span data-ttu-id="d3113-112">형식 검색</span><span class="sxs-lookup"><span data-stu-id="d3113-112">Type Discovery</span></span>  

<span data-ttu-id="d3113-113">Code First 개발을 사용 하는 경우 일반적으로 개념적 (도메인) 모델을 정의 하는.NET Framework 클래스를 작성 하 여 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-113">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="d3113-114">클래스를 정의 하는 것 외에도 또한 수 있도록 해야 **DbContext** 모델에 포함 하려는 유형을 알고 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-114">In addition to defining the classes, you also need to let **DbContext** know which types you want to include in the model.</span></span> <span data-ttu-id="d3113-115">파생 되는 컨텍스트 클래스를 정의 하면이 위해 **DbContext** 노출 **DbSet** 모델의 일부가 되도록 하려는 형식에 대 한 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-115">To do this, you define a context class that derives from **DbContext** and exposes **DbSet** properties for the types that you want to be part of the model.</span></span> <span data-ttu-id="d3113-116">코드에 먼저 이러한 형식은 포함 하 고도 가져오게 됩니다 모든 참조 형식을 참조 된 형식을 다른 어셈블리에 정의 된 경우에 합니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-116">Code First will include these types and also will pull in any referenced types, even if the referenced types are defined in a different assembly.</span></span>  

<span data-ttu-id="d3113-117">형식 상속 계층 구조에 참여할 경우 정의 하기에 충분 한 **DbSet** 기본 클래스와 동일한 어셈블리에 있는 경우 기본 클래스 및 파생된 형식에 대 한 속성을 자동으로 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-117">If your types participate in an inheritance hierarchy, it is enough to define a **DbSet** property for the base class, and the derived types will be automatically included, if they are in the same assembly as the base class.</span></span>  

<span data-ttu-id="d3113-118">다음 예에서 하나씩만 있기 **DbSet** 에 정의 된 속성을 **SchoolEntities** 클래스 (**부서**).</span><span class="sxs-lookup"><span data-stu-id="d3113-118">In the following example, there is only one **DbSet** property defined on the **SchoolEntities** class (**Departments**).</span></span> <span data-ttu-id="d3113-119">코드는 먼저 검색 하 고 모든 참조 된 형식에서 끌어오기를이 속성을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-119">Code First uses this property to discover and pull in any referenced types.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
    public DbSet<Department> Departments { get; set; }
}

public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public string Location { get; set; }
    public string Days { get; set; }
    public System.DateTime Time { get; set; }
}
```  

<span data-ttu-id="d3113-120">모델에서 형식을 제외 하려는 경우 사용 합니다 **NotMapped** 특성 또는 **DbModelBuilder.Ignore** fluent API.</span><span class="sxs-lookup"><span data-stu-id="d3113-120">If you want to exclude a type from the model, use the **NotMapped** attribute or the **DbModelBuilder.Ignore** fluent API.</span></span>  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a><span data-ttu-id="d3113-121">기본 키 규칙</span><span class="sxs-lookup"><span data-stu-id="d3113-121">Primary Key Convention</span></span>  

<span data-ttu-id="d3113-122">먼저 코드는 클래스의 속성 (없습니다 대/소문자 구분), "ID" 라고 합니다. 또는 클래스 이름 뒤에 "ID"는 경우 속성이 기본 키 임을 유추 합니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-122">Code First infers that a property is a primary key if a property on a class is named “ID” (not case sensitive), or the class name followed by "ID".</span></span> <span data-ttu-id="d3113-123">기본 키 속성의 형식이 숫자 또는 GUID 경우 id 열으로 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-123">If the type of the primary key property is numeric or GUID it will be configured as an identity column.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a><span data-ttu-id="d3113-124">관계 규칙</span><span class="sxs-lookup"><span data-stu-id="d3113-124">Relationship Convention</span></span>  

<span data-ttu-id="d3113-125">Entity Framework, 탐색 속성에는 두 엔터티 형식 간의 관계를 탐색 하는 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-125">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span> <span data-ttu-id="d3113-126">모든 개체에는 해당 개체가 참여하는 모든 관계에 대한 탐색 속성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-126">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="d3113-127">탐색 속성을 사용 하면 탐색 하 고 참조 개체를 반환 합니다. 양방향에서 관계 관리 (복합성이 중 하나인 경우 또는 0 또는 1) 또는 컬렉션 (복합성이 많은) 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-127">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="d3113-128">코드는 먼저 여러분의 형식에 정의 된 탐색 속성을 기준으로 관계를 유추 합니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-128">Code First infers relationships based on the navigation properties defined on your types.</span></span>  

<span data-ttu-id="d3113-129">탐색 속성 외에도 종속 개체를 나타내는 형식에 외래 키 속성을 포함 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-129">In addition to navigation properties, we recommend that you include foreign key properties on the types that represent dependent objects.</span></span> <span data-ttu-id="d3113-130">계정 기본 키 속성으로 동일한 데이터 형식 및 이름을 다음 형식 중 하나를 따르는 속성 관계에 대 한 외래 키를 나타냅니다. '\<탐색 속성 이름\>\<주 기본 키 속성 이름\>','\<주 클래스 이름\>\<기본 키 속성 이름\>', 또는 '\<기본 키 속성을 주체 이름\>'.</span><span class="sxs-lookup"><span data-stu-id="d3113-130">Any property with the same data type as the principal primary key property and with a name that follows one of the following formats represents a foreign key for the relationship: '\<navigation property name\>\<principal primary key property name\>', '\<principal class name\>\<primary key property name\>', or '\<principal primary key property name\>'.</span></span> <span data-ttu-id="d3113-131">여러 일치 항목이 발견 되 면 우선 순위는 위에 나열 된 순서로 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-131">If multiple matches are found then precedence is given in the order listed above.</span></span> <span data-ttu-id="d3113-132">외래 키 검색에 대/소문자 구분 없는 경우</span><span class="sxs-lookup"><span data-stu-id="d3113-132">Foreign key detection is not case sensitive.</span></span> <span data-ttu-id="d3113-133">Code First는 외래 키 속성이 검색 되 면 외래 키의 null 허용 여부에 따라 관계의 복합성을 유추 합니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-133">When a foreign key property is detected, Code First infers the multiplicity of the relationship based on the nullability of the foreign key.</span></span> <span data-ttu-id="d3113-134">속성이 null을 허용 하는 경우 다음 관계로 등록 되어 선택 사항입니다. 관계에 등록은 필요에 따라 합니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-134">If the property is nullable then the relationship is registered as optional; otherwise the relationship is registered as required.</span></span>  

<span data-ttu-id="d3113-135">종속 엔터티에 외래 키를 nullable 없는 경우 Code First 설정한 cascade delete 관계에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-135">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="d3113-136">종속 엔터티에 외래 키에서 null을 허용 하 고 Code First에서 설정 하지 않습니다 cascade delete 관계 보안 주체가 삭제 될 때 외래 키 설정이 적용 됩니다 하는 경우 null입니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-136">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span> <span data-ttu-id="d3113-137">복합성 및 cascade 동작에서 검색을 삭제 흐름 API를 사용 하 여 규칙을 재정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-137">The multiplicity and cascade delete behavior detected by convention can be overridden by using the fluent API.</span></span>  

<span data-ttu-id="d3113-138">다음 예제에서는 탐색 속성 및 foreign key 부서 및 강좌 클래스 간의 관계를 정의 하려면 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-138">In the following example the navigation properties and a foreign key are used to define the relationship between the Department and Course classes.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}
```  

> [!NOTE]
> <span data-ttu-id="d3113-139">동일한 형식 간에 여러 관계가 있는 경우 (예를 들어, 정의한를 **Person** 및 **책** 클래스 위치를 **Person** 클래스를 포함 합니다  **ReviewedBooks** 하 고 **AuthoredBooks** 탐색 속성 및 **책** 클래스를 포함 합니다 **작성자** 및  **검토자** 탐색 속성) 데이터 주석 또는 fluent API를 사용 하 여 관계를 수동으로 구성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-139">If you have multiple relationships between the same types (for example, suppose you define the **Person** and **Book** classes, where the **Person** class contains the **ReviewedBooks** and **AuthoredBooks** navigation properties and the **Book** class contains the **Author** and **Reviewer** navigation properties) you need to manually configure the relationships by using Data Annotations or the fluent API.</span></span> <span data-ttu-id="d3113-140">자세한 내용은 [데이터 주석-관계](~/ef6/modeling/code-first/data-annotations.md) 하 고 [Fluent API-관계](~/ef6/modeling/code-first/fluent/relationships.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-140">For more information, see [Data Annotations - Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>  

## <a name="complex-types-convention"></a><span data-ttu-id="d3113-141">복합 형식 규칙</span><span class="sxs-lookup"><span data-stu-id="d3113-141">Complex Types Convention</span></span>  

<span data-ttu-id="d3113-142">Code First는 기본 키를 유추할 수 없습니다, 그리고 및 기본 키 데이터 주석 또는 fluent API를 통해 등록 된 클래스 정의 검색 하는 경우 형식은 복합 형식으로 등록 자동으로 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-142">When Code First discovers a class definition where a primary key cannot be inferred, and no primary key is registered through data annotations or the fluent API, then the type is automatically registered as a complex type.</span></span> <span data-ttu-id="d3113-143">복합 형식 검색 형식에는 엔터티 형식을 참조 하 고 다른 형식의 컬렉션 속성에서 참조 되지 않는 속성 없는지도 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-143">Complex type detection also requires that the type does not have properties that reference entity types and is not referenced from a collection property on another type.</span></span> <span data-ttu-id="d3113-144">다음 클래스 정의 Code First 유추 하는 **세부 정보** 기본 키를 있기 때문에 복합 유형입니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-144">Given the following class definitions Code First would infer that **Details** is a complex type because it has no primary key.</span></span>  

``` csharp
public partial class OnsiteCourse : Course
{
    public OnsiteCourse()
    {
        Details = new Details();
    }

    public Details Details { get; set; }
}

public class Details
{
    public System.DateTime Time { get; set; }
    public string Location { get; set; }
    public string Days { get; set; }
}
```  

## <a name="connection-string-convention"></a><span data-ttu-id="d3113-145">연결 문자열 규칙</span><span class="sxs-lookup"><span data-stu-id="d3113-145">Connection String Convention</span></span>  

<span data-ttu-id="d3113-146">DbContext 참조를 사용 하 여 연결을 검색 하는 규칙에 알아보려면 [모델과 연결](~/ef6/fundamentals/configuring/connection-strings.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-146">To learn about the conventions that DbContext uses to discover the connection to use see [Connections and Models](~/ef6/fundamentals/configuring/connection-strings.md).</span></span>  

## <a name="removing-conventions"></a><span data-ttu-id="d3113-147">규칙 제거</span><span class="sxs-lookup"><span data-stu-id="d3113-147">Removing Conventions</span></span>  

<span data-ttu-id="d3113-148">System.Data.Entity.ModelConfiguration.Conventions 네임 스페이스에 정의 된 규칙을 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-148">You can remove any of the conventions defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span> <span data-ttu-id="d3113-149">다음 예제에서는 제거 **PluralizingTableNameConvention**합니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-149">The following example removes **PluralizingTableNameConvention**.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
     . . .

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention, the generated tables  
        // will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}
```  

## <a name="custom-conventions"></a><span data-ttu-id="d3113-150">사용자 지정 규칙</span><span class="sxs-lookup"><span data-stu-id="d3113-150">Custom Conventions</span></span>  

<span data-ttu-id="d3113-151">사용자 지정 규칙은 ef6에서부터 지원 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-151">Custom conventions are supported in EF6 onwards.</span></span> <span data-ttu-id="d3113-152">자세한 내용은 참조 [사용자 지정 코드의 첫 번째 규칙](~/ef6/modeling/code-first/conventions/custom.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="d3113-152">For more information see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>
