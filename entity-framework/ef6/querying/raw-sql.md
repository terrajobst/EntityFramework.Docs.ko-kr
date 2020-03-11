---
title: 원시 SQL 쿼리-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 168aee67186535bf2a50705e86bfc5a88147e369
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414472"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="0ec2a-102">원시 SQL 쿼리</span><span class="sxs-lookup"><span data-stu-id="0ec2a-102">Raw SQL Queries</span></span>
<span data-ttu-id="0ec2a-103">Entity Framework를 사용 하면 엔터티 클래스와 함께 LINQ를 사용 하 여 쿼리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-103">Entity Framework allows you to query using LINQ with your entity classes.</span></span> <span data-ttu-id="0ec2a-104">그러나 원시 SQL을 사용 하 여 데이터베이스에 대해 직접 쿼리를 실행 하려는 경우가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-104">However, there may be times that you want to run queries using raw SQL directly against the database.</span></span> <span data-ttu-id="0ec2a-105">여기에는 저장 프로시저를 호출 하는 작업이 포함 됩니다 .이 프로시저는 현재 저장 프로시저에 대 한 매핑을 지원 하지 않는 Code First 모델에 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-105">This includes calling stored procedures, which can be helpful for Code First models that currently do not support mapping to stored procedures.</span></span> <span data-ttu-id="0ec2a-106">이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="writing-sql-queries-for-entities"></a><span data-ttu-id="0ec2a-107">엔터티에 대 한 SQL 쿼리 작성</span><span class="sxs-lookup"><span data-stu-id="0ec2a-107">Writing SQL queries for entities</span></span>  

<span data-ttu-id="0ec2a-108">DbSet의 SqlQuery 메서드는 엔터티 인스턴스를 반환 하는 원시 SQL 쿼리를 작성할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-108">The SqlQuery method on DbSet allows a raw SQL query to be written that will return entity instances.</span></span> <span data-ttu-id="0ec2a-109">반환 된 개체는 LINQ 쿼리에서 반환 되는 것과 마찬가지로 컨텍스트에 의해 추적 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-109">The returned objects will be tracked by the context just as they would be if they were returned by a LINQ query.</span></span> <span data-ttu-id="0ec2a-110">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-110">For example:</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="0ec2a-111">LINQ 쿼리와 마찬가지로 쿼리는 결과가 열거 될 때까지 실행 되지 않습니다. 위의 예제에서이 작업은 ToList를 호출 하 여 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-111">Note that, just as for LINQ queries, the query is not executed until the results are enumerated—in the example above this is done with the call to ToList.</span></span>  

<span data-ttu-id="0ec2a-112">두 가지 이유로 원시 SQL 쿼리를 작성할 때마다 주의를 기울여야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-112">Care should be taken whenever raw SQL queries are written for two reasons.</span></span> <span data-ttu-id="0ec2a-113">먼저 요청 된 형식의 엔터티만 반환 되도록 쿼리를 작성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-113">First, the query should be written to ensure that it only returns entities that are really of the requested type.</span></span> <span data-ttu-id="0ec2a-114">예를 들어 상속과 같은 기능을 사용 하는 경우 잘못 된 CLR 형식의 엔터티를 만드는 쿼리를 작성 하는 것이 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-114">For example, when using features such as inheritance it is easy to write a query that will create entities that are of the wrong CLR type.</span></span>  

<span data-ttu-id="0ec2a-115">둘째, 일부 원시 SQL 쿼리 유형은 특히 SQL 삽입 공격과 관련 된 잠재적인 보안 위험을 노출 합니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-115">Second, some types of raw SQL query expose potential security risks, especially around SQL injection attacks.</span></span> <span data-ttu-id="0ec2a-116">이러한 공격 으로부터 보호 하기 위해 올바른 방법으로 쿼리에서 매개 변수를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-116">Make sure that you use parameters in your query in the correct way to guard against such attacks.</span></span>  

### <a name="loading-entities-from-stored-procedures"></a><span data-ttu-id="0ec2a-117">저장 프로시저에서 엔터티 로드</span><span class="sxs-lookup"><span data-stu-id="0ec2a-117">Loading entities from stored procedures</span></span>  

<span data-ttu-id="0ec2a-118">DbSet를 사용 하 여 저장 프로시저의 결과에서 엔터티를 로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-118">You can use DbSet.SqlQuery to load entities from the results of a stored procedure.</span></span> <span data-ttu-id="0ec2a-119">예를 들어 다음 코드에서는 dbo를 호출 합니다. 데이터베이스의 GetBlogs 프로시저:</span><span class="sxs-lookup"><span data-stu-id="0ec2a-119">For example, the following code calls the dbo.GetBlogs procedure in the database:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

<span data-ttu-id="0ec2a-120">다음 구문을 사용 하 여 저장 프로시저에 매개 변수를 전달할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-120">You can also pass parameters to a stored procedure using the following syntax:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a><span data-ttu-id="0ec2a-121">비 엔터티 형식에 대 한 SQL 쿼리 작성</span><span class="sxs-lookup"><span data-stu-id="0ec2a-121">Writing SQL queries for non-entity types</span></span>  

<span data-ttu-id="0ec2a-122">기본 형식을 포함 하 여 모든 형식의 인스턴스를 반환 하는 SQL 쿼리는 데이터베이스 클래스의 SqlQuery 메서드를 사용 하 여 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-122">A SQL query returning instances of any type, including primitive types, can be created using the SqlQuery method on the Database class.</span></span> <span data-ttu-id="0ec2a-123">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-123">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="0ec2a-124">개체가 엔터티 형식의 인스턴스인 경우에도 데이터베이스의 SqlQuery에서 반환 된 결과는 컨텍스트에 의해 추적 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-124">The results returned from SqlQuery on Database will never be tracked by the context even if the objects are instances of an entity type.</span></span>  

## <a name="sending-raw-commands-to-the-database"></a><span data-ttu-id="0ec2a-125">데이터베이스에 원시 명령 보내기</span><span class="sxs-lookup"><span data-stu-id="0ec2a-125">Sending raw commands to the database</span></span>  

<span data-ttu-id="0ec2a-126">데이터베이스의 ExecuteSqlCommand 메서드를 사용 하 여 쿼리가 아닌 명령을 데이터베이스로 보낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-126">Non-query commands can be sent to the database using the ExecuteSqlCommand method on Database.</span></span> <span data-ttu-id="0ec2a-127">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-127">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

<span data-ttu-id="0ec2a-128">ExecuteSqlCommand를 사용 하는 데이터베이스의 데이터 변경 내용은 데이터베이스에서 엔터티가 로드 되거나 다시 로드 될 때까지 컨텍스트에 불투명 합니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-128">Note that any changes made to data in the database using ExecuteSqlCommand are opaque to the context until entities are loaded or reloaded from the database.</span></span>  

### <a name="output-parameters"></a><span data-ttu-id="0ec2a-129">출력 매개 변수</span><span class="sxs-lookup"><span data-stu-id="0ec2a-129">Output Parameters</span></span>  

<span data-ttu-id="0ec2a-130">출력 매개 변수를 사용 하는 경우 결과를 완전히 읽을 때까지 해당 값을 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-130">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="0ec2a-131">이는 DbDataReader의 기본 동작 때문입니다. 자세한 내용은 [DataReader를 사용 하 여 데이터 검색](https://go.microsoft.com/fwlink/?LinkID=398589) 을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="0ec2a-131">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>  
