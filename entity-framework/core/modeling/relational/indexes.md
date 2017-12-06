---
title: "인덱스-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: 683b580bb155e0416f13c5d63e3280078fbcee21
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a><span data-ttu-id="b9a4d-102">인덱스</span><span class="sxs-lookup"><span data-stu-id="b9a4d-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="b9a4d-103">이 섹션의 구성을 일반적 관계형 데이터베이스에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b9a4d-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="b9a4d-104">여기에 표시 된 확장 메서드를 사용할 수 있는 관계형 데이터베이스 공급자를 설치할 때 (공유 인해 *Microsoft.EntityFrameworkCore.Relational* 패키지).</span><span class="sxs-lookup"><span data-stu-id="b9a4d-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="b9a4d-105">관계형 데이터베이스에서 인덱스 Entity Framework의 핵심에서 인덱스와 동일한 개념에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="b9a4d-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="b9a4d-106">규칙</span><span class="sxs-lookup"><span data-stu-id="b9a4d-106">Conventions</span></span>

<span data-ttu-id="b9a4d-107">인덱스 이름은 규칙에 따라 `IX_<type name>_<property name>`합니다.</span><span class="sxs-lookup"><span data-stu-id="b9a4d-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="b9a4d-108">복합 인덱스에 대 한 `<property name>` 속성 이름의 밑줄 구분 된 목록이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b9a4d-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b9a4d-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="b9a4d-109">Data Annotations</span></span>

<span data-ttu-id="b9a4d-110">데이터 주석을 사용 하 여 인덱스를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b9a4d-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b9a4d-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="b9a4d-111">Fluent API</span></span>

<span data-ttu-id="b9a4d-112">인덱스의 이름을 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b9a4d-112">You can use the Fluent API to configure the name of an index.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/IndexName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .HasName("Index_Url");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
