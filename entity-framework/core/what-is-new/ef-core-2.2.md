---
title: EF Core 2.2의 새로운 기능 - EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: 79b4efc3aee23e19a9ea1deb6373b9984b77f886
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688757"
---
# <a name="new-features-in-ef-core-22"></a><span data-ttu-id="ff8cc-102">EF Core 2.2의 새로운 기능</span><span class="sxs-lookup"><span data-stu-id="ff8cc-102">New features in EF Core 2.2</span></span>

## <a name="spatial-data-support"></a><span data-ttu-id="ff8cc-103">공간 데이터 지원</span><span class="sxs-lookup"><span data-stu-id="ff8cc-103">Spatial data support</span></span>

<span data-ttu-id="ff8cc-104">개체의 물리적 위치 및 모양을 나타내는 데 공간 데이터를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-104">Spatial data can be used to represent the physical location and shape of objects.</span></span>
<span data-ttu-id="ff8cc-105">대부분의 데이터베이스는 기본적으로 공간 데이터를 저장하고 인덱싱하고 쿼리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-105">Many databases can natively store, index, and query spatial data.</span></span> <span data-ttu-id="ff8cc-106">일반적인 시나리오로는 지정된 거리 내의 개체에 대한 쿼리, 다각형에 지정된 위치가 포함되는지 테스트 등이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-106">Common scenarios include querying for objects within a given distance, and testing if a polygon contains a given location.</span></span>
<span data-ttu-id="ff8cc-107">EF Core 2.2는 이제 [NTS](https://github.com/NetTopologySuite/NetTopologySuite)(NetTopologySuite) 라이브러리의 형식을 사용하는 다양한 데이터베이스의 공간 데이터 작업을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-107">EF Core 2.2 now supports working with spatial data from various databases using types from the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS) library.</span></span>

<span data-ttu-id="ff8cc-108">공간 데이터 지원은 일련의 공급자별 확장 패키지로 구현됩니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-108">Spatial data support is implemented as a series of provider-specific extension packages.</span></span>
<span data-ttu-id="ff8cc-109">이러한 각각의 패키지는 NTS 형식과 메서드에 대한 매핑, 데이터베이스의 해당 공간 형식 및 함수를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-109">Each of these packages contributes mappings for NTS types and methods, and the corresponding spatial types and functions in the database.</span></span>
<span data-ttu-id="ff8cc-110">그러한 공급자 확장은 [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/)([Npgsql 프로젝트](http://www.npgsql.org/))에 대해 현재 사용 가능합니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-110">Such provider extensions are now available for [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), and [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (from the [Npgsql project](http://www.npgsql.org/)).</span></span>
<span data-ttu-id="ff8cc-111">공간 형식은 추가 확장 없이 [EF Core 메모리 내 공급자](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/)에서 바로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-111">Spatial types can be used directly with the [EF Core in-memory provider](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) without additional extensions.</span></span>

<span data-ttu-id="ff8cc-112">공급자 확장을 설치하면 지원되는 형식의 속성을 엔터티에 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-112">Once the provider extension is installed, you can add properties of supported types to your entities.</span></span> <span data-ttu-id="ff8cc-113">예:</span><span class="sxs-lookup"><span data-stu-id="ff8cc-113">For example:</span></span>

``` csharp
using NetTopologySuite.Geometries;

namespace MyApp
{
  public class Friend
  {
    [Key]
    public string Name { get; set; }
  
    [Required]
    public Point Location { get; set; }
  }
}
``` 

<span data-ttu-id="ff8cc-114">그런 다음, 공간 데이터와 함께 엔터티를 지속할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-114">You can then persist entities with spatial data:</span></span>

``` csharp
using (var context = new MyDbContext())
{
    context.Add(
        new Friend
        {
            Name = "Bill",
            Location = new Point(-122.34877, 47.6233355) {SRID = 4326 }
        });
    context.SaveChanges();
}
```
<span data-ttu-id="ff8cc-115">그리고 공간 데이터 및 작업을 기반으로 데이터베이스 쿼리를 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-115">And you can execute database queries based on spatial data and operations:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="ff8cc-116">이 기능에 대한 자세한 내용은 [공간 형식 설명서](xref:core/modeling/spatial)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-116">For more information on this feature, see the [spatial types documentation](xref:core/modeling/spatial).</span></span> 

## <a name="collections-of-owned-entities"></a><span data-ttu-id="ff8cc-117">소유 엔터티 컬렉션</span><span class="sxs-lookup"><span data-stu-id="ff8cc-117">Collections of owned entities</span></span>

<span data-ttu-id="ff8cc-118">EF Core 2.0은 일대일 연결에서 소유권을 모델링하는 기능을 추가했습니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-118">EF Core 2.0 added the ability to model ownership in one-to-one associations.</span></span>
<span data-ttu-id="ff8cc-119">EF Core 2.2는 일대다 연결에 소유권을 표시하는 기능을 확장합니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-119">EF Core 2.2 extends the ability to express ownership to one-to-many associations.</span></span>
<span data-ttu-id="ff8cc-120">소유권은 엔터티가 사용되는 방식을 제한하는 데 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-120">Ownership helps constrain how entities are used.</span></span>

<span data-ttu-id="ff8cc-121">예를 들어, 소유 엔터티는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-121">For example, owned entities:</span></span>
- <span data-ttu-id="ff8cc-122">다른 엔터티 형식의 탐색 속성에만 표시될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-122">Can only ever appear on navigation properties of other entity types.</span></span> 
- <span data-ttu-id="ff8cc-123">자동으로 로드되며, 소유자와 함께 DbContext에 의해서만 추적될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-123">Are automatically loaded, and can only be tracked by a DbContext alongside their owner.</span></span>

<span data-ttu-id="ff8cc-124">관계형 데이터베이스에서 소유 컬렉션은 일반적인 일대다 연결처럼 소유자의 별도의 테이블에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-124">In relational databases, owned collections are mapped to separate tables from the owner, just like regular one-to-many associations.</span></span>
<span data-ttu-id="ff8cc-125">그러나 문서 지향적 데이터베이스에서는 소유 컬렉션 또는 참조의 소유 엔터티를 소유자와 동일한 문서 내에 중첩할 계획입니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-125">But in document-oriented databases, we plan to nest owned entities (in owned collections or references) within the same document as the owner.</span></span>

<span data-ttu-id="ff8cc-126">새 OwnsMany() API를 호출하여 이 기능을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-126">You can use the feature by calling the new OwnsMany() API:</span></span>

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

<span data-ttu-id="ff8cc-127">자세한 내용은 [업데이트된 소유 엔터티 설명서](xref:core/modeling/owned-entities#collections-of-owned-types)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-127">For more information, see the [updated owned entities documentation](xref:core/modeling/owned-entities#collections-of-owned-types).</span></span>

## <a name="query-tags"></a><span data-ttu-id="ff8cc-128">쿼리 태그</span><span class="sxs-lookup"><span data-stu-id="ff8cc-128">Query tags</span></span>

<span data-ttu-id="ff8cc-129">이 기능은 로그에서 캡처한 생성된 SQL 쿼리와 코드의 LINQ 쿼리의 상관 관계를 간소화합니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-129">This feature simplifies the correlation of LINQ queries in code with generated SQL queries captured in logs.</span></span>

<span data-ttu-id="ff8cc-130">쿼리 태그를 사용하려면 새 TagWith() 메서드를 사용하여 LINQ 쿼리에 주석을 답니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-130">To take advantage of query tags, you annotate a LINQ query using the new TagWith() method.</span></span>
<span data-ttu-id="ff8cc-131">이전 예에서 공간 쿼리 사용:</span><span class="sxs-lookup"><span data-stu-id="ff8cc-131">Using the spatial query from a previous example:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="ff8cc-132">이 LINQ 쿼리는 다음 SQL 출력을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-132">This LINQ query will produce the following SQL output:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="ff8cc-133">자세한 내용은 [쿼리 태그 설명서](xref:core/querying/tags)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ff8cc-133">For more information, see the [query tags documentation](xref:core/querying/tags).</span></span> 