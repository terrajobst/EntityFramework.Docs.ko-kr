---
title: "Devart 데이터베이스 공급자 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: aad70a75-d04d-4d63-a489-7f9062a3c73d
ms.technology: entity-framework-core
uid: core/providers/devart/index
ms.openlocfilehash: 04de917b3327a6f544292781bca42a1170c199ad
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="devart-ef-core-database-providers"></a><span data-ttu-id="87bac-102">Devart EF Core 데이터베이스 공급자</span><span class="sxs-lookup"><span data-stu-id="87bac-102">Devart EF Core Database Providers</span></span>

<span data-ttu-id="87bac-103">Devart는 광범위한 데이터베이스에 대한 데이터베이스 공급자를 제공하는 타사 공급자 작성기입니다.</span><span class="sxs-lookup"><span data-stu-id="87bac-103">Devart is a third party provider writer that offers database providers for a wide range of databases.</span></span> <span data-ttu-id="87bac-104">자세한 내용은 [devart.com/dotconnect](https://www.devart.com/dotconnect/)에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="87bac-104">Find out more at [devart.com/dotconnect](https://www.devart.com/dotconnect/).</span></span>

> [!NOTE]  
> <span data-ttu-id="87bac-105">이 공급자는 Entity Framework Core 프로젝트의 일부로 유지 관리되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="87bac-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="87bac-106">타사 공급자를 고려할 때는 품질, 라이선싱, 지원 등을 평가하여 요구 사항에 부합하는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="87bac-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="paid-versions-only"></a><span data-ttu-id="87bac-107">유료 버전만</span><span class="sxs-lookup"><span data-stu-id="87bac-107">Paid Versions Only</span></span>

<span data-ttu-id="87bac-108">Devart dotConnect은 상용 타사 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="87bac-108">Devart dotConnect is a commercial third party provider.</span></span> <span data-ttu-id="87bac-109">Entity Framework는 유료 dotConnect 버전에서만 지원됩니다.</span><span class="sxs-lookup"><span data-stu-id="87bac-109">Entity Framework support is only available in paid versions of dotConnect.</span></span>

## <a name="install"></a><span data-ttu-id="87bac-110">설치</span><span class="sxs-lookup"><span data-stu-id="87bac-110">Install</span></span>

<span data-ttu-id="87bac-111">설치 지침은 [Devart dotConnect 설명서](https://www.devart.com/dotconnect/)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="87bac-111">See the [Devart dotConnect documentation](https://www.devart.com/dotconnect/) for installation instructions.</span></span>

## <a name="get-started"></a><span data-ttu-id="87bac-112">시작</span><span class="sxs-lookup"><span data-stu-id="87bac-112">Get Started</span></span>

<span data-ttu-id="87bac-113">시작은 [Devart dotConnect Entity Framework 설명서](https://www.devart.com/dotconnect/entityframework.html) 및 [Entity Framework Core 1 지원에 관한 블로그 글](http://blog.devart.com/entity-framework-core-1-entity-framework-7-support.html)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="87bac-113">See the [Devart dotConnect Entity Framework documentation](https://www.devart.com/dotconnect/entityframework.html) and [blog article about Entity Framework Core 1 Support](http://blog.devart.com/entity-framework-core-1-entity-framework-7-support.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="87bac-114">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="87bac-114">Supported Database Engines</span></span>

* <span data-ttu-id="87bac-115">MySQL</span><span class="sxs-lookup"><span data-stu-id="87bac-115">MySQL</span></span>

* <span data-ttu-id="87bac-116">Oracle</span><span class="sxs-lookup"><span data-stu-id="87bac-116">Oracle</span></span>

* <span data-ttu-id="87bac-117">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="87bac-117">PostgreSQL</span></span>

* <span data-ttu-id="87bac-118">SQLite</span><span class="sxs-lookup"><span data-stu-id="87bac-118">SQLite</span></span>

* <span data-ttu-id="87bac-119">DB2</span><span class="sxs-lookup"><span data-stu-id="87bac-119">DB2</span></span>

* [<span data-ttu-id="87bac-120">클라우드 앱</span><span class="sxs-lookup"><span data-stu-id="87bac-120">Cloud apps</span></span>](https://www.devart.com/dotconnect/#cloud)

## <a name="supported-platforms"></a><span data-ttu-id="87bac-121">지원되는 플랫폼</span><span class="sxs-lookup"><span data-stu-id="87bac-121">Supported Platforms</span></span>

* <span data-ttu-id="87bac-122">.NET Framework(4.5.1부터)</span><span class="sxs-lookup"><span data-stu-id="87bac-122">.NET Framework (4.5.1 onwards)</span></span>
* <span data-ttu-id="87bac-123">.NET Core 1.0 이상(Oracle, MySQL, PostgreSQL, SQLite용 공급자)</span><span class="sxs-lookup"><span data-stu-id="87bac-123">.NET Core 1.0 and higher (providers for Oracle, MySQL, PostgreSQL, SQLite)</span></span>
