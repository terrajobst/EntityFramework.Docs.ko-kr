---
title: 추적 및 비 추적 쿼리 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: 985adc795f379199a3bacc985843f32f3168cf64
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993357"
---
# <a name="tracking-vs-no-tracking-queries"></a>추적 및 비 추적 쿼리

추적 동작은 Entity Framework Core가 해당 변경 추적 장치에서 엔터티 인스턴스에 대한 정보를 유지할지 여부를 제어합니다. 엔터티를 추적하는 경우 엔터티에서 검색된 변경 내용은 `SaveChanges()` 중 데이터베이스에 유지됩니다. 또한 Entity Framework Core는 추적 쿼리에서 가져온 엔터티와 이전에 DbContext 인스턴스에 로드된 엔터티 사이의 탐색 속성을 수정합니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)을 볼 수 있습니다.

## <a name="tracking-queries"></a>추적 쿼리

기본적으로 엔터티 형식을 반환하는 쿼리는 추적됩니다. 즉, 해당 엔터티 인스턴스를 변경하고 `SaveChanges()`에서 해당 변경 내용을 유지하도록 할 수 있습니다.

다음 예제에서는 `SaveChanges()` 중에 블로그 등급 변경을 검색하고 데이터베이스에 유지합니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>비 추적 쿼리

비 추적 쿼리는 읽기 전용 시나리오에서 결과가 사용되는 경우에 유용합니다. 이 쿼리는 변경 내용 추적 정보를 설정할 필요가 없기 때문에 더 빠르게 실행할 수 있습니다.

개별 쿼리를 비 추적 쿼리로 전환할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

컨텍스트 인스턴스 수준에서 기본 추적 동작을 변경할 수도 있습니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> 추적 쿼리는 실행 쿼리 내에서 ID 확인을 수행하지 않습니다. 결과 집합에 동일한 엔터티가 여러 번 포함되는 경우 결과 집합의 각 항목에 대해 엔터티 클래스의 동일한 인스턴스가 반환됩니다. 그러나 약한 참조는 이미 반환된 엔터티를 추적하는 데 사용됩니다. ID가 동일한 이전 결과가 범위를 벗어나고 가비지 수집이 실행되는 경우 새 엔터티 인스턴스를 가져올 수 있습니다. 자세한 내용은 [쿼리 작동 방식](overview.md)을 참조하세요.

## <a name="tracking-and-projections"></a>추적 및 프로젝션

쿼리의 결과 형식이 엔터티 형식이 아닌 경우에도 결과에 엔터티 형식이 포함되어 있으면 기본적으로 추적됩니다. 무명 형식을 반환하는 다음 쿼리에서는 결과 집합에 있는 `Blog`의 인스턴스가 추적됩니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=7)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Blog = b,
                Posts = b.Posts.Count()
            });
}
```

결과 집합에 엔터티 형식이 포함되어 있지 않으면 추적이 수행되지 않습니다. 엔터티의 일부 값은 있지만 실제 엔터티 형식의 인스턴스는 없는 무명 형식을 반환하는 다음 쿼리에서는 추적이 수행되지 않습니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Id = b.BlogId,
                Url = b.Url
            });
}
```
