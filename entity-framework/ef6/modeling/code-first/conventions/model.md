---
title: 모델 기반 규칙-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: c873e9a216bd9bd1934f2149ae6af602072f3608
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656164"
---
# <a name="model-based-conventions"></a><span data-ttu-id="0899b-102">모델 기반 규칙</span><span class="sxs-lookup"><span data-stu-id="0899b-102">Model-Based Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="0899b-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="0899b-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="0899b-105">모델 기반 규칙은 규칙 기반 모델 구성의 고급 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-105">Model based conventions are an advanced method of convention based model configuration.</span></span> <span data-ttu-id="0899b-106">대부분의 시나리오에서는 [DbModelBuilder의 사용자 지정 Code First 규칙 API](~/ef6/modeling/code-first/conventions/custom.md) 를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-106">For most scenarios the [Custom Code First Convention API on DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) should be used.</span></span> <span data-ttu-id="0899b-107">모델 기반 규칙을 사용 하기 전에 규칙에 대 한 DbModelBuilder API를 이해 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-107">An understanding of the DbModelBuilder API for conventions is recommended before using model based conventions.</span></span>  

<span data-ttu-id="0899b-108">모델 기반 규칙을 사용 하면 표준 규칙을 통해 구성할 수 없는 속성 및 테이블에 영향을 주는 규칙을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-108">Model based conventions allow the creation of conventions that affect properties and tables which are not configurable through standard conventions.</span></span> <span data-ttu-id="0899b-109">이러한 예로는 계층 구조 모델 및 독립 연결 열에 따라 테이블의 판별자 열이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-109">Examples of these are discriminator columns in table per hierarchy models and Independent Association columns.</span></span>  

## <a name="creating-a-convention"></a><span data-ttu-id="0899b-110">규칙 만들기</span><span class="sxs-lookup"><span data-stu-id="0899b-110">Creating a Convention</span></span>   

<span data-ttu-id="0899b-111">모델 기반 규칙을 만드는 첫 번째 단계는 파이프라인에서 규칙을 모델에 적용 해야 하는 경우를 선택 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-111">The first step in creating a model based convention is choosing when in the pipeline the convention needs to be applied to the model.</span></span> <span data-ttu-id="0899b-112">모델 규칙에는 개념적 (C 공백) 및 저장소 (S-Space)의 두 가지 유형이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-112">There are two types of model conventions, Conceptual (C-Space) and Store (S-Space).</span></span> <span data-ttu-id="0899b-113">C 공백 규칙은 응용 프로그램에서 작성 하는 모델에 적용 되는 반면, 데이터베이스를 나타내는 모델 버전에는 공백 규칙이 적용 되 고 자동으로 생성 된 열의 이름은와 같은 항목을 제어 합니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-113">A C-Space convention is applied to the model that the application builds, whereas an S-Space convention is applied to the version of the model that represents the database and controls things such as how automatically-generated columns are named.</span></span>  

<span data-ttu-id="0899b-114">모델 규칙은 IConceptualModelConvention 또는 IStoreModelConvention에서 확장 되는 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-114">A model convention is a class that extends from either IConceptualModelConvention or IStoreModelConvention.</span></span>  <span data-ttu-id="0899b-115">이러한 인터페이스는 규칙이 적용 되는 데이터 형식을 필터링 하는 데 사용 되는 MetadataItem 형식일 수 있는 제네릭 형식을 모두 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-115">These interfaces both accept a generic type that can be of type MetadataItem which is used to filter the data type that the convention applies to.</span></span>  

## <a name="adding-a-convention"></a><span data-ttu-id="0899b-116">규칙 추가</span><span class="sxs-lookup"><span data-stu-id="0899b-116">Adding a Convention</span></span>   

<span data-ttu-id="0899b-117">모델 규칙은 일반 규칙 클래스와 동일한 방식으로 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-117">Model conventions are added in the same way as regular conventions classes.</span></span> <span data-ttu-id="0899b-118">**Onmodelcreating** 메서드에서 모델의 규칙 목록에 규칙을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-118">In the **OnModelCreating** method, add the convention to the list of conventions for a model.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

public class BlogContext : DbContext  
{  
    public DbSet<Post> Posts { get; set; }  
    public DbSet<Comment> Comments { get; set; }  

    protected override void OnModelCreating(DbModelBuilder modelBuilder)  
    {  
        modelBuilder.Conventions.Add<MyModelBasedConvention>();  
    }  
}
```  

<span data-ttu-id="0899b-119">규칙을 사용 하 여 다른 규칙과 관련 하 여 규칙을 추가할 수도 있습니다. AddBefore\<\> 또는 규칙. Addbefore\<\> 메서드.</span><span class="sxs-lookup"><span data-stu-id="0899b-119">A convention can also be added in relation to another convention using the Conventions.AddBefore\<\> or Conventions.AddAfter\<\> methods.</span></span> <span data-ttu-id="0899b-120">Entity Framework 적용 되는 규칙에 대 한 자세한 내용은 참고 섹션을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="0899b-120">For more information about the conventions that Entity Framework applies see the notes section.</span></span>  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a><span data-ttu-id="0899b-121">예: 판별자 모델 규칙</span><span class="sxs-lookup"><span data-stu-id="0899b-121">Example: Discriminator Model Convention</span></span>  

<span data-ttu-id="0899b-122">EF에 의해 생성 된 열의 이름은 다른 규칙 Api로 수행할 수 없는 경우의 예입니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-122">Renaming columns generated by EF is an example of something that you can’t do with the other conventions APIs.</span></span>  <span data-ttu-id="0899b-123">이 경우 모델 규칙을 사용 하는 것이 유일한 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-123">This is a situation where using model conventions is your only option.</span></span>  

<span data-ttu-id="0899b-124">모델 기반 규칙을 사용 하 여 생성 된 열을 구성 하는 방법의 예는 판별자 열의 이름을 지정 하는 방식을 사용자 지정 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-124">An example of how to use a model based convention to configure generated columns is customizing the way discriminator columns are named.</span></span>  <span data-ttu-id="0899b-125">다음은 "판별자" 라는 모델의 모든 열을 "EntityType"으로 바꾸는 간단한 모델 기반 규칙의 예입니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-125">Below is an example of a simple model based convention that renames every column in the model named “Discriminator” to “EntityType”.</span></span>  <span data-ttu-id="0899b-126">여기에는 개발자가 단순히 "판별자" 라는 열이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-126">This includes columns that the developer simply named “Discriminator”.</span></span> <span data-ttu-id="0899b-127">"판별자" 열은 생성 된 열 이므로이를 S 공간에서 실행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-127">Since the “Discriminator” column is a generated column this needs to run in S-Space.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

class DiscriminatorRenamingConvention : IStoreModelConvention<EdmProperty>  
{  
    public void Apply(EdmProperty property, DbModel model)  
    {            
        if (property.Name == "Discriminator")  
        {  
            property.Name = "EntityType";  
        }  
    }  
}
```  

## <a name="example-general-ia-renaming-convention"></a><span data-ttu-id="0899b-128">예: 일반 IA 이름 바꾸기 규칙</span><span class="sxs-lookup"><span data-stu-id="0899b-128">Example: General IA Renaming Convention</span></span>   

<span data-ttu-id="0899b-129">작업에 사용 되는 모델 기반 규칙의 또 다른 예는 독립 연결 (IAs)의 이름을 지정 하는 방법을 구성 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-129">Another more complicated example of model based conventions in action is to configure the way that Independent Associations (IAs) are named.</span></span>  <span data-ttu-id="0899b-130">이는 IAs가 EF에 의해 생성 되 고 DbModelBuilder API가 액세스할 수 있는 모델에 없는 경우에 모델 규칙이 적용 되는 상황입니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-130">This is a situation where Model conventions are applicable because IAs are generated by EF and aren’t present in the model that the DbModelBuilder API can access.</span></span>  

<span data-ttu-id="0899b-131">EF는 IA를 생성할 때 EntityType_KeyName 라는 열을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-131">When EF generates an IA, it creates a column named EntityType_KeyName.</span></span> <span data-ttu-id="0899b-132">예를 들어 CustomerId 라는 키 열이 있는 Customer 라는 연결의 경우 이름이 Customer_CustomerId 인 열을 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-132">For example, for an association named Customer with a key column named CustomerId it would generate a column named Customer_CustomerId.</span></span> <span data-ttu-id="0899b-133">다음 규칙은 IA에 대해 생성 된 열 이름에서 '\_' 문자를 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-133">The following convention strips the ‘\_’ character out of the column name that is generated for the IA.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

// Provides a convention for fixing the independent association (IA) foreign key column names.  
public class ForeignKeyNamingConvention : IStoreModelConvention<AssociationType>
{

    public void Apply(AssociationType association, DbModel model)
    {
        // Identify ForeignKey properties (including IAs)  
        if (association.IsForeignKey)
        {
            // rename FK columns  
            var constraint = association.Constraint;
            if (DoPropertiesHaveDefaultNames(constraint.FromProperties, constraint.ToRole.Name, constraint.ToProperties))
            {
                NormalizeForeignKeyProperties(constraint.FromProperties);
            }
            if (DoPropertiesHaveDefaultNames(constraint.ToProperties, constraint.FromRole.Name, constraint.FromProperties))
            {
                NormalizeForeignKeyProperties(constraint.ToProperties);
            }
        }
    }

    private bool DoPropertiesHaveDefaultNames(ReadOnlyMetadataCollection<EdmProperty> properties, string roleName, ReadOnlyMetadataCollection<EdmProperty> otherEndProperties)
    {
        if (properties.Count != otherEndProperties.Count)
        {
            return false;
        }

        for (int i = 0; i < properties.Count; ++i)
        {
            if (!properties[i].Name.EndsWith("_" + otherEndProperties[i].Name))
            {
                return false;
            }
        }
        return true;
    }

    private void NormalizeForeignKeyProperties(ReadOnlyMetadataCollection<EdmProperty> properties)
    {
        for (int i = 0; i < properties.Count; ++i)
        {
            int underscoreIndex = properties[i].Name.IndexOf('_');
            if (underscoreIndex > 0)
            {
                properties[i].Name = properties[i].Name.Remove(underscoreIndex, 1);
            }                 
        }
    }
}
```  

## <a name="extending-existing-conventions"></a><span data-ttu-id="0899b-134">기존 규칙 확장</span><span class="sxs-lookup"><span data-stu-id="0899b-134">Extending Existing Conventions</span></span>   

<span data-ttu-id="0899b-135">모델에 이미 적용 Entity Framework 규칙 중 하 나와 비슷한 규칙을 작성 해야 하는 경우 처음부터 다시 작성 하지 않아도 되도록 해당 규칙을 항상 확장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-135">If you need to write a convention that is similar to one of the conventions that Entity Framework already applies to your model you can always extend that convention to avoid having to rewrite it from scratch.</span></span>  <span data-ttu-id="0899b-136">이에 대 한 예는 기존 Id 일치 규칙을 사용자 지정으로 바꾸는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-136">An example of this is to replace the existing Id matching convention with a custom one.</span></span>   <span data-ttu-id="0899b-137">키 규칙을 재정의 하는 추가 혜택은 이미 검색 되거나 명시적으로 구성 된 키가 없는 경우에만 재정의 된 메서드를 호출 한다는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-137">An added benefit to overriding the key convention is that the overridden method will get called only if there is no key already detected or explicitly configured.</span></span> <span data-ttu-id="0899b-138">Entity Framework에서 사용 하는 규칙 목록은 [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx)에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-138">A list of conventions that used by Entity Framework is available here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;
using System.Linq;  

// Convention to detect primary key properties.
// Recognized naming patterns in order of precedence are:
// 1. 'Key'
// 2. [type name]Key
// Primary key detection is case insensitive.
public class CustomKeyDiscoveryConvention : KeyDiscoveryConvention
{
    private const string Id = "Key";

    protected override IEnumerable<EdmProperty> MatchKeyProperty(
        EntityType entityType, IEnumerable<EdmProperty> primitiveProperties)
    {
        Debug.Assert(entityType != null);
        Debug.Assert(primitiveProperties != null);

        var matches = primitiveProperties
            .Where(p => Id.Equals(p.Name, StringComparison.OrdinalIgnoreCase));

        if (!matches.Any())
       {
            matches = primitiveProperties
                .Where(p => (entityType.Name + Id).Equals(p.Name, StringComparison.OrdinalIgnoreCase));
        }

        // If the number of matches is more than one, then multiple properties matched differing only by
        // case--for example, "Key" and "key".  
        if (matches.Count() > 1)
        {
            throw new InvalidOperationException("Multiple properties match the key convention");
        }

        return matches;
    }
}
```  

<span data-ttu-id="0899b-139">그런 다음 기존 키 규칙 앞에 새 규칙을 추가 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-139">We then need to add our new convention before the existing key convention.</span></span> <span data-ttu-id="0899b-140">CustomKeyDiscoveryConvention 규칙을 추가 하면 IdKeyDiscoveryConvention을 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-140">After we add the CustomKeyDiscoveryConvention, we can remove the IdKeyDiscoveryConvention.</span></span>  <span data-ttu-id="0899b-141">기존 IdKeyDiscoveryConvention을 제거 하지 않은 경우이 규칙은 먼저 실행 되기 때문에 Id 검색 규칙 보다 우선적으로 적용 되지만 "key" 속성이 없는 경우 "id" 규칙이 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-141">If we didn’t remove the existing IdKeyDiscoveryConvention this convention would still take precedence over the Id discovery convention since it is run first, but in the case where no “key” property is found, the “id” convention will run.</span></span>  <span data-ttu-id="0899b-142">각 규칙은 모델을 독립적으로 작동 하 고 모든 것을 함께 결합 하는 것이 아니라 이전 규칙에 의해 업데이트 된 것으로 확인 하기 때문에이 동작을 볼 수 있습니다. 예를 들어 이전 규칙은 열 이름을 사용자 지정 규칙 (이름이 관심이 없는 경우)에 관심이 있으면 해당 열에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-142">We see this behavior because each convention sees the model as updated by the previous convention (rather than operating on it independently and all being combined together) so that if for example, a previous convention updated a column name to match something of interest to your custom convention (when before that the name was not of interest) then it will apply to that column.</span></span>  

``` csharp
public class BlogContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Comment> Comments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new CustomKeyDiscoveryConvention());
        modelBuilder.Conventions.Remove<IdKeyDiscoveryConvention>();
    }
}
```  

## <a name="notes"></a><span data-ttu-id="0899b-143">노트</span><span class="sxs-lookup"><span data-stu-id="0899b-143">Notes</span></span>  

<span data-ttu-id="0899b-144">Entity Framework에서 현재 적용 하는 규칙 목록은 MSDN 설명서 ( [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx))에서 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-144">A list of conventions that are currently applied by Entity Framework is available in the MSDN documentation here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  <span data-ttu-id="0899b-145">이 목록은 소스 코드에서 직접 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-145">This list is pulled directly from our source code.</span></span>  <span data-ttu-id="0899b-146">Entity Framework 6의 소스 코드는 [GitHub](https://github.com/aspnet/entityframework6/) 에서 사용할 수 있으며 Entity Framework에서 사용 하는 대부분의 규칙은 사용자 지정 모델 기반 규칙을 위한 좋은 출발점입니다.</span><span class="sxs-lookup"><span data-stu-id="0899b-146">The source code for Entity Framework 6 is available on [GitHub](https://github.com/aspnet/entityframework6/) and many of the conventions used by Entity Framework are good starting points for custom model based conventions.</span></span>  
