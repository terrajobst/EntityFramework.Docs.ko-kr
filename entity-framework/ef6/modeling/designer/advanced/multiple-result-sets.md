---
title: 여러 결과 집합-EF6 사용 하 여 프로시저를 저장합니다.
author: divega
ms.date: 2016-10-23
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
ms.openlocfilehash: 56c28f05bd7efe1b54d6cadd32afe0e9c6cf38b5
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251013"
---
# <a name="stored-procedures-with-multiple-result-sets"></a><span data-ttu-id="3c705-102">여러 결과 집합을 사용 하 여 저장된 프로시저</span><span class="sxs-lookup"><span data-stu-id="3c705-102">Stored Procedures with Multiple Result Sets</span></span>
<span data-ttu-id="3c705-103">사용 하 여 저장 하는 경우에 따라 둘 이상의 결과 반환 해야 하는 프로시저 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-103">Sometimes when using stored procedures you will need to return more than one result set.</span></span> <span data-ttu-id="3c705-104">이 시나리오는 데이터베이스의 수를 줄이는 데 일반적으로 단일 화면을 구성 하는 데 필요한 왕복 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-104">This scenario is commonly used to reduce the number of database round trips required to compose a single screen.</span></span> <span data-ttu-id="3c705-105">EF5, 이전 Entity Framework 저장된 프로시저를 호출할 수 있지만 첫 번째 결과 집합 호출 코드에만 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-105">Prior to EF5, Entity Framework would allow the stored procedure to be called but would only return the first result set to the calling code.</span></span>

<span data-ttu-id="3c705-106">이 문서에서는 Entity Framework에서 저장된 프로시저에서 둘 이상의 결과 집합에 액세스 하는 데 사용할 수 있는 두 가지 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-106">This article will show you two ways that you can use to access more than one result set from a stored procedure in Entity Framework.</span></span> <span data-ttu-id="3c705-107">단순히 코드를 사용 하 여 먼저 모두 코드와 함께 작동 하는 EF 디자이너 및 EF 디자이너 에서만 작동 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-107">One that uses just code and works with both Code first and the EF Designer and one that only works with the EF Designer.</span></span> <span data-ttu-id="3c705-108">도구 및이 대 한 API 지원을 개선 해야 나중에 Entity Framework의 버전입니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-108">The tooling and API support for this should improve in future versions of Entity Framework.</span></span>

## <a name="model"></a><span data-ttu-id="3c705-109">모델</span><span class="sxs-lookup"><span data-stu-id="3c705-109">Model</span></span>

<span data-ttu-id="3c705-110">이 문서의 예제에서는 기본 블로그를 사용 하 고 단일 블로그 게시물 모델 블로그 게시물 및 다양 한 게시물에 속한 키를 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-110">The examples in this article use a basic Blog and Posts model where a blog has many posts and a post belongs to a single blog.</span></span> <span data-ttu-id="3c705-111">모든 블로그 및 게시물을 다음과 같이 결과 반환 하는 데이터베이스에서 저장된 프로시저를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-111">We will use a stored procedure in the database that returns all blogs and posts, something like this:</span></span>

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a><span data-ttu-id="3c705-112">코드를 사용 하 여 설정 하는 여러 결과 액세스</span><span class="sxs-lookup"><span data-stu-id="3c705-112">Accessing Multiple Result Sets with Code</span></span>

<span data-ttu-id="3c705-113">이 저장된 프로시저를 실행 하려면 원시 SQL 명령을 실행 하려면 코드를 사용 하 여 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-113">We can execute use code to issue a raw SQL command to execute our stored procedure.</span></span> <span data-ttu-id="3c705-114">이 방법의 장점은 작동 한다는 것 모두 코드를 사용 하 여 먼저 및 EF 디자이너입니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-114">The advantage of this approach is that it works with both Code first and the EF Designer.</span></span>

<span data-ttu-id="3c705-115">여러 결과 얻기 위해 IObjectContextAdapter 인터페이스를 사용 하 여 ObjectContext API를 삭제 해야 하는 작업을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-115">In order to get multiple result sets working we need to drop to the ObjectContext API by using the IObjectContextAdapter interface.</span></span>

<span data-ttu-id="3c705-116">ObjectContext 있으면 저장된 프로시저의 결과를 추적 하 고 EF에서 정상적으로 사용할 수 있는 엔터티로 변환 하는 변환 메서드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-116">Once we have an ObjectContext then we can use the Translate method to translate the results of our stored procedure into entities that can be tracked and used in EF as normal.</span></span> <span data-ttu-id="3c705-117">다음 코드 샘플 작동에서 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-117">The following code sample demonstrates this in action.</span></span>

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

<span data-ttu-id="3c705-118">변환 메서드는 우리가 받은 절차, EntitySet 이름, 및는 MergeOption를 실행 하는 경우 판독기를 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-118">The Translate method accepts the reader that we received when we executed the procedure, an EntitySet name, and a MergeOption.</span></span> <span data-ttu-id="3c705-119">EntitySet 이름을 파생 컨텍스트에서 DbSet 속성으로 동일 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-119">The EntitySet name will be the same as the DbSet property on your derived context.</span></span> <span data-ttu-id="3c705-120">MergeOption 열거형 동일한 엔터티는 이미 메모리에 존재 하는 경우 결과 처리 하는 방법을 제어 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-120">The MergeOption enum controls how results are handled if the same entity already exists in memory.</span></span>

<span data-ttu-id="3c705-121">여기에서는 컬렉션을 반복 블로그 마치도록 하겠습니다 NextResult, 이것이 중요 한 위의 코드를 제공 하므로 첫 번째 결과 집합에는 다음 결과 집합으로 이동 하기 전에 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-121">Here we iterate through the collection of blogs before we call NextResult, this is important given the above code because the first result set must be consumed before moving to the next result set.</span></span>

<span data-ttu-id="3c705-122">두 변환 되 면 다른 엔터티는 블로그 및 게시물 엔터티를 EF에서 추적 됩니다 하 고 수정 또는 삭제 및 정상적으로 저장할 수 있도록 메서드 호출 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-122">Once the two translate methods are called then the Blog and Post entities are tracked by EF the same way as any other entity and so can be modified or deleted and saved as normal.</span></span>

>[!NOTE]
> <span data-ttu-id="3c705-123">EF는 변환 메서드를 사용 하 여 엔터티를 만들 때 계정에 모든 매핑을 사용 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-123">EF does not take any mapping into account when it creates entities using the Translate method.</span></span> <span data-ttu-id="3c705-124">결과 클래스에서 속성 이름의 집합의 열 이름을 일치 하기만 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-124">It will simply match column names in the result set with property names on your classes.</span></span>

>[!NOTE]
> <span data-ttu-id="3c705-125">블로그 엔터티 중 하나에 대 한 게시 속성에 액세스를 사용 하는 지연 로드 있는 EF 모든 지연 로드 하려면 데이터베이스에 연결 됩니다 게시를 모두 로드 이미 있는 경우에 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-125">That if you have lazy loading enabled, accessing the posts property on one of the blog entities then EF will connect to the database to lazily load all posts, even though we have already loaded them all.</span></span> <span data-ttu-id="3c705-126">즉, 모든 게시물 로드 여부 또는 데이터베이스에 더 많은 경우 EF 알 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-126">This is because EF cannot know whether or not you have loaded all posts or if there are more in the database.</span></span> <span data-ttu-id="3c705-127">이 문제를 방지 하려면 지연 로드를 사용 하지 않도록 설정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-127">If you want to avoid this then you will need to disable lazy loading.</span></span>

## <a name="multiple-result-sets-with-configured-in-edmx"></a><span data-ttu-id="3c705-128">구성 된 EDMX를 사용 하 여 여러 결과 집합</span><span class="sxs-lookup"><span data-stu-id="3c705-128">Multiple Result Sets with Configured in EDMX</span></span>

>[!NOTE]
> <span data-ttu-id="3c705-129">EDMX에 여러 결과 집합을 구성 하려면.NET Framework 4.5를 대상 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-129">You must target .NET Framework 4.5 to be able to configure multiple result sets in EDMX.</span></span> <span data-ttu-id="3c705-130">.NET 4.0을 대상으로 하는 경우 이전 섹션에 표시 된 코드 기반 메서드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-130">If you are targeting .NET 4.0 you can use the code-based method shown in the previous section.</span></span>

<span data-ttu-id="3c705-131">EF 디자이너를 사용 하는 경우 반환 되는 다양 한 결과 집합에 대 한 알 수 있도록 모델을도 수정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-131">If you are using the EF Designer, you can also modify your model so that it knows about the different result sets that will be returned.</span></span> <span data-ttu-id="3c705-132">포인터는 도구는 여러 결과 아닙니다 전에 알아두어야 할 한 가지 설정 인식 edmx 파일을 수동으로 편집 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-132">One thing to know before hand is that the tooling is not multiple result set aware, so you will need to manually edit the edmx file.</span></span> <span data-ttu-id="3c705-133">이 작동 비슷하지만 VS에서 모델의 유효성 검사를 나누기는 edmx 파일을 편집 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-133">Editing the edmx file like this will work but it will also break the validation of the model in VS.</span></span> <span data-ttu-id="3c705-134">모델 유효성을 검사 하는 경우 항상 오류를 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-134">So if you validate your model you will always get errors.</span></span>

-   <span data-ttu-id="3c705-135">이 작업을 수행 하기 위해 단일 결과 집합 쿼리 방식으로 모델에 저장된 프로시저를 추가 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-135">In order to do this you need to add the stored procedure to your model as you would for a single result set query.</span></span>
-   <span data-ttu-id="3c705-136">만든 후이 모델을 마우스 오른쪽 단추로 클릭 하 고 선택 해야 **연결 프로그램...**</span><span class="sxs-lookup"><span data-stu-id="3c705-136">Once you have this then you need to right click on your model and select **Open With..**</span></span> <span data-ttu-id="3c705-137">그런 다음 **Xml**</span><span class="sxs-lookup"><span data-stu-id="3c705-137">then **Xml**</span></span>

    ![열린](~/ef6/media/openas.png)

<span data-ttu-id="3c705-139">있다면 다음 단계를 수행 해야 하는 다음 XML로 열린 모델.</span><span class="sxs-lookup"><span data-stu-id="3c705-139">Once you have the model opened as XML then you need to do the following steps:</span></span>

-   <span data-ttu-id="3c705-140">모델에서 복합 형식 및 함수 가져오기를 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-140">Find the complex type and function import in your model:</span></span>

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

 

-   <span data-ttu-id="3c705-141">복합 형식 제거</span><span class="sxs-lookup"><span data-stu-id="3c705-141">Remove the complex type</span></span>
-   <span data-ttu-id="3c705-142">다음과 같이 표시 됩니다 여기서에서 엔터티에 매핑되는 function import를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-142">Update the function import so that it maps to your entities, in our case it will look like the following:</span></span>

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

<span data-ttu-id="3c705-143">이렇게 하면 모델 저장된 프로시저 두 컬렉션, 블로그 및 게시물 항목 중 하나를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-143">This tells the model that the stored procedure will return two collections, one of blog entries and one of post entries.</span></span>

-   <span data-ttu-id="3c705-144">함수 매핑 요소를 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-144">Find the function mapping element:</span></span>

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

-   <span data-ttu-id="3c705-145">다음과 같은 반환 되는 각 엔터티에 대해 하나를 사용 하 여 결과 매핑을 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-145">Replace the result mapping with one for each entity being returned, such as the following:</span></span>

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

<span data-ttu-id="3c705-146">결과 집합을 기본적으로 생성 하는 것과 같은 복합 형식에 매핑할 수 이기도 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-146">It is also possible to map the result sets to complex types, such as the one created by default.</span></span> <span data-ttu-id="3c705-147">이렇게 하려면, 제거 하는 대신 새 복합 형식을 만들고 복합 형식을 everywhere는 엔터티 이름과 위의 예제에서 사용한를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-147">To do this you create a new complex type, instead of removing them, and use the complex types everywhere that you had used the entity names in the examples above.</span></span>

<span data-ttu-id="3c705-148">이러한 매핑을 변경한 후 모델 저장 한 저장된 프로시저를 사용 하려면 다음 코드를 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-148">Once these mappings have been changed then you can save the model and execute the following code to use the stored procedure:</span></span>

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
> <span data-ttu-id="3c705-149">모델에 대 한 edmx 파일을 수동으로 편집 하는 경우 어느 데이터베이스에서 모델 다시 생성 하면 덮어씁니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-149">If you manually edit the edmx file for your model it will be overwritten if you ever regenerate the model from the database.</span></span>

## <a name="summary"></a><span data-ttu-id="3c705-150">요약</span><span class="sxs-lookup"><span data-stu-id="3c705-150">Summary</span></span>

<span data-ttu-id="3c705-151">다음 Entity Framework를 사용 하 여 설정 하는 여러 결과 액세스 하는 두 가지 방법을 설명 했습니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-151">Here we have shown two different methods of accessing multiple result sets using Entity Framework.</span></span> <span data-ttu-id="3c705-152">둘 다 상황에 따라 유효 하 고 기본 설정 하는 것 처럼 보이는 상황에 가장 적합 한 선택 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-152">Both of them are equally valid depending on your situation and preferences and you should choose the one that seems best for your circumstances.</span></span> <span data-ttu-id="3c705-153">여러 결과 집합 수에 대 한 지원 향상 이후 버전의 Entity Framework 및는이 문서의 단계를 수행 합니다. 더 이상 필요한 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="3c705-153">It is planned that the support for multiple result sets will be improved in future versions of Entity Framework and that performing the steps in this document will no longer be necessary.</span></span>
