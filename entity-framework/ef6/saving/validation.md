---
title: 유효성 검사-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 2c5e6f1b3f60862124bafcac42e8859a7591f8e6
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812154"
---
# <a name="data-validation"></a>데이터 유효성 검사
> [!NOTE]
> **Ef 4.1** 이상-이 페이지에서 설명 하는 기능, api 등은 Entity Framework 4.1에서 도입 되었습니다. 이전 버전을 사용 하는 경우 일부 또는 모든 정보는 적용 되지 않습니다.

이 페이지의 내용은 원래 Julie Lerman ([https://thedatafarm.com](https://thedatafarm.com))에서 작성 한 문서에서 적용 됩니다.

Entity Framework는 클라이언트 쪽 유효성 검사를 위해 사용자 인터페이스를 통해 제공 되거나 서버 쪽 유효성 검사에 사용 될 수 있는 다양 한 유효성 검사 기능을 제공 합니다. Code first를 사용 하는 경우 annotation 또는 흐름 API 구성을 사용 하 여 유효성 검사를 지정할 수 있습니다. 더 복잡 한 추가 유효성 검사는 코드에서 지정 될 수 있으며, 모델이 code first, model first 또는 database first에서 hails 여부에 따라 작동 합니다.

## <a name="the-model"></a>모델

간단한 클래스 쌍 (블로그 및 게시물)으로 유효성 검사를 살펴보겠습니다.

``` csharp
public class Blog
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string BloggerName { get; set; }
    public DateTime DateCreated { get; set; }
    public virtual ICollection<Post> Posts { get; set; }
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

Code First은 코드 첫 번째 클래스를 구성 하는 한 가지 방법으로 `System.ComponentModel.DataAnnotations` 어셈블리의 주석을 사용 합니다. 이러한 주석 중 하나는 `Required`, `MaxLength` 및 `MinLength`와 같은 규칙을 제공 하는 주석입니다. 여러 .NET 클라이언트 응용 프로그램은 이러한 주석 (예: ASP.NET MVC)도 인식 합니다. 이러한 주석으로 클라이언트 쪽 및 서버 쪽 유효성 검사를 모두 수행할 수 있습니다. 예를 들어 블로그 제목 속성을 필수 속성으로 강제할 수 있습니다.

``` csharp
[Required]
public string Title { get; set; }
```

응용 프로그램에서 코드 또는 태그를 추가로 변경 하지 않으면 기존 MVC 응용 프로그램이 클라이언트 쪽 유효성 검사를 수행 하며, 속성 및 주석 이름을 사용 하 여 동적으로 메시지를 작성 합니다.

![그림 1](~/ef6/media/figure01.png)

이 Create view의 다시 게시 메서드에서는 새 블로그를 데이터베이스에 저장 하는 데 Entity Framework를 사용 하지만 응용 프로그램이 해당 코드에 도달 하기 전에 MVC의 클라이언트 쪽 유효성 검사가 트리거됩니다.

그러나 클라이언트 쪽 유효성 검사는 글머리 기호 교정은 아닙니다. 사용자는 브라우저의 기능에 영향을 줄 수 있으며, 해커가 일부 trickery을 사용 하 여 UI 유효성 검사를 피할 수 있습니다. 그러나 Entity Framework `Required` 주석을 인식 하 고 유효성 검사를 수행 합니다.

이를 테스트 하는 간단한 방법은 MVC의 클라이언트 쪽 유효성 검사 기능을 사용 하지 않도록 설정 하는 것입니다. MVC 응용 프로그램의 web.config 파일에서이 작업을 수행할 수 있습니다. AppSettings 섹션에는 ClientValidationEnabled의 키가 있습니다. 이 키를 false로 설정 하면 UI에서 유효성 검사를 수행할 수 없습니다.

``` xml
<appSettings>
    <add key="ClientValidationEnabled"value="false"/>
    ...
</appSettings>
```

클라이언트 쪽 유효성 검사를 사용 하지 않도록 설정한 경우에도 응용 프로그램에서 동일한 응답을 받게 됩니다. "제목 필드가 필요 합니다." 라는 오류 메시지는 이전 처럼 표시 됩니다. 지금은 서버 쪽 유효성 검사의 결과를 제외 하 고, Entity Framework는 `Required` 주석에 대해 유효성 검사를 수행 하 고 (데이터베이스에 전송 하는 `INSERT` 명령을 작성 하기 전에도), 메시지를 표시 하는 MVC로 오류를 반환 합니다.

## <a name="fluent-api"></a>흐름 API

주석 대신 code first의 흐름 API를 사용 하 여 동일한 클라이언트 쪽 & 서버 쪽 유효성 검사를 가져올 수 있습니다. `Required`를 사용 하는 대신 MaxLength 유효성 검사를 사용 하 여이를 표시 합니다.

흐름 API 구성은 code first가 클래스에서 모델을 빌드하는 경우에 적용 됩니다. DbContext 클래스의 OnModelCreating 메서드를 재정의 하 여 구성을 주입할 수 있습니다. 다음은 BloggerName 속성이 10 자를 넘지 않도록 지정 하는 구성입니다.

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

흐름 API 구성에 따라 throw 된 유효성 검사 오류는 UI에 자동으로 도달 하지 않지만 코드에서 캡처하여 적절 하 게 응답할 수 있습니다.

다음은 응용 프로그램의 BlogController 클래스에서 최대 10 자를 초과 하는 BloggerName로 블로그를 저장 하려고 할 Entity Framework 때 해당 유효성 검사 오류를 캡처하는 몇 가지 예외 처리 오류 코드입니다.

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
    catch (DbEntityValidationException ex)
    {
        var error = ex.EntityValidationErrors.First().ValidationErrors.First();
        this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
        return View();
    }
}
```

유효성 검사는 자동으로 다시 뷰에 전달 되지 않으므로 `ModelState.AddModelError`를 사용 하는 추가 코드를 사용 하는 이유를 알 수 있습니다. 이렇게 하면 오류 세부 정보가 뷰에 표시 되 고이를 통해 `ValidationMessageFor` Htmlhelper를 사용 하 여 오류가 표시 됩니다.

``` csharp
@Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a>IValidatableObject

`IValidatableObject`은 `System.ComponentModel.DataAnnotations`에 상주 하는 인터페이스입니다. Entity Framework API의 일부가 아니지만, Entity Framework 클래스에서 서버 쪽 유효성 검사를 위해 활용할 수 있습니다. `IValidatableObject`에서는 SaveChanges 중에를 호출 하는 `Validate` Entity Framework 메서드를 제공 하거나 클래스의 유효성을 검사할 때마다 사용자가 직접 호출할 수 있습니다.

`Required` 및 `MaxLength` 같은 구성은 단일 필드에 대해 유효성 검사를 수행 합니다. `Validate` 방법에서는 예를 들어 두 필드를 비교 하는 것 보다 훨씬 더 복잡 한 논리를 사용할 수 있습니다.

다음 예제에서는 `IValidatableObject`을 구현 하도록 `Blog` 클래스를 확장 한 다음 `Title` 및 `BloggerName` 일치 하지 않는 규칙을 제공 합니다.

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
            yield return new ValidationResult(
                "Blog Title cannot match Blogger Name",
                new[] { nameof(Title), nameof(BloggerName) });
        }
    }
}
```

`ValidationResult` 생성자는 오류 메시지를 나타내는 `string` 및 유효성 검사와 관련 된 멤버 이름을 나타내는 `string`의 배열을 사용 합니다. 이 유효성 검사에서는 `Title`와 `BloggerName`를 모두 확인 하므로 두 속성 이름이 모두 반환 됩니다.

흐름 API에서 제공 하는 유효성 검사와는 달리이 유효성 검사 결과는 뷰에서 인식 되며 이전에 `ModelState` 오류를 추가 하는 데 사용한 예외 처리기가 필요 하지 않습니다. `ValidationResult`에서 두 속성 이름을 모두 설정 했기 때문에 MVC HtmlHelpers 두 속성 모두에 대해 오류 메시지를 표시 합니다.

![그림 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a>DbContext 엔터티

`DbContext`에는 `ValidateEntity`라는 재정의 가능한 메서드가 있습니다. `SaveChanges`를 호출 하면 Entity Framework는 해당 캐시의 각 엔터티에 대해이 메서드를 호출 하 여 상태를 `Unchanged`하지 않습니다. 여기에 유효성 검사 논리를 직접 추가 하거나이 메서드를 사용 하 여 이전 섹션에서 추가 된 `Blog.Validate` 메서드를 호출할 수도 있습니다.

다음은 새 `Post`의 유효성을 검사 하 여 게시 제목이 이미 사용 되지 않았는지 확인 하는 `ValidateEntity` 재정의의 예입니다. 먼저 엔터티가 post이 고 상태가 추가 되었는지 확인 합니다. 해당 하는 경우에는 데이터베이스에서 동일한 제목의 게시물이 이미 있는지 확인 합니다. 기존 게시물이 이미 있으면 새 `DbEntityValidationResult` 만들어집니다.

단일 엔터티에 대 한 `DbEntityEntry` 및 `ICollection<DbValidationErrors>`을 `DbEntityValidationResult` 합니다. 이 메서드가 시작 될 때 `DbEntityValidationResult` 인스턴스화되고 검색 된 모든 오류가 `ValidationErrors` 컬렉션에 추가 됩니다.

``` csharp
protected override DbEntityValidationResult ValidateEntity (
    System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
    IDictionary<object, object> items)
{
    var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());

    if (entityEntry.Entity is Post post && entityEntry.State == EntityState.Added)
    {
        // Check for uniqueness of post title
        if (Posts.Where(p => p.Title == post.Title).Any())
        {
            result.ValidationErrors.Add(
                    new System.Data.Entity.Validation.DbValidationError(
                        nameof(Title),
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

## <a name="explicitly-triggering-validation"></a>명시적으로 유효성 검사 트리거

`SaveChanges` 호출은이 문서에서 설명 하는 모든 유효성 검사를 트리거합니다. 그러나 `SaveChanges`를 사용 하지 않아도 됩니다. 응용 프로그램의 다른 위치에서 유효성을 검사 하는 것이 좋습니다.

`DbContext.GetValidationErrors`는 주석 또는 흐름 API에 의해 정의 된 유효성 검사, `IValidatableObject` (예: `Blog.Validate`)에서 생성 된 유효성 검사 및 `DbContext.ValidateEntity` 메서드에서 수행 된 유효성 검사를 모두 트리거합니다.

다음 코드는 `DbContext`의 현재 인스턴스에서 `GetValidationErrors`를 호출 합니다. `ValidationErrors` 엔터티 유형별로 그룹화 되어 `DbEntityValidationResult`합니다. 이 코드는 메서드를 통해 반환 된 `DbEntityValidationResult`를 먼저 반복한 다음 내의 각 `DbValidationError`를 반복 합니다.

``` csharp
foreach (var validationResult in db.GetValidationErrors())
{
    foreach (var error in validationResult.ValidationErrors)
    {
        Debug.WriteLine(
            "Entity Property: {0}, Error {1}",
            error.PropertyName,
            error.ErrorMessage);
    }
}
```

## <a name="other-considerations-when-using-validation"></a>유효성 검사를 사용할 때 기타 고려 사항

Entity Framework 유효성 검사를 사용할 때 고려해 야 할 몇 가지 사항은 다음과 같습니다.

- 유효성 검사 중에 지연 로드를 사용할 수 없습니다.
- EF는 매핑되지 않은 속성 (데이터베이스의 열에 매핑되지 않은 속성)에서 데이터 주석의 유효성을 검사 합니다.
- 유효성 검사는 `SaveChanges`중에 변경 내용이 검색 된 후에 수행 됩니다. 유효성 검사 중에 변경을 수행 하는 경우 변경 추적 장치에 알려야 합니다.
- 유효성 검사 중 오류가 발생 하는 경우 `DbUnexpectedValidationException` throw 됨
- 모델에 포함 Entity Framework 되는 패싯 (최대 길이, 필수 등)은 클래스에 데이터 주석이 없거나 EF Designer를 사용 하 여 모델을 만든 경우에도 유효성 검사를 발생 시킵니다.
- 선행 규칙:
  - 흐름 API 호출은 해당 데이터 주석을 재정의 합니다.
- 실행 순서:
  - 속성 유효성 검사가 형식 유효성 검사 전에 발생 합니다.
  - 형식 유효성 검사는 속성 유효성 검사가 성공 하는 경우에만 발생 합니다.
- 속성이 복잡 한 경우 유효성 검사에는 다음 항목도 포함 됩니다.
  - 복합 형식 속성에 대 한 속성 수준 유효성 검사
  - 복합 형식에 대 한 `IValidatableObject` 유효성 검사를 포함 하 여 복합 형식에 대 한 형식 수준 유효성 검사

## <a name="summary"></a>요약

Entity Framework 유효성 검사 API는 MVC에서 클라이언트 쪽 유효성 검사를 통해 매우 원활 하 게 재생 되지만 클라이언트 쪽 유효성 검사를 사용 하지 않아도 됩니다. Entity Framework는 code first 흐름 API를 사용 하 여 적용 한 데이터 주석 또는 구성에 대해 서버 쪽의 유효성 검사를 처리 합니다.

또한 `IValidatableObject` 인터페이스를 사용 하거나 `DbContext.ValidateEntity` 메서드를 탭 하 여 동작을 사용자 지정 하는 다양 한 확장성 지점이 살펴보았습니다. Code First, Model First 또는 Database First 워크플로를 사용 하 여 개념적 모델을 설명 하는 경우에는 `DbContext`를 통해 마지막 두 가지 유효성 검사 방법을 사용할 수 있습니다.
