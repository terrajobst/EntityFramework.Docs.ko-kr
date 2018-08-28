---
title: 사용자 고유의 test double-EF6 사용 하 여 테스트
author: divega
ms.date: 2016-10-23
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: fa63c483e0a55b6cbd43382f68ab9592f3c1768b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997219"
---
# <a name="testing-with-your-own-test-doubles"></a>사용자 고유의 test double을 사용 하 여 테스트
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

응용 프로그램에 대 한 테스트를 작성 하는 경우 데이터베이스 도달 하지 않도록 하는 것이 좋습니다.  Entity Framework를 사용 하 여 메모리 내 데이터 컨텍스트를 만들어 – – 테스트를 통해 정의 되는 동작을 사용 하 여이 작업을 수행할 수 있습니다.  

## <a name="options-for-creating-test-doubles"></a>Test double을 만들기 위한 옵션  

메모리 내 버전이 컨텍스트를 만드는 데 사용할 수 있는 두 가지 방법이 있습니다.  

- **사용자 고유의 test double을 만드는** -이 방법은 사용자의 컨텍스트 및 Dbset 고유한 메모리 내 구현의 작성 하는 것입니다. 이 클래스의 동작 하지만 작성 하 고 적절 한 양의 코드를 소유 하는 포함 될 수 있습니다 하는 방법에 대 한 제어 많이 제공 합니다.  
- **모의 프레임 워크를 사용 하 여 test double을 만들려면** -(예: Moq) 모의 프레임 워크를 사용 하 여 할 수 있습니다의 메모리 내 구현을 컨텍스트와를 런타임에 동적으로 만든 집합입니다.  

이 문서에서는 고유한 테스트를 이중 만들기를 처리 합니다. 모의 프레임 워크를 사용 하는 방법은 참조 하세요. [모의 프레임 워크를 사용 하 여 테스트](mocking.md)합니다.  

## <a name="testing-with-pre-ef6-versions"></a>EF6 이전 버전을 사용 하 여 테스트  

이 문서에 나와 있는 코드는 EF6 호환 됩니다. EF5 및 이전 버전을 사용 하 여 테스트에 대 한 참조 [가짜 컨텍스트를 사용 하 여 테스트](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/)합니다.  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>EF에서 메모리 test double의 제한 사항  

메모리 내 test double은 단위 테스트 응용 프로그램의 EF를 사용 하는 비트 수준 검사를 제공 하는 좋은 방법 수 있습니다. 그러나 이렇게 하면 사용 하는 LINQ to Objects 메모리 내 데이터에 대 한 쿼리를 실행 합니다. 이 데이터베이스에 대해 실행 되는 SQL 쿼리를 변환할 EF의 LINQ 공급자 (LINQ to Entities)를 사용 하 여 보다 다양 한 동작이 발생할 수 있습니다.  

이러한 차이의 예로 관련된 데이터를 로드 합니다. 블로그 시리즈를 만들면 관련 게시물 각 다음 각 블로그에 대해 항상 관련된 게시물을 로드는 메모리 내 데이터를 사용 하는 경우. 그러나 데이터베이스에 대해 실행 하는 경우 이러한 데이터는 Include 메서드를 사용 하는 경우 로드만 됩니다.  

이러한 이유로 항상 일정 수준의 엔드-투-엔드 데이터베이스에 대해 올바르게 응용 프로그램이 작동 되도록 (단위 테스트) 외에도 테스트를 포함 하는 것이 좋습니다.  

## <a name="following-along-with-this-article"></a>이 문서와 함께 다음  

이 문서에서는 전체 코드 샘플을 원하는 경우 과정을 따르려면 Visual Studio를 복사할 수 있습니다. 만드는 것이 **단위 테스트 프로젝트** 해야 대상 **.NET Framework 4.5** 비동기를 사용 하는 섹션을 완료 합니다.  

## <a name="creating-a-context-interface"></a>상황에 맞는 인터페이스 만들기  

여기서는 EF를 사용 하는 서비스를 테스트 확인을 모델입니다. 테스트에 대 한 메모리 내 버전과 EF 컨텍스트를 교체 하기 위해 인터페이스 정의 바로 EF 컨텍스트입니다 (및 메모리에 이중) imeplement를 합니다.  

테스트 하려는 서비스 쿼리는 및 우리의 컨텍스트의 DbSet 속성을 사용 하 여 데이터를 수정 하 고 또한 데이터베이스에 변경 내용을 푸시할 SaveChanges를 호출 합니다. 따라서 이러한 멤버는 인터페이스에 포함 되 고 있습니다.  

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

테스트 하려는 서비스 이용 EF는 BloggingContext 및 블로그 및 게시물 클래스를 구성 하는 모델입니다. 이 코드는 EF 디자이너에서 생성 될 수 있습니다 또는 Code First 모델입니다.  

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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a>EF 디자이너를 사용 하 여 상황에 맞는 인터페이스 구현  

이 상황에 맞는 IBloggingContext 인터페이스를 구현 하는 참고 합니다.  

Code First 사용 하는 경우 인터페이스를 구현 하는 직접 컨텍스트를 편집할 수 있습니다. EF 디자이너를 사용 하는 경우 컨텍스트를 생성 하는 T4 템플릿을 편집 해야 합니다. 엽니다는 \<model_name\>합니다. Edmx 파일에서 중첩 된 Context.tt 파일을 다음 코드 조각은 찾고 표시 된 것 처럼 인터페이스에 추가 합니다.  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a>서비스 테스트  

메모리 내 test double을 사용 하 여 테스트 보여 주기 위해는 BlogService에 대 한 몇 가지 테스트를 작성 하려고 합니다. 새 블로그 (AddBlog)를 만들 수 있는 서비스 이며 (GetAllBlogs) 이름으로 정렬 된 모든 블로그를 반환 합니다. GetAllBlogs, 외에 (GetAllBlogsAsync) 이름순으로 정렬 하는 모든 블로그를 비동기적으로 가져오기 됩니다 하는 메서드를 제공 했습니다.  

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

<a name="creating-the-in-memory-test-doubles"/> # # 두 배로-메모리 내 테스트를 작성 하는 중  

이제 실제 EF 모델을 사용할 수 있는 서비스 한 테스트를 위해 사용할 수 있다는 double 메모리 내 테스트를 만들 차례입니다. 용으로 만든 TestContext 테스트 double 바로 컨텍스트입니다. 테스트를 지원 하기 위해 원하는 동작을 선택할을 test double에서 실행 하려고 합니다. 이 예제에서 방금 SaveChanges를 호출 하는 횟수 만큼을 캡처 중인지 있지만 테스트 중인 시나리오를 확인 하는 데 필요한 논리를 포함할 수 있습니다.  

메모리 내 구현의 DbSet 제공 하는 TestDbSet도 만들었습니다. (찾기), 제외 DbSet에 모든 메서드에 대 한 전체 문서로 제공 하지만 테스트 시나리오에 사용할 멤버를 구현 해야 합니다.  

TestDbSet에서는 비동기 쿼리를 처리할 수 있는지 확인 하려면 포함 된 몇 가지 다른 인프라 클래스를 활용 합니다.  

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

### <a name="implementing-find"></a>찾을 구현  

Find 메서드는 일반적으로 구현 하기가 어렵습니다. 테스트 해야 하는 경우 각 지원 해야 하는 엔터티 형식에 대 한 DbSet 찾을 테스트를 만드는 것이 쉽습니다 Find 메서드는 코드를 사용 합니다. 그런 다음 아래와 같이 특정 형식의 엔터티를 검색할 논리를 작성할 수 있습니다.  

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

테스트 하기 위해 수행 해야 됩니다. 다음 테스트 TestContext 고이 컨텍스트를 기반으로 하는 서비스를 만듭니다. 서비스는 AddBlog 메서드를 사용 하 여 새 블로그 –를 만들려면 사용 됩니다. 마지막으로 테스트 서비스 컨텍스트의 블로그 속성에 새 블로그를 추가 하 고 컨텍스트 SaveChanges 호출을 확인 합니다.  

Double 메모리 내 테스트를 사용 하 여 테스트할 수 있습니다 작업 유형의 예로 든 것일 뿐 이며 test double 및 요구 사항에 맞게 확인 논리를 조정할 수 있습니다.  

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

쿼리를 수행 하는이 이번 테스트-의 또 다른 예는 다음과 같습니다. 테스트는 해당 블로그 속성-데이터를 알파벳 순서로 정렬 되지 않습니다. 일부 데이터가 포함 된 테스트 컨텍스트를 만들어 시작 합니다. 테스트 컨텍스트를 기반으로 하는 BlogService을 만들고 이름별 GetAllBlogs에서 다시 얻게 데이터가 정렬 되는 확인 한 다음 우리가 합니다.  

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

마지막으로, 비동기 인프라에 포함 되어 있는지 확인 하는 비동기 메서드를 사용 하는 하나 이상의 테스트 작성 [TestDbSet](#creating-the-in-memory-test-doubles) 작동 합니다.  

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
