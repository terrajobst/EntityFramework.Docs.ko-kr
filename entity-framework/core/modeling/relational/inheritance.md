---
title: 상속 (관계형 데이터베이스)-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 22eed0002b5903d3cfd18a7e4af0fcd2d46a5c4c
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152365"
---
# <a name="inheritance-relational-database"></a>상속 (관계형 데이터베이스)

> [!NOTE]  
> 이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다. 여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).

상속 EF 모델에는 데이터베이스에서 엔터티 클래스에서 상속 표현 방법을 제어 하는 데 사용 됩니다.

> [!NOTE]  
> 현재 계층-테이블 (TPH) 패턴만 EF 코어에서 구현 됩니다. 테이블 콘크리트-형식당 하나의 TPC ()는 사용할 수 없습니다 및 다른 일반 패턴도 같은 테이블 형식당 (TPT).

## <a name="conventions"></a>규칙

규칙에 따라 테이블 계층당 (TPH) 패턴을 사용 하 여 상속 매핑되지 것입니다. TPH 계층의 모든 형식에 대 한 데이터를 저장 하는 단일 테이블을 사용 합니다. 판별자 열은 각 행이 나타내는 유형을 식별 하는 데 사용 됩니다.

EF 코어는 두 개 이상의 상속 된 형식이 모델에 명시적으로 포함 된 경우에 상속을 설치 (참조 [상속](../inheritance.md) 자세한 세부 정보에 대 한).

다음은 간단한 상속 시나리오 및 TPH 패턴을 사용 하 여 관계형 데이터베이스 테이블에 저장 된 데이터를 보여 주는 예제입니다. *판별자* 열 유형을 식별 *블로그* 각 행에 저장 됩니다.

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

상속을 구성 하 데이터 주석을 사용할 수 없습니다.

## <a name="fluent-api"></a>Fluent API

판별자 열 및 계층 구조에서 각 유형을 식별 하는 데 사용 되는 값의 형식과 이름을 구성 하려면 Fluent API를 사용할 수 있습니다.

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

위의 예에 판별자로 만들어집니다는 [속성 그림자](xref:core/modeling/shadow-properties) 에 계층의 기준 엔터티입니다. 모델의 속성 이므로, 다른 속성과 마찬가지로 구성할 수 있습니다. 예를 들어, 기본값, 규칙을 통한 판별자를 사용 하는 경우 최대 길이 설정 하려면:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

판별자는 엔터티의 실제 CLR 속성에도 매핑할 수 있습니다. 예:
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

이러한 두 가지를 함께 결합 수 있기를 모두 판별자의 실제 속성 매핑 구성:
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
