---
title: SQLite로 테스트-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: f7f847d8c766c0d4d7577ea6760ee72a17f84933
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414634"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="e7a4f-102">SQLite 테스트</span><span class="sxs-lookup"><span data-stu-id="e7a4f-102">Testing with SQLite</span></span>

<span data-ttu-id="e7a4f-103">SQLite에는 실제 데이터베이스 작업의 오버 헤드 없이 SQLite를 사용 하 여 관계형 데이터베이스에 대 한 테스트를 작성할 수 있게 해 주는 메모리 내 모드가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7a4f-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="e7a4f-104">GitHub에서이 문서의 [샘플](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) 을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7a4f-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="e7a4f-105">예제 테스트 시나리오</span><span class="sxs-lookup"><span data-stu-id="e7a4f-105">Example testing scenario</span></span>

<span data-ttu-id="e7a4f-106">응용 프로그램 코드에서 블로그와 관련 된 일부 작업을 수행할 수 있도록 하는 다음 서비스를 고려 하십시오.</span><span class="sxs-lookup"><span data-stu-id="e7a4f-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="e7a4f-107">내부적으로는 SQL Server 데이터베이스에 연결 하는 `DbContext`을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7a4f-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="e7a4f-108">이 컨텍스트를 바꿔 메모리 내 SQLite 데이터베이스에 연결 하 여 코드를 수정 하지 않고도이 서비스에 대 한 효율적인 테스트를 작성 하거나 컨텍스트의 테스트 double을 만들기 위해 많은 작업을 수행 하는 것이 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7a4f-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="e7a4f-109">컨텍스트 준비</span><span class="sxs-lookup"><span data-stu-id="e7a4f-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="e7a4f-110">두 데이터베이스 공급자를 구성 하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="e7a4f-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="e7a4f-111">테스트에서 InMemory 공급자를 사용 하도록 컨텍스트를 외부적으로 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7a4f-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="e7a4f-112">컨텍스트에서 `OnConfiguring`를 재정의 하 여 데이터베이스 공급자를 구성 하는 경우 데이터베이스 공급자가 아직 구성 되지 않은 경우에만 구성 되도록 일부 조건부 코드를 추가 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7a4f-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="e7a4f-113">ASP.NET Core를 사용 하는 경우 데이터베이스 공급자가 컨텍스트 외부 (Startup.cs)에서 구성 되었으므로이 코드가 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e7a4f-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="e7a4f-114">테스트를 위한 생성자 추가</span><span class="sxs-lookup"><span data-stu-id="e7a4f-114">Add a constructor for testing</span></span>

<span data-ttu-id="e7a4f-115">다른 데이터베이스에 대해 테스트를 사용 하도록 설정 하는 가장 간단한 방법은 `DbContextOptions<TContext>`을 허용 하는 생성자를 노출 하도록 컨텍스트를 수정 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="e7a4f-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="e7a4f-116">`DbContextOptions<TContext>`은 연결할 데이터베이스와 같은 모든 설정에 컨텍스트를 알려 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7a4f-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="e7a4f-117">이는 컨텍스트에서 OnConfiguring 메서드를 실행 하 여 작성 된 것과 동일한 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="e7a4f-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="e7a4f-118">테스트 작성</span><span class="sxs-lookup"><span data-stu-id="e7a4f-118">Writing tests</span></span>

<span data-ttu-id="e7a4f-119">이 공급자를 사용 하 여 테스트 하는 열쇠는 컨텍스트에 SQLite를 사용 하도록 지시 하 고 메모리 내 데이터베이스의 범위를 제어 하는 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="e7a4f-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="e7a4f-120">데이터베이스의 범위는 연결을 열고 닫는 방법으로 제어 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7a4f-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="e7a4f-121">데이터베이스는 연결이 열려 있는 기간으로 범위가 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7a4f-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="e7a4f-122">일반적으로 각 테스트 메서드에 대해 완전히 정리 된 데이터베이스를 원합니다.</span><span class="sxs-lookup"><span data-stu-id="e7a4f-122">Typically you want a clean database for each test method.</span></span>

>[!TIP]
> <span data-ttu-id="e7a4f-123">`SqliteConnection()` 및 `.UseSqlite()` 확장 메서드를 사용 하려면 NuGet 패키지 [microsoft.entityframeworkcore.tools.dotnet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="e7a4f-123">To use `SqliteConnection()` and the `.UseSqlite()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
