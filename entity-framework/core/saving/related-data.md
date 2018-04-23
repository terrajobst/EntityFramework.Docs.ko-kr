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
# <a name="saving-related-data"></a>관련된 데이터 저장

격리 된 엔터티 외에도 작업도 가능 모델에 정의 된 관계를 사용 합니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/)을 볼 수 있습니다.

## <a name="adding-a-graph-of-new-entities"></a>새 엔터티의 그래프를 추가합니다.

여러 가지 새로운 관련된 엔터티를 만드는 경우 추가 둘 중 하나를 컨텍스트에 하면 다른 사람이 너무 추가할 수 있도록.

다음 예에서 블로그 및 관련 된 게시물 3 개는 모든 데이터베이스에 삽입 합니다. 게시물 검색 및를 통해 연결할 수 있기 때문에 추가 되는 `Blog.Posts` 탐색 속성입니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> EntityEntry.State 속성을 사용 하 여 방금 단일 엔터티의 상태를 설정 합니다. 예를 들어, `context.Entry(blog).State = EntityState.Modified`을 입력합니다.

## <a name="adding-a-related-entity"></a>관련된 엔터티를 추가합니다.

컨텍스트에서 이미 추적 된 엔터티의 탐색 속성에서 새 엔터티를 참조 하는 경우 엔터티를 검색 하 고 데이터베이스에 삽입 됩니다.

다음 예제에서는 `post` 에 추가 되기 때문에 엔터티 삽입는 `Posts` 속성의는 `blog` 엔터티가 데이터베이스에서 인출 된 있는 합니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>관계를 변경

엔터티의 탐색 속성을 변경 하면 해당 변경 내용은 데이터베이스에 외래 키 열에 만들어집니다.

다음 예제에서는 `post` 새에 속하는 것으로 엔터티가 업데이트 되 `blog` 엔터티 때문에 해당 `Blog` 를 가리키도록 탐색 속성은 `blog`합니다. `blog` 컨텍스트에서 이미 추적 된 엔터티의 탐색 속성에 의해 참조 되는 새 엔터티 이기 때문에 데이터베이스에 삽입도 됩니다 (`post`).

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>관계 제거

참조 탐색을 설정 하 여 관계를 제거할 수 있습니다 `null`, 또는 컬렉션 탐색에서 관련된 엔터티를 제거 합니다.

관계 제거 종속 엔터티가에 의도 하지 않은, cascade에 따라 관계에 구성 된 동작을 삭제할 수 있습니다.

기본적으로 필요한 관계에 대 한 계단식 삭제 동작 구성 된 하 고 자식/종속 엔터티는 데이터베이스에서 삭제 됩니다. 선택적 관계 모두 삭제 하지 기본적으로 구성 되어 있지만 외래 키 속성을 설정할 수는 null로 합니다.

참조 [필수 및 선택적 관계](../modeling/relationships.md#required-and-optional-relationships) 관계 requiredness 구성할 수 있는 방법을 알아봅니다.

참조 [모두 삭제](cascade-delete.md) 은 구성할 수 있는 방법을 명시적으로 및 규칙에 따라 선택 방법을 cascade 동작을 삭제 하는 방법에 대 한 자세한 내용은 작업에 대 한 합니다.

다음 예제에서는 하위 삭제 간의 관계에 구성 된 `Blog` 및 `Post`이므로 `post` 엔터티는 데이터베이스에서 삭제 됩니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
