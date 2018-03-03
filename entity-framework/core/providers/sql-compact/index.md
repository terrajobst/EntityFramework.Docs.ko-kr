---
title: "Microsoft SQL Server Compact Edition 데이터베이스 공급자 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a><span data-ttu-id="6e5e7-102">Microsoft SQL Server Compact Edition EF Core 데이터베이스 공급자 </span><span class="sxs-lookup"><span data-stu-id="6e5e7-102">Microsoft SQL Server Compact Edition EF Core Database Provider</span></span>

<span data-ttu-id="6e5e7-103">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 SQL Server Compact Edition에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6e5e7-103">This database provider allows Entity Framework Core to be used with SQL Server Compact Edition.</span></span> <span data-ttu-id="6e5e7-104">이 공급자는[ErikEJ/EntityFramework.SqlServerCompact GitHub 프로젝트](https://github.com/ErikEJ/EntityFramework.SqlServerCompact)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="6e5e7-104">The provider is maintained as part of the [ErikEJ/EntityFramework.SqlServerCompact GitHub Project](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span>

> [!NOTE]  
> <span data-ttu-id="6e5e7-105">이 공급자는 Entity Framework Core 프로젝트의 일부로 유지 관리되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="6e5e7-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="6e5e7-106">타사 공급자를 고려할 때는 품질, 라이선싱, 지원 등을 평가하여 요구 사항에 부합하는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="6e5e7-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="6e5e7-107">설치</span><span class="sxs-lookup"><span data-stu-id="6e5e7-107">Install</span></span>

<span data-ttu-id="6e5e7-108">SQL Server Compact Edition 4.0을 사용하려면 [EntityFrameworkCore.SqlServerCompact40 NuGet 패키지](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="6e5e7-108">To work with SQL Server Compact Edition 4.0, install the [EntityFrameworkCore.SqlServerCompact40 NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

<span data-ttu-id="6e5e7-109">SQL Server Compact Edition 3.5를 사용하려면 [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="6e5e7-109">To work with SQL Server Compact Edition 3.5, install the [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a><span data-ttu-id="6e5e7-110">시작</span><span class="sxs-lookup"><span data-stu-id="6e5e7-110">Get Started</span></span>

<span data-ttu-id="6e5e7-111">[프로젝트 사이트의 시작 설명서](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications) 참조</span><span class="sxs-lookup"><span data-stu-id="6e5e7-111">See the [getting started documentation on the project site](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="6e5e7-112">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="6e5e7-112">Supported Database Engines</span></span>

* <span data-ttu-id="6e5e7-113">SQL Server Compact Edition 3.5</span><span class="sxs-lookup"><span data-stu-id="6e5e7-113">SQL Server Compact Edition 3.5</span></span>

* <span data-ttu-id="6e5e7-114">SQL Server Compact Edition 4.0</span><span class="sxs-lookup"><span data-stu-id="6e5e7-114">SQL Server Compact Edition 4.0</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="6e5e7-115">지원되는 플랫폼</span><span class="sxs-lookup"><span data-stu-id="6e5e7-115">Supported Platforms</span></span>

* <span data-ttu-id="6e5e7-116">.NET Framework(4.5.1부터)</span><span class="sxs-lookup"><span data-stu-id="6e5e7-116">.NET Framework (4.5.1 onwards)</span></span>
