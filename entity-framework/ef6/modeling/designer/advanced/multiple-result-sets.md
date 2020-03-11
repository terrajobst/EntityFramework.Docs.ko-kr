---
title: 여러 결과 집합을 포함 하는 저장 프로시저-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
ms.openlocfilehash: 098ed88ba52e211965baf3660f0e51bd74c71efd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415456"
---
# <a name="stored-procedures-with-multiple-result-sets"></a><span data-ttu-id="6ce35-102">여러 결과 집합을 포함 하는 저장 프로시저</span><span class="sxs-lookup"><span data-stu-id="6ce35-102">Stored Procedures with Multiple Result Sets</span></span>
<span data-ttu-id="6ce35-103">저장 프로시저를 사용할 때 두 개 이상의 결과 집합을 반환 해야 하는 경우도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-103">Sometimes when using stored procedures you will need to return more than one result set.</span></span> <span data-ttu-id="6ce35-104">이 시나리오는 단일 화면을 구성 하는 데 필요한 데이터베이스 왕복 횟수를 줄이는 데 주로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-104">This scenario is commonly used to reduce the number of database round trips required to compose a single screen.</span></span><span data-ttu-id="6ce35-105"> EF5 이전에는 Entity Framework 저장 프로시저를 호출할 수 있지만 첫 번째 결과 집합만 호출 코드로 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-105"> Prior to EF5, Entity Framework would allow the stored procedure to be called but would only return the first result set to the calling code.</span></span>

<span data-ttu-id="6ce35-106">이 문서에서는 Entity Framework의 저장 프로시저에서 둘 이상의 결과 집합에 액세스 하는 데 사용할 수 있는 두 가지 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-106">This article will show you two ways that you can use to access more than one result set from a stored procedure in Entity Framework.</span></span> <span data-ttu-id="6ce35-107">코드를 사용 하 고 코드 first와 EF Designer 모두와 함께 작동 하는 코드와 EF Designer 에서만 작동 하는 항목입니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-107">One that uses just code and works with both Code first and the EF Designer and one that only works with the EF Designer.</span></span> <span data-ttu-id="6ce35-108">이에 대 한 도구 및 API 지원은 이후 버전의 Entity Framework에서 개선 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-108">The tooling and API support for this should improve in future versions of Entity Framework.</span></span>

## <a name="model"></a><span data-ttu-id="6ce35-109">모델</span><span class="sxs-lookup"><span data-stu-id="6ce35-109">Model</span></span>

<span data-ttu-id="6ce35-110">이 문서의 예제에서는 블로그에 많은 게시물이 있고 게시물이 단일 블로그에 속하는 기본 블로그 및 게시물 모델을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-110">The examples in this article use a basic Blog and Posts model where a blog has many posts and a post belongs to a single blog.</span></span> <span data-ttu-id="6ce35-111">모든 블로그 및 게시물을 반환 하는 데이터베이스의 저장 프로시저를 사용 합니다. 예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-111">We will use a stored procedure in the database that returns all blogs and posts, something like this:</span></span>

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a><span data-ttu-id="6ce35-112">코드를 사용 하 여 여러 결과 집합에 액세스</span><span class="sxs-lookup"><span data-stu-id="6ce35-112">Accessing Multiple Result Sets with Code</span></span>

<span data-ttu-id="6ce35-113">코드를 실행 하 여 저장 프로시저를 실행 하는 원시 SQL 명령을 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-113">We can execute use code to issue a raw SQL command to execute our stored procedure.</span></span> <span data-ttu-id="6ce35-114">이 방법의 장점은 Code first와 EF Designer 모두에서 작동 한다는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-114">The advantage of this approach is that it works with both Code first and the EF Designer.</span></span>

<span data-ttu-id="6ce35-115">여러 결과 집합을 사용 하려면 IObjectContextAdapter 인터페이스를 사용 하 여 ObjectContext API에 삭제 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-115">In order to get multiple result sets working we need to drop to the ObjectContext API by using the IObjectContextAdapter interface.</span></span>

<span data-ttu-id="6ce35-116">ObjectContext를 만든 후에는 변환 메서드를 사용 하 여 저장 프로시저의 결과를 EF로 추적 하 고 사용할 수 있는 엔터티로 변환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-116">Once we have an ObjectContext then we can use the Translate method to translate the results of our stored procedure into entities that can be tracked and used in EF as normal.</span></span> <span data-ttu-id="6ce35-117">다음 코드 샘플에서는 이러한 작업을 수행 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-117">The following code sample demonstrates this in action.</span></span>

``` csharp
    using (var db = new BloggingContext())
    {
        // If using Code First we need to make sure the model is built before we open the connection
        // This isn't required for models created with the EF Designer
        db.Database.Initialize(force: false);

        // Create a SQL command to execute the sproc
        var cmd = db.Database.Connection.CreateCommand();
        cmd.CommandText = "[dbo].[GetAllBlogsAndPosts]";

        try
        {

            db.Database.Connection.Open();
            // Run the sproc
            var reader = cmd.ExecuteReader();

            // Read Blogs from the first result set
            var blogs = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Blog>(reader, "Blogs", MergeOption.AppendOnly);   


            foreach (var item in blogs)
            {
                Console.WriteLine(item.Name);
            }        

            // Move to second result set and read Posts
            reader.NextResult();
            var posts = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Post>(reader, "Posts", MergeOption.AppendOnly);


            foreach (var item in posts)
            {
                Console.WriteLine(item.Title);
            }
        }
        finally
        {
            db.Database.Connection.Close();
        }
    }
```

<span data-ttu-id="6ce35-118">변환 메서드는 프로시저, EntitySet 이름 및 MergeOption을 실행할 때 받은 판독기를 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-118">The Translate method accepts the reader that we received when we executed the procedure, an EntitySet name, and a MergeOption.</span></span> <span data-ttu-id="6ce35-119">EntitySet 이름은 파생 컨텍스트의 DbSet 속성과 동일 합니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-119">The EntitySet name will be the same as the DbSet property on your derived context.</span></span> <span data-ttu-id="6ce35-120">MergeOption enum은 동일한 엔터티가 메모리에 이미 있는 경우 결과를 처리 하는 방법을 제어 합니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-120">The MergeOption enum controls how results are handled if the same entity already exists in memory.</span></span>

<span data-ttu-id="6ce35-121">다음 결과 집합으로 이동 하기 전에 첫 번째 결과 집합을 사용 해야 하기 때문에이 코드는 다음 결과 집합으로 이동 하기 전에 다음 결과 집합을 사용 해야 하기 때문에이 경우에는 NextResult를 호출 하기 전에 블로그 컬렉션에서</span><span class="sxs-lookup"><span data-stu-id="6ce35-121">Here we iterate through the collection of blogs before we call NextResult, this is important given the above code because the first result set must be consumed before moving to the next result set.</span></span>

<span data-ttu-id="6ce35-122">두 변환 메서드를 호출 하면 블로그 및 게시물 엔터티가 다른 엔터티와 동일한 방식으로 EF를 통해 추적 되므로 수정 하거나 삭제 하 고 정상적으로 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-122">Once the two translate methods are called then the Blog and Post entities are tracked by EF the same way as any other entity and so can be modified or deleted and saved as normal.</span></span>

>[!NOTE]
> <span data-ttu-id="6ce35-123">EF는 변환 메서드를 사용 하 여 엔터티를 만들 때 어떤 매핑도 고려 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-123">EF does not take any mapping into account when it creates entities using the Translate method.</span></span> <span data-ttu-id="6ce35-124">결과 집합의 열 이름을 클래스의 속성 이름과 일치 하기만 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-124">It will simply match column names in the result set with property names on your classes.</span></span>

>[!NOTE]
> <span data-ttu-id="6ce35-125">지연 로드를 사용 하도록 설정 하 고 블로그 엔터티 중 하나에서 게시물 속성에 액세스 하는 경우 EF는 이미 모든 게시를 로드 한 경우에도 데이터베이스에 연결 하 여 모든 게시물을 지연 로드 합니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-125">That if you have lazy loading enabled, accessing the posts property on one of the blog entities then EF will connect to the database to lazily load all posts, even though we have already loaded them all.</span></span> <span data-ttu-id="6ce35-126">EF는 모든 게시물을 로드 했는지 아니면 데이터베이스에 추가 된 내용이 있는지 여부를 알 수 없기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-126">This is because EF cannot know whether or not you have loaded all posts or if there are more in the database.</span></span> <span data-ttu-id="6ce35-127">이렇게 하지 않으려면 지연 로드를 사용 하지 않도록 설정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-127">If you want to avoid this then you will need to disable lazy loading.</span></span>

## <a name="multiple-result-sets-with-configured-in-edmx"></a><span data-ttu-id="6ce35-128">EDMX에서 구성 된 여러 결과 집합</span><span class="sxs-lookup"><span data-stu-id="6ce35-128">Multiple Result Sets with Configured in EDMX</span></span>

>[!NOTE]
> <span data-ttu-id="6ce35-129">EDMX에서 여러 결과 집합을 구성할 수 있으려면 .NET Framework 4.5를 대상으로 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-129">You must target .NET Framework 4.5 to be able to configure multiple result sets in EDMX.</span></span> <span data-ttu-id="6ce35-130">.NET 4.0를 대상으로 하는 경우 이전 섹션에 표시 된 코드 기반 메서드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-130">If you are targeting .NET 4.0 you can use the code-based method shown in the previous section.</span></span>

<span data-ttu-id="6ce35-131">EF Designer를 사용 하는 경우 반환 되는 다른 결과 집합을 알 수 있도록 모델을 수정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-131">If you are using the EF Designer, you can also modify your model so that it knows about the different result sets that will be returned.</span></span> <span data-ttu-id="6ce35-132">이전에 알아두어야 할 한 가지 사항은 도구가 여러 결과 집합을 인식 하지 않기 때문입니다. 따라서 edmx 파일을 수동으로 편집 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-132">One thing to know before hand is that the tooling is not multiple result set aware, so you will need to manually edit the edmx file.</span></span> <span data-ttu-id="6ce35-133">이와 같이 edmx 파일을 편집 하는 작업은 작동 하지만 VS에서 모델의 유효성 검사도 중단 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-133">Editing the edmx file like this will work but it will also break the validation of the model in VS.</span></span> <span data-ttu-id="6ce35-134">따라서 모델의 유효성을 검사 하는 경우 항상 오류가 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-134">So if you validate your model you will always get errors.</span></span>

-   <span data-ttu-id="6ce35-135">이 작업을 수행 하려면 단일 결과 집합 쿼리와 마찬가지로 저장 프로시저를 모델에 추가 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-135">In order to do this you need to add the stored procedure to your model as you would for a single result set query.</span></span>
-   <span data-ttu-id="6ce35-136">이렇게 하면 모델을 마우스 오른쪽 단추로 클릭 하 고 연결 프로그램을 선택 해야 **합니다.**</span><span class="sxs-lookup"><span data-stu-id="6ce35-136">Once you have this then you need to right click on your model and select **Open With..**</span></span> <span data-ttu-id="6ce35-137">그런 다음 **Xml**</span><span class="sxs-lookup"><span data-stu-id="6ce35-137">then **Xml**</span></span>

    ![다음으로 열기](~/ef6/media/openas.png)

<span data-ttu-id="6ce35-139">모델을 XML로 연 후에는 다음 단계를 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-139">Once you have the model opened as XML then you need to do the following steps:</span></span>

-   <span data-ttu-id="6ce35-140">모델에서 복합 형식 및 함수 가져오기를 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-140">Find the complex type and function import in your model:</span></span>

``` xml
    <!-- CSDL content -->
    <edmx:ConceptualModels>

    ...

      <FunctionImport Name="GetAllBlogsAndPosts" ReturnType="Collection(BlogModel.GetAllBlogsAndPosts_Result)" />

    ...

      <ComplexType Name="GetAllBlogsAndPosts_Result">
        <Property Type="Int32" Name="BlogId" Nullable="false" />
        <Property Type="String" Name="Name" Nullable="false" MaxLength="255" />
        <Property Type="String" Name="Description" Nullable="true" />
      </ComplexType>

    ...

    </edmx:ConceptualModels>
```

 

-   <span data-ttu-id="6ce35-141">복합 형식 제거</span><span class="sxs-lookup"><span data-stu-id="6ce35-141">Remove the complex type</span></span>
-   <span data-ttu-id="6ce35-142">함수 가져오기를 엔터티에 매핑되도록 업데이트 합니다 .이 경우에는 다음과 같이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-142">Update the function import so that it maps to your entities, in our case it will look like the following:</span></span>

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

<span data-ttu-id="6ce35-143">이렇게 하면 저장 프로시저가 두 개의 컬렉션 (블로그 항목 중 하나 및 post 항목 중 하나)을 반환 한다는 것을 모델에 알립니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-143">This tells the model that the stored procedure will return two collections, one of blog entries and one of post entries.</span></span>

-   <span data-ttu-id="6ce35-144">함수 매핑 요소를 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-144">Find the function mapping element:</span></span>

``` xml
    <!-- C-S mapping content -->
    <edmx:Mappings>

    ...

      <FunctionImportMapping FunctionImportName="GetAllBlogsAndPosts" FunctionName="BlogModel.Store.GetAllBlogsAndPosts">
        <ResultMapping>
          <ComplexTypeMapping TypeName="BlogModel.GetAllBlogsAndPosts_Result">
            <ScalarProperty Name="BlogId" ColumnName="BlogId" />
            <ScalarProperty Name="Name" ColumnName="Name" />
            <ScalarProperty Name="Description" ColumnName="Description" />
          </ComplexTypeMapping>
        </ResultMapping>
      </FunctionImportMapping>

    ...

    </edmx:Mappings>
```

-   <span data-ttu-id="6ce35-145">반환 되는 각 엔터티에 대해 다음과 같이 결과 매핑을 하나로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-145">Replace the result mapping with one for each entity being returned, such as the following:</span></span>

``` xml
    <ResultMapping>
      <EntityTypeMapping TypeName ="BlogModel.Blog">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="Name" ColumnName="Name" />
        <ScalarProperty Name="Description" ColumnName="Description" />
      </EntityTypeMapping>
    </ResultMapping>
    <ResultMapping>
      <EntityTypeMapping TypeName="BlogModel.Post">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="PostId" ColumnName="PostId"/>
        <ScalarProperty Name="Title" ColumnName="Title" />
        <ScalarProperty Name="Text" ColumnName="Text" />
      </EntityTypeMapping>
    </ResultMapping>
```

<span data-ttu-id="6ce35-146">기본적으로 만들어진 것과 같은 복합 형식에 결과 집합을 매핑할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-146">It is also possible to map the result sets to complex types, such as the one created by default.</span></span> <span data-ttu-id="6ce35-147">이렇게 하려면 새 복합 형식을 제거 하는 대신 새 복합 형식을 만들고 위의 예제에서 엔터티 이름을 사용한 모든 위치에서 복합 형식을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-147">To do this you create a new complex type, instead of removing them, and use the complex types everywhere that you had used the entity names in the examples above.</span></span>

<span data-ttu-id="6ce35-148">이러한 매핑이 변경 된 후에는 모델을 저장 하 고 다음 코드를 실행 하 여 저장 프로시저를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-148">Once these mappings have been changed then you can save the model and execute the following code to use the stored procedure:</span></span>

``` csharp
    using (var db = new BlogEntities())
    {
        var results = db.GetAllBlogsAndPosts();

        foreach (var result in results)
        {
            Console.WriteLine("Blog: " + result.Name);
        }

        var posts = results.GetNextResult<Post>();

        foreach (var result in posts)
        {
            Console.WriteLine("Post: " + result.Title);
        }

        Console.ReadLine();
    }
```

>[!NOTE]
> <span data-ttu-id="6ce35-149">모델에 대 한 edmx 파일을 수동으로 편집 하는 경우 데이터베이스에서 모델을 다시 생성 하는 경우 덮어씁니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-149">If you manually edit the edmx file for your model it will be overwritten if you ever regenerate the model from the database.</span></span>

## <a name="summary"></a><span data-ttu-id="6ce35-150">요약</span><span class="sxs-lookup"><span data-stu-id="6ce35-150">Summary</span></span>

<span data-ttu-id="6ce35-151">여기서는 Entity Framework을 사용 하 여 여러 결과 집합에 액세스 하는 두 가지 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-151">Here we have shown two different methods of accessing multiple result sets using Entity Framework.</span></span> <span data-ttu-id="6ce35-152">이러한 두 가지 모두 상황 및 기본 설정에 따라 동일 하 게 유효 하며, 환경에 가장 적합 한 것을 선택 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-152">Both of them are equally valid depending on your situation and preferences and you should choose the one that seems best for your circumstances.</span></span> <span data-ttu-id="6ce35-153">이후 버전의 Entity Framework에서는 여러 결과 집합에 대 한 지원이 향상 될 예정 이며,이 문서의 단계를 수행 하는 것은 더 이상 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="6ce35-153">It is planned that the support for multiple result sets will be improved in future versions of Entity Framework and that performing the steps in this document will no longer be necessary.</span></span>
