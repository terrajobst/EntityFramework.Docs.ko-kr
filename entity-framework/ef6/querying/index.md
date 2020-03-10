---
title: 엔터티 쿼리 및 찾기 - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
ms.openlocfilehash: 29a86817e250a2f53ecaa73e8fa4bf93452f0497
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412978"
---
# <a name="querying-and-finding-entities"></a><span data-ttu-id="2fe32-102">엔터티 쿼리 및 찾기</span><span class="sxs-lookup"><span data-stu-id="2fe32-102">Querying and Finding Entities</span></span>
<span data-ttu-id="2fe32-103">이 토픽에서는 LINQ 및 Find 메서드를 포함하여 Entity Framework를 사용하여 데이터를 쿼리할 수 있는 다양한 방법을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-103">This topic covers the various ways you can query for data using Entity Framework, including LINQ and the Find method.</span></span> <span data-ttu-id="2fe32-104">이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="finding-entities-using-a-query"></a><span data-ttu-id="2fe32-105">쿼리를 사용하여 엔터티 찾기</span><span class="sxs-lookup"><span data-stu-id="2fe32-105">Finding entities using a query</span></span>  

<span data-ttu-id="2fe32-106">DbSet 및 IDbSet는 IQueryable을 구현하므로 데이터베이스에 대한 LINQ 쿼리를 작성하는 시작점으로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-106">DbSet and IDbSet implement IQueryable and so can be used as the starting point for writing a LINQ query against the database.</span></span> <span data-ttu-id="2fe32-107">지금은 LINQ에 대해 자세히 알아보는 대신, 몇 가지 간단한 예제만 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-107">This is not the appropriate place for an in-depth discussion of LINQ, but here are a couple of simple examples:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs with names starting with B
    var blogs = from b in context.Blogs
                   where b.Name.StartsWith("B")
                   select b;

    // Query for the Blog named ADO.NET Blog
    var blog = context.Blogs
                    .Where(b => b.Name == "ADO.NET Blog")
                    .FirstOrDefault();
}
```  

<span data-ttu-id="2fe32-108">DbSet IDbSet는 항상 데이터베이스에 대한 쿼리 만들며 반환된 엔터티가 이미 컨텍스트에 있더라도 항상 데이터베이스에 대한 왕복이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-108">Note that DbSet and IDbSet always create queries against the database and will always involve a round trip to the database even if the entities returned already exist in the context.</span></span> <span data-ttu-id="2fe32-109">다음과 같은 경우 데이터베이스에 대해 쿼리가 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-109">A query is executed against the database when:</span></span>  

- <span data-ttu-id="2fe32-110">**foreach**(C#) 또는 **For Each**(Visual Basic) 문에 의해 열거된 경우.</span><span class="sxs-lookup"><span data-stu-id="2fe32-110">It is enumerated by a **foreach** (C#) or **For Each** (Visual Basic) statement.</span></span>  
- <span data-ttu-id="2fe32-111">[ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary) 또는 [ToList](https://msdn.microsoft.com/library/bb342261)와 같은 컬렉션 작업에 의해 열거된 경우.</span><span class="sxs-lookup"><span data-stu-id="2fe32-111">It is enumerated by a collection operation such as [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary), or [ToList](https://msdn.microsoft.com/library/bb342261).</span></span>  
- <span data-ttu-id="2fe32-112">쿼리의 가장 바깥쪽 부분에 [First](https://msdn.microsoft.com/library/bb291976) 또는 [Any](https://msdn.microsoft.com/library/bb337697) 같은 LINQ 연산자가 지정된 경우.</span><span class="sxs-lookup"><span data-stu-id="2fe32-112">LINQ operators such as [First](https://msdn.microsoft.com/library/bb291976) or [Any](https://msdn.microsoft.com/library/bb337697) are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="2fe32-113">DbSet의 [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) 확장 메서드, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx) 메서드 및 Database.ExecuteSqlCommand 메서드가 호출됩니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-113">The following methods are called: the [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) extension method on a DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx), and Database.ExecuteSqlCommand.</span></span>  

<span data-ttu-id="2fe32-114">데이터베이스에서 결과가 반환되면 컨텍스트에 없는 개체는 컨텍스트에 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-114">When results are returned from the database, objects that do not exist in the context are attached to the context.</span></span> <span data-ttu-id="2fe32-115">개체가 이미 컨텍스트에 있는 경우 기존 개체가 반환됩니다(항목 개체 속성의 현재 값과 원래 값을 데이터베이스 값으로 덮어쓰지 **않습니다**).</span><span class="sxs-lookup"><span data-stu-id="2fe32-115">If an object is already in the context, the existing object is returned (the current and original values of the object's properties in the entry are **not** overwritten with database values).</span></span>  

<span data-ttu-id="2fe32-116">쿼리를 수행하면 컨텍스트에 추가되었지만 아직 데이터베이스에 저장되지 않은 엔터티는 결과 집합의 일부로 반환되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-116">When you perform a query, entities that have been added to the context but have not yet been saved to the database are not returned as part of the result set.</span></span> <span data-ttu-id="2fe32-117">컨텍스트에 있는 데이터를 가져오는 방법은 [로컬 데이터](~/ef6/querying/local-data.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="2fe32-117">To get the data that is in the context, see [Local Data](~/ef6/querying/local-data.md).</span></span>  

<span data-ttu-id="2fe32-118">쿼리가 데이터베이스의 어떤 행도 반환하지 않는 경우 결과는 **null**이 아니라 빈 컬렉션입니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-118">If a query returns no rows from the database, the result will be an empty collection, rather than **null**.</span></span>  

## <a name="finding-entities-using-primary-keys"></a><span data-ttu-id="2fe32-119">기본 키를 사용하여 엔터티 찾기</span><span class="sxs-lookup"><span data-stu-id="2fe32-119">Finding entities using primary keys</span></span>  

<span data-ttu-id="2fe32-120">DbSet의 Find 메서드는 기본 키 값을 사용하여 컨텍스트에서 추적하는 엔터티를 찾으려고 시도합니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-120">The Find method on DbSet uses the primary key value to attempt to find an entity tracked by the context.</span></span> <span data-ttu-id="2fe32-121">컨텍스트에서 엔터티를 찾을 수 없는 경우 쿼리를 데이터베이스로 보내서 데이터베이스에서 엔터티를 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-121">If the entity is not found in the context then a query will be sent to the database to find the entity there.</span></span> <span data-ttu-id="2fe32-122">컨텍스트 또는 데이터베이스에 엔터티가 없으면 null이 반환됩니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-122">Null is returned if the entity is not found in the context or in the database.</span></span>  

<span data-ttu-id="2fe32-123">Find는 다음과 같은 두 가지 측면에서 쿼리 사용과 큰 차이가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-123">Find is different from using a query in two significant ways:</span></span>  

- <span data-ttu-id="2fe32-124">지정된 키가 있는 엔터티를 컨텍스트에서 찾을 수 없는 경우에만 데이터베이스에 대한 왕복이 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-124">A round-trip to the database will only be made if the entity with the given key is not found in the context.</span></span>  
- <span data-ttu-id="2fe32-125">Find는 추가됨 상태인 엔터티를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-125">Find will return entities that are in the Added state.</span></span> <span data-ttu-id="2fe32-126">즉, Find는 컨텍스트에 추가되었지만 아직 데이터베이스에 저장되지 않은 엔터티를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-126">That is, Find will return entities that have been added to the context but have not yet been saved to the database.</span></span>  
### <a name="finding-an-entity-by-primary-key"></a><span data-ttu-id="2fe32-127">기본 키로 엔터티 찾기</span><span class="sxs-lookup"><span data-stu-id="2fe32-127">Finding an entity by primary key</span></span>  

<span data-ttu-id="2fe32-128">다음 코드는 몇 가지 Find 사용 사례를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-128">The following code shows some uses of Find:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Will hit the database
    var blog = context.Blogs.Find(3);

    // Will return the same instance without hitting the database
    var blogAgain = context.Blogs.Find(3);

    context.Blogs.Add(new Blog { Id = -1 });

    // Will find the new blog even though it does not exist in the database
    var newBlog = context.Blogs.Find(-1);

    // Will find a User which has a string primary key
    var user = context.Users.Find("johndoe1987");
}
```  

### <a name="finding-an-entity-by-composite-primary-key"></a><span data-ttu-id="2fe32-129">복합 기본 키로 엔터티 찾기</span><span class="sxs-lookup"><span data-stu-id="2fe32-129">Finding an entity by composite primary key</span></span>  

<span data-ttu-id="2fe32-130">Entity Framework는 엔터티에 두 개 이상의 속성으로 구성되는 복합 키를 허용합니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-130">Entity Framework allows your entities to have composite keys - that's a key that is made up of more than one property.</span></span> <span data-ttu-id="2fe32-131">예를 들어 특정 블로그에 대한 사용자 설정을 나타내는 BlogSettings 엔터티가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-131">For example, you could have a BlogSettings entity that represents a users settings for a particular blog.</span></span> <span data-ttu-id="2fe32-132">사용자는 BlogId 및 Username의 조합을 BlogSettings 기본 키로 만들기 위해 선택할 수 있는 블로그마다 오직 하나의 BlogSettings만 갖기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-132">Because a user would only ever have one BlogSettings for each blog you could chose to make the primary key of BlogSettings a combination of BlogId and Username.</span></span> <span data-ttu-id="2fe32-133">다음 코드는 BlogId = 3이고 Username = "johndoe1987"인 BlogSettings를 찾으려고 시도합니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-133">The following code attempts to find the BlogSettings with BlogId = 3 and Username = "johndoe1987":</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

<span data-ttu-id="2fe32-134">복합 키가 있는 경우 ColumnAttribute 또는 흐름 API를 사용하여 복합 키 속성의 순서를 지정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-134">Note that when you have composite keys you need to use ColumnAttribute or the fluent API to specify an ordering for the properties of the composite key.</span></span> <span data-ttu-id="2fe32-135">키를 구성하는 값을 지정할 때 Find 호출에서 이 순서를 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2fe32-135">The call to Find must use this order when specifying the values that form the key.</span></span>  
