---
title: "동시성 토큰-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a><span data-ttu-id="3d8b5-102">동시성 토큰</span><span class="sxs-lookup"><span data-stu-id="3d8b5-102">Concurrency Tokens</span></span>

<span data-ttu-id="3d8b5-103">속성이 동시성 토큰으로 구성 된 다른 사용자가 해당 레코드에 변경 내용을 저장할 때 데이터베이스의 해당 값을 수정 EF 확인 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-103">If a property is configured as a concurrency token then EF will check that no other user has modified that value in the database when saving changes to that record.</span></span> <span data-ttu-id="3d8b5-104">EF는 낙관적 동시성 패턴을 사용 하 여, 값을 변경 하지 않은 것으로 가정 하 고 데이터를 저장 하지만 값이 변경 발견 되 면 throw 하려고 것을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-104">EF uses an optimistic concurrency pattern, meaning it will assume the value has not changed and try to save the data, but throw if it finds the value has been changed.</span></span>

<span data-ttu-id="3d8b5-105">예를 들어 구성 해야 할 수 있습니다 `LastName` 에 `Person` 동시성 토큰 되도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-105">For example we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="3d8b5-106">즉, 한 사용자의 일부 변경 내용을 저장 하려고 하는 경우는 `Person`, 다른 사용자가을 변경 하지만 `LastName` 다음 예외가 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-106">This means that if one user tries to save some changes to a `Person`, but another user has changed the `LastName` then an exception will be thrown.</span></span> <span data-ttu-id="3d8b5-107">응용 프로그램이이 레코드의 변경 내용을 저장 하기 전에 동일한 실제 사용자는 여전히 나타냅니다 되도록 사용자를 입력할 수 있도록이 바람직 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-107">This may be desirable so that your application can prompt the user to ensure this record still represents the same actual person before saving their changes.</span></span>

> [!NOTE]
> <span data-ttu-id="3d8b5-108">이 페이지에는 동시성 토큰을 구성 하는 방법을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-108">This page documents how to configure concurrency tokens.</span></span> <span data-ttu-id="3d8b5-109">참조 [동시성 처리](../saving/concurrency.md) 응용 프로그램에서 낙관적 동시성을 사용 하는 방법에 대 한 예제입니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-109">See [Handling Concurrency](../saving/concurrency.md) for examples of how to use optimistic concurrency in your application.</span></span>

## <a name="how-concurrency-tokens-work-in-ef"></a><span data-ttu-id="3d8b5-110">동시성 토큰 EF에서 작동 하는 방법</span><span class="sxs-lookup"><span data-stu-id="3d8b5-110">How concurrency tokens work in EF</span></span>

<span data-ttu-id="3d8b5-111">데이터 저장소 업데이트 또는 삭제할 여전히 모든 레코드에 컨텍스트 데이터베이스에서 원래 데이터를 로드할 때 할당 된 동시성 토큰에 대 한 동일한 값이 있는지 확인 하 여 동시성 토큰을 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-111">Data stores can enforce concurrency tokens by checking that any record being updated or deleted still has the same value for the concurrency token that was assigned when the context originally loaded the data from the database.</span></span>

<span data-ttu-id="3d8b5-112">관계형 데이터베이스의 동시성 토큰을 포함 하 여이으로 달성 되는 예를 들어는 `WHERE` 모든 절 `UPDATE` 또는 `DELETE` 명령 및 영향을 받는 행의 번호를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-112">For example, relational databases achieve this by including the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` commands and checking the number of rows that were affected.</span></span> <span data-ttu-id="3d8b5-113">동시성 토큰 여전히 일치 하는 경우 행이 하나씩 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-113">If the concurrency token still matches then one row will be updated.</span></span> <span data-ttu-id="3d8b5-114">값은 데이터베이스에 변경 된 경우 아무 행도 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-114">If the value in the database has changed, then no rows are updated.</span></span>

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="conventions"></a><span data-ttu-id="3d8b5-115">규칙</span><span class="sxs-lookup"><span data-stu-id="3d8b5-115">Conventions</span></span>

<span data-ttu-id="3d8b5-116">규칙에 따라 속성이 동시성 토큰으로 구성 되지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-116">By convention, properties are never configured as concurrency tokens.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3d8b5-117">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="3d8b5-117">Data Annotations</span></span>

<span data-ttu-id="3d8b5-118">동시성 토큰으로 속성을 구성 하는 데이터 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-118">You can use the Data Annotations to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a><span data-ttu-id="3d8b5-119">Fluent API</span><span class="sxs-lookup"><span data-stu-id="3d8b5-119">Fluent API</span></span>

<span data-ttu-id="3d8b5-120">동시성 토큰으로 속성을 구성 하는 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-120">You can use the Fluent API to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a><span data-ttu-id="3d8b5-121">타임 스탬프/행 버전</span><span class="sxs-lookup"><span data-stu-id="3d8b5-121">Timestamp/row version</span></span>

<span data-ttu-id="3d8b5-122">타임 스탬프 행이 삽입 되거나 업데이트 될 때마다 데이터베이스에서 새 값을 생성 되는 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-122">A timestamp is a property where a new value is generated by the database every time a row is inserted or updated.</span></span> <span data-ttu-id="3d8b5-123">속성이는 동시성 토큰으로도 처리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-123">The property is also treated as a concurrency token.</span></span> <span data-ttu-id="3d8b5-124">이렇게 하면 다른 사용자가 데이터에 대 한 쿼리 한 이후 업데이트 하려고 하는 행을 수정 하는 경우 예외가 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-124">This ensures you will get an exception if anyone else has modified a row that you are trying to update since you queried for the data.</span></span>

<span data-ttu-id="3d8b5-125">사용 중인 데이터베이스 공급자가 어떻게 작업을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-125">How this is achieved is up to the database provider being used.</span></span> <span data-ttu-id="3d8b5-126">SQL Server에 대 한 타임 스탬프는 대개 사용에 *byte* 수 있는 속성으로 설치는 *ROWVERSION* 데이터베이스의 열입니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-126">For SQL Server, timestamp is usually used on a *byte[]* property, which will be setup as a *ROWVERSION* column in the database.</span></span>

### <a name="conventions"></a><span data-ttu-id="3d8b5-127">규칙</span><span class="sxs-lookup"><span data-stu-id="3d8b5-127">Conventions</span></span>

<span data-ttu-id="3d8b5-128">규칙에 따라 타임 스탬프로 속성이 구성 되지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-128">By convention, properties are never configured as timestamps.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="3d8b5-129">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="3d8b5-129">Data Annotations</span></span>

<span data-ttu-id="3d8b5-130">속성을 구성을 타임 스탬프 데이터 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-130">You can use Data Annotations to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a><span data-ttu-id="3d8b5-131">Fluent API</span><span class="sxs-lookup"><span data-stu-id="3d8b5-131">Fluent API</span></span>

<span data-ttu-id="3d8b5-132">속성을 구성을 타임 스탬프 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d8b5-132">You can use the Fluent API to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
