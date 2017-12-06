---
title: "최대 길이-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
ms.technology: entity-framework-core
uid: core/modeling/max-length
ms.openlocfilehash: 7325c0c3328477473392bf9e7c82f1696bb4f424
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="maximum-length"></a><span data-ttu-id="f502d-102">최대 길이</span><span class="sxs-lookup"><span data-stu-id="f502d-102">Maximum Length</span></span>

<span data-ttu-id="f502d-103">최대 길이 구성 합니다. 지정된 된 속성에 사용할 적절 한 데이터 형식에 대 한 데이터 저장소에 대 한 힌트를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="f502d-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="f502d-104">최대 길이에 적용 됩니다 배열 데이터 형식 같은 `string` 및 `byte[]`합니다.</span><span class="sxs-lookup"><span data-stu-id="f502d-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="f502d-105">Entity Framework 데이터 공급자에 게 전달 하기 전에 최대 길이 대 한 유효성 검사를 수행 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f502d-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="f502d-106">적절 한 있는지 검사 하기 위해 공급자 또는 데이터 저장소는 합니다.</span><span class="sxs-lookup"><span data-stu-id="f502d-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="f502d-107">예를 들어 기본 열의 데이터 형식으로 예외가 발생 합니다 최대 길이 초과 하는 SQL Server를 대상으로 지정은 허용 하지 않습니다 과도 한 데이터를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="f502d-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="f502d-108">규칙</span><span class="sxs-lookup"><span data-stu-id="f502d-108">Conventions</span></span>

<span data-ttu-id="f502d-109">규칙에 따라 속성에 대 한 적절 한 데이터 형식을 선택 하 여 데이터베이스 공급자까지 유지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f502d-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="f502d-110">속성의 길이 대 한 데이터베이스 공급자 데이터의 가장 긴 길이 대 한 허용 하는 데이터 유형을 일반적으로 선택 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f502d-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="f502d-111">Microsoft SQL Server에서 사용 하는 예를 들어 `nvarchar(max)` 에 대 한 `string` 속성 (또는 `nvarchar(450)` 열을 키로 사용 되는 경우).</span><span class="sxs-lookup"><span data-stu-id="f502d-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f502d-112">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="f502d-112">Data Annotations</span></span>

<span data-ttu-id="f502d-113">속성에 대 한 최대 길이 구성 하려면 데이터 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f502d-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="f502d-114">이렇게 하면 SQL Server를 대상으로이 예제는 `nvarchar(500)` 사용 중인 데이터 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="f502d-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="f502d-115">Fluent API</span><span class="sxs-lookup"><span data-stu-id="f502d-115">Fluent API</span></span>

<span data-ttu-id="f502d-116">속성에 대 한 최대 길이 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f502d-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="f502d-117">이렇게 하면 SQL Server를 대상으로이 예제는 `nvarchar(500)` 사용 중인 데이터 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="f502d-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

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
