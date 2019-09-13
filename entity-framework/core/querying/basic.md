---
title: 기본 쿼리 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
uid: core/querying/basic
ms.openlocfilehash: 49daa0d37175244617993cc6e911cbd59d27079f
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921749"
---
# <a name="basic-queries"></a><span data-ttu-id="51557-102">기본 쿼리</span><span class="sxs-lookup"><span data-stu-id="51557-102">Basic Queries</span></span>

<span data-ttu-id="51557-103">LINQ(Language Integrated Query)를 사용하여 데이터베이스에서 엔터티를 로드하는 방법을 알아보세요.</span><span class="sxs-lookup"><span data-stu-id="51557-103">Learn how to load entities from the database using Language Integrated Query (LINQ).</span></span>

> [!TIP]  
> <span data-ttu-id="51557-104">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51557-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="101-linq-samples"></a><span data-ttu-id="51557-105">101 LINQ 샘플</span><span class="sxs-lookup"><span data-stu-id="51557-105">101 LINQ samples</span></span>

<span data-ttu-id="51557-106">이 페이지에서는 Entity Framework Core로 일반 작업을 수행하는 몇 가지 예제를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="51557-106">This page shows a few examples to achieve common tasks with Entity Framework Core.</span></span> <span data-ttu-id="51557-107">LINQ로 수행할 수 있는 작업을 보여주는 광범위한 샘플 모음은 [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b)(101 LINQ 샘플)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="51557-107">For an extensive set of samples showing what is possible with LINQ, see [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="51557-108">모든 데이터 로드</span><span class="sxs-lookup"><span data-stu-id="51557-108">Loading all data</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a><span data-ttu-id="51557-109">단일 엔터티 로드</span><span class="sxs-lookup"><span data-stu-id="51557-109">Loading a single entity</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a><span data-ttu-id="51557-110">필터링</span><span class="sxs-lookup"><span data-stu-id="51557-110">Filtering</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
