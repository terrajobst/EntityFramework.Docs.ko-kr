---
title: 비동기 쿼리 - EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: 415c57df599f1cb1a255f01d1072890fedfd6d2b
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921687"
---
# <a name="asynchronous-queries"></a>비동기 쿼리

비동기 쿼리는 데이터베이스에서 쿼리가 실행되는 동안 스레드를 차단하지 않습니다. 이 기능은 씩 클라이언트 애플리케이션의 UI 고정을 방지하는 데 유용할 수 있습니다. 또한 비동기 작업은 웹 애플리케이션에서 처리량을 늘릴 수 있어, 데이터베이스 작업이 완료되는 동안 스레드가 해제되어 다른 요청을 처리할 수 있습니다. 자세한 내용은 [C#의 비동기 프로그래밍](https://docs.microsoft.com/dotnet/csharp/async)을 참조하세요.

> [!WARNING]  
> EF Core는 동일한 컨텍스트 인스턴스에서 실행되는 여러 병렬 작업을 지원하지 않습니다. 항상 작업이 완료될 때까지 대기한 후 다음 작업을 시작해야 합니다. 일반적으로 이 작업은 각 비동기 작업에서 `await` 키워드를 사용하여 수행합니다.

Entity Framework Core는 쿼리를 실행하여 결과를 반환하도록 하는 LINQ 메서드의 대안으로 사용할 수 있는 비동기 확장 메서드 집합을 제공합니다. 예를 들어 `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()` 등이 있습니다. 이러한 메서드는 LINQ 식 트리만 작성하고 데이터베이스에서 쿼리가 실행되도록 하지 않기 때문에 `Where(...)`, `OrderBy(...)` 등 LINQ 연산자의 비동기 버전이 없습니다.

> [!IMPORTANT]  
> EF Core 비동기 확장 메서드는 `Microsoft.EntityFrameworkCore` 네임스페이스에 정의됩니다. 메서드를 사용하려면 이 네임스페이스를 가져와야 합니다.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#Sample)]
