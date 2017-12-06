---
title: "테이블 매핑-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: 73957d9c77e6856bfb65e10e6b373c337101f7d9
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="table-mapping"></a>테이블 매핑

> [!NOTE]  
> 이 섹션의 구성을 일반적 관계형 데이터베이스에 적용 됩니다. 여기에 표시 된 확장 메서드를 사용할 수 있는 관계형 데이터베이스 공급자를 설치할 때 (공유 인해 *Microsoft.EntityFrameworkCore.Relational* 패키지).

매핑 테이블에서 쿼리 하는 및에 데이터베이스에 저장 하는 테이블 데이터를 식별 합니다.

## <a name="conventions"></a>규칙

규칙에 따라 각 엔터티에 동일한 이름 갖는 테이블에 매핑하기 위해 설치 프로그램 됩니다는 `DbSet<TEntity>` 파생 컨텍스트에 엔터티를 노출 하는 속성입니다. 하지 않으면 `DbSet<TEntity>` 포함 된 지정된 된 엔터티의 클래스 이름이 사용 됩니다.

## <a name="data-annotations"></a>데이터 주석

형식에 매핑되는 테이블을 구성 하려면 데이터 주석을 사용할 수 있습니다.

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

테이블이 속한 스키마를 지정할 수 있습니다.

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API

형식에 매핑되는 테이블을 구성 하려면 Fluent API를 사용할 수 있습니다.

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

테이블이 속한 스키마를 지정할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
