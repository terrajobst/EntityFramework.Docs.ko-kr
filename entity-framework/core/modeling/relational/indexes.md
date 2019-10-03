---
title: 인덱스 (관계형 데이터베이스)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 7bb74d0bfa6090b597eb988a46f00494e25f233e
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813641"
---
# <a name="indexes-relational-database"></a><span data-ttu-id="3d477-102">인덱스 (관계형 데이터베이스)</span><span class="sxs-lookup"><span data-stu-id="3d477-102">Indexes (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="3d477-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d477-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3d477-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="3d477-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3d477-105">관계형 데이터베이스의 인덱스는 Entity Framework의 핵심 인덱스와 동일한 개념에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d477-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="3d477-106">규칙</span><span class="sxs-lookup"><span data-stu-id="3d477-106">Conventions</span></span>

<span data-ttu-id="3d477-107">규칙에 따라 인덱스의 이름은 `IX_<type name>_<property name>`입니다.</span><span class="sxs-lookup"><span data-stu-id="3d477-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="3d477-108">복합 인덱스 `<property name>` 의 경우 속성 이름의 밑줄로 구분 된 목록이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d477-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3d477-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="3d477-109">Data Annotations</span></span>

<span data-ttu-id="3d477-110">데이터 주석을 사용 하 여 인덱스를 구성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="3d477-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="3d477-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="3d477-111">Fluent API</span></span>

<span data-ttu-id="3d477-112">흐름 API를 사용 하 여 인덱스 이름을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d477-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="3d477-113">필터를 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d477-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="3d477-114">SQL Server 공급자 EF를 사용 하는 경우 고유 인덱스의 일부인 모든 null 허용 열에 대해 ' IS NOT NULL ' 필터를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d477-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="3d477-115">이 규칙을 재정의 하려면 값을 `null` 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d477-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a><span data-ttu-id="3d477-116">SQL Server 인덱스에 열 포함</span><span class="sxs-lookup"><span data-stu-id="3d477-116">Include Columns in SQL Server Indexes</span></span>

<span data-ttu-id="3d477-117">쿼리의 모든 열이 키 또는 키가 아닌 열로 인덱스에 포함 되는 경우에는 [포괄 열이 있는 인덱스](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) 를 구성 하 여 쿼리 성능을 크게 향상 시킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d477-117">You can configure [indexes with included columns](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) to significantly improve query performance when all columns in the query are included in the index as key or non-key columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ForSqlServerHasIndex.cs?name=Model)]
