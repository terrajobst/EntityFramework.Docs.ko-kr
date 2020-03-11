---
title: DbContext 사용-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: d961ffd8bed7f5b2f82dcfa30fc0241b7437be50
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413884"
---
# <a name="working-with-dbcontext"></a><span data-ttu-id="ce747-102">DbContext 작업</span><span class="sxs-lookup"><span data-stu-id="ce747-102">Working with DbContext</span></span>

<span data-ttu-id="ce747-103">Entity Framework를 사용 하 여 .NET 개체를 사용 하 여 데이터를 쿼리, 삽입, 업데이트 및 삭제 하려면 먼저 모델에 정의 된 엔터티와 관계를 데이터베이스의 테이블에 매핑하는 [모델을 만들어야](~/ef6/modeling/index.md) 합니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-103">In order to use Entity Framework to query, insert, update, and delete data using .NET objects, you first need to [Create a Model](~/ef6/modeling/index.md) which maps the entities and relationships that are defined in your model to tables in a database.</span></span>

<span data-ttu-id="ce747-104">모델이 있으면 응용 프로그램이 상호 작용 하는 기본 클래스가 `System.Data.Entity.DbContext` 됩니다 (종종 컨텍스트 클래스 라고도 함).</span><span class="sxs-lookup"><span data-stu-id="ce747-104">Once you have a model, the primary class your application interacts with is `System.Data.Entity.DbContext` (often referred to as the context class).</span></span> <span data-ttu-id="ce747-105">모델에 연결 된 DbContext를 사용 하 여 다음을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-105">You can use a DbContext associated to a model to:</span></span>
- <span data-ttu-id="ce747-106">쿼리 작성 및 실행</span><span class="sxs-lookup"><span data-stu-id="ce747-106">Write and execute queries</span></span>   
- <span data-ttu-id="ce747-107">쿼리 결과를 엔터티 개체로 구체화</span><span class="sxs-lookup"><span data-stu-id="ce747-107">Materialize query results as entity objects</span></span>
- <span data-ttu-id="ce747-108">해당 개체에 대 한 변경 내용 추적</span><span class="sxs-lookup"><span data-stu-id="ce747-108">Track changes that are made to those objects</span></span>
- <span data-ttu-id="ce747-109">데이터베이스에 개체 변경 내용을 다시 유지</span><span class="sxs-lookup"><span data-stu-id="ce747-109">Persist object changes back on the database</span></span>
- <span data-ttu-id="ce747-110">메모리의 개체를 UI 컨트롤에 바인딩</span><span class="sxs-lookup"><span data-stu-id="ce747-110">Bind objects in memory to UI controls</span></span>

<span data-ttu-id="ce747-111">이 페이지는 컨텍스트 클래스를 관리 하는 방법에 대 한 몇 가지 지침을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-111">This page gives some guidance on how to manage the context class.</span></span>  

## <a name="defining-a-dbcontext-derived-class"></a><span data-ttu-id="ce747-112">DbContext 파생 클래스 정의</span><span class="sxs-lookup"><span data-stu-id="ce747-112">Defining a DbContext derived class</span></span>  

<span data-ttu-id="ce747-113">컨텍스트를 사용 하 여 작업 하는 권장 방법은 DbContext에서 파생 되는 클래스를 정의 하 고 컨텍스트에서 지정 된 엔터티의 컬렉션을 나타내는 DbSet 속성을 노출 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-113">The recommended way to work with context is to define a class that derives from DbContext and exposes DbSet properties that represent collections of the specified entities in the context.</span></span> <span data-ttu-id="ce747-114">EF Designer를 사용 하 여 작업 하는 경우 컨텍스트가 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-114">If you are working with the EF Designer, the context will be generated for you.</span></span> <span data-ttu-id="ce747-115">Code First 사용 하는 경우 일반적으로 컨텍스트를 직접 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-115">If you are working with Code First, you will typically write the context yourself.</span></span>  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

<span data-ttu-id="ce747-116">컨텍스트가 있으면 이러한 속성을 통해 컨텍스트에서 (`Add` 또는 `Attach` 메서드 사용)를 쿼리하거나 추가 (`Remove`사용) 하 여 엔터티를 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-116">Once you have a context, you would query for, add (using `Add` or `Attach` methods ) or remove (using `Remove`) entities in the context through these properties.</span></span> <span data-ttu-id="ce747-117">컨텍스트 개체의 `DbSet` 속성에 액세스 하는 것은 지정 된 형식의 모든 엔터티를 반환 하는 시작 쿼리를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-117">Accessing a `DbSet` property on a context object represent a starting query that returns all entities of the specified type.</span></span> <span data-ttu-id="ce747-118">속성에 액세스 하는 것은 쿼리를 실행 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-118">Note that just accessing a property will not execute the query.</span></span> <span data-ttu-id="ce747-119">쿼리가 실행 되는 경우는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-119">A query is executed when:</span></span>  

- <span data-ttu-id="ce747-120">`foreach`(C#) 또는 `For Each`(Visual Basic) 문에 의해 열거된 경우</span><span class="sxs-lookup"><span data-stu-id="ce747-120">It is enumerated by a `foreach` (C#) or `For Each` (Visual Basic) statement.</span></span>  
- <span data-ttu-id="ce747-121">`ToArray`, `ToDictionary`, `ToList`등의 컬렉션 작업에 의해 열거 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-121">It is enumerated by a collection operation such as `ToArray`, `ToDictionary`, or `ToList`.</span></span>  
- <span data-ttu-id="ce747-122">`First` 또는 `Any`와 같은 LINQ 연산자는 쿼리의 가장 바깥쪽 부분에 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-122">LINQ operators such as `First` or `Any` are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="ce747-123">지정 된 키를 가진 엔터티가 컨텍스트에 아직 로드 되지 않은 경우 `Load` 확장 메서드, `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`및 `DbSet<T>.Find`중 하나가 호출 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-123">One of the following methods are called: the `Load` extension method, `DbEntityEntry.Reload`,  `Database.ExecuteSqlCommand`, and `DbSet<T>.Find`, if an entity with the specified key is not found already loaded in the context.</span></span>  

## <a name="lifetime"></a><span data-ttu-id="ce747-124">수명</span><span class="sxs-lookup"><span data-stu-id="ce747-124">Lifetime</span></span>  

<span data-ttu-id="ce747-125">컨텍스트의 수명은 인스턴스가 생성 될 때 시작 되 고 인스턴스가 삭제 되거나 가비지 수집 될 때 종료 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-125">The lifetime of the context begins when the instance is created and ends when the instance is either disposed or garbage-collected.</span></span> <span data-ttu-id="ce747-126">컨텍스트 컨트롤에서 제어 하는 모든 리소스가 블록의 끝에서 삭제 되도록 하려면 **사용** 을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-126">Use **using** if you want all the resources that the context controls to be disposed at the end of the block.</span></span> <span data-ttu-id="ce747-127">**를 사용 하는**경우 컴파일러는 try/finally 블록을 자동으로 만들고 **finally** 블록에서 dispose를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-127">When you use **using**, the compiler automatically creates a try/finally block and calls dispose in the **finally** block.</span></span>  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

<span data-ttu-id="ce747-128">컨텍스트의 수명을 결정할 때 몇 가지 일반적인 지침은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-128">Here are some general guidelines when deciding on the lifetime of the context:</span></span>  

- <span data-ttu-id="ce747-129">웹 응용 프로그램을 사용할 때는 요청당 컨텍스트 인스턴스를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-129">When working with Web applications, use a context instance per request.</span></span>  
- <span data-ttu-id="ce747-130">Windows Presentation Foundation (WPF) 또는 Windows Forms를 사용 하 여 작업 하는 경우에는 폼 마다 컨텍스트 인스턴스를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-130">When working with Windows Presentation Foundation (WPF) or Windows Forms, use a context instance per form.</span></span> <span data-ttu-id="ce747-131">이렇게 하면 컨텍스트에서 제공 하는 변경 내용 추적 기능을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-131">This lets you use change-tracking functionality that context provides.</span></span>  
- <span data-ttu-id="ce747-132">컨텍스트 인스턴스가 종속성 주입 컨테이너에 의해 생성 되는 경우 일반적으로 해당 컨텍스트를 삭제 하는 것은 컨테이너의 책임입니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-132">If the context instance is created by a dependency injection container, it is usually the responsibility of the container to dispose the context.</span></span>
- <span data-ttu-id="ce747-133">응용 프로그램 코드에서 컨텍스트를 만든 경우 더 이상 필요 하지 않은 컨텍스트를 삭제 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-133">If the context is created in application code, remember to dispose of the context when it is no longer required.</span></span>  
- <span data-ttu-id="ce747-134">장기 실행 컨텍스트를 사용할 때 다음 사항을 고려 합니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-134">When working with long-running context consider the following:</span></span>  
    - <span data-ttu-id="ce747-135">더 많은 개체와 해당 참조를 메모리에 로드 하면 컨텍스트의 메모리 소비가 빠르게 증가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-135">As you load more objects and their references into memory, the memory consumption of the context may increase rapidly.</span></span> <span data-ttu-id="ce747-136">이로 인해 성능 문제가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-136">This may cause performance issues.</span></span>  
    - <span data-ttu-id="ce747-137">컨텍스트는 스레드로부터 안전 하지 않으므로 동시에 작업을 수행 하는 여러 스레드 간에 공유 되어서는 안 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-137">The context is not thread-safe, therefore it should not be shared across multiple threads doing work on it concurrently.</span></span>
    - <span data-ttu-id="ce747-138">예외로 인해 컨텍스트가 복구할 수 없는 상태가 되 면 전체 응용 프로그램이 종료 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-138">If an exception causes the context to be in an unrecoverable state, the whole application may terminate.</span></span>  
    - <span data-ttu-id="ce747-139">데이터가 쿼리되는 시간과 업데이트되는 시간의 간격이 커짐에 따라 동시성 관련 문제가 발생할 가능성이 높아집니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-139">The chances of running into concurrency-related issues increase as the gap between the time when the data is queried and updated grows.</span></span>  

## <a name="connections"></a><span data-ttu-id="ce747-140">연결</span><span class="sxs-lookup"><span data-stu-id="ce747-140">Connections</span></span>  

<span data-ttu-id="ce747-141">기본적으로 컨텍스트는 데이터베이스에 대 한 연결을 관리 합니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-141">By default, the context manages connections to the database.</span></span> <span data-ttu-id="ce747-142">컨텍스트는 필요에 따라 연결을 열고 닫습니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-142">The context opens and closes connections as needed.</span></span> <span data-ttu-id="ce747-143">예를 들어 컨텍스트는 연결을 열어 쿼리를 실행 한 다음 모든 결과 집합이 처리 될 때 연결을 닫습니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-143">For example, the context opens a connection to execute a query, and then closes the connection when all the result sets have been processed.</span></span>  

<span data-ttu-id="ce747-144">연결이 열리고 닫히는 때를 세부적으로 제어해야 하는 경우가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-144">There are cases when you want to have more control over when the connection opens and closes.</span></span> <span data-ttu-id="ce747-145">예를 들어 SQL Server Compact를 사용 하는 경우 성능 향상을 위해 응용 프로그램의 수명 동안 데이터베이스에 대 한 별도의 열린 연결을 유지 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-145">For example, when working with SQL Server Compact, it is often recommended to maintain a separate open connection to the database for the lifetime of the application to improve performance.</span></span> <span data-ttu-id="ce747-146">`Connection` 속성을 사용하여 이 프로세스를 수동으로 관리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ce747-146">You can manage this process manually by using the `Connection` property.</span></span>  
