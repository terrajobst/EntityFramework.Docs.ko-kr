---
title: 코드의 첫 번째 삽입, 업데이트 및 Delete 저장된 프로시저-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
ms.openlocfilehash: a0448fb44dabb2e03b2358aa7b4f69d92cffa15a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994575"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a>코드의 첫 번째 삽입, 업데이트 및 Delete 저장된 프로시저
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

기본적으로 Code First 구성 모든 엔터티를 삽입, 업데이트 및 직접 테이블 액세스를 사용 하 여 명령을 삭제 합니다. Ef6에서부터 모델의 일부 또는 모든 엔터티에 대 한 저장된 프로시저를 사용 하 여 Code First 모델을 구성할 수 있습니다.  

## <a name="basic-entity-mapping"></a>기본 엔터티 매핑  

삽입 저장된 프로시저를 사용 하 여 옵트인 하 고, 업데이트 하 고, Fluent API를 사용 하 여 삭제할 수 있습니다.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

이 작업을 수행 하는 것은 일부 규칙 데이터베이스에서 저장된 프로시저의 예상된 셰이프를 작성 하는 데 Code First 발생 합니다.  

- 3 개의 저장 프로시저  **\<type_name\>삽입 (_i)** 합니다  **\<type_name\>업데이트 (_u)** 하 고  **\<형식 이름을\>삭제 (_d)** (예: Blog_Insert, Blog_Update 및 Blog_Delete).  
- 매개 변수 이름은 속성 이름에 해당합니다.  
  > [!NOTE]
  > 지정된 된 속성에 대 한 열 이름을 바꾸려면 HasColumnName() 또는 열 특성을 사용 하는 경우이 이름은 속성 이름 대신 매개 변수에 대해 사용 됩니다.  
- **Insert 저장 프로시저** 생성 된 저장소로 표시 된 항목을 제외한 모든 속성에 대 한 매개 변수를 포함 합니다 (identity 또는 계산). 저장된 프로시저에는 각 생성 된 저장소 속성에 대 한 열에 있는 결과 집합을 반환 해야 합니다.  
- **업데이트 저장 프로시저** 'Computed' 저장소 생성 패턴을 사용 하 여 표시 된 항목을 제외한 모든 속성에 대 한 매개 변수를 갖습니다. 일부 동시성 토큰 원래 값에 대 한 매개 변수가 필요한, 참조를 *동시성 토큰* 대 한 자세한 내용은 아래 섹션입니다. 저장된 프로시저에는 각 계산 된 속성에 대 한 열에 있는 결과 집합을 반환 해야 합니다.  
- **Delete 저장 프로시저** 엔터티 (또는 여러 매개 변수 엔터티에 복합 키가 있는 경우)의 키 값에 대 한 매개 변수를 포함 해야 합니다. 또한 delete 프로시저도 있어야 모든 독립 연결 외래 키에 대 한 매개 변수에서 대상 테이블 (해당 외래 키 속성이 엔터티에 선언 되지 않은 관계) 합니다. 일부 동시성 토큰 원래 값에 대 한 매개 변수가 필요한, 참조를 *동시성 토큰* 대 한 자세한 내용은 아래 섹션입니다.  

예를 들어 다음 클래스를 사용 합니다.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

기본 저장 프로시저는 다음과 같습니다.  

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS BlogId
END
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId;
CREATE PROCEDURE [dbo].[Blog_Delete]  
  @BlogId int  
AS  
  DELETE FROM [dbo].[Blogs]
  WHERE BlogId = @BlogId
```  

### <a name="overriding-the-defaults"></a>기본값 재정의  

기본적으로 구성 된 것의 일부 또는 전체를 재정의할 수 있습니다.  

하나 이상의 저장된 프로시저의 이름을 변경할 수 있습니다. 이 예제에만 업데이트 저장 프로시저를 이름을 바꿉니다.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

이 예제는 3 개의 저장된 프로시저를 모두의 이름을 바꿉니다.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

이 예제에서 호출 하 여 모두 연결 되어, 있지만 람다 블록 구문을 사용할 수도 있습니다.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    {  
      s.Update(u => u.HasName("modify_blog"));  
      s.Delete(d => d.HasName("delete_blog"));  
      s.Insert(i => i.HasName("insert_blog"));  
    });
```  

이 예에서는 update 저장 프로시저에 BlogId 속성에 대 한 매개 변수를 이름을 바꿉니다.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

이러한 호출은 모두 포함 된 연결 가능한 되 고 구성 가능 합니다. 모든 3 개의 저장된 프로시저 및 매개 변수 이름을 변경 하는 예는 다음과 같습니다.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")  
                   .Parameter(b => b.BlogId, "blog_id")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url"))  
     .Delete(d => d.HasName("delete_blog")  
                   .Parameter(b => b.BlogId, "blog_id"))  
     .Insert(i => i.HasName("insert_blog")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url")));
```  

또한 데이터베이스에서 생성 된 값이 포함 된 결과 집합의 열 이름을 변경할 수 있습니다.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures(s =>
    s.Insert(i => i.Result(b => b.BlogId, "generated_blog_identity")));
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS generated_blog_id
END
```  

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a>클래스 (독립 연결)에서 외래 키가 없는 관계  

외래 키 속성은 클래스 정의에 포함 되어, 다른 속성으로 동일한 방식으로 해당 매개 변수를 바꿀 수 있습니다. 클래스에서 외래 키 속성이 없는 관계가 존재 하는 경우 기본 매개 변수 이름은  **\<navigation_property_name\>_\<primary_key_name\>** 합니다.  

예를 들어, 다음 클래스 정의 저장된 프로시저를 삽입 및 업데이트 게시물에서 예상 되 고 Blog_BlogId 매개 변수가 유발 합니다.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }

  public List<Post> Posts { get; set; }  
}  

public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public Blog Blog { get; set; }  
}
```  

### <a name="overriding-the-defaults"></a>기본값 재정의  

매개 변수는 기본 키 속성에 경로 제공 하 여 클래스에 포함 되지 않은 외래 키에 대 한 매개 변수를 변경할 수 있습니다.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

종속 엔터티 (즉, 탐색 속성이 없는 경우 Post.Blog 속성 없음) 연결 메서드를 사용 하 여 관계의 다른 끝을 식별 하 고 각 키 속성에 해당 하는 매개 변수를 구성 합니다.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a>동시성 토큰  

Update 및 delete 저장 프로시저 동시 성과 함께 처리 해야 할 수도 있습니다.  

- 엔터티의 동시성 토큰에 있으면 저장된 프로시저 (영향을 받는 행) 업데이트/삭제 된 행 수를 반환 하는 출력 매개 변수를 필요에 따라 있을 수 있습니다. RowsAffectedParameter 메서드를 사용 하 여 이러한 매개 변수를 구성 되어야 합니다.  
기본적으로 EF는 영향을 받은 행 수를 확인 하려면 ExecuteNonQuery의 반환 값을 사용 합니다. 영향을 받는 행 출력 매개 변수는 ExecuteNonQuery (EF의 관점에서) 예상률을의 반환 값을 초래 하 여 저장 프로시저의 논리를 수행 하는 경우에 유용 지정 실행의 끝입니다.  
- 각 동시성 토큰에 있는 명명 된 매개 변수 됩니다  **\<property_name\>_Original** (예를 들어 Timestamp_Original). 이 전달할 값을 데이터베이스에서 쿼리할 때 –이 속성의 원래 값입니다.  
    - 타임 스탬프 – 등은 데이터베이스에 의해 계산 되는 동시성 토큰은 원래 값 매개 변수를 하나만 됩니다.  
    - 동시성 토큰으로 설정 되어 있는 비-계산 된 속성 업데이트 프로시저에서 새 값에 대 한 매개 변수를 갖게 됩니다. 이 새 값에 대해 앞에서 설명한 명명 규칙을 사용 합니다. 이러한 토큰의 예가 사용 될 수 블로그 URL 동시성 토큰으로,이 코드 (달리만 데이터베이스에서 업데이트 되는 타임 스탬프 토큰) 하 여 새 값으로 업데이트할 수 있으므로 새 값이 필요 합니다.  

이 예제는 클래스 및 타임 스탬프 동시성 토큰을 사용 하 여 저장된 프로시저를 업데이트 합니다.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
  [Timestamp]
  public byte[] Timestamp { get; set; }
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Timestamp_Original rowversion  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Timestamp] = @Timestamp_Original
```  

다음은 예제 클래스 및 동시성 계산 되지 않은 토큰을 사용 하 여 저장된 프로시저를 업데이트 합니다.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  [ConcurrencyCheck]
  public string Url { get; set; }  
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Url_Original nvarchar(max),
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Url] = @Url_Original
```  

### <a name="overriding-the-defaults"></a>기본값 재정의  

필요에 따라 영향을 받는 행 매개 변수를 도입할 수 있습니다.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

– 원래 값만 전달 된 – 계산 데이터베이스 동시성 토큰에 대 한 원래 값에 대 한 매개 변수 이름을 바꾸려면 메커니즘 이름 바꾸기 표준 매개 변수를 바로 사용할 수 있습니다.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

– 모두 원래 및 새 값의 전달 방식 – 계산 되지 않은 동시성 토큰에 대 한 각 매개 변수에 대 한 이름을 제공할 수 있도록 매개 변수 오버 로드를 사용할 수 있습니다.  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a>다대다 관계  

이 섹션의 예를 들어 다음 클래스를 사용 하겠습니다.  

``` csharp
public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public List<Tag> Tags { get; set; }  
}  

public class Tag  
{  
  public int TagId { get; set; }  
  public string TagName { get; set; }  

  public List<Post> Posts { get; set; }  
}
```  

다대다 관계 대부분 다음 구문 사용 하 여 저장된 프로시저에 매핑할 수 있습니다.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

다른 구성이 제공 되지 않은 경우에 다음 저장된 프로시저 모양은 기본적으로 사용 됩니다.  

- 두 개의 저장 프로시저  **\<type_one\>\<type_two\>삽입 (_i)** 고  **\<type_one\>\<type_two \>삭제 (_d)** (예: PostTag_Insert 및 PostTag_Delete).  
- 매개 변수는 각 형식에 대 한 키 값 됩니다. 각 매개 변수 중의 이름을 **\<type_name\>_\<property_name\>** (예: Post_PostId 및 Tag_TagId).

다음 예제에서는 삽입 및 저장된 프로시저를 업데이트 합니다.  

``` SQL
CREATE PROCEDURE [dbo].[PostTag_Insert]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  INSERT INTO [dbo].[Post_Tags] (Post_PostId, Tag_TagId)   
  VALUES (@Post_PostId, @Tag_TagId)
CREATE PROCEDURE [dbo].[PostTag_Delete]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  DELETE FROM [dbo].[Post_Tags]    
  WHERE Post_PostId = @Post_PostId AND Tag_TagId = @Tag_TagId
```  

### <a name="overriding-the-defaults"></a>기본값 재정의  

비슷한 방식으로 엔터티를 저장 프로시저에 프로시저 이름과 매개 변수를 구성할 수 있습니다.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.HasName("add_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id"))  
     .Delete(d => d.HasName("remove_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id")));
```  
