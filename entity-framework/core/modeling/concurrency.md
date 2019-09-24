---
title: 동시성 토큰-EF Core
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: db768c1de99000be91d33764ccd3c3924237f8bb
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197458"
---
# <a name="concurrency-tokens"></a>동시성 토큰

> [!NOTE]
> 이 페이지에서는 동시성 토큰을 구성 하는 방법을 설명 합니다. 응용 프로그램에서 동시성 충돌을 처리 하는 방법에 대 한 예제와 동시성 제어의 EF Core 작동 방식에 대 한 자세한 설명은 [동시성 충돌 처리](../saving/concurrency.md) 를 참조 하세요.

동시성 토큰으로 구성 된 속성은 낙관적 동시성 제어를 구현 하는 데 사용 됩니다.

## <a name="conventions"></a>규칙

규칙에 따라 속성은 동시성 토큰으로 구성 되지 않습니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 속성을 동시성 토큰으로 구성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Fluent API

흐름 API를 사용 하 여 속성을 동시성 토큰으로 구성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>타임 스탬프/행 버전

타임 스탬프는 행이 삽입 되거나 업데이트 될 때마다 데이터베이스에서 새 값이 생성 되는 속성입니다. 속성도 동시성 토큰으로 처리 됩니다. 이렇게 하면 다른 사용자가 데이터에 대해 쿼리 한 이후에 업데이트 하려고 하는 행을 수정한 경우 예외가 발생 합니다.

이 작업을 수행 하는 방법은 사용 되는 데이터베이스 공급자에 따라 결정 됩니다. SQL Server에 대 한 타임 스탬프는 일반적으로 *byte []* 속성에 사용 됩니다 .이 속성은 데이터베이스에서 *ROWVERSION* 열로 설정 됩니다.

### <a name="conventions"></a>규칙

규칙에 따라 속성은 타임 스탬프로 구성 되지 않습니다.

### <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 속성을 타임 스탬프로 구성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Fluent API

흐름 API를 사용 하 여 속성을 타임 스탬프로 구성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Timestamp.cs#ConfigureTimestampFluent)]
