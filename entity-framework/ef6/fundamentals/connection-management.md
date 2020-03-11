---
title: 연결 관리-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
ms.openlocfilehash: a6352bbbc38c38bd5f30536736ec969056df2c7d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414868"
---
# <a name="connection-management"></a>연결 관리
이 페이지에서는 컨텍스트와 데이터베이스의 기능에 연결을 전달 하는 것과 관련 된 Entity Framework 동작에 대해 설명 합니다. **Open ()** API.  

## <a name="passing-connections-to-the-context"></a>컨텍스트에 연결 전달  

### <a name="behavior-for-ef5-and-earlier-versions"></a>EF5 이전 버전에 대 한 동작  

연결을 허용 하는 두 개의 생성자가 있습니다.  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

이러한 작업은 사용할 수 있지만 몇 가지 제한 사항을 해결 해야 합니다.  

1. 이러한 두 개체 중 하나에 대 한 열린 연결을 전달 하는 경우 프레임 워크에서 처음으로 사용 하려고 하면 이미 열려 있는 연결을 다시 열 수 없다는 InvalidOperationException이 throw 됩니다.  
2. ContextOwnsConnection 플래그는 컨텍스트가 삭제 될 때 기본 저장소 연결을 삭제 해야 하는지 여부를 의미 하는 것으로 해석 됩니다. 그러나이 설정에 관계 없이 컨텍스트가 삭제 되 면 항상 저장소 연결이 닫힙니다. 따라서 동일한 연결을 사용 하는 DbContext이 두 개 이상 있는 경우 어떤 컨텍스트가 먼저 삭제 되는지는 먼저 연결을 닫습니다. 예를 들어 DbContext를 사용 하 여 기존 ADO.NET 연결을 혼합 한 경우 DbContext는 항상 연결이 삭제 될 때 해당 연결을 닫습니다. .  

닫힌 연결을 전달 하 고, 모든 컨텍스트를 만든 후에 해당 연결을 여는 코드를 실행 하는 경우 위의 첫 번째 제한 사항을 해결할 수 있습니다.  

``` csharp
using System.Collections.Generic;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Linq;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExampleEF5
    {         
        public static void TwoDbContextsOneConnection()
        {
            using (var context1 = new BloggingContext())
            {
                var conn =
                    ((EntityConnection)  
                        ((IObjectContextAdapter)context1).ObjectContext.Connection)  
                            .StoreConnection;

                using (var context2 = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    context2.Database.ExecuteSqlCommand(
                        @"UPDATE Blogs SET Rating = 5" +
                        " WHERE Name LIKE '%Entity Framework%'");

                    var query = context1.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context1.SaveChanges();
                }
            }
        }
    }
}
```  

두 번째 제한은 연결을 닫을 준비가 될 때까지 DbContext 개체를 삭제 하는 것을 방지 해야 한다는 것을 의미 합니다.  

### <a name="behavior-in-ef6-and-future-versions"></a>EF6 및 이후 버전의 동작  

EF6 및 이후 버전에서 DbContext는 동일한 두 개의 생성자를 포함 하지만 수신 될 때 생성자에 전달 된 연결을 닫을 필요가 없습니다. 이제 다음과 같은 작업을 수행할 수 있습니다.  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExample
    {
        public static void PassingAnOpenConnection()
        {
            using (var conn = new SqlConnection("{connectionString}"))
            {
                conn.Open();

                var sqlCommand = new SqlCommand();
                sqlCommand.Connection = conn;
                sqlCommand.CommandText =
                    @"UPDATE Blogs SET Rating = 5" +
                     " WHERE Name LIKE '%Entity Framework%'";
                sqlCommand.ExecuteNonQuery();

                using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    var query = context.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context.SaveChanges();
                }

                var sqlCommand2 = new SqlCommand();
                sqlCommand2.Connection = conn;
                sqlCommand2.CommandText =
                    @"UPDATE Blogs SET Rating = 7" +
                     " WHERE Name LIKE '%Entity Framework Rocks%'";
                sqlCommand2.ExecuteNonQuery();
            }
        }
    }
}
```  

또한 contextOwnsConnection 플래그는 이제 DbContext 삭제 될 때 연결이 닫히고 삭제 되는지 여부를 제어 합니다. 따라서 위의 예에서는 컨텍스트가 삭제 될 때 (32 줄) 연결이 닫혀 있지 않습니다. 즉, 이전 버전의 EF에서와 같이 연결 자체가 삭제 된 경우 (40 줄)에는 연결이 닫혀 있지 않습니다.  

물론 원하는 경우에도 DbContext가 연결을 제어할 수 있습니다 (contextOwnsConnection를 true로 설정 하거나 다른 생성자 중 하나를 사용).  

> [!NOTE]
> 이 새 모델에서 트랜잭션을 사용할 때 몇 가지 사항을 추가로 고려해 야 합니다. 자세한 내용은 [트랜잭션 작업](~/ef6/saving/transactions.md)을 참조 하세요.  

## <a name="databaseconnectionopen"></a>Database. Connection. Open ()  

### <a name="behavior-for-ef5-and-earlier-versions"></a>EF5 이전 버전에 대 한 동작  

EF5 이전 버전에서는 기본 저장소 연결의 실제 상태를 반영 하도록 **ObjectContext** 를 업데이트 하지 않은 버그가 있습니다. 예를 들어 다음 코드를 실행 하는 경우 기본 저장소 연결이 **열려**있더라도 상태를 **닫을** 수 있습니다.  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

데이터베이스 연결을 호출 하 여 데이터베이스 연결을 여는 경우에는 다음에 쿼리를 실행 하거나 데이터베이스 연결을 필요로 하는 모든 항목 (예: SaveChanges ())을 호출 하는 경우를 제외 하 고 기본 저장소 연결을 닫습니다. 그러면 컨텍스트는 다른 데이터베이스 작업이 필요한 경우 언제 든 지 다시 열고 다시 연결을 닫습니다.  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;

namespace ConnectionManagementExamples
{
    public class DatabaseOpenConnectionBehaviorEF5
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open  
                // (though ObjectContext.Connection.State will report closed)

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);

                // The underlying store connection is still open  

                context.SaveChanges();

                // After SaveChanges() the underlying store connection is closed  
                // Each SaveChanges() / query etc now opens and immediately closes
                // the underlying store connection

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();
            }
        }
    }
}
```  

### <a name="behavior-in-ef6-and-future-versions"></a>EF6 및 이후 버전의 동작  

EF6 및 이후 버전의 경우 호출 하는 코드가 컨텍스트를 호출 하 여 연결을 열도록 선택 하는 방식을 수행 했습니다. Database. Connection. Open ()을 수행 하는 것이 좋습니다 .이 경우 프레임 워크는 연결 열기 및 닫기를 제어 하 고 더 이상 연결을 자동으로 닫지 않는 것으로 가정 합니다.  

> [!NOTE]
> 이로 인해 오랜 시간 동안 열려 있는 연결을 사용 하 여 신중 하 게 사용할 수 있습니다.  

또한 코드를 업데이트 하 여 이제 ObjectContext. Connection. State가 기본 연결의 상태를 올바르게 추적 합니다.  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Core.EntityClient;
using System.Data.Entity.Infrastructure;

namespace ConnectionManagementExamples
{
    internal class DatabaseOpenConnectionBehaviorEF6
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open and the
                // ObjectContext.Connection.State correctly reports open too

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection remains open for the next operation  

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection is still open

           } // The context is disposed – so now the underlying store connection is closed
        }
    }
}
```  
