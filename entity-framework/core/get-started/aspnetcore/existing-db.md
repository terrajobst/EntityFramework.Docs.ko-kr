---
title: ASP.NET Core에서 시작 - 기존 데이터베이스 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: db2469d0badd428734425c1f568667f00bef2f4f
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30151016"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="3c4a3-102">ASP.NET Core에서 기존 데이터베이스로 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="3c4a3-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="3c4a3-103">이 연습에서는 Entity Framework를 사용하여 기본 데이터 액세스를 수행하는 ASP.NET Core MVC 응용 프로그램을 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="3c4a3-104">리버스 엔지니어링을 사용하여 기존 데이터베이스를 기반으로 Entity Framework 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="3c4a3-105">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c4a3-106">전제 조건</span><span class="sxs-lookup"><span data-stu-id="3c4a3-106">Prerequisites</span></span>

<span data-ttu-id="3c4a3-107">이 연습을 완료하려면 다음 필수 구성 요소가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="3c4a3-108">[Visual Studio 201715.3](https://www.visualstudio.com/downloads/) 및 다음 워크로드:</span><span class="sxs-lookup"><span data-stu-id="3c4a3-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="3c4a3-109">**ASP.NET 및 웹 개발**(**웹 및 클라우드** 아래)</span><span class="sxs-lookup"><span data-stu-id="3c4a3-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="3c4a3-110">**.NET Core 플랫폼 간 개발**(**기타 도구 집합** 아래)</span><span class="sxs-lookup"><span data-stu-id="3c4a3-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="3c4a3-111">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="3c4a3-111">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* [<span data-ttu-id="3c4a3-112">Blogging 데이터베이스</span><span class="sxs-lookup"><span data-stu-id="3c4a3-112">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="3c4a3-113">Blogging 데이터베이스</span><span class="sxs-lookup"><span data-stu-id="3c4a3-113">Blogging database</span></span>

<span data-ttu-id="3c4a3-114">이 자습서에서는 LocalDb 인스턴스의 **Blogging** 데이터베이스를 기존 데이터베이스로 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="3c4a3-115">**이미 다른 자습서의 일부로 Blogging** 데이터베이스를 만든 경우 다음 단계를 건너뛸 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-115">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="3c4a3-116">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-116">Open Visual Studio</span></span>
* <span data-ttu-id="3c4a3-117">**도구 -> 데이터베이스에 연결...**</span><span class="sxs-lookup"><span data-stu-id="3c4a3-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="3c4a3-118">**Microsoft SQL Server**를 선택하고 **계속**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="3c4a3-119">**서버 이름**으로 **(localdb)\mssqllocaldb**를 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="3c4a3-120">**데이터베이스 이름**으로 **master**를 입력하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="3c4a3-121">이제 master 데이터베이스가 **서버 탐색기**의 **데이터 연결**에 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="3c4a3-122">**서버 탐색기**에서 데이터베이스를 마우스 오른쪽 단추로 클릭하고 **새 쿼리**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="3c4a3-123">아래 나열된 스크립트를 쿼리 편집기로 복사합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-123">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="3c4a3-124">쿼리 편집기를 마우스 오른쪽 단추로 클릭하고 **실행**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="3c4a3-125">새 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="3c4a3-125">Create a new project</span></span>

* <span data-ttu-id="3c4a3-126">Visual Studio 2017을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="3c4a3-127">**파일 -> 새로 만들기 -> 프로젝트...**</span><span class="sxs-lookup"><span data-stu-id="3c4a3-127">**File -> New -> Project...**</span></span>
* <span data-ttu-id="3c4a3-128">왼쪽 메뉴에서 **설치됨 -> 템플릿 -> Visual C# -> 웹**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-128">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="3c4a3-129">**ASP.NET Core 웹 응용 프로그램(.NET Core)** 프로젝트 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-129">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="3c4a3-130">이름으로 **EFGetStarted.AspNetCore.ExistingDb**를 입력하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="3c4a3-131">**새 ASP.NET Core 웹 응용 프로그램** 대화 상자가 표시될 때까지 기다립니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="3c4a3-132">**ASP.NET Core 템플릿 2.0**에서 **웹 응용 프로그램(모델-뷰-컨트롤러)** 을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-132">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="3c4a3-133">**인증**이 **인증 없음**으로 설정되었는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="3c4a3-134">**확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-134">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="3c4a3-135">Entity Framework 설치</span><span class="sxs-lookup"><span data-stu-id="3c4a3-135">Install Entity Framework</span></span>

<span data-ttu-id="3c4a3-136">EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-136">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="3c4a3-137">이 연습에서는 SQL Server를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-137">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="3c4a3-138">사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="3c4a3-139">**도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="3c4a3-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="3c4a3-140">`Install-Package Microsoft.EntityFrameworkCore.SqlServer` 실행</span><span class="sxs-lookup"><span data-stu-id="3c4a3-140">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="3c4a3-141">일부 Entity Framework Tools를 사용하여 데이터베이스에서 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-141">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="3c4a3-142">따라서 도구 패키지도 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-142">So we will install the tools package as well:</span></span>

* <span data-ttu-id="3c4a3-143">`Install-Package Microsoft.EntityFrameworkCore.Tools` 실행</span><span class="sxs-lookup"><span data-stu-id="3c4a3-143">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="3c4a3-144">일부 ASP.NET Core 스캐폴딩 도구를 사용하여 나중에 컨트롤러와 뷰를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-144">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="3c4a3-145">따라서 이 디자인 패키지도 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-145">So we will install this design package as well:</span></span>

* <span data-ttu-id="3c4a3-146">`Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design` 실행</span><span class="sxs-lookup"><span data-stu-id="3c4a3-146">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="3c4a3-147">모델 리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="3c4a3-147">Reverse engineer your model</span></span>

<span data-ttu-id="3c4a3-148">이제 기존 데이터베이스를 기반으로 EF 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-148">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="3c4a3-149">**도구 -> NuGet 패키지 관리자 -> 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="3c4a3-149">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="3c4a3-150">다음 명령을 실행하여 기존 데이터베이스에서 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-150">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="3c4a3-151">`The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`이라는 오류가 표시되면 Visual Studio를 닫았다가 다시 엽니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-151">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="3c4a3-152">위의 명령에 `-Tables` 인수를 추가하여 엔터티를 생성할 테이블을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-152">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="3c4a3-153">예:</span><span class="sxs-lookup"><span data-stu-id="3c4a3-153">E.g.</span></span> <span data-ttu-id="3c4a3-154">`-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-154">`-Tables Blog,Post`.</span></span>

<span data-ttu-id="3c4a3-155">리버스 엔지니어링 프로세스에서는 기존 데이터베이스의 스키마를 기반으로 엔터티 클래스(`Blog.cs` & `Post.cs`) 및 파생 컨텍스트(`BloggingContext.cs`)를 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-155">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="3c4a3-156">엔터티 클래스는 쿼리하고 저장할 데이터를 나타내는 단순 C# 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-156">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="3c4a3-157">컨텍스트는 데이터베이스가 있는 세션을 나타내며 이를 통해 엔터티 클래스의 인스턴스를 쿼리하고 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-157">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="3c4a3-158">종속성 주입에 컨텍스트 등록</span><span class="sxs-lookup"><span data-stu-id="3c4a3-158">Register your context with dependency injection</span></span>

<span data-ttu-id="3c4a3-159">종속성 주입은 ASP.NET Core의 중심 개념입니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-159">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="3c4a3-160">서비스(예: `BloggingContext`)는 응용 프로그램 시작 중에 종속성 주입에 등록됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-160">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="3c4a3-161">이러한 서비스(예: MVC 컨트롤러)가 필요한 구성 요소에는 생성자 매개 변수 또는 속성을 통해 이러한 서비스가 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-161">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="3c4a3-162">종속성 주입에 대한 자세한 내용은 ASP.NET 사이트에서 [종속성 주입](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) 문서를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-162">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="3c4a3-163">인라인 컨텍스트 구성 제거</span><span class="sxs-lookup"><span data-stu-id="3c4a3-163">Remove inline context configuration</span></span>

<span data-ttu-id="3c4a3-164">ASP.NET Core에서 구성은 일반적으로 **Startup.cs**에서 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-164">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="3c4a3-165">이 패턴을 따르기 위해 데이터베이스 공급자의 구성을 **Startup.cs**로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-165">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="3c4a3-166">`Models\BloggingContext.cs`를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-166">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="3c4a3-167">`OnConfiguring(...)` 메서드를 삭제합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-167">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="3c4a3-168">종속성 주입으로 구성이 컨텍스트에 전달될 수 있도록 다음 생성자를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-168">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="3c4a3-169">Startup.cs에서 컨텍스트 등록 및 구성</span><span class="sxs-lookup"><span data-stu-id="3c4a3-169">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="3c4a3-170">MVC 컨트롤러에서 `BloggingContext`를 사용하려면 이 항목을 서비스로 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-170">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="3c4a3-171">**Startup.cs**를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-171">Open **Startup.cs**</span></span>
* <span data-ttu-id="3c4a3-172">파일 시작 부분에 다음 `using` 문을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-172">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="3c4a3-173">이제 `AddDbContext(...)` 메서드를 사용하여 서비스로 등록할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-173">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="3c4a3-174">`ConfigureServices(...)` 메서드를 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-174">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="3c4a3-175">다음 코드를 추가하여 컨텍스트를 서비스로 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-175">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="3c4a3-176">실제 응용 프로그램에서는 일반적으로 연결 문자열을 구성 파일에 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-176">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="3c4a3-177">간단한 설명을 위해 여기에서는 코드로 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-177">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="3c4a3-178">자세한 내용은 [연결 문자열](../../miscellaneous/connection-strings.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-178">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="3c4a3-179">컨트롤러 만들기</span><span class="sxs-lookup"><span data-stu-id="3c4a3-179">Create a controller</span></span>

<span data-ttu-id="3c4a3-180">다음으로 프로젝트에서 스캐폴딩을 사용하도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-180">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="3c4a3-181">**솔루션 탐색기**에서 **컨트롤러** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 -> 컨트롤러...** 를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-181">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="3c4a3-182">**전체 종속성**을 선택하고 **추가**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-182">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="3c4a3-183">열리는 `ScaffoldingReadMe.txt` 파일의 지침은 무시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-183">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="3c4a3-184">이제 스캐폴딩이 사용 가능하므로 `Blog` 엔터티에 대한 컨트롤러를 스캐폴딩할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-184">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="3c4a3-185">**솔루션 탐색기**에서 **컨트롤러** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 -> 컨트롤러...** 를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-185">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="3c4a3-186">**보기 포함 MVC 컨트롤러, Entity Framework**를 선택하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-186">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="3c4a3-187">**모델 클래스**를 **Blog**로 설정하고 **데이터 컨텍스트 클래스**를 **BloggingContext**로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-187">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="3c4a3-188">**추가**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-188">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="3c4a3-189">응용 프로그램 실행</span><span class="sxs-lookup"><span data-stu-id="3c4a3-189">Run the application</span></span>

<span data-ttu-id="3c4a3-190">이제 응용 프로그램을 실행하여 작동하는지 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-190">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="3c4a3-191">**디버그 -> 디버깅하지 않고 시작**</span><span class="sxs-lookup"><span data-stu-id="3c4a3-191">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="3c4a3-192">응용 프로그램이 빌드되고 웹 브라우저에서 열립니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-192">The application will build and open in a web browser</span></span>
* <span data-ttu-id="3c4a3-193">`/Blogs`로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-193">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="3c4a3-194">**새로 만들기**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-194">Click **Create New**</span></span>
* <span data-ttu-id="3c4a3-195">새 블로그의 **URL**을 입력하고 **만들기**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="3c4a3-195">Enter a **Url** for the new blog and click **Create**</span></span>

![이미지](_static/create.png)

![이미지](_static/index-existing-db.png)
