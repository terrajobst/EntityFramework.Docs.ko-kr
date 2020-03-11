---
title: EF Core 릴리스 및 계획
author: ajcvickers
ms.date: 03/03/2020
ms.assetid: C21F89EE-FB08-4ED9-A2A0-76CB7656E6E4
uid: core/what-is-new/index
ms.openlocfilehash: 2c41f65d1fead8430a39c6230a0f22506686504e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413208"
---
# <a name="ef-core-releases-and-planning"></a>EF Core 릴리스 및 계획

## <a name="stable-releases"></a>안정적인 릴리스

| Release | 대상 프레임워크 | 지원 만료일: | 링크
|:--------|------------------|-----------------|------
| [EF Core 3.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.1.2) | .NET Standard 2.0 | 2022년 12월 3일(LTS) | [알림](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-3-1-and-entity-framework-6-4/)
| ~~[EF Core 3.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.3)~~ | .NET Standard 2.1 | 2020년 3월 3일 만료됨 | [알림](https://devblogs.microsoft.com/dotnet/announcing-ef-core-3-0-and-ef-6-3-general-availability/) / [주요 변경 내용](ef-core-3.0/breaking-changes.md)
| ~~[EF Core 2.2](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.2.6)~~ | .NET Standard 2.0 | 2019년 12월 23일 만료됨 | [알림](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-2/)
| [EF Core 2.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.1.14) | .NET Standard 2.0 | 2021년 8월 21일(LTS) | [알림](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-1/)
| ~~[EF Core 2.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.0.3)~~ | .NET Standard 2.0 | 2018년 10월 1일 만료됨 | [알림](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-0/)
| ~~[EF Core 1.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.1.6)~~ | .NET Standard 1.3 | 2019년 6월 27일 만료됨 | [알림](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-1-1/)
| ~~[EF Core 1.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.0.6)~~ | .NET Standard 1.3 | 2019년 6월 27일 만료됨 | [알림](https://devblogs.microsoft.com/dotnet/entity-framework-core-1-0-0-available/)

각 EF Core 릴리스에서 지원되는 특정 플랫폼에 대한 자세한 내용은 [지원되는 플랫폼](../platforms/index.md)을 참조하세요.

지원 만료 및 LTS(장기 지원) 릴리스에 대한 자세한 내용은 [.NET 지원 정책](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)을 참조하세요.

## <a name="guidance-on-updating-to-new-releases"></a>새 릴리스로 업데이트에 대한 지침

* 보안 및 기타 치명적 버그에 대해 지원되는 릴리스를 패치합니다. 항상 제공된 릴리스의 최신 패치를 사용하세요. 예를 들어 EF Core 2.1의 경우 2.1.14를 사용하세요.
* 주 버전 업데이트(예: EF Core 2에서 EF Core 3)에는 종종 주요 변경 내용이 포함되어 있습니다. 주요 버전을 업데이트할 때는 철저한 테스트를 수행하는 것이 좋습니다. 주요 변경 내용을 처리하는 방법에 대한 지침은 위의 주요 변경 내용 링크를 사용하세요.
* 부 버전 업데이트에는 일반적으로 주요 변경 내용이 포함되지 않습니다. 그러나 새로운 기능으로 인해 재발이 발생할 수 있으므로 철저한 테스트는 여전히 권장됩니다.

## <a name="release-planning-and-schedules"></a>릴리스 계획 및 일정

EF Core 릴리스는 [.NET Core 배송 일정](https://github.com/dotnet/core/blob/master/roadmap.md)에 따라 정렬됩니다.

패치 릴리스는 일반적으로 매월 제공되지만 리드 타임이 깁니다.
이를 개선하기 위해 노력하고 있습니다.

각 릴리스에 제공되는 항목을 결정하는 방법에 대한 자세한 내용은 [릴리스 계획 프로세스](release-planning.md)를 참조하세요.
일반적으로 다음 주 또는 부 버전보다 더 세부적으로 계획하지는 않습니다.

## <a name="ef-core-50"></a>EF Core 5.0

다음 계획된 안정적인 릴리스는 **EF Core 5.0**이며 2020년 11월에 예정되어 있습니다.

[EF Core 5.0에 대한 고급 계획](ef-core-5.0/plan.md)은 문서화된 다음 [릴리스 계획 프로세스](release-planning.md)에 따라 작성되었습니다.

계획에 대한 피드백은 중요합니다.
이슈의 중요성을 나타내는 가장 좋은 방법은 GitHub에서 해당 이슈에 대해 투표(엄지손가락을 위로 👍)하는 것입니다.
그런 다음 이 데이터는 다음 릴리스를 위해 계획 프로세스에 반영됩니다.

### <a name="get-it-now"></a>지금 바로 사용하세요!

EF Core 5.0 패키지는 [일별 빌드](https://github.com/aspnet/AspNetCore/blob/master/docs/DailyBuilds.md)로 **지금 사용할 수 있습니다**. 

일별 빌드를 사용하는 것은 이슈를 찾고 가능한 한 빨리 피드백을 제공하는 좋은 방법입니다.
그런 피드백을 빨리 제공할수록 다음 공식 릴리스 전에 실행될 가능성이 커집니다.
각 빌드마다 플랫폼 당 56,000회 이상의 테스트를 실행하여 일별 빌드를 양호한 상태로 유지하기 위해 노력하고 있습니다.

미리 보기 패키지는 연말에 NuGet에 제공됩니다.
