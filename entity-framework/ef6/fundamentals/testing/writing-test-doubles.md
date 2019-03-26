---
title: 사용자 고유의 test double-EF6 사용 하 여 테스트
author: divega
ms.date: 10/23/2016
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: 9db56e28cd89084fece36c3e5a2c1b4495991d01
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419733"
---
# <a name="testing-with-your-own-test-doubles"></a><span data-ttu-id="8d7db-102">사용자 고유의 test double을 사용 하 여 테스트</span><span class="sxs-lookup"><span data-stu-id="8d7db-102">Testing with your own test doubles</span></span>
> [!NOTE]
> <span data-ttu-id="8d7db-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="8d7db-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="8d7db-105">응용 프로그램에 대 한 테스트를 작성 하는 경우 데이터베이스 도달 하지 않도록 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="8d7db-106">Entity Framework를 사용 하 여 메모리 내 데이터 컨텍스트를 만들어 – – 테스트를 통해 정의 되는 동작을 사용 하 여이 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="8d7db-107">Test double을 만들기 위한 옵션</span><span class="sxs-lookup"><span data-stu-id="8d7db-107">Options for creating test doubles</span></span>  

<span data-ttu-id="8d7db-108">메모리 내 버전이 컨텍스트를 만드는 데 사용할 수 있는 두 가지 방법이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="8d7db-109">**사용자 고유의 test double을 만드는** -이 방법은 사용자의 컨텍스트 및 Dbset 고유한 메모리 내 구현의 작성 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="8d7db-110">이 클래스의 동작 하지만 작성 하 고 적절 한 양의 코드를 소유 하는 포함 될 수 있습니다 하는 방법에 대 한 제어 많이 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="8d7db-111">**모의 프레임 워크를 사용 하 여 test double을 만들려면** -(예: Moq) 모의 프레임 워크를 사용 하 여 할 수 있습니다의 메모리 내 구현을 컨텍스트와를 런타임에 동적으로 만든 집합입니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of you context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="8d7db-112">이 문서에서는 고유한 테스트를 이중 만들기를 처리 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-112">This article will deal with creating your own test double.</span></span> <span data-ttu-id="8d7db-113">모의 프레임 워크를 사용 하는 방법은 참조 하세요. [모의 프레임 워크를 사용 하 여 테스트](mocking.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-113">For information on using a mocking framework see [Testing with a Mocking Framework](mocking.md).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="8d7db-114">EF6 이전 버전을 사용 하 여 테스트</span><span class="sxs-lookup"><span data-stu-id="8d7db-114">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="8d7db-115">이 문서에 나와 있는 코드는 EF6 호환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-115">The code shown in this article is compatible with EF6.</span></span> <span data-ttu-id="8d7db-116">EF5 및 이전 버전을 사용 하 여 테스트에 대 한 참조 [가짜 컨텍스트를 사용 하 여 테스트](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/)합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-116">For testing with EF5 and earlier version see [Testing with a Fake Context](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="8d7db-117">EF에서 메모리 test double의 제한 사항</span><span class="sxs-lookup"><span data-stu-id="8d7db-117">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="8d7db-118">메모리 내 test double은 단위 테스트 응용 프로그램의 EF를 사용 하는 비트 수준 검사를 제공 하는 좋은 방법 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-118">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="8d7db-119">그러나 이렇게 하면 사용 하는 LINQ to Objects 메모리 내 데이터에 대 한 쿼리를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-119">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="8d7db-120">이 데이터베이스에 대해 실행 되는 SQL 쿼리를 변환할 EF의 LINQ 공급자 (LINQ to Entities)를 사용 하 여 보다 다양 한 동작이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-120">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="8d7db-121">이러한 차이의 예로 관련된 데이터를 로드 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-121">One example of such a difference is loading related data.</span></span> <span data-ttu-id="8d7db-122">블로그 시리즈를 만들면 관련 게시물 각 다음 각 블로그에 대해 항상 관련된 게시물을 로드는 메모리 내 데이터를 사용 하는 경우.</span><span class="sxs-lookup"><span data-stu-id="8d7db-122">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="8d7db-123">그러나 데이터베이스에 대해 실행 하는 경우 이러한 데이터는 Include 메서드를 사용 하는 경우 로드만 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-123">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="8d7db-124">이러한 이유로 항상 일정 수준의 엔드-투-엔드 데이터베이스에 대해 올바르게 응용 프로그램이 작동 되도록 (단위 테스트) 외에도 테스트를 포함 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-124">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="8d7db-125">이 문서와 함께 다음</span><span class="sxs-lookup"><span data-stu-id="8d7db-125">Following along with this article</span></span>  

<span data-ttu-id="8d7db-126">이 문서에서는 전체 코드 샘플을 원하는 경우 과정을 따르려면 Visual Studio를 복사할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-126">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="8d7db-127">만드는 것이 **단위 테스트 프로젝트** 해야 대상 **.NET Framework 4.5** 비동기를 사용 하는 섹션을 완료 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-127">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="creating-a-context-interface"></a><span data-ttu-id="8d7db-128">상황에 맞는 인터페이스 만들기</span><span class="sxs-lookup"><span data-stu-id="8d7db-128">Creating a context interface</span></span>  

<span data-ttu-id="8d7db-129">여기서는 EF를 사용 하는 서비스를 테스트 확인을 모델입니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-129">We're going to look at testing a service that makes use of an EF model.</span></span> <span data-ttu-id="8d7db-130">테스트에 대 한 메모리 내 버전과 EF 컨텍스트를 교체 하기 위해 바로 EF 컨텍스트입니다 (및 메모리에 이중)에서 구현 하는 인터페이스를 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-130">In order to be able to replace our EF context with an in-memory version for testing, we'll define an interface that our EF context (and it's in-memory double) will implement.</span></span>

<span data-ttu-id="8d7db-131">테스트 하려는 서비스 쿼리는 및 우리의 컨텍스트의 DbSet 속성을 사용 하 여 데이터를 수정 하 고 또한 데이터베이스에 변경 내용을 푸시할 SaveChanges를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-131">The service we are going to test will query and modify data using the DbSet properties of our context and also call SaveChanges to push changes to the database.</span></span> <span data-ttu-id="8d7db-132">따라서 이러한 멤버는 인터페이스에 포함 되 고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-132">So we're including these members on the interface.</span></span>  

``` csharp
using System.Data.Entity;

namespace TestingDemo
{
    public interface IBloggingContext
    {
        DbSet<Blog> Blogs { get; }
        DbSet<Post> Posts { get; }
        int SaveChanges();
    }
}
```  

## <a name="the-ef-model"></a><span data-ttu-id="8d7db-133">EF 모델</span><span class="sxs-lookup"><span data-stu-id="8d7db-133">The EF model</span></span>  

<span data-ttu-id="8d7db-134">테스트 하려는 서비스 이용 EF는 BloggingContext 및 블로그 및 게시물 클래스를 구성 하는 모델입니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-134">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="8d7db-135">이 코드는 EF 디자이너에서 생성 될 수 있습니다 또는 Code First 모델입니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-135">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext, IBloggingContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a><span data-ttu-id="8d7db-136">EF 디자이너를 사용 하 여 상황에 맞는 인터페이스 구현</span><span class="sxs-lookup"><span data-stu-id="8d7db-136">Implementing the context interface with the EF Designer</span></span>  

<span data-ttu-id="8d7db-137">이 상황에 맞는 IBloggingContext 인터페이스를 구현 하는 참고 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-137">Note that our context implements the IBloggingContext interface.</span></span>  

<span data-ttu-id="8d7db-138">Code First 사용 하는 경우 인터페이스를 구현 하는 직접 컨텍스트를 편집할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-138">If you are using Code First then you can edit your context directly to implement the interface.</span></span> <span data-ttu-id="8d7db-139">EF 디자이너를 사용 하는 경우 컨텍스트를 생성 하는 T4 템플릿을 편집 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-139">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="8d7db-140">엽니다는 \<model_name\>합니다. Edmx 파일에서 중첩 된 Context.tt 파일을 다음 코드 조각은 찾고 표시 된 것 처럼 인터페이스에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-140">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the interface as shown.</span></span>  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a><span data-ttu-id="8d7db-141">서비스 테스트</span><span class="sxs-lookup"><span data-stu-id="8d7db-141">Service to be tested</span></span>  

<span data-ttu-id="8d7db-142">메모리 내 test double을 사용 하 여 테스트 보여 주기 위해는 BlogService에 대 한 몇 가지 테스트를 작성 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-142">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="8d7db-143">새 블로그 (AddBlog)를 만들 수 있는 서비스 이며 (GetAllBlogs) 이름으로 정렬 된 모든 블로그를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-143">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="8d7db-144">GetAllBlogs, 외에 (GetAllBlogsAsync) 이름순으로 정렬 하는 모든 블로그를 비동기적으로 가져오기 됩니다 하는 메서드를 제공 했습니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-144">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class BlogService
    {
        private IBloggingContext _context;

        public BlogService(IBloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = new Blog { Name = name, Url = url };
            _context.Blogs.Add(blog);
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

<span data-ttu-id="8d7db-145"><a name="creating-the-in-memory-test-doubles"/> # # 두 배로-메모리 내 테스트를 작성 하는 중</span><span class="sxs-lookup"><span data-stu-id="8d7db-145"><a name="creating-the-in-memory-test-doubles"/> ## Creating the in-memory test doubles</span></span>  

<span data-ttu-id="8d7db-146">이제 실제 EF 모델을 사용할 수 있는 서비스 한 테스트를 위해 사용할 수 있다는 double 메모리 내 테스트를 만들 차례입니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-146">Now that we have the real EF model and the service that can use it, it's time to create the in-memory test double that we can use for testing.</span></span> <span data-ttu-id="8d7db-147">용으로 만든 TestContext 테스트 double 바로 컨텍스트입니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-147">We've created a TestContext test double for our context.</span></span> <span data-ttu-id="8d7db-148">테스트를 지원 하기 위해 원하는 동작을 선택할을 test double에서 실행 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-148">In test doubles we get to choose the behavior we want in order to support the tests we are going to run.</span></span> <span data-ttu-id="8d7db-149">이 예제에서 방금 SaveChanges를 호출 하는 횟수 만큼을 캡처 중인지 있지만 테스트 중인 시나리오를 확인 하는 데 필요한 논리를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-149">In this example we're just capturing the number of times SaveChanges is called, but you can include whatever logic is needed to verify the scenario you are testing.</span></span>  

<span data-ttu-id="8d7db-150">메모리 내 구현의 DbSet 제공 하는 TestDbSet도 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-150">We've also created a TestDbSet that provides an in-memory implementation of DbSet.</span></span> <span data-ttu-id="8d7db-151">(찾기), 제외 DbSet에 모든 메서드에 대 한 전체 문서로 제공 하지만 테스트 시나리오에 사용할 멤버를 구현 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-151">We've provided a complete implemention for all the methods on DbSet (except for Find), but you only need to implement the members that your test scenario will use.</span></span>  

<span data-ttu-id="8d7db-152">TestDbSet에서는 비동기 쿼리를 처리할 수 있는지 확인 하려면 포함 된 몇 가지 다른 인프라 클래스를 활용 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-152">TestDbSet makes use of some other infrastructure classes that we've included to ensure that async queries can be processed.</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class TestContext : IBloggingContext
    {
        public TestContext()
        {
            this.Blogs = new TestDbSet<Blog>();
            this.Posts = new TestDbSet<Post>();
        }

        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
        public int SaveChangesCount { get; private set; }
        public int SaveChanges()
        {
            this.SaveChangesCount++;
            return 1;
        }
    }

    public class TestDbSet<TEntity> : DbSet<TEntity>, IQueryable, IEnumerable<TEntity>, IDbAsyncEnumerable<TEntity>
        where TEntity : class
    {
        ObservableCollection<TEntity> _data;
        IQueryable _query;

        public TestDbSet()
        {
            _data = new ObservableCollection<TEntity>();
            _query = _data.AsQueryable();
        }

        public override TEntity Add(TEntity item)
        {
            _data.Add(item);
            return item;
        }

        public override TEntity Remove(TEntity item)
        {
            _data.Remove(item);
            return item;
        }

        public override TEntity Attach(TEntity item)
        {
            _data.Add(item);
            return item;
        }

        public override TEntity Create()
        {
            return Activator.CreateInstance<TEntity>();
        }

        public override TDerivedEntity Create<TDerivedEntity>()
        {
            return Activator.CreateInstance<TDerivedEntity>();
        }

        public override ObservableCollection<TEntity> Local
        {
            get { return _data; }
        }

        Type IQueryable.ElementType
        {
            get { return _query.ElementType; }
        }

        Expression IQueryable.Expression
        {
            get { return _query.Expression; }
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<TEntity>(_query.Provider); }
        }

        System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
        {
            return _data.GetEnumerator();
        }

        IEnumerator<TEntity> IEnumerable<TEntity>.GetEnumerator()
        {
            return _data.GetEnumerator();
        }

        IDbAsyncEnumerator<TEntity> IDbAsyncEnumerable<TEntity>.GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<TEntity>(_data.GetEnumerator());
        }
    }

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

### <a name="implementing-find"></a><span data-ttu-id="8d7db-153">찾을 구현</span><span class="sxs-lookup"><span data-stu-id="8d7db-153">Implementing Find</span></span>  

<span data-ttu-id="8d7db-154">Find 메서드는 일반적으로 구현 하기가 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-154">The Find method is difficult to implement in a generic fashion.</span></span> <span data-ttu-id="8d7db-155">테스트 해야 하는 경우 각 지원 해야 하는 엔터티 형식에 대 한 DbSet 찾을 테스트를 만드는 것이 쉽습니다 Find 메서드는 코드를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-155">If you need to test code that makes use of the Find method it is easiest to create a test DbSet for each of the entity types that need to support find.</span></span> <span data-ttu-id="8d7db-156">그런 다음 아래와 같이 특정 형식의 엔터티를 검색할 논리를 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-156">You can then write logic to find that particular type of entity, as shown below.</span></span>  

``` csharp
using System.Linq;

namespace TestingDemo
{
    class TestBlogDbSet : TestDbSet<Blog>
    {
        public override Blog Find(params object[] keyValues)
        {
            var id = (int)keyValues.Single();
            return this.SingleOrDefault(b => b.BlogId == id);
        }
    }
}
```  

## <a name="writing-some-tests"></a><span data-ttu-id="8d7db-157">일부 테스트 작성</span><span class="sxs-lookup"><span data-stu-id="8d7db-157">Writing some tests</span></span>  

<span data-ttu-id="8d7db-158">테스트 하기 위해 수행 해야 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-158">That’s all we need to do to start testing.</span></span> <span data-ttu-id="8d7db-159">다음 테스트 TestContext 고이 컨텍스트를 기반으로 하는 서비스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-159">The following test creates a TestContext and then a service based on this context.</span></span> <span data-ttu-id="8d7db-160">서비스는 AddBlog 메서드를 사용 하 여 새 블로그 –를 만들려면 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-160">The service is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="8d7db-161">마지막으로 테스트 서비스 컨텍스트의 블로그 속성에 새 블로그를 추가 하 고 컨텍스트 SaveChanges 호출을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-161">Finally, the test verifies that the service added a new Blog to the context's Blogs property and called SaveChanges on the context.</span></span>  

<span data-ttu-id="8d7db-162">Double 메모리 내 테스트를 사용 하 여 테스트할 수 있습니다 작업 유형의 예로 든 것일 뿐 이며 test double 및 요구 사항에 맞게 확인 논리를 조정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-162">This is just an example of the types of things you can test with an in-memory test double and you can adjust the logic of the test doubles and the verification to meet your requirements.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var context = new TestContext();

            var service = new BlogService(context);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            Assert.AreEqual(1, context.Blogs.Count());
            Assert.AreEqual("ADO.NET Blog", context.Blogs.Single().Name);
            Assert.AreEqual("http://blogs.msdn.com/adonet", context.Blogs.Single().Url);
            Assert.AreEqual(1, context.SaveChangesCount);
        }
    }
}
```  

<span data-ttu-id="8d7db-163">쿼리를 수행 하는이 이번 테스트-의 또 다른 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-163">Here is another example of a test - this time one that performs a query.</span></span> <span data-ttu-id="8d7db-164">테스트는 해당 블로그 속성-데이터를 알파벳 순서로 정렬 되지 않습니다. 일부 데이터가 포함 된 테스트 컨텍스트를 만들어 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-164">The test starts by creating a test context with some data in its Blog property - note that the data is not in alphabetical order.</span></span> <span data-ttu-id="8d7db-165">테스트 컨텍스트를 기반으로 하는 BlogService을 만들고 이름별 GetAllBlogs에서 다시 얻게 데이터가 정렬 되는 확인 한 다음 우리가 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-165">We can then create a BlogService based on our test context and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var context = new TestContext();
            context.Blogs.Add(new Blog { Name = "BBB" });
            context.Blogs.Add(new Blog { Name = "ZZZ" });
            context.Blogs.Add(new Blog { Name = "AAA" });

            var service = new BlogService(context);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

<span data-ttu-id="8d7db-166">마지막으로, 비동기 인프라에 포함 되어 있는지 확인 하는 비동기 메서드를 사용 하는 하나 이상의 테스트 작성 [TestDbSet](#creating-the-in-memory-test-doubles) 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="8d7db-166">Finally, we'll write one more test that uses our async method to ensure that the async infrastructure we included in [TestDbSet](#creating-the-in-memory-test-doubles) is working.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Collections.Generic;
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
            var context = new TestContext();
            context.Blogs.Add(new Blog { Name = "BBB" });
            context.Blogs.Add(new Blog { Name = "ZZZ" });
            context.Blogs.Add(new Blog { Name = "AAA" });

            var service = new BlogService(context);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
