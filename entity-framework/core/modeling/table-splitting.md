---
title: 테이블 분할-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 4a0bfaf017106a0bfdff084b1c472bdc17459a89
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562593"
---
# <a name="table-splitting"></a>테이블 분

>[!NOTE]
> 이 기능은 EF Core 2.0의 새로운 기능입니다.

EF Core는 단일 행에 두 개 이상의 엔터티를 매핑할 수 있습니다. 이 이라고 _분할 테이블_ 하거나 _테이블 공유_합니다.

## <a name="configuration"></a>구성

테이블 엔터티 유형을 동일한 테이블에 매핑할 수 해야 분할을 사용 하려면 동일한 열에 매핑된 기본 키 및 하나 이상의 관계를 엔터티 형식의 기본 키와 동일한 테이블의 다른 사이 구성 된 경우

테이블 분할에 대 한 일반적인 시나리오에서에서 사용 하는 열의 하위 집합만 테이블 성능 크거나 캡슐화 합니다.

이 예제의 `Order` 의 하위 집합을 나타내는 `DetailedOrder`합니다.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

필수 구성 외에도 호출 `HasBaseType((string)null)` 매핑을 사용 하지 않으려면 `DetailedOrder` 와 동일한 계층에 `Order`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

참조 된 [전체 샘플 프로젝트](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) 자세한 컨텍스트.

## <a name="usage"></a>사용

저장 하 고 테이블 분할을 사용 하 여 엔터티 쿼리 다른 엔터티는 동일한 방식으로 수행 됩니다, 그리고 유일한 차이점은 삽입에 대 한 행을 공유 하는 모든 엔터티를 추적할 수 있어야 합니다.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>동시성 토큰

테이블 공유 엔터티 형식의 동시성 토큰 있으면 다음이 포함 되어야 합니다는 같은 테이블에 매핑된 엔터티 중 하나에이 업데이트 될 때 오래 된 동시성 토큰 값을 방지 하려면 다른 모든 엔터티 형식에.

사용 하는 코드에 노출 하지 않는 것이 가능한 섀도 상태의 하나 만드세요.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]