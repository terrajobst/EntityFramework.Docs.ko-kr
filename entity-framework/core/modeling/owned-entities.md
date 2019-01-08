---
title: 소유한 엔터티 형식-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: b2d72b08de79939904bf4e726c695440c906a8aa
ms.sourcegitcommit: 7bde8e6ad3c4565a4638646ce04bcf5e66f7b5fd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/07/2019
ms.locfileid: "54069206"
---
# <a name="owned-entity-types"></a>소유 된 엔터티 형식

>[!NOTE]
> 이 기능은 EF Core 2.0의 새로운 기능입니다.

EF Core 모델 엔터티 형식을 다른 엔터티 형식의 탐색 속성에만 나타날 수 있습니다. 이러한 이라고 _소유한 엔터티 형식_합니다. 소유 된 엔터티 형식이 포함 된 엔터티는 해당 _소유자_합니다.

## <a name="explicit-configuration"></a>명시적으로 구성

형식에에서 포함 되지 않습니다 EF Core에서 모델 규칙에 따라 엔터티를 소유 합니다. 사용할 수는 `OwnsOne` 에서 메서드 `OnModelCreating` 주석을 사용 하 여 형식 또는 `OwnedAttribute` (EF Core 2.1의 새로운) 유형을 소유 된 형식으로 구성 합니다.

이 예제에서는 `StreetAddress` id 속성이 없는 형식입니다. 특정 주문의 배송 주소를 지정하기 위한 Order 형식 속성으로 사용됩니다.

사용할 수 있습니다는 `OwnedAttribute` 다른 엔터티 형식에서 참조 하는 경우 소유 엔터티로 처리 하려면:

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

것도 사용할 수는 `OwnsOne` 에서 메서드 `OnModelCreating` 지정 하는 합니다 `ShippingAddress` 소유한 엔터티의 속성이 `Order` 엔터티 형식 하 고 필요한 경우 추가 패싯을 구성 하 합니다.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

경우는 `ShippingAddress` 속성은 private 합니다 `Order` 종류의 문자열 버전을 사용할 수 있습니다는 `OwnsOne` 메서드:

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

참조 된 [전체 샘플 프로젝트](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) 자세한 컨텍스트. 

## <a name="implicit-keys"></a>암시적 키

소유 된 형식의 구성 `OwnsOne` 참조 탐색을 통해 검색 하거나 소유자를 사용 하 여 한 일 관계가 항상, 따라서 외래 키 값은 고유 키 값을 직접 필요 하지 않은 합니다. 이전 예에서 `StreetAddress` 형식 키 속성을 정의할 필요가 없습니다.  

EF Core에서 이러한 개체를 추적 하는 방법을 이해 하려면 기본 키로 만들어졌는지 생각 하면 편리 것을 [섀도 속성](xref:core/modeling/shadow-properties) 소유 된 형식에 대 한 합니다. 소유 된 형식의 인스턴스 키의 값은 소유자 인스턴스 키의 값과 동일 하 게 됩니다.

## <a name="collections-of-owned-types"></a>소유 된 형식의 컬렉션

>[!NOTE]
> 이 기능은 EF Core 2.2의 새로운 기능입니다.

소유 된 형식의 컬렉션을 구성 하려면 `OwnsMany` 에서 사용할 `OnModelCreating`합니다. 그러나 기본 키 구성 되지 않습니다 기본적 되므로 명시적으로 지정 해야 합니다. 일반적으로 외래 키 소유자 및 섀도 상태에 있을 수도 있습니다 고유 속성을 추가로 통합 하는 엔터티의 이러한 형식에 대 한 복합 키를 사용 하는 것:

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a>테이블 분할 형식과 소유 하는 매핑

관계형을 사용 하는 경우 데이터베이스에 규칙 참조에 의해 소유 형식은 소유자와 동일한 테이블에 매핑됩니다. 이렇게 하려면 두 개의 테이블 분할: 소유자의 데이터를 저장할 일부 열을 사용 하 고 소유 엔터티의 데이터를 저장할 일부 열이 사용 됩니다. 이 기능은 일반적인 테이블 분할 이라고 합니다.

> [!TIP]
> 테이블 분할 사용 하 여 저장 된 형식의 수를 소유한 같은 방식으로 사용할 방법을 복합 형식에는 EF6에서 사용 됩니다.

규칙에 따라 EF Core는 열 이름을 지정 하는 데이터베이스 패턴을 소유 된 엔터티 형식의 속성에 대 한 _Navigation_OwnedEntityProperty_합니다. 따라서는 `StreetAddress` 속성 'ShippingAddress_Street' 및 'ShippingAddress_City' 이름의 '주문' 테이블에 표시 됩니다.

추가할 수 있습니다는 `HasColumnName` 메서드 열 이름을 바꾸려면:

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>여러 소유 형식에서 동일한.NET 형식 공유

소유 된 엔터티 형식 수 동일한.NET 형식을 다른 소유 된 엔터티 형식으로 하므로.NET 형식 만으로는 소유 된 형식 식별 하 합니다.

소유자에 게에서 소유 된 엔터티를 가리키는 속성은 이러한 경우에는 _탐색을 정의_ 소유 된 엔터티 형식입니다. EF Core의 관점에서 정의 탐색에는.NET 형식 함께 형식 id의 일부입니다.   

다음 클래스의 예를 들어 `ShippingAddress` 하 고 `BillingAddress` 둘 다 동일한.NET 형식의 `StreetAddress`:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

EF Core에서 이러한 개체의 추적 된 인스턴스를 구분 하는 방법을 이해 하기 위해 정의 탐색 소유 된 형식의.NET 형식과 소유자의 키 값과 함께 인스턴스 키의 일부 바뀌었기는 생각 하는 유용할 수 있습니다.

## <a name="nested-owned-types"></a>중첩 된 소유 된 형식

이 예제의 `OrderDetails` 소유 `BillingAddress` 하 고 `ShippingAddress`, 둘 `StreetAddress` 형식입니다. 그리고 `OrderDetails`는 `DetailedOrder` 형식입니다.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

중첩 된 소유 된 형식 외에도 소유 된 형식에는 일반 엔터티를 참조할 수 일 수도 있습니다 소유자 또는 다른 엔터티를 소유 된 엔터티는 종속 측의으로 합니다. 이 기능은 EF6에서 복합 형식 외에도 소유 된 엔터티 형식을 설정합니다.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

연결할 수는 `OwnsOne` fluent 호출이 모델을 구성 하는 메서드:

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

사용 하 여 동일한 작업 목표를 달성할 수 이기도 `OwnedAttribute` 둘 다에서 `OrderDetails` 고 `StreetAdress`입니다.

## <a name="storing-owned-types-in-separate-tables"></a>소유 된 형식은 별도 테이블에 저장

또한 EF6 복잡 한 형식과 달리 소유 된 형식 소유자의 별도 테이블에 저장할 수 있습니다. 소유 된 형식 소유자와 동일한 테이블에 매핑하는 규칙을 재정의 하기 위해 호출할 수 있습니다 단순히 `ToTable` 하 고 다른 테이블 이름을 제공 합니다. 다음 예제에서는 매핑될 `OrderDetails` 및에서 별도 테이블에는 두 주소 `DetailedOrder`:

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a>소유 된 형식 쿼리

소유자를 쿼리할 때 소유된 형식은 기본적으로 포함됩니다. 사용 하는 데 필요한 것을 `Include` 메서드, 소유 된 형식은 별도 테이블에 저장 된 경우에 합니다. 설명 하기 전에 모델을 기반으로 다음을 가져오도록 `Order`, `OrderDetails` 소유한 두 `StreetAddresses` 데이터베이스에서:

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>제한 사항

이러한 제한 사항 중 일부는 어떻게 소유한 엔터티 형식 작업에 기본적인 되지만 일부는 우리가 할 수 있습니다 나중에 릴리스를 제거 하는 제한 사항:

### <a name="by-design-restrictions"></a>디자인에 따라 제한
- 만들 수 없습니다는 `DbSet<T>` 소유 된 형식에 대 한
- 호출할 수 없습니다 `Entity<T>()` 소유 된 형식에서 사용 하 여 `ModelBuilder`

### <a name="current-shortcomings"></a>현재 단점
- 포함 하는 상속 계층 구조에서 소유 하는 엔터티 형식은 지원 되지 않습니다.
- 참조 탐색 소유한 엔터티 형식 소유자에 게에서 별도 테이블에 명시적으로 매핑되는 경우가 아니면 null 일 수 없습니다.
- (이 소유 된 엔터티 형식을 사용 하 여 구현할 수 없는 값 개체에 대 한 잘 알려진 시나리오) 하는 여러 소유자가 소유 된 엔터티 형식의 인스턴스를 공유할 수 없습니다.

### <a name="shortcomings-in-previous-versions"></a>이전 버전의 단점
- EF Core 2.0 탐색 소유한 엔터티 소유자 계층에서 별도 테이블에 매핑됩니다는 명시적으로 하지 않는 한 엔터티 형식이 파생된 엔터티 형식에서 선언할 수 없습니다 소유 합니다. EF Core 2.1에서이 제한이 제거 되었습니다.
- EF Core 2.0 및 2.1만 참조 소유 된 형식에 탐색 지원 되었습니다. EF Core 2.2에이 제한이 제거 되었습니다.
