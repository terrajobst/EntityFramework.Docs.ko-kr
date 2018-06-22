---
title: 연결 복원 력-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: aec69577cd4b19fdebedb33ed6fd8f2665b0a032
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053533"
---
# <a name="connection-resiliency"></a><span data-ttu-id="f5e53-102">연결 복원력</span><span class="sxs-lookup"><span data-stu-id="f5e53-102">Connection Resiliency</span></span>

<span data-ttu-id="f5e53-103">연결 복원에 실패 한 데이터베이스 명령을 자동으로 다시 시도합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="f5e53-104">제공 된 "실행" 하는 전략을 오류를 감지 하 고 명령을 다시 시도 하는 데 필요한 논리를 캡슐화 하 여 모든 데이터베이스와 기능을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="f5e53-105">EF 핵심 공급자는 해당 특정 데이터베이스 오류 조건 및 최적의 다시 시도 정책에 맞게 실행 전략을 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="f5e53-106">예를 들어, SQL Server 공급자는 SQL Server (SQL Azure 등) 하기 위해 특별히 설계 되는 실행 전략을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="f5e53-107">다시 시도할 수 있는 예외 유형을 확인 하 고 있어서 최대 재시도 횟수, 재시도 등의 간격에 대 한 실제 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="f5e53-108">실행 전략 여 컨텍스트에 대 한 옵션을 구성할 때 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="f5e53-109">이 일반적으로는 `OnConfiguring` 또는 파생 컨텍스트의 메서드 `Startup.cs` ASP.NET Core 응용 프로그램에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-109">This is typically in the `OnConfiguring` method of your derived context, or in `Startup.cs` for an ASP.NET Core application.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a><span data-ttu-id="f5e53-110">사용자 지정 실행 전략</span><span class="sxs-lookup"><span data-stu-id="f5e53-110">Custom execution strategy</span></span>

<span data-ttu-id="f5e53-111">자신만의 기본값을 변경 하려는 경우 사용자 지정 실행 전략을 등록 하는 메커니즘이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-111">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="f5e53-112">실행 전략 및 트랜잭션</span><span class="sxs-lookup"><span data-stu-id="f5e53-112">Execution strategies and transactions</span></span>

<span data-ttu-id="f5e53-113">오류에 자동으로 다시 시도 하는 실행 전략 각 작업에 실패 하는 재시도 블록에서 재생 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-113">An execution strategy that automatically retries on failures needs to be able to play back each operation in an retry block that fails.</span></span> <span data-ttu-id="f5e53-114">EF 코어를 통해 수행 하는 각 작업 즉, 각 쿼리 및를 호출할 때마다 다시 시도할 수는 자체 작업이 됩니다. 다시 시도 설정 되 면 `SaveChanges()` 일시적인 오류가 발생 하는 경우 하나의 단위로 재시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-114">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation, i.e. each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="f5e53-115">그러나 코드를 사용 하 여 트랜잭션을 시작 하는 경우 `BeginTransaction()` 하나의 단위로 처리 해야 하는 작업의 사용자 정의 그룹을 정의 하는, 즉, 트랜잭션 내의 모든 항목이 다시 재생할 수는 오류가 발생 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-115">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, i.e. everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="f5e53-116">실행 전략을 사용 하는 경우이 작업을 수행 하려고 하면 다음과 같은 예외를 받습니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-116">You will receive an exception like the following if you attempt to do this when using an execution strategy.</span></span>

> <span data-ttu-id="f5e53-117">InvalidOperationException: 구성 된 실행 전략 'SqlServerRetryingExecutionStrategy' 사용자가 시작한 트랜잭션을 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-117">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="f5e53-118">'DbContext.Database.CreateExecutionStrategy()'에서 반환 된 실행 전략을 사용 하 여 트랜잭션을 다시 시도할 수 한 단위로 모든 작업을 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-118">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="f5e53-119">해결 방법은 수동으로 실행 해야 하는 모든 항목을 나타내는 대리자를 사용 하 여 실행 전략을 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-119">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="f5e53-120">일시적인 오류가 발생 하는 경우 실행 전략 대리자를 다시 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-120">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="f5e53-121">트랜잭션 커밋 실패 및 멱 문제</span><span class="sxs-lookup"><span data-stu-id="f5e53-121">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="f5e53-122">일반적으로 연결 오류가 있는 경우 현재 트랜잭션이 롤백됩니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-122">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="f5e53-123">그러나 트랜잭션이 된 상태는 연결이 끊어질 경우 되 고 커밋된 결과로 생성 된 트랜잭션 상태를 알 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-123">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> <span data-ttu-id="f5e53-124">이 참조 [블로그 게시물](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) 내용을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-124">See this [blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) for more details.</span></span>

<span data-ttu-id="f5e53-125">기본적으로는 실행 전략은 작업을 다시 시도 트랜잭션이 롤백 되었습니다 있지만 그렇지 않은 경우 경우 이렇게 하면 예외가 새 데이터베이스 상태 호환 되지 않거나 시킬 수 있는 경우 **데이터 손상** 경우는 작업 예를 들어 자동 생성 된 키 값을 가진 새 행을 삽입할 때 특정 상태에 의존 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-125">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="f5e53-126">이 처리 하는 방법은 여러 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-126">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="f5e53-127">옵션 1-Do 거의 대부분 아무것도 표시 되지 않음</span><span class="sxs-lookup"><span data-stu-id="f5e53-127">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="f5e53-128">이 상태는 실제로 발생 하는 경우 작동이 실패 하도록 응용 프로그램에 대 한 허용 되므로 트랜잭션 커밋하는 동안 연결 오류가 발생할 가능성이 낮습니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-128">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="f5e53-129">그러나 중복 행을 추가 하는 대신 예외가 발생을 보장 하기 위해 저장소 생성 키를 사용 하지 않도록 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-129">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="f5e53-130">클라이언트에서 생성 된 GUID 값 또는 클라이언트 쪽 값 생성기를 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-130">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="f5e53-131">옵션 2-Rebuild 응용 프로그램 상태</span><span class="sxs-lookup"><span data-stu-id="f5e53-131">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="f5e53-132">현재 삭제 `DbContext`합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-132">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="f5e53-133">새 `DbContext` 데이터베이스에서 응용 프로그램의 상태를 복원 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-133">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="f5e53-134">마지막 작업 수 완료 되지 않은 성공적으로 사용자에 게 알립니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-134">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="f5e53-135">옵션 3-상태 확인 추가</span><span class="sxs-lookup"><span data-stu-id="f5e53-135">Option 3 - Add state verification</span></span>

<span data-ttu-id="f5e53-136">대부분의 데이터베이스 상태를 변경 하는 작업 성공 여부를 확인 하는 코드를 추가 하는 것이 같습니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-136">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="f5e53-137">-더 쉽게 확인 하는 확장 메서드를 제공 하는 EF `IExecutionStrategy.ExecuteInTransaction`합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-137">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="f5e53-138">시작 하 고 트랜잭션을 커밋합니다 및 에서도의 함수를 적용이 메서드는 `verifySucceeded` 트랜잭션 커밋 중에 일시적인 오류가 발생할 때 호출 되는 매개 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-138">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="f5e53-139">여기 `SaveChanges` 와 함께 호출 되는지 `acceptAllChangesOnSuccess` 로 설정 `false` 의 상태를 변경 하지 않으려면는 `Blog` 엔터티를 `Unchanged` 경우 `SaveChanges` 성공 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-139">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="f5e53-140">따라서 커밋에 실패 하 고 트랜잭션이 롤백 동일한 작업을 다시 시도를 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-140">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="f5e53-141">옵션 4-트랜잭션을 수동으로 추적</span><span class="sxs-lookup"><span data-stu-id="f5e53-141">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="f5e53-142">저장소 생성 키를 사용 하거나 수행 되는 작업에 의존 하지 않는 제네릭 방법이 커밋 실패 처리 필요한 해야 할 경우 각 트랜잭션 커밋이 실패 하면 체크 인 된 ID는 할당할 수 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-142">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="f5e53-143">트랜잭션의 상태를 추적 하는 데 사용 되는 데이터베이스에 테이블을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-143">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="f5e53-144">각 트랜잭션의 시작 부분에 있는 테이블에 행을 삽입 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-144">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="f5e53-145">커밋 연결에 실패 하면 데이터베이스에서 해당 행이 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-145">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="f5e53-146">커밋에 성공 하면 테이블의 증가 방지 하기 위해 해당 행을 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-146">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="f5e53-147">확인에 사용 되는 컨텍스트 연결이 트랜잭션 커밋 중 실패 한 경우 다시 확인 하는 동안 실패할 수로 정의 하는 실행 전략에 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="f5e53-147">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
