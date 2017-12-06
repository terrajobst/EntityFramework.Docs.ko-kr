---
title: "인덱스-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: 683b580bb155e0416f13c5d63e3280078fbcee21
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a>인덱스

> [!NOTE]  
> 이 섹션의 구성을 일반적 관계형 데이터베이스에 적용 됩니다. 여기에 표시 된 확장 메서드를 사용할 수 있는 관계형 데이터베이스 공급자를 설치할 때 (공유 인해 *Microsoft.EntityFrameworkCore.Relational* 패키지).

관계형 데이터베이스에서 인덱스 Entity Framework의 핵심에서 인덱스와 동일한 개념에 매핑됩니다.

## <a name="conventions"></a>규칙

인덱스 이름은 규칙에 따라 `IX_<type name>_<property name>`합니다. 복합 인덱스에 대 한 `<property name>` 속성 이름의 밑줄 구분 된 목록이 됩니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 인덱스를 구성할 수 있습니다.

## <a name="fluent-api"></a>Fluent API

인덱스의 이름을 구성 하려면 Fluent API를 사용할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/IndexName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .HasName("Index_Url");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
