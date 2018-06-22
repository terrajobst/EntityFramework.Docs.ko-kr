---
title: 최대 길이-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
ms.technology: entity-framework-core
uid: core/modeling/max-length
ms.openlocfilehash: 7325c0c3328477473392bf9e7c82f1696bb4f424
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052673"
---
# <a name="maximum-length"></a>최대 길이

최대 길이 구성 합니다. 지정된 된 속성에 사용할 적절 한 데이터 형식에 대 한 데이터 저장소에 대 한 힌트를 제공 합니다. 최대 길이에 적용 됩니다 배열 데이터 형식 같은 `string` 및 `byte[]`합니다.

> [!NOTE]  
> Entity Framework 데이터 공급자에 게 전달 하기 전에 최대 길이 대 한 유효성 검사를 수행 하지 않습니다. 적절 한 있는지 검사 하기 위해 공급자 또는 데이터 저장소는 합니다. 예를 들어 기본 열의 데이터 형식으로 예외가 발생 합니다 최대 길이 초과 하는 SQL Server를 대상으로 지정은 허용 하지 않습니다 과도 한 데이터를 저장 합니다.

## <a name="conventions"></a>규칙

규칙에 따라 속성에 대 한 적절 한 데이터 형식을 선택 하 여 데이터베이스 공급자까지 유지 됩니다. 속성의 길이 대 한 데이터베이스 공급자 데이터의 가장 긴 길이 대 한 허용 하는 데이터 유형을 일반적으로 선택 됩니다. Microsoft SQL Server에서 사용 하는 예를 들어 `nvarchar(max)` 에 대 한 `string` 속성 (또는 `nvarchar(450)` 열을 키로 사용 되는 경우).

## <a name="data-annotations"></a>데이터 주석

속성에 대 한 최대 길이 구성 하려면 데이터 주석을 사용할 수 있습니다. 이렇게 하면 SQL Server를 대상으로이 예제는 `nvarchar(500)` 사용 중인 데이터 형식입니다.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API

속성에 대 한 최대 길이 구성 하려면 Fluent API를 사용할 수 있습니다. 이렇게 하면 SQL Server를 대상으로이 예제는 `nvarchar(500)` 사용 중인 데이터 형식입니다.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .HasMaxLength(500);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
