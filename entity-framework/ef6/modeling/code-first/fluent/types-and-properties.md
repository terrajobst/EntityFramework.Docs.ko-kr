---
title: Fluent API 구성 및 등록 정보 및 형식 매핑-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: e65a3f4721e5c28de63d143e1143f3584e145477
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996989"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a>Fluent API-구성 하 고 속성 및 형식 매핑
Entity Framework Code First를 사용 하 여 작업 하는 경우 기본 동작은 POCO 클래스는 EF에 포함 하는 규칙 집합을 사용 하 여 테이블에 매핑할 합니다. 그러나 경우에 따라 없거나 하지 않으려는 해당 규칙을 따르는 및 규칙을 지정 하는 새로운 이외의에 엔터티를 매핑해야 합니다.  

두 가지 기본 규칙 이외의 즉 사용 하는 EF를 구성할 수 있습니다 [주석을](~/ef6/modeling/code-first/data-annotations.md) 또는 EFs fluent API. 주석을 사용 하 여 얻을 수 없는 매핑 시나리오는 주석을 흐름 API 기능의 하위 집합을 설명 합니다. 이 문서는 fluent API를 사용 하 여 속성을 구성 하는 방법을 보여 주기 위해 설계 되었습니다.  

Code first fluent API에는 가장 일반적으로 재정의 하 여 액세스 하는 [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) 에서 파생 된 메서드 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx)합니다. 다음 샘플 코드를 복사 하 고로 사용 하 여 사용 되는 모델을 확인 하려는 경우에 모델에 맞게 사용자 지정할 수 있습니다 및 fluent api 사용 하 여 다양 한 작업을 수행 하는 방법을 만들어진-가이 문서의 끝에 제공 됩니다.  

## <a name="model-wide-settings"></a>모델 수준 설정  

### <a name="default-schema-ef6-onwards"></a>기본 스키마 (EF6부터 해당)  

EF6을 사용 하 여 시작 수 메서드를 사용 HasDefaultSchema DbModelBuilder에 모든 테이블, 저장된 프로시저 등을 사용 하도록 데이터베이스 스키마를 지정 합니다. 이 기본 설정에 다른 스키마를 명시적으로 구성 하는 모든 개체에 대 한 재정의 됩니다.  

``` csharp
modelBuilder.HasDefaultSchema(“sales”);
```  

### <a name="custom-conventions-ef6-onwards"></a>사용자 지정 규칙 (EF6부터 해당)  

Code First에 포함 된 것을 보완 하기 위해 사용자 고유의 규칙을 만들 수 있습니다 EF6부터 시작 합니다. 자세한 내용은 참조 하세요. [사용자 지정 코드의 첫 번째 규칙](~/ef6/modeling/code-first/conventions/custom.md)합니다.  

## <a name="property-mapping"></a>속성 매핑  

합니다 [속성](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) 메서드를 사용 하는 엔터티 또는 복합 형식에 속하는 각 속성에 대 한 특성을 구성 합니다. 속성 메서드는 지정된 된 속성에 대 한 구성 개체를 가져오기 위해 사용 됩니다. 구성 개체의 옵션을; 구성 되는 형식에 따라 다릅니다. 예를 들어 IsUnicode는 문자열 속성 에서만 사용할 수 있습니다.  

### <a name="configuring-a-primary-key"></a>기본 키를 구성합니다.  

기본 키에 대 한 Entity Framework 규칙이입니다.  

1. 해당 이름은 "ID" 또는 "Id" 속성을 정의 하는 클래스  
2. 클래스 이름 뒤에 "ID" 또는 "Id" 또는  

기본 키 속성을 명시적으로 설정 하려면 HasKey 메서드를 사용할 수 있습니다. 다음 예제에서는 HasKey 메서드는 OfficeAssignment 유형에 따라 InstructorID 기본 키를 구성 하려면 사용 됩니다.  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a>복합 기본 키를 구성합니다.  

다음 예에서는 부서 형식의 복합 기본 키로 DepartmentID 및 이름 속성을 구성 합니다.  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a>숫자 기본 키에 대 한 id 전환  

다음 예에서는를 나타내는 값을 데이터베이스에서 생성 되지 것입니다 System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None를 DepartmentID 속성을 설정 합니다.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a>속성의 최대 길이 지정  

다음 예제에서는 Name 속성 50 자 이하의 해야 합니다. 50 자 보다 긴 값을 확인 하는 경우는 [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) 예외입니다. 이 모델에서 Code First는 데이터베이스를 만드는 경우 50 자로 이름 열의 최대 길이 설정 됩니다.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a>속성을 구성 합니다.  

다음 예에는 Name 속성이 필요 합니다. 이름을 지정 하지 않으면, DbEntityValidationException 예외를 얻게 됩니다. 이 모델에서 Code First는 데이터베이스를 만드는 경우 다음이 속성을 저장 하는 데 일반적으로 됩니다 nullable이 아닌 합니다.  

> [!NOTE]
> 경우에 따라 속성이 필요한 경우에 null이 아닌 수를 데이터베이스에 열 수 없습니다. 예를 들어 경우 TPH 상속 전략 데이터를 사용 하 여 여러 형식에 대 한에 저장 됩니다 단일 테이블. 파생된 형식이 필요한 속성을 포함 하는 경우 열 있으므로 할 수 없습니다 nullable이 아닌 계층의 모든 형식이 아닌이 속성이 없습니다.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a>하나 이상의 속성에서 인덱스를 구성합니다.  

> [!NOTE]
> **EF6.1 이상만** -The 인덱스 특성은 Entity Framework 6.1에서 도입 되었습니다. 이전 버전을 사용 하는 경우이 섹션의 정보는 적용 되지 않습니다.  

할 수 있습니다 하지만 Fluent API에 의해 고유 하 게 지원 되지 않습니다 인덱스 만들기에 대 한 지원 사용 **IndexAttribute** Fluent API를 통해. 인덱스 특성은 다음 파이프라인의 뒷부분에 나오는 데이터베이스에서 인덱스에 설정 된 모델에는 모델 주석을 포함 하 여 처리 됩니다. 수동으로 주석을 추가할 수 있습니다 이러한 동일한 Fluent API를 사용 합니다.  

이 작업을 수행 하는 가장 쉬운 방법은의 인스턴스를 만드는 방법은 **IndexAttribute** 새 인덱스에 대 한 모든 설정이 포함 된 합니다. 인스턴스를 만들 수 있습니다 **IndexAnnotation** 변환 하는 EF 특정 형식인 합니다 **IndexAttribute** EF 모델에 저장할 수 있는 모델 주석으로 설정 합니다. 이러한 다음 전달할 수 있습니다는 **HasColumnAnnotation** 메서드를 사용 하지 마십시오 Fluent API에 대해 **인덱스** 주석에 대 한 합니다.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

사용할 수 있는 설정의 전체 목록은 **IndexAttribute**를 참조 합니다 *인덱스* 부분 [Code First 데이터 주석](~/ef6/modeling/code-first/data-annotations.md). 인덱스 이름 사용자 지정, 고유 인덱스 만들기 및 다중 열 인덱스를 만드는 것이 여기 있습니다.  

배열을 전달 하 여 단일 속성에 여러 인덱스가 주석을 지정할 수 있습니다 **IndexAttribute** 의 생성자에 게 **IndexAnnotation**합니다.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation(
        "Index",  
        new IndexAnnotation(new[]
            {
                new IndexAttribute("Index1"),
                new IndexAttribute("Index2") { IsUnique = true }
            })));
```  

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a>데이터베이스에서 열을 CLR 속성을 매핑할 필요가 지정  

다음 예에서는 CLR 형식의 속성을 데이터베이스의 열에 매핑되지 않은 지정 하는 방법을 보여 줍니다.  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a>CLR 속성을 데이터베이스의 특정 열에 매핑  

다음 예제에서는 DepartmentName 데이터베이스 열에 이름을 CLR 속성을 매핑합니다.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>모델에서 정의 되지 않은 외래 키 이름 바꾸기  

CLR 형식에서 외래 키를 정의 하지만 데이터베이스에 있어야 하는 이름 지정을 하지 않으려는 경우 다음을 수행 합니다.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a>문자열 속성을 유니코드 콘텐츠를 지원 하는지 여부를 구성 합니다.  

기본적으로 문자열이 유니코드 (SQL Server에서 nvarchar) 됩니다. Varchar 형식 문자열로 되도록 지정 하려면 IsUnicode 메서드를 사용할 수 있습니다.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a>데이터베이스 열의 데이터 형식을 구성합니다.  

합니다 [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) 메서드 같은 기본 형식의 다른 표현에 매핑할 수 있도록 합니다. 이 메서드를 사용 하 여 프로그래머가 아닌 런타임에 데이터 변환을 수행할 수 있습니다. 데이터베이스 관계가 없는 것과 IsUnicode 하는 방법은 기본 설정은 열의 varchar, note 합니다.  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a>복합 형식에 속성을 구성합니다.  

복합 형식에 대해 스칼라 속성을 구성 하는 방법은 두 가지가 있습니다.  

ComplexTypeConfiguration에서 속성을 호출할 수 있습니다.  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

또한 복합 형식의 속성에 액세스 하려면 점 표기법을 사용할 수 있습니다.  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a>낙관적 동시성 토큰으로 사용할 속성 구성  

엔터티의 속성을 나타낸다고 동시성 토큰을 지정 하려면 ConcurrencyCheck 특성 또는 IsConcurrencyToken 메서드를 사용할 수 있습니다.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

또한 데이터베이스에서 행 버전이 되도록 속성을 구성 하려면 IsRowVersion 메서드를 사용할 수 있습니다. 행 버전에는 낙관적 동시성 토큰이 되도록 항목을 자동으로 구성 되도록 속성을 설정 합니다.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a>형식 매핑  

### <a name="specifying-that-a-class-is-a-complex-type"></a>클래스는 복합 형식 지정  

규칙에 따라 지정 된 기본 키를 갖는 형식에 복합 형식으로 처리 됩니다. 일부의 시나리오가 없는 Code First 감지 하지 복합 형식 (예를 들어 수행 ID 라는 속성이 없지만 경우 기본 키에 대 한 의미 하지는 않습니다). 이러한 경우에 fluent API를 사용 하 여 명시적으로 형식을 복합 형식 인지를 지정 하는 있습니다.  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a>CLR 엔터티 형식을 데이터베이스에서 테이블을 매핑할 필요가 지정  

다음 예제에서 데이터베이스의 테이블에 매핑되는 CLR 형식을 제외 하는 방법을 보여 줍니다.  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a>데이터베이스의 특정 테이블에 엔터티 형식 매핑  

부서의 모든 속성 t_ 부서를 호출 하는 테이블의 열에 매핑됩니다.  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

또한 다음과 같은 스키마 이름을 지정할 수 있습니다.  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a>테이블-당-TPH (계층) 상속 매핑  

TPH 매핑 시나리오에서 상속 계층 구조의 모든 형식은 단일 테이블에 매핑됩니다. 판별자 열은 각 행의 형식을 식별 하기 위해 사용 됩니다. Code First를 사용 하 여 모델을 만들 때 TPH 상속 계층 구조에 참여 하는 형식에 대 한 기본 전략입니다. 기본적으로 판별자 열 이름 "Discriminator" 테이블에 추가 됩니다 하 고 계층의 각 형식의 CLR 형식 이름을 판별자 값에 사용 됩니다. Fluent API를 사용 하 여 기본 동작을 수정할 수 있습니다.  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a>매핑 테이블 형식당 (TPT) 상속  

TPT 매핑 시나리오에서 모든 형식은 개별 테이블에 매핑됩니다. 기본 형식 또는 파생 형식에만 속하는 속성은 해당 형식에 매핑되는 테이블에 저장됩니다. 파생 형식에 매핑되는 테이블에는 또한 기본 테이블을 사용 하 여 파생된 테이블을 조인 하는 외래 키를 저장 합니다.  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a>구체적 형식당 테이블 클래스 (TPC) 상속 매핑  

TPC 매핑 시나리오에서 계층의 모든 비추상 형식은 개별 테이블에 매핑됩니다. 파생된 클래스에 매핑되는 테이블 데이터베이스에서 기본 클래스에 매핑되는 테이블에는 관계가 없습니다. 상속 된 속성을 포함 한 클래스의 모든 속성은 해당 테이블의 열에 매핑됩니다.  

각 파생된 형식을 구성 하는 MapInheritedProperties 메서드를 호출 합니다. MapInheritedProperties 파생된 클래스에 대 한 테이블에 새 열에 기본 클래스에서 상속 된 모든 속성을 다시 매핑합니다.  

> [!NOTE]
> TPC 상속 계층 구조에 참여 하는 테이블을 공유 하지 않기 때문에 기본 키 되도록 중복 엔터티 키 동일한 id 초기값을 사용 하 여 데이터베이스에서 생성 된 값이 있는 경우 서브 클래스에 매핑되는 테이블에 삽입 하는 경우 note 합니다. 이 문제를 해결 하려면 각 테이블에 대 한 다른 초기 시드 값을 지정 하거나 기본 키 속성에 대 한 id입니다. Id는 정수 키 속성에 대 한 기본값은 Code First를 사용 하 여 작업 하는 경우입니다.  

``` csharp
modelBuilder.Entity<Course>()
    .Property(c => c.CourseID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);

modelBuilder.Entity<OnsiteCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnsiteCourse");
});

modelBuilder.Entity<OnlineCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnlineCourse");
});
```  

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a>엔터티 형식의 속성 (엔터티 분할) 데이터베이스의 여러 테이블에 매핑  

엔터티 분할을 여러 테이블을 분산할 수 엔터티 형식의 속성을 허용 합니다. 다음 예제에서는 부서 엔터티를 두 테이블로 분할 됩니다: 부서 및 DepartmentDetails 합니다. 엔터티 분할은 특정 테이블에 속성 하위 집합을 매핑할 Map 메서드를 여러 번 호출을 사용 합니다.  

``` csharp
modelBuilder.Entity<Department>()
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Name });
        m.ToTable("Department");
    })
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Administrator, t.StartDate, t.Budget });
        m.ToTable("DepartmentDetails");
    });
```  

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a>여러 엔터티 형식 (테이블 분할) 데이터베이스의 테이블에 매핑  

다음 예제에서는 한 테이블에 기본 키를 공유 하는 두 개의 엔터티 형식을 매핑합니다.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a>Insert/Update/Delete 저장 프로시저 (EF6부터)에 엔터티 형식 매핑  

EF6을 사용 하 여 시작 엔터티 삽입 업데이트에 대 한 저장된 프로시저를 사용 하 고 삭제를 매핑할 수 있습니다. 자세한 내용을 참조 하세요 [첫 번째 Insert/Update/Delete 저장 프로시저 코드](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md)합니다.  

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
