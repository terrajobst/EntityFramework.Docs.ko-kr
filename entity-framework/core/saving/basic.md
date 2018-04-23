---
title: 기본 저장-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: deead323301dc4a0ee0748b4536ddff4596b99e6
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2018
---
# <a name="basic-save"></a><span data-ttu-id="77e22-102">기본 저장</span><span class="sxs-lookup"><span data-stu-id="77e22-102">Basic Save</span></span>

<span data-ttu-id="77e22-103">추가, 수정 및 사용자 컨텍스트 및 엔터티 클래스를 사용 하 여 데이터를 제거 하는 방법에 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="77e22-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="77e22-104">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77e22-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="77e22-105">데이터 추가</span><span class="sxs-lookup"><span data-stu-id="77e22-105">Adding Data</span></span>

<span data-ttu-id="77e22-106">사용 하 여는 *DbSet.Add* 메서드를 엔터티 클래스의 새 인스턴스를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="77e22-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="77e22-107">호출 하는 경우 데이터베이스에 삽입 될 데이터 *SaveChanges*합니다.</span><span class="sxs-lookup"><span data-stu-id="77e22-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="77e22-108">에 설명 된 대로에 전달 되는 엔터티의 전체 그래프에 사용 하는 추가, 연결, 및 Update 메서드는 [관련 데이터](related-data.md) 섹션.</span><span class="sxs-lookup"><span data-stu-id="77e22-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="77e22-109">또는 단일 엔터티의 상태를 설정 하는 EntityEntry.State 속성을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77e22-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="77e22-110">예를 들어, `context.Entry(blog).State = EntityState.Modified`을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="77e22-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="77e22-111">데이터 업데이트</span><span class="sxs-lookup"><span data-stu-id="77e22-111">Updating Data</span></span>

<span data-ttu-id="77e22-112">EF는 변경 내용을 컨텍스트에 의해 추적 되는 기존 엔터티를 자동으로 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="77e22-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="77e22-113">여기에 부하/쿼리할 데이터베이스에서 엔터티 및 엔터티 이전에 추가 하 여 데이터베이스에 저장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="77e22-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="77e22-114">속성에 할당 된 값을 수정 하 고 호출 *SaveChanges*합니다.</span><span class="sxs-lookup"><span data-stu-id="77e22-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="77e22-115">데이터 삭제</span><span class="sxs-lookup"><span data-stu-id="77e22-115">Deleting Data</span></span>

<span data-ttu-id="77e22-116">사용 하 여는 *DbSet.Remove* 메서드를 엔터티 클래스의 인스턴스를 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="77e22-116">Use the *DbSet.Remove* method to delete instances of you entity classes.</span></span>

<span data-ttu-id="77e22-117">삭제 하는 동안 됩니다 엔터티가 데이터베이스에 이미 있으면 *SaveChanges*합니다.</span><span class="sxs-lookup"><span data-stu-id="77e22-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="77e22-118">엔터티 (예: 것은 추적 추가) 데이터베이스에 아직 저장 되지 않은 경우 컨텍스트를 제거 하 고 더 이상 됩니다 때 삽입 *SaveChanges* 호출 됩니다.</span><span class="sxs-lookup"><span data-stu-id="77e22-118">If the entity has not yet been saved to the database (i.e. it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="77e22-119">단일 SaveChanges에 여러 작업</span><span class="sxs-lookup"><span data-stu-id="77e22-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="77e22-120">단일 호출으로 여러 개의 추가/업데이트/제거 작업을 결합할 수 *SaveChanges*합니다.</span><span class="sxs-lookup"><span data-stu-id="77e22-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="77e22-121">대부분의 데이터베이스 공급자에 대 한 *SaveChanges* 가 트랜잭션 큐입니다.</span><span class="sxs-lookup"><span data-stu-id="77e22-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="77e22-122">즉, 모든 작업이 성공 하거나 실패 하 고 작업 적용 되지 것입니다 왼쪽 부분적으로.</span><span class="sxs-lookup"><span data-stu-id="77e22-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
