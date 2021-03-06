---
title: 생성 된 값-EF Core
description: Entity Framework Core를 사용 하는 경우 속성에 대 한 값 생성을 구성 하는 방법
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/generated-properties
ms.openlocfilehash: 9c616e157ff1bdb9700f436a7ae2788330fe5d45
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413902"
---
# <a name="generated-values"></a>생성된 값

## <a name="value-generation-patterns"></a>값 생성 패턴

속성에 사용할 수 있는 값 생성 패턴에는 다음 세 가지가 있습니다.

* 값 생성 안 함
* 추가 시 생성 되는 값
* 추가 또는 업데이트 시 생성 되는 값

### <a name="no-value-generation"></a>값 생성 안 함

값을 생성 하지 않으면 항상 데이터베이스에 저장할 올바른 값을 제공 하는 것입니다. 이 유효한 값은 컨텍스트에 추가 되기 전에 새 엔터티에 할당 되어야 합니다.

### <a name="value-generated-on-add"></a>추가 시 생성 되는 값

추가 시 생성 된 값은 새 엔터티에 대 한 값이 생성 됨을 의미 합니다.

사용 되는 데이터베이스 공급자에 따라 값이 EF 또는 데이터베이스에서 클라이언트 쪽에서 생성 될 수 있습니다. 데이터베이스에서 값을 생성 하는 경우 컨텍스트에 엔터티를 추가할 때 EF에서 임시 값을 할당할 수 있습니다. 이 임시 값은 `SaveChanges()`중에 생성 된 데이터베이스 값으로 대체 됩니다.

속성에 할당 된 값이 있는 컨텍스트에 엔터티를 추가 하는 경우 EF는 새 값을 생성 하는 대신 해당 값을 삽입 하려고 시도 합니다. 속성에는 CLR 기본값`null` (`string`, `int``0`, `Guid.Empty`에 대 한 `Guid`등)이 할당 되지 않은 경우 해당 값이 할당 된 것으로 간주 됩니다. 자세한 내용은 [생성 된 속성에 대 한 명시적 값](../saving/explicit-values-generated-properties.md)을 참조 하세요.

> [!WARNING]
> 추가 된 엔터티에 대해 값이 생성 되는 방법은 사용 되는 데이터베이스 공급자에 따라 달라 집니다. 데이터베이스 공급자는 일부 속성 형식에 대해 값 생성을 자동으로 설정할 수 있지만 다른 속성은 값이 생성 되는 방법을 수동으로 설정 해야 할 수도 있습니다.
>
> 예를 들어 SQL Server를 사용 하는 경우 SQL Server 순차 GUID 알고리즘을 사용 하 여 `GUID` 속성에 대 한 값이 자동으로 생성 됩니다. 그러나 추가 시 `DateTime` 속성이 생성 되도록 지정 하는 경우 값을 생성 하는 방법을 설정 해야 합니다. 이 작업을 수행 하는 한 가지 방법은 기본값 `GETDATE()`를 구성 하는 것입니다. [기본값](relational/default-values.md)을 참조 하세요.

### <a name="value-generated-on-add-or-update"></a>추가 또는 업데이트 시 생성 되는 값

추가 또는 업데이트 시 생성 된 값은 레코드가 저장 될 때마다 (삽입 또는 업데이트) 새 값이 생성 됨을 의미 합니다.

`value generated on add`와 같이 새로 추가 된 엔터티 인스턴스의 속성 값을 지정 하면 생성 되는 값 대신 해당 값이 삽입 됩니다. 업데이트 시 명시적 값을 설정할 수도 있습니다. 자세한 내용은 [생성 된 속성에 대 한 명시적 값](../saving/explicit-values-generated-properties.md)을 참조 하세요.

> [!WARNING]
> 추가 및 업데이트 된 엔터티에 대해 값이 생성 되는 방법은 사용 되는 데이터베이스 공급자에 따라 달라 집니다. 데이터베이스 공급자는 일부 속성 유형에 대해 값 생성을 자동으로 설정할 수 있으며, 다른 속성은 값이 생성 되는 방법을 수동으로 설정 해야 합니다.
>
> 예를 들어 SQL Server를 사용 하는 경우 추가 또는 업데이트에서 생성 되 고 동시성 토큰으로 표시 되는 `byte[]` 속성은 `rowversion` 데이터 형식으로 설정 되므로 데이터베이스에서 값이 생성 됩니다. 그러나 `DateTime` 속성이 추가 또는 업데이트에서 생성 되도록 지정 하는 경우 값을 생성 하는 방법을 설정 해야 합니다. 이 작업을 수행 하는 한 가지 방법은 `GETDATE()` ( [기본값](relational/default-values.md)참조)의 기본값을 구성 하 여 새 행에 대 한 값을 생성 하는 것입니다. 그런 다음 데이터베이스 트리거를 사용 하 여 업데이트 중 값을 생성할 수 있습니다 (예: 다음 예제 트리거).
>
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="value-generated-on-add"></a>추가 시 생성 되는 값

규칙에 따라 short, int, long 또는 Guid 형식의 비 복합 기본 키는 응용 프로그램에서 값을 제공 하지 않는 경우 삽입 된 엔터티에 대해 생성 된 값을 갖도록 설정 됩니다. 데이터베이스 공급자는 일반적으로 필요한 구성을 처리 합니다. 예를 들어 SQL Server의 숫자 기본 키는 자동으로 ID 열로 설정 됩니다.

다음과 같이 삽입 된 엔터티에 대해 값이 생성 되도록 속성을 구성할 수 있습니다.

### <a name="data-annotations"></a>[데이터 주석](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAdd.cs?name=ValueGeneratedOnAdd&highlight=5)]

### <a name="fluent-api"></a>[흐름 API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAdd.cs?name=ValueGeneratedOnAdd&highlight=5)]

***

> [!WARNING]
> 이렇게 하면 EF가 추가 된 엔터티에 대해 생성 된 값을 알 수 있으므로 EF가 실제 메커니즘을 설정 하 여 값을 생성 하는 것을 보장 하지는 않습니다. 자세한 내용은 [추가 섹션에서 생성 된 값](#value-generated-on-add) 을 참조 하세요.

### <a name="default-values"></a>기본값

관계형 데이터베이스에서는 기본값을 사용 하 여 열을 구성할 수 있습니다. 해당 열에 대 한 값 없이 행을 삽입 하면 기본값이 사용 됩니다.

속성에서 기본값을 구성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultValue.cs?name=DefaultValue&highlight=5)]

또한 기본값을 계산 하는 데 사용 되는 SQL 조각을 지정할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultValueSql.cs?name=DefaultValueSql&highlight=5)]

기본값을 지정 하면 추가 시 속성을 생성 된 값으로 암시적으로 구성 합니다.

## <a name="value-generated-on-add-or-update"></a>추가 또는 업데이트 시 생성 되는 값

### <a name="data-annotations"></a>[데이터 주석](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAddOrUpdate.cs?name=ValueGeneratedOnAddOrUpdate&highlight=5)]

### <a name="fluent-api"></a>[흐름 API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs?name=ValueGeneratedOnAddOrUpdate&highlight=5)]

***

> [!WARNING]
> 이렇게 하면 EF가 추가 되거나 업데이트 된 엔터티에 대해 생성 된 값을 알 수 있으므로 EF는 값을 생성 하는 실제 메커니즘을 설정 하는 것을 보장 하지 않습니다. 자세한 내용은 [추가 또는 업데이트 섹션에서 생성 된 값](#value-generated-on-add-or-update) 을 참조 하세요.

### <a name="computed-columns"></a>계산 열

일부 관계형 데이터베이스에서 열은 일반적으로 다른 열을 참조 하는 식을 사용 하 여 데이터베이스에서 값을 계산 하도록 구성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ComputedColumn.cs?name=ComputedColumn&highlight=5)]

> [!NOTE]
> 경우에 따라 열 값이 인출 될 때마다 ( *가상* 열이 라고도 함) 계산 되 고, 다른 경우에는 행 및 저장 된 모든 업데이트에 대해 계산 됩니다 ( *저장 된* 열 또는 *지속형* 열이 라고도 함). 이는 데이터베이스 공급자 마다 다릅니다.

## <a name="no-value-generation"></a>값 생성 안 함

속성에 대해 값 생성을 비활성화 하는 것은 일반적으로 규칙에서 값 생성을 위해 구성 하는 경우에 필요 합니다. 예를 들어 int 형식의 기본 키가 있는 경우에는 add에 생성 된 값으로 암시적으로 설정 됩니다. 다음을 통해이를 사용 하지 않도록 설정할 수 있습니다.

### <a name="data-annotations"></a>[데이터 주석](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedNever.cs?name=ValueGeneratedNever&highlight=3)]

### <a name="fluent-api"></a>[흐름 API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedNever.cs?name=ValueGeneratedNever&highlight=5)]

***
