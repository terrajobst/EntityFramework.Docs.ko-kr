---
title: Code First 마이그레이션 - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36591d8f-36e1-4835-8a51-90f34f633d1e
ms.openlocfilehash: e5a91af73bab9d45b0f1f4242ce503c6b6f407f6
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413308"
---
# <a name="code-first-migrations"></a><span data-ttu-id="b7c83-102">Code First 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="b7c83-102">Code First Migrations</span></span>
<span data-ttu-id="b7c83-103">Code First 워크플로를 사용하는 경우 Code First 마이그레이션은 애플리케이션의 데이터베이스 스키마를 향상시키는 데 권장되는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-103">Code First Migrations is the recommended way to evolve your application's database schema if you are using the Code First workflow.</span></span> <span data-ttu-id="b7c83-104">마이그레이션에서 제공하는 도구 집합이 허용하는 작업은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-104">Migrations provide a set of tools that allow:</span></span>

1. <span data-ttu-id="b7c83-105">EF 모델을 사용하는 초기 데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="b7c83-105">Create an initial database that works with your EF model</span></span>
2. <span data-ttu-id="b7c83-106">EF 모델에 대한 변경 내용을 추적하기 위한 마이그레이션 생성</span><span class="sxs-lookup"><span data-stu-id="b7c83-106">Generating migrations to keep track of changes you make to your EF model</span></span>
2. <span data-ttu-id="b7c83-107">이러한 변경 내용으로 데이터베이스를 최신 상태로 유지</span><span class="sxs-lookup"><span data-stu-id="b7c83-107">Keep your database up to date with those changes</span></span>

<span data-ttu-id="b7c83-108">다음 연습에서는 Entity Framework의 Code First 마이그레이션에 대해 간략히 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-108">The following walkthrough will provide an overview Code First Migrations in Entity Framework.</span></span> <span data-ttu-id="b7c83-109">연습 전체를 수행하거나 관심 있는 주제로 건너뛸 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-109">You can either complete the entire walkthrough or skip to the topic you are interested in.</span></span> <span data-ttu-id="b7c83-110">다음 항목을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-110">The following topics are covered:</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="b7c83-111">초기 모델 및 데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="b7c83-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="b7c83-112">마이그레이션 사용을 시작하기 전에 먼저 사용할 프로젝트와 Code First 모델이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="b7c83-113">이 연습에서는 정식 **Blog** 및 **Post** 모델을 사용하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="b7c83-114">새 **MigrationsDemo** 콘솔 애플리케이션을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-114">Create a new **MigrationsDemo** Console application</span></span>
-   <span data-ttu-id="b7c83-115">최신 버전의 **EntityFramework** NuGet 패키지를 프로젝트에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="b7c83-116">**도구 –&gt; 라이브러리 패키지 관리자 –&gt; 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="b7c83-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="b7c83-117">**Install-Package EntityFramework** 명령 실행</span><span class="sxs-lookup"><span data-stu-id="b7c83-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="b7c83-118">아래 표시된 코드가 있는 **Model.cs** 파일을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="b7c83-119">이 코드에서는 도메인 모델을 구성하는 단일 **Blog** 클래스와 EF Code First 컨텍스트인 **BlogContext** 클래스를 정의합니다</span><span class="sxs-lookup"><span data-stu-id="b7c83-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsDemo
      {
          public class BlogContext : DbContext
          {
              public DbSet<Blog> Blogs { get; set; }
          }

          public class Blog
          {
              public int BlogId { get; set; }
              public string Name { get; set; }
          }
      }
  ```

-   <span data-ttu-id="b7c83-120">이제 모델이 준비되었으므로 데이터 액세스를 수행하는 데 사용할 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="b7c83-121">**Program.cs** 파일을 아래 표시된 코드로 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-121">Update the **Program.cs** file with the code shown below.</span></span>

  ``` csharp
      using System;
      using System.Collections.Generic;
      using System.Linq;
      using System.Text;

      namespace MigrationsDemo
      {
          class Program
          {
              static void Main(string[] args)
              {
                  using (var db = new BlogContext())
                  {
                      db.Blogs.Add(new Blog { Name = "Another Blog " });
                      db.SaveChanges();

                      foreach (var blog in db.Blogs)
                      {
                          Console.WriteLine(blog.Name);
                      }
                  }

                  Console.WriteLine("Press any key to exit...");
                  Console.ReadKey();
              }
          }
      }
  ```

-   <span data-ttu-id="b7c83-122">애플리케이션을 실행하면 **MigrationsCodeDemo.BlogContext** 데이터베이스가 만들어졌음을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-122">Run your application and you will see that a **MigrationsCodeDemo.BlogContext** database is created for you.</span></span>

    ![데이터베이스 LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="b7c83-124">마이그레이션을 사용하도록 설정</span><span class="sxs-lookup"><span data-stu-id="b7c83-124">Enabling Migrations</span></span>

<span data-ttu-id="b7c83-125">이제 모델을 약간 변경할 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="b7c83-126">Blog 클래스에 Url 속성을 소개해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="b7c83-127">애플리케이션을 다시 실행하는 경우 InvalidOperationException이 발생합니다. 이 예외에서는 *'BlogContext' 컨텍스트를 지원하는 모델이 변경되었습니다. Code First 마이그레이션을 사용하여 데이터베이스를 업데이트하는 것이 좋습니다.’라고 나타냅니다( [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *).**</span><span class="sxs-lookup"><span data-stu-id="b7c83-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="b7c83-128">예외에서 암시하듯이 이제 Code First 마이그레이션 사용을 시작할 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="b7c83-129">첫 번째 단계는 컨텍스트에 맞게 마이그레이션을 사용하도록 설정하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-129">The first step is to enable migrations for our context.</span></span>

-   <span data-ttu-id="b7c83-130">패키지 관리자 콘솔에서 **Enable-Migrations** 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-130">Run the **Enable-Migrations** command in Package Manager Console</span></span>

    <span data-ttu-id="b7c83-131">이 명령은 프로젝트에 **Migrations** 폴더를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-131">This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="b7c83-132">이 새 폴더에는 두 개의 파일이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-132">This new folder contains two files:</span></span>

-   <span data-ttu-id="b7c83-133">**Configuration 클래스입니다.**</span><span class="sxs-lookup"><span data-stu-id="b7c83-133">**The Configuration class.**</span></span> <span data-ttu-id="b7c83-134">이 클래스를 사용하면 컨텍스트에 맞게 마이그레이션이 작동하는 방식을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-134">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="b7c83-135">이 연습에서는 기본 구성만 사용하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-135">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="b7c83-136">*프로젝트에는 Code First 컨텍스트가 하나만 있으므로 Enable-Migrations는 이 구성이 적용되는 컨텍스트 유형을 자동으로 채웁니다.*</span><span class="sxs-lookup"><span data-stu-id="b7c83-136">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>
-   <span data-ttu-id="b7c83-137">**InitialCreate 마이그레이션**입니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-137">**An InitialCreate migration**.</span></span> <span data-ttu-id="b7c83-138">이 마이그레이션은 마이그레이션을 사용하도록 설정하기 전에 Code First를 통해 데이터베이스를 만들었기 때문에 생성되었습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-138">This migration was generated because we already had Code First create a database for us, before we enabled migrations.</span></span> <span data-ttu-id="b7c83-139">이 스캐폴드된 마이그레이션의 코드는 데이터베이스에 이미 만들어진 개체를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-139">The code in this scaffolded migration represents the objects that have already been created in the database.</span></span> <span data-ttu-id="b7c83-140">여기서는 **BlogId** 및 **Name** 열이 있는 **Blog** 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-140">In our case that is the **Blog** table with a **BlogId** and **Name** columns.</span></span> <span data-ttu-id="b7c83-141">파일 이름에는 순서를 지정하는 데 도움이 되는 타임스탬프가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-141">The filename includes a timestamp to help with ordering.</span></span>
    <span data-ttu-id="b7c83-142">*데이터베이스를 아직 만들지 않았으면 이 InitialCreate 마이그레이션이 프로젝트에 추가되지 않습니다. 대신 처음으로 Add-Migration을 호출할 때 이러한 테이블을 만드는 코드가 새 마이그레이션에 스캐폴드됩니다.*</span><span class="sxs-lookup"><span data-stu-id="b7c83-142">*If the database had not already been created this InitialCreate migration would not have been added to the project. Instead, the first time we call Add-Migration the code to create these tables would be scaffolded to a new migration.*</span></span>

### <a name="multiple-models-targeting-the-same-database"></a><span data-ttu-id="b7c83-143">동일한 데이터베이스를 대상으로 하는 여러 모델</span><span class="sxs-lookup"><span data-stu-id="b7c83-143">Multiple Models Targeting the Same Database</span></span>

<span data-ttu-id="b7c83-144">EF6 이전 버전을 사용하는 경우 하나의 Code First 모델만 데이터베이스의 스키마를 생성/관리하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-144">When using versions prior to EF6, only one Code First model could be used to generate/manage the schema of a database.</span></span> <span data-ttu-id="b7c83-145">이는 데이터베이스당 하나의 **\_\_MigrationsHistory** 테이블로 인해 어떤 항목이 어떤 모델에 속하는지 식별할 수 있는 방법이 없기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-145">This is the result of a single **\_\_MigrationsHistory** table per database with no way to identify which entries belong to which model.</span></span>

<span data-ttu-id="b7c83-146">EF6부터 **Configuration** 클래스에는 **ContextKey** 속성이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-146">Starting with EF6, the **Configuration** class includes a **ContextKey** property.</span></span> <span data-ttu-id="b7c83-147">이는 각 Code First 모델에 대한 고유 식별자로 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-147">This acts as a unique identifier for each Code First model.</span></span> <span data-ttu-id="b7c83-148">**\_\_MigrationsHistory** 테이블의 해당 열을 사용하면 여러 모델의 항목을 사용하여 테이블을 공유할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-148">A corresponding column in the **\_\_MigrationsHistory** table allows entries from multiple models to share the table.</span></span> <span data-ttu-id="b7c83-149">기본적으로 이 속성은 컨텍스트의 정규화된 이름으로 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-149">By default, this property is set to the fully qualified name of your context.</span></span>

## <a name="generating--running-migrations"></a><span data-ttu-id="b7c83-150">마이그레이션 생성 및 실행</span><span class="sxs-lookup"><span data-stu-id="b7c83-150">Generating & Running Migrations</span></span>

<span data-ttu-id="b7c83-151">Code First 마이그레이션에는 익숙해져야 하는 다음 두 가지 기본 명령이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-151">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="b7c83-152">**Add-Migration**은 마지막 마이그레이션을 만든 후에 대한 변경 내용에 따라 다음 마이그레이션을 스캐폴드합니다</span><span class="sxs-lookup"><span data-stu-id="b7c83-152">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="b7c83-153">**Update-Database**는 보류 중인 모든 마이그레이션을 데이터베이스에 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-153">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="b7c83-154">추가한 새 Url 속성을 처리하기 위해 마이그레이션을 스캐폴드해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-154">We need to scaffold a migration to take care of the new Url property we have added.</span></span> <span data-ttu-id="b7c83-155">**Add-Migration** 명령을 사용하면 이러한 마이그레이션에 이름을 지정할 수 있습니다. 여기서는 **AddBlogUrl**로 지정하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-155">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogUrl**.</span></span>

-   <span data-ttu-id="b7c83-156">패키지 관리자 콘솔에서 **Add-Migration AddBlogUrl** 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-156">Run the **Add-Migration AddBlogUrl** command in Package Manager Console</span></span>
-   <span data-ttu-id="b7c83-157">이제 **Migrations** 폴더에 새 **AddBlogUrl** 마이그레이션이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-157">In the **Migrations** folder we now have a new **AddBlogUrl** migration.</span></span> <span data-ttu-id="b7c83-158">마이그레이션 파일 이름은 순서를 지정하는 데 도움이 되는 타임스탬프로 미리 확정됩니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-158">The migration filename is pre-fixed with a timestamp to help with ordering</span></span>

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogUrl : DbMigration
        {
            public override void Up()
            {
                AddColumn("dbo.Blogs", "Url", c => c.String());
            }

            public override void Down()
            {
                DropColumn("dbo.Blogs", "Url");
            }
        }
    }
```

<span data-ttu-id="b7c83-159">이제 이 마이그레이션을 편집하거나 추가할 수 있지만, 현재로서는 모든 것이 매우 좋아 보입니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-159">We could now edit or add to this migration but everything looks pretty good.</span></span> <span data-ttu-id="b7c83-160">**Update-Database**를 사용하여 이 마이그레이션을 데이터베이스에 적용하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-160">Let’s use **Update-Database** to apply this migration to the database.</span></span>

-   <span data-ttu-id="b7c83-161">패키지 관리자 콘솔에서 **Update-Database** 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-161">Run the **Update-Database** command in Package Manager Console</span></span>
-   <span data-ttu-id="b7c83-162">Code First 마이그레이션에서는 **Migrations** 폴더의 마이그레이션과 데이터베이스에 적용된 마이그레이션을 비교합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-162">Code First Migrations will compare the migrations in our **Migrations** folder with the ones that have been applied to the database.</span></span> <span data-ttu-id="b7c83-163">**AddBlogUrl** 마이그레이션을 적용해야 함을 알 수 있으므로 이를 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-163">It will see that the **AddBlogUrl** migration needs to be applied, and run it.</span></span>

<span data-ttu-id="b7c83-164">이제 **MigrationsDemo.BlogContext** 데이터베이스가 업데이트되어 **Blogs** 테이블에 **Url** 열이 포함되었습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-164">The **MigrationsDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

## <a name="customizing-migrations"></a><span data-ttu-id="b7c83-165">마이그레이션 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="b7c83-165">Customizing Migrations</span></span>

<span data-ttu-id="b7c83-166">지금까지 마이그레이션을 생성하고 변경하지 않은 채 그대로 실행했습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-166">So far we’ve generated and run a migration without making any changes.</span></span> <span data-ttu-id="b7c83-167">이제 기본적으로 생성되는 코드를 편집해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-167">Now let’s look at editing the code that gets generated by default.</span></span>

-   <span data-ttu-id="b7c83-168">모델을 약간 변경할 시간입니다. **Blog** 클래스에 **Rating** 속성을 추가하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-168">It’s time to make some more changes to our model, let’s add a new **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

-   <span data-ttu-id="b7c83-169">새 **Post** 클래스도 추가하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-169">Let's also add a new **Post** class</span></span>

``` csharp
    public class Post
    {
        public int PostId { get; set; }
        [MaxLength(200)]
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
```

-   <span data-ttu-id="b7c83-170">또한 **Blog** 클래스에 **Posts** 컬렉션도 추가하여 **Blog**와 **Post** 간 관계의 다른 끝 부분을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-170">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="b7c83-171">**Add-Migration** 명령을 사용하여 마이그레이션 시 Code First 마이그레이션에서 가장 적합한 마이그레이션을 추측하도록 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-171">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span> <span data-ttu-id="b7c83-172">여기서는 이 마이그레이션을 **AddPostClass**라고 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-172">We’re going to call this migration **AddPostClass**.</span></span>

-   <span data-ttu-id="b7c83-173">패키지 관리자 콘솔에서 **Add-Migration AddPostClass** 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-173">Run the **Add-Migration AddPostClass** command in Package Manager Console.</span></span>

<span data-ttu-id="b7c83-174">Code First 마이그레이션은 이러한 변경을 매우 효과적으로 스캐폴드했지만 변경하려는 몇 가지 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-174">Code First Migrations did a pretty good job of scaffolding these changes, but there are some things we might want to change:</span></span>

1.  <span data-ttu-id="b7c83-175">먼저 **Posts.Title** 열에 고유 인덱스를 추가하겠습니다(아래 코드의 22 및 29번째 줄에 추가).</span><span class="sxs-lookup"><span data-stu-id="b7c83-175">First up, let’s add a unique index to **Posts.Title** column (Adding in line 22 & 29 in the code below).</span></span>
2.  <span data-ttu-id="b7c83-176">null 허용 **Blogs.Rating** 열도 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-176">We’re also adding a non-nullable **Blogs.Rating** column.</span></span> <span data-ttu-id="b7c83-177">테이블에 기존 데이터가 있는 경우 새 열에 대한 데이터 형식의 CLR 기본값(Rating은 정수이므로 **0**임)이 할당됩니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-177">If there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="b7c83-178">그러나 **Blogs** 테이블의 기존 행이 적절한 등급으로 시작되도록 기본값인 **3**을 지정하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-178">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
    <span data-ttu-id="b7c83-179">(아래 코드의 24번째 줄에 기본값이 지정되어 있습니다.)</span><span class="sxs-lookup"><span data-stu-id="b7c83-179">(You can see the default value specified on line 24 of the code below)</span></span>

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddPostClass : DbMigration
        {
            public override void Up()
            {
                CreateTable(
                    "dbo.Posts",
                    c => new
                        {
                            PostId = c.Int(nullable: false, identity: true),
                            Title = c.String(maxLength: 200),
                            Content = c.String(),
                            BlogId = c.Int(nullable: false),
                        })
                    .PrimaryKey(t => t.PostId)
                    .ForeignKey("dbo.Blogs", t => t.BlogId, cascadeDelete: true)
                    .Index(t => t.BlogId)
                    .Index(p => p.Title, unique: true);

                AddColumn("dbo.Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropIndex("dbo.Posts", new[] { "Title" });
                DropIndex("dbo.Posts", new[] { "BlogId" });
                DropForeignKey("dbo.Posts", "BlogId", "dbo.Blogs");
                DropColumn("dbo.Blogs", "Rating");
                DropTable("dbo.Posts");
            }
        }
    }
```

<span data-ttu-id="b7c83-180">편집한 마이그레이션이 준비되었으므로 **Update-Database**를 사용하여 데이터베이스를 최신 상태로 만들겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-180">Our edited migration is ready to go, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="b7c83-181">이번에는 Code First 마이그레이션이 실행되는 SQL을 확인할 수 있도록 **-Verbose** 플래그를 지정하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-181">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="b7c83-182">패키지 관리자 콘솔에서 **Update-Database -Verbose** 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-182">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="data-motion--custom-sql"></a><span data-ttu-id="b7c83-183">데이터 이동/사용자 지정 SQL</span><span class="sxs-lookup"><span data-stu-id="b7c83-183">Data Motion / Custom SQL</span></span>

<span data-ttu-id="b7c83-184">지금까지 데이터를 변경하거나 이동하지 않는 마이그레이션 작업을 살펴보았으며, 이제는 일부 데이터를 이동해야 하는 작업을 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-184">So far we have looked at migration operations that don’t change or move any data, now let’s look at something that needs to move some data around.</span></span> <span data-ttu-id="b7c83-185">데이터 이동은 아직 기본적으로 지원하지 없지만, 스크립트의 어느 지점에서든 임의의 몇 가지 SQL 명령을 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-185">There is no native support for data motion yet, but we can run some arbitrary SQL commands at any point in our script.</span></span>

-   <span data-ttu-id="b7c83-186">**Post.Abstract** 속성을 모델에 추가하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-186">Let’s add a **Post.Abstract** property to our model.</span></span> <span data-ttu-id="b7c83-187">나중에 **Content** 열의 시작 부분에 있는 일부 텍스트를 사용하여 기존 게시에 대한 **Abstract**를 미리 채우게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-187">Later, we’re going to pre-populate the **Abstract** for existing posts using some text from the start of the **Content** column.</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="b7c83-188">**Add-Migration** 명령을 사용하여 마이그레이션 시 Code First 마이그레이션에서 가장 적합한 마이그레이션을 추측하도록 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-188">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span>

-   <span data-ttu-id="b7c83-189">패키지 관리자 콘솔에서 **Add-Migration AddPostAbstract** 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-189">Run the **Add-Migration AddPostAbstract** command in Package Manager Console.</span></span>
-   <span data-ttu-id="b7c83-190">생성된 마이그레이션은 스키마 변경을 처리하지만, 각 게시의 처음 100개 문자를 사용하여 **Abstract** 열을 미리 채울 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-190">The generated migration takes care of the schema changes but we also want to pre-populate the **Abstract** column using the first 100 characters of content for each post.</span></span> <span data-ttu-id="b7c83-191">이렇게 하려면 열이 추가된 후 SQL로 드롭다운하여 **UPDATE** 문을 실행하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-191">We can do this by dropping down to SQL and running an **UPDATE** statement after the column is added.</span></span>
    <span data-ttu-id="b7c83-192">(아래 코드의 12번째 줄에 추가)</span><span class="sxs-lookup"><span data-stu-id="b7c83-192">(Adding in line 12 in the code below)</span></span>

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddPostAbstract : DbMigration
        {
            public override void Up()
            {
                AddColumn("dbo.Posts", "Abstract", c => c.String());

                Sql("UPDATE dbo.Posts SET Abstract = LEFT(Content, 100) WHERE Abstract IS NULL");
            }

            public override void Down()
            {
                DropColumn("dbo.Posts", "Abstract");
            }
        }
    }
```

<span data-ttu-id="b7c83-193">편집한 마이그레이션이 좋아 보이므로 **Update-Database**를 사용하여 데이터베이스를 최신 상태로 만들겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-193">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="b7c83-194">데이터베이스에 대해 실행되는 SQL을 볼 수 있도록 **-Verbose** 플래그를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-194">We’ll specify the **–Verbose** flag so that we can see the SQL being run against the database.</span></span>

-   <span data-ttu-id="b7c83-195">패키지 관리자 콘솔에서 **Update-Database -Verbose** 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-195">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="migrate-to-a-specific-version-including-downgrade"></a><span data-ttu-id="b7c83-196">특정 버전으로 마이그레이션(다운그레이드 포함)</span><span class="sxs-lookup"><span data-stu-id="b7c83-196">Migrate to a Specific Version (Including Downgrade)</span></span>

<span data-ttu-id="b7c83-197">지금까지는 항상 최신 마이그레이션으로 업그레이드했지만, 특정 마이그레이션으로 업그레이드/다운그레이드하려는 경우가 있을 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-197">So far we have always upgraded to the latest migration, but there may be times when you want upgrade/downgrade to a specific migration.</span></span>

<span data-ttu-id="b7c83-198">**AddBlogUrl** 마이그레이션을 실행한 이후의 상태로 데이터베이스를 마이그레이션하려고 한다고 가정해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-198">Let’s say we want to migrate our database to the state it was in after running our **AddBlogUrl** migration.</span></span> <span data-ttu-id="b7c83-199">**-TargetMigration** 스위치를 사용하여 이 마이그레이션으로 다운그레이드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-199">We can use the **–TargetMigration** switch to downgrade to this migration.</span></span>

-   <span data-ttu-id="b7c83-200">패키지 관리자 콘솔에서 **Update-Database–TargetMigration: AddBlogUrl** 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-200">Run the **Update-Database –TargetMigration: AddBlogUrl** command in Package Manager Console.</span></span>

<span data-ttu-id="b7c83-201">이 명령은 **AddBlogAbstract** 및 **AddPostClass** 마이그레이션에 대한 Down(다운그레이드) 스크립트를 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-201">This command will run the Down script for our **AddBlogAbstract** and **AddPostClass** migrations.</span></span>

<span data-ttu-id="b7c83-202">빈 데이터베이스로 완전히 다시 롤백하려면 **Update-Database –TargetMigration: $InitialDatabase** 명령을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-202">If you want to roll all the way back to an empty database then you can use the **Update-Database –TargetMigration: $InitialDatabase** command.</span></span>

## <a name="getting-a-sql-script"></a><span data-ttu-id="b7c83-203">SQL 스크립트 가져오기</span><span class="sxs-lookup"><span data-stu-id="b7c83-203">Getting a SQL Script</span></span>

<span data-ttu-id="b7c83-204">다른 개발자가 컴퓨터에 대해 이러한 변경이 필요한 경우 변경 내용이 원본 제어에서 확인되면 한 번만 동기화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-204">If another developer wants these changes on their machine they can just sync once we check our changes into source control.</span></span> <span data-ttu-id="b7c83-205">새 마이그레이션이 적용되면 Update-Database 명령을 실행하여 변경 내용을 로컬에 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-205">Once they have our new migrations they can just run the Update-Database command to have the changes applied locally.</span></span> <span data-ttu-id="b7c83-206">그러나 이러한 변경 내용을 테스트 서버 및 최종적으로 프로덕션 환경에 적용하려면 DBA에게 전달할 수 있는 SQL 스크립트가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-206">However if we want to push these changes out to a test server, and eventually production, we probably want a SQL script we can hand off to our DBA.</span></span>

-   <span data-ttu-id="b7c83-207">**Update-Database** 명령을 실행하지만, 이번에는 변경 내용이 적용되지 않고 스크립트로 작성되도록 **–Script** 플래그를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-207">Run the **Update-Database** command but this time specify the **–Script** flag so that changes are written to a script rather than applied.</span></span> <span data-ttu-id="b7c83-208">또한 스크립트를 생성하기 위해 원본 및 대상 마이그레이션을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-208">We’ll also specify a source and target migration to generate the script for.</span></span> <span data-ttu-id="b7c83-209">빈 데이터베이스( **$InitialDatabase**)에서 최신 버전(**AddPostAbstract** 마이그레이션)으로 이동하는 스크립트를 만들겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-209">We want a script to go from an empty database (**$InitialDatabase**) to the latest version (migration **AddPostAbstract**).</span></span>
    <span data-ttu-id="b7c83-210">*대상 마이그레이션을 지정하지 않으면 마이그레이션에서 최신 마이그레이션을 대상으로 사용합니다. 원본 마이그레이션을 지정하지 않으면 마이그레이션에서 데이터베이스의 현재 상태를 사용합니다.*</span><span class="sxs-lookup"><span data-stu-id="b7c83-210">*If you don’t specify a target migration, Migrations will use the latest migration as the target. If you don't specify a source migrations, Migrations will use the current state of the database.*</span></span>
-   <span data-ttu-id="b7c83-211">패키지 관리자 콘솔에서 **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-211">Run the **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** command in Package Manager Console</span></span>

<span data-ttu-id="b7c83-212">Code First 마이그레이션은 마이그레이션 파이프라인을 실행하지만, 실제로 변경 내용을 적용하는 대신 .sql 파일에 변경 내용을 기록합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-212">Code First Migrations will run the migration pipeline but instead of actually applying the changes it will write them out to a .sql file for you.</span></span> <span data-ttu-id="b7c83-213">스크립트가 생성되면 Visual Studio에서 열어서 보거나 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-213">Once the script is generated, it is opened for you in Visual Studio, ready for you to view or save.</span></span>

### <a name="generating-idempotent-scripts"></a><span data-ttu-id="b7c83-214">idempotent 스크립트 생성</span><span class="sxs-lookup"><span data-stu-id="b7c83-214">Generating Idempotent Scripts</span></span>

<span data-ttu-id="b7c83-215">EF6부터 **–SourceMigration $InitialDatabase**를 지정하면 생성되는 스크립트가 'idempotent'(멱등원)가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-215">Starting with EF6, if you specify **–SourceMigration $InitialDatabase** then the generated script will be ‘idempotent’.</span></span> <span data-ttu-id="b7c83-216">현재 idempotent 스크립트는 모든 버전의 데이터베이스를 최신 버전(또는 **–TargetMigration**을 사용하는 경우 지정한 버전)으로 업그레이드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-216">Idempotent scripts can upgrade a database currently at any version to the latest version (or the specified version if you use **–TargetMigration**).</span></span> <span data-ttu-id="b7c83-217">생성된 스크립트에는 **\_\_MigrationsHistory** 테이블을 확인하고 이전에 적용되지 않은 변경 내용만 적용하는 논리가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-217">The generated script includes logic to check the **\_\_MigrationsHistory** table and only apply changes that haven't been previously applied.</span></span>

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a><span data-ttu-id="b7c83-218">애플리케이션 시작 시 자동 업그레이드(MigrateDatabaseToLatestVersion 이니셜라이저)</span><span class="sxs-lookup"><span data-stu-id="b7c83-218">Automatically Upgrading on Application Startup (MigrateDatabaseToLatestVersion Initializer)</span></span>

<span data-ttu-id="b7c83-219">애플리케이션을 배포하는 경우 애플리케이션이 시작될 때 데이터베이스를 자동으로 업그레이드할 수 있습니다(보류 중인 마이그레이션 적용).</span><span class="sxs-lookup"><span data-stu-id="b7c83-219">If you are deploying your application you may want it to automatically upgrade the database (by applying any pending migrations) when the application launches.</span></span> <span data-ttu-id="b7c83-220">이렇게 하려면 **MigrateDatabaseToLatestVersion** 데이터베이스 이니셜라이저를 등록하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-220">You can do this by registering the **MigrateDatabaseToLatestVersion** database initializer.</span></span> <span data-ttu-id="b7c83-221">데이터베이스 이니셜라이저에는 데이터베이스가 올바르게 설정되었는지 확인하는 데 사용되는 몇 가지 논리가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-221">A database initializer simply contains some logic that is used to make sure the database is setup correctly.</span></span> <span data-ttu-id="b7c83-222">이 논리는 애플리케이션 프로세스(**AppDomain**) 내에서 컨텍스트가 처음 사용될 때 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-222">This logic is run the first time the context is used within the application process (**AppDomain**).</span></span>

<span data-ttu-id="b7c83-223">컨텍스트(14번째 줄)를 사용하기 전에 BlogContext에 대한 **MigrateDatabaseToLatestVersion** 이니셜라이저를 설정하기 위해 **Program.cs** 파일을 아래와 같이 업데이트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-223">We can update the **Program.cs** file, as shown below, to set the **MigrateDatabaseToLatestVersion** initializer for BlogContext before we use the context (Line 14).</span></span> <span data-ttu-id="b7c83-224">또한 **System.Data.Entity** 네임스페이스(5번째 줄)에 using 문도 추가해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-224">Note that you also need to add a using statement for the **System.Data.Entity** namespace (Line 5).</span></span>

<span data-ttu-id="b7c83-225">*이 이니셜라이저의 인스턴스를 만들 때 컨텍스트 유형(**BlogContext**)과 마이그레이션 구성(**Configuration**)을 지정해야 합니다. 마이그레이션 구성은 마이그레이션을 사용하도록 설정할 때 **Migrations** 폴더에 추가된 클래스입니다*</span><span class="sxs-lookup"><span data-stu-id="b7c83-225">*When we create an instance of this initializer we need to specify the context type (**BlogContext**) and the migrations configuration (**Configuration**) - the migrations configuration is the class that got added to our **Migrations** folder when we enabled Migrations.*</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Data.Entity;
    using MigrationsDemo.Migrations;

    namespace MigrationsDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                Database.SetInitializer(new MigrateDatabaseToLatestVersion<BlogContext, Configuration>());

                using (var db = new BlogContext())
                {
                    db.Blogs.Add(new Blog { Name = "Another Blog " });
                    db.SaveChanges();

                    foreach (var blog in db.Blogs)
                    {
                        Console.WriteLine(blog.Name);
                    }
                }

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }
        }
    }
```

<span data-ttu-id="b7c83-226">이제 애플리케이션이 실행될 때마다 먼저 대상 데이터베이스가 최신 상태인지 확인하고, 그렇지 않으면 보류 중인 마이그레이션을 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="b7c83-226">Now whenever our application runs it will first check if the database it is targeting is up-to-date, and apply any pending migrations if it is not.</span></span>
