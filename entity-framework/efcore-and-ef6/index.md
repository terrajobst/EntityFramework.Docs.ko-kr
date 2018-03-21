---
title: "EF Core & EF6 비교"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 4609ecbc9e24d8a359694d256523c64141b5ff62
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2018
---
# <a name="compare-ef-core--ef6"></a>EF Core & EF6 비교

Entity Framework, Entity Framework Core 및 Entity Framework 6에는 두 가지 다른 버전이 있습니다.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6(EF6)은 수년 간 기능 및 안정화를 통해 테스트와 시험을 거친 데이터 액세스 기술입니다. 2008년에 .NET Framework 3.5 SP1 및 Visual Studio 2008 SP1의 일부로 처음 릴리스되었습니다. EF4.1 릴리스부터는 현재 NuGet.org에서 가장 인기 있는 패키지 중 하나인 [EntityFramework NuGet 패키지](https://www.nuget.org/packages/EntityFramework/)가 탑재되었습니다.

EF6은 계속 지우너되는 제품이며 앞으로 버그 수정 사항과 작은 개선이 이어질 것입니다.

## <a name="entity-framework-core"></a>Entity Framework Core

EF Core(Entity Framework Core)는 Entity Framework의 가볍고 확장 가능하며 플랫폼 교차적인 버전입니다. EF Core는 EF6에 비해 여러 개선 사항과 새 기능이 도입되었습니다. 동시에 EF Core는 새 코드 베이스로, EF6처럼 성숙하지는 않습니다.

EF Core는 EF6의 개발자 경험을 바탕으로 하고 대부분의 상위 API가 그대로 유지되므로 EF6을 사용했다면 EF Core가 매우 친숙할 것입니다. 그러면서도 EF Core는 완전히 새로운 핵심 구성 요소를 기반으로 구축되었습니다. 즉 EF Core가 EF6의 모든 기능을 자동으로 상속하지는 않습니다. 일부 기능은 향후 릴리스에서 제공할 것이지만, 그 밖에 덜 사용되는 기능은 EF Core에서 구현되지 않습니다.

확장 가능하고 가벼운 새 코어에서도 EF6에서 구현되지 않은 몇 가지 중요한 기능을 EF Core에서 추가할 수 있습니다(예: LINQ 쿼리에서의 대체 키, 일괄 업데이트, 혼합 클라이언트/데이터베이스 평가).
