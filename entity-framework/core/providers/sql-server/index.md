---
title: Microsoft SQL Server 데이터베이스 공급자 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: 1e75bc4bf334b1a60d13a2ec9ef314e3afcf0273
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812094"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="ba8d1-102">Microsoft SQL Server EF Core 데이터베이스 공급자</span><span class="sxs-lookup"><span data-stu-id="ba8d1-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="ba8d1-103">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 Microsoft SQL Server(SQL Azure 포함)에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ba8d1-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="ba8d1-104">공급자는 [Entity Framework Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="ba8d1-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="ba8d1-105">설치</span><span class="sxs-lookup"><span data-stu-id="ba8d1-105">Install</span></span>

<span data-ttu-id="ba8d1-106">[Microsoft.EntityFrameworkCore.SqlServer NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="ba8d1-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

# <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="ba8d1-107">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ba8d1-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

# <a name="visual-studiotabvs"></a>[<span data-ttu-id="ba8d1-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ba8d1-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> <span data-ttu-id="ba8d1-109">버전 3.0.0 이후 공급자는 Microsoft.Data.SqlClient를 참조합니다(System.Data.SqlClient에 종속된 이전 버전).</span><span class="sxs-lookup"><span data-stu-id="ba8d1-109">Since version 3.0.0, the provider references Microsoft.Data.SqlClient (previous versions depended on System.Data.SqlClient).</span></span> <span data-ttu-id="ba8d1-110">프로젝트가 SqlClient에 직접 종속될 경우, 올바른 패키지를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ba8d1-110">If your project takes a direct dependency on SqlClient, make sure it references the correct package.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="ba8d1-111">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="ba8d1-111">Supported Database Engines</span></span>

* <span data-ttu-id="ba8d1-112">Microsoft SQL Server(2012 이상)</span><span class="sxs-lookup"><span data-stu-id="ba8d1-112">Microsoft SQL Server (2012 onwards)</span></span>
