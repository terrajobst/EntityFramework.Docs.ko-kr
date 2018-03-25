---
title: ASP.NET Core에서 시작 - 기존 데이터베이스 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: db2469d0badd428734425c1f568667f00bef2f4f
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/23/2018
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>ASP.NET Core에서 기존 데이터베이스로 EF Core 시작

이 연습에서는 Entity Framework를 사용하여 기본 데이터 액세스를 수행하는 ASP.NET Core MVC 응용 프로그램을 빌드합니다. 리버스 엔지니어링을 사용하여 기존 데이터베이스를 기반으로 Entity Framework 모델을 만듭니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb)을 볼 수 있습니다.

## <a name="prerequisites"></a>전제 조건

이 연습을 완료하려면 다음 필수 구성 요소가 필요합니다.

* [Visual Studio 201715.3](https://www.visualstudio.com/downloads/) 및 다음 워크로드:
  * **ASP.NET 및 웹 개발**(**웹 및 클라우드** 아래)
  * **.NET Core 플랫폼 간 개발**(**기타 도구 집합** 아래)
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).
* [Blogging 데이터베이스](#blogging-database)

### <a name="blogging-database"></a>Blogging 데이터베이스

이 자습서에서는 LocalDb 인스턴스의 **Blogging** 데이터베이스를 기존 데이터베이스로 사용합니다.

> [!TIP]  
> **이미 다른 자습서의 일부로 Blogging** 데이터베이스를 만든 경우 다음 단계를 건너뛸 수 있습니다.

* Visual Studio를 엽니다.
* **도구 -> 데이터베이스에 연결...**
* **Microsoft SQL Server**를 선택하고 **계속**을 클릭합니다.
* **서버 이름**으로 **(localdb)\mssqllocaldb**를 입력합니다.
* **데이터베이스 이름**으로 **master**를 입력하고 **확인**을 클릭합니다.
* 이제 master 데이터베이스가 **서버 탐색기**의 **데이터 연결**에 표시됩니다.
* **서버 탐색기**에서 데이터베이스를 마우스 오른쪽 단추로 클릭하고 **새 쿼리**를 선택합니다.
* 아래 나열된 스크립트를 쿼리 편집기로 복사합니다.
* 쿼리 편집기를 마우스 오른쪽 단추로 클릭하고 **실행**을 선택합니다.

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>새 프로젝트 만들기

* Visual Studio 2017을 엽니다.
* **파일 -> 새로 만들기 -> 프로젝트...**
* 왼쪽 메뉴에서 **설치됨 -> 템플릿 -> Visual C# -> 웹**을 선택합니다.
* **ASP.NET Core 웹 응용 프로그램(.NET Core)** 프로젝트 템플릿을 선택합니다.
* 이름으로 **EFGetStarted.AspNetCore.ExistingDb**를 입력하고 **확인**을 클릭합니다.
* **새 ASP.NET Core 웹 응용 프로그램** 대화 상자가 표시될 때까지 기다립니다.
* **ASP.NET Core 템플릿 2.0**에서 **웹 응용 프로그램(모델-뷰-컨트롤러)**을 선택합니다.
* **인증**이 **인증 없음**으로 설정되었는지 확인합니다.
* **확인**을 클릭합니다.

## <a name="install-entity-framework"></a>Entity Framework 설치

EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다. 이 연습에서는 SQL Server를 사용합니다. 사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.

* **도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**

* `Install-Package Microsoft.EntityFrameworkCore.SqlServer` 실행

일부 Entity Framework Tools를 사용하여 데이터베이스에서 모델을 만듭니다. 따라서 도구 패키지도 설치합니다.

* `Install-Package Microsoft.EntityFrameworkCore.Tools` 실행

일부 ASP.NET Core 스캐폴딩 도구를 사용하여 나중에 컨트롤러와 뷰를 만듭니다. 따라서 이 디자인 패키지도 설치합니다.

* `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design` 실행

## <a name="reverse-engineer-your-model"></a>모델 리버스 엔지니어링

이제 기존 데이터베이스를 기반으로 EF 모델을 만듭니다.

* **도구 -> NuGet 패키지 관리자 -> 패키지 관리자 콘솔**
* 다음 명령을 실행하여 기존 데이터베이스에서 모델을 만듭니다.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

`The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`이라는 오류가 표시되면 Visual Studio를 닫았다가 다시 엽니다.

> [!TIP]  
> 위의 명령에 `-Tables` 인수를 추가하여 엔터티를 생성할 테이블을 지정할 수 있습니다. 예: `-Tables Blog,Post`.

리버스 엔지니어링 프로세스에서는 기존 데이터베이스의 스키마를 기반으로 엔터티 클래스(`Blog.cs` & `Post.cs`) 및 파생 컨텍스트(`BloggingContext.cs`)를 만들었습니다.

 엔터티 클래스는 쿼리하고 저장할 데이터를 나타내는 단순 C# 개체입니다.

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 컨텍스트는 데이터베이스가 있는 세션을 나타내며 이를 통해 엔터티 클래스의 인스턴스를 쿼리하고 저장할 수 있습니다.

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a>종속성 주입에 컨텍스트 등록

종속성 주입은 ASP.NET Core의 중심 개념입니다. 서비스(예: `BloggingContext`)는 응용 프로그램 시작 중에 종속성 주입에 등록됩니다. 이러한 서비스(예: MVC 컨트롤러)가 필요한 구성 요소에는 생성자 매개 변수 또는 속성을 통해 이러한 서비스가 제공됩니다. 종속성 주입에 대한 자세한 내용은 ASP.NET 사이트에서 [종속성 주입](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) 문서를 참조하세요.

### <a name="remove-inline-context-configuration"></a>인라인 컨텍스트 구성 제거

ASP.NET Core에서 구성은 일반적으로 **Startup.cs**에서 수행됩니다. 이 패턴을 따르기 위해 데이터베이스 공급자의 구성을 **Startup.cs**로 이동합니다.

* `Models\BloggingContext.cs`를 엽니다.
* `OnConfiguring(...)` 메서드를 삭제합니다.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* 종속성 주입으로 구성이 컨텍스트에 전달될 수 있도록 다음 생성자를 추가합니다.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a>Startup.cs에서 컨텍스트 등록 및 구성

MVC 컨트롤러에서 `BloggingContext`를 사용하려면 이 항목을 서비스로 등록합니다.

* **Startup.cs**를 엽니다.
* 파일 시작 부분에 다음 `using` 문을 추가합니다.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

이제 `AddDbContext(...)` 메서드를 사용하여 서비스로 등록할 수 있습니다.
* `ConfigureServices(...)` 메서드를 찾습니다.
* 다음 코드를 추가하여 컨텍스트를 서비스로 등록합니다.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> 실제 응용 프로그램에서는 일반적으로 연결 문자열을 구성 파일에 저장합니다. 간단한 설명을 위해 여기에서는 코드로 정의합니다. 자세한 내용은 [연결 문자열](../../miscellaneous/connection-strings.md)을 참조하세요.

## <a name="create-a-controller"></a>컨트롤러 만들기

다음으로 프로젝트에서 스캐폴딩을 사용하도록 설정합니다.

* **솔루션 탐색기**에서 **컨트롤러** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 -> 컨트롤러...**를 선택합니다.
* **전체 종속성**을 선택하고 **추가**를 클릭합니다.
* 열리는 `ScaffoldingReadMe.txt` 파일의 지침은 무시할 수 있습니다.

이제 스캐폴딩이 사용 가능하므로 `Blog` 엔터티에 대한 컨트롤러를 스캐폴딩할 수 있습니다.

* **솔루션 탐색기**에서 **컨트롤러** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 -> 컨트롤러...**를 선택합니다.
* **보기 포함 MVC 컨트롤러, Entity Framework**를 선택하고 **확인**을 클릭합니다.
* **모델 클래스**를 **Blog**로 설정하고 **데이터 컨텍스트 클래스**를 **BloggingContext**로 설정합니다.
* **추가**를 클릭합니다.

## <a name="run-the-application"></a>응용 프로그램 실행

이제 응용 프로그램을 실행하여 작동하는지 확인할 수 있습니다.

* **디버그 -> 디버깅하지 않고 시작**
* 응용 프로그램이 빌드되고 웹 브라우저에서 열립니다.
* `/Blogs`로 이동합니다.
* **새로 만들기**를 클릭합니다.
* 새 블로그의 **URL**을 입력하고 **만들기**를 클릭합니다.

![이미지](_static/create.png)

![이미지](_static/index-existing-db.png)
