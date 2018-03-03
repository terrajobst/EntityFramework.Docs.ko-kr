---
title: "Pomelo MySQL 데이터베이스 공급자 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d0198c04-d30d-4419-98f8-a54690cea3c8
ms.technology: entity-framework-core
uid: core/providers/pomelo/index
ms.openlocfilehash: 9ddcda1ae7b058c01937867817e2666b97e7f37a
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="pomelo-ef-core-database-provider-for-mysql"></a><span data-ttu-id="38bc4-102">MySQL용 Pomelo EF Core 데이터베이스 공급자</span><span class="sxs-lookup"><span data-stu-id="38bc4-102">Pomelo EF Core Database Provider for MySQL</span></span>

<span data-ttu-id="38bc4-103">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 MySQL에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="38bc4-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="38bc4-104">이 공급자는 [Pomelo Foundation 프로젝트](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="38bc4-104">The provider is maintained as part of the [Pomelo Foundation Project](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="38bc4-105">이 공급자는 Entity Framework Core 프로젝트의 일부로 유지 관리되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="38bc4-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="38bc4-106">타사 공급자를 고려할 때는 품질, 라이선싱, 지원 등을 평가하여 요구 사항에 부합하는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="38bc4-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="38bc4-107">설치</span><span class="sxs-lookup"><span data-stu-id="38bc4-107">Install</span></span>

<span data-ttu-id="38bc4-108">[Pomelo.EntityFrameworkCore.MySql NuGet 패키지](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="38bc4-108">Install the [Pomelo.EntityFrameworkCore.MySql NuGet package](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span></span>

``` powershell
Install-Package Pomelo.EntityFrameworkCore.MySql
```

## <a name="get-started"></a><span data-ttu-id="38bc4-109">시작</span><span class="sxs-lookup"><span data-stu-id="38bc4-109">Get Started</span></span>

<span data-ttu-id="38bc4-110">다음 리소스는 이 공급자를 시작하는 데 도움이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="38bc4-110">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="38bc4-111">시작 설명서</span><span class="sxs-lookup"><span data-stu-id="38bc4-111">Getting started documentation</span></span>](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md#getting-started)

* [<span data-ttu-id="38bc4-112">Yuuko 블로그 샘플 응용 프로그램</span><span class="sxs-lookup"><span data-stu-id="38bc4-112">Yuuko Blog sample application</span></span>](https://github.com/PomeloFoundation/YuukoBlog)

## <a name="supported-database-engines"></a><span data-ttu-id="38bc4-113">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="38bc4-113">Supported Database Engines</span></span>

* <span data-ttu-id="38bc4-114">MySQL</span><span class="sxs-lookup"><span data-stu-id="38bc4-114">MySQL</span></span>
* <span data-ttu-id="38bc4-115">MariaDB</span><span class="sxs-lookup"><span data-stu-id="38bc4-115">MariaDB</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="38bc4-116">지원되는 플랫폼</span><span class="sxs-lookup"><span data-stu-id="38bc4-116">Supported Platforms</span></span>

* <span data-ttu-id="38bc4-117">.NET Framework(4.5.1부터)</span><span class="sxs-lookup"><span data-stu-id="38bc4-117">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="38bc4-118">.NET Core</span><span class="sxs-lookup"><span data-stu-id="38bc4-118">.NET Core</span></span>

* <span data-ttu-id="38bc4-119">Mono(4.2.0부터)</span><span class="sxs-lookup"><span data-stu-id="38bc4-119">Mono (4.2.0 onwards)</span></span>
