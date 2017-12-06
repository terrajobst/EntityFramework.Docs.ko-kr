---
title: "순서-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
ms.technology: entity-framework-core
uid: core/modeling/relational/sequences
ms.openlocfilehash: 98a40aeecbec0fd9fb9cc108d6b5f98178dea403
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="sequences"></a><span data-ttu-id="228b7-102">시퀀스</span><span class="sxs-lookup"><span data-stu-id="228b7-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="228b7-103">이 섹션의 구성을 일반적 관계형 데이터베이스에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="228b7-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="228b7-104">여기에 표시 된 확장 메서드를 사용할 수 있는 관계형 데이터베이스 공급자를 설치할 때 (공유 인해 *Microsoft.EntityFrameworkCore.Relational* 패키지).</span><span class="sxs-lookup"><span data-stu-id="228b7-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="228b7-105">시퀀스는 데이터베이스에 연속 숫자 값을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="228b7-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="228b7-106">시퀀스 된 특정 테이블에 연결 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="228b7-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="228b7-107">규칙</span><span class="sxs-lookup"><span data-stu-id="228b7-107">Conventions</span></span>

<span data-ttu-id="228b7-108">규칙에 따라 시퀀스에 모델에 제공 되지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="228b7-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="228b7-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="228b7-109">Data Annotations</span></span>

<span data-ttu-id="228b7-110">하지 데이터 주석을 사용 하는 순서를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="228b7-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="228b7-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="228b7-111">Fluent API</span></span>

<span data-ttu-id="228b7-112">모델의 시퀀스를 만들려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="228b7-112">You can use the Fluent API to create a sequence in the model.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Sequence.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="228b7-113">또한 해당 스키마, 시작 값 및 증분 값 등 시퀀스의 추가 측면을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="228b7-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceConfigured.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);
    }
}
```

<span data-ttu-id="228b7-114">시퀀스 도입 되 면 모델의 속성에 대 한 값을 생성 하려면 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="228b7-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="228b7-115">사용할 수는 예를 들어 [기본값](default-values.md) 를 시퀀스에서 다음 값을 삽입 합니다.</span><span class="sxs-lookup"><span data-stu-id="228b7-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceUsed.cs?highlight=11,12,13)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);

        modelBuilder.Entity<Order>()
            .Property(o => o.OrderNo)
            .HasDefaultValueSql("NEXT VALUE FOR shared.OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```
