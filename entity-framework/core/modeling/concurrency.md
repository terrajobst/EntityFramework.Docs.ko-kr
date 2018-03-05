---
title: "동시성 토큰-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: f3cf28d5c54e63aa76058e9fe1d9f3de5b37d579
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/05/2018
---
# <a name="concurrency-tokens"></a>동시성 토큰

> [!NOTE]
> 이 페이지에는 동시성 토큰을 구성 하는 방법을 설명 합니다. 참조 [동시성 충돌 처리](../saving/concurrency.md) 자세한 설명은 EF 코어 및 응용 프로그램에서 동시성 충돌을 처리 하는 방법의 예제에서 동시성 제어의 작동 방식에 대 한 합니다.

동시성 토큰으로 구성 된 속성은 낙관적 동시성 제어를 구현 하는 데 사용 됩니다.

## <a name="conventions"></a>규칙

규칙에 따라 속성이 동시성 토큰으로 구성 되지 됩니다.

## <a name="data-annotations"></a>데이터 주석

동시성 토큰으로 속성을 구성 하는 데이터 주석을 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Fluent API

동시성 토큰으로 속성을 구성 하는 Fluent API를 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>타임 스탬프/행 버전

타임 스탬프 행이 삽입 되거나 업데이트 될 때마다 데이터베이스에서 새 값을 생성 되는 속성입니다. 속성이는 동시성 토큰으로도 처리 됩니다. 이렇게 하면 다른 사용자가 데이터에 대 한 쿼리 한 이후 업데이트 하려고 하는 행을 수정 하는 경우 예외가 발생 합니다.

사용 중인 데이터베이스 공급자가 어떻게 작업을 수행 합니다. SQL Server에 대 한 타임 스탬프는 대개 사용에 *byte* 수 있는 속성으로 설치는 *ROWVERSION* 데이터베이스의 열입니다.

### <a name="conventions"></a>규칙

규칙에 따라 타임 스탬프로 속성이 구성 되지 됩니다.

### <a name="data-annotations"></a>데이터 주석

속성을 구성을 타임 스탬프 데이터 주석을 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Fluent API

속성을 구성을 타임 스탬프 Fluent API를 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
