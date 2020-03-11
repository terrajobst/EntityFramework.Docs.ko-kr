---
title: 팀 환경에서의 마이그레이션-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/teams
ms.openlocfilehash: 6c17c56277821159962884aef72d46c624442e20
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414244"
---
# <a name="migrations-in-team-environments"></a>팀 환경의 마이그레이션

팀 환경에서 마이그레이션을 사용 하는 경우 모델 스냅숏 파일에 주의를 기울여야 합니다. 이 파일은 사용자의 동료가 자신에 게 완전히 병합 되어 있는지 또는 공유 하기 전에 마이그레이션을 다시 만들어 충돌을 해결 해야 하는지 여부를 알려줍니다.

## <a name="merging"></a>병합

팀 동료 로부터 마이그레이션을 병합할 때 모델 스냅숏 파일에서 충돌이 발생할 수 있습니다. 두 변경 사항이 관련이 없는 경우에는 병합이 간단 하 고 두 개의 마이그레이션이 공존할 수 있습니다. 예를 들어 고객 엔터티 형식 구성에서 다음과 같은 병합 충돌이 발생할 수 있습니다.

``` output
<<<<<<< Mine
b.Property<bool>("Deactivated");
=======
b.Property<int>("LoyaltyPoints");
>>>>>>> Theirs
```

이러한 속성은 모두 최종 모델에 있어야 하므로 두 속성을 추가 하 여 병합을 완료 합니다. 대부분의 경우 버전 제어 시스템에서 자동으로 이러한 변경 내용을 병합할 수 있습니다.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

이러한 경우 마이그레이션과 동료가 서로 독립적으로 진행 됩니다. 둘 중 하나가 먼저 적용 될 수 있으므로, 팀과 공유 하기 전에 마이그레이션을 추가로 변경할 필요가 없습니다.

## <a name="resolving-conflicts"></a>충돌 해결

모델 스냅숏 모델을 병합할 때 true 충돌이 발생할 수 있습니다. 예를 들어 사용자와 사용자의 동료가 각각 동일한 속성의 이름을 바꿀 수 있습니다.

``` output
<<<<<<< Mine
b.Property<string>("Username");
=======
b.Property<string>("Alias");
>>>>>>> Theirs
```

이러한 종류의 충돌이 발생 하면 마이그레이션을 다시 만들어 문제를 해결 합니다. 다음 단계를 수행하세요.

1. 병합을 중단 하 고 병합 하기 전에 작업 디렉터리로 롤백
2. 마이그레이션 제거 (그러나 모델 변경 내용 유지)
3. 작업 디렉터리에 동료가 변경한 내용 병합
4. 마이그레이션 다시 추가

이 작업을 수행한 후에는 두 개의 마이그레이션을 올바른 순서로 적용할 수 있습니다. 먼저 마이그레이션이 적용 되 고, 열 이름을 *Alias*로 바꾸고, 마이그레이션한 후 *사용자 이름*으로 이름을 바꿉니다.

마이그레이션은 팀의 나머지 부분과 안전 하 게 공유할 수 있습니다.
