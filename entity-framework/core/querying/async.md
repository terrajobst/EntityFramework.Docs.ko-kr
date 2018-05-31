---
title: 비동기 쿼리 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052683"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="efc26-102">비동기 쿼리</span><span class="sxs-lookup"><span data-stu-id="efc26-102">Asynchronous Queries</span></span>

<span data-ttu-id="efc26-103">비동기 쿼리는 데이터베이스에서 쿼리가 실행되는 동안 스레드를 차단하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="efc26-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="efc26-104">이 기능은 씩 클라이언트 응용 프로그램의 UI 고정을 방지하는 데 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efc26-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="efc26-105">또한 비동기 작업은 웹 응용 프로그램에서 처리량을 늘릴 수 있어, 데이터베이스 작업이 완료되는 동안 스레드가 해제되어 다른 요청을 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="efc26-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="efc26-106">자세한 내용은 [C#의 비동기 프로그래밍](https://docs.microsoft.com/dotnet/csharp/async)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="efc26-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="efc26-107">EF Core는 동일한 컨텍스트 인스턴스에서 실행되는 여러 병렬 작업을 지원하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="efc26-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="efc26-108">항상 작업이 완료될 때까지 대기한 후 다음 작업을 시작해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="efc26-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="efc26-109">일반적으로 이 작업은 각 비동기 작업에서 `await` 키워드를 사용하여 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="efc26-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="efc26-110">Entity Framework Core는 쿼리를 실행하여 결과를 반환하도록 하는 LINQ 메서드의 대안으로 사용할 수 있는 비동기 확장 메서드 집합을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="efc26-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="efc26-111">예를 들어 `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()` 등이 있습니다. 이러한 메서드는 LINQ 식 트리만 작성하고 데이터베이스에서 쿼리가 실행되도록 하지 않기 때문에 `Where(...)`, `OrderBy(...)` 등 LINQ 연산자의 비동기 버전이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="efc26-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="efc26-112">EF Core 비동기 확장 메서드는 `Microsoft.EntityFrameworkCore` 네임스페이스에 정의됩니다.</span><span class="sxs-lookup"><span data-stu-id="efc26-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="efc26-113">메서드를 사용하려면 이 네임스페이스를 가져와야 합니다.</span><span class="sxs-lookup"><span data-stu-id="efc26-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
