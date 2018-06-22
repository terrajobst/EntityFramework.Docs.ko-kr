---
title: 팀 환경-EF 코어에서 마이그레이션
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
ms.locfileid: "26053803"
---
<a name="migrations-in-team-environments"></a>팀 환경에서 마이그레이션
===============================
팀 환경에서 마이그레이션 작업을 하는 경우 모델 스냅숏 파일에 부가적인 주의 해야 합니다. 이 파일 하는 경우 이렇게 마이그레이션 병합 완전히의 기업과 것을 공유 하기 전에 마이그레이션을 다시 만들어서 충돌을 해결 하는 경우를 전달할 수 있습니다.

<a name="merging"></a>병합
-------
다른 팀 멤버에서 마이그레이션을 병합할 때 모델 스냅숏 파일에서 충돌을 발생할 수 있습니다. 변경 내용이 모두 관련 없는 경우 병합 trivial 이며 두 마이그레이션 공존할 수 있습니다. 예를 들어 다음과 같은 고객 엔터티 유형의 구성에 병합 충돌이 발생할 수 있습니다.

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

최종 모델에 있는 필요가 이러한 두 속성이 없기 때문에 두 속성을 추가 하 여 병합을 완료 합니다. 대부분의 경우에서 버전 제어 시스템을 이러한 변경 내용이 자동으로 병합 될 수 있습니다.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

이러한 경우 마이그레이션 및 팀 동료의 마이그레이션은 서로 독립적입니다. 둘 중 하나가 적용 되지 않아 먼저 하므로 팀과 공유 하기 전에 마이그레이션에 추가로 변경할 필요가 없습니다.

<a name="resolving-conflicts"></a>충돌 해결
-------------------
경우에 따라 모델 스냅숏 모델 병합할 때 true 충돌이 발생 합니다. 예를 들어 사용자와 동료가 각 바꿀 수도 동일한 속성입니다.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

이러한 종류의 충돌을 발생 하는 경우에 마이그레이션을 다시 만들어서 해결 합니다. 아래 단계를 수행합니다.

1. 병합 및 rollback을 병합 하기 전에 작업 디렉터리를 중단 합니다.
2. 마이그레이션 제거 (하지만 모델 변경 내용을 유지)
3. 이렇게 변경 내용을 작업 디렉터리에 병합
4. 마이그레이션을 다시 추가

이 작업을 수행한 후 두 마이그레이션 올바른 순서로 적용할 수 있습니다. 마이그레이션을 먼저 적용 된 다음에 열 이름을 바꾸면 *별칭*, 그 후 마이그레이션 이름을 바꾸거나를 *사용자 이름*합니다.

마이그레이션은 팀의 나머지와 안전 하 게 공유할 수 있습니다.
