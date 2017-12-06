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
# <a name="required-and-optional-properties"></a><span data-ttu-id="2b5b2-102">필수 및 선택적 속성</span><span class="sxs-lookup"><span data-stu-id="2b5b2-102">Required and Optional Properties</span></span>

<span data-ttu-id="2b5b2-103">속성에 포함 하려면 유효한 경우 선택적으로 간주 됩니다 `null`합니다.</span><span class="sxs-lookup"><span data-stu-id="2b5b2-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="2b5b2-104">경우 `null` 는 잘못 된 값은 필수 속성으로 간주 됩니다 속성에 할당할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b5b2-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="2b5b2-105">규칙</span><span class="sxs-lookup"><span data-stu-id="2b5b2-105">Conventions</span></span>

<span data-ttu-id="2b5b2-106">규칙에 따라 해당 CLR 형식이 null를 포함할 수 있는 속성 구성 될 옵션으로 (`string`, `int?`, `byte[]`등.).</span><span class="sxs-lookup"><span data-stu-id="2b5b2-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="2b5b2-107">CLR 형식이 null을 포함할 수 없습니다 하는 속성을 구성할 수는 필요에 따라 (`int`, `decimal`, `bool`등.).</span><span class="sxs-lookup"><span data-stu-id="2b5b2-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="2b5b2-108">CLR 형식이 null을 포함할 수 없는 속성 옵션으로 구성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="2b5b2-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="2b5b2-109">Entity Framework에 필요한 속성을 고려 항상 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b5b2-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2b5b2-110">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="2b5b2-110">Data Annotations</span></span>

<span data-ttu-id="2b5b2-111">속성이 필수임을 나타내기 위해 데이터 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b5b2-111">You can use Data Annotations to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="2b5b2-112">Fluent API</span><span class="sxs-lookup"><span data-stu-id="2b5b2-112">Fluent API</span></span>

<span data-ttu-id="2b5b2-113">속성이 필수임을 나타내기 위해 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b5b2-113">You can use the Fluent API to indicate that a property is required.</span></span>

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
