---
title: 원시 SQL 쿼리-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 6b00648939ccedffeed09b4e1d6e8d70fa262a36
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490586"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="8652e-102">원시 SQL 쿼리</span><span class="sxs-lookup"><span data-stu-id="8652e-102">Raw SQL Queries</span></span>
<span data-ttu-id="8652e-103">Entity Framework를 사용 하면 LINQ를 사용 하 여 엔터티 클래스를 사용 하 여 쿼리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-103">Entity Framework allows you to query using LINQ with your entity classes.</span></span> <span data-ttu-id="8652e-104">그러나 데이터베이스에 대해 직접 원시 SQL을 사용 하 여 쿼리를 실행 하려는 경우가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-104">However, there may be times that you want to run queries using raw SQL directly against the database.</span></span> <span data-ttu-id="8652e-105">여기에 현재 저장된 프로시저 매핑을 지원 하지 않는 Code First 모델에 대 한 도움이 될 수 있는 저장된 프로시저를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-105">This includes calling stored procedures, which can be helpful for Code First models that currently do not support mapping to stored procedures.</span></span> <span data-ttu-id="8652e-106">이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="writing-sql-queries-for-entities"></a><span data-ttu-id="8652e-107">엔터티에 대 한 SQL 쿼리를 작성합니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-107">Writing SQL queries for entities</span></span>  

<span data-ttu-id="8652e-108">DbSet SqlQuery 메서드를 쓸 엔터티 인스턴스를 반환할 원시 SQL 쿼리를 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-108">The SqlQuery method on DbSet allows a raw SQL query to be written that will return entity instances.</span></span> <span data-ttu-id="8652e-109">LINQ 쿼리에 의해 반환 된 경우 됩니다 것 처럼 반환 되는 개체 컨텍스트에 의해 추적 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-109">The returned objects will be tracked by the context just as they would be if they were returned by a LINQ query.</span></span> <span data-ttu-id="8652e-110">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="8652e-110">For example:</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="8652e-111">LINQ 쿼리, 마찬가지로 쿼리가 실행 되지 않는 결과 열거 될 때까지 참고-위의 예제에서 이렇게 ToList 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-111">Note that, just as for LINQ queries, the query is not executed until the results are enumerated—in the example above this is done with the call to ToList.</span></span>  

<span data-ttu-id="8652e-112">원시 SQL 쿼리는 두 가지 이유로 작성 될 때마다 주의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-112">Care should be taken whenever raw SQL queries are written for two reasons.</span></span> <span data-ttu-id="8652e-113">먼저 쿼리는 실제로 요청 된 형식의 엔터티를만 반환 되도록 작성 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-113">First, the query should be written to ensure that it only returns entities that are really of the requested type.</span></span> <span data-ttu-id="8652e-114">예를 들어, 상속 등의 기능을 사용 하는 경우 쉽습니다 CLR 형식이 잘못 된 엔터티를 통해 생성 되는 쿼리를 작성 하려면.</span><span class="sxs-lookup"><span data-stu-id="8652e-114">For example, when using features such as inheritance it is easy to write a query that will create entities that are of the wrong CLR type.</span></span>  

<span data-ttu-id="8652e-115">둘째, 일부 유형의 원시 SQL 쿼리는 SQL 삽입 공격 특히 잠재적인 보안 위험을 노출합니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-115">Second, some types of raw SQL query expose potential security risks, especially around SQL injection attacks.</span></span> <span data-ttu-id="8652e-116">이러한 공격 방지 하기 위해 올바른 방법으로 쿼리에 매개 변수를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-116">Make sure that you use parameters in your query in the correct way to guard against such attacks.</span></span>  

### <a name="loading-entities-from-stored-procedures"></a><span data-ttu-id="8652e-117">저장된 프로시저에서 엔터티를 로드합니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-117">Loading entities from stored procedures</span></span>  

<span data-ttu-id="8652e-118">저장된 프로시저 결과에서 엔터티를 로드 하려면 DbSet.SqlQuery를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-118">You can use DbSet.SqlQuery to load entities from the results of a stored procedure.</span></span> <span data-ttu-id="8652e-119">예를 들어, 다음 코드를 dbo를 호출합니다. 데이터베이스의 GetBlogs 프로시저:</span><span class="sxs-lookup"><span data-stu-id="8652e-119">For example, the following code calls the dbo.GetBlogs procedure in the database:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

<span data-ttu-id="8652e-120">또한 다음 구문을 사용 하 여 저장된 프로시저에 매개 변수를 전달할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-120">You can also pass parameters to a stored procedure using the following syntax:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a><span data-ttu-id="8652e-121">비 엔터티 형식에 대 한 SQL 쿼리를 작성합니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-121">Writing SQL queries for non-entity types</span></span>  

<span data-ttu-id="8652e-122">SqlQuery 메서드를 사용 하 여 데이터베이스 클래스의 기본 형식을 비롯 한 모든 형식의 인스턴스를 반환 하 여 SQL 쿼리를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-122">A SQL query returning instances of any type, including primitive types, can be created using the SqlQuery method on the Database class.</span></span> <span data-ttu-id="8652e-123">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="8652e-123">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="8652e-124">SqlQuery 데이터베이스에서 반환 된 결과 개체는 엔터티 형식의 인스턴스인 경우에 컨텍스트에 의해 추적 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-124">The results returned from SqlQuery on Database will never be tracked by the context even if the objects are instances of an entity type.</span></span>  

## <a name="sending-raw-commands-to-the-database"></a><span data-ttu-id="8652e-125">데이터베이스에 원시 명령 보내기</span><span class="sxs-lookup"><span data-stu-id="8652e-125">Sending raw commands to the database</span></span>  

<span data-ttu-id="8652e-126">쿼리가 아닌 명령 ExecuteSqlCommand 메서드를 사용 하 여 데이터베이스에서 데이터베이스에 보낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-126">Non-query commands can be sent to the database using the ExecuteSqlCommand method on Database.</span></span> <span data-ttu-id="8652e-127">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="8652e-127">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

<span data-ttu-id="8652e-128">엔터티가 로드 되거나 데이터베이스에서 다시 로드 될 때까지 ExecuteSqlCommand를 사용 하 여 데이터베이스의 데이터에 대 한 변경 내용을 컨텍스트에 불투명는 note 합니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-128">Note that any changes made to data in the database using ExecuteSqlCommand are opaque to the context until entities are loaded or reloaded from the database.</span></span>  

### <a name="output-parameters"></a><span data-ttu-id="8652e-129">출력 매개 변수</span><span class="sxs-lookup"><span data-stu-id="8652e-129">Output Parameters</span></span>  

<span data-ttu-id="8652e-130">출력 매개 변수를 사용 하는 경우 결과 완전히 읽을 때까지 해당 값은 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-130">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="8652e-131">내용은 DbDataReader의 기본 동작으로 인해 이것이 [DataReader를 사용 하 여 데이터 검색](http://go.microsoft.com/fwlink/?LinkID=398589) 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="8652e-131">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>  
