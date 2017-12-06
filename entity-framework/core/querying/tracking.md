---
title: "Vs를 추적 합니다. No 추적 쿼리가-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/28/2017
---
# <a name="tracking-vs-no-tracking-queries"></a>Vs를 추적 합니다. No 추적 쿼리

Entity Framework Core는 변경 추적 장치에 엔터티 인스턴스에 대 한 정보를 보존 됩니다 여부 동작 컨트롤을 추적 합니다. 엔터티의 내용이 엔터티를 추적 하는 동안 데이터베이스에 유지 됩니다 `SaveChanges()`합니다. 엔터티 Framework Core는 또한 픽스업 탐색 속성 추적 쿼리에서 가져온 엔터티 및 DbContext 인스턴스에 이전에 로드 된 엔터티 사이입니다.

> [!TIP]  
> 이 문서를 볼 수 있습니다 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub에서 합니다.

## <a name="tracking-queries"></a>추적 쿼리

기본적으로 엔터티 형식을 반환 하는 쿼리 추적 됩니다. 즉, 해당 엔터티 인스턴스를 변경할 수 있으며 이러한 변경으로 유지 `SaveChanges()`합니다.

다음 예에서 블로그 등급에 대 한 변경 검색 되며 하는 동안 데이터베이스에 유지 `SaveChanges()`합니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>No 추적 쿼리

추적 쿼리가 더 이상는 읽기 전용 시나리오에서 결과 사용 하는 경우에 유용 합니다. 이들은 신속 하 게 설치 변경 내용 추적 정보가 없는 필요가 없기 때문에 실행할 수입니다.

개별 쿼리 수 없음-추적을 스왑할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

또한 기본 컨텍스트 인스턴스 수준에서 동작 추적을 변경할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> 추적 쿼리 없음은 여전히 실행 쿼리 내에서 id 확인을 수행 합니다. 동일한 엔터티 여러 번 포함 하는 결과 집합을 결과 집합의 각 항목에 대 한 엔터티 클래스의 동일한 인스턴스에 반환 됩니다. 하지만 약한 참조가 이미 반환 된 엔터티를 추적 하기 위해 사용 됩니다. 동일한 id 가진 이전 결과 범위를 벗어나거나 가비지 수집을 실행 하는 경우 새 엔터티 인스턴스를 발생할 수 있습니다. 자세한 내용은 참조 [쿼리 작동 방식](overview.md)합니다.

## <a name="tracking-and-projections"></a>추적 및 예측

쿼리의 결과 형식을 결과 엔터티 형식이 포함 된 경우 엔터티 형식에 있지 않더라도 계속 추적 기본적으로 합니다. 다음 쿼리를 익명 형식이의 인스턴스를 반환 하는 `Blog` 결과에서 집합을 추적 합니다.

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

결과 집합의 모든 엔터티 형식에 들어 있지 않으면 없음 추적을 수행 합니다. 다음 쿼리에서 익명 형식을 반환 하는 엔터티 (하지만 실제 엔터티 형식 인스턴스가 없는) 값 중 일부는 없는 추적 수행 합니다.

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
