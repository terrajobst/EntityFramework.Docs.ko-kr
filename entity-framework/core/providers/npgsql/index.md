---
title: "PostgreSQL용 Npgsql 데이터베이스 공급자 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a><span data-ttu-id="6aeae-102">PostgreSQL용 Npgsql EF Core 데이터베이스 공급자</span><span class="sxs-lookup"><span data-stu-id="6aeae-102">Npgsql EF Core Database Provider for PostgreSQL</span></span>

<span data-ttu-id="6aeae-103">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 PostgreSQL에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6aeae-103">This database provider allows Entity Framework Core to be used with PostgreSQL.</span></span> <span data-ttu-id="6aeae-104">이 공급자는 [Npgsql 프로젝트](http://www.npgsql.org)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="6aeae-104">The provider is maintained as part of the [Npgsql project](http://www.npgsql.org).</span></span>

> [!NOTE]  
> <span data-ttu-id="6aeae-105">이 공급자는 Entity Framework Core 프로젝트의 일부로 유지 관리되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="6aeae-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="6aeae-106">타사 공급자를 고려할 때는 품질, 라이선싱, 지원 등을 평가하여 요구 사항에 부합하는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="6aeae-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="6aeae-107">설치</span><span class="sxs-lookup"><span data-stu-id="6aeae-107">Install</span></span>

<span data-ttu-id="6aeae-108">[Npgsql.EntityFrameworkCore.PostgreSQL NuGet 패키지](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="6aeae-108">Install the [Npgsql.EntityFrameworkCore.PostgreSQL NuGet package](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span></span>

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a><span data-ttu-id="6aeae-109">시작</span><span class="sxs-lookup"><span data-stu-id="6aeae-109">Get Started</span></span>

<span data-ttu-id="6aeae-110">시작은 [Npgsql 설명서](http://www.npgsql.org/efcore/index.html)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="6aeae-110">See the [Npgsql documentation](http://www.npgsql.org/efcore/index.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="6aeae-111">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="6aeae-111">Supported Database Engines</span></span>

* <span data-ttu-id="6aeae-112">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="6aeae-112">PostgreSQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="6aeae-113">지원되는 플랫폼</span><span class="sxs-lookup"><span data-stu-id="6aeae-113">Supported Platforms</span></span>

* <span data-ttu-id="6aeae-114">.NET Framework(4.5.1부터)</span><span class="sxs-lookup"><span data-stu-id="6aeae-114">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="6aeae-115">.NET Core</span><span class="sxs-lookup"><span data-stu-id="6aeae-115">.NET Core</span></span>

* <span data-ttu-id="6aeae-116">Mono(4.2.0부터)</span><span class="sxs-lookup"><span data-stu-id="6aeae-116">Mono (4.2.0 onwards)</span></span>
