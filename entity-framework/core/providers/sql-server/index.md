---
title: Microsoft SQL Server 데이터베이스 공급자 - EF Core
description: Entity Framework Core를 Microsoft SQL Server에서 사용할 수 있도록 허용하는 데이터베이스 공급자에 관한 문서
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: baae668a7ec255e35ab0e23e5c5ddfa47bda917e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413148"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="8b285-103">Microsoft SQL Server EF Core 데이터베이스 공급자</span><span class="sxs-lookup"><span data-stu-id="8b285-103">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="8b285-104">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 Microsoft SQL Server(Azure SQL Database 포함)에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8b285-104">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including Azure SQL Database).</span></span> <span data-ttu-id="8b285-105">공급자는 [Entity Framework Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="8b285-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="8b285-106">설치</span><span class="sxs-lookup"><span data-stu-id="8b285-106">Install</span></span>

<span data-ttu-id="8b285-107">[Microsoft.EntityFrameworkCore.SqlServer NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="8b285-107">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="8b285-108">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8b285-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="visual-studio"></a>[<span data-ttu-id="8b285-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b285-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> <span data-ttu-id="8b285-110">버전 3.0.0 이후 공급자는 Microsoft.Data.SqlClient를 참조합니다(System.Data.SqlClient에 종속된 이전 버전).</span><span class="sxs-lookup"><span data-stu-id="8b285-110">Since version 3.0.0, the provider references Microsoft.Data.SqlClient (previous versions depended on System.Data.SqlClient).</span></span> <span data-ttu-id="8b285-111">프로젝트가 SqlClient에 직접 종속될 경우, Microsoft.Data.SqlClient 패키지를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="8b285-111">If your project takes a direct dependency on SqlClient, make sure it references the Microsoft.Data.SqlClient package.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="8b285-112">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="8b285-112">Supported Database Engines</span></span>

* <span data-ttu-id="8b285-113">Microsoft SQL Server(2012 이상)</span><span class="sxs-lookup"><span data-stu-id="8b285-113">Microsoft SQL Server (2012 onwards)</span></span>
