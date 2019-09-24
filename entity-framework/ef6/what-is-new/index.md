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
# <a name="whats-new-in-ef6"></a>EF6의 새로운 기능

최신 기능 및 높은 안정성을 얻을 수 있도록 Entity Framework 최신 릴리스 버전을 사용하는 것이 좋습니다.
하지만 이전 버전을 사용해야 하거나 최신 시험판 릴리스의 개선된 기능을 사용해 보려는 분들도 있다는 사실을 잘 알고 있습니다.
EF 특정 버전을 설치하는 방법은 [Entity Framework 받기](~/ef6/fundamentals/install.md)를 참조하세요.

## <a name="ef-630"></a>EF 6.3.0

EF 6.3.0 런타임은 2019년 9월에 NuGet에서 출시되었습니다. 이 릴리스의 주요 목표는 EF 6을 사용하는 기존 애플리케이션을 .NET Core 3.0으로 손쉽게 마이그레이션하는 것이었습니다. 커뮤니티 역시 여러 버그 수정과 기능 향상에 기여했습니다. 자세한 내용은 각 6.3.0 [마일스톤](https://github.com/aspnet/EntityFramework6/milestones?state=closed)에서 종결된 문제를 참조하세요. 다음은 몇 가지 주목할 만한 것입니다.

- .NET Core 3.0 지원
  - 이제 EntityFramework 패키지는 .NET Framework 4.x 외에도 .NET Standard 2.1을 대상으로 합니다.
  - 마이그레이션 명령이 프로세스에서 실행되고 SDK 스타일 프로젝트에서 사용할 수 있도록 다시 작성되었습니다.
- SQL Server HierarchyId 지원
- Roslyn 및 NuGet PackageReference와의 호환성이 향상되었습니다.
- 어셈블리로부터 마이그레이션을 사용, 추가, 스크립팅 및 적용하기 위한 ef6.exe를 추가했습니다. 이는 migrate.exe를 대체합니다.

## <a name="past-releases"></a>이전 릴리스

[이전 릴리스](past-releases.md) 페이지에는 모든 EF 이전 버전 및 각 릴리스에 도입된 주요 기능 아카이브가 포함되어 있습니다.
