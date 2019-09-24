---
title: 상속 (관계형 데이터베이스)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: a7fb19f9c86d1768967d172c006eb5d894254e0c
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196937"
---
# <a name="inheritance-relational-database"></a>상속(관계형데이터베이스)

> [!NOTE]  
> 이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다. 여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).

EF 모델의 상속은 데이터베이스에서 엔터티 클래스의 상속이 표시 되는 방식을 제어 하는 데 사용 됩니다.

> [!NOTE]  
> 현재는 EF Core에서 TPH (계층당 하나의 테이블) 패턴만 구현 됩니다. 형식당 하나의 테이블 (TPT) 및 TPC (테이블당)와 같은 기타 일반적인 패턴은 아직 사용할 수 없습니다.

## <a name="conventions"></a>규칙

규칙에 따라 상속은 TPH (계층당 하나의 테이블) 패턴을 사용 하 여 매핑됩니다. TPH는 단일 테이블을 사용 하 여 계층의 모든 형식에 대 한 데이터를 저장 합니다. 판별자 열은 각 행이 나타내는 유형을 식별 하는 데 사용 됩니다.

EF Core는 둘 이상의 상속 된 형식이 모델에 명시적으로 포함 된 경우에만 상속을 설정 합니다 (자세한 내용은 [상속](../inheritance.md) 참조).

다음은 TPH 패턴을 사용 하 여 관계형 데이터베이스 테이블에 저장 된 데이터와 간단한 상속 시나리오를 보여 주는 예입니다. *판별자* 열은 각 행에 저장 되는 *블로그의* 유형을 식별 합니다.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/InheritanceDbSets.cs)] -->
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

>[!NOTE]
> TPH 매핑을 사용 하는 경우 필요에 따라 데이터베이스 열이 자동으로 nullable로 설정 됩니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석은 상속을 구성 하는 데 사용할 수 없습니다.

## <a name="fluent-api"></a>Fluent API

흐름 API를 사용 하 여 판별자 열의 이름 및 유형과 계층의 각 유형을 식별 하는 데 사용 되는 값을 구성할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
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

위의 예제에서 판별자는 계층의 기본 엔터티에서 [그림자 속성](xref:core/modeling/shadow-properties) 으로 생성 됩니다. 이 속성은 모델의 속성 이므로 다른 속성과 마찬가지로 구성할 수 있습니다. 예를 들어, 기본 규칙 판별자를 사용 하는 경우 최대 길이를 설정 하려면 다음을 수행 합니다.

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

판별자는 엔터티의 실제 CLR 속성에 매핑될 수도 있습니다. 예:
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

이러한 두 항목을 함께 결합 하 여 판별자를 실제 속성에 매핑하고 구성할 수 있습니다.
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
