---
title: EF Core-SQLite를 사용 하 여 테스트
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: bc9d6768a90ce17160c4126d2a68fddaa30d63de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996870"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="bc755-102">SQLite를 사용 하 여 테스트</span><span class="sxs-lookup"><span data-stu-id="bc755-102">Testing with SQLite</span></span>

<span data-ttu-id="bc755-103">SQLite에는 메모리 내 모드에서는 SQLite를 사용 하 여 실제 데이터베이스 작업의 오버 헤드 없이 관계형 데이터베이스에 대해 테스트를 쓸 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc755-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="bc755-104">이 문서를 볼 수 있습니다 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) GitHub에서</span><span class="sxs-lookup"><span data-stu-id="bc755-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="bc755-105">예제 테스트 시나리오</span><span class="sxs-lookup"><span data-stu-id="bc755-105">Example testing scenario</span></span>

<span data-ttu-id="bc755-106">다음 서비스를 블로그와 관련 된 몇 가지 작업을 수행 하는 응용 프로그램 코드를 허용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="bc755-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="bc755-107">사용 하 여 내부적으로 `DbContext` SQL Server 데이터베이스에 연결 하는 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc755-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="bc755-108">많은 테스트를 만드는 작업을 수행 하거나 코드를 수정 하지 않고이 서비스에 대 한 효율적인 테스트를 작성할 수 있도록 메모리에 SQLite 데이터베이스에 연결 하려면이 컨텍스트를 교환 하려면 유용 컨텍스트의 double입니다.</span><span class="sxs-lookup"><span data-stu-id="bc755-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="bc755-109">컨텍스트 준비</span><span class="sxs-lookup"><span data-stu-id="bc755-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="bc755-110">두 데이터베이스 공급자를 구성 하지 않으려면</span><span class="sxs-lookup"><span data-stu-id="bc755-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="bc755-111">테스트에서 외부적으로 InMemory 공급자를 사용 하도록 컨텍스트를 구성 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc755-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="bc755-112">데이터베이스 공급자를 재정의 하 여 구성 하는 경우 `OnConfiguring` 프로그램 컨텍스트에서 해야 이미 구성 되지 않은 한 경우에 데이터베이스 공급자를 구성 하는 일부 조건부 코드를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc755-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="bc755-113">ASP.NET Core를 사용 하는 경우 다음 필요가 없습니다이 코드 (Startup.cs)의 컨텍스트 외부에서 데이터베이스 공급자 구성 되었기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="bc755-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="bc755-114">테스트에 대 한 생성자를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc755-114">Add a constructor for testing</span></span>

<span data-ttu-id="bc755-115">가장 간단한 방법은 다른 데이터베이스에 대해 테스트할 수 있도록 허용 하는 생성자를 노출 하 여 컨텍스트를 수정 하는 것을 `DbContextOptions<TContext>`입니다.</span><span class="sxs-lookup"><span data-stu-id="bc755-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="bc755-116">`DbContextOptions<TContext>` 모든 데이터베이스에 연결 하는 등 설정을 컨텍스트를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="bc755-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="bc755-117">이 컨텍스트에서 OnConfiguring 메서드를 실행 하 여 기본 제공 되는 동일한 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="bc755-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="bc755-118">테스트 작성</span><span class="sxs-lookup"><span data-stu-id="bc755-118">Writing tests</span></span>

<span data-ttu-id="bc755-119">이 공급자를 사용 하 여 테스트 키가 SQLite를 사용 하 고 메모리 내 데이터베이스의 범위 제어 컨텍스트를 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bc755-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="bc755-120">데이터베이스의 범위를 열고 연결을 닫는 중 제어 됩니다.</span><span class="sxs-lookup"><span data-stu-id="bc755-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="bc755-121">데이터베이스는 연결이 열려 있는 기간 범위로 제한 됩니다.</span><span class="sxs-lookup"><span data-stu-id="bc755-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="bc755-122">일반적으로 각 테스트 메서드에 대 한 데이터베이스를 정리 합니다.</span><span class="sxs-lookup"><span data-stu-id="bc755-122">Typically you want a clean database for each test method.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
