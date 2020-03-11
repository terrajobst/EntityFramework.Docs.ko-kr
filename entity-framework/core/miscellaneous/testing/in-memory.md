---
title: InMemory를 사용 하 여 테스트-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 18641677098c20d9172136b07868dcb647d189c6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414028"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="f0531-102">InMemory 테스트</span><span class="sxs-lookup"><span data-stu-id="f0531-102">Testing with InMemory</span></span>

<span data-ttu-id="f0531-103">InMemory 공급자는 실제 데이터베이스 작업의 오버 헤드 없이 실제 데이터베이스에 연결 하는 것과 유사한 항목을 사용 하 여 구성 요소를 테스트 하려는 경우에 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="f0531-104">GitHub에서 이 문서의 [샘플](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="f0531-105">InMemory가 관계형 데이터베이스가 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-105">InMemory is not a relational database</span></span>

<span data-ttu-id="f0531-106">EF Core 데이터베이스 공급자는 관계형 데이터베이스가 될 필요는 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="f0531-107">InMemory는 테스트를 위한 범용 데이터베이스로 설계 되었으며 관계형 데이터베이스를 모방 하도록 설계 되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="f0531-108">이에 대 한 몇 가지 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-108">Some examples of this include:</span></span>

* <span data-ttu-id="f0531-109">InMemory를 통해 관계형 데이터베이스에서 참조 무결성 제약 조건을 위반 하는 데이터를 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="f0531-110">모델의 속성에 DefaultValueSql (string)을 사용 하는 경우이는 관계형 데이터베이스 API 이며 InMemory에 대해 실행 하는 경우에는 영향을 주지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="f0531-111">[Timestamp/row 버전](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` 또는 `IsRowVersion`)을 통한 동시성은 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="f0531-112">이전 동시성 토큰을 사용 하 여 업데이트를 수행 하는 경우 [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) 이 throw 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="f0531-113">많은 테스트를 위해 이러한 차이가 중요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="f0531-114">그러나 진정한 관계형 데이터베이스 처럼 동작 하는 항목을 테스트 하려면 [SQLite 메모리 내 모드](sqlite.md)를 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="f0531-115">예제 테스트 시나리오</span><span class="sxs-lookup"><span data-stu-id="f0531-115">Example testing scenario</span></span>

<span data-ttu-id="f0531-116">응용 프로그램 코드에서 블로그와 관련 된 일부 작업을 수행할 수 있도록 하는 다음 서비스를 고려 하십시오.</span><span class="sxs-lookup"><span data-stu-id="f0531-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="f0531-117">내부적으로는 SQL Server 데이터베이스에 연결 하는 `DbContext`을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="f0531-118">코드를 수정 하거나 컨텍스트의 테스트 double을 만들기 위해 많은 작업을 수행 하지 않고도이 서비스에 대 한 효율적인 테스트를 작성할 수 있도록이 컨텍스트를 바꿔 InMemory 데이터베이스에 연결 하는 것이 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="f0531-119">컨텍스트 준비</span><span class="sxs-lookup"><span data-stu-id="f0531-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="f0531-120">두 데이터베이스 공급자를 구성 하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="f0531-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="f0531-121">테스트에서 InMemory 공급자를 사용 하도록 컨텍스트를 외부적으로 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="f0531-122">컨텍스트에서 `OnConfiguring`를 재정의 하 여 데이터베이스 공급자를 구성 하는 경우 데이터베이스 공급자가 아직 구성 되지 않은 경우에만 구성 되도록 일부 조건부 코드를 추가 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="f0531-123">ASP.NET Core를 사용 하는 경우 데이터베이스 공급자가 컨텍스트 외부 (Startup.cs)에서 이미 구성 되었으므로이 코드가 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="f0531-124">테스트를 위한 생성자 추가</span><span class="sxs-lookup"><span data-stu-id="f0531-124">Add a constructor for testing</span></span>

<span data-ttu-id="f0531-125">다른 데이터베이스에 대해 테스트를 사용 하도록 설정 하는 가장 간단한 방법은 `DbContextOptions<TContext>`을 허용 하는 생성자를 노출 하도록 컨텍스트를 수정 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="f0531-126">`DbContextOptions<TContext>`은 연결할 데이터베이스와 같은 모든 설정에 컨텍스트를 알려 줍니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="f0531-127">이는 컨텍스트에서 OnConfiguring 메서드를 실행 하 여 작성 된 것과 동일한 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="f0531-128">테스트 작성</span><span class="sxs-lookup"><span data-stu-id="f0531-128">Writing tests</span></span>

<span data-ttu-id="f0531-129">이 공급자를 사용 하 여 테스트 하는 핵심은 InMemory 공급자를 사용 하도록 컨텍스트에 지시 하 고 메모리 내 데이터베이스의 범위를 제어 하는 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="f0531-130">일반적으로 각 테스트 메서드에 대해 완전히 정리 된 데이터베이스를 원합니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="f0531-131">다음은 InMemory 데이터베이스를 사용 하는 테스트 클래스의 예제입니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="f0531-132">각 테스트 메서드는 고유한 데이터베이스 이름을 지정 합니다. 즉, 각 메서드에 고유한 InMemory 데이터베이스가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f0531-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="f0531-133">`.UseInMemoryDatabase()` 확장 메서드를 사용 하려면 NuGet 패키지 [microsoft.entityframeworkcore.tools.dotnet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="f0531-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
