---
title: 소유한 엔터티 형식-EF Core
author: julielerman
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: afbc853feab56d31a8ceba0da05fe6dc508b65bb
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994071"
---
# <a name="owned-entity-types"></a>소유 된 엔터티 형식

>[!NOTE]
> 이 기능은 EF Core 2.0의 새로운 기능입니다.

EF Core 모델 엔터티 형식을 다른 엔터티 형식의 탐색 속성에만 나타날 수 있습니다. 이러한 이라고 _소유한 엔터티 형식_합니다. 소유 된 엔터티 형식이 포함 된 엔터티는 해당 _소유자_합니다.

## <a name="explicit-configuration"></a>명시적으로 구성

형식에에서 포함 되지 않습니다 EF Core에서 모델 규칙에 따라 엔터티를 소유 합니다. 사용할 수는 `OwnsOne` 에서 메서드 `OnModelCreating` 주석을 사용 하 여 형식 또는 `OwnedAttribute` (EF Core 2.1의 새로운) 유형을 소유 된 형식으로 구성 합니다.

이 예제에서는 StreetAddress id 속성이 없는 형식입니다. 특정 주문의 배송 주소를 지정하기 위한 Order 형식 속성으로 사용됩니다. `OnModelCreating`를 사용 하 여는 `OwnsOne` ShippingAddress 속성이 Order 형식 소유한 엔터티 임을 지정 하는 방법입니다.

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

// OnModelCreating
modelBuilder.Entity<Order>().OwnsOne(p => p.ShippingAddress);
```

ShippingAddress 속성이 Order 형식에서 private 인 경우의 문자열 버전을 사용할 수 있습니다는 `OwnsOne` 메서드:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

이 예에서 사용 된 `OwnedAttribute` 동일한 목표를 달성 하려면:

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="implicit-keys"></a>암시적 키

EF Core 2.0 및 2.1에서 소유 된 형식은 참조 탐색 속성에 대해서만 가리킬 수 있습니다. 소유 된 형식의 컬렉션을 사용 하는 것이 없습니다. 이러한 참조 형식을 항상 일대일 관계가 소유자와, 키 값을 직접 필요 하지 않으므로 소유 합니다. 앞의 예에서 StreetAddress 형식은 키 속성을 정의할 필요가 없습니다.  

EF Core에서 이러한 개체를 추적 하는 방법을 이해 하려면 기본 키로 만들어졌는지 생각 하면 편리 것을 [섀도 속성](xref:core/modeling/shadow-properties) 소유 된 형식에 대 한 합니다. 소유 된 형식의 인스턴스 키의 값은 소유자 인스턴스 키의 값과 동일 하 게 됩니다.      

## <a name="mapping-owned-types-with-table-splitting"></a>테이블 분할 형식과 소유 하는 매핑

관계형 데이터베이스를 사용할 때는 소유 된 형식의 규칙에 따라 매핑됩니다 소유자와 동일한 테이블입니다. 이렇게 하려면 두 개의 테이블 분할: 소유자의 데이터를 저장할 일부 열을 사용 하 고 소유 엔터티의 데이터를 저장할 일부 열이 사용 됩니다. 이 기능은 일반적인 테이블 분할 이라고 합니다.

> [!TIP]
> 수 있는 저장 된 테이블 분할 유형을 소유 하 고 사용 어떻게 복합 형식에 EF6 매우 유사 합니다.

규칙에 따라 EF Core는 열 이름을 지정 하는 데이터베이스 패턴을 소유 된 엔터티 형식의 속성에 대 한 _EntityProperty_OwnedEntityProperty_합니다. 따라서 StreetAddress 속성 ShippingAddress_Street 및 ShippingAddress_City 이름을 사용 하 여 Orders 테이블에 표시 됩니다.

추가할 수 있습니다는 `HasColumnName` 해당 열 이름 바꾸기 방법입니다. 매핑을 것 StreetAddress 공용 속성 인 경우

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>여러 소유 형식에서 동일한.NET 형식 공유

소유 된 엔터티 형식 수 동일한.NET 형식을 다른 소유 된 엔터티 형식으로 하므로.NET 형식 만으로는 소유 된 형식 식별 하 합니다.

소유자에 게에서 소유 된 엔터티를 가리키는 속성은 이러한 경우에는 _탐색을 정의_ 소유 된 엔터티 형식입니다. EF Core의 관점에서 정의 탐색에는.NET 형식 함께 형식 id의 일부입니다.   

예를 들어, 다음 클래스의 ShippingAddress BillingAddress와 같은.NET 유형, StreetAddress 모두:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

EF Core에서 이러한 개체의 추적 된 인스턴스를 구분 하는 방법을 이해 하기 위해 정의 탐색 소유 된 형식의.NET 형식과 소유자의 키 값과 함께 인스턴스 키의 일부 바뀌었기는 생각 하는 유용할 수 있습니다.

## <a name="nested-owned-types"></a>중첩 된 소유 된 형식

이 예에서 OrderDetails BillingAddress 및 두 StreetAddress 형식인 ShippingAddress 소유 합니다. 그런 다음 Order type OrderDetails는 소유 합니다.

``` csharp
public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
    public OrderStatus Status { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public enum OrderStatus
{
    Pending,
    Shipped
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

연결할 수는 `OwnsOne` fluent 매핑에서이 모델을 구성 하는 메서드:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

사용 하 여 동일한 작업을 수행 하는 것이 불가능 `OwnedAttribute` OrderDetails와 StreetAdress 합니다.

중첩 된 소유 된 형식 외에도 소유 된 형식에는 일반 엔터티를 참조할 수 있습니다. 다음 예에서 국가 일반 소유 되지 않은 엔터티입니다.

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

이 기능은 EF6에서 복합 형식 외에도 소유 된 엔터티 형식을 설정합니다.

## <a name="storing-owned-types-in-separate-tables"></a>소유 된 형식은 별도 테이블에 저장

또한 EF6 복잡 한 형식과 달리 소유 된 형식 소유자의 별도 테이블에 저장할 수 있습니다. 소유 된 형식 소유자와 동일한 테이블에 매핑하는 규칙을 재정의 하기 위해 호출할 수 있습니다 단순히 `ToTable` 하 고 다른 테이블 이름을 제공 합니다. 다음 예제에서는 매핑될 OrderDetails 및 해당 두 주소 별도 테이블에 주문에서:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
        od.ToTable("OrderDetails");
    });
```

## <a name="querying-owned-types"></a>소유 된 형식 쿼리

소유자를 쿼리할 때 소유된 형식은 기본적으로 포함됩니다. 사용 하는 데 필요한 것을 `Include` 메서드, 소유 된 형식은 별도 테이블에 저장 된 경우에 합니다. 설명 하기 전에 모델을 기반으로 다음 쿼리는 순서, OrderDetails 및 데이터베이스에서 모든 보류 중인 주문에 대 한 두 소유 StreetAddresses를 끌어옵니다.

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>제한 사항

이러한 제한 사항 중 일부는 어떻게 소유한 엔터티 형식 작업에 기본적인 되지만 일부는 우리가 할 수 있습니다 나중에 릴리스를 제거 하는 제한 사항:

### <a name="shortcomings-in-previous-versions"></a>이전 버전의 단점
- EF Core 2.0 탐색 소유한 엔터티 소유자 계층에서 별도 테이블에 매핑됩니다는 명시적으로 하지 않는 한 엔터티 형식이 파생된 엔터티 형식에서 선언할 수 없습니다 소유 합니다. EF Core 2.1에서이 제한이 제거 되었습니다.

### <a name="current-shortcomings"></a>현재 단점
- 포함 하는 상속 계층 구조에서 소유 하는 엔터티 형식은 지원 되지 않습니다.
- 컬렉션 탐색 속성 (탐색은 현재 지원만 참조)에 의해 소유 된 엔터티 형식은 가리키는 수 없습니다.
- 탐색 소유한 엔터티 형식 소유자에 게에서 별도 테이블에 명시적으로 매핑되는 경우가 아니면 null 일 수 없습니다.
- (이 소유 된 엔터티 형식을 사용 하 여 구현할 수 없는 값 개체에 대 한 잘 알려진 시나리오) 하는 여러 소유자가 소유 된 엔터티 형식의 인스턴스를 공유할 수 없습니다.

### <a name="by-design-restrictions"></a>디자인에 따라 제한
- 만들 수 없습니다는 `DbSet<T>`
- 호출할 수 없습니다 `Entity<T>()` 소유 된 형식에서 사용 하 여 `ModelBuilder`
