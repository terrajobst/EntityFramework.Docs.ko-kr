---
title: 기본 키-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: bdb31964d717c64bddc28e7f1ce55b261e285a9b
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196952"
---
# <a name="primary-keys"></a><span data-ttu-id="511f2-102">기본 키</span><span class="sxs-lookup"><span data-stu-id="511f2-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="511f2-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="511f2-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="511f2-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="511f2-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="511f2-105">기본 키 제약 조건은 각 엔터티 형식의 키에 대해 도입 됩니다.</span><span class="sxs-lookup"><span data-stu-id="511f2-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="511f2-106">규칙</span><span class="sxs-lookup"><span data-stu-id="511f2-106">Conventions</span></span>

<span data-ttu-id="511f2-107">규칙에 따라 데이터베이스의 기본 키 이름이로 지정 `PK_<type name>`됩니다.</span><span class="sxs-lookup"><span data-stu-id="511f2-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="511f2-108">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="511f2-108">Data Annotations</span></span>

<span data-ttu-id="511f2-109">기본 키의 관계형 데이터베이스 관련 요소는 데이터 주석을 사용 하 여 구성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="511f2-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="511f2-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="511f2-110">Fluent API</span></span>

<span data-ttu-id="511f2-111">흐름 API를 사용 하 여 데이터베이스의 primary key 제약 조건 이름을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="511f2-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/KeyName.cs?highlight=9)] -->
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
