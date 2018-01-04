---
title: "연결이 끊긴된 엔터티-EF 코어"
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: 0ea02876b9594d54c971a7b70fcf7ce591e56ba0
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/22/2017
---
# <a name="disconnected-entities"></a>연결이 끊긴된 엔터티

DbContext 인스턴스는 자동으로 데이터베이스에서 반환 되는 엔터티를 추적 합니다. 이러한 엔터티를 변경 하는 다음 검색 SaveChanges가 호출 하 고 필요에 따라 데이터베이스 업데이트 됩니다. 참조 [기본 저장](basic.md) 및 [관련 데이터](related-data.md) 대 한 자세한 내용은 합니다.

그러나 경우에 따라 엔터티 쿼리 컨텍스트 인스턴스를 사용 하 고 다음 다른 인스턴스를 사용 하 여를 저장 합니다. 이 엔터티는 쿼리, 클라이언트에 전송, 수정, 요청에서 서버에 다시 보내고 위치 저장 한 다음 웹 응용 프로그램과 같은 "연결이 끊긴된 된" 시나리오에서 자주 발생 합니다. 이 경우 두 번째 컨텍스트 엔터티는 새 있는지 여부를 알아야 (삽입할지) 요구 사항이 나 (업데이트할지) 기존 인스턴스.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/)을 볼 수 있습니다.

## <a name="identifying-new-entities"></a>새 엔터티를 식별합니다.

### <a name="client-identifies-new-entities"></a>새 엔터티를 식별 하는 클라이언트

클라이언트가 서버에 알리기 때문 기존 또는 새 엔터티가 인지 때 다루기가 가장 간단 하 게 표시 합니다. 예를 들어 자주 새 엔터티를 삽입 하는 요청 간에 차이가 있는 기존 엔터티 업데이트 요청을 합니다.

이 섹션의 나머지 부분에서는 사례를 설명에 다른 방법으로 삽입 하거나 업데이트할 것인지 결정 하는 데 필요한 것입니다.

### <a name="with-auto-generated-keys"></a>자동 생성 키 사용

자동으로 생성 된 키의 값은 엔터티를 삽입 또는 업데이트할 수 있는지 여부를 결정 하기 위한 자주 사용할 수 있습니다. 키 하지 않은 경우 엔터티 새 이어야 하며 삽입 해야 합니다 (즉, 아직 CLR 기본값인 null, 0, 등)를 설정 했습니다. 반면에 키 값에 설정한 경우 다음 이미 이전에 저장 해야 하 고 이제을 업데이트 해야 합니다. 즉, 키 값이 있으면 않으면 엔터티는 클라이언트에 보낸 쿼리 되었습니다 방문 하 여가 지금 업데이트 해야 합니다.

엔터티 형식을 알 수 있을 때 설정 되지 않은 키를 확인 하는 것이 쉽습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

그러나 EF에 모든 엔터티 형식 및 키 형식에 대 한이 작업을 수행 하는 기본 제공 방법이:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> 엔터티는 Added 상태의 경우에 엔터티는 컨텍스트에 의해 추적 되는 즉시 키가 설정 됩니다. 엔터티 및 TrackGraph API를 사용 하는 경우 각 등는 마찬가지로으로 수행할 작업을 결정 그래프 검토 때 도움이 됩니다. 여기에 표시 된 방식에는 키 값만 사용 해야 _전에_ 엔터티를 추적 하는 호출 합니다.

### <a name="with-other-keys"></a>다른 키와

일부 다른 메커니즘은 키 값을 자동으로 생성 되지 않은 경우 새 엔터티를 식별 하려면 필요 합니다. 두 가지 일반적인 방법을이 있습니다.
 * 엔터티에 대 한 쿼리
 * 클라이언트에서 플래그를 전달 합니다.

엔터티를 쿼리하려면 Find 메서드를 사용 하기만 됩니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

클라이언트에서 플래그를 전달 하기 위한 전체 코드를 표시 하려면이 문서의 범위를 벗어납니다. 웹 앱에서 일반적으로 다양 한 작업에 대 한 다른 요청을 수행 하거나 특정 상태 요청에 전달 다음 컨트롤러에 추출 의미 합니다.

## <a name="saving-single-entities"></a>단일 엔터티 저장

Insert 또는 update 필요 여부 다음 추가 또는 업데이트를 적절 하 게 사용할 수 있습니다 알려진 경우:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

그러나 엔터티 키 값 자동 생성을 사용 하는 경우 업데이트 메서드 두 경우 모두에 대해 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

큐브의 Update 메서드에 일반적으로 업데이트를 하지 삽입에 대 한 엔터티를 표시합니다. 그러나의 엔터티에 자동 생성 키를 대신 엔터티는에 대 한 자동으로 표시 한 다음 키 값이 없는 설정 된에 삽입 합니다.

> [!TIP]  
> 이 문제는 EF 코어 2.0에서 도입 되었습니다. 이전 릴리스에서 해야 하는 항상 명시적으로 추가 또는 업데이트를 선택 합니다.

엔터티는 자동 생성 키를 사용 하지 않는 경우 응용 프로그램 엔터티 삽입 또는 업데이트 여부를 결정 해야 합니다: 예:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

여기에 단계는.
* 데이터베이스 호출 되므로이 ID 가진 블로그 포함 되어 있지 않은 한 후에 null을 반환 찾기 추가 삽입을 위해 표시 합니다.
* 찾기 엔터티를 반환 하는 경우 다음 데이터베이스에 있으며 컨텍스트 이제 기존 엔터티를 추적
  * 그런 다음 SetValues를 사용 하 여이 엔터티를 클라이언트에서 제공 되는 것에 모든 속성에 대 한 값을 설정 합니다.
  * SetValues 호출은 필요에 따라 업데이트할 엔터티를 표시 합니다.

> [!TIP]  
> 추적 된 엔터티입니다의 값이 다른 속성을 수정한 SetValues은 표시만 합니다. 즉,는 업데이트를 보낼 때 실제로 변경 된 열만 업데이트 됩니다. (및 변경 된 내용이, 업데이트가 전혀 전송 됩니다.)

## <a name="working-with-graphs"></a>그래프 작업

### <a name="all-newall-existing-entities"></a>모든 새/all 기존 엔터티

그래프 사용의 예로 삽입 되었거나 해당 컬렉션과 관련 된 게시물 함께 블로그 업데이트 됩니다. 그래프에 있는 모든 엔터티를 삽입할 또는 모두를 업데이트 해야 하는 경우 프로세스는 단일 개체에 대해 위에서 설명한 것과 동일 됩니다. 예를 들어, 블로그 및 다음과 같이 만든 게시물의 그래프:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

다음과 같이 삽입 될 수 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

블로그 및 삽입할 수 있는 모든 게시물 추가에 대 한 호출으로 표시 됩니다.

마찬가지로, 그래프에 있는 모든 엔터티를 업데이트 해야 하는 경우 다음 업데이트 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

블로그 및 모든 게시물 업데이트할 표시 됩니다.

### <a name="mix-of-new-and-existing-entities"></a>신규 및 기존 엔터티의 조합

자동 생성 키와 업데이트 다시 사용할 수 삽입 및 업데이트가 모두에 대 한 그래프에 삽입 해야 하는 엔터티 및 업데이트 해야 하는 것이 혼합 되어 있는 경우에:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

업데이트는 그래프, 블로그 또는 기타 모든 엔터티를 업데이트 하도록 표시 된 동안 키 값 집합을 설치 하지 않은 경우 삽입에 대 한 게시의 모든 엔터티를 표시 합니다.

이전에 자동 생성 키를 사용 하지 않는 경우 쿼리 및 처리 일부 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>삭제를 처리합니다.

삭제 후 처리 하기 어려울 수 있습니다 해야 삭제할 수 없으면 엔터티의 의미 하는 경우가 많습니다. 이 처리 하는 한 가지 방법은 엔터티는 실제로 삭제 하지 않고 삭제 하는 것으로 표시 되는 "소프트 삭제"를 사용 하는 것입니다. 삭제 한 다음 업데이트와 동일 하 게 됩니다. 소프트 삭제를 사용 하 여 구현할 수 있습니다 [필터 쿼리](xref:core/querying/filters)합니다.

True 삭제에 대 한 그래프 차이 기본적으로 이란 수행 하려면 쿼리 패턴의 확장을 사용 하는 일반적인 패턴은 예:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

내부적으로 추가, 연결, 및 Update 사용 그래프 순회 Unchanged 여부 것으로 표시 되어야 합니다 (삽입)를 추가, 수정 (업데이트)에 대 한 각 엔터티에 대해 수행 하는 결정 (아무 작업도 수행할)을, 또는 (삭제)를 삭제 합니다. 이 메커니즘은 TrackGraph API를 통해 노출 됩니다. 예를 들어, 엔터티의 그래프 다시 보내면 처리 방법을 나타내는 각 엔터티에 대해 일부 플래그를 설정 한다고 가정해 보겠습니다. TrackGraph이이 플래그를 처리 한 다음 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

플래그의 예제에서는 간단한 설명을 위해 엔터티의 일부로 표시 됩니다. 일반적으로 플래그의 일부는 DTO 또는 일부 다른 상태는 요청에 포함 된 것입니다.
