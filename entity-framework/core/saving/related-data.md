---
title: 저장 된 데이터-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: b0ed25267c85e82db18d8a89693b6040db7e4b34
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2018
---
# <a name="saving-related-data"></a><span data-ttu-id="ee57a-102">관련된 데이터 저장</span><span class="sxs-lookup"><span data-stu-id="ee57a-102">Saving Related Data</span></span>

<span data-ttu-id="ee57a-103">격리 된 엔터티 외에도 작업도 가능 모델에 정의 된 관계를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="ee57a-104">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="ee57a-105">새 엔터티의 그래프를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-105">Adding a graph of new entities</span></span>

<span data-ttu-id="ee57a-106">여러 가지 새로운 관련된 엔터티를 만드는 경우 추가 둘 중 하나를 컨텍스트에 하면 다른 사람이 너무 추가할 수 있도록.</span><span class="sxs-lookup"><span data-stu-id="ee57a-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="ee57a-107">다음 예에서 블로그 및 관련 된 게시물 3 개는 모든 데이터베이스에 삽입 합니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="ee57a-108">게시물 검색 및를 통해 연결할 수 있기 때문에 추가 되는 `Blog.Posts` 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="ee57a-109">EntityEntry.State 속성을 사용 하 여 방금 단일 엔터티의 상태를 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="ee57a-110">예를 들어, `context.Entry(blog).State = EntityState.Modified`을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="ee57a-111">관련된 엔터티를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-111">Adding a related entity</span></span>

<span data-ttu-id="ee57a-112">컨텍스트에서 이미 추적 된 엔터티의 탐색 속성에서 새 엔터티를 참조 하는 경우 엔터티를 검색 하 고 데이터베이스에 삽입 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="ee57a-113">다음 예제에서는 `post` 에 추가 되기 때문에 엔터티 삽입는 `Posts` 속성의는 `blog` 엔터티가 데이터베이스에서 인출 된 있는 합니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="ee57a-114">관계를 변경</span><span class="sxs-lookup"><span data-stu-id="ee57a-114">Changing relationships</span></span>

<span data-ttu-id="ee57a-115">엔터티의 탐색 속성을 변경 하면 해당 변경 내용은 데이터베이스에 외래 키 열에 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="ee57a-116">다음 예제에서는 `post` 새에 속하는 것으로 엔터티가 업데이트 되 `blog` 엔터티 때문에 해당 `Blog` 를 가리키도록 탐색 속성은 `blog`합니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="ee57a-117">`blog` 컨텍스트에서 이미 추적 된 엔터티의 탐색 속성에 의해 참조 되는 새 엔터티 이기 때문에 데이터베이스에 삽입도 됩니다 (`post`).</span><span class="sxs-lookup"><span data-stu-id="ee57a-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="ee57a-118">관계 제거</span><span class="sxs-lookup"><span data-stu-id="ee57a-118">Removing relationships</span></span>

<span data-ttu-id="ee57a-119">참조 탐색을 설정 하 여 관계를 제거할 수 있습니다 `null`, 또는 컬렉션 탐색에서 관련된 엔터티를 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="ee57a-120">관계 제거 종속 엔터티가에 의도 하지 않은, cascade에 따라 관계에 구성 된 동작을 삭제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="ee57a-121">기본적으로 필요한 관계에 대 한 계단식 삭제 동작 구성 된 하 고 자식/종속 엔터티는 데이터베이스에서 삭제 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="ee57a-122">선택적 관계 모두 삭제 하지 기본적으로 구성 되어 있지만 외래 키 속성을 설정할 수는 null로 합니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="ee57a-123">참조 [필수 및 선택적 관계](../modeling/relationships.md#required-and-optional-relationships) 관계 requiredness 구성할 수 있는 방법을 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="ee57a-124">참조 [모두 삭제](cascade-delete.md) 은 구성할 수 있는 방법을 명시적으로 및 규칙에 따라 선택 방법을 cascade 동작을 삭제 하는 방법에 대 한 자세한 내용은 작업에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="ee57a-125">다음 예제에서는 하위 삭제 간의 관계에 구성 된 `Blog` 및 `Post`이므로 `post` 엔터티는 데이터베이스에서 삭제 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ee57a-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
