---
title: "기본 쿼리-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
ms.technology: entity-framework-core
uid: core/querying/basic
ms.openlocfilehash: 5070faf2aeeffad680e24e7de5a0ffa03a8f0064
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="basic-queries"></a><span data-ttu-id="e2c38-102">기본 쿼리</span><span class="sxs-lookup"><span data-stu-id="e2c38-102">Basic Queries</span></span>

<span data-ttu-id="e2c38-103">언어 통합 쿼리 (LINQ)를 사용 하 여 데이터베이스에서 엔터티를 로드 하는 방법에 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="e2c38-103">Learn how to load entities from the database using Language Integrate Query (LINQ).</span></span>

> [!TIP]  
> <span data-ttu-id="e2c38-104">이 문서를 볼 수 있습니다 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub에서 합니다.</span><span class="sxs-lookup"><span data-stu-id="e2c38-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="101-linq-samples"></a><span data-ttu-id="e2c38-105">101 LINQ 예제</span><span class="sxs-lookup"><span data-stu-id="e2c38-105">101 LINQ samples</span></span>

<span data-ttu-id="e2c38-106">이 페이지에는 Entity Framework Core 관련 일반 작업을 달성 하기 위해 몇 가지 예가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e2c38-106">This page shows a few examples to achieve common tasks with Entity Framework Core.</span></span> <span data-ttu-id="e2c38-107">다양 한 LINQ로 가능한 보여 주는 샘플 집합을 참조 하십시오. [101 LINQ 예제](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b)합니다.</span><span class="sxs-lookup"><span data-stu-id="e2c38-107">For an extensive set of samples showing what is possible with LINQ, see [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="e2c38-108">모든 데이터를 로드합니다.</span><span class="sxs-lookup"><span data-stu-id="e2c38-108">Loading all data</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a><span data-ttu-id="e2c38-109">단일 엔터티를 로드합니다.</span><span class="sxs-lookup"><span data-stu-id="e2c38-109">Loading a single entity</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a><span data-ttu-id="e2c38-110">필터링</span><span class="sxs-lookup"><span data-stu-id="e2c38-110">Filtering</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
