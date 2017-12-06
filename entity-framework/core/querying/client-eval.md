---
title: "클라이언트 vs입니다. 서버 평가-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
ms.technology: entity-framework-core
uid: core/querying/client-eval
ms.openlocfilehash: e1852b780041e9e92fb4d25129175346e3a601a3
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="0308a-102">클라이언트 vs입니다. 서버 평가</span><span class="sxs-lookup"><span data-stu-id="0308a-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="0308a-103">Entity Framework Core는 클라이언트와 데이터베이스에 밀어 넣는 것의 일부에서 평가 되 고 쿼리의 일부를 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="0308a-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="0308a-104">데이터베이스의 쿼리 부분을 평가할 수를 확인 하려면 데이터베이스 공급자가 합니다.</span><span class="sxs-lookup"><span data-stu-id="0308a-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="0308a-105">이 문서를 볼 수 있습니다 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub에서 합니다.</span><span class="sxs-lookup"><span data-stu-id="0308a-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="0308a-106">클라이언트 평가</span><span class="sxs-lookup"><span data-stu-id="0308a-106">Client evaluation</span></span>

<span data-ttu-id="0308a-107">다음 예에서 도우미 메서드는 SQL Server 데이터베이스에서 반환 되는 블로그에 대 한 Url을 표준화 하 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0308a-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="0308a-108">SQL Server 공급자에이 메서드를 구현 하는 방법을에 없는 대 한 정보를 SQL로 변환 하는 것이 불가능 합니다.</span><span class="sxs-lookup"><span data-stu-id="0308a-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="0308a-109">쿼리의 다른 모든 측면에서 평가 되는 데이터베이스에 있지만 전달 반환 된 `URL` 이 메서드를 통해 클라이언트에서 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="0308a-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs?highlight=6)] -->
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

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
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

## <a name="disabling-client-evaluation"></a><span data-ttu-id="0308a-110">클라이언트 평가 사용 하지 않도록 설정</span><span class="sxs-lookup"><span data-stu-id="0308a-110">Disabling client evaluation</span></span>

<span data-ttu-id="0308a-111">클라이언트 평가 성능이 저하 될 수 있습니다는 어떤 경우에 매우 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0308a-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="0308a-112">필터에서 도우미 메서드를 사용할 이제는 다음 쿼리를 살펴보십시오.</span><span class="sxs-lookup"><span data-stu-id="0308a-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="0308a-113">이 데이터베이스에서 수행할 수 없으므로, 메모리 변환한 다음 필터 데이터를 끌어온 모든 클라이언트에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0308a-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="0308a-114">에 해당 데이터의 양을 필터링, 데이터의 양에 따라이 성능이 저하 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0308a-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

<span data-ttu-id="0308a-115">기본적으로 EF 코어 클라이언트 평가 수행 될 때 경고를 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="0308a-115">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="0308a-116">참조 [로깅](../miscellaneous/logging.md) 로깅 출력 보기에 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="0308a-116">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="0308a-117">클라이언트 평가 throw 하거나 아무 작업도 수행 하지 발생할 경우 동작을 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0308a-117">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="0308a-118">일반적으로 컨텍스트-에 대 한 옵션을 설정할 때 이렇게 `DbContext.OnConfiguring`, 또는 `Startup.cs` ASP.NET Core를 사용 하는 경우.</span><span class="sxs-lookup"><span data-stu-id="0308a-118">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
