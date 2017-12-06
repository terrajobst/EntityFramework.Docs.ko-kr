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
# <a name="client-vs-server-evaluation"></a>클라이언트 vs입니다. 서버 평가

Entity Framework Core는 클라이언트와 데이터베이스에 밀어 넣는 것의 일부에서 평가 되 고 쿼리의 일부를 지원 합니다. 데이터베이스의 쿼리 부분을 평가할 수를 확인 하려면 데이터베이스 공급자가 합니다.

> [!TIP]  
> 이 문서를 볼 수 있습니다 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub에서 합니다.

## <a name="client-evaluation"></a>클라이언트 평가

다음 예에서 도우미 메서드는 SQL Server 데이터베이스에서 반환 되는 블로그에 대 한 Url을 표준화 하 사용 됩니다. SQL Server 공급자에이 메서드를 구현 하는 방법을에 없는 대 한 정보를 SQL로 변환 하는 것이 불가능 합니다. 쿼리의 다른 모든 측면에서 평가 되는 데이터베이스에 있지만 전달 반환 된 `URL` 이 메서드를 통해 클라이언트에서 수행 합니다.

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

## <a name="disabling-client-evaluation"></a>클라이언트 평가 사용 하지 않도록 설정

클라이언트 평가 성능이 저하 될 수 있습니다는 어떤 경우에 매우 유용할 수 있습니다. 필터에서 도우미 메서드를 사용할 이제는 다음 쿼리를 살펴보십시오. 이 데이터베이스에서 수행할 수 없으므로, 메모리 변환한 다음 필터 데이터를 끌어온 모든 클라이언트에 적용 됩니다. 에 해당 데이터의 양을 필터링, 데이터의 양에 따라이 성능이 저하 될 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

기본적으로 EF 코어 클라이언트 평가 수행 될 때 경고를 기록 합니다. 참조 [로깅](../miscellaneous/logging.md) 로깅 출력 보기에 대 한 자세한 내용은 합니다. 클라이언트 평가 throw 하거나 아무 작업도 수행 하지 발생할 경우 동작을 변경할 수 있습니다. 일반적으로 컨텍스트-에 대 한 옵션을 설정할 때 이렇게 `DbContext.OnConfiguring`, 또는 `Startup.cs` ASP.NET Core를 사용 하는 경우.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
