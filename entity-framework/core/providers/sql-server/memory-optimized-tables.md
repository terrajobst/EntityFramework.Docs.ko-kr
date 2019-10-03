---
title: Microsoft SQL Server 데이터베이스 공급자-메모리 최적화 테이블-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 7383e74b3f83172f9b8e0eaf9bd09d4e187e87f8
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813489"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="efec9-102">SQL Server EF Core 데이터베이스 공급자에서 메모리 액세스에 최적화 된 테이블 지원</span><span class="sxs-lookup"><span data-stu-id="efec9-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="efec9-103">[메모리](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) 액세스에 최적화 된 테이블은 전체 테이블이 메모리에 상주 하는 SQL Server의 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="efec9-103">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="efec9-104">테이블 데이터의 보조 복사본이 디스크에서 유지 관리되는데, 이는 내구성 목적입니다.</span><span class="sxs-lookup"><span data-stu-id="efec9-104">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="efec9-105">메모리 액세스에 최적화 된 테이블의 데이터는 데이터베이스 복구 중에만 디스크에서 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="efec9-105">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="efec9-106">예를 들어 서버를 다시 시작한 후입니다.</span><span class="sxs-lookup"><span data-stu-id="efec9-106">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="efec9-107">메모리 액세스에 최적화 된 테이블 구성</span><span class="sxs-lookup"><span data-stu-id="efec9-107">Configuring a memory-optimized table</span></span>

<span data-ttu-id="efec9-108">엔터티가 매핑된 테이블이 메모리 최적화되도록 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efec9-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="efec9-109">EF Core를 사용하여 모델을 기반으로 데이터베이스를 만들고 유지 관리할 경우(마이그레이션 또는 `Database.EnsureCreated()` 사용) 이러한 엔터티에 대한 메모리 최적화 테이블이 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="efec9-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
