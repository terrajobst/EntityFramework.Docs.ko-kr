---
title: "인덱스-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: f577fccfefc6908edf2ac47ae630323d7a9f5f2b
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="indexes"></a><span data-ttu-id="3493f-102">인덱스</span><span class="sxs-lookup"><span data-stu-id="3493f-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="3493f-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="3493f-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3493f-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="3493f-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3493f-105">관계형 데이터베이스에서 인덱스 Entity Framework의 핵심에서 인덱스와 동일한 개념에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="3493f-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="3493f-106">규칙</span><span class="sxs-lookup"><span data-stu-id="3493f-106">Conventions</span></span>

<span data-ttu-id="3493f-107">인덱스 이름은 규칙에 따라 `IX_<type name>_<property name>`합니다.</span><span class="sxs-lookup"><span data-stu-id="3493f-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="3493f-108">복합 인덱스에 대 한 `<property name>` 속성 이름의 밑줄 구분 된 목록이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3493f-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3493f-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="3493f-109">Data Annotations</span></span>

<span data-ttu-id="3493f-110">데이터 주석을 사용 하 여 인덱스를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3493f-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="3493f-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="3493f-111">Fluent API</span></span>

<span data-ttu-id="3493f-112">인덱스의 이름을 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3493f-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="3493f-113">필터를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3493f-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="3493f-114">고유 인덱스의 일부인 모든 null 허용 열에 대 한 추가 SQL Server 공급자 EF를 사용 하는 ' IS NOT NULL' 필터링 합니다.</span><span class="sxs-lookup"><span data-stu-id="3493f-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="3493f-115">제공할 수 있습니다이 규칙을 재정의 하는 `null` 값입니다.</span><span class="sxs-lookup"><span data-stu-id="3493f-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
