---
title: 흐름 API-관계-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
ms.openlocfilehash: 05f282c02699f8bf3c71197ac5e01000f1855917
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415762"
---
# <a name="fluent-api---relationships"></a><span data-ttu-id="546c4-102">흐름 API-관계</span><span class="sxs-lookup"><span data-stu-id="546c4-102">Fluent API - Relationships</span></span>
> [!NOTE]
> <span data-ttu-id="546c4-103">이 페이지에서는 흐름 API를 사용 하 여 Code First 모델에서 관계를 설정 하는 방법에 대 한 정보를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-103">This page provides information about setting up relationships in your Code First model using the fluent API.</span></span> <span data-ttu-id="546c4-104">EF의 관계 및 관계를 사용 하 여 데이터에 액세스 하 고 조작 하는 방법에 대 한 일반적인 내용은 [관계 & 탐색 속성](~/ef6/fundamentals/relationships.md)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="546c4-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>  

<span data-ttu-id="546c4-105">Code First 작업할 때 도메인 CLR 클래스를 정의 하 여 모델을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-105">When working with Code First, you define your model by defining your domain CLR classes.</span></span> <span data-ttu-id="546c4-106">기본적으로 Entity Framework는 Code First 규칙을 사용 하 여 클래스를 데이터베이스 스키마에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-106">By default, Entity Framework uses the Code First conventions to map your classes to the database schema.</span></span> <span data-ttu-id="546c4-107">Code First 명명 규칙을 사용 하는 경우 대부분의 경우 Code First를 사용 하 여 클래스에 정의 된 외래 키와 탐색 속성을 기반으로 테이블 간의 관계를 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-107">If you use the Code First naming conventions, in most cases you can rely on Code First to set up relationships between your tables based on the foreign keys and navigation properties that you define on the classes.</span></span> <span data-ttu-id="546c4-108">클래스를 정의할 때 규칙을 따르지 않거나 규칙이 작동 하는 방식을 변경 하려는 경우 흐름 API 또는 데이터 주석을 사용 하 여 테이블 간의 관계를 매핑할 수 Code First 클래스를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-108">If you do not follow the conventions when defining your classes, or if you want to change the way the conventions work, you can use the fluent API or data annotations to configure your classes so Code First can map the relationships between your tables.</span></span>  

## <a name="introduction"></a><span data-ttu-id="546c4-109">소개</span><span class="sxs-lookup"><span data-stu-id="546c4-109">Introduction</span></span>  

<span data-ttu-id="546c4-110">흐름 API를 사용 하 여 관계를 구성 하는 경우 EntityTypeConfiguration 인스턴스로 시작한 다음 HasRequired, Hasrequired 또는 Hasrequired 메서드를 사용 하 여이 엔터티가 참여 하는 관계의 형식을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-110">When configuring a relationship with the fluent API, you start with the EntityTypeConfiguration instance and then use the HasRequired, HasOptional, or HasMany method to specify the type of relationship this entity participates in.</span></span> <span data-ttu-id="546c4-111">HasRequired 및 Hasrequired 메서드는 참조 탐색 속성을 나타내는 람다 식을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-111">The HasRequired and HasOptional methods take a lambda expression that represents a reference navigation property.</span></span> <span data-ttu-id="546c4-112">HasMany 메서드는 컬렉션 탐색 속성을 나타내는 람다 식을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-112">The HasMany method takes a lambda expression that represents a collection navigation property.</span></span> <span data-ttu-id="546c4-113">그런 다음 WithRequired, Withrequired 및 Withrequired 메서드를 사용 하 여 역 탐색 속성을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-113">You can then configure an inverse navigation property by using the WithRequired, WithOptional, and WithMany methods.</span></span> <span data-ttu-id="546c4-114">이러한 메서드에는 인수를 사용 하지 않으며 단방향 탐색을 사용 하 여 카디널리티를 지정 하는 데 사용할 수 있는 오버 로드가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-114">These methods have overloads that do not take arguments and can be used to specify cardinality with unidirectional navigations.</span></span>  

<span data-ttu-id="546c4-115">그런 다음 HasForeignKey 메서드를 사용 하 여 외래 키 속성을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-115">You can then configure foreign key properties by using the HasForeignKey method.</span></span> <span data-ttu-id="546c4-116">이 메서드는 외래 키로 사용할 속성을 나타내는 람다 식을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-116">This method takes a lambda expression that represents the property to be used as the foreign key.</span></span>  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a><span data-ttu-id="546c4-117">필수-옵션 관계 (일 대 일 또는 1 개) 구성</span><span class="sxs-lookup"><span data-stu-id="546c4-117">Configuring a Required-to-Optional Relationship (One-to–Zero-or-One)</span></span>  

<span data-ttu-id="546c4-118">다음 예에서는 일 대 영 또는 일 관계를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-118">The following example configures a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="546c4-119">OfficeAssignment에는 기본 키와 외래 키 인 InstructorID 속성이 있습니다 .이 속성의 이름은 기본 키를 구성 하는 데 HasKey 메서드를 사용 하는 규칙을 따르지 않기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-119">The OfficeAssignment has the InstructorID property that is a primary key and a foreign key, because the name of the property does not follow the convention the HasKey method is used to configure the primary key.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a><span data-ttu-id="546c4-120">양쪽 끝이 필요한 관계 구성 (일 대 일)</span><span class="sxs-lookup"><span data-stu-id="546c4-120">Configuring a Relationship Where Both Ends Are Required (One-to-One)</span></span>  

<span data-ttu-id="546c4-121">대부분의 경우 Entity Framework는 어떤 형식을 관계의 보안 주체 인지 유추할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-121">In most cases Entity Framework can infer which type is the dependent and which is the principal in a relationship.</span></span> <span data-ttu-id="546c4-122">그러나 관계의 양쪽 끝이 모두 필요 하거나 두 쪽 모두 선택 사항인 경우 종속 및 보안 주체를 식별할 수 Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="546c4-122">However, when both ends of the relationship are required or both sides are optional Entity Framework cannot identify the dependent and principal.</span></span> <span data-ttu-id="546c4-123">관계의 두 end가 모두 필요한 경우에는 HasRequired 메서드 뒤에 WithRequiredPrincipal 또는 Withrequiredprincipal를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-123">When both ends of the relationship are required, use WithRequiredPrincipal or WithRequiredDependent after the HasRequired method.</span></span> <span data-ttu-id="546c4-124">관계의 양쪽 끝이 선택 사항인 경우 HasOptional 메서드 뒤에 WithOptionalPrincipal 또는 WithOptionalDependent를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-124">When both ends of the relationship are optional, use WithOptionalPrincipal or WithOptionalDependent after the HasOptional method.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a><span data-ttu-id="546c4-125">다 대 다 관계 구성</span><span class="sxs-lookup"><span data-stu-id="546c4-125">Configuring a Many-to-Many Relationship</span></span>  

<span data-ttu-id="546c4-126">다음 코드는 강좌와 강사 유형 간에 다 대 다 관계를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-126">The following code configures a many-to-many relationship between the Course and Instructor types.</span></span> <span data-ttu-id="546c4-127">다음 예에서는 기본 Code First 규칙을 사용 하 여 조인 테이블을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-127">In the following example, the default Code First conventions are used to create a join table.</span></span> <span data-ttu-id="546c4-128">따라서 Course_CourseID 및 Instructor_InstructorID 열을 사용 하 여 CourseInstructor 테이블을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-128">As a result the CourseInstructor table is created with Course_CourseID and Instructor_InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

<span data-ttu-id="546c4-129">조인 테이블 이름 및 테이블의 열 이름을 지정 하려는 경우 Map 메서드를 사용 하 여 추가 구성을 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-129">If you want to specify the join table name and the names of the columns in the table you need to do additional configuration by using the Map method.</span></span> <span data-ttu-id="546c4-130">다음 코드는 CourseID 및 InstructorID 열을 사용 하 여 CourseInstructor 테이블을 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-130">The following code generates the CourseInstructor table with CourseID and InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
    .Map(m =>
    {
        m.ToTable("CourseInstructor");
        m.MapLeftKey("CourseID");
        m.MapRightKey("InstructorID");
    });
```  

## <a name="configuring-a-relationship-with-one-navigation-property"></a><span data-ttu-id="546c4-131">하나의 탐색 속성을 사용 하 여 관계 구성</span><span class="sxs-lookup"><span data-stu-id="546c4-131">Configuring a Relationship with One Navigation Property</span></span>  

<span data-ttu-id="546c4-132">단방향 (단방향) 관계는 탐색 속성이 관계 종료 중 하나에만 정의 되 고 둘 다에서 정의 되지 않은 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-132">A one-directional (also called unidirectional) relationship is when a navigation property is defined on only one of the relationship ends and not on both.</span></span> <span data-ttu-id="546c4-133">규칙에 따라 Code First 항상 단방향 관계를 일 대 다로 해석 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-133">By convention, Code First always interprets a unidirectional relationship as one-to-many.</span></span> <span data-ttu-id="546c4-134">예를 들어 강사 유형에만 탐색 속성이 있는 강사와 OfficeAssignment 사이에 일 대 일 관계를 사용 하려면 흐름 API를 사용 하 여이 관계를 구성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-134">For example, if you want a one-to-one relationship between Instructor and OfficeAssignment, where you have a navigation property on only the Instructor type, you need to use the fluent API to configure this relationship.</span></span>  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a><span data-ttu-id="546c4-135">계단식 삭제 사용</span><span class="sxs-lookup"><span data-stu-id="546c4-135">Enabling Cascade Delete</span></span>  

<span data-ttu-id="546c4-136">WillCascadeOnDelete 메서드를 사용 하 여 관계에 대해 cascade delete를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-136">You can configure cascade delete on a relationship by using the WillCascadeOnDelete method.</span></span> <span data-ttu-id="546c4-137">종속 엔터티의 외래 키가 null을 허용 하지 않는 경우 Code First는 관계에서 cascade delete를 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-137">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="546c4-138">종속 엔터티의 외래 키가 null Code First을 허용 하는 경우에는 관계에서 cascade delete를 설정 하지 않고, 주 서버가 삭제 되 면 외래 키가 null로 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-138">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span>  

<span data-ttu-id="546c4-139">다음을 사용 하 여 이러한 cascade delete 규칙을 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-139">You can remove these cascade delete conventions by using:</span></span>  

<span data-ttu-id="546c4-140">modelBuilder. OneToManyCascadeDeleteConvention\>()\<제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>()</span></span>  
<span data-ttu-id="546c4-141">modelBuilder. ManyToManyCascadeDeleteConvention\>()\<제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>()</span></span>  

<span data-ttu-id="546c4-142">다음 코드는 관계를 필수로 구성 하 고 cascade delete를 사용 하지 않도록 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-142">The following code configures the relationship to be required and then disables cascade delete.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a><span data-ttu-id="546c4-143">복합 외래 키 구성</span><span class="sxs-lookup"><span data-stu-id="546c4-143">Configuring a Composite Foreign Key</span></span>  

<span data-ttu-id="546c4-144">부서 유형의 기본 키가 DepartmentID 및 Name 속성으로 구성 된 경우에는 다음과 같이 학과의 기본 키와 과정 유형에 대 한 외래 키를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-144">If the primary key on the Department type consisted of DepartmentID and Name properties, you would configure the primary key for the Department and the foreign key on the Course types as follows:</span></span>  

``` csharp
// Composite primary key
modelBuilder.Entity<Department>()
.HasKey(d => new { d.DepartmentID, d.Name });

// Composite foreign key
modelBuilder.Entity<Course>()  
    .HasRequired(c => c.Department)  
    .WithMany(d => d.Courses)
    .HasForeignKey(d => new { d.DepartmentID, d.DepartmentName });
```  

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="546c4-145">모델에 정의 되지 않은 외래 키 이름 바꾸기</span><span class="sxs-lookup"><span data-stu-id="546c4-145">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="546c4-146">CLR 유형에 외래 키를 정의 하지 않고 데이터베이스에 포함 되어야 하는 이름을 지정 하려면 다음을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-146">If you choose not to define a foreign key on the CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a><span data-ttu-id="546c4-147">Code First 규칙을 따르지 않는 외래 키 이름 구성</span><span class="sxs-lookup"><span data-stu-id="546c4-147">Configuring a Foreign Key Name That Does Not Follow the Code First Convention</span></span>  

<span data-ttu-id="546c4-148">강의 클래스의 foreign key 속성이 DepartmentID 대신 SomeDepartmentID 이라고 하는 경우 다음을 수행 하 여 SomeDepartmentID를 외래 키로 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-148">If the foreign key property on the Course class was called SomeDepartmentID instead of DepartmentID, you would need to do the following to specify that you want SomeDepartmentID to be the foreign key:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a><span data-ttu-id="546c4-149">예제에 사용 되는 모델</span><span class="sxs-lookup"><span data-stu-id="546c4-149">Model Used in Samples</span></span>  

<span data-ttu-id="546c4-150">다음 Code First 모델은이 페이지의 샘플에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="546c4-150">The following Code First model is used for the samples on this page.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.ModelConfiguration.Conventions;
// add a reference to System.ComponentModel.DataAnnotations DLL
using System.ComponentModel.DataAnnotations;
using System.Collections.Generic;
using System;

public class SchoolEntities : DbContext
{
    public DbSet<Course> Courses { get; set; }
    public DbSet<Department> Departments { get; set; }
    public DbSet<Instructor> Instructors { get; set; }
    public DbSet<OfficeAssignment> OfficeAssignments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention then the generated tables will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}

public class Department
{
    public Department()
    {
        this.Courses = new HashSet<Course>();
    }
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }
    public decimal Budget { get; set; }
    public System.DateTime StartDate { get; set; }
    public int? Administrator { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; private set; }
}

public class Course
{
    public Course()
    {
        this.Instructors = new HashSet<Instructor>();
    }
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
    public virtual ICollection<Instructor> Instructors { get; private set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

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

public class Instructor
{
    public Instructor()
    {
        this.Courses = new List<Course>();
    }

    // Primary key
    public int InstructorID { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
    public System.DateTime HireDate { get; set; }

    // Navigation properties
    public virtual ICollection<Course> Courses { get; private set; }
}

public class OfficeAssignment
{
    // Specifying InstructorID as a primary
    [Key()]
    public Int32 InstructorID { get; set; }

    public string Location { get; set; }

    // When Entity Framework sees Timestamp attribute
    // it configures ConcurrencyCheck and DatabaseGeneratedPattern=Computed.
    [Timestamp]
    public Byte[] Timestamp { get; set; }

    // Navigation property
    public virtual Instructor Instructor { get; set; }
}
```  
