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
# <a name="working-with-transactions"></a>트랜잭션 작업
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

이 문서에서는 트랜잭션 작업을 쉽게 수행할 수 있도록 EF5 이후 추가 된 향상 된 기능을 포함 하 여 EF6의 트랜잭션을 사용 하는 방법을 설명 합니다.  

## <a name="what-ef-does-by-default"></a>기본적으로 수행 되는 EF  

모든 버전의 Entity Framework에서 **SaveChanges ()** 를 실행 하 여 데이터베이스에 삽입, 업데이트 또는 삭제할 때마다 프레임 워크는 해당 작업을 트랜잭션으로 래핑합니다. 이 트랜잭션은 작업을 실행 한 후 완료 되는 동안만 지속 됩니다. 다른 작업을 실행 하면 새 트랜잭션이 시작 됩니다.  

**ExecuteSqlCommand ()** 로 시작 하는 경우 EF6 ()에서 기본적으로 명령이 아직 없는 경우 해당 트랜잭션에서 명령을 래핑합니다. 원할 경우이 동작을 재정의할 수 있는이 메서드의 오버 로드가 있습니다. 또한 ObjectContext와 같은 Api를 통해 모델에 포함 된 저장 프로시저의 실행을 EF6 합니다. **ExecuteFunction ()** 는 동일한 기능을 수행 합니다. 단, 기본 동작은 현재 재정의할 수 없습니다.  

두 경우 모두 트랜잭션의 격리 수준은 데이터베이스 공급자가 기본 설정을 고려 하는 격리 수준입니다. 예를 들어 SQL Server에서이는 커밋된 읽기입니다.  

Entity Framework는 트랜잭션에서 쿼리를 래핑하지 않습니다.  

이 기본 기능은 많은 사용자에 게 적합 하며 EF6에서 다른 작업을 수행할 필요가 없습니다. 언제 든 지 코드를 작성 하면 됩니다.  

그러나 일부 사용자는 트랜잭션을 보다 강력 하 게 제어 해야 합니다 .이에 대해서는 다음 섹션에서 설명 합니다.  

## <a name="how-the-apis-work"></a>Api 작동 방법  

EF6 Entity Framework 이전에는 데이터베이스 연결 자체를 열 때 insisted (이미 열려 있는 연결이 전달 된 경우 예외를 throw). 트랜잭션은 열린 연결 에서만 시작할 수 있으므로 사용자가 여러 작업을 하나의 트랜잭션으로 래핑할 수 있는 유일한 방법은 [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) 를 사용 하거나 ObjectContext를 사용 하는 것입니다 **. 연결** 속성 및 시작 반환 된 **Entityconnection** 개체에서 **Open ()** 및 **BeginTransaction ()** 를 직접 호출 합니다. 또한 원본 데이터베이스 연결에서 트랜잭션을 시작한 경우에는 데이터베이스에 연결 된 API 호출이 실패 합니다.  

> [!NOTE]
> 닫힌 연결 허용의 제한은 Entity Framework 6에서 제거 되었습니다. 자세한 내용은 [연결 관리](~/ef6/fundamentals/connection-management.md)를 참조 하세요.  

EF6부터 프레임 워크는 이제 다음을 제공 합니다.  

1. **Database.BeginTransaction()** : 사용자가 기존 DbContext 내에서 트랜잭션을 직접 시작 하 고 완료 하는 것이 더 쉬운 방법입니다. 이렇게 하면 여러 작업을 동일한 트랜잭션 내에서 결합할 수 있으므로 모두 커밋되거나 모두 롤백됩니다. 또한 사용자가 트랜잭션의 격리 수준을 더 쉽게 지정할 수 있습니다.  
2. **UseTransaction ()** : DbContext가 Entity Framework 외부에서 시작 된 트랜잭션을 사용할 수 있도록 합니다.  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a>동일한 컨텍스트 내에서 여러 작업을 하나의 트랜잭션으로 결합  

**BeginTransaction ()** 에는 두 가지 재정의가 있습니다. 하나는 명시적 [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) 를 사용 하 고 다른 하나는 인수를 사용 하지 않고 기본 데이터베이스 공급자의 기본 IsolationLevel를 사용 합니다. 두 재정의 모두 기본 저장소 트랜잭션에서 커밋 및 롤백을 수행 하는 **commit ()** 및 **rollback ()** 메서드를 제공 하는 **dbcontexttransaction** 개체를 반환 합니다.  

**Dbcontexttransaction** 은 커밋되거나 롤백된 후 삭제 됩니다. 이를 수행 하는 한 가지 쉬운 방법은 **using (...)을 사용 하는 것입니다. {...}** using 블록이 완료 되 면 **Dispose ()** 를 자동으로 호출 하는 구문입니다.  

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
> 트랜잭션을 시작 하려면 기본 저장소 연결이 열려 있어야 합니다. 따라서 BeginTransaction ()를 호출 하면 연결이 아직 열려 있지 않은 경우 연결이 열립니다. DbContextTransaction이 연결을 연 경우 Dispose ()가 호출 될 때이를 닫습니다.  

### <a name="passing-an-existing-transaction-to-the-context"></a>기존 트랜잭션을 컨텍스트에 전달  

때로는 범위에서 훨씬 더 광범위 하 고, 동일한 데이터베이스에 대 한 작업을 포함 하 고 EF 외부에서 작업을 포함 하는 트랜잭션을 원할 수 있습니다. 이를 수행 하려면 연결을 열고 트랜잭션을 직접 시작한 다음 EF a에 게 이미 열려 있는 데이터베이스 연결을 사용 하 고 b)를 사용 하 여 해당 연결에서 기존 트랜잭션을 사용 해야 합니다.  

이렇게 하려면 기존 연결 매개 변수를 사용 하는 DbContext 생성자 중 하나에서 상속 되는 컨텍스트 클래스의 생성자와 contextOwnsConnection 부울로 생성자를 정의 하 고 사용 해야 합니다.  

> [!NOTE]
> 이 시나리오에서 호출 하는 경우 contextOwnsConnection 플래그를 false로 설정 해야 합니다. 이는 작업이 완료 될 때 연결을 닫지 않아야 함을 Entity Framework에 알리기 때문에 중요 합니다. 예를 들어 아래 줄 4를 참조 하십시오.  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

또한 IsolationLevel를 사용 하 여 (기본 설정을 방지 하려는 경우) 트랜잭션을 직접 시작 하 고 연결에서 이미 시작 된 기존 트랜잭션이 있음을 Entity Framework 합니다 (아래의 33 줄 참조).  

그런 다음 SqlConnection 자체 나 DbContext에서 직접 데이터베이스 작업을 실행할 수 있습니다. 이러한 모든 작업은 하나의 트랜잭션 내에서 실행 됩니다. 트랜잭션을 커밋하거나 롤백하고 해당 데이터베이스에서 삭제 ()를 호출 하는 일은 물론 데이터베이스 연결을 닫고 삭제 하기 위한 책임이 있습니다. 예를 들어:  

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

### <a name="clearing-up-the-transaction"></a>트랜잭션을 지우는 중

Null을 UseTransaction ()에 전달 하 여 현재 트랜잭션에 대 한 Entity Framework 정보를 지울 수 있습니다. 이 작업을 수행 하는 경우 기존 트랜잭션을 커밋하거나 롤백하지 않기 때문에이 작업을 수행 하려는 경우에만 주의 해 서 사용 하는 것이 좋습니다. Entity Framework  

### <a name="errors-in-usetransaction"></a>UseTransaction의 오류

다음 경우에 트랜잭션을 전달 하는 경우 UseTransaction ()에서 예외가 표시 됩니다.  
- 기존 트랜잭션이 이미 있는 Entity Framework  
- Entity Framework은 (는) TransactionScope 내에서 이미 작동 중입니다.  
- 전달 된 트랜잭션의 연결 개체가 null입니다. 즉, 트랜잭션이 연결과 연결 되지 않습니다. 일반적으로 트랜잭션이 이미 완료 된 부호입니다.  
- 전달 된 트랜잭션의 연결 개체가 Entity Framework의 연결과 일치 하지 않습니다.  

## <a name="using-transactions-with-other-features"></a>다른 기능과 함께 트랜잭션 사용  

이 섹션에서는 위의 트랜잭션이와 상호 작용 하는 방법에 대해 자세히 설명 합니다.  

- 연결 복원력  
- 비동기 메서드  
- TransactionScope 트랜잭션  

### <a name="connection-resiliency"></a>연결 복원력  

새 연결 복원 력 기능은 사용자가 시작한 트랜잭션에서 작동 하지 않습니다. 자세한 내용은 [실행 다시 시도 전략](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported)을 참조 하세요.  

### <a name="asynchronous-programming"></a>비동기 프로그래밍  

이전 섹션에 설명 된 방법에는 [비동기 쿼리 및 저장 메서드](~/ef6/fundamentals/async.md
)를 사용할 수 있는 추가 옵션이 나 설정이 필요 하지 않습니다. 그러나 비동기 메서드 내에서 수행 하는 작업에 따라이로 인해 전체 응용 프로그램의 성능에 좋지 않은 교착 상태나 차단이 발생할 수 있습니다.  

### <a name="transactionscope-transactions"></a>TransactionScope 트랜잭션  

EF6 이전에는 더 큰 범위 트랜잭션을 제공 하는 권장 방법을 TransactionScope 개체를 사용 했습니다.  

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

SqlConnection 및 Entity Framework는 모두 앰비언트 TransactionScope 트랜잭션을 사용 하므로 함께 커밋됩니다.  

.NET 4.5.1 TransactionScope부터 [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) 열거를 사용 하 여 비동기 메서드를 사용 하도록 업데이트 되었습니다.  

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

TransactionScope 접근 방식에는 여전히 몇 가지 제한 사항이 있습니다.  

- 비동기 메서드를 사용 하려면 .NET 4.5.1 이상이 필요 합니다.  
- 단 하나의 연결만 있고 (클라우드 시나리오는 분산 트랜잭션을 지원 하지 않는 경우) 클라우드 시나리오에서 사용할 수 없습니다.  
- 이전 섹션의 UseTransaction () 방법과 함께 사용할 수 없습니다.  
- DDL을 실행 하 고 MSDTC 서비스를 통해 분산 트랜잭션을 사용 하도록 설정 하지 않은 경우 예외를 throw 합니다.  

TransactionScope 방법의 장점은 다음과 같습니다.  

- 지정 된 데이터베이스에 두 개 이상의 연결을 설정 하거나 동일한 트랜잭션 내의 다른 데이터베이스에 대 한 연결을 사용 하 여 하나의 데이터베이스에 연결을 결합 하는 경우 로컬 트랜잭션이 분산 트랜잭션으로 자동 업그레이드 됩니다 (참고: 다음이 있어야 합니다. 이 작업에 대 한 분산 트랜잭션을 허용 하도록 MSDTC 서비스를 구성 했습니다.  
- 코딩의 용이성 트랜잭션을 앰비언트로 처리 하 고 명시적으로 제어 하는 대신 백그라운드에서 암시적으로 처리 하는 것을 선호 하는 경우에는 TransactionScope 방법이 더 적합 합니다.  

요약 하자면, 위의 새 BeginTransaction () 및 UseTransaction () Api를 사용 하면 대부분의 사용자에 게 더 이상 TransactionScope 방법이 필요 하지 않습니다. 계속 해 서 TransactionScope를 사용 하는 경우 위의 제한 사항을 알고 있어야 합니다. 가능한 경우 이전 섹션에 설명 된 방법을 사용 하는 것이 좋습니다.  
