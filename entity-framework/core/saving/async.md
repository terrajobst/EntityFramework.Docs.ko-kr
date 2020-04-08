---
title: 비동기 저장 - EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 0823b86f0579dd3e42f6bd2aebfb433d3cbe00ab
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413692"
---
# <a name="asynchronous-saving"></a><span data-ttu-id="b88b9-102">비동기 저장</span><span class="sxs-lookup"><span data-stu-id="b88b9-102">Asynchronous Saving</span></span>

<span data-ttu-id="b88b9-103">비동기 저장은 데이터베이스에 변경 내용을 쓰는 동안 스레드가 차단되지 않도록 도와줍니다.</span><span class="sxs-lookup"><span data-stu-id="b88b9-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="b88b9-104">이 기능은 씩 클라이언트 애플리케이션의 UI 고정을 방지하는 데 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b88b9-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="b88b9-105">또한 비동기 작업은 웹 애플리케이션에서 처리량을 늘릴 수 있어, 데이터베이스 작업이 완료되는 동안 스레드가 해제되어 다른 요청을 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b88b9-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="b88b9-106">자세한 내용은 [C#의 비동기 프로그래밍](https://docs.microsoft.com/dotnet/csharp/async)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="b88b9-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="b88b9-107">EF Core는 동일한 컨텍스트 인스턴스에서 실행되는 여러 병렬 작업을 지원하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b88b9-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="b88b9-108">항상 작업이 완료될 때까지 대기한 후 다음 작업을 시작해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b88b9-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="b88b9-109">일반적으로 이 작업은 각 비동기 작업에서 `await` 키워드를 사용하여 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="b88b9-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="b88b9-110">Entity Framework Core는 `DbContext.SaveChangesAsync()`를 `DbContext.SaveChanges()`에 대한 비동기 대안으로 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="b88b9-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
