---
title: 팀 환경-EF Core 마이그레이션
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: e8ff7f468d5ab6dbd6285f1abf9199e413288d10
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997697"
---
<a name="migrations-in-team-environments"></a>팀 환경에서 마이그레이션
===============================
마이그레이션 팀 환경에서에서 작업할 때에 모델 스냅숏 파일에 추가 주의 해야 합니다. 이 파일에 팀 동료의 마이그레이션 기업과 완전히 병합 또는 공유 하기 전에 먼저 마이그레이션을 다시 만들어 충돌을 해결 해야 할 경우 알려 줄 수 있습니다.

<a name="merging"></a>병합
-------
마이그레이션에서 동료를 병합 하면 모델 스냅숏 파일에서 충돌을 발생할 수 있습니다. 변경 내용을 모두 관련 없으면 병합 trivial 이며 두 마이그레이션 공존할 수 있습니다. 예를 들어, 다음과 같은 고객 엔터티 유형의 구성에 병합 충돌이 발생할 수 있습니다.

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

이러한 속성을 모두 최종 모델에 존재 해야, 하므로 두 속성을 추가 하 여 병합을 완료 합니다. 대부분의 경우에서 버전 제어 시스템 수에 대 한 이러한 변경 내용을 자동으로 병합 될 수 있습니다.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

이러한 경우 팀 동료의 마이그레이션하고 마이그레이션에는 서로 독립적입니다. 둘 중 하나가 먼저 적용할 수 없습니다, 이후 마이그레이션 팀과 공유 하기 전에 추가로 변경할 필요가 없습니다.

<a name="resolving-conflicts"></a>충돌 해결
-------------------
경우에 따라 스냅숏 model 병합할 때 true 충돌이 발생 합니다. 예를 들어 하 고 동료가 각 바꿀 수도 동일한 속성입니다.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

이러한 종류의 충돌을 발생 하는 경우에 마이그레이션을 다시 만들어 해결 합니다. 아래 단계를 수행합니다.

1. 병합 및 병합 하기 전에 작업 디렉터리에 롤백 중단
2. 마이그레이션 제거 (하지만 모델 변경 내용을 유지)
3. 동료가 변경 내용을 작업 디렉터리에 병합
4. 마이그레이션을 다시 추가

그러면 두 마이그레이션이 올바른 순서로 적용할 수 있습니다. 해당 마이그레이션 먼저 적용 된 열 이름 바꾸기 *별칭*, 이후 마이그레이션 이름을 바꿉니다 하 *Username*합니다.

마이그레이션 팀의 나머지 부분을 사용 하 여 안전 하 게 공유할 수 있습니다.
