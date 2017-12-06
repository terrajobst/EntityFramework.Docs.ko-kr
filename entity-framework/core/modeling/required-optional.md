---
title: "필수/선택 속성-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
ms.technology: entity-framework-core
uid: core/modeling/required-optional
ms.openlocfilehash: 2af1d49e12ef980f81cb9c00556dee471673ccae
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="required-and-optional-properties"></a>필수 및 선택적 속성

속성에 포함 하려면 유효한 경우 선택적으로 간주 됩니다 `null`합니다. 경우 `null` 는 잘못 된 값은 필수 속성으로 간주 됩니다 속성에 할당할 수 있습니다.

## <a name="conventions"></a>규칙

규칙에 따라 해당 CLR 형식이 null를 포함할 수 있는 속성 구성 될 옵션으로 (`string`, `int?`, `byte[]`등.). CLR 형식이 null을 포함할 수 없습니다 하는 속성을 구성할 수는 필요에 따라 (`int`, `decimal`, `bool`등.).

> [!NOTE]  
> CLR 형식이 null을 포함할 수 없는 속성 옵션으로 구성할 수 없습니다. Entity Framework에 필요한 속성을 고려 항상 됩니다.

## <a name="data-annotations"></a>데이터 주석

속성이 필수임을 나타내기 위해 데이터 주석을 사용할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API

속성이 필수임을 나타내기 위해 Fluent API를 사용할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
