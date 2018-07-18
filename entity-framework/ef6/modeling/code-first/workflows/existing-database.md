---
title: 기존 데이터베이스-EF6에 code First
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
caps.latest.revision: 3
ms.openlocfilehash: 47581a7ae9ff534d26ce82365bcbe2b254247495
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122290"
---
# <a name="code-first-to-an-existing-database"></a>기존 데이터베이스에 대 한 code First
이 비디오 및 단계별 연습에서는 기존 데이터베이스를 대상으로 하는 Code First 개발에 대 한 소개를 제공 합니다. C를 사용 하 여 모델을 정의 하는 코드 먼저 허용\# 또는 VB.Net 클래스입니다. 클래스 및 속성 또는 fluent API를 사용 하 여 특성을 사용 하 여 필요에 따라 추가 구성을 수행할 수 있습니다.

## <a name="watch-the-video"></a>비디오를 시청 하세요.
이 비디오 [Channel 9의 출시](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-)합니다.

## <a name="pre-requisites"></a>필수 조건

해야 합니다 **Visual Studio 2012** 하거나 **Visual Studio 2013** 이 연습을 완료 하려면 설치 합니다.

버전도 해야 **6.1** (또는 이상)의 합니다 **Entity Framework Tools for Visual Studio** 설치 합니다. 참조 [Entity Framework 가져오기](~/ef6/fundamentals/install.md) Entity Framework Tools의 최신 버전 설치에 대 한 정보에 대 한 합니다.

## <a name="1-create-an-existing-database"></a>1. 기존 데이터베이스를 만들려면

일반적으로 이미 만들어집니다, 기존 데이터베이스를 대상으로 할 때 있지만이 연습에 액세스 하려면 데이터베이스를 만들려고 해야 합니다.

데이터베이스를 생성 해 보겠습니다.

-   Visual Studio를 엽니다.
-   **보기-&gt; 서버 탐색기**
-   마우스 오른쪽 단추로 클릭 **데이터 연결-&gt; 연결 추가 중...**
-   데이터베이스에 연결 하지 않았으면 **서버 탐색기** 를 선택 해야 하기 전에 **Microsoft SQL Server** 데이터 원본으로

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   LocalDB 인스턴스에 연결 하 고 입력 **블로깅** 데이터베이스 이름으로

    ![LocalDBConnection](~/ef6/media/localdbconnection.png)

-   선택 **확인** 를 묻는 새 데이터베이스를 만들려는 경우 **예**

    ![CreateDatabaseDialog](~/ef6/media/createdatabasedialog.png)

-   새 데이터베이스 이제 서버 탐색기에서 마우스 나타나고 선택 **새 쿼리**
-   새 쿼리를 다음과 같은 SQL 복사 후 선택한 쿼리를 마우스 오른쪽 단추로 클릭 **실행**

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a>2. 응용 프로그램 만들기

간단 하 게 데이터 액세스를 수행 하려면 Code First를 사용 하는 기본적인 콘솔 응용 프로그램을 빌드 하겠습니다.

-   Visual Studio를 엽니다.
-   **파일만&gt; 새로운 기능-&gt; 프로젝트...**
-   선택 **Windows** 왼쪽된 메뉴에서 및 **콘솔 응용 프로그램**
-   입력 **CodeFirstExistingDatabaseSample** 이름으로
-   **확인**을 선택합니다.

 

## <a name="3-reverse-engineer-model"></a>3. 모델 리버스 엔지니어링

사용 하 여 Entity Framework Tools for Visual Studio를 데이터베이스에 매핑하기 위해 일부 초기 코드를 생성 하는 데 도움이 되도록 하겠습니다. 이러한 도구 원하는 경우 손으로 입력도 수 하는 코드를 생성 됩니다.

-   **프로젝트-&gt; 새 항목 추가...**
-   선택 **데이터** 왼쪽된 메뉴에서 차례로 **ADO.NET 엔터티 데이터 모델**
-   입력 **BloggingContext** 이름과 클릭 **확인**
-   그러면는 **엔터티 데이터 모델 마법사**
-   선택 **데이터베이스에서 Code First** 를 클릭 하 고 **다음**

    ![WizardOneCFE](~/ef6/media/wizardonecfe.png)

-   첫 번째 섹션에서 만든 데이터베이스에 연결을 선택 하 고 클릭 **다음**

    ![WizardTwoCFE](~/ef6/media/wizardtwocfe.png)

-   옆에 있는 확인란을 클릭 **테이블** 모든 테이블을 가져오고 클릭 **마침**

    ![WizardThreeCFE](~/ef6/media/wizardthreecfe.png)

리버스 엔지니어링 프로세스에는 항목 수가 완료 되 면를 추가한 프로젝트에 보겠습니다 추가 된 기능을 확인해 보세요.

### <a name="configuration-file"></a>구성 파일

App.config 파일을 프로젝트에 추가 되었습니다 기존 데이터베이스에 연결 문자열을 포함 하는이 파일.

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

*구성 파일에서 같은 다른 설정과 너무 보면, 이러한 기본 설정이 EF Code First 데이터베이스를 만들 위치를 알려 주는 합니다. 응용 프로그램에서 이러한 설정은 무시 됩니다 기존 데이터베이스에 매핑하는 것 때문입니다.*

### <a name="derived-context"></a>파생된 컨텍스트

A **BloggingContext** 클래스가 프로젝트에 추가 되었습니다. 컨텍스트를 쿼리하고 데이터를 저장할 수 있어 데이터베이스와 세션을 나타냅니다.
컨텍스트에 노출 된 **DbSet&lt;TEntity&gt;**  모델의 각 형식에 대 한 합니다. 기본 생성자를 사용 하 여 기본 생성자를 호출 하는 또한 보면 합니다 **이름 =** 구문입니다. 그러면 Code First는이 컨텍스트에서 사용 하도록 연결 문자열 구성 파일에서 로드 되어야 합니다.

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

*항상 사용 해야 합니다 **이름 =** config 파일에서 연결 문자열을 사용 하는 경우에 구문이 있습니다. 연결 문자열에 없는 경우 다음 Entity Framework는 throw 하는 것이 이렇게 규칙에 따라 새 데이터베이스를 만드는 대신 합니다.*

### <a name="model-classes"></a>모델 클래스

마지막으로 **블로그** 하 고 **Post** 클래스 프로젝트를 추가 했습니다. 이들은 모델을 구성 하는 도메인 클래스입니다. Code First 규칙을 기존 데이터베이스의 구조와 일치 하지는 구성을 지정 하는 클래스에 적용할 데이터 주석은 볼 수 있습니다. 표시 예를 들어 합니다 **StringLength** 주석이 **Blog.Name** 및 **Blog.Url** 최대 길이가 있기 때문 **200** 에 데이터베이스 (데이터베이스 공급자-지 원하는 최대 길이 사용 하는 Code First이 기본값인 **nvarchar (max)** SQL Server에서).

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a>4. 읽기 및 데이터 쓰기

이제 모델이 준비 일부 데이터 액세스를 사용 하는 시간입니다. 구현 된 **Main** 에서 메서드 **Program.cs** 아래와 같이 합니다. 이 코드는 컨텍스트의 새 인스턴스를 만듭니다 및 다음 새로 삽입을 사용 하 여 **블로그**합니다. LINQ 쿼리를 사용 하 여 모든 검색할 **블로그** 기준으로 사전순으로 정렬 하는 데이터베이스에서 **Title**합니다.

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

이제 응용 프로그램을 실행 하 고 테스트할 수 있습니다.

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a>데이터베이스의 변경 될까요?

Code First 데이터베이스 마법사를 조정 하 고 수정할 수 있는 클래스의 시작 지점 집합을 생성 하도록 설계 되었습니다. 데이터베이스 스키마를 변경 하는 경우 수동으로 클래스를 편집 하거나 클래스를 덮어쓰려면 다른 리버스 엔지니어링을 수행 합니다.

## <a name="using-code-first-migrations-to-an-existing-database"></a>기존 데이터베이스에 Code First 마이그레이션 사용

기존 데이터베이스를 사용 하 여 Code First 마이그레이션을 사용 하려는 경우 참조 [기존 데이터베이스에 Code First 마이그레이션을](~/ef6/modeling/code-first/migrations/existing-database.md)합니다.

## <a name="summary"></a>요약

이 연습에서는 기존 데이터베이스를 사용 하 여 Code First 개발에 살펴보았습니다. 리버스 엔지니어링할 데이터베이스에 매핑되고 데이터 저장 및 검색 하기 위해 사용할 수 있는 클래스 집합을 Entity Framework Tools for Visual Studio를 사용 했습니다.
