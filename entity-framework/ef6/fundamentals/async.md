---
title: 비동기 쿼리 및 저장-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 0642dc13e7aa3906fa1495031c62701fc16f0192
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414862"
---
# <a name="async-query-and-save"></a><span data-ttu-id="27150-102">비동기 쿼리 및 저장</span><span class="sxs-lookup"><span data-stu-id="27150-102">Async query and save</span></span>
> [!NOTE]
> <span data-ttu-id="27150-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="27150-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="27150-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="27150-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="27150-105">EF6는 비동기 쿼리에 대 한 지원을 도입 하 고 .NET 4.5에 도입 된 [async 및 wait 키워드](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) 를 사용 하 여 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="27150-105">EF6 introduced support for asynchronous query and save using the [async and await keywords](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) that were introduced in .NET 4.5.</span></span> <span data-ttu-id="27150-106">모든 응용 프로그램은 비동기의 이점을 누릴 수 있지만 장기 실행, 네트워크 또는 i/o 바인딩된 작업을 처리할 때 클라이언트 응답성 및 서버 확장성을 향상 시키는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="27150-106">While not all applications may benefit from asynchrony, it can be used to improve client responsiveness and server scalability when handling long-running, network or I/O-bound tasks.</span></span>

## <a name="when-to-really-use-async"></a><span data-ttu-id="27150-107">비동기를 사용 하는 경우</span><span class="sxs-lookup"><span data-stu-id="27150-107">When to really use async</span></span>

<span data-ttu-id="27150-108">이 연습의 목적은 비동기 및 동기 프로그램 실행 간의 차이를 쉽게 확인할 수 있도록 하는 방식으로 비동기 개념을 소개 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="27150-108">The purpose of this walkthrough is to introduce the async concepts in a way that makes it easy to observe the difference between asynchronous and synchronous program execution.</span></span> <span data-ttu-id="27150-109">이 연습은 비동기 프로그래밍에서 혜택을 제공 하는 주요 시나리오를 설명 하기 위한 것이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="27150-109">This walkthrough is not intended to illustrate any of the key scenarios where async programming provides benefits.</span></span>

<span data-ttu-id="27150-110">비동기 프로그래밍은 기본적으로 관리 되는 스레드의 계산 시간이 필요 하지 않은 작업을 기다리는 동안 다른 작업을 수행 하기 위해 현재 관리 되는 스레드 (.NET 코드를 실행 하는 스레드)를 해제 하는 데 중점을 두었습니다.</span><span class="sxs-lookup"><span data-stu-id="27150-110">Async programming is primarily focused on freeing up the current managed thread (thread running .NET code) to do other work while it waits for an operation that does not require any compute time from a managed thread.</span></span> <span data-ttu-id="27150-111">예를 들어 데이터베이스 엔진이 쿼리를 처리 하는 동안 .NET 코드에서 수행할 작업은 없습니다.</span><span class="sxs-lookup"><span data-stu-id="27150-111">For example, whilst the database engine is processing a query there is nothing to be done by .NET code.</span></span>

<span data-ttu-id="27150-112">클라이언트 응용 프로그램 (WinForms, WPF 등)에서 현재 스레드를 사용 하 여 비동기 작업을 수행 하는 동안 UI의 응답성을 유지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="27150-112">In client applications (WinForms, WPF, etc.) the current thread can be used to keep the UI responsive while the async operation is performed.</span></span> <span data-ttu-id="27150-113">서버 응용 프로그램 (ASP.NET 등)에서 스레드를 사용 하 여 다른 들어오는 요청을 처리할 수 있습니다. 이렇게 하면 메모리 사용량을 줄이거나 서버의 처리량을 늘릴 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="27150-113">In server applications (ASP.NET etc.) the thread can be used to process other incoming requests - this can reduce memory usage and/or increase throughput of the server.</span></span>

<span data-ttu-id="27150-114">비동기를 사용 하는 대부분의 응용 프로그램에서는 눈에 띄는 이점이 없으며,이는 좋지 않을 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="27150-114">In most applications using async will have no noticeable benefits and even could be detrimental.</span></span> <span data-ttu-id="27150-115">테스트, 프로 파일링 및 일반적인 의미를 사용 하 여 커밋하기 전에 특정 시나리오에서 비동기의 영향을 측정 합니다.</span><span class="sxs-lookup"><span data-stu-id="27150-115">Use tests, profiling and common sense to measure the impact of async in your particular scenario before committing to it.</span></span>

<span data-ttu-id="27150-116">다음은 async에 대해 자세히 알아볼 수 있는 몇 가지 추가 리소스입니다.</span><span class="sxs-lookup"><span data-stu-id="27150-116">Here are some more resources to learn about async:</span></span>

-   [<span data-ttu-id="27150-117">Brandon Bray 's .NET 4.5의 async/wait 개요</span><span class="sxs-lookup"><span data-stu-id="27150-117">Brandon Bray’s overview of async/await in .NET 4.5</span></span>](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   <span data-ttu-id="27150-118">MSDN Library의 [비동기 프로그래밍](https://msdn.microsoft.com/library/hh191443.aspx) 페이지</span><span class="sxs-lookup"><span data-stu-id="27150-118">[Asynchronous Programming](https://msdn.microsoft.com/library/hh191443.aspx) pages in the MSDN Library</span></span>
-   <span data-ttu-id="27150-119">[Async를 사용 하 여 ASP.NET 웹 응용 프로그램을 빌드하는 방법](https://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (서버 처리량이 증가 하는 데모 포함)</span><span class="sxs-lookup"><span data-stu-id="27150-119">[How to Build ASP.NET Web Applications Using Async](https://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (includes a demo of increased server throughput)</span></span>

## <a name="create-the-model"></a><span data-ttu-id="27150-120">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="27150-120">Create the model</span></span>

<span data-ttu-id="27150-121">[Code First 워크플로](~/ef6/modeling/code-first/workflows/new-database.md) 를 사용 하 여 모델을 만들고 데이터베이스를 생성 하지만, 비동기 기능은 ef Designer를 사용 하 여 만든 것을 비롯 한 모든 EF 모델에서 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="27150-121">We’ll be using the [Code First workflow](~/ef6/modeling/code-first/workflows/new-database.md) to create our model and generate the database, however the asynchronous functionality will work with all EF models including those created with the EF Designer.</span></span>

-   <span data-ttu-id="27150-122">콘솔 응용 프로그램을 만들고이를 **Asyncdemo** 로 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="27150-122">Create a Console Application and call it **AsyncDemo**</span></span>
-   <span data-ttu-id="27150-123">EntityFramework NuGet 패키지 추가</span><span class="sxs-lookup"><span data-stu-id="27150-123">Add the EntityFramework NuGet package</span></span>
    -   <span data-ttu-id="27150-124">솔루션 탐색기에서 **Asyncdemo** 프로젝트를 마우스 오른쪽 단추로 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="27150-124">In Solution Explorer, right-click on the **AsyncDemo** project</span></span>
    -   <span data-ttu-id="27150-125">**NuGet 패키지 관리 ...** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="27150-125">Select **Manage NuGet Packages…**</span></span>
    -   <span data-ttu-id="27150-126">NuGet 패키지 관리 대화 상자에서 **온라인** 탭을 선택 하 고 **entityframework** 패키지를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="27150-126">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
    -   <span data-ttu-id="27150-127">**설치**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="27150-127">Click **Install**</span></span>
-   <span data-ttu-id="27150-128">다음 구현으로 **Model.cs** 클래스를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="27150-128">Add a **Model.cs** class with the following implementation</span></span>

``` csharp
    using System.Collections.Generic;
    using System.Data.Entity;

    namespace AsyncDemo
    {
        public class BloggingContext : DbContext
        {
            public DbSet<Blog> Blogs { get; set; }
            public DbSet<Post> Posts { get; set; }
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
    }
```

 

## <a name="create-a-synchronous-program"></a><span data-ttu-id="27150-129">동기 프로그램 만들기</span><span class="sxs-lookup"><span data-stu-id="27150-129">Create a synchronous program</span></span>

<span data-ttu-id="27150-130">이제 EF 모델이 있으므로 일부 데이터 액세스를 수행 하는 데 사용 하는 몇 가지 코드를 작성해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="27150-130">Now that we have an EF model, let's write some code that uses it to perform some data access.</span></span>

-   <span data-ttu-id="27150-131">**Program.cs** 의 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="27150-131">Replace the contents of **Program.cs** with the following code</span></span>

``` csharp
    using System;
    using System.Linq;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                PerformDatabaseOperations();

                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static void PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    Console.WriteLine("Calling SaveChanges.");
                    db.SaveChanges();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="27150-132">이 코드는 데이터베이스에 새 **블로그** 를 저장 한 다음 데이터베이스에서 모든 **블로그** 를 검색 하 여 **콘솔**에 출력 하는 **PerformDatabaseOperations** 메서드를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="27150-132">This code calls the **PerformDatabaseOperations** method which saves a new **Blog** to the database and then retrieves all **Blogs** from the database and prints them to the **Console**.</span></span> <span data-ttu-id="27150-133">그러면 프로그램에서 요일의 견적을 **콘솔**에 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="27150-133">After this, the program writes a quote of the day to the **Console**.</span></span>

<span data-ttu-id="27150-134">코드는 동기식 이므로 프로그램을 실행할 때 다음과 같은 실행 흐름을 관찰할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="27150-134">Since the code is synchronous, we can observe the following execution flow when we run the program:</span></span>

1.  <span data-ttu-id="27150-135">**SaveChanges** 는 데이터베이스에 새 **블로그** 를 푸시하는 것으로 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="27150-135">**SaveChanges** begins to push the new **Blog** to the database</span></span>
2.  <span data-ttu-id="27150-136">**SaveChanges** 완료</span><span class="sxs-lookup"><span data-stu-id="27150-136">**SaveChanges** completes</span></span>
3.  <span data-ttu-id="27150-137">모든 **블로그의** 쿼리가 데이터베이스로 전송 됨</span><span class="sxs-lookup"><span data-stu-id="27150-137">Query for all **Blogs** is sent to the database</span></span>
4.  <span data-ttu-id="27150-138">쿼리가 반환 되 고 결과가 **콘솔** 에 기록 됩니다.</span><span class="sxs-lookup"><span data-stu-id="27150-138">Query returns and results are written to **Console**</span></span>
5.  <span data-ttu-id="27150-139">요일의 견적이 **콘솔** 에 기록 됩니다.</span><span class="sxs-lookup"><span data-stu-id="27150-139">Quote of the day is written to **Console**</span></span>

![동기화 출력](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a><span data-ttu-id="27150-141">비동기 설정</span><span class="sxs-lookup"><span data-stu-id="27150-141">Making it asynchronous</span></span>

<span data-ttu-id="27150-142">이제 프로그램을 시작 하 고 실행 했으므로 새 async 및 wait 키워드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="27150-142">Now that we have our program up and running, we can begin making use of the new async and await keywords.</span></span> <span data-ttu-id="27150-143">Program.cs을 다음과 같이 변경 했습니다.</span><span class="sxs-lookup"><span data-stu-id="27150-143">We've made the following changes to Program.cs</span></span>

1.  <span data-ttu-id="27150-144">줄 2: **system.object** 네임 스페이스에 대 한 USING 문은 EF async 확장 메서드에 대 한 액세스를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="27150-144">Line 2: The using statement for the **System.Data.Entity** namespace gives us access to the EF async extension methods.</span></span>
2.  <span data-ttu-id="27150-145">줄 4: **system.object** 네임 스페이스에 대 한 using 문을 사용 하면 **작업** 유형을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="27150-145">Line 4: The using statement for the **System.Threading.Tasks** namespace allows us to use the **Task** type.</span></span>
3.  <span data-ttu-id="27150-146">줄 12 & 18: **PerformSomeDatabaseOperations** (12 줄)의 진행률을 모니터링 하 고 프로그램 실행을 차단 하 여 프로그램의 모든 작업이 완료 되 면 (줄 18)이 작업에 대 한 프로그램 실행을 차단 하는 작업으로 캡처하는 중입니다.</span><span class="sxs-lookup"><span data-stu-id="27150-146">Line 12 & 18: We are capturing as task that monitors the progress of **PerformSomeDatabaseOperations** (line 12) and then blocking program execution for this task to complete once all the work for the program is done (line 18).</span></span>
4.  <span data-ttu-id="27150-147">25 줄: **async** 로 표시 되도록 **PerformSomeDatabaseOperations** 를 업데이트 하 고 **작업**을 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="27150-147">Line 25: We've update **PerformSomeDatabaseOperations** to be marked as **async** and return a **Task**.</span></span>
5.  <span data-ttu-id="27150-148">35 줄: 이제 비동기 버전의 SaveChanges를 호출 하 고 완료를 기다리는 중입니다.</span><span class="sxs-lookup"><span data-stu-id="27150-148">Line 35: We're now calling the Async version of SaveChanges and awaiting it's completion.</span></span>
6.  <span data-ttu-id="27150-149">줄 42: 현재는 ToList의 비동기 버전을 호출 하 고 결과를 기다리고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="27150-149">Line 42: We're now calling the Async version of ToList and awaiting on the result.</span></span>

<span data-ttu-id="27150-150">System.object 네임 스페이스에서 사용 가능한 확장 메서드의 포괄적인 목록은 QueryableExtensions 클래스를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="27150-150">For a comprehensive list of available extension methods in the System.Data.Entity namespace, refer to the QueryableExtensions class.</span></span> <span data-ttu-id="27150-151">*또한 using 문에 "System.object 사용"을 추가 해야 합니다.*</span><span class="sxs-lookup"><span data-stu-id="27150-151">*You’ll also need to add “using System.Data.Entity” to your using statements.*</span></span>

``` csharp
    using System;
    using System.Data.Entity;
    using System.Linq;
    using System.Threading.Tasks;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                var task = PerformDatabaseOperations();

                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                task.Wait();

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static async Task PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    Console.WriteLine("Calling SaveChanges.");
                    await db.SaveChangesAsync();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = await (from b in db.Blogs
                                orderby b.Name
                                select b).ToListAsync();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" - " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="27150-152">이제 코드는 비동기 이므로 프로그램을 실행할 때 다른 실행 흐름을 관찰할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="27150-152">Now that the code is asynchronous, we can observe a different execution flow when we run the program:</span></span>

1. <span data-ttu-id="27150-153">**SaveChanges** 는 데이터베이스에 새 **블로그** 를 푸시하는 것으로 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="27150-153">**SaveChanges** begins to push the new **Blog** to the database</span></span>  
    <span data-ttu-id="27150-154">*명령이 데이터베이스로 전송 되 면 현재 관리 되는 스레드에 더 이상 계산 시간이 필요 하지 않습니다. **PerformDatabaseOperations** 메서드는 실행이 완료 되지 않은 경우에도를 반환 하 고 Main 메서드의 프로그램 흐름은 계속 됩니다.*</span><span class="sxs-lookup"><span data-stu-id="27150-154">*Once the command is sent to the database no more compute time is needed on the current managed thread. The **PerformDatabaseOperations** method returns (even though it hasn't finished executing) and program flow in the Main method continues.*</span></span>
2. <span data-ttu-id="27150-155">**요일의 견적이 콘솔에 기록 됩니다.**</span><span class="sxs-lookup"><span data-stu-id="27150-155">**Quote of the day is written to Console**</span></span>  
    <span data-ttu-id="27150-156">*Main 메서드에서 수행할 작업이 더 이상 없으므로 관리 되는 스레드는 데이터베이스 작업이 완료 될 때까지 대기 호출에서 차단 됩니다. 완료 되 면 **PerformDatabaseOperations** 의 나머지가 실행 됩니다.*</span><span class="sxs-lookup"><span data-stu-id="27150-156">*Since there is no more work to do in the Main method, the managed thread is blocked on the Wait call until the database operation completes. Once it completes, the remainder of our **PerformDatabaseOperations** will be executed.*</span></span>
3.  <span data-ttu-id="27150-157">**SaveChanges** 완료</span><span class="sxs-lookup"><span data-stu-id="27150-157">**SaveChanges** completes</span></span>  
4.  <span data-ttu-id="27150-158">모든 **블로그의** 쿼리가 데이터베이스로 전송 됨</span><span class="sxs-lookup"><span data-stu-id="27150-158">Query for all **Blogs** is sent to the database</span></span>  
    <span data-ttu-id="27150-159">*또한 데이터베이스에서 쿼리가 처리 되는 동안 관리 되는 스레드는 다른 작업을 수행할 수 있습니다. 다른 모든 실행이 완료 된 후에도 스레드는 대기 호출에서 중단 됩니다.*</span><span class="sxs-lookup"><span data-stu-id="27150-159">*Again, the managed thread is free to do other work while the query is processed in the database. Since all other execution has completed, the thread will just halt on the Wait call though.*</span></span>
5.  <span data-ttu-id="27150-160">쿼리가 반환 되 고 결과가 **콘솔** 에 기록 됩니다.</span><span class="sxs-lookup"><span data-stu-id="27150-160">Query returns and results are written to **Console**</span></span>  

![비동기 출력](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a><span data-ttu-id="27150-162">요점은</span><span class="sxs-lookup"><span data-stu-id="27150-162">The takeaway</span></span>

<span data-ttu-id="27150-163">이제 EF의 비동기 메서드를 사용 하는 것이 얼마나 쉬운지 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="27150-163">We now saw how easy it is to make use of EF’s asynchronous methods.</span></span> <span data-ttu-id="27150-164">비동기의 이점은 간단한 콘솔 앱에서 매우 명확 하지 않을 수 있지만, 장기 실행 또는 네트워크 바인딩된 작업이 응용 프로그램을 차단 하거나 많은 수의 스레드를 발생 시킬 수 있는 경우에도 이와 동일한 전략을 적용할 수 있습니다. 메모리 사용 공간을 늘립니다.</span><span class="sxs-lookup"><span data-stu-id="27150-164">Although the advantages of async may not be very apparent with a simple console app, these same strategies can be applied in situations where long-running or network-bound activities might otherwise block the application, or cause a large number of threads to increase the memory footprint.</span></span>
