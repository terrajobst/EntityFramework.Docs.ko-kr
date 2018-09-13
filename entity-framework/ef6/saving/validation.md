---
title: EF6 유효성 검사
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 3aeb33763819544618c4a3068bb278c9b23409b6
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490647"
---
# <a name="data-validation"></a>데이터 유효성 검사
> [!NOTE]
> **EF4.1 이상만** -Api 기능 등이이 페이지에 설명 된 Entity Framework 4.1에서 도입 되었습니다. 일부 또는 모든 정보는 이전 버전을 사용 하는 경우 적용 되지 않습니다.

이 페이지에 있는 콘텐츠는 및 Julie lerman 작성 원래 작성 된 문서 ([http://thedatafarm.com](http://thedatafarm.com)).

Entity Framework는 다양 한 클라이언트 쪽 유효성 검사에 대 한 사용자 인터페이스를 통해 공급할 수 또는 서버 쪽 유효성 검사에 사용할 수 있는 유효성 검사 기능을 제공 합니다. 코드를 처음 사용 하는 경우 주석 또는 fluent API 구성이 사용 하 여 유효성 검사를 지정할 수 있습니다. 추가 유효성 검사 및 복잡 한 코드에서 지정 될 수 있으며 모델 코드에서 먼저 hails 인지 관계 없이 작동 하는 모델을 우선 또는 먼저 데이터베이스입니다.

## <a name="the-model"></a>모델

클래스의 간단한 쌍을 사용 하 여 유효성 검사를 설명 하겠습니다: 블로그 및 게시물.

``` csharp
    public class Blog
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public string BloggerName { get; set; }
          public DateTime DateCreated { get; set; }
          public virtual ICollection<Post> Posts { get; set; }
          }
      }

      public class Post
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public DateTime DateCreated { get; set; }
          public string Content { get; set; }
          public int BlogId { get; set; }
          public ICollection<Comment> Comments { get; set; }
      }
```
## <a name="data-annotations"></a>데이터 주석

코드는 먼저 코드 첫 번째 클래스를 구성 하는 하나의 수단으로 주석을 System.ComponentModel.DataAnnotations 어셈블리를 사용 합니다. 이러한 주석은 간에 필요 MinLength 및 MaxLength와 같은 규칙을 제공 하는입니다. .NET 클라이언트 응용 프로그램의 수는 또한 이러한 주석, 예를 들어, ASP.NET MVC를 인식합니다. 두 클라이언트 쪽 및 서버 쪽 유효성 검사 이러한 주석 사용 하 여 얻을 수 있습니다. 예를 들어 블로그 Title 속성 필수 속성을 적용할 수 있습니다.

``` csharp
    [Required]
    public string Title { get; set; }
```

추가 코드 또는 응용 프로그램에서 태그 변경 없이 기존 MVC 응용 프로그램에는 클라이언트 쪽 유효성 검사, 속성 및 주석 이름을 사용 하 여 메시지를 동적으로 작성을 수행 합니다.

![그림 1](~/ef6/media/figure01.png)

게시물에 Create view이 메서드의 백업할 Entity Framework는 새 블로그가 데이터베이스에 저장 하는 데 사용 되는 있지만 MVC의 클라이언트 쪽 유효성 검사는 응용 프로그램 코드에 도달 하기 전에 트리거됩니다.

그러나 클라이언트 쪽 유효성 검사 완벽 않습니다. 사용자가 브라우저의 기능에 영향을 줄 수 있습니다 하거나 더 심한 아직 해커가 수 사용 일부 책략 UI 유효성 검사를 하지 않으려면 합니다. 하지만 Entity Framework 또한 필요한 주석이 인식 하 고 유효성을 검사 합니다.

이 테스트 하는 간단한 방법은 MVC의 클라이언트 쪽 유효성 검사 기능을 사용 하지 않도록 설정 됩니다. MVC 응용 프로그램의 web.config 파일에서이 수행할 수 있습니다. AppSettings 섹션에 ClientValidationEnabled 키 이 키를 false로 설정 UI에서 유효성 검사를 수행 하지 것입니다.

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

클라이언트 쪽 유효성 검사를 사용 하지 않도록 설정도 사용 하 여 응용 프로그램에서 동일한 응답을 받을 수 있습니다. 오류 메시지 "제목 필드는 필수"로 표시 됩니다. 이제 제외 하 고 서버 쪽 유효성 검사의 결과가 됩니다. Entity Framework는 유효성 검사를 수행 필요 주석을 (빌드 및 INSERT 명령 데이터베이스를 전송에 분명) 전에 메시지를 표시 하는 MVC로 오류를 반환 합니다.

## <a name="fluent-api"></a>Fluent API

코드를 사용 하면 첫 번째의 fluent API 주석 대신를 동일한 클라이언트 쪽 및 서버 쪽 유효성 검사 합니다. 대신 필요한 사용 하겠습니다 MaxLength 유효성 검사를 사용 하 여이 합니다.

코드를 클래스에서 모델을 먼저 만들고 대로 Fluent API 구성이 적용 됩니다. DbContext 클래스의 OnModelCreating 메서드를 재정의 하 여 구성을 삽입할 수 있습니다. 다음은 BloggerName 속성은 10 자 보다 길지 수를 지정 하는 구성입니다.

``` csharp
    public class BlogContext : DbContext
      {
          public DbSet<Blog> Blogs { get; set; }
          public DbSet<Post> Posts { get; set; }
          public DbSet<Comment> Comments { get; set; }

          protected override void OnModelCreating(DbModelBuilder modelBuilder)
          {
              modelBuilder.Entity<Blog>().Property(p => p.BloggerName).HasMaxLength(10);
          }
        }
```

발생 한 유효성 검사 오류 Fluent API 구성에 따라는 자동으로 도달률 UI 있지만 수 캡처하지 않습니다 것 코드를 다음 응답에 적절 하 게 합니다.

여기의 예외 처리 오류 코드는 Entity Framework를 사용 하 여 최대 10 자 초과 BloggerName 블로그를 저장 하려고 할 때 해당 유효성 검사 오류를 캡처하는 응용 프로그램의 BlogController 클래스에 일부입니다.

``` csharp
    [HttpPost]
    public ActionResult Edit(int id, Blog blog)
    {
        try
        {
            db.Entry(blog).State = EntityState.Modified;
            db.SaveChanges();
            return RedirectToAction("Index");
        }
        catch(DbEntityValidationException ex)
        {
            var error = ex.EntityValidationErrors.First().ValidationErrors.First();
            this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
            return View();
        }
    }
```

유효성 검사 ModelState.AddModelError를 사용 하는 추가 코드를 사용 하는 이유는 보기로 다시 자동으로 전달 하지 않습니다. 오류 세부 정보를 사용 하 여 ValidationMessageFor Htmlhelper 오류를 표시 하도록 뷰 하 하 게 하는 것이 이렇게 합니다.

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a>IValidatableObject

IValidatableObject에 System.ComponentModel.DataAnnotations 존재 하는 인터페이스입니다. 아니지만 Entity Framework API의 일부를 계속 활용 하 여 서버 쪽 유효성 검사에 대 한 Entity Framework 클래스에 있습니다. IValidatableObject은 SaveChanges 때나 Entity Framework를 호출 하는 Validate 메서드를 호출할 수 직접 클래스의 유효성을 검사 하려는 언제 든 지를 제공 합니다.

와 같은 필요한 구성 및 MaxLength 하지 단일 필드에서 수행 합니다. Validate 메서드를 두 개의 필드를 비교 합니다. 예를 들어, 훨씬 더 복잡 한 논리를 포함할 수 있습니다.

다음 예에서 블로그 클래스 IValidatableObject를 구현 하 고 제목 및 BloggerName 일치할 수 없습니다. 규칙을 제공 하도록 확장 되었습니다.

``` csharp
    public class Blog : IValidatableObject
     {
         public int Id { get; set; }
         [Required]
         public string Title { get; set; }
         public string BloggerName { get; set; }
         public DateTime DateCreated { get; set; }
         public virtual ICollection<Post> Posts { get; set; }

         public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
         {
             if (Title == BloggerName)
             {
                 yield return new ValidationResult
                  ("Blog Title cannot match Blogger Name", new[] { "Title", “BloggerName” });
             }
         }
     }
```

ValidationResult 생성자 오류 메시지와 유효성 검사와 연관 된 멤버 이름을 나타내는 문자열 배열을 나타내는 문자열입니다. 이 유효성 검사 제목과 BloggerName 모두 확인 되므로 속성 이름은 모두 반환 됩니다.

Fluent API에서 제공 되는 유효성 검사와는 달리이 유효성 검사 결과 보기에서 인식 됩니다 및 ModelState에 오류를 추가 하려면 이전 사용 하는 예외 처리기 필요 하지 않습니다. ValidationResult의 두 속성 이름이 설정, 때문에 MVC HtmlHelpers 해당 속성을 모두에 대 한 오류 메시지를 표시 합니다.

![그림 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a>DbContext.ValidateEntity

DbContext은 ValidateEntity 호출 재정의 가능한 메서드를 포함 합니다. SaveChanges를 호출 하면 Entity Framework 상태가 변경 되지 않은 아닌 해당 캐시의 각 엔터티에 대해이 메서드를 호출 합니다. 넣으면 유효성 검사 논리 사용 하 여 또는 여기에서 직접 예를 들어, 이전 섹션에서 추가한 Blog.Validate 메서드를 호출 하려면이 메서드.

게시물 제목을 이미 사용 되지 않은 확인에 대 한 새 게시의 유효성을 검사 하는 ValidateEntity 재정의의 예는 다음과 같습니다. 먼저 확인 하는 경우 엔터티 게시물 상태로 추가 되어 있는지 확인 합니다. 하는 경우 다음 검색 데이터베이스를 같은 제목의 게시물 이미 있는지 확인 합니다. 기존 게시물 이미 있으면 새 DbEntityValidationResult 만들어집니다.

DbEntityValidationResult를 DbEntityEntry 및는 ICollection의 DbValidationErrors 단일 엔터티에 대 한 보관 합니다. 이 메서드의 처음에 DbEntityValidationResult 인스턴스화되고 검색 된 모든 오류 ValidationErrors 컬렉션에 추가 됩니다.

``` csharp
    protected override DbEntityValidationResult ValidateEntity (
        System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
        IDictionary\<object, object> items)
    {
        var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());
        if (entityEntry.Entity is Post && entityEntry.State == EntityState.Added)
        {
            Post post = entityEntry.Entity as Post;
            //check for uniqueness of post title
            if (Posts.Where(p => p.Title == post.Title).Count() > 0)
            {
                result.ValidationErrors.Add(
                        new System.Data.Entity.Validation.DbValidationError("Title",
                        "Post title must be unique."));
            }
        }

        if (result.ValidationErrors.Count > 0)
        {
            return result;
        }
        else
        {
         return base.ValidateEntity(entityEntry, items);
        }
    }
```

## <a name="explicitly-triggering-validation"></a>명시적으로 유효성 검사를 트리거하지

SaveChanges 호출 모두이 문서에서 설명 하는 유효성 검사를 트리거합니다. 하지만 SaveChanges에 의존할 필요가 없습니다. 응용 프로그램의 다른 곳에서 유효성을 검사할 수도 있습니다.

DbContext.GetValidationErrors 모든 유효성 검사, 주석 또는 Fluent API에 의해 정의 된, IValidatableObject (예를 들어 Blog.Validate)에서 만든 유효성 검사 및는 DbContext.ValidateEntity에서 수행 하는 유효성 검사를 트리거 메서드입니다.

다음 코드는 DbContext의 현재 인스턴스에서 GetValidationErrors를 호출 합니다. ValidationErrors는 DbValidationRestuls에 엔터티 형식별으로 그룹화 됩니다. 확장 메서드에서 반환한 DbValidationResults 한 다음 각 ValidationError 내에서 코드를 먼저 반복 합니다.

``` csharp
    foreach (var validationResults in db.GetValidationErrors())
        {
            foreach (var error in validationResults.ValidationErrors)
            {
                Debug.WriteLine(
                                  "Entity Property: {0}, Error {1}",
                                  error.PropertyName,
                                  error.ErrorMessage);
            }
        }
```

## <a name="other-considerations-when-using-validation"></a>유효성 검사를 사용 하는 경우 기타 고려 사항

다른 몇 가지 Entity Framework 유효성 검사를 사용 하는 경우 고려할 사항은 다음과 같습니다.

-   유효성 검사 중 지연 로드가 비활성화 되었습니다.
-   EF는 비 매핑된 속성 (데이터베이스의 열에 매핑되지 않은 속성)에 대 한 데이터 주석 유효성을 검사 합니다.
-   SaveChanges 중 변경이 감지 되 면 유효성 검사가 수행 됩니다. 유효성 검사 중 변경한 경우 변경 내용 추적기에 알리기 위해 사용자의 책임입니다.
-   DbUnexpectedValidationException은 유효성 검사 중 오류가 발생 한 경우 throw 됩니다.
-   Entity Framework 모델 (필요한 최대 길이 등)에 포함 된 패싯 데이터 주석을 클래스에 없는 및/또는 EF 디자이너를 사용 하 여 모델을 만들 경우에 유효성 검사를 발생 합니다.
-   우선 순위 규칙:
    -   Fluent API 호출에는 해당 데이터 주석을 재정의합니다
-   실행 순서:
    -   속성의 유효성 검사 유형 유효성 검사 전에 발생
    -   유형 유효성 검사 속성 유효성 검사에 성공 하는 경우에 발생
-   속성은 복잡 한 경우 해당 유효성 검사도 포함 됩니다.
    -   복합 형식 속성에 속성 수준의 유효성 검사
    -   복합 형식에 대해 IValidatableObject 유효성 검사를 포함 하 여 복잡 한 형식에서 형식 수준 유효성 검사

## <a name="summary"></a>요약

Entity Framework에서 유효성 검사 API MVC의 클라이언트 쪽 유효성 검사를 사용 하 여 매끄럽게 수행 하지만 클라이언트 쪽 유효성 검사에 의존할 필요가 없습니다. Entity Framework DataAnnotations 또는 code first Fluent API를 사용 하 여 적용 한 구성에 대 한 서버 쪽에서 유효성 검사 처리 됩니다.

수의 IValidatableObject 인터페이스를 사용 하 든 DbContet.ValidateEntity 메서드로 탭 동작을 사용자 지정 확장성 요소를 살펴보았습니다. 및 Code First, Model First 또는 Database First 워크플로 사용 하 여 개념적 모델을 설명 하는지 여부를 유효성 검사는 이러한 마지막 두 가지 방법을 DbContext를 통해 사용할 수 있습니다.
