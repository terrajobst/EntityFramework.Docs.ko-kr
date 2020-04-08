---
title: 트랜잭션 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 390d89398ebfdf015804749e71ff0b61d3f278d3
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413620"
---
# <a name="using-transactions"></a>트랜잭션 사용

트랜잭션을 사용하면 여러 데이터베이스 작업을 원자성 방식으로 처리할 수 있습니다. 트랜잭션이 커밋되면 모든 작업이 성공적으로 데이터베이스에 적용됩니다. 트랜잭션이 롤백되면 데이터베이스에 아무 작업도 적용되지 않습니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/)을 볼 수 있습니다.

## <a name="default-transaction-behavior"></a>기본 트랜잭션 동작

기본적으로 데이터베이스 공급자가 트랜잭션을 지원하는 경우 `SaveChanges()`에 대한 단일 호출의 모든 변경 내용이 트랜잭션에 적용됩니다. 변경이 실패하면 트랜잭션이 롤백되고 변경 내용이 데이터베이스에 적용되지 않습니다. 즉, `SaveChanges()`이 완전히 성공하도록 보장되거나 오류가 발생하는 경우 데이터베이스가 수정되지 않은 상태로 유지됩니다.

대부분의 애플리케이션에서는 이 기본 동작이면 충분합니다. 애플리케이션 요구 사항에서 필요하다고 생각되는 경우에만 트랜잭션을 수동으로 제어해야 합니다.

## <a name="controlling-transactions"></a>트랜잭션 제어

`DbContext.Database` API를 사용하여 트랜잭션을 시작하고 커밋하고 롤백할 수 있습니다. 다음 예제에서는 단일 트랜잭션에서 실행되는 두 개의 `SaveChanges()` 작업과 LINQ 쿼리를 보여줍니다.

일부 데이터베이스 공급자는 트랜잭션을 지원하지 않습니다. 일부 공급자는 트랜잭션 API가 호출될 때 throw되거나 작동하지 않을 수 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a>컨텍스트 간 트랜잭션(관계형 데이터베이스만 해당)

여러 컨텍스트 인스턴스 간에 트랜잭션을 공유할 수도 있습니다. 이 기능은 관계형 데이터베이스와 관련된 `DbTransaction` 및 `DbConnection`을 사용해야 하므로 관계형 데이터베이스 공급자를 사용하는 경우에만 사용할 수 있습니다.

트랜잭션을 공유하려면 컨텍스트가 `DbConnection`과 `DbTransaction`을 모두 공유해야 합니다.

### <a name="allow-connection-to-be-externally-provided"></a>연결을 외부에서 제공하도록 허용

`DbConnection`을 공유하려면 이를 구성할 때 컨텍스트로 연결을 전달하는 기능이 필요합니다.

`DbConnection`을 외부에서 제공하도록 하는 가장 쉬운 방법은 `DbContext.OnConfiguring` 메서드를 사용하여 컨텍스트를 구성하고 `DbContextOptions`를 외부에서 만들어 컨텍스트 생성자에 전달하는 것을 중지하는 것입니다.

> [!TIP]  
> `DbContextOptionsBuilder`는 `DbContext.OnConfiguring`에서 컨텍스트를 구성하는 데 사용되는 API이며, 이제 외부에서 이 API를 사용하여 `DbContextOptions`를 만듭니다.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

또는 `DbContext.OnConfiguring`을 계속 사용하지만 `DbConnection`에서 저장된 후 사용되는 `DbContext.OnConfiguring`을 허용합니다.

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

### <a name="share-connection-and-transaction"></a>연결 및 트랜잭션 공유

이제 동일한 연결을 공유하는 여러 컨텍스트 인스턴스를 작성할 수 있습니다. 그런 다음, `DbContext.Database.UseTransaction(DbTransaction)` API를 사용하여 두 컨텍스트를 동일한 트랜잭션에 등록합니다.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>외부 DbTransactions 사용(관계형 데이터베이스만 해당)

여러 데이터 액세스 기술을 사용하여 관계형 데이터베이스에 액세스하는 경우 이러한 서로 다른 기술에서 수행하는 작업 간에 트랜잭션을 공유할 수 있습니다.

다음 예제에서는 동일한 트랜잭션에서 ADO.NET SqlClient 작업 및 Entity Framework Core 작업을 수행하는 방법을 보여줍니다.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>System.Transactions 사용

> [!NOTE]  
> 이 기능은 EF Core 2.1의 새로운 기능입니다.

더 큰 범위를 조정해야 하는 경우 앰비언트 트랜잭션을 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

또한 명시적 트랜잭션에 등록할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a>System.Transactions의 제한 사항  

1. EF Core는 데이터베이스 공급자를 사용하여 System.Transactions에 대한 지원을 구현합니다. .NET Framework용 ADO.NET 공급자에서는 지원이 매우 일반적이지만 .NET Core에는 API가 최근에 추가되었으므로 지원이 광범위하지 않습니다. 공급자가 System.Transactions에 대한 지원을 구현하지 않는 경우 이러한 API에 대한 호출을 완전히 무시해도 됩니다. .NET Core용 SqlClient는 2.1부터 지원을 구현합니다. .NET Core용 SqlClient 2.0에서 이 기능을 사용하려고 하면 예외가 throw됩니다.

   > [!IMPORTANT]  
   > 트랜잭션을 관리하는 데 사용하기 전에 API가 공급자에서 올바르게 동작하는지 테스트하는 것이 좋습니다. 그렇지 않으면 데이터베이스 공급자의 유지 관리자에게 문의하는 것이 좋습니다.

2. 버전 2.1부터 .NET Core의 System.Transactions 구현에 분산 트랜잭션에 대한 지원이 포함되지 않으므로 `TransactionScope`이나 `CommittableTransaction`을 사용하여 여러 리소스 관리자 간에 트랜잭션을 조정할 수 없습니다.
