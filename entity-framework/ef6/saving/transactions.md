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
# <a name="working-with-transactions"></a>트랜잭션 사용
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

이 문서에서는 트랜잭션 사용 하 여 작업을 쉽게 EF5 이후 추가 했습니다의 향상 된 기능을 포함 하 여 EF6에서 트랜잭션을 사용 하 여 설명 합니다.  

## <a name="what-ef-does-by-default"></a>기본적으로 EF는 무엇입니까  

Entity Framework의 모든 버전에서 실행할 때마다 **savechanges ()** 트랜잭션에서 해당 작업을 래핑하는 삽입, 업데이트 또는 프레임 워크를 데이터베이스에서 삭제 합니다. 이 트랜잭션 작업을 실행할 시간 동안만 지속 되 고 완료 합니다. 이러한 다른 작업을 실행 하면 새 트랜잭션이 시작 됩니다.  

EF6부터 **Database.ExecuteSqlCommand()** 기본적으로는 자동으로 줄 명령을 트랜잭션에 있는 이미 해제 되었습니다. 이 메서드의 오버 로드 하려는 경우이 동작을 재정의할 수 있도록 하는 경우 와 같은 Api 통해 모델에 포함 하는 저장된 프로시저의 EF6 실행에도 **ObjectContext.ExecuteFunction()** (한다는 점을 제외 하면 기본 동작은 현재 재정의할 수 없습니다) 동일한 작업을 수행 합니다.  

두 경우 모두 트랜잭션의 격리 수준을 격리 수준 데이터베이스 공급자에 기본 설정인 것으로 간주 됩니다. 기본적으로 예를 들어 SQL Server에서이 READ COMMITTED입니다.  

Entity Framework 트랜잭션에서 쿼리를 래핑하지 않습니다.  

이 기본 기능은 사용자 하 고, 따라서 EF6;에서 다른 아무 작업도 수행 하지 않아도 많은 적합 항상 수행한 것 처럼 코드를 작성 합니다.  

그러나 일부 사용자의 거래를 통해 제어 필요 – 다음 섹션에서 설명 합니다.  

## <a name="how-the-apis-work"></a>Api의 작동 방법  

EF6 Entity Framework 전에 개발해왔으므로 연결을 여는 데이터베이스 자체 (해당 예외가 발생가 이미 열려 있는 연결 된 경우). 사용자 하나의 트랜잭션으로 여러 작업을 래핑할 수는 유일한 방법은 사용 되었음을 의미 하 트랜잭션이 열려 있는 연결에만 시작할 수 있으므로이 [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) 사용할지는  **ObjectContext.Connection** 속성과 시작 호출 **open ()** 하 고 **begintransaction ()** 에서 반환 된 직접 **EntityConnection** 개체입니다. 또한 직접 기본 데이터베이스 연결에서 트랜잭션을 시작 했습니다 하는 경우 데이터베이스를 연결 하는 API 호출 실패 합니다.  

> [!NOTE]
> Entity Framework 6에서 닫힌된 연결을 받아들일만 제한이 제거 되었습니다. 자세한 내용은 참조 하세요 [연결 관리](~/ef6/fundamentals/connection-management.md)합니다.  

EF6을 사용 하 여 프레임 워크를 지금 시작 하는 작업에 제공 합니다.  

1. **Database.BeginTransaction()** : 시작 하 고 여러 작업이 동일한 트랜잭션 내에 결합 되도록 하는 기존 DbContext – 내에서 직접 트랜잭션을 완료 하려면 사용자에 대 한 쉬운 방법 이므로 모두 커밋되거나 모두 롤백 하나로. 또한 보다 쉽게 트랜잭션 격리 수준을 지정할 수 있습니다.  
2. **Database.UseTransaction()** : Entity Framework 외부에서 시작 된 트랜잭션을 사용 하도록 DbContext 수 있습니다.  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a>여러 작업을 동일한 컨텍스트 내에서 하나의 트랜잭션으로 결합  

**Database.BeginTransaction()** 는 두 가지 재정의가 – 명시적를 사용 하는 하나 [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) 및 인수를 받지 IsolationLevel 기본 데이터베이스 공급자에서 기본값을 사용 하는 하나입니다. 두 경우 모두 반환을 **DbContextTransaction** 제공 하는 개체 **commit ()** 하 고 **Rollback()** 기본 저장소에 커밋 및 롤백 수행 하는 방법 트랜잭션입니다.  

합니다 **DbContextTransaction** 커밋되거나 롤백 되 면 삭제 될 것입니다. 한 가지 쉬운 방식으로이 작업을 수행 하는 **using(...) {...}** 자동으로 호출 구문을 **dispose ()** 완료 블록을 사용 하 여:  

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
> 기본 저장소 연결이 열려 있는지 필요한 트랜잭션을 시작 합니다. Database.BeginTransaction()를 호출 하므로 아직 열려 있지 않은 경우 연결이 열립니다. DbContextTransaction 해당 연결을 연 경우 다음 닫힙니다 고 dispose () 호출 되 면 합니다.  

### <a name="passing-an-existing-transaction-to-the-context"></a>기존 트랜잭션 컨텍스트를 전달  

경우에 따라는 범위 내의 더 광범위 하 고 작업 EF 외부에 있지만 동일한 데이터베이스를 완전히 포함 된 트랜잭션을 하려고 합니다. 이렇게 하려면 연결을 열 및 직접 트랜잭션을 시작 하며 EF는) 이미 열린 데이터베이스 연결을 사용 하는 데 b) 기존 트랜잭션에 해당 연결에서 사용 하 여 알려 줍니다.  

이렇게 하려면 정의 하 고 부울 i) 기존 연결 매개 변수 및 contextOwnsConnection ii)을 사용 하는 DbContext 생성자 중 하나에서 상속 되는 컨텍스트 클래스에서 생성자를 사용 해야 합니다.  

> [!NOTE]
> ContextOwnsConnection 플래그는이 시나리오에서 호출 된 경우 false로 설정 되어야 합니다. 이 닫지 마십시오 연결 된 완료 되 면 Entity Framework 알려줍니다 중요입니다 (예를 들어, 줄 아래 4 참조).  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

(기본 설정 하지 않으려는 경우는 IsolationLevel 포함) 직접 트랜잭션을 시작 하 고 기존 트랜잭션에 연결에서 이미 시작 되었음을 알리는 Entity Framework를 사용 해야 또한 (줄 33 아래 참조).  

그런 다음 자체 SqlConnection 또는 DbContext 직접 데이터베이스 작업을 실행할 수 있습니다. 이러한 모든 작업은 단일 트랜잭션 내에서 실행 됩니다. 수행한 커밋 또는 트랜잭션 롤백에 대 한 및 dispose ()를 호출 및 닫기 및 데이터베이스 연결을 삭제 해야 합니다. 예를 들어:  

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

### <a name="clearing-up-the-transaction"></a>트랜잭션을 지우기

현재 트랜잭션의 Entity Framework의 기술 자료를 지우려면 Database.UseTransaction()에 null을 전달할 수 있습니다. Entity Framework는 모두 커밋 또는 롤백 이렇게 하면 기존 트랜잭션에 있으므로 신중 하 게 사용 하 고 수행 하려고 할 것이 확실 한 경우에 됩니다.  

### <a name="errors-in-usetransaction"></a>UseTransaction의 오류

트랜잭션을 전달 하는 경우 예외가 Database.UseTransaction() 나타납니다 경우:  
- Entity Framework에 이미 기존 트랜잭션  
- Entity Framework는 TransactionScope 내에서 이미 작동  
- 전달 된 트랜잭션에서 연결 개체가 null입니다. 즉, 트랜잭션은 연결과 관련 된 아닙니다. – 일반적으로이 트랜잭션이 이미 완료 된 기호  
- 전달 된 트랜잭션 연결 개체는 Entity Framework의 연결을 일치 하지 않습니다.  

## <a name="using-transactions-with-other-features"></a>다른 기능을 사용 하 여 트랜잭션 사용  

이 섹션에서는 위의 트랜잭션 상호 작용 하는 방법을 자세히 설명 합니다.  

- 연결 복원력  
- 비동기 메서드  
- TransactionScope 트랜잭션  

### <a name="connection-resiliency"></a>연결 복원력  

사용자가 시작한 트랜잭션을 사용 하 여 새 연결 복원 력 기능이 작동 하지 않습니다. 자세한 내용은 참조 하세요 [재시도 실행 전략](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported)합니다.  

### <a name="asynchronous-programming"></a>비동기 프로그래밍  

이전 섹션에서 설명한 방법이 필요 없는 추가 옵션 또는 설정을 사용 하는 [비동기 메서드를 저장 및 쿼리](~/ef6/fundamentals/async.md
)합니다. 하지만, 비동기 메서드 내에서 수행할 작업에 따라이 발생할 장기 실행 트랜잭션-전체 응용 프로그램의 성능에 부정적는 차단 또는 교착 상태에서 발생할 수 있습니다.  

### <a name="transactionscope-transactions"></a>TransactionScope 트랜잭션  

EF6 전에 더 큰 범위 트랜잭션을 제공 하는 권장된 방법을 TransactionScope 개체를 사용 하는 것:  

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

SqlConnection 및 Entity Framework는 모두 앰비언트 TransactionScope 트랜잭션을 사용 하 고 따라서 함께 커밋할 수 있습니다.  

.NET 4.5.1도 사용을 통해 비동기 메서드를 사용 하 여 작동 하도록 업데이트 되었습니다 TransactionScope를 사용 하 여 시작 합니다 [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) 열거형:  

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

여전히 몇 가지 제한 사항이 TransactionScope 방법:  

- .NET 4.5.1이 필요 이상의 비동기 메서드를 사용 하 여 작동 합니다.  
- 하나의 연결이 확실 하지 않으면 클라우드 시나리오에서 사용할 수 없습니다 (클라우드 시나리오 분산된 트랜잭션을 지원 하지 않습니다).  
- 이전 섹션의 Database.UseTransaction() 방법을 함께 사용할 수 없습니다.  
- DDL을 실행 하 고 MSDTC 서비스를 통해 분산된 트랜잭션을 사용 하지 않도록 설정한 경우 예외가 throw 됩니다.  

TransactionScope 방식의 이점은 다음과 같습니다.  

- 이 자동으로 업그레이드 로컬 트랜잭션을 분산 트랜잭션으로 둘 이상의 연결 지정된 된 데이터베이스 또는 동일한 트랜잭션에서 다른 데이터베이스에 대 한 연결을 사용 하 여 하나의 데이터베이스에 대 한 연결을 결합 하는 경우 (참고: 있어야 MSDTC 서비스가 작동 하려면이 옵션에 대 한 분산된 트랜잭션을 허용 하도록 구성).  
- 코딩 편의성입니다. 제어 아래에서 명시적으로 하는 것이 아니라 앰비언트 되며 백그라운드에서 암시적으로 사용 하 여 돌림 트랜잭션을 선호 하는 경우 다음 TransactionScope 접근 방식은 적합할 수 있습니다 더 합니다.  

요약 하면, 새 Database.BeginTransaction() 및 위의 Database.UseTransaction() Api와 TransactionScope 방법은 더 이상 대부분의 사용자에 대 한 필요 합니다. TransactionScope를 사용 하 여 계속 수행 되 면 다음는 위의 제한 사항을 고려해 야 합니다. 가능한 경우 대신 이전 섹션에서 설명한 방법을 사용 하는 것이 좋습니다.  
