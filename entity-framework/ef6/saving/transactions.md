---
title: 트랜잭션-EF6 사용
author: divega
ms.date: 10/23/2016
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 7197733ab25c8475746e7863963384730919e3ff
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489780"
---
# <a name="working-with-transactions"></a><span data-ttu-id="ef809-102">트랜잭션 사용</span><span class="sxs-lookup"><span data-stu-id="ef809-102">Working with Transactions</span></span>
> [!NOTE]
> <span data-ttu-id="ef809-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="ef809-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="ef809-105">이 문서에서는 트랜잭션 사용 하 여 작업을 쉽게 EF5 이후 추가 했습니다의 향상 된 기능을 포함 하 여 EF6에서 트랜잭션을 사용 하 여 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-105">This document will describe using transactions in EF6 including the enhancements we have added since EF5 to make working with transactions easy.</span></span>  

## <a name="what-ef-does-by-default"></a><span data-ttu-id="ef809-106">기본적으로 EF는 무엇입니까</span><span class="sxs-lookup"><span data-stu-id="ef809-106">What EF does by default</span></span>  

<span data-ttu-id="ef809-107">Entity Framework의 모든 버전에서 실행할 때마다 **savechanges ()** 트랜잭션에서 해당 작업을 래핑하는 삽입, 업데이트 또는 프레임 워크를 데이터베이스에서 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-107">In all versions of Entity Framework, whenever you execute **SaveChanges()** to insert, update or delete on the database the framework will wrap that operation in a transaction.</span></span> <span data-ttu-id="ef809-108">이 트랜잭션 작업을 실행할 시간 동안만 지속 되 고 완료 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-108">This transaction lasts only long enough to execute the operation and then completes.</span></span> <span data-ttu-id="ef809-109">이러한 다른 작업을 실행 하면 새 트랜잭션이 시작 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-109">When you execute another such operation a new transaction is started.</span></span>  

<span data-ttu-id="ef809-110">EF6부터 **Database.ExecuteSqlCommand()** 기본적으로는 자동으로 줄 명령을 트랜잭션에 있는 이미 해제 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-110">Starting with EF6 **Database.ExecuteSqlCommand()** by default will wrap the command in a transaction if one was not already present.</span></span> <span data-ttu-id="ef809-111">이 메서드의 오버 로드 하려는 경우이 동작을 재정의할 수 있도록 하는 경우</span><span class="sxs-lookup"><span data-stu-id="ef809-111">There are overloads of this method that allow you to override this behavior if you wish.</span></span> <span data-ttu-id="ef809-112">와 같은 Api 통해 모델에 포함 하는 저장된 프로시저의 EF6 실행에도 **ObjectContext.ExecuteFunction()** (한다는 점을 제외 하면 기본 동작은 현재 재정의할 수 없습니다) 동일한 작업을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-112">Also in EF6 execution of stored procedures included in the model through APIs such as **ObjectContext.ExecuteFunction()** does the same (except that the default behavior cannot at the moment be overridden).</span></span>  

<span data-ttu-id="ef809-113">두 경우 모두 트랜잭션의 격리 수준을 격리 수준 데이터베이스 공급자에 기본 설정인 것으로 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-113">In either case, the isolation level of the transaction is whatever isolation level the database provider considers its default setting.</span></span> <span data-ttu-id="ef809-114">기본적으로 예를 들어 SQL Server에서이 READ COMMITTED입니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-114">By default, for instance, on SQL Server this is READ COMMITTED.</span></span>  

<span data-ttu-id="ef809-115">Entity Framework 트랜잭션에서 쿼리를 래핑하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-115">Entity Framework does not wrap queries in a transaction.</span></span>  

<span data-ttu-id="ef809-116">이 기본 기능은 사용자 하 고, 따라서 EF6;에서 다른 아무 작업도 수행 하지 않아도 많은 적합 항상 수행한 것 처럼 코드를 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-116">This default functionality is suitable for a lot of users and if so there is no need to do anything different in EF6; just write the code as you always did.</span></span>  

<span data-ttu-id="ef809-117">그러나 일부 사용자의 거래를 통해 제어 필요 – 다음 섹션에서 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-117">However some users require greater control over their transactions – this is covered in the following sections.</span></span>  

## <a name="how-the-apis-work"></a><span data-ttu-id="ef809-118">Api의 작동 방법</span><span class="sxs-lookup"><span data-stu-id="ef809-118">How the APIs work</span></span>  

<span data-ttu-id="ef809-119">EF6 Entity Framework 전에 개발해왔으므로 연결을 여는 데이터베이스 자체 (해당 예외가 발생가 이미 열려 있는 연결 된 경우).</span><span class="sxs-lookup"><span data-stu-id="ef809-119">Prior to EF6 Entity Framework insisted on opening the database connection itself (it threw an exception if it was passed a connection that was already open).</span></span> <span data-ttu-id="ef809-120">사용자 하나의 트랜잭션으로 여러 작업을 래핑할 수는 유일한 방법은 사용 되었음을 의미 하 트랜잭션이 열려 있는 연결에만 시작할 수 있으므로이 [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) 사용할지는  **ObjectContext.Connection** 속성과 시작 호출 **open ()** 하 고 **begintransaction ()** 에서 반환 된 직접 **EntityConnection** 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-120">Since a transaction can only be started on an open connection, this meant that the only way a user could wrap several operations into one transaction was either to use a [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) or use the **ObjectContext.Connection** property and start calling **Open()** and **BeginTransaction()** directly on the returned **EntityConnection** object.</span></span> <span data-ttu-id="ef809-121">또한 직접 기본 데이터베이스 연결에서 트랜잭션을 시작 했습니다 하는 경우 데이터베이스를 연결 하는 API 호출 실패 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-121">In addition, API calls which contacted the database would fail if you had started a transaction on the underlying database connection on your own.</span></span>  

> [!NOTE]
> <span data-ttu-id="ef809-122">Entity Framework 6에서 닫힌된 연결을 받아들일만 제한이 제거 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-122">The limitation of only accepting closed connections was removed in Entity Framework 6.</span></span> <span data-ttu-id="ef809-123">자세한 내용은 참조 하세요 [연결 관리](~/ef6/fundamentals/connection-management.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-123">For details, see [Connection Management](~/ef6/fundamentals/connection-management.md).</span></span>  

<span data-ttu-id="ef809-124">EF6을 사용 하 여 프레임 워크를 지금 시작 하는 작업에 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-124">Starting with EF6 the framework now provides:</span></span>  

1. <span data-ttu-id="ef809-125">**Database.BeginTransaction()** : 시작 하 고 여러 작업이 동일한 트랜잭션 내에 결합 되도록 하는 기존 DbContext – 내에서 직접 트랜잭션을 완료 하려면 사용자에 대 한 쉬운 방법 이므로 모두 커밋되거나 모두 롤백 하나로.</span><span class="sxs-lookup"><span data-stu-id="ef809-125">**Database.BeginTransaction()** : An easier method for a user to start and complete transactions themselves within an existing DbContext – allowing several operations to be combined within the same transaction and hence either all committed or all rolled back as one.</span></span> <span data-ttu-id="ef809-126">또한 보다 쉽게 트랜잭션 격리 수준을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-126">It also allows the user to more easily specify the isolation level for the transaction.</span></span>  
2. <span data-ttu-id="ef809-127">**Database.UseTransaction()** : Entity Framework 외부에서 시작 된 트랜잭션을 사용 하도록 DbContext 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-127">**Database.UseTransaction()** : which allows the DbContext to use a transaction which was started outside of the Entity Framework.</span></span>  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a><span data-ttu-id="ef809-128">여러 작업을 동일한 컨텍스트 내에서 하나의 트랜잭션으로 결합</span><span class="sxs-lookup"><span data-stu-id="ef809-128">Combining several operations into one transaction within the same context</span></span>  

<span data-ttu-id="ef809-129">**Database.BeginTransaction()** 는 두 가지 재정의가 – 명시적를 사용 하는 하나 [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) 및 인수를 받지 IsolationLevel 기본 데이터베이스 공급자에서 기본값을 사용 하는 하나입니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-129">**Database.BeginTransaction()** has two overrides – one which takes an explicit [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) and one which takes no arguments and uses the default IsolationLevel from the underlying database provider.</span></span> <span data-ttu-id="ef809-130">두 경우 모두 반환을 **DbContextTransaction** 제공 하는 개체 **commit ()** 하 고 **Rollback()** 기본 저장소에 커밋 및 롤백 수행 하는 방법 트랜잭션입니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-130">Both overrides return a **DbContextTransaction** object which provides **Commit()** and **Rollback()** methods which perform commit and rollback on the underlying store transaction.</span></span>  

<span data-ttu-id="ef809-131">합니다 **DbContextTransaction** 커밋되거나 롤백 되 면 삭제 될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-131">The **DbContextTransaction** is meant to be disposed once it has been committed or rolled back.</span></span> <span data-ttu-id="ef809-132">한 가지 쉬운 방식으로이 작업을 수행 하는 **using(...) {...}**</span><span class="sxs-lookup"><span data-stu-id="ef809-132">One easy way to accomplish this is the **using(…) {…}**</span></span> <span data-ttu-id="ef809-133">자동으로 호출 구문을 **dispose ()** 완료 블록을 사용 하 여:</span><span class="sxs-lookup"><span data-stu-id="ef809-133">syntax which will automatically call **Dispose()** when the using block completes:</span></span>  

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
                    try
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
                    catch (Exception)
                    {
                        dbContextTransaction.Rollback();
                    }
                }
            }
        }
    }
}
```  

> [!NOTE]
> <span data-ttu-id="ef809-134">기본 저장소 연결이 열려 있는지 필요한 트랜잭션을 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-134">Beginning a transaction requires that the underlying store connection is open.</span></span> <span data-ttu-id="ef809-135">Database.BeginTransaction()를 호출 하므로 아직 열려 있지 않은 경우 연결이 열립니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-135">So calling Database.BeginTransaction() will open the connection  if it is not already opened.</span></span> <span data-ttu-id="ef809-136">DbContextTransaction 해당 연결을 연 경우 다음 닫힙니다 고 dispose () 호출 되 면 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-136">If DbContextTransaction opened the connection then it will close it when Dispose() is called.</span></span>  

### <a name="passing-an-existing-transaction-to-the-context"></a><span data-ttu-id="ef809-137">기존 트랜잭션 컨텍스트를 전달</span><span class="sxs-lookup"><span data-stu-id="ef809-137">Passing an existing transaction to the context</span></span>  

<span data-ttu-id="ef809-138">경우에 따라는 범위 내의 더 광범위 하 고 작업 EF 외부에 있지만 동일한 데이터베이스를 완전히 포함 된 트랜잭션을 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-138">Sometimes you would like a transaction which is even broader in scope and which includes operations on the same database but outside of EF completely.</span></span> <span data-ttu-id="ef809-139">이렇게 하려면 연결을 열 및 직접 트랜잭션을 시작 하며 EF는) 이미 열린 데이터베이스 연결을 사용 하는 데 b) 기존 트랜잭션에 해당 연결에서 사용 하 여 알려 줍니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-139">To accomplish this you must open the connection and start the transaction yourself and then tell EF a) to use the already-opened database connection, and b) to use the existing transaction on that connection.</span></span>  

<span data-ttu-id="ef809-140">이렇게 하려면 정의 하 고 부울 i) 기존 연결 매개 변수 및 contextOwnsConnection ii)을 사용 하는 DbContext 생성자 중 하나에서 상속 되는 컨텍스트 클래스에서 생성자를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-140">To do this you must define and use a constructor on your context class which inherits from one of the DbContext constructors which take i) an existing connection parameter and ii) the contextOwnsConnection boolean.</span></span>  

> [!NOTE]
> <span data-ttu-id="ef809-141">ContextOwnsConnection 플래그는이 시나리오에서 호출 된 경우 false로 설정 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-141">The contextOwnsConnection flag must be set to false when called in this scenario.</span></span> <span data-ttu-id="ef809-142">이 닫지 마십시오 연결 된 완료 되 면 Entity Framework 알려줍니다 중요입니다 (예를 들어, 줄 아래 4 참조).</span><span class="sxs-lookup"><span data-stu-id="ef809-142">This is important as it informs Entity Framework that it should not close the connection when it is done with it (for example, see line 4 below):</span></span>  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

<span data-ttu-id="ef809-143">(기본 설정 하지 않으려는 경우는 IsolationLevel 포함) 직접 트랜잭션을 시작 하 고 기존 트랜잭션에 연결에서 이미 시작 되었음을 알리는 Entity Framework를 사용 해야 또한 (줄 33 아래 참조).</span><span class="sxs-lookup"><span data-stu-id="ef809-143">Furthermore, you must start the transaction yourself (including the IsolationLevel if you want to avoid the default setting) and let Entity Framework know that there is an existing transaction already started on the connection (see line 33 below).</span></span>  

<span data-ttu-id="ef809-144">그런 다음 자체 SqlConnection 또는 DbContext 직접 데이터베이스 작업을 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-144">Then you are free to execute database operations either directly on the SqlConnection itself, or on the DbContext.</span></span> <span data-ttu-id="ef809-145">이러한 모든 작업은 단일 트랜잭션 내에서 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-145">All such operations are executed within one transaction.</span></span> <span data-ttu-id="ef809-146">수행한 커밋 또는 트랜잭션 롤백에 대 한 및 dispose ()를 호출 및 닫기 및 데이터베이스 연결을 삭제 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-146">You take responsibility for committing or rolling back the transaction and for calling Dispose() on it, as well as for closing and disposing the database connection.</span></span> <span data-ttu-id="ef809-147">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="ef809-147">For example:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
sing System.Transactions;

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
                   try
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
                    catch (Exception)
                    {
                        sqlTxn.Rollback();
                    }
                }
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a><span data-ttu-id="ef809-148">트랜잭션을 지우기</span><span class="sxs-lookup"><span data-stu-id="ef809-148">Clearing up the transaction</span></span>

<span data-ttu-id="ef809-149">현재 트랜잭션의 Entity Framework의 기술 자료를 지우려면 Database.UseTransaction()에 null을 전달할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-149">You can pass null to Database.UseTransaction() to clear Entity Framework’s knowledge of the current transaction.</span></span> <span data-ttu-id="ef809-150">Entity Framework는 모두 커밋 또는 롤백 이렇게 하면 기존 트랜잭션에 있으므로 신중 하 게 사용 하 고 수행 하려고 할 것이 확실 한 경우에 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-150">Entity Framework will neither commit nor rollback the existing transaction when you do this, so use with care and only if you’re sure this is what you want to do.</span></span>  

### <a name="errors-in-usetransaction"></a><span data-ttu-id="ef809-151">UseTransaction의 오류</span><span class="sxs-lookup"><span data-stu-id="ef809-151">Errors in UseTransaction</span></span>

<span data-ttu-id="ef809-152">트랜잭션을 전달 하는 경우 예외가 Database.UseTransaction() 나타납니다 경우:</span><span class="sxs-lookup"><span data-stu-id="ef809-152">You will see an exception from Database.UseTransaction() if you pass a transaction when:</span></span>  
- <span data-ttu-id="ef809-153">Entity Framework에 이미 기존 트랜잭션</span><span class="sxs-lookup"><span data-stu-id="ef809-153">Entity Framework already has an existing transaction</span></span>  
- <span data-ttu-id="ef809-154">Entity Framework는 TransactionScope 내에서 이미 작동</span><span class="sxs-lookup"><span data-stu-id="ef809-154">Entity Framework is already operating within a TransactionScope</span></span>  
- <span data-ttu-id="ef809-155">전달 된 트랜잭션에서 연결 개체가 null입니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-155">The connection object in the transaction passed is null.</span></span> <span data-ttu-id="ef809-156">즉, 트랜잭션은 연결과 관련 된 아닙니다. – 일반적으로이 트랜잭션이 이미 완료 된 기호</span><span class="sxs-lookup"><span data-stu-id="ef809-156">That is, the transaction is not associated with a connection – usually this is a sign that that transaction has already completed</span></span>  
- <span data-ttu-id="ef809-157">전달 된 트랜잭션 연결 개체는 Entity Framework의 연결을 일치 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-157">The connection object in the transaction passed does not match the Entity Framework’s connection.</span></span>  

## <a name="using-transactions-with-other-features"></a><span data-ttu-id="ef809-158">다른 기능을 사용 하 여 트랜잭션 사용</span><span class="sxs-lookup"><span data-stu-id="ef809-158">Using transactions with other features</span></span>  

<span data-ttu-id="ef809-159">이 섹션에서는 위의 트랜잭션 상호 작용 하는 방법을 자세히 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-159">This section details how the above transactions interact with:</span></span>  

- <span data-ttu-id="ef809-160">연결 복원력</span><span class="sxs-lookup"><span data-stu-id="ef809-160">Connection resiliency</span></span>  
- <span data-ttu-id="ef809-161">비동기 메서드</span><span class="sxs-lookup"><span data-stu-id="ef809-161">Asynchronous methods</span></span>  
- <span data-ttu-id="ef809-162">TransactionScope 트랜잭션</span><span class="sxs-lookup"><span data-stu-id="ef809-162">TransactionScope transactions</span></span>  

### <a name="connection-resiliency"></a><span data-ttu-id="ef809-163">연결 복원력</span><span class="sxs-lookup"><span data-stu-id="ef809-163">Connection Resiliency</span></span>  

<span data-ttu-id="ef809-164">사용자가 시작한 트랜잭션을 사용 하 여 새 연결 복원 력 기능이 작동 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-164">The new Connection Resiliency feature does not work with user-initiated transactions.</span></span> <span data-ttu-id="ef809-165">자세한 내용은 참조 하세요 [재시도 실행 전략](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported)합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-165">For details, see [Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span></span>  

### <a name="asynchronous-programming"></a><span data-ttu-id="ef809-166">비동기 프로그래밍</span><span class="sxs-lookup"><span data-stu-id="ef809-166">Asynchronous Programming</span></span>  

<span data-ttu-id="ef809-167">이전 섹션에서 설명한 방법이 필요 없는 추가 옵션 또는 설정을 사용 하는 [비동기 메서드를 저장 및 쿼리](~/ef6/fundamentals/async.md
)합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-167">The approach outlined in the previous sections needs no further options or settings to work with the [asynchronous query and save methods](~/ef6/fundamentals/async.md
).</span></span> <span data-ttu-id="ef809-168">하지만, 비동기 메서드 내에서 수행할 작업에 따라이 발생할 장기 실행 트랜잭션-전체 응용 프로그램의 성능에 부정적는 차단 또는 교착 상태에서 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-168">But be aware that, depending on what you do within the asynchronous methods, this may result in long-running transactions – which can in turn cause deadlocks or blocking which is bad for the performance of the overall application.</span></span>  

### <a name="transactionscope-transactions"></a><span data-ttu-id="ef809-169">TransactionScope 트랜잭션</span><span class="sxs-lookup"><span data-stu-id="ef809-169">TransactionScope Transactions</span></span>  

<span data-ttu-id="ef809-170">EF6 전에 더 큰 범위 트랜잭션을 제공 하는 권장된 방법을 TransactionScope 개체를 사용 하는 것:</span><span class="sxs-lookup"><span data-stu-id="ef809-170">Prior to EF6 the recommended way of providing larger scope transactions was to use a TransactionScope object:</span></span>  

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

<span data-ttu-id="ef809-171">SqlConnection 및 Entity Framework는 모두 앰비언트 TransactionScope 트랜잭션을 사용 하 고 따라서 함께 커밋할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-171">The SqlConnection and Entity Framework would both use the ambient TransactionScope transaction and hence be committed together.</span></span>  

<span data-ttu-id="ef809-172">.NET 4.5.1도 사용을 통해 비동기 메서드를 사용 하 여 작동 하도록 업데이트 되었습니다 TransactionScope를 사용 하 여 시작 합니다 [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) 열거형:</span><span class="sxs-lookup"><span data-stu-id="ef809-172">Starting with .NET 4.5.1 TransactionScope has been updated to also work with asynchronous methods via the use of the [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeration:</span></span>  

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

<span data-ttu-id="ef809-173">여전히 몇 가지 제한 사항이 TransactionScope 방법:</span><span class="sxs-lookup"><span data-stu-id="ef809-173">There are still some limitations to the TransactionScope approach:</span></span>  

- <span data-ttu-id="ef809-174">.NET 4.5.1이 필요 이상의 비동기 메서드를 사용 하 여 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-174">Requires .NET 4.5.1 or greater to work with asynchronous methods.</span></span>  
- <span data-ttu-id="ef809-175">하나의 연결이 확실 하지 않으면 클라우드 시나리오에서 사용할 수 없습니다 (클라우드 시나리오 분산된 트랜잭션을 지원 하지 않습니다).</span><span class="sxs-lookup"><span data-stu-id="ef809-175">It cannot be used in cloud scenarios unless you are sure you have one and only one connection (cloud scenarios do not support distributed transactions).</span></span>  
- <span data-ttu-id="ef809-176">이전 섹션의 Database.UseTransaction() 방법을 함께 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-176">It cannot be combined with the Database.UseTransaction() approach of the previous sections.</span></span>  
- <span data-ttu-id="ef809-177">DDL을 실행 하 고 MSDTC 서비스를 통해 분산된 트랜잭션을 사용 하지 않도록 설정한 경우 예외가 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-177">It will throw exceptions if you issue any DDL and have not enabled distributed transactions through the MSDTC Service.</span></span>  

<span data-ttu-id="ef809-178">TransactionScope 방식의 이점은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-178">Advantages of the TransactionScope approach:</span></span>  

- <span data-ttu-id="ef809-179">이 자동으로 업그레이드 로컬 트랜잭션을 분산 트랜잭션으로 둘 이상의 연결 지정된 된 데이터베이스 또는 동일한 트랜잭션에서 다른 데이터베이스에 대 한 연결을 사용 하 여 하나의 데이터베이스에 대 한 연결을 결합 하는 경우 (참고: 있어야 MSDTC 서비스가 작동 하려면이 옵션에 대 한 분산된 트랜잭션을 허용 하도록 구성).</span><span class="sxs-lookup"><span data-stu-id="ef809-179">It will automatically upgrade a local transaction to a distributed transaction if you make more than one connection to a given database or combine a connection to one database with a connection to a different database within the same transaction (note: you must have the MSDTC service configured to allow distributed transactions for this to work).</span></span>  
- <span data-ttu-id="ef809-180">코딩 편의성입니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-180">Ease of coding.</span></span> <span data-ttu-id="ef809-181">제어 아래에서 명시적으로 하는 것이 아니라 앰비언트 되며 백그라운드에서 암시적으로 사용 하 여 돌림 트랜잭션을 선호 하는 경우 다음 TransactionScope 접근 방식은 적합할 수 있습니다 더 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-181">If you prefer the transaction to be ambient and dealt with implicitly in the background rather than explicitly under you control then the TransactionScope approach may suit you better.</span></span>  

<span data-ttu-id="ef809-182">요약 하면, 새 Database.BeginTransaction() 및 위의 Database.UseTransaction() Api와 TransactionScope 방법은 더 이상 대부분의 사용자에 대 한 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-182">In summary, with the new Database.BeginTransaction() and Database.UseTransaction() APIs above, the TransactionScope approach is no longer necessary for most users.</span></span> <span data-ttu-id="ef809-183">TransactionScope를 사용 하 여 계속 수행 되 면 다음는 위의 제한 사항을 고려해 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-183">If you do continue to use TransactionScope then be aware of the above limitations.</span></span> <span data-ttu-id="ef809-184">가능한 경우 대신 이전 섹션에서 설명한 방법을 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="ef809-184">We recommend using the approach outlined in the previous sections instead where possible.</span></span>  
