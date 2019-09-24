---
title: 필드 지원-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: c3ca8bb97992c192672e8c2f2040b0de029df68d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197487"
---
# <a name="backing-fields"></a>지원 필드

> [!NOTE]  
> 이 기능은 EF Core 1.1의 새로운 기능입니다.

지원 필드를 사용 하면 EF가 속성이 아닌 필드를 읽고 쓸 수 있습니다. 이는 클래스의 캡슐화를 사용 하 여 응용 프로그램 코드를 통해 데이터에 대 한 액세스를 제한 하는 기능을 사용 하는 것을 제한 하는 데 사용 되는 경우에 유용할 수 있지만, 이러한 제한을 사용 하지 않고 값을 데이터베이스에서 읽거나 데이터베이스에 써야 합니다. 된.

## <a name="conventions"></a>규칙

규칙에 따라 다음 필드는 지정 된 속성에 대 한 지원 필드로 검색 됩니다 (우선 순위에 따라 나열 됨). 모델에 포함 된 속성에 대해서만 필드가 검색 됩니다. 모델에 포함 된 속성에 대 한 자세한 내용은 [속성을 제외 하 고 & 포함](included-properties.md)을 참조 하세요.

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

지원 필드를 구성 하면 EF는 속성 setter를 사용 하는 대신 데이터베이스에서 엔터티 인스턴스를 구체화 하는 경우 해당 필드에 직접 기록 합니다. EF가 다른 시간에 값을 읽거나 써야 하는 경우 가능 하면 속성을 사용 합니다. 예를 들어 EF가 속성의 값을 업데이트 해야 하는 경우 속성 setter (정의 된 경우)를 사용 합니다. 속성이 읽기 전용 이면 필드에 기록 됩니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석으로는 지원 필드를 구성할 수 없습니다.

## <a name="fluent-api"></a>Fluent API

흐름 API를 사용 하 여 속성에 대 한 지원 필드를 구성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>필드가 사용 되는 경우 제어

EF에서 필드 또는 속성을 사용 하는 경우을 구성할 수 있습니다. 지원 되는 옵션은 [Propertyaccessmode 열거형](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) 을 참조 하세요.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>속성이 없는 필드

엔터티 클래스에 해당 CLR 속성이 없는 모델에서 개념적 속성을 만들 수도 있습니다. 대신 필드를 사용 하 여 엔터티에 데이터를 저장 합니다. 이는 데이터가 변경 추적 장치에 저장 되는 [섀도 속성과](shadow-properties.md)는 다릅니다. 일반적으로 엔터티 클래스에서 메서드를 사용 하 여 값을 가져오거나 설정 하는 경우에 사용 됩니다.

`Property(...)` API의 필드 이름을 EF에 제공할 수 있습니다. 지정 된 이름의 속성이 없으면 EF에서 필드를 찾습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

필드 이름 이외의 이름을 속성에 지정 하도록 선택할 수도 있습니다. 이 이름은 모델을 만들 때 사용 되며, 특히 데이터베이스의에 매핑되는 열 이름에 사용 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldConceptualProperty.cs#Sample)]

엔터티 클래스에 속성이 없는 경우 LINQ 쿼리에서 `EF.Property(...)` 메서드를 사용 하 여 개념적으로 모델의 일부인 속성을 참조할 수 있습니다.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
