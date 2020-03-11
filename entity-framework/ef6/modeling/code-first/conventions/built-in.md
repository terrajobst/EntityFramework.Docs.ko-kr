---
title: Code First 규칙-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
ms.openlocfilehash: 4d03a32db5d84eb37c22617a95005b272172a65d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415948"
---
# <a name="code-first-conventions"></a><span data-ttu-id="72e99-102">Code First 규칙</span><span class="sxs-lookup"><span data-stu-id="72e99-102">Code First Conventions</span></span>
<span data-ttu-id="72e99-103">Code First를 사용 하면 또는 Visual Basic .NET 클래스를 C# 사용 하 여 모델을 설명할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-103">Code First enables you to describe a model by using C# or Visual Basic .NET classes.</span></span> <span data-ttu-id="72e99-104">모델의 기본 셰이프는 규칙을 사용 하 여 검색 됩니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-104">The basic shape of the model is detected by using conventions.</span></span> <span data-ttu-id="72e99-105">규칙은 Code First 작업할 때 클래스 정의에 따라 개념적 모델을 자동으로 구성 하는 데 사용 되는 규칙 집합입니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-105">Conventions are sets of rules that are used to automatically configure a conceptual model based on class definitions when working with Code First.</span></span> <span data-ttu-id="72e99-106">규칙은 System.web. ModelConfiguration 네임 스페이스에 정의 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-106">The conventions are defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>  

<span data-ttu-id="72e99-107">데이터 주석 또는 흐름 API를 사용 하 여 모델을 추가로 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-107">You can further configure your model by using data annotations or the fluent API.</span></span> <span data-ttu-id="72e99-108">우선 순위는 흐름 API를 통해 구성 되 고 그 다음에 데이터 주석과 규칙을 통해 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-108">Precedence is given to configuration through the fluent API followed by data annotations and then conventions.</span></span> <span data-ttu-id="72e99-109">자세한 내용은 [데이터 주석](~/ef6/modeling/code-first/data-annotations.md), [흐름 api-관계](~/ef6/modeling/code-first/fluent/relationships.md), [흐름 & 속성](~/ef6/modeling/code-first/fluent/types-and-properties.md) 및 [VB.NET를 사용 하 여 흐름 api](~/ef6/modeling/code-first/fluent/vb.md)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="72e99-109">For more information see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - Types & Properties](~/ef6/modeling/code-first/fluent/types-and-properties.md) and [Fluent API with VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span></span>  

<span data-ttu-id="72e99-110">[API 설명서](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx)에서 제공 되는 Code First 규칙의 자세한 목록을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-110">A detailed list of Code First conventions is available in the [API Documentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span> <span data-ttu-id="72e99-111">이 항목에서는 Code First에서 사용 하는 규칙의 개요를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-111">This topic provides an overview of the conventions used by Code First.</span></span>  

## <a name="type-discovery"></a><span data-ttu-id="72e99-112">유형 검색</span><span class="sxs-lookup"><span data-stu-id="72e99-112">Type Discovery</span></span>  

<span data-ttu-id="72e99-113">Code First 개발을 사용 하는 경우 일반적으로 개념 (도메인) 모델을 정의 하는 .NET Framework 클래스를 작성 하는 것으로 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-113">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="72e99-114">클래스를 정의 하는 것 외에도 **DbContext** 에 모델에 포함 하려는 형식을 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-114">In addition to defining the classes, you also need to let **DbContext** know which types you want to include in the model.</span></span> <span data-ttu-id="72e99-115">이렇게 하려면 **DbContext** 에서 파생 되는 컨텍스트 클래스를 정의 하 고 모델에 포함할 형식의 **dbset** 속성을 노출 합니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-115">To do this, you define a context class that derives from **DbContext** and exposes **DbSet** properties for the types that you want to be part of the model.</span></span> <span data-ttu-id="72e99-116">Code First는 이러한 형식을 포함 하 고 참조 된 형식이 다른 어셈블리에 정의 된 경우에도 모든 참조 된 형식을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-116">Code First will include these types and also will pull in any referenced types, even if the referenced types are defined in a different assembly.</span></span>  

<span data-ttu-id="72e99-117">형식이 상속 계층 구조에 참여 하는 경우 기본 클래스에 대 한 **Dbset** 속성을 정의 하는 데 충분 하며 기본 클래스와 동일한 어셈블리에 있는 경우 파생 형식이 자동으로 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-117">If your types participate in an inheritance hierarchy, it is enough to define a **DbSet** property for the base class, and the derived types will be automatically included, if they are in the same assembly as the base class.</span></span>  

<span data-ttu-id="72e99-118">다음 예에서는 **SchoolEntities** 클래스 (**부서**)에 하나의 **dbset** 속성만 정의 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-118">In the following example, there is only one **DbSet** property defined on the **SchoolEntities** class (**Departments**).</span></span> <span data-ttu-id="72e99-119">Code First는이 속성을 사용 하 여 참조 된 형식을 검색 하 고 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-119">Code First uses this property to discover and pull in any referenced types.</span></span>  

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

<span data-ttu-id="72e99-120">모델에서 형식을 제외 하려면 **Notmapped** 특성 또는 **dbmodelbuilder** 를 사용 합니다. 흐름 API를 무시 합니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-120">If you want to exclude a type from the model, use the **NotMapped** attribute or the **DbModelBuilder.Ignore** fluent API.</span></span>  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a><span data-ttu-id="72e99-121">기본 키 규칙</span><span class="sxs-lookup"><span data-stu-id="72e99-121">Primary Key Convention</span></span>  

<span data-ttu-id="72e99-122">클래스의 속성 이름이 "ID" (대/소문자 구분 안 함) 이거나 클래스 이름 앞에 "ID"가 있는 경우 Code First는 속성이 기본 키 임을 유추 합니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-122">Code First infers that a property is a primary key if a property on a class is named “ID” (not case sensitive), or the class name followed by "ID".</span></span> <span data-ttu-id="72e99-123">기본 키 속성의 형식이 numeric 또는 GUID 이면 id 열로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-123">If the type of the primary key property is numeric or GUID it will be configured as an identity column.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a><span data-ttu-id="72e99-124">관계 규칙</span><span class="sxs-lookup"><span data-stu-id="72e99-124">Relationship Convention</span></span>  

<span data-ttu-id="72e99-125">Entity Framework 탐색 속성은 두 엔터티 형식 간의 관계를 탐색 하는 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-125">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span> <span data-ttu-id="72e99-126">모든 개체에는 해당 개체가 참여하는 모든 관계에 대한 탐색 속성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-126">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="72e99-127">탐색 속성을 사용 하면 두 방향에서 관계를 탐색 하 고 관리할 수 있습니다. 참조 개체 (복합성이 하나 또는 0 또는 1)를 반환 하거나 복합성이 많은 경우 컬렉션을 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-127">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="72e99-128">Code First는 형식에 정의 된 탐색 속성을 기준으로 관계를 유추 합니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-128">Code First infers relationships based on the navigation properties defined on your types.</span></span>  

<span data-ttu-id="72e99-129">탐색 속성 외에 종속 개체를 나타내는 형식에 외래 키 속성을 포함 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-129">In addition to navigation properties, we recommend that you include foreign key properties on the types that represent dependent objects.</span></span> <span data-ttu-id="72e99-130">주 기본 키 속성과 형식이 같으며 이름이 다음 형식 중 하나를 따르는 속성은 관계의 외래 키를 나타냅니다. '\<탐색 속성 이름\>\<주체 기본 키 속성 이름\>', '\<주 클래스 이름\>\<기본 키 속성 이름\>' 또는 '\<주 키 속성 이름\>'.</span><span class="sxs-lookup"><span data-stu-id="72e99-130">Any property with the same data type as the principal primary key property and with a name that follows one of the following formats represents a foreign key for the relationship: '\<navigation property name\>\<principal primary key property name\>', '\<principal class name\>\<primary key property name\>', or '\<principal primary key property name\>'.</span></span> <span data-ttu-id="72e99-131">일치 하는 항목이 여러 개 있으면 위에 나열 된 순서로 우선 순위가 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-131">If multiple matches are found then precedence is given in the order listed above.</span></span> <span data-ttu-id="72e99-132">외래 키 검색은 대/소문자를 구분 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-132">Foreign key detection is not case sensitive.</span></span> <span data-ttu-id="72e99-133">외래 키 속성이 검색 되 면 외래 키의 null 허용 여부에 따라 관계의 복합성을 유추 Code First 합니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-133">When a foreign key property is detected, Code First infers the multiplicity of the relationship based on the nullability of the foreign key.</span></span> <span data-ttu-id="72e99-134">속성이 null을 허용 하면 관계가 옵션으로 등록 됩니다. 그렇지 않으면 필요에 따라 관계가 등록 됩니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-134">If the property is nullable then the relationship is registered as optional; otherwise the relationship is registered as required.</span></span>  

<span data-ttu-id="72e99-135">종속 엔터티의 외래 키가 null을 허용 하지 않는 경우 Code First는 관계에서 cascade delete를 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-135">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="72e99-136">종속 엔터티의 외래 키가 null Code First을 허용 하는 경우에는 관계에서 cascade delete를 설정 하지 않고, 주 서버가 삭제 되 면 외래 키가 null로 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-136">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span> <span data-ttu-id="72e99-137">규칙에 따라 검색 되는 복합성 및 cascade delete 동작은 흐름 API를 사용 하 여 재정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-137">The multiplicity and cascade delete behavior detected by convention can be overridden by using the fluent API.</span></span>  

<span data-ttu-id="72e99-138">다음 예에서는 탐색 속성과 외래 키를 사용 하 여 학과와 강의 클래스 간의 관계를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-138">In the following example the navigation properties and a foreign key are used to define the relationship between the Department and Course classes.</span></span>  

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
> <span data-ttu-id="72e99-139">동일한 형식 간에 여러 관계가 있는 경우 (예 **: person 클래스** **에** **ReviewedBooks** 및 흐름 **Edbooks** 탐색 속성이 포함 되 고 **book** 클래스에 **Author** **및** **검토자** 탐색 속성이 포함 되어 있는 경우) 데이터 주석 또는 API를 사용 하 여 관계를 수동으로 구성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-139">If you have multiple relationships between the same types (for example, suppose you define the **Person** and **Book** classes, where the **Person** class contains the **ReviewedBooks** and **AuthoredBooks** navigation properties and the **Book** class contains the **Author** and **Reviewer** navigation properties) you need to manually configure the relationships by using Data Annotations or the fluent API.</span></span> <span data-ttu-id="72e99-140">자세한 내용은 [데이터 주석-관계](~/ef6/modeling/code-first/data-annotations.md) 및 [흐름](~/ef6/modeling/code-first/fluent/relationships.md)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="72e99-140">For more information, see [Data Annotations - Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>  

## <a name="complex-types-convention"></a><span data-ttu-id="72e99-141">복합 형식 규칙</span><span class="sxs-lookup"><span data-stu-id="72e99-141">Complex Types Convention</span></span>  

<span data-ttu-id="72e99-142">Code First 기본 키를 유추할 수 없는 클래스 정의를 검색 하는 경우 기본 키가 데이터 주석 또는 흐름 API를 통해 등록 되지 않으면 형식이 자동으로 복합 형식으로 등록 됩니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-142">When Code First discovers a class definition where a primary key cannot be inferred, and no primary key is registered through data annotations or the fluent API, then the type is automatically registered as a complex type.</span></span> <span data-ttu-id="72e99-143">복합 형식 검색에서는 엔터티 형식을 참조 하 고 다른 형식의 컬렉션 속성에서 참조 되지 않는 속성이 형식에 포함 되어 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-143">Complex type detection also requires that the type does not have properties that reference entity types and is not referenced from a collection property on another type.</span></span> <span data-ttu-id="72e99-144">다음 클래스 정의를 지정 하는 경우 기본 키가 없기 때문에 **세부 정보** 는 복합 형식 임을 유추할 Code First.</span><span class="sxs-lookup"><span data-stu-id="72e99-144">Given the following class definitions Code First would infer that **Details** is a complex type because it has no primary key.</span></span>  

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

## <a name="connection-string-convention"></a><span data-ttu-id="72e99-145">연결 문자열 규칙</span><span class="sxs-lookup"><span data-stu-id="72e99-145">Connection String Convention</span></span>  

<span data-ttu-id="72e99-146">DbContext에서 사용할 연결을 검색 하는 데 사용 하는 규칙에 대해 알아보려면 [연결 및 모델](~/ef6/fundamentals/configuring/connection-strings.md)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="72e99-146">To learn about the conventions that DbContext uses to discover the connection to use see [Connections and Models](~/ef6/fundamentals/configuring/connection-strings.md).</span></span>  

## <a name="removing-conventions"></a><span data-ttu-id="72e99-147">규칙 제거</span><span class="sxs-lookup"><span data-stu-id="72e99-147">Removing Conventions</span></span>  

<span data-ttu-id="72e99-148">System.object 네임 스페이스에 정의 된 모든 규칙을 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-148">You can remove any of the conventions defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span> <span data-ttu-id="72e99-149">다음 예에서는 **PluralizingTableNameConvention**를 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-149">The following example removes **PluralizingTableNameConvention**.</span></span>  

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

## <a name="custom-conventions"></a><span data-ttu-id="72e99-150">사용자 지정 규칙</span><span class="sxs-lookup"><span data-stu-id="72e99-150">Custom Conventions</span></span>  

<span data-ttu-id="72e99-151">사용자 지정 규칙은 EF6에서 지원 됩니다.</span><span class="sxs-lookup"><span data-stu-id="72e99-151">Custom conventions are supported in EF6 onwards.</span></span> <span data-ttu-id="72e99-152">자세한 내용은 [사용자 지정 Code First 규칙](~/ef6/modeling/code-first/conventions/custom.md)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="72e99-152">For more information see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>
