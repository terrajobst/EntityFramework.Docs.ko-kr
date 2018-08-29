---
title: 인덱스-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 87fe893243377e3ab83d419ae9bedf813ca50c3f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995482"
---
# <a name="indexes"></a>인덱스

인덱스는 많은 데이터 저장소 간에 일반적인 개념입니다. 데이터 저장소에서 해당 구현은 다를 수 있지만 사용 되는 열 (또는 열의 집합)에 따라 조회 게 효율적입니다.

## <a name="conventions"></a>규칙

규칙에 따라 각 속성 (또는 속성 집합)에서 외래 키로 사용 되는 인덱스가 만들어집니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 인덱스를 만들 수 있습니다.

## <a name="fluent-api"></a>Fluent API

단일 속성에 대 한 인덱스를 지정 하는 Fluent API를 사용할 수 있습니다. 기본적으로 인덱스가 고유 하지 않은 경우

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Index.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

지정할 수도 있습니다 인덱스 고유 되도록 두 개의 엔터티가 없습니다. 지정 된 속성에 대 한 동일한 값을 가질 수 있다는 의미입니다.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

또한 둘 이상의 열을 통해 인덱스를 지정할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .HasIndex(p => new { p.FirstName, p.LastName });
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

> [!TIP]  
> 고유 속성 집합이 당 인덱스는 하나만 있습니다. Fluent API를 사용 하 여 규칙 또는 이전 구성을 정의 된 인덱스에 이미 있는 속성 집합에 인덱스를 구성 하는 경우 다음는 변경할 해당 인덱스의 정의 합니다. 이 규칙에 따라 생성 된 인덱스를 추가로 구성 하려는 경우에 유용 합니다.
