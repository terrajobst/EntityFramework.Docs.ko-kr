---
title: 테이블 매핑-EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: ef6080b5d543c2a68a680be2b9effc1c9d531030
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949011"
---
# <a name="table-mapping"></a>테이블 매핑

> [!NOTE]  
> 이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다. 여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).

테이블 매핑 테이블 데이터에서 쿼리 및 데이터베이스에 저장 해야 식별 합니다.

## <a name="conventions"></a>규칙

규칙에 따라 각 엔터티에 설정할 같은 이름의 테이블에 매핑하는 `DbSet<TEntity>` 파생 컨텍스트의 엔터티를 노출 하는 속성입니다. 없으면 `DbSet<TEntity>` 포함 된 지정된 된 엔터티 클래스 이름이 사용 됩니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석 형식에 매핑되는 테이블을 구성 하려면 사용할 수 있습니다.

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

형식에 매핑되는 테이블을 구성 하는 Fluent API를 사용할 수 있습니다.

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

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
