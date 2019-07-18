---
title: 트랜잭션 작업-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 7030dc675993339f72c935f6b430cead85fecb7f
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306520"
---
# <a name="working-with-transactions"></a><span data-ttu-id="89062-102">트랜잭션 작업</span><span class="sxs-lookup"><span data-stu-id="89062-102">Working with Transactions</span></span>
> [!NOTE]
> <span data-ttu-id="89062-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="89062-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="89062-105">이 문서에서는 트랜잭션 작업을 쉽게 수행할 수 있도록 EF5 이후 추가 된 향상 된 기능을 포함 하 여 EF6의 트랜잭션을 사용 하는 방법을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-105">This document will describe using transactions in EF6 including the enhancements we have added since EF5 to make working with transactions easy.</span></span>  

## <a name="what-ef-does-by-default"></a><span data-ttu-id="89062-106">기본적으로 수행 되는 EF</span><span class="sxs-lookup"><span data-stu-id="89062-106">What EF does by default</span></span>  

<span data-ttu-id="89062-107">모든 버전의 Entity Framework에서 **SaveChanges ()** 를 실행 하 여 데이터베이스에 삽입, 업데이트 또는 삭제할 때마다 프레임 워크는 해당 작업을 트랜잭션으로 래핑합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-107">In all versions of Entity Framework, whenever you execute **SaveChanges()** to insert, update or delete on the database the framework will wrap that operation in a transaction.</span></span> <span data-ttu-id="89062-108">이 트랜잭션은 작업을 실행 한 후 완료 되는 동안만 지속 됩니다.</span><span class="sxs-lookup"><span data-stu-id="89062-108">This transaction lasts only long enough to execute the operation and then completes.</span></span> <span data-ttu-id="89062-109">다른 작업을 실행 하면 새 트랜잭션이 시작 됩니다.</span><span class="sxs-lookup"><span data-stu-id="89062-109">When you execute another such operation a new transaction is started.</span></span>  

<span data-ttu-id="89062-110">**ExecuteSqlCommand ()** 로 시작 하는 경우 EF6 ()에서 기본적으로 명령이 아직 없는 경우 해당 트랜잭션에서 명령을 래핑합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-110">Starting with EF6 **Database.ExecuteSqlCommand()** by default will wrap the command in a transaction if one was not already present.</span></span> <span data-ttu-id="89062-111">원할 경우이 동작을 재정의할 수 있는이 메서드의 오버 로드가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-111">There are overloads of this method that allow you to override this behavior if you wish.</span></span> <span data-ttu-id="89062-112">또한 ObjectContext와 같은 Api를 통해 모델에 포함 된 저장 프로시저의 실행을 EF6 합니다. **ExecuteFunction ()** 는 동일한 기능을 수행 합니다. 단, 기본 동작은 현재 재정의할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-112">Also in EF6 execution of stored procedures included in the model through APIs such as **ObjectContext.ExecuteFunction()** does the same (except that the default behavior cannot at the moment be overridden).</span></span>  

<span data-ttu-id="89062-113">두 경우 모두 트랜잭션의 격리 수준은 데이터베이스 공급자가 기본 설정을 고려 하는 격리 수준입니다.</span><span class="sxs-lookup"><span data-stu-id="89062-113">In either case, the isolation level of the transaction is whatever isolation level the database provider considers its default setting.</span></span> <span data-ttu-id="89062-114">예를 들어 SQL Server에서이는 커밋된 읽기입니다.</span><span class="sxs-lookup"><span data-stu-id="89062-114">By default, for instance, on SQL Server this is READ COMMITTED.</span></span>  

<span data-ttu-id="89062-115">Entity Framework는 트랜잭션에서 쿼리를 래핑하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-115">Entity Framework does not wrap queries in a transaction.</span></span>  

<span data-ttu-id="89062-116">이 기본 기능은 많은 사용자에 게 적합 하며 EF6에서 다른 작업을 수행할 필요가 없습니다. 언제 든 지 코드를 작성 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="89062-116">This default functionality is suitable for a lot of users and if so there is no need to do anything different in EF6; just write the code as you always did.</span></span>  

<span data-ttu-id="89062-117">그러나 일부 사용자는 트랜잭션을 보다 강력 하 게 제어 해야 합니다 .이에 대해서는 다음 섹션에서 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-117">However some users require greater control over their transactions – this is covered in the following sections.</span></span>  

## <a name="how-the-apis-work"></a><span data-ttu-id="89062-118">Api 작동 방법</span><span class="sxs-lookup"><span data-stu-id="89062-118">How the APIs work</span></span>  

<span data-ttu-id="89062-119">EF6 Entity Framework 이전에는 데이터베이스 연결 자체를 열 때 insisted (이미 열려 있는 연결이 전달 된 경우 예외를 throw).</span><span class="sxs-lookup"><span data-stu-id="89062-119">Prior to EF6 Entity Framework insisted on opening the database connection itself (it threw an exception if it was passed a connection that was already open).</span></span> <span data-ttu-id="89062-120">트랜잭션은 열린 연결 에서만 시작할 수 있으므로 사용자가 여러 작업을 하나의 트랜잭션으로 래핑할 수 있는 유일한 방법은 [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) 를 사용 하거나 ObjectContext를 사용 하는 것입니다 **. 연결** 속성 및 시작 반환 된 **Entityconnection** 개체에서 **Open ()** 및 **BeginTransaction ()** 를 직접 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-120">Since a transaction can only be started on an open connection, this meant that the only way a user could wrap several operations into one transaction was either to use a [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) or use the **ObjectContext.Connection** property and start calling **Open()** and **BeginTransaction()** directly on the returned **EntityConnection** object.</span></span> <span data-ttu-id="89062-121">또한 원본 데이터베이스 연결에서 트랜잭션을 시작한 경우에는 데이터베이스에 연결 된 API 호출이 실패 합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-121">In addition, API calls which contacted the database would fail if you had started a transaction on the underlying database connection on your own.</span></span>  

> [!NOTE]
> <span data-ttu-id="89062-122">닫힌 연결 허용의 제한은 Entity Framework 6에서 제거 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-122">The limitation of only accepting closed connections was removed in Entity Framework 6.</span></span> <span data-ttu-id="89062-123">자세한 내용은 [연결 관리](~/ef6/fundamentals/connection-management.md)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="89062-123">For details, see [Connection Management](~/ef6/fundamentals/connection-management.md).</span></span>  

<span data-ttu-id="89062-124">EF6부터 프레임 워크는 이제 다음을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-124">Starting with EF6 the framework now provides:</span></span>  

1. <span data-ttu-id="89062-125">**Database.BeginTransaction()** : 사용자가 기존 DbContext 내에서 트랜잭션을 직접 시작 하 고 완료 하는 것이 더 쉬운 방법입니다. 이렇게 하면 여러 작업을 동일한 트랜잭션 내에서 결합할 수 있으므로 모두 커밋되거나 모두 롤백됩니다.</span><span class="sxs-lookup"><span data-stu-id="89062-125">**Database.BeginTransaction()** : An easier method for a user to start and complete transactions themselves within an existing DbContext – allowing several operations to be combined within the same transaction and hence either all committed or all rolled back as one.</span></span> <span data-ttu-id="89062-126">또한 사용자가 트랜잭션의 격리 수준을 더 쉽게 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-126">It also allows the user to more easily specify the isolation level for the transaction.</span></span>  
2. <span data-ttu-id="89062-127">**UseTransaction ()** : DbContext가 Entity Framework 외부에서 시작 된 트랜잭션을 사용할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-127">**Database.UseTransaction()** : which allows the DbContext to use a transaction which was started outside of the Entity Framework.</span></span>  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a><span data-ttu-id="89062-128">동일한 컨텍스트 내에서 여러 작업을 하나의 트랜잭션으로 결합</span><span class="sxs-lookup"><span data-stu-id="89062-128">Combining several operations into one transaction within the same context</span></span>  

<span data-ttu-id="89062-129">**BeginTransaction ()** 에는 두 가지 재정의가 있습니다. 하나는 명시적 [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) 를 사용 하 고 다른 하나는 인수를 사용 하지 않고 기본 데이터베이스 공급자의 기본 IsolationLevel를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-129">**Database.BeginTransaction()** has two overrides – one which takes an explicit [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) and one which takes no arguments and uses the default IsolationLevel from the underlying database provider.</span></span> <span data-ttu-id="89062-130">두 재정의 모두 기본 저장소 트랜잭션에서 커밋 및 롤백을 수행 하는 **commit ()** 및 **rollback ()** 메서드를 제공 하는 **dbcontexttransaction** 개체를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-130">Both overrides return a **DbContextTransaction** object which provides **Commit()** and **Rollback()** methods which perform commit and rollback on the underlying store transaction.</span></span>  

<span data-ttu-id="89062-131">**Dbcontexttransaction** 은 커밋되거나 롤백된 후 삭제 됩니다.</span><span class="sxs-lookup"><span data-stu-id="89062-131">The **DbContextTransaction** is meant to be disposed once it has been committed or rolled back.</span></span> <span data-ttu-id="89062-132">이를 수행 하는 한 가지 쉬운 방법은 **using (...)을 사용 하는 것입니다. {...}**</span><span class="sxs-lookup"><span data-stu-id="89062-132">One easy way to accomplish this is the **using(…) {…}**</span></span> <span data-ttu-id="89062-133">using 블록이 완료 되 면 **Dispose ()** 를 자동으로 호출 하는 구문입니다.</span><span class="sxs-lookup"><span data-stu-id="89062-133">syntax which will automatically call **Dispose()** when the using block completes:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void StartOwnTransactionWithinContext()
        {
            using (var context = new BloggingContext())
            {
                using (var dbContextTransaction = context.Database.BeginTransaction())
                {
                    context.Database.ExecuteSqlCommand(
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'"
                        );

                    var query = context.Posts.Where(p => p.Blog.Rating >= 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }

                    context.SaveChanges();

                    dbContextTransaction.Commit();
                }
            }
        }
    }
}
```  

> [!NOTE]
> <span data-ttu-id="89062-134">트랜잭션을 시작 하려면 기본 저장소 연결이 열려 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-134">Beginning a transaction requires that the underlying store connection is open.</span></span> <span data-ttu-id="89062-135">따라서 BeginTransaction ()를 호출 하면 연결이 아직 열려 있지 않은 경우 연결이 열립니다.</span><span class="sxs-lookup"><span data-stu-id="89062-135">So calling Database.BeginTransaction() will open the connection  if it is not already opened.</span></span> <span data-ttu-id="89062-136">DbContextTransaction이 연결을 연 경우 Dispose ()가 호출 될 때이를 닫습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-136">If DbContextTransaction opened the connection then it will close it when Dispose() is called.</span></span>  

### <a name="passing-an-existing-transaction-to-the-context"></a><span data-ttu-id="89062-137">기존 트랜잭션을 컨텍스트에 전달</span><span class="sxs-lookup"><span data-stu-id="89062-137">Passing an existing transaction to the context</span></span>  

<span data-ttu-id="89062-138">때로는 범위에서 훨씬 더 광범위 하 고, 동일한 데이터베이스에 대 한 작업을 포함 하 고 EF 외부에서 작업을 포함 하는 트랜잭션을 원할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-138">Sometimes you would like a transaction which is even broader in scope and which includes operations on the same database but outside of EF completely.</span></span> <span data-ttu-id="89062-139">이를 수행 하려면 연결을 열고 트랜잭션을 직접 시작한 다음 EF a에 게 이미 열려 있는 데이터베이스 연결을 사용 하 고 b)를 사용 하 여 해당 연결에서 기존 트랜잭션을 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-139">To accomplish this you must open the connection and start the transaction yourself and then tell EF a) to use the already-opened database connection, and b) to use the existing transaction on that connection.</span></span>  

<span data-ttu-id="89062-140">이렇게 하려면 기존 연결 매개 변수를 사용 하는 DbContext 생성자 중 하나에서 상속 되는 컨텍스트 클래스의 생성자와 contextOwnsConnection 부울로 생성자를 정의 하 고 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-140">To do this you must define and use a constructor on your context class which inherits from one of the DbContext constructors which take i) an existing connection parameter and ii) the contextOwnsConnection boolean.</span></span>  

> [!NOTE]
> <span data-ttu-id="89062-141">이 시나리오에서 호출 하는 경우 contextOwnsConnection 플래그를 false로 설정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-141">The contextOwnsConnection flag must be set to false when called in this scenario.</span></span> <span data-ttu-id="89062-142">이는 작업이 완료 될 때 연결을 닫지 않아야 함을 Entity Framework에 알리기 때문에 중요 합니다. 예를 들어 아래 줄 4를 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="89062-142">This is important as it informs Entity Framework that it should not close the connection when it is done with it (for example, see line 4 below):</span></span>  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

<span data-ttu-id="89062-143">또한 IsolationLevel를 사용 하 여 (기본 설정을 방지 하려는 경우) 트랜잭션을 직접 시작 하 고 연결에서 이미 시작 된 기존 트랜잭션이 있음을 Entity Framework 합니다 (아래의 33 줄 참조).</span><span class="sxs-lookup"><span data-stu-id="89062-143">Furthermore, you must start the transaction yourself (including the IsolationLevel if you want to avoid the default setting) and let Entity Framework know that there is an existing transaction already started on the connection (see line 33 below).</span></span>  

<span data-ttu-id="89062-144">그런 다음 SqlConnection 자체 나 DbContext에서 직접 데이터베이스 작업을 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-144">Then you are free to execute database operations either directly on the SqlConnection itself, or on the DbContext.</span></span> <span data-ttu-id="89062-145">이러한 모든 작업은 하나의 트랜잭션 내에서 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="89062-145">All such operations are executed within one transaction.</span></span> <span data-ttu-id="89062-146">트랜잭션을 커밋하거나 롤백하고 해당 데이터베이스에서 삭제 ()를 호출 하는 일은 물론 데이터베이스 연결을 닫고 삭제 하기 위한 책임이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-146">You take responsibility for committing or rolling back the transaction and for calling Dispose() on it, as well as for closing and disposing the database connection.</span></span> <span data-ttu-id="89062-147">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="89062-147">For example:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
     class TransactionsExample
     {
        static void UsingExternalTransaction()
        {
            using (var conn = new SqlConnection("..."))
            {
               conn.Open();

               using (var sqlTxn = conn.BeginTransaction(System.Data.IsolationLevel.Snapshot))
               {
                   var sqlCommand = new SqlCommand();
                   sqlCommand.Connection = conn;
                   sqlCommand.Transaction = sqlTxn;
                   sqlCommand.CommandText =
                       @"UPDATE Blogs SET Rating = 5" +
                        " WHERE Name LIKE '%Entity Framework%'";
                   sqlCommand.ExecuteNonQuery();

                   using (var context =  
                     new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        context.Database.UseTransaction(sqlTxn);

                        var query =  context.Posts.Where(p => p.Blog.Rating >= 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }
                       context.SaveChanges();
                    }

                    sqlTxn.Commit();
                }
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a><span data-ttu-id="89062-148">트랜잭션을 지우는 중</span><span class="sxs-lookup"><span data-stu-id="89062-148">Clearing up the transaction</span></span>

<span data-ttu-id="89062-149">Null을 UseTransaction ()에 전달 하 여 현재 트랜잭션에 대 한 Entity Framework 정보를 지울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-149">You can pass null to Database.UseTransaction() to clear Entity Framework’s knowledge of the current transaction.</span></span> <span data-ttu-id="89062-150">이 작업을 수행 하는 경우 기존 트랜잭션을 커밋하거나 롤백하지 않기 때문에이 작업을 수행 하려는 경우에만 주의 해 서 사용 하는 것이 좋습니다. Entity Framework</span><span class="sxs-lookup"><span data-stu-id="89062-150">Entity Framework will neither commit nor rollback the existing transaction when you do this, so use with care and only if you’re sure this is what you want to do.</span></span>  

### <a name="errors-in-usetransaction"></a><span data-ttu-id="89062-151">UseTransaction의 오류</span><span class="sxs-lookup"><span data-stu-id="89062-151">Errors in UseTransaction</span></span>

<span data-ttu-id="89062-152">다음 경우에 트랜잭션을 전달 하는 경우 UseTransaction ()에서 예외가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="89062-152">You will see an exception from Database.UseTransaction() if you pass a transaction when:</span></span>  
- <span data-ttu-id="89062-153">기존 트랜잭션이 이미 있는 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="89062-153">Entity Framework already has an existing transaction</span></span>  
- <span data-ttu-id="89062-154">Entity Framework은 (는) TransactionScope 내에서 이미 작동 중입니다.</span><span class="sxs-lookup"><span data-stu-id="89062-154">Entity Framework is already operating within a TransactionScope</span></span>  
- <span data-ttu-id="89062-155">전달 된 트랜잭션의 연결 개체가 null입니다.</span><span class="sxs-lookup"><span data-stu-id="89062-155">The connection object in the transaction passed is null.</span></span> <span data-ttu-id="89062-156">즉, 트랜잭션이 연결과 연결 되지 않습니다. 일반적으로 트랜잭션이 이미 완료 된 부호입니다.</span><span class="sxs-lookup"><span data-stu-id="89062-156">That is, the transaction is not associated with a connection – usually this is a sign that that transaction has already completed</span></span>  
- <span data-ttu-id="89062-157">전달 된 트랜잭션의 연결 개체가 Entity Framework의 연결과 일치 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-157">The connection object in the transaction passed does not match the Entity Framework’s connection.</span></span>  

## <a name="using-transactions-with-other-features"></a><span data-ttu-id="89062-158">다른 기능과 함께 트랜잭션 사용</span><span class="sxs-lookup"><span data-stu-id="89062-158">Using transactions with other features</span></span>  

<span data-ttu-id="89062-159">이 섹션에서는 위의 트랜잭션이와 상호 작용 하는 방법에 대해 자세히 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-159">This section details how the above transactions interact with:</span></span>  

- <span data-ttu-id="89062-160">연결 복원력</span><span class="sxs-lookup"><span data-stu-id="89062-160">Connection resiliency</span></span>  
- <span data-ttu-id="89062-161">비동기 메서드</span><span class="sxs-lookup"><span data-stu-id="89062-161">Asynchronous methods</span></span>  
- <span data-ttu-id="89062-162">TransactionScope 트랜잭션</span><span class="sxs-lookup"><span data-stu-id="89062-162">TransactionScope transactions</span></span>  

### <a name="connection-resiliency"></a><span data-ttu-id="89062-163">연결 복원력</span><span class="sxs-lookup"><span data-stu-id="89062-163">Connection Resiliency</span></span>  

<span data-ttu-id="89062-164">새 연결 복원 력 기능은 사용자가 시작한 트랜잭션에서 작동 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-164">The new Connection Resiliency feature does not work with user-initiated transactions.</span></span> <span data-ttu-id="89062-165">자세한 내용은 [실행 다시 시도 전략](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="89062-165">For details, see [Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span></span>  

### <a name="asynchronous-programming"></a><span data-ttu-id="89062-166">비동기 프로그래밍</span><span class="sxs-lookup"><span data-stu-id="89062-166">Asynchronous Programming</span></span>  

<span data-ttu-id="89062-167">이전 섹션에 설명 된 방법에는 [비동기 쿼리 및 저장 메서드](~/ef6/fundamentals/async.md
)를 사용할 수 있는 추가 옵션이 나 설정이 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-167">The approach outlined in the previous sections needs no further options or settings to work with the [asynchronous query and save methods](~/ef6/fundamentals/async.md
).</span></span> <span data-ttu-id="89062-168">그러나 비동기 메서드 내에서 수행 하는 작업에 따라이로 인해 전체 응용 프로그램의 성능에 좋지 않은 교착 상태나 차단이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-168">But be aware that, depending on what you do within the asynchronous methods, this may result in long-running transactions – which can in turn cause deadlocks or blocking which is bad for the performance of the overall application.</span></span>  

### <a name="transactionscope-transactions"></a><span data-ttu-id="89062-169">TransactionScope 트랜잭션</span><span class="sxs-lookup"><span data-stu-id="89062-169">TransactionScope Transactions</span></span>  

<span data-ttu-id="89062-170">EF6 이전에는 더 큰 범위 트랜잭션을 제공 하는 권장 방법을 TransactionScope 개체를 사용 했습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-170">Prior to EF6 the recommended way of providing larger scope transactions was to use a TransactionScope object:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void UsingTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeOption.Required))
            {
                using (var conn = new SqlConnection("..."))
                {
                    conn.Open();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    sqlCommand.ExecuteNonQuery();

                    using (var context =
                        new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }
                        context.SaveChanges();
                    }
                }

                scope.Complete();
            }
        }
    }
}
```  

<span data-ttu-id="89062-171">SqlConnection 및 Entity Framework는 모두 앰비언트 TransactionScope 트랜잭션을 사용 하므로 함께 커밋됩니다.</span><span class="sxs-lookup"><span data-stu-id="89062-171">The SqlConnection and Entity Framework would both use the ambient TransactionScope transaction and hence be committed together.</span></span>  

<span data-ttu-id="89062-172">.NET 4.5.1 TransactionScope부터 [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) 열거를 사용 하 여 비동기 메서드를 사용 하도록 업데이트 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-172">Starting with .NET 4.5.1 TransactionScope has been updated to also work with asynchronous methods via the use of the [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeration:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        public static void AsyncTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeAsyncFlowOption.Enabled))
            {
                using (var conn = new SqlConnection("..."))
                {
                    await conn.OpenAsync();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    await sqlCommand.ExecuteNonQueryAsync();

                    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        await context.SaveChangesAsync();
                    }
                }
            }
        }
    }
}
```  

<span data-ttu-id="89062-173">TransactionScope 접근 방식에는 여전히 몇 가지 제한 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-173">There are still some limitations to the TransactionScope approach:</span></span>  

- <span data-ttu-id="89062-174">비동기 메서드를 사용 하려면 .NET 4.5.1 이상이 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-174">Requires .NET 4.5.1 or greater to work with asynchronous methods.</span></span>  
- <span data-ttu-id="89062-175">단 하나의 연결만 있고 (클라우드 시나리오는 분산 트랜잭션을 지원 하지 않는 경우) 클라우드 시나리오에서 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-175">It cannot be used in cloud scenarios unless you are sure you have one and only one connection (cloud scenarios do not support distributed transactions).</span></span>  
- <span data-ttu-id="89062-176">이전 섹션의 UseTransaction () 방법과 함께 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-176">It cannot be combined with the Database.UseTransaction() approach of the previous sections.</span></span>  
- <span data-ttu-id="89062-177">DDL을 실행 하 고 MSDTC 서비스를 통해 분산 트랜잭션을 사용 하도록 설정 하지 않은 경우 예외를 throw 합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-177">It will throw exceptions if you issue any DDL and have not enabled distributed transactions through the MSDTC Service.</span></span>  

<span data-ttu-id="89062-178">TransactionScope 방법의 장점은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-178">Advantages of the TransactionScope approach:</span></span>  

- <span data-ttu-id="89062-179">지정 된 데이터베이스에 두 개 이상의 연결을 설정 하거나 동일한 트랜잭션 내의 다른 데이터베이스에 대 한 연결을 사용 하 여 하나의 데이터베이스에 연결을 결합 하는 경우 로컬 트랜잭션이 분산 트랜잭션으로 자동 업그레이드 됩니다 (참고: 다음이 있어야 합니다. 이 작업에 대 한 분산 트랜잭션을 허용 하도록 MSDTC 서비스를 구성 했습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-179">It will automatically upgrade a local transaction to a distributed transaction if you make more than one connection to a given database or combine a connection to one database with a connection to a different database within the same transaction (note: you must have the MSDTC service configured to allow distributed transactions for this to work).</span></span>  
- <span data-ttu-id="89062-180">코딩의 용이성</span><span class="sxs-lookup"><span data-stu-id="89062-180">Ease of coding.</span></span> <span data-ttu-id="89062-181">트랜잭션을 앰비언트로 처리 하 고 명시적으로 제어 하는 대신 백그라운드에서 암시적으로 처리 하는 것을 선호 하는 경우에는 TransactionScope 방법이 더 적합 합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-181">If you prefer the transaction to be ambient and dealt with implicitly in the background rather than explicitly under you control then the TransactionScope approach may suit you better.</span></span>  

<span data-ttu-id="89062-182">요약 하자면, 위의 새 BeginTransaction () 및 UseTransaction () Api를 사용 하면 대부분의 사용자에 게 더 이상 TransactionScope 방법이 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-182">In summary, with the new Database.BeginTransaction() and Database.UseTransaction() APIs above, the TransactionScope approach is no longer necessary for most users.</span></span> <span data-ttu-id="89062-183">계속 해 서 TransactionScope를 사용 하는 경우 위의 제한 사항을 알고 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="89062-183">If you do continue to use TransactionScope then be aware of the above limitations.</span></span> <span data-ttu-id="89062-184">가능한 경우 이전 섹션에 설명 된 방법을 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="89062-184">We recommend using the approach outlined in the previous sections instead where possible.</span></span>  
