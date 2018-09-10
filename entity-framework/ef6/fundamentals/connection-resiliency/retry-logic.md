---
title: 연결 복원 력 및 다시 시도 논리-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: d7e58abfa17c5537cdc9b0068e7c2a3c2e390038
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250520"
---
# <a name="connection-resiliency-and-retry-logic"></a><span data-ttu-id="c99fd-102">연결 복원 력 및 다시 시도 논리</span><span class="sxs-lookup"><span data-stu-id="c99fd-102">Connection resiliency and retry logic</span></span>
> [!NOTE]
> <span data-ttu-id="c99fd-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="c99fd-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="c99fd-105">데이터베이스 서버에 연결 하는 응용 프로그램 백 엔드에서 실패 및 네트워크 불안정으로 인해 연결 중단에 취약 항상 되었을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-105">Applications connecting to a database server have always been vulnerable to connection breaks due to back-end failures and network instability.</span></span> <span data-ttu-id="c99fd-106">그러나 전용된 데이터베이스 서버에 대해 작업 기반 LAN 환경에서 이러한 오류는 드물게 충분히 이러한 오류를 처리 하는 추가 논리가 필요 하지 않음을 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-106">However, in a LAN based environment working against dedicated database servers these errors are rare enough that extra logic to handle those failures is not often required.</span></span> <span data-ttu-id="c99fd-107">클라우드의 증가 함에 따라 덜 안정적인 네트워크 연결 중단 발생에 대 한 보다 일반적인 되었습니다를 통해 Windows Azure SQL Database와 같은 데이터베이스 서버 및 연결 기반 합니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-107">With the rise of cloud based database servers such as Windows Azure SQL Database and connections over less reliable networks it is now more common for connection breaks to occur.</span></span> <span data-ttu-id="c99fd-108">이 일시적인 시간 제한 및 기타 일시적인 오류를 일으키는 네트워크 불안정 또는 연결 제한 등 서비스의 공정성 보장을 사용 하 여 데이터베이스를 클라우드 방어 기술 때문일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-108">This could be due to defensive techniques that cloud databases use to ensure fairness of service, such as connection throttling, or to instability in the network causing intermittent timeouts and other transient errors.</span></span>  

<span data-ttu-id="c99fd-109">연결 복원 력 자동으로 이러한 연결 중단으로 인해 실패 하는 명령을 다시 시도 하는 EF에 대 한 기능을 말합니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-109">Connection Resiliency refers to the ability for EF to automatically retry any commands that fail due to these connection breaks.</span></span>  

## <a name="execution-strategies"></a><span data-ttu-id="c99fd-110">실행 전략</span><span class="sxs-lookup"><span data-stu-id="c99fd-110">Execution Strategies</span></span>  

<span data-ttu-id="c99fd-111">연결 다시 시도 IDbExecutionStrategy 인터페이스의 구현에 의해 처리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-111">Connection retry is taken care of by an implementation of the IDbExecutionStrategy interface.</span></span> <span data-ttu-id="c99fd-112">IDbExecutionStrategy 구현 작업을 수락 하 고 예외가 발생 하는 경우 다시 시도 하 여 적절 한 경우를 결정 하 고 있으면 다시 시도 대 한 지불 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-112">Implementations of the IDbExecutionStrategy will be responsible for accepting an operation and, if an exception occurs, determining if a retry is appropriate and retrying if it is.</span></span> <span data-ttu-id="c99fd-113">EF와 함께 제공 되는 실행 전략 4 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-113">There are four execution strategies that ship with EF:</span></span>  

1. <span data-ttu-id="c99fd-114">**DefaultExecutionStrategy**:이 실행 전략은 모든 작업을 재시도 하지 않습니다, sql server 이외의 데이터베이스에 대 한 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-114">**DefaultExecutionStrategy**: this execution strategy does not retry any operations, it is the default for databases other than sql server.</span></span>  
2. <span data-ttu-id="c99fd-115">**DefaultSqlExecutionStrategy**: 기본적으로 사용 되는 내부 실행 전략입니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-115">**DefaultSqlExecutionStrategy**: this is an internal execution strategy that is used by default.</span></span> <span data-ttu-id="c99fd-116">그러나이 전략에 다시 시도 하지 않습니다, 그리고 일시적인 연결 복원 력을 할 수 있는 사용자에 게 알려 될 수 있는 모든 예외 바꿈됩니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-116">This strategy does not retry at all, however, it will wrap any exceptions that could be transient to inform users that they might want to enable connection resiliency.</span></span>  
3. <span data-ttu-id="c99fd-117">**DbExecutionStrategy**:이 클래스는 다른 실행 전략을 포함 하 여 사용자 고유의 사용자 지정에 대 한 기본 클래스로 적합 합니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-117">**DbExecutionStrategy**: this class is suitable as a base class for other execution strategies, including your own custom ones.</span></span> <span data-ttu-id="c99fd-118">초기 재시도 발생 0 지연 및 지연 시간을 사용 하 여 증가 기하급수적으로 최대 재시도 횟수가 적중 될 때까지 지 수 재시도 정책을 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-118">It implements an exponential retry policy, where the initial retry happens with zero delay and the delay increases exponentially until the maximum retry count is hit.</span></span> <span data-ttu-id="c99fd-119">이 클래스는 추상 ShouldRetryOn 메서드는 예외를 재시도할 제어 하려면 파생 된 실행 전략에서 구현 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-119">This class has an abstract ShouldRetryOn method that can be implemented in derived execution strategies to control which exceptions should be retried.</span></span>  
4. <span data-ttu-id="c99fd-120">**SqlAzureExecutionStrategy**:이 실행 전략이 DbExecutionStrategy에서 상속 하 고 Azure SQL Database를 사용 하 여 작업 하는 경우 가능한 일시적인 것으로 알려진 된 예외가 다시 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-120">**SqlAzureExecutionStrategy**: this execution strategy inherits from DbExecutionStrategy and will retry on exceptions that are known to be possibly transient when working with Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="c99fd-121">실행 전략 2 및 4 EntityFramework.SqlServer 어셈블리에 있는 EF와 함께 제공 되는 Sql Server 공급자에 포함 되 고 SQL Server와 함께 작동 하도록 설계 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-121">Execution strategies 2 and 4 are included in the Sql Server provider that ships with EF, which is in the EntityFramework.SqlServer assembly and are designed to work with SQL Server.</span></span>  

## <a name="enabling-an-execution-strategy"></a><span data-ttu-id="c99fd-122">실행 전략을 사용 하도록 설정</span><span class="sxs-lookup"><span data-stu-id="c99fd-122">Enabling an Execution Strategy</span></span>  

<span data-ttu-id="c99fd-123">SetExecutionStrategy 메서드를 사용 하 여 EF 실행 전략을 사용 하 게 하는 가장 쉬운 방법은는 [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) 클래스:</span><span class="sxs-lookup"><span data-stu-id="c99fd-123">The easiest way to tell EF to use an execution strategy is with the SetExecutionStrategy method of the [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) class:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

<span data-ttu-id="c99fd-124">이 코드는 SqlAzureExecutionStrategy SQL Server에 연결할 때 사용 하 여 EF 하도록 지시 합니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-124">This code tells EF to use the SqlAzureExecutionStrategy when connecting to SQL Server.</span></span>  

## <a name="configuring-the-execution-strategy"></a><span data-ttu-id="c99fd-125">실행 전략 구성</span><span class="sxs-lookup"><span data-stu-id="c99fd-125">Configuring the Execution Strategy</span></span>  

<span data-ttu-id="c99fd-126">SqlAzureExecutionStrategy의 생성자는 MaxRetryCount 및 MaxDelay 두 매개 변수를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-126">The constructor of SqlAzureExecutionStrategy can accept two parameters, MaxRetryCount and MaxDelay.</span></span> <span data-ttu-id="c99fd-127">MaxRetry 횟수가 전략을 다시 시도 하는 최대 횟수입니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-127">MaxRetry count is the maximum number of times that the strategy will retry.</span></span> <span data-ttu-id="c99fd-128">MaxDelay는 실행 전략을 사용 하는 재시도 사이의 최대 지연 시간을 나타내는 TimeSpan입니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-128">The MaxDelay is a TimeSpan representing the maximum delay between retries that the execution strategy will use.</span></span>  

<span data-ttu-id="c99fd-129">최대 재시도 횟수 1 및 최대 지연 시간을 30 초로 설정 하려면 다음 execue 어떻게 해야 합니까?</span><span class="sxs-lookup"><span data-stu-id="c99fd-129">To set the maximum number of retries to 1 and the maximum delay to 30 seconds you would execue the following:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy(
            "System.Data.SqlClient",
            () => new SqlAzureExecutionStrategy(1, TimeSpan.FromSeconds(30)));
    }
}
```  

<span data-ttu-id="c99fd-130">SqlAzureExecutionStrategy 일시적인 오류가 발생 하지만 더 이상 다시 시도 하거나 최대 한도 각 재시도 사이의 지연 됩니다 첫 번째 시간 초과 또는 총 시간 최대 지연 시간에 도달 하는 즉시 다시 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-130">The SqlAzureExecutionStrategy will retry instantly the first time a transient failure occurs, but will delay longer between each retry until either the max retry limit is exceeded or the total time hits the max delay.</span></span>  

<span data-ttu-id="c99fd-131">실행 전략 제한 된 수의 예외는 일반적으로 tansient만 다시 시도 해결 하려면 오류가 일시적이 지 또는 시간이 너무 오래 걸리는 있는 경우 RetryLimitExceeded 예외 catch 할 뿐만 아니라 다른 오류를 처리 해야 자체입니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-131">The execution strategies will only retry a limited number of exceptions that are usually tansient, you will still need to handle other errors as well as catching the RetryLimitExceeded exception for the case where an error is not transient or takes too long to resolve itself.</span></span>  

<span data-ttu-id="c99fd-132">다시 시도 실행 전략을 사용 하는 경우 몇 가지 알려진 제한 사항을 가지 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-132">There are some known of limitations when using a retrying execution strategy:</span></span>  

## <a name="streaming-queries-are-not-supported"></a><span data-ttu-id="c99fd-133">스트리밍 쿼리가 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-133">Streaming queries are not supported</span></span>  

<span data-ttu-id="c99fd-134">기본적으로 EF6 및 이후 버전 스트리밍 하는 것이 아니라 쿼리 결과 버퍼링 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-134">By default, EF6 and later version will buffer query results rather than streaming them.</span></span> <span data-ttu-id="c99fd-135">결과 수를 스트리밍 하려는 경우 LINQ to Entities 쿼리의 스트리밍 변경 하려면 AsStreaming 메서드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-135">If you want to have results streamed you can use the AsStreaming method to change a LINQ to Entities query to streaming.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

<span data-ttu-id="c99fd-136">다시 시도 실행 전략을 등록 하는 경우 스트리밍 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-136">Streaming is not supported when a retrying execution strategy is registered.</span></span> <span data-ttu-id="c99fd-137">이 제한은 연결 되 고 반환 된 결과 통해 도중 삭제할 수 없습니다 때문에 존재 합니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-137">This limitation exists because the connection could drop part way through the results being returned.</span></span> <span data-ttu-id="c99fd-138">이 문제가 발생 하면 EF 전체 쿼리를 다시 실행 해야 하는 경우 되었지만 신뢰할 수 있는 이미 어떤 결과가 반환 된 알 수 없습니다 (데이터 변경한 초기 쿼리가 전송 된 결과 다른 순서로 다시 가져올 수 있습니다, 결과 고유 식별자가 없을 수 있으므로 등.).</span><span class="sxs-lookup"><span data-stu-id="c99fd-138">When this occurs, EF needs to re-run the entire query but has no reliable way of knowing which results have already been returned (data may have changed since the initial query was sent, results may come back in a different order, results may not have a unique identifier, etc.).</span></span>  

## <a name="user-initiated-transactions-are-not-supported"></a><span data-ttu-id="c99fd-139">사용자가 시작한 트랜잭션은 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-139">User initiated transactions are not supported</span></span>  

<span data-ttu-id="c99fd-140">다시 시도에서 발생 하는 실행 전략을 구성한 경우 트랜잭션 사용과 관련 한 몇 가지 제한이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-140">When you have configured an execution strategy that results in retries, there are some limitations around the use of transactions.</span></span>  

<span data-ttu-id="c99fd-141">기본적으로 EF는 트랜잭션 내에서 모든 데이터베이스 업데이트를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-141">By default, EF will perform any database updates within a transaction.</span></span> <span data-ttu-id="c99fd-142">이렇게 하려면 아무 작업도 수행할 필요가 없습니다, 그리고 EF 항상이 자동으로 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-142">You don’t need to do anything to enable this, EF always does this automatically.</span></span>  

<span data-ttu-id="c99fd-143">예를 들어, 다음 코드에서는 자동으로 트랜잭션 내에서 SaveChanges 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-143">For example, in the following code SaveChanges is automatically performed within a transaction.</span></span> <span data-ttu-id="c99fd-144">SaveChanges가 트랜잭션을 롤백할 수는 다음 새 사이트의 중 하나를 삽입 하 고 데이터베이스에 적용할 변경 후에 실패 합니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-144">If SaveChanges were to fail after inserting one of the new Site’s then the transaction would be rolled back and no changes applied to the database.</span></span> <span data-ttu-id="c99fd-145">컨텍스트도 SaveChanges 변경 내용을 적용 하는 다시 시도를 다시 호출할 수 있도록 상태에서 그대로 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-145">The context is also left in a state that allows SaveChanges to be called again to retry applying the changes.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

<span data-ttu-id="c99fd-146">다시 시도 실행 전략을 사용 하지 않는 경우에 단일 트랜잭션에서 여러 작업을 래핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-146">When not using a retrying execution strategy you can wrap multiple operations in a single transaction.</span></span> <span data-ttu-id="c99fd-147">예를 들어, 다음 코드는 단일 트랜잭션에서 두 SaveChanges 호출을 래핑합니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-147">For example, the following code wraps two SaveChanges calls in a single transaction.</span></span> <span data-ttu-id="c99fd-148">두 작업의 일부가 실패 하면 변경 하는 경우 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-148">If any part of either operation fails then none of the changes are applied.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Site { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }
}
```  

<span data-ttu-id="c99fd-149">EF 모든 이전 작업을 다시 시도 하는 방법을 인식 하지 않기 때문에 다시 시도 실행 전략을 사용 하는 경우에 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-149">This is not supported when using a retrying execution strategy because EF isn’t aware of any previous operations and how to retry them.</span></span> <span data-ttu-id="c99fd-150">예를 들어, 두 번째 SaveChanges 실패 한 경우 다음 EF 더 이상 첫 번째 SaveChanges 호출을 다시 시도 하는 데 필요한 정보를 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-150">For example, if the second SaveChanges failed then EF no longer has the required information to retry the first SaveChanges call.</span></span>  

### <a name="workaround-suspend-execution-strategy"></a><span data-ttu-id="c99fd-151">해결 방법: 실행 전략 일시 중단</span><span class="sxs-lookup"><span data-stu-id="c99fd-151">Workaround: Suspend Execution Strategy</span></span>  

<span data-ttu-id="c99fd-152">해결 방법 중 하나 트랜잭션 시작 사용자를 사용 해야 하는 코드 부분에 대 한 다시 시도 실행 전략 일시 중단 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-152">One possible workaround is to suspend the retrying execution strategy for the piece of code that needs to use a user initiated transaction.</span></span> <span data-ttu-id="c99fd-153">이 작업을 수행 하는 가장 쉬운 방법은 코드 SuspendExecutionStrategy 플래그 구성 클래스 및 변경 플래그를 설정 하는 경우 기본 (retying 아닌) 실행 전략을 반환할 실행 전략 람다를 추가 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-153">The easiest way to do this is to add a SuspendExecutionStrategy flag to your code based configuration class and change the execution strategy lambda to return the default (non-retying) execution strategy when the flag is set.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;
using System.Runtime.Remoting.Messaging;

namespace Demo
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            this.SetExecutionStrategy("System.Data.SqlClient", () => SuspendExecutionStrategy
              ? (IDbExecutionStrategy)new DefaultExecutionStrategy()
              : new SqlAzureExecutionStrategy());
        }

        public static bool SuspendExecutionStrategy
        {
            get
            {
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy")  false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

<span data-ttu-id="c99fd-154">플래그 값을 저장 하도록 CallContext 사용 하는 것을 참고 합니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-154">Note that we are using CallContext to store the flag value.</span></span> <span data-ttu-id="c99fd-155">이 스레드 로컬 저장소에 유사한 기능을 제공 하지만 안전 비동기 쿼리를 포함 하 여 비동기 코드를 사용 하 여 Entity Framework와 함께 저장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-155">This provides similar functionality to thread local storage but is safe to use with asynchronous code - including async query and save with Entity Framework.</span></span>  

<span data-ttu-id="c99fd-156">이제 사용자가 시작한 트랜잭션을 사용 하는 코드 섹션에 대 한 실행 전략 일시 중단할 수 있습니다 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-156">We can now suspend the execution strategy for the section of code that uses a user initiated transaction.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    MyConfiguration.SuspendExecutionStrategy = true;

    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }

    MyConfiguration.SuspendExecutionStrategy = false;
}
```  

### <a name="workaround-manually-call-execution-strategy"></a><span data-ttu-id="c99fd-157">해결 방법: 실행 전략을 직접 호출</span><span class="sxs-lookup"><span data-stu-id="c99fd-157">Workaround: Manually Call Execution Strategy</span></span>  

<span data-ttu-id="c99fd-158">다른 옵션은 수동으로 실행 전략을 사용 하 고 작업 중 하나가 실패 하면 모든 재시도 수 있도록 실행 논리의 전체 집합을 제공 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-158">Another option is to manually use the execution strategy and give it the entire set of logic to be run, so that it can retry everything if one of the operations fails.</span></span> <span data-ttu-id="c99fd-159">-기술을 사용 하 여 실행 전략 일시 중단 작업을 계속 합니다 위의-재시도 가능한 코드 블록 내에서 사용 되는 컨텍스트 다시 시도 하지 않도록 되도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-159">We still need to suspend the execution strategy - using the technique shown above - so that any contexts used inside the retryable code block do not attempt to retry.</span></span>  

<span data-ttu-id="c99fd-160">다시 시도 하려면 코드 블록 내에서 컨텍스트를 생성 해야 하는 참고 합니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-160">Note that any contexts should be constructed within the code block to be retried.</span></span> <span data-ttu-id="c99fd-161">이렇게 하면 각 재시도 대 한 클린 상태로 시작 했습니다.</span><span class="sxs-lookup"><span data-stu-id="c99fd-161">This ensures that we are starting with a clean state for each retry.</span></span>  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

MyConfiguration.SuspendExecutionStrategy = true;

executionStrategy.Execute(
    () =>
    {
        using (var db = new BloggingContext())
        {
            using (var trn = db.Database.BeginTransaction())
            {
                db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                db.SaveChanges();

                db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
                db.SaveChanges();

                trn.Commit();
            }
        }
    });

MyConfiguration.SuspendExecutionStrategy = false;
```  
