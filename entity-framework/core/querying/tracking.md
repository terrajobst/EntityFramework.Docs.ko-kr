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
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="a3b84-102">추적 및 비 추적 쿼리</span><span class="sxs-lookup"><span data-stu-id="a3b84-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="a3b84-103">추적 동작은 Entity Framework Core가 해당 변경 추적 장치에서 엔터티 인스턴스에 대한 정보를 유지할지 여부를 제어합니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="a3b84-104">엔터티를 추적하는 경우 엔터티에서 검색된 변경 내용은 `SaveChanges()` 중 데이터베이스에 유지됩니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="a3b84-105">또한 Entity Framework Core는 추적 쿼리에서 가져온 엔터티와 이전에 DbContext 인스턴스에 로드된 엔터티 사이의 탐색 속성을 수정합니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="a3b84-106">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="a3b84-107">추적 쿼리</span><span class="sxs-lookup"><span data-stu-id="a3b84-107">Tracking queries</span></span>

<span data-ttu-id="a3b84-108">기본적으로 엔터티 형식을 반환하는 쿼리는 추적됩니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="a3b84-109">즉, 해당 엔터티 인스턴스를 변경하고 `SaveChanges()`에서 해당 변경 내용을 유지하도록 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="a3b84-110">다음 예제에서는 `SaveChanges()` 중에 블로그 등급 변경을 검색하고 데이터베이스에 유지합니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="a3b84-111">비 추적 쿼리</span><span class="sxs-lookup"><span data-stu-id="a3b84-111">No-tracking queries</span></span>

<span data-ttu-id="a3b84-112">비 추적 쿼리는 읽기 전용 시나리오에서 결과가 사용되는 경우에 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="a3b84-113">이 쿼리는 변경 내용 추적 정보를 설정할 필요가 없기 때문에 더 빠르게 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="a3b84-114">개별 쿼리를 비 추적 쿼리로 전환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="a3b84-115">컨텍스트 인스턴스 수준에서 기본 추적 동작을 변경할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="a3b84-116">추적 쿼리는 실행 쿼리 내에서 ID 확인을 수행하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-116">No tracking queries still perform identity resolution within the excuting query.</span></span> <span data-ttu-id="a3b84-117">결과 집합에 동일한 엔터티가 여러 번 포함되는 경우 결과 집합의 각 항목에 대해 엔터티 클래스의 동일한 인스턴스가 반환됩니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="a3b84-118">그러나 약한 참조는 이미 반환된 엔터티를 추적하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="a3b84-119">ID가 동일한 이전 결과가 범위를 벗어나고 가비지 수집이 실행되는 경우 새 엔터티 인스턴스를 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="a3b84-120">자세한 내용은 [쿼리 작동 방식](overview.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="a3b84-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="a3b84-121">추적 및 프로젝션</span><span class="sxs-lookup"><span data-stu-id="a3b84-121">Tracking and projections</span></span>

<span data-ttu-id="a3b84-122">쿼리의 결과 형식이 엔터티 형식이 아닌 경우에도 결과에 엔터티 형식이 포함되어 있으면 기본적으로 추적됩니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="a3b84-123">무명 형식을 반환하는 다음 쿼리에서는 결과 집합에 있는 `Blog`의 인스턴스가 추적됩니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

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

<span data-ttu-id="a3b84-124">결과 집합에 엔터티 형식이 포함되어 있지 않으면 추적이 수행되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="a3b84-125">엔터티의 일부 값은 있지만 실제 엔터티 형식의 인스턴스는 없는 무명 형식을 반환하는 다음 쿼리에서는 추적이 수행되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="a3b84-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

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
