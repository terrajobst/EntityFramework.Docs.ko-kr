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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>SQL Server EF Core 데이터베이스 공급자에서 메모리 최적화 테이블을 지원합니다.

> [!NOTE]  
>
> 이 기능은 EF Core 1.1에서 도입되었습니다.

[메모리 최적화 테이블](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) 는 전체 테이블을 메모리에 있는 SQL Server의 기능입니다. 테이블 데이터의 두 번째 복사는 내구성 목적 없지만 디스크에 유지 됩니다. 메모리 최적화 테이블의 데이터는 데이터베이스 복구 중 디스크에서 읽기만 합니다. 예를 들어, 후 서버를 다시 시작 합니다.

## <a name="configuring-a-memory-optimized-table"></a>메모리 최적화 테이블을 구성합니다.

엔터티가 매핑된 테이블이 메모리 최적화되도록 지정할 수 있습니다. EF Core를 사용하여 모델을 기반으로 데이터베이스를 만들고 유지 관리할 경우(마이그레이션 또는 `Database.EnsureCreated()` 사용) 이러한 엔터티에 대한 메모리 최적화 테이블이 만들어집니다.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
