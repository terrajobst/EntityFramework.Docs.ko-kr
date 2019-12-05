---
title: 테이블 분할-EF Core
description: Entity Framework Core를 사용 하 여 테이블 분할을 구성 하는 방법
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
uid: core/modeling/table-splitting
ms.openlocfilehash: 0e48c516de43cdc2b54c56f1a96f5e01f9fbbbc4
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824563"
---
# <a name="table-splitting"></a>테이블 분할

>[!NOTE]
> 이 기능은 EF Core 2.0의 새로운 기능입니다.

EF Core를 사용 하 여 둘 이상의 엔터티를 단일 행에 매핑할 수 있습니다. 이를 _테이블 분할_ 또는 _테이블 공유_라고 합니다.

## <a name="configuration"></a>구성

테이블 분할을 사용 하려면 엔터티 형식을 동일한 테이블에 매핑해야 하며, 기본 키가 동일한 열에 매핑되고, 한 엔터티 형식의 기본 키와 동일한 테이블에 있는 하나 이상의 관계가 구성 되어 있어야 합니다.

테이블 분할에 대 한 일반적인 시나리오는 성능 또는 캡슐화를 향상 하기 위해 테이블의 열 하위 집합만 사용 하는 것입니다.

이 예제 `Order`에서는 `DetailedOrder`의 하위 집합을 나타냅니다.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

필요한 구성 외에도 `Property(o => o.Status).HasColumnName("Status")`을 호출 하 여 `DetailedOrder.Status`를 `Order.Status`와 동일한 열에 매핑합니다.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> 자세한 컨텍스트는 [전체 샘플 프로젝트](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) 를 참조 하세요.

## <a name="usage"></a>용도

테이블 분할을 사용 하 여 엔터티를 저장 하 고 쿼리 하는 작업은 다른 엔터티와 동일한 방식으로 수행 됩니다. EF Core 3.0부터 종속 엔터티 참조를 `null`수 있습니다. 종속 엔터티에서 사용 하는 모든 열이 데이터베이스 `NULL` 이면 쿼리할 때 생성 되는 인스턴스가 없습니다. 이는 또한 모든 속성이 선택 사항이 며 `null`로 설정 되어 예상 되지 않을 수 있습니다.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>동시성 토큰

테이블을 공유 하는 엔터티 형식에 동시성 토큰이 있는 경우 동일한 테이블에 매핑된 엔터티 중 하나만 업데이트 되 면 부실 동시성 토큰 값을 방지 하기 위해 다른 모든 엔터티 형식에 포함 되어야 합니다.

소비 하는 코드에 노출 되지 않도록 하려면 섀도 상태에서 하나를 만들 수 있습니다.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
