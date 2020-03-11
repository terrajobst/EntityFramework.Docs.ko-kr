---
title: 자동 Code First 마이그레이션-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: 2713afaf09707b7696e90464aac9945c2d82d274
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415702"
---
# <a name="automatic-code-first-migrations"></a><span data-ttu-id="2d0da-102">자동 Code First 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="2d0da-102">Automatic Code First Migrations</span></span>
<span data-ttu-id="2d0da-103">자동 마이그레이션을 사용 하면 각 변경 내용에 대 한 코드 파일을 프로젝트에 포함 하지 않고도 Code First 마이그레이션를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-103">Automatic Migrations allows you to use Code First Migrations without having a code file in your project for each change you make.</span></span> <span data-ttu-id="2d0da-104">모든 변경 내용을 자동으로 적용할 수 있는 것은 아닙니다. 예를 들어, 열 이름을 바꾸면 코드 기반 마이그레이션을 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-104">Not all changes can be applied automatically - for example column renames require the use of a code-based migration.</span></span>

> [!NOTE]
> <span data-ttu-id="2d0da-105">이 문서에서는 기본 시나리오에서 Code First 마이그레이션를 사용 하는 방법을 알고 있다고 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="2d0da-106">그렇지 않으면 계속 하기 전에 [Code First 마이그레이션](~/ef6/modeling/code-first/migrations/index.md) 읽어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="recommendation-for-team-environments"></a><span data-ttu-id="2d0da-107">팀 환경에 대 한 권장 사항</span><span class="sxs-lookup"><span data-stu-id="2d0da-107">Recommendation for Team Environments</span></span>

<span data-ttu-id="2d0da-108">자동 및 코드 기반 마이그레이션을 섞어서 수 있지만이는 팀 개발 시나리오에서 권장 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-108">You can intersperse automatic and code-based migrations but this is not recommended in team development scenarios.</span></span> <span data-ttu-id="2d0da-109">소스 제어를 사용 하는 개발자 팀의 일부인 경우에는 순수 하 게 자동 마이그레이션 또는 순수 코드 기반 마이그레이션을 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-109">If you are part of a team of developers that use source control you should either use purely automatic migrations or purely code-based migrations.</span></span> <span data-ttu-id="2d0da-110">자동 마이그레이션의 제한 사항을 고려 하 여 팀 환경에서 코드 기반 마이그레이션을 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-110">Given the limitations of automatic migrations we recommend using code-based migrations in team environments.</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="2d0da-111">초기 모델 및 데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="2d0da-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="2d0da-112">마이그레이션 사용을 시작하기 전에 먼저 사용할 프로젝트와 Code First 모델이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="2d0da-113">이 연습에서는 정식 **Blog** 및 **Post** 모델을 사용하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="2d0da-114">새 **MigrationsAutomaticDemo** 콘솔 응용 프로그램 만들기</span><span class="sxs-lookup"><span data-stu-id="2d0da-114">Create a new **MigrationsAutomaticDemo** Console application</span></span>
-   <span data-ttu-id="2d0da-115">최신 버전의 **EntityFramework** NuGet 패키지를 프로젝트에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="2d0da-116">**도구 –&gt; 라이브러리 패키지 관리자 –&gt; 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="2d0da-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="2d0da-117">**Install-Package EntityFramework** 명령 실행</span><span class="sxs-lookup"><span data-stu-id="2d0da-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="2d0da-118">아래 표시된 코드가 있는 **Model.cs** 파일을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="2d0da-119">이 코드에서는 도메인 모델을 구성하는 단일 **Blog** 클래스와 EF Code First 컨텍스트인 **BlogContext** 클래스를 정의합니다</span><span class="sxs-lookup"><span data-stu-id="2d0da-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsAutomaticDemo
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

-   <span data-ttu-id="2d0da-120">이제 모델이 준비되었으므로 데이터 액세스를 수행하는 데 사용할 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="2d0da-121">**Program.cs** 파일을 아래 표시된 코드로 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-121">Update the **Program.cs** file with the code shown below.</span></span>

  ``` csharp
      using System;
      using System.Collections.Generic;
      using System.Linq;
      using System.Text;

      namespace MigrationsAutomaticDemo
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

-   <span data-ttu-id="2d0da-122">응용 프로그램을 실행 하면 **MigrationsAutomaticCodeDemo 컨텍스트** 데이터베이스가 생성 되는 것을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-122">Run your application and you will see that a **MigrationsAutomaticCodeDemo.BlogContext** database is created for you.</span></span>

    ![데이터베이스 LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="2d0da-124">마이그레이션을 사용하도록 설정</span><span class="sxs-lookup"><span data-stu-id="2d0da-124">Enabling Migrations</span></span>

<span data-ttu-id="2d0da-125">이제 모델을 약간 변경할 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="2d0da-126">Blog 클래스에 Url 속성을 소개해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="2d0da-127">응용 프로그램을 다시 실행 하는 경우 *데이터베이스를 만든 후에 ' BlogContext ' 컨텍스트를 지 원하는 모델이 변경 되었다는 InvalidOperationException을 얻게 됩니다. Code First 마이그레이션를 사용 하 여 데이터베이스를 업데이트 하는 것이 좋습니다 (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *).*</span><span class="sxs-lookup"><span data-stu-id="2d0da-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="2d0da-128">예외에서 암시하듯이 이제 Code First 마이그레이션 사용을 시작할 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="2d0da-129">자동 마이그레이션을 사용 하려고 하기 때문에 **– enableautomatic** migration 스위치를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-129">Because we want to use automatic migrations we’re going to specify the **–EnableAutomaticMigrations** switch.</span></span>

-   <span data-ttu-id="2d0da-130">패키지 관리자 콘솔에서 **사용-마이그레이션-Enable자동 마이그레이션** 명령을 실행 합니다 .이 명령은 **마이그레이션** 폴더를 프로젝트에 추가 했습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-130">Run the **Enable-Migrations –EnableAutomaticMigrations** command in Package Manager Console This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="2d0da-131">이 새 폴더에는 하나의 파일이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-131">This new folder contains one file:</span></span>

-   <span data-ttu-id="2d0da-132">**Configuration 클래스입니다.**</span><span class="sxs-lookup"><span data-stu-id="2d0da-132">**The Configuration class.**</span></span> <span data-ttu-id="2d0da-133">이 클래스를 사용하면 컨텍스트에 맞게 마이그레이션이 작동하는 방식을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-133">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="2d0da-134">이 연습에서는 기본 구성만 사용하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-134">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="2d0da-135">*프로젝트에는 Code First 컨텍스트가 하나만 있으므로 Enable-Migrations는 이 구성이 적용되는 컨텍스트 유형을 자동으로 채웁니다.*</span><span class="sxs-lookup"><span data-stu-id="2d0da-135">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>

 

## <a name="your-first-automatic-migration"></a><span data-ttu-id="2d0da-136">첫 번째 자동 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="2d0da-136">Your First Automatic Migration</span></span>

<span data-ttu-id="2d0da-137">Code First 마이그레이션에는 익숙해져야 하는 다음 두 가지 기본 명령이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-137">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="2d0da-138">**Add-Migration**은 마지막 마이그레이션을 만든 후에 대한 변경 내용에 따라 다음 마이그레이션을 스캐폴드합니다</span><span class="sxs-lookup"><span data-stu-id="2d0da-138">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="2d0da-139">**Update-Database**는 보류 중인 모든 마이그레이션을 데이터베이스에 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-139">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="2d0da-140">추가 마이그레이션 (필요한 경우는 제외)을 사용 하지 않도록 하 고, 변경 내용을 자동으로 계산 하 고 적용할 Code First 마이그레이션에 집중 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-140">We are going to avoid using Add-Migration (unless we really need to) and focus on letting Code First Migrations automatically calculate and apply the changes.</span></span> <span data-ttu-id="2d0da-141">**업데이트-데이터베이스** 를 사용 하 여 모델 (새 **Blog. Ur**l 속성)의 변경 내용을 데이터베이스로 푸시하는 Code First 마이그레이션을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-141">Let’s use **Update-Database** to get Code First Migrations to push the changes to our model (the new **Blog.Ur**l property) to the database.</span></span>

-   <span data-ttu-id="2d0da-142">패키지 관리자 콘솔에서 **업데이트 데이터베이스** 명령을 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-142">Run the **Update-Database** command in Package Manager Console.</span></span>

<span data-ttu-id="2d0da-143">이제 **MigrationsAutomaticDemo 컨텍스트** 데이터베이스는 **블로그** 테이블에 **Url** 열을 포함 하도록 업데이트 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-143">The **MigrationsAutomaticDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

 

## <a name="your-second-automatic-migration"></a><span data-ttu-id="2d0da-144">두 번째 자동 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="2d0da-144">Your Second Automatic Migration</span></span>

<span data-ttu-id="2d0da-145">다른 변경을 수행 하 고 변경 내용을 데이터베이스에 자동으로 푸시 Code First 마이그레이션 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-145">Let’s make another change and let Code First Migrations automatically push the changes to the database for us.</span></span>

-   <span data-ttu-id="2d0da-146">새 **Post** 클래스도 추가하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-146">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="2d0da-147">또한 **Blog** 클래스에 **Posts** 컬렉션도 추가하여 **Blog**와 **Post** 간 관계의 다른 끝 부분을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-147">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="2d0da-148">이제 **업데이트-데이터베이스** 를 사용 하 여 데이터베이스를 최신 상태로 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-148">Now use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="2d0da-149">이번에는 Code First 마이그레이션이 실행되는 SQL을 확인할 수 있도록 **-Verbose** 플래그를 지정하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-149">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="2d0da-150">패키지 관리자 콘솔에서 **Update-Database -Verbose** 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-150">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="adding-a-code-based-migration"></a><span data-ttu-id="2d0da-151">코드 기반 마이그레이션 추가</span><span class="sxs-lookup"><span data-stu-id="2d0da-151">Adding a Code Based Migration</span></span>

<span data-ttu-id="2d0da-152">이제에 대 한 코드 기반 마이그레이션을 사용 하고자 하는 항목을 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-152">Now let’s look at something we might want to use a code-based migration for.</span></span>

-   <span data-ttu-id="2d0da-153">**블로그** 클래스에 **등급** 속성을 추가 해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-153">Let’s add a **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

<span data-ttu-id="2d0da-154">**업데이트 데이터베이스** 를 실행 하 여 이러한 변경 내용을 데이터베이스에 푸시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-154">We could just run **Update-Database** to push these changes to the database.</span></span> <span data-ttu-id="2d0da-155">그러나 null을 **허용 하지 않는**블로그를 추가 하 고 **있습니다. 평점** 열. 테이블에 기존 데이터가 있으면 새 열에 대 한 데이터 형식의 CLR 기본값 (등급은 정수)을 할당 받게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-155">However, we're adding a non-nullable **Blogs.Rating** column, if there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="2d0da-156">그러나 **Blogs** 테이블의 기존 행이 적절한 등급으로 시작되도록 기본값인 **3**을 지정하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-156">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
<span data-ttu-id="2d0da-157">추가 마이그레이션 명령을 사용 하 여이 변경 내용을 코드 기반 마이그레이션에 기록 하 여 편집할 수 있도록 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-157">Let’s use the Add-Migration command to write this change out to a code-based migration so that we can edit it.</span></span> <span data-ttu-id="2d0da-158">**마이그레이션 추가** 명령을 사용 하 여 이러한 마이그레이션의 이름을 지정할 수 있습니다. 우리는 우리가 **AddBlogRating**.</span><span class="sxs-lookup"><span data-stu-id="2d0da-158">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogRating**.</span></span>

-   <span data-ttu-id="2d0da-159">패키지 관리자 콘솔에서 **AddBlogRating 추가-마이그레이션** 명령을 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-159">Run the **Add-Migration AddBlogRating** command in Package Manager Console.</span></span>
-   <span data-ttu-id="2d0da-160">이제 마이그레이션 **폴더에** 새 **AddBlogRating** 마이그레이션이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-160">In the **Migrations** folder we now have a new **AddBlogRating** migration.</span></span> <span data-ttu-id="2d0da-161">마이그레이션 파일 이름은 순서를 지정 하는 데 도움이 되는 타임 스탬프로 미리 고정 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-161">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="2d0da-162">생성 된 코드를 편집 하 여 블로그의 기본값 3을 지정 합니다 (아래 코드에서 줄 10).</span><span class="sxs-lookup"><span data-stu-id="2d0da-162">Let’s edit the generated code to specify a default value of 3 for Blog.Rating (Line 10 in the code below)</span></span>

<span data-ttu-id="2d0da-163">*또한 마이그레이션에는 일부 메타 데이터를 캡처하는 코드 숨김이 있습니다. 이 메타 데이터를 사용 하면 Code First 마이그레이션이 코드 기반 마이그레이션 전에 수행한 자동 마이그레이션을 복제할 수 있습니다. 이는 다른 개발자가 마이그레이션을 실행 하려는 경우 나 응용 프로그램을 배포 해야 하는 경우에 중요 합니다.*</span><span class="sxs-lookup"><span data-stu-id="2d0da-163">*The migration also has a code-behind file that captures some metadata. This metadata will allow Code First Migrations to replicate the automatic migrations we performed before this code-based migration. This is important if another developer wants to run our migrations or when it’s time to deploy our application.*</span></span>

``` csharp
    namespace MigrationsAutomaticDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogRating : DbMigration
        {
            public override void Up()
            {
                AddColumn("Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropColumn("Blogs", "Rating");
            }
        }
    }
```

<span data-ttu-id="2d0da-164">편집한 마이그레이션이 좋아 보이므로 **Update-Database**를 사용하여 데이터베이스를 최신 상태로 만들겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-164">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span>

-   <span data-ttu-id="2d0da-165">패키지 관리자 콘솔에서 **업데이트 데이터베이스** 명령을 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-165">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="back-to-automatic-migrations"></a><span data-ttu-id="2d0da-166">자동 마이그레이션으로 돌아가기</span><span class="sxs-lookup"><span data-stu-id="2d0da-166">Back to Automatic Migrations</span></span>

<span data-ttu-id="2d0da-167">이제 간단한 변경 내용에 대 한 자동 마이그레이션으로 다시 전환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-167">We are now free to switch back to automatic migrations for our simpler changes.</span></span> <span data-ttu-id="2d0da-168">Code First 마이그레이션는 각 코드 기반 마이그레이션의 코드 기반 파일에 저장 되는 메타 데이터를 기반으로 자동 및 코드 기반 마이그레이션을 올바른 순서로 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-168">Code First Migrations will take care of performing the automatic and code-based migrations in the correct order based on the metadata it is storing in the code-behind file for each code-based migration.</span></span>

-   <span data-ttu-id="2d0da-169">모델에 대 한 추상 속성을 추가 해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-169">Let’s add a Post.Abstract property to our model</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="2d0da-170">이제 **업데이트-데이터베이스** 를 사용 하 여 자동 마이그레이션을 사용 하 여이 변경 내용을 데이터베이스에 푸시할 Code First 마이그레이션 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-170">Now we can use **Update-Database** to get Code First Migrations to push this change to the database using an automatic migration.</span></span>

-   <span data-ttu-id="2d0da-171">패키지 관리자 콘솔에서 **업데이트 데이터베이스** 명령을 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-171">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="summary"></a><span data-ttu-id="2d0da-172">요약</span><span class="sxs-lookup"><span data-stu-id="2d0da-172">Summary</span></span>

<span data-ttu-id="2d0da-173">이 연습에서는 자동 마이그레이션을 사용 하 여 모델 변경 사항을 데이터베이스로 푸시하는 방법에 대해 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-173">In this walkthrough you saw how to use automatic migrations to push model changes to the database.</span></span> <span data-ttu-id="2d0da-174">또한 더 많은 제어가 필요한 경우 자동 마이그레이션 사이에서 코드 기반 마이그레이션을 스 캐 폴드 하 고 실행 하는 방법도 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="2d0da-174">You also saw how to scaffold and run code-based migrations in between automatic migrations when you need more control.</span></span>
