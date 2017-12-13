---
title: ".NET Framework에서 시작 - 새 데이터베이스 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>.NET Framework에서 새 데이터베이스로 EF Core 시작

이 연습에서는 Entity Framework를 사용하여 Microsoft SQL Server 데이터베이스에 대해 기본 데이터 액세스를 수행하는 콘솔 응용 프로그램을 빌드합니다. 마이그레이션을 사용하여 모델에서 데이터베이스를 만듭니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb)을 볼 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 연습을 완료하려면 다음 필수 구성 요소가 필요합니다.

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [NuGet 패키지 관리자의 최신 버전](https://dist.nuget.org/index.html)

* [Windows PowerShell의 최신 버전](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a>새 프로젝트 만들기

* Visual Studio를 엽니다.

* 파일 > 새로 만들기 > 프로젝트...

* 왼쪽 메뉴에서 [템플릿] > [Visual C#] > [Windows 클래식 바탕 화면]을 선택합니다.

* **콘솔 앱(.NET Framework)** 프로젝트 템플릿을 선택합니다.

* **.NET Framework 4.5.1** 이상을 대상으로 하는지 확인합니다.

* 프로젝트에 이름을 지정하고 **확인**을 클릭합니다.

## <a name="install-entity-framework"></a>Entity Framework 설치

EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다. 이 연습에서는 SQL Server를 사용합니다. 사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.

* 도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔

* `Install-Package Microsoft.EntityFrameworkCore.SqlServer` 실행

이 연습의 뒷부분에서는 일부 Entity Framework Tools를 사용하여 데이터베이스를 유지 관리합니다. 따라서 도구 패키지도 설치합니다.

* `Install-Package Microsoft.EntityFrameworkCore.Tools` 실행

## <a name="create-your-model"></a>모델 만들기

이제 모델을 구성하는 컨텍스트 및 엔터티 클래스를 정의할 수 있습니다.

* 프로젝트 > 클래스 추가...

* 이름으로 *Model.cs*를 입력하고 **확인**을 클릭합니다.

* 파일의 내용을 다음 코드로 바꿉니다.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }

        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

> [!TIP]  
> 실제 응용 프로그램에서는 각 클래스를 별도의 파일에 저장하고, `App.Config` 파일에 연결 문자열을 저장하고, `ConfigurationManager`를 사용하여 파일을 읽습니다. 간단한 설명을 위해 모든 항목을 이 학습서에 대한 단일 코드 파일에 저장합니다.

## <a name="create-your-database"></a>데이터베이스 만들기

이제 모델이 있으므로 마이그레이션을 사용하여 데이터베이스를 만들 수 있습니다.

* 도구 -> NuGet 패키지 관리자 -> 패키지 관리자 콘솔

* `Add-Migration MyFirstMigration`를 실행하여 마이그레이션을 스캐폴딩하고 모델에 대한 초기 테이블 집합을 만듭니다.

* `Update-Database`를 실행하여 새 마이그레이션을 데이터베이스에 적용합니다. 데이터베이스가 아직 없기 때문에 마이그레이션이 적용되기 전에 사용자의 데이터베이스가 생성됩니다.

> [!TIP]  
> 나중에 모델을 변경할 경우 `Add-Migration` 명령을 사용하여 새 마이그레이션을 스캐폴딩하고 데이터베이스에서 해당 스키마를 변경합니다. 스캐폴딩된 코드를 확인하고 필요한 내용을 변경한 후 `Update-Database` 명령을 사용하여 변경 내용을 데이터베이스에 적용할 수 있습니다.
>
>EF는 데이터베이스의 `__EFMigrationsHistory` 테이블을 사용하여 이미 데이터베이스에 적용된 마이그레이션을 추적합니다.

## <a name="use-your-model"></a>모델 사용

이제 모델을 사용하여 데이터 액세스를 수행할 수 있습니다.

* *Program.cs*를 엽니다.

* 파일의 내용을 다음 코드로 바꿉니다.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blogs)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* 디버그 > 디버깅하지 않고 시작

하나의 블로그가 데이터베이스에 저장되고 모든 블로그의 세부 정보가 콘솔에 출력되는 것을 알 수 있습니다.

![image](_static/output-new-db.png)
