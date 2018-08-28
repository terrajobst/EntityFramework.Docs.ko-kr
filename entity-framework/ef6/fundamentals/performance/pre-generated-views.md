---
title: 미리 생성 된 매핑 보기-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: 397569ef374cb44d4938f9e201b588a26c408f6e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996474"
---
# <a name="pre-generated-mapping-views"></a>미리 생성 된 매핑 보기
Entity Framework 쿼리 실행 수 또는 데이터 원본에 변경 내용을 저장 하기 전에 데이터베이스에 액세스 하는 매핑 뷰 집합을 생성 해야 합니다 것입니다. 이러한 매핑 뷰 추상 방식으로 데이터베이스를 나타내는 Entity SQL 문 집합이 되며 응용 프로그램 도메인 별로 캐시 되는 메타 데이터의 일부입니다. 동일한 응용 프로그램 도메인에서 동일한 컨텍스트의 여러 인스턴스를 만들면 매핑을 뷰 다시 생성 하는 것이 아니라 캐시 된 메타 데이터를 다시 사용 됩니다. 첫 번째 쿼리를 실행 하는 전체 비용의 큰 부분을 매핑 뷰 생성 되므로, Entity Framework를 사용 하면 매핑 뷰를 미리 생성 하 고 컴파일된 프로젝트에 포함할 수 있습니다. 자세한 내용은 [성능 고려 사항 (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md)합니다.

## <a name="generating-mapping-views-with-the-ef-power-tools"></a>EF Power Tools 사용 하 여 뷰 매핑 생성

미리 보기를 생성 하는 가장 쉬운 방법은 사용 하는 것은 [EF Power Tools](http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d)합니다. 설치 된 Power 도구를 만든 후에 뷰를 생성 하는 메뉴 옵션을 아래와 같이 해야 합니다.

-   에 대 한 **Code First** 모델 DbContext 클래스를 포함 하는 코드 파일에는 마우스 오른쪽 단추로 클릭 합니다.
-   에 대 한 **EF 디자이너** 모델 EDMX 파일에는 마우스 오른쪽 단추로 클릭 합니다.

![generateViews](~/ef6/media/generateviews.png)

생성 된 다음과 같은 클래스가 있다고는 프로세스가 완료 되 면

![generatedViews](~/ef6/media/generatedviews.png)

이제 실행 하면 EF 응용 프로그램 필요에 따라 보기를 로드 하려면이 클래스가 사용 됩니다. 모델이 변경 하 고 다시를 생성 하지 않습니다이 클래스는 경우 EF에서 예외가 throw 됩니다.

## <a name="generating-mapping-views-from-code---ef6-onwards"></a>EF6부터 코드에서 뷰 매핑 생성

뷰를 생성 하는 다른 방법은 EF에서 제공 하는 Api를 사용 하는 것입니다. 하지만이 메서드를 사용 하는 경우 자유롭게 있지만 뷰를 직접 로드 해야 뷰를 직렬화 할 수 있습니다.

> [!NOTE]
> **EF6부터만** -Entity Framework 6에서이 섹션에 표시 된 Api 도입 되었습니다. 이전 버전을 사용 하는 경우이 정보는 적용 되지 않습니다.

### <a name="generating-views"></a>뷰 생성

뷰를 생성 하는 Api System.Data.Entity.Core.Mapping.StorageMappingItemCollection 클래스에 있습니다. ObjectContext의 MetadataWorkspace를 사용 하 여 컨텍스트를 StorageMappingCollection를 검색할 수 있습니다. 이 사용 하 여 액세스할 수 있습니다 최신 DbContext API를 사용 하는 경우 아래와 같은 IObjectContextAdapter,이 코드에 dbContext를 호출 하 여 파생된 DbContext의 인스턴스는:

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

StorageMappingItemCollection 했으면 GenerateViews 및 ComputeMappingHashValue 메서드에 대 한 액세스를 가져올 수 있습니다.

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

첫 번째 방법은 컨테이너 매핑에서 각 보기에 대 한 항목이 들어 있는 사전을 만듭니다. 두 번째 방법은 단일 컨테이너 매핑에 대 한 해시 값을 계산 하 고 모델 뷰 미리 생성 된 이후 변경 되지 않은 유효성 검사를 런타임에 사용 됩니다. 재정의 두 가지 방법의 여러 컨테이너 매핑을 포함 하는 복잡 한 시나리오에 대 한 제공 됩니다.

뷰를 생성할 때 GenerateViews 메서드를 호출 하 고 결과 EntitySetBase 및 DbMappingView를 쓸 수는 합니다. ComputeMappingHashValue 메서드에 의해 생성 된 해시를 저장 해야 합니다.

### <a name="loading-views"></a>뷰를 로드합니다.

GenerateViews 메서드에 의해 생성 된 뷰를 로드 하기 위해 DbMappingViewCache 추상 클래스에서 상속 되는 클래스를 사용 하 여 EF를 제공할 수 있습니다. DbMappingViewCache 구현 해야 하는 두 메서드를 지정 합니다.

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

MappingHashValue 속성 ComputeMappingHashValue 메서드에 의해 생성 된 해시를 반환 해야 합니다. EF 때 보기용 요청할 것인지 먼저 생성 되며이 속성에서 반환 하는 해시를 사용 하 여 모델의 해시 값을 비교 합니다. 일치 하지 않는 경우 EF는 EntityCommandCompilationException 예외를 throw 합니다.

영역의 GetView 메서드에 EntitySetBase 받아들이고에 생성 된 EntitySql를 포함 하는 DbMappingVIew GenerateViews 메서드에 의해 생성 된 사전에 지정 된 EntitySetBase 연관 된 반환 해야 합니다. EF를 요청 하는 경우는 없는 다음 GetView 뷰 null을 반환 해야 합니다.

다음은 저장 및 필요한 EntitySql를 검색 하는 한 가지 방법은 알에 설명한 것 처럼 전원 도구를 사용 하 여 생성 되는 DbMappingViewCache에서 추출한 것입니다.

``` csharp
    public override string MappingHashValue
    {
        get { return "a0b843f03dd29abee99789e190a6fb70ce8e93dc97945d437d9a58fb8e2afd2e"; }
    }

    public override DbMappingView GetView(EntitySetBase extent)
    {
        if (extent == null)
        {
            throw new ArgumentNullException("extent");
        }

        var extentName = extent.EntityContainer.Name + "." + extent.Name;

        if (extentName == "BlogContext.Blogs")
        {
            return GetView2();
        }

        if (extentName == "BlogContext.Posts")
        {
            return GetView3();
        }

        return null;
    }

    private static DbMappingView GetView2()
    {
        return new DbMappingView(@"
            SELECT VALUE -- Constructing Blogs
            [BlogApp.Models.Blog](T1.Blog_BlogId, T1.Blog_Test, T1.Blog_title, T1.Blog_Active, T1.Blog_SomeDecimal)
            FROM (
            SELECT
                T.BlogId AS Blog_BlogId,
                T.Test AS Blog_Test,
                T.title AS Blog_title,
                T.Active AS Blog_Active,
                T.SomeDecimal AS Blog_SomeDecimal,
                True AS _from0
            FROM CodeFirstDatabase.Blog AS T
            ) AS T1");
    }
```

EF를 사용 하려면 추가 하 여 DbMappingViewCache에 대해 생성 된 컨텍스트를 지정 하는 DbMappingViewCacheTypeAttribute를 사용 합니다. 아래 코드에서 MyMappingViewCache 클래스를 사용 하 여 BlogContext를 연결 합니다.

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

복잡 한 시나리오에 대 한 매핑 뷰 캐시 팩터리를 지정 하 여 매핑 캐시 인스턴스 보기를 제공할 수 있습니다. 이 추상 클래스 System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory를 구현 하 여 수행할 수 있습니다. 매핑 보기 캐시 팩터리에 사용 되는 인스턴스를 검색 또는 StorageMappingItemCollection.MappingViewCacheFactoryproperty를 사용 하 여 설정할 수 있습니다.
