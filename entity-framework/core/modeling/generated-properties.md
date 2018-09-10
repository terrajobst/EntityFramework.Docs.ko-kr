---
title: 생성 된 값-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
uid: core/modeling/generated-properties
ms.openlocfilehash: 9ecfa924a0614f327f0bd202cb7dda95bea810af
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250701"
---
# <a name="generated-values"></a>생성 된 값

## <a name="value-generation-patterns"></a>값 생성 패턴

속성에 대해 사용할 수 있는 세 가지 값 생성 패턴 가지가 있습니다.
* 값 생성 안 함
* 생성 된 값 추가
* 생성 된 값에 추가 또는 업데이트

### <a name="no-value-generation"></a>값 생성 안 함

값을 생성 하지 않을 입력 해야 하는 항상 데이터베이스에 저장할 수 있는 유효한 값을 의미 합니다. 새 엔터티를 컨텍스트에 추가 되기 전에이 유효한 값을 할당 되어야 합니다.

### <a name="value-generated-on-add"></a>생성 된 값 추가

생성 되는 값 의미를 추가 하는 새 엔터티에 대 한 값이 생성 됩니다.

사용 중인 데이터베이스 공급자에 따라 값이 되었을 EF 또는 데이터베이스에서 클라이언트 쪽을 생성 합니다. 다음 값은 데이터베이스에서 생성, 컨텍스트에 엔터티를 추가 하면 EF는 임시 값을 할당할 수 있습니다. 이 임시 값이 하는 동안 데이터베이스에서 생성 된 값으로 바뀝니다 다음 `SaveChanges()`합니다.

속성에 할당 된 값이 있는 컨텍스트에 엔터티를 추가 하는 경우 EF는 새로 생성 하는 것이 아니라 해당 값을 삽입할 시도 합니다. 속성이 CLR 기본값 할당 되지 않은 경우 할당 된 값으로 간주 됩니다 (`null` 에 대 한 `string`를 `0` 에 대 한 `int`합니다 `Guid.Empty` 에 대 한 `Guid`등.). 자세한 내용은 [생성 된 속성에 대 한 명시적 값](../saving/explicit-values-generated-properties.md)합니다.

> [!WARNING]  
> 추가 된 엔터티에 대 한 값은 생성 하는 방법을 사용 중인 데이터베이스 공급자에 따라 달라 집니다. 데이터베이스 공급자 일부 속성 유형의 경우 값 생성을 자동으로 설정할 수 있지만 값이 생성 되는 방식을 수동으로 설정 해야 할 수 있습니다 다른 합니다.
>
> 예를 들어, SQL Server를 사용 하는 경우 값은 자동으로 생성할 `GUID` 속성 (SQL Server 순차적 GUID 알고리즘 사용). 그러나 지정 하는 경우는 `DateTime` 속성이 생성 됩니다에 추가한 다음, 값을 생성할 수 있는 방법을 설정 해야 합니다. 이 작업을 수행 하는 한 가지 방법은 기본 값을 구성 하는 것 `GETDATE()`를 참조 하세요 [기본값](relational/default-values.md)합니다.

### <a name="value-generated-on-add-or-update"></a>생성 된 값에 추가 또는 업데이트

생성 된 값에 추가 또는 업데이트 (삽입 또는 업데이트) 레코드를 저장할 때 새 값을 생성 되도록을 의미 합니다.

같은 `value generated on add`새로 추가 된 인스턴스에서 생성 되는 값이 아닌 값은 삽입 될 엔터티의 속성 값을 지정 하는 경우. 업데이트 하는 경우 명시적 값을 설정할 수 이기도 합니다. 자세한 내용은 [생성 된 속성에 대 한 명시적 값](../saving/explicit-values-generated-properties.md)합니다.

> [!WARNING]
> 추가 되 고 업데이트 된 엔터티에 대 한 값은 생성 하는 방법을 사용 중인 데이터베이스 공급자에 따라 달라 집니다. 자동으로 데이터베이스 공급자는 다른 해야 수동으로 값을 생성 되는 방식을 설정 하는 동안 일부 속성 유형의 경우 값 생성을 설정할 수 있습니다.
> 
> 예를 들어, SQL Server 사용 시 `byte[]` 에서 생성 하는 대로 설정 된 속성을 추가 또는 업데이트 및 설치 된 동시성 토큰으로 표시 됩니다는 `rowversion` 데이터 형식-값 데이터베이스에서 생성 될 수 있도록 합니다. 그러나 지정 하는 경우는 `DateTime` 속성이 생성 됩니다에 추가 또는 업데이트를 다음 값을 생성할 수 있는 방법을 설정 해야 합니다. 이 작업을 수행 하는 한 가지 방법은 기본 값을 구성 하는 것 `GETDATE()` (참조 [기본값](relational/default-values.md)) 새 행에 대 한 값을 생성 합니다. 그런 다음 (예: 다음 예제에서는 트리거) 업데이트 하는 동안 값을 생성 하려면 데이터베이스 트리거를 사용할 수 있습니다.
> 
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>규칙

규칙에 따라 비 복합 기본 키 형식이 short, int, long, 또는 Guid 값에서 생성 된 추가 설치 됩니다. 다른 모든 속성 값을 생성 하지 않을 사용 하 여 설정 됩니다.

## <a name="data-annotations"></a>데이터 주석

### <a name="no-value-generation-data-annotations"></a>(데이터 주석)를 생성 하지 않음 값

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>생성 된 값 (데이터 주석)를 추가 합니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> 이렇게 하면 EF를 있음을 아는 추가 된 엔터티에 대 한 값이 생성 됩니다 EF에서는 값을 생성 하는 실제 메커니즘을 설정 하는 보장 하지 않습니다만 있습니다. 참조 [에 생성 된 값 추가](#value-generated-on-add) 대 한 자세한 내용은 섹션입니다.

### <a name="value-generated-on-add-or-update-data-annotations"></a>생성 된 값 추가 또는 업데이트 (데이터 주석)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> 이렇게 하면 EF를 있음을 아는 추가 되거나 업데이트 된 엔터티에 대 한 값이 생성 됩니다 EF에서는 값을 생성 하는 실제 메커니즘을 설정 하는 보장 하지 않습니다만 있습니다. 참조 [에 생성 된 값을 추가 하거나 업데이트할](#value-generated-on-add-or-update) 대 한 자세한 내용은 섹션입니다.

## <a name="fluent-api"></a>Fluent API

지정된 된 속성에 대 한 값 생성 패턴을 변경 하려면 Fluent API를 사용할 수 있습니다.

### <a name="no-value-generation-fluent-api"></a>(Fluent API)를 생성 하지 않음 값

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>생성 된 값 (Fluent API) 추가

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` 방금 추가 된 엔터티에 대 한 값이 생성 되는, EF에서는 값을 생성 하는 실제 메커니즘을 설정 하는 보장 하지 않습니다 알고 EF 수 있습니다.  참조 [에 생성 된 값 추가](#value-generated-on-add) 대 한 자세한 내용은 섹션입니다.

### <a name="value-generated-on-add-or-update-fluent-api"></a>생성 된 값 추가 또는 업데이트 (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> 이렇게 하면 EF를 있음을 아는 추가 되거나 업데이트 된 엔터티에 대 한 값이 생성 됩니다 EF에서는 값을 생성 하는 실제 메커니즘을 설정 하는 보장 하지 않습니다만 있습니다. 참조 [에 생성 된 값을 추가 하거나 업데이트할](#value-generated-on-add-or-update) 대 한 자세한 내용은 섹션입니다.
