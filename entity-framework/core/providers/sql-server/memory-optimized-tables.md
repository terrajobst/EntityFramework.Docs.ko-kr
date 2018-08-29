---
title: Microsoft SQL Server 데이터베이스 공급자-메모리 액세스에 최적화 된 테이블-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 63d2cbf8b69e4f1945ad60914e284fb42c48e8db
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995804"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="c6ae0-102">SQL Server EF Core 데이터베이스 공급자에서 메모리 최적화 테이블을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="c6ae0-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="c6ae0-103">이 기능은 EF Core 1.1에서 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="c6ae0-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="c6ae0-104">[메모리 최적화 테이블](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) 는 전체 테이블을 메모리에 있는 SQL Server의 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="c6ae0-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="c6ae0-105">테이블 데이터의 두 번째 복사는 내구성 목적 없지만 디스크에 유지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6ae0-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="c6ae0-106">메모리 최적화 테이블의 데이터는 데이터베이스 복구 중 디스크에서 읽기만 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6ae0-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="c6ae0-107">예를 들어, 후 서버를 다시 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6ae0-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="c6ae0-108">메모리 최적화 테이블을 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="c6ae0-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="c6ae0-109">엔터티가 매핑된 테이블이 메모리 최적화되도록 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6ae0-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="c6ae0-110">EF Core를 사용하여 모델을 기반으로 데이터베이스를 만들고 유지 관리할 경우(마이그레이션 또는 `Database.EnsureCreated()` 사용) 이러한 엔터티에 대한 메모리 최적화 테이블이 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="c6ae0-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
