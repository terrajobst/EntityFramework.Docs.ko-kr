---
title: 기존 데이터베이스에 Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: 61980bbd1f236f496a9d4fd92aa52264f1454615
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182625"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="584c0-102">기존 데이터베이스로 Code First</span><span class="sxs-lookup"><span data-stu-id="584c0-102">Code First to an Existing Database</span></span>
<span data-ttu-id="584c0-103">이 비디오 및 단계별 연습은 기존 데이터베이스를 대상으로 하는 Code First 개발에 대 한 소개를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="584c0-104">Code First를 사용 하면 C\# 또는 VB.Net 클래스를 사용 하 여 모델을 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="584c0-105">필요에 따라 클래스와 속성 또는 흐름 API를 사용 하 여 특성을 사용 하 여 추가 구성을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="584c0-106">비디오 시청</span><span class="sxs-lookup"><span data-stu-id="584c0-106">Watch the video</span></span>
<span data-ttu-id="584c0-107">이 비디오는 [이제 Channel 9에서 사용할 수](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-)있습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-107">This video is [now available on Channel 9](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="584c0-108">필수 구성 요소</span><span class="sxs-lookup"><span data-stu-id="584c0-108">Pre-Requisites</span></span>

<span data-ttu-id="584c0-109">이 연습을 완료 하려면 **Visual Studio 2012** 또는 **Visual Studio 2013** 이 설치 되어 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="584c0-110">**Visual Studio 용 Entity Framework Tools** 버전 **6.1** (또는 이상)이 설치 되어 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="584c0-111">최신 버전의 Entity Framework Tools를 설치 하는 방법에 대 한 자세한 내용은 [Entity Framework 가져오기](~/ef6/fundamentals/install.md) 를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="584c0-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="584c0-112">1. 기존 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-112">1. Create an Existing Database</span></span>

<span data-ttu-id="584c0-113">일반적으로 기존 데이터베이스를 대상으로 하는 경우에는 이미 생성 되지만이 연습에서는 액세스할 데이터베이스를 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="584c0-114">계속 해 서 데이터베이스를 생성 해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="584c0-115">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-115">Open Visual Studio</span></span>
-   <span data-ttu-id="584c0-116">**뷰&gt; 서버 탐색기**</span><span class="sxs-lookup"><span data-stu-id="584c0-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="584c0-117">데이터 연결을 마우스 오른쪽 단추로 클릭 하 **&gt; 연결 추가** ...를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="584c0-118">**서버 탐색기** 데이터베이스에 연결 하지 않은 경우 **Microsoft SQL Server** 를 데이터 원본으로 선택 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![데이터 원본 선택](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="584c0-120">LocalDB 인스턴스에 연결 하 고 데이터베이스 이름으로 **블로그** 를 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![LocalDB 연결](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="584c0-122">**확인** 을 선택 하 고 새 데이터베이스를 만들지 여부를 묻는 메시지가 표시 되 면 **예** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![데이터베이스 만들기 대화 상자](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="584c0-124">이제 새 데이터베이스가 서버 탐색기에 표시 되 면 마우스 오른쪽 단추로 클릭 하 고 **새 쿼리** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="584c0-125">다음 SQL을 새 쿼리에 복사한 다음 쿼리를 마우스 오른쪽 단추로 클릭 하 고 **실행** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

## <a name="2-create-the-application"></a><span data-ttu-id="584c0-126">2. 응용 프로그램 만들기</span><span class="sxs-lookup"><span data-stu-id="584c0-126">2. Create the Application</span></span>

<span data-ttu-id="584c0-127">간단 하 게 유지 하기 위해 Code First를 사용 하 여 데이터 액세스를 수행 하는 기본 콘솔 응용 프로그램을 빌드 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-127">To keep things simple we’re going to build a basic console application that uses Code First to perform data access:</span></span>

-   <span data-ttu-id="584c0-128">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-128">Open Visual Studio</span></span>
-   <span data-ttu-id="584c0-129">**파일&gt; 새&gt; 프로젝트 ...**</span><span class="sxs-lookup"><span data-stu-id="584c0-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="584c0-130">왼쪽 메뉴 및 **콘솔 응용 프로그램** 에서 **Windows** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="584c0-131">이름으로 **CodeFirstExistingDatabaseSample** 을 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="584c0-132">**확인**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="584c0-133">3. 리버스 엔지니어링 모델</span><span class="sxs-lookup"><span data-stu-id="584c0-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="584c0-134">Visual Studio에 대 한 Entity Framework Tools를 사용 하 여 데이터베이스에 매핑하는 초기 코드를 생성 하는 데 도움을 드리겠습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-134">We’re going to make use of the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="584c0-135">이러한 도구는 원하는 경우 직접 입력할 수도 있는 코드를 생성 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="584c0-136">**프로젝트-새 항목 추가&gt; ...**</span><span class="sxs-lookup"><span data-stu-id="584c0-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="584c0-137">왼쪽 메뉴에서 **데이터** 를 선택 하 고 **ADO.NET** 를 선택 엔터티 데이터 모델</span><span class="sxs-lookup"><span data-stu-id="584c0-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="584c0-138">이름으로 **BloggingContext** 를 입력 하 고 **확인을** 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="584c0-139">그러면 **엔터티 데이터 모델 마법사** 가 시작 됩니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="584c0-140">**데이터베이스에서 Code First** 를 선택 하 고 **다음** 을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-140">Select **Code First from Database** and click **Next**</span></span>

    ![마법사 One CFE](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="584c0-142">첫 번째 섹션에서 만든 데이터베이스에 대 한 연결을 선택 하 고 **다음** 을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![마법사 2 CFE](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="584c0-144">테이블 옆의 확인란을 **클릭 하 여** 모든 테이블을 가져오고 **마침** 을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![마법사 3 CFE](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="584c0-146">리버스 엔지니어링 프로세스가 완료 되 면 프로젝트에 많은 항목이 추가 된 후 추가 된 항목을 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="584c0-147">구성 파일</span><span class="sxs-lookup"><span data-stu-id="584c0-147">Configuration file</span></span>

<span data-ttu-id="584c0-148">App.config 파일이 프로젝트에 추가 되었습니다 .이 파일에는 기존 데이터베이스에 대 한 연결 문자열이 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="584c0-149">*구성 파일에서 일부 다른 설정도 확인할 수 있습니다. 이러한 설정에는 데이터베이스를 만들 위치 Code First를 알려 주는 기본 EF 설정이 있습니다. 기존 데이터베이스에 매핑하기 때문에 응용 프로그램에서 이러한 설정이 무시 됩니다.*</span><span class="sxs-lookup"><span data-stu-id="584c0-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="584c0-150">파생 컨텍스트</span><span class="sxs-lookup"><span data-stu-id="584c0-150">Derived Context</span></span>

<span data-ttu-id="584c0-151">**BloggingContext** 클래스가 프로젝트에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="584c0-152">컨텍스트는 데이터베이스와의 세션을 나타내며,이를 통해 데이터를 쿼리하고 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="584c0-153">컨텍스트는 모델의 각 형식에 대 한 **Dbset&lt;&gt;** 를 노출 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="584c0-154">기본 생성자가 **name =** 구문을 사용 하 여 기본 생성자를 호출 하는 것도 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="584c0-155">이는이 컨텍스트에 사용할 연결 문자열을 구성 파일에서 로드 해야 함을 Code First에 알려 줍니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

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

<span data-ttu-id="584c0-156">*구성 파일에서 연결 문자열을 사용 하는 경우 항상 **name =** 구문을 사용 해야 합니다. 이렇게 하면 연결 문자열이 없는 경우 규칙에 따라 새 데이터베이스를 만드는 대신 Entity Framework가 throw 됩니다.*</span><span class="sxs-lookup"><span data-stu-id="584c0-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="584c0-157">모델 클래스</span><span class="sxs-lookup"><span data-stu-id="584c0-157">Model classes</span></span>

<span data-ttu-id="584c0-158">마지막으로, **블로그** 및 **게시물** 클래스도 프로젝트에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="584c0-159">모델을 구성 하는 도메인 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="584c0-160">Code First 규칙이 기존 데이터베이스의 구조와 맞지 않는 구성을 지정 하기 위해 클래스에 적용 되는 데이터 주석을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="584c0-161">예를 들어 **Blog.Name** 및 최대에 대 한 **stringlength** 주석은 데이터베이스에서 최대 길이가 **200** 이기 때문에 표시 됩니다 **.** 기본값은 데이터베이스 공급자가 지 원하는 길이 Code First (SQL Server의 경우 **nvarchar (max)** )를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

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

## <a name="4-reading--writing-data"></a><span data-ttu-id="584c0-162">4. 데이터 쓰기 & 읽기</span><span class="sxs-lookup"><span data-stu-id="584c0-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="584c0-163">이제 모델을 사용 하 여 일부 데이터에 액세스 하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="584c0-164">아래와 같이 **Program.cs** 에서 **Main** 메서드를 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="584c0-165">이 코드는 컨텍스트의 새 인스턴스를 만든 다음이를 사용 하 여 새 **블로그**를 삽입 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="584c0-166">그런 다음 LINQ 쿼리를 사용 하 여 데이터베이스에서 **제목별**로 사전순으로 정렬 된 모든 **블로그** 를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

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

<span data-ttu-id="584c0-167">이제 응용 프로그램을 실행 하 고 테스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-167">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="584c0-168">데이터베이스가 변경 되 면 어떻게 되나요?</span><span class="sxs-lookup"><span data-stu-id="584c0-168">What if My Database Changes?</span></span>

<span data-ttu-id="584c0-169">데이터베이스에 Code First 마법사는 조정 하 고 수정할 수 있는 클래스의 시작 지점 집합을 생성 하도록 디자인 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="584c0-170">데이터베이스 스키마가 변경 되 면 클래스를 수동으로 편집 하거나 다른 역방향 엔지니어를 수행 하 여 클래스를 덮어쓸 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="584c0-171">기존 데이터베이스에 Code First 마이그레이션 사용</span><span class="sxs-lookup"><span data-stu-id="584c0-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="584c0-172">기존 데이터베이스에서 Code First 마이그레이션를 사용 하려면 [기존 데이터베이스에 Code First 마이그레이션을](~/ef6/modeling/code-first/migrations/existing-database.md)참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="584c0-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="584c0-173">요약</span><span class="sxs-lookup"><span data-stu-id="584c0-173">Summary</span></span>

<span data-ttu-id="584c0-174">이 연습에서는 기존 데이터베이스를 사용 하 여 Code First 개발을 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="584c0-175">Visual Studio 용 Entity Framework Tools를 사용 하 여 데이터베이스에 매핑되고 데이터를 저장 하 고 검색 하는 데 사용할 수 있는 클래스 집합을 리버스 엔지니어링 했습니다.</span><span class="sxs-lookup"><span data-stu-id="584c0-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>
