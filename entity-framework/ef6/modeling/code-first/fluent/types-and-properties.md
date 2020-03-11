---
title: 흐름 API-속성 및 형식 구성 및 매핑-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: 7371cc99142ccf8fc6bea237d7d58d1e67fcecec
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415756"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a><span data-ttu-id="3de10-102">흐름 API-속성 및 형식 구성 및 매핑</span><span class="sxs-lookup"><span data-stu-id="3de10-102">Fluent API - Configuring and Mapping Properties and Types</span></span>
<span data-ttu-id="3de10-103">Entity Framework Code First 작업할 때 기본 동작은 EF 클래스를 구운 규칙 집합을 사용 하는 테이블에 매핑하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-103">When working with Entity Framework Code First the default behavior is to map your POCO classes to tables using a set of conventions baked into EF.</span></span> <span data-ttu-id="3de10-104">그러나 경우에 따라 이러한 규칙을 따르지 않거나 규칙이 적용 되는 것 이외의 엔터티에 엔터티를 매핑해야 하는 경우도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-104">Sometimes, however, you cannot or do not want to follow those conventions and need to map entities to something other than what the conventions dictate.</span></span>  

<span data-ttu-id="3de10-105">두 가지 주요 방법으로는 규칙, 즉 [주석](~/ef6/modeling/code-first/data-annotations.md) 또는 EFS 흐름 API 이외의 다른 항목을 사용 하도록 EF를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-105">There are two main ways you can configure EF to use something other than conventions, namely [annotations](~/ef6/modeling/code-first/data-annotations.md) or EFs fluent API.</span></span> <span data-ttu-id="3de10-106">주석은 흐름 API 기능의 하위 집합을 포함 하므로 주석을 사용 하 여 얻을 수 없는 매핑 시나리오가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-106">The annotations only cover a subset of the fluent API functionality, so there are mapping scenarios that cannot be achieved using annotations.</span></span> <span data-ttu-id="3de10-107">이 문서는 흐름 API를 사용 하 여 속성을 구성 하는 방법을 보여 주기 위해 작성 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-107">This article is designed to demonstrate how to use the fluent API to configure properties.</span></span>  

<span data-ttu-id="3de10-108">Code first 흐름 API는 파생 된 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx)에 대 한 [onmodelcreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) 메서드를 재정의 하 여 가장 일반적으로 액세스 합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-108">The code first fluent API is most commonly accessed by overriding the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) method on your derived [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span></span> <span data-ttu-id="3de10-109">다음 샘플은 흐름 api를 사용 하 여 다양 한 작업을 수행 하는 방법을 보여 주기 위해 설계 되었으며 코드를 복사 하 여 모델에 맞게 사용자 지정할 수 있도록 합니다. 모델을 그대로 사용할 수 있는 모델을 확인 하려는 경우이 문서의 끝에 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-109">The following samples are designed to show how to do various tasks with the fluent api and allow you to copy the code out and customize it to suit your model, if you wish to see the model that they can be used with as-is then it is provided at the end of this article.</span></span>  

## <a name="model-wide-settings"></a><span data-ttu-id="3de10-110">모델 전체 설정</span><span class="sxs-lookup"><span data-stu-id="3de10-110">Model-Wide Settings</span></span>  

### <a name="default-schema-ef6-onwards"></a><span data-ttu-id="3de10-111">기본 스키마 (EF6)</span><span class="sxs-lookup"><span data-stu-id="3de10-111">Default Schema (EF6 onwards)</span></span>  

<span data-ttu-id="3de10-112">EF6부터 DbModelBuilder의 HasDefaultSchema 메서드를 사용 하 여 모든 테이블, 저장 프로시저 등에 사용할 데이터베이스 스키마를 지정할 수 있습니다. 이 기본 설정은 다른 스키마를 명시적으로 구성 하는 모든 개체에 대해 재정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-112">Starting with EF6 you can use the HasDefaultSchema method on DbModelBuilder to specify the database schema to use for all tables, stored procedures, etc. This default setting will be overridden for any objects that you explicitly configure a different schema for.</span></span>  

``` csharp
modelBuilder.HasDefaultSchema("sales");
```  

### <a name="custom-conventions-ef6-onwards"></a><span data-ttu-id="3de10-113">사용자 지정 규칙 (EF6)</span><span class="sxs-lookup"><span data-stu-id="3de10-113">Custom Conventions (EF6 onwards)</span></span>  

<span data-ttu-id="3de10-114">EF6부터 사용자 고유의 규칙을 만들어 Code First에 포함 된 규칙을 보완할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-114">Starting with EF6 you can create your own conventions to supplement the ones included in Code First.</span></span> <span data-ttu-id="3de10-115">자세한 내용은 [사용자 지정 Code First 규칙](~/ef6/modeling/code-first/conventions/custom.md)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="3de10-115">For more details, see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>  

## <a name="property-mapping"></a><span data-ttu-id="3de10-116">속성 매핑</span><span class="sxs-lookup"><span data-stu-id="3de10-116">Property Mapping</span></span>  

<span data-ttu-id="3de10-117">[속성](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) 메서드는 엔터티 또는 복합 형식에 속하는 각 속성에 대 한 특성을 구성 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-117">The [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) method is used to configure attributes for each property belonging to an entity or complex type.</span></span> <span data-ttu-id="3de10-118">속성 메서드는 지정 된 속성에 대 한 구성 개체를 가져오는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-118">The Property method is used to obtain a configuration object for a given property.</span></span> <span data-ttu-id="3de10-119">구성 개체의 옵션은 구성 되는 형식에 따라 다릅니다. IsUnicode는 문자열 속성에 대해서만 사용할 수 있습니다. 예를 들면입니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-119">The options on the configuration object are specific to the type being configured; IsUnicode is available only on string properties for example.</span></span>  

### <a name="configuring-a-primary-key"></a><span data-ttu-id="3de10-120">기본 키 구성</span><span class="sxs-lookup"><span data-stu-id="3de10-120">Configuring a Primary Key</span></span>  

<span data-ttu-id="3de10-121">기본 키에 대 한 Entity Framework 규칙은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-121">The Entity Framework convention for primary keys is:</span></span>  

1. <span data-ttu-id="3de10-122">클래스는 이름이 "ID" 또는 "Id" 인 속성을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-122">Your class defines a property whose name is “ID” or “Id”</span></span>  
2. <span data-ttu-id="3de10-123">또는 클래스 이름 뒤에 "ID" 또는 "Id"를 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-123">or a class name followed by “ID” or “Id”</span></span>  

<span data-ttu-id="3de10-124">속성을 기본 키로 명시적으로 설정 하려면 HasKey 메서드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-124">To explicitly set a property to be a primary key, you can use the HasKey method.</span></span> <span data-ttu-id="3de10-125">다음 예제에서는 HasKey 메서드를 사용 하 여 OfficeAssignment 형식에 InstructorID 기본 키를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-125">In the following example, the HasKey method is used to configure the InstructorID primary key on the OfficeAssignment type.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a><span data-ttu-id="3de10-126">복합 기본 키 구성</span><span class="sxs-lookup"><span data-stu-id="3de10-126">Configuring a Composite Primary Key</span></span>  

<span data-ttu-id="3de10-127">다음 예에서는 DepartmentID 및 Name 속성이 부서 유형의 복합 기본 키가 되도록 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-127">The following example configures the DepartmentID and Name properties to be the composite primary key of the Department type.</span></span>  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a><span data-ttu-id="3de10-128">숫자 기본 키의 Id를 해제 합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-128">Switching off Identity for Numeric Primary Keys</span></span>  

<span data-ttu-id="3de10-129">다음 예에서는 DepartmentID 속성을 System.componentmodel로 설정 하 여 데이터베이스에서 값이 생성 되지 않음을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-129">The following example sets the DepartmentID property to System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that the value will not be generated by the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a><span data-ttu-id="3de10-130">속성의 최대 길이 지정</span><span class="sxs-lookup"><span data-stu-id="3de10-130">Specifying the Maximum Length on a Property</span></span>  

<span data-ttu-id="3de10-131">다음 예제에서 Name 속성은 50 자이 하 여야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-131">In the following example, the Name property should be no longer than 50 characters.</span></span> <span data-ttu-id="3de10-132">50 자 보다 긴 값을 설정 하면 [Dbentityvalidationexception](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) 예외가 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-132">If you make the value longer than 50 characters, you will get a [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception.</span></span> <span data-ttu-id="3de10-133">이 모델에서 데이터베이스를 만드는 Code First 경우에는 이름 열의 최대 길이를 50 자로 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-133">If Code First creates a database from this model it will also set the maximum length of the Name column to 50 characters.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a><span data-ttu-id="3de10-134">필수 속성 구성</span><span class="sxs-lookup"><span data-stu-id="3de10-134">Configuring the Property to be Required</span></span>  

<span data-ttu-id="3de10-135">다음 예에서는 Name 속성이 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-135">In the following example, the Name property is required.</span></span> <span data-ttu-id="3de10-136">이름을 지정 하지 않으면 DbEntityValidationException 예외가 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-136">If you do not specify the Name, you will get a DbEntityValidationException exception.</span></span> <span data-ttu-id="3de10-137">이 모델에서 데이터베이스를 만드는 Code First 경우이 속성을 저장 하는 데 사용 되는 열은 일반적으로 null을 허용 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-137">If Code First creates a database from this model then the column used to store this property will usually be non-nullable.</span></span>  

> [!NOTE]
> <span data-ttu-id="3de10-138">속성이 필요한 경우에도 데이터베이스의 열을 null을 허용 하지 않는 경우도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-138">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="3de10-139">예를 들어 여러 형식에 대해 TPH 상속 전략 데이터를 사용 하는 경우 단일 테이블에 저장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-139">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="3de10-140">파생 된 형식에 필수 속성이 포함 된 경우에는 해당 열을 null을 허용 하지 않는 형식으로 만들 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-140">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a><span data-ttu-id="3de10-141">하나 이상의 속성에 대 한 인덱스 구성</span><span class="sxs-lookup"><span data-stu-id="3de10-141">Configuring an Index on one or more properties</span></span>  

> [!NOTE]
> <span data-ttu-id="3de10-142">**Ef 6.1** 이상-인덱스 특성이 Entity Framework 6.1에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-142">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="3de10-143">이전 버전을 사용 하는 경우이 섹션의 정보는 적용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-143">If you are using an earlier version the information in this section does not apply.</span></span>  

<span data-ttu-id="3de10-144">인덱스를 만드는 작업은 기본적으로 흐름 API에서 지원 되지 않지만 흐름 API를 통해 **Indexattribute** 에 대 한 지원을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-144">Creating indexes isn't natively supported by the Fluent API, but you can make use of the support for **IndexAttribute** via the Fluent API.</span></span> <span data-ttu-id="3de10-145">인덱스 특성은 모델에 모델 주석을 포함 하 여 처리 된 다음 파이프라인에서 나중에 데이터베이스의 인덱스로 전환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-145">Index attributes are processed by including a model annotation on the model that is then turned into an Index in the database later in the pipeline.</span></span> <span data-ttu-id="3de10-146">흐름 API를 사용 하 여 이러한 동일한 주석을 수동으로 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-146">You can manually add these same annotations using the Fluent API.</span></span>  

<span data-ttu-id="3de10-147">이 작업을 수행 하는 가장 쉬운 방법은 새 인덱스에 대 한 모든 설정을 포함 하는 **Indexattribute** 의 인스턴스를 만드는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-147">The easiest way to do this is to create an instance of **IndexAttribute** that contains all the settings for the new index.</span></span> <span data-ttu-id="3de10-148">그런 다음, **Indexannotation** 설정을 ef 모델에 저장할 수 있는 모델 주석으로 변환 하는 ef 특정 유형인 **indexannotation** 의 인스턴스를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-148">You can then create an instance of **IndexAnnotation** which is an EF specific type that will convert the **IndexAttribute** settings into a model annotation that can be stored on the EF model.</span></span> <span data-ttu-id="3de10-149">그런 다음 흐름 API의 **Hascolumnannotation** 메서드에이를 전달 하 여 주석의 이름 **인덱스** 를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-149">These can then be passed to the **HasColumnAnnotation** method on the Fluent API, specifying the name **Index** for the annotation.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

<span data-ttu-id="3de10-150">**Indexattribute**에서 사용할 수 있는 설정의 전체 목록은 [Code First 데이터 주석의](~/ef6/modeling/code-first/data-annotations.md) *인덱스* 섹션을 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="3de10-150">For a complete list of the settings available in **IndexAttribute**, see the *Index* section of [Code First Data Annotations](~/ef6/modeling/code-first/data-annotations.md).</span></span> <span data-ttu-id="3de10-151">여기에는 인덱스 이름을 사용자 지정 하 고, 고유 인덱스를 만들고, 다중 열 인덱스를 만드는 작업이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-151">This includes customizing the index name, creating unique indexes, and creating multi-column indexes.</span></span>  

<span data-ttu-id="3de10-152">Indexattribute의 배열을 **Indexattribute**의 생성자에 전달 하 여 단일 속성에 여러 인덱스 주석을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-152">You can specify multiple index annotations on a single property by passing an array of **IndexAttribute** to the constructor of **IndexAnnotation**.</span></span>  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a><span data-ttu-id="3de10-153">CLR 속성을 데이터베이스의 열에 매핑하지 않도록 지정</span><span class="sxs-lookup"><span data-stu-id="3de10-153">Specifying Not to Map a CLR Property to a Column in the Database</span></span>  

<span data-ttu-id="3de10-154">다음 예제에서는 CLR 형식의 속성이 데이터베이스의 열에 매핑되지 않도록 지정 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-154">The following example shows how to specify that a property on a CLR type is not mapped to a column in the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a><span data-ttu-id="3de10-155">데이터베이스의 특정 열에 CLR 속성 매핑</span><span class="sxs-lookup"><span data-stu-id="3de10-155">Mapping a CLR Property to a Specific Column in the Database</span></span>  

<span data-ttu-id="3de10-156">다음 예에서는 이름 CLR 속성을 DepartmentName 데이터베이스 열에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-156">The following example maps the Name CLR property to the DepartmentName database column.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="3de10-157">모델에 정의 되지 않은 외래 키 이름 바꾸기</span><span class="sxs-lookup"><span data-stu-id="3de10-157">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="3de10-158">CLR 유형에 외래 키를 정의 하지 않고 데이터베이스에 포함 되어야 하는 이름을 지정 하려면 다음을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-158">If you choose not to define a foreign key on a CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a><span data-ttu-id="3de10-159">문자열 속성이 유니코드 콘텐츠를 지원 하는지 여부 구성</span><span class="sxs-lookup"><span data-stu-id="3de10-159">Configuring whether a String Property Supports Unicode Content</span></span>  

<span data-ttu-id="3de10-160">기본적으로 문자열은 유니코드 (SQL Server의 nvarchar)입니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-160">By default strings are Unicode (nvarchar in SQL Server).</span></span> <span data-ttu-id="3de10-161">IsUnicode 메서드를 사용 하 여 문자열을 varchar 형식으로 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-161">You can use the IsUnicode method to specify that a string should be of varchar type.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a><span data-ttu-id="3de10-162">데이터베이스 열의 데이터 형식 구성</span><span class="sxs-lookup"><span data-stu-id="3de10-162">Configuring the Data Type of a Database Column</span></span>  

<span data-ttu-id="3de10-163">[HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) 메서드를 사용 하면 동일한 기본 형식의 다른 표현에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-163">The [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) method enables mapping to different representations of the same basic type.</span></span> <span data-ttu-id="3de10-164">이 메서드를 사용 하면 런타임에 데이터 변환을 수행할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-164">Using this method does not enable you to perform any conversion of the data at run time.</span></span> <span data-ttu-id="3de10-165">IsUnicode는 데이터베이스에 관계 없이 열을 varchar로 설정 하는 기본 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-165">Note that IsUnicode is the preferred way of setting columns to varchar, as it is database agnostic.</span></span>  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a><span data-ttu-id="3de10-166">복합 유형에 대 한 속성 구성</span><span class="sxs-lookup"><span data-stu-id="3de10-166">Configuring Properties on a Complex Type</span></span>  

<span data-ttu-id="3de10-167">복합 형식에서 스칼라 속성을 구성 하는 방법에는 두 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-167">There are two ways to configure scalar properties on a complex type.</span></span>  

<span data-ttu-id="3de10-168">ComplexTypeConfiguration에서 속성을 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-168">You can call Property on ComplexTypeConfiguration.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

<span data-ttu-id="3de10-169">점 표기법을 사용 하 여 복합 형식의 속성에 액세스할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-169">You can also use the dot notation to access a property of a complex type.</span></span>  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a><span data-ttu-id="3de10-170">낙관적 동시성 토큰으로 사용할 속성 구성</span><span class="sxs-lookup"><span data-stu-id="3de10-170">Configuring a Property to Be Used as an Optimistic Concurrency Token</span></span>  

<span data-ttu-id="3de10-171">엔터티의 속성이 동시성 토큰을 나타내도록 지정 하려면 ConcurrencyCheck 특성 또는 IsConcurrencyToken 메서드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-171">To specify that a property in an entity represents a concurrency token, you can use either the ConcurrencyCheck attribute or the IsConcurrencyToken method.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

<span data-ttu-id="3de10-172">IsRowVersion 메서드를 사용 하 여 데이터베이스의 행 버전으로 속성을 구성할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-172">You can also use the IsRowVersion method to configure the property to be a row version in the database.</span></span> <span data-ttu-id="3de10-173">속성을 행 버전으로 설정 하면 자동으로 낙관적 동시성 토큰이 되도록 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-173">Setting the property to be a row version automatically configures it to be an optimistic concurrency token.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a><span data-ttu-id="3de10-174">형식 매핑</span><span class="sxs-lookup"><span data-stu-id="3de10-174">Type Mapping</span></span>  

### <a name="specifying-that-a-class-is-a-complex-type"></a><span data-ttu-id="3de10-175">클래스가 복합 형식으로 지정</span><span class="sxs-lookup"><span data-stu-id="3de10-175">Specifying That a Class Is a Complex Type</span></span>  

<span data-ttu-id="3de10-176">규칙에 따라 기본 키가 지정 되지 않은 형식은 복합 형식으로 처리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-176">By convention, a type that has no primary key specified is treated as a complex type.</span></span> <span data-ttu-id="3de10-177">Code First에서 복합 형식을 검색 하지 않는 몇 가지 시나리오가 있습니다. 예를 들어 ID 라는 속성이 있지만 기본 키가 아닌 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-177">There are some scenarios where Code First will not detect a complex type (for example, if you do have a property called ID, but you do not mean for it to be a primary key).</span></span> <span data-ttu-id="3de10-178">이러한 경우 흐름 API를 사용 하 여 형식이 복합 형식이 되도록 명시적으로 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-178">In such cases, you would use the fluent API to explicitly specify that a type is a complex type.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a><span data-ttu-id="3de10-179">CLR 엔터티 형식을 데이터베이스의 테이블에 매핑하지 않도록 지정</span><span class="sxs-lookup"><span data-stu-id="3de10-179">Specifying Not to Map a CLR Entity Type to a Table in the Database</span></span>  

<span data-ttu-id="3de10-180">다음 예에서는 데이터베이스의 테이블에 매핑할 CLR 형식을 제외 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-180">The following example shows how to exclude a CLR type from being mapped to a table in the database.</span></span>  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a><span data-ttu-id="3de10-181">엔터티 형식을 데이터베이스의 특정 테이블에 매핑</span><span class="sxs-lookup"><span data-stu-id="3de10-181">Mapping an Entity Type to a Specific Table in the Database</span></span>  

<span data-ttu-id="3de10-182">학과의 모든 속성은 t_ 부서 라는 테이블의 열에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-182">All properties of Department will be mapped to columns in a table called t_ Department.</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

<span data-ttu-id="3de10-183">다음과 같이 스키마 이름을 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-183">You can also specify the schema name like this:</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a><span data-ttu-id="3de10-184">TPH (계층당 하나의 테이블) 상속 매핑</span><span class="sxs-lookup"><span data-stu-id="3de10-184">Mapping the Table-Per-Hierarchy (TPH) Inheritance</span></span>  

<span data-ttu-id="3de10-185">TPH 매핑 시나리오에서 상속 계층 구조의 모든 형식은 단일 테이블에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-185">In the TPH mapping scenario, all types in an inheritance hierarchy are mapped to a single table.</span></span> <span data-ttu-id="3de10-186">판별자 열은 각 행의 유형을 식별 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-186">A discriminator column is used to identify the type of each row.</span></span> <span data-ttu-id="3de10-187">Code First를 사용 하 여 모델을 만들 때 TPH는 상속 계층 구조에 참여 하는 형식에 대 한 기본 전략입니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-187">When creating your model with Code First, TPH is the default strategy for the types that participate in the inheritance hierarchy.</span></span> <span data-ttu-id="3de10-188">기본적으로 판별자 열은 이름이 "판별자" 인 테이블에 추가 되 고 계층 구조에 있는 각 형식의 CLR 형식 이름이 판별자 값에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-188">By default, the discriminator column is added to the table with the name “Discriminator” and the CLR type name of each type in the hierarchy is used for the discriminator values.</span></span> <span data-ttu-id="3de10-189">흐름 API를 사용 하 여 기본 동작을 수정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-189">You can modify the default behavior by using the fluent API.</span></span>  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a><span data-ttu-id="3de10-190">형식당 하나의 테이블 (TPT) 상속 매핑</span><span class="sxs-lookup"><span data-stu-id="3de10-190">Mapping the Table-Per-Type (TPT) Inheritance</span></span>  

<span data-ttu-id="3de10-191">TPT mapping 시나리오에서 모든 형식은 개별 테이블에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-191">In the TPT mapping scenario, all types are mapped to individual tables.</span></span> <span data-ttu-id="3de10-192">기본 형식 또는 파생 형식에만 속하는 속성은 해당 형식에 매핑되는 테이블에 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-192">Properties that belong solely to a base type or derived type are stored in a table that maps to that type.</span></span> <span data-ttu-id="3de10-193">파생 형식에 매핑되는 테이블은 파생 테이블을 기본 테이블과 조인 하는 외래 키도 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-193">Tables that map to derived types also store a foreign key that joins the derived table with the base table.</span></span>  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a><span data-ttu-id="3de10-194">TPC (비추상 클래스) 상속 매핑</span><span class="sxs-lookup"><span data-stu-id="3de10-194">Mapping the Table-Per-Concrete Class (TPC) Inheritance</span></span>  

<span data-ttu-id="3de10-195">TPC 매핑 시나리오에서는 계층 구조의 모든 비추상 형식이 개별 테이블에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-195">In the TPC mapping scenario, all non-abstract types in the hierarchy are mapped to individual tables.</span></span> <span data-ttu-id="3de10-196">파생 클래스에 매핑되는 테이블은 데이터베이스의 기본 클래스에 매핑되는 테이블과 관계가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-196">The tables that map to the derived classes have no relationship to the table that maps to the base class in the database.</span></span> <span data-ttu-id="3de10-197">상속 된 속성을 포함 하 여 클래스의 모든 속성은 해당 테이블의 열에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-197">All properties of a class, including inherited properties, are mapped to columns of the corresponding table.</span></span>  

<span data-ttu-id="3de10-198">MapInheritedProperties 메서드를 호출 하 여 각 파생 형식을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-198">Call the MapInheritedProperties method to configure each derived type.</span></span> <span data-ttu-id="3de10-199">MapInheritedProperties는 기본 클래스에서 파생 된 클래스에 대 한 테이블의 새 열로 상속 된 모든 속성을 다시 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-199">MapInheritedProperties remaps all properties that were inherited from the base class to new columns in the table for the derived class.</span></span>  

> [!NOTE]
> <span data-ttu-id="3de10-200">TPC 상속 계층 구조에 참여 하는 테이블은 기본 키를 공유 하지 않으므로 동일한 id 초기값을 가진 데이터베이스 생성 값이 있는 경우 하위 클래스에 매핑되는 테이블에 삽입할 때 중복 된 엔터티 키가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-200">Note that because the tables participating in TPC inheritance hierarchy do not share a primary key there will be duplicate entity keys when inserting in tables that are mapped to subclasses if you have database generated values with the same identity seed.</span></span> <span data-ttu-id="3de10-201">이 문제를 해결 하려면 각 테이블에 대해 다른 초기 초기값을 지정 하거나 기본 키 속성에서 id를 해제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-201">To solve this problem you can either specify a different initial seed value for each table or switch off identity on the primary key property.</span></span> <span data-ttu-id="3de10-202">Id는 Code First 작업할 때 정수 키 속성의 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-202">Identity is the default value for integer key properties when working with Code First.</span></span>  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a><span data-ttu-id="3de10-203">엔터티 형식의 속성을 데이터베이스의 여러 테이블에 매핑 (엔터티 분할)</span><span class="sxs-lookup"><span data-stu-id="3de10-203">Mapping Properties of an Entity Type to Multiple Tables in the Database (Entity Splitting)</span></span>  

<span data-ttu-id="3de10-204">엔터티 분할을 사용 하면 엔터티 형식의 속성이 여러 테이블에 분산 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-204">Entity splitting allows the properties of an entity type to be spread across multiple tables.</span></span> <span data-ttu-id="3de10-205">다음 예에서는 부서 엔터티가 학과 및 DepartmentDetails의 두 테이블로 분할 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-205">In the following example, the Department entity is split into two tables: Department and DepartmentDetails.</span></span> <span data-ttu-id="3de10-206">엔터티 분할에서는 Map 메서드를 여러 번 호출 하 여 속성의 하위 집합을 특정 테이블에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-206">Entity splitting uses multiple calls to the Map method to map a subset of properties to a specific table.</span></span>  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a><span data-ttu-id="3de10-207">여러 엔터티 형식을 데이터베이스의 한 테이블에 매핑 (테이블 분할)</span><span class="sxs-lookup"><span data-stu-id="3de10-207">Mapping Multiple Entity Types to One Table in the Database (Table Splitting)</span></span>  

<span data-ttu-id="3de10-208">다음 예에서는 기본 키를 공유 하는 두 엔터티 형식을 하나의 테이블에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-208">The following example maps two entity types that share a primary key to one table.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a><span data-ttu-id="3de10-209">Insert/Update/Delete 저장 프로시저에 엔터티 형식 매핑 (EF6 이상)</span><span class="sxs-lookup"><span data-stu-id="3de10-209">Mapping an Entity Type to Insert/Update/Delete Stored Procedures (EF6 onwards)</span></span>  

<span data-ttu-id="3de10-210">EF6부터 삽입 업데이트 및 삭제에 대 한 저장 프로시저를 사용 하도록 엔터티를 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-210">Starting with EF6 you can map an entity to use stored procedures for insert update and delete.</span></span> <span data-ttu-id="3de10-211">자세한 내용은 [Code First Insert/Update/Delete 저장 프로시저](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="3de10-211">For more details see, [Code First Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span></span>  

## <a name="model-used-in-samples"></a><span data-ttu-id="3de10-212">예제에 사용 되는 모델</span><span class="sxs-lookup"><span data-stu-id="3de10-212">Model Used in Samples</span></span>  

<span data-ttu-id="3de10-213">다음 Code First 모델은이 페이지의 샘플에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3de10-213">The following Code First model is used for the samples on this page.</span></span>  

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
