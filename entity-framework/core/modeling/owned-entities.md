---
title: 소유한 엔터티 형식-EF Core
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: 3eb7480625db4ebc3ce0b7a18d042139f888dab8
ms.sourcegitcommit: 0935ff275ae739243297f5b97eb21414398125c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39201895"
---
# <a name="owned-entity-types"></a><span data-ttu-id="25cf3-102">소유 된 엔터티 형식</span><span class="sxs-lookup"><span data-stu-id="25cf3-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="25cf3-103">이 기능은 EF Core 2.0의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="25cf3-104">EF Core 모델 엔터티 형식을 다른 엔터티 형식의 탐색 속성에만 나타날 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="25cf3-105">이러한 이라고 _소유한 엔터티 형식_합니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-105">These are called _owned entity types_.</span></span> <span data-ttu-id="25cf3-106">소유 된 엔터티 형식이 포함 된 엔터티는 해당 _소유자_합니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="25cf3-107">명시적으로 구성</span><span class="sxs-lookup"><span data-stu-id="25cf3-107">Explicit configuration</span></span>

<span data-ttu-id="25cf3-108">형식에에서 포함 되지 않습니다 EF Core에서 모델 규칙에 따라 엔터티를 소유 합니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="25cf3-109">사용할 수는 `OwnsOne` 에서 메서드 `OnModelCreating` 주석을 사용 하 여 형식 또는 `OwnedAttribute` (EF Core 2.1의 새로운) 유형을 소유 된 형식으로 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="25cf3-110">이 예제에서는 StreetAddress id 속성이 없는 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="25cf3-111">특정 주문의 배송 주소를 지정하기 위한 Order 형식 속성으로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="25cf3-112">`OnModelCreating`를 사용 하 여는 `OwnsOne` ShippingAddress 속성이 Order 형식 소유한 엔터티 임을 지정 하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

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

<span data-ttu-id="25cf3-113">ShippingAddress 속성이 Order 형식에서 private 인 경우의 문자열 버전을 사용할 수 있습니다는 `OwnsOne` 메서드:</span><span class="sxs-lookup"><span data-stu-id="25cf3-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="25cf3-114">이 예에서 사용 된 `OwnedAttribute` 동일한 목표를 달성 하려면:</span><span class="sxs-lookup"><span data-stu-id="25cf3-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

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

## <a name="implicit-keys"></a><span data-ttu-id="25cf3-115">암시적 키</span><span class="sxs-lookup"><span data-stu-id="25cf3-115">Implicit keys</span></span>

<span data-ttu-id="25cf3-116">EF Core 2.0 및 2.1에서 소유 된 형식은 참조 탐색 속성에 대해서만 가리킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="25cf3-117">소유 된 형식의 컬렉션을 사용 하는 것이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="25cf3-118">이러한 참조 형식을 항상 일대일 관계가 소유자와, 키 값을 직접 필요 하지 않으므로 소유 합니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="25cf3-119">앞의 예에서 StreetAddress 형식은 키 속성을 정의할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="25cf3-120">EF Core에서 이러한 개체를 추적 하는 방법을 이해 하려면 기본 키로 만들어졌는지 생각 하면 편리 것을 [섀도 속성](xref:core/modeling/shadow-properties) 소유 된 형식에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-120">In order to understand how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="25cf3-121">소유 된 형식의 인스턴스 키의 값은 소유자 인스턴스 키의 값과 동일 하 게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="25cf3-122">테이블 분할 형식과 소유 하는 매핑</span><span class="sxs-lookup"><span data-stu-id="25cf3-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="25cf3-123">관계형 데이터베이스를 사용할 때는 소유 된 형식의 규칙에 따라 매핑됩니다 소유자와 동일한 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="25cf3-124">이렇게 하려면 두 개의 테이블 분할: 소유자의 데이터를 저장할 일부 열을 사용 하 고 소유 엔터티의 데이터를 저장할 일부 열이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="25cf3-125">이 기능은 일반적인 테이블 분할 이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="25cf3-126">수 있는 저장 된 테이블 분할 유형을 소유 하 고 사용 어떻게 복합 형식에 EF6 매우 유사 합니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="25cf3-127">규칙에 따라 EF Core는 열 이름을 지정 하는 데이터베이스 패턴을 소유 된 엔터티 형식의 속성에 대 한 _EntityProperty_OwnedEntityProperty_합니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="25cf3-128">따라서 StreetAddress 속성 ShippingAddress_Street 및 ShippingAddress_City 이름을 사용 하 여 Orders 테이블에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="25cf3-129">추가할 수 있습니다는 `HasColumnName` 해당 열 이름 바꾸기 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="25cf3-130">매핑을 것 StreetAddress 공용 속성 인 경우</span><span class="sxs-lookup"><span data-stu-id="25cf3-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="25cf3-131">여러 소유 형식에서 동일한.NET 형식 공유</span><span class="sxs-lookup"><span data-stu-id="25cf3-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="25cf3-132">소유 된 엔터티 형식 수 동일한.NET 형식을 다른 소유 된 엔터티 형식으로 하므로.NET 형식 만으로는 소유 된 형식 식별 하 합니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="25cf3-133">소유자에 게에서 소유 된 엔터티를 가리키는 속성은 이러한 경우에는 _탐색을 정의_ 소유 된 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="25cf3-134">EF Core의 관점에서 정의 탐색에는.NET 형식 함께 형식 id의 일부입니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="25cf3-135">예를 들어, 다음 클래스의 ShippingAddress BillingAddress와 같은.NET 유형, StreetAddress 모두:</span><span class="sxs-lookup"><span data-stu-id="25cf3-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="25cf3-136">EF Core에서 이러한 개체의 추적 된 인스턴스를 구분 하는 방법을 이해 하기 위해 정의 탐색 소유 된 형식의.NET 형식과 소유자의 키 값과 함께 인스턴스 키의 일부 바뀌었기는 생각 하는 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="25cf3-137">중첩 된 소유 된 형식</span><span class="sxs-lookup"><span data-stu-id="25cf3-137">Nested owned types</span></span>

<span data-ttu-id="25cf3-138">이 예에서 OrderDetails BillingAddress 및 두 StreetAddress 형식인 ShippingAddress 소유 합니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="25cf3-139">그런 다음 Order type OrderDetails는 소유 합니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-139">Then OrderDetails is owned by the Order type.</span></span>

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

<span data-ttu-id="25cf3-140">연결할 수는 `OwnsOne` fluent 매핑에서이 모델을 구성 하는 메서드:</span><span class="sxs-lookup"><span data-stu-id="25cf3-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="25cf3-141">사용 하 여 동일한 작업을 수행 하는 것이 불가능 `OwnedAttribute` OrderDetails와 StreetAdress 합니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="25cf3-142">중첩 된 소유 된 형식 외에도 소유 된 형식에는 일반 엔터티를 참조할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="25cf3-143">다음 예에서 국가 일반 소유 되지 않은 엔터티입니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-143">In the following example, Country is a regular non-owned entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="25cf3-144">이 기능은 EF6에서 복합 형식 외에도 소유 된 엔터티 형식을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="25cf3-145">소유 된 형식은 별도 테이블에 저장</span><span class="sxs-lookup"><span data-stu-id="25cf3-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="25cf3-146">또한 EF6 복잡 한 형식과 달리 소유 된 형식 소유자의 별도 테이블에 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="25cf3-147">소유 된 형식 소유자와 동일한 테이블에 매핑하는 규칙을 재정의 하기 위해 호출할 수 있습니다 단순히 `ToTable` 하 고 다른 테이블 이름을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="25cf3-148">다음 예제에서는 매핑될 OrderDetails 및 해당 두 주소 별도 테이블에 주문에서:</span><span class="sxs-lookup"><span data-stu-id="25cf3-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a><span data-ttu-id="25cf3-149">소유 된 형식 쿼리</span><span class="sxs-lookup"><span data-stu-id="25cf3-149">Querying owned types</span></span>

<span data-ttu-id="25cf3-150">소유자를 쿼리할 때 소유된 형식은 기본적으로 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="25cf3-151">사용 하는 데 필요한 것을 `Include` 메서드, 소유 된 형식은 별도 테이블에 저장 된 경우에 합니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="25cf3-152">설명 하기 전에 모델을 기반으로 다음 쿼리는 순서, OrderDetails 및 데이터베이스에서 모든 보류 중인 주문에 대 한 두 소유 StreetAddresses를 끌어옵니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreetAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="25cf3-153">제한 사항</span><span class="sxs-lookup"><span data-stu-id="25cf3-153">Limitations</span></span>

<span data-ttu-id="25cf3-154">이러한 제한 사항 중 일부는 어떻게 소유한 엔터티 형식 작업에 기본적인 되지만 일부는 우리가 할 수 있습니다 나중에 릴리스를 제거 하는 제한 사항:</span><span class="sxs-lookup"><span data-stu-id="25cf3-154">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="25cf3-155">이전 버전의 단점</span><span class="sxs-lookup"><span data-stu-id="25cf3-155">Shortcomings in previous versions</span></span>
- <span data-ttu-id="25cf3-156">EF Core 2.0 탐색 소유한 엔터티 소유자 계층에서 별도 테이블에 매핑됩니다는 명시적으로 하지 않는 한 엔터티 형식이 파생된 엔터티 형식에서 선언할 수 없습니다 소유 합니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-156">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="25cf3-157">EF Core 2.1에서이 제한이 제거 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-157">This limitation has been removed in EF Core 2.1</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="25cf3-158">현재 단점</span><span class="sxs-lookup"><span data-stu-id="25cf3-158">Current shortcomings</span></span>
- <span data-ttu-id="25cf3-159">포함 하는 상속 계층 구조에서 소유 하는 엔터티 형식은 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-159">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="25cf3-160">컬렉션 탐색 속성 (탐색은 현재 지원만 참조)에 의해 소유 된 엔터티 형식은 가리키는 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-160">Owned entity types cannot be pointed at by a collection navigation property (only reference navigations are currently supported)</span></span>
- <span data-ttu-id="25cf3-161">탐색 소유한 엔터티 형식 소유자에 게에서 별도 테이블에 명시적으로 매핑되는 경우가 아니면 null 일 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-161">Navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="25cf3-162">(이 소유 된 엔터티 형식을 사용 하 여 구현할 수 없는 값 개체에 대 한 잘 알려진 시나리오) 하는 여러 소유자가 소유 된 엔터티 형식의 인스턴스를 공유할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="25cf3-162">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="25cf3-163">디자인에 따라 제한</span><span class="sxs-lookup"><span data-stu-id="25cf3-163">By-design restrictions</span></span>
- <span data-ttu-id="25cf3-164">만들 수 없습니다는 `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="25cf3-164">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="25cf3-165">호출할 수 없습니다 `Entity<T>()` 소유 된 형식에서 사용 하 여 `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="25cf3-165">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
