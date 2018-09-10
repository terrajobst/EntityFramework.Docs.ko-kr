---
title: EF6 유효성 검사
author: divega
ms.date: 2016-10-23
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 65639b0f91f54ee2cd1336f6b6cd4caf45ede680
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251026"
---
# <a name="data-validation"></a><span data-ttu-id="f0a15-102">데이터 유효성 검사</span><span class="sxs-lookup"><span data-stu-id="f0a15-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="f0a15-103">**EF4.1 이상만** -Api 기능 등이이 페이지에 설명 된 Entity Framework 4.1에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="f0a15-104">일부 또는 모든 정보는 이전 버전을 사용 하는 경우 적용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="f0a15-105">이 페이지에 있는 콘텐츠는 및 Julie lerman 작성 원래 작성 된 문서 ([http://thedatafarm.com](http://thedatafarm.com)).</span><span class="sxs-lookup"><span data-stu-id="f0a15-105">The content on this page is adapted from and article originally written by Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span></span>

<span data-ttu-id="f0a15-106">Entity Framework는 다양 한 클라이언트 쪽 유효성 검사에 대 한 사용자 인터페이스를 통해 공급할 수 또는 서버 쪽 유효성 검사에 사용할 수 있는 유효성 검사 기능을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="f0a15-107">코드를 처음 사용 하는 경우 주석 또는 fluent API 구성이 사용 하 여 유효성 검사를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="f0a15-108">추가 유효성 검사 및 복잡 한 코드에서 지정 될 수 있으며 모델 코드에서 먼저 hails 인지 관계 없이 작동 하는 모델을 우선 또는 먼저 데이터베이스입니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="f0a15-109">모델</span><span class="sxs-lookup"><span data-stu-id="f0a15-109">The model</span></span>

<span data-ttu-id="f0a15-110">클래스의 간단한 쌍을 사용 하 여 유효성 검사를 설명 하겠습니다: 블로그 및 게시물.</span><span class="sxs-lookup"><span data-stu-id="f0a15-110">I’ll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

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
## <a name="data-annotations"></a><span data-ttu-id="f0a15-111">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="f0a15-111">Data Annotations</span></span>

<span data-ttu-id="f0a15-112">코드는 먼저 코드 첫 번째 클래스를 구성 하는 하나의 수단으로 주석을 System.ComponentModel.DataAnnotations 어셈블리를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-112">Code First uses annotations from the System.ComponentModel.DataAnnotations assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="f0a15-113">이러한 주석은 간에 필요 MinLength 및 MaxLength와 같은 규칙을 제공 하는입니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-113">Among these annotations are those which provide rules such as the Required, MaxLength and MinLength.</span></span> <span data-ttu-id="f0a15-114">.NET 클라이언트 응용 프로그램의 수는 또한 이러한 주석, 예를 들어, ASP.NET MVC를 인식합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="f0a15-115">두 클라이언트 쪽 및 서버 쪽 유효성 검사 이러한 주석 사용 하 여 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="f0a15-116">예를 들어 블로그 Title 속성 필수 속성을 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="f0a15-117">추가 코드 또는 응용 프로그램에서 태그 변경 없이 기존 MVC 응용 프로그램에는 클라이언트 쪽 유효성 검사, 속성 및 주석 이름을 사용 하 여 메시지를 동적으로 작성을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![그림 1](~/ef6/media/figure01.png)

<span data-ttu-id="f0a15-119">게시물에 Create view이 메서드의 백업할 Entity Framework는 새 블로그가 데이터베이스에 저장 하는 데 사용 되는 있지만 MVC의 클라이언트 쪽 유효성 검사는 응용 프로그램 코드에 도달 하기 전에 트리거됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC’s client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="f0a15-120">그러나 클라이언트 쪽 유효성 검사 완벽 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="f0a15-121">사용자가 브라우저의 기능에 영향을 줄 수 있습니다 하거나 더 심한 아직 해커가 수 사용 일부 책략 UI 유효성 검사를 하지 않으려면 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="f0a15-122">하지만 Entity Framework 또한 필요한 주석이 인식 하 고 유효성을 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-122">But Entity Framework will also recognize the Required annotation and validate it.</span></span>

<span data-ttu-id="f0a15-123">이 테스트 하는 간단한 방법은 MVC의 클라이언트 쪽 유효성 검사 기능을 사용 하지 않도록 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-123">A simple way to test this is to disable MVC’s client-side validation feature.</span></span> <span data-ttu-id="f0a15-124">MVC 응용 프로그램의 web.config 파일에서이 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-124">You can do this in the MVC application’s web.config file.</span></span> <span data-ttu-id="f0a15-125">AppSettings 섹션에 ClientValidationEnabled 키</span><span class="sxs-lookup"><span data-stu-id="f0a15-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="f0a15-126">이 키를 false로 설정 UI에서 유효성 검사를 수행 하지 것입니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

<span data-ttu-id="f0a15-127">클라이언트 쪽 유효성 검사를 사용 하지 않도록 설정도 사용 하 여 응용 프로그램에서 동일한 응답을 받을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="f0a15-128">오류 메시지 "제목 필드는 필수"로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-128">The error message “The Title field is required” will be displayed as.</span></span> <span data-ttu-id="f0a15-129">이제 제외 하 고 서버 쪽 유효성 검사의 결과가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="f0a15-130">Entity Framework는 유효성 검사를 수행 필요 주석을 (빌드 및 INSERT 명령 데이터베이스를 전송에 분명) 전에 메시지를 표시 하는 MVC로 오류를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-130">Entity Framework will perform the validation on the Required annotation (before it even bothers to build and INSERT command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="f0a15-131">Fluent API</span><span class="sxs-lookup"><span data-stu-id="f0a15-131">Fluent API</span></span>

<span data-ttu-id="f0a15-132">코드를 사용 하면 첫 번째의 fluent API 주석 대신를 동일한 클라이언트 쪽 및 서버 쪽 유효성 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-132">You can use code first’s fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="f0a15-133">대신 필요한 사용 하겠습니다 MaxLength 유효성 검사를 사용 하 여이 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-133">Rather than use Required, I’ll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="f0a15-134">코드를 클래스에서 모델을 먼저 만들고 대로 Fluent API 구성이 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="f0a15-135">DbContext 클래스의 OnModelCreating 메서드를 재정의 하 여 구성을 삽입할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-135">You can inject the configurations by overriding the DbContext class’ OnModelCreating  method.</span></span> <span data-ttu-id="f0a15-136">다음은 BloggerName 속성은 10 자 보다 길지 수를 지정 하는 구성입니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

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

<span data-ttu-id="f0a15-137">발생 한 유효성 검사 오류 Fluent API 구성에 따라는 자동으로 도달률 UI 있지만 수 캡처하지 않습니다 것 코드를 다음 응답에 적절 하 게 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="f0a15-138">여기의 예외 처리 오류 코드는 Entity Framework를 사용 하 여 최대 10 자 초과 BloggerName 블로그를 저장 하려고 할 때 해당 유효성 검사 오류를 캡처하는 응용 프로그램의 BlogController 클래스에 일부입니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-138">Here’s some exception handling error code in the application’s BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

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

<span data-ttu-id="f0a15-139">유효성 검사 ModelState.AddModelError를 사용 하는 추가 코드를 사용 하는 이유는 보기로 다시 자동으로 전달 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-139">The validation doesn’t automatically get passed back into the view which is why the additional code that uses ModelState.AddModelError is being used.</span></span> <span data-ttu-id="f0a15-140">오류 세부 정보를 사용 하 여 ValidationMessageFor Htmlhelper 오류를 표시 하도록 뷰 하 하 게 하는 것이 이렇게 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-140">This ensures that the error details make it to the view which will then use the ValidationMessageFor Htmlhelper to display the error.</span></span>

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="f0a15-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="f0a15-141">IValidatableObject</span></span>

<span data-ttu-id="f0a15-142">IValidatableObject에 System.ComponentModel.DataAnnotations 존재 하는 인터페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-142">IValidatableObject is an interface that lives in System.ComponentModel.DataAnnotations.</span></span> <span data-ttu-id="f0a15-143">아니지만 Entity Framework API의 일부를 계속 활용 하 여 서버 쪽 유효성 검사에 대 한 Entity Framework 클래스에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="f0a15-144">IValidatableObject은 SaveChanges 때나 Entity Framework를 호출 하는 Validate 메서드를 호출할 수 직접 클래스의 유효성을 검사 하려는 언제 든 지를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-144">IValidatableObject provides a Validate method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="f0a15-145">와 같은 필요한 구성 및 MaxLength 하지 단일 필드에서 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-145">Configurations such as Required and MaxLength perform validaton on a single field.</span></span> <span data-ttu-id="f0a15-146">Validate 메서드를 두 개의 필드를 비교 합니다. 예를 들어, 훨씬 더 복잡 한 논리를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-146">In the Validate method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="f0a15-147">다음 예에서 블로그 클래스 IValidatableObject를 구현 하 고 제목 및 BloggerName 일치할 수 없습니다. 규칙을 제공 하도록 확장 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-147">In the following example, the Blog class has been extended to implement IValidatableObject and then provide a rule that the Title and BloggerName cannot match.</span></span>

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

<span data-ttu-id="f0a15-148">ValidationResult 생성자 오류 메시지와 유효성 검사와 연관 된 멤버 이름을 나타내는 문자열 배열을 나타내는 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-148">The ValidationResult constructor takes a string that represents the error message and an array of strings that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="f0a15-149">이 유효성 검사 제목과 BloggerName 모두 확인 되므로 속성 이름은 모두 반환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-149">Since this validation checks both the Title and the BloggerName, both property names are returned.</span></span>

<span data-ttu-id="f0a15-150">Fluent API에서 제공 되는 유효성 검사와는 달리이 유효성 검사 결과 보기에서 인식 됩니다 및 ModelState에 오류를 추가 하려면 이전 사용 하는 예외 처리기 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into ModelState is unnecessary.</span></span> <span data-ttu-id="f0a15-151">ValidationResult의 두 속성 이름이 설정, 때문에 MVC HtmlHelpers 해당 속성을 모두에 대 한 오류 메시지를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-151">Because I set both property names in the ValidationResult, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![그림 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="f0a15-153">DbContext.ValidateEntity</span><span class="sxs-lookup"><span data-stu-id="f0a15-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="f0a15-154">DbContext은 ValidateEntity 호출 재정의 가능한 메서드를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-154">DbContext has an Overridable method called ValidateEntity.</span></span> <span data-ttu-id="f0a15-155">SaveChanges를 호출 하면 Entity Framework 상태가 변경 되지 않은 아닌 해당 캐시의 각 엔터티에 대해이 메서드를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-155">When you call SaveChanges, Entity Framework will call this method for each entity in its cache whose state is not Unchanged.</span></span> <span data-ttu-id="f0a15-156">넣으면 유효성 검사 논리 사용 하 여 또는 여기에서 직접 예를 들어, 이전 섹션에서 추가한 Blog.Validate 메서드를 호출 하려면이 메서드.</span><span class="sxs-lookup"><span data-stu-id="f0a15-156">You can put validation logic directly in here or even use this method to call, for example, the Blog.Validate method added in the previous section.</span></span>

<span data-ttu-id="f0a15-157">게시물 제목을 이미 사용 되지 않은 확인에 대 한 새 게시의 유효성을 검사 하는 ValidateEntity 재정의의 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-157">Here’s an example of a ValidateEntity override that validates new Posts to ensure that the post title hasn’t been used already.</span></span> <span data-ttu-id="f0a15-158">먼저 확인 하는 경우 엔터티 게시물 상태로 추가 되어 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="f0a15-159">하는 경우 다음 검색 데이터베이스를 같은 제목의 게시물 이미 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-159">If that’s the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="f0a15-160">기존 게시물 이미 있으면 새 DbEntityValidationResult 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-160">If there is an existing post already, then a new DbEntityValidationResult is created.</span></span>

<span data-ttu-id="f0a15-161">DbEntityValidationResult를 DbEntityEntry 및는 ICollection의 DbValidationErrors 단일 엔터티에 대 한 보관 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-161">DbEntityValidationResult houses a DbEntityEntry and an ICollection of DbValidationErrors for a single entity.</span></span> <span data-ttu-id="f0a15-162">이 메서드의 처음에 DbEntityValidationResult 인스턴스화되고 검색 된 모든 오류 ValidationErrors 컬렉션에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-162">At the start of this method, a  DbEntityValidationResult is instantiated and then any errors that are discovered are added into its ValidationErrors collection.</span></span>

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

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="f0a15-163">명시적으로 유효성 검사를 트리거하지</span><span class="sxs-lookup"><span data-stu-id="f0a15-163">Explicitly triggering validation</span></span>

<span data-ttu-id="f0a15-164">SaveChanges 호출 모두이 문서에서 설명 하는 유효성 검사를 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-164">A call to SaveChanges triggers all of the validations covered in this article.</span></span> <span data-ttu-id="f0a15-165">하지만 SaveChanges에 의존할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-165">But you don’t need to rely on SaveChanges.</span></span> <span data-ttu-id="f0a15-166">응용 프로그램의 다른 곳에서 유효성을 검사할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="f0a15-167">DbContext.GetValidationErrors 모든 유효성 검사, 주석 또는 Fluent API에 의해 정의 된, IValidatableObject (예를 들어 Blog.Validate)에서 만든 유효성 검사 및는 DbContext.ValidateEntity에서 수행 하는 유효성 검사를 트리거 메서드입니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-167">DbContext.GetValidationErrors will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in IValidatableObject (for example, Blog.Validate), and the validations performed in the DbContext.ValidateEntity method.</span></span>

<span data-ttu-id="f0a15-168">다음 코드는 DbContext의 현재 인스턴스에서 GetValidationErrors를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-168">The following code will call GetValidationErrors on the current instance of a DbContext.</span></span> <span data-ttu-id="f0a15-169">ValidationErrors는 DbValidationRestuls에 엔터티 형식별으로 그룹화 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-169">ValidationErrors are grouped by entity type into DbValidationRestuls.</span></span> <span data-ttu-id="f0a15-170">확장 메서드에서 반환한 DbValidationResults 한 다음 각 ValidationError 내에서 코드를 먼저 반복 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-170">The code iterates first through the DbValidationResults returned by the method and then through each ValidationError inside.</span></span>

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

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="f0a15-171">유효성 검사를 사용 하는 경우 기타 고려 사항</span><span class="sxs-lookup"><span data-stu-id="f0a15-171">Other considerations when using validation</span></span>

<span data-ttu-id="f0a15-172">다른 몇 가지 Entity Framework 유효성 검사를 사용 하는 경우 고려할 사항은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

-   <span data-ttu-id="f0a15-173">유효성 검사 중 지연 로드가 비활성화 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-173">Lazy loading is disabled during validation.</span></span>
-   <span data-ttu-id="f0a15-174">EF는 비 매핑된 속성 (데이터베이스의 열에 매핑되지 않은 속성)에 대 한 데이터 주석 유효성을 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database).</span></span>
-   <span data-ttu-id="f0a15-175">SaveChanges 중 변경이 감지 되 면 유효성 검사가 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-175">Validation is performed after changes are detected during SaveChanges.</span></span> <span data-ttu-id="f0a15-176">유효성 검사 중 변경한 경우 변경 내용 추적기에 알리기 위해 사용자의 책임입니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-176">If you make changes during validation it is your responsibility to notify the change tracker.</span></span>
-   <span data-ttu-id="f0a15-177">DbUnexpectedValidationException은 유효성 검사 중 오류가 발생 한 경우 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-177">DbUnexpectedValidationException is thrown if errors occur during validation.</span></span>
-   <span data-ttu-id="f0a15-178">Entity Framework 모델 (필요한 최대 길이 등)에 포함 된 패싯 데이터 주석을 클래스에 없는 및/또는 EF 디자이너를 사용 하 여 모델을 만들 경우에 유효성 검사를 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are not data annotations on your classes and/or you used the EF Designer to create your model.</span></span>
-   <span data-ttu-id="f0a15-179">우선 순위 규칙:</span><span class="sxs-lookup"><span data-stu-id="f0a15-179">Precedence rules:</span></span>
    -   <span data-ttu-id="f0a15-180">Fluent API 호출에는 해당 데이터 주석을 재정의합니다</span><span class="sxs-lookup"><span data-stu-id="f0a15-180">Fluent API calls override the corresponding data annotations</span></span>
-   <span data-ttu-id="f0a15-181">실행 순서:</span><span class="sxs-lookup"><span data-stu-id="f0a15-181">Execution order:</span></span>
    -   <span data-ttu-id="f0a15-182">속성의 유효성 검사 유형 유효성 검사 전에 발생</span><span class="sxs-lookup"><span data-stu-id="f0a15-182">Property validation occurs before type validation</span></span>
    -   <span data-ttu-id="f0a15-183">유형 유효성 검사 속성 유효성 검사에 성공 하는 경우에 발생</span><span class="sxs-lookup"><span data-stu-id="f0a15-183">Type validation only occurs if property validation succeeds</span></span>
-   <span data-ttu-id="f0a15-184">속성은 복잡 한 경우 해당 유효성 검사도 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-184">If a property is complex its validation will also include:</span></span>
    -   <span data-ttu-id="f0a15-185">복합 형식 속성에 속성 수준의 유효성 검사</span><span class="sxs-lookup"><span data-stu-id="f0a15-185">Property-level validation on the complex type properties</span></span>
    -   <span data-ttu-id="f0a15-186">복합 형식에 대해 IValidatableObject 유효성 검사를 포함 하 여 복잡 한 형식에서 형식 수준 유효성 검사</span><span class="sxs-lookup"><span data-stu-id="f0a15-186">Type level validation on the complex type, including IValidatableObject validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="f0a15-187">요약</span><span class="sxs-lookup"><span data-stu-id="f0a15-187">Summary</span></span>

<span data-ttu-id="f0a15-188">Entity Framework에서 유효성 검사 API MVC의 클라이언트 쪽 유효성 검사를 사용 하 여 매끄럽게 수행 하지만 클라이언트 쪽 유효성 검사에 의존할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="f0a15-189">Entity Framework DataAnnotations 또는 code first Fluent API를 사용 하 여 적용 한 구성에 대 한 서버 쪽에서 유효성 검사 처리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="f0a15-190">수의 IValidatableObject 인터페이스를 사용 하 든 DbContet.ValidateEntity 메서드로 탭 동작을 사용자 지정 확장성 요소를 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-190">You also saw a number of extensibility points for customizing the behavior whether you use the IValidatableObject interface or tap into the DbContet.ValidateEntity method.</span></span> <span data-ttu-id="f0a15-191">및 Code First, Model First 또는 Database First 워크플로 사용 하 여 개념적 모델을 설명 하는지 여부를 유효성 검사는 이러한 마지막 두 가지 방법을 DbContext를 통해 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0a15-191">And these last two means of validation are available through the DbContext, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
