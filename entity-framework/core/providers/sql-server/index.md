---
title: Microsoft SQL Server 데이터베이스 공급자 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: 70e7f3d4bc1f57f1b23d9b3e0bd6264236ddbd27
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149203"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="82a58-102">Microsoft SQL Server EF Core 데이터베이스 공급자</span><span class="sxs-lookup"><span data-stu-id="82a58-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="82a58-103">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 Microsoft SQL Server(SQL Azure 포함)에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="82a58-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="82a58-104">공급자는 [Entity Framework Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="82a58-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="82a58-105">설치</span><span class="sxs-lookup"><span data-stu-id="82a58-105">Install</span></span>

<span data-ttu-id="82a58-106">[Microsoft.EntityFrameworkCore.SqlServer NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="82a58-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="supported-database-engines"></a><span data-ttu-id="82a58-107">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="82a58-107">Supported Database Engines</span></span>

* <span data-ttu-id="82a58-108">Microsoft SQL Server(2012 이상)</span><span class="sxs-lookup"><span data-stu-id="82a58-108">Microsoft SQL Server (2012 onwards)</span></span>
