---
title: 소유 엔터티 형식-EF Core
description: Entity Framework Core를 사용할 때 소유 엔터티 형식 또는 집계를 구성 하는 방법
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/owned-entities
ms.openlocfilehash: 7b6d1b3bccbfceb85f03a580ba03a45984d29c74
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824600"
---
# <a name="owned-entity-types"></a><span data-ttu-id="cdc7f-103">소유한 엔터티 형식</span><span class="sxs-lookup"><span data-stu-id="cdc7f-103">Owned Entity Types</span></span>

> [!NOTE]
> <span data-ttu-id="cdc7f-104">이 기능은 EF Core 2.0의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-104">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="cdc7f-105">EF Core를 사용 하면 다른 엔터티 형식의 탐색 속성에만 나타날 수 있는 엔터티 형식을 모델링할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-105">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="cdc7f-106">이러한 엔터티를 _소유 된 엔터티 형식_이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-106">These are called _owned entity types_.</span></span> <span data-ttu-id="cdc7f-107">소유 된 엔터티 형식을 포함 하는 엔터티는 해당 _소유자_입니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-107">The entity containing an owned entity type is its _owner_.</span></span>

<span data-ttu-id="cdc7f-108">소유 된 엔터티는 기본적으로 소유자의 일부 이며이를 제외 하 고는 [집계](https://martinfowler.com/bliki/DDD_Aggregate.html)와 개념적으로 유사 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-108">Owned entities are essentially a part of the owner and cannot exist without it, they are conceptually similar to [aggregates](https://martinfowler.com/bliki/DDD_Aggregate.html).</span></span> <span data-ttu-id="cdc7f-109">즉, 소유 된 형식은 소유자와 관계의 종속 측에 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-109">This means that the owned type is by definition on the dependent side of the relationship with the owner.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="cdc7f-110">명시적 구성</span><span class="sxs-lookup"><span data-stu-id="cdc7f-110">Explicit configuration</span></span>

<span data-ttu-id="cdc7f-111">소유 된 엔터티 형식은 규칙에 따라 모델의 EF Core에 포함 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-111">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="cdc7f-112">`OnModelCreating`에서 `OwnsOne` 메서드를 사용 하거나 `OwnedAttribute`를 사용 하 여 형식에 주석을 달 수 있습니다 (EF Core 2.1의 새로운 형식).</span><span class="sxs-lookup"><span data-stu-id="cdc7f-112">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="cdc7f-113">이 예제에서 `StreetAddress`는 identity 속성이 없는 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-113">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="cdc7f-114">특정 주문의 배송 주소를 지정하기 위한 Order 형식 속성으로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-114">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="cdc7f-115">`OwnedAttribute`를 사용 하 여 다른 엔터티 형식에서 참조 되는 경우 소유 엔터티로 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-115">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="cdc7f-116">`OnModelCreating`에서 `OwnsOne` 메서드를 사용 하 여 `ShippingAddress` 속성이 `Order` 엔터티 형식의 소유 된 엔터티 임을 지정 하 고 필요한 경우 추가 패싯을 구성할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-116">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="cdc7f-117">`ShippingAddress` 속성이 `Order` 형식에서 private 이면 `OwnsOne` 메서드의 문자열 버전을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-117">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="cdc7f-118">자세한 컨텍스트는 [전체 샘플 프로젝트](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) 를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-118">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span>

## <a name="implicit-keys"></a><span data-ttu-id="cdc7f-119">암시적 키</span><span class="sxs-lookup"><span data-stu-id="cdc7f-119">Implicit keys</span></span>

<span data-ttu-id="cdc7f-120">`OwnsOne`로 구성 되거나 참조 탐색을 통해 검색 된 소유 형식은 항상 소유자와 일 대 일 관계를 가지 므로 외래 키 값이 고유 하므로 고유한 키 값이 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-120">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="cdc7f-121">이전 예제에서 `StreetAddress` 형식은 키 속성을 정의할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-121">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="cdc7f-122">EF Core에서 이러한 개체를 추적 하는 방법을 이해 하기 위해 소유 된 형식에 대 한 [그림자 속성](xref:core/modeling/shadow-properties) 으로 기본 키가 생성 되는 것을 알고 있으면 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-122">In order to understand how EF Core tracks these objects, it is useful to know that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="cdc7f-123">소유 된 형식의 인스턴스 키 값은 소유자 인스턴스 키의 값과 동일 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-123">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="cdc7f-124">소유 된 형식의 컬렉션</span><span class="sxs-lookup"><span data-stu-id="cdc7f-124">Collections of owned types</span></span>

> [!NOTE]
> <span data-ttu-id="cdc7f-125">이 기능은 EF Core 2.2의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-125">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="cdc7f-126">소유 된 형식의 컬렉션을 구성 하려면 `OnModelCreating`에서 `OwnsMany`를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-126">To configure a collection of owned types use `OwnsMany` in `OnModelCreating`.</span></span>

<span data-ttu-id="cdc7f-127">소유 된 형식에는 기본 키가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-127">Owned types need a primary key.</span></span> <span data-ttu-id="cdc7f-128">.NET 형식에 적합 한 후보 속성이 없으면 하나를 만들 수 EF Core.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-128">If there are no good candidates properties on the .NET type, EF Core can try to create one.</span></span> <span data-ttu-id="cdc7f-129">그러나 소유 된 형식이 컬렉션을 통해 정의 되는 경우에는 `OwnsOne`에 대 한 것과 같이 소유 된 인스턴스의 소유자 및 기본 키로 외래 키 역할을 수행 하기 위해 섀도 속성을 만드는 것 만으로는 충분 하지 않습니다. 각각에 대해 소유 된 형식 인스턴스가 여러 개 있을 수 있습니다. 소유자 이므로 소유자의 키로는 소유 된 각 인스턴스에 대 한 고유 id를 제공 하기에 충분 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-129">However, when owned types are defined through a collection, it isn't enough to just create a shadow property to act as both the foreign key into the owner and the primary key of the owned instance, as we do for `OwnsOne`: there can be multiple owned type instances for each owner, and hence the key of the owner isn't enough to provide a unique identity for each owned instance.</span></span>

<span data-ttu-id="cdc7f-130">이에 대 한 가장 간단한 두 가지 솔루션은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-130">The two most straightforward solutions to this are:</span></span>

- <span data-ttu-id="cdc7f-131">소유자를 가리키는 외래 키와 관계 없이 새 속성에 서로게이트 기본 키를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-131">Defining a surrogate primary key on a new property independent of the foreign key that points to the owner.</span></span> <span data-ttu-id="cdc7f-132">포함 된 값은 모든 소유자에서 고유 해야 합니다 (예: 부모 {1}에 자식 {1}있는 경우 부모 {2}에 자식 {1}포함 될 수 없음). 따라서 값에 내재 된 의미가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-132">The contained values would need to be unique across all owners (e.g. if Parent {1} has Child {1}, then Parent {2} cannot have Child {1}), so the value doesn't have any inherent meaning.</span></span> <span data-ttu-id="cdc7f-133">외래 키가 기본 키의 일부가 아니므로 해당 값을 변경할 수 있으므로 한 부모에서 다른 부모로 자식 항목을 이동할 수 있습니다. 그러나 일반적으로 집계 의미 체계를 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-133">Since the foreign key is not part of the primary key its values can be changed, so you could move a child from one parent to another one, however this usually goes against aggregate semantics.</span></span>
- <span data-ttu-id="cdc7f-134">외래 키 및 추가 속성을 복합 키로 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-134">Using the foreign key and an additional property as a composite key.</span></span> <span data-ttu-id="cdc7f-135">이제 추가 속성 값은 지정 된 부모에 대해서만 고유 해야 합니다. 따라서 부모 {1}에 자식 {1,1} 있는 경우 부모 {2}에도 자식 {2,1}포함 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-135">The additional property value now only needs to be unique for a given parent (so if Parent {1} has Child {1,1} then Parent {2} can still have Child {2,1}).</span></span> <span data-ttu-id="cdc7f-136">기본 키의 외래 키 부분을 만들면 소유자와 소유 된 엔터티 간의 관계가 변경할 수 없게 되 고 집계 의미 체계가 향상 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-136">By making the foreign key part of the primary key the relationship between the owner and the owned entity becomes immutable and reflects aggregate semantics better.</span></span> <span data-ttu-id="cdc7f-137">이 EF Core 기본적으로 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-137">This is what EF Core does by default.</span></span>

<span data-ttu-id="cdc7f-138">이 예제에서는 `Distributor` 클래스를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-138">In this example we'll use the `Distributor` class:</span></span>

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

<span data-ttu-id="cdc7f-139">기본적으로 `ShippingCenters` 탐색 속성을 통해 참조 하는 소유 된 형식에 사용 되는 기본 키는 `("DistributorId", "Id")` 됩니다. 여기서 `"DistributorId"`는 FK 이며 `"Id"`는 고유한 `int` 값입니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-139">By default the primary key used for the owned type referenced through the `ShippingCenters` navigation property will be `("DistributorId", "Id")` where `"DistributorId"` is the FK and `"Id"` is a unique `int` value.</span></span>

<span data-ttu-id="cdc7f-140">다른 PK 호출을 구성 하려면 다음을 `HasKey`합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-140">To configure a different PK call `HasKey`:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> <span data-ttu-id="cdc7f-141">EF Core 3.0 `WithOwner()` 메서드가 없으므로이 호출을 제거 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-141">Before EF Core 3.0 `WithOwner()` method didn't exist so this call should be removed.</span></span> <span data-ttu-id="cdc7f-142">또한 기본 키는 자동으로 검색 되지 않으므로 항상 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-142">Also the primary key was not discovered automatically so it always had be specified.</span></span>

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="cdc7f-143">테이블 분할을 사용 하 여 소유 된 형식 매핑</span><span class="sxs-lookup"><span data-stu-id="cdc7f-143">Mapping owned types with table splitting</span></span>

<span data-ttu-id="cdc7f-144">관계형 데이터베이스를 사용 하는 경우 기본적으로 소유 하는 참조는 소유자와 동일한 테이블에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-144">When using relational databases, by default reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="cdc7f-145">이렇게 하려면 두 개의 테이블을 분할 해야 합니다. 일부 열은 소유자의 데이터를 저장 하는 데 사용 되 고 일부 열은 소유 된 엔터티의 데이터를 저장 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-145">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="cdc7f-146">이는 [테이블 분할](table-splitting.md)이라는 일반적인 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-146">This is a common feature known as [table splitting](table-splitting.md).</span></span>

<span data-ttu-id="cdc7f-147">기본적으로 EF Core는 패턴 _Navigation_OwnedEntityProperty_따라 소유 된 엔터티 형식의 속성에 대 한 데이터베이스 열 이름을로 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-147">By default, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="cdc7f-148">따라서 `StreetAddress` 속성은 이름이 ' ShippingAddress_Street '이 고 ' ShippingAddress_City ' 인 ' Orders ' 테이블에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-148">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="cdc7f-149">`HasColumnName` 메서드를 사용 하 여 해당 열의 이름을 바꿀 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-149">You can use the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

> [!NOTE]
> <span data-ttu-id="cdc7f-150">[Ignore](/dotnet/api/microsoft.entityframeworkcore.metadata.builders.ownednavigationbuilder.ignore) 와 같은 일반적인 엔터티 형식 구성 메서드는 대부분 동일한 방식으로 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-150">Most of the normal entity type configuration methods like [Ignore](/dotnet/api/microsoft.entityframeworkcore.metadata.builders.ownednavigationbuilder.ignore) can be called in the same way.</span></span>

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="cdc7f-151">여러 개의 소유 된 형식 간에 동일한 .NET 형식 공유</span><span class="sxs-lookup"><span data-stu-id="cdc7f-151">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="cdc7f-152">소유 된 엔터티 형식은 다른 소유 된 엔터티 형식과 동일한 .NET 형식일 수 있으므로 .NET 형식은 소유 된 형식을 식별 하기에 충분 하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-152">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="cdc7f-153">이러한 경우 소유자를 가리키는 속성은 소유 된 엔터티 형식을 정의 하는 _탐색_ 이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-153">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="cdc7f-154">EF Core 관점에서, 정의 탐색은 .NET 형식과 함께 형식의 id의 일부입니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-154">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>

<span data-ttu-id="cdc7f-155">예를 들어 다음 클래스 `ShippingAddress` 및 `BillingAddress` 모두 동일한 .NET 유형인 `StreetAddress`입니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-155">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="cdc7f-156">EF Core에서 이러한 개체의 추적 된 인스턴스를 구분 하는 방법을 이해 하기 위해 정의 하는 탐색이 소유자의 키 값과 소유 된 형식의 .NET 형식으로 인스턴스 키의 일부가 되는 것으로 생각 하는 것이 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-156">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="cdc7f-157">중첩 된 소유 형식</span><span class="sxs-lookup"><span data-stu-id="cdc7f-157">Nested owned types</span></span>

<span data-ttu-id="cdc7f-158">이 예에서는 `StreetAddress` 형식인 `BillingAddress` 및 `ShippingAddress`를 소유 `OrderDetails` 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-158">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="cdc7f-159">그리고 `OrderDetails`는 `DetailedOrder` 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-159">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="cdc7f-160">소유 된 형식에 대 한 각 탐색은 완전히 독립적으로 구성 된 별도의 엔터티 형식을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-160">Each navigation to an owned type defines a separate entity type with completely independent configuration.</span></span>

<span data-ttu-id="cdc7f-161">중첩 된 소유 형식 외에도 소유 된 형식은 정규 엔터티를 참조할 수 있으며 소유 된 엔터티가 종속 측에 있으면 소유자 이거나 다른 엔터티가 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-161">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="cdc7f-162">이 기능은 소유 하는 엔터티 형식을 EF6의 복합 형식과 분리 하 여 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-162">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="cdc7f-163">흐름 호출에 `OwnsOne` 메서드를 연결 하 여이 모델을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-163">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="cdc7f-164">소유자에서 뒤로 가리키기 탐색 속성을 구성 하는 데 사용 되는 `WithOwner` 호출을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-164">Notice the `WithOwner` call used to configure the navigation property pointing back at the owner.</span></span> <span data-ttu-id="cdc7f-165">소유권 관계의 일부가 아닌 소유자 엔터티 형식에 대 한 탐색을 구성 하려면 `WithOwner()` 인수 없이 호출 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-165">To configure a navigation to the owner entity type that's not part of the ownership relationship `WithOwner()` should be called without any arguments.</span></span>

<span data-ttu-id="cdc7f-166">`OrderDetails`와 `StreetAddress`에서 `OwnedAttribute`를 사용 하 여 결과를 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-166">It is possible to achieve the result using `OwnedAttribute` on both `OrderDetails` and `StreetAddress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="cdc7f-167">소유 형식을 별도의 테이블에 저장</span><span class="sxs-lookup"><span data-stu-id="cdc7f-167">Storing owned types in separate tables</span></span>

<span data-ttu-id="cdc7f-168">또한 EF6 복합 형식과 달리 소유 된 형식은 소유자와는 별도의 테이블에 저장 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-168">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="cdc7f-169">소유 된 형식을 소유자와 동일한 테이블에 매핑하는 규칙을 재정의 하려면 단순히 `ToTable`를 호출 하 고 다른 테이블 이름을 제공 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-169">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="cdc7f-170">다음 예에서는 `OrderDetails` 및 두 개의 주소를 `DetailedOrder`별도의 테이블에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-170">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

<span data-ttu-id="cdc7f-171">또한 `TableAttribute`를 사용 하 여이를 수행할 수 있지만, 소유 된 형식에 여러 개의 탐색이 있는 경우에는이 작업이 실패 합니다 .이 경우 여러 엔터티 형식이 동일한 테이블에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-171">It is also possible to use the `TableAttribute` to accomplish this, but note that this would fail if there are multiple navigations to the owned type since in that case multiple entity types would be mapped to the same table.</span></span>

## <a name="querying-owned-types"></a><span data-ttu-id="cdc7f-172">소유 형식 쿼리</span><span class="sxs-lookup"><span data-stu-id="cdc7f-172">Querying owned types</span></span>

<span data-ttu-id="cdc7f-173">소유자를 쿼리할 때 소유된 형식은 기본적으로 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-173">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="cdc7f-174">소유 된 형식이 별도의 테이블에 저장 된 경우에도 `Include` 메서드를 사용할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-174">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="cdc7f-175">앞에서 설명한 모델에 따라 다음 쿼리는 데이터베이스에서 `Order`, `OrderDetails` 및 소유 된 두 개의 `StreetAddresses`을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-175">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="cdc7f-176">제한 사항</span><span class="sxs-lookup"><span data-stu-id="cdc7f-176">Limitations</span></span>

<span data-ttu-id="cdc7f-177">이러한 제한 사항 중 일부는 소유 된 엔터티 형식이 작동 하는 방식에 대 한 기본 이지만 다른 일부는 이후 릴리스에서 제거할 수 있는 제한 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-177">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="cdc7f-178">디자인 제한 사항</span><span class="sxs-lookup"><span data-stu-id="cdc7f-178">By-design restrictions</span></span>

- <span data-ttu-id="cdc7f-179">소유 된 형식에 대 한 `DbSet<T>`를 만들 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-179">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="cdc7f-180">에서 소유 된 형식을 사용 하는 `Entity<T>()`를 호출할 수 없습니다 `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="cdc7f-180">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="cdc7f-181">현재 단점</span><span class="sxs-lookup"><span data-stu-id="cdc7f-181">Current shortcomings</span></span>

- <span data-ttu-id="cdc7f-182">소유 된 엔터티 형식에는 상속 계층을 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-182">Owned entity types cannot have inheritance hierarchies</span></span>
- <span data-ttu-id="cdc7f-183">소유 된 엔터티 형식에 대 한 참조 탐색은 소유자와 별도의 테이블에 명시적으로 매핑되지 않는 한 null 일 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-183">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="cdc7f-184">소유 된 엔터티 형식의 인스턴스는 여러 소유자가 공유할 수 없습니다 .이는 소유 된 엔터티 형식을 사용 하 여 구현할 수 없는 값 개체에 대 한 잘 알려진 시나리오입니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-184">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="cdc7f-185">이전 버전의 단점</span><span class="sxs-lookup"><span data-stu-id="cdc7f-185">Shortcomings in previous versions</span></span>

- <span data-ttu-id="cdc7f-186">EF Core 2.0에서 소유 된 엔터티는 소유자 계층 구조와 별도의 테이블에 명시적으로 매핑되어 있지 않는 한, 파생 엔터티 형식에서 소유 된 엔터티 형식에 대 한 탐색을 선언할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-186">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="cdc7f-187">EF Core 2.1에서이 제한이 제거 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-187">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="cdc7f-188">EF Core 2.0 및 2.1에서는 소유 된 형식에 대 한 참조 탐색만 지원 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-188">In EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="cdc7f-189">EF Core 2.2에서이 제한이 제거 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="cdc7f-189">This limitation has been removed in EF Core 2.2</span></span>
