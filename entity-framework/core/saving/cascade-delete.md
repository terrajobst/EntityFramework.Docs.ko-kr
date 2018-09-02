---
title: 하위 삭제 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: afe00ddb1b487c6b1b2ea42708c9967a57cea04b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995244"
---
# <a name="cascade-delete"></a><span data-ttu-id="fbbef-102">하위 삭제</span><span class="sxs-lookup"><span data-stu-id="fbbef-102">Cascade Delete</span></span>

<span data-ttu-id="fbbef-103">하위 삭제는 데이터베이스 용어에서 행을 삭제하여 관련 행 삭제를 자동으로 트리거하도록 하는 특성을 설명하는 데 일반적으로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="fbbef-104">EF Core에서 삭제 동작을 설명하는 밀접하게 관련된 개념으로 부모와의 관계가 끊어진 경우 자식 엔터티의 자동 삭제가 있습니다. 일반적으로 “고아 삭제”라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this is commonly known as "deleting orphans".</span></span>

<span data-ttu-id="fbbef-105">EF Core는 여러 다른 삭제 동작을 구현하며 개별 관계의 삭제 동작 구성을 허용합니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="fbbef-106">또한 EF Core는 [관계의 필수 여부](../modeling/relationships.md#required-and-optional-relationships)에 따라 각 관계에 대해 유용한 기본 삭제 동작을 자동으로 구성하는 규칙을 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship](../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="fbbef-107">삭제 동작</span><span class="sxs-lookup"><span data-stu-id="fbbef-107">Delete behaviors</span></span>
<span data-ttu-id="fbbef-108">삭제 동작은 *DeleteBehavior* 열거자 유형에 정의되며 *OnDelete* 흐름 API에 전달하여 주/부모 엔터티의 삭제 또는 종속/자식 엔터티에 대한 관계 끊기가 종속/자식 엔터티에 부작용을 일으키는지 여부를 제어할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="fbbef-109">주/부모 엔터티가 삭제되거나 자식에 대한 관계가 끊어지는 경우 EF에서 다음 세 가지 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="fbbef-110">자식/종속을 삭제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="fbbef-111">자식의 외래 키 값을 null로 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="fbbef-112">자식을 변경하지 않고 그대로 유지합니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="fbbef-113">EF Core 모델에 구성된 삭제 동작은 주 엔터티가 EF Core를 사용하여 삭제되고 종속 엔터티가 메모리에 로드되는 경우(즉, 추적된 종속 항목의 경우)에만 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (that is, for tracked dependents).</span></span> <span data-ttu-id="fbbef-114">컨텍스트에서 추적되지 않는 데이터에 필요한 작업이 적용되도록 데이터베이스에서 해당 하위 삭제 동작을 설정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="fbbef-115">EF Core를 사용하여 데이터베이스를 만드는 경우에는 이 하위 삭제 동작이 자동으로 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="fbbef-116">위의 두 번째 작업에서 외래 키가 null 허용이 아닌 경우 외래 키 값을 null로 설정할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="fbbef-117">null을 허용하지 않는 외래 키는 필수 관계와 동일합니다. 이러한 경우 EF Core는 SaveChanges가 호출될 때까지 외래 키 속성이 null로 표시되었음을 추적하며, 이때 변경 내용을 데이터베이스에 유지할 수 없기 때문에 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="fbbef-118">이는 데이터베이스에서 제약 조건 위반을 가져오는 것과 유사합니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="fbbef-119">아래 표에 나열된 대로 네 가지 삭제 동작이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-119">There are four delete behaviors, as listed in the tables below.</span></span>

### <a name="optional-relationships"></a><span data-ttu-id="fbbef-120">선택적 관계</span><span class="sxs-lookup"><span data-stu-id="fbbef-120">Optional relationships</span></span>
<span data-ttu-id="fbbef-121">선택적 관계(null 허용 외래 키)인 경우 null 외래 키 값을 저장하여 다음과 같은 효과가 발생하도록 할 수 ‘있습니다’.</span><span class="sxs-lookup"><span data-stu-id="fbbef-121">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="fbbef-122">동작 이름</span><span class="sxs-lookup"><span data-stu-id="fbbef-122">Behavior Name</span></span>               | <span data-ttu-id="fbbef-123">메모리의 종속/자식에 대한 영향</span><span class="sxs-lookup"><span data-stu-id="fbbef-123">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="fbbef-124">데이터베이스의 종속/자식에 대한 영향</span><span class="sxs-lookup"><span data-stu-id="fbbef-124">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="fbbef-125">**Cascade**</span><span class="sxs-lookup"><span data-stu-id="fbbef-125">**Cascade**</span></span>                 | <span data-ttu-id="fbbef-126">엔터티가 삭제됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-126">Entities are deleted</span></span>                   | <span data-ttu-id="fbbef-127">엔터티가 삭제됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-127">Entities are deleted</span></span>                   |
| <span data-ttu-id="fbbef-128">**ClientSetNull**(기본값)</span><span class="sxs-lookup"><span data-stu-id="fbbef-128">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="fbbef-129">외래 키 속성이 null로 설정됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-129">Foreign key properties are set to null</span></span> | <span data-ttu-id="fbbef-130">없음</span><span class="sxs-lookup"><span data-stu-id="fbbef-130">None</span></span>                                   |
| <span data-ttu-id="fbbef-131">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="fbbef-131">**SetNull**</span></span>                 | <span data-ttu-id="fbbef-132">외래 키 속성이 null로 설정됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-132">Foreign key properties are set to null</span></span> | <span data-ttu-id="fbbef-133">외래 키 속성이 null로 설정됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-133">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="fbbef-134">**Restrict**</span><span class="sxs-lookup"><span data-stu-id="fbbef-134">**Restrict**</span></span>                | <span data-ttu-id="fbbef-135">없음</span><span class="sxs-lookup"><span data-stu-id="fbbef-135">None</span></span>                                   | <span data-ttu-id="fbbef-136">없음</span><span class="sxs-lookup"><span data-stu-id="fbbef-136">None</span></span>                                   |

### <a name="required-relationships"></a><span data-ttu-id="fbbef-137">필수 관계</span><span class="sxs-lookup"><span data-stu-id="fbbef-137">Required relationships</span></span>
<span data-ttu-id="fbbef-138">필수 관계(null 허용 외래 키)인 경우 null 외래 키 값을 저장하여 다음과 같은 효과가 발생하도록 할 수 ‘없습니다’.</span><span class="sxs-lookup"><span data-stu-id="fbbef-138">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="fbbef-139">동작 이름</span><span class="sxs-lookup"><span data-stu-id="fbbef-139">Behavior Name</span></span>         | <span data-ttu-id="fbbef-140">메모리의 종속/자식에 대한 영향</span><span class="sxs-lookup"><span data-stu-id="fbbef-140">Effect on dependent/child in memory</span></span> | <span data-ttu-id="fbbef-141">데이터베이스의 종속/자식에 대한 영향</span><span class="sxs-lookup"><span data-stu-id="fbbef-141">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="fbbef-142">**Cascade**(기본값)</span><span class="sxs-lookup"><span data-stu-id="fbbef-142">**Cascade** (Default)</span></span> | <span data-ttu-id="fbbef-143">엔터티가 삭제됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-143">Entities are deleted</span></span>                | <span data-ttu-id="fbbef-144">엔터티가 삭제됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-144">Entities are deleted</span></span>                  |
| <span data-ttu-id="fbbef-145">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="fbbef-145">**ClientSetNull**</span></span>     | <span data-ttu-id="fbbef-146">SaveChanges가 throw됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-146">SaveChanges throws</span></span>                  | <span data-ttu-id="fbbef-147">없음</span><span class="sxs-lookup"><span data-stu-id="fbbef-147">None</span></span>                                  |
| <span data-ttu-id="fbbef-148">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="fbbef-148">**SetNull**</span></span>           | <span data-ttu-id="fbbef-149">SaveChanges가 throw됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-149">SaveChanges throws</span></span>                  | <span data-ttu-id="fbbef-150">SaveChanges가 throw됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-150">SaveChanges throws</span></span>                    |
| <span data-ttu-id="fbbef-151">**Restrict**</span><span class="sxs-lookup"><span data-stu-id="fbbef-151">**Restrict**</span></span>          | <span data-ttu-id="fbbef-152">없음</span><span class="sxs-lookup"><span data-stu-id="fbbef-152">None</span></span>                                | <span data-ttu-id="fbbef-153">없음</span><span class="sxs-lookup"><span data-stu-id="fbbef-153">None</span></span>                                  |

<span data-ttu-id="fbbef-154">위의 표에서 ‘없음’은 제약 조건 위반을 발생시킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-154">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="fbbef-155">예를 들어 주/자식 엔터티가 삭제되었지만 종속/자식의 외래 키를 변경하는 작업을 수행하지 않으면 외래 제약 조건 위반으로 인해 데이터베이스에서 SaveChanges가 throw될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-155">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="fbbef-156">상위 수준에서 다음을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-156">At a high level:</span></span>
* <span data-ttu-id="fbbef-157">부모가 있어야 엔터티가 존재할 수 있는 경우 EF에서 자동으로 하위를 삭제할 수 있도록 하려면 *Cascade*를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-157">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="fbbef-158">부모가 있어야 존재할 수 있는 엔터티는 일반적으로 *Cascade*가 기본값인 필수 관계를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-158">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="fbbef-159">엔터티에 부모가 있거나 없을 수 있는 경우 EF에서 외래 키를 자동으로 무효화하도록 하려면 *ClientSetNull*을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-159">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="fbbef-160">부모가 없어야 존재할 수 있는 엔터티는 일반적으로 *ClientSetNull*이 기본값인 선택적 관계를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-160">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="fbbef-161">자식 엔터티가 로드되지 않은 경우에도 데이터베이스에서 null 값을 자식 외래 키로 전파하도록 하려면 *SetNull*을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-161">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="fbbef-162">그러나 데이터베이스에서 이를 지원해야 하며, 이렇게 데이터베이스를 구성하면 다른 제한 사항이 나타날 수 있으므로 실제로는 이 옵션이 비현실적인 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-162">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="fbbef-163">이로 인해 *SetNull*은 기본값이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-163">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="fbbef-164">EF Core에서 엔터티를 자동으로 삭제하거나 외래 키를 자동으로 무효화하도록 하지 않으려면 *Restrict*를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-164">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="fbbef-165">이 경우 코드에서 자식 엔터티 및 해당 외래 키 값을 수동으로 동기화된 상태로 유지하도록 해야 합니다. 그러지 않으면 제약 조건 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-165">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="fbbef-166">EF6와 달리 EF Core에서는 하위 삭제 효과가 즉시 발생하지 않고 SaveChanges가 호출될 때만 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-166">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="fbbef-167">**EF Core 2.0의 변경 내용:** 이전 릴리스에서는 *Restrict*로 인해 추적된 종속 엔터티의 선택적 외래 키 속성이 null로 설정되었으며, 선택적 관계에 대한 기본 삭제 동작이었습니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-167">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="fbbef-168">EF Core 2.0에서는 해당 동작을 나타내기 위해 *ClientSetNull*이 도입되었으며 선택적 관계의 기본값이 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-168">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="fbbef-169">종속 엔터티에 의도하지 않은 영향을 주지 않도록 *Restrict*의 동작이 조정되었습니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-169">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="fbbef-170">엔터티 삭제 예제</span><span class="sxs-lookup"><span data-stu-id="fbbef-170">Entity deletion examples</span></span>

<span data-ttu-id="fbbef-171">다음 코드는 다운로드하여 실행할 수 있는 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/)의 일부입니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-171">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="fbbef-172">이 샘플에서는 부모 엔터티가 삭제될 경우 선택적 관계와 필수 관계에 대한 각 삭제 동작에서 발생하는 상황을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-172">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="fbbef-173">각 변형을 살펴보고 어떤 상황이 발생하는지 파악해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-173">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="fbbef-174">필수 또는 선택적 관계의 DeleteBehavior.Cascade</span><span class="sxs-lookup"><span data-stu-id="fbbef-174">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="fbbef-175">블로그가 삭제됨으로 표시됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-175">Blog is marked as Deleted</span></span>
* <span data-ttu-id="fbbef-176">SaveChanges까지 하위 삭제가 수행되지 않으므로 게시물이 처음에 변경되지 않은 상태로 유지됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-176">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="fbbef-177">SaveChanges에서 두 종속/자식(게시물)에 대해 삭제를 전송한 다음, 주/부모(블로그)에 대해 삭제를 전송함</span><span class="sxs-lookup"><span data-stu-id="fbbef-177">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="fbbef-178">저장 후에는 모든 엔터티가 데이터베이스에서 삭제되었으므로 분리됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-178">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="fbbef-179">필수 관계의 DeleteBehavior.ClientSetNull 또는 DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="fbbef-179">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="fbbef-180">블로그가 삭제됨으로 표시됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-180">Blog is marked as Deleted</span></span>
* <span data-ttu-id="fbbef-181">SaveChanges까지 하위 삭제가 수행되지 않으므로 게시물이 처음에 변경되지 않은 상태로 유지됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-181">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="fbbef-182">SaveChanges에서 게시물 FK를 null로 설정하려고 시도하지만 FK가 null을 허용하지 않으므로 실패함</span><span class="sxs-lookup"><span data-stu-id="fbbef-182">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="fbbef-183">선택적 관계의 DeleteBehavior.ClientSetNull 또는 DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="fbbef-183">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="fbbef-184">블로그가 삭제됨으로 표시됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-184">Blog is marked as Deleted</span></span>
* <span data-ttu-id="fbbef-185">SaveChanges까지 하위 삭제가 수행되지 않으므로 게시물이 처음에 변경되지 않은 상태로 유지됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-185">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="fbbef-186">SaveChanges에서 주/부모(블로그)를 삭제하기 전에 두 종속/하위(게시물)의 FK를 null로 설정함</span><span class="sxs-lookup"><span data-stu-id="fbbef-186">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="fbbef-187">저장 후에는 주/부모(블로그)가 삭제되지만 종속/자식(게시물)은 계속 추적됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-187">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="fbbef-188">추적된 종속/자식(게시물)은 null FK 값을 갖고 삭제된 주/부모(블로그)에 대한 참조는 제거됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-188">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="fbbef-189">필수 또는 선택적 관계의 DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="fbbef-189">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="fbbef-190">블로그가 삭제됨으로 표시됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-190">Blog is marked as Deleted</span></span>
* <span data-ttu-id="fbbef-191">SaveChanges까지 하위 삭제가 수행되지 않으므로 게시물이 처음에 변경되지 않은 상태로 유지됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-191">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="fbbef-192">*Restrict*에서 FK를 null로 자동 설정하지 않도록 EF에 지시하기 때문에 null이 아닌 상태로 유지되며 저장하지 않고 SaveChanges가 throw됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-192">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="fbbef-193">고아 삭제 예제</span><span class="sxs-lookup"><span data-stu-id="fbbef-193">Delete orphans examples</span></span>

<span data-ttu-id="fbbef-194">다음 코드는 다운로드하여 실행할 수 있는 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/)의 일부입니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-194">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="fbbef-195">이 샘플에서는 부모/주와 자식/종속 같 관계가 끊어진 경우 선택적 관계와 필수 관계에 대한 각 삭제 동작에서 발생하는 상황을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-195">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="fbbef-196">이 예제에서는 주/부모(블로그)의 컬렉션 탐색 속성에서 종속/자식(게시물)을 제거하여 관계를 끊습니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-196">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="fbbef-197">그러나 주/부모에 대한 종속/자식의 참조가 무효화되는 경우에는 동작이 동일합니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-197">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="fbbef-198">각 변형을 살펴보고 어떤 상황이 발생하는지 파악해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-198">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="fbbef-199">필수 또는 선택적 관계의 DeleteBehavior.Cascade</span><span class="sxs-lookup"><span data-stu-id="fbbef-199">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="fbbef-200">관계를 끊으면 FK가 null로 표시되기 때문에 게시물이 수정됨으로 표시됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-200">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="fbbef-201">FK가 null을 허용하지 않으면 null로 표시되는 경우에도 실제 값은 변경되지 않음</span><span class="sxs-lookup"><span data-stu-id="fbbef-201">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="fbbef-202">SaveChanges는 종속/자식(게시물)에 대한 삭제를 전송함</span><span class="sxs-lookup"><span data-stu-id="fbbef-202">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="fbbef-203">저장 후에는 종속/자식(게시물)이 데이터베이스에서 삭제되었으므로 분리됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-203">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="fbbef-204">필수 관계의 DeleteBehavior.ClientSetNull 또는 DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="fbbef-204">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="fbbef-205">관계를 끊으면 FK가 null로 표시되기 때문에 게시물이 수정됨으로 표시됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-205">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="fbbef-206">FK가 null을 허용하지 않으면 null로 표시되는 경우에도 실제 값은 변경되지 않음</span><span class="sxs-lookup"><span data-stu-id="fbbef-206">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="fbbef-207">SaveChanges에서 게시물 FK를 null로 설정하려고 시도하지만 FK가 null을 허용하지 않으므로 실패함</span><span class="sxs-lookup"><span data-stu-id="fbbef-207">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="fbbef-208">선택적 관계의 DeleteBehavior.ClientSetNull 또는 DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="fbbef-208">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="fbbef-209">관계를 끊으면 FK가 null로 표시되기 때문에 게시물이 수정됨으로 표시됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-209">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="fbbef-210">FK가 null을 허용하지 않으면 null로 표시되는 경우에도 실제 값은 변경되지 않음</span><span class="sxs-lookup"><span data-stu-id="fbbef-210">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="fbbef-211">SaveChanges에서 두 종속/자식(게시물)의 FK를 null로 설정함</span><span class="sxs-lookup"><span data-stu-id="fbbef-211">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="fbbef-212">저장 후에는 종속/자식(게시물)이 null FK 값을 갖고 삭제된 주/부모(블로그)에 대한 참조가 제거됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-212">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="fbbef-213">필수 또는 선택적 관계의 DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="fbbef-213">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="fbbef-214">관계를 끊으면 FK가 null로 표시되기 때문에 게시물이 수정됨으로 표시됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-214">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="fbbef-215">FK가 null을 허용하지 않으면 null로 표시되는 경우에도 실제 값은 변경되지 않음</span><span class="sxs-lookup"><span data-stu-id="fbbef-215">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="fbbef-216">*Restrict*에서 FK를 null로 자동 설정하지 않도록 EF에 지시하기 때문에 null이 아닌 상태로 유지되며 저장하지 않고 SaveChanges가 throw됨</span><span class="sxs-lookup"><span data-stu-id="fbbef-216">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="fbbef-217">추적되지 않은 엔터티에 대한 하위 삭제</span><span class="sxs-lookup"><span data-stu-id="fbbef-217">Cascading to untracked entities</span></span>

<span data-ttu-id="fbbef-218">*SaveChanges*를 호출하면 하위 삭제 규칙이 컨텍스트에서 추적되는 모든 개체에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-218">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="fbbef-219">이 상황은 위에 표시된 모든 예제에서 발생하므로, 주/부모(블로그)와 모든 종속/자식(게시물)을 삭제하도록 SQL이 생성되었습니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-219">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="fbbef-220">`Include(b => b.Posts)`를 사용하지 않고 게시물도 포함하도록 블로그를 쿼리하는 경우처럼 주 엔터티만 로드되는 경우에는 SaveChanges에서 주/부모를 삭제하는 SQL만 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-220">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="fbbef-221">데이터베이스에서 해당 하위 삭제 동작을 구성한 경우에만 종속/자식(게시물)이 삭제됩니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-221">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="fbbef-222">EF를 사용하여 데이터베이스를 만드는 경우에는 이 하위 삭제 동작이 자동으로 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="fbbef-222">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
