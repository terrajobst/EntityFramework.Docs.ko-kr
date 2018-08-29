---
title: 필수/선택적 속성-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: b6716a5b03e1afc2933e317d606ef50f986c22c7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995499"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="15e44-102">필수 및 선택적 속성</span><span class="sxs-lookup"><span data-stu-id="15e44-102">Required and Optional Properties</span></span>

<span data-ttu-id="15e44-103">속성에 포함 하려면 유효한 경우 선택적으로 간주 됩니다 `null`합니다.</span><span class="sxs-lookup"><span data-stu-id="15e44-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="15e44-104">경우 `null` 필수 속성으로 간주 됩니다 속성에 할당할 올바른 값이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="15e44-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="15e44-105">규칙</span><span class="sxs-lookup"><span data-stu-id="15e44-105">Conventions</span></span>

<span data-ttu-id="15e44-106">규칙에 따라 CLR 형식이 null을 포함할 수 있습니다 속성 구성지 것입니다 옵션으로 (`string`, `int?`, `byte[]`등.).</span><span class="sxs-lookup"><span data-stu-id="15e44-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="15e44-107">CLR 형식이 null을 포함할 수 없습니다. 속성을 구성할 수는 필요에 따라 (`int`, `decimal`, `bool`등.).</span><span class="sxs-lookup"><span data-stu-id="15e44-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="15e44-108">CLR 형식이 null을 포함할 수 없습니다. 속성은 선택적으로 구성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="15e44-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="15e44-109">항상 Entity Framework에 필요한 속성을 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="15e44-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="15e44-110">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="15e44-110">Data Annotations</span></span>

<span data-ttu-id="15e44-111">필수 속성 임을 데이터 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="15e44-111">You can use Data Annotations to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="15e44-112">Fluent API</span><span class="sxs-lookup"><span data-stu-id="15e44-112">Fluent API</span></span>

<span data-ttu-id="15e44-113">필수 속성 임을 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="15e44-113">You can use the Fluent API to indicate that a property is required.</span></span>

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
