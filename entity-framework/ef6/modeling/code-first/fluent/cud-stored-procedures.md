---
title: 저장 프로시저 삽입, 업데이트 및 삭제 Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
ms.openlocfilehash: bfc56671814aec1965ac054ff901297e5cdbbecb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415774"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a>저장 프로시저 삽입, 업데이트 및 삭제 Code First
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

기본적으로 Code First는 직접 테이블 액세스를 사용 하 여 삽입, 업데이트 및 삭제 명령을 수행 하도록 모든 엔터티를 구성 합니다. EF6부터 모델의 일부 또는 모든 엔터티에 대해 저장 프로시저를 사용 하도록 Code First 모델을 구성할 수 있습니다.  

## <a name="basic-entity-mapping"></a>기본 엔터티 매핑  

흐름 API를 사용 하 여 삽입, 업데이트 및 삭제를 위해 저장 프로시저를 사용 하도록 선택할 수 있습니다.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

이렇게 하면 Code First는 일부 규칙을 사용 하 여 데이터베이스에 저장 프로시저의 예상 되는 모양을 작성 합니다.  

- **\<type_name\>_Insert**\<type_name\> **_Update\<type_name** **\>_Delete** Blog_Insert) 라는 세 개의 저장 프로시저  
- 매개 변수 이름은 속성 이름에 해당 합니다.  
  > [!NOTE]
  > HasColumnName () 또는 Column 특성을 사용 하 여 지정 된 속성의 열 이름을 바꾸면이 이름이 속성 이름 대신 매개 변수에 사용 됩니다.  
- **Insert 저장 프로시저는** 생성 된 저장소 (id 또는 계산 됨)로 표시 된 속성을 제외 하 고 모든 속성에 대 한 매개 변수를 포함 합니다. 저장 프로시저는 생성 된 각 저장소 속성에 대 한 열이 포함 된 결과 집합을 반환 해야 합니다.  
- **업데이트 저장 프로시저** 에는 저장소 생성 패턴 ' 계산 됨 '으로 표시 된 속성을 제외 하 고 모든 속성에 대 한 매개 변수가 있습니다. 일부 동시성 토큰에는 원래 값에 대 한 매개 변수가 필요 합니다. 자세한 내용은 아래의 *동시성 토큰* 섹션을 참조 하십시오. 저장 프로시저는 계산 된 각 속성에 대 한 열이 있는 결과 집합을 반환 해야 합니다.  
- **Delete 저장 프로시저는** 엔터티의 키 값에 대 한 매개 변수를 포함 해야 합니다 (또는 엔터티에 복합 키가 있는 경우 여러 매개 변수). 또한 delete 프로시저에는 대상 테이블의 모든 독립 연결 외래 키에 대 한 매개 변수 (엔터티에 선언 된 해당 외래 키 속성이 없는 관계)가 있어야 합니다. 일부 동시성 토큰에는 원래 값에 대 한 매개 변수가 필요 합니다. 자세한 내용은 아래의 *동시성 토큰* 섹션을 참조 하십시오.  

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

기본적으로 구성 된 항목의 일부 또는 전부를 재정의할 수 있습니다.  

하나 이상의 저장 프로시저의 이름을 변경할 수 있습니다. 이 예에서는 업데이트 저장 프로시저만의 이름을 바꿉니다.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

이 예에서는 세 개의 저장 프로시저 모두의 이름을 바꿉니다.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

이러한 예제에서 호출은 함께 연결 되지만 람다 블록 구문을 사용할 수도 있습니다.  

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

이 예에서는 update 저장 프로시저의 BlogId 속성에 대 한 매개 변수 이름을 바꿉니다.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

이러한 호출은 모두 연결 가능한 및 구성할 수 있습니다. 다음은 세 개의 저장 프로시저와 해당 매개 변수의 이름을 바꾸는 예입니다.  

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

데이터베이스에서 생성 된 값을 포함 하는 결과 집합에서 열 이름을 변경할 수도 있습니다.  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a>클래스에 외래 키가 없는 관계 (독립적인 연결)  

외래 키 속성이 클래스 정의에 포함 된 경우 다른 속성과 동일한 방식으로 해당 매개 변수의 이름을 바꿀 수 있습니다. 클래스에서 외래 키 속성을 사용 하지 않고 관계가 있는 경우 기본 매개 변수 이름은 **\<navigation_property_name\>_\<primary_key_name\>** 합니다.  

예를 들어 다음 클래스 정의를 통해 저장 프로시저에서 게시를 삽입 하 고 업데이트 하는 데 Blog_BlogId 매개 변수가 필요 합니다.  

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

매개 변수 메서드에 기본 키 속성의 경로를 제공 하 여 클래스에 포함 되지 않은 외래 키에 대 한 매개 변수를 변경할 수 있습니다.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

종속 엔터티에 대 한 탐색 속성이 없는 경우 (즉, Post. Blog 속성을 지정 하지 않으면 Association 메서드를 사용 하 여 관계의 다른 end를 식별 한 다음 각 키 속성에 해당 하는 매개 변수를 구성할 수 있습니다.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a>동시성 토큰  

업데이트 및 삭제 저장 프로시저는 동시성을 처리 해야 할 수도 있습니다.  

- 엔터티에 동시성 토큰이 포함 된 경우 저장 프로시저는 업데이트/삭제 된 행 수 (영향을 받는 행)를 반환 하는 출력 매개 변수를 선택적으로 포함할 수 있습니다. 이러한 매개 변수는 RowsAffectedParameter 메서드를 사용 하 여 구성 해야 합니다.  
기본적으로 EF는 ExecuteNonQuery의 반환 값을 사용 하 여 영향을 받는 행 수를 확인 합니다. 행이 영향을 받는 출력 매개 변수를 지정 하는 것은 실행이 끝날 때 ExecuteNonQuery의 반환 값이 잘못 된 (EF 관점에서) 반환 값이 잘못 되는 경우에 유용 합니다.  
- 각 동시성 토큰에는 **\<property_name\>_Original** (예: Timestamp_Original) 라는 매개 변수가 있습니다. 이 속성의 원래 값이 전달 됩니다 .이 값은 데이터베이스에서 쿼리할 때 값입니다.  
    - 데이터베이스에 의해 계산 되는 동시성 토큰 (예: 타임 스탬프)에는 원래 값 매개 변수만 있습니다.  
    - 동시성 토큰으로 설정 된 계산 되지 않은 속성에도 업데이트 프로시저의 새 값에 대 한 매개 변수가 포함 됩니다. 이는 이미 새 값에 대해 설명한 명명 규칙을 사용 합니다. 이러한 토큰의 예로는 블로그의 URL을 동시성 토큰으로 사용 하는 경우 새 값이 필요 합니다 .이는 데이터베이스에 의해서만 업데이트 되는 타임 스탬프 토큰과 달리 코드에서 새 값으로 업데이트 될 수 있기 때문입니다.  

이는 타임 스탬프 동시성 토큰을 사용 하는 예제 클래스 및 업데이트 저장 프로시저입니다.  

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

다음은 계산 되지 않은 동시성 토큰이 포함 된 예제 클래스 및 업데이트 저장 프로시저입니다.  

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

필요에 따라 행에 영향을 주는 매개 변수를 도입할 수 있습니다.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

데이터베이스에서 계산 된 동시성 토큰의 경우 원래 값만 전달 됩니다. 표준 매개 변수 이름 바꾸기 메커니즘을 사용 하 여 원래 값에 대 한 매개 변수의 이름을 바꿀 수 있습니다.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

원래 값과 새 값이 모두 전달 되는 계산 되지 않은 동시성 토큰의 경우 각 매개 변수의 이름을 제공할 수 있게 해 주는 매개 변수의 오버 로드를 사용할 수 있습니다.  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a>다대다 관계  

이 섹션에서는 다음 클래스를 예로 사용 합니다.  

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

다음 구문을 사용 하 여 다 대 다 관계를 저장 프로시저에 매핑할 수 있습니다.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

다른 구성을 제공 하지 않으면 기본적으로 다음 저장 프로시저 셰이프가 사용 됩니다.  

- 두 개의 저장 프로시저 **\<type_one\>\<type_two**\>_Insert\<**type_one\>\<** type_two\>_Delete PostTag_Insert).  
- 매개 변수는 각 형식에 대 한 키 값이 됩니다. \<되는 각 매개 변수의 이름 **type_name\>_\<property_name**\>(예: Post_PostId 및 Tag_TagId).

다음은 insert 및 update 저장 프로시저의 예입니다.  

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

프로시저 및 매개 변수 이름은 엔터티 저장 프로시저와 유사한 방식으로 구성할 수 있습니다.  

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
