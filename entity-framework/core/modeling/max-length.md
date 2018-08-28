---
title: 최대 길이-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: e54d3671f378b96a49eaf4cb312e72072813fc6d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996193"
---
# <a name="maximum-length"></a><span data-ttu-id="a858f-102">최대 길이</span><span class="sxs-lookup"><span data-stu-id="a858f-102">Maximum Length</span></span>

<span data-ttu-id="a858f-103">지정된 된 속성에 사용할 적절 한 데이터 형식에 대 한 데이터 저장소에 힌트를 제공 최대 길이 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="a858f-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="a858f-104">최대 길이에 적용 됩니다 배열 데이터 형식이 같은 `string` 고 `byte[]`입니다.</span><span class="sxs-lookup"><span data-stu-id="a858f-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="a858f-105">Entity Framework 데이터 공급자에 게 전달 하기 전에 최대 길이의 유효성 검사를 수행 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="a858f-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="a858f-106">해당 하는 경우의 유효성을 검사 하려면 공급자 또는 데이터 저장소는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="a858f-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="a858f-107">예를 들어, 기본 열의 데이터 형식에 예외가 발생 하면 최대 길이 초과 하는 SQL Server를 대상으로 하는 경우 과도 한 데이터를 저장할 수을 허용 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="a858f-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="a858f-108">규칙</span><span class="sxs-lookup"><span data-stu-id="a858f-108">Conventions</span></span>

<span data-ttu-id="a858f-109">규칙에 따라 해당 에서만 데이터베이스 공급자 속성에 대 한 적절 한 데이터 형식을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="a858f-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="a858f-110">길이가 속성을 데이터베이스 공급자는 일반적으로 데이터의 가장 긴 길이 허용 하는 데이터 형식을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="a858f-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="a858f-111">Microsoft SQL Server가 사용 하는 예를 들어 `nvarchar(max)` 에 대 한 `string` 속성 (또는 `nvarchar(450)` 열을 키로 사용 되는 경우).</span><span class="sxs-lookup"><span data-stu-id="a858f-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a858f-112">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="a858f-112">Data Annotations</span></span>

<span data-ttu-id="a858f-113">속성에 대 한 최대 길이 구성 하려면 데이터 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a858f-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="a858f-114">이 예제에서는이 인해 SQL Server를 대상으로 하는 `nvarchar(500)` 사용 되는 데이터 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="a858f-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="a858f-115">Fluent API</span><span class="sxs-lookup"><span data-stu-id="a858f-115">Fluent API</span></span>

<span data-ttu-id="a858f-116">속성에 대 한 최대 길이 구성 하는 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a858f-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="a858f-117">이 예제에서는이 인해 SQL Server를 대상으로 하는 `nvarchar(500)` 사용 되는 데이터 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="a858f-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

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
