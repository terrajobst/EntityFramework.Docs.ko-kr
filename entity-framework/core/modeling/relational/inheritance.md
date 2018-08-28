---
title: 상속 (관계형 데이터베이스)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 019893ec8268ef9e59d581799a13d63610c80616
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996324"
---
# <a name="inheritance-relational-database"></a>상속 (관계형 데이터베이스)

> [!NOTE]  
> 이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다. 여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).

EF 모델의 상속에는 데이터베이스에서 엔터티 클래스에서 상속은 표시 하는 방법을 제어 하는 데 사용 됩니다.

> [!NOTE]  
> 현재, 계층당 하나의 테이블 (TPH) 패턴에만 EF Core에서 구현 됩니다. 다른 일반 패턴도 테이블 형식당 (TPT) 등의 테이블 구체적인-형식당 하나의 TPC ()는 사용할 수 없습니다.

## <a name="conventions"></a>규칙

규칙에 따라 상속 매핑될 당 계층당 테이블 (TPH) 패턴을 사용 합니다. TPH는 계층의 모든 형식에 대 한 데이터를 저장 하는 단일 테이블을 사용 합니다. 판별자 열은 각 행이 나타내는 하는 형식을 식별 하기 위해 사용 됩니다.

두 개 이상의 상속 된 형식이 모델에 명시적으로 포함 된 경우 EF Core에서 상속만 설치 됩니다 (참조 [상속](../inheritance.md) 자세한).

다음은 간단한 상속 시나리오 및 패턴을 TPH를 사용 하 여 관계형 데이터베이스 테이블에 저장 된 데이터를 보여 주는 예제입니다. 합니다 *판별자* 열의 형식을 식별 *블로그* 각 행에 저장 됩니다.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![이미지](_static/inheritance-tph-data.png)

## <a name="data-annotations"></a>데이터 주석

상속을 구성 하려면 데이터 주석을 사용할 수 없습니다.

## <a name="fluent-api"></a>Fluent API

판별자 열 및 계층의 각 형식을 식별 하는 데 사용 되는 값의 종류와 이름을 구성 하는 Fluent API를 사용할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

## <a name="configuring-the-discriminator-property"></a>판별자 속성 구성

위의 예제에서 판별자로 만들어집니다를 [섀도 속성](xref:core/modeling/shadow-properties) 에서 계층의 기준 엔터티입니다. 모델의 속성 이므로 다른 속성과 마찬가지로 구성할 수 있습니다. 예를 들어, 기본값, 규칙에 따라 판별자를 사용 하는 경우 최대 길이 설정 하려면:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

판별자 엔터티에 실제 CLR 속성에도 매핑할 수 있습니다. 예를 들어:
```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

이러한 두 가지를 결합 있기를 모두 매핑하고 판별자의 실제 속성 구성:
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
