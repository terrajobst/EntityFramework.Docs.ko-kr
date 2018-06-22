---
title: 계산된 열-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
ms.technology: entity-framework-core
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 95312504286bd34cc666b5a21273835c4b35d379
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052483"
---
# <a name="computed-columns"></a><span data-ttu-id="d6171-102">계산 열</span><span class="sxs-lookup"><span data-stu-id="d6171-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="d6171-103">이 섹션의 구성을 일반적 관계형 데이터베이스에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d6171-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="d6171-104">여기에 표시 된 확장 메서드를 사용할 수 있는 관계형 데이터베이스 공급자를 설치할 때 (공유 인해 *Microsoft.EntityFrameworkCore.Relational* 패키지).</span><span class="sxs-lookup"><span data-stu-id="d6171-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="d6171-105">계산된 열은 열 값은 데이터베이스에서 계산 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d6171-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="d6171-106">계산된 열 해당 값을 계산 테이블의 다른 열을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d6171-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="d6171-107">규칙</span><span class="sxs-lookup"><span data-stu-id="d6171-107">Conventions</span></span>

<span data-ttu-id="d6171-108">규칙에 따라 계산된 열은 모델에서 만들어지지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="d6171-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d6171-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="d6171-109">Data Annotations</span></span>

<span data-ttu-id="d6171-110">데이터 주석을 사용한 계산된 열을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d6171-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d6171-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="d6171-111">Fluent API</span></span>

<span data-ttu-id="d6171-112">속성을 계산된 열에 매핑해야 지정 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d6171-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/ComputedColumn.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .Property(p => p.DisplayName)
            .HasComputedColumnSql("[LastName] + ', ' + [FirstName]");
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string DisplayName { get; set; }
}
```
