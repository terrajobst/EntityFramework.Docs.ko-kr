---
title: 소유 엔터티 형식-EF Core
description: Entity Framework Core를 사용할 때 소유 엔터티 형식 또는 집계를 구성 하는 방법
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/owned-entities
ms.openlocfilehash: 30b91b6e66b6c0f516d1ba12485304b52770cbef
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781237"
---
# <a name="owned-entity-types"></a>소유한 엔터티 형식

EF Core를 사용 하면 다른 엔터티 형식의 탐색 속성에만 나타날 수 있는 엔터티 형식을 모델링할 수 있습니다. 이러한 엔터티를 _소유 된 엔터티 형식_이라고 합니다. 소유 된 엔터티 형식을 포함 하는 엔터티는 해당 _소유자_입니다.

소유 된 엔터티는 기본적으로 소유자의 일부 이며이를 제외 하 고는 [집계](https://martinfowler.com/bliki/DDD_Aggregate.html)와 개념적으로 유사 합니다. 즉, 소유 된 형식은 소유자와 관계의 종속 측에 정의 됩니다.

## <a name="explicit-configuration"></a>명시적 구성

소유 된 엔터티 형식은 규칙에 따라 모델의 EF Core에 포함 되지 않습니다. `OnModelCreating`에서 `OwnsOne` 메서드를 사용 하거나 `OwnedAttribute`를 사용 하 여 형식에 주석을 달 수 있습니다 (EF Core 2.1의 새로운 형식).

이 예제에서 `StreetAddress`는 identity 속성이 없는 형식입니다. 특정 주문의 배송 주소를 지정하기 위한 Order 형식 속성으로 사용됩니다.

`OwnedAttribute`를 사용 하 여 다른 엔터티 형식에서 참조 되는 경우 소유 엔터티로 처리할 수 있습니다.

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

`OnModelCreating`에서 `OwnsOne` 메서드를 사용 하 여 `ShippingAddress` 속성이 `Order` 엔터티 형식의 소유 된 엔터티 임을 지정 하 고 필요한 경우 추가 패싯을 구성할 수도 있습니다.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

`ShippingAddress` 속성이 `Order` 형식에서 private 이면 `OwnsOne` 메서드의 문자열 버전을 사용할 수 있습니다.

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

자세한 컨텍스트는 [전체 샘플 프로젝트](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) 를 참조 하세요.

## <a name="implicit-keys"></a>암시적 키

`OwnsOne`로 구성 되거나 참조 탐색을 통해 검색 된 소유 형식은 항상 소유자와 일 대 일 관계를 가지 므로 외래 키 값이 고유 하므로 고유한 키 값이 필요 하지 않습니다. 이전 예제에서 `StreetAddress` 형식은 키 속성을 정의할 필요가 없습니다.  

EF Core에서 이러한 개체를 추적 하는 방법을 이해 하기 위해 소유 된 형식에 대 한 [그림자 속성](xref:core/modeling/shadow-properties) 으로 기본 키가 생성 되는 것을 알고 있으면 유용 합니다. 소유 된 형식의 인스턴스 키 값은 소유자 인스턴스 키의 값과 동일 합니다.

## <a name="collections-of-owned-types"></a>소유 된 형식의 컬렉션

> [!NOTE]
> 이 기능은 EF Core 2.2의 새로운 기능입니다.

소유 된 형식의 컬렉션을 구성 하려면 `OnModelCreating`에서 `OwnsMany`를 사용 합니다.

소유 된 형식에는 기본 키가 필요 합니다. .NET 형식에 적합 한 후보 속성이 없으면 하나를 만들 수 EF Core. 그러나 소유 된 형식이 컬렉션을 통해 정의 되는 경우에는 `OwnsOne`와 같이 소유 된 인스턴스의 소유자 및 기본 키에 대 한 외래 키 역할을 수행 하기 위해 섀도 속성을 만드는 것 만으로는 충분 하지 않습니다. 각 소유자에 대해 소유 된 형식 인스턴스가 여러 개 있을 수 있으므로 소유자의 키가 소유한 각 인스턴스에 대해 고유한 id를 제공 하기에 충분 하지 않습니다.

이에 대 한 가장 간단한 두 가지 솔루션은 다음과 같습니다.

- 소유자를 가리키는 외래 키와 관계 없이 새 속성에 서로게이트 기본 키를 정의 합니다. 포함 된 값은 모든 소유자에서 고유 해야 합니다 (예: 부모 {1}에 자식 {1}있는 경우 부모 {2}에 자식 {1}포함 될 수 없음). 따라서 값에 내재 된 의미가 없습니다. 외래 키가 기본 키의 일부가 아니므로 해당 값을 변경할 수 있으므로 한 부모에서 다른 부모로 자식 항목을 이동할 수 있습니다. 그러나 일반적으로 집계 의미 체계를 기반으로 합니다.
- 외래 키 및 추가 속성을 복합 키로 사용 합니다. 이제 추가 속성 값은 지정 된 부모에 대해서만 고유 해야 합니다. 따라서 부모 {1}에 자식 {1,1} 있는 경우 부모 {2}에도 자식 {2,1}포함 될 수 있습니다. 기본 키의 외래 키 부분을 만들면 소유자와 소유 된 엔터티 간의 관계가 변경할 수 없게 되 고 집계 의미 체계가 향상 됩니다. 이 EF Core 기본적으로 수행 됩니다.

이 예제에서는 `Distributor` 클래스를 사용 합니다.

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

기본적으로 `ShippingCenters` 탐색 속성을 통해 참조 하는 소유 된 형식에 사용 되는 기본 키는 `("DistributorId", "Id")` 됩니다. 여기서 `"DistributorId"`는 FK 이며 `"Id"`는 고유한 `int` 값입니다.

다른 PK 호출을 구성 하려면 다음을 `HasKey`합니다.

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> EF Core 3.0 `WithOwner()` 메서드가 없으므로이 호출을 제거 해야 합니다. 또한 기본 키는 자동으로 검색 되지 않으므로 항상 지정 해야 합니다.

## <a name="mapping-owned-types-with-table-splitting"></a>테이블 분할을 사용 하 여 소유 된 형식 매핑

관계형 데이터베이스를 사용 하는 경우 기본적으로 소유 하는 참조는 소유자와 동일한 테이블에 매핑됩니다. 이렇게 하려면 두 개의 테이블을 분할 해야 합니다. 일부 열은 소유자의 데이터를 저장 하는 데 사용 되 고 일부 열은 소유 된 엔터티의 데이터를 저장 하는 데 사용 됩니다. 이는 [테이블 분할](table-splitting.md)이라는 일반적인 기능입니다.

기본적으로 EF Core는 패턴 _Navigation_OwnedEntityProperty_따라 소유 된 엔터티 형식의 속성에 대 한 데이터베이스 열 이름을로 합니다. 따라서 `StreetAddress` 속성은 이름이 ' ShippingAddress_Street '이 고 ' ShippingAddress_City ' 인 ' Orders ' 테이블에 표시 됩니다.

`HasColumnName` 메서드를 사용 하 여 해당 열의 이름을 바꿀 수 있습니다.

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

> [!NOTE]
> [Ignore](/dotnet/api/microsoft.entityframeworkcore.metadata.builders.ownednavigationbuilder.ignore) 와 같은 일반적인 엔터티 형식 구성 메서드는 대부분 동일한 방식으로 호출할 수 있습니다.

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>여러 개의 소유 된 형식 간에 동일한 .NET 형식 공유

소유 된 엔터티 형식은 다른 소유 된 엔터티 형식과 동일한 .NET 형식일 수 있으므로 .NET 형식은 소유 된 형식을 식별 하기에 충분 하지 않을 수 있습니다.

이러한 경우 소유자를 가리키는 속성은 소유 된 엔터티 형식을 정의 하는 _탐색_ 이 됩니다. EF Core 관점에서, 정의 탐색은 .NET 형식과 함께 형식의 id의 일부입니다.

예를 들어 다음 클래스 `ShippingAddress` 및 `BillingAddress` 모두 동일한 .NET 유형인 `StreetAddress`입니다.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

EF Core에서 이러한 개체의 추적 된 인스턴스를 구분 하는 방법을 이해 하기 위해 정의 하는 탐색이 소유자의 키 값과 소유 된 형식의 .NET 형식으로 인스턴스 키의 일부가 되는 것으로 생각 하는 것이 유용할 수 있습니다.

## <a name="nested-owned-types"></a>중첩 된 소유 형식

이 예에서는 `StreetAddress` 형식인 `BillingAddress` 및 `ShippingAddress`를 소유 `OrderDetails` 합니다. 그리고 `OrderDetails`는 `DetailedOrder` 형식입니다.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

소유 된 형식에 대 한 각 탐색은 완전히 독립적으로 구성 된 별도의 엔터티 형식을 정의 합니다.

중첩 된 소유 형식 외에도 소유 된 형식은 정규 엔터티를 참조할 수 있으며 소유 된 엔터티가 종속 측에 있으면 소유자 이거나 다른 엔터티가 될 수 있습니다. 이 기능은 소유 하는 엔터티 형식을 EF6의 복합 형식과 분리 하 여 설정 합니다.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

흐름 호출에 `OwnsOne` 메서드를 연결 하 여이 모델을 구성할 수 있습니다.

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

소유자에서 뒤로 가리키기 탐색 속성을 구성 하는 데 사용 되는 `WithOwner` 호출을 확인 합니다. 소유권 관계의 일부가 아닌 소유자 엔터티 형식에 대 한 탐색을 구성 하려면 `WithOwner()` 인수 없이 호출 해야 합니다.

`OrderDetails`와 `StreetAddress`에서 `OwnedAttribute`를 사용 하 여 결과를 얻을 수 있습니다.

## <a name="storing-owned-types-in-separate-tables"></a>소유 형식을 별도의 테이블에 저장

또한 EF6 복합 형식과 달리 소유 된 형식은 소유자와는 별도의 테이블에 저장 될 수 있습니다. 소유 된 형식을 소유자와 동일한 테이블에 매핑하는 규칙을 재정의 하려면 단순히 `ToTable`를 호출 하 고 다른 테이블 이름을 제공 하면 됩니다. 다음 예에서는 `OrderDetails` 및 두 개의 주소를 `DetailedOrder`별도의 테이블에 매핑합니다.

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

또한 `TableAttribute`를 사용 하 여이를 수행할 수 있지만, 소유 된 형식에 여러 개의 탐색이 있는 경우에는이 작업이 실패 합니다 .이 경우 여러 엔터티 형식이 동일한 테이블에 매핑됩니다.

## <a name="querying-owned-types"></a>소유 형식 쿼리

소유자를 쿼리할 때 소유된 형식은 기본적으로 포함됩니다. 소유 된 형식이 별도의 테이블에 저장 된 경우에도 `Include` 메서드를 사용할 필요가 없습니다. 앞에서 설명한 모델에 따라 다음 쿼리는 데이터베이스에서 `Order`, `OrderDetails` 및 소유 된 두 개의 `StreetAddresses`을 가져옵니다.

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>제한 사항

이러한 제한 사항 중 일부는 소유 된 엔터티 형식이 작동 하는 방식에 대 한 기본 이지만 다른 일부는 이후 릴리스에서 제거할 수 있는 제한 사항이 있습니다.

### <a name="by-design-restrictions"></a>디자인 제한 사항

- 소유 된 형식에 대 한 `DbSet<T>`를 만들 수 없습니다.
- 에서 소유 된 형식을 사용 하는 `Entity<T>()`를 호출할 수 없습니다 `ModelBuilder`

### <a name="current-shortcomings"></a>현재 단점

- 소유 된 엔터티 형식에는 상속 계층을 사용할 수 없습니다.
- 소유 된 엔터티 형식에 대 한 참조 탐색은 소유자와 별도의 테이블에 명시적으로 매핑되지 않는 한 null 일 수 없습니다.
- 소유 된 엔터티 형식의 인스턴스는 여러 소유자가 공유할 수 없습니다 .이는 소유 된 엔터티 형식을 사용 하 여 구현할 수 없는 값 개체에 대 한 잘 알려진 시나리오입니다.

### <a name="shortcomings-in-previous-versions"></a>이전 버전의 단점

- EF Core 2.0에서 소유 된 엔터티는 소유자 계층 구조와 별도의 테이블에 명시적으로 매핑되어 있지 않는 한, 파생 엔터티 형식에서 소유 된 엔터티 형식에 대 한 탐색을 선언할 수 없습니다. EF Core 2.1에서이 제한이 제거 되었습니다.
- EF Core 2.0 및 2.1에서는 소유 된 형식에 대 한 참조 탐색만 지원 됩니다. EF Core 2.2에서이 제한이 제거 되었습니다.
