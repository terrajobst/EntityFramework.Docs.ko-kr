---
title: 기본 키-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: 916f3adbcd08cb1037c7fbf68e99630feb321a61
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998070"
---
# <a name="primary-keys"></a>기본 키

> [!NOTE]  
> 이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다. 여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).

각 엔터티 형식의 키에 대 한 기본 키 제약 조건을 도입 되었습니다.

## <a name="conventions"></a>규칙

규칙에 따라 데이터베이스의 기본 키 이름은 `PK_<type name>`합니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 기본 키의 없습니다 관계형 데이터베이스 특정 측면을 구성할 수 있습니다.

## <a name="fluent-api"></a>Fluent API

데이터베이스의 기본 키 제약 조건의 이름을 구성 하려면 Fluent API를 사용할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/KeyName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasKey(b => b.BlogId)
            .HasName("PrimaryKey_BlogId");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
