---
title: 모의 프레임 워크-EF6 사용 하 여 테스트
author: divega
ms.date: 10/23/2016
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: 20799b55b2dffe27637c4fb84df06cee174e6dd9
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284098"
---
# <a name="testing-with-a-mocking-framework"></a>모의 프레임 워크를 사용 하 여 테스트
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

응용 프로그램에 대 한 테스트를 작성 하는 경우 데이터베이스 도달 하지 않도록 하는 것이 좋습니다.  Entity Framework를 사용 하 여 메모리 내 데이터 컨텍스트를 만들어 – – 테스트를 통해 정의 되는 동작을 사용 하 여이 작업을 수행할 수 있습니다.  

## <a name="options-for-creating-test-doubles"></a>Test double을 만들기 위한 옵션  

메모리 내 버전이 컨텍스트를 만드는 데 사용할 수 있는 두 가지 방법이 있습니다.  

- **사용자 고유의 test double을 만드는** -이 방법은 사용자의 컨텍스트 및 Dbset 고유한 메모리 내 구현의 작성 하는 것입니다. 이 클래스의 동작 하지만 작성 하 고 적절 한 양의 코드를 소유 하는 포함 될 수 있습니다 하는 방법에 대 한 제어 많이 제공 합니다.  
- **모의 프레임 워크를 사용 하 여 test double을 만들려면** -(예: Moq) 모의 프레임 워크를 사용 하 여 할 수 있습니다의 메모리 내 구현을 컨텍스트와를 런타임에 동적으로 만든 집합입니다.  

이 문서에서 모의 프레임 워크를 사용 하 여 처리 합니다. 사용자 고유의 test double을 만드는 참조 [사용자 고유의 테스트 Double을 사용 하 여 테스트](writing-test-doubles.md)합니다.  

모의 프레임 워크를 사용 하 여 EF를 사용 하 여 보여 주기 위해 Moq를 사용 하려고 합니다. Moq를 얻을 수 있는 가장 쉬운 방법은 설치 하는 것은 [NuGet에서 패키지를 Moq](http://nuget.org/packages/Moq/)합니다.  

## <a name="testing-with-pre-ef6-versions"></a>EF6 이전 버전을 사용 하 여 테스트  

이 문서에 나와 있는 시나리오 EF6에서 DbSet을 변경 했습니다에 종속 됩니다. EF5 및 이전 버전을 사용 하 여 테스트에 대 한 참조 [가짜 컨텍스트를 사용 하 여 테스트](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/)합니다.  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>EF에서 메모리 test double의 제한 사항  

메모리 내 test double은 단위 테스트 응용 프로그램의 EF를 사용 하는 비트 수준 검사를 제공 하는 좋은 방법 수 있습니다. 그러나 이렇게 하면 사용 하는 LINQ to Objects 메모리 내 데이터에 대 한 쿼리를 실행 합니다. 이 데이터베이스에 대해 실행 되는 SQL 쿼리를 변환할 EF의 LINQ 공급자 (LINQ to Entities)를 사용 하 여 보다 다양 한 동작이 발생할 수 있습니다.  

이러한 차이의 예로 관련된 데이터를 로드 합니다. 블로그 시리즈를 만들면 관련 게시물 각 다음 각 블로그에 대해 항상 관련된 게시물을 로드는 메모리 내 데이터를 사용 하는 경우. 그러나 데이터베이스에 대해 실행 하는 경우 이러한 데이터는 Include 메서드를 사용 하는 경우 로드만 됩니다.  

이러한 이유로 항상 일정 수준의 엔드-투-엔드 데이터베이스에 대해 올바르게 응용 프로그램이 작동 되도록 (단위 테스트) 외에도 테스트를 포함 하는 것이 좋습니다.  

## <a name="following-along-with-this-article"></a>이 문서와 함께 다음  

이 문서에서는 전체 코드 샘플을 원하는 경우 과정을 따르려면 Visual Studio를 복사할 수 있습니다. 만드는 것이 **단위 테스트 프로젝트** 해야 대상 **.NET Framework 4.5** 비동기를 사용 하는 섹션을 완료 합니다.  

## <a name="the-ef-model"></a>EF 모델  

테스트 하려는 서비스 이용 EF는 BloggingContext 및 블로그 및 게시물 클래스를 구성 하는 모델입니다. 이 코드는 EF 디자이너에서 생성 될 수 있습니다 또는 Code First 모델입니다.  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a>EF 디자이너를 사용 하 여 가상 DbSet 속성  

Note 컨텍스트의 DbSet 속성을 가상으로 표시 됩니다. 이렇게 하면 모의 프레임 워크에는 컨텍스트 및 모의 구현 사용 하 여 이러한 속성을 재정의에서 파생 됩니다.  

사용할 경우 Code First 다음 클래스를 직접 편집할 수 있습니다. EF 디자이너를 사용 하는 경우 컨텍스트를 생성 하는 T4 템플릿을 편집 해야 합니다. 엽니다는 \<model_name\>합니다. Edmx 파일에서 중첩 된 Context.tt 파일은 다음 코드 조각은 찾고 표시 된 것 처럼 virtual 키워드에 추가 합니다.  

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

## <a name="testing-non-query-scenarios"></a>쿼리가 아닌 시나리오를 테스트합니다.  

쿼리가 아닌 메서드를 테스트 하기 위해 수행 해야 됩니다. 다음 테스트 Moq를 사용 하 여 컨텍스트를 만듭니다. 그런 다음 DbSet을 만듭니다\<블로그\> 컨텍스트의 블로그 속성에서 반환 될 연결 하 고 있습니다. 다음으로, 컨텍스트 AddBlog 메서드를 사용 하 여 새 블로그 –를 만들려면 다음 사용 되는 새 BlogService 만드는 사용 됩니다. 마지막으로 테스트 서비스 새 블로그를 추가 하 고 컨텍스트 SaveChanges 호출을 확인 합니다.  

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

## <a name="testing-query-scenarios"></a>쿼리 시나리오를 테스트합니다.  

Double DbSet 테스트에 대 한 쿼리를 실행할 수 있도록 구현의 IQueryable 설치 해야 합니다. 첫 번째 단계에서 일부 메모리 내 데이터를 만들 때 – 목록을 사용 하는 것\<블로그\>합니다. 다음으로 만들겠습니다. 컨텍스트 및 DBSet\<블로그\> 한 다음 linq to Objects 공급자 목록을 사용 하 여 작동 하는 위임만 하는 DbSet – IQueryable 구현 연결\<T\>합니다.  

이 test double에 기반을 BlogService을 만들고 이름별 GetAllBlogs에서 다시 얻게 데이터가 정렬 되는 확인 한 다음에서는 합니다.  

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
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(0 => data.GetEnumerator());

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

### <a name="testing-with-async-queries"></a>비동기 쿼리를 사용 하 여 테스트

Entity Framework 6에는 비동기적으로 쿼리를 실행 하는 데 사용할 수 있는 확장 메서드 집합을 도입 했습니다. 이러한 메서드의 예로 ToListAsync, FirstAsync, ForEachAsync 등이 있습니다.  

Entity Framework 쿼리에서 LINQ를 사용 하므로, 확장 메서드는 IQueryable 및 IEnumerable에 정의 됩니다. 그러나는 Entity Framework와 함께 사용할만 디자인 되어 있으므로 LINQ 쿼리는 Entity Framework 쿼리 없는에서 크레딧을 사용 하려고 하면 다음 오류가 나타날 수 있습니다.

> 소스 IQueryable IDbAsyncEnumerable를 구현 하지 않는{0}합니다. 비동기 작업의 Entity Framework IDbAsyncEnumerable를 구현 하는 원본만 사용할 수 있습니다. 자세한 내용은 참조 하세요 [ http://go.microsoft.com/fwlink/?LinkId=287068 ](https://go.microsoft.com/fwlink/?LinkId=287068)합니다.  

비동기 메서드는 EF 쿼리를 실행 하는 경우에 지원 됩니다을 하는 동안에 메모리 내에 대해 실행 되는 DbSet의 double을 테스트할 때 단위 테스트에서 사용 하는 것이 좋습니다.  

비동기 메서드를 사용 하기 위해 비동기 쿼리를 처리 하는 메모리 내 DbAsyncQueryProvider 생성 해야 합니다. Moq를 사용 하 여 쿼리 공급자를 설치 하 수, 하는 동안 코드의 테스트 double 구현을 만들려면 훨씬 쉽습니다. 이 구현에 대 한 코드는 다음과 같습니다.  

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

비동기 쿼리 공급자를 만들었으므로 새 GetAllBlogsAsync 메서드에 대 한 단위 테스트를 작성할 수 했습니다.  

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
