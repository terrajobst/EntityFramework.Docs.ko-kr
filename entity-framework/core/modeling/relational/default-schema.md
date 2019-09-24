---
title: 기본 스키마-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: ae903ed7200859430aecc55073651236759bc6ce
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197135"
---
# <a name="default-schema"></a>기본 스키마

> [!NOTE]  
> 이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다. 여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).

기본 스키마는 해당 개체에 대해 스키마가 명시적으로 구성 되지 않은 경우 개체가 만들어지는 데이터베이스 스키마입니다.

## <a name="conventions"></a>규칙

규칙에 따라 데이터베이스 공급자가 가장 적합 한 기본 스키마를 선택 합니다. 예를 들어 `dbo` 스키마를 사용 하는 Microsoft SQL Server는 sqlite에서 스키마가 지원 되지 않으므로 sqlite는 스키마를 사용 하지 않습니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 기본 스키마를 설정할 수 없습니다.

## <a name="fluent-api"></a>Fluent API

흐름 API를 사용 하 여 기본 스키마를 지정할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultSchema.cs?highlight=7)] -->
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
