---
title: "엔터티 형식-EF 코어 소유"
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: a6823377eb626ca92263c31351e1aef61db5a787
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="owned-entity-types"></a><span data-ttu-id="12437-102">소유한 엔터티 형식</span><span class="sxs-lookup"><span data-stu-id="12437-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="12437-103">이 기능은 EF 코어 2.0의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="12437-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="12437-104">EF 코어 다른 엔터티 형식의 탐색 속성에만 표시할 수 있는 모델 엔터티 형식에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="12437-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="12437-105">이 라고 _엔터티 형식 소유_합니다.</span><span class="sxs-lookup"><span data-stu-id="12437-105">These are called _owned entity types_.</span></span> <span data-ttu-id="12437-106">소유 엔터티 형식이 포함 된 엔터티는 해당 _소유자_합니다.</span><span class="sxs-lookup"><span data-stu-id="12437-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="12437-107">명시적 구성</span><span class="sxs-lookup"><span data-stu-id="12437-107">Explicit configuration</span></span>

<span data-ttu-id="12437-108">형식에에서 포함 되지 않습니다 EF 코어 모델 규칙에 따라 엔터티를 소유 합니다.</span><span class="sxs-lookup"><span data-stu-id="12437-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="12437-109">사용할 수 있습니다는 `OwnsOne` 에서 메서드 `OnModelCreating` 주석을 사용 하 여 형식 또는 `OwnedAttrbibute` (EF 코어 2.1의 새로운) 유형을 소유 하는 형식으로 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="12437-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttrbibute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="12437-110">이 예제에서는 StreetAddress 포함 형식인 identity 속성이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="12437-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="12437-111">특정 주문의 배송 주소를 지정하기 위한 Order 형식 속성으로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="12437-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="12437-112">`OnModelCreating`를 사용 하 여는 `OwnsOne` ShippingAddress 속성이 Order 형식의 소유 엔터티 인지를 지정 하는 메서드.</span><span class="sxs-lookup"><span data-stu-id="12437-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

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

<span data-ttu-id="12437-113">ShippingAddress 속성이 Order type private 인 경우의 문자열 버전을 사용할 수 있습니다는 `OwnsOne` 메서드:</span><span class="sxs-lookup"><span data-stu-id="12437-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="12437-114">이 예에서는 사용 된 `OwnedAttribute` 같은 목표를 달성 하기:</span><span class="sxs-lookup"><span data-stu-id="12437-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

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

## <a name="implicit-keys"></a><span data-ttu-id="12437-115">암시적 키</span><span class="sxs-lookup"><span data-stu-id="12437-115">Implicit keys</span></span>

<span data-ttu-id="12437-116">EF 코어 2.0 및 2.1에만 참조 탐색 속성을 소유 형식 가리킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="12437-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="12437-117">소유 형식의 컬렉션을 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="12437-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="12437-118">이러한 참조는 항상 형식 소유자와 한 일 관계에 있는, 따라서 키 값을 직접 않아도 소유 합니다.</span><span class="sxs-lookup"><span data-stu-id="12437-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="12437-119">이전 예에서 StreetAddress 형식 키 속성을 정의 하 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="12437-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="12437-120">EF 코어에서 이러한 개체를 추적 하는 방법을 이해 하려면에서 기본 키로 만들어졌는지 생각 하면 도움이 될는 [속성 그림자](xref:core/modeling/shadow-properties) 소유 형식에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="12437-120">In order to understanding how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="12437-121">소유 형식의 인스턴스 키의 값은 소유자 인스턴스 키의 값과 동일 하 게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="12437-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="12437-122">소유 하는 테이블 분할을 사용 하는 형식 매핑</span><span class="sxs-lookup"><span data-stu-id="12437-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="12437-123">관계형 데이터베이스를 사용할 때 형식을 소유 하는 규칙에 따라 매핑됩니다 소유자와 같은 테이블.</span><span class="sxs-lookup"><span data-stu-id="12437-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="12437-124">이렇게 하려면 두 개의 테이블 분할: 일부 열에서 소유자의 데이터를 저장 하는 데 사용 됩니다 하 고 일부 열에서 소유한 엔터티의 데이터를 저장 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="12437-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="12437-125">이것이 테이블 분할 이라고 하는 일반적인 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="12437-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="12437-126">수 있는 저장 된 테이블 분할 유형을 소유 하 고 사용 어떻게 복합 형식에 EF6 매우 유사 합니다.</span><span class="sxs-lookup"><span data-stu-id="12437-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="12437-127">규칙에 따라 EF 코어 패턴을 소유한 엔터티 형식의 속성에 대 한 데이터베이스 열에 이름을 _EntityProperty_OwnedEntityProperty_합니다.</span><span class="sxs-lookup"><span data-stu-id="12437-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="12437-128">따라서 StreetAddress 속성 ShippingAddress_Street 및 ShippingAddress_City 이름을 Orders 테이블에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="12437-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="12437-129">추가할 수 있습니다는 `HasColumnName` 열의 이름을 바꿀 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="12437-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="12437-130">매핑을 것 경우 StreetAddress은 공용 속성</span><span class="sxs-lookup"><span data-stu-id="12437-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="12437-131">여러 소유 형식에서 같은.NET 형식 공유</span><span class="sxs-lookup"><span data-stu-id="12437-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="12437-132">소유한 엔터티 형식을 수 있습니다 다른 소유한 엔터티 형식과 같은.NET 형식의 따라서.NET 유형을 소유 유형을 식별 하기에 충분 한 수 없게 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="12437-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="12437-133">소유자 로부터 소유 엔터티를 가리키는 속성은 이러한 경우에는 _탐색 정의_ 소유 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="12437-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="12437-134">EF 코어의 관점에서 정의 탐색에는.NET 형식 함께 형식 id의 일부입니다.</span><span class="sxs-lookup"><span data-stu-id="12437-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="12437-135">예를 들어 다음과 같은 클래스의 ShippingAddress 및 BillingAddress 조치가 모두는 동일한 유형의.NET StreetAddress:</span><span class="sxs-lookup"><span data-stu-id="12437-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="12437-136">EF 코어에서 이러한 개체의 추적 된 인스턴스를 구분 하는 방법을 이해 하기 위해 정의 탐색을 소유자의 키 값과 함께 인스턴스 키의 일부 및 소유 형식의.NET 형식을 수 생각 하면 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="12437-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="12437-137">중첩 소유 형식</span><span class="sxs-lookup"><span data-stu-id="12437-137">Nested owned types</span></span>

<span data-ttu-id="12437-138">이 예에서 OrderDetails BillingAddress 및 두 StreetAddress 형식인 ShippingAddress 소유 합니다.</span><span class="sxs-lookup"><span data-stu-id="12437-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="12437-139">다음은 주문 유형을 OrderDetails는 소유 합니다.</span><span class="sxs-lookup"><span data-stu-id="12437-139">Then OrderDetails is owned by the Order type.</span></span>

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

<span data-ttu-id="12437-140">에 연결할 수는 `OwnsOne` fluent 매핑에서이 모델을 구성 하는 메서드:</span><span class="sxs-lookup"><span data-stu-id="12437-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="12437-141">사용 하 여 동일한 작업을 수행할 수 `OwnedAttribute` OrderDetails와 StreetAdress 합니다.</span><span class="sxs-lookup"><span data-stu-id="12437-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="12437-142">중첩 된 소유 형식 외에도 소유 형식 일반 엔터티를 참조할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="12437-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="12437-143">다음 예제에서는 국가 일반 (즉, 소유 되지 않은-) 엔터티입니다.</span><span class="sxs-lookup"><span data-stu-id="12437-143">In the following example, Country is a regular (i.e. non-owned) entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="12437-144">이 기능은 EF6 소유한 엔터티 형식 복합 형식 외에도 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="12437-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="12437-145">별도 테이블에는 형식을 소유 하 고 저장</span><span class="sxs-lookup"><span data-stu-id="12437-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="12437-146">또한 EF6 복합 형식과 달리 소유 형식 소유자 로부터 별도 테이블에 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="12437-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="12437-147">소유 형식 소유자와 같은 테이블에 매핑하는 규칙을 재정의 하기 위해 호출 하면 `ToTable` 하 고 다른 테이블 이름을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="12437-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="12437-148">다음 예제에서는 OrderDetails와 그 두 주소에 매핑됩니다 별도 테이블에서 순서:</span><span class="sxs-lookup"><span data-stu-id="12437-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a><span data-ttu-id="12437-149">소유 형식 쿼리</span><span class="sxs-lookup"><span data-stu-id="12437-149">Querying owned types</span></span>

<span data-ttu-id="12437-150">소유자를 쿼리할 때 소유된 형식은 기본적으로 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="12437-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="12437-151">사용할 필요는 없습니다는 `Include` 메서드를 소유 형식이 별도 테이블에 저장 하는 경우에 마찬가지입니다.</span><span class="sxs-lookup"><span data-stu-id="12437-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="12437-152">다음 쿼리에서 앞에서 설명한 것 모델에 따라, 순서, OrderDetails 및 데이터베이스에서 보류 중인 모든 주문에 대해 두 개의 소유 StreeAddresses 끌어오고:</span><span class="sxs-lookup"><span data-stu-id="12437-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreeAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="12437-153">제한 사항</span><span class="sxs-lookup"><span data-stu-id="12437-153">Limitations</span></span>

<span data-ttu-id="12437-154">다음은 몇 가지 제한 사항이 소유한 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="12437-154">Here are some limitations of owned entity types.</span></span> <span data-ttu-id="12437-155">기본 형식 작업 방법 소유 하는 경우 이러한 제한 사항 중 일부는 항목도 있지만 몇 가지 이후 릴리스를 제거 하 고 싶습니다 지정 시간 제한:</span><span class="sxs-lookup"><span data-stu-id="12437-155">Some of these limitations are fundamental to how owned types work, but some others are point-in-time restrictions that we would like to remove in future releases:</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="12437-156">현재 단점</span><span class="sxs-lookup"><span data-stu-id="12437-156">Current shortcomings</span></span>
- <span data-ttu-id="12437-157">소유 형식의 상속은 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="12437-157">Inheritance of owned types is not supported</span></span>
- <span data-ttu-id="12437-158">컬렉션 탐색 속성에 의해 소유 형식에서 지정한 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="12437-158">Owned types cannot be pointed at by a collection navigation property</span></span>
- <span data-ttu-id="12437-159">테이블에서 분할을 사용 하므로 기본 다른 테이블에 명시적으로 매핑된 하지 않는 한 다음과 같은 제한 사항이 방향이 소유 합니다.</span><span class="sxs-lookup"><span data-stu-id="12437-159">Since they use table splitting by default owned types also have the following restrictions unless explicitly mapped to a different table:</span></span>
   - <span data-ttu-id="12437-160">파생된 형식에 의해 소유할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="12437-160">They cannot be owned by a derived type</span></span>
   - <span data-ttu-id="12437-161">정의 하는 탐색 속성을 설정할 수 없습니다 null로 (즉, 소유 하 고 동일한 테이블에서 유형은 항상 필요)</span><span class="sxs-lookup"><span data-stu-id="12437-161">The defining navigation property cannot be set to null (i.e. owned types on the same table are always required)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="12437-162">디자인에 따라 제한 사항</span><span class="sxs-lookup"><span data-stu-id="12437-162">By-design restrictions</span></span>
- <span data-ttu-id="12437-163">만들 수 없습니다는 `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="12437-163">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="12437-164">호출할 수 없습니다 `Entity<T>()` 에 소유 형식의 `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="12437-164">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
