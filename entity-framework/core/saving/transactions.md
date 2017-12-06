---
title: "트랜잭션-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: a2f890c0af1e83cbcc1d40d68540ff7132a9bafd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="using-transactions"></a>트랜잭션 사용

트랜잭션을 사용 여러 데이터베이스 작업을 원자성 방식으로 처리 될 수 있습니다. 트랜잭션이 커밋된 경우에 데이터베이스에 성공적으로 적용 됩니다 모든 작업. 트랜잭션이 롤백되면 데이터베이스에 적용 됩니다는 작업은 없습니다.

> [!TIP]  
> 이 문서를 볼 수 있습니다 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) GitHub에서 합니다.

## <a name="default-transaction-behavior"></a>기본 트랜잭션 동작

기본적으로 데이터베이스 공급자에는 트랜잭션을 지원 하는 경우 모든 변경 내용에 대 한 단일 호출에서 `SaveChanges()` 트랜잭션에 적용 됩니다. 변경 내용 중 하나라도 실패할 경우 트랜잭션이 롤백 및 데이터베이스에 적용 된 변경 하는 것입니다. 즉 `SaveChanges()` 하거나 완전히 성공 하거나 데이터베이스 오류가 발생 하면 수정 되지 않은 상태로 유지 되도록 합니다.

대부분의 응용 프로그램에 대 한이 기본 동작으로 충분 합니다. 응용 프로그램 요구 사항을 하다 고 필요한 경우에 수동으로 트랜잭션을 제어 해야 합니다.

## <a name="controlling-transactions"></a>트랜잭션 제어

사용할 수는 `DbContext.Database` 시작, 커밋, API 및 rollback 트랜잭션. 다음 예제에서는 두 개의 `SaveChanges()` 단일 트랜잭션 내에서 실행 중인 작업 및 LINQ 쿼리 합니다.

모든 데이터베이스 공급자에는 트랜잭션을 지원 합니다. 일부 공급자 throw 될 수 있습니다 또는 아무 트랜잭션 Api 호출 됩니다.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?highlight=3,17,18,19)] -->
``` csharp
        using (var context = new BloggingContext())
        {
            using (var transaction = context.Database.BeginTransaction())
            {
                try
                {
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();

                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/visualstudio" });
                    context.SaveChanges();

                    var blogs = context.Blogs
                        .OrderBy(b => b.Url)
                        .ToList();

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="cross-context-transaction-relational-databases-only"></a>컨텍스트 간 트랜잭션 (관계형 데이터베이스에만 해당)

또한 여러 컨텍스트 인스턴스에서 트랜잭션을 공유할 수 있습니다. 사용 하기 때문에 관계형 데이터베이스 공급자를 사용 하는 경우에이 기능을 사용할 수만 `DbTransaction` 및 `DbConnection`, 관계형 데이터베이스와 다릅니다.

트랜잭션을 공유 하는 컨텍스트를 공유 해야 둘 다는 `DbConnection` 및 `DbTransaction`합니다.

### <a name="allow-connection-to-be-externally-provided"></a>외부에서 제공 되는 연결 허용

공유는 `DbConnection` 것을 생성할 때는 컨텍스트에 대 한 연결을 전달 하는 기능이 필요 합니다.

허용 하는 가장 쉬운 방법은 `DbConnection` 사용을 중지 하는 외부에서 제공 되는 `DbContext.OnConfiguring` 메서드 컨텍스트를 구성 하 고 외부에서 만드는 `DbContextOptions` 컨텍스트 생성자에 전달 합니다.

> [!TIP]  
> `DbContextOptionsBuilder`사용 하는 API가 `DbContext.OnConfiguring` 컨텍스트를 구성 하려면 이제 하려는 외부에서 만드는 데 사용할 `DbContextOptions`합니다.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=3,4,5)] -->
``` csharp
    public class BloggingContext : DbContext
    {
        public BloggingContext(DbContextOptions<BloggingContext> options)
            : base(options)
        { }

        public DbSet<Blog> Blogs { get; set; }
    }
```

계속 사용 하는 대체 항목은 `DbContext.OnConfiguring`, 하지만 허용는 `DbConnection` 저장 되 고 다음에 사용 하는 `DbContext.OnConfiguring`합니다.

``` csharp
public class BloggingContext : DbContext
{
    private DbConnection _connection;

    public BloggingContext(DbConnection connection)
    {
      _connection = connection;
    }

    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(_connection);
    }
}
```

### <a name="share-connection-and-transaction"></a>공유 연결 및 트랜잭션

이제 같은 연결을 공유 하는 여러 상황에 맞는 인스턴스를 만들 수 있습니다. 다음 사용 하 여는 `DbContext.Database.UseTransaction(DbTransaction)` API 두 컨텍스트가 모두 동일한 트랜잭션에 참여 합니다.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=1,2,3,7,16,23,24,25)] -->
``` csharp
        var options = new DbContextOptionsBuilder<BloggingContext>()
            .UseSqlServer(new SqlConnection(connectionString))
            .Options;

        using (var context1 = new BloggingContext(options))
        {
            using (var transaction = context1.Database.BeginTransaction())
            {
                try
                {
                    context1.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context1.SaveChanges();

                    using (var context2 = new BloggingContext(options))
                    {
                        context2.Database.UseTransaction(transaction.GetDbTransaction());

                        var blogs = context2.Blogs
                            .OrderBy(b => b.Url)
                            .ToList();
                    }

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="using-external-dbtransactions-relational-databases-only"></a>외부 DbTransactions (관계형 데이터베이스에만 해당)를 사용 하 여

여러 데이터 액세스 기술은 관계형 데이터베이스에 액세스를 사용 하는 경우에 이러한 다른 기술에 의해 수행 되는 작업 간에 트랜잭션이 공유 하는 것이 좋습니다.

다음 예제에서는 동일한 트랜잭션 내에서 ADO.NET SqlClient 작업 및 Entity Framework Core 작업을 수행 하는 방법을 보여 줍니다.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?highlight=4,10,21,26,27,28)] -->
``` csharp
        var connection = new SqlConnection(connectionString);
        connection.Open();

        using (var transaction = connection.BeginTransaction())
        {
            try
            {
                // Run raw ADO.NET command in the transaction
                var command = connection.CreateCommand();
                command.Transaction = transaction;
                command.CommandText = "DELETE FROM dbo.Blogs";
                command.ExecuteNonQuery();

                // Run an EF Core command in the transaction
                var options = new DbContextOptionsBuilder<BloggingContext>()
                    .UseSqlServer(connection)
                    .Options;

                using (var context = new BloggingContext(options))
                {
                    context.Database.UseTransaction(transaction);
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();
                }

                // Commit transaction if all commands succeed, transaction will auto-rollback
                // when disposed if either commands fails
                transaction.Commit();
            }
            catch (System.Exception)
            {
                // TODO: Handle failure
            }
```
