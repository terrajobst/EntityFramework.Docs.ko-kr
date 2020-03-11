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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>SQL Server EF Core 데이터베이스 공급자에서 메모리 액세스에 최적화 된 테이블 지원

[메모리](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) 액세스에 최적화 된 테이블은 전체 테이블이 메모리에 상주 하는 SQL Server의 기능입니다. 테이블 데이터의 보조 복사본이 디스크에서 유지 관리되는데, 이는 내구성 목적입니다. 메모리 최적화 테이블의 데이터는 데이터베이스 복구 중에만 디스크에서 읽습니다. 예를 들면, 서버를 다시 시작한 후입니다.

## <a name="configuring-a-memory-optimized-table"></a>메모리 액세스에 최적화 된 테이블 구성

엔터티가 매핑된 테이블이 메모리 최적화되도록 지정할 수 있습니다. EF Core를 사용 하 여 모델 ( [마이그레이션](xref:core/managing-schemas/migrations/index) 또는 [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated))을 기반으로 데이터베이스를 만들고 유지 관리 하는 경우 이러한 엔터티에 대해 메모리 최적화 테이블이 생성 됩니다.

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
