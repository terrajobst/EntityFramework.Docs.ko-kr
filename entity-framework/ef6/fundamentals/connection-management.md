---
title: 연결 관리-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
caps.latest.revision: 3
ms.openlocfilehash: 9588ef85435d3c0218defdc098f1e7150fb7ef72
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122763"
---
# <a name="connection-management"></a>연결 관리
이 페이지는 컨텍스트 및의 기능에 연결을 전달 하는 관련 하 여 Entity Framework의 동작을 설명 합니다 **Database.Connection.Open()** API.  

## <a name="passing-connections-to-the-context"></a>컨텍스트에 전달 연결  

### <a name="behavior-for-ef5-and-earlier-versions"></a>EF5 및 이전 버전에 대 한 동작  

연결을 허용 하는 생성자 두 가지가 있습니다.  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

이 방법을 사용 하는 것이 가능 하지만 몇 가지 제한 사항 해결 해야:  

1. 그런 다음 처음 프레임 워크를 InvalidOperationException 예외가 사용 하려고 합니다. 이러한 열려 있는 연결을 전달 하면 말하고 다시 열 수 없습니다 이미 열려 있는 연결 합니다.  
2. ContextOwnsConnection 플래그 컨텍스트가 삭제 될 때 기본 저장소 연결을 삭제 해야 하는지 여부를 의미로 해석 됩니다. 그러나 해당 설정에 관계 없이 저장소 연결이 항상 닫힌 컨텍스트가 삭제 될 때. 동일한 연결을 사용 하 여 DbContext를 둘 이상 있는 경우 어떤 컨텍스트가 먼저 연결을 종료 합니다 (마찬가지로 DbContext 사용 하 여 기존 ADO.NET 연결을 혼합 한 경우 DbContext 항상 연결이 닫힙니다 삭제 되 면) 하므로 .  

닫힌된 연결을 전달 하 고만 열리는 것 모든 컨텍스트에서 생성 된 후 코드를 실행 하 여 위의 첫 번째 제한 사항을 해결 하는 것이 가능 합니다.  

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

두 번째 제한은 의미에서 닫을 수에 대 한 연결에 대 한 준비가 될 때까지 DbContext 개체를 삭제 하지 않도록 해야 합니다.  

### <a name="behavior-in-ef6-and-future-versions"></a>EF6 및 이후 버전의 동작  

EF6 및 이후 버전에서 DbContext 동일한 두 명의 생성자가 있지만 수신 될 때 생성자에 전달 된 연결을 닫을 수 있음을 더 이상 필요 합니다. 따라서이 수 있게 되었습니다.  

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

ContextOwnsConnection 플래그를 지금 제어 여부는 연결이 모두 닫히고 DbContext가 삭제 될 때 삭제 합니다. 위의 예제에서 연결이 닫혀 있지 않으면 컨텍스트가 경우 하므로 이전 버전의 EF 되었을 있지만 자체 연결이 삭제 될 때가 아니라 (줄 32)를 삭제 (40 줄).  

물론 것도 가능 (방금 true 또는 다른 생성자 중 하나를 사용 하려면 집합 contextOwnsConnection) 연결을 제어할 수 dbcontext 따라서 하려는 경우.  

> [!NOTE]
> 이 새 모델을 사용 하 여 트랜잭션을 사용할 때는 몇 가지 추가 고려 사항이 있습니다. 자세한 내용은 참조 하십시오 [트랜잭션과 작업](~/ef6/saving/transactions.md)합니다.  

## <a name="databaseconnectionopen"></a>Database.Connection.Open()  

### <a name="behavior-for-ef5-and-earlier-versions"></a>EF5 및 이전 버전에 대 한 동작  

EF5 및 이전 버전에는 버그는 합니다 **ObjectContext.Connection.State** 기본 저장소 연결의 실제 상태를 반영 하도록 업데이트 되지 않았습니다. 예를 들어, 다음 코드를 실행 하는 경우 있습니다 수 반환할 상태 **닫힘** 기본 연결을 저장 하는 사실 없지만입니다 **오픈**합니다.  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

별도로 Database.Connection.Open()를 호출 하 여 데이터베이스 연결을 열면 됩니다 열린 다음에 하면 쿼리를 실행 하거나 데이터베이스에 연결 해야 하는 호출 될 때까지 (예를 들어 SaveChanges()) 뒤는 기본 저장소 연결을 닫습니다. 컨텍스트를 다시 열리고 다시 다른 데이터베이스 작업은 필요 하 든 지 연결을 닫습니다.  

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

EF6 및 이후 버전에 대 한 이동 방법은 호출 코드에서 호출 컨텍스트와 연결 하 여 하기로 선택한 경우. Database.Connection.Open() 고 이렇게 하는 것에 대 한 이유가 있으며 열기 및 연결의 닫기에 대 한 제어 하려고 하면 연결이 자동으로 닫힙니다 더 이상를 프레임 워크를 가정 합니다.  

> [!NOTE]
> 오랫동안 신중 하 게 되므로 사용에 대 한 열려 있는 연결을 일으킬 수 있습니다.  

코드를 ObjectContext.Connection.State 이제는 추적 기본 연결 상태를 올바르게 있도록도 업데이트 했습니다.  

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
