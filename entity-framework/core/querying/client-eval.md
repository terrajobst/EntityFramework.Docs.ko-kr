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
# <a name="client-vs-server-evaluation"></a>클라이언트 및 서버 평가

Entity Framework Core는 클라이언트에서 평가되는 쿼리 부분과 데이터베이스로 푸시되는 쿼리 부분을 지원합니다. 데이터베이스에서 평가될 쿼리의 부분은 데이터베이스 공급자가 결정합니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)을 볼 수 있습니다.

## <a name="client-evaluation"></a>클라이언트 평가

다음 예제에서는 도우미 메서드가 SQL Server 데이터베이스에 반환되는 블로그의 URL을 표준화하는 데 사용됩니다. SQL Server 공급자는 이 메서드가 구현되는 방법에 대한 인사이트가 없기 때문에 이 메서드를 SQL로 변환할 수 없습니다. 쿼리의 다른 모든 측면은 데이터베이스에서 평가되지만 이 메서드를 통해 반환된 `URL`을 전달하는 작업은 클라이언트에서 수행됩니다.

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

## <a name="client-evaluation-performance-issues"></a>클라이언트 평가 성능 문제

클라이언트 평가는 매우 유용할 수 있지만 경우에 따라 성능이 저하될 수 있습니다. 이제 도우미 메서드가 필터에서 사용되는 다음 쿼리를 살펴보겠습니다. 데이터베이스에서는 이 작업을 수행할 수 없으므로 모든 데이터를 메모리로 풀한 다음, 클라이언트에서 필터를 적용합니다. 데이터양과 해당 데이터가 필터링되는 양에 따라 성능이 저하될 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a>클라이언트 평가 로깅

기본적으로 EF Core는 클라이언트 평가가 수행될 때 표시되는 경고를 로깅합니다. 로깅 출력 보기에 대한 자세한 내용은 [로깅](../miscellaneous/logging.md)을 참조하세요. 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a>선택적 동작: 클라이언트 평가에 대한 예외를 발생시킵니다.

클라이언트 평가가 수행될 때 동작을 throw나 아무 작업도 하지 않음으로 변경할 수 있습니다. 이 작업은 컨텍스트에 대한 옵션을 설정할 때 수행하며, 일반적으로 `DbContext.OnConfiguring`에서나 `Startup.cs`(ASP.NET Core를 사용하는 경우)에서 수행합니다.

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
