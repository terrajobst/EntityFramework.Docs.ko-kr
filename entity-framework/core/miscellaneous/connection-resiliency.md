---
title: 연결 복원 력-EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: 07646e6ead845c38537945a03367ac7f50784236
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414136"
---
# <a name="connection-resiliency"></a><span data-ttu-id="8488b-102">연결 복원력</span><span class="sxs-lookup"><span data-stu-id="8488b-102">Connection Resiliency</span></span>

<span data-ttu-id="8488b-103">연결 복원 력을 실패 한 데이터베이스 명령을 자동으로 다시 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="8488b-104">이 기능은 실패를 검색 하 고 명령을 다시 시도 하는 데 필요한 논리를 캡슐화 하는 "실행 전략"을 제공 하 여 모든 데이터베이스에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="8488b-105">EF Core 공급자는 특정 데이터베이스 오류 조건 및 최적의 재시도 정책에 맞는 실행 전략을 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="8488b-106">예를 들어 SQL Server 공급자에는 SQL Server에 맞게 특별히 조정 된 실행 전략 (SQL Azure 포함)이 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="8488b-107">다시 시도할 수 있는 예외 유형과 최대 다시 시도 횟수, 다시 시도 간 지연 등의 적절 한 기본값을 가진 예외 유형을 인식 합니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="8488b-108">컨텍스트에 대 한 옵션을 구성할 때 실행 전략이 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="8488b-109">이는 일반적으로 파생 컨텍스트의 `OnConfiguring` 메서드에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-109">This is typically in the `OnConfiguring` method of your derived context:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

<span data-ttu-id="8488b-110">또는 `Startup.cs` ASP.NET Core 응용 프로그램:</span><span class="sxs-lookup"><span data-stu-id="8488b-110">or in `Startup.cs` for an ASP.NET Core application:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<PicnicContext>(
        options => options.UseSqlServer(
            "<connection string>",
            providerOptions => providerOptions.EnableRetryOnFailure()));
}
```

## <a name="custom-execution-strategy"></a><span data-ttu-id="8488b-111">사용자 지정 실행 전략</span><span class="sxs-lookup"><span data-stu-id="8488b-111">Custom execution strategy</span></span>

<span data-ttu-id="8488b-112">기본값을 변경 하려는 경우 고유한 사용자 지정 실행 전략을 등록 하는 메커니즘이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-112">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="8488b-113">실행 전략 및 트랜잭션</span><span class="sxs-lookup"><span data-stu-id="8488b-113">Execution strategies and transactions</span></span>

<span data-ttu-id="8488b-114">오류 시 자동으로 다시 시도 하는 실행 전략은 실패 한 재시도 블록에서 각 작업을 재생할 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-114">An execution strategy that automatically retries on failures needs to be able to play back each operation in a retry block that fails.</span></span> <span data-ttu-id="8488b-115">다시 시도를 사용 하도록 설정 하면 EF Core을 통해 수행 하는 각 작업이 자체 다시 시도 가능 작업이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-115">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation.</span></span> <span data-ttu-id="8488b-116">즉, 일시적인 오류가 발생 하는 경우 각 쿼리 및 `SaveChanges()`에 대 한 각 호출은 하나의 단위로 다시 시도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-116">That is, each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="8488b-117">그러나 코드가 `BeginTransaction()`를 사용 하 여 트랜잭션을 시작 하는 경우 하나의 단위로 처리 해야 하는 사용자 고유의 작업 그룹을 정의 하 고 트랜잭션 내의 모든 항목을 재생 해야 하는 경우 오류가 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-117">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, and everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="8488b-118">실행 전략을 사용할 때이 작업을 수행 하려고 하면 다음과 같은 예외가 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-118">You will receive an exception like the following if you attempt to do this when using an execution strategy:</span></span>

> <span data-ttu-id="8488b-119">InvalidOperationException: 구성 된 실행 전략 ' SqlServerRetryingExecutionStrategy '은 사용자가 시작한 트랜잭션을 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-119">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="8488b-120">'DbContext.Database.CreateExecutionStrategy()'에서 반환된 실행 전략을 사용하여 트랜잭션을 다시 시도가 가능한 단위로 트랜잭션의 모든 작업을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-120">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="8488b-121">이 솔루션은 실행 해야 하는 모든 항목을 나타내는 대리자를 사용 하 여 실행 전략을 수동으로 호출 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-121">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="8488b-122">일시적인 오류가 발생하면, 실행 전략에서 대리자를 다시 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-122">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

<span data-ttu-id="8488b-123">이 방법은 앰비언트 트랜잭션과 함께 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-123">This approach can also be used with ambient transactions.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#AmbientTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="8488b-124">트랜잭션 커밋 실패 및 멱 등 성 문제</span><span class="sxs-lookup"><span data-stu-id="8488b-124">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="8488b-125">일반적으로 연결 오류가 발생 하면 현재 트랜잭션이 롤백됩니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-125">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="8488b-126">그러나 트랜잭션을 커밋하는 동안 연결이 끊어진 경우 트랜잭션 결과 상태를 알 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-126">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> 

<span data-ttu-id="8488b-127">기본적으로 실행 전략은 트랜잭션이 롤백된 것 처럼 작업을 다시 시도 하지만 그렇지 않은 경우에는 새 데이터베이스 상태가 호환 되지 않는 경우 예외가 발생 하거나, 자동 생성 된 키 값이 있는 새 행을 삽입 하는 등의 작업에서 특정 상태를 사용 하지 않는 경우 **데이터가 손상** 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-127">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="8488b-128">여러 가지 방법으로이를 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-128">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="8488b-129">옵션 1-전혀 하지 않음 (거의)</span><span class="sxs-lookup"><span data-stu-id="8488b-129">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="8488b-130">트랜잭션 커밋 중에 연결 오류가 발생 하는 경우이 상태가 실제로 발생 하는 경우 응용 프로그램이 실패할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-130">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="8488b-131">그러나 중복 행을 추가 하는 대신 예외가 throw 되도록 하기 위해 저장소에서 생성 된 키를 사용 하지 않도록 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-131">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="8488b-132">클라이언트에서 생성 된 GUID 값 또는 클라이언트 쪽 값 생성기를 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-132">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="8488b-133">옵션 2-응용 프로그램 상태 다시 빌드</span><span class="sxs-lookup"><span data-stu-id="8488b-133">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="8488b-134">현재 `DbContext`을 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-134">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="8488b-135">새 `DbContext`을 만들고 데이터베이스에서 응용 프로그램의 상태를 복원 합니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-135">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="8488b-136">마지막 작업이 성공적으로 완료 되지 않았을 수 있음을 사용자에 게 알립니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-136">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="8488b-137">옵션 3-상태 확인 추가</span><span class="sxs-lookup"><span data-stu-id="8488b-137">Option 3 - Add state verification</span></span>

<span data-ttu-id="8488b-138">데이터베이스 상태를 변경 하는 대부분의 작업에 대해 성공 여부를 확인 하는 코드를 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-138">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="8488b-139">EF는이 작업을 보다 쉽게 `IExecutionStrategy.ExecuteInTransaction`하는 확장 메서드를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-139">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="8488b-140">이 메서드는 트랜잭션을 시작 하 고 커밋하고 트랜잭션 커밋 중에 일시적인 오류가 발생할 때 호출 되는 `verifySucceeded` 매개 변수 에서도 함수를 수락 합니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-140">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="8488b-141">`Unchanged` 성공 하는 경우 `Blog` 엔터티의 상태를 `SaveChanges`으로 변경 하지 않도록 `acceptAllChangesOnSuccess`를 `false`로 설정 하 여이 `SaveChanges` 호출 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-141">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="8488b-142">이렇게 하면 커밋이 실패 하 고 트랜잭션이 롤백되는 경우 동일한 작업을 다시 시도할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-142">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="8488b-143">옵션 4-수동으로 트랜잭션 추적</span><span class="sxs-lookup"><span data-stu-id="8488b-143">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="8488b-144">저장소에서 생성 된 키를 사용 해야 하는 경우 또는 수행 된 작업에 의존 하지 않는 커밋 실패를 처리 하는 일반적인 방법이 필요한 경우 커밋에 실패할 때 확인 되는 ID를 각 트랜잭션에 할당할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-144">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="8488b-145">트랜잭션 상태를 추적 하는 데 사용 되는 데이터베이스에 테이블을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-145">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="8488b-146">각 트랜잭션의 시작 부분에서 테이블에 행을 삽입 합니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-146">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="8488b-147">커밋하는 동안 연결이 실패 하는 경우 데이터베이스에 해당 하는 행이 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-147">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="8488b-148">커밋이 성공한 경우 테이블의 증가를 방지 하려면 해당 행을 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-148">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="8488b-149">확인 하는 데 사용 되는 컨텍스트에 트랜잭션 커밋 중에 오류가 발생 한 경우이를 확인 하는 동안 연결이 다시 실패할 가능성이 있으므로 실행 전략을 정의 했는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="8488b-149">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
