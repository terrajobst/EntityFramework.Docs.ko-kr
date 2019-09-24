---
title: 생성 된 값-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
uid: core/modeling/generated-properties
ms.openlocfilehash: 6b38fd2e540ec29674f1116e7c204052d06ca1bc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197434"
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

사용 되는 데이터베이스 공급자에 따라 값이 EF 또는 데이터베이스에서 클라이언트 쪽에서 생성 될 수 있습니다. 데이터베이스에서 값을 생성 하는 경우 컨텍스트에 엔터티를 추가할 때 EF에서 임시 값을 할당할 수 있습니다. 이 임시 값은에서 생성 된 데이터베이스 값 `SaveChanges()`으로 대체 됩니다.

속성에 할당 된 값이 있는 컨텍스트에 엔터티를 추가 하는 경우 EF는 새 값을 생성 하는 대신 해당 값을 삽입 하려고 시도 합니다. 속성에는 CLR 기본값 (`null` `0` `string` `int` for,`Guid.Empty` for, for, 등)이 할당 되지 않은 경우 할당 된 값이 있는 것으로 간주 됩니다. `Guid` 자세한 내용은 [생성 된 속성에 대 한 명시적 값](../saving/explicit-values-generated-properties.md)을 참조 하세요.

> [!WARNING]  
> 추가 된 엔터티에 대해 값이 생성 되는 방법은 사용 되는 데이터베이스 공급자에 따라 달라 집니다. 데이터베이스 공급자는 일부 속성 형식에 대해 값 생성을 자동으로 설정할 수 있지만 다른 속성은 값이 생성 되는 방법을 수동으로 설정 해야 할 수도 있습니다.
>
> 예를 들어 SQL Server를 사용 하는 경우 속성에 대 한 `GUID` 값이 자동으로 생성 됩니다 (SQL Server 순차적 GUID 알고리즘 사용). 그러나 추가 시 속성을 `DateTime` 생성 하도록 지정 하는 경우 값을 생성 하는 방법을 설정 해야 합니다. 이 작업을 수행 하는 한 가지 방법은 기본값을 구성 `GETDATE()`하는 것입니다. [기본값](relational/default-values.md)을 참조 하세요.

### <a name="value-generated-on-add-or-update"></a>추가 또는 업데이트 시 생성 되는 값

추가 또는 업데이트 시 생성 된 값은 레코드가 저장 될 때마다 (삽입 또는 업데이트) 새 값이 생성 됨을 의미 합니다.

마찬가지로 `value generated on add`, 새로 추가 된 엔터티 인스턴스의 속성 값을 지정 하는 경우 생성 되는 값 대신 해당 값이 삽입 됩니다. 업데이트 시 명시적 값을 설정할 수도 있습니다. 자세한 내용은 [생성 된 속성에 대 한 명시적 값](../saving/explicit-values-generated-properties.md)을 참조 하세요.

> [!WARNING]
> 추가 및 업데이트 된 엔터티에 대해 값이 생성 되는 방법은 사용 되는 데이터베이스 공급자에 따라 달라 집니다. 데이터베이스 공급자는 일부 속성 유형에 대해 값 생성을 자동으로 설정할 수 있으며, 다른 속성은 값이 생성 되는 방법을 수동으로 설정 해야 합니다.
> 
> 예를 들어 SQL Server `byte[]` 를 사용 하는 경우 추가 또는 업데이트에 생성 되 고 동시성 토큰으로 표시 되는 속성은 데이터베이스에서 값이 생성 되도록 `rowversion` 데이터 형식으로 설정 됩니다. 그러나 추가 또는 업데이트 시 `DateTime` 속성이 생성 되도록 지정 하는 경우에는 값을 생성 하는 방법을 설정 해야 합니다. 이 작업을 수행 하는 한 가지 방법은 기본값 `GETDATE()` ( [기본값](relational/default-values.md)참조)을 구성 하 여 새 행에 대 한 값을 생성 하는 것입니다. 그런 다음 데이터베이스 트리거를 사용 하 여 업데이트 중 값을 생성할 수 있습니다 (예: 다음 예제 트리거).
> 
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>규칙

규칙에 따라 short, int, long 또는 Guid 형식의 비 복합 기본 키는 add에 생성 된 값을 갖도록 설정 됩니다. 다른 모든 속성은 값을 생성 하지 않고 설정 됩니다.

## <a name="data-annotations"></a>데이터 주석

### <a name="no-value-generation-data-annotations"></a>값 생성 안 함 (데이터 주석)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Add에 생성 된 값 (데이터 주석)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> 이렇게 하면 EF가 추가 된 엔터티에 대해 생성 된 값을 알 수 있으므로 EF가 실제 메커니즘을 설정 하 여 값을 생성 하는 것을 보장 하지는 않습니다. 자세한 내용은 [추가 섹션에서 생성 된 값](#value-generated-on-add) 을 참조 하세요.

### <a name="value-generated-on-add-or-update-data-annotations"></a>추가 또는 업데이트 시 생성 된 값 (데이터 주석)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> 이렇게 하면 EF가 추가 되거나 업데이트 된 엔터티에 대해 생성 된 값을 알 수 있으므로 EF는 값을 생성 하는 실제 메커니즘을 설정 하는 것을 보장 하지 않습니다. 자세한 내용은 [추가 또는 업데이트 섹션에서 생성 된 값](#value-generated-on-add-or-update) 을 참조 하세요.

## <a name="fluent-api"></a>Fluent API

흐름 API를 사용 하 여 지정 된 속성에 대 한 값 생성 패턴을 변경할 수 있습니다.

### <a name="no-value-generation-fluent-api"></a>값 생성 안 함 (흐름 API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Add에 생성 된 값 (흐름 API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()`EF가 추가 된 엔터티에 대해 생성 된 값을 알 수 있으므로 EF는 값을 생성 하는 실제 메커니즘을 설정 하는 것을 보장 하지 않습니다.  자세한 내용은 [추가 섹션에서 생성 된 값](#value-generated-on-add) 을 참조 하세요.

### <a name="value-generated-on-add-or-update-fluent-api"></a>추가 또는 업데이트 시 생성 된 값 (흐름 API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> 이렇게 하면 EF가 추가 되거나 업데이트 된 엔터티에 대해 생성 된 값을 알 수 있으므로 EF는 값을 생성 하는 실제 메커니즘을 설정 하는 것을 보장 하지 않습니다. 자세한 내용은 [추가 또는 업데이트 섹션에서 생성 된 값](#value-generated-on-add-or-update) 을 참조 하세요.
