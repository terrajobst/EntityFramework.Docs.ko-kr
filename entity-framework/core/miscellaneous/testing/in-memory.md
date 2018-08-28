---
title: InMemory-EF Core 사용 하 여 테스트
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 2754d1deba98fcee0eb88669293b2197545c8874
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997894"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="21624-102">InMemory 테스트</span><span class="sxs-lookup"><span data-stu-id="21624-102">Testing with InMemory</span></span>

<span data-ttu-id="21624-103">InMemory 공급자는 실제 데이터베이스 작업의 오버 헤드 없이 실제 데이터베이스에 연결할 대략적으로 보여 주는 항목을 사용 하는 구성 요소를 테스트 하려는 경우에 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="21624-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="21624-104">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21624-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="21624-105">InMemory 관계형 데이터베이스가 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="21624-105">InMemory is not a relational database</span></span>

<span data-ttu-id="21624-106">EF Core 데이터베이스 공급자는 관계형 데이터베이스를 사용할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="21624-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="21624-107">InMemory 테스트에 대 한 범용 데이터베이스 되도록 설계 되었습니다 및 관계형 데이터베이스를 모방 하지는 못합니다.</span><span class="sxs-lookup"><span data-stu-id="21624-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="21624-108">몇 가지 예가이 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="21624-108">Some examples of this include:</span></span>

* <span data-ttu-id="21624-109">InMemory를 사용 하면 관계형 데이터베이스의 참조 무결성 제약 조건을 위반 하는 데이터를 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21624-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="21624-110">DefaultValueSql(string) 속성에 대 한 모델에서을 사용 하면 관계형 데이터베이스 API 이며 InMemory에 대해 실행할 때 영향을 주지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="21624-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="21624-111">[타임 스탬프/행 버전을 통해 동시성](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` 또는 `IsRowVersion`) 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="21624-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="21624-112">더 [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) 업데이트가 수행 되는 경우 throw 됩니다 이전 동시성 토큰을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="21624-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="21624-113">여러 테스트 목적에 대 한 이러한 차이점은 중요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="21624-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="21624-114">그러나 true 관계형 데이터베이스 처럼 작동 하는 것에 대해 테스트 하려는 경우 다음 사용해 보십시오 [SQLite 메모리 내 모드](sqlite.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="21624-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="21624-115">예제 테스트 시나리오</span><span class="sxs-lookup"><span data-stu-id="21624-115">Example testing scenario</span></span>

<span data-ttu-id="21624-116">다음 서비스를 블로그와 관련 된 몇 가지 작업을 수행 하는 응용 프로그램 코드를 허용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="21624-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="21624-117">사용 하 여 내부적으로 `DbContext` SQL Server 데이터베이스에 연결 하는 합니다.</span><span class="sxs-lookup"><span data-stu-id="21624-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="21624-118">많은 테스트를 만드는 작업을 수행 하거나 코드를 수정 하지 않고이 서비스에 대 한 효율적인 테스트를 작성할 수 있도록 InMemory 데이터베이스에 연결 하려면이 컨텍스트를 교환 하려면 유용 컨텍스트의 double입니다.</span><span class="sxs-lookup"><span data-stu-id="21624-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="21624-119">컨텍스트 준비</span><span class="sxs-lookup"><span data-stu-id="21624-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="21624-120">두 데이터베이스 공급자를 구성 하지 않으려면</span><span class="sxs-lookup"><span data-stu-id="21624-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="21624-121">테스트에서 외부적으로 InMemory 공급자를 사용 하도록 컨텍스트를 구성 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="21624-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="21624-122">데이터베이스 공급자를 재정의 하 여 구성 하는 경우 `OnConfiguring` 프로그램 컨텍스트에서 해야 이미 구성 되지 않은 한 경우에 데이터베이스 공급자를 구성 하는 일부 조건부 코드를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="21624-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="21624-123">ASP.NET Core를 사용 하는 경우 다음 필요가 없습니다이 코드에 데이터베이스 공급자 (Startup.cs)의 컨텍스트 외부에서 이미 구성 되었기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="21624-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="21624-124">테스트에 대 한 생성자를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="21624-124">Add a constructor for testing</span></span>

<span data-ttu-id="21624-125">가장 간단한 방법은 다른 데이터베이스에 대해 테스트할 수 있도록 허용 하는 생성자를 노출 하 여 컨텍스트를 수정 하는 것을 `DbContextOptions<TContext>`입니다.</span><span class="sxs-lookup"><span data-stu-id="21624-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="21624-126">`DbContextOptions<TContext>` 모든 데이터베이스에 연결 하는 등 설정을 컨텍스트를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="21624-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="21624-127">이 컨텍스트에서 OnConfiguring 메서드를 실행 하 여 기본 제공 되는 동일한 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="21624-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="21624-128">테스트 작성</span><span class="sxs-lookup"><span data-stu-id="21624-128">Writing tests</span></span>

<span data-ttu-id="21624-129">이 공급자를 사용 하 여 테스트 키가 InMemory 공급자를 사용 하면 메모리 내 데이터베이스의 범위를 제어 하는 컨텍스트를 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="21624-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="21624-130">일반적으로 각 테스트 메서드에 대 한 데이터베이스를 정리 합니다.</span><span class="sxs-lookup"><span data-stu-id="21624-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="21624-131">InMemory 데이터베이스를 사용 하는 테스트 클래스의 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="21624-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="21624-132">각 테스트 메서드는 각 메서드는 자체 InMemory 데이터베이스 고유 데이터베이스 이름을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="21624-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="21624-133">사용 하 여 `.UseInMemoryDatabase()` 확장 메서드, NuGet 패키지 참조 `Microsoft.EntityFrameworkCore.InMemory`합니다.</span><span class="sxs-lookup"><span data-stu-id="21624-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
