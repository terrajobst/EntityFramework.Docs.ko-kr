---
title: "키 (기본)-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
ms.technology: entity-framework-core
uid: core/modeling/keys
ms.openlocfilehash: f3bf3c7f2a28e065b350fe000a5164406cd5ca08
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="keys-primary"></a>키 (기본)

키를 각 엔터티 인스턴스에 대 한 기본 고유 식별자로 사용 됩니다. 개념에 매핑됩니다 관계형 데이터베이스를 사용 하는 경우는 *기본 키*합니다. 기본 키가 아닌 고유 식별자를 구성할 수도 있습니다 (참조 [대체 키](alternate-keys.md) 자세한 정보에 대 한).

## <a name="conventions"></a>규칙

라는 속성 규칙에 따라 `Id` 또는 `<type name>Id` 엔터티의 키로 구성 됩니다.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a>데이터 주석

단일 속성을 엔터티 키를 구성 하려면 데이터 주석을 사용할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=3,4)] -->
``` csharp
class Car
{
    [Key]
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API

단일 속성을 엔터티 키를 구성 하려면 Fluent API를 사용할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => c.LicensePlate);
    }
}

class Car
{
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

또한 여러 속성을 엔터티 (복합 키 라고도 함)의 키를 구성 하려면 Fluent API를 사용할 수 있습니다. 복합 키를 구성 해야 Fluent API를 사용 하 여-규칙은 복합 키를 설치 하지 하 고 하나를 구성 하려면 데이터 주석을 사용 하지 않는 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public string State { get; set; }
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```