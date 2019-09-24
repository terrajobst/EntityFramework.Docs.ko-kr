---
title: 기본 스키마-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: ae903ed7200859430aecc55073651236759bc6ce
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197135"
---
# <a name="default-schema"></a><span data-ttu-id="1e1b1-102">기본 스키마</span><span class="sxs-lookup"><span data-stu-id="1e1b1-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="1e1b1-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="1e1b1-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="1e1b1-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="1e1b1-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="1e1b1-105">기본 스키마는 해당 개체에 대해 스키마가 명시적으로 구성 되지 않은 경우 개체가 만들어지는 데이터베이스 스키마입니다.</span><span class="sxs-lookup"><span data-stu-id="1e1b1-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="1e1b1-106">규칙</span><span class="sxs-lookup"><span data-stu-id="1e1b1-106">Conventions</span></span>

<span data-ttu-id="1e1b1-107">규칙에 따라 데이터베이스 공급자가 가장 적합 한 기본 스키마를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e1b1-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="1e1b1-108">예를 들어 `dbo` 스키마를 사용 하는 Microsoft SQL Server는 sqlite에서 스키마가 지원 되지 않으므로 sqlite는 스키마를 사용 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="1e1b1-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1e1b1-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="1e1b1-109">Data Annotations</span></span>

<span data-ttu-id="1e1b1-110">데이터 주석을 사용 하 여 기본 스키마를 설정할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="1e1b1-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="1e1b1-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="1e1b1-111">Fluent API</span></span>

<span data-ttu-id="1e1b1-112">흐름 API를 사용 하 여 기본 스키마를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1e1b1-112">You can use the Fluent API to specify a default schema.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultSchema.cs?highlight=7)] -->
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
