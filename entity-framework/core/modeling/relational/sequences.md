---
title: 순서-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: eb9d9896966af0ad6b778047a1ed6af7358e8eb2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994518"
---
# <a name="sequences"></a><span data-ttu-id="494c9-102">시퀀스</span><span class="sxs-lookup"><span data-stu-id="494c9-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="494c9-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="494c9-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="494c9-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="494c9-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="494c9-105">시퀀스는 데이터베이스의 순차적 숫자 값을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="494c9-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="494c9-106">시퀀스는 특정 테이블을 사용 하 여 연결 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="494c9-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="494c9-107">규칙</span><span class="sxs-lookup"><span data-stu-id="494c9-107">Conventions</span></span>

<span data-ttu-id="494c9-108">규칙에 따라 시퀀스에서 모델에 도입 되지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="494c9-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="494c9-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="494c9-109">Data Annotations</span></span>

<span data-ttu-id="494c9-110">하지 데이터 주석을 사용 하 여 시퀀스를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="494c9-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="494c9-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="494c9-111">Fluent API</span></span>

<span data-ttu-id="494c9-112">모델의 시퀀스를 만들려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="494c9-112">You can use the Fluent API to create a sequence in the model.</span></span>

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

<span data-ttu-id="494c9-113">또한 해당 스키마, 시작 값 및 증분 값 등 시퀀스의 추가 측면을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="494c9-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

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

<span data-ttu-id="494c9-114">시퀀스에 도입 되 면 모델의 속성에 대 한 값을 생성에 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="494c9-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="494c9-115">예를 들어 사용할 수 있습니다 [기본값](default-values.md) 시퀀스에서 다음 값을 삽입 합니다.</span><span class="sxs-lookup"><span data-stu-id="494c9-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

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
