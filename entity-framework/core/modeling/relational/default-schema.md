---
title: "기본 스키마-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
ms.technology: entity-framework-core
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 26106deb2d4e35ecf33e97790a83f9af77991aed
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="default-schema"></a>기본 스키마

> [!NOTE]  
> 이 섹션의 구성을 일반적 관계형 데이터베이스에 적용 됩니다. 여기에 표시 된 확장 메서드를 사용할 수 있는 관계형 데이터베이스 공급자를 설치할 때 (공유 인해 *Microsoft.EntityFrameworkCore.Relational* 패키지).

기본 스키마는 만들어지는 개체에 스키마 해당 개체에 대해 명시적으로 구성 되지 않은 경우 데이터베이스 스키마를 사용 합니다.

## <a name="conventions"></a>규칙

규칙에 따라 데이터베이스 공급자는 가장 적합 한 기본 스키마를 선택 합니다. Microsoft SQL Server에서 사용 하는 예를 들어는 `dbo` 스키마 및 SQLite는 있으므로 사용 하지는 스키마 (스키마 SQLite에서는 지원 되지 않습니다).

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 기본 스키마를 설정할 수 없습니다.

## <a name="fluent-api"></a>Fluent API

기본 스키마를 지정 하려면 Fluent API를 사용할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultSchema.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDefaultSchema("blogging");
    }
}
```
