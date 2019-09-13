---
title: 클라이언트 및 서버 평가 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: cb207d9e1b1004a4084dd6fc66712183b5bdd5dc
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921709"
---
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="bbdac-102">클라이언트 및 서버 평가</span><span class="sxs-lookup"><span data-stu-id="bbdac-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="bbdac-103">Entity Framework Core는 클라이언트에서 평가되는 쿼리 부분과 데이터베이스로 푸시되는 쿼리 부분을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="bbdac-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="bbdac-104">데이터베이스에서 평가될 쿼리의 부분은 데이터베이스 공급자가 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="bbdac-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="bbdac-105">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbdac-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="bbdac-106">클라이언트 평가</span><span class="sxs-lookup"><span data-stu-id="bbdac-106">Client evaluation</span></span>

<span data-ttu-id="bbdac-107">다음 예제에서는 도우미 메서드가 SQL Server 데이터베이스에 반환되는 블로그의 URL을 표준화하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="bbdac-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="bbdac-108">SQL Server 공급자는 이 메서드가 구현되는 방법에 대한 인사이트가 없기 때문에 이 메서드를 SQL로 변환할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="bbdac-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="bbdac-109">쿼리의 다른 모든 측면은 데이터베이스에서 평가되지만 이 메서드를 통해 반환된 `URL`을 전달하는 작업은 클라이언트에서 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="bbdac-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs?highlight=6)] -->
``` csharp
var blogs = context.Blogs
    .OrderByDescending(blog => blog.Rating)
    .Select(blog => new
    {
        Id = blog.BlogId,
        Url = StandardizeUrl(blog.Url)
    })
    .ToList();
```

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
public static string StandardizeUrl(string url)
{
    url = url.ToLower();

    if (!url.StartsWith("http://"))
    {
        url = string.Concat("http://", url);
    }

    return url;
}
```

## <a name="client-evaluation-performance-issues"></a><span data-ttu-id="bbdac-110">클라이언트 평가 성능 문제</span><span class="sxs-lookup"><span data-stu-id="bbdac-110">Client evaluation performance issues</span></span>

<span data-ttu-id="bbdac-111">클라이언트 평가는 매우 유용할 수 있지만 경우에 따라 성능이 저하될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbdac-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="bbdac-112">이제 도우미 메서드가 필터에서 사용되는 다음 쿼리를 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="bbdac-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="bbdac-113">데이터베이스에서는 이 작업을 수행할 수 없으므로 모든 데이터를 메모리로 풀한 다음, 클라이언트에서 필터를 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="bbdac-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="bbdac-114">데이터양과 해당 데이터가 필터링되는 양에 따라 성능이 저하될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbdac-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a><span data-ttu-id="bbdac-115">클라이언트 평가 로깅</span><span class="sxs-lookup"><span data-stu-id="bbdac-115">Client evaluation logging</span></span>

<span data-ttu-id="bbdac-116">기본적으로 EF Core는 클라이언트 평가가 수행될 때 표시되는 경고를 로깅합니다.</span><span class="sxs-lookup"><span data-stu-id="bbdac-116">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="bbdac-117">로깅 출력 보기에 대한 자세한 내용은 [로깅](../miscellaneous/logging.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bbdac-117">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a><span data-ttu-id="bbdac-118">선택적 동작: 클라이언트 평가에 대한 예외를 발생시킵니다.</span><span class="sxs-lookup"><span data-stu-id="bbdac-118">Optional behavior: throw an exception for client evaluation</span></span>

<span data-ttu-id="bbdac-119">클라이언트 평가가 수행될 때 동작을 throw나 아무 작업도 하지 않음으로 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbdac-119">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="bbdac-120">이 작업은 컨텍스트에 대한 옵션을 설정할 때 수행하며, 일반적으로 `DbContext.OnConfiguring`에서나 `Startup.cs`(ASP.NET Core를 사용하는 경우)에서 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="bbdac-120">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
