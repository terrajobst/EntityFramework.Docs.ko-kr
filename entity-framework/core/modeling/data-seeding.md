---
title: 데이터 시드-EF Core
author: AndriySvyryd
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 48ba2389de4b57dbe4c2b2124911c71440d45556
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994481"
---
# <a name="data-seeding"></a>데이터 시드

> [!NOTE]  
> 이 기능은 EF Core 2.1의 새로운 기능입니다.

데이터 시드는 데이터베이스를 채우는 초기 데이터를 제공할 수 있습니다. 와 달리 ef6에서 EF Core에서 데이터를 시드 연관 된 모델 구성의 일부로 엔터티 형식입니다. 그런 다음 EF Core [마이그레이션을](xref:core/managing-schemas/migrations/index) 삽입, 업데이트 또는 삭제할 모델의 새 버전으로 데이터베이스를 업그레이드 하는 경우 적용 해야 하는 작업 항목에 자동으로 계산할 수 있습니다.

예를 들어 사용할 수 있습니다에 대 한 시드 데이터를 구성 하는 `Blog` 에서 `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

외래 키 값 관계가 있는 엔터티를 추가 하려면를 지정 해야 합니다. 자주 외래 키 속성을 되므로 섀도 상태의 값을 설정 하는 익명 클래스 수를 사용 해야 합니다.

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

사용 하도록 권장 되는 엔터티를 추가한 후 [마이그레이션을](xref:core/managing-schemas/migrations/index) 변경 내용을 적용 합니다. 

사용할 수 있습니다 `context.Database.EnsureCreated()` 메모리 내 공급자를 사용 하는 경우 또는 예를 들어 테스트 데이터베이스에 대 한 시드 데이터를 포함 하는 새 데이터베이스를 만들려고 합니다. 해당 경우 데이터베이스가 이미 `EnsureCreated()` 스키마 또는 데이터베이스에 시드 데이터 업데이트 하지 것입니다.
