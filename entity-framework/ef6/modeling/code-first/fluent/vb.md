---
title: VB.NET를 사용 하는 흐름 API-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 763dc6a2-764a-4600-896c-f6f13abf56ec
ms.openlocfilehash: df3e61fa5e2d24873336511e90231a7d78d32535
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182665"
---
# <a name="fluent-api-with-vbnet"></a><span data-ttu-id="24111-102">VB.NET를 사용 하는 흐름 API</span><span class="sxs-lookup"><span data-stu-id="24111-102">Fluent API with VB.NET</span></span>
<span data-ttu-id="24111-103">Code First를 사용 하면 C\# 또는 VB.NET 클래스를 사용 하 여 모델을 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24111-103">Code First allows you to define your model using C\# or VB.NET classes.</span></span> <span data-ttu-id="24111-104">필요에 따라 클래스 및 속성의 특성을 사용 하거나 흐름 API를 사용 하 여 추가 구성을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24111-104">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span> <span data-ttu-id="24111-105">이 연습에서는 VB.NET을 사용 하 여 흐름 API 구성을 수행 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="24111-105">This walkthrough shows how to perform fluent API configuration using VB.NET.</span></span>

<span data-ttu-id="24111-106">이 페이지에서는 Code First에 대해 기본적으로 이해 하 고 있다고 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="24111-106">This page assumes you have a basic understanding of Code First.</span></span> <span data-ttu-id="24111-107">Code First에 대 한 자세한 내용은 다음 연습을 확인 하세요.</span><span class="sxs-lookup"><span data-stu-id="24111-107">Check out the following walkthroughs for more information on Code First:</span></span>

-   [<span data-ttu-id="24111-108">새 데이터베이스에 Code First</span><span class="sxs-lookup"><span data-stu-id="24111-108">Code First to a New Database</span></span>](~/ef6/modeling/code-first/workflows/new-database.md)
-   [<span data-ttu-id="24111-109">기존 데이터베이스로 Code First</span><span class="sxs-lookup"><span data-stu-id="24111-109">Code First to an Existing Database</span></span>](~/ef6/modeling/code-first/workflows/existing-database.md)

## <a name="pre-requisites"></a><span data-ttu-id="24111-110">필수 구성 요소</span><span class="sxs-lookup"><span data-stu-id="24111-110">Pre-Requisites</span></span>

<span data-ttu-id="24111-111">이 연습을 완료 하려면 Visual Studio 2010 또는 Visual Studio 2012 이상이 설치 되어 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="24111-111">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="24111-112">Visual Studio 2010을 사용 하는 경우 [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) 도 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="24111-112">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed</span></span>

## <a name="create-the-application"></a><span data-ttu-id="24111-113">애플리케이션 만들기</span><span class="sxs-lookup"><span data-stu-id="24111-113">Create the Application</span></span>

<span data-ttu-id="24111-114">간단 하 게 유지 하기 위해 Code First를 사용 하 여 데이터 액세스를 수행 하는 기본 콘솔 응용 프로그램을 빌드 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="24111-114">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="24111-115">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="24111-115">Open Visual Studio</span></span>
-   <span data-ttu-id="24111-116">**파일&gt; 새&gt; 프로젝트 ...**</span><span class="sxs-lookup"><span data-stu-id="24111-116">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="24111-117">왼쪽 메뉴 및 **콘솔 응용 프로그램** 에서 **Windows** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="24111-117">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="24111-118">이름으로 **Codefirstvbsample** 을 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="24111-118">Enter **CodeFirstVBSample** as the name</span></span>
-   <span data-ttu-id="24111-119">**확인**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="24111-119">Select **OK**</span></span>

## <a name="define-the-model"></a><span data-ttu-id="24111-120">모델 정의</span><span class="sxs-lookup"><span data-stu-id="24111-120">Define the Model</span></span>

<span data-ttu-id="24111-121">이 단계에서는 개념적 모델을 나타내는 VB.NET POCO 엔터티 형식을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="24111-121">In this step you will define VB.NET POCO entity types that represent the conceptual model.</span></span> <span data-ttu-id="24111-122">클래스는 기본 클래스에서 파생 되거나 인터페이스를 구현할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="24111-122">The classes do not need to derive from any base classes or implement any interfaces.</span></span>

-   <span data-ttu-id="24111-123">프로젝트에 새 클래스를 추가 하 고 클래스 이름으로 **SchoolModel** 를 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="24111-123">Add a new class to the project, enter **SchoolModel** for the class name</span></span>
-   <span data-ttu-id="24111-124">새 클래스의 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="24111-124">Replace the contents of the new class with the following code</span></span>

``` vb
   Public Class Department
        Public Sub New()
            Me.Courses = New List(Of Course)()
        End Sub

        ' Primary key
        Public Property DepartmentID() As Integer
        Public Property Name() As String
        Public Property Budget() As Decimal
        Public Property StartDate() As Date
        Public Property Administrator() As Integer?
        Public Overridable Property Courses() As ICollection(Of Course)
    End Class

    Public Class Course
        Public Sub New()
            Me.Instructors = New HashSet(Of Instructor)()
        End Sub

        ' Primary key
        Public Property CourseID() As Integer
        Public Property Title() As String
        Public Property Credits() As Integer

        ' Foreign  key that does not follow the Code First convention.
        ' The fluent API will be used to configure DepartmentID_FK  to be the foreign key for this entity.
        Public Property DepartmentID_FK() As Integer

        ' Navigation properties
         Public Overridable Property Department() As Department
         Public Overridable Property Instructors() As ICollection(Of Instructor)
    End Class

    Public Class OnlineCourse
        Inherits Course

        Public Property URL() As String
    End Class

    Partial Public Class OnsiteCourse
        Inherits Course

        Public Sub New()
            Details = New OnsiteCourseDetails()
        End Sub

        Public Property Details() As OnsiteCourseDetails
     End Class

    ' Complex type
    Public Class OnsiteCourseDetails
        Public Property Time() As Date
        Public Property Location() As String
        Public Property Days() As String
    End Class

    Public Class Person
        ' Primary key
        Public Property PersonID() As Integer
        Public Property LastName() As String
        Public Property FirstName() As String
    End Class

    Public Class Instructor
        Inherits Person

        Public Sub New()
            Me.Courses = New List(Of Course)()
        End Sub

        Public Property HireDate() As Date

        ' Navigation properties
        Private privateCourses As ICollection(Of Course)
        Public Overridable Property Courses() As ICollection(Of Course)
        Public Overridable Property OfficeAssignment() As OfficeAssignment
    End Class

    Public Class OfficeAssignment
        ' Primary key that does not follow the Code First convention.
        ' The HasKey method is used later to configure the primary key for the entity.
        Public Property InstructorID() As Integer

        Public Property Location() As String
        Public Property Timestamp() As Byte()

        ' Navigation property
        Public Overridable Property Instructor() As Instructor
    End Class
```

## <a name="define-a-derived-context"></a><span data-ttu-id="24111-125">파생 컨텍스트 정의</span><span class="sxs-lookup"><span data-stu-id="24111-125">Define a Derived Context</span></span>

<span data-ttu-id="24111-126">Entity Framework에서 형식을 사용 하기 시작 하므로 EntityFramework NuGet 패키지를 추가 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="24111-126">We’re about to start to using types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="24111-127">\* \* 프로젝트 – **NuGet 패키지를 관리**&gt; ...</span><span class="sxs-lookup"><span data-stu-id="24111-127">\*\*Project –&gt; **Manage NuGet Packages…**</span></span>
> [!NOTE]
> <span data-ttu-id="24111-128">**NuGet 패키지 관리** ...가 없는 경우</span><span class="sxs-lookup"><span data-stu-id="24111-128">If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="24111-129">옵션을 선택 하면 [최신 버전의 NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) 을 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="24111-129">option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="24111-130">**온라인** 탭을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="24111-130">Select the **Online** tab</span></span>
-   <span data-ttu-id="24111-131">**Entityframework** 패키지를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="24111-131">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="24111-132">**설치** 클릭</span><span class="sxs-lookup"><span data-stu-id="24111-132">Click **Install**</span></span>

<span data-ttu-id="24111-133">이제 데이터베이스와의 세션을 나타내는 파생 컨텍스트를 정의 하 여 데이터를 쿼리하고 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24111-133">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="24111-134">DbContext에서 파생 되는 컨텍스트를 정의 하 고 모델의 각 클래스에 대해 형식화 된 DbSet&lt;&gt;를 노출 합니다.</span><span class="sxs-lookup"><span data-stu-id="24111-134">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

-   <span data-ttu-id="24111-135">프로젝트에 새 클래스를 추가 하 고 클래스 이름으로 **schoolcontext.cs** 를 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="24111-135">Add a new class to the project, enter **SchoolContext** for the class name</span></span>
-   <span data-ttu-id="24111-136">새 클래스의 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="24111-136">Replace the contents of the new class with the following code</span></span>

``` vb
    Imports System.Data.Entity
    Imports System.Data.Entity.Infrastructure
    Imports System.Data.Entity.ModelConfiguration.Conventions
    Imports System.ComponentModel.DataAnnotations
    Imports System.ComponentModel.DataAnnotations.Schema

    Public Class SchoolContext
        Inherits DbContext

        Public Property OfficeAssignments() As DbSet(Of OfficeAssignment)
        Public Property Instructors() As DbSet(Of Instructor)
        Public Property Courses() As DbSet(Of Course)
        Public Property Departments() As DbSet(Of Department)

        Protected Overrides Sub OnModelCreating(ByVal modelBuilder As DbModelBuilder)
        End Sub
    End Class
```

## <a name="configuring-with-the-fluent-api"></a><span data-ttu-id="24111-137">흐름 API를 사용 하 여 구성</span><span class="sxs-lookup"><span data-stu-id="24111-137">Configuring with the Fluent API</span></span>

<span data-ttu-id="24111-138">이 섹션에서는 흐름 Api를 사용 하 여 테이블 매핑, 속성에 대 한 속성 매핑 및 테이블 간의 관계를 모델 형식\\구성 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="24111-138">This section demonstrates how to use the fluent APIs to configure types to tables mapping, properties to columns mapping, and relationships between tables\\type in your model.</span></span> <span data-ttu-id="24111-139">흐름 API는 **Dbmodelbuilder** 형식을 통해 노출 되며 **DbContext**에서 **onmodelcreating** 메서드를 재정의 하 여 가장 일반적으로 액세스 합니다.</span><span class="sxs-lookup"><span data-stu-id="24111-139">The fluent API is exposed through the **DbModelBuilder** type and is most commonly accessed by overriding the **OnModelCreating** method on **DbContext**.</span></span>

-   <span data-ttu-id="24111-140">다음 코드를 복사 하 여 **schoolcontext.cs** 클래스에 정의 된 **onmodelcreating** 메서드에 추가 합니다. 주석은 각 매핑의 의미를 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="24111-140">Copy the following code and add it to the **OnModelCreating** method defined on the **SchoolContext** class The comments explain what each mapping does</span></span>

``` vb
' Configure Code First to ignore PluralizingTableName convention
' If you keep this convention then the generated tables
' will have pluralized names.
modelBuilder.Conventions.Remove(Of PluralizingTableNameConvention)()


' Specifying that a Class is a Complex Type

' The model defined in this topic defines a type OnsiteCourseDetails.
' By convention, a type that has no primary key specified
' is treated as a complex type.
' There are some scenarios where Code First will not
' detect a complex type (for example, if you do have a property
' called ID, but you do not mean for it to be a primary key).
' In such cases, you would use the fluent API to
' explicitly specify that a type is a complex type.
modelBuilder.ComplexType(Of OnsiteCourseDetails)()


' Mapping a CLR Entity Type to a Specific Table in the Database.

' All properties of OfficeAssignment will be mapped
' to columns  in a table called t_OfficeAssignment.
modelBuilder.Entity(Of OfficeAssignment)().ToTable("t_OfficeAssignment")


' Mapping the Table-Per-Hierarchy (TPH) Inheritance

' In the TPH mapping scenario, all types in an inheritance hierarchy
' are mapped to a single table.
' A discriminator column is used to identify the type of each row.
' When creating your model with Code First,      
' TPH is the default strategy for the types that
' participate in the inheritance hierarchy.
' By default, the discriminator column is added
' to the table with the name “Discriminator”
' and the CLR type name of each type in the hierarchy
' is used for the discriminator values.
' You can modify the default behavior by using the fluent API.
modelBuilder.Entity(Of Person)().
    Map(Of Person)(Function(t) t.Requires("Type").
        HasValue("Person")).
        Map(Of Instructor)(Function(t) t.Requires("Type").
        HasValue("Instructor"))


' Mapping the Table-Per-Type (TPT) Inheritance

' In the TPT mapping scenario, all types are mapped to individual tables.
' Properties that belong solely to a base type or derived type are stored
' in a table that maps to that type. Tables that map to derived types
' also store a foreign key that joins the derived table with the base table.
modelBuilder.Entity(Of Course)().ToTable("Course")
modelBuilder.Entity(Of OnsiteCourse)().ToTable("OnsiteCourse")
modelBuilder.Entity(Of OnlineCourse)().ToTable("OnlineCourse")


' Configuring a Primary Key

' If your class defines a property whose name is “ID” or “Id”,
' or a class name followed by “ID” or “Id”,
' the Entity Framework treats this property as a primary key by convention.
' If your property name does not follow this pattern, use the HasKey method
' to configure the primary key for the entity.
modelBuilder.Entity(Of OfficeAssignment)().
    HasKey(Function(t) t.InstructorID)


' Specifying the Maximum Length on a Property

' In the following example, the Name property
' should be no longer than 50 characters.
' If you make the value longer than 50 characters,
' you will get a DbEntityValidationException exception.
modelBuilder.Entity(Of Department)().Property(Function(t) t.Name).
    HasMaxLength(60)


' Configuring the Property to be Required

' In the following example, the Name property is required.
' If you do not specify the Name,
' you will get a DbEntityValidationException exception.
' The database column used to store this property will be non-nullable.
modelBuilder.Entity(Of Department)().Property(Function(t) t.Name).
    IsRequired()


' Switching off Identity for Numeric Primary Keys

' The following example sets the DepartmentID property to
' System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that
' the value will not be generated by the database.
modelBuilder.Entity(Of Course)().Property(Function(t) t.CourseID).
    HasDatabaseGeneratedOption(DatabaseGeneratedOption.None)

'Specifying NOT to Map a CLR Property to a Column in the Database
 modelBuilder.Entity(Of Department)().
     Ignore(Function(t) t.Administrator)

'Mapping a CLR Property to a Specific Column in the Database
 modelBuilder.Entity(Of Department)().Property(Function(t) t.Budget).
     HasColumnName("DepartmentBudget")

'Configuring the Data Type of a Database Column
 modelBuilder.Entity(Of Department)().Property(Function(t) t.Name).
     HasColumnType("varchar")

'Configuring Properties on a Complex Type
modelBuilder.Entity(Of OnsiteCourse)().Property(Function(t) t.Details.Days).
    HasColumnName("Days")
modelBuilder.Entity(Of OnsiteCourse)().Property(Function(t) t.Details.Location).
    HasColumnName("Location")
modelBuilder.Entity(Of OnsiteCourse)().Property(Function(t) t.Details.Time).
    HasColumnName("Time")


' Map one-to-zero or one relationship

' The OfficeAssignment has the InstructorID
' property that is a primary key and a foreign key.
modelBuilder.Entity(Of OfficeAssignment)().
    HasRequired(Function(t) t.Instructor).
    WithOptional(Function(t) t.OfficeAssignment)


' Configuring a Many-to-Many Relationship

' The following code configures a many-to-many relationship
' between the Course  and Instructor types.
' In the following example, the default Code First conventions
' are used  to create a join table.
' As a result the CourseInstructor table is created with
' Course_CourseID  and Instructor_InstructorID columns.
modelBuilder.Entity(Of Course)().
    HasMany(Function(t) t.Instructors).
    WithMany(Function(t) t.Courses)


' Configuring a Many-to-Many Relationship and specifying the names
' of the columns in the join table

' If you want to specify the join table name
' and the names of the columns in the table
' you need to do additional configuration by using the Map method.
' The following code generates the CourseInstructor
' table with CourseID and InstructorID columns.
modelBuilder.Entity(Of Course)().
    HasMany(Function(t) t.Instructors).
    WithMany(Function(t) t.Courses).
    Map(Sub(m)
            m.ToTable("CourseInstructor")
            m.MapLeftKey("CourseID")
            m.MapRightKey("InstructorID")
        End Sub)


' Configuring a foreign key name that does not follow the Code First convention

' The foreign key property on the Course class is called DepartmentID_FK
' since that does not follow Code First conventions you need to explicitly specify
' that you want DepartmentID_FK to be the foreign key.
modelBuilder.Entity(Of Course)().
    HasRequired(Function(t) t.Department).
    WithMany(Function(t) t.Courses).
    HasForeignKey(Function(t) t.DepartmentID_FK)


' Enabling Cascade Delete

' By default, if a foreign key on the dependent entity is not nullable,
' then Code First sets cascade delete on the relationship.
' If a foreign key on the dependent entity is nullable,
' Code First does not set cascade delete on the relationship,
' and when the principal is deleted the foreign key will be set to null.
' The following code configures cascade delete on the relationship.

' You can also remove the cascade delete conventions by using:
' modelBuilder.Conventions.Remove<OneToManyCascadeDeleteConvention>()
' and modelBuilder.Conventions.Remove<ManyToManyCascadeDeleteConvention>().
modelBuilder.Entity(Of Course)().
    HasRequired(Function(t) t.Department).
    WithMany(Function(t) t.Courses).
    HasForeignKey(Function(d) d.DepartmentID_FK).
    WillCascadeOnDelete(False)
```

## <a name="using-the-model"></a><span data-ttu-id="24111-141">모델 사용</span><span class="sxs-lookup"><span data-stu-id="24111-141">Using the Model</span></span>

<span data-ttu-id="24111-142">**Schoolcontext.cs** 를 사용 하 여 일부 데이터 액세스를 수행 하 여 작동 중인 모델을 확인해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="24111-142">Let's perform some data access using the **SchoolContext** to see out model in action.</span></span>

-   <span data-ttu-id="24111-143">Main 함수가 정의 된 module1.vb 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="24111-143">Open the Module1.vb file where the Main function is defined</span></span>
-   <span data-ttu-id="24111-144">다음의 Module1 정의를 복사 하 여 붙여넣습니다.</span><span class="sxs-lookup"><span data-stu-id="24111-144">Copy and paste the following Module1 definition</span></span>

``` vb
Imports System.Data.Entity

Module Module1

    Sub Main()

    Using context As New SchoolContext()

            ' Create and save a new Department.
            Console.Write("Enter a name for a new Department: ")
            Dim name = Console.ReadLine()

            Dim department = New Department With { .Name = name, .StartDate = DateTime.Now }
            context.Departments.Add(department)
            context.SaveChanges()

            ' Display all Departments from the database ordered by name
            Dim departments =
                From d In context.Departments
                Order By d.Name
                Select d

            Console.WriteLine("All Departments in the database:")
            For Each department In departments
                Console.WriteLine(department.Name)
            Next

        End Using

        Console.WriteLine("Press any key to exit...")
        Console.ReadKey()

    End Sub

End Module
```

<span data-ttu-id="24111-145">이제 응용 프로그램을 실행 하 고 테스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24111-145">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Department: Computing
All Departments in the database:
Computing
Press any key to exit...
```
