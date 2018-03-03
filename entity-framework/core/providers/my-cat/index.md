---
title: "Pomelo MyCat 데이터베이스 공급자 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 02/27/2017
ms.assetid: ad798f02-a7a4-45c1-b0a9-8e92f5dc6ff0
ms.technology: entity-framework-core
uid: core/providers/my-cat/index
ms.openlocfilehash: e13da3ab47e56d1b869e445f2398eda6ea84a79f
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="pomelo-mycat-ef-core-database-provider"></a><span data-ttu-id="882c5-102">Pomelo MyCat EF Core 데이터베이스 공급자</span><span class="sxs-lookup"><span data-stu-id="882c5-102">Pomelo MyCat EF Core Database Provider</span></span>

<span data-ttu-id="882c5-103">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 [MyCat](https://github.com/MyCATApache/Mycat-Server)에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="882c5-103">This database provider allows Entity Framework Core to be used with [MyCat](https://github.com/MyCATApache/Mycat-Server).</span></span> <span data-ttu-id="882c5-104">이 공급자는 [Pomelo Foundation 프로젝트](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="882c5-104">The provider is maintained as part of the [Pomelo Foundation Project](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy).</span></span>

> [!NOTE]  
> <span data-ttu-id="882c5-105">이 공급자는 Entity Framework Core 프로젝트의 일부로 유지 관리되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="882c5-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="882c5-106">타사 공급자를 고려할 때는 품질, 라이선싱, 지원 등을 평가하여 요구 사항에 부합하는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="882c5-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="882c5-107">설치</span><span class="sxs-lookup"><span data-stu-id="882c5-107">Install</span></span>

<span data-ttu-id="882c5-108">[프로젝트 사이트에서 최신 릴리스](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy/releases)를 다운로드하여 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="882c5-108">Download and run [the latest release from the project site](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy/releases).</span></span>

## <a name="get-started"></a><span data-ttu-id="882c5-109">시작</span><span class="sxs-lookup"><span data-stu-id="882c5-109">Get Started</span></span>

<span data-ttu-id="882c5-110">다음 리소스는 이 공급자를 시작하는 데 도움이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="882c5-110">The following resources will help you get started with this provider.</span></span>
 * [<span data-ttu-id="882c5-111">설정 단계</span><span class="sxs-lookup"><span data-stu-id="882c5-111">Setup steps</span></span>](https://github.com/aspnet/EntityFramework.Docs/issues/252)
 * [<span data-ttu-id="882c5-112">시작 비디오</span><span class="sxs-lookup"><span data-stu-id="882c5-112">Getting started video</span></span>](https://www.youtube.com/watch?v=q0CXfFNtMZo)

## <a name="supported-database-engines"></a><span data-ttu-id="882c5-113">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="882c5-113">Supported Database Engines</span></span>

* <span data-ttu-id="882c5-114">MyCat</span><span class="sxs-lookup"><span data-stu-id="882c5-114">MyCat</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="882c5-115">지원되는 플랫폼</span><span class="sxs-lookup"><span data-stu-id="882c5-115">Supported Platforms</span></span>

* <span data-ttu-id="882c5-116">.NET Framework(4.5.1부터)</span><span class="sxs-lookup"><span data-stu-id="882c5-116">.NET Framework (4.5.1 onwards)</span></span>
* <span data-ttu-id="882c5-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="882c5-117">.NET Core</span></span>
* <span data-ttu-id="882c5-118">Mono(4.2.0부터)</span><span class="sxs-lookup"><span data-stu-id="882c5-118">Mono (4.2.0 onwards)</span></span>
