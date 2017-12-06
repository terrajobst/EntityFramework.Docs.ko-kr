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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>SQL Server EF 코어 데이터베이스 공급자에 메모리 액세스에 최적화 된 테이블을 지원합니다.

> [!NOTE]  
>
> 이 기능은 EF 코어 1.1에서 도입 되었습니다.

[메모리 액세스에 최적화 된 테이블](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) 전체 테이블이 메모리에 있는 SQL Server의 기능입니다. 테이블 데이터의 보조 복사본이 디스크에서 유지 관리되는데, 이는 내구성 목적입니다. 메모리 최적화 테이블의 데이터는 데이터베이스 복구 중에만 디스크에서 읽습니다. 예를 들면, 서버를 다시 시작한 후입니다.

## <a name="configuring-a-memory-optimized-table"></a>메모리 액세스에 최적화 된 테이블 구성

메모리 액세스에 최적화 된 테이블에는 엔터티를 매핑할 인지를 지정할 수 있습니다. 모델에 기반 EF 코어를 사용 하 여 만들고 데이터베이스를 유지할 경우 (마이그레이션과 함께 하나 또는 `Database.EnsureCreated()`), 이러한 엔터티에 대 한 메모리 액세스에 최적화 된 테이블이 생성 됩니다.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
