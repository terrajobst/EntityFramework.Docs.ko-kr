---
title: Microsoft SQL Server 데이터베이스 공급자 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: f0aa290e8c5166c278f8c9782c4304de5e91f26b
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813499"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="39c7f-102">Microsoft SQL Server EF Core 데이터베이스 공급자</span><span class="sxs-lookup"><span data-stu-id="39c7f-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="39c7f-103">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 Microsoft SQL Server(SQL Azure 포함)에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="39c7f-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="39c7f-104">공급자는 [Entity Framework Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="39c7f-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="39c7f-105">설치</span><span class="sxs-lookup"><span data-stu-id="39c7f-105">Install</span></span>

<span data-ttu-id="39c7f-106">[Microsoft.EntityFrameworkCore.SqlServer NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="39c7f-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

# <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="39c7f-107">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="39c7f-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

# <a name="visual-studiotabvs"></a>[<span data-ttu-id="39c7f-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39c7f-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="39c7f-109">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="39c7f-109">Supported Database Engines</span></span>

* <span data-ttu-id="39c7f-110">Microsoft SQL Server(2012 이상)</span><span class="sxs-lookup"><span data-stu-id="39c7f-110">Microsoft SQL Server (2012 onwards)</span></span>
