---
title: 미리 생성 된 매핑 뷰-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: 1fda9fe9638adce9b24a6b81aa081effeb0def81
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416032"
---
# <a name="pre-generated-mapping-views"></a>미리 생성 된 매핑 뷰
Entity Framework 쿼리를 실행 하거나 데이터 원본에 대 한 변경 내용을 저장 하려면 먼저 데이터베이스에 액세스할 수 있는 매핑 뷰 집합을 생성 해야 합니다. 이러한 매핑 뷰는 데이터베이스를 추상적으로 나타내는 일련의 Entity SQL 문이 며, 응용 프로그램 도메인당 캐시 되는 메타 데이터의 일부입니다. 동일한 응용 프로그램 도메인에서 동일한 컨텍스트의 인스턴스를 여러 개 만들 경우 해당 인스턴스를 다시 생성 하는 대신 캐시 된 메타 데이터의 매핑 뷰를 다시 사용 합니다. 매핑 뷰 생성은 첫 번째 쿼리를 실행 하는 전체 비용의 중요 한 부분 이므로 Entity Framework를 사용 하 여 매핑 뷰를 미리 생성 하 고 컴파일된 프로젝트에 포함할 수 있습니다. 자세한 내용은  [성능 고려 사항 (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md)을 참조 하세요.

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a>EF Power Tools Community Edition을 사용 하 여 매핑 보기 생성

보기를 미리 생성 하는 가장 쉬운 방법은 [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition)을 사용 하는 것입니다. Power Tools가 설치 되 면 아래와 같이 보기를 생성 하는 메뉴 옵션을 사용할 수 있습니다.

-   **Code First** 모델의 경우 DbContext 클래스가 포함 된 코드 파일을 마우스 오른쪽 단추로 클릭 합니다.
-   **EF 디자이너** 모델의 경우 EDMX 파일을 마우스 오른쪽 단추로 클릭 합니다.

![뷰 생성](~/ef6/media/generateviews.png)

프로세스가 완료 되 면 다음과 비슷한 클래스가 생성 됩니다.

![생성 된 뷰](~/ef6/media/generatedviews.png)

이제 응용 프로그램을 실행할 때 EF는이 클래스를 사용 하 여 필요에 따라 뷰를 로드 합니다. 모델이 변경 되 고이 클래스를 다시 생성 하지 않으면 EF는 예외를 throw 합니다.

## <a name="generating-mapping-views-from-code---ef6-onwards"></a>코드에서 매핑 뷰 생성-EF6 이상

뷰를 생성 하는 다른 방법은 EF에서 제공 하는 Api를 사용 하는 것입니다. 이 메서드를 사용 하는 경우 원하는 뷰를 자유롭게 serialize 할 수 있지만 뷰를 직접 로드 해야 합니다.

> [!NOTE]
> **EF6** 이상-이 섹션에 표시 된 api는 Entity Framework 6에서 도입 되었습니다. 이전 버전을 사용 하는 경우이 정보는 적용 되지 않습니다.

### <a name="generating-views"></a>뷰 생성

뷰를 생성 하는 Api는 StorageMappingItemCollection 클래스에 있습니다. ObjectContext의 MetadataWorkspace를 사용 하 여 컨텍스트의 StorageMappingCollection를 검색할 수 있습니다. 최신 DbContext API를 사용 하는 경우 아래와 같은 IObjectContextAdapter를 사용 하 여이에 액세스할 수 있습니다 .이 코드에서는 dbContext 라는 파생 DbContext의 인스턴스를 포함 합니다.

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

StorageMappingItemCollection 하면 GenerateViews 및 ComputeMappingHashValue 메서드에 대 한 액세스 권한을 얻을 수 있습니다.

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

첫 번째 메서드는 컨테이너 매핑의 각 뷰에 대해 항목을 사용 하 여 사전을 만듭니다. 두 번째 메서드는 단일 컨테이너 매핑에 대 한 해시 값을 계산 하 고 런타임에 뷰가 미리 생성 된 이후 모델이 변경 되지 않은 것을 확인 하는 데 사용 됩니다. 여러 컨테이너 매핑과 관련 된 복잡 한 시나리오에는 두 메서드의 재정의가 제공 됩니다.

뷰를 생성할 때 GenerateViews 메서드를 호출한 다음 결과 EntitySetBase 및 DbMappingView를 작성 합니다. ComputeMappingHashValue 메서드에 의해 생성 된 해시도 저장 해야 합니다.

### <a name="loading-views"></a>뷰 로드

GenerateViews 메서드에서 생성 된 뷰를 로드 하기 위해 DbMappingViewCache abstract 클래스에서 상속 되는 클래스를 사용 하 여 EF를 제공할 수 있습니다. DbMappingViewCache는 구현 해야 하는 두 가지 메서드를 지정 합니다.

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

MappingHashValue 속성은 ComputeMappingHashValue 메서드에 의해 생성 된 해시를 반환 해야 합니다. EF가 보기를 요청 하면 먼저 모델의 해시 값을 생성 하 여이 속성에 의해 반환 된 해시와 비교 합니다. 일치 하지 않는 경우 EF가 EntityCommandCompilationException 예외를 throw 합니다.

GetView 메서드는 EntitySetBase를 수락 하며, GenerateViews 메서드에 의해 생성 된 사전에서 지정 된 EntitySetBase와 연결 된에 대해 생성 된 EntitySql이 포함 된 DbMappingVIew를 반환 해야 합니다. EF가 없는 뷰를 요청 하는 경우에는 GetView에서 null을 반환 해야 합니다.

다음은 위에 설명 된 대로 전원 도구를 사용 하 여 생성 되는 DbMappingViewCache의 추출입니다. 여기에는 필요한 EntitySql을 저장 하 고 검색 하는 한 가지 방법이 나와 있습니다.

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

EF를 사용 하 여 DbMappingViewCache를 사용 하도록 하려면 DbMappingViewCacheTypeAttribute를 사용 하 여 해당 개체를 만든 컨텍스트를 지정 합니다. 아래 코드에서는 BlogContext를 MyMappingViewCache 클래스와 연결 합니다.

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

더 복잡 한 시나리오의 경우 매핑 뷰 캐시 인스턴스는 매핑 뷰 캐시 팩터리를 지정 하 여 제공 될 수 있습니다. 추상 클래스 MappingViews. DbMappingViewCacheFactory를 구현 하 여이 작업을 수행할 수 있습니다. StorageMappingItemCollection. MappingViewCacheFactoryproperty를 사용 하 여 사용 되는 매핑 뷰 캐시 팩터리의 인스턴스를 검색 하거나 설정할 수 있습니다.
