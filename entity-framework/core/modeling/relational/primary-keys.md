---
title: "기본 키-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
ms.technology: entity-framework-core
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: fcb1871149c0f20a2576864028b4171904de1982
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="primary-keys"></a><span data-ttu-id="72ff4-102">기본 키</span><span class="sxs-lookup"><span data-stu-id="72ff4-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="72ff4-103">이 섹션의 구성을 일반적 관계형 데이터베이스에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="72ff4-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="72ff4-104">여기에 표시 된 확장 메서드를 사용할 수 있는 관계형 데이터베이스 공급자를 설치할 때 (공유 인해 *Microsoft.EntityFrameworkCore.Relational* 패키지).</span><span class="sxs-lookup"><span data-stu-id="72ff4-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="72ff4-105">각 엔터티 형식의 키에 대 한 기본 키 제약 조건 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="72ff4-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="72ff4-106">규칙</span><span class="sxs-lookup"><span data-stu-id="72ff4-106">Conventions</span></span>

<span data-ttu-id="72ff4-107">규칙에 따라 데이터베이스의 기본 키로 이름이 지정 됩니다 `PK_<type name>`합니다.</span><span class="sxs-lookup"><span data-stu-id="72ff4-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="72ff4-108">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="72ff4-108">Data Annotations</span></span>

<span data-ttu-id="72ff4-109">데이터 주석을 사용 하는 기본 키의 없음 관계형 데이터베이스 특정 측면을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="72ff4-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="72ff4-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="72ff4-110">Fluent API</span></span>

<span data-ttu-id="72ff4-111">데이터베이스의 기본 키 제약 조건의 이름을 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="72ff4-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/KeyName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasKey(b => b.BlogId)
            .HasName("PrimaryKey_BlogId");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
