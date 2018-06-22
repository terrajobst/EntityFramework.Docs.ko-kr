---
title: 포함 및 제외 속성-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
ms.technology: entity-framework-core
uid: core/modeling/included-properties
ms.openlocfilehash: a6eaea4319f6a4d30c223265bf75a88731a38443
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052493"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="72262-102">포함 및 제외 속성</span><span class="sxs-lookup"><span data-stu-id="72262-102">Including & Excluding Properties</span></span>

<span data-ttu-id="72262-103">모델의 속성을 포함 하 여 EF에 해당 속성에 대 한 메타 데이터를 의미 하 고 읽기 / 쓰기 데이터베이스에서 /에 값을 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="72262-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="72262-104">규칙</span><span class="sxs-lookup"><span data-stu-id="72262-104">Conventions</span></span>

<span data-ttu-id="72262-105">규칙에 따라 공용 속성 getter 및 setter를 모델에 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="72262-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="72262-106">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="72262-106">Data Annotations</span></span>

<span data-ttu-id="72262-107">모델에서 속성을 제외 하려면 데이터 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="72262-107">You can use Data Annotations to exclude a property from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=6)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    [NotMapped]
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="72262-108">Fluent API</span><span class="sxs-lookup"><span data-stu-id="72262-108">Fluent API</span></span>

<span data-ttu-id="72262-109">모델에서 속성을 제외 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="72262-109">You can use the Fluent API to exclude a property from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Ignore(b => b.LoadedFromDatabase);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public DateTime LoadedFromDatabase { get; set; }
}
```
