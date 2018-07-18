---
title: 여러 결과 집합-EF6 사용 하 여 프로시저를 저장합니다.
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
caps.latest.revision: 3
ms.openlocfilehash: 68d544b0c553868ad1ff36cd24db19cff08db073
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122389"
---
# <a name="stored-procedures-with-multiple-result-sets"></a>여러 결과 집합을 사용 하 여 저장된 프로시저
사용 하 여 저장 하는 경우에 따라 둘 이상의 결과 반환 해야 하는 프로시저 설정 합니다. 이 시나리오는 데이터베이스의 수를 줄이는 데 일반적으로 단일 화면을 구성 하는 데 필요한 왕복 합니다. EF5, 이전 Entity Framework 저장된 프로시저를 호출할 수 있지만 첫 번째 결과 집합 호출 코드에만 반환 합니다.

이 문서에서는 Entity Framework에서 저장된 프로시저에서 둘 이상의 결과 집합에 액세스 하는 데 사용할 수 있는 두 가지 방법을 보여 줍니다. 단순히 코드를 사용 하 여 먼저 모두 코드와 함께 작동 하는 EF 디자이너 및 EF 디자이너 에서만 작동 하는 것입니다. 도구 및이 대 한 API 지원을 개선 해야 나중에 Entity Framework의 버전입니다.

## <a name="model"></a>모델

이 문서의 예제에서는 기본 블로그를 사용 하 고 단일 블로그 게시물 모델 블로그 게시물 및 다양 한 게시물에 속한 키를 누릅니다. 모든 블로그 및 게시물을 다음과 같이 결과 반환 하는 데이터베이스에서 저장된 프로시저를 사용 합니다.

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a>코드를 사용 하 여 설정 하는 여러 결과 액세스

이 저장된 프로시저를 실행 하려면 원시 SQL 명령을 실행 하려면 코드를 사용 하 여 실행할 수 있습니다. 이 방법의 장점은 작동 한다는 것 모두 코드를 사용 하 여 먼저 및 EF 디자이너입니다.

여러 결과 얻기 위해 IObjectContextAdapter 인터페이스를 사용 하 여 ObjectContext API를 삭제 해야 하는 작업을 설정 합니다.

ObjectContext 있으면 저장된 프로시저의 결과를 추적 하 고 EF에서 정상적으로 사용할 수 있는 엔터티로 변환 하는 변환 메서드를 사용할 수 있습니다. 다음 코드 샘플 작동에서 방법을 보여 줍니다.

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

변환 메서드는 우리가 받은 절차, EntitySet 이름, 및는 MergeOption를 실행 하는 경우 판독기를 허용 합니다. EntitySet 이름을 파생 컨텍스트에서 DbSet 속성으로 동일 합니다. MergeOption 열거형 동일한 엔터티는 이미 메모리에 존재 하는 경우 결과 처리 하는 방법을 제어 합니다.

여기에서는 컬렉션을 반복 블로그 마치도록 하겠습니다 NextResult, 이것이 중요 한 위의 코드를 제공 하므로 첫 번째 결과 집합에는 다음 결과 집합으로 이동 하기 전에 사용 해야 합니다.

두 변환 되 면 다른 엔터티는 블로그 및 게시물 엔터티를 EF에서 추적 됩니다 하 고 수정 또는 삭제 및 정상적으로 저장할 수 있도록 메서드 호출 됩니다.

>[!NOTE]
> EF는 변환 메서드를 사용 하 여 엔터티를 만들 때 계정에 모든 매핑을 사용 하지 않습니다. 결과 클래스에서 속성 이름의 집합의 열 이름을 일치 하기만 합니다.

>[!NOTE]
> 블로그 엔터티 중 하나에 대 한 게시 속성에 액세스를 사용 하는 지연 로드 있는 EF 모든 지연 로드 하려면 데이터베이스에 연결 됩니다 게시를 모두 로드 이미 있는 경우에 합니다. 즉, 모든 게시물 로드 여부 또는 데이터베이스에 더 많은 경우 EF 알 수 없습니다. 이 문제를 방지 하려면 지연 로드를 사용 하지 않도록 설정 해야 합니다.

## <a name="multiple-result-sets-with-configured-in-edmx"></a>구성 된 EDMX를 사용 하 여 여러 결과 집합

>[!NOTE]
> EDMX에 여러 결과 집합을 구성 하려면.NET Framework 4.5를 대상 해야 합니다. .NET 4.0을 대상으로 하는 경우 이전 섹션에 표시 된 코드 기반 메서드를 사용할 수 있습니다.

EF 디자이너를 사용 하는 경우 반환 되는 다양 한 결과 집합에 대 한 알 수 있도록 모델을도 수정할 수 있습니다. 포인터는 도구는 여러 결과 아닙니다 전에 알아두어야 할 한 가지 설정 인식 edmx 파일을 수동으로 편집 해야 합니다. 이 작동 비슷하지만 VS에서 모델의 유효성 검사를 나누기는 edmx 파일을 편집 합니다. 모델 유효성을 검사 하는 경우 항상 오류를 발생할 수 있습니다.

-   이 작업을 수행 하기 위해 단일 결과 집합 쿼리 방식으로 모델에 저장된 프로시저를 추가 해야 합니다.
-   만든 후이 모델을 마우스 오른쪽 단추로 클릭 하 고 선택 해야 **연결 프로그램...** 그런 다음 **Xml**

    ![OpenAs](~/ef6/media/openas.png)

있다면 다음 단계를 수행 해야 하는 다음 XML로 열린 모델.

-   모델에서 복합 형식 및 함수 가져오기를 찾습니다.

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

 

-   복합 형식 제거
-   다음과 같이 표시 됩니다 여기서에서 엔터티에 매핑되는 function import를 업데이트 합니다.

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

이렇게 하면 모델 저장된 프로시저 두 컬렉션, 블로그 및 게시물 항목 중 하나를 반환 합니다.

-   함수 매핑 요소를 찾습니다.

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

-   다음과 같은 반환 되는 각 엔터티에 대해 하나를 사용 하 여 결과 매핑을 바꿉니다.

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

결과 집합을 기본적으로 생성 하는 것과 같은 복합 형식에 매핑할 수 이기도 합니다. 이렇게 하려면, 제거 하는 대신 새 복합 형식을 만들고 복합 형식을 everywhere는 엔터티 이름과 위의 예제에서 사용한를 사용 합니다.

이러한 매핑을 변경한 후 모델 저장 한 저장된 프로시저를 사용 하려면 다음 코드를 실행할 수 있습니다.

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
> 모델에 대 한 edmx 파일을 수동으로 편집 하는 경우 어느 데이터베이스에서 모델 다시 생성 하면 덮어씁니다.

## <a name="summary"></a>요약

다음 Entity Framework를 사용 하 여 설정 하는 여러 결과 액세스 하는 두 가지 방법을 설명 했습니다. 둘 다 상황에 따라 유효 하 고 기본 설정 하는 것 처럼 보이는 상황에 가장 적합 한 선택 해야 합니다. 여러 결과 집합 수에 대 한 지원 향상 이후 버전의 Entity Framework 및는이 문서의 단계를 수행 합니다. 더 이상 필요한 예정입니다.
