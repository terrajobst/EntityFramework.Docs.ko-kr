---
title: 트랜잭션 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: 6e6ded74e15187b387e8e0b2ad00cb47a84ff7e8
ms.sourcegitcommit: 6cf6493d81b6d81b0b0f37a00e0fc23ec7189158
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/12/2018
ms.locfileid: "42447656"
---
# <a name="using-transactions"></a><span data-ttu-id="61069-102">트랜잭션 사용</span><span class="sxs-lookup"><span data-stu-id="61069-102">Using Transactions</span></span>

<span data-ttu-id="61069-103">트랜잭션을 사용하면 여러 데이터베이스 작업을 원자성 방식으로 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="61069-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="61069-104">트랜잭션이 커밋되면 모든 작업이 성공적으로 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="61069-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="61069-105">트랜잭션이 롤백되면 데이터베이스에 아무 작업도 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="61069-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="61069-106">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="61069-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="61069-107">기본 트랜잭션 동작</span><span class="sxs-lookup"><span data-stu-id="61069-107">Default transaction behavior</span></span>

<span data-ttu-id="61069-108">기본적으로 데이터베이스 공급자가 트랜잭션을 지원하는 경우 `SaveChanges()`에 대한 단일 호출의 모든 변경 내용이 트랜잭션에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="61069-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="61069-109">변경이 실패하면 트랜잭션이 롤백되고 변경 내용이 데이터베이스에 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="61069-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="61069-110">즉, `SaveChanges()`이 완전히 성공하도록 보장되거나 오류가 발생하는 경우 데이터베이스가 수정되지 않은 상태로 유지됩니다.</span><span class="sxs-lookup"><span data-stu-id="61069-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="61069-111">대부분의 응용 프로그램에서는 이 기본 동작이면 충분합니다.</span><span class="sxs-lookup"><span data-stu-id="61069-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="61069-112">응용 프로그램 요구 사항에서 필요하다고 생각되는 경우에만 트랜잭션을 수동으로 제어해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="61069-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="61069-113">트랜잭션 제어</span><span class="sxs-lookup"><span data-stu-id="61069-113">Controlling transactions</span></span>

<span data-ttu-id="61069-114">`DbContext.Database` API를 사용하여 트랜잭션을 시작하고 커밋하고 롤백할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="61069-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="61069-115">다음 예제에서는 단일 트랜잭션에서 실행되는 두 개의 `SaveChanges()` 작업과 LINQ 쿼리를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="61069-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="61069-116">일부 데이터베이스 공급자는 트랜잭션을 지원하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="61069-116">Not all database providers support transactions.</span></span> <span data-ttu-id="61069-117">일부 공급자는 트랜잭션 API가 호출될 때 throw되거나 작동하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="61069-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="61069-118">컨텍스트 간 트랜잭션(관계형 데이터베이스만 해당)</span><span class="sxs-lookup"><span data-stu-id="61069-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="61069-119">여러 컨텍스트 인스턴스 간에 트랜잭션을 공유할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="61069-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="61069-120">이 기능은 관계형 데이터베이스와 관련된 `DbTransaction` 및 `DbConnection`을 사용해야 하므로 관계형 데이터베이스 공급자를 사용하는 경우에만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="61069-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="61069-121">트랜잭션을 공유하려면 컨텍스트가 `DbConnection`과 `DbTransaction`을 모두 공유해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="61069-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="61069-122">연결을 외부에서 제공하도록 허용</span><span class="sxs-lookup"><span data-stu-id="61069-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="61069-123">`DbConnection`을 공유하려면 이를 구성할 때 컨텍스트로 연결을 전달하는 기능이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="61069-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="61069-124">`DbConnection`을 외부에서 제공하도록 하는 가장 쉬운 방법은 `DbContext.OnConfiguring` 메서드를 사용하여 컨텍스트를 구성하고 `DbContextOptions`를 외부에서 만들어 컨텍스트 생성자에 전달하는 것을 중지하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="61069-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="61069-125">`DbContextOptionsBuilder`는 `DbContext.OnConfiguring`에서 컨텍스트를 구성하는 데 사용되는 API이며, 이제 외부에서 이 API를 사용하여 `DbContextOptions`를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="61069-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="61069-126">또는 `DbContext.OnConfiguring`을 계속 사용하지만 `DbContext.OnConfiguring`에서 저장된 후 사용되는 `DbConnection`을 허용합니다.</span><span class="sxs-lookup"><span data-stu-id="61069-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="61069-127">연결 및 트랜잭션 공유</span><span class="sxs-lookup"><span data-stu-id="61069-127">Share connection and transaction</span></span>

<span data-ttu-id="61069-128">이제 동일한 연결을 공유하는 여러 컨텍스트 인스턴스를 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="61069-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="61069-129">그런 다음, `DbContext.Database.UseTransaction(DbTransaction)` API를 사용하여 두 컨텍스트를 동일한 트랜잭션에 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="61069-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="61069-130">외부 DbTransactions 사용(관계형 데이터베이스만 해당)</span><span class="sxs-lookup"><span data-stu-id="61069-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="61069-131">여러 데이터 액세스 기술을 사용하여 관계형 데이터베이스에 액세스하는 경우 이러한 서로 다른 기술에서 수행하는 작업 간에 트랜잭션을 공유할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="61069-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="61069-132">다음 예제에서는 동일한 트랜잭션에서 ADO.NET SqlClient 작업 및 Entity Framework Core 작업을 수행하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="61069-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="61069-133">System.Transactions 사용</span><span class="sxs-lookup"><span data-stu-id="61069-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="61069-134">이 기능은 EF Core 2.1의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="61069-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="61069-135">더 큰 범위를 조정해야 하는 경우 앰비언트 트랜잭션을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="61069-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

<span data-ttu-id="61069-136">또한 명시적 트랜잭션에 등록할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="61069-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="61069-137">System.Transactions의 제한 사항</span><span class="sxs-lookup"><span data-stu-id="61069-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="61069-138">EF Core는 데이터베이스 공급자를 사용하여 System.Transactions에 대한 지원을 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="61069-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="61069-139">.NET Framework용 ADO.NET 공급자에서는 지원이 매우 일반적이지만 .NET Core에는 API가 최근에 추가되었으므로 지원이 광범위하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="61069-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not as widespread.</span></span> <span data-ttu-id="61069-140">공급자가 System.Transactions에 대한 지원을 구현하지 않는 경우 이러한 API에 대한 호출을 완전히 무시해도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="61069-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="61069-141">.NET Core용 SqlClient는 2.1부터 지원을 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="61069-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="61069-142">.NET Core용 SqlClient 2.0에서 이 기능을 사용하려고 하면 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="61069-142">SqlClient for .NET Core 2.0 will throw an exception of you attempt to use the feature.</span></span> 

   > [!IMPORTANT]  
   > <span data-ttu-id="61069-143">트랜잭션을 관리하는 데 사용하기 전에 API가 공급자에서 올바르게 동작하는지 테스트하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="61069-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="61069-144">그렇지 않으면 데이터베이스 공급자의 유지 관리자에게 문의하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="61069-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span> 

2. <span data-ttu-id="61069-145">버전 2.1부터 .NET Core의 System.Transactions 구현에 분산 트랜잭션에 대한 지원이 포함되지 않으므로 `TransactionScope`이나 `CommitableTransaction`을 사용하여 여러 리소스 관리자 간에 트랜잭션을 조정할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="61069-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommitableTransaction` to coordinate transactions across multiple resource managers.</span></span> 
