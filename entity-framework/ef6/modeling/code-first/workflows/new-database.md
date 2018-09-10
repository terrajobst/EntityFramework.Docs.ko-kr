---
title: 새 데이터베이스-EF6 대 한 code First
author: divega
ms.date: 2016-10-23
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: 8ed1bfbc3536acc0d83b9c8ecdd180aeb44eff83
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251052"
---
# <a name="code-first-to-a-new-database"></a><span data-ttu-id="bb171-102">새 데이터베이스에 대 한 code First</span><span class="sxs-lookup"><span data-stu-id="bb171-102">Code First to a New Database</span></span>
<span data-ttu-id="bb171-103">이 비디오 및 단계별 연습에서는 새 데이터베이스를 대상으로 하는 Code First 개발에 대 한 소개를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="bb171-104">이 시나리오는 존재 하지 않는 데이터베이스를 대상으로 포함 및 Code First는 만들거나 빈 데이터베이스는 Code First는 새 테이블을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-104">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="bb171-105">C를 사용 하 여 모델을 정의 하는 코드 먼저 허용\# 또는 VB.Net 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-105">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="bb171-106">클래스 및 속성 또는 fluent API를 사용 하 여 특성을 사용 하 여 추가 구성을 수행할 필요에 따라 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-106">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="bb171-107">비디오를 시청 하세요.</span><span class="sxs-lookup"><span data-stu-id="bb171-107">Watch the video</span></span>
<span data-ttu-id="bb171-108">이 비디오에서는 새 데이터베이스를 대상으로 하는 Code First 개발에 대해 소개 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-108">This video provides an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="bb171-109">이 시나리오는 존재 하지 않는 데이터베이스를 대상으로 포함 및 Code First는 만들거나 빈 데이터베이스는 Code First는 새 테이블을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-109">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="bb171-110">먼저 코드를 사용 하면 C# 또는 VB.Net 클래스를 사용 하 여 모델을 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-110">Code First allows you to define your model using C# or VB.Net classes.</span></span> <span data-ttu-id="bb171-111">클래스 및 속성 또는 fluent API를 사용 하 여 특성을 사용 하 여 추가 구성을 수행할 필요에 따라 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-111">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

<span data-ttu-id="bb171-112">**작성자**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="bb171-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="bb171-113">**비디오**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span><span class="sxs-lookup"><span data-stu-id="bb171-113">**Video**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span></span>

# <a name="pre-requisites"></a><span data-ttu-id="bb171-114">필수 조건</span><span class="sxs-lookup"><span data-stu-id="bb171-114">Pre-Requisites</span></span>

<span data-ttu-id="bb171-115">적어도 Visual studio 2010 해야 하거나이 연습을 완료 하려면 Visual Studio 2012를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="bb171-116">Visual Studio 2010을 사용 하는 경우 해야 할 [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="bb171-117">1. 응용 프로그램 만들기</span><span class="sxs-lookup"><span data-stu-id="bb171-117">1. Create the Application</span></span>

<span data-ttu-id="bb171-118">간단 하 게 데이터 액세스를 수행 하려면 Code First를 사용 하는 기본적인 콘솔 응용 프로그램을 빌드하는 것이 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-118">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="bb171-119">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-119">Open Visual Studio</span></span>
-   <span data-ttu-id="bb171-120">**파일만&gt; 새로운 기능-&gt; 프로젝트...**</span><span class="sxs-lookup"><span data-stu-id="bb171-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="bb171-121">선택 **Windows** 왼쪽된 메뉴에서 및 **콘솔 응용 프로그램**</span><span class="sxs-lookup"><span data-stu-id="bb171-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="bb171-122">입력 **CodeFirstNewDatabaseSample** 이름으로</span><span class="sxs-lookup"><span data-stu-id="bb171-122">Enter **CodeFirstNewDatabaseSample** as the name</span></span>
-   <span data-ttu-id="bb171-123">**확인**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-123">Select **OK**</span></span>

## <a name="2-create-the-model"></a><span data-ttu-id="bb171-124">2. 모델 만들기</span><span class="sxs-lookup"><span data-stu-id="bb171-124">2. Create the Model</span></span>

<span data-ttu-id="bb171-125">클래스를 사용 하 여 매우 간단한 모델을 정의 해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-125">Let’s define a very simple model using classes.</span></span> <span data-ttu-id="bb171-126">방금 정의 고 Program.cs 파일에는 있지만 아웃 클래스를 별도 파일 및 잠재적으로 별도 프로젝트를 분할 하는 실제 응용 프로그램에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-126">We’re just defining them in the Program.cs file but in a real world application you would split your classes out into separate files and potentially a separate project.</span></span>

<span data-ttu-id="bb171-127">프로그램 클래스 정의 Program.cs에서 아래 다음 두 클래스를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-127">Below the Program class definition in Program.cs add the following two classes.</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

<span data-ttu-id="bb171-128">만들고 두 개의 탐색 속성 (Blog.Posts 및 Post.Blog) 가상 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-128">You’ll notice that we’re making the two navigation properties (Blog.Posts and Post.Blog) virtual.</span></span> <span data-ttu-id="bb171-129">따라서 Entity Framework의 지연 로드 기능이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-129">This enables the Lazy Loading feature of Entity Framework.</span></span> <span data-ttu-id="bb171-130">지연 로드는 이러한 속성의 내용을 자동으로 로드할 데이터베이스에서 액세스 하려고 할 때를 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-130">Lazy Loading means that the contents of these properties will be automatically loaded from the database when you try to access them.</span></span>

## <a name="3-create-a-context"></a><span data-ttu-id="bb171-131">3. 컨텍스트 만들기</span><span class="sxs-lookup"><span data-stu-id="bb171-131">3. Create a Context</span></span>

<span data-ttu-id="bb171-132">이제 데이터베이스를 쿼리하고 데이터를 저장할 수 있어를 사용 하 여 세션을 나타내는 파생된 컨텍스트를 정의 하는 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-132">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="bb171-133">System.Data.Entity.DbContext에서 파생 되 고 형식화 된 DbSet을 노출 하는 컨텍스트를 정의 했습니다&lt;TEntity&gt; 모델의 각 클래스에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-133">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

<span data-ttu-id="bb171-134">이제 EntityFramework NuGet 패키지를 추가 해야 하므로 Entity Framework에서 형식 사용 하기 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-134">We’re now starting to use types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="bb171-135">**프로젝트&gt; NuGet 패키지 관리...**</span><span class="sxs-lookup"><span data-stu-id="bb171-135">**Project –&gt; Manage NuGet Packages…**</span></span>
    <span data-ttu-id="bb171-136">참고: 없는 경우는 **NuGet 패키지 관리...**</span><span class="sxs-lookup"><span data-stu-id="bb171-136">Note: If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="bb171-137">설치 해야 하는 옵션을 [최신 버전의 NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span><span class="sxs-lookup"><span data-stu-id="bb171-137">option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="bb171-138">선택 된 **Online** 탭</span><span class="sxs-lookup"><span data-stu-id="bb171-138">Select the **Online** tab</span></span>
-   <span data-ttu-id="bb171-139">선택 된 **EntityFramework** 패키지</span><span class="sxs-lookup"><span data-stu-id="bb171-139">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="bb171-140">클릭 **설치**</span><span class="sxs-lookup"><span data-stu-id="bb171-140">Click **Install**</span></span>

<span data-ttu-id="bb171-141">추가 하 여 문을 Program.cs 맨 위에 있는 System.Data.Entity에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-141">Add a using statement for System.Data.Entity at the top of Program.cs.</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="bb171-142">Program.cs에서 Post 클래스 아래 다음 파생된 컨텍스트를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-142">Below the Post class in Program.cs add the following derived context.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

<span data-ttu-id="bb171-143">Program.cs 이제 포함 항목의 전체 목록은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-143">Here is a complete listing of what Program.cs should now contain.</span></span>

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

<span data-ttu-id="bb171-144">데이터 저장 및 검색을 시작 해야 하는 모든 코드입니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-144">That is all the code we need to start storing and retrieving data.</span></span> <span data-ttu-id="bb171-145">물론 백그라운드에서 진행 상당한 되며 알아보겠습니다 살펴보겠습니다는 첫 번째 있지만 잠시에서 하는지 확인해 보겠습니다 작업.</span><span class="sxs-lookup"><span data-stu-id="bb171-145">Obviously there is quite a bit going on behind the scenes and we’ll take a look at that in a moment but first let’s see it in action.</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="bb171-146">4. 읽기 및 데이터 쓰기</span><span class="sxs-lookup"><span data-stu-id="bb171-146">4. Reading & Writing Data</span></span>

<span data-ttu-id="bb171-147">아래 표시 된 것과 같이 Program.cs의 Main 메서드를 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-147">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="bb171-148">이 코드는이 컨텍스트의 새 인스턴스를 만듭니다 및 다음 새 블로그 삽입을 사용 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-148">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="bb171-149">다음 LINQ 쿼리를 사용 하 여 제목으로 사전순으로 정렬 하는 데이터베이스에서 모든 블로그를 검색 하려면.</span><span class="sxs-lookup"><span data-stu-id="bb171-149">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="bb171-150">이제 응용 프로그램을 실행 하 고 테스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-150">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a><span data-ttu-id="bb171-151">내 데이터는 어디 입니까?</span><span class="sxs-lookup"><span data-stu-id="bb171-151">Where’s My Data?</span></span>

<span data-ttu-id="bb171-152">관례상 DbContext를 데이터베이스를 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-152">By convention DbContext has created a database for you.</span></span>

-   <span data-ttu-id="bb171-153">(Visual Studio 2010을 사용 하 여 기본적으로 설치 됨)을 로컬 SQL Express 인스턴스를 사용할 수 있는 경우 Code First가 만든 데이터베이스 인스턴스에서</span><span class="sxs-lookup"><span data-stu-id="bb171-153">If a local SQL Express instance is available (installed by default with Visual Studio 2010) then Code First has created the database on that instance</span></span>
-   <span data-ttu-id="bb171-154">SQL Express를 사용할 수 없는 경우 Code First를 시도 하 고 사용 [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (Visual Studio 2012를 사용 하 여 기본적으로 설치 됨)</span><span class="sxs-lookup"><span data-stu-id="bb171-154">If SQL Express isn’t available then Code First will try and use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installed by default with Visual Studio 2012)</span></span>
-   <span data-ttu-id="bb171-155">데이터베이스는이 경우에서 파생 컨텍스트의 정규화 된 이름 뒤에 오는 이름은 **CodeFirstNewDatabaseSample.BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="bb171-155">The database is named after the fully qualified name of the derived context, in our case that is **CodeFirstNewDatabaseSample.BloggingContext**</span></span>

<span data-ttu-id="bb171-156">이 방금 기본 규칙 및 Code First를 사용 하는 데이터베이스를 변경 하는 방법은 여러 가지가 있습니다,에 자세한 정보가 제공 됩니다는 **DbContext는 모델 및 데이터베이스 연결을 검색 하는 방법을** 항목입니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-156">These are just the default conventions and there are various ways to change the database that Code First uses, more information is available in the **How DbContext Discovers the Model and Database Connection** topic.</span></span>
<span data-ttu-id="bb171-157">Visual Studio에서 서버 탐색기를 사용 하 여이 데이터베이스에 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-157">You can connect to this database using Server Explorer in Visual Studio</span></span>

-   <span data-ttu-id="bb171-158">**보기-&gt; 서버 탐색기**</span><span class="sxs-lookup"><span data-stu-id="bb171-158">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="bb171-159">마우스 오른쪽 단추로 클릭 **데이터 연결** 선택한 **연결 추가 중...**</span><span class="sxs-lookup"><span data-stu-id="bb171-159">Right click on **Data Connections** and select **Add Connection…**</span></span>
-   <span data-ttu-id="bb171-160">Microsoft SQL Server 데이터 원본으로 선택 해야 하기 전에 서버 탐색기에서 데이터베이스에 연결 하지 않은 경우</span><span class="sxs-lookup"><span data-stu-id="bb171-160">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![데이터 원본 선택](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="bb171-162">LocalDB 또는 어느에 따라 설치한 SQL Express에 연결</span><span class="sxs-lookup"><span data-stu-id="bb171-162">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>

<span data-ttu-id="bb171-163">이제 Code First에서 만든 스키마를 검사 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-163">We can now inspect the schema that Code First created.</span></span>

![초기 스키마](~/ef6/media/schemainitial.png)

<span data-ttu-id="bb171-165">DbContext에 정의한 DbSet 속성을 확인 하 여 모델에 포함할 어떤 클래스를 산출 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-165">DbContext worked out what classes to include in the model by looking at the DbSet properties that we defined.</span></span> <span data-ttu-id="bb171-166">다음 테이블 및 열 이름을 결정, 데이터 형식을 결정 하 고, 기본 키 등을 찾을 Code First 규칙의 기본 집합을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-166">It then uses the default set of Code First conventions to determine table and column names, determine data types, find primary keys, etc.</span></span> <span data-ttu-id="bb171-167">이 연습의 뒷부분에서 이러한 규칙을 재정의할 수 있습니다 하는 방법을 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-167">Later in this walkthrough we’ll look at how you can override these conventions.</span></span>

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="bb171-168">5. 모델 변경 처리</span><span class="sxs-lookup"><span data-stu-id="bb171-168">5. Dealing with Model Changes</span></span>

<span data-ttu-id="bb171-169">이제 데이터베이스 스키마를 업데이트 해야 하는 이러한 변경을 수행 하는 경우 모델을를 일부 변경 하는 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-169">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span> <span data-ttu-id="bb171-170">이렇게 하려면 short에 대 한 Code First 마이그레이션을 또는 마이그레이션 이라는 기능을 사용 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-170">To do this we are going to use a feature called Code First Migrations, or Migrations for short.</span></span>

<span data-ttu-id="bb171-171">마이그레이션은 데이터베이스 스키마를 업그레이드 (및 다운 그레이드) 하는 방법을 설명 하는 단계는 정렬 된 집합이 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-171">Migrations allows us to have an ordered set of steps that describe how to upgrade (and downgrade) our database schema.</span></span> <span data-ttu-id="bb171-172">각 마이그레이션 라고 하는 이러한 단계를 적용할 변경 내용을 설명 하는 일부 코드를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-172">Each of these steps, known as a migration, contains some code that describes the changes to be applied.</span></span> 

<span data-ttu-id="bb171-173">첫 번째 단계는 BloggingContext에 대 한 Code First 마이그레이션을 사용 하도록 설정 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-173">The first step is to enable Code First Migrations for our BloggingContext.</span></span>

-   <span data-ttu-id="bb171-174">**도구-&gt; 라이브러리 패키지 관리자-&gt; 패키지 관리자 콘솔**</span><span class="sxs-lookup"><span data-stu-id="bb171-174">**Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
-   <span data-ttu-id="bb171-175">패키지 관리자 콘솔에서 **Enable-Migrations** 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-175">Run the **Enable-Migrations** command in Package Manager Console</span></span>
-   <span data-ttu-id="bb171-176">새 마이그레이션 폴더 두 항목을 포함 하는 프로젝트에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-176">A new Migrations folder has been added to our project that contains two items:</span></span>
    -   <span data-ttu-id="bb171-177">**Configuration.cs** –이 파일에는 마이그레이션에 대 한 마이그레이션을 사용할 설정이 포함 되어 있습니다. BloggingContext 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-177">**Configuration.cs** – This file contains the settings that Migrations will use for migrating BloggingContext.</span></span> <span data-ttu-id="bb171-178">이 연습에서는 아무 것도 변경할 필요가 없습니다 이지만 여기 네임 스페이스를 변경 하는 시드 데이터를 다른 데이터베이스에 대 한 등록 공급자를 지정할 수 있는 마이그레이션 등에서 생성 되는 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-178">We don’t need to change anything for this walkthrough, but here is where you can specify seed data, register providers for other databases, changes the namespace that migrations are generated in etc.</span></span>
    -   <span data-ttu-id="bb171-179">**&lt;타임 스탬프&gt;\_InitialCreate.cs** –이 첫 번째 마이그레이션을, 블로그 및 게시물 테이블이 포함 된 하나에 빈 데이터베이스에서 수행 하려면 데이터베이스에 이미 적용 된 변경 내용을 나타내는 .</span><span class="sxs-lookup"><span data-stu-id="bb171-179">**&lt;timestamp&gt;\_InitialCreate.cs** – This is your first migration, it represents the changes that have already been applied to the database to take it from being an empty database to one that includes the Blogs and Posts tables.</span></span> <span data-ttu-id="bb171-180">하지만 하도록 Code First 마이그레이션을로 변환 되었을 것 마이그레이션에 옵트인 했습니다 했으므로 우리에 게 있어 이러한 테이블을 자동으로 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-180">Although we let Code First automatically create these tables for us, now that we have opted in to Migrations they have been converted into a Migration.</span></span> <span data-ttu-id="bb171-181">먼저 코드에도이 마이그레이션이 이미 적용 된 로컬 데이터베이스에 기록 됩니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-181">Code First has also recorded in our local database that this Migration has already been applied.</span></span> <span data-ttu-id="bb171-182">파일의 타임 스탬프는 정렬을 위해 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-182">The timestamp on the filename is used for ordering purposes.</span></span>

    <span data-ttu-id="bb171-183">이제 모델을 사용 하 여 변경 하기, 블로그 클래스에 Url 속성을 추가 해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-183">Now let’s make a change to our model, add a Url property to the Blog class:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   <span data-ttu-id="bb171-184">실행 합니다 **Add-migration AddUrl** 패키지 관리자 콘솔에서 명령을 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-184">Run the **Add-Migration AddUrl** command in Package Manager Console.</span></span>
    <span data-ttu-id="bb171-185">추가 마이그레이션 명령을 마지막 마이그레이션 이후 변경 내용을 확인 하 고 발견 되는 변경 내용으로 새 마이그레이션을 스 캐 폴딩 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-185">The Add-Migration command checks for changes since your last migration and scaffolds a new migration with any changes that are found.</span></span> <span data-ttu-id="bb171-186">마이그레이션을; 이름을 제공할 수 있습니다. 이 경우 마이그레이션 'AddUrl' 이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-186">We can give migrations a name; in this case we are calling the migration ‘AddUrl’.</span></span>
    <span data-ttu-id="bb171-187">스 캐 폴드 된 코드는 우리 해야 한다는 dbo에 문자열 데이터를 보유할 수에 있는 Url 열을 추가 합니다. 블로그 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-187">The scaffolded code is saying that we need to add a Url column, that can hold string data, to the dbo.Blogs table.</span></span> <span data-ttu-id="bb171-188">필요한 경우 스 캐 폴드 된 코드를 편집할 수 있지만 경우에 필수는 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-188">If needed, we could edit the scaffolded code but that’s not required in this case.</span></span>

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
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

-   <span data-ttu-id="bb171-189">실행 합니다 **Update-database** 패키지 관리자 콘솔에서 명령을 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-189">Run the **Update-Database** command in Package Manager Console.</span></span> <span data-ttu-id="bb171-190">이 명령은 데이터베이스에 보류 중인 마이그레이션을 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-190">This command will apply any pending migrations to the database.</span></span> <span data-ttu-id="bb171-191">InitialCreate 마이그레이션 마이그레이션을 새 AddUrl 마이그레이션 적용만 있으므로 이미 적용 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-191">Our InitialCreate migration has already been applied so migrations will just apply our new AddUrl migration.</span></span>
    <span data-ttu-id="bb171-192">팁: 사용할 수는 **– Verbose** 보려면 데이터베이스에 대해 실행 되는 SQL 데이터베이스 업데이트를 호출 하는 경우를 전환 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-192">Tip: You can use the **–Verbose** switch when calling Update-Database to see the SQL that is being executed against the database.</span></span>

<span data-ttu-id="bb171-193">새 Url 열이 이제 블로그 테이블 데이터베이스에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-193">The new Url column is now added to the Blogs table in the database:</span></span>

![Url 사용 하 여 스키마](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a><span data-ttu-id="bb171-195">6. 데이터 주석</span><span class="sxs-lookup"><span data-stu-id="bb171-195">6. Data Annotations</span></span>

<span data-ttu-id="bb171-196">지금까지 EF는 기본 규칙을 사용 하 여 모델을 검색만 하도록 했습니다 하지만 클래스에는 규칙을 따르지 시점과 추가 구성을 수행 하려면 먼저 시간 되도록 것입니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-196">So far we’ve just let EF discover the model using its default conventions, but there are going to be times when our classes don’t follow the conventions and we need to be able to perform further configuration.</span></span> <span data-ttu-id="bb171-197">두 가지 방법으로이; 이 섹션의 데이터 주석 및 다음 흐름 API는 다음 섹션에서 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-197">There are two options for this; we’ll look at Data Annotations in this section and then the fluent API in the next section.</span></span>

-   <span data-ttu-id="bb171-198">모델에 추가할 사용자 클래스</span><span class="sxs-lookup"><span data-stu-id="bb171-198">Let’s add a User class to our model</span></span>

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="bb171-199">이 파생된 컨텍스트 집합을 추가 해야</span><span class="sxs-lookup"><span data-stu-id="bb171-199">We also need to add a set to our derived context</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   <span data-ttu-id="bb171-200">표시 된 오류는 마이그레이션을 추가 하려고 할 경우 얻게 "*EntityType 'User' 키가 없는 정의 합니다. 키를 정의이 EntityType에 대 한 합니다. "*</span><span class="sxs-lookup"><span data-stu-id="bb171-200">If we tried to add a migration we’d get an error saying “*EntityType ‘User’ has no key defined. Define the key for this EntityType.”*</span></span> <span data-ttu-id="bb171-201">EF에 사용자 이름을 사용자에 대 한 기본 키를 함을 알 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-201">because EF has no way of knowing that Username should be the primary key for User.</span></span>
-   <span data-ttu-id="bb171-202">사용 하 여 추가 하므로 이제 데이터 주석을 사용 하는 것 Program.cs 맨 위에 있는 문</span><span class="sxs-lookup"><span data-stu-id="bb171-202">We’re going to use Data Annotations now so we need to add a using statement at the top of Program.cs</span></span>

```
using System.ComponentModel.DataAnnotations;
```

-   <span data-ttu-id="bb171-203">이제 기본 키 것을 식별 하기 위해 사용자 이름 속성에 주석 달기</span><span class="sxs-lookup"><span data-stu-id="bb171-203">Now annotate the Username property to identify that it is the primary key</span></span>

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="bb171-204">사용 된 **Add-migration AddUser** 데이터베이스 변경 명령을 적용 하는 마이그레이션을 스 캐 폴딩</span><span class="sxs-lookup"><span data-stu-id="bb171-204">Use the **Add-Migration AddUser** command to scaffold a migration to apply these changes to the database</span></span>
-   <span data-ttu-id="bb171-205">실행 합니다 **Update-database** 새 마이그레이션을 데이터베이스에 적용 하는 명령</span><span class="sxs-lookup"><span data-stu-id="bb171-205">Run the **Update-Database** command to apply the new migration to the database</span></span>

<span data-ttu-id="bb171-206">이제 새 테이블은 데이터베이스에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-206">The new table is now added to the database:</span></span>

![사용자와 스키마](~/ef6/media/schemawithusers.png)

<span data-ttu-id="bb171-208">EF에서 지원 되는 주석의 전체 목록은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-208">The full list of annotations supported by EF is:</span></span>

-   [<span data-ttu-id="bb171-209">KeyAttribute</span><span class="sxs-lookup"><span data-stu-id="bb171-209">KeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [<span data-ttu-id="bb171-210">StringLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="bb171-210">StringLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [<span data-ttu-id="bb171-211">MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="bb171-211">MaxLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [<span data-ttu-id="bb171-212">ConcurrencyCheckAttribute</span><span class="sxs-lookup"><span data-stu-id="bb171-212">ConcurrencyCheckAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [<span data-ttu-id="bb171-213">RequiredAttribute</span><span class="sxs-lookup"><span data-stu-id="bb171-213">RequiredAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [<span data-ttu-id="bb171-214">TimestampAttribute</span><span class="sxs-lookup"><span data-stu-id="bb171-214">TimestampAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [<span data-ttu-id="bb171-215">ComplexTypeAttribute</span><span class="sxs-lookup"><span data-stu-id="bb171-215">ComplexTypeAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [<span data-ttu-id="bb171-216">ColumnAttribute</span><span class="sxs-lookup"><span data-stu-id="bb171-216">ColumnAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [<span data-ttu-id="bb171-217">TableAttribute</span><span class="sxs-lookup"><span data-stu-id="bb171-217">TableAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [<span data-ttu-id="bb171-218">InversePropertyAttribute</span><span class="sxs-lookup"><span data-stu-id="bb171-218">InversePropertyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [<span data-ttu-id="bb171-219">ForeignKeyAttribute</span><span class="sxs-lookup"><span data-stu-id="bb171-219">ForeignKeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [<span data-ttu-id="bb171-220">DatabaseGeneratedAttribute</span><span class="sxs-lookup"><span data-stu-id="bb171-220">DatabaseGeneratedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [<span data-ttu-id="bb171-221">NotMappedAttribute</span><span class="sxs-lookup"><span data-stu-id="bb171-221">NotMappedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a><span data-ttu-id="bb171-222">7. Fluent API</span><span class="sxs-lookup"><span data-stu-id="bb171-222">7. Fluent API</span></span>

<span data-ttu-id="bb171-223">이전 섹션에서 데이터 주석을 사용 하 여 보완 하거나 규칙에 따라 검색 된 항목을 재정의에 대해 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-223">In the previous section we looked at using Data Annotations to supplement or override what was detected by convention.</span></span> <span data-ttu-id="bb171-224">Code First fluent API를 통해 모델을 구성 하는 다른 방법은 됩니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-224">The other way to configure the model is via the Code First fluent API.</span></span>

<span data-ttu-id="bb171-225">대부분의 모델 구성 할 수 있는 간단한 데이터 주석을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-225">Most model configuration can be done using simple data annotations.</span></span> <span data-ttu-id="bb171-226">Fluent API는 데이터 주석 할 수 있는 모든 또한 불가능 데이터 주석을 사용한 몇 가지 고급 구성에 설명 하는 모델 구성을 지정 하는 고급 방법을 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-226">The fluent API is a more advanced way of specifying model configuration that covers everything that data annotations can do in addition to some more advanced configuration not possible with data annotations.</span></span> <span data-ttu-id="bb171-227">데이터 주석과 흐름 API를 함께 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-227">Data annotations and the fluent API can be used together.</span></span>

<span data-ttu-id="bb171-228">Fluent API에 액세스 하려면 DbContext의 OnModelCreating 메서드를 재정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-228">To access the fluent API you override the OnModelCreating method in DbContext.</span></span> <span data-ttu-id="bb171-229">에 방문 하셔서 User.DisplayName 표시할에 저장 되는 열의 이름을 바꿀 경우를 가정해\_이름입니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-229">Let’s say we wanted to rename the column that User.DisplayName is stored in to display\_name.</span></span>

-   <span data-ttu-id="bb171-230">다음 코드를 사용 하 여 BloggingContext OnModelCreating 메서드 재정의</span><span class="sxs-lookup"><span data-stu-id="bb171-230">Override the OnModelCreating method on BloggingContext with the following code</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   <span data-ttu-id="bb171-231">사용 합니다 **Add-migration ChangeDisplayName** 데이터베이스 변경 명령을 적용 하는 마이그레이션을 스 캐 폴딩 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-231">Use the **Add-Migration ChangeDisplayName** command to scaffold a migration to apply these changes to the database.</span></span>
-   <span data-ttu-id="bb171-232">실행 합니다 **Update-database** 새 마이그레이션을 데이터베이스에 적용할 명령.</span><span class="sxs-lookup"><span data-stu-id="bb171-232">Run the **Update-Database** command to apply the new migration to the database.</span></span>

<span data-ttu-id="bb171-233">표시할 DisplayName 열 이름이 이제\_이름:</span><span class="sxs-lookup"><span data-stu-id="bb171-233">The DisplayName column is now renamed to display\_name:</span></span>

![이름을 바꿀 표시 이름과 함께 스키마](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a><span data-ttu-id="bb171-235">요약</span><span class="sxs-lookup"><span data-stu-id="bb171-235">Summary</span></span>

<span data-ttu-id="bb171-236">이 연습에서는 새 데이터베이스를 사용 하 여 Code First 개발에 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-236">In this walkthrough we looked at Code First development using a new database.</span></span> <span data-ttu-id="bb171-237">클래스를 사용 하 여 모델을 정의 하 고 데이터베이스를 만들고 저장 하 고 데이터를 검색 하는 모델을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-237">We defined a model using classes then used that model to create a database and store and retrieve data.</span></span> <span data-ttu-id="bb171-238">데이터베이스가 만들어진 후로 발전 한 모델 스키마를 변경 하려면 Code First 마이그레이션을 사용 했습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-238">Once the database was created we used Code First Migrations to change the schema as our model evolved.</span></span> <span data-ttu-id="bb171-239">데이터 주석 및 Fluent API를 사용 하 여 모델을 구성 하는 방법을 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="bb171-239">We also saw how to configure a model using Data Annotations and the Fluent API.</span></span>
