---
title: 소유한 엔터티 형식-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: fe7e07b8bd483fb3f9b672ee78ef7541f06a21a4
ms.sourcegitcommit: e66745c9f91258b2cacf5ff263141be3cba4b09e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/06/2019
ms.locfileid: "54058775"
---
# <a name="owned-entity-types"></a><span data-ttu-id="f7c49-102">소유 된 엔터티 형식</span><span class="sxs-lookup"><span data-stu-id="f7c49-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="f7c49-103">이 기능은 EF Core 2.0의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="f7c49-104">EF Core 모델 엔터티 형식을 다른 엔터티 형식의 탐색 속성에만 나타날 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="f7c49-105">이러한 이라고 _소유한 엔터티 형식_합니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-105">These are called _owned entity types_.</span></span> <span data-ttu-id="f7c49-106">소유 된 엔터티 형식이 포함 된 엔터티는 해당 _소유자_합니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="f7c49-107">명시적으로 구성</span><span class="sxs-lookup"><span data-stu-id="f7c49-107">Explicit configuration</span></span>

<span data-ttu-id="f7c49-108">형식에에서 포함 되지 않습니다 EF Core에서 모델 규칙에 따라 엔터티를 소유 합니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="f7c49-109">사용할 수는 `OwnsOne` 에서 메서드 `OnModelCreating` 주석을 사용 하 여 형식 또는 `OwnedAttribute` (EF Core 2.1의 새로운) 유형을 소유 된 형식으로 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="f7c49-110">이 예제에서는 `StreetAddress` id 속성이 없는 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-110">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="f7c49-111">특정 주문의 배송 주소를 지정하기 위한 Order 형식 속성으로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="f7c49-112">사용할 수 있습니다는 `OwnedAttribute` 다른 엔터티 형식에서 참조 하는 경우 소유 엔터티로 처리 하려면:</span><span class="sxs-lookup"><span data-stu-id="f7c49-112">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="f7c49-113">것도 사용할 수는 `OwnsOne` 에서 메서드 `OnModelCreating` 지정 하는 합니다 `ShippingAddress` 소유한 엔터티의 속성이 `Order` 엔터티 형식 하 고 필요한 경우 추가 패싯을 구성 하 합니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-113">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="f7c49-114">경우는 `ShippingAddress` 속성은 private 합니다 `Order` 종류의 문자열 버전을 사용할 수 있습니다는 `OwnsOne` 메서드:</span><span class="sxs-lookup"><span data-stu-id="f7c49-114">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="f7c49-115">참조 된 [전체 샘플 프로젝트](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) 자세한 컨텍스트.</span><span class="sxs-lookup"><span data-stu-id="f7c49-115">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span> 

## <a name="implicit-keys"></a><span data-ttu-id="f7c49-116">암시적 키</span><span class="sxs-lookup"><span data-stu-id="f7c49-116">Implicit keys</span></span>

<span data-ttu-id="f7c49-117">소유 된 형식의 구성 `OwnsOne` 참조 탐색을 통해 검색 하거나 소유자를 사용 하 여 한 일 관계가 항상, 따라서 외래 키 값은 고유 키 값을 직접 필요 하지 않은 합니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-117">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="f7c49-118">이전 예에서 `StreetAddress` 형식 키 속성을 정의할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-118">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="f7c49-119">EF Core에서 이러한 개체를 추적 하는 방법을 이해 하려면 기본 키로 만들어졌는지 생각 하면 편리 것을 [섀도 속성](xref:core/modeling/shadow-properties) 소유 된 형식에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-119">In order to understand how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="f7c49-120">소유 된 형식의 인스턴스 키의 값은 소유자 인스턴스 키의 값과 동일 하 게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-120">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="f7c49-121">소유 된 형식의 컬렉션</span><span class="sxs-lookup"><span data-stu-id="f7c49-121">Collections of owned types</span></span>

>[!NOTE]
> <span data-ttu-id="f7c49-122">이 기능은 EF Core 2.2의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-122">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="f7c49-123">소유 된 형식의 컬렉션을 구성 하려면 `OwnsMany` 에서 사용할 `OnModelCreating`합니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-123">To configure a collection of owned types `OwnsMany` should be used in `OnModelCreating`.</span></span> <span data-ttu-id="f7c49-124">그러나 기본 키 구성 되지 않습니다 기본적 있으므로 명시적으로 지정할 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-124">However the primary key will not be configured by convention, so it need to be specified explicitly.</span></span> <span data-ttu-id="f7c49-125">일반적으로 외래 키 소유자 및 섀도 상태에 있을 수도 있습니다 고유 속성을 추가로 통합 하는 엔터티의 이러한 형식에 대 한 복합 키를 사용 하는 것:</span><span class="sxs-lookup"><span data-stu-id="f7c49-125">It is common to use a complex key for these type of entities incorporating the foreign key to the owner and an additional unique property that can also be in shadow state:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="f7c49-126">테이블 분할 형식과 소유 하는 매핑</span><span class="sxs-lookup"><span data-stu-id="f7c49-126">Mapping owned types with table splitting</span></span>

<span data-ttu-id="f7c49-127">관계형을 사용 하는 경우 데이터베이스에 규칙 참조에 의해 소유 형식은 소유자와 동일한 테이블에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-127">When using relational databases, by convention reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="f7c49-128">이렇게 하려면 두 개의 테이블 분할: 소유자의 데이터를 저장할 일부 열을 사용 하 고 소유 엔터티의 데이터를 저장할 일부 열이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-128">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="f7c49-129">이 기능은 일반적인 테이블 분할 이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-129">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="f7c49-130">테이블 분할 사용 하 여 저장 된 형식의 수를 소유한 같은 방식으로 사용할 방법을 복합 형식에는 EF6에서 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-130">Owned types stored with table splitting can be used similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="f7c49-131">규칙에 따라 EF Core는 열 이름을 지정 하는 데이터베이스 패턴을 소유 된 엔터티 형식의 속성에 대 한 _Navigation_OwnedEntityProperty_합니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-131">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="f7c49-132">따라서는 `StreetAddress` 속성 'ShippingAddress_Street' 및 'ShippingAddress_City' 이름의 '주문' 테이블에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-132">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="f7c49-133">추가할 수 있습니다는 `HasColumnName` 메서드 열 이름을 바꾸려면:</span><span class="sxs-lookup"><span data-stu-id="f7c49-133">You can append the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="f7c49-134">여러 소유 형식에서 동일한.NET 형식 공유</span><span class="sxs-lookup"><span data-stu-id="f7c49-134">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="f7c49-135">소유 된 엔터티 형식 수 동일한.NET 형식을 다른 소유 된 엔터티 형식으로 하므로.NET 형식 만으로는 소유 된 형식 식별 하 합니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-135">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="f7c49-136">소유자에 게에서 소유 된 엔터티를 가리키는 속성은 이러한 경우에는 _탐색을 정의_ 소유 된 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-136">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="f7c49-137">EF Core의 관점에서 정의 탐색에는.NET 형식 함께 형식 id의 일부입니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-137">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="f7c49-138">다음 클래스의 예를 들어 `ShippingAddress` 하 고 `BillingAddress` 둘 다 동일한.NET 형식의 `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="f7c49-138">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="f7c49-139">EF Core에서 이러한 개체의 추적 된 인스턴스를 구분 하는 방법을 이해 하기 위해 정의 탐색 소유 된 형식의.NET 형식과 소유자의 키 값과 함께 인스턴스 키의 일부 바뀌었기는 생각 하는 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-139">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="f7c49-140">중첩 된 소유 된 형식</span><span class="sxs-lookup"><span data-stu-id="f7c49-140">Nested owned types</span></span>

<span data-ttu-id="f7c49-141">이 예제의 `OrderDetails` 소유 `BillingAddress` 하 고 `ShippingAddress`, 둘 `StreetAddress` 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-141">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="f7c49-142">그리고 `OrderDetails`는 `DetailedOrder` 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-142">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="f7c49-143">중첩 된 소유 된 형식 외에도 소유 된 형식에는 일반 엔터티를 참조할 수 일 수도 있습니다 소유자 또는 다른 엔터티를 소유 된 엔터티는 종속 측의으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-143">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="f7c49-144">이 기능은 EF6에서 복합 형식 외에도 소유 된 엔터티 형식을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="f7c49-145">연결할 수는 `OwnsOne` fluent 호출이 모델을 구성 하는 메서드:</span><span class="sxs-lookup"><span data-stu-id="f7c49-145">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="f7c49-146">사용 하 여 동일한 작업 목표를 달성할 수 이기도 `OwnedAttribute` 둘 다에서 `OrderDetails` 고 `StreetAdress`입니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-146">It is also possible to achieve the same thing using `OwnedAttribute` on both `OrderDetails` and `StreetAdress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="f7c49-147">소유 된 형식은 별도 테이블에 저장</span><span class="sxs-lookup"><span data-stu-id="f7c49-147">Storing owned types in separate tables</span></span>

<span data-ttu-id="f7c49-148">또한 EF6 복잡 한 형식과 달리 소유 된 형식 소유자의 별도 테이블에 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-148">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="f7c49-149">소유 된 형식 소유자와 동일한 테이블에 매핑하는 규칙을 재정의 하기 위해 호출할 수 있습니다 단순히 `ToTable` 하 고 다른 테이블 이름을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-149">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="f7c49-150">다음 예제에서는 매핑될 `OrderDetails` 및에서 별도 테이블에는 두 주소 `DetailedOrder`:</span><span class="sxs-lookup"><span data-stu-id="f7c49-150">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a><span data-ttu-id="f7c49-151">소유 된 형식 쿼리</span><span class="sxs-lookup"><span data-stu-id="f7c49-151">Querying owned types</span></span>

<span data-ttu-id="f7c49-152">소유자를 쿼리할 때 소유된 형식은 기본적으로 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-152">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="f7c49-153">사용 하는 데 필요한 것을 `Include` 메서드, 소유 된 형식은 별도 테이블에 저장 된 경우에 합니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-153">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="f7c49-154">설명 하기 전에 모델을 기반으로 다음을 가져오도록 `Order`, `OrderDetails` 소유한 두 `StreetAddresses` 데이터베이스에서:</span><span class="sxs-lookup"><span data-stu-id="f7c49-154">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="f7c49-155">제한 사항</span><span class="sxs-lookup"><span data-stu-id="f7c49-155">Limitations</span></span>

<span data-ttu-id="f7c49-156">이러한 제한 사항 중 일부는 어떻게 소유한 엔터티 형식 작업에 기본적인 되지만 일부는 우리가 할 수 있습니다 나중에 릴리스를 제거 하는 제한 사항:</span><span class="sxs-lookup"><span data-stu-id="f7c49-156">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="f7c49-157">디자인에 따라 제한</span><span class="sxs-lookup"><span data-stu-id="f7c49-157">By-design restrictions</span></span>
- <span data-ttu-id="f7c49-158">만들 수 없습니다는 `DbSet<T>` 소유 된 형식에 대 한</span><span class="sxs-lookup"><span data-stu-id="f7c49-158">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="f7c49-159">호출할 수 없습니다 `Entity<T>()` 소유 된 형식에서 사용 하 여 `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="f7c49-159">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="f7c49-160">현재 단점</span><span class="sxs-lookup"><span data-stu-id="f7c49-160">Current shortcomings</span></span>
- <span data-ttu-id="f7c49-161">포함 하는 상속 계층 구조에서 소유 하는 엔터티 형식은 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-161">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="f7c49-162">참조 탐색 소유한 엔터티 형식 소유자에 게에서 별도 테이블에 명시적으로 매핑되는 경우가 아니면 null 일 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-162">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="f7c49-163">(이 소유 된 엔터티 형식을 사용 하 여 구현할 수 없는 값 개체에 대 한 잘 알려진 시나리오) 하는 여러 소유자가 소유 된 엔터티 형식의 인스턴스를 공유할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-163">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="f7c49-164">이전 버전의 단점</span><span class="sxs-lookup"><span data-stu-id="f7c49-164">Shortcomings in previous versions</span></span>
- <span data-ttu-id="f7c49-165">EF Core 2.0 탐색 소유한 엔터티 소유자 계층에서 별도 테이블에 매핑됩니다는 명시적으로 하지 않는 한 엔터티 형식이 파생된 엔터티 형식에서 선언할 수 없습니다 소유 합니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-165">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="f7c49-166">EF Core 2.1에서이 제한이 제거 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-166">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="f7c49-167">EF Core 2.0 및 2.1만 참조 소유 된 형식에 탐색 지원 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-167">In EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="f7c49-168">EF Core 2.2에이 제한이 제거 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="f7c49-168">This limitation has been removed in EF Core 2.2</span></span>
