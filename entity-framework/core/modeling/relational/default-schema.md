---
title: 기본 스키마-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 800551bbadd0a9e8b5eb7070a8ccf6ed2407e3d2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995368"
---
# <a name="default-schema"></a><span data-ttu-id="fdef2-102">기본 스키마</span><span class="sxs-lookup"><span data-stu-id="fdef2-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="fdef2-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="fdef2-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="fdef2-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="fdef2-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="fdef2-105">기본 스키마에 만들어지는 개체에서 해당 개체에 대 한 스키마를 명시적으로 구성 되지 않은 경우 데이터베이스 스키마가입니다.</span><span class="sxs-lookup"><span data-stu-id="fdef2-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="fdef2-106">규칙</span><span class="sxs-lookup"><span data-stu-id="fdef2-106">Conventions</span></span>

<span data-ttu-id="fdef2-107">규칙에 따라 데이터베이스 공급자는 가장 적합 한 기본 스키마를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="fdef2-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="fdef2-108">Microsoft SQL Server가 사용 하는 예를 들어를 `dbo` 스키마 및 SQLite를 있으므로 사용 하지는 스키마 (스키마 SQLite에서 지원 되지 않습니다).</span><span class="sxs-lookup"><span data-stu-id="fdef2-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="fdef2-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="fdef2-109">Data Annotations</span></span>

<span data-ttu-id="fdef2-110">데이터 주석을 사용 하 여 기본 스키마를 설정할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="fdef2-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="fdef2-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="fdef2-111">Fluent API</span></span>

<span data-ttu-id="fdef2-112">기본 스키마를 지정 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fdef2-112">You can use the Fluent API to specify a default schema.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultSchema.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDefaultSchema("blogging");
    }
}
```
