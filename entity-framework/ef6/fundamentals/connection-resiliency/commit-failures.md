---
title: 트랜잭션 커밋 오류-EF6를 처리합니다.
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
caps.latest.revision: 3
ms.openlocfilehash: 95ac69a005a70f31086bd4d3c0088bd3e82d28e2
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122562"
---
# <a name="handling-transaction-commit-failures"></a><span data-ttu-id="531cb-102">트랜잭션 커밋 오류 처리</span><span class="sxs-lookup"><span data-stu-id="531cb-102">Handling transaction commit failures</span></span>
> [!NOTE]
> <span data-ttu-id="531cb-103">**EF6.1 이상만** -Api 기능 등이이 페이지에 설명 된 Entity Framework 6.1에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-103">**EF6.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="531cb-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="531cb-105">6.1 일부로 EF에 대 한 새 연결 복원 력 기능 도입 됩니다: 검색 하 고 일시적인 연결 오류가 발생할 트랜잭션 커밋 승인을 영향을 주는 경우 자동으로 복구 하는 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-105">As part of 6.1 we are introducing a new connection resiliency feature for EF: the ability to detect and recover automatically when transient connection failures affect the acknowledgement of transaction commits.</span></span> <span data-ttu-id="531cb-106">시나리오의 전체 세부 정보는 블로그 게시물에 잘 설명 [SQL Database 연결 및 멱 등 성 문제가](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx)합니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-106">The full details of the scenario are best described in the blog post [SQL Database Connectivity and the Idempotency Issue](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span></span>  <span data-ttu-id="531cb-107">요약 하자면, 시나리오의 트랜잭션 커밋하는 동안 예외가 발생 하는 경우는 두 가지 가능한 원인:</span><span class="sxs-lookup"><span data-stu-id="531cb-107">In summary, the scenario is that when an exception is raised during a transaction commit there are two possible causes:</span></span>  

1. <span data-ttu-id="531cb-108">서버에서 트랜잭션 커밋이 실패 했습니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-108">The transaction commit failed on the server</span></span>
2. <span data-ttu-id="531cb-109">서버에서 트랜잭션 커밋에 성공 했지만 연결 문제를 방지 클라이언트에 도달 하에서 성공 알림</span><span class="sxs-lookup"><span data-stu-id="531cb-109">The transaction commit succeeded on the server but a connectivity issue prevented the success notification from reaching the client</span></span>  

<span data-ttu-id="531cb-110">사용자나 응용 프로그램이 첫 번째 상황은 표시 되는 경우 작업을 다시 시도할 수 있지만 두 번째 상황이 발생 하면 다시 시도 하지 않아야 합니다 및 응용 프로그램을 자동으로 복구할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-110">When the first situation happens the application or the user can retry the operation, but when the second situation occurs retries should be avoided and the application could recover automatically.</span></span> <span data-ttu-id="531cb-111">과제 없으면 무엇 이었습니까 실제 응용 프로그램이 올바른 작업 과정을 선택할 수 없습니다. 예외가, 커밋하는 동안 보고 된 이유를 검색할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-111">The challenge is that without the ability to detect what was the actual reason an exception was reported during commit, the application cannot choose the right course of action.</span></span> <span data-ttu-id="531cb-112">6.1 EF의에서 새로운 기능에는 트랜잭션이 성공 하 고 오른쪽 작업 과정을 투명 하 게 수행 하는 경우 데이터베이스를 사용 하 여 다시 확인 하려면 EF 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-112">The new feature in EF 6.1 allows EF to double-check with the database if the transaction succeeded and take the right course of action transparently.</span></span>  

## <a name="using-the-feature"></a><span data-ttu-id="531cb-113">기능 사용</span><span class="sxs-lookup"><span data-stu-id="531cb-113">Using the feature</span></span>  

<span data-ttu-id="531cb-114">기능을 사용 하도록 설정 하려면에 대 한 호출을 포함 해야 [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) 의 생성자에서 프로그램 **DbConfiguration**합니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-114">In order to enable the feature you need include a call to [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) in the constructor of your **DbConfiguration**.</span></span> <span data-ttu-id="531cb-115">익숙한 없는 경우 **DbConfiguration**를 참조 하십시오 [코드 기반 구성](~/ef6/fundamentals/configuring/code-based.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-115">If you are unfamiliar with **DbConfiguration**, see [Code Based Configuration](~/ef6/fundamentals/configuring/code-based.md).</span></span> <span data-ttu-id="531cb-116">트랜잭션이 실제로 커밋하지 못했습니다 일시적인 오류로 인해 서버에서 상황에 도움이 되는 ef6를 도입 했습니다 자동 재시도와 함께에서이 기능을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-116">This feature can be used in combination with the automatic retries we introduced in EF6, which help in the situation in which the transaction actually failed to commit on the server due to a transient failure:</span></span>  

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

## <a name="how-transactions-are-tracked"></a><span data-ttu-id="531cb-117">트랜잭션은 추적 하는 방법</span><span class="sxs-lookup"><span data-stu-id="531cb-117">How transactions are tracked</span></span>  

<span data-ttu-id="531cb-118">라는 데이터베이스에 EF가 새 테이블을 추가할 자동으로 기능을 사용 하는 경우 **__Transactions**합니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-118">When the feature is enabled, EF will automatically add a new table to the database called **__Transactions**.</span></span> <span data-ttu-id="531cb-119">EF에서 트랜잭션 만들어지고 커밋하는 동안 트랜잭션 오류가 발생할 경우 해당 행이 있는지 검사 될 때마다 새 행이이 테이블에 삽입 됩니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-119">A new row is inserted in this table every time a transaction is created by EF and that row is checked for existence if a transaction failure occurs during commit.</span></span>  

<span data-ttu-id="531cb-120">EF는 더 이상 필요 하지 않은 경우 테이블의 행을 정리 하는 최고의 수행 하지만 일부 경우에서 수동으로 테이블을 제거 해야 할 수 있습니다에 대 한 중간 응용 프로그램에 종료 되 면 테이블 증가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-120">Although EF will do a best effort to prune rows from the table when they aren’t needed anymore, the table can grow if the application exits prematurely and for that reason you may need to purge the table manually in some cases.</span></span>  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a><span data-ttu-id="531cb-121">이전 버전을 사용 하 여 커밋 실패를 처리 하는 방법</span><span class="sxs-lookup"><span data-stu-id="531cb-121">How to handle commit failures with previous Versions</span></span>

<span data-ttu-id="531cb-122">EF 6.1 이전 EF 제품에 커밋 실패를 처리 하는 메커니즘 하지 못했습니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-122">Before EF 6.1 there was not mechanism to handle commit failures in the EF product.</span></span> <span data-ttu-id="531cb-123">EF6의 이전 버전에 적용할 수 있는 이러한 상황을 처리 하는 방법은 여러 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-123">There are several ways to dealing with this situation that can be applied to previous versions of EF6:</span></span>  

### <a name="option-1---do-nothing"></a><span data-ttu-id="531cb-124">옵션 1-작업 안 함</span><span class="sxs-lookup"><span data-stu-id="531cb-124">Option 1 - Do nothing</span></span>  

<span data-ttu-id="531cb-125">트랜잭션 커밋하는 동안 연결 오류의 가능성 낮은 이므로 응용 프로그램이이 조건에서 실제로 발생 하는 경우 실패를 허용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-125">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>  

## <a name="option-2---use-the-database-to-reset-state"></a><span data-ttu-id="531cb-126">옵션 2-데이터베이스를 사용 하 여 상태를 다시 설정</span><span class="sxs-lookup"><span data-stu-id="531cb-126">Option 2 - Use the database to reset state</span></span>  

1. <span data-ttu-id="531cb-127">현재 DbContext를 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-127">Discard the current DbContext</span></span>  
2. <span data-ttu-id="531cb-128">새 DbContext를 만들고 데이터베이스에서 응용 프로그램의 상태를 복원 합니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-128">Create a new DbContext and restore the state of your application from the database</span></span>  
3. <span data-ttu-id="531cb-129">마지막 작업 수 완료 되지 않은 성공적으로 사용자에 게 알릴</span><span class="sxs-lookup"><span data-stu-id="531cb-129">Inform the user that the last operation might not have been completed successfully</span></span>  

## <a name="option-3---manually-track-the-transaction"></a><span data-ttu-id="531cb-130">옵션 3-수동으로 트랜잭션 추적</span><span class="sxs-lookup"><span data-stu-id="531cb-130">Option 3 - Manually track the transaction</span></span>  

1. <span data-ttu-id="531cb-131">트랜잭션의 상태를 추적 하는 데 데이터베이스에 추적 된 테이블을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-131">Add a non-tracked table to the database used to track the status of the transactions.</span></span>  
2. <span data-ttu-id="531cb-132">각 트랜잭션의 시작 부분에 있는 테이블에 행을 삽입 합니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-132">Insert a row into the table at the beginning of each transaction.</span></span>  
3. <span data-ttu-id="531cb-133">커밋 중 연결이 실패 하는 경우 데이터베이스에서 해당 행의 존재 여부 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-133">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>  
    - <span data-ttu-id="531cb-134">행이 있는 경우 정상적으로 계속, 트랜잭션이 성공적으로 커밋된 처럼</span><span class="sxs-lookup"><span data-stu-id="531cb-134">If the row is present, continue normally, as the transaction was committed successfully</span></span>  
    - <span data-ttu-id="531cb-135">행이 없으면 실행 전략을 사용 하 여 현재 작업을 다시 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-135">If the row is absent, use an execution strategy to retry the current operation.</span></span>  
4. <span data-ttu-id="531cb-136">커밋이 성공 하면 테이블의 증가 방지 하려면 해당 행을 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-136">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>  

<span data-ttu-id="531cb-137">[이 블로그 게시물](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) SQL Azure이 작업을 수행 하는 것에 대 한 샘플 코드가 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="531cb-137">[This blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contains sample code for accomplishing this on SQL Azure.</span></span>  
