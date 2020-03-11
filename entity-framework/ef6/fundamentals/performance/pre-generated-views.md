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
# <a name="pre-generated-mapping-views"></a><span data-ttu-id="012f8-102">미리 생성 된 매핑 뷰</span><span class="sxs-lookup"><span data-stu-id="012f8-102">Pre-generated mapping views</span></span>
<span data-ttu-id="012f8-103">Entity Framework 쿼리를 실행 하거나 데이터 원본에 대 한 변경 내용을 저장 하려면 먼저 데이터베이스에 액세스할 수 있는 매핑 뷰 집합을 생성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-103">Before the Entity Framework can execute a query or save changes to the data source, it must generate a set of mapping views to access the database.</span></span> <span data-ttu-id="012f8-104">이러한 매핑 뷰는 데이터베이스를 추상적으로 나타내는 일련의 Entity SQL 문이 며, 응용 프로그램 도메인당 캐시 되는 메타 데이터의 일부입니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-104">These mapping views are a set of Entity SQL statement that represent the database in an abstract way, and are part of the metadata which is cached per application domain.</span></span> <span data-ttu-id="012f8-105">동일한 응용 프로그램 도메인에서 동일한 컨텍스트의 인스턴스를 여러 개 만들 경우 해당 인스턴스를 다시 생성 하는 대신 캐시 된 메타 데이터의 매핑 뷰를 다시 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-105">If you create multiple instances of the same context in the same application domain, they will reuse mapping views from the cached metadata rather than regenerating them.</span></span> <span data-ttu-id="012f8-106">매핑 뷰 생성은 첫 번째 쿼리를 실행 하는 전체 비용의 중요 한 부분 이므로 Entity Framework를 사용 하 여 매핑 뷰를 미리 생성 하 고 컴파일된 프로젝트에 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-106">Because mapping view generation is a significant part of the overall cost of executing the first query, the Entity Framework enables you to pre-generate mapping views and include them in the compiled project.</span></span><span data-ttu-id="012f8-107"> 자세한 내용은  [성능 고려 사항 (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="012f8-107"> For more information, see  [Performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span></span>

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a><span data-ttu-id="012f8-108">EF Power Tools Community Edition을 사용 하 여 매핑 보기 생성</span><span class="sxs-lookup"><span data-stu-id="012f8-108">Generating Mapping Views with the EF Power Tools Community Edition</span></span>

<span data-ttu-id="012f8-109">보기를 미리 생성 하는 가장 쉬운 방법은 [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition)을 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-109">The easiest way to pre-generate views is to use the [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition).</span></span> <span data-ttu-id="012f8-110">Power Tools가 설치 되 면 아래와 같이 보기를 생성 하는 메뉴 옵션을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-110">Once you have the Power Tools installed you will have a menu option to Generate Views, as below.</span></span>

-   <span data-ttu-id="012f8-111">**Code First** 모델의 경우 DbContext 클래스가 포함 된 코드 파일을 마우스 오른쪽 단추로 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-111">For **Code First** models right-click on the code file that contains your DbContext class.</span></span>
-   <span data-ttu-id="012f8-112">**EF 디자이너** 모델의 경우 EDMX 파일을 마우스 오른쪽 단추로 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-112">For **EF Designer** models right-click on your EDMX file.</span></span>

![뷰 생성](~/ef6/media/generateviews.png)

<span data-ttu-id="012f8-114">프로세스가 완료 되 면 다음과 비슷한 클래스가 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-114">Once the process is finished you will have a class similar to the following generated</span></span>

![생성 된 뷰](~/ef6/media/generatedviews.png)

<span data-ttu-id="012f8-116">이제 응용 프로그램을 실행할 때 EF는이 클래스를 사용 하 여 필요에 따라 뷰를 로드 합니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-116">Now when you run your application EF will use this class to load views as required.</span></span> <span data-ttu-id="012f8-117">모델이 변경 되 고이 클래스를 다시 생성 하지 않으면 EF는 예외를 throw 합니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-117">If your model changes and you do not re-generate this class then EF will throw an exception.</span></span>

## <a name="generating-mapping-views-from-code---ef6-onwards"></a><span data-ttu-id="012f8-118">코드에서 매핑 뷰 생성-EF6 이상</span><span class="sxs-lookup"><span data-stu-id="012f8-118">Generating Mapping Views from Code - EF6 Onwards</span></span>

<span data-ttu-id="012f8-119">뷰를 생성 하는 다른 방법은 EF에서 제공 하는 Api를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-119">The other way to generate views is to use the APIs that EF provides.</span></span> <span data-ttu-id="012f8-120">이 메서드를 사용 하는 경우 원하는 뷰를 자유롭게 serialize 할 수 있지만 뷰를 직접 로드 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-120">When using this method you have the freedom to serialize the views however you like, but you also need to load the views yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="012f8-121">**EF6** 이상-이 섹션에 표시 된 api는 Entity Framework 6에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-121">**EF6 Onwards Only** - The APIs shown in this section were introduced in Entity Framework 6.</span></span> <span data-ttu-id="012f8-122">이전 버전을 사용 하는 경우이 정보는 적용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-122">If you are using an earlier version this information does not apply.</span></span>

### <a name="generating-views"></a><span data-ttu-id="012f8-123">뷰 생성</span><span class="sxs-lookup"><span data-stu-id="012f8-123">Generating Views</span></span>

<span data-ttu-id="012f8-124">뷰를 생성 하는 Api는 StorageMappingItemCollection 클래스에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-124">The APIs to generate views are on the System.Data.Entity.Core.Mapping.StorageMappingItemCollection class.</span></span> <span data-ttu-id="012f8-125">ObjectContext의 MetadataWorkspace를 사용 하 여 컨텍스트의 StorageMappingCollection를 검색할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-125">You can retrieve a StorageMappingCollection for a Context by using the MetadataWorkspace of an ObjectContext.</span></span> <span data-ttu-id="012f8-126">최신 DbContext API를 사용 하는 경우 아래와 같은 IObjectContextAdapter를 사용 하 여이에 액세스할 수 있습니다 .이 코드에서는 dbContext 라는 파생 DbContext의 인스턴스를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-126">If you are using the newer DbContext API then you can access this by using the IObjectContextAdapter like below, in this code we have an instance of your derived DbContext called dbContext:</span></span>

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

<span data-ttu-id="012f8-127">StorageMappingItemCollection 하면 GenerateViews 및 ComputeMappingHashValue 메서드에 대 한 액세스 권한을 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-127">Once you have the StorageMappingItemCollection then you can get access to the GenerateViews and ComputeMappingHashValue methods.</span></span>

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

<span data-ttu-id="012f8-128">첫 번째 메서드는 컨테이너 매핑의 각 뷰에 대해 항목을 사용 하 여 사전을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-128">The first method creates a dictionary with an entry for each view in the container mapping.</span></span> <span data-ttu-id="012f8-129">두 번째 메서드는 단일 컨테이너 매핑에 대 한 해시 값을 계산 하 고 런타임에 뷰가 미리 생성 된 이후 모델이 변경 되지 않은 것을 확인 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-129">The second method computes a hash value for the single container mapping and is used at runtime to validate that the model has not changed since the views were pre-generated.</span></span> <span data-ttu-id="012f8-130">여러 컨테이너 매핑과 관련 된 복잡 한 시나리오에는 두 메서드의 재정의가 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-130">Overrides of the two methods are provided for complex scenarios involving multiple container mappings.</span></span>

<span data-ttu-id="012f8-131">뷰를 생성할 때 GenerateViews 메서드를 호출한 다음 결과 EntitySetBase 및 DbMappingView를 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-131">When generating views you will call the GenerateViews method and then write out the resulting EntitySetBase and DbMappingView.</span></span> <span data-ttu-id="012f8-132">ComputeMappingHashValue 메서드에 의해 생성 된 해시도 저장 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-132">You will also need to store the hash generated by the ComputeMappingHashValue method.</span></span>

### <a name="loading-views"></a><span data-ttu-id="012f8-133">뷰 로드</span><span class="sxs-lookup"><span data-stu-id="012f8-133">Loading Views</span></span>

<span data-ttu-id="012f8-134">GenerateViews 메서드에서 생성 된 뷰를 로드 하기 위해 DbMappingViewCache abstract 클래스에서 상속 되는 클래스를 사용 하 여 EF를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-134">In order to load the views generated by the GenerateViews method, you can provide EF with a class that inherits from the DbMappingViewCache abstract class.</span></span> <span data-ttu-id="012f8-135">DbMappingViewCache는 구현 해야 하는 두 가지 메서드를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-135">DbMappingViewCache specifies two methods that you must implement:</span></span>

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

<span data-ttu-id="012f8-136">MappingHashValue 속성은 ComputeMappingHashValue 메서드에 의해 생성 된 해시를 반환 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-136">The MappingHashValue property must return the hash generated by the ComputeMappingHashValue method.</span></span> <span data-ttu-id="012f8-137">EF가 보기를 요청 하면 먼저 모델의 해시 값을 생성 하 여이 속성에 의해 반환 된 해시와 비교 합니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-137">When EF is going to ask for views it will first generate and compare the hash value of the model with the hash returned by this property.</span></span> <span data-ttu-id="012f8-138">일치 하지 않는 경우 EF가 EntityCommandCompilationException 예외를 throw 합니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-138">If they do not match then EF will throw an EntityCommandCompilationException exception.</span></span>

<span data-ttu-id="012f8-139">GetView 메서드는 EntitySetBase를 수락 하며, GenerateViews 메서드에 의해 생성 된 사전에서 지정 된 EntitySetBase와 연결 된에 대해 생성 된 EntitySql이 포함 된 DbMappingVIew를 반환 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-139">The GetView method will accept an EntitySetBase and you need to return a DbMappingVIew containing the EntitySql that was generated for that was associated with the given EntitySetBase in the dictionary generated by the GenerateViews method.</span></span> <span data-ttu-id="012f8-140">EF가 없는 뷰를 요청 하는 경우에는 GetView에서 null을 반환 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-140">If EF asks for a view that you do not have then GetView should return null.</span></span>

<span data-ttu-id="012f8-141">다음은 위에 설명 된 대로 전원 도구를 사용 하 여 생성 되는 DbMappingViewCache의 추출입니다. 여기에는 필요한 EntitySql을 저장 하 고 검색 하는 한 가지 방법이 나와 있습니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-141">The following is an extract from the DbMappingViewCache that is generated with the Power Tools as described above, in it we see one way to store and retrieve the EntitySql required.</span></span>

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

<span data-ttu-id="012f8-142">EF를 사용 하 여 DbMappingViewCache를 사용 하도록 하려면 DbMappingViewCacheTypeAttribute를 사용 하 여 해당 개체를 만든 컨텍스트를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-142">To have EF use your DbMappingViewCache you add use the DbMappingViewCacheTypeAttribute, specifying the context that it was created for.</span></span> <span data-ttu-id="012f8-143">아래 코드에서는 BlogContext를 MyMappingViewCache 클래스와 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-143">In the code below we associate the BlogContext with the MyMappingViewCache class.</span></span>

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

<span data-ttu-id="012f8-144">더 복잡 한 시나리오의 경우 매핑 뷰 캐시 인스턴스는 매핑 뷰 캐시 팩터리를 지정 하 여 제공 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-144">For more complex scenarios, mapping view cache instances can be provided by specifying a mapping view cache factory.</span></span> <span data-ttu-id="012f8-145">추상 클래스 MappingViews. DbMappingViewCacheFactory를 구현 하 여이 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-145">This can be done by implementing the abstract class System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span></span> <span data-ttu-id="012f8-146">StorageMappingItemCollection. MappingViewCacheFactoryproperty를 사용 하 여 사용 되는 매핑 뷰 캐시 팩터리의 인스턴스를 검색 하거나 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="012f8-146">The instance of the mapping view cache factory that is used can be retrieved or set using the StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span></span>
