---
title: EF Core-동시성 토큰
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: 0051d416544a11385f99d36e45843c5b20725af7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994228"
---
# <a name="concurrency-tokens"></a>동시성 토큰

> [!NOTE]
> 이 페이지에는 동시성 토큰을 구성 하는 방법을 설명 합니다. 참조 [동시성 충돌 처리](../saving/concurrency.md) 자세한 설명은 EF Core 및 응용 프로그램에서 동시성 충돌을 처리 하는 방법의 예제에서 동시성 제어가 작동 하는 방법에 대 한 합니다.

속성이 동시성 토큰으로 구성 된 낙관적 동시성 제어를 구현에 사용 됩니다.

## <a name="conventions"></a>규칙

규칙에 따라 속성이 동시성 토큰으로 구성 되지 됩니다.

## <a name="data-annotations"></a>데이터 주석

동시성 토큰으로 속성을 구성 하는 데이터 주석을 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Fluent API

동시성 토큰으로 속성을 구성 하는 Fluent API를 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>타임 스탬프/행 버전

타임 스탬프 속성인 행이 삽입 되거나 업데이트 될 때마다 데이터베이스에서 새 값을 생성 됩니다. 속성이는 동시성 토큰으로도 처리 됩니다. 이렇게 하면 다른 사용자가 하므로 데이터에 대 한 쿼리를 업데이트 하려고 하는 행을 수정 하면 예외를 얻을 수 있습니다.

이렇게 하는 방법을 하는 것은 사용 중인 데이터베이스 공급자 책임입니다. SQL Server에 대 한 타임 스탬프는 일반적으로 사용에 *byte* 될 속성을 설정 된 *ROWVERSION* 데이터베이스의 열.

### <a name="conventions"></a>규칙

규칙에 따라 속성 타임 스탬프로 구성 되지 됩니다.

### <a name="data-annotations"></a>데이터 주석

타임 스탬프 속성을 구성 하려면 데이터 주석을 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Fluent API

타임 스탬프와 속성을 구성 하는 Fluent API를 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
