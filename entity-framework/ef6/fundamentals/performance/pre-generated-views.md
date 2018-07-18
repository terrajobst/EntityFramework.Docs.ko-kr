---
title: 미리 생성 된 매핑 보기-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
caps.latest.revision: 3
ms.openlocfilehash: 9e74176d02afc424118219eec8e016843333cbb8
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122589"
---
# <a name="pre-generated-mapping-views"></a><span data-ttu-id="4eae1-102">미리 생성 된 매핑 보기</span><span class="sxs-lookup"><span data-stu-id="4eae1-102">Pre-generated mapping views</span></span>
<span data-ttu-id="4eae1-103">Entity Framework 쿼리 실행 수 또는 데이터 원본에 변경 내용을 저장 하기 전에 데이터베이스에 액세스 하는 매핑 뷰 집합을 생성 해야 합니다 것입니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-103">Before the Entity Framework can execute a query or save changes to the data source, it must generate a set of mapping views to access the database.</span></span> <span data-ttu-id="4eae1-104">이러한 매핑 뷰 추상 방식으로 데이터베이스를 나타내는 Entity SQL 문 집합이 되며 응용 프로그램 도메인 별로 캐시 되는 메타 데이터의 일부입니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-104">These mapping views are a set of Entity SQL statement that represent the database in an abstract way, and are part of the metadata which is cached per application domain.</span></span> <span data-ttu-id="4eae1-105">동일한 응용 프로그램 도메인에서 동일한 컨텍스트의 여러 인스턴스를 만들면 매핑을 뷰 다시 생성 하는 것이 아니라 캐시 된 메타 데이터를 다시 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-105">If you create multiple instances of the same context in the same application domain, they will reuse mapping views from the cached metadata rather than regenerating them.</span></span> <span data-ttu-id="4eae1-106">첫 번째 쿼리를 실행 하는 전체 비용의 큰 부분을 매핑 뷰 생성 되므로, Entity Framework를 사용 하면 매핑 뷰를 미리 생성 하 고 컴파일된 프로젝트에 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-106">Because mapping view generation is a significant part of the overall cost of executing the first query, the Entity Framework enables you to pre-generate mapping views and include them in the compiled project.</span></span> <span data-ttu-id="4eae1-107">자세한 내용은 [성능 고려 사항 (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-107">For more information, see  [Performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span></span>

## <a name="generating-mapping-views-with-the-ef-power-tools"></a><span data-ttu-id="4eae1-108">EF Power Tools 사용 하 여 뷰 매핑 생성</span><span class="sxs-lookup"><span data-stu-id="4eae1-108">Generating Mapping Views with the EF Power Tools</span></span>

<span data-ttu-id="4eae1-109">미리 보기를 생성 하는 가장 쉬운 방법은 사용 하는 것은 [EF Power Tools](http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d)합니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-109">The easiest way to pre-generate views is to use the [EF Power Tools](http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d).</span></span> <span data-ttu-id="4eae1-110">설치 된 Power 도구를 만든 후에 뷰를 생성 하는 메뉴 옵션을 아래와 같이 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-110">Once you have the Power Tools installed you will have a menu option to Generate Views, as below.</span></span>

-   <span data-ttu-id="4eae1-111">에 대 한 **Code First** 모델 DbContext 클래스를 포함 하는 코드 파일에는 마우스 오른쪽 단추로 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-111">For **Code First** models right-click on the code file that contains your DbContext class.</span></span>
-   <span data-ttu-id="4eae1-112">에 대 한 **EF 디자이너** 모델 EDMX 파일에는 마우스 오른쪽 단추로 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-112">For **EF Designer** models right-click on your EDMX file.</span></span>

![generateViews](~/ef6/media/generateviews.png)

<span data-ttu-id="4eae1-114">생성 된 다음과 같은 클래스가 있다고는 프로세스가 완료 되 면</span><span class="sxs-lookup"><span data-stu-id="4eae1-114">Once the process is finished you will have a class similar to the following generated</span></span>

![generatedViews](~/ef6/media/generatedviews.png)

<span data-ttu-id="4eae1-116">이제 실행 하면 EF 응용 프로그램 필요에 따라 보기를 로드 하려면이 클래스가 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-116">Now when you run your application EF will use this class to load views as required.</span></span> <span data-ttu-id="4eae1-117">모델이 변경 하 고 다시를 생성 하지 않습니다이 클래스는 경우 EF에서 예외가 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-117">If your model changes and you do not re-generate this class then EF will throw an exception.</span></span>

## <a name="generating-mapping-views-from-code---ef6-onwards"></a><span data-ttu-id="4eae1-118">EF6부터 코드에서 뷰 매핑 생성</span><span class="sxs-lookup"><span data-stu-id="4eae1-118">Generating Mapping Views from Code - EF6 Onwards</span></span>

<span data-ttu-id="4eae1-119">뷰를 생성 하는 다른 방법은 EF에서 제공 하는 Api를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-119">The other way to generate views is to use the APIs that EF provides.</span></span> <span data-ttu-id="4eae1-120">하지만이 메서드를 사용 하는 경우 자유롭게 있지만 뷰를 직접 로드 해야 뷰를 직렬화 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-120">When using this method you have the freedom to serialize the views however you like, but you also need to load the views yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="4eae1-121">**EF6부터만** -Entity Framework 6에서이 섹션에 표시 된 Api 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-121">**EF6 Onwards Only** - The APIs shown in this section were introduced in Entity Framework 6.</span></span> <span data-ttu-id="4eae1-122">이전 버전을 사용 하는 경우이 정보는 적용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-122">If you are using an earlier version this information does not apply.</span></span>

### <a name="generating-views"></a><span data-ttu-id="4eae1-123">뷰 생성</span><span class="sxs-lookup"><span data-stu-id="4eae1-123">Generating Views</span></span>

<span data-ttu-id="4eae1-124">뷰를 생성 하는 Api System.Data.Entity.Core.Mapping.StorageMappingItemCollection 클래스에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-124">The APIs to generate views are on the System.Data.Entity.Core.Mapping.StorageMappingItemCollection class.</span></span> <span data-ttu-id="4eae1-125">ObjectContext의 MetadataWorkspace를 사용 하 여 컨텍스트를 StorageMappingCollection를 검색할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-125">You can retrieve a StorageMappingCollection for a Context by using the MetadataWorkspace of an ObjectContext.</span></span> <span data-ttu-id="4eae1-126">이 사용 하 여 액세스할 수 있습니다 최신 DbContext API를 사용 하는 경우 아래와 같은 IObjectContextAdapter,이 코드에 dbContext를 호출 하 여 파생된 DbContext의 인스턴스는:</span><span class="sxs-lookup"><span data-stu-id="4eae1-126">If you are using the newer DbContext API then you can access this by using the IObjectContextAdapter like below, in this code we have an instance of your derived DbContext called dbContext:</span></span>

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

<span data-ttu-id="4eae1-127">StorageMappingItemCollection 했으면 GenerateViews 및 ComputeMappingHashValue 메서드에 대 한 액세스를 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-127">Once you have the StorageMappingItemCollection then you can get access to the GenerateViews and ComputeMappingHashValue methods.</span></span>

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

<span data-ttu-id="4eae1-128">첫 번째 방법은 컨테이너 매핑에서 각 보기에 대 한 항목이 들어 있는 사전을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-128">The first method creates a dictionary with an entry for each view in the container mapping.</span></span> <span data-ttu-id="4eae1-129">두 번째 방법은 단일 컨테이너 매핑에 대 한 해시 값을 계산 하 고 모델 뷰 미리 생성 된 이후 변경 되지 않은 유효성 검사를 런타임에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-129">The second method computes a hash value for the single container mapping and is used at runtime to validate that the model has not changed since the views were pre-generated.</span></span> <span data-ttu-id="4eae1-130">재정의 두 가지 방법의 여러 컨테이너 매핑을 포함 하는 복잡 한 시나리오에 대 한 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-130">Overrides of the two methods are provided for complex scenarios involving multiple container mappings.</span></span>

<span data-ttu-id="4eae1-131">뷰를 생성할 때 GenerateViews 메서드를 호출 하 고 결과 EntitySetBase 및 DbMappingView를 쓸 수는 합니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-131">When generating views you will call the GenerateViews method and then write out the resulting EntitySetBase and DbMappingView.</span></span> <span data-ttu-id="4eae1-132">ComputeMappingHashValue 메서드에 의해 생성 된 해시를 저장 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-132">You will also need to store the hash generated by the ComputeMappingHashValue method.</span></span>

### <a name="loading-views"></a><span data-ttu-id="4eae1-133">뷰를 로드합니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-133">Loading Views</span></span>

<span data-ttu-id="4eae1-134">GenerateViews 메서드에 의해 생성 된 뷰를 로드 하기 위해 DbMappingViewCache 추상 클래스에서 상속 되는 클래스를 사용 하 여 EF를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-134">In order to load the views generated by the GenerateViews method, you can provide EF with a class that inherits from the DbMappingViewCache abstract class.</span></span> <span data-ttu-id="4eae1-135">DbMappingViewCache 구현 해야 하는 두 메서드를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-135">DbMappingViewCache specifies two methods that you must implement:</span></span>

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

<span data-ttu-id="4eae1-136">MappingHashValue 속성 ComputeMappingHashValue 메서드에 의해 생성 된 해시를 반환 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-136">The MappingHashValue property must return the hash generated by the ComputeMappingHashValue method.</span></span> <span data-ttu-id="4eae1-137">EF 때 보기용 요청할 것인지 먼저 생성 되며이 속성에서 반환 하는 해시를 사용 하 여 모델의 해시 값을 비교 합니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-137">When EF is going to ask for views it will first generate and compare the hash value of the model with the hash returned by this property.</span></span> <span data-ttu-id="4eae1-138">일치 하지 않는 경우 EF는 EntityCommandCompilationException 예외를 throw 합니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-138">If they do not match then EF will throw an EntityCommandCompilationException exception.</span></span>

<span data-ttu-id="4eae1-139">영역의 GetView 메서드에 EntitySetBase 받아들이고에 생성 된 EntitySql를 포함 하는 DbMappingVIew GenerateViews 메서드에 의해 생성 된 사전에 지정 된 EntitySetBase 연관 된 반환 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-139">The GetView method will accept an EntitySetBase and you need to return a DbMappingVIew containing the EntitySql that was generated for that was associated with the given EntitySetBase in the dictionary generated by the GenerateViews method.</span></span> <span data-ttu-id="4eae1-140">EF를 요청 하는 경우는 없는 다음 GetView 뷰 null을 반환 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-140">If EF asks for a view that you do not have then GetView should return null.</span></span>

<span data-ttu-id="4eae1-141">다음은 저장 및 필요한 EntitySql를 검색 하는 한 가지 방법은 알에 설명한 것 처럼 전원 도구를 사용 하 여 생성 되는 DbMappingViewCache에서 추출한 것입니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-141">The following is an extract from the DbMappingViewCache that is generated with the Power Tools as described above, in it we see one way to store and retrieve the EntitySql required.</span></span>

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

<span data-ttu-id="4eae1-142">EF를 사용 하려면 추가 하 여 DbMappingViewCache에 대해 생성 된 컨텍스트를 지정 하는 DbMappingViewCacheTypeAttribute를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-142">To have EF use your DbMappingViewCache you add use the DbMappingViewCacheTypeAttribute, specifying the context that it was created for.</span></span> <span data-ttu-id="4eae1-143">아래 코드에서 MyMappingViewCache 클래스를 사용 하 여 BlogContext를 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-143">In the code below we associate the BlogContext with the MyMappingViewCache class.</span></span>

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

<span data-ttu-id="4eae1-144">복잡 한 시나리오에 대 한 매핑 뷰 캐시 팩터리를 지정 하 여 매핑 캐시 인스턴스 보기를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-144">For more complex scenarios, mapping view cache instances can be provided by specifying a mapping view cache factory.</span></span> <span data-ttu-id="4eae1-145">이 추상 클래스 System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory를 구현 하 여 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-145">This can be done by implementing the abstract class System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span></span> <span data-ttu-id="4eae1-146">매핑 보기 캐시 팩터리에 사용 되는 인스턴스를 검색 또는 StorageMappingItemCollection.MappingViewCacheFactoryproperty를 사용 하 여 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4eae1-146">The instance of the mapping view cache factory that is used can be retrieved or set using the StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span></span>
