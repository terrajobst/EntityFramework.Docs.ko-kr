---
title: .NET Framework에서 시작 - 기존 데이터베이스 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 39e77ab8c124df67458cc5fa6db2882b65943ebe
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39388470"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>.NET Framework에서 기존 데이터베이스로 EF Core 시작

이 연습에서는 Entity Framework를 사용하여 Microsoft SQL Server 데이터베이스에 대해 기본 데이터 액세스를 수행하는 콘솔 응용 프로그램을 빌드합니다. 리버스 엔지니어링을 사용하여 기존 데이터베이스를 기반으로 Entity Framework 모델을 만듭니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb)을 볼 수 있습니다.

## <a name="prerequisites"></a>전제 조건

이 연습을 완료하려면 다음 필수 구성 요소가 필요합니다.

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) - 최소 15.3 버전

* [NuGet 패키지 관리자의 최신 버전](https://dist.nuget.org/index.html)

* [Windows PowerShell의 최신 버전](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [Blogging 데이터베이스](#blogging-database)

### <a name="blogging-database"></a>Blogging 데이터베이스

이 자습서에서는 LocalDb 인스턴스의 **Blogging** 데이터베이스를 기존 데이터베이스로 사용합니다.

> [!TIP]  
> **이미 다른 자습서의 일부로 Blogging** 데이터베이스를 만든 경우 다음 단계를 건너뛸 수 있습니다.

* Visual Studio를 엽니다.

* 도구 > 데이터베이스에 연결...

* **Microsoft SQL Server**를 선택하고 **계속**을 클릭합니다.

* **서버 이름**으로 **(localdb)\mssqllocaldb**를 입력합니다.

* **데이터베이스 이름**으로 **master**를 입력하고 **확인**을 클릭합니다.

* 이제 master 데이터베이스가 **서버 탐색기**의 **데이터 연결**에 표시됩니다.

* **서버 탐색기**에서 데이터베이스를 마우스 오른쪽 단추로 클릭하고 **새 쿼리**를 선택합니다.

* 아래 나열된 스크립트를 쿼리 편집기로 복사합니다.

* 쿼리 편집기를 마우스 오른쪽 단추로 클릭하고 **실행**을 선택합니다.

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>새 프로젝트 만들기

* Visual Studio를 엽니다.

* 파일 > 새로 만들기 > 프로젝트...

* 왼쪽 메뉴에서 [템플릿] > [Visual C#] > [Windows]를 선택합니다.

* **콘솔 응용 프로그램** 프로젝트 템플릿을 선택합니다.

* **.NET Framework 4.6.1** 이상을 대상으로 하는지 확인합니다.

* 프로젝트에 이름을 지정하고 **확인**을 클릭합니다.

## <a name="install-entity-framework"></a>Entity Framework 설치

EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다. 이 연습에서는 SQL Server를 사용합니다. 사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.

* 도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔

* `Install-Package Microsoft.EntityFrameworkCore.SqlServer` 실행

기존 데이터베이스에서 리버스 엔지니어링을 사용하려면 몇 개의 다른 패키지도 설치해야 합니다.

* `Install-Package Microsoft.EntityFrameworkCore.Tools` 실행

## <a name="reverse-engineer-your-model"></a>모델 리버스 엔지니어링

이제 기존 데이터베이스를 기반으로 EF 모델을 만듭니다.

* 도구 -> NuGet 패키지 관리자 -> 패키지 관리자 콘솔

* 다음 명령을 실행하여 기존 데이터베이스에서 모델을 만듭니다.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

리버스 엔지니어링 프로세스에서는 기존 데이터베이스의 스키마를 기반으로 엔터티 클래스 및 파생 컨텍스트를 만들었습니다. 엔터티 클래스는 쿼리하고 저장할 데이터를 나타내는 단순 C# 개체입니다.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)] -->
``` csharp
using System;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class Blog
    {
        public Blog()
        {
            Post = new HashSet<Post>();
        }

        public int BlogId { get; set; }
        public string Url { get; set; }

        public virtual ICollection<Post> Post { get; set; }
    }
}
```

컨텍스트는 데이터베이스가 있는 세션을 나타내며 이를 통해 엔터티 클래스의 인스턴스를 쿼리하고 저장할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class BloggingContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
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

        public virtual DbSet<Blog> Blog { get; set; }
        public virtual DbSet<Post> Post { get; set; }
    }
}
```

## <a name="use-your-model"></a>모델 사용

이제 모델을 사용하여 데이터 액세스를 수행할 수 있습니다.

* *Program.cs*를 엽니다.

* 파일의 내용을 다음 코드로 바꿉니다.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blog.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blog)
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

![이미지](_static/output-existing-db.png)
