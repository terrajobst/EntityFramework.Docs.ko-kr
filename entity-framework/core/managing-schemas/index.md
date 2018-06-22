---
title: 데이터베이스 스키마 관리 - EF Core
author: bricelam
ms.author: divega
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 765c80f43832e51471928d5f653aa12c6bd7c7ac
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
ms.locfileid: "26049385"
---
# <a name="managing-database-schemas"></a>데이터베이스 스키마 관리
EF Core는 EF Core 모델과 데이터베이스 스키마를 동기 상태로 유지하는 두 가지 기본 방법을 제공합니다. 두 방법 중에 하나를 선택하려면 EF Core 모델 또는 데이터베이스 스키마 중 어느 것이 올바른 원본인지 판단합니다.

EF Core 모델을 원본으로 사용하려면 [마이그레이션][1]을 사용합니다. 이 방법에서는 EF Core 모델을 변경하면 해당 스키마 변경 내용을 데이터베이스에 증분 적용하므로 EF Core 모델과의 호환성을 유지합니다. 

데이터베이스 스키마를 원본으로 하려면 [리버스 엔지니어링][2]을 사용합니다. 이 방법에서는 데이터베이스 스키마를 EF Core 모델로 리버스 엔지니어링하여 DbContext와 엔터티 형식 클래스를 스캐폴드할 수 있습니다.

> [!NOTE]
> [API 만들기 및 삭제][3]를 통해서도 EF Core 모델로부터 데이터베이스 스키마를 만들 수 있습니다. 그러나 이 작업은 주로 테스트, 프로토타입 및 기타 데이터베이스 삭제가 허용되는 시나리오를 위한 것입니다.


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
