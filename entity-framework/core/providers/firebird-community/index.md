---
title: "FirebirdSQL 데이터베이스 공급자 - EF Core"
author: ralmsdeveloper
ms.author: ralmsdeveloper
ms.date: 11/22/2017
ms.assetid: d0168c04-d30d-4219-98f8-a54690cea3c6
ms.technology: entity-framework-core
uid: core/providers/firebird-community/index
ms.openlocfilehash: 682988a91ef04dbd552588a537f53124b931f17d
ms.sourcegitcommit: 1cbd3d3cd92bdaf8223b8821c58200bcfed10ede
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/23/2017
---
# <a name="firebird-ef-core-database-provider"></a><span data-ttu-id="7bc1f-102">Firebird EF Core 데이터베이스 공급자</span><span class="sxs-lookup"><span data-stu-id="7bc1f-102">Firebird EF Core Database Provider</span></span>

<span data-ttu-id="7bc1f-103">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 FirebirdSQL에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7bc1f-103">This database provider allows Entity Framework Core to be used with FirebirdSQL.</span></span> <span data-ttu-id="7bc1f-104">이 공급자는[ralmsdeveloper/EntityFrameworkCore.FirebirdSQL GitHub 프로젝트](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="7bc1f-104">The provider is maintained as part of the [ralmsdeveloper/EntityFrameworkCore.FirebirdSQL GitHub Project](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="7bc1f-105">이 공급자는 Entity Framework Core 프로젝트의 일부로 유지 관리되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="7bc1f-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="7bc1f-106">타사 공급자를 고려할 때는 품질, 라이선싱, 지원 등을 평가하여 요구 사항에 부합하는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="7bc1f-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="7bc1f-107">설치</span><span class="sxs-lookup"><span data-stu-id="7bc1f-107">Install</span></span>

<span data-ttu-id="7bc1f-108">[EntityFrameworkCore.FirebirdSQL NuGet 패키지](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="7bc1f-108">Install the [EntityFrameworkCore.FirebirdSQL NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span></span>

``` powershell
Install-Package EntityFrameworkCore.FirebirdSQL
```

## <a name="get-started"></a><span data-ttu-id="7bc1f-109">시작</span><span class="sxs-lookup"><span data-stu-id="7bc1f-109">Get Started</span></span>

<span data-ttu-id="7bc1f-110">[프로젝트 사이트의 시작 설명서](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki) 참조</span><span class="sxs-lookup"><span data-stu-id="7bc1f-110">See the [getting started documentation on the project site](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="7bc1f-111">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="7bc1f-111">Supported Database Engines</span></span>

* <span data-ttu-id="7bc1f-112">FirebirdSql 2.5</span><span class="sxs-lookup"><span data-stu-id="7bc1f-112">FirebirdSql 2.5</span></span>
* <span data-ttu-id="7bc1f-113">FirebirdSql 3.X</span><span class="sxs-lookup"><span data-stu-id="7bc1f-113">FirebirdSql 3.X</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="7bc1f-114">지원되는 플랫폼</span><span class="sxs-lookup"><span data-stu-id="7bc1f-114">Supported Platforms</span></span>

* <span data-ttu-id="7bc1f-115">.NET Framework(4.5.1부터)</span><span class="sxs-lookup"><span data-stu-id="7bc1f-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="7bc1f-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="7bc1f-116">.NET Core</span></span>

* <span data-ttu-id="7bc1f-117">Mono(4.2.0부터)</span><span class="sxs-lookup"><span data-stu-id="7bc1f-117">Mono (4.2.0 onwards)</span></span>
