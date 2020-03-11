---
title: 동시성 토큰-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: bfeb611f222f7195fe22d920b452b40cc4addf90
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414622"
---
# <a name="concurrency-tokens"></a>동시성 토큰

> [!NOTE]
> 이 페이지에서는 동시성 토큰을 구성 하는 방법을 설명 합니다. 응용 프로그램에서 동시성 충돌을 처리 하는 방법에 대 한 예제와 동시성 제어의 EF Core 작동 방식에 대 한 자세한 설명은 [동시성 충돌 처리](../saving/concurrency.md) 를 참조 하세요.

동시성 토큰으로 구성 된 속성은 낙관적 동시성 제어를 구현 하는 데 사용 됩니다.

## <a name="configuration"></a>구성

### <a name="data-annotations"></a>[데이터 주석](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs?name=Concurrency&highlight=5)]

### <a name="fluent-api"></a>[흐름 API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs?name=Concurrency&highlight=5)]

***

## <a name="timestamprowversion"></a>Timestamp/rowversion

Timestamp/rowversion는 행이 삽입 되거나 업데이트 될 때마다 데이터베이스에서 새 값이 자동으로 생성 되는 속성입니다. 또한 속성은 동시성 토큰으로 처리 되므로 업데이트 하는 행이 쿼리 이후 변경 된 경우 예외를 받게 됩니다. 정확한 세부 정보는 사용 되는 데이터베이스 공급자에 따라 달라 집니다. SQL Server의 경우 일반적으로 *byte []* 속성이 사용 됩니다 .이 속성은 데이터베이스에서 *ROWVERSION* 열로 설정 됩니다.

다음과 같이 속성을 timestamp/rowversion로 구성할 수 있습니다.

### <a name="data-annotations"></a>[데이터 주석](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs?name=Timestamp&highlight=7)]

### <a name="fluent-api"></a>[흐름 API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Timestamp.cs?name=Timestamp&highlight=9,17)]

***
