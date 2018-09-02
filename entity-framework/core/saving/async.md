---
title: 비동기 저장 - EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 6f482a77300ff2930953686751a579b022bf6f77
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997284"
---
# <a name="asynchronous-saving"></a>비동기 저장

비동기 저장은 데이터베이스에 변경 내용을 쓰는 동안 스레드가 차단되지 않도록 도와줍니다. 이 기능은 씩 클라이언트 응용 프로그램의 UI 고정을 방지하는 데 유용할 수 있습니다. 또한 비동기 작업은 웹 응용 프로그램에서 처리량을 늘릴 수 있어, 데이터베이스 작업이 완료되는 동안 스레드가 해제되어 다른 요청을 처리할 수 있습니다. 자세한 내용은 [C#의 비동기 프로그래밍](https://docs.microsoft.com/dotnet/csharp/async)을 참조하세요.

> [!WARNING]  
> EF Core는 동일한 컨텍스트 인스턴스에서 실행되는 여러 병렬 작업을 지원하지 않습니다. 항상 작업이 완료될 때까지 대기한 후 다음 작업을 시작해야 합니다. 일반적으로 이 작업은 각 비동기 작업에서 `await` 키워드를 사용하여 수행합니다.

Entity Framework Core는 `DbContext.SaveChangesAsync()`를 `DbContext.SaveChanges()`에 대한 비동기 대안으로 제공합니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
