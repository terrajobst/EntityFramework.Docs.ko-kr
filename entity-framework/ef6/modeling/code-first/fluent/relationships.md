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
# <a name="fluent-api---relationships"></a>흐름 API-관계
> [!NOTE]
> 이 페이지에서는 흐름 API를 사용 하 여 Code First 모델에서 관계를 설정 하는 방법에 대 한 정보를 제공 합니다. EF의 관계 및 관계를 사용 하 여 데이터에 액세스 하 고 조작 하는 방법에 대 한 일반적인 내용은 [관계 & 탐색 속성](~/ef6/fundamentals/relationships.md)을 참조 하세요.  

Code First 작업할 때 도메인 CLR 클래스를 정의 하 여 모델을 정의 합니다. 기본적으로 Entity Framework는 Code First 규칙을 사용 하 여 클래스를 데이터베이스 스키마에 매핑합니다. Code First 명명 규칙을 사용 하는 경우 대부분의 경우 Code First를 사용 하 여 클래스에 정의 된 외래 키와 탐색 속성을 기반으로 테이블 간의 관계를 설정할 수 있습니다. 클래스를 정의할 때 규칙을 따르지 않거나 규칙이 작동 하는 방식을 변경 하려는 경우 흐름 API 또는 데이터 주석을 사용 하 여 테이블 간의 관계를 매핑할 수 Code First 클래스를 구성할 수 있습니다.  

## <a name="introduction"></a>소개  

흐름 API를 사용 하 여 관계를 구성 하는 경우 EntityTypeConfiguration 인스턴스로 시작한 다음 HasRequired, Hasrequired 또는 Hasrequired 메서드를 사용 하 여이 엔터티가 참여 하는 관계의 형식을 지정 합니다. HasRequired 및 Hasrequired 메서드는 참조 탐색 속성을 나타내는 람다 식을 사용 합니다. HasMany 메서드는 컬렉션 탐색 속성을 나타내는 람다 식을 사용 합니다. 그런 다음 WithRequired, Withrequired 및 Withrequired 메서드를 사용 하 여 역 탐색 속성을 구성할 수 있습니다. 이러한 메서드에는 인수를 사용 하지 않으며 단방향 탐색을 사용 하 여 카디널리티를 지정 하는 데 사용할 수 있는 오버 로드가 있습니다.  

그런 다음 HasForeignKey 메서드를 사용 하 여 외래 키 속성을 구성할 수 있습니다. 이 메서드는 외래 키로 사용할 속성을 나타내는 람다 식을 사용 합니다.  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a>필수-옵션 관계 (일 대 일 또는 1 개) 구성  

다음 예에서는 일 대 영 또는 일 관계를 구성 합니다. OfficeAssignment에는 기본 키와 외래 키 인 InstructorID 속성이 있습니다 .이 속성의 이름은 기본 키를 구성 하는 데 HasKey 메서드를 사용 하는 규칙을 따르지 않기 때문입니다.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a>양쪽 끝이 필요한 관계 구성 (일 대 일)  

대부분의 경우 Entity Framework는 어떤 형식을 관계의 보안 주체 인지 유추할 수 있습니다. 그러나 관계의 양쪽 끝이 모두 필요 하거나 두 쪽 모두 선택 사항인 경우 종속 및 보안 주체를 식별할 수 Entity Framework. 관계의 두 end가 모두 필요한 경우에는 HasRequired 메서드 뒤에 WithRequiredPrincipal 또는 Withrequiredprincipal를 사용 합니다. 관계의 양쪽 끝이 선택 사항인 경우 HasOptional 메서드 뒤에 WithOptionalPrincipal 또는 WithOptionalDependent를 사용 합니다.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a>다 대 다 관계 구성  

다음 코드는 강좌와 강사 유형 간에 다 대 다 관계를 구성 합니다. 다음 예에서는 기본 Code First 규칙을 사용 하 여 조인 테이블을 만듭니다. 따라서 Course_CourseID 및 Instructor_InstructorID 열을 사용 하 여 CourseInstructor 테이블을 만듭니다.  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

조인 테이블 이름 및 테이블의 열 이름을 지정 하려는 경우 Map 메서드를 사용 하 여 추가 구성을 수행 해야 합니다. 다음 코드는 CourseID 및 InstructorID 열을 사용 하 여 CourseInstructor 테이블을 생성 합니다.  

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

## <a name="configuring-a-relationship-with-one-navigation-property"></a>하나의 탐색 속성을 사용 하 여 관계 구성  

단방향 (단방향) 관계는 탐색 속성이 관계 종료 중 하나에만 정의 되 고 둘 다에서 정의 되지 않은 경우입니다. 규칙에 따라 Code First 항상 단방향 관계를 일 대 다로 해석 합니다. 예를 들어 강사 유형에만 탐색 속성이 있는 강사와 OfficeAssignment 사이에 일 대 일 관계를 사용 하려면 흐름 API를 사용 하 여이 관계를 구성 해야 합니다.  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a>계단식 삭제 사용  

WillCascadeOnDelete 메서드를 사용 하 여 관계에 대해 cascade delete를 구성할 수 있습니다. 종속 엔터티의 외래 키가 null을 허용 하지 않는 경우 Code First는 관계에서 cascade delete를 설정 합니다. 종속 엔터티의 외래 키가 null Code First을 허용 하는 경우에는 관계에서 cascade delete를 설정 하지 않고, 주 서버가 삭제 되 면 외래 키가 null로 설정 됩니다.  

다음을 사용 하 여 이러한 cascade delete 규칙을 제거할 수 있습니다.  

modelBuilder. OneToManyCascadeDeleteConvention\>()\<제거 합니다.  
modelBuilder. ManyToManyCascadeDeleteConvention\>()\<제거 합니다.  

다음 코드는 관계를 필수로 구성 하 고 cascade delete를 사용 하지 않도록 설정 합니다.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a>복합 외래 키 구성  

부서 유형의 기본 키가 DepartmentID 및 Name 속성으로 구성 된 경우에는 다음과 같이 학과의 기본 키와 과정 유형에 대 한 외래 키를 구성 합니다.  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>모델에 정의 되지 않은 외래 키 이름 바꾸기  

CLR 유형에 외래 키를 정의 하지 않고 데이터베이스에 포함 되어야 하는 이름을 지정 하려면 다음을 수행 합니다.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a>Code First 규칙을 따르지 않는 외래 키 이름 구성  

강의 클래스의 foreign key 속성이 DepartmentID 대신 SomeDepartmentID 이라고 하는 경우 다음을 수행 하 여 SomeDepartmentID를 외래 키로 지정 해야 합니다.  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a>예제에 사용 되는 모델  

다음 Code First 모델은이 페이지의 샘플에 사용 됩니다.  

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
