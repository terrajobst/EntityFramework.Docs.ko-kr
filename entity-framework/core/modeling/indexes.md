---
title: 인덱스-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: b6f11401b69bd8e8795f6b22e5392ba16fc9ba2e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197252"
---
# <a name="indexes"></a>인덱스

인덱스는 많은 데이터 저장소에서 일반적인 개념입니다. 데이터 저장소에서의 구현은 다를 수 있지만 열 (또는 열 집합)을 기반으로 조회를 보다 효율적으로 만드는 데 사용 됩니다.

## <a name="conventions"></a>규칙

규칙에 따라 외래 키로 사용 되는 각 속성 (또는 속성 집합)에 인덱스가 생성 됩니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 인덱스를 만들 수 없습니다.

## <a name="fluent-api"></a>Fluent API

흐름 API를 사용 하 여 단일 속성에 대 한 인덱스를 지정할 수 있습니다. 기본적으로 인덱스는 고유 하지 않습니다.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Index.cs?highlight=7,8)] -->
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

인덱스가 고유 하도록 지정할 수도 있습니다. 즉, 두 엔터티가 지정 된 속성에 대해 동일한 값을 가질 수 없습니다.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

둘 이상의 열에 대해 인덱스를 지정할 수도 있습니다.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexComposite.cs?highlight=7,8)] -->
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
> 속성의 고유 집합 마다 하나의 인덱스만 있습니다. 흐름 API를 사용 하 여 인덱스를 이미 정의 하는 속성 집합에서 규칙 또는 이전 구성으로 인덱스를 구성 하는 경우 해당 인덱스의 정의가 변경 됩니다. 규칙으로 만들어진 인덱스를 추가로 구성 하려는 경우에 유용 합니다.
