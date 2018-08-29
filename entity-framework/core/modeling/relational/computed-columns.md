---
title: 계산된 열-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: b88efdf69e5100e4eff55f3a41925d2d8e7c3178
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993955"
---
# <a name="computed-columns"></a><span data-ttu-id="17625-102">계산된 열</span><span class="sxs-lookup"><span data-stu-id="17625-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="17625-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="17625-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="17625-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="17625-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="17625-105">계산된 열은 데이터베이스의 값은 계산 열.</span><span class="sxs-lookup"><span data-stu-id="17625-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="17625-106">계산된 열 해당 값을 계산 테이블의 다른 열을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="17625-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="17625-107">규칙</span><span class="sxs-lookup"><span data-stu-id="17625-107">Conventions</span></span>

<span data-ttu-id="17625-108">규칙에 따라 계산 된 열은 모델에서 만들어지지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="17625-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="17625-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="17625-109">Data Annotations</span></span>

<span data-ttu-id="17625-110">데이터 주석을 사용한 계산된 열을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="17625-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="17625-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="17625-111">Fluent API</span></span>

<span data-ttu-id="17625-112">속성을 계산된 열일 매핑되어야 지정 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="17625-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

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
