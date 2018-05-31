---
title: 연결이 끊긴 엔터티 - EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: 0b145217d40027c4b8e4746e9c5651652a28c9eb
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152418"
---
# <a name="disconnected-entities"></a>연결이 끊긴 엔터티

DbContext 인스턴스는 데이터베이스에서 반환된 엔터티를 자동으로 추적합니다. 이러한 엔터티의 변경 내용은 SaveChanges를 호출하고 필요에 따라 데이터베이스를 업데이트할 때 감지됩니다. 자세한 내용은 [기본 저장](basic.md) 및 [관련 데이터](related-data.md)를 참조하세요.

그러나 하나의 컨텍스트 인스턴스를 사용하여 엔터티를 쿼리하고 다른 인스턴스를 사용하여 저장하는 경우가 있습니다. 이런 상황은 엔터티를 쿼리하고, 클라이언트로 전송하고, 수정하고, 요청에서 서버로 다시 전송한 후 저장하는 웹 응용 프로그램과 같이 “연결이 끊긴” 시나리오에서 종종 발생합니다. 이 경우 두 번째 컨텍스트 인스턴스는 엔터티가 삽입해야 하는 새 엔터티인지 업데이트해야 하는 기존 엔터티인지를 알고 있어야 합니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/)을 볼 수 있습니다.

> [!TIP]
> EF Core는 지정된 기본 키 값이 있는 엔터티의 인스턴스 하나만 추적할 수 있습니다. 문제가 되지 않도록 하는 가장 좋은 방법은 각 작업 단위에 대해 단기 컨텍스트를 사용하여 컨텍스트가 빈 상태로 시작되고 엔터티를 연결하고 해당 엔터티를 저장한 후 컨텍스트가 삭제되고 취소되도록 하는 것입니다.

## <a name="identifying-new-entities"></a>새 엔터티 식별

### <a name="client-identifies-new-entities"></a>클라이언트에서 새 엔터티 식별

처리할 가장 간단한 경우는 새 엔터티인지 기존 엔터티인지 여부를 클라이언트가 서버에 알리는 경우입니다. 예를 들어 새 엔터티를 삽입하는 요청은 기존 엔터티를 업데이트하는 요청과 다릅니다.

이 섹션의 나머지 부분에서는 삽입할지 업데이트할지를 다른 방법으로 확인해야 하는 경우에 대해 설명합니다.

### <a name="with-auto-generated-keys"></a>자동 생성 키 사용

종종 자동으로 생성된 키 값을 사용하여 엔터티를 삽입해야 하는지 업데이트해야 하는지를 확인할 수 있습니다. 키가 설정되지 않은 경우(예: 아직 null, 0 등의 CLR 기본값이 있는 경우) 새 엔터티이며 삽입해야 합니다. 반면 키 값이 설정된 경우 이전에 이미 저장되었으므로 업데이트해야 합니다. 즉, 키에 값이 있으면 엔터티를 쿼리하고 클라이언트로 전송했으며 이제 다시 돌아가 업데이트해야 합니다.

엔터티 형식을 알고 있으면 설정 해제된 키를 쉽게 확인할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

그러나 EF에는 엔터티 형식 및 키 형식에 따라 이 작업을 수행하는 기본 제공 방법도 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> 엔터티가 Added 상태인 경우에도 키는 컨텍스트에서 엔터티를 추적하는 즉시 설정됩니다. 따라서 TrackGraph API를 사용하는 경우와 같이 엔터티 그래프를 트래버스하고 각각에 대해 수행할 작업을 결정할 때 도움이 됩니다. 키 값은 엔터티를 추적하기 위해 호출하기 ‘전에’ 여기에 표시된 방법으로만 사용해야 합니다.

### <a name="with-other-keys"></a>다른 키 사용

키 값이 자동으로 생성되지 않는 경우 몇 가지 다른 메커니즘으로 새 엔터티를 식별해야 합니다. 두 가지 일반적인 접근 방법이 있습니다.
 * 엔터티 쿼리
 * 클라이언트에서 플래그 전달

엔터티를 쿼리하려면 Find 메서드를 사용하면 됩니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

클라이언트에서 플래그를 전달하는 전체 코드를 보여주는 것은 이 문서의 범위를 벗어납니다. 웹앱에서는 일반적으로 작업에 따라 다른 요청을 하거나, 요청에서 몇 가지 상태를 전달한 후 컨트롤러에서 추출합니다.

## <a name="saving-single-entities"></a>단일 엔터티 저장

삽입이 필요한지 업데이트가 필요한지를 알고 있는 경우 Add 또는 Update를 적절하게 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

그러나 엔터티에서 자동 생성 키 값을 사용하는 경우에는 두 경우 모두에 Update 메서드를 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

Update 메서드는 일반적으로 엔터티를 삽입이 아닌 업데이트하도록 표시합니다. 그러나 엔터티에 자동 생성 키가 있고 키 값이 설정되지 않은 경우 엔터티는 자동으로 삽입하도록 표시됩니다.

> [!TIP]  
> 이 동작은 EF Core 2.0에서 도입되었습니다. 이전 릴리스에서는 항상 Add 또는 Update를 명시적으로 선택해야 합니다.

엔터티에서 자동 생성 키를 사용하지 않는 경우 응용 프로그램에서 엔터티를 삽입해야 하는지 업데이트해야 하는지를 결정해야 합니다. 예를 들면 다음과 같습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

단계는 다음과 같습니다.
* Find에서 null을 반환하는 경우 데이터베이스에 이 ID를 사용하는 블로그가 아직 포함되어 있지 않으므로 Add를 호출하여 삽입하도록 표시합니다.
* Find에서 엔터티를 반환하는 경우 엔터티가 데이터베이스에 있고 컨텍스트에서 기존 엔터티를 추적하고 있는 것입니다.
  * 그러면 SetValues를 사용하여 이 엔터티의 모든 속성에 대한 값을 클라이언트에서 가져온 값으로 설정합니다.
  * SetValues 호출에서 필요에 따라 업데이트하도록 엔터티를 표시합니다.

> [!TIP]  
> SetValues는 추적된 엔터티의 값과 다른 값을 갖는 속성만 수정된 것으로 표시합니다. 즉, 업데이트가 전송되는 경우 실제로 변경된 열만 업데이트됩니다. 또한 변경된 내용이 없으면 업데이트가 전송되지 않습니다.

## <a name="working-with-graphs"></a>그래프 작업

### <a name="identity-resolution"></a>ID 확인

위에서 설명한 대로 EF Core는 지정된 기본 키 값이 있는 엔터티의 인스턴스 하나만 추적할 수 있습니다. 그래프로 작업할 때 이 고정 조건이 유지되도록 이상적으로 그래프를 만들어야 하며 컨텍스트는 하나의 작업 단위에만 사용해야 합니다. 그래프에 중복이 포함되어 있는 경우 EF로 보내기 전에 그래프를 처리하여 여러 인스턴스를 하나로 통합해야 합니다. 인스턴스에 충돌하는 값과 관계가 있는 경우 간단하지 않을 수 있으므로 충돌 해결을 피하려면 응용 프로그램 파이프라인에서 최대한 빨리 중복 통합을 수행해야 합니다.

### <a name="all-newall-existing-entities"></a>모든 새 엔터티/모든 기존 엔터티

그래프 작업의 예로는 연관된 게시물 컬렉션과 함께 블로그를 삽입하거나 업데이트하는 작업이 있습니다. 그래프의 모든 엔터티를 삽입해야 하거나 모두 업데이트해야 하는 경우 프로세스는 위에서 단일 엔터티에 대해 설명한 것과 동일합니다. 예를 들어 다음과 같이 블로그 및 게시물 그래프를 만들 수 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

다음과 같이 삽입할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

Add를 호출하여 블로그 및 모든 게시물이 삽입되도록 표시합니다.

마찬가지로 그래프의 모든 엔터티를 업데이트해야 하는 경우 Update를 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

블로그 및 모든 게시물이 업데이트되도록 표시됩니다.

### <a name="mix-of-new-and-existing-entities"></a>새 엔터티와 기존 엔터티의 혼합

자동 생성 키를 사용하면 그래프에 삽입할 엔터티와 업데이트할 엔터티가 혼합되어 있는 경우에도 삽입과 업데이트 모두에 Update를 다시 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

Update는 키 값 집합이 없는 경우 그래프의 모든 엔터티인 블로그 또는 게시물을 삽입하도록 표시하고 다른 모든 엔터티는 업데이트하도록 표시합니다.

앞에서처럼 자동 생성 키를 사용하지 않는 경우 쿼리 및 일부 처리를 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>삭제 처리

엔터티가 없으면 삭제되어야 함을 의미하기 때문에 삭제는 처리하기가 어려울 수 있습니다. 삭제를 처리하는 한 가지 방법은 엔터티가 실제로 삭제되는 것이 아니라 삭제됨으로 표시되도록 “일시 삭제”를 사용하는 것입니다. 그러면 삭제가 업데이트와 동일하게 됩니다. 일시 삭제는 [쿼리 필터](xref:core/querying/filters)를 사용하여 구현할 수 있습니다.

실제 삭제의 경우 일반적인 패턴은 쿼리 패턴의 확장을 사용하여 기본적으로 그래프 차이점을 수행하는 것입니다. 예:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

내부적으로 Add, Attach 및 Update는 그래프 통과를 사용하여 각 엔터티에 대해 추가됨(삽입), 수정됨(업데이트), 변경되지 않음(아무 작업도 하지 않음) 또는 삭제됨(삭제)으로 표시할지 여부를 결정합니다. 이 메커니즘은 TrackGraph API를 통해 노출됩니다. 예를 들어 클라이언트가 엔터티의 그래프를 다시 전송할 때 각 엔터티에 대해 처리되어야 하는 방법을 나타내는 몇 가지 플래그를 설정한다고 가정해 보겠습니다. 그런 다음, TrackGraph를 사용하여 이 플래그를 처리할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

플래그는 예제를 간단하게 나타내기 위해 엔터티의 일부로만 표시됩니다. 일반적으로 플래그는 요청에 포함된 DTO 또는 다른 몇 가지 상태의 일부가 됩니다.
