---
title: 새 데이터베이스에 Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: d540fc6e84049f345ae22998f94c309e0be73fc3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415546"
---
# <a name="code-first-to-a-new-database"></a>새 데이터베이스에 Code First
이 비디오 및 단계별 연습에서는 새 데이터베이스를 대상으로 하는 Code First 개발에 대해 소개 합니다. 이 시나리오에는 존재 하지 않는 데이터베이스를 대상으로 하 고 Code First 만들거나 새 테이블을 추가할 Code First 있는 빈 데이터베이스가 포함 됩니다. Code First를 사용 하면 C\# 또는 VB.Net 클래스를 사용 하 여 모델을 정의할 수 있습니다. 필요에 따라 클래스 및 속성의 특성을 사용 하거나 흐름 API를 사용 하 여 추가 구성을 수행할 수 있습니다.

## <a name="watch-the-video"></a>비디오 보기
이 비디오에서는 새 데이터베이스를 대상으로 하는 Code First 개발에 대해 소개 합니다. 이 시나리오에는 존재 하지 않는 데이터베이스를 대상으로 하 고 Code First 만들거나 새 테이블을 추가할 Code First 있는 빈 데이터베이스가 포함 됩니다. Code First를 사용 하면 또는 VB.Net 클래스를 C# 사용 하 여 모델을 정의할 수 있습니다. 필요에 따라 클래스 및 속성의 특성을 사용 하거나 흐름 API를 사용 하 여 추가 구성을 수행할 수 있습니다.

**작성자**: [Rowan Miller](https://romiller.com/)

**비디오**: [wmv](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [wmv (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)

## <a name="pre-requisites"></a>필수 구성 요소

이 연습을 완료 하려면 Visual Studio 2010 또는 Visual Studio 2012 이상이 설치 되어 있어야 합니다.

Visual Studio 2010을 사용 하는 경우 [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) 도 설치 해야 합니다.

## <a name="1-create-the-application"></a>1. 응용 프로그램 만들기

간단 하 게 유지 하기 위해 Code First를 사용 하 여 데이터 액세스를 수행 하는 기본 콘솔 응용 프로그램을 빌드 하겠습니다.

-   Visual Studio를 엽니다.
-   **파일&gt; 새&gt; 프로젝트 ...**
-   왼쪽 메뉴 및 **콘솔 응용 프로그램** 에서 **Windows** 를 선택 합니다.
-   이름으로 **CodeFirstNewDatabaseSample** 을 입력 합니다.
-   **확인**을 선택합니다.

## <a name="2-create-the-model"></a>2. 모델 만들기

클래스를 사용 하 여 매우 간단한 모델을 정의 하겠습니다. Program.cs 파일에서 정의 하 고 있지만 실제 응용 프로그램에서는 클래스를 개별 파일 및 잠재적으로 개별 프로젝트로 분할 합니다.

Program.cs의 Program 클래스 정의 아래에 다음 두 개의 클래스를 추가 합니다.

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

두 개의 탐색 속성 (블로그 게시물 및 게시물 블로그)이 가상으로 작성 되 고 있음을 알 수 있습니다. 이렇게 하면 Entity Framework의 지연 로드 기능을 사용할 수 있습니다. 지연 로드는 액세스 하려고 할 때 이러한 속성의 내용이 데이터베이스에서 자동으로 로드 됨을 의미 합니다.

## <a name="3-create-a-context"></a>3. 컨텍스트 만들기

이제 데이터베이스와의 세션을 나타내는 파생 컨텍스트를 정의 하 여 데이터를 쿼리하고 저장할 수 있습니다. DbContext에서 파생 되는 컨텍스트를 정의 하 고 모델의 각 클래스에 대해 형식화 된 DbSet&lt;&gt;를 노출 합니다.

이제 Entity Framework의 형식을 사용 하기 시작 하므로 EntityFramework NuGet 패키지를 추가 해야 합니다.

-   **프로젝트 – NuGet 패키지를 관리&gt; ...**
    참고: **NuGet 패키지 관리 ...** 옵션을 선택 하면 [최신 버전의 NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) 을 설치 해야 합니다.
-   **온라인** 탭을 선택 합니다.
-   **Entityframework** 패키지를 선택 합니다.
-   **설치**를 클릭합니다.

Program.cs의 맨 위에 using 문을 추가 합니다.

``` csharp
using System.Data.Entity;
```

Program.cs의 Post 클래스 아래에 다음 파생 컨텍스트를 추가 합니다.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

다음은 Program.cs에 포함 되어야 하는 내용에 대 한 전체 목록입니다.

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

데이터 저장 및 검색을 시작 하는 데 필요한 모든 코드입니다. 당연히 약간의 작업을 수행 하는 것이 좋지만 잠시 후에 수행 하는 것을 살펴보겠습니다.

## <a name="4-reading--writing-data"></a>4. 데이터 쓰기 & 읽기

아래와 같이 Program.cs에서 Main 메서드를 구현 합니다. 이 코드는 컨텍스트의 새 인스턴스를 만든 다음이를 사용 하 여 새 블로그를 삽입 합니다. 그런 다음 LINQ 쿼리를 사용 하 여 데이터베이스에서 제목별로 사전순으로 정렬 된 모든 블로그를 검색 합니다.

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
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a>내 데이터는 어디에 있나요?

DbContext에서 규칙에 따라 데이터베이스를 만들었습니다.

-   로컬 SQL Express 인스턴스를 사용할 수 있는 경우 (기본적으로 Visual Studio 2010와 함께 설치 됨) Code First에서 해당 인스턴스에 데이터베이스를 만들었습니다.
-   SQL Express를 사용할 수 없는 경우 Code First에서 [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) 를 시도 하 고 사용 합니다 (Visual Studio 2012와 함께 기본적으로 설치 됨).
-   데이터베이스 이름은 파생 컨텍스트의 정규화 된 이름 (여기서는 **CodeFirstNewDatabaseSample. BloggingContext**

이러한 규칙은 기본 규칙 이며 Code First 사용 하는 데이터베이스를 변경 하는 다양 한 방법이 있습니다. 자세한 내용은 **모델 및 데이터베이스 연결을 검색 하는 방법** 항목에서 확인할 수 있습니다.
Visual Studio에서 서버 탐색기를 사용 하 여이 데이터베이스에 연결할 수 있습니다.

-   **뷰&gt; 서버 탐색기**
-   **데이터 연결** 을 마우스 오른쪽 단추로 클릭 하 고 **연결 추가** ...를 선택 합니다.
-   서버 탐색기 데이터베이스에 연결 하지 않은 경우 Microsoft SQL Server를 데이터 원본으로 선택 해야 합니다.

    ![데이터 원본 선택](~/ef6/media/selectdatasource.png)

-   설치한 항목에 따라 LocalDB 또는 SQL Express에 연결

이제 Code First 만든 스키마를 검사할 수 있습니다.

![스키마 초기](~/ef6/media/schemainitial.png)

DbContext는 정의한 DbSet 속성을 살펴보면 모델에 포함할 클래스를 처리 했습니다. 그런 다음 기본 Code First 규칙 집합을 사용 하 여 테이블 및 열 이름을 결정 하 고, 데이터 형식을 결정 하 고, 기본 키를 찾습니다. 이 연습의 뒷부분에서는 이러한 규칙을 재정의할 수 있는 방법을 살펴보겠습니다.

## <a name="5-dealing-with-model-changes"></a>5. 모델 변경 처리

이제 모델을 변경할 때 이러한 변경 작업을 수행 하면 데이터베이스 스키마도 업데이트 해야 합니다. 이렇게 하려면 Code First 마이그레이션 라는 기능 또는 짧은 마이그레이션 기능을 사용 합니다.

마이그레이션을 통해 데이터베이스 스키마를 업그레이드 하 고 다운 그레이드 하는 방법을 설명 하는 일련의 단계를 수행할 수 있습니다. 마이그레이션 이라고 하는 이러한 각 단계에는 적용 되는 변경 내용을 설명 하는 코드가 포함 되어 있습니다. 

첫 번째 단계는 BloggingContext에 대 한 Code First 마이그레이션를 사용 하도록 설정 하는 것입니다.

-   **도구-&gt; 라이브러리 패키지 관리자-&gt; 패키지 관리자 콘솔**
-   패키지 관리자 콘솔에서 **Enable-Migrations** 명령을 실행합니다.
-   새 마이그레이션 폴더가 다음 두 항목을 포함 하는 프로젝트에 추가 되었습니다.
    -   **Configuration.cs** –이 파일에는 마이그레이션 시 BloggingContext 마이그레이션에 사용할 설정이 포함 되어 있습니다. 이 연습에서는 어떤 것도 변경할 필요가 없지만, 여기서는 초기값 데이터를 지정 하 고, 다른 데이터베이스에 대 한 공급자를 등록 하 고, 마이그레이션이 생성 되는 네임 스페이스를 변경할 수 있습니다.
    -   **&lt;timestamp&gt;\_InitialCreate.cs** – 첫 번째 마이그레이션은 데이터베이스에 이미 적용 된 변경 내용을 표시 하 여 블로그 및 게시물 테이블이 포함 된 데이터베이스에 대 한 빈 데이터베이스를 가져오는 것입니다. 이러한 테이블을 자동으로 만들 Code First 있지만 마이그레이션에 옵트인 (opt in) 한 후에는 마이그레이션로 변환 되었습니다. 이 마이그레이션이 이미 적용 된 Code First 로컬 데이터베이스에도 기록 됩니다. 파일 이름에 대 한 타임 스탬프는 정렬 목적으로 사용 됩니다.

    이제 모델을 변경 하 고, 블로그 클래스에 Url 속성을 추가 하겠습니다.

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   패키지 관리자 콘솔에서 **추가 마이그레이션 AddUrl** 명령을 실행 합니다.
    마이그레이션 추가 명령은 마지막 마이그레이션 이후 변경 내용을 확인 하 고 검색 된 변경 내용을 사용 하 여 새 마이그레이션을 스 캐 폴드. 마이그레이션 이름을 지정할 수 있습니다. 이 경우 ' AddUrl ' 마이그레이션을 호출 합니다.
    스 캐 폴드 코드는 문자열 데이터를 보유할 수 있는 Url 열을 dbo에 추가 해야 한다는 것을 의미 합니다. 블로그 테이블. 필요한 경우 스 캐 폴드 코드를 편집할 수 있지만이 경우에는 필요 하지 않습니다.

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

-   패키지 관리자 콘솔에서 **업데이트 데이터베이스** 명령을 실행 합니다. 이 명령은 보류 중인 모든 마이그레이션을 데이터베이스에 적용 합니다. InitialCreate 마이그레이션은 이미 적용 되어 마이그레이션 시 새로운 AddUrl 마이그레이션만 적용 됩니다.
    팁: 업데이트-데이터베이스를 호출할 때 **– Verbose** 스위치를 사용 하 여 데이터베이스에 대해 실행 되는 SQL을 확인할 수 있습니다.

이제 데이터베이스의 블로그 테이블에 새 Url 열이 추가 됩니다.

![Url이 포함 된 스키마](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a>6. 데이터 주석

지금까지 EF는 기본 규칙을 사용 하 여 모델을 검색 하지만 클래스에서 규칙을 따르지 않고 추가 구성을 수행 해야 하는 경우가 있었습니다. 이에 대 한 두 가지 옵션이 있습니다. 이 섹션에서 데이터 주석을 확인 하 고 다음 섹션에서 흐름 API를 살펴보겠습니다.

-   모델에 사용자 클래스를 추가 해 보겠습니다.

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   또한 파생 된 컨텍스트에 집합을 추가 해야 합니다.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   마이그레이션을 추가 하려고 하면 "*EntityType ' 사용자 '에 키가 정의 되어 있지 않다는 오류 메시지가 표시 됩니다. 이 EntityType의 키를 정의 합니다. "* EF에는 사용자 이름이 사용자에 대 한 기본 키 여야 한다는 것을 알 수 있는 방법이 없습니다.
-   이제 데이터 주석을 사용할 예정 이므로 Program.cs의 맨 위에 using 문을 추가 해야 합니다.

```csharp
using System.ComponentModel.DataAnnotations;
```

-   이제 사용자 이름 속성에 주석을 추가 하 여 기본 키인지를 식별 합니다.

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Migration 스 캐 폴드 **AddUser** 명령을 사용 하 여 이러한 변경 내용을 데이터베이스에 적용 하는 마이그레이션 추가
-   **Update-database** 명령을 실행 하 여 데이터베이스에 새 마이그레이션을 적용 합니다.

이제 새 테이블이 데이터베이스에 추가 됩니다.

![사용자가 있는 스키마](~/ef6/media/schemawithusers.png)

EF에서 지원 되는 전체 주석 목록은 다음과 같습니다.

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

## <a name="7-fluent-api"></a>7. 흐름 API

이전 섹션에서는 데이터 주석을 사용 하 여 규칙에 따라 검색 된 항목을 보완 하거나 재정의 하는 방법을 살펴보았습니다. 모델을 구성 하는 다른 방법은 Code First 흐름 API를 통하는 것입니다.

대부분의 모델 구성은 간단한 데이터 주석을 사용 하 여 수행할 수 있습니다. 흐름 API는 데이터 주석을 사용 하 여 데이터 주석으로 수행할 수 없는 몇 가지 고급 구성 외에도 데이터 주석을 통해 수행할 수 있는 모든 것을 포함 하는 모델 구성을 지정 하는 보다 고급 방법입니다. 데이터 주석과 흐름 API를 함께 사용할 수 있습니다.

흐름 API에 액세스 하려면 DbContext에서 OnModelCreating 메서드를 재정의 합니다. \_이름을 표시 하기 위해 User. DisplayName이 저장 된 열의 이름을 바꾸려고 한다고 가정해 보겠습니다.

-   다음 코드를 사용 하 여 BloggingContext에서 OnModelCreating 메서드를 재정의 합니다.

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

-   마이그레이션을 스 캐 폴드 하 여 데이터베이스에 이러한 변경 내용을 적용 하려면 migration **Changechangedisplayname** 명령을 사용 합니다.
-   **업데이트-데이터베이스** 명령을 실행 하 여 데이터베이스에 새 마이그레이션을 적용 합니다.

이제 DisplayName 열 이름이\_이름을 표시 하도록 변경 되었습니다.

![표시 이름이 변경 된 스키마](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a>요약

이 연습에서는 새 데이터베이스를 사용 하 여 Code First 개발을 살펴보았습니다. 그런 다음 클래스를 사용 하 여 모델을 정의 하 고이 모델을 사용 하 여 데이터베이스를 만들고 데이터를 저장 및 검색 합니다. 데이터베이스를 만든 후에는 모델이 진화 함에 따라 스키마를 변경 Code First 마이그레이션 했습니다. 또한 데이터 주석과 흐름 API를 사용 하 여 모델을 구성 하는 방법도 살펴보았습니다.
