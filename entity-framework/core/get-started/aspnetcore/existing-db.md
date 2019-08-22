---
title: ASP.NET Core에서 시작 - 기존 데이터베이스 - EF Core
author: rowanmiller
description: ASP.NET Core에서 기존 데이터베이스로 EF Core 시작
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: eeebd75bebe85994c6439e06243e113f2bda814c
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565233"
---
# <a name="get-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="69d32-103">ASP.NET Core에서 기존 데이터베이스로 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="69d32-103">Get started with EF Core on ASP.NET Core with an existing database</span></span>

<span data-ttu-id="69d32-104">이 자습서에서는 Entity Framework Core를 사용하여 기본 데이터 액세스를 수행하는 ASP.NET Core MVC 애플리케이션을 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-104">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="69d32-105">기존 데이터베이스를 리버스 엔지니어링하여 Entity Framework 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-105">You reverse engineer an existing database to create an Entity Framework model.</span></span>

<span data-ttu-id="69d32-106">[GitHub에서 이 아티클의 샘플을 봅니다](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="69d32-106">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="69d32-107">전제 조건</span><span class="sxs-lookup"><span data-stu-id="69d32-107">Prerequisites</span></span>

<span data-ttu-id="69d32-108">다음 소프트웨어를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-108">Install the following software:</span></span>

* <span data-ttu-id="69d32-109">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) 및 다음 워크로드:</span><span class="sxs-lookup"><span data-stu-id="69d32-109">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="69d32-110">**ASP.NET 및 웹 개발**(**웹 및 클라우드** 아래)</span><span class="sxs-lookup"><span data-stu-id="69d32-110">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="69d32-111">**.NET Core 플랫폼 간 개발**(**기타 도구 집합** 아래)</span><span class="sxs-lookup"><span data-stu-id="69d32-111">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="69d32-112">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="69d32-112">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-blogging-database"></a><span data-ttu-id="69d32-113">Blogging 데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="69d32-113">Create Blogging database</span></span>

<span data-ttu-id="69d32-114">이 자습서에서는 LocalDb 인스턴스의 **Blogging** 데이터베이스를 기존 데이터베이스로 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span> <span data-ttu-id="69d32-115">**이미 다른 자습서의 일부로 Blogging** 데이터베이스를 만든 경우 다음 단계를 건너뜁니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-115">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="69d32-116">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-116">Open Visual Studio</span></span>
* <span data-ttu-id="69d32-117">**도구 -> 데이터베이스에 연결...**</span><span class="sxs-lookup"><span data-stu-id="69d32-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="69d32-118">**Microsoft SQL Server**를 선택하고 **계속**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="69d32-119">**서버 이름**으로 **(localdb)\mssqllocaldb**를 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="69d32-120">**데이터베이스 이름**으로 **master**를 입력하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="69d32-121">이제 master 데이터베이스가 **서버 탐색기**의 **데이터 연결**에 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="69d32-122">**서버 탐색기**에서 데이터베이스를 마우스 오른쪽 단추로 클릭하고 **새 쿼리**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="69d32-123">아래 나열된 스크립트를 쿼리 편집기로 복사합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-123">Copy the script listed below into the query editor</span></span>
* <span data-ttu-id="69d32-124">쿼리 편집기를 마우스 오른쪽 단추로 클릭하고 **실행**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="69d32-125">새 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="69d32-125">Create a new project</span></span>

* <span data-ttu-id="69d32-126">Visual Studio 2017을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="69d32-127">**파일 > 새로 만들기 > 프로젝트...**</span><span class="sxs-lookup"><span data-stu-id="69d32-127">**File > New > Project...**</span></span>
* <span data-ttu-id="69d32-128">왼쪽 메뉴에서 **설치됨 > Visual C# > 웹**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-128">From the left menu select **Installed > Visual C# > Web**</span></span>
* <span data-ttu-id="69d32-129">**ASP.NET Core 웹 애플리케이션** 프로젝트 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-129">Select the **ASP.NET Core Web Application** project template</span></span>
* <span data-ttu-id="69d32-130">**EFGetStarted.AspNetCore.ExistingDb**를 이름으로 입력(나중에 코드에서 사용하는 네임스페이스와 정확히 일치해야 함)하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name (it has to match exactly the namespace later used in the code) and click **OK**</span></span> 
* <span data-ttu-id="69d32-131">**새 ASP.NET Core 웹 애플리케이션** 대화 상자가 표시될 때까지 기다립니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="69d32-132">대상 프레임워크 드롭다운이 **.NET Core**로 설정되고 버전 드롭다운이 **ASP.NET Core 2.1**로 설정되었는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-132">Make sure that the target framework dropdown is set to **.NET Core**, and the version dropdown is set to **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="69d32-133">**웹 애플리케이션(모델-뷰-컨트롤러)** 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-133">Select the **Web Application (Model-View-Controller)** template</span></span>
* <span data-ttu-id="69d32-134">**인증**이 **인증 없음**으로 설정되었는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-134">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="69d32-135">**확인** 을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-135">Click **OK**</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="69d32-136">Entity Framework Core 설치</span><span class="sxs-lookup"><span data-stu-id="69d32-136">Install Entity Framework Core</span></span>

<span data-ttu-id="69d32-137">EF Core를 설치하려면 대상으로 지정할 EF Core 데이터베이스 공급자에 대한 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="69d32-138">사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="69d32-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="69d32-139">이 자습서에서는 SQL Server를 사용하기 때문에 공급자 패키지를 설치할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-139">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="69d32-140">SQL Server 공급자 패키지는 [Microsoft.AspnetCore.App 메타패키지](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1)에 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-140">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="69d32-141">모델 리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="69d32-141">Reverse engineer your model</span></span>

<span data-ttu-id="69d32-142">이제 기존 데이터베이스를 기반으로 EF 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-142">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="69d32-143">**도구 -> NuGet 패키지 관리자 -> 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="69d32-143">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="69d32-144">다음 명령을 실행하여 기존 데이터베이스에서 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-144">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="69d32-145">`The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`이라는 오류가 표시되면 Visual Studio를 닫았다가 다시 엽니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-145">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="69d32-146">위의 명령에 `-Tables` 인수를 추가하여 엔터티를 생성할 테이블을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-146">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="69d32-147">예를 들어, `-Tables Blog,Post`을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-147">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="69d32-148">리버스 엔지니어링 프로세스에서는 기존 데이터베이스의 스키마를 기반으로 엔터티 클래스(`Blog.cs` & `Post.cs`) 및 파생 컨텍스트(`BloggingContext.cs`)를 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-148">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="69d32-149">엔터티 클래스는 쿼리하고 저장할 데이터를 나타내는 단순 C# 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-149">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="69d32-150">`Blog` 및 `Post` 엔터티 클래스는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-150">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> <span data-ttu-id="69d32-151">지연 로드를 사용하도록 설정하려면 탐색 속성을 `virtual`로 만들 수 있습니다(Blog.Post 및 Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="69d32-151">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

 <span data-ttu-id="69d32-152">컨텍스트는 데이터베이스가 있는 세션을 나타내며 이를 통해 엔터티 클래스의 인스턴스를 쿼리하고 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-152">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="69d32-153">종속성 주입에 컨텍스트 등록</span><span class="sxs-lookup"><span data-stu-id="69d32-153">Register your context with dependency injection</span></span>

<span data-ttu-id="69d32-154">종속성 주입은 ASP.NET Core의 중심 개념입니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-154">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="69d32-155">서비스(예: `BloggingContext`)는 애플리케이션 시작 중에 종속성 주입에 등록됩니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-155">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="69d32-156">이러한 서비스(예: MVC 컨트롤러)가 필요한 구성 요소에는 생성자 매개 변수 또는 속성을 통해 이러한 서비스가 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-156">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="69d32-157">종속성 주입에 대한 자세한 내용은 ASP.NET 사이트에서 [종속성 주입](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) 문서를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="69d32-157">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="69d32-158">Startup.cs에서 컨텍스트 등록 및 구성</span><span class="sxs-lookup"><span data-stu-id="69d32-158">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="69d32-159">`BloggingContext`를 MVC 컨트롤러에 사용할 수 있도록 서비스로 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-159">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="69d32-160">**Startup.cs**를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-160">Open **Startup.cs**</span></span>
* <span data-ttu-id="69d32-161">파일 시작 부분에 다음 `using` 문을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-161">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="69d32-162">이제 `AddDbContext(...)` 메서드를 사용하여 서비스로 등록할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-162">Now you can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="69d32-163">`ConfigureServices(...)` 메서드를 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-163">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="69d32-164">다음 강조 표시된 코드를 추가하여 컨텍스트를 서비스로 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-164">Add the following highlighted code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> <span data-ttu-id="69d32-165">실제 애플리케이션에서는 일반적으로 연결 문자열을 구성 파일 또는 환경 변수에 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-165">In a real application you would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="69d32-166">편의상 이 자습서는 코드에서 정의했습니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-166">For the sake of simplicity, this tutorial has you define it in code.</span></span> <span data-ttu-id="69d32-167">자세한 내용은 [연결 문자열](../../miscellaneous/connection-strings.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="69d32-167">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="69d32-168">컨트롤러 및 보기 만들기</span><span class="sxs-lookup"><span data-stu-id="69d32-168">Create a controller and views</span></span>

* <span data-ttu-id="69d32-169">**솔루션 탐색기**에서 **컨트롤러** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 -> 컨트롤러...** 를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-169">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="69d32-170">**보기 포함 MVC 컨트롤러, Entity Framework**를 선택하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-170">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="69d32-171">**모델 클래스**를 **Blog**로 설정하고 **데이터 컨텍스트 클래스**를 **BloggingContext**로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-171">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="69d32-172">**추가**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-172">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="69d32-173">애플리케이션 실행</span><span class="sxs-lookup"><span data-stu-id="69d32-173">Run the application</span></span>

<span data-ttu-id="69d32-174">이제 애플리케이션을 실행하여 작동하는지 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-174">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="69d32-175">**디버그 -> 디버깅하지 않고 시작**</span><span class="sxs-lookup"><span data-stu-id="69d32-175">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="69d32-176">애플리케이션이 빌드되고 웹 브라우저에서 열립니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-176">The application builds and opens in a web browser</span></span>
* <span data-ttu-id="69d32-177">`/Blogs`로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-177">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="69d32-178">**새로 만들기**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-178">Click **Create New**</span></span>
* <span data-ttu-id="69d32-179">새 블로그의 **URL**을 입력하고 **만들기**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="69d32-179">Enter a **Url** for the new blog and click **Create**</span></span>

  ![페이지 만들기](_static/create.png)

  ![인덱스 페이지](_static/index-existing-db.png)

## <a name="next-steps"></a><span data-ttu-id="69d32-182">다음 단계</span><span class="sxs-lookup"><span data-stu-id="69d32-182">Next steps</span></span>

<span data-ttu-id="69d32-183">컨텍스트 및 엔터티 클래스를 스캐폴드하는 방법에 대한 자세한 내용은 다음 문서를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="69d32-183">For more information about how to scaffold a context and entity classes, see the following articles:</span></span>
* [<span data-ttu-id="69d32-184">리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="69d32-184">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
* [<span data-ttu-id="69d32-185">Entity Framework Core 도구 참조 - .NET CLI</span><span class="sxs-lookup"><span data-stu-id="69d32-185">Entity Framework Core tools reference - .NET CLI</span></span>](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [<span data-ttu-id="69d32-186">Entity Framework Core 도구 참조 - 패키지 관리자 콘솔</span><span class="sxs-lookup"><span data-stu-id="69d32-186">Entity Framework Core tools reference - Package Manager Console</span></span>](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)
