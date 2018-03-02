---
title: "생성 된 값-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
ms.technology: entity-framework-core
uid: core/modeling/generated-properties
ms.openlocfilehash: 892494461bcf49ee10d05c972da0ba19ca003c35
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="generated-values"></a>생성 된 값

## <a name="value-generation-patterns"></a>값 생성 패턴이

속성에 대해 사용할 수 있는 세 가지 값 생성 패턴이 있습니다.

### <a name="no-value-generation"></a>값을 생성 하지 않을

값을 생성 하지 않을 의미 데이터베이스에 저장 하려면 올바른 값을 항상 제공 합니다. 유효한 값이 컨텍스트에 추가 되기 전에 새 엔터티에 할당 되어야 합니다.

### <a name="value-generated-on-add"></a>에 생성 된 값 추가

에 생성 된 값에 추가 의미 있는 새 엔터티에 대 한 값을 생성 합니다.

사용 중인 데이터베이스 공급자에 따라 수 있는 값 EF 또는 데이터베이스에서 클라이언트 쪽을 생성 합니다. 다음 데이터베이스에서 생성 한 값은 컨텍스트에 엔터티를 추가 하면 EF 임시 값을 할당할 수 있습니다. 이 임시 값 하는 동안 데이터베이스에서 생성 된 값으로 대체 됩니다 `SaveChanges()`합니다.

속성에 할당 된 값을 가진 컨텍스트에 엔터티를 추가 하는 경우에 새로 생성 하는 대신 해당 값을 삽입할 EF 시도 합니다. 속성 값이 CLR 기본값 할당 되지 않은 경우 할당으로 간주 됩니다 (`null` 에 대 한 `string`, `0` 에 대 한 `int`, `Guid.Empty` 에 대 한 `Guid`등.). 자세한 내용은 참조 [생성 된 속성에 대 한 명시적 값](..\saving\explicit-values-generated-properties.md)합니다.

> [!WARNING]  
> 추가 된 엔터티에 대 한 값은 생성 하는 방법을 사용 중인 데이터베이스 공급자에 따라 달라 집니다. 데이터베이스 공급자 일부 속성 형식에 대 한 값 생성을 자동으로 설치 될 수 있습니다 하지만 값이 생성 되는 방식을 수동으로 설치 해야 할 수 있습니다 다른 합니다.
>
> 예를 들어 SQL Server를 사용할 때 값은 자동으로 생성 `GUID` (SQL Server에서 순차적 GUID 알고리즘을 사용 하 여) 속성입니다. 그러나 지정 하는 경우는 `DateTime` 속성이 생성 된 추가 시, 값을 생성할 수 있는 방법을 설치 해야 합니다. 기본값을 구성 하는이 작업을 수행 하는 한 가지 방법은 `GETDATE()`, 참조 [기본값](relational/default-values.md)합니다.

### <a name="value-generated-on-add-or-update"></a>생성 된 값에 추가 또는 업데이트

생성 된 값에 추가 또는 업데이트 (삽입 또는 업데이트) 레코드가 저장 될 때마다 새 값을 생성을 의미 합니다.

마찬가지로 `value generated on add`생성 되는 값이 아닌 값을 삽입할 엔터티를 새로 추가 된 인스턴스에서 속성에 대 한 값을 지정 합니다. 업데이트할 때 명시적 값을 설정할 수 이기도 합니다. 자세한 내용은 참조 [생성 된 속성에 대 한 명시적 값](..\saving\explicit-values-generated-properties.md)합니다.

> [!WARNING]  
> 추가 및 업데이트 된 엔터티에 대 한 값은 생성 하는 방법을 사용 중인 데이터베이스 공급자에 따라 달라 집니다. 자동으로 데이터베이스 공급자 수 필요 해야 수동으로 값은 생성 하는 방법을 설정 하는 다른 일부 속성 형식에 대 한 값 생성을 설치 해야 합니다.
>
> 예를 들어, SQL Server를 사용 하는 경우 `byte[]` 에 생성 된 대로 설정 된 속성을 추가 또는 업데이트 하 고 설치 프로그램을 동시성 토큰으로 표시 됩니다는 `rowversion` 하도록 데이터 형식을-데이터베이스에서 값을 생성 합니다. 그러나 지정 하는 경우는 `DateTime` 속성이 생성 된 추가 / 업데이트를 다음 값을 생성할 수 있는 방법을 설정 해야 합니다. 기본값을 구성 하는이 작업을 수행 하는 한 가지 방법은 `GETDATE()` (참조 [기본값](relational/default-values.md)) 새 행에 대 한 값을 생성 합니다. 다음 (예: 다음 예제에서는 트리거) 업데이트 하는 동안 값을 생성 하는 데이터베이스 트리거를 사용할 수 있습니다.
>
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>규칙

규칙에 따라 비 복합 기본 키 형식이 short, int, long, 또는 Guid 값에서 생성 된 추가 설치 됩니다. 다른 모든 속성을 생성 하지 않을 값으로 설정 됩니다.

## <a name="data-annotations"></a>데이터 주석

### <a name="no-value-generation-data-annotations"></a>값 생성 하지 않을 (데이터 주석)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>에 생성 된 값 (데이터 주석)를 추가 합니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> 방금 추가 된 엔터티에 대 한 값이 생성 되어, EF는 값을 생성 하는 실제 메커니즘을 설치를 보장 하지 않습니다 확인 EF 수 있습니다. 참조 [에 생성 된 값 추가](#value-generated-on-add) 자세한 내용은 섹션.

### <a name="value-generated-on-add-or-update-data-annotations"></a>에 생성 된 값을 추가 또는 업데이트 (데이터 주석)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> 이 추가 되거나 업데이트 된 엔터티에 대 한 값이 생성 되어, EF는 값을 생성 하는 실제 메커니즘을 설치를 보장 하지 않습니다 확인 EF를 있습니다. 참조 [에 생성 된 값을 추가 하거나 업데이트할](#value-generated-on-add-or-update) 자세한 내용은 섹션.

## <a name="fluent-api"></a>Fluent API

지정된 된 속성에 값 생성 패턴을 변경 하려면 Fluent API를 사용할 수 있습니다.

### <a name="no-value-generation-fluent-api"></a>값 생성 하지 않을 (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>에 생성 된 값 (Fluent API)를 추가 합니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` 방금 추가 된 엔터티에 대 한 값이 생성 되어, EF는 값을 생성 하는 실제 메커니즘을 설치를 보장 하지 않습니다 확인 EF 수 있습니다.  참조 [에 생성 된 값 추가](#value-generated-on-add) 자세한 내용은 섹션.

### <a name="value-generated-on-add-or-update-fluent-api"></a>에 생성 된 값을 추가 또는 업데이트 (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> 이 추가 되거나 업데이트 된 엔터티에 대 한 값이 생성 되어, EF는 값을 생성 하는 실제 메커니즘을 설치를 보장 하지 않습니다 확인 EF를 있습니다. 참조 [에 생성 된 값을 추가 하거나 업데이트할](#value-generated-on-add-or-update) 자세한 내용은 섹션.
