---
title: "하위 삭제-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: e1cb194d7c7472af59eb44fe2a084fa16c40c186
ms.sourcegitcommit: 3b21a7fdeddc7b3c70d9b7777b72bef61f59216c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/22/2018
---
# <a name="cascade-delete"></a><span data-ttu-id="26525-102">하위 삭제</span><span class="sxs-lookup"><span data-stu-id="26525-102">Cascade Delete</span></span>

<span data-ttu-id="26525-103">모두 삭제 한 행을 관련 된 행의 삭제를 자동으로 트리거하도록의 삭제를 허용 하는 특징을 설명 하기 위해 데이터베이스 용어에서 주로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="26525-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="26525-104">EF 코어 delete 동작도 적용 밀접 하 게 관련 된 개념은 하나의 자식 엔터티 관계 상위 되었을 때 자동으로 삭제할 어렵다는-일반적으로 "고아 파일 삭제" 이라고이 i입니다.</span><span class="sxs-lookup"><span data-stu-id="26525-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this i commonly known as "deleting orphans".</span></span>

<span data-ttu-id="26525-105">EF 코어는 몇 가지 다른 삭제 동작을 구현 하 고 개별 관계의 delete 동작의 구성을 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="26525-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="26525-106">EF 코어 [requiredness 관계의]에 따라 각 관계에 대 한 유용한 기본값 삭제 동작을 자동으로 구성 하는 규칙을 구현 (../modeling/relationships.md#required-and-optional-relationships)입니다.</span><span class="sxs-lookup"><span data-stu-id="26525-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship] (../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="26525-107">동작 삭제</span><span class="sxs-lookup"><span data-stu-id="26525-107">Delete behaviors</span></span>
<span data-ttu-id="26525-108">삭제 동작에 정의 된는 *DeleteBehavior* 열거자 입력 하 고에 전달 될 수는 *OnDelete* fluent API를 제어 하는지 여부를 사용자/부모 엔터티 또는의 비활성화할 삭제는 엔터티에도 종속/자식 관계에 종속/자식 엔터티 부작용 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="26525-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="26525-109">세 가지 작업 EF는 사용자/부모 엔터티가 삭제 되거나 자식에 대 한 관계를 끊는 때 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="26525-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="26525-110">자식/종속 항목을 삭제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="26525-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="26525-111">어린이 외래 키 값을 설정할 수 있습니다 null로</span><span class="sxs-lookup"><span data-stu-id="26525-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="26525-112">자식이 변경 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="26525-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="26525-113">EF 핵심 모델에서 구성 된 delete 동작 EF 코어를 사용 하 여 주 엔터티가 삭제 되 고 종속 엔터티가 (즉, 추적 된 종속 파일)의 메모리에 로드 된 경우에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="26525-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (i.e. for tracked dependents).</span></span> <span data-ttu-id="26525-114">해당 cascade 동작 컨텍스트에서 추적 중이지 않은 데이터를 확인 하기 위해 데이터베이스에서 설치 프로그램에 적용 되는 데 필요한 동작에 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="26525-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="26525-115">EF 코어를 사용 하 여 데이터베이스를 만드는 경우이 cascade 동작이 있습니다 설치 됩니다.</span><span class="sxs-lookup"><span data-stu-id="26525-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="26525-116">위의 두 번째 작업에 대 한 외래 키 값을 null로 설정지 않습니다 외래 키에서 null을 허용 하는 경우에 유효 합니다.</span><span class="sxs-lookup"><span data-stu-id="26525-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="26525-117">(Nullable이 아닌 외래 키가 필수 관계 같음.) 이러한 경우 EF 코어 외래 키 속성이 될 때 변경 내용을 데이터베이스에 유지할 수 없는 때문에 예외가 throw 됩니다 SaveChanges를 호출할 때까지 null로 표시 되어 있습니다를 추적 합니다.</span><span class="sxs-lookup"><span data-stu-id="26525-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="26525-118">이것은 데이터베이스에서 제약 조건 위반을 얻는 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="26525-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="26525-119">4 개 delete 되어 동작은 아래 표에 나와 있습니다.</span><span class="sxs-lookup"><span data-stu-id="26525-119">There are four delete behaviors, as listed in the tables below.</span></span> <span data-ttu-id="26525-120">선택적 관계 (nullable 외래 키)에 대 한 것 _은_ 그 결과 다음과 같은 효과가 null 외래 키 값을 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="26525-120">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="26525-121">동작 이름</span><span class="sxs-lookup"><span data-stu-id="26525-121">Behavior Name</span></span> | <span data-ttu-id="26525-122">종속/자식 메모리에 대 한 영향</span><span class="sxs-lookup"><span data-stu-id="26525-122">Effect on dependent/child in memory</span></span> | <span data-ttu-id="26525-123">종속/자식 데이터베이스에 대 한 영향</span><span class="sxs-lookup"><span data-stu-id="26525-123">Effect on dependent/child in database</span></span>
|-|-|-
| <span data-ttu-id="26525-124">**Cascade**</span><span class="sxs-lookup"><span data-stu-id="26525-124">**Cascade**</span></span> | <span data-ttu-id="26525-125">엔터티 삭제</span><span class="sxs-lookup"><span data-stu-id="26525-125">Entities are deleted</span></span> | <span data-ttu-id="26525-126">엔터티 삭제</span><span class="sxs-lookup"><span data-stu-id="26525-126">Entities are deleted</span></span>
| <span data-ttu-id="26525-127">**ClientSetNull** (기본값)</span><span class="sxs-lookup"><span data-stu-id="26525-127">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="26525-128">외래 키 속성은 null로</span><span class="sxs-lookup"><span data-stu-id="26525-128">Foreign key properties are set to null</span></span> | <span data-ttu-id="26525-129">없음</span><span class="sxs-lookup"><span data-stu-id="26525-129">None</span></span>
| <span data-ttu-id="26525-130">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="26525-130">**SetNull**</span></span> | <span data-ttu-id="26525-131">외래 키 속성은 null로</span><span class="sxs-lookup"><span data-stu-id="26525-131">Foreign key properties are set to null</span></span> | <span data-ttu-id="26525-132">외래 키 속성은 null로</span><span class="sxs-lookup"><span data-stu-id="26525-132">Foreign key properties are set to null</span></span>
| <span data-ttu-id="26525-133">**제한**</span><span class="sxs-lookup"><span data-stu-id="26525-133">**Restrict**</span></span> | <span data-ttu-id="26525-134">없음</span><span class="sxs-lookup"><span data-stu-id="26525-134">None</span></span> | <span data-ttu-id="26525-135">없음</span><span class="sxs-lookup"><span data-stu-id="26525-135">None</span></span>

<span data-ttu-id="26525-136">필요한 관계 (nullable이 아닌 외래 키)에 대 한 _하지_ 그 결과 다음과 같은 효과가 null 외래 키 값을 저장할 수:</span><span class="sxs-lookup"><span data-stu-id="26525-136">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="26525-137">동작 이름</span><span class="sxs-lookup"><span data-stu-id="26525-137">Behavior Name</span></span> | <span data-ttu-id="26525-138">종속/자식 메모리에 대 한 영향</span><span class="sxs-lookup"><span data-stu-id="26525-138">Effect on dependent/child in memory</span></span> | <span data-ttu-id="26525-139">종속/자식 데이터베이스에 대 한 영향</span><span class="sxs-lookup"><span data-stu-id="26525-139">Effect on dependent/child in database</span></span>
|-|-|-
| <span data-ttu-id="26525-140">**Cascade** (기본값)</span><span class="sxs-lookup"><span data-stu-id="26525-140">**Cascade** (Default)</span></span> | <span data-ttu-id="26525-141">엔터티 삭제</span><span class="sxs-lookup"><span data-stu-id="26525-141">Entities are deleted</span></span> | <span data-ttu-id="26525-142">엔터티 삭제</span><span class="sxs-lookup"><span data-stu-id="26525-142">Entities are deleted</span></span>
| <span data-ttu-id="26525-143">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="26525-143">**ClientSetNull**</span></span> | <span data-ttu-id="26525-144">SaveChanges를 발생 시킵니다.</span><span class="sxs-lookup"><span data-stu-id="26525-144">SaveChanges throws</span></span> | <span data-ttu-id="26525-145">없음</span><span class="sxs-lookup"><span data-stu-id="26525-145">None</span></span>
| <span data-ttu-id="26525-146">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="26525-146">**SetNull**</span></span> | <span data-ttu-id="26525-147">SaveChanges를 발생 시킵니다.</span><span class="sxs-lookup"><span data-stu-id="26525-147">SaveChanges throws</span></span> | <span data-ttu-id="26525-148">SaveChanges를 발생 시킵니다.</span><span class="sxs-lookup"><span data-stu-id="26525-148">SaveChanges throws</span></span>
| <span data-ttu-id="26525-149">**제한**</span><span class="sxs-lookup"><span data-stu-id="26525-149">**Restrict**</span></span> | <span data-ttu-id="26525-150">없음</span><span class="sxs-lookup"><span data-stu-id="26525-150">None</span></span> | <span data-ttu-id="26525-151">없음</span><span class="sxs-lookup"><span data-stu-id="26525-151">None</span></span>

<span data-ttu-id="26525-152">위의 표에 *None* 제약 조건 위반이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="26525-152">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="26525-153">예를 들어 주/자식 엔터티 삭제 표시 되지만 아무 작업도 수행 종속/자식의 외래 키를 변경 하려면 다음 데이터베이스 가능성이를 throw 합니다 SaveChanges 외부 제약 조건 위반 합니다.</span><span class="sxs-lookup"><span data-stu-id="26525-153">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="26525-154">상위 수준:</span><span class="sxs-lookup"><span data-stu-id="26525-154">At a high level:</span></span>
* <span data-ttu-id="26525-155">부모 없이 존재할 수 없는 엔터티 있고 EF 자식을 자동으로 삭제 하기 위한 처리 하기를 원하는 다음 사용 하 여 *Cascade*합니다.</span><span class="sxs-lookup"><span data-stu-id="26525-155">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="26525-156">부모 일반적으로 확인 없이 존재할 수 없는 엔터티 필요한 관계를 활용 *Cascade* 값이 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="26525-156">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="26525-157">수도 부모가 없을 수 있는 엔터티가 있고 EF, 외래 키 무효화 처리 하기를 원하는 다음 사용 하 여 *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="26525-157">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="26525-158">부모 일반적으로 확인 없이 존재할 수 있는 엔터티 선택적 관계를 사용 하 여 *ClientSetNull* 값이 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="26525-158">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="26525-159">데이터베이스에도 자식 외래 키에 null 값을 전파 하려고 할 경우 자식 엔터티 로드 되지 않습니다, 사용 하 여 *SetNull*합니다.</span><span class="sxs-lookup"><span data-stu-id="26525-159">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="26525-160">그러나 note 데이터베이스는이 지원 해야 않으며 다른 제한이 발생할 수 있습니다 이와 같은 데이터베이스를 구성 하는 실제로 사용 하면이 옵션으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="26525-160">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="26525-161">이 인해 *SetNull* 기본값이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="26525-161">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="26525-162">EF 핵심을 적이 자동으로 엔터티를 삭제 하거나 자동으로 외래 키 null 다음 사용 하지 않으려면 *Restrict*합니다.</span><span class="sxs-lookup"><span data-stu-id="26525-162">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="26525-163">이 위해서는 코드 유지는 자식 엔터티 및 해당 외래 키 값 동기화 수동으로 참고 그렇지 않은 경우 제약 조건 예외가 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="26525-163">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="26525-164">달리 EF6, EF 코어의 영향이 발생 하지 않으므로 즉시 않고 대신 SaveChanges를 호출할 때에 합니다.</span><span class="sxs-lookup"><span data-stu-id="26525-164">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="26525-165">**EF 코어 2.0의 변경 내용:** 이전 릴리스에서 *Restrict* 로 설정 해야 추적 된 종속 엔터티에 외래 키 속성 (옵션)로 인해 null 및 기본 선택적 관계에 대 한 동작을 삭제 했습니다.</span><span class="sxs-lookup"><span data-stu-id="26525-165">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="26525-166">EF 코어 2.0에서는 *ClientSetNull* 선택적 관계에 대 한 기본값을 문제가 있고 해당 동작을 표시 하기 위해 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="26525-166">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="26525-167">동작은 *Restrict* 종속 엔터티가에 예기치 않은 결과가 사용 해야 조정 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="26525-167">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="26525-168">엔터티 삭제 예</span><span class="sxs-lookup"><span data-stu-id="26525-168">Entity deletion examples</span></span>

<span data-ttu-id="26525-169">아래 코드의 일부인는 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) 다운로드 한 실행 수입니다.</span><span class="sxs-lookup"><span data-stu-id="26525-169">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="26525-170">어떤 일이 생기 선택 및 필수 관계에 대 한 각 삭제 동작에 대 한 부모 엔터티가 삭제 될 때 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="26525-170">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="26525-171">상황을 이해 하려면 각 변형을 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="26525-171">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="26525-172">필수 또는 선택적 관계와 DeleteBehavior.Cascade</span><span class="sxs-lookup"><span data-stu-id="26525-172">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="26525-173">블로그 삭제로 표시</span><span class="sxs-lookup"><span data-stu-id="26525-173">Blog is marked as Deleted</span></span>
* <span data-ttu-id="26525-174">단계적 SaveChanges 될 때까지 발생 하지 않으므로 이후 처음 게시물 Unchanged 유지</span><span class="sxs-lookup"><span data-stu-id="26525-174">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="26525-175">SaveChanges 보냅니다 삭제와에 대 한 종속 항목/하위 (게시) 다음 사용자/부모 (블로그)</span><span class="sxs-lookup"><span data-stu-id="26525-175">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="26525-176">저장 한 후 모든 엔터티는 이제 데이터베이스에서 삭제 한 후 분리</span><span class="sxs-lookup"><span data-stu-id="26525-176">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="26525-177">DeleteBehavior.ClientSetNull 또는 필수 관계와 DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="26525-177">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="26525-178">블로그 삭제로 표시</span><span class="sxs-lookup"><span data-stu-id="26525-178">Blog is marked as Deleted</span></span>
* <span data-ttu-id="26525-179">단계적 SaveChanges 될 때까지 발생 하지 않으므로 이후 처음 게시물 Unchanged 유지</span><span class="sxs-lookup"><span data-stu-id="26525-179">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="26525-180">SaveChanges FK post null로 설정 하려고 시도 하지만 외래 키에서 null을 허용 하기 때문에이 작업이 실패 하면</span><span class="sxs-lookup"><span data-stu-id="26525-180">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="26525-181">DeleteBehavior.ClientSetNull 또는 선택적 관계와 DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="26525-181">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="26525-182">블로그 삭제로 표시</span><span class="sxs-lookup"><span data-stu-id="26525-182">Blog is marked as Deleted</span></span>
* <span data-ttu-id="26525-183">단계적 SaveChanges 될 때까지 발생 하지 않으므로 이후 처음 게시물 Unchanged 유지</span><span class="sxs-lookup"><span data-stu-id="26525-183">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="26525-184">SaveChanges 시도 사용자/부모 (블로그)를 삭제 하기 전에 두 종속 항목/하위 (게시물) FK을 null로 설정</span><span class="sxs-lookup"><span data-stu-id="26525-184">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="26525-185">저장 한 후 사용자/부모 (블로그)는 삭제 되지만 종속 항목/하위 (게시)를 여전히 추적</span><span class="sxs-lookup"><span data-stu-id="26525-185">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="26525-186">추적 된 종속 항목/하위 (게시물) 이제 null 외래 키 값이 있는 않으며 삭제 된 사용자/부모 (블로그)에 대 한 참조의 삭제 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="26525-186">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="26525-187">필수 또는 선택적 관계와 DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="26525-187">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="26525-188">블로그 삭제로 표시</span><span class="sxs-lookup"><span data-stu-id="26525-188">Blog is marked as Deleted</span></span>
* <span data-ttu-id="26525-189">단계적 SaveChanges 될 때까지 발생 하지 않으므로 이후 처음 게시물 Unchanged 유지</span><span class="sxs-lookup"><span data-stu-id="26525-189">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="26525-190">이후 *Restrict* 자동으로 null로 외래 키를 설정 하는 EF 지시 null이 아닌 및 저장 하지 않고 SaveChanges를 발생 시킵니다.</span><span class="sxs-lookup"><span data-stu-id="26525-190">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="26525-191">삭제 고아 예</span><span class="sxs-lookup"><span data-stu-id="26525-191">Delete orphans examples</span></span>

<span data-ttu-id="26525-192">아래 코드의 일부인는 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) 하는 실행 다운로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="26525-192">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded an run.</span></span> <span data-ttu-id="26525-193">샘플 하는 경우 선택 사항이 고 필수 관계에 대 한 각 삭제 동작에 대 한 부모/보안 주체 및 해당 자식/종속 항목 간의 관계를 끊는 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="26525-193">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="26525-194">이 예제에서는 사용자/부모 (블로그)의 컬렉션 탐색 속성에서 종속 항목/하위 (게시)를 제거 하 여 관계를 끊는 합니다.</span><span class="sxs-lookup"><span data-stu-id="26525-194">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="26525-195">그러나 동작은 동일한 사용자/부모에 종속/자식 참조 대신 않게 닫히면 될 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="26525-195">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="26525-196">상황을 이해 하려면 각 변형을 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="26525-196">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="26525-197">필수 또는 선택적 관계와 DeleteBehavior.Cascade</span><span class="sxs-lookup"><span data-stu-id="26525-197">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="26525-198">Null로 표시 되어야 FK 발생 관계 비활성화할 게시물 Modified로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="26525-198">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="26525-199">외래 키에서 null을 허용 하는 경우 다음의 실제 값은 변경 되지 null로 표시 되어 있지만</span><span class="sxs-lookup"><span data-stu-id="26525-199">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="26525-200">SaveChanges 종속 항목/자식 (게시)에 대 한 삭제를 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="26525-200">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="26525-201">저장 한 후 종속 항목/하위 (게시물)는 이제 데이터베이스에서 삭제 한 후 분리</span><span class="sxs-lookup"><span data-stu-id="26525-201">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="26525-202">DeleteBehavior.ClientSetNull 또는 필수 관계와 DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="26525-202">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="26525-203">Null로 표시 되어야 FK 발생 관계 비활성화할 게시물 Modified로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="26525-203">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="26525-204">외래 키에서 null을 허용 하는 경우 다음의 실제 값은 변경 되지 null로 표시 되어 있지만</span><span class="sxs-lookup"><span data-stu-id="26525-204">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="26525-205">SaveChanges FK post null로 설정 하려고 시도 하지만 외래 키에서 null을 허용 하기 때문에이 작업이 실패 하면</span><span class="sxs-lookup"><span data-stu-id="26525-205">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="26525-206">DeleteBehavior.ClientSetNull 또는 선택적 관계와 DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="26525-206">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="26525-207">Null로 표시 되어야 FK 발생 관계 비활성화할 게시물 Modified로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="26525-207">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="26525-208">외래 키에서 null을 허용 하는 경우 다음의 실제 값은 변경 되지 null로 표시 되어 있지만</span><span class="sxs-lookup"><span data-stu-id="26525-208">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="26525-209">SaveChanges를 null로 두 종속 항목/하위 (게시물) FK 설정</span><span class="sxs-lookup"><span data-stu-id="26525-209">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="26525-210">저장 한 후 종속 항목/하위 (게시물) 이제 null 외래 키 값이 있는 및 삭제 된 사용자/부모 (블로그)에 대 한 참조는 제거 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="26525-210">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="26525-211">필수 또는 선택적 관계와 DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="26525-211">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="26525-212">Null로 표시 되어야 FK 발생 관계 비활성화할 게시물 Modified로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="26525-212">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="26525-213">외래 키에서 null을 허용 하는 경우 다음의 실제 값은 변경 되지 null로 표시 되어 있지만</span><span class="sxs-lookup"><span data-stu-id="26525-213">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="26525-214">이후 *Restrict* 자동으로 null로 외래 키를 설정 하는 EF 지시 null이 아닌 및 저장 하지 않고 SaveChanges를 발생 시킵니다.</span><span class="sxs-lookup"><span data-stu-id="26525-214">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="26525-215">추적 되지 않은 엔터티에도 종속</span><span class="sxs-lookup"><span data-stu-id="26525-215">Cascading to untracked entities</span></span>

<span data-ttu-id="26525-216">호출 하는 경우 *SaveChanges*, cascade 규칙 삭제는 컨텍스트에 의해 추적 되는 엔터티에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="26525-216">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="26525-217">이러한 모든 상황은 위에 표시 된 예에서는 이기 때문에 SQL 사용자/부모 (블로그)와 모든 종속 항목/하위 (게시)을 모두 삭제 하도록 생성:</span><span class="sxs-lookup"><span data-stu-id="26525-217">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="26525-218">경우에에서 주 서버를 로드-예를 들어 때 쿼리가 만들어지는 없이 블로그에 대 한 프로그램 `Include(b => b.Posts)` 게시물-포함 하도록 한 다음 SaveChanges 생성 되 고 주/부모를 삭제 하는 SQL:</span><span class="sxs-lookup"><span data-stu-id="26525-218">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="26525-219">종속 항목/하위 (게시물) 데이터베이스에 해당 cascade 동작을 구성 하는 경우에 삭제 됩니다.</span><span class="sxs-lookup"><span data-stu-id="26525-219">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="26525-220">EF를 사용 하 여 데이터베이스를 만드는 경우이 cascade 동작이 있습니다 설치 됩니다.</span><span class="sxs-lookup"><span data-stu-id="26525-220">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
