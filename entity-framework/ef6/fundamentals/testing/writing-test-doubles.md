---
title: 사용자 고유의 테스트를 사용 하 여 테스트 EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: 3d8933fb5e17f8c01f3971495a1fcdb5b8cfab57
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72446032"
---
# <a name="testing-with-your-own-test-doubles"></a>사용자 고유의 테스트를 사용 하 여 테스트 두 배
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

응용 프로그램에 대 한 테스트를 작성 하는 경우 데이터베이스에 대 한 적중을 방지 하는 것이 좋습니다.  Entity Framework를 사용 하면 테스트에 정의 된 동작을 사용 하 여 메모리 내 데이터를 사용 하는 컨텍스트를 만들어이를 달성할 수 있습니다.  

## <a name="options-for-creating-test-doubles"></a>테스트 double을 만드는 옵션  

컨텍스트의 메모리 내 버전을 만드는 데 사용할 수 있는 두 가지 방법이 있습니다.  

- **사용자 고유의 테스트 만들기** -이 방법은 사용자의 컨텍스트와 dbsets의 고유한 메모리 내 구현을 작성 하는 것을 포함 합니다. 이렇게 하면 클래스가 동작 하는 방식을 제어할 수 있을 뿐만 아니라 적절 한 양의 코드를 작성 하 고 소유 하는 것을 포함할 수 있습니다.  
- **모의 프레임 워크를 사용 하 여 테스트 double 만들기** – 모의 프레임 워크 (예: moq)를 사용 하 여 런타임의 메모리 내 구현 및 런타임에 동적으로 생성 된 집합을 사용할 수 있습니다.  

이 문서에서는 사용자 고유의 테스트를 만드는 과정을 다룹니다. 모의 프레임 워크를 사용 하는 방법에 대 한 자세한 내용은 [모의 프레임 워크로 테스트](mocking.md)를 참조 하세요.  

## <a name="testing-with-pre-ef6-versions"></a>사전 EF6 버전으로 테스트  

이 문서에 표시 된 코드는 EF6와 호환 됩니다. EF5 이전 버전을 사용한 테스트는 [가짜 컨텍스트로 테스트](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/)를 참조 하세요.  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>EF 메모리 내 테스트 double의 제한 사항  

메모리 내 테스트 double은 EF를 사용 하는 응용 프로그램의 비트 단위 테스트 수준 범위를 제공 하는 좋은 방법일 수 있습니다. 그러나이 작업을 수행 하는 경우 LINQ to Objects를 사용 하 여 메모리 내 데이터에 대해 쿼리를 실행 합니다. 이렇게 하면 EF의 LINQ 공급자 (LINQ to Entities)를 사용 하 여 데이터베이스에 대해 실행 되는 SQL로 쿼리를 변환 하는 것과 다른 동작이 발생할 수 있습니다.  

이러한 차이점의 한 가지 예는 관련 데이터를 로드 하는 것입니다. 각각 관련 게시물이 있는 일련의 블로그를 만드는 경우 메모리 내 데이터를 사용 하는 경우 관련 게시물이 항상 각 블로그에 대해 로드 됩니다. 그러나 데이터베이스에 대해 실행 하는 경우에는 Include 메서드를 사용 하는 경우에만 데이터가 로드 됩니다.  

따라서 응용 프로그램이 데이터베이스에 대해 올바르게 작동 하도록 하기 위해 항상 특정 수준의 종단 간 테스트 (단위 테스트 포함)를 포함 하는 것이 좋습니다.  

## <a name="following-along-with-this-article"></a>이 문서와 함께 다음을 수행 합니다.  

이 문서에서는 Visual Studio로 복사 하 여 원하는 경우 따라 진행할 수 있는 전체 코드 목록을 제공 합니다. **단위 테스트 프로젝트** 를 만드는 것이 가장 쉽지만 비동기를 사용 하는 섹션을 완료 하려면 **.NET Framework 4.5** 를 대상으로 해야 합니다.  

## <a name="creating-a-context-interface"></a>컨텍스트 인터페이스 만들기  

EF 모델을 사용 하는 서비스 테스트를 살펴보겠습니다. EF 컨텍스트를 테스트를 위해 메모리 내 버전으로 바꿀 수 있도록 EF 컨텍스트 (및 메모리 내 double)가 구현 하는 인터페이스를 정의 합니다.

테스트 하려는 서비스는 컨텍스트의 DbSet 속성을 사용 하 여 데이터를 쿼리 및 수정 하 고 SaveChanges를 호출 하 여 데이터베이스에 변경 내용을 푸시합니다. 이러한 멤버를 인터페이스에 포함 하 고 있습니다.  

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

## <a name="the-ef-model"></a>EF 모델  

테스트 하려는 서비스는 BloggingContext 및 블로그 및 게시물 클래스로 구성 된 EF 모델을 사용 합니다. 이 코드는 EF Designer에서 생성 되었거나 Code First 모델 일 수 있습니다.  

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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a>EF Designer를 사용 하 여 컨텍스트 인터페이스 구현  

이 컨텍스트는 IBloggingContext 인터페이스를 구현 합니다.  

Code First를 사용 하는 경우 컨텍스트를 직접 편집 하 여 인터페이스를 구현할 수 있습니다. EF Designer를 사용 하는 경우 컨텍스트를 생성 하는 T4 템플릿을 편집 해야 합니다. @No__t-0model_name @ no__t-1을 엽니다. Edmx 파일에 중첩 된 Context.tt 파일, 다음 코드 조각을 찾고 인터페이스에서 다음과 같이를 추가 합니다.  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a>테스트할 서비스  

메모리 내 테스트 double로 테스트 하는 방법을 보여 주기 위해 BlogService에 대 한 몇 가지 테스트를 작성할 예정입니다. 서비스는 새 블로그 (AddBlog)를 만들고 이름을 기준으로 정렬 된 모든 블로그 (GetAllBlogs)를 반환할 수 있습니다. GetAllBlogs 외에도 이름 (GetAllBlogsAsync)으로 정렬 된 모든 블로그를 비동기적으로 가져오는 메서드도 제공 했습니다.  

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

## <a name="creating-the-in-memory-test-doubles"></a>메모리 내 테스트 double 만들기  

이제 실제 EF 모델 및이를 사용할 수 있는 서비스가 있으므로 테스트에 사용할 수 있는 메모리 내 테스트 double을 만들 시간입니다. 컨텍스트에 대해 TestContext test double을 만들었습니다. 테스트의 두 배에서는 실행할 테스트를 지원 하기 위해 원하는 동작을 선택 합니다. 이 예제에서는 SaveChanges가 호출 되는 횟수를 캡처 하지만 테스트 중인 시나리오를 확인 하는 데 필요한 모든 논리를 포함할 수 있습니다.  

DbSet의 메모리 내 구현을 제공 하는 TestDbSet도 만들었습니다. DbSet의 모든 메서드에 대 한 완전 한 구현을 (찾기는 제외)를 제공 했지만 테스트 시나리오에서 사용할 멤버를 구현 하기만 하면 됩니다.  

TestDbSet은 비동기 쿼리를 처리할 수 있도록 포함 된 일부 다른 인프라 클래스를 사용 합니다.  

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

### <a name="implementing-find"></a>찾기 구현  

Find 메서드는 일반적인 방식으로 구현 하기 어렵습니다. Find 메서드를 사용 하는 코드를 테스트 해야 하는 경우 찾기를 지원 해야 하는 각 엔터티 형식에 대해 테스트 DbSet을 만드는 것이 가장 간편 합니다. 그런 다음 아래와 같이 특정 유형의 엔터티를 찾기 위한 논리를 작성할 수 있습니다.  

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

## <a name="writing-some-tests"></a>일부 테스트 작성  

테스트를 시작 하려면이 작업을 수행 해야 합니다. 다음 테스트는이 컨텍스트를 기반으로 하는 TestContext 및 서비스를 만듭니다. 그런 다음 AddBlog 메서드를 사용 하 여 새 블로그를 만드는 데 서비스를 사용 합니다. 마지막으로 테스트는 서비스에서 컨텍스트의 블로그 속성에 새 블로그를 추가 하 고 컨텍스트에서 SaveChanges를 호출 했는지 확인 합니다.  

이는 메모리 내 테스트 double로 테스트할 수 있는 작업 유형의 한 예이 고 테스트의 논리와 요구 사항에 맞게 확인 하는 것을 조정할 수 있습니다.  

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

다음은 쿼리를 수행 하는 테스트의 또 다른 예입니다. 테스트는 블로그 속성의 일부 데이터를 사용 하 여 테스트 컨텍스트를 만드는 것으로 시작 됩니다. 데이터는 사전순으로 정렬 되지 않습니다. 그런 다음 테스트 컨텍스트에 따라 BlogService를 만들고 GetAllBlogs 데이터를 이름별로 정렬 하 여 확인할 수 있습니다.  

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

마지막으로, 비동기 메서드를 사용 하 여 [Testdbset](#creating-the-in-memory-test-doubles) 에 포함 된 비동기 인프라가 작동 하는지 확인 하는 테스트를 하나 더 작성 합니다.  

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
