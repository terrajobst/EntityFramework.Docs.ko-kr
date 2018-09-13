---
title: Fluent API 구성 및 등록 정보 및 형식 매핑-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: 031376d2fc4778e6f0fa2434ab7ccfd45d436c4a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490201"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a><span data-ttu-id="91de3-102">Fluent API-구성 하 고 속성 및 형식 매핑</span><span class="sxs-lookup"><span data-stu-id="91de3-102">Fluent API - Configuring and Mapping Properties and Types</span></span>
<span data-ttu-id="91de3-103">Entity Framework Code First를 사용 하 여 작업 하는 경우 기본 동작은 POCO 클래스는 EF에 포함 하는 규칙 집합을 사용 하 여 테이블에 매핑할 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-103">When working with Entity Framework Code First the default behavior is to map your POCO classes to tables using a set of conventions baked into EF.</span></span> <span data-ttu-id="91de3-104">그러나 경우에 따라 없거나 하지 않으려는 해당 규칙을 따르는 및 규칙을 지정 하는 새로운 이외의에 엔터티를 매핑해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-104">Sometimes, however, you cannot or do not want to follow those conventions and need to map entities to something other than what the conventions dictate.</span></span>  

<span data-ttu-id="91de3-105">두 가지 기본 규칙 이외의 즉 사용 하는 EF를 구성할 수 있습니다 [주석을](~/ef6/modeling/code-first/data-annotations.md) 또는 EFs fluent API.</span><span class="sxs-lookup"><span data-stu-id="91de3-105">There are two main ways you can configure EF to use something other than conventions, namely [annotations](~/ef6/modeling/code-first/data-annotations.md) or EFs fluent API.</span></span> <span data-ttu-id="91de3-106">주석을 사용 하 여 얻을 수 없는 매핑 시나리오는 주석을 흐름 API 기능의 하위 집합을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-106">The annotations only cover a subset of the fluent API functionality, so there are mapping scenarios that cannot be achieved using annotations.</span></span> <span data-ttu-id="91de3-107">이 문서는 fluent API를 사용 하 여 속성을 구성 하는 방법을 보여 주기 위해 설계 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-107">This article is designed to demonstrate how to use the fluent API to configure properties.</span></span>  

<span data-ttu-id="91de3-108">Code first fluent API에는 가장 일반적으로 재정의 하 여 액세스 하는 [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) 에서 파생 된 메서드 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx)합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-108">The code first fluent API is most commonly accessed by overriding the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) method on your derived [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span></span> <span data-ttu-id="91de3-109">다음 샘플 코드를 복사 하 고로 사용 하 여 사용 되는 모델을 확인 하려는 경우에 모델에 맞게 사용자 지정할 수 있습니다 및 fluent api 사용 하 여 다양 한 작업을 수행 하는 방법을 만들어진-가이 문서의 끝에 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-109">The following samples are designed to show how to do various tasks with the fluent api and allow you to copy the code out and customize it to suit your model, if you wish to see the model that they can be used with as-is then it is provided at the end of this article.</span></span>  

## <a name="model-wide-settings"></a><span data-ttu-id="91de3-110">모델 수준 설정</span><span class="sxs-lookup"><span data-stu-id="91de3-110">Model-Wide Settings</span></span>  

### <a name="default-schema-ef6-onwards"></a><span data-ttu-id="91de3-111">기본 스키마 (EF6부터 해당)</span><span class="sxs-lookup"><span data-stu-id="91de3-111">Default Schema (EF6 onwards)</span></span>  

<span data-ttu-id="91de3-112">EF6을 사용 하 여 시작 수 메서드를 사용 HasDefaultSchema DbModelBuilder에 모든 테이블, 저장된 프로시저 등을 사용 하도록 데이터베이스 스키마를 지정 합니다. 이 기본 설정에 다른 스키마를 명시적으로 구성 하는 모든 개체에 대 한 재정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-112">Starting with EF6 you can use the HasDefaultSchema method on DbModelBuilder to specify the database schema to use for all tables, stored procedures, etc. This default setting will be overridden for any objects that you explicitly configure a different schema for.</span></span>  

``` csharp
modelBuilder.HasDefaultSchema(“sales”);
```  

### <a name="custom-conventions-ef6-onwards"></a><span data-ttu-id="91de3-113">사용자 지정 규칙 (EF6부터 해당)</span><span class="sxs-lookup"><span data-stu-id="91de3-113">Custom Conventions (EF6 onwards)</span></span>  

<span data-ttu-id="91de3-114">Code First에 포함 된 것을 보완 하기 위해 사용자 고유의 규칙을 만들 수 있습니다 EF6부터 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-114">Starting with EF6 you can create your own conventions to supplement the ones included in Code First.</span></span> <span data-ttu-id="91de3-115">자세한 내용은 참조 하세요. [사용자 지정 코드의 첫 번째 규칙](~/ef6/modeling/code-first/conventions/custom.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-115">For more details, see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>  

## <a name="property-mapping"></a><span data-ttu-id="91de3-116">속성 매핑</span><span class="sxs-lookup"><span data-stu-id="91de3-116">Property Mapping</span></span>  

<span data-ttu-id="91de3-117">합니다 [속성](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) 메서드를 사용 하는 엔터티 또는 복합 형식에 속하는 각 속성에 대 한 특성을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-117">The [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) method is used to configure attributes for each property belonging to an entity or complex type.</span></span> <span data-ttu-id="91de3-118">속성 메서드는 지정된 된 속성에 대 한 구성 개체를 가져오기 위해 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-118">The Property method is used to obtain a configuration object for a given property.</span></span> <span data-ttu-id="91de3-119">구성 개체의 옵션을; 구성 되는 형식에 따라 다릅니다. 예를 들어 IsUnicode는 문자열 속성 에서만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-119">The options on the configuration object are specific to the type being configured; IsUnicode is available only on string properties for example.</span></span>  

### <a name="configuring-a-primary-key"></a><span data-ttu-id="91de3-120">기본 키를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-120">Configuring a Primary Key</span></span>  

<span data-ttu-id="91de3-121">기본 키에 대 한 Entity Framework 규칙이입니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-121">The Entity Framework convention for primary keys is:</span></span>  

1. <span data-ttu-id="91de3-122">해당 이름은 "ID" 또는 "Id" 속성을 정의 하는 클래스</span><span class="sxs-lookup"><span data-stu-id="91de3-122">Your class defines a property whose name is “ID” or “Id”</span></span>  
2. <span data-ttu-id="91de3-123">클래스 이름 뒤에 "ID" 또는 "Id" 또는</span><span class="sxs-lookup"><span data-stu-id="91de3-123">or a class name followed by “ID” or “Id”</span></span>  

<span data-ttu-id="91de3-124">기본 키 속성을 명시적으로 설정 하려면 HasKey 메서드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-124">To explicitly set a property to be a primary key, you can use the HasKey method.</span></span> <span data-ttu-id="91de3-125">다음 예제에서는 HasKey 메서드는 OfficeAssignment 유형에 따라 InstructorID 기본 키를 구성 하려면 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-125">In the following example, the HasKey method is used to configure the InstructorID primary key on the OfficeAssignment type.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a><span data-ttu-id="91de3-126">복합 기본 키를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-126">Configuring a Composite Primary Key</span></span>  

<span data-ttu-id="91de3-127">다음 예에서는 부서 형식의 복합 기본 키로 DepartmentID 및 이름 속성을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-127">The following example configures the DepartmentID and Name properties to be the composite primary key of the Department type.</span></span>  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a><span data-ttu-id="91de3-128">숫자 기본 키에 대 한 id 전환</span><span class="sxs-lookup"><span data-stu-id="91de3-128">Switching off Identity for Numeric Primary Keys</span></span>  

<span data-ttu-id="91de3-129">다음 예에서는를 나타내는 값을 데이터베이스에서 생성 되지 것입니다 System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None를 DepartmentID 속성을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-129">The following example sets the DepartmentID property to System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that the value will not be generated by the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a><span data-ttu-id="91de3-130">속성의 최대 길이 지정</span><span class="sxs-lookup"><span data-stu-id="91de3-130">Specifying the Maximum Length on a Property</span></span>  

<span data-ttu-id="91de3-131">다음 예제에서는 Name 속성 50 자 이하의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-131">In the following example, the Name property should be no longer than 50 characters.</span></span> <span data-ttu-id="91de3-132">50 자 보다 긴 값을 확인 하는 경우는 [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) 예외입니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-132">If you make the value longer than 50 characters, you will get a [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception.</span></span> <span data-ttu-id="91de3-133">이 모델에서 Code First는 데이터베이스를 만드는 경우 50 자로 이름 열의 최대 길이 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-133">If Code First creates a database from this model it will also set the maximum length of the Name column to 50 characters.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a><span data-ttu-id="91de3-134">속성을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-134">Configuring the Property to be Required</span></span>  

<span data-ttu-id="91de3-135">다음 예에는 Name 속성이 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-135">In the following example, the Name property is required.</span></span> <span data-ttu-id="91de3-136">이름을 지정 하지 않으면, DbEntityValidationException 예외를 얻게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-136">If you do not specify the Name, you will get a DbEntityValidationException exception.</span></span> <span data-ttu-id="91de3-137">이 모델에서 Code First는 데이터베이스를 만드는 경우 다음이 속성을 저장 하는 데 일반적으로 됩니다 nullable이 아닌 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-137">If Code First creates a database from this model then the column used to store this property will usually be non-nullable.</span></span>  

> [!NOTE]
> <span data-ttu-id="91de3-138">경우에 따라 속성이 필요한 경우에 null이 아닌 수를 데이터베이스에 열 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-138">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="91de3-139">예를 들어 경우 TPH 상속 전략 데이터를 사용 하 여 여러 형식에 대 한에 저장 됩니다 단일 테이블.</span><span class="sxs-lookup"><span data-stu-id="91de3-139">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="91de3-140">파생된 형식이 필요한 속성을 포함 하는 경우 열 있으므로 할 수 없습니다 nullable이 아닌 계층의 모든 형식이 아닌이 속성이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-140">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a><span data-ttu-id="91de3-141">하나 이상의 속성에서 인덱스를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-141">Configuring an Index on one or more properties</span></span>  

> [!NOTE]
> <span data-ttu-id="91de3-142">**EF6.1 이상만** -The 인덱스 특성은 Entity Framework 6.1에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-142">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="91de3-143">이전 버전을 사용 하는 경우이 섹션의 정보는 적용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-143">If you are using an earlier version the information in this section does not apply.</span></span>  

<span data-ttu-id="91de3-144">할 수 있습니다 하지만 Fluent API에 의해 고유 하 게 지원 되지 않습니다 인덱스 만들기에 대 한 지원 사용 **IndexAttribute** Fluent API를 통해.</span><span class="sxs-lookup"><span data-stu-id="91de3-144">Creating indexes isn't natively supported by the Fluent API, but you can make use of the support for **IndexAttribute** via the Fluent API.</span></span> <span data-ttu-id="91de3-145">인덱스 특성은 다음 파이프라인의 뒷부분에 나오는 데이터베이스에서 인덱스에 설정 된 모델에는 모델 주석을 포함 하 여 처리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-145">Index attributes are processed by including a model annotation on the model that is then turned into an Index in the database later in the pipeline.</span></span> <span data-ttu-id="91de3-146">수동으로 주석을 추가할 수 있습니다 이러한 동일한 Fluent API를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-146">You can manually add these same annotations using the Fluent API.</span></span>  

<span data-ttu-id="91de3-147">이 작업을 수행 하는 가장 쉬운 방법은의 인스턴스를 만드는 방법은 **IndexAttribute** 새 인덱스에 대 한 모든 설정이 포함 된 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-147">The easiest way to do this is to create an instance of **IndexAttribute** that contains all the settings for the new index.</span></span> <span data-ttu-id="91de3-148">인스턴스를 만들 수 있습니다 **IndexAnnotation** 변환 하는 EF 특정 형식인 합니다 **IndexAttribute** EF 모델에 저장할 수 있는 모델 주석으로 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-148">You can then create an instance of **IndexAnnotation** which is an EF specific type that will convert the **IndexAttribute** settings into a model annotation that can be stored on the EF model.</span></span> <span data-ttu-id="91de3-149">이러한 다음 전달할 수 있습니다는 **HasColumnAnnotation** 메서드를 사용 하지 마십시오 Fluent API에 대해 **인덱스** 주석에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-149">These can then be passed to the **HasColumnAnnotation** method on the Fluent API, specifying the name **Index** for the annotation.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

<span data-ttu-id="91de3-150">사용할 수 있는 설정의 전체 목록은 **IndexAttribute**를 참조 합니다 *인덱스* 부분 [Code First 데이터 주석](~/ef6/modeling/code-first/data-annotations.md).</span><span class="sxs-lookup"><span data-stu-id="91de3-150">For a complete list of the settings available in **IndexAttribute**, see the *Index* section of [Code First Data Annotations](~/ef6/modeling/code-first/data-annotations.md).</span></span> <span data-ttu-id="91de3-151">인덱스 이름 사용자 지정, 고유 인덱스 만들기 및 다중 열 인덱스를 만드는 것이 여기 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-151">This includes customizing the index name, creating unique indexes, and creating multi-column indexes.</span></span>  

<span data-ttu-id="91de3-152">배열을 전달 하 여 단일 속성에 여러 인덱스가 주석을 지정할 수 있습니다 **IndexAttribute** 의 생성자에 게 **IndexAnnotation**합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-152">You can specify multiple index annotations on a single property by passing an array of **IndexAttribute** to the constructor of **IndexAnnotation**.</span></span>  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a><span data-ttu-id="91de3-153">데이터베이스에서 열을 CLR 속성을 매핑할 필요가 지정</span><span class="sxs-lookup"><span data-stu-id="91de3-153">Specifying Not to Map a CLR Property to a Column in the Database</span></span>  

<span data-ttu-id="91de3-154">다음 예에서는 CLR 형식의 속성을 데이터베이스의 열에 매핑되지 않은 지정 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-154">The following example shows how to specify that a property on a CLR type is not mapped to a column in the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a><span data-ttu-id="91de3-155">CLR 속성을 데이터베이스의 특정 열에 매핑</span><span class="sxs-lookup"><span data-stu-id="91de3-155">Mapping a CLR Property to a Specific Column in the Database</span></span>  

<span data-ttu-id="91de3-156">다음 예제에서는 DepartmentName 데이터베이스 열에 이름을 CLR 속성을 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-156">The following example maps the Name CLR property to the DepartmentName database column.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="91de3-157">모델에서 정의 되지 않은 외래 키 이름 바꾸기</span><span class="sxs-lookup"><span data-stu-id="91de3-157">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="91de3-158">CLR 형식에서 외래 키를 정의 하지만 데이터베이스에 있어야 하는 이름 지정을 하지 않으려는 경우 다음을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-158">If you choose not to define a foreign key on a CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a><span data-ttu-id="91de3-159">문자열 속성을 유니코드 콘텐츠를 지원 하는지 여부를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-159">Configuring whether a String Property Supports Unicode Content</span></span>  

<span data-ttu-id="91de3-160">기본적으로 문자열이 유니코드 (SQL Server에서 nvarchar) 됩니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-160">By default strings are Unicode (nvarchar in SQL Server).</span></span> <span data-ttu-id="91de3-161">Varchar 형식 문자열로 되도록 지정 하려면 IsUnicode 메서드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-161">You can use the IsUnicode method to specify that a string should be of varchar type.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a><span data-ttu-id="91de3-162">데이터베이스 열의 데이터 형식을 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-162">Configuring the Data Type of a Database Column</span></span>  

<span data-ttu-id="91de3-163">합니다 [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) 메서드 같은 기본 형식의 다른 표현에 매핑할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-163">The [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) method enables mapping to different representations of the same basic type.</span></span> <span data-ttu-id="91de3-164">이 메서드를 사용 하 여 프로그래머가 아닌 런타임에 데이터 변환을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-164">Using this method does not enable you to perform any conversion of the data at run time.</span></span> <span data-ttu-id="91de3-165">데이터베이스 관계가 없는 것과 IsUnicode 하는 방법은 기본 설정은 열의 varchar, note 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-165">Note that IsUnicode is the preferred way of setting columns to varchar, as it is database agnostic.</span></span>  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a><span data-ttu-id="91de3-166">복합 형식에 속성을 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-166">Configuring Properties on a Complex Type</span></span>  

<span data-ttu-id="91de3-167">복합 형식에 대해 스칼라 속성을 구성 하는 방법은 두 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-167">There are two ways to configure scalar properties on a complex type.</span></span>  

<span data-ttu-id="91de3-168">ComplexTypeConfiguration에서 속성을 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-168">You can call Property on ComplexTypeConfiguration.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

<span data-ttu-id="91de3-169">또한 복합 형식의 속성에 액세스 하려면 점 표기법을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-169">You can also use the dot notation to access a property of a complex type.</span></span>  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a><span data-ttu-id="91de3-170">낙관적 동시성 토큰으로 사용할 속성 구성</span><span class="sxs-lookup"><span data-stu-id="91de3-170">Configuring a Property to Be Used as an Optimistic Concurrency Token</span></span>  

<span data-ttu-id="91de3-171">엔터티의 속성을 나타낸다고 동시성 토큰을 지정 하려면 ConcurrencyCheck 특성 또는 IsConcurrencyToken 메서드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-171">To specify that a property in an entity represents a concurrency token, you can use either the ConcurrencyCheck attribute or the IsConcurrencyToken method.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

<span data-ttu-id="91de3-172">또한 데이터베이스에서 행 버전이 되도록 속성을 구성 하려면 IsRowVersion 메서드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-172">You can also use the IsRowVersion method to configure the property to be a row version in the database.</span></span> <span data-ttu-id="91de3-173">행 버전에는 낙관적 동시성 토큰이 되도록 항목을 자동으로 구성 되도록 속성을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-173">Setting the property to be a row version automatically configures it to be an optimistic concurrency token.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a><span data-ttu-id="91de3-174">형식 매핑</span><span class="sxs-lookup"><span data-stu-id="91de3-174">Type Mapping</span></span>  

### <a name="specifying-that-a-class-is-a-complex-type"></a><span data-ttu-id="91de3-175">클래스는 복합 형식 지정</span><span class="sxs-lookup"><span data-stu-id="91de3-175">Specifying That a Class Is a Complex Type</span></span>  

<span data-ttu-id="91de3-176">규칙에 따라 지정 된 기본 키를 갖는 형식에 복합 형식으로 처리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-176">By convention, a type that has no primary key specified is treated as a complex type.</span></span> <span data-ttu-id="91de3-177">일부의 시나리오가 없는 Code First 감지 하지 복합 형식 (예를 들어 수행 ID 라는 속성이 없지만 경우 기본 키에 대 한 의미 하지는 않습니다).</span><span class="sxs-lookup"><span data-stu-id="91de3-177">There are some scenarios where Code First will not detect a complex type (for example, if you do have a property called ID, but you do not mean for it to be a primary key).</span></span> <span data-ttu-id="91de3-178">이러한 경우에 fluent API를 사용 하 여 명시적으로 형식을 복합 형식 인지를 지정 하는 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-178">In such cases, you would use the fluent API to explicitly specify that a type is a complex type.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a><span data-ttu-id="91de3-179">CLR 엔터티 형식을 데이터베이스에서 테이블을 매핑할 필요가 지정</span><span class="sxs-lookup"><span data-stu-id="91de3-179">Specifying Not to Map a CLR Entity Type to a Table in the Database</span></span>  

<span data-ttu-id="91de3-180">다음 예제에서 데이터베이스의 테이블에 매핑되는 CLR 형식을 제외 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-180">The following example shows how to exclude a CLR type from being mapped to a table in the database.</span></span>  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a><span data-ttu-id="91de3-181">데이터베이스의 특정 테이블에 엔터티 형식 매핑</span><span class="sxs-lookup"><span data-stu-id="91de3-181">Mapping an Entity Type to a Specific Table in the Database</span></span>  

<span data-ttu-id="91de3-182">부서의 모든 속성 t_ 부서를 호출 하는 테이블의 열에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-182">All properties of Department will be mapped to columns in a table called t_ Department.</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

<span data-ttu-id="91de3-183">또한 다음과 같은 스키마 이름을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-183">You can also specify the schema name like this:</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a><span data-ttu-id="91de3-184">테이블-당-TPH (계층) 상속 매핑</span><span class="sxs-lookup"><span data-stu-id="91de3-184">Mapping the Table-Per-Hierarchy (TPH) Inheritance</span></span>  

<span data-ttu-id="91de3-185">TPH 매핑 시나리오에서 상속 계층 구조의 모든 형식은 단일 테이블에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-185">In the TPH mapping scenario, all types in an inheritance hierarchy are mapped to a single table.</span></span> <span data-ttu-id="91de3-186">판별자 열은 각 행의 형식을 식별 하기 위해 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-186">A discriminator column is used to identify the type of each row.</span></span> <span data-ttu-id="91de3-187">Code First를 사용 하 여 모델을 만들 때 TPH 상속 계층 구조에 참여 하는 형식에 대 한 기본 전략입니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-187">When creating your model with Code First, TPH is the default strategy for the types that participate in the inheritance hierarchy.</span></span> <span data-ttu-id="91de3-188">기본적으로 판별자 열 이름 "Discriminator" 테이블에 추가 됩니다 하 고 계층의 각 형식의 CLR 형식 이름을 판별자 값에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-188">By default, the discriminator column is added to the table with the name “Discriminator” and the CLR type name of each type in the hierarchy is used for the discriminator values.</span></span> <span data-ttu-id="91de3-189">Fluent API를 사용 하 여 기본 동작을 수정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-189">You can modify the default behavior by using the fluent API.</span></span>  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a><span data-ttu-id="91de3-190">매핑 테이블 형식당 (TPT) 상속</span><span class="sxs-lookup"><span data-stu-id="91de3-190">Mapping the Table-Per-Type (TPT) Inheritance</span></span>  

<span data-ttu-id="91de3-191">TPT 매핑 시나리오에서 모든 형식은 개별 테이블에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-191">In the TPT mapping scenario, all types are mapped to individual tables.</span></span> <span data-ttu-id="91de3-192">기본 형식 또는 파생 형식에만 속하는 속성은 해당 형식에 매핑되는 테이블에 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-192">Properties that belong solely to a base type or derived type are stored in a table that maps to that type.</span></span> <span data-ttu-id="91de3-193">파생 형식에 매핑되는 테이블에는 또한 기본 테이블을 사용 하 여 파생된 테이블을 조인 하는 외래 키를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-193">Tables that map to derived types also store a foreign key that joins the derived table with the base table.</span></span>  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a><span data-ttu-id="91de3-194">구체적 형식당 테이블 클래스 (TPC) 상속 매핑</span><span class="sxs-lookup"><span data-stu-id="91de3-194">Mapping the Table-Per-Concrete Class (TPC) Inheritance</span></span>  

<span data-ttu-id="91de3-195">TPC 매핑 시나리오에서 계층의 모든 비추상 형식은 개별 테이블에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-195">In the TPC mapping scenario, all non-abstract types in the hierarchy are mapped to individual tables.</span></span> <span data-ttu-id="91de3-196">파생된 클래스에 매핑되는 테이블 데이터베이스에서 기본 클래스에 매핑되는 테이블에는 관계가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-196">The tables that map to the derived classes have no relationship to the table that maps to the base class in the database.</span></span> <span data-ttu-id="91de3-197">상속 된 속성을 포함 한 클래스의 모든 속성은 해당 테이블의 열에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-197">All properties of a class, including inherited properties, are mapped to columns of the corresponding table.</span></span>  

<span data-ttu-id="91de3-198">각 파생된 형식을 구성 하는 MapInheritedProperties 메서드를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-198">Call the MapInheritedProperties method to configure each derived type.</span></span> <span data-ttu-id="91de3-199">MapInheritedProperties 파생된 클래스에 대 한 테이블에 새 열에 기본 클래스에서 상속 된 모든 속성을 다시 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-199">MapInheritedProperties remaps all properties that were inherited from the base class to new columns in the table for the derived class.</span></span>  

> [!NOTE]
> <span data-ttu-id="91de3-200">TPC 상속 계층 구조에 참여 하는 테이블을 공유 하지 않기 때문에 기본 키 되도록 중복 엔터티 키 동일한 id 초기값을 사용 하 여 데이터베이스에서 생성 된 값이 있는 경우 서브 클래스에 매핑되는 테이블에 삽입 하는 경우 note 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-200">Note that because the tables participating in TPC inheritance hierarchy do not share a primary key there will be duplicate entity keys when inserting in tables that are mapped to subclasses if you have database generated values with the same identity seed.</span></span> <span data-ttu-id="91de3-201">이 문제를 해결 하려면 각 테이블에 대 한 다른 초기 시드 값을 지정 하거나 기본 키 속성에 대 한 id입니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-201">To solve this problem you can either specify a different initial seed value for each table or switch off identity on the primary key property.</span></span> <span data-ttu-id="91de3-202">Id는 정수 키 속성에 대 한 기본값은 Code First를 사용 하 여 작업 하는 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-202">Identity is the default value for integer key properties when working with Code First.</span></span>  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a><span data-ttu-id="91de3-203">엔터티 형식의 속성 (엔터티 분할) 데이터베이스의 여러 테이블에 매핑</span><span class="sxs-lookup"><span data-stu-id="91de3-203">Mapping Properties of an Entity Type to Multiple Tables in the Database (Entity Splitting)</span></span>  

<span data-ttu-id="91de3-204">엔터티 분할을 여러 테이블을 분산할 수 엔터티 형식의 속성을 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-204">Entity splitting allows the properties of an entity type to be spread across multiple tables.</span></span> <span data-ttu-id="91de3-205">다음 예제에서는 부서 엔터티를 두 테이블로 분할 됩니다: 부서 및 DepartmentDetails 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-205">In the following example, the Department entity is split into two tables: Department and DepartmentDetails.</span></span> <span data-ttu-id="91de3-206">엔터티 분할은 특정 테이블에 속성 하위 집합을 매핑할 Map 메서드를 여러 번 호출을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-206">Entity splitting uses multiple calls to the Map method to map a subset of properties to a specific table.</span></span>  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a><span data-ttu-id="91de3-207">여러 엔터티 형식 (테이블 분할) 데이터베이스의 테이블에 매핑</span><span class="sxs-lookup"><span data-stu-id="91de3-207">Mapping Multiple Entity Types to One Table in the Database (Table Splitting)</span></span>  

<span data-ttu-id="91de3-208">다음 예제에서는 한 테이블에 기본 키를 공유 하는 두 개의 엔터티 형식을 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-208">The following example maps two entity types that share a primary key to one table.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a><span data-ttu-id="91de3-209">Insert/Update/Delete 저장 프로시저 (EF6부터)에 엔터티 형식 매핑</span><span class="sxs-lookup"><span data-stu-id="91de3-209">Mapping an Entity Type to Insert/Update/Delete Stored Procedures (EF6 onwards)</span></span>  

<span data-ttu-id="91de3-210">EF6을 사용 하 여 시작 엔터티 삽입 업데이트에 대 한 저장된 프로시저를 사용 하 고 삭제를 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-210">Starting with EF6 you can map an entity to use stored procedures for insert update and delete.</span></span> <span data-ttu-id="91de3-211">자세한 내용을 참조 하세요 [첫 번째 Insert/Update/Delete 저장 프로시저 코드](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-211">For more details see, [Code First Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span></span>  

## <a name="model-used-in-samples"></a><span data-ttu-id="91de3-212">샘플에 사용 된 모델</span><span class="sxs-lookup"><span data-stu-id="91de3-212">Model Used in Samples</span></span>  

<span data-ttu-id="91de3-213">다음 Code First 모델은이 페이지에서 샘플에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="91de3-213">The following Code First model is used for the samples on this page.</span></span>  

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
