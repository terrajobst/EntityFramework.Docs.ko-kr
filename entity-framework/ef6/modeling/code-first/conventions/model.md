---
title: 모델 기반 규칙-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
caps.latest.revision: 3
ms.openlocfilehash: 135a51d93a06c0d64732438f067df4ce2675fbe2
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122768"
---
# <a name="model-based-conventions"></a>모델 기반 규칙
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

모델 기반 규칙은 규칙 기반 모델 구성의 고급 방법을 합니다. 대부분의 시나리오에는 [DbModelBuilder에 사용자 지정 Code First 규칙 API](~/ef6/modeling/code-first/conventions/custom.md) 사용 해야 합니다. 모델 기반 규칙을 사용 하기 전에 DbModelBuilder API 규칙에 대 한 이해가 것이 좋습니다.  

모델 기반 규칙 속성과 표준 규칙을 통해 구성 가능 하지 않은 테이블에 영향을 주는 규칙 생성을 허용 합니다. 이러한 예로 계층 모델 당 테이블의 판별자 열 및 독립 연결 열이 있습니다.  

## <a name="creating-a-convention"></a>규칙 만들기   

모델 기반 규칙을 만드는 첫 번째 단계는 파이프라인에서 규칙을 모델에 적용 해야 하는 경우 선택 됩니다. 모델 규칙, 개념 (C-영역) 및 저장소 (S-영역)는 다음과 같은 두 종류가 있습니다. C-공간 규칙에서 S 공간의 규칙 데이터베이스를 나타내는 모델의 버전에 적용 되는 반면 응용 프로그램을 빌드 및 컨트롤 등 방법을 자동으로 생성 된 열 이름을 지정 하는 모델에 적용 됩니다.  

모델 규칙은 IConceptualModelConvention 또는 IStoreModelConvention에서 확장 하는 클래스입니다.  이러한 인터페이스 둘 다 수락할 수 있는 제네릭 형식을 MetadataItem 필터 규칙에 적용 되는 데이터 형식에 사용 되는 입력 합니다.  

## <a name="adding-a-convention"></a>규칙을 추가합니다.   

규칙은 규칙 일반 클래스와 동일한 방식으로 추가 됩니다. 에 **OnModelCreating** 메서드를 모델에 대 한 규칙의 목록에는 규칙을 추가 합니다.  

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

다른 규칙을 Conventions.AddBefore를 사용 하 여 관련 된 규칙을 추가할 수 있습니다\< \> 또는 Conventions.AddAfter\< \> 메서드. Entity Framework에 적용 되는 규칙에 대 한 자세한 내용은 참고 섹션을 참조 하세요.  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a>예: 판별자 모델 규칙  

EF에서 생성 된 열 이름 바꾸기는 다른 규칙 Api 사용 하 여 수행할 수 없는 항목의 예시입니다.  이 상황 여기서 유일한 옵션은 모델 규칙을 사용 합니다.  

생성 된 열을 구성 하는 모델 기반 규칙을 사용 하는 방법의 예로 판별자 열의 이름은 방식을 사용자 지정 됩니다.  "EntityType"을 "Discriminator" 이라는 모델의 모든 열 이름을 변경 하는 간단한 모델 기반 규칙의 예는 다음과 같습니다.  이는 개발자에 간단히 "Discriminator" 라는 열이 포함 됩니다. "Discriminator" 열에 생성된 된 열이 있으므로 S 공간에서 실행 해야 합니다.  

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

## <a name="example-general-ia-renaming-convention"></a>예: 일반 IA 규칙 이름 바꾸기   

실행 중인 규칙 기반 모델의 더 복잡 한 다른 예제 독립 연결 (IAs) 라고 하는 방법은 구성 하는 것입니다.  이 모델 규칙 IAs EF에서 생성 되기 때문에 적용할 수 있는 않았고 DbModelBuilder API에 액세스할 수 있는 모델에 없는 상황입니다.  

EF는 IA를 생성할 때 EntityType_KeyName 라는 열을 만듭니다. 예를 들어 고객 키 열이 있는 명명 된 연결에 대 한 이름이 CustomerId Customer_CustomerId 라는 열 생성 되는 것입니다. 다음 규칙 줄무늬를 '\_' 문자는 IA.에 대해 생성 된 열 이름에서  

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
        for (int i = 0; i \< properties.Count; ++i)
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

Entity Framework는 이미 모델에 적용 되는 규칙 중 하나에 유사한 규칙을 작성 하는 경우에 항상 처음부터 다시 작성 하지 않으려면 해당 규칙을 확장할 수 있습니다.  이 예제를 사용자 지정 규칙을 일치 하는 기존 Id를 바꾸는 것입니다.   주요 규칙을 재정의 하는 추가 혜택은 키가 없는 이미 있거나 명시적으로 구성 하는 경우에 재정의 된 메서드는 호출 됩니다. 규칙의 목록을 Entity Framework에서 사용 하는 사용할 수 있는 여기: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx)합니다.  

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

그런 다음 기존 키 규칙 전에 새 규칙을 추가 해야 합니다. CustomKeyDiscoveryConvention를 추가한 후 IdKeyDiscoveryConvention를 제거할 수 있습니다.  이 규칙은 여전히 우선 Id 검색 규칙을 먼저 하지만 "키" 속성이 있는 경우 실행 되므로 기존 IdKeyDiscoveryConvention를 제거 하지 않은 경우 "id" 규칙 실행 됩니다.  각 규칙은 이전 규칙의 일치 하는 열 이름을 업데이트 경우 예를 들어 되도록에 독립적으로 운영 보다는 함께 조합 하는 모든 이전 규칙에 따라 업데이트 모델을 확인 하므로이 동작은 표시 (이전에 이름이 없을 때 관심)에 사용자 지정 규칙을 관련 열에 적용 됩니다.  

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

## <a name="notes"></a>노트  

현재 Entity Framework에서 적용 되는 규칙 목록은 MSDN 설명서를 여기에서 사용할 수 있습니다: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx)합니다.  이 목록은 소스 코드에서 직접 가져옵니다.  Entity Framework 6에 대 한 소스 코드는에서 사용할 수 있습니다 [GitHub](https://github.com/aspnet/entityframework6/) 좋은 다양 한 Entity Framework에서 사용 하는 규칙 및 규칙 기반 사용자 지정 모델에 대 한 시작점입니다.  
