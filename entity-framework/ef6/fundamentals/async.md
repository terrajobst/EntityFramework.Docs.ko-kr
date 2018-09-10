---
title: 비동기 쿼리 및 저장-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 35604fc16ea37415d39801831aa162d0d42c2a2f
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250753"
---
# <a name="async-query-and-save"></a><span data-ttu-id="8a052-102">비동기 쿼리 및 저장</span><span class="sxs-lookup"><span data-stu-id="8a052-102">Async query and save</span></span>
> [!NOTE]
> <span data-ttu-id="8a052-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="8a052-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="8a052-105">EF6 비동기 쿼리를 사용 하 여 저장 지원을 도입 합니다 [async 및 await 키워드](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) .NET 4.5에서 도입 된 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-105">EF6 introduced support for asynchronous query and save using the [async and await keywords](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) that were introduced in .NET 4.5.</span></span> <span data-ttu-id="8a052-106">일부 응용 프로그램은 비동기에서 이점을 얻을 수, 하는 동안 장기 실행, 네트워크 또는 O 바인딩된 작업을 처리 하는 경우 클라이언트 응답성 및 서버 확장성을 개선 하기 위해 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-106">While not all applications may benefit from asynchrony, it can be used to improve client responsiveness and server scalability when handling long-running, network or I/O-bound tasks.</span></span>

## <a name="when-to-really-use-async"></a><span data-ttu-id="8a052-107">실제로 비동기를 사용 하는 경우</span><span class="sxs-lookup"><span data-stu-id="8a052-107">When to really use async</span></span>

<span data-ttu-id="8a052-108">이 연습의 목적은 쉽게 비동기 및 동기 프로그램 실행 간의 차이 알 수 있도록 하는 방식으로 비동기 개념을 소개 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-108">The purpose of this walkthrough is to introduce the async concepts in a way that makes it easy to observe the difference between asynchronous and synchronous program execution.</span></span> <span data-ttu-id="8a052-109">이 연습에서는 아닙니다 주요 시나리오 중 하나를 보여 주기 위해 혜택을 제공 하는 비동기 프로그래밍 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-109">This walkthrough is not intended to illustrate any of the key scenarios where async programming provides benefits.</span></span>

<span data-ttu-id="8a052-110">비동기 프로그래밍은 주로 계산 든 필요 하지 않은 작업까지 기다리는 동안 다른 작업을 수행 하려면 현재 관리 되는 스레드 (스레드가 실행 중인.NET 코드)를 확보에서 관리 되는 스레드입니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-110">Async programming is primarily focused on freeing up the current managed thread (thread running .NET code) to do other work while it waits for an operation that does not require any compute time from a managed thread.</span></span> <span data-ttu-id="8a052-111">예를 들어 데이터베이스 엔진은 쿼리를 처리 하는 동안 없는.NET 코드에 의해 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-111">For example, whilst the database engine is processing a query there is nothing to be done by .NET code.</span></span>

<span data-ttu-id="8a052-112">클라이언트 응용 프로그램 (WinForms, WPF 등)에서 현재 스레드에서 비동기 작업이 수행 되는 동안 UI를 응답 가능한 상태로 유지 하려면 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-112">In client applications (WinForms, WPF, etc.) the current thread can be used to keep the UI responsive while the async operation is performed.</span></span> <span data-ttu-id="8a052-113">-들어오는 다른 요청을 처리 하는 데 사용할 수는 스레드 서버 응용 프로그램 (ASP.NET 등)에서 메모리 사용량을 낮춥니다. 하거나 서버의 처리량을 늘릴 수 있습니다이.</span><span class="sxs-lookup"><span data-stu-id="8a052-113">In server applications (ASP.NET etc.) the thread can be used to process other incoming requests - this can reduce memory usage and/or increase throughput of the server.</span></span>

<span data-ttu-id="8a052-114">비동기를 사용 하 여 대부분의 응용 프로그램에서에 눈에 띄는 경우 이점은 없고 있고도 저하 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-114">In most applications using async will have no noticeable benefits and even could be detrimental.</span></span> <span data-ttu-id="8a052-115">테스트, 프로 파일링 및 상식적인을 사용 하 여 커밋하기 전에 특정 시나리오에서 비동기의 영향을 측정 하기.</span><span class="sxs-lookup"><span data-stu-id="8a052-115">Use tests, profiling and common sense to measure the impact of async in your particular scenario before committing to it.</span></span>

<span data-ttu-id="8a052-116">몇 가지 추가 비동기에 대 한 학습 리소스는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-116">Here are some more resources to learn about async:</span></span>

-   [<span data-ttu-id="8a052-117">Brandon Bray의 async/await.NET 4.5 개요</span><span class="sxs-lookup"><span data-stu-id="8a052-117">Brandon Bray’s overview of async/await in .NET 4.5</span></span>](http://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   <span data-ttu-id="8a052-118">[비동기 프로그래밍](https://msdn.microsoft.com/library/hh191443.aspx) MSDN 라이브러리에서 페이지</span><span class="sxs-lookup"><span data-stu-id="8a052-118">[Asynchronous Programming](https://msdn.microsoft.com/library/hh191443.aspx) pages in the MSDN Library</span></span>
-   <span data-ttu-id="8a052-119">[비동기를 사용 하 여 ASP.NET 웹 응용 프로그램 빌드 방법](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (향상 된 서버 처리량 데모 포함)</span><span class="sxs-lookup"><span data-stu-id="8a052-119">[How to Build ASP.NET Web Applications Using Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (includes a demo of increased server throughput)</span></span>

## <a name="create-the-model"></a><span data-ttu-id="8a052-120">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="8a052-120">Create the model</span></span>

<span data-ttu-id="8a052-121">하지만 사용 합니다 [Code First 워크플로](~/ef6/modeling/code-first/workflows/new-database.md) 모델 EF 디자이너를 사용 하 여 만든 라이선스 포함 하는 모든 EF 모델을 사용 하 여 비동기 기능을 작동 데이터베이스를 생성 하려면.</span><span class="sxs-lookup"><span data-stu-id="8a052-121">We’ll be using the [Code First workflow](~/ef6/modeling/code-first/workflows/new-database.md) to create our model and generate the database, however the asynchronous functionality will work with all EF models including those created with the EF Designer.</span></span>

-   <span data-ttu-id="8a052-122">콘솔 응용 프로그램을 만들고 **AsyncDemo**</span><span class="sxs-lookup"><span data-stu-id="8a052-122">Create a Console Application and call it **AsyncDemo**</span></span>
-   <span data-ttu-id="8a052-123">EntityFramework NuGet 패키지 추가</span><span class="sxs-lookup"><span data-stu-id="8a052-123">Add the EntityFramework NuGet package</span></span>
    -   <span data-ttu-id="8a052-124">솔루션 탐색기에서 마우스 오른쪽 단추로 클릭 합니다 **AsyncDemo** 프로젝트</span><span class="sxs-lookup"><span data-stu-id="8a052-124">In Solution Explorer, right-click on the **AsyncDemo** project</span></span>
    -   <span data-ttu-id="8a052-125">선택 **NuGet 패키지 관리...**</span><span class="sxs-lookup"><span data-stu-id="8a052-125">Select **Manage NuGet Packages…**</span></span>
    -   <span data-ttu-id="8a052-126">NuGet 패키지 관리 대화 상자에서 선택 합니다 **Online** 탭을 선택 합니다 **EntityFramework** 패키지</span><span class="sxs-lookup"><span data-stu-id="8a052-126">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
    -   <span data-ttu-id="8a052-127">클릭 **설치**</span><span class="sxs-lookup"><span data-stu-id="8a052-127">Click **Install**</span></span>
-   <span data-ttu-id="8a052-128">추가 된 **Model.cs** 다음 구현 클래스</span><span class="sxs-lookup"><span data-stu-id="8a052-128">Add a **Model.cs** class with the following implementation</span></span>

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

 

## <a name="create-a-synchronous-program"></a><span data-ttu-id="8a052-129">동기 프로그램 만들기</span><span class="sxs-lookup"><span data-stu-id="8a052-129">Create a synchronous program</span></span>

<span data-ttu-id="8a052-130">이제 EF 모델을 만들었으므로 일부 데이터 액세스를 수행 하는 데 사용 하는 일부 코드를 작성해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-130">Now that we have an EF model, let's write some code that uses it to perform some data access.</span></span>

-   <span data-ttu-id="8a052-131">내용을 바꿉니다 **Program.cs** 다음 코드를 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="8a052-131">Replace the contents of **Program.cs** with the following code</span></span>

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

                Console.WriteLine();
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
                    db.SaveChanges();

                    // Query for all blogs ordered by name
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine();
                    Console.WriteLine("All blogs:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="8a052-132">이 코드는 호출을 **PerformDatabaseOperations** 새 저장 하는 메서드 **블로그** 및 데이터베이스에 다음 검색 모든 **블로그** 데이터베이스에서 하 고 출력 하는 **콘솔**합니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-132">This code calls the **PerformDatabaseOperations** method which saves a new **Blog** to the database and then retrieves all **Blogs** from the database and prints them to the **Console**.</span></span> <span data-ttu-id="8a052-133">그러면 프로그램 일의 따옴표를 기록 합니다 **콘솔**.</span><span class="sxs-lookup"><span data-stu-id="8a052-133">After this, the program writes a quote of the day to the **Console**.</span></span>

<span data-ttu-id="8a052-134">동기 코드 이므로 프로그램을 실행 하면 다음 실행 흐름을 확인할 수 있습니다 것:</span><span class="sxs-lookup"><span data-stu-id="8a052-134">Since the code is synchronous, we can observe the following execution flow when we run the program:</span></span>

1.  <span data-ttu-id="8a052-135">**SaveChanges** 새 적용할 시작 **블로그** 데이터베이스로</span><span class="sxs-lookup"><span data-stu-id="8a052-135">**SaveChanges** begins to push the new **Blog** to the database</span></span>
2.  <span data-ttu-id="8a052-136">**SaveChanges** 완료</span><span class="sxs-lookup"><span data-stu-id="8a052-136">**SaveChanges** completes</span></span>
3.  <span data-ttu-id="8a052-137">모든 쿼리 **블로그** 데이터베이스로 전송 됩니다</span><span class="sxs-lookup"><span data-stu-id="8a052-137">Query for all **Blogs** is sent to the database</span></span>
4.  <span data-ttu-id="8a052-138">쿼리를 반환 하 고 결과에 기록 되며 **콘솔**</span><span class="sxs-lookup"><span data-stu-id="8a052-138">Query returns and results are written to **Console**</span></span>
5.  <span data-ttu-id="8a052-139">날짜의 견적에 쓸 때 **콘솔**</span><span class="sxs-lookup"><span data-stu-id="8a052-139">Quote of the day is written to **Console**</span></span>

![동기화 출력](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a><span data-ttu-id="8a052-141">비동기 만들기</span><span class="sxs-lookup"><span data-stu-id="8a052-141">Making it asynchronous</span></span>

<span data-ttu-id="8a052-142">이제 프로그램을 실행 했으므로 새 비동기를 사용 하 고 await 키워드 시작할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-142">Now that we have our program up and running, we can begin making use of the new async and await keywords.</span></span> <span data-ttu-id="8a052-143">Program.cs에 다음 변경 내용을 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-143">We've made the following changes to Program.cs</span></span>

1.  <span data-ttu-id="8a052-144">2 번 줄: using 문을 합니다 **System.Data.Entity** 네임 스페이스 액세스를 제공 EF 비동기 확장 메서드.</span><span class="sxs-lookup"><span data-stu-id="8a052-144">Line 2: The using statement for the **System.Data.Entity** namespace gives us access to the EF async extension methods.</span></span>
2.  <span data-ttu-id="8a052-145">줄 4:를 사용 하 여 문을 합니다 **System.Threading.Tasks** 네임 스페이스를 사용할 수 있습니다 합니다 **작업** 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-145">Line 4: The using statement for the **System.Threading.Tasks** namespace allows us to use the **Task** type.</span></span>
3.  <span data-ttu-id="8a052-146">줄 12 & 18:의 진행률을 모니터링 하는 작업으로 캡처하는 것 **PerformSomeDatabaseOperations** (줄 12) 및 다음이 프로그램 실행 차단 작업 완료 한 번에 모든 작업 프로그램 (18 번 줄) 수행에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-146">Line 12 & 18: We are capturing as task that monitors the progress of **PerformSomeDatabaseOperations** (line 12) and then blocking program execution for this task to complete once all the work for the program is done (line 18).</span></span>
4.  <span data-ttu-id="8a052-147">25 번 줄: 업데이트 되었습니다 **PerformSomeDatabaseOperations** 로 표시 되어야 **비동기** 반환 하 고는 **작업**합니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-147">Line 25: We've update **PerformSomeDatabaseOperations** to be marked as **async** and return a **Task**.</span></span>
5.  <span data-ttu-id="8a052-148">줄 35:는 이제 SaveChanges의 비동기 버전을 호출 하 고 해당 완료 대기 중입니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-148">Line 35: We're now calling the Async version of SaveChanges and awaiting it's completion.</span></span>
6.  <span data-ttu-id="8a052-149">줄 42:는 이제 ToList의 비동기 버전을 호출 하 고 결과에서 대기 중입니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-149">Line 42: We're now calling hte Async version of ToList and awaiting on the result.</span></span>

<span data-ttu-id="8a052-150">System.Data.Entity 네임 스페이스에서 사용 가능한 확장 메서드의 전체 목록을, QueryableExtensions 클래스를 가리킵니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-150">For a comprehensive list of available extension methods in the System.Data.Entity namespace, refer to the QueryableExtensions class.</span></span> <span data-ttu-id="8a052-151">*또한 해야 "System.Data.Entity"를 사용 하 여 추가 하 여 문입니다.*</span><span class="sxs-lookup"><span data-stu-id="8a052-151">*You’ll also need to add “using System.Data.Entity” to your using statements.*</span></span>

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

<span data-ttu-id="8a052-152">이제 코드의 비동기, 프로그램을 실행 하면 다양 한 실행 흐름을 확인할 수 있습니다 것:</span><span class="sxs-lookup"><span data-stu-id="8a052-152">Now that the code is asyncronous, we can observe a different execution flow when we run the program:</span></span>

1.  <span data-ttu-id="8a052-153">**SaveChanges** 새 적용할 시작 **블로그** 데이터베이스로 *명령을 보낸 후 데이터베이스에 더 이상 현재 관리 되는 스레드에서 데 필요한 시간을 계산 합니다. 합니다 **PerformDatabaseOperations** 메서드 (경우에 실행 완료 되지 않은)를 반환 하 고 Main 메서드에서 프로그램 흐름을 계속 합니다.*</span><span class="sxs-lookup"><span data-stu-id="8a052-153">**SaveChanges** begins to push the new **Blog** to the database *Once the command is sent to the database no more compute time is needed on the current managed thread. The **PerformDatabaseOperations** method returns (even though it hasn't finished executing) and program flow in the Main method continues.*</span></span>
2.  <span data-ttu-id="8a052-154">**하루 중 견적 콘솔에 기록 됩니다**
    \*대기에서 관리 되는 스레드는 차단 Main 메서드에서 수행할 작업이 더 이상 이므로 데이터베이스 작업이 완료 될 때까지 호출 합니다. 완료 되 면 나머지 우리의 **PerformDatabaseOperations** \* 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-154">**Quote of the day is written to Console**
*Since there is no more work to do in the Main method, the managed thread is blocked on the Wait call until the database operation completes. Once it completes, the remainder of our **PerformDatabaseOperations*** will be executed.</span></span>
3.  <span data-ttu-id="8a052-155">**SaveChanges** 완료</span><span class="sxs-lookup"><span data-stu-id="8a052-155">**SaveChanges** completes</span></span>
4.  <span data-ttu-id="8a052-156">모든 쿼리 **블로그** 데이터베이스로 전송 됩니다 *다시 관리 되는 스레드는 데이터베이스의 쿼리 처리 되는 동안 다른 작업을 수행할 수 있습니다. 다른 모든 실행 완료 후 스레드가 대기 호출 하지만 중단만 됩니다.*</span><span class="sxs-lookup"><span data-stu-id="8a052-156">Query for all **Blogs** is sent to the database *Again, the managed thread is free to do other work while the query is processed in the database. Since all other execution has completed, the thread will just halt on the Wait call though.*</span></span>
5.  <span data-ttu-id="8a052-157">쿼리를 반환 하 고 결과에 기록 되며 **콘솔**</span><span class="sxs-lookup"><span data-stu-id="8a052-157">Query returns and results are written to **Console**</span></span>

![비동기 출력](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a><span data-ttu-id="8a052-159">코딩할</span><span class="sxs-lookup"><span data-stu-id="8a052-159">The takeaway</span></span>

<span data-ttu-id="8a052-160">이제 살펴본 것이 얼마나 쉬운지 확인 하려면 EF의 비동기 메서드를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-160">We now saw how easy it is to make use of EF’s asynchronous methods.</span></span> <span data-ttu-id="8a052-161">비동기의 장점은 매우 간단한 콘솔 앱을 사용 하 여 명확 하지 않을 수 있습니다., 있지만 이러한 동일한 전략 또는 있는 경우 장기 실행 또는 네트워크 바인딩된 작업 수 그렇지 않은 경우 응용 프로그램을 차단 하면 많은 수의 스레드를 적용할 수 있습니다. 메모리 사용 공간을 늘립니다.</span><span class="sxs-lookup"><span data-stu-id="8a052-161">Although the advantages of async may not be very apparent with a simple console app, these same strategies can be applied in situations where long-running or network-bound activities might otherwise block the application, or cause a large number of threads to increase the memory footprint.</span></span>
