---
title: 기존 데이터베이스-EF6에 code First
author: divega
ms.date: 2016-10-23
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: fedfb921919582e2cdb5f3bc497f11889b972ad6
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251078"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="df9ad-102">기존 데이터베이스에 대 한 code First</span><span class="sxs-lookup"><span data-stu-id="df9ad-102">Code First to an Existing Database</span></span>
<span data-ttu-id="df9ad-103">이 비디오 및 단계별 연습에서는 기존 데이터베이스를 대상으로 하는 Code First 개발에 대 한 소개를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="df9ad-104">C를 사용 하 여 모델을 정의 하는 코드 먼저 허용\# 또는 VB.Net 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="df9ad-105">클래스 및 속성 또는 fluent API를 사용 하 여 특성을 사용 하 여 필요에 따라 추가 구성을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="df9ad-106">비디오를 시청 하세요.</span><span class="sxs-lookup"><span data-stu-id="df9ad-106">Watch the video</span></span>
<span data-ttu-id="df9ad-107">이 비디오 [Channel 9의 출시](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-)합니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-107">This video is [now available on Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="df9ad-108">필수 조건</span><span class="sxs-lookup"><span data-stu-id="df9ad-108">Pre-Requisites</span></span>

<span data-ttu-id="df9ad-109">해야 합니다 **Visual Studio 2012** 하거나 **Visual Studio 2013** 이 연습을 완료 하려면 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="df9ad-110">버전도 해야 **6.1** (또는 이상)의 합니다 **Entity Framework Tools for Visual Studio** 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="df9ad-111">참조 [Entity Framework 가져오기](~/ef6/fundamentals/install.md) Entity Framework Tools의 최신 버전 설치에 대 한 정보에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="df9ad-112">1. 기존 데이터베이스를 만들려면</span><span class="sxs-lookup"><span data-stu-id="df9ad-112">1. Create an Existing Database</span></span>

<span data-ttu-id="df9ad-113">일반적으로 이미 만들어집니다, 기존 데이터베이스를 대상으로 할 때 있지만이 연습에 액세스 하려면 데이터베이스를 만들려고 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="df9ad-114">데이터베이스를 생성 해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="df9ad-115">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-115">Open Visual Studio</span></span>
-   <span data-ttu-id="df9ad-116">**보기-&gt; 서버 탐색기**</span><span class="sxs-lookup"><span data-stu-id="df9ad-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="df9ad-117">마우스 오른쪽 단추로 클릭 **데이터 연결-&gt; 연결 추가 중...**</span><span class="sxs-lookup"><span data-stu-id="df9ad-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="df9ad-118">데이터베이스에 연결 하지 않았으면 **서버 탐색기** 를 선택 해야 하기 전에 **Microsoft SQL Server** 데이터 원본으로</span><span class="sxs-lookup"><span data-stu-id="df9ad-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![데이터 원본 선택](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="df9ad-120">LocalDB 인스턴스에 연결 하 고 입력 **블로깅** 데이터베이스 이름으로</span><span class="sxs-lookup"><span data-stu-id="df9ad-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![LocalDB 연결](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="df9ad-122">선택 **확인** 를 묻는 새 데이터베이스를 만들려는 경우 **예**</span><span class="sxs-lookup"><span data-stu-id="df9ad-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![데이터베이스 대화 상자 만들기](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="df9ad-124">새 데이터베이스 이제 서버 탐색기에서 마우스 나타나고 선택 **새 쿼리**</span><span class="sxs-lookup"><span data-stu-id="df9ad-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="df9ad-125">새 쿼리를 다음과 같은 SQL 복사 후 선택한 쿼리를 마우스 오른쪽 단추로 클릭 **실행**</span><span class="sxs-lookup"><span data-stu-id="df9ad-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a><span data-ttu-id="df9ad-126">2. 응용 프로그램 만들기</span><span class="sxs-lookup"><span data-stu-id="df9ad-126">2. Create the Application</span></span>

<span data-ttu-id="df9ad-127">간단 하 게 데이터 액세스를 수행 하려면 Code First를 사용 하는 기본적인 콘솔 응용 프로그램을 빌드 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-127">To keep things simple we’re going to build a basic console application that uses Code First to perform data access:</span></span>

-   <span data-ttu-id="df9ad-128">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-128">Open Visual Studio</span></span>
-   <span data-ttu-id="df9ad-129">**파일만&gt; 새로운 기능-&gt; 프로젝트...**</span><span class="sxs-lookup"><span data-stu-id="df9ad-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="df9ad-130">선택 **Windows** 왼쪽된 메뉴에서 및 **콘솔 응용 프로그램**</span><span class="sxs-lookup"><span data-stu-id="df9ad-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="df9ad-131">입력 **CodeFirstExistingDatabaseSample** 이름으로</span><span class="sxs-lookup"><span data-stu-id="df9ad-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="df9ad-132">**확인**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="df9ad-133">3. 모델 리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="df9ad-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="df9ad-134">사용 하 여 Entity Framework Tools for Visual Studio를 데이터베이스에 매핑하기 위해 일부 초기 코드를 생성 하는 데 도움이 되도록 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-134">We’re going to make use of the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="df9ad-135">이러한 도구 원하는 경우 손으로 입력도 수 하는 코드를 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="df9ad-136">**프로젝트-&gt; 새 항목 추가...**</span><span class="sxs-lookup"><span data-stu-id="df9ad-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="df9ad-137">선택 **데이터** 왼쪽된 메뉴에서 차례로 **ADO.NET 엔터티 데이터 모델**</span><span class="sxs-lookup"><span data-stu-id="df9ad-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="df9ad-138">입력 **BloggingContext** 이름과 클릭 **확인**</span><span class="sxs-lookup"><span data-stu-id="df9ad-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="df9ad-139">그러면는 **엔터티 데이터 모델 마법사**</span><span class="sxs-lookup"><span data-stu-id="df9ad-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="df9ad-140">선택 **데이터베이스에서 Code First** 를 클릭 하 고 **다음**</span><span class="sxs-lookup"><span data-stu-id="df9ad-140">Select **Code First from Database** and click **Next**</span></span>

    ![하나의 CFE 마법사](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="df9ad-142">첫 번째 섹션에서 만든 데이터베이스에 연결을 선택 하 고 클릭 **다음**</span><span class="sxs-lookup"><span data-stu-id="df9ad-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![두 CFE 마법사](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="df9ad-144">옆에 있는 확인란을 클릭 **테이블** 모든 테이블을 가져오고 클릭 **마침**</span><span class="sxs-lookup"><span data-stu-id="df9ad-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![마법사의 세 가지 CFE](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="df9ad-146">리버스 엔지니어링 프로세스에는 항목 수가 완료 되 면를 추가한 프로젝트에 보겠습니다 추가 된 기능을 확인해 보세요.</span><span class="sxs-lookup"><span data-stu-id="df9ad-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="df9ad-147">구성 파일</span><span class="sxs-lookup"><span data-stu-id="df9ad-147">Configuration file</span></span>

<span data-ttu-id="df9ad-148">App.config 파일을 프로젝트에 추가 되었습니다 기존 데이터베이스에 연결 문자열을 포함 하는이 파일.</span><span class="sxs-lookup"><span data-stu-id="df9ad-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="df9ad-149">*구성 파일에서 같은 다른 설정과 너무 보면, 이러한 기본 설정이 EF Code First 데이터베이스를 만들 위치를 알려 주는 합니다. 응용 프로그램에서 이러한 설정은 무시 됩니다 기존 데이터베이스에 매핑하는 것 때문입니다.*</span><span class="sxs-lookup"><span data-stu-id="df9ad-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="df9ad-150">파생된 컨텍스트</span><span class="sxs-lookup"><span data-stu-id="df9ad-150">Derived Context</span></span>

<span data-ttu-id="df9ad-151">A **BloggingContext** 클래스가 프로젝트에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="df9ad-152">컨텍스트를 쿼리하고 데이터를 저장할 수 있어 데이터베이스와 세션을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="df9ad-153">컨텍스트에 노출 된 **DbSet&lt;TEntity&gt;**  모델의 각 형식에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="df9ad-154">기본 생성자를 사용 하 여 기본 생성자를 호출 하는 또한 보면 합니다 **이름 =** 구문입니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="df9ad-155">그러면 Code First는이 컨텍스트에서 사용 하도록 연결 문자열 구성 파일에서 로드 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

<span data-ttu-id="df9ad-156">*항상 사용 해야 합니다 **이름 =** config 파일에서 연결 문자열을 사용 하는 경우에 구문이 있습니다. 연결 문자열에 없는 경우 다음 Entity Framework는 throw 하는 것이 이렇게 규칙에 따라 새 데이터베이스를 만드는 대신 합니다.*</span><span class="sxs-lookup"><span data-stu-id="df9ad-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="df9ad-157">모델 클래스</span><span class="sxs-lookup"><span data-stu-id="df9ad-157">Model classes</span></span>

<span data-ttu-id="df9ad-158">마지막으로 **블로그** 하 고 **Post** 클래스 프로젝트를 추가 했습니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="df9ad-159">이들은 모델을 구성 하는 도메인 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="df9ad-160">Code First 규칙을 기존 데이터베이스의 구조와 일치 하지는 구성을 지정 하는 클래스에 적용할 데이터 주석은 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="df9ad-161">표시 예를 들어 합니다 **StringLength** 주석이 **Blog.Name** 및 **Blog.Url** 최대 길이가 있기 때문 **200** 에 데이터베이스 (데이터베이스 공급자-지 원하는 최대 길이 사용 하는 Code First이 기본값인 **nvarchar (max)** SQL Server에서).</span><span class="sxs-lookup"><span data-stu-id="df9ad-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a><span data-ttu-id="df9ad-162">4. 읽기 및 데이터 쓰기</span><span class="sxs-lookup"><span data-stu-id="df9ad-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="df9ad-163">이제 모델이 준비 일부 데이터 액세스를 사용 하는 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="df9ad-164">구현 된 **Main** 에서 메서드 **Program.cs** 아래와 같이 합니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="df9ad-165">이 코드는 컨텍스트의 새 인스턴스를 만듭니다 및 다음 새로 삽입을 사용 하 여 **블로그**합니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="df9ad-166">LINQ 쿼리를 사용 하 여 모든 검색할 **블로그** 기준으로 사전순으로 정렬 하는 데이터베이스에서 **Title**합니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="df9ad-167">이제 응용 프로그램을 실행 하 고 테스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-167">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="df9ad-168">데이터베이스의 변경 될까요?</span><span class="sxs-lookup"><span data-stu-id="df9ad-168">What if My Database Changes?</span></span>

<span data-ttu-id="df9ad-169">Code First 데이터베이스 마법사를 조정 하 고 수정할 수 있는 클래스의 시작 지점 집합을 생성 하도록 설계 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="df9ad-170">데이터베이스 스키마를 변경 하는 경우 수동으로 클래스를 편집 하거나 클래스를 덮어쓰려면 다른 리버스 엔지니어링을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="df9ad-171">기존 데이터베이스에 Code First 마이그레이션 사용</span><span class="sxs-lookup"><span data-stu-id="df9ad-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="df9ad-172">기존 데이터베이스를 사용 하 여 Code First 마이그레이션을 사용 하려는 경우 참조 [기존 데이터베이스에 Code First 마이그레이션을](~/ef6/modeling/code-first/migrations/existing-database.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="df9ad-173">요약</span><span class="sxs-lookup"><span data-stu-id="df9ad-173">Summary</span></span>

<span data-ttu-id="df9ad-174">이 연습에서는 기존 데이터베이스를 사용 하 여 Code First 개발에 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="df9ad-175">리버스 엔지니어링할 데이터베이스에 매핑되고 데이터 저장 및 검색 하기 위해 사용할 수 있는 클래스 집합을 Entity Framework Tools for Visual Studio를 사용 했습니다.</span><span class="sxs-lookup"><span data-stu-id="df9ad-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>
