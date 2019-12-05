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
# <a name="specifying-azure-sql-database-options"></a><span data-ttu-id="4694b-103">Azure SQL Database 옵션 지정</span><span class="sxs-lookup"><span data-stu-id="4694b-103">Specifying Azure SQL Database Options</span></span>

>[!NOTE]
> <span data-ttu-id="4694b-104">이 API EF Core 3.1의 새로운 API입니다.</span><span class="sxs-lookup"><span data-stu-id="4694b-104">This API is new in EF Core 3.1.</span></span>

<span data-ttu-id="4694b-105">Azure SQL Database는 일반적으로 Azure Portal을 통해 구성 되는 [다양 한 가격 옵션을](https://azure.microsoft.com/pricing/details/sql-database/single/) 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="4694b-105">Azure SQL Database provides [a variety of pricing options](https://azure.microsoft.com/pricing/details/sql-database/single/) that are usually configured through the Azure Portal.</span></span> <span data-ttu-id="4694b-106">그러나 [EF Core 마이그레이션을](xref:core/managing-schemas/migrations/index) 사용 하 여 스키마를 관리 하는 경우 모델 자체에서 원하는 옵션을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4694b-106">However if you are managing the schema using [EF Core migrations](xref:core/managing-schemas/migrations/index) you can specify the desired options in the model itself.</span></span>

<span data-ttu-id="4694b-107">[Hasservicetier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier)를 사용 하 여 데이터베이스의 서비스 계층 (버전)을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4694b-107">You can specify the service tier of the database (EDITION) using [HasServiceTier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):</span></span>

[!code-csharp[HasServiceTier](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasServiceTier)]

<span data-ttu-id="4694b-108">[Hasdatabasemaxsize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize)를 사용 하 여 데이터베이스의 최대 크기를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4694b-108">You can specify the maximum size of the database using [HasDatabaseMaxSize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):</span></span>

[!code-csharp[HasDatabaseMaxSize](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasDatabaseMaxSize)]

<span data-ttu-id="4694b-109">[HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel)를 사용 하 여 데이터베이스의 성능 수준 (SERVICE_OBJECTIVE)을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4694b-109">You can specify the performance level of the database (SERVICE_OBJECTIVE) using [HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevel)]

<span data-ttu-id="4694b-110">[HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) 를 사용 하 여 값이 문자열 리터럴이 아니기 때문에 탄력적 풀을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="4694b-110">Use [HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) to configure the elastic pool, since the value is not a string literal:</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevelSql)]


>[!TIP]
> <span data-ttu-id="4694b-111">[ALTER database 설명서](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current)에서 지원 되는 모든 값을 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4694b-111">You can find all the supported values in the [ALTER DATABASE documentation](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current).</span></span>