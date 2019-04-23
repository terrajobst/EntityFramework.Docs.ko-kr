---
title: 관계-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 9ef1a9269fc99f5b27a81c11a161ed5f9d74180d
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929939"
---
# <a name="relationships"></a>관계

두 엔터티를 정의 하는 관계를 서로 연결 합니다. 관계형 데이터베이스에서 외래 키 제약 조건이 표시 됩니다.

> [!NOTE]  
> 대부분이 문서의 샘플의 개념을 설명 하에 일 대 다 관계를 사용 합니다. 한 일 및 다 대 다 관계의 예제를 보려면 합니다 [다른 관계 패턴](#other-relationship-patterns) 문서의 끝에 있는 섹션입니다.

## <a name="definition-of-terms"></a>용어 정의

관계를 설명 하는 데 사용 되는 약관의 여러 가지

* **종속 엔터티:** 이 외래 키 속성을 포함 하는 엔터티입니다. 관계의 'child' 라고도 합니다.

* **주 엔터티:** 이 기본/대체 키 속성을 포함 하는 엔터티입니다. 관계의 'parent' 라고도 합니다.

* **외래 키:** 엔터티 관련 된 보안 주체 키 속성의 값을 저장 하는 데 사용 되는 종속 엔터티의 속성입니다.

* **계정 키:** 주 엔터티를 고유 하 게 식별 하는 속성입니다. 이 기본 키 또는 대체 키 수 있습니다.

* **탐색 속성:** 관련된 엔터티를 대 한 참조를 포함 하는 보안 주체 및/또는 종속 엔터티에 정의 된 속성입니다.

  * **컬렉션 탐색 속성:** 여러 관련된 엔터티에 대 한 참조를 포함 하는 탐색 속성입니다.

  * **참조 탐색 속성:** 단일 관련 엔터티에 대 한 참조를 보유 하는 탐색 속성입니다.

  * **역 탐색 속성:** 특정 탐색 속성에 설명 하는 경우이 용어 관계의 다른 쪽에 탐색 속성을 가리킵니다.

다음 코드 목록 사이 일 대 다 관계를 보여 줍니다. `Blog` 및 `Post`

* `Post` 종속 엔터티는

* `Blog` 주 엔터티는

* `Post.BlogId` 외래 키

* `Blog.BlogId` 계정 키 (이 경우에 대체 키를 사용 하지 않고 기본 키)

* `Post.Blog` 참조 탐색 속성

* `Blog.Posts` 컬렉션 탐색 속성은

* `Post.Blog` 역 탐색 속성인 `Blog.Posts` (또는 그 반대로)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>규칙

규칙에 따라 형식에서 검색 하는 탐색 속성 경우 관계가 만들어집니다. 속성을 가리키는 형식이 매핑될 수 없습니다. 스칼라 형식으로 현재 데이터베이스 공급자가 하는 경우 탐색 속성을 간주 됩니다.

> [!NOTE]  
> 주 엔터티의 기본 키 규칙에서 검색 된 관계 항상 대상으로 합니다. 대체 키를 대상으로 Fluent API를 사용 하 여 추가 구성을 수행 해야 합니다.

### <a name="fully-defined-relationships"></a>완벽 하 게 정의 된 관계

관계에 대 한 가장 일반적인 패턴은 종속 엔터티 클래스에 정의 된 외래 키 속성 및 관계의 양쪽에 정의 된 탐색 속성입니다.

* 두 형식 간의 탐색 속성 쌍이 없으면 동일한 관계의 역 탐색 속성으로 구성 됩니다.

* 종속 엔터티 속성을 포함 하는 경우 `<primary key property name>`, `<navigation property name><primary key property name>`, 또는 `<principal entity name><primary key property name>` 다음 외래 키로 구성 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> 두 형식 간에 정의 된 여러 탐색 속성이 많은 경우 (즉, 서로를 가리키는 탐색의 고유한 쌍을 둘 이상), 다음 관계가 없으면 규칙에서 만들어지고 수동으로 식별 하도록를 구성 해야 하는 방법을 위로 탐색 속성 쌍입니다.

### <a name="no-foreign-key-property"></a>외래 키 속성 없음

종속 엔터티 클래스에 정의 된 외래 키 속성이 있어야 하는 것이 좋습니다, 있지만 필요 하지 않습니다. 섀도 외래 키 속성 이름의 도입 될 외래 키 속성이 없으면 `<navigation property name><principal key property name>` (참조 [섀도 속성](shadow-properties.md) 자세한).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>단일 탐색 속성

(역 없는 탐색 및 외래 키 속성이) 탐색 속성 중 하나를 포함 하 여 규칙에 따라 정의 된 관계에 적합 합니다. 단일 탐색 속성 및 외래 키 속성 수도 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>하위 삭제

규칙에 따라 계단식 삭제로 설정 됩니다 *Cascade* 필요한 관계에 대 한 및 *ClientSetNull* 선택적 관계에 대 한 합니다. *Cascade* 종속 엔터티도 삭제 됩니다 것을 의미 합니다. *ClientSetNull* 메모리에 로드 되지 않은 종속 엔터티가 남아 있음을 변경 되지 않음 및 수동으로 삭제 하거나 업데이트 해야 유효한 주 엔터티를 가리키도록 합니다. 메모리에 로드 되는 엔터티의 경우 EF Core는 외래 키 속성을 null로 설정 하려고 합니다.

참조 된 [필수 및 선택적 관계](#required-and-optional-relationships) 필수 및 선택적 관계 간의 차이점에 대 한 섹션입니다.

참조 [Cascade Delete](../saving/cascade-delete.md) 다양 한 방법에 대 한 자세한 내용은 삭제 동작과 규칙에 따라 사용 하는 기본값에 대 한 합니다.

## <a name="data-annotations"></a>데이터 주석

관계를 구성 하려면 사용할 수 있는 두 데이터 주석이 `[ForeignKey]` 고 `[InverseProperty]`입니다. 사용할 수는 `System.ComponentModel.DataAnnotations.Schema` 네임 스페이스입니다.

### <a name="foreignkey"></a>[ForeignKey]

지정 된 관계에 대 한 외래 키 속성으로 사용할 해야 속성을 구성 하려면 데이터 주석을 사용할 수 있습니다. 외래 키 속성은 규칙에 따라 검색 되지 경우 일반적으로 수행 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> `[ForeignKey]` 주석 관계의 탐색 속성 중 하나에 배치할 수 있습니다. 종속 엔터티 클래스의 탐색 속성을 이동 하지 않아도 됩니다.

### <a name="inverseproperty"></a>[InverseProperty]

종속 및 주체 엔터티에서 탐색 속성 쌍 방법을 구성 하려면 데이터 주석을 사용할 수 있습니다. 두 엔터티 형식 간의 탐색 속성 쌍이 하나만 있으면 일반적으로 수행 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a>Fluent API

Fluent API의 관계를 구성 하 여 관계를 구성 하는 탐색 속성을 식별 하 여 시작 합니다. `HasOne` 또는 `HasMany` 에서 구성을 시작 하는 엔터티 형식의 탐색 속성을 식별 합니다. 다음에 대 한 호출을 연결할 `WithOne` 또는 `WithMany` 역 탐색을 식별 합니다. `HasOne`/`WithOne` 참조 탐색 속성에 사용 되 고 `HasMany` / `WithMany` 컬렉션 탐색 속성에 사용 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a>단일 탐색 속성

하나의 탐색 속성이 있는 경우 매개 변수가 없는 오버 로드가 `WithOne` 고 `WithMany`입니다. 이 나타내지만 참조 또는 관계의 반대쪽 끝에서 수집 하는 개념적 엔터티 클래스에 포함 된 탐색 속성이 없습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a>외래 키

지정 된 관계에 대 한 외래 키 속성으로 사용할 해야 속성을 구성 하는 Fluent API를 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?highlight=17)]

다음 코드 샘플에는 복합 외래 키를 구성 하는 방법을 보여 줍니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?highlight=20)]

문자열 오버 로드를 사용할 수 있습니다 `HasForeignKey(...)` 외래 키로 섀도 속성을 구성 하려면 (참조 [섀도 속성](shadow-properties.md) 자세한). (아래와 같이) 외래 키로 사용 하기 전에 모델에 섀도 속성을 명시적으로 추가 하는 것이 좋습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a>주체 키

외래 키 기본 키 이외의 속성 참조를 원한다 면 관계에 대 한 주 키 속성을 구성 하려면 Fluent API를 사용할 수 있습니다. 대체 키로 설정할 키는 보안 주체를 자동으로 구성 하는 속성 (참조 [대체 키](alternate-keys.md) 자세한).

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

다음 코드 목록은 복합 기본 키를 구성 하는 방법을 보여 줍니다.

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
> 주체 키 속성을 지정 하는 순서는 외래 키에 대 한 지정 된 순서를 일치 해야 합니다.

### <a name="required-and-optional-relationships"></a>필수 및 선택적 관계

관계 필수 인지 선택적인 지 여부를 구성 하는 Fluent API를 사용할 수 있습니다. 궁극적으로이 외래 키 속성이 필수 인지 선택적인 지 여부를 제어 합니다. 섀도 상태 외래 키를 사용 하는 경우 가장 유용 합니다. 외래 키 속성이 엔터티 클래스에 있는 경우 requiredness 관계의 외래 키 속성의 필수 또는 선택 인지 여부에 따라 결정 됩니다 (참조 [필수 및 선택적 속성](required-optional.md) 자세한 정보)입니다.

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

지정 된 관계에 대 한 하위 삭제 동작을 명시적으로 구성 하는 Fluent API를 사용할 수 있습니다.

참조 [Cascade Delete](../saving/cascade-delete.md) 각 옵션에 자세히 알아보려면 저장 데이터 섹션에 있습니다.

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

한 일 관계 양쪽 모두에 대 한 참조 탐색 속성을 가집니다. 이러한에 일 대 다 관계와 동일한 규칙을 따르지만 고유 인덱스는 하나만 종속 각 보안 주체 관련 되도록 외래 키 속성에 도입 되었습니다.

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
> EF는 외래 키 속성을 검색 하는 기능에 따라 종속 되도록 엔터티 중 하나를 선택 합니다. 종속 항목으로 잘못 된 엔터티를 선택 하면이 문제를 해결 하려면 Fluent API를 사용할 수 있습니다.

Fluent API를 사용 하 여 관계를 구성할 때 사용 합니다 `HasOne` 고 `WithOne` 메서드.

종속 엔터티 형식으로 지정 해야 하는 외래 키를 구성 하는 경우 제공 된 제네릭 매개 변수가 알 수 있습니다. `HasForeignKey` 아래 목록에 있습니다. 1 대 다 관계 명확한 참조 탐색이 있는 엔터티가 종속 및 컬렉션을 사용 하 여 보안 주체가 되는 것입니다. 이 이지만 있으므로 한 일 관계에서 명시적으로 정의 해야 합니다.

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

다 대 다 관계 없이 조인 테이블을 나타내는 엔터티 클래스는 아직 지원 되지 않습니다. 그러나 조인 테이블에 별도 두 가지에 일 대 다 관계 매핑 엔터티 클래스를 포함 하 여 다 대 다 관계를 나타낼 수 있습니다.

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
