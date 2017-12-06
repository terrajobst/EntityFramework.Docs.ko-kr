---
title: "상속 (관계형 데이터베이스)-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: a7f697dfe2b93c7b93a2dd14945732db4f37628c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="inheritance-relational-database"></a>상속 (관계형 데이터베이스)

> [!NOTE]  
> 이 섹션의 구성을 일반적 관계형 데이터베이스에 적용 됩니다. 여기에 표시 된 확장 메서드를 사용할 수 있는 관계형 데이터베이스 공급자를 설치할 때 (공유 인해 *Microsoft.EntityFrameworkCore.Relational* 패키지).

상속 EF 모델에는 데이터베이스에서 엔터티 클래스에서 상속 표현 방법을 제어 하는 데 사용 됩니다.

## <a name="conventions"></a>규칙

규칙에 따라 테이블 계층당 (TPH) 패턴을 사용 하 여 상속 매핑되지 것입니다. TPH 계층의 모든 형식에 대 한 데이터를 저장 하는 단일 테이블을 사용 합니다. 판별자 열은 각 행이 나타내는 유형을 식별 하는 데 사용 됩니다.

EF는 두 개 이상의 상속 된 형식이 모델에 명시적으로 포함 된 경우에 상속을 설치 (참조 [상속](../inheritance.md) 자세한 세부 정보에 대 한).

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
