---
title: 트랜잭션 커밋 실패 처리-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: 27e75e6a1919ee2300fe76cfcdf67cceaad887b3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414676"
---
# <a name="handling-transaction-commit-failures"></a><span data-ttu-id="0d2b8-102">트랜잭션 커밋 실패 처리</span><span class="sxs-lookup"><span data-stu-id="0d2b8-102">Handling transaction commit failures</span></span>
> [!NOTE]
> <span data-ttu-id="0d2b8-103">**Ef 6.1** 이상-이 페이지에서 설명 하는 기능, api 등은 Entity Framework 6.1에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-103">**EF6.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="0d2b8-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="0d2b8-105">6\.1의 일부로 EF: 일시적인 연결 오류가 트랜잭션 커밋의 승인에 영향을 주는 경우 자동으로 검색 하 고 복구할 수 있는 EF: 새 연결 복원 력 기능을 도입 하 고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-105">As part of 6.1 we are introducing a new connection resiliency feature for EF: the ability to detect and recover automatically when transient connection failures affect the acknowledgement of transaction commits.</span></span> <span data-ttu-id="0d2b8-106">이 시나리오에 대 한 자세한 내용은 블로그 게시물 [SQL Database 연결 및 멱 등 성 문제](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx)에 설명 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-106">The full details of the scenario are best described in the blog post [SQL Database Connectivity and the Idempotency Issue](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span></span>  <span data-ttu-id="0d2b8-107">요약 하자면, 시나리오는 트랜잭션 커밋 중에 예외가 발생 하는 경우 두 가지 가능한 원인이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-107">In summary, the scenario is that when an exception is raised during a transaction commit there are two possible causes:</span></span>  

1. <span data-ttu-id="0d2b8-108">서버에서 트랜잭션 커밋이 실패 했습니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-108">The transaction commit failed on the server</span></span>
2. <span data-ttu-id="0d2b8-109">서버에서 트랜잭션 커밋이 성공 했지만 연결 문제로 인해 성공 알림이 클라이언트에 도달 하지 못했습니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-109">The transaction commit succeeded on the server but a connectivity issue prevented the success notification from reaching the client</span></span>  

<span data-ttu-id="0d2b8-110">첫 번째 상황이 응용 프로그램에서 발생 하거나 사용자가 작업을 다시 시도할 수 있지만 두 번째 상황에서 재시도를 방지 하 고 응용 프로그램이 자동으로 복구 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-110">When the first situation happens the application or the user can retry the operation, but when the second situation occurs retries should be avoided and the application could recover automatically.</span></span> <span data-ttu-id="0d2b8-111">문제는 커밋 중에 예외가 보고 된 실제 이유를 검색 하는 기능이 없으므로 응용 프로그램에서 올바른 작업 과정을 선택할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-111">The challenge is that without the ability to detect what was the actual reason an exception was reported during commit, the application cannot choose the right course of action.</span></span> <span data-ttu-id="0d2b8-112">EF 6.1의 새로운 기능을 통해 EF는 트랜잭션이 성공한 경우 데이터베이스를 다시 확인 하 고 적절 한 작업을 투명 하 게 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-112">The new feature in EF 6.1 allows EF to double-check with the database if the transaction succeeded and take the right course of action transparently.</span></span>  

## <a name="using-the-feature"></a><span data-ttu-id="0d2b8-113">기능 사용</span><span class="sxs-lookup"><span data-stu-id="0d2b8-113">Using the feature</span></span>  

<span data-ttu-id="0d2b8-114">이 기능을 사용 하도록 설정 하려면 **Dbconfiguration**의 생성자에 [settransactionhandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) 에 대 한 호출을 포함 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-114">In order to enable the feature you need include a call to [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) in the constructor of your **DbConfiguration**.</span></span> <span data-ttu-id="0d2b8-115">**Dbconfiguration**에 익숙하지 않은 경우 [코드 기반 구성](~/ef6/fundamentals/configuring/code-based.md)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-115">If you are unfamiliar with **DbConfiguration**, see [Code Based Configuration](~/ef6/fundamentals/configuring/code-based.md).</span></span> <span data-ttu-id="0d2b8-116">이 기능은 EF6에서 도입 된 자동 재시도와 함께 사용할 수 있습니다 .이는 일시적인 오류로 인해 트랜잭션이 실제로 서버에서 커밋하지 못한 상황에 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-116">This feature can be used in combination with the automatic retries we introduced in EF6, which help in the situation in which the transaction actually failed to commit on the server due to a transient failure:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

public class MyConfiguration : DbConfiguration  
{
  public MyConfiguration()  
  {  
    SetTransactionHandler(SqlProviderServices.ProviderInvariantName, () => new CommitFailureHandler());  
    SetExecutionStrategy(SqlProviderServices.ProviderInvariantName, () => new SqlAzureExecutionStrategy());  
  }  
}
```  

## <a name="how-transactions-are-tracked"></a><span data-ttu-id="0d2b8-117">트랜잭션 추적 방법</span><span class="sxs-lookup"><span data-stu-id="0d2b8-117">How transactions are tracked</span></span>  

<span data-ttu-id="0d2b8-118">이 기능을 사용 하도록 설정 하면 EF가 **__Transactions**라는 데이터베이스에 새 테이블을 자동으로 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-118">When the feature is enabled, EF will automatically add a new table to the database called **__Transactions**.</span></span> <span data-ttu-id="0d2b8-119">EF에서 트랜잭션을 만들 때마다이 테이블에 새 행이 삽입 되 고 커밋 중에 트랜잭션 오류가 발생 하는 경우 해당 행이 있는지 확인 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-119">A new row is inserted in this table every time a transaction is created by EF and that row is checked for existence if a transaction failure occurs during commit.</span></span>  

<span data-ttu-id="0d2b8-120">EF가 더 이상 필요 하지 않을 때 테이블에서 행을 정리 하는 데 가장 적합 하지만, 응용 프로그램이 중간에 종료 될 때 테이블이 커질 수 있으며, 따라서 경우에 따라 테이블을 수동으로 제거 해야 할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-120">Although EF will do a best effort to prune rows from the table when they aren’t needed anymore, the table can grow if the application exits prematurely and for that reason you may need to purge the table manually in some cases.</span></span>  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a><span data-ttu-id="0d2b8-121">이전 버전과의 커밋 실패를 처리 하는 방법</span><span class="sxs-lookup"><span data-stu-id="0d2b8-121">How to handle commit failures with previous Versions</span></span>

<span data-ttu-id="0d2b8-122">EF 6.1 이전에는 EF 제품의 커밋 실패를 처리 하는 메커니즘이 없었습니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-122">Before EF 6.1 there was not mechanism to handle commit failures in the EF product.</span></span> <span data-ttu-id="0d2b8-123">이전 버전의 EF6에 적용 될 수 있는 이러한 상황을 처리 하는 몇 가지 방법이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-123">There are several ways to dealing with this situation that can be applied to previous versions of EF6:</span></span>  

* <span data-ttu-id="0d2b8-124">옵션 1-아무 작업도 수행 하지 않음</span><span class="sxs-lookup"><span data-stu-id="0d2b8-124">Option 1 - Do nothing</span></span>  

  <span data-ttu-id="0d2b8-125">트랜잭션 커밋 중에 연결 오류가 발생 하는 경우이 상태가 실제로 발생 하는 경우 응용 프로그램이 실패할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-125">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>  

* <span data-ttu-id="0d2b8-126">옵션 2-데이터베이스를 사용 하 여 상태 다시 설정</span><span class="sxs-lookup"><span data-stu-id="0d2b8-126">Option 2 - Use the database to reset state</span></span>  

  1. <span data-ttu-id="0d2b8-127">현재 DbContext를 취소 합니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-127">Discard the current DbContext</span></span>  
  2. <span data-ttu-id="0d2b8-128">새 DbContext을 만들고 데이터베이스에서 응용 프로그램의 상태를 복원 합니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-128">Create a new DbContext and restore the state of your application from the database</span></span>  
  3. <span data-ttu-id="0d2b8-129">마지막 작업이 성공적으로 완료 되지 않았을 수 있음을 사용자에 게 알립니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-129">Inform the user that the last operation might not have been completed successfully</span></span>  

* <span data-ttu-id="0d2b8-130">옵션 3-수동으로 트랜잭션 추적</span><span class="sxs-lookup"><span data-stu-id="0d2b8-130">Option 3 - Manually track the transaction</span></span>  

  1. <span data-ttu-id="0d2b8-131">트랜잭션 상태를 추적 하는 데 사용 되는 데이터베이스에 추적 되지 않은 테이블을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-131">Add a non-tracked table to the database used to track the status of the transactions.</span></span>  
  2. <span data-ttu-id="0d2b8-132">각 트랜잭션의 시작 부분에서 테이블에 행을 삽입 합니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-132">Insert a row into the table at the beginning of each transaction.</span></span>  
  3. <span data-ttu-id="0d2b8-133">커밋하는 동안 연결이 실패 하는 경우 데이터베이스에 해당 하는 행이 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-133">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>  
     - <span data-ttu-id="0d2b8-134">행이 있는 경우 트랜잭션이 성공적으로 커밋 됨에 따라 정상적으로 계속 진행 합니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-134">If the row is present, continue normally, as the transaction was committed successfully</span></span>  
     - <span data-ttu-id="0d2b8-135">행이 없으면 실행 전략을 사용 하 여 현재 작업을 다시 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-135">If the row is absent, use an execution strategy to retry the current operation.</span></span>  
  4. <span data-ttu-id="0d2b8-136">커밋이 성공한 경우 테이블의 증가를 방지 하려면 해당 행을 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-136">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>  

<span data-ttu-id="0d2b8-137">[이 블로그 게시물](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) 에는 SQL Azure에서이를 수행 하기 위한 샘플 코드가 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0d2b8-137">[This blog post](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contains sample code for accomplishing this on SQL Azure.</span></span>  
