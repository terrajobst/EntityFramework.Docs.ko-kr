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
# <a name="saving-related-data"></a>관련 데이터 저장

격리된 엔터티 외에 모델에 정의된 관계도 사용할 수 있습니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/)을 볼 수 있습니다.

## <a name="adding-a-graph-of-new-entities"></a>새 엔터티 그래프 추가

새로운 관련 엔터티를 여러 개 만든 경우 이들 중 하나를 컨텍스트에 추가하면 다른 엔터티도 추가될 수 있습니다.

다음 예제에서는 블로그와 세 개의 관련 게시물이 모두 데이터베이스에 삽입됩니다. `Blog.Posts` 탐색 속성을 통해 게시물에 연결할 수 있으므로 게시물을 검색하여 추가합니다.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> EntityEntry.State 속성을 사용하여 단일 엔터티의 상태를 설정합니다. 예를 들어 `context.Entry(blog).State = EntityState.Modified`과 같은 형식입니다.

## <a name="adding-a-related-entity"></a>관련 엔터티 추가하기

컨텍스트에서 이미 추적한 엔터티의 탐색 속성에서 새 엔터티를 참조하는 경우 엔터티가 검색되어 데이터베이스에 삽입됩니다.

다음 예제에서는 `post` 엔터티가 데이터베이스에서 가져온 `Posts` 엔터티의 `blog` 속성에 추가되어 삽입됩니다.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>관계 변경

엔터티의 탐색 속성을 변경하는 경우 데이터베이스의 외래 키 열에도 해당 변경 내용이 적용됩니다.

다음 예제에서는 해당 `post` 탐색 속성이 `blog`를 가리키도록 설정되어 있으므로 `Blog` 엔터티가 새 `blog` 엔터티에 속하도록 업데이트됩니다. 또한 `blog`는 컨텍스트에서 이미 추적한 엔터티(`post`)의 탐색 속성에서 참조되는 새 엔터티이므로 데이터베이스에 삽입됩니다.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>관계 제거

참조 탐색을 `null`로 설정하거나 컬렉션 탐색에서 관련 엔터티를 제거하여 관계를 제거할 수 있습니다.

관계를 제거하면 관계에 구성된 하위 삭제 동작에 따라 종속 엔터티에 의도하지 않은 영향을 줄 수 있습니다.

기본적으로 필수 관계의 경우 하위 삭제 동작이 구성되고 자식/종속 엔터티가 데이터베이스에서 삭제됩니다. 선택적 관계의 경우 기본적으로 하위 삭제가 구성되지 않고 외래 키 속성이 null로 설정됩니다.

관계의 필수 여부를 구성하는 방법에 대해 알아보려면 [필수 및 선택적 관계](../modeling/relationships.md#required-and-optional-relationships)를 참조하세요.

하위 삭제 동작의 작동 방식, 이 동작을 명시적으로 구성하는 방법 및 규칙에서 선택하는 방법에 대한 자세한 내용은 [하위 삭제](cascade-delete.md)를 참조하세요.

다음 예제에서는 하위 삭제가 `Blog`와 `Post` 간의 관계에 구성되어 `post` 엔터티가 데이터베이스에서 삭제됩니다.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
