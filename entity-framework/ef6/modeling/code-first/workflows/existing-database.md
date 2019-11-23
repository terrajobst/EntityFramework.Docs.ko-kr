---
title: 기존 데이터베이스에 Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: 61980bbd1f236f496a9d4fd92aa52264f1454615
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182625"
---
# <a name="code-first-to-an-existing-database"></a>기존 데이터베이스로 Code First
이 비디오 및 단계별 연습은 기존 데이터베이스를 대상으로 하는 Code First 개발에 대 한 소개를 제공 합니다. Code First를 사용 하면 C\# 또는 VB.Net 클래스를 사용 하 여 모델을 정의할 수 있습니다. 필요에 따라 클래스와 속성 또는 흐름 API를 사용 하 여 특성을 사용 하 여 추가 구성을 수행할 수 있습니다.

## <a name="watch-the-video"></a>비디오 시청
이 비디오는 [이제 Channel 9에서 사용할 수](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-)있습니다.

## <a name="pre-requisites"></a>필수 구성 요소

이 연습을 완료 하려면 **Visual Studio 2012** 또는 **Visual Studio 2013** 이 설치 되어 있어야 합니다.

**Visual Studio 용 Entity Framework Tools** 버전 **6.1** (또는 이상)이 설치 되어 있어야 합니다. 최신 버전의 Entity Framework Tools를 설치 하는 방법에 대 한 자세한 내용은 [Entity Framework 가져오기](~/ef6/fundamentals/install.md) 를 참조 하세요.

## <a name="1-create-an-existing-database"></a>1. 기존 데이터베이스를 만듭니다.

일반적으로 기존 데이터베이스를 대상으로 하는 경우에는 이미 생성 되지만이 연습에서는 액세스할 데이터베이스를 만들어야 합니다.

계속 해 서 데이터베이스를 생성 해 보겠습니다.

-   Visual Studio를 엽니다.
-   **뷰&gt; 서버 탐색기**
-   데이터 연결을 마우스 오른쪽 단추로 클릭 하 **&gt; 연결 추가** ...를 클릭 합니다.
-   **서버 탐색기** 데이터베이스에 연결 하지 않은 경우 **Microsoft SQL Server** 를 데이터 원본으로 선택 해야 합니다.

    ![데이터 원본 선택](~/ef6/media/selectdatasource.png)

-   LocalDB 인스턴스에 연결 하 고 데이터베이스 이름으로 **블로그** 를 입력 합니다.

    ![LocalDB 연결](~/ef6/media/localdbconnection.png)

-   **확인** 을 선택 하 고 새 데이터베이스를 만들지 여부를 묻는 메시지가 표시 되 면 **예** 를 선택 합니다.

    ![데이터베이스 만들기 대화 상자](~/ef6/media/createdatabasedialog.png)

-   이제 새 데이터베이스가 서버 탐색기에 표시 되 면 마우스 오른쪽 단추로 클릭 하 고 **새 쿼리** 를 선택 합니다.
-   다음 SQL을 새 쿼리에 복사한 다음 쿼리를 마우스 오른쪽 단추로 클릭 하 고 **실행** 을 선택 합니다.

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

간단 하 게 유지 하기 위해 Code First를 사용 하 여 데이터 액세스를 수행 하는 기본 콘솔 응용 프로그램을 빌드 하겠습니다.

-   Visual Studio를 엽니다.
-   **파일&gt; 새&gt; 프로젝트 ...**
-   왼쪽 메뉴 및 **콘솔 응용 프로그램** 에서 **Windows** 를 선택 합니다.
-   이름으로 **CodeFirstExistingDatabaseSample** 을 입력 합니다.
-   **확인**을 선택합니다.

 

## <a name="3-reverse-engineer-model"></a>3. 리버스 엔지니어링 모델

Visual Studio에 대 한 Entity Framework Tools를 사용 하 여 데이터베이스에 매핑하는 초기 코드를 생성 하는 데 도움을 드리겠습니다. 이러한 도구는 원하는 경우 직접 입력할 수도 있는 코드를 생성 하는 것입니다.

-   **프로젝트-새 항목 추가&gt; ...**
-   왼쪽 메뉴에서 **데이터** 를 선택 하 고 **ADO.NET** 를 선택 엔터티 데이터 모델
-   이름으로 **BloggingContext** 를 입력 하 고 **확인을** 클릭 합니다.
-   그러면 **엔터티 데이터 모델 마법사** 가 시작 됩니다.
-   **데이터베이스에서 Code First** 를 선택 하 고 **다음** 을 클릭 합니다.

    ![마법사 One CFE](~/ef6/media/wizardonecfe.png)

-   첫 번째 섹션에서 만든 데이터베이스에 대 한 연결을 선택 하 고 **다음** 을 클릭 합니다.

    ![마법사 2 CFE](~/ef6/media/wizardtwocfe.png)

-   테이블 옆의 확인란을 **클릭 하 여** 모든 테이블을 가져오고 **마침** 을 클릭 합니다.

    ![마법사 3 CFE](~/ef6/media/wizardthreecfe.png)

리버스 엔지니어링 프로세스가 완료 되 면 프로젝트에 많은 항목이 추가 된 후 추가 된 항목을 살펴보겠습니다.

### <a name="configuration-file"></a>구성 파일

App.config 파일이 프로젝트에 추가 되었습니다 .이 파일에는 기존 데이터베이스에 대 한 연결 문자열이 포함 되어 있습니다.

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

*구성 파일에서 일부 다른 설정도 확인할 수 있습니다. 이러한 설정에는 데이터베이스를 만들 위치 Code First를 알려 주는 기본 EF 설정이 있습니다. 기존 데이터베이스에 매핑하기 때문에 응용 프로그램에서 이러한 설정이 무시 됩니다.*

### <a name="derived-context"></a>파생 컨텍스트

**BloggingContext** 클래스가 프로젝트에 추가 되었습니다. 컨텍스트는 데이터베이스와의 세션을 나타내며,이를 통해 데이터를 쿼리하고 저장할 수 있습니다.
컨텍스트는 모델의 각 형식에 대 한 **Dbset&lt;&gt;** 를 노출 합니다. 기본 생성자가 **name =** 구문을 사용 하 여 기본 생성자를 호출 하는 것도 확인할 수 있습니다. 이는이 컨텍스트에 사용할 연결 문자열을 구성 파일에서 로드 해야 함을 Code First에 알려 줍니다.

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

*구성 파일에서 연결 문자열을 사용 하는 경우 항상 **name =** 구문을 사용 해야 합니다. 이렇게 하면 연결 문자열이 없는 경우 규칙에 따라 새 데이터베이스를 만드는 대신 Entity Framework가 throw 됩니다.*

### <a name="model-classes"></a>모델 클래스

마지막으로, **블로그** 및 **게시물** 클래스도 프로젝트에 추가 되었습니다. 모델을 구성 하는 도메인 클래스입니다. Code First 규칙이 기존 데이터베이스의 구조와 맞지 않는 구성을 지정 하기 위해 클래스에 적용 되는 데이터 주석을 볼 수 있습니다. 예를 들어 **Blog.Name** 및 최대에 대 한 **stringlength** 주석은 데이터베이스에서 최대 길이가 **200** 이기 때문에 표시 됩니다 **.** 기본값은 데이터베이스 공급자가 지 원하는 길이 Code First (SQL Server의 경우 **nvarchar (max)** )를 사용 하는 것입니다.

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

## <a name="4-reading--writing-data"></a>4. 데이터 쓰기 & 읽기

이제 모델을 사용 하 여 일부 데이터에 액세스 하는 데 사용할 수 있습니다. 아래와 같이 **Program.cs** 에서 **Main** 메서드를 구현 합니다. 이 코드는 컨텍스트의 새 인스턴스를 만든 다음이를 사용 하 여 새 **블로그**를 삽입 합니다. 그런 다음 LINQ 쿼리를 사용 하 여 데이터베이스에서 **제목별**로 사전순으로 정렬 된 모든 **블로그** 를 검색 합니다.

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

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a>데이터베이스가 변경 되 면 어떻게 되나요?

데이터베이스에 Code First 마법사는 조정 하 고 수정할 수 있는 클래스의 시작 지점 집합을 생성 하도록 디자인 되었습니다. 데이터베이스 스키마가 변경 되 면 클래스를 수동으로 편집 하거나 다른 역방향 엔지니어를 수행 하 여 클래스를 덮어쓸 수 있습니다.

## <a name="using-code-first-migrations-to-an-existing-database"></a>기존 데이터베이스에 Code First 마이그레이션 사용

기존 데이터베이스에서 Code First 마이그레이션를 사용 하려면 [기존 데이터베이스에 Code First 마이그레이션을](~/ef6/modeling/code-first/migrations/existing-database.md)참조 하세요.

## <a name="summary"></a>요약

이 연습에서는 기존 데이터베이스를 사용 하 여 Code First 개발을 살펴보았습니다. Visual Studio 용 Entity Framework Tools를 사용 하 여 데이터베이스에 매핑되고 데이터를 저장 하 고 검색 하는 데 사용할 수 있는 클래스 집합을 리버스 엔지니어링 했습니다.
