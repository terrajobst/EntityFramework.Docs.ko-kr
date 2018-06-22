---
title: 대체 키-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
ms.technology: entity-framework-core
uid: core/modeling/alternate-keys
ms.openlocfilehash: 09f86a8932b71ec8f30ee90a088091a00233c20f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052473"
---
# <a name="alternate-keys"></a>대체 키

대체 키의 기본 키 외에도 각 엔터티 인스턴스에 대 한 대체 고유 식별자로 사용 됩니다. 대체 키 관계의 대상으로 사용할 수 있습니다. 관계형 데이터베이스를 사용 하는 경우에 대체 키 열과 하나 이상의 외래 키 제약 조건이 있는 열을 참조 하는 고유 인덱스/제약 조건의 개념에 매핑됩니다.

> [!TIP]  
> 대체 키 보다는 고유 인덱스를 원하는 다음 열의 고유성을 강제 적용 하려면 참조 [인덱스](indexes.md)합니다. EF, 대체 키 외래 키의 대상으로 사용할 수 있기 때문에 고유 인덱스에 비해 향상 된 기능을 제공 합니다.

필요할 때에 대 한 대체 키 일반적으로 도입 하 고 수동으로 구성할 필요가 없습니다. 참조 [규칙](#conventions) 내용을 확인 합니다.

## <a name="conventions"></a>규칙

기본 키 관계의 대상으로 없는 속성을 식별 하는 경우 규칙에 따라 대체 키를 도입 되었습니다.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/AlternateKey.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogUrl)
            .HasPrincipalKey(b => b.Url);
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

    public string BlogUrl { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 대체 키를 구성할 수 있습니다.

## <a name="fluent-api"></a>Fluent API

대체 키 수를 단일 속성을 구성 하려면 Fluent API를 사용할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate);
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```

대체 키 (복합 대체 키로 알려짐)를 여러 속성을 구성을 Fluent API를 사용할 수도 있습니다.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```
