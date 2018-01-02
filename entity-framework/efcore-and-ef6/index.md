---
title: "EF Core & EF6 비교"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 4442e6931327f6a07d98aee47211ddbeed51a467
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="compare-ef-core--ef6"></a><span data-ttu-id="8b6b1-102">EF Core & EF6 비교</span><span class="sxs-lookup"><span data-stu-id="8b6b1-102">Compare EF Core & EF6</span></span>

<span data-ttu-id="8b6b1-103">Entity Framework, Entity Framework Core 및 Entity Framework 6에는 두 가지 버전이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-103">There are two versions of Entity Framework, Entity Framework Core and Entity Framework 6.</span></span>

## <a name="entity-framework-6"></a><span data-ttu-id="8b6b1-104">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="8b6b1-104">Entity Framework 6</span></span>

<span data-ttu-id="8b6b1-105">Entity Framework 6(EF6)은 수년 간 기능 및 안정화를 통해 테스트와 시험을 거친 데이터 액세스 기술입니다.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-105">Entity Framework 6 (EF6) is a tried and tested data access technology with many years of features and stabilization.</span></span> <span data-ttu-id="8b6b1-106">2008년에 .NET Framework 3.5 SP1 및 Visual Studio 2008 SP1의 일부로 처음 릴리스되었습니다.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-106">It first released in 2008, as part of .NET Framework 3.5 SP1 and Visual Studio 2008 SP1.</span></span> <span data-ttu-id="8b6b1-107">EF4.1 릴리스부터는 현재 NuGet.org에서 가장 인기 있는 패키지인 [EntityFramework NuGet 패키지](https://www.nuget.org/packages/EntityFramework/)가 탑재되었습니다.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-107">Starting with the EF4.1 release it has shipped as the [EntityFramework NuGet package](https://www.nuget.org/packages/EntityFramework/) - currently the most popular package on NuGet.org.</span></span>

<span data-ttu-id="8b6b1-108">EF6은 계속 지우너되는 제품이며 앞으로 버그 수정 사항과 작은 개선이 이어질 것입니다.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-108">EF6 continues to be a supported product, and will continue to see bug fixes and minor improvements for some time to come.</span></span>

## <a name="entity-framework-core"></a><span data-ttu-id="8b6b1-109">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="8b6b1-109">Entity Framework Core</span></span>

<span data-ttu-id="8b6b1-110">EF Core(Entity Framework Core)는 Entity Framework의 가볍고 확장 가능하며 플랫폼 교차적인 버전입니다.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-110">Entity Framework Core (EF Core) is a lightweight, extensible, and cross-platform version of Entity Framework.</span></span> <span data-ttu-id="8b6b1-111">EF Core는 EF6에 비해 여러 개선 사항과 새 기능이 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-111">EF Core introduces many improvements and new features when compared with EF6.</span></span> <span data-ttu-id="8b6b1-112">동시에 EF Core는 새 코드 베이스로, EF6처럼 성숙하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-112">At the same time, EF Core is a new code base and not as mature as EF6.</span></span>

<span data-ttu-id="8b6b1-113">EF Core는 EF6의 개발자 경험을 바탕으로 하고 대부분의 상위 API가 그대로 유지되므로 EF6을 사용했다면 EF Core가 매우 친숙할 것입니다.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-113">EF Core keeps the developer experience from EF6, and most of the top-level APIs remain the same too, so EF Core will feel very familiar to folks who have used EF6.</span></span> <span data-ttu-id="8b6b1-114">그러면서도 EF Core는 완전히 새로운 핵심 구성 요소를 기반으로 구축되었습니다.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-114">At the same time, EF Core is built over a completely new set of core components.</span></span> <span data-ttu-id="8b6b1-115">즉 EF Core가 EF6의 모든 기능을 자동으로 상속하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-115">This means EF Core doesn't automatically inherit all the features from EF6.</span></span> <span data-ttu-id="8b6b1-116">일부 기능은 향후 릴리스에서 제공할 것이며(예: 지연 로드, 연결 복원력) 그 밖에 덜 사용되는 기능은 EF Core에서 구현되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8b6b1-116">Some of these features will show up in future releases (such as lazy loading and connection resiliency), other less commonly used features will not be implemented in EF Core.</span></span>

<span data-ttu-id="8b6b1-117">확장 가능하고 가벼운 새 코어에서도 EF6에서 구현되지 않은 몇 가지 중요한 기능을 EF Core에서 추가할 수 있습니다(예: LINQ 쿼리에서의 대체 키, 혼합 클라이언트/데이터베이스 평가).</span><span class="sxs-lookup"><span data-stu-id="8b6b1-117">The new, extensible, and lightweight core has also allowed us to add some features to EF Core that will not be implemented in EF6 (such as alternate keys and mixed client/database evaluation in LINQ queries).</span></span>
