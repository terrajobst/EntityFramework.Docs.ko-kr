---
title: 관련 데이터 저장 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
uid: core/saving/related-data
ms.openlocfilehash: 86d32b6172ee21c12a15e9ed4bb0142afc99c8bd
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413614"
---
# <a name="saving-related-data"></a><span data-ttu-id="b826e-102">관련 데이터 저장</span><span class="sxs-lookup"><span data-stu-id="b826e-102">Saving Related Data</span></span>

<span data-ttu-id="b826e-103">격리된 엔터티 외에 모델에 정의된 관계도 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b826e-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="b826e-104">GitHub에서 이 문서의 [샘플](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b826e-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="b826e-105">새 엔터티 그래프 추가</span><span class="sxs-lookup"><span data-stu-id="b826e-105">Adding a graph of new entities</span></span>

<span data-ttu-id="b826e-106">새로운 관련 엔터티를 여러 개 만든 경우 이들 중 하나를 컨텍스트에 추가하면 다른 엔터티도 추가될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b826e-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="b826e-107">다음 예제에서는 블로그와 세 개의 관련 게시물이 모두 데이터베이스에 삽입됩니다.</span><span class="sxs-lookup"><span data-stu-id="b826e-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="b826e-108">`Blog.Posts` 탐색 속성을 통해 게시물에 연결할 수 있으므로 게시물을 검색하여 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="b826e-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="b826e-109">EntityEntry.State 속성을 사용하여 단일 엔터티의 상태를 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="b826e-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="b826e-110">예를 들어 `context.Entry(blog).State = EntityState.Modified`과 같은 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="b826e-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="b826e-111">관련 엔터티 추가하기</span><span class="sxs-lookup"><span data-stu-id="b826e-111">Adding a related entity</span></span>

<span data-ttu-id="b826e-112">컨텍스트에서 이미 추적한 엔터티의 탐색 속성에서 새 엔터티를 참조하는 경우 엔터티가 검색되어 데이터베이스에 삽입됩니다.</span><span class="sxs-lookup"><span data-stu-id="b826e-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="b826e-113">다음 예제에서는 `post` 엔터티가 데이터베이스에서 가져온 `Posts` 엔터티의 `blog` 속성에 추가되어 삽입됩니다.</span><span class="sxs-lookup"><span data-stu-id="b826e-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="b826e-114">관계 변경</span><span class="sxs-lookup"><span data-stu-id="b826e-114">Changing relationships</span></span>

<span data-ttu-id="b826e-115">엔터티의 탐색 속성을 변경하는 경우 데이터베이스의 외래 키 열에도 해당 변경 내용이 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="b826e-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="b826e-116">다음 예제에서는 해당 `post` 탐색 속성이 `blog`를 가리키도록 설정되어 있으므로 `Blog` 엔터티가 새 `blog` 엔터티에 속하도록 업데이트됩니다.</span><span class="sxs-lookup"><span data-stu-id="b826e-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="b826e-117">또한 `blog`는 컨텍스트에서 이미 추적한 엔터티(`post`)의 탐색 속성에서 참조되는 새 엔터티이므로 데이터베이스에 삽입됩니다.</span><span class="sxs-lookup"><span data-stu-id="b826e-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="b826e-118">관계 제거</span><span class="sxs-lookup"><span data-stu-id="b826e-118">Removing relationships</span></span>

<span data-ttu-id="b826e-119">참조 탐색을 `null`로 설정하거나 컬렉션 탐색에서 관련 엔터티를 제거하여 관계를 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b826e-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="b826e-120">관계를 제거하면 관계에 구성된 하위 삭제 동작에 따라 종속 엔터티에 의도하지 않은 영향을 줄 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b826e-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="b826e-121">기본적으로 필수 관계의 경우 하위 삭제 동작이 구성되고 자식/종속 엔터티가 데이터베이스에서 삭제됩니다.</span><span class="sxs-lookup"><span data-stu-id="b826e-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="b826e-122">선택적 관계의 경우 기본적으로 하위 삭제가 구성되지 않고 외래 키 속성이 null로 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="b826e-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="b826e-123">관계의 필수 여부를 구성하는 방법에 대해 알아보려면 [필수 및 선택적 관계](../modeling/relationships.md#required-and-optional-relationships)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="b826e-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="b826e-124">하위 삭제 동작의 작동 방식, 이 동작을 명시적으로 구성하는 방법 및 규칙에서 선택하는 방법에 대한 자세한 내용은 [하위 삭제](cascade-delete.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="b826e-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="b826e-125">다음 예제에서는 하위 삭제가 `Blog`와 `Post` 간의 관계에 구성되어 `post` 엔터티가 데이터베이스에서 삭제됩니다.</span><span class="sxs-lookup"><span data-stu-id="b826e-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
