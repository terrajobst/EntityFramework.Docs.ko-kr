---
title: Microsoft SQL Server 데이터베이스 공급자-메모리 최적화 테이블-EF Core
description: SQL Server Entity Framework Core 데이터베이스 공급자를 사용 하 여 메모리 최적화 테이블을 사용 하는 방법
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: a504fb3487aea6dd36abf204a7427095e3d29118
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414766"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="16a66-103">SQL Server EF Core 데이터베이스 공급자에서 메모리 액세스에 최적화 된 테이블 지원</span><span class="sxs-lookup"><span data-stu-id="16a66-103">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="16a66-104">[메모리](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) 액세스에 최적화 된 테이블은 전체 테이블이 메모리에 상주 하는 SQL Server의 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="16a66-104">[Memory-Optimized Tables](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="16a66-105">테이블 데이터의 보조 복사본이 디스크에서 유지 관리되는데, 이는 내구성 목적입니다.</span><span class="sxs-lookup"><span data-stu-id="16a66-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="16a66-106">메모리 최적화 테이블의 데이터는 데이터베이스 복구 중에만 디스크에서 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="16a66-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="16a66-107">예를 들면, 서버를 다시 시작한 후입니다.</span><span class="sxs-lookup"><span data-stu-id="16a66-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="16a66-108">메모리 액세스에 최적화 된 테이블 구성</span><span class="sxs-lookup"><span data-stu-id="16a66-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="16a66-109">엔터티가 매핑된 테이블이 메모리 최적화되도록 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16a66-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="16a66-110">EF Core를 사용 하 여 모델 ( [마이그레이션](xref:core/managing-schemas/migrations/index) 또는 [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated))을 기반으로 데이터베이스를 만들고 유지 관리 하는 경우 이러한 엔터티에 대해 메모리 최적화 테이블이 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="16a66-110">When using EF Core to create and maintain a database based on your model (either with [migrations](xref:core/managing-schemas/migrations/index) or [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), a memory-optimized table will be created for these entities.</span></span>

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
