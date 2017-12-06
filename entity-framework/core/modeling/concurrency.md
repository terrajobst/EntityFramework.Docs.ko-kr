---
title: "동시성 토큰-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a>동시성 토큰

속성이 동시성 토큰으로 구성 된 다른 사용자가 해당 레코드에 변경 내용을 저장할 때 데이터베이스의 해당 값을 수정 EF 확인 됩니다. EF는 낙관적 동시성 패턴을 사용 하 여, 값을 변경 하지 않은 것으로 가정 하 고 데이터를 저장 하지만 값이 변경 발견 되 면 throw 하려고 것을 의미 합니다.

예를 들어 구성 해야 할 수 있습니다 `LastName` 에 `Person` 동시성 토큰 되도록 합니다. 즉, 한 사용자의 일부 변경 내용을 저장 하려고 하는 경우는 `Person`, 다른 사용자가을 변경 하지만 `LastName` 다음 예외가 throw 됩니다. 응용 프로그램이이 레코드의 변경 내용을 저장 하기 전에 동일한 실제 사용자는 여전히 나타냅니다 되도록 사용자를 입력할 수 있도록이 바람직 할 수 있습니다.

> [!NOTE]
> 이 페이지에는 동시성 토큰을 구성 하는 방법을 설명 합니다. 참조 [동시성 처리](../saving/concurrency.md) 응용 프로그램에서 낙관적 동시성을 사용 하는 방법에 대 한 예제입니다.

## <a name="how-concurrency-tokens-work-in-ef"></a>동시성 토큰 EF에서 작동 하는 방법

데이터 저장소 업데이트 또는 삭제할 여전히 모든 레코드에 컨텍스트 데이터베이스에서 원래 데이터를 로드할 때 할당 된 동시성 토큰에 대 한 동일한 값이 있는지 확인 하 여 동시성 토큰을 적용할 수 있습니다.

관계형 데이터베이스의 동시성 토큰을 포함 하 여이으로 달성 되는 예를 들어는 `WHERE` 모든 절 `UPDATE` 또는 `DELETE` 명령 및 영향을 받는 행의 번호를 확인 합니다. 동시성 토큰 여전히 일치 하는 경우 행이 하나씩 업데이트 됩니다. 값은 데이터베이스에 변경 된 경우 아무 행도 업데이트 됩니다.

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

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
