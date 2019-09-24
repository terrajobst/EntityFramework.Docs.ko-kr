---
title: 테이블 매핑-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 62dce317b901bc862b3c7d20ed1d176805bb24dd
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196961"
---
# <a name="table-mapping"></a>테이블 매핑

> [!NOTE]  
> 이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다. 여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).

테이블 매핑은 데이터베이스에서 쿼리하고 저장할 테이블 데이터를 식별 합니다.

## <a name="conventions"></a>규칙

규칙에 따라 각 엔터티는 파생 컨텍스트에 엔터티를 노출하는 `DbSet<TEntity>` 속성과 이름이 같은 테이블에 매핑되도록 설정됩니다. 지정 된 `DbSet<TEntity>` 엔터티에가 포함 되어 있지 않으면 클래스 이름이 사용 됩니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 형식이 매핑되는 테이블을 구성할 수 있습니다.

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

테이블이 속한 스키마를 지정할 수도 있습니다.

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API

흐름 API를 사용 하 여 형식이 매핑되는 테이블을 구성할 수 있습니다.

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

테이블이 속한 스키마를 지정할 수도 있습니다.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
