---
title: "InMemory-EF 코어를 사용 하 여 테스트"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 33690e3424d0777930d3cb8167575fb0f4ddd8f7
ms.sourcegitcommit: d096484dcf9eff73d9943fa60db7a418b10ca0b3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/22/2018
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="6b8e3-102">InMemory를 사용 하 여 테스트</span><span class="sxs-lookup"><span data-stu-id="6b8e3-102">Testing with InMemory</span></span>

<span data-ttu-id="6b8e3-103">InMemory 공급자 접근 실제 데이터베이스 작업의 오버 헤드 없이 실제 데이터베이스에 연결 하는 수준의 암호화를 사용 하는 구성 요소를 테스트 하려는 경우 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="6b8e3-104">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="6b8e3-105">InMemory 관계형 데이터베이스가 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-105">InMemory is not a relational database</span></span>

<span data-ttu-id="6b8e3-106">EF 핵심 데이터베이스 공급자는 관계형 데이터베이스를 사용할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="6b8e3-107">InMemory는 테스트를 위해 일반 용도의 데이터베이스 되도록 설계 및 관계형 데이터베이스와 유사 하 게 설계 되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="6b8e3-108">이 다음과의 몇 가지 예:</span><span class="sxs-lookup"><span data-stu-id="6b8e3-108">Some examples of this include:</span></span>
* <span data-ttu-id="6b8e3-109">InMemory를 사용 하면 관계형 데이터베이스의 참조 무결성 제약 조건을 위반 하는 데이터를 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>

* <span data-ttu-id="6b8e3-110">모델의 속성에 대 한 DefaultValueSql(string)를 사용 하는 경우 관계형 데이터베이스 API 이므로 InMemory에 대해 실행할 때 영향을 미치지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>

> [!TIP]  
> <span data-ttu-id="6b8e3-111">목적으로 테스트에 대 한 이러한 차이 중요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-111">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="6b8e3-112">그러나 true 관계형 데이터베이스 처럼 작동 하는 것에 대해 테스트 하려는 경우 다음 사용을 고려해 [SQLite 메모리 내 모드](sqlite.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-112">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="6b8e3-113">테스트 시나리오의 예</span><span class="sxs-lookup"><span data-stu-id="6b8e3-113">Example testing scenario</span></span>

<span data-ttu-id="6b8e3-114">서비스를 응용 프로그램 코드 블로그와 관련 된 많은 작업을 수행 하도록 허용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-114">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="6b8e3-115">내부적으로 사용 하는 `DbContext` SQL Server 데이터베이스에 연결 하 합니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-115">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="6b8e3-116">코드를 수정 하지 않고이 서비스에 대 한 효율적인 테스트를 작성 하거나 많은 테스트를 만드는 작업을 수행할 수 있도록 InMemory 데이터베이스에 연결 하기 위한이 컨텍스트와 바꿔 유용할 컨텍스트의 double입니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-116">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="6b8e3-117">컨텍스트 사용 준비</span><span class="sxs-lookup"><span data-stu-id="6b8e3-117">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="6b8e3-118">두 데이터베이스 공급자를 구성 하지 않으려면</span><span class="sxs-lookup"><span data-stu-id="6b8e3-118">Avoid configuring two database providers</span></span>

<span data-ttu-id="6b8e3-119">테스트에서 InMemory 공급자를 사용 하는 컨텍스트 외부에서 구성 하려는 합니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-119">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="6b8e3-120">재정의 하 여 데이터베이스 공급자를 구성 하는 경우 `OnConfiguring` 사용자 컨텍스트에서 다음 필요한 이미 구성 되지 않은 한 경우에 데이터베이스 공급자를 구성 하는 수 있도록 일부 조건부 코드를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-120">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="6b8e3-121">ASP.NET Core를 사용 하는 경우 다음 않아도이 코드 데이터베이스 공급자 (Startup.cs)의 컨텍스트 외부에서 이미 구성 되어 있으므로 합니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-121">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="6b8e3-122">테스트에 대 한 생성자를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-122">Add a constructor for testing</span></span>

<span data-ttu-id="6b8e3-123">가장 간단한 방법은 다른 데이터베이스에 대해 테스트할 수 있도록 허용 하는 생성자를 노출 하 여 컨텍스트를 수정 하는 것을 `DbContextOptions<TContext>`합니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-123">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="6b8e3-124">`DbContextOptions<TContext>`에 연결 하는 데이터베이스와 같은 해당 설정의 모든 컨텍스트를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-124">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="6b8e3-125">사용자 컨텍스트에서 OnConfiguring 메서드를 실행 하 여 작성 되는 동일한 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-125">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="6b8e3-126">테스트 작성</span><span class="sxs-lookup"><span data-stu-id="6b8e3-126">Writing tests</span></span>

<span data-ttu-id="6b8e3-127">이 공급자를 사용 하 여 테스트 하는 키 InMemory 공급자를 사용 하기 위해 메모리 내 데이터베이스의 범위 제어 컨텍스트를 알 수 있다는 점입니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-127">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="6b8e3-128">보통 각 테스트 메서드에 대 한 데이터베이스 정리 합니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-128">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="6b8e3-129">InMemory 데이터베이스를 사용 하는 테스트 클래스의 예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-129">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="6b8e3-130">각 테스트 메서드는 각 방법에는 고유한 InMemory 데이터베이스를 의미 하 고 고유한 데이터베이스 이름을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-130">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="6b8e3-131">사용 하 여 `.UseInMemoryDatabase()` 확장 메서드를 참조 NuGet 패키지 `Microsoft.EntityFrameworkCore.InMemory`합니다.</span><span class="sxs-lookup"><span data-stu-id="6b8e3-131">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
