---
title: 동시성 충돌 처리 - EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: e050b17bfa31a4785161c700bc0355e83162b405
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993114"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="e8fb4-102">동시성 충돌 처리</span><span class="sxs-lookup"><span data-stu-id="e8fb4-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="e8fb4-103">이 페이지에서는 EF Core에서 동시성의 작동 방식과 응용 프로그램에서 동시성 충돌을 처리하는 방법에 대해 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="e8fb4-104">모델에서 동시성 토큰을 구성하는 방법에 대한 자세한 내용은 [동시성 토큰](xref:core/modeling/concurrency)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="e8fb4-105">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="e8fb4-106">‘데이터베이스 동시성’이란 여러 프로세스 또는 사용자가 동시에 데이터베이스에서 동일한 데이터를 액세스하거나 변경하는 상황을 의미합니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="e8fb4-107">‘동시성 제어’는 동시 변경 내용이 있을 경우 데이터 일관성을 보장하는 데 사용되는 특정 메커니즘을 의미합니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="e8fb4-108">EF Core는 ‘낙관적 동시성 제어’를 구현하여 여러 프로세스 또는 사용자가 동기화 또는 잠금의 오버헤드 없이 독립적으로 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="e8fb4-109">이상적인 상황에서는 이러한 변경 내용이 서로 방해가 되지 않으므로 성공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="e8fb4-110">최악의 경우 둘 이상의 프로세스에서 충돌하는 변경을 수행하려고 하여 하나만 성공합니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="e8fb4-111">EF Core에서 동시성 제어의 작동 방식</span><span class="sxs-lookup"><span data-stu-id="e8fb4-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="e8fb4-112">동시성 토큰으로 구성된 속성을 사용하여 낙관적 동시성 제어를 구현합니다. `SaveChanges` 중에 업데이트 또는 삭제 작업이 수행될 때마다 데이터베이스의 동시성 토큰 값이 EF Core에서 읽은 원래 값과 비교됩니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="e8fb4-113">값이 일치하면 작업이 완료됩니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="e8fb4-114">값이 일치하지 않으면 EF Core는 다른 사용자가 충돌하는 작업을 수행했다고 가정하고 현재 트랜잭션을 중단합니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="e8fb4-115">다른 사용자가 현재 작업과 충돌하는 작업을 수행한 경우를 ‘동시성 충돌’이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="e8fb4-116">데이터베이스 공급자는 동시성 토큰 값 비교를 구현해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="e8fb4-117">관계형 데이터베이스에서 EF Core는 `UPDATE` 또는 `DELETE` 문의 `WHERE` 절에 동시성 토큰 값 검사를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="e8fb4-118">문을 실행한 후 EF Core는 영향을 받은 행 수를 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="e8fb4-119">영향을 받은 행이 없는 경우 동시성 충돌이 감지되고 EF Core는 `DbUpdateConcurrencyException`을 throw합니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="e8fb4-120">예를 들어 `Person`의 `LastName`을 동시성 토큰으로 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="e8fb4-121">그러면 Person에 대한 업데이트 작업에서 `WHERE` 절에 동시성 검사를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="e8fb4-122">동시성 충돌 해결</span><span class="sxs-lookup"><span data-stu-id="e8fb4-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="e8fb4-123">이전 예제에서 한 사용자는 `Person`의 일부 변경 내용을 저장하려고 하는데 다른 사용자는 `LastName`을 이미 변경한 경우 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName`, then an exception will be thrown.</span></span>

<span data-ttu-id="e8fb4-124">이때 응용 프로그램은 충돌하는 변경 내용으로 인해 업데이트가 실패했음을 사용자에게 알리기만 하고 계속 진행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="e8fb4-125">그러나 이 레코드가 여전히 동일한 실제 사람을 나타내도록 하고 작업을 다시 시도하도록 사용자에게 메시지를 표시하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="e8fb4-126">이 프로세스는 ‘동시성 충돌 해결’의 예제입니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="e8fb4-127">동시성 충돌 해결에는 현재 `DbContext`에서 보류 중인 변경 내용을 데이터베이스의 값과 병합하는 작업이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="e8fb4-128">병합되는 값은 응용 프로그램에 따라 달라지며 사용자 입력으로 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="e8fb4-129">**세 가지 값 집합을 동시성 충돌 해결에 사용할 수 있습니다.**</span><span class="sxs-lookup"><span data-stu-id="e8fb4-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

* <span data-ttu-id="e8fb4-130">**현재 값**은 응용 프로그램이 데이터베이스에 쓰려고 시도하는 값입니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-130">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="e8fb4-131">**원래 값**은 편집을 수행하기 전에 데이터베이스에서 처음에 검색된 값입니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="e8fb4-132">**데이터베이스 값**은 현재 데이터베이스에 저장된 값입니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="e8fb4-133">동시성 충돌을 처리하는 일반적인 접근 방법은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="e8fb4-134">`SaveChanges` 중 `DbUpdateConcurrencyException`을 catch합니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="e8fb4-135">`DbUpdateConcurrencyException.Entries`를 사용하여 영향을 받는 엔터티에 대해 새로운 변경 내용 집합을 준비합니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="e8fb4-136">데이터베이스의 현재 값을 반영하도록 동시성 토큰의 원래 값을 새로 고칩니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="e8fb4-137">충돌이 발생하지 않을 때까지 프로세스를 다시 시도합니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="e8fb4-138">다음 예제에서는 `Person.FirstName` 및 `Person.LastName`을 동시성 토큰으로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-138">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency tokens.</span></span> <span data-ttu-id="e8fb4-139">저장할 값을 선택하는 응용 프로그램별 논리를 포함하는 위치에 `// TODO:` 주석이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8fb4-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
