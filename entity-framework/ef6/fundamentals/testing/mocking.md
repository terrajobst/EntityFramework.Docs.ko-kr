---
title: 모의 프레임 워크를 사용 하 여 테스트-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: 790e077c5b30c4a68a96b3c1a99b40893b2bbe55
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415996"
---
# <a name="testing-with-a-mocking-framework"></a><span data-ttu-id="54b77-102">모의 프레임 워크를 사용 하 여 테스트</span><span class="sxs-lookup"><span data-stu-id="54b77-102">Testing with a mocking framework</span></span>
> [!NOTE]
> <span data-ttu-id="54b77-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="54b77-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="54b77-105">응용 프로그램에 대 한 테스트를 작성 하는 경우 데이터베이스에 대 한 적중을 방지 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="54b77-106">Entity Framework를 사용 하면 테스트에 정의 된 동작을 사용 하 여 메모리 내 데이터를 사용 하는 컨텍스트를 만들어이를 달성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="54b77-107">테스트 double을 만드는 옵션</span><span class="sxs-lookup"><span data-stu-id="54b77-107">Options for creating test doubles</span></span>  

<span data-ttu-id="54b77-108">컨텍스트의 메모리 내 버전을 만드는 데 사용할 수 있는 두 가지 방법이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="54b77-109">**사용자 고유의 테스트 만들기** -이 방법은 사용자의 컨텍스트와 dbsets의 고유한 메모리 내 구현을 작성 하는 것을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="54b77-110">이렇게 하면 클래스가 동작 하는 방식을 제어할 수 있을 뿐만 아니라 적절 한 양의 코드를 작성 하 고 소유 하는 것을 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="54b77-111">**모의 프레임 워크를 사용 하 여 테스트 double 만들기** – 모의 프레임 워크 (예: moq)를 사용 하 여 컨텍스트의 메모리 내 구현을 사용 하 고 런타임에 동적으로 생성 되는 집합을 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of your context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="54b77-112">이 문서에서는 모의 프레임 워크를 사용 하는 방법을 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-112">This article will deal with using a mocking framework.</span></span> <span data-ttu-id="54b77-113">사용자 고유의 테스트를 만들려면 [Double 테스트를 사용 하 여 테스트 double](writing-test-doubles.md)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="54b77-113">For creating your own test doubles see [Testing with Your Own Test Doubles](writing-test-doubles.md).</span></span>  

<span data-ttu-id="54b77-114">모의 프레임 워크에서 EF를 사용 하는 방법을 시연 하기 위해 Moq를 사용할 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-114">To demonstrate using EF with a mocking framework we are going to use Moq.</span></span> <span data-ttu-id="54b77-115">Moq를 얻는 가장 쉬운 방법은 [NuGet에서 moq 패키지](https://nuget.org/packages/Moq/)를 설치 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-115">The easiest way to get Moq is to install the [Moq package from NuGet](https://nuget.org/packages/Moq/).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="54b77-116">사전 EF6 버전으로 테스트</span><span class="sxs-lookup"><span data-stu-id="54b77-116">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="54b77-117">이 문서에 나와 있는 시나리오는 EF6에서 DbSet에 적용 한 일부 변경 내용에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-117">The scenario shown in this article is dependent on some changes we made to DbSet in EF6.</span></span> <span data-ttu-id="54b77-118">EF5 이전 버전을 사용한 테스트는 [가짜 컨텍스트로 테스트](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="54b77-118">For testing with EF5 and earlier version see [Testing with a Fake Context](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="54b77-119">EF 메모리 내 테스트 double의 제한 사항</span><span class="sxs-lookup"><span data-stu-id="54b77-119">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="54b77-120">메모리 내 테스트 double은 EF를 사용 하는 응용 프로그램의 비트 단위 테스트 수준 범위를 제공 하는 좋은 방법일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-120">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="54b77-121">그러나이 작업을 수행 하는 경우 LINQ to Objects를 사용 하 여 메모리 내 데이터에 대해 쿼리를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-121">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="54b77-122">이렇게 하면 EF의 LINQ 공급자 (LINQ to Entities)를 사용 하 여 데이터베이스에 대해 실행 되는 SQL로 쿼리를 변환 하는 것과 다른 동작이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-122">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="54b77-123">이러한 차이점의 한 가지 예는 관련 데이터를 로드 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-123">One example of such a difference is loading related data.</span></span> <span data-ttu-id="54b77-124">각각 관련 게시물이 있는 일련의 블로그를 만드는 경우 메모리 내 데이터를 사용 하는 경우 관련 게시물이 항상 각 블로그에 대해 로드 됩니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-124">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="54b77-125">그러나 데이터베이스에 대해 실행 하는 경우에는 Include 메서드를 사용 하는 경우에만 데이터가 로드 됩니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-125">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="54b77-126">따라서 응용 프로그램이 데이터베이스에 대해 올바르게 작동 하도록 하기 위해 항상 특정 수준의 종단 간 테스트 (단위 테스트 포함)를 포함 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-126">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="54b77-127">이 문서와 함께 다음을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-127">Following along with this article</span></span>  

<span data-ttu-id="54b77-128">이 문서에서는 Visual Studio로 복사 하 여 원하는 경우 따라 진행할 수 있는 전체 코드 목록을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-128">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="54b77-129">**단위 테스트 프로젝트** 를 만드는 것이 가장 쉽지만 비동기를 사용 하는 섹션을 완료 하려면 **.NET Framework 4.5** 를 대상으로 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-129">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="the-ef-model"></a><span data-ttu-id="54b77-130">EF 모델</span><span class="sxs-lookup"><span data-stu-id="54b77-130">The EF model</span></span>  

<span data-ttu-id="54b77-131">테스트 하려는 서비스는 BloggingContext 및 블로그 및 게시물 클래스로 구성 된 EF 모델을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-131">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="54b77-132">이 코드는 EF Designer에서 생성 되었거나 Code First 모델 일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-132">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext
    {
        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }
        public string Url { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }
}
```  

### <a name="virtual-dbset-properties-with-ef-designer"></a><span data-ttu-id="54b77-133">EF Designer를 사용 하 여 가상 DbSet 속성</span><span class="sxs-lookup"><span data-stu-id="54b77-133">Virtual DbSet properties with EF Designer</span></span>  

<span data-ttu-id="54b77-134">컨텍스트의 DbSet 속성은 virtual로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-134">Note that the DbSet properties on the context are marked as virtual.</span></span> <span data-ttu-id="54b77-135">이렇게 하면 모의 프레임 워크가 컨텍스트에서 파생 되 고 모의 구현으로 이러한 속성을 재정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-135">This will allow the mocking framework to derive from our context and overriding these properties with a mocked implementation.</span></span>  

<span data-ttu-id="54b77-136">Code First를 사용 하는 경우 클래스를 직접 편집할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-136">If you are using Code First then you can edit your classes directly.</span></span> <span data-ttu-id="54b77-137">EF Designer를 사용 하는 경우 컨텍스트를 생성 하는 T4 템플릿을 편집 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-137">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="54b77-138">\<model_name\>를 엽니다. Context.tt 파일은 edmx 파일에 중첩 되어 있습니다. 다음 코드 조각을 찾아 virtual 키워드에 다음과 같이 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-138">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the virtual keyword as shown.</span></span>  

``` csharp
public string DbSet(EntitySet entitySet)
{
    return string.Format(
        CultureInfo.InvariantCulture,
        "{0} virtual DbSet\<{1}> {2} {{ get; set; }}",
        Accessibility.ForReadOnlyProperty(entitySet),
        _typeMapper.GetTypeName(entitySet.ElementType),
        _code.Escape(entitySet));
}
```  

## <a name="service-to-be-tested"></a><span data-ttu-id="54b77-139">테스트할 서비스</span><span class="sxs-lookup"><span data-stu-id="54b77-139">Service to be tested</span></span>  

<span data-ttu-id="54b77-140">메모리 내 테스트 double로 테스트 하는 방법을 보여 주기 위해 BlogService에 대 한 몇 가지 테스트를 작성할 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-140">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="54b77-141">서비스는 새 블로그 (AddBlog)를 만들고 이름을 기준으로 정렬 된 모든 블로그 (GetAllBlogs)를 반환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-141">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="54b77-142">GetAllBlogs 외에도 이름 (GetAllBlogsAsync)으로 정렬 된 모든 블로그를 비동기적으로 가져오는 메서드도 제공 했습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-142">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class BlogService
    {
        private BloggingContext _context;

        public BlogService(BloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = _context.Blogs.Add(new Blog { Name = name, Url = url });
            _context.SaveChanges();

            return blog;
        }

        public List<Blog> GetAllBlogs()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return query.ToList();
        }

        public async Task<List<Blog>> GetAllBlogsAsync()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return await query.ToListAsync();
        }
    }
}
```  

## <a name="testing-non-query-scenarios"></a><span data-ttu-id="54b77-143">쿼리가 아닌 시나리오 테스트</span><span class="sxs-lookup"><span data-stu-id="54b77-143">Testing non-query scenarios</span></span>  

<span data-ttu-id="54b77-144">쿼리가 아닌 메서드 테스트를 시작 하기 위해 수행 해야 하는 작업입니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-144">That’s all we need to do to start testing non-query methods.</span></span> <span data-ttu-id="54b77-145">다음 테스트에서는 Moq를 사용 하 여 컨텍스트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-145">The following test uses Moq to create a context.</span></span> <span data-ttu-id="54b77-146">그런 다음 DbSet\<블로그\>를 만들고, 컨텍스트의 블로그 속성에서 반환 될 와이어를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-146">It then creates a DbSet\<Blog\> and wires it up to be returned from the context’s Blogs property.</span></span> <span data-ttu-id="54b77-147">그런 다음, 컨텍스트를 사용 하 여 새 BlogService를 만든 다음 AddBlog 메서드를 사용 하 여 새 블로그를 만드는 데 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-147">Next, the context is used to create a new BlogService which is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="54b77-148">마지막으로 테스트는 서비스에서 새 블로그를 추가 하 고 컨텍스트에서 SaveChanges를 호출 했는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-148">Finally, the test verifies that the service added a new Blog and called SaveChanges on the context.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Data.Entity;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var mockSet = new Mock<DbSet<Blog>>();

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(m => m.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            mockSet.Verify(m => m.Add(It.IsAny<Blog>()), Times.Once());
            mockContext.Verify(m => m.SaveChanges(), Times.Once());
        }
    }
}
```  

## <a name="testing-query-scenarios"></a><span data-ttu-id="54b77-149">쿼리 시나리오 테스트</span><span class="sxs-lookup"><span data-stu-id="54b77-149">Testing query scenarios</span></span>  

<span data-ttu-id="54b77-150">DbSet 테스트 double에 대해 쿼리를 실행할 수 있으려면 IQueryable의 구현을 설정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-150">In order to be able to execute queries against our DbSet test double we need to setup an implementation of IQueryable.</span></span> <span data-ttu-id="54b77-151">첫 번째 단계는 일부 메모리 내 데이터를 만드는 것입니다.\<블로그\>목록을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-151">The first step is to create some in-memory data – we’re using a List\<Blog\>.</span></span> <span data-ttu-id="54b77-152">다음으로는 컨텍스트 및 DBSet\<블로그\> 만든 다음, DbSet에 대 한 IQueryable 구현을 연결 합니다 .이는 목록\<T\>에서 작동 하는 LINQ to Objects 공급자에 게 위임 됩니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-152">Next, we create a context and DBSet\<Blog\> then wire up the IQueryable implementation for the DbSet – they’re just delegating to the LINQ to Objects provider that works with List\<T\>.</span></span>  

<span data-ttu-id="54b77-153">그런 다음 테스트 double을 기반으로 BlogService를 만들고 GetAllBlogs 데이터를 이름별로 정렬할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-153">We can then create a BlogService based on our test doubles and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Provider).Returns(data.Provider);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

### <a name="testing-with-async-queries"></a><span data-ttu-id="54b77-154">비동기 쿼리를 사용 하 여 테스트</span><span class="sxs-lookup"><span data-stu-id="54b77-154">Testing with async queries</span></span>

<span data-ttu-id="54b77-155">Entity Framework 6에서는 쿼리를 비동기적으로 실행 하는 데 사용할 수 있는 확장 메서드 집합을 도입 했습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-155">Entity Framework 6 introduced a set of extension methods that can be used to asynchronously execute a query.</span></span> <span data-ttu-id="54b77-156">이러한 메서드의 예로는 ToListAsync, FirstAsync, ForEachAsync 등이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-156">Examples of these methods include ToListAsync, FirstAsync, ForEachAsync, etc.</span></span>  

<span data-ttu-id="54b77-157">쿼리에서 LINQ를 사용 Entity Framework 하기 때문에 확장 메서드는 IQueryable 및 IEnumerable에 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-157">Because Entity Framework queries make use of LINQ, the extension methods are defined on IQueryable and IEnumerable.</span></span> <span data-ttu-id="54b77-158">그러나 Entity Framework 사용 하도록 디자인 되었기 때문에 Entity Framework 쿼리가 아닌 LINQ 쿼리에서 사용 하려고 하면 다음 오류가 나타날 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-158">However, because they are only designed to be used with Entity Framework you may receive the following error if you try to use them on a LINQ query that isn’t an Entity Framework query:</span></span>

> <span data-ttu-id="54b77-159">원본 IQueryable은 IDbAsyncEnumerable{0}를 구현 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-159">The source IQueryable doesn't implement IDbAsyncEnumerable{0}.</span></span> <span data-ttu-id="54b77-160">IDbAsyncEnumerable을 구현 하는 원본만 Entity Framework 비동기 작업에 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-160">Only sources that implement IDbAsyncEnumerable can be used for Entity Framework asynchronous operations.</span></span> <span data-ttu-id="54b77-161">자세한 내용은 [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="54b77-161">For more details see [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068).</span></span>  

<span data-ttu-id="54b77-162">비동기 메서드는 EF 쿼리를 실행 하는 경우에만 지원 되지만 DbSet의 메모리 내 테스트 double에 대해 실행 될 때 단위 테스트에서 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-162">Whilst the async methods are only supported when running against an EF query, you may want to use them in your unit test when running against an in-memory test double of a DbSet.</span></span>  

<span data-ttu-id="54b77-163">비동기 메서드를 사용 하려면 비동기 쿼리를 처리 하기 위해 메모리 내 DbAsyncQueryProvider를 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-163">In order to use the async methods we need to create an in-memory DbAsyncQueryProvider to process the async query.</span></span> <span data-ttu-id="54b77-164">Moq를 사용 하 여 쿼리 공급자를 설정 하는 것이 가능 하기는 하지만 코드에서 테스트 이중 구현을 만드는 것이 훨씬 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-164">Whilst it would be possible to setup a query provider using Moq, it is much easier to create a test double implementation in code.</span></span> <span data-ttu-id="54b77-165">이 구현에 대 한 코드는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-165">The code for this implementation is as follows:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
    internal class TestDbAsyncQueryProvider<TEntity> : IDbAsyncQueryProvider
    {
        private readonly IQueryProvider _inner;

        internal TestDbAsyncQueryProvider(IQueryProvider inner)
        {
            _inner = inner;
        }

        public IQueryable CreateQuery(Expression expression)
        {
            return new TestDbAsyncEnumerable<TEntity>(expression);
        }

        public IQueryable<TElement> CreateQuery<TElement>(Expression expression)
        {
            return new TestDbAsyncEnumerable<TElement>(expression);
        }

        public object Execute(Expression expression)
        {
            return _inner.Execute(expression);
        }

        public TResult Execute<TResult>(Expression expression)
        {
            return _inner.Execute<TResult>(expression);
        }

        public Task<object> ExecuteAsync(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute(expression));
        }

        public Task<TResult> ExecuteAsync<TResult>(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute<TResult>(expression));
        }
    }

    internal class TestDbAsyncEnumerable<T> : EnumerableQuery<T>, IDbAsyncEnumerable<T>, IQueryable<T>
    {
        public TestDbAsyncEnumerable(IEnumerable<T> enumerable)
            : base(enumerable)
        { }

        public TestDbAsyncEnumerable(Expression expression)
            : base(expression)
        { }

        public IDbAsyncEnumerator<T> GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<T>(this.AsEnumerable().GetEnumerator());
        }

        IDbAsyncEnumerator IDbAsyncEnumerable.GetAsyncEnumerator()
        {
            return GetAsyncEnumerator();
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<T>(this); }
        }
    }

    internal class TestDbAsyncEnumerator<T> : IDbAsyncEnumerator<T>
    {
        private readonly IEnumerator<T> _inner;

        public TestDbAsyncEnumerator(IEnumerator<T> inner)
        {
            _inner = inner;
        }

        public void Dispose()
        {
            _inner.Dispose();
        }

        public Task<bool> MoveNextAsync(CancellationToken cancellationToken)
        {
            return Task.FromResult(_inner.MoveNext());
        }

        public T Current
        {
            get { return _inner.Current; }
        }

        object IDbAsyncEnumerator.Current
        {
            get { return Current; }
        }
    }
}
```  

<span data-ttu-id="54b77-166">이제 비동기 쿼리 공급자를 만들었으므로 새로운 GetAllBlogsAsync 메서드에 대 한 단위 테스트를 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54b77-166">Now that we have an async query provider we can write a unit test for our new GetAllBlogsAsync method.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    [TestClass]
    public class AsyncQueryTests
    {
        [TestMethod]
        public async Task GetAllBlogsAsync_orders_by_name()
        {

            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IDbAsyncEnumerable<Blog>>()
                .Setup(m => m.GetAsyncEnumerator())
                .Returns(new TestDbAsyncEnumerator<Blog>(data.GetEnumerator()));

            mockSet.As<IQueryable<Blog>>()
                .Setup(m => m.Provider)
                .Returns(new TestDbAsyncQueryProvider<Blog>(data.Provider));

            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
