---
title: "비동기 절약-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
ms.technology: entity-framework-core
uid: core/saving/async
ms.openlocfilehash: 640e5f131b0e9ef4e5028d1dcaf80f3e5bbd9d9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-saving"></a>비동기 저장

비동기 저장의 변경 내용이 데이터베이스에 기록 되는 동안에 스레드를 차단 하지 않습니다. 이 고정 하는 굵은 클라이언트 응용 프로그램의 UI를 방지할 수 있습니다. 비동기 작업에 있는 스레드 수를 확보할 수 데이터베이스 작업이 완료 될 때 다른 요청을 처리 하는 웹 응용 프로그램에서 처리량을 높일 수도 있습니다. 자세한 내용은 참조 [C#에서 비동기 프로그래밍](https://docs.microsoft.com/dotnet/csharp/async)합니다.

> [!WARNING]  
> EF 코어 동일한 컨텍스트 인스턴스에에서 실행 되 고 여러 병렬 작업을 지원 하지 않습니다. 다음 작업을 시작 하기 전에 완료 작업에 대해 기다려야 항상 합니다. 사용 하 여 일반적으로 이렇게는 `await` 각 비동기 작업에는 키워드입니다.

Entity Framework Core 제공 `DbContext.SaveChangesAsync()` 비동기 대신 `DbContext.SaveChanges()`합니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
