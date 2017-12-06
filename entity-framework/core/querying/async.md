---
title: "비동기 쿼리-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-queries"></a>비동기 쿼리

비동기 쿼리 동안 데이터베이스에 쿼리가 실행 되는 스레드를 차단 하지 마십시오. 이 고정 하는 굵은 클라이언트 응용 프로그램의 UI를 방지할 수 있습니다. 비동기 작업에 있는 스레드 수를 확보할 수 데이터베이스 작업이 완료 될 때 다른 요청을 처리 하는 웹 응용 프로그램에서 처리량을 높일 수도 있습니다. 자세한 내용은 참조 [C#에서 비동기 프로그래밍](https://docs.microsoft.com/dotnet/csharp/async)합니다.

> [!WARNING]  
> EF 코어 동일한 컨텍스트 인스턴스에에서 실행 되 고 여러 병렬 작업을 지원 하지 않습니다. 다음 작업을 시작 하기 전에 완료 작업에 대해 기다려야 항상 합니다. 사용 하 여 일반적으로 이렇게는 `await` 각 비동기 작업에는 키워드입니다.

Entity Framework Core LINQ 쿼리를 실행 하는 메서드와 반환 된 결과 대 안으로 사용할 수 있는 비동기 확장 메서드를 제공 합니다. 예를 들면 `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`등입니다. 않습니다 LINQ 연산자의 비동기 버전와 같은 `Where(...)`, `OrderBy(...)`등 이러한 메서드 데이터만 LINQ 식 트리를 작성 하 고 데이터베이스에서 실행할 쿼리를 일으키지 않습니다.

> [!IMPORTANT]  
> 에 정의 된 EF 코어 비동기 확장 메서드는 `Microsoft.EntityFrameworkCore` 네임 스페이스입니다. 이 네임 스페이스를 사용할 수는 방법에 대 한 가져와야 합니다.

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
