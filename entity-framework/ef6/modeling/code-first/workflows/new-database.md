---
title: 새 데이터베이스-EF6 대 한 code First
author: divega
ms.date: 2016-10-23
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: 8ed1bfbc3536acc0d83b9c8ecdd180aeb44eff83
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251052"
---
# <a name="code-first-to-a-new-database"></a>새 데이터베이스에 대 한 code First
이 비디오 및 단계별 연습에서는 새 데이터베이스를 대상으로 하는 Code First 개발에 대 한 소개를 제공 합니다. 이 시나리오는 존재 하지 않는 데이터베이스를 대상으로 포함 및 Code First는 만들거나 빈 데이터베이스는 Code First는 새 테이블을 추가 합니다. C를 사용 하 여 모델을 정의 하는 코드 먼저 허용\# 또는 VB.Net 클래스입니다. 클래스 및 속성 또는 fluent API를 사용 하 여 특성을 사용 하 여 추가 구성을 수행할 필요에 따라 있습니다.

## <a name="watch-the-video"></a>비디오를 시청 하세요.
이 비디오에서는 새 데이터베이스를 대상으로 하는 Code First 개발에 대해 소개 합니다. 이 시나리오는 존재 하지 않는 데이터베이스를 대상으로 포함 및 Code First는 만들거나 빈 데이터베이스는 Code First는 새 테이블을 추가 합니다. 먼저 코드를 사용 하면 C# 또는 VB.Net 클래스를 사용 하 여 모델을 정의할 수 있습니다. 클래스 및 속성 또는 fluent API를 사용 하 여 특성을 사용 하 여 추가 구성을 수행할 필요에 따라 있습니다.

**작성자**: [Rowan Miller](http://romiller.com/)

**비디오**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)

# <a name="pre-requisites"></a>필수 조건

적어도 Visual studio 2010 해야 하거나이 연습을 완료 하려면 Visual Studio 2012를 설치 합니다.

Visual Studio 2010을 사용 하는 경우 해야 할 [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) 설치 합니다.

## <a name="1-create-the-application"></a>1. 응용 프로그램 만들기

간단 하 게 데이터 액세스를 수행 하려면 Code First를 사용 하는 기본적인 콘솔 응용 프로그램을 빌드하는 것이 하겠습니다.

-   Visual Studio를 엽니다.
-   **파일만&gt; 새로운 기능-&gt; 프로젝트...**
-   선택 **Windows** 왼쪽된 메뉴에서 및 **콘솔 응용 프로그램**
-   입력 **CodeFirstNewDatabaseSample** 이름으로
-   **확인**을 선택합니다.

## <a name="2-create-the-model"></a>2. 모델 만들기

클래스를 사용 하 여 매우 간단한 모델을 정의 해 보겠습니다. 방금 정의 고 Program.cs 파일에는 있지만 아웃 클래스를 별도 파일 및 잠재적으로 별도 프로젝트를 분할 하는 실제 응용 프로그램에 있습니다.

프로그램 클래스 정의 Program.cs에서 아래 다음 두 클래스를 추가 합니다.

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

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
```

만들고 두 개의 탐색 속성 (Blog.Posts 및 Post.Blog) 가상 알 수 있습니다. 따라서 Entity Framework의 지연 로드 기능이 있습니다. 지연 로드는 이러한 속성의 내용을 자동으로 로드할 데이터베이스에서 액세스 하려고 할 때를 의미 합니다.

## <a name="3-create-a-context"></a>3. 컨텍스트 만들기

이제 데이터베이스를 쿼리하고 데이터를 저장할 수 있어를 사용 하 여 세션을 나타내는 파생된 컨텍스트를 정의 하는 시간입니다. System.Data.Entity.DbContext에서 파생 되 고 형식화 된 DbSet을 노출 하는 컨텍스트를 정의 했습니다&lt;TEntity&gt; 모델의 각 클래스에 대 한 합니다.

이제 EntityFramework NuGet 패키지를 추가 해야 하므로 Entity Framework에서 형식 사용 하기 시작 합니다.

-   **프로젝트&gt; NuGet 패키지 관리...**
    참고: 없는 경우는 **NuGet 패키지 관리...** 설치 해야 하는 옵션을 [최신 버전의 NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)
-   선택 된 **Online** 탭
-   선택 된 **EntityFramework** 패키지
-   클릭 **설치**

추가 하 여 문을 Program.cs 맨 위에 있는 System.Data.Entity에 대 한 합니다.

``` csharp
using System.Data.Entity;
```

Program.cs에서 Post 클래스 아래 다음 파생된 컨텍스트를 추가 합니다.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

Program.cs 이제 포함 항목의 전체 목록은 다음과 같습니다.

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

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

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

데이터 저장 및 검색을 시작 해야 하는 모든 코드입니다. 물론 백그라운드에서 진행 상당한 되며 알아보겠습니다 살펴보겠습니다는 첫 번째 있지만 잠시에서 하는지 확인해 보겠습니다 작업.

## <a name="4-reading--writing-data"></a>4. 읽기 및 데이터 쓰기

아래 표시 된 것과 같이 Program.cs의 Main 메서드를 구현 합니다. 이 코드는이 컨텍스트의 새 인스턴스를 만듭니다 및 다음 새 블로그 삽입을 사용 하 여 합니다. 다음 LINQ 쿼리를 사용 하 여 제목으로 사전순으로 정렬 하는 데이터베이스에서 모든 블로그를 검색 하려면.

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
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a>내 데이터는 어디 입니까?

관례상 DbContext를 데이터베이스를 만들었습니다.

-   (Visual Studio 2010을 사용 하 여 기본적으로 설치 됨)을 로컬 SQL Express 인스턴스를 사용할 수 있는 경우 Code First가 만든 데이터베이스 인스턴스에서
-   SQL Express를 사용할 수 없는 경우 Code First를 시도 하 고 사용 [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (Visual Studio 2012를 사용 하 여 기본적으로 설치 됨)
-   데이터베이스는이 경우에서 파생 컨텍스트의 정규화 된 이름 뒤에 오는 이름은 **CodeFirstNewDatabaseSample.BloggingContext**

이 방금 기본 규칙 및 Code First를 사용 하는 데이터베이스를 변경 하는 방법은 여러 가지가 있습니다,에 자세한 정보가 제공 됩니다는 **DbContext는 모델 및 데이터베이스 연결을 검색 하는 방법을** 항목입니다.
Visual Studio에서 서버 탐색기를 사용 하 여이 데이터베이스에 연결할 수 있습니다.

-   **보기-&gt; 서버 탐색기**
-   마우스 오른쪽 단추로 클릭 **데이터 연결** 선택한 **연결 추가 중...**
-   Microsoft SQL Server 데이터 원본으로 선택 해야 하기 전에 서버 탐색기에서 데이터베이스에 연결 하지 않은 경우

    ![데이터 원본 선택](~/ef6/media/selectdatasource.png)

-   LocalDB 또는 어느에 따라 설치한 SQL Express에 연결

이제 Code First에서 만든 스키마를 검사 수 있습니다.

![초기 스키마](~/ef6/media/schemainitial.png)

DbContext에 정의한 DbSet 속성을 확인 하 여 모델에 포함할 어떤 클래스를 산출 합니다. 다음 테이블 및 열 이름을 결정, 데이터 형식을 결정 하 고, 기본 키 등을 찾을 Code First 규칙의 기본 집합을 사용 합니다. 이 연습의 뒷부분에서 이러한 규칙을 재정의할 수 있습니다 하는 방법을 살펴보겠습니다.

## <a name="5-dealing-with-model-changes"></a>5. 모델 변경 처리

이제 데이터베이스 스키마를 업데이트 해야 하는 이러한 변경을 수행 하는 경우 모델을를 일부 변경 하는 시간입니다. 이렇게 하려면 short에 대 한 Code First 마이그레이션을 또는 마이그레이션 이라는 기능을 사용 하려고 합니다.

마이그레이션은 데이터베이스 스키마를 업그레이드 (및 다운 그레이드) 하는 방법을 설명 하는 단계는 정렬 된 집합이 있을 수 있습니다. 각 마이그레이션 라고 하는 이러한 단계를 적용할 변경 내용을 설명 하는 일부 코드를 포함 합니다. 

첫 번째 단계는 BloggingContext에 대 한 Code First 마이그레이션을 사용 하도록 설정 하는 것입니다.

-   **도구-&gt; 라이브러리 패키지 관리자-&gt; 패키지 관리자 콘솔**
-   패키지 관리자 콘솔에서 **Enable-Migrations** 명령을 실행합니다.
-   새 마이그레이션 폴더 두 항목을 포함 하는 프로젝트에 추가 되었습니다.
    -   **Configuration.cs** –이 파일에는 마이그레이션에 대 한 마이그레이션을 사용할 설정이 포함 되어 있습니다. BloggingContext 합니다. 이 연습에서는 아무 것도 변경할 필요가 없습니다 이지만 여기 네임 스페이스를 변경 하는 시드 데이터를 다른 데이터베이스에 대 한 등록 공급자를 지정할 수 있는 마이그레이션 등에서 생성 되는 합니다.
    -   **&lt;타임 스탬프&gt;\_InitialCreate.cs** –이 첫 번째 마이그레이션을, 블로그 및 게시물 테이블이 포함 된 하나에 빈 데이터베이스에서 수행 하려면 데이터베이스에 이미 적용 된 변경 내용을 나타내는 . 하지만 하도록 Code First 마이그레이션을로 변환 되었을 것 마이그레이션에 옵트인 했습니다 했으므로 우리에 게 있어 이러한 테이블을 자동으로 만듭니다. 먼저 코드에도이 마이그레이션이 이미 적용 된 로컬 데이터베이스에 기록 됩니다. 파일의 타임 스탬프는 정렬을 위해 사용 됩니다.

    이제 모델을 사용 하 여 변경 하기, 블로그 클래스에 Url 속성을 추가 해 보겠습니다.

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   실행 합니다 **Add-migration AddUrl** 패키지 관리자 콘솔에서 명령을 합니다.
    추가 마이그레이션 명령을 마지막 마이그레이션 이후 변경 내용을 확인 하 고 발견 되는 변경 내용으로 새 마이그레이션을 스 캐 폴딩 합니다. 마이그레이션을; 이름을 제공할 수 있습니다. 이 경우 마이그레이션 'AddUrl' 이라고 합니다.
    스 캐 폴드 된 코드는 우리 해야 한다는 dbo에 문자열 데이터를 보유할 수에 있는 Url 열을 추가 합니다. 블로그 테이블입니다. 필요한 경우 스 캐 폴드 된 코드를 편집할 수 있지만 경우에 필수는 아닙니다.

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   실행 합니다 **Update-database** 패키지 관리자 콘솔에서 명령을 합니다. 이 명령은 데이터베이스에 보류 중인 마이그레이션을 적용 됩니다. InitialCreate 마이그레이션 마이그레이션을 새 AddUrl 마이그레이션 적용만 있으므로 이미 적용 되었습니다.
    팁: 사용할 수는 **– Verbose** 보려면 데이터베이스에 대해 실행 되는 SQL 데이터베이스 업데이트를 호출 하는 경우를 전환 합니다.

새 Url 열이 이제 블로그 테이블 데이터베이스에 추가 됩니다.

![Url 사용 하 여 스키마](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a>6. 데이터 주석

지금까지 EF는 기본 규칙을 사용 하 여 모델을 검색만 하도록 했습니다 하지만 클래스에는 규칙을 따르지 시점과 추가 구성을 수행 하려면 먼저 시간 되도록 것입니다. 두 가지 방법으로이; 이 섹션의 데이터 주석 및 다음 흐름 API는 다음 섹션에서 살펴보겠습니다.

-   모델에 추가할 사용자 클래스

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   이 파생된 컨텍스트 집합을 추가 해야

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   표시 된 오류는 마이그레이션을 추가 하려고 할 경우 얻게 "*EntityType 'User' 키가 없는 정의 합니다. 키를 정의이 EntityType에 대 한 합니다. "* EF에 사용자 이름을 사용자에 대 한 기본 키를 함을 알 수 없습니다.
-   사용 하 여 추가 하므로 이제 데이터 주석을 사용 하는 것 Program.cs 맨 위에 있는 문

```
using System.ComponentModel.DataAnnotations;
```

-   이제 기본 키 것을 식별 하기 위해 사용자 이름 속성에 주석 달기

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   사용 된 **Add-migration AddUser** 데이터베이스 변경 명령을 적용 하는 마이그레이션을 스 캐 폴딩
-   실행 합니다 **Update-database** 새 마이그레이션을 데이터베이스에 적용 하는 명령

이제 새 테이블은 데이터베이스에 추가 됩니다.

![사용자와 스키마](~/ef6/media/schemawithusers.png)

EF에서 지원 되는 주석의 전체 목록은 다음과 같습니다.

-   [KeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [StringLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [MaxLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [ConcurrencyCheckAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [RequiredAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [TimestampAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [ComplexTypeAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [ColumnAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [TableAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [InversePropertyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [ForeignKeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [DatabaseGeneratedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [NotMappedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a>7. Fluent API

이전 섹션에서 데이터 주석을 사용 하 여 보완 하거나 규칙에 따라 검색 된 항목을 재정의에 대해 살펴보았습니다. Code First fluent API를 통해 모델을 구성 하는 다른 방법은 됩니다.

대부분의 모델 구성 할 수 있는 간단한 데이터 주석을 사용 합니다. Fluent API는 데이터 주석 할 수 있는 모든 또한 불가능 데이터 주석을 사용한 몇 가지 고급 구성에 설명 하는 모델 구성을 지정 하는 고급 방법을 합니다. 데이터 주석과 흐름 API를 함께 사용할 수 있습니다.

Fluent API에 액세스 하려면 DbContext의 OnModelCreating 메서드를 재정의할 수 있습니다. 에 방문 하셔서 User.DisplayName 표시할에 저장 되는 열의 이름을 바꿀 경우를 가정해\_이름입니다.

-   다음 코드를 사용 하 여 BloggingContext OnModelCreating 메서드 재정의

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   사용 합니다 **Add-migration ChangeDisplayName** 데이터베이스 변경 명령을 적용 하는 마이그레이션을 스 캐 폴딩 합니다.
-   실행 합니다 **Update-database** 새 마이그레이션을 데이터베이스에 적용할 명령.

표시할 DisplayName 열 이름이 이제\_이름:

![이름을 바꿀 표시 이름과 함께 스키마](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a>요약

이 연습에서는 새 데이터베이스를 사용 하 여 Code First 개발에 살펴보았습니다. 클래스를 사용 하 여 모델을 정의 하 고 데이터베이스를 만들고 저장 하 고 데이터를 검색 하는 모델을 사용 합니다. 데이터베이스가 만들어진 후로 발전 한 모델 스키마를 변경 하려면 Code First 마이그레이션을 사용 했습니다. 데이터 주석 및 Fluent API를 사용 하 여 모델을 구성 하는 방법을 살펴보았습니다.
