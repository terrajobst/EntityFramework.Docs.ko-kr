---
title: 데이터 시드는-EF 코어
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 7028e1923152b27f56721dab75aae8b9c2f5ad75
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163202"
---
# <a name="data-seeding"></a>데이터 시드

> [!NOTE]  
> 이 기능은 EF 코어 2.1의 새로운 기능입니다.

데이터 시드는 데이터베이스를 채우는 초기 데이터를 제공할 수 있습니다. 와 달리 EF6 EF 코어에서의 데이터 시드 연관 된 모델 구성의 일부로 엔터티 형식입니다. 그런 다음 EF 코어 [마이그레이션](xref:core/managing-schemas/migrations/index) 삽입, 업데이트 또는 삭제 작업을 데이터베이스 모델의 새 버전으로 업그레이드 하는 경우 적용할 필요가 무엇을 자동으로 계산할 수 있습니다.

예를 들어, 사용할 수 있습니다에 대 한 데이터를 구성 하는 `Blog` 에 `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

외래 키 값 관계가 있는 엔터티를 추가 하려면 지정 해야 합니다. 자주 외래 키 속성은 그림자 상태 이므로 익명 클래스 값을 설정 하려면 사용할 수 해야 합니다.

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

사용할 것이 좋습니다 엔터티를 추가 [마이그레이션](xref:core/managing-schemas/migrations/index) 변경 내용을 적용 합니다. 

사용할 수 있습니다 `context.Database.EnsureCreated()` 메모리 내 공급자를 사용 하는 경우 또는 테스트 데이터베이스에 대 한 예를 들어 시드 데이터가 포함 된 새 데이터베이스를 만들 수 있습니다. 되는 경우 데이터베이스가 이미 `EnsureCreated()` 스키마 나 데이터베이스에서 시드 데이터 업데이트 모두 됩니다.
