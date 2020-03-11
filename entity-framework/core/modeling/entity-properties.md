---
title: 엔터티 속성-EF Core
description: Entity Framework Core를 사용 하 여 엔터티 속성을 구성 하 고 매핑하는 방법
author: roji
ms.date: 12/10/2019
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/entity-properties
ms.openlocfilehash: b67603fbffd1f1c8506bc21f8972c851eb8eef29
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414568"
---
# <a name="entity-properties"></a>엔터티 속성

모델의 각 엔터티 형식에는 데이터베이스에서 읽고 쓸 EF Core는 속성 집합이 있습니다. 관계형 데이터베이스를 사용 하는 경우 엔터티 속성은 테이블 열에 매핑됩니다.

## <a name="included-and-excluded-properties"></a>포함 된 속성 및 제외 된 속성

규칙에 따라 getter 및 setter가 있는 모든 public 속성이 모델에 포함 됩니다.

특정 속성은 다음과 같이 제외할 수 있습니다.

### <a name="data-annotations"></a>[데이터 주석](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?name=IgnoreProperty&highlight=6)]

### <a name="fluent-api"></a>[흐름 API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?name=IgnoreProperty&highlight=3,4)]

***

## <a name="column-names"></a>열 이름

규칙에 따라 관계형 데이터베이스를 사용 하는 경우 엔터티 속성은 속성과 이름이 같은 테이블 열에 매핑됩니다.

다른 이름을 사용 하 여 열을 구성 하는 것을 선호 하는 경우 다음과 같이 할 수 있습니다.

### <a name="data-annotations"></a>[데이터 주석](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnName.cs?Name=ColumnName&highlight=3)]

### <a name="fluent-api"></a>[흐름 API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnName.cs?Name=ColumnName&highlight=3-5)]

***

## <a name="column-data-types"></a>열 데이터 형식

관계형 데이터베이스를 사용 하는 경우 데이터베이스 공급자는 속성의 .NET 형식을 기반으로 데이터 형식을 선택 합니다. 구성 된 [최대 길이](#maximum-length)와 같은 다른 메타 데이터 (속성이 기본 키의 일부 인지 여부 등)도 고려 합니다.

예를 들어 SQL Server `DateTime` 속성을 `datetime2(7)` `string` 열에 매핑하고 속성을 `nvarchar(max)` 열 (또는 키로 사용 되는 속성의 경우 `nvarchar(450)`)에 매핑합니다.

열에 정확한 데이터 형식을 지정 하도록 열을 구성할 수도 있습니다. 예를 들어 다음 코드는 최대 길이가 `200` 인 비유니코드 문자열로 `Url`를 구성 하 고 `2``5` 및 소수 자릿수를 사용 하 여 `Rating` 합니다.

### <a name="data-annotations"></a>[데이터 주석](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnDataType.cs?name=ColumnDataType&highlight=4,6)]

### <a name="fluent-api"></a>[흐름 API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnDataType.cs?name=ColumnDataType&highlight=5-6)]

***

### <a name="maximum-length"></a>최대 길이

최대 길이를 구성 하면 지정 된 속성에 대해 선택할 적절 한 열 데이터 형식에 대 한 힌트를 데이터베이스 공급자에 게 제공 합니다. 최대 길이는 `string` 및 `byte[]`와 같은 배열 데이터 형식에만 적용 됩니다.

> [!NOTE]
> Entity Framework는 공급자에 게 데이터를 전달 하기 전에 최대 길이에 대 한 유효성 검사를 수행 하지 않습니다. 해당 하는 경우 공급자 또는 데이터 저장소에서 유효성을 검사 합니다. 예를 들어 SQL Server를 대상으로 지정 하는 경우 최대 길이를 초과 하면 기본 열의 데이터 형식에 따라 초과 되는 데이터가 저장 되는 것을 허용 하지 않기 때문에 예외가 발생 합니다.

다음 예제에서는 최대 길이 500를 구성 하면 SQL Server에서 `nvarchar(500)` 형식의 열이 생성 됩니다.

#### <a name="data-annotations"></a>[데이터 주석](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?name=MaxLength&highlight=4)]

#### <a name="fluent-api"></a>[흐름 API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?name=MaxLength&highlight=3-5)]

***

## <a name="required-and-optional-properties"></a>필수 및 선택적 속성

속성은 `null`를 포함 하는 데 유효 하면 선택 사항으로 간주 됩니다. `null` 속성에 할당 되는 유효한 값이 아닌 경우 필수 속성으로 간주 됩니다. 관계형 데이터베이스 스키마에 매핑할 때 필수 속성은 null을 허용 하지 않는 열로 생성 되 고 선택적 속성은 null을 허용 하는 열로 생성 됩니다.

### <a name="conventions"></a>표기 규칙

규칙에 따라 .NET 형식이 null을 포함할 수 있는 속성은 선택 사항으로 구성 되 고, .NET 형식은 null을 포함할 수 없는 속성은 필수로 구성 됩니다. 예를 들어 .NET 값 형식 (`int`, `decimal`, `bool`등)을 포함 하는 모든 속성은 필수로 구성 되며 nullable .NET 값 형식 (`int?`, `decimal?`, `bool?`등)을 포함 하는 모든 속성은 선택 사항으로 구성 됩니다.

C#8에는 null이 포함 될 수 있는지 여부를 나타내는 [nullable 참조 형식](/dotnet/csharp/tutorials/nullable-reference-types)이라는 새로운 기능이 도입 되었습니다. 이 기능은 기본적으로 사용 하지 않도록 설정 되어 있으며, 사용 하도록 설정 된 경우 다음과 같은 방식으로 EF Core의 동작을 수정 합니다.

* Nullable 참조 형식이 사용 하지 않도록 설정 된 경우 (기본값) .NET 참조 형식이 포함 된 모든 속성은 규칙에 따라 선택적으로 구성 됩니다 (예: `string`).
* Nullable 참조 형식이 사용 되는 경우 속성은 .NET 형식의 C# null 허용 여부에 따라 구성 됩니다. `string?`은 선택적으로 구성 되지만 `string`는 필수로 구성 됩니다.

다음 예제에서는 nullable 참조 기능을 사용 하지 않도록 설정 하 고 (기본값) 사용 하도록 설정 된 필수 및 선택적 속성이 포함 된 엔터티 형식을 보여 줍니다.

#### <a name="without-nullable-reference-types-default"></a>[Null을 허용 하지 않는 참조 형식 (기본값)](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

#### <a name="with-nullable-reference-types"></a>[Nullable 참조 형식 사용](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

코드에 C# 표시 되는 null 허용 여부를 EF Core의 모델 및 데이터베이스로 전달 하 고 흐름 API 또는 데이터 주석을 사용 하 여 동일한 개념을 두 번 표현 하므로 nullable 참조 형식을 사용 하는 것이 좋습니다.

> [!NOTE]
> 기존 프로젝트에서 nullable 참조 형식을 사용 하도록 설정 하는 경우 주의 해야 합니다. 이전에 선택적으로 구성 된 참조 형식 속성은 nullable로 명시적으로 주석 처리 되지 않는 한 이제 필수로 구성 됩니다. 관계형 데이터베이스 스키마를 관리할 때이로 인해 데이터베이스 열의 null 허용 여부를 변경 하는 마이그레이션이 생성 될 수 있습니다.

Nullable 참조 형식 및 EF Core와 함께 사용 하는 방법에 대 한 자세한 내용은 [이 기능에 대 한 전용 설명서 페이지를 참조](xref:core/miscellaneous/nullable-reference-types)하세요.

### <a name="explicit-configuration"></a>명시적 구성

규칙에 따라 선택적인 속성은 다음과 같이 필수로 구성할 수 있습니다.

#### <a name="data-annotations"></a>[데이터 주석](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?name=Required&highlight=4)]

#### <a name="fluent-api"></a>[흐름 API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?name=Required&highlight=3-5)]

***
