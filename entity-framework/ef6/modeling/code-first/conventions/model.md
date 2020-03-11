---
title: 모델 기반 규칙-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: c873e9a216bd9bd1934f2149ae6af602072f3608
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415846"
---
# <a name="model-based-conventions"></a>모델 기반 규칙
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

모델 기반 규칙은 규칙 기반 모델 구성의 고급 방법입니다. 대부분의 시나리오에서는 [DbModelBuilder의 사용자 지정 Code First 규칙 API](~/ef6/modeling/code-first/conventions/custom.md) 를 사용 해야 합니다. 모델 기반 규칙을 사용 하기 전에 규칙에 대 한 DbModelBuilder API를 이해 하는 것이 좋습니다.  

모델 기반 규칙을 사용 하면 표준 규칙을 통해 구성할 수 없는 속성 및 테이블에 영향을 주는 규칙을 만들 수 있습니다. 이러한 예로는 계층 구조 모델 및 독립 연결 열에 따라 테이블의 판별자 열이 있습니다.  

## <a name="creating-a-convention"></a>규칙 만들기   

모델 기반 규칙을 만드는 첫 번째 단계는 파이프라인에서 규칙을 모델에 적용 해야 하는 경우를 선택 하는 것입니다. 모델 규칙에는 개념적 (C 공백) 및 저장소 (S-Space)의 두 가지 유형이 있습니다. C 공백 규칙은 응용 프로그램에서 작성 하는 모델에 적용 되는 반면, 데이터베이스를 나타내는 모델 버전에는 공백 규칙이 적용 되 고 자동으로 생성 된 열의 이름은와 같은 항목을 제어 합니다.  

모델 규칙은 IConceptualModelConvention 또는 IStoreModelConvention에서 확장 되는 클래스입니다.  이러한 인터페이스는 규칙이 적용 되는 데이터 형식을 필터링 하는 데 사용 되는 MetadataItem 형식일 수 있는 제네릭 형식을 모두 허용 합니다.  

## <a name="adding-a-convention"></a>규칙 추가   

모델 규칙은 일반 규칙 클래스와 동일한 방식으로 추가 됩니다. **Onmodelcreating** 메서드에서 모델의 규칙 목록에 규칙을 추가 합니다.  

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

규칙을 사용 하 여 다른 규칙과 관련 하 여 규칙을 추가할 수도 있습니다. AddBefore\<\> 또는 규칙. Addbefore\<\> 메서드. Entity Framework 적용 되는 규칙에 대 한 자세한 내용은 참고 섹션을 참조 하세요.  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a>예: 판별자 모델 규칙  

EF에 의해 생성 된 열의 이름은 다른 규칙 Api로 수행할 수 없는 경우의 예입니다.  이 경우 모델 규칙을 사용 하는 것이 유일한 옵션입니다.  

모델 기반 규칙을 사용 하 여 생성 된 열을 구성 하는 방법의 예는 판별자 열의 이름을 지정 하는 방식을 사용자 지정 하는 것입니다.  다음은 "판별자" 라는 모델의 모든 열을 "EntityType"으로 바꾸는 간단한 모델 기반 규칙의 예입니다.  여기에는 개발자가 단순히 "판별자" 라는 열이 포함 됩니다. "판별자" 열은 생성 된 열 이므로이를 S 공간에서 실행 해야 합니다.  

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

## <a name="example-general-ia-renaming-convention"></a>예: 일반 IA 이름 바꾸기 규칙   

작업에 사용 되는 모델 기반 규칙의 또 다른 예는 독립 연결 (IAs)의 이름을 지정 하는 방법을 구성 하는 것입니다.  이는 IAs가 EF에 의해 생성 되 고 DbModelBuilder API가 액세스할 수 있는 모델에 없는 경우에 모델 규칙이 적용 되는 상황입니다.  

EF는 IA를 생성할 때 EntityType_KeyName 라는 열을 만듭니다. 예를 들어 CustomerId 라는 키 열이 포함 된 Customer 라는 연결의 경우 Customer_CustomerId 이라는 열을 생성 합니다. 다음 규칙은 IA에 대해 생성 된 열 이름에서 '\_' 문자를 제거 합니다.  

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

## <a name="extending-existing-conventions"></a>기존 규칙 확장   

모델에 이미 적용 Entity Framework 규칙 중 하 나와 비슷한 규칙을 작성 해야 하는 경우 처음부터 다시 작성 하지 않아도 되도록 해당 규칙을 항상 확장할 수 있습니다.  이에 대 한 예는 기존 Id 일치 규칙을 사용자 지정으로 바꾸는 것입니다.   키 규칙을 재정의 하는 추가 혜택은 이미 검색 되거나 명시적으로 구성 된 키가 없는 경우에만 재정의 된 메서드를 호출 한다는 것입니다. Entity Framework에서 사용 하는 규칙 목록은 [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx)에서 사용할 수 있습니다.  

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

그런 다음 기존 키 규칙 앞에 새 규칙을 추가 해야 합니다. CustomKeyDiscoveryConvention 규칙을 추가 하면 IdKeyDiscoveryConvention을 제거할 수 있습니다.  기존 IdKeyDiscoveryConvention을 제거 하지 않은 경우이 규칙은 먼저 실행 되기 때문에 Id 검색 규칙 보다 우선적으로 적용 되지만 "key" 속성이 없는 경우 "id" 규칙이 실행 됩니다.  각 규칙은 모델을 독립적으로 작동 하 고 모든 것을 함께 결합 하는 것이 아니라 이전 규칙에 의해 업데이트 된 것으로 확인 하기 때문에이 동작을 볼 수 있습니다. 예를 들어 이전 규칙은 열 이름을 사용자 지정 규칙 (이름이 관심이 없는 경우)에 관심이 있으면 해당 열에 적용 됩니다.  

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

## <a name="notes"></a>참고  

Entity Framework에서 현재 적용 하는 규칙 목록은 MSDN 설명서 ( [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx))에서 확인할 수 있습니다.  이 목록은 소스 코드에서 직접 가져옵니다.  Entity Framework 6의 소스 코드는 [GitHub](https://github.com/aspnet/entityframework6/) 에서 사용할 수 있으며 Entity Framework에서 사용 하는 대부분의 규칙은 사용자 지정 모델 기반 규칙을 위한 좋은 출발점입니다.  
