---
title: 새로운 기능 - EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 568790d9c9bb7dd2213907bef8fa090710cd3ba0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149130"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="e0354-102">EF6의 새로운 기능</span><span class="sxs-lookup"><span data-stu-id="e0354-102">What's New in EF6</span></span>

<span data-ttu-id="e0354-103">최신 기능 및 높은 안정성을 얻을 수 있도록 Entity Framework 최신 릴리스 버전을 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="e0354-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="e0354-104">하지만 이전 버전을 사용해야 하거나 최신 시험판 릴리스의 개선된 기능을 사용해 보려는 분들도 있다는 사실을 잘 알고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e0354-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="e0354-105">EF 특정 버전을 설치하는 방법은 [Entity Framework 받기](~/ef6/fundamentals/install.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="e0354-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="e0354-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="e0354-106">EF 6.3.0</span></span>

<span data-ttu-id="e0354-107">EF 6.3.0 런타임은 2019년 9월에 NuGet에서 출시되었습니다.</span><span class="sxs-lookup"><span data-stu-id="e0354-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="e0354-108">이 릴리스의 주요 목표는 EF 6을 사용하는 기존 애플리케이션을 .NET Core 3.0으로 손쉽게 마이그레이션하는 것이었습니다.</span><span class="sxs-lookup"><span data-stu-id="e0354-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="e0354-109">커뮤니티 역시 여러 버그 수정과 기능 향상에 기여했습니다.</span><span class="sxs-lookup"><span data-stu-id="e0354-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="e0354-110">자세한 내용은 각 6.3.0 [마일스톤](https://github.com/aspnet/EntityFramework6/milestones?state=closed)에서 종결된 문제를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="e0354-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="e0354-111">다음은 몇 가지 주목할 만한 것입니다.</span><span class="sxs-lookup"><span data-stu-id="e0354-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="e0354-112">.NET Core 3.0 지원</span><span class="sxs-lookup"><span data-stu-id="e0354-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="e0354-113">이제 EntityFramework 패키지는 .NET Framework 4.x 외에도 .NET Standard 2.1을 대상으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="e0354-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x</span></span>
  - <span data-ttu-id="e0354-114">마이그레이션 명령이 프로세스에서 실행되고 SDK 스타일 프로젝트에서 사용할 수 있도록 다시 작성되었습니다.</span><span class="sxs-lookup"><span data-stu-id="e0354-114">The migrations commands have been rewritten to execute out of process and work with SDK-style projects</span></span>
- <span data-ttu-id="e0354-115">SQL Server HierarchyId 지원</span><span class="sxs-lookup"><span data-stu-id="e0354-115">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="e0354-116">Roslyn 및 NuGet PackageReference와의 호환성이 향상되었습니다.</span><span class="sxs-lookup"><span data-stu-id="e0354-116">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="e0354-117">어셈블리로부터 마이그레이션을 사용, 추가, 스크립팅 및 적용하기 위한 ef6.exe를 추가했습니다.</span><span class="sxs-lookup"><span data-stu-id="e0354-117">Added ef6.exe for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="e0354-118">이는 migrate.exe를 대체합니다.</span><span class="sxs-lookup"><span data-stu-id="e0354-118">This replaces migrate.exe</span></span>

## <a name="past-releases"></a><span data-ttu-id="e0354-119">이전 릴리스</span><span class="sxs-lookup"><span data-stu-id="e0354-119">Past Releases</span></span>

<span data-ttu-id="e0354-120">[이전 릴리스](past-releases.md) 페이지에는 모든 EF 이전 버전 및 각 릴리스에 도입된 주요 기능 아카이브가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e0354-120">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
