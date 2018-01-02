---
title: "요약 개요 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: 13de9cf98111b8e253e073c591fcec04206b4c4f
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
# <a name="entity-framework-core-quick-overview"></a>Entity Framework Core 요약 개요

EF(Entity Framework) Core는 널리 사용되는 Entity Framework 데이터 액세스 기술의 가볍고 확장 가능하며 플랫폼 교차적인 버전입니다.

EF Core는 개체 관계형 매퍼(O/RM)로, .NET 개발자들이 .NET 개체를 사용하여 데이터베이스 작업을 수행할 수 있습니다. 개발자들이 보통 작성해야 하는 데이터 액세스 코드가 대부분 필요하지 않게 됩니다. EF Core 는 여러 데이터베이스 엔진을 지원합니다. 자세한 내용은 [데이터베이스 공급자](providers/index.md)를 참조하세요.

코드 작성을 통해 학습하려면 [시작](get-started/index.md) 가이드 중 하나를 사용하여 EF Core를 시작해 보는 것이 좋습니다.

## <a name="latest-version-ef-core-20"></a>최신 버전: EF Core 2.0

EF Core를 잘 알고 있고 새 버전의 세부 정보로 직접 이동하려는 경우:

- **[EF Core 2.0의 새로운 기능](what-is-new/index.md)**
- **[EF Core 2.0으로 기존 응용 프로그램 업그레이드](miscellaneous/1x-2x-upgrade.md)**

## <a name="get-entity-framework-core"></a>Entity Framework Core 구하기

사용할 데이터베이스 공급자에 대한 [NuGet 패키지를 설치합니다](https://docs.nuget.org/ndocs/quickstart/use-a-package). 예를 들어 명령줄에서 `dotnet` 도구를 사용하여 교차 플랫폼 개발에서 SQL Server 공급자를 설치하려면

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

또는 Visual Studio에서 패키지 관리자 콘솔을 사용합니다.

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
사용 가능한 공급자에 대한 정보는 [데이터베이스 공급자](providers/index.md), 더 자세한 설치 단계는 [EF Core 설치](get-started/install/index.md)를 참조하세요.

## <a name="the-model"></a>모델

EF Core에서는 데이터 액세스가 모델을 통해 수행됩니다. 모델은 엔터티 클래스와, 데이터베이스와의 세션을 나타내는 파생된 컨텍스트로 구성되어 데이터를 쿼리하고 저장할 수 있습니다. 자세한 내용은 [모델 만들기](modeling/index.md)를 참조하세요.

기존 데이터베이스에서 모델을 생성하고 데이터베이스에 맞는 모델을 직접 작성하거나 EF 마이그레이션을 사용하여 모델로부터 데이터베이스를 만들 수 있습니다(이후 시간에 따라 모델 변경과 함께 확장).

``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace Intro
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }
        public int Rating { get; set; }
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

## <a name="querying"></a>쿼리

엔터티 클래스의 인스턴스는 LINQ(Language Integrated Query)를 사용하여 데이터베이스에서 검색됩니다. 자세한 내용은 [데이터 쿼리](querying/index.md)를 참조하세요.

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a>데이터 저장

데이터는 엔터티 클래스의 인스턴스를 통해 데이터베이스에서 만들어지고 삭제되며 수정됩니다. 자세한 내용은 자세한 내용은 [데이터 저장](saving/index.md)을 참조하세요.

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
