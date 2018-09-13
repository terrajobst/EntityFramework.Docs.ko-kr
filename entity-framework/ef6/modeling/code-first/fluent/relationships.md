---
title: Fluent API-관계-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
ms.openlocfilehash: 05f282c02699f8bf3c71197ac5e01000f1855917
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490469"
---
# <a name="fluent-api---relationships"></a>Fluent API-관계
> [!NOTE]
> 이 페이지는 Code First fluent API를 사용 하 여 모델의 관계를 설정 하는 방법에 대 한 정보를 제공 합니다. EF 및 관계를 사용 하 여 데이터 액세스 및 조작 하는 방법에 관계에 대 한 일반적인 정보를 참조 하세요 [관계 및 탐색 속성](~/ef6/fundamentals/relationships.md)합니다.  

Code First를 사용 하 여 작업을 하는 경우 도메인 CLR 클래스를 정의 하 여 모델을 정의 합니다. 기본적으로 Entity Framework Code First 규칙을 사용 하 여 데이터베이스 스키마 클래스를 매핑할. Code First 명명 규칙을 사용 하는 경우 대부분의 경우에서 외래 키 및 클래스에서 정의 하는 탐색 속성에 따라 테이블 간의 관계를 설정 하려면 Code First에서 사용할 수 있습니다. 클래스를 정의 하는 경우 규칙을 따르지 않거나 규칙의 작동 방식을 변경 하려는 경우, Code First 테이블 간에 관계를 매핑할 수 있도록 클래스를 구성 하려면 데이터 주석 또는 fluent API를 사용할 수 있습니다.  

## <a name="introduction"></a>소개  

Fluent API를 사용 하 여 관계를 구성할 때 EntityTypeConfiguration 인스턴스를 시작 하 고 HasRequired, HasOptional, 또는 HasMany 메서드를 사용 하 여이 엔터티가 참여 하는 관계의 유형을 지정 합니다. HasRequired 메서드와 HasOptional 참조 탐색 속성을 나타내는 람다 식을 사용 합니다. HasMany 메서드는 컬렉션 탐색 속성을 나타내는 람다 식입니다. WithRequired, WithOptional, 및 WithMany 메서드를 사용 하 여 역 탐색 속성을 구성할 수 있습니다. 이러한 방법에는 인수를 사용 하지 않는 및 단방향 탐색을 사용 하 여 카디널리티를 지정 하는 데 사용 될 수 있는 오버 로드가 있습니다.  

그런 다음 HasForeignKey 메서드를 사용 하 여 외래 키 속성을 구성할 수 있습니다. 이 메서드는 외래 키로 사용할 속성을 나타내는 람다 식을 사용 합니다.  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a>(1-에-0 또는 1) 필요한-에-선택적 관계를 구성합니다.  

다음 예제에서는--0-또는-일대일 관계를 구성합니다. OfficeAssignment 속성이 InstructorID는 기본 키 및 외래 키를 하므로 속성의 이름을 HasKey 메서드를 사용 하 여를 기본 키를 구성 하는 규칙을 따르지 않습니다.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a>여기서 양쪽 (일대일) 필요한 관계를 구성 합니다.  

대부분의 경우에는 종속 형식 및 관계의 보안 주체는 Entity Framework 유추할 수 있습니다. 그러나 이러한 경우 모두 관계의 end를 필수 또는 양쪽 모두 선택적 Entity Framework 종속 및 주체를 식별할 수 없습니다. 관계의 양쪽 끝이 필요한 경우, WithRequiredPrincipal 또는 WithRequiredDependent HasRequired 메서드를 사용 합니다. 관계의 두 end는 선택 사항, WithOptionalPrincipal 또는 WithOptionalDependent HasOptional 메서드를 사용 합니다.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a>다 대 다 관계를 구성합니다.  

다음 코드는 강좌와 강사 형식 간의 다 대 다 관계를 구성합니다. 다음 예제에서는 조인 테이블을 만드는 기본 Code First 규칙 사용 됩니다. 결과적으로 CourseInstructor 테이블 Course_CourseID 및 Instructor_InstructorID 열을 사용 하 여 생성 됩니다.  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

테이블에서 조인 테이블 이름 및 열 이름을 지정 하려는 경우에 Map 메서드를 사용 하 여 추가 구성을 수행 해야 합니다. 다음 코드는 CourseID 및 InstructorID 열이 있는 CourseInstructor 테이블을 생성합니다.  

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

## <a name="configuring-a-relationship-with-one-navigation-property"></a>관계 탐색 속성을 사용 하 여 구성  

한 방향 (단방향) 관계는 탐색 속성을 둘 다에 없는 관계 end 중 하나만 정의 된 경우. 관례적으로 Code First 항상 해석에 일 대 다 관계는 단방향 합니다. 예를 들어 한 일 관계가 Instructor와 OfficeAssignment를 해야 하는 탐색 속성에서 Instructor 형식에만, 원하는 경우 fluent API를 사용 하 여이 관계를 구성 해야 합니다.  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a>계단식 삭제를 사용 하도록 설정  

WillCascadeOnDelete 메서드를 사용 하 여 관계에서 계단식 삭제를 구성할 수 있습니다. 종속 엔터티에 외래 키를 nullable 없는 경우 Code First 설정한 cascade delete 관계에 있습니다. 종속 엔터티에 외래 키에서 null을 허용 하 고 Code First에서 설정 하지 않습니다 cascade delete 관계 보안 주체가 삭제 될 때 외래 키 설정이 적용 됩니다 하는 경우 null입니다.  

사용 하 여 이러한 하위 삭제 규칙을 제거할 수 있습니다.  

modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>)  
modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>)  

다음 코드는 필수 관계가 되도록 구성 하 고 하위 삭제를 사용 하지 않도록 설정 합니다.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a>복합 외래 키를 구성합니다.  

DepartmentID 및 Name 속성으로 이루어져 부서 형식에 기본 키, 경우 부서 및 과정 형식에서 외래 키에 대 한 기본 키 다음과 같이 구성 됩니다.  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>모델에서 정의 되지 않은 외래 키 이름 바꾸기  

CLR 형식에서 외래 키를 정의 하지만 데이터베이스에 있어야 하는 이름 지정을 하지 않으려는 경우 다음을 수행 합니다.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a>코드 첫 번째 규칙을 따르지 않는 외래 키 이름 구성  

과정 클래스에 외래 키 속성 대신 DepartmentID SomeDepartmentID 호출 SomeDepartmentID 외래 키를 지정 하려면 다음을 수행 해야 합니다.  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a>샘플에 사용 된 모델  

다음 Code First 모델은이 페이지에서 샘플에 사용 됩니다.  

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
