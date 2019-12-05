---
title: Microsoft SQL Server 데이터베이스 공급자-Azure SQL Database 옵션-EF Core
description: SQL Server Entity Framework Core 데이터베이스 공급자를 사용 하 여 Azure SQL Database에 대 한 서비스 계층 및 성능 수준을 지정 하는 방법
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/azure-sql-database
ms.openlocfilehash: c4f7b91110a0e700ed06130661e611cf45bee05f
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824793"
---
# <a name="specifying-azure-sql-database-options"></a>Azure SQL Database 옵션 지정

>[!NOTE]
> 이 API EF Core 3.1의 새로운 API입니다.

Azure SQL Database는 일반적으로 Azure Portal을 통해 구성 되는 [다양 한 가격 옵션을](https://azure.microsoft.com/pricing/details/sql-database/single/) 제공 합니다. 그러나 [EF Core 마이그레이션을](xref:core/managing-schemas/migrations/index) 사용 하 여 스키마를 관리 하는 경우 모델 자체에서 원하는 옵션을 지정할 수 있습니다.

[Hasservicetier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier)를 사용 하 여 데이터베이스의 서비스 계층 (버전)을 지정할 수 있습니다.

[!code-csharp[HasServiceTier](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasServiceTier)]

[Hasdatabasemaxsize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize)를 사용 하 여 데이터베이스의 최대 크기를 지정할 수 있습니다.

[!code-csharp[HasDatabaseMaxSize](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasDatabaseMaxSize)]

[HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel)를 사용 하 여 데이터베이스의 성능 수준 (SERVICE_OBJECTIVE)을 지정할 수 있습니다.

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevel)]

[HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) 를 사용 하 여 값이 문자열 리터럴이 아니기 때문에 탄력적 풀을 구성 합니다.

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevelSql)]


>[!TIP]
> [ALTER database 설명서](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current)에서 지원 되는 모든 값을 찾을 수 있습니다.