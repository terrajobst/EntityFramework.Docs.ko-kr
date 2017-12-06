---
title: "Microsoft SQL Server 데이터베이스-메모리 액세스에 최적화 된 테이블-공급자 EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 83a0decffc5946d036903b8b8add59f0ea31b21f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="d5845-102">SQL Server EF 코어 데이터베이스 공급자에 메모리 액세스에 최적화 된 테이블을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="d5845-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="d5845-103">이 기능은 EF 코어 1.1에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="d5845-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="d5845-104">[메모리 액세스에 최적화 된 테이블](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) 전체 테이블이 메모리에 있는 SQL Server의 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="d5845-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="d5845-105">테이블 데이터의 보조 복사본이 디스크에서 유지 관리되는데, 이는 내구성 목적입니다.</span><span class="sxs-lookup"><span data-stu-id="d5845-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="d5845-106">메모리 최적화 테이블의 데이터는 데이터베이스 복구 중에만 디스크에서 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="d5845-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="d5845-107">예를 들면, 서버를 다시 시작한 후입니다.</span><span class="sxs-lookup"><span data-stu-id="d5845-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="d5845-108">메모리 액세스에 최적화 된 테이블 구성</span><span class="sxs-lookup"><span data-stu-id="d5845-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="d5845-109">메모리 액세스에 최적화 된 테이블에는 엔터티를 매핑할 인지를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5845-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="d5845-110">모델에 기반 EF 코어를 사용 하 여 만들고 데이터베이스를 유지할 경우 (마이그레이션과 함께 하나 또는 `Database.EnsureCreated()`), 이러한 엔터티에 대 한 메모리 액세스에 최적화 된 테이블이 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d5845-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
