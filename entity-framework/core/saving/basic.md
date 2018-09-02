---
title: 기본 저장 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 23e0e4611f642d59048fca5a808d0782b22caa1e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994803"
---
# <a name="basic-save"></a><span data-ttu-id="29db2-102">기본 저장</span><span class="sxs-lookup"><span data-stu-id="29db2-102">Basic Save</span></span>

<span data-ttu-id="29db2-103">컨텍스트 및 엔터티 클래스를 사용하여 데이터를 추가, 수정 및 제거하는 방법을 알아보세요.</span><span class="sxs-lookup"><span data-stu-id="29db2-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="29db2-104">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="29db2-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="29db2-105">데이터 추가</span><span class="sxs-lookup"><span data-stu-id="29db2-105">Adding Data</span></span>

<span data-ttu-id="29db2-106">*DbSet.Add* 메서드를 사용하여 엔터티 클래스의 새 인스턴스를 추가하세요.</span><span class="sxs-lookup"><span data-stu-id="29db2-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="29db2-107">*SaveChanges*를 호출할 때 데이터가 데이터베이스에 삽입됩니다.</span><span class="sxs-lookup"><span data-stu-id="29db2-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="29db2-108">Add, Attach 및 Update 메서드는 모두 [관련 데이터](related-data.md) 섹션에 설명된 대로 해당 메서드에 전달된 엔터티의 전체 그래프에서 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="29db2-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="29db2-109">또는 EntityEntry.State 속성을 사용하여 단일 엔터티의 상태를 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="29db2-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="29db2-110">예를 들어, `context.Entry(blog).State = EntityState.Modified`을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="29db2-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="29db2-111">데이터 업데이트</span><span class="sxs-lookup"><span data-stu-id="29db2-111">Updating Data</span></span>

<span data-ttu-id="29db2-112">EF는 컨텍스트에서 추적하는 기존 엔터티의 변경 내용을 자동으로 검색합니다.</span><span class="sxs-lookup"><span data-stu-id="29db2-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="29db2-113">여기에는 데이터베이스에서 로드/쿼리하는 엔터티 및 이전에 데이터베이스에 추가되고 저장된 엔터티가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="29db2-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="29db2-114">속성에 할당된 값을 수정한 후 *SaveChanges*를 호출하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="29db2-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="29db2-115">데이터 삭제</span><span class="sxs-lookup"><span data-stu-id="29db2-115">Deleting Data</span></span>

<span data-ttu-id="29db2-116">*DbSet.Remove* 메서드를 사용하여 엔터티 클래스의 인스턴스를 삭제하세요.</span><span class="sxs-lookup"><span data-stu-id="29db2-116">Use the *DbSet.Remove* method to delete instances of your entity classes.</span></span>

<span data-ttu-id="29db2-117">엔터티가 데이터베이스에 이미 있는 경우 *SaveChanges* 중에 삭제됩니다.</span><span class="sxs-lookup"><span data-stu-id="29db2-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="29db2-118">엔터티가 아직 데이터베이스에 저장되지 않은 경우(즉, 추가됨으로 추적되는 경우) 컨텍스트에서 제거되고 *SaveChanges*가 호출될 때 더 이상 삽입되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="29db2-118">If the entity has not yet been saved to the database (that is, it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="29db2-119">단일 SaveChanges의 여러 작업</span><span class="sxs-lookup"><span data-stu-id="29db2-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="29db2-120">여러 가지 추가/업데이트/제거 작업을 *SaveChanges*에 대한 단일 호출에 결합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="29db2-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="29db2-121">대부분의 데이터베이스 공급자에서 *SaveChanges*는 트랜잭션입니다.</span><span class="sxs-lookup"><span data-stu-id="29db2-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="29db2-122">즉, 모든 작업이 성공 또는 실패이며 작업이 부분적으로 적용된 상태로 유지되지 않음을 의미합니다.</span><span class="sxs-lookup"><span data-stu-id="29db2-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
