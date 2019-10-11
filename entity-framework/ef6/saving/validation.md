---
title: 유효성 검사-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 4162c2eb60109459c799da7cf4c1a9c8e84548b6
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182126"
---
# <a name="data-validation"></a><span data-ttu-id="87f5c-102">데이터 유효성 검사</span><span class="sxs-lookup"><span data-stu-id="87f5c-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="87f5c-103">**Ef 4.1** 이상-이 페이지에서 설명 하는 기능, api 등은 Entity Framework 4.1에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="87f5c-104">이전 버전을 사용 하는 경우 일부 또는 모든 정보는 적용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="87f5c-105">이 페이지의 내용은 원래 Julie Lerman ([https://thedatafarm.com](http://thedatafarm.com))에서 작성 한 아티클에서 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-105">The content on this page is adapted from an article originally written by Julie Lerman ([https://thedatafarm.com](http://thedatafarm.com)).</span></span>

<span data-ttu-id="87f5c-106">Entity Framework는 클라이언트 쪽 유효성 검사를 위해 사용자 인터페이스를 통해 제공 되거나 서버 쪽 유효성 검사에 사용 될 수 있는 다양 한 유효성 검사 기능을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="87f5c-107">Code first를 사용 하는 경우 annotation 또는 흐름 API 구성을 사용 하 여 유효성 검사를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="87f5c-108">더 복잡 한 추가 유효성 검사는 코드에서 지정 될 수 있으며, 모델이 code first, model first 또는 database first에서 hails 여부에 따라 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="87f5c-109">모델</span><span class="sxs-lookup"><span data-stu-id="87f5c-109">The model</span></span>

<span data-ttu-id="87f5c-110">간단한 클래스 쌍을 사용 하 여 유효성 검사를 살펴보겠습니다. 블로그 및 게시물.</span><span class="sxs-lookup"><span data-stu-id="87f5c-110">I'll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="87f5c-111">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="87f5c-111">Data Annotations</span></span>

<span data-ttu-id="87f5c-112">Code First은 코드 첫 번째 클래스를 구성 하는 한 가지 방법으로 `System.ComponentModel.DataAnnotations` 어셈블리의 주석을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-112">Code First uses annotations from the `System.ComponentModel.DataAnnotations` assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="87f5c-113">이러한 주석 중 하나는 `Required`, `MaxLength` 및 `MinLength`와 같은 규칙을 제공 하는 주석입니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-113">Among these annotations are those which provide rules such as the `Required`, `MaxLength` and `MinLength`.</span></span> <span data-ttu-id="87f5c-114">여러 .NET 클라이언트 응용 프로그램은 이러한 주석 (예: ASP.NET MVC)도 인식 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="87f5c-115">이러한 주석으로 클라이언트 쪽 및 서버 쪽 유효성 검사를 모두 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="87f5c-116">예를 들어 블로그 제목 속성을 필수 속성으로 강제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
[Required]
public string Title { get; set; }
```

<span data-ttu-id="87f5c-117">응용 프로그램에서 코드 또는 태그를 추가로 변경 하지 않으면 기존 MVC 응용 프로그램이 클라이언트 쪽 유효성 검사를 수행 하며, 속성 및 주석 이름을 사용 하 여 동적으로 메시지를 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![그림 1](~/ef6/media/figure01.png)

<span data-ttu-id="87f5c-119">이 Create view의 다시 게시 메서드에서는 새 블로그를 데이터베이스에 저장 하는 데 Entity Framework를 사용 하지만 응용 프로그램이 해당 코드에 도달 하기 전에 MVC의 클라이언트 쪽 유효성 검사가 트리거됩니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC's client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="87f5c-120">그러나 클라이언트 쪽 유효성 검사는 글머리 기호 교정은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="87f5c-121">사용자는 브라우저의 기능에 영향을 줄 수 있으며, 해커가 일부 trickery을 사용 하 여 UI 유효성 검사를 피할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="87f5c-122">그러나 Entity Framework는 `Required` 주석을 인식 하 고 유효성을 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-122">But Entity Framework will also recognize the `Required` annotation and validate it.</span></span>

<span data-ttu-id="87f5c-123">이를 테스트 하는 간단한 방법은 MVC의 클라이언트 쪽 유효성 검사 기능을 사용 하지 않도록 설정 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-123">A simple way to test this is to disable MVC's client-side validation feature.</span></span> <span data-ttu-id="87f5c-124">MVC 응용 프로그램의 web.config 파일에서이 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-124">You can do this in the MVC application's web.config file.</span></span> <span data-ttu-id="87f5c-125">AppSettings 섹션에는 ClientValidationEnabled의 키가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="87f5c-126">이 키를 false로 설정 하면 UI에서 유효성 검사를 수행할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
<appSettings>
    <add key="ClientValidationEnabled"value="false"/>
    ...
</appSettings>
```

<span data-ttu-id="87f5c-127">클라이언트 쪽 유효성 검사를 사용 하지 않도록 설정한 경우에도 응용 프로그램에서 동일한 응답을 받게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="87f5c-128">"제목 필드가 필요 합니다." 라는 오류 메시지는 이전 처럼 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-128">The error message "The Title field is required" will be displayed as before.</span></span> <span data-ttu-id="87f5c-129">지금은 서버 쪽 유효성 검사의 결과를 제외 하 고,</span><span class="sxs-lookup"><span data-stu-id="87f5c-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="87f5c-130">Entity Framework은 (는) `Required` 주석에서 유효성 검사를 수행 하 고 (데이터베이스에 전송 하는 `INSERT` 명령을 작성 하기 bothers) 메시지를 표시 하는 MVC로 오류를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-130">Entity Framework will perform the validation on the `Required` annotation (before it even bothers to build an `INSERT` command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="87f5c-131">Fluent API</span><span class="sxs-lookup"><span data-stu-id="87f5c-131">Fluent API</span></span>

<span data-ttu-id="87f5c-132">주석 대신 code first의 흐름 API를 사용 하 여 동일한 클라이언트 쪽 & 서버 쪽 유효성 검사를 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-132">You can use code first's fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="87f5c-133">@No__t-0을 사용 하는 대신 MaxLength 유효성 검사를 사용 하 여이를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-133">Rather than use `Required`, I'll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="87f5c-134">흐름 API 구성은 code first가 클래스에서 모델을 빌드하는 경우에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="87f5c-135">DbContext 클래스의 OnModelCreating 메서드를 재정의 하 여 구성을 주입할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-135">You can inject the configurations by overriding the DbContext class' OnModelCreating  method.</span></span> <span data-ttu-id="87f5c-136">다음은 BloggerName 속성이 10 자를 넘지 않도록 지정 하는 구성입니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

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

<span data-ttu-id="87f5c-137">흐름 API 구성에 따라 throw 된 유효성 검사 오류는 UI에 자동으로 도달 하지 않지만 코드에서 캡처하여 적절 하 게 응답할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="87f5c-138">다음은 응용 프로그램의 BlogController 클래스에서 최대 10 자를 초과 하는 BloggerName로 블로그를 저장 하려고 할 Entity Framework 때 해당 유효성 검사 오류를 캡처하는 몇 가지 예외 처리 오류 코드입니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-138">Here's some exception handling error code in the application's BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

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

<span data-ttu-id="87f5c-139">유효성 검사는 자동으로 다시 뷰에 전달 되지 않으므로 `ModelState.AddModelError`을 사용 하는 추가 코드를 사용 하는 이유입니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-139">The validation doesn't automatically get passed back into the view which is why the additional code that uses `ModelState.AddModelError` is being used.</span></span> <span data-ttu-id="87f5c-140">이렇게 하면 오류 세부 정보에서 뷰를 확인 하 여 오류를 표시 하는 `ValidationMessageFor` Htmlhelper를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-140">This ensures that the error details make it to the view which will then use the `ValidationMessageFor` Htmlhelper to display the error.</span></span>

``` csharp
@Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="87f5c-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="87f5c-141">IValidatableObject</span></span>

<span data-ttu-id="87f5c-142">`IValidatableObject`은 `System.ComponentModel.DataAnnotations`에 상주 하는 인터페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-142">`IValidatableObject` is an interface that lives in `System.ComponentModel.DataAnnotations`.</span></span> <span data-ttu-id="87f5c-143">Entity Framework API의 일부가 아니지만, Entity Framework 클래스에서 서버 쪽 유효성 검사를 위해 활용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="87f5c-144">`IValidatableObject`은 SaveChanges 중에를 호출 하는 @no__t 1 Entity Framework 메서드를 제공 하거나 클래스의 유효성을 검사할 때마다 사용자가 직접 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-144">`IValidatableObject` provides a `Validate` method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="87f5c-145">@No__t-0 및 `MaxLength`과 같은 구성은 단일 필드에 대해 유효성 검사를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-145">Configurations such as `Required` and `MaxLength` perform validation on a single field.</span></span> <span data-ttu-id="87f5c-146">@No__t-0 메서드에서는 두 필드를 비교 하는 것과 같이 보다 복잡 한 논리를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-146">In the `Validate` method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="87f5c-147">다음 예제에서는 `Blog` 클래스가 `IValidatableObject`을 구현 하도록 확장 된 후 `Title`와 `BloggerName`이 일치할 수 없다는 규칙을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-147">In the following example, the `Blog` class has been extended to implement `IValidatableObject` and then provide a rule that the `Title` and `BloggerName` cannot match.</span></span>

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

<span data-ttu-id="87f5c-148">@No__t-0 생성자는 오류 메시지 및 유효성 검사와 관련 된 멤버 이름을 나타내는 `string` s 배열을 나타내는 `string`을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-148">The `ValidationResult` constructor takes a `string` that represents the error message and an array of `string`s that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="87f5c-149">이 유효성 검사는 `Title`과 `BloggerName`을 모두 검사 하므로 두 속성 이름이 모두 반환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-149">Since this validation checks both the `Title` and the `BloggerName`, both property names are returned.</span></span>

<span data-ttu-id="87f5c-150">흐름 API에서 제공 하는 유효성 검사와는 달리이 유효성 검사 결과는 뷰에서 인식 되며, 이전에이 오류를 `ModelState`에 추가 하는 데 사용한 예외 처리기가 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into `ModelState` is unnecessary.</span></span> <span data-ttu-id="87f5c-151">@No__t-0에 두 속성 이름을 모두 설정 했기 때문에 MVC HtmlHelpers 이러한 두 속성 모두에 대해 오류 메시지를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-151">Because I set both property names in the `ValidationResult`, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![그림 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="87f5c-153">DbContext.ValidateEntity</span><span class="sxs-lookup"><span data-stu-id="87f5c-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="87f5c-154">`DbContext`에는 `ValidateEntity` 이라는 재정의 가능한 메서드가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-154">`DbContext` has an overridable method called `ValidateEntity`.</span></span> <span data-ttu-id="87f5c-155">@No__t-0을 호출 하면 Entity Framework는 해당 @no__t 캐시의 각 엔터티에 대해이 메서드를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-155">When you call `SaveChanges`, Entity Framework will call this method for each entity in its cache whose state is not `Unchanged`.</span></span> <span data-ttu-id="87f5c-156">여기에 유효성 검사 논리를 직접 추가 하거나이 메서드를 사용 하 여 이전 섹션에 추가 된 `Blog.Validate` 메서드를 호출할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-156">You can put validation logic directly in here or even use this method to call, for example, the `Blog.Validate` method added in the previous section.</span></span>

<span data-ttu-id="87f5c-157">다음은 새 `Post`의 유효성을 검사 하 여 게시 제목이 이미 사용 되지 않았는지 확인 하는 @no__t 0 재정의 예제입니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-157">Here's an example of a `ValidateEntity` override that validates new `Post`s to ensure that the post title hasn't been used already.</span></span> <span data-ttu-id="87f5c-158">먼저 엔터티가 post이 고 상태가 추가 되었는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="87f5c-159">해당 하는 경우에는 데이터베이스에서 동일한 제목의 게시물이 이미 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-159">If that's the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="87f5c-160">기존 게시물이 이미 있으면 새 `DbEntityValidationResult`이 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-160">If there is an existing post already, then a new `DbEntityValidationResult` is created.</span></span>

<span data-ttu-id="87f5c-161">`DbEntityValidationResult`에는 단일 엔터티에 대 한 @no__t 1과 `ICollection<DbValidationErrors>`가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-161">`DbEntityValidationResult` houses a `DbEntityEntry` and an `ICollection<DbValidationErrors>` for a single entity.</span></span> <span data-ttu-id="87f5c-162">이 메서드가 시작 될 때 `DbEntityValidationResult`이 인스턴스화되고 검색 된 모든 오류가 `ValidationErrors` 컬렉션에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-162">At the start of this method, a `DbEntityValidationResult` is instantiated and then any errors that are discovered are added into its `ValidationErrors` collection.</span></span>

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

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="87f5c-163">명시적으로 유효성 검사 트리거</span><span class="sxs-lookup"><span data-stu-id="87f5c-163">Explicitly triggering validation</span></span>

<span data-ttu-id="87f5c-164">@No__t-0에 대 한 호출은이 문서에서 설명 하는 모든 유효성 검사를 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-164">A call to `SaveChanges` triggers all of the validations covered in this article.</span></span> <span data-ttu-id="87f5c-165">그러나 `SaveChanges`을 사용 하지 않아도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-165">But you don't need to rely on `SaveChanges`.</span></span> <span data-ttu-id="87f5c-166">응용 프로그램의 다른 위치에서 유효성을 검사 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="87f5c-167">`DbContext.GetValidationErrors`은 주석 또는 흐름 API에 정의 된 유효성 검사, `IValidatableObject`에서 만든 유효성 검사 (예: `Blog.Validate`) 및 `DbContext.ValidateEntity` 메서드에서 수행 된 유효성 검사를 모두 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-167">`DbContext.GetValidationErrors` will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in `IValidatableObject` (for example, `Blog.Validate`), and the validations performed in the `DbContext.ValidateEntity` method.</span></span>

<span data-ttu-id="87f5c-168">다음 코드는-1 @no__t의 현재 인스턴스에서 `GetValidationErrors`을 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-168">The following code will call `GetValidationErrors` on the current instance of a `DbContext`.</span></span> <span data-ttu-id="87f5c-169">`ValidationErrors`은 엔터티 형식별로 `DbEntityValidationResult`로 그룹화 됩니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-169">`ValidationErrors` are grouped by entity type into `DbEntityValidationResult`.</span></span> <span data-ttu-id="87f5c-170">이 코드는 메서드를 통해 반환 된 @no__t 0부터 시작 하 여 내의 각 `DbValidationError`을 통해 먼저 반복 됩니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-170">The code iterates first through the `DbEntityValidationResult`s returned by the method and then through each `DbValidationError` inside.</span></span>

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

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="87f5c-171">유효성 검사를 사용할 때 기타 고려 사항</span><span class="sxs-lookup"><span data-stu-id="87f5c-171">Other considerations when using validation</span></span>

<span data-ttu-id="87f5c-172">Entity Framework 유효성 검사를 사용할 때 고려해 야 할 몇 가지 사항은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

- <span data-ttu-id="87f5c-173">유효성 검사 중에 지연 로드를 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-173">Lazy loading is disabled during validation</span></span>
- <span data-ttu-id="87f5c-174">EF는 매핑되지 않은 속성 (데이터베이스의 열에 매핑되지 않은 속성)에서 데이터 주석의 유효성을 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database)</span></span>
- <span data-ttu-id="87f5c-175">-0 @no__t 중에 변경 내용이 검색 된 후 유효성 검사가 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-175">Validation is performed after changes are detected during `SaveChanges`.</span></span> <span data-ttu-id="87f5c-176">유효성 검사 중에 변경을 수행 하는 경우 변경 추적 장치에 알려야 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-176">If you make changes during validation it is your responsibility to notify the change tracker</span></span>
- <span data-ttu-id="87f5c-177">유효성 검사 중에 오류가 발생 하면 `DbUnexpectedValidationException`이 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-177">`DbUnexpectedValidationException` is thrown if errors occur during validation</span></span>
- <span data-ttu-id="87f5c-178">모델에 포함 Entity Framework 되는 패싯 (최대 길이, 필수 등)은 클래스에 데이터 주석이 없거나 EF Designer를 사용 하 여 모델을 만든 경우에도 유효성 검사를 발생 시킵니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are no data annotations in your classes and/or you used the EF Designer to create your model</span></span>
- <span data-ttu-id="87f5c-179">선행 규칙:</span><span class="sxs-lookup"><span data-stu-id="87f5c-179">Precedence rules:</span></span>
  - <span data-ttu-id="87f5c-180">흐름 API 호출은 해당 데이터 주석을 재정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-180">Fluent API calls override the corresponding data annotations</span></span>
- <span data-ttu-id="87f5c-181">실행 순서:</span><span class="sxs-lookup"><span data-stu-id="87f5c-181">Execution order:</span></span>
  - <span data-ttu-id="87f5c-182">속성 유효성 검사가 형식 유효성 검사 전에 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-182">Property validation occurs before type validation</span></span>
  - <span data-ttu-id="87f5c-183">형식 유효성 검사는 속성 유효성 검사가 성공 하는 경우에만 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-183">Type validation only occurs if property validation succeeds</span></span>
- <span data-ttu-id="87f5c-184">속성이 복잡 한 경우 유효성 검사에는 다음 항목도 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-184">If a property is complex, its validation will also include:</span></span>
  - <span data-ttu-id="87f5c-185">복합 형식 속성에 대 한 속성 수준 유효성 검사</span><span class="sxs-lookup"><span data-stu-id="87f5c-185">Property-level validation on the complex type properties</span></span>
  - <span data-ttu-id="87f5c-186">복합 형식에 대 한 `IValidatableObject` 유효성 검사를 포함 하 여 복합 형식에 대 한 형식 수준 유효성 검사</span><span class="sxs-lookup"><span data-stu-id="87f5c-186">Type level validation on the complex type, including `IValidatableObject` validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="87f5c-187">요약</span><span class="sxs-lookup"><span data-stu-id="87f5c-187">Summary</span></span>

<span data-ttu-id="87f5c-188">Entity Framework 유효성 검사 API는 MVC에서 클라이언트 쪽 유효성 검사를 통해 매우 원활 하 게 재생 되지만 클라이언트 쪽 유효성 검사를 사용 하지 않아도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="87f5c-189">Entity Framework는 code first 흐름 API를 사용 하 여 적용 한 데이터 주석 또는 구성에 대해 서버 쪽의 유효성 검사를 처리 합니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="87f5c-190">@No__t-0 인터페이스를 사용 하거나 `DbContext.ValidateEntity` 메서드를 탭 하 여 동작을 사용자 지정 하는 다양 한 확장성 지점이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-190">You also saw a number of extensibility points for customizing the behavior whether you use the `IValidatableObject` interface or tap into the `DbContext.ValidateEntity` method.</span></span> <span data-ttu-id="87f5c-191">Code First, Model First 또는 Database First 워크플로를 사용 하 여 개념적 모델을 설명 하는지 여부에 관계 없이 이러한 마지막 두 가지 유효성 검사 방법은 `DbContext`을 통해 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="87f5c-191">And these last two means of validation are available through the `DbContext`, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
