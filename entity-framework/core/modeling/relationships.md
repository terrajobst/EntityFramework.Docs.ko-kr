---
title: 관계-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 1732d32643effb0f12111191ed4ba3abb4182d93
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26053033"
---
# <a name="relationships"></a>Relationships

관계는 두 엔터티 간에 서로 관계 된 합니다. 관계형 데이터베이스에 외래 키 제약 조건에 의해 표시 됩니다.

> [!NOTE]  
> 이 문서에 있는 샘플 대부분 개념을 보여 주기 위해는 일 대 다 관계를 사용 합니다. 일대일 및 다 대 다 관계의 예제는 [다른 관계 패턴](#other-relationship-patterns) 문서의 끝에 나오는 섹션.

## <a name="definition-of-terms"></a>용어 정의

관계를 설명 하는 데 사용 하는 약관의 여러 가지

* **종속 엔터티에:** 외래 키 속성을 포함 하는 엔터티입니다. 관계의 'child' 라고도 합니다.

* **주 엔터티:** 기본/대체 키 속성을 포함 하는 엔터티입니다. 관계의 'parent' 라고도 합니다.

* **외래 키:** 엔터티 관련 된 주 키 속성의 값을 저장 하는 데 사용 되는 종속 엔터티의 속성입니다.

* **주 키:** 주 엔터티를 고유 하 게 식별 하는 속성입니다. 이 기본 키 또는 대체 키 수 있습니다.

* **탐색 속성:** 관련된 엔터티를 참조를 포함 하는 보안 주체 및/또는 종속 엔터티에 정의 된 속성입니다.

  * **컬렉션 탐색 속성:** 많은 관련된 엔터티에 대 한 참조를 포함 하는 탐색 속성입니다.

  * **탐색 속성을 참조:** 단일 관련 엔터티에 대 한 참조를 포함 하는 탐색 속성입니다.

  * **역 탐색 속성:** 특정 탐색 속성을 설명할 때이 용어는 관계의 반대쪽에 탐색 속성입니다.

다음 코드 목록 간의 일 대 다 관계를 보여 줍니다. `Blog` 및`Post`

* `Post`종속 엔터티는

* `Blog`주 엔터티는

* `Post.BlogId`외래 키

* `Blog.BlogId`(이 경우에 대체 키가 아닌 기본 키)는 주 키

* `Post.Blog`속성은 참조 탐색 속성은

* `Blog.Posts`컬렉션 탐색 속성

* `Post.Blog`역 탐색 속성은 `Blog.Posts` (및 그 반대의 경우도 마찬가지)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>규칙

규칙에 따라 형식에서 검색 하는 탐색 속성에 있으면 관계가 만들어집니다. 이것이 가리키는 형식이 매핑될 수 없습니다. 스칼라 형식으로 현재 데이터베이스 공급자가 하는 경우 속성 탐색 속성을 간주 됩니다.

> [!NOTE]  
> 규칙으로 검색 되는 관계를 주 엔터티의 기본 키를 대상 항상 합니다. 대체 키를 대상으로 Fluent API를 사용 하 여 추가 구성을 수행 해야 합니다.

### <a name="fully-defined-relationships"></a>완전 하 게 정의 된 관계

관계에 대 한 가장 일반적인 패턴은 종속 엔터티 클래스에 정의 된 외래 키 속성 및 관계의 양쪽 끝에 정의 된 탐색 속성입니다.

* 두 형식 간에 한 쌍의 탐색 속성 항목이 있으면 동일한 관계의 역 탐색 속성으로 구성 됩니다.

* 종속 엔터티 속성을 포함 하는 경우 `<primary key property name>`, `<navigation property name><primary key property name>`, 또는 `<principal entity name><primary key property name>` 다음 외래 키로 구성 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> 두 형식 간의 정의 된 여러 탐색 속성이 있는 경우 (즉, 서로를 가리키는 탐색의 고유한 쌍을 둘 이상의) 및 규칙에 따라 관계가 없으면 만들어집니다 다음 수동으로 식별 하도록를 구성 해야 합니다는 방법을 위쪽 탐색 속성 쌍입니다.

### <a name="no-foreign-key-property"></a>외래 키 속성 없음

종속 엔터티 클래스에 정의 된 외래 키 속성이 있어야 하는 것이 좋지만, 필요 하지 않습니다. 그림자 외래 키 속성 이름으로 도입 될 것 없는 외래 키 속성이 있으면 `<navigation property name><principal key property name>` (참조 [그림자 속성](shadow-properties.md) 자세한 정보에 대 한).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>단일 탐색 속성

(없음 역 탐색 및 외래 키 속성 없음) 한 탐색 속성을 포함 하 여 관계가 규칙에 따라 정의 됩니다. 또한 단일 탐색 속성 및 외래 키 속성을 가질 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>하위 삭제

규칙에 따라 하위 삭제로 설정 됩니다 *Cascade* 필요한 관계에 대 한 및 *ClientSetNull* 선택적 관계에 대 한 합니다. *Cascade* 의미 종속 된 엔터티도 삭제 됩니다. *ClientSetNull* 메모리에 로드 되지 않는 종속 엔터티가 유지 되는 의미 변경 하지 않고 및 수동으로 삭제 하거나 업데이트 해야 유효한 주 엔터티를 가리키도록 합니다. 메모리에 로드 되는 엔터티, EF 코어 외래 키 속성을 null로 설정 하려고 시도 합니다.

참조는 [필수 및 선택적 관계](#required-and-optional-relationships) 필수 및 선택적 관계 간의 차이 대 한 섹션.

참조 [모두 삭제](../saving/cascade-delete.md) 서로 다른 방법에 대 한 자세한 내용은 동작 및 규칙에 따라 사용 하는 기본값을 삭제 합니다.

## <a name="data-annotations"></a>데이터 주석

관계를 구성 하는 데 사용할 수 있는 두 개의 데이터 주석이 `[ForeignKey]` 및 `[InverseProperty]`합니다.

### <a name="foreignkey"></a>[ForeignKey]

주어진된 관계에 대 한 외래 키 속성으로 사용할 해야 속성을 구성 하려면 데이터 주석을 사용할 수 있습니다. 이 규칙에 따라 외래 키 속성이 검색 되지 않는 경우 일반적으로 수행 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> `[ForeignKey]` 주석 관계에서 두 탐색 속성 중 하나에 배치할 수 있습니다. 종속 엔터티 클래스에는 탐색 속성에는 필요 하지 않습니다.

### <a name="inverseproperty"></a>[InverseProperty]

종속 및 주체 엔터티에 대 한 탐색 속성 쌍 연결 하는 방법을 구성 하려면 데이터 주석을 사용할 수 있습니다. 이 두 엔터티 형식 간의 탐색 속성 쌍 하나만 있으면 일반적으로 수행 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a>Fluent API

Fluent API에서 관계를 구성 하려면 먼저 관계를 구성 하는 탐색 속성을 식별 합니다. `HasOne`또는 `HasMany` 에서 구성을 시작 하는 엔터티 형식의 탐색 속성을 식별 합니다. 다음에 대 한 호출 체인 `WithOne` 또는 `WithMany` 역 탐색을 식별할 수 있습니다. `HasOne`/`WithOne`참조 탐색 속성에 사용 되 고 `HasMany` / `WithMany` 컬렉션 탐색 속성에 사용 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a>단일 탐색 속성

한 탐색 속성만 있는 경우 매개 변수가 없는 오버 로드가 `WithOne` 및 `WithMany`합니다. 개념적으로 참조 또는 컬렉션, 관계의 다른 쪽 end에 있지만 엔터티 클래스에 포함 된 탐색 속성이 없기 나타냅니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a>외래 키

주어진된 관계에 대 한 외래 키 속성으로 사용할 해야 속성을 구성 하려면 Fluent API를 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

다음 코드 샘플에는 복합 외래 키를 구성 하는 방법을 보여 줍니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

문자열 오버 로드를 사용할 수 있습니다 `HasForeignKey(...)` 외래 키로 그림자 속성을 구성 하려면 (참조 [그림자 속성](shadow-properties.md) 자세한 정보에 대 한). 모델에 (아래와 같이) 외래 키로 사용 하기 전에 섀도 속성을 명시적으로 추가 하는 것이 좋습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a>보안 주체가 키

외래 키 기본 키 이외의 속성을 참조 하려는 경우에 관계에 대 한 주 키 속성을 구성 하려면 Fluent API를 사용할 수 있습니다. 대체 키 파일로 설치할 하면 주 키가 자동으로 구성 하는 속성 (참조 [대체 키](alternate-keys.md) 자세한 정보에 대 한).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

다음 코드 샘플에 복합 기본 키를 구성 하는 방법을 보여 줍니다.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> 주 키 속성을 지정할 수 있는 순서에는 외래 키에 대 한 지정 된 순서와 일치 해야 합니다.

### <a name="required-and-optional-relationships"></a>필수 및 선택적 관계

관계를 필수 또는 선택적 구성 하려면 Fluent API를 사용할 수 있습니다. 궁극적으로 외래 키 속성이 필수 인지 선택적인 지 여부를 제어 합니다. 그림자 상태 외래 키를 사용 하는 경우 가장 유용 합니다. 엔터티 클래스에서 외래 키 속성이 있는 경우 requiredness 관계의 외래 키 속성이 필수 인지 선택적인 여부에 따라 결정 됩니다 (참조 [필수 및 선택적 속성](required-optional.md) 더 정보)입니다.

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
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
            .IsRequired();
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

    public Blog Blog { get; set; }
}
```

### <a name="cascade-delete"></a>하위 삭제

주어진된 관계에 대 한 cascade delete 동작을 명시적으로 구성 하려면 Fluent API를 사용할 수 있습니다.

참조 [모두 삭제](../saving/cascade-delete.md) 각 옵션에 대 한 자세한 내용은 대 한 Data 섹션.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
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
            .OnDelete(DeleteBehavior.Cascade);
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

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a>다른 관계 패턴

### <a name="one-to-one"></a>한 일

한 일 관계 양쪽에서 참조 탐색 속성을 가집니다. 일 대 다 관계와 동일한 규칙을 따르지만 고유 인덱스는 하나만 종속 각 보안 주체에 관련 되도록 외래 키 속성에 도입 되었습니다.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> EF 외래 키 속성을 검색 하는 기능에 따라 종속 되도록 엔터티 중 하나를 선택 합니다. 종속 항목으로 잘못 된 엔터티를 선택 하면이 문제를 해결 Fluent API를 사용할 수 있습니다.

관계 Fluent API를 구성할 때 사용 된 `HasOne` 및 `WithOne` 메서드.

종속 엔터티 형식-을 지정 해야 하는 외래 키를 구성 하는 경우에 제공 되는 제네릭 매개 변수 확인 `HasForeignKey` 아래 목록에 있습니다. 1 대 다 관계에서 참조 탐색 엔터티가 종속 항목 및 컬렉션에 있는 주 서버는 명확있지 않습니다. 있지만이 따라서 일대일 관계에는 명시적으로 정의 해야 합니다.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a>다 대 다

다 대 다 관계를 나타내는 조인 테이블 엔터티 클래스 없이 아직 지원 되지 않습니다. 그러나 별도 두 대 다 관계 매핑 및 조인 테이블에 대 한 엔터티 클래스를 포함 하 여 다 대 다 관계를 나타낼 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
