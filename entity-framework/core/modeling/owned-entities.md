---
title: 엔터티 형식-EF 코어 소유
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: f2f05499a3e3494f420d916df2db19667a6f1e29
ms.sourcegitcommit: 26f33758c47399ae933f22fec8e1d19fa7d2c0b7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/19/2018
---
# <a name="owned-entity-types"></a>소유한 엔터티 형식

>[!NOTE]
> 이 기능은 EF 코어 2.0의 새로운 기능입니다.

EF 코어 다른 엔터티 형식의 탐색 속성에만 표시할 수 있는 모델 엔터티 형식에 있습니다. 이 라고 _엔터티 형식 소유_합니다. 소유 엔터티 형식이 포함 된 엔터티는 해당 _소유자_합니다.

## <a name="explicit-configuration"></a>명시적 구성

형식에에서 포함 되지 않습니다 EF 코어 모델 규칙에 따라 엔터티를 소유 합니다. 사용할 수 있습니다는 `OwnsOne` 에서 메서드 `OnModelCreating` 주석을 사용 하 여 형식 또는 `OwnedAttribute` (EF 코어 2.1의 새로운) 유형을 소유 하는 형식으로 구성 합니다.

이 예제에서는 StreetAddress 포함 형식인 identity 속성이 없습니다. 특정 주문의 배송 주소를 지정하기 위한 Order 형식 속성으로 사용됩니다. `OnModelCreating`를 사용 하 여는 `OwnsOne` ShippingAddress 속성이 Order 형식의 소유 엔터티 인지를 지정 하는 메서드.

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

ShippingAddress 속성이 Order type private 인 경우의 문자열 버전을 사용할 수 있습니다는 `OwnsOne` 메서드:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

이 예에서는 사용 된 `OwnedAttribute` 같은 목표를 달성 하기:

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

EF 코어 2.0 및 2.1에만 참조 탐색 속성을 소유 형식 가리킬 수 있습니다. 소유 형식의 컬렉션을 사용할 수 없습니다. 이러한 참조는 항상 형식 소유자와 한 일 관계에 있는, 따라서 키 값을 직접 않아도 소유 합니다. 이전 예에서 StreetAddress 형식 키 속성을 정의 하 필요 하지 않습니다.  

EF 코어에서 이러한 개체를 추적 하는 방법을 이해 하려면에서 기본 키로 만들어졌는지 생각 하면 도움이 될는 [속성 그림자](xref:core/modeling/shadow-properties) 소유 형식에 대 한 합니다. 소유 형식의 인스턴스 키의 값은 소유자 인스턴스 키의 값과 동일 하 게 됩니다.      

## <a name="mapping-owned-types-with-table-splitting"></a>소유 하는 테이블 분할을 사용 하는 형식 매핑

관계형 데이터베이스를 사용할 때 형식을 소유 하는 규칙에 따라 매핑됩니다 소유자와 같은 테이블. 이렇게 하려면 두 개의 테이블 분할: 일부 열에서 소유자의 데이터를 저장 하는 데 사용 됩니다 하 고 일부 열에서 소유한 엔터티의 데이터를 저장 하는 데 사용 됩니다. 이것이 테이블 분할 이라고 하는 일반적인 기능입니다.

> [!TIP]
> 수 있는 저장 된 테이블 분할 유형을 소유 하 고 사용 어떻게 복합 형식에 EF6 매우 유사 합니다.

규칙에 따라 EF 코어 패턴을 소유한 엔터티 형식의 속성에 대 한 데이터베이스 열에 이름을 _EntityProperty_OwnedEntityProperty_합니다. 따라서 StreetAddress 속성 ShippingAddress_Street 및 ShippingAddress_City 이름을 Orders 테이블에 표시 됩니다.

추가할 수 있습니다는 `HasColumnName` 열의 이름을 바꿀 수 있습니다. 매핑을 것 경우 StreetAddress은 공용 속성

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>여러 소유 형식에서 같은.NET 형식 공유

소유한 엔터티 형식을 수 있습니다 다른 소유한 엔터티 형식과 같은.NET 형식의 따라서.NET 유형을 소유 유형을 식별 하기에 충분 한 수 없게 수 있습니다.

소유자 로부터 소유 엔터티를 가리키는 속성은 이러한 경우에는 _탐색 정의_ 소유 엔터티 형식입니다. EF 코어의 관점에서 정의 탐색에는.NET 형식 함께 형식 id의 일부입니다.   

예를 들어 다음과 같은 클래스의 ShippingAddress 및 BillingAddress 조치가 모두는 동일한 유형의.NET StreetAddress:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

EF 코어에서 이러한 개체의 추적 된 인스턴스를 구분 하는 방법을 이해 하기 위해 정의 탐색을 소유자의 키 값과 함께 인스턴스 키의 일부 및 소유 형식의.NET 형식을 수 생각 하면 유용할 수 있습니다.

## <a name="nested-owned-types"></a>중첩 소유 형식

이 예에서 OrderDetails BillingAddress 및 두 StreetAddress 형식인 ShippingAddress 소유 합니다. 다음은 주문 유형을 OrderDetails는 소유 합니다.

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

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

에 연결할 수는 `OwnsOne` fluent 매핑에서이 모델을 구성 하는 메서드:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

사용 하 여 동일한 작업을 수행할 수 `OwnedAttribute` OrderDetails와 StreetAdress 합니다.

중첩 된 소유 형식 외에도 소유 형식 일반 엔터티를 참조할 수 있습니다. 다음 예제에서는 국가 일반 (즉, 소유 되지 않은-) 엔터티입니다.

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

이 기능은 EF6 소유한 엔터티 형식 복합 형식 외에도 설정합니다.

## <a name="storing-owned-types-in-separate-tables"></a>별도 테이블에는 형식을 소유 하 고 저장

또한 EF6 복합 형식과 달리 소유 형식 소유자 로부터 별도 테이블에 저장할 수 있습니다. 소유 형식 소유자와 같은 테이블에 매핑하는 규칙을 재정의 하기 위해 호출 하면 `ToTable` 하 고 다른 테이블 이름을 제공 합니다. 다음 예제에서는 OrderDetails와 그 두 주소에 매핑됩니다 별도 테이블에서 순서:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a>소유 형식 쿼리

소유자를 쿼리할 때 소유된 형식은 기본적으로 포함됩니다. 사용할 필요는 없습니다는 `Include` 메서드를 소유 형식이 별도 테이블에 저장 하는 경우에 마찬가지입니다. 다음 쿼리에서 앞에서 설명한 것 모델에 따라, 순서, OrderDetails 및 데이터베이스에서 보류 중인 모든 주문에 대해 두 개의 소유 StreeAddresses 끌어오고:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>제한 사항

다음은 몇 가지 제한 사항이 소유한 엔터티 형식입니다. 기본 형식 작업 방법 소유 하는 경우 이러한 제한 사항 중 일부는 항목도 있지만 몇 가지 이후 릴리스를 제거 하 고 싶습니다 지정 시간 제한:

### <a name="current-shortcomings"></a>현재 단점
- 소유 형식의 상속은 지원 되지 않습니다.
- 컬렉션 탐색 속성에 의해 소유 형식에서 지정한 수 없습니다.
- 테이블에서 분할을 사용 하므로 기본 다른 테이블에 명시적으로 매핑된 하지 않는 한 다음과 같은 제한 사항이 방향이 소유 합니다.
   - 파생된 형식에 의해 소유할 수 없습니다.
   - 정의 하는 탐색 속성을 설정할 수 없습니다 null로 (즉, 소유 하 고 동일한 테이블에서 유형은 항상 필요)

### <a name="by-design-restrictions"></a>디자인에 따라 제한 사항
- 만들 수 없습니다는 `DbSet<T>`
- 호출할 수 없습니다 `Entity<T>()` 에 소유 형식의 `ModelBuilder`
