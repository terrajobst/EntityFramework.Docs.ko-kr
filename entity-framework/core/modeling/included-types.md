---
title: 형식을 제외한 & 포함-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: 1249e71c02e58afe7fe06b3fdcf523dfa0c9b17c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655733"
---
# <a name="including--excluding-types"></a><span data-ttu-id="abbac-102">형식 포함 및 제외</span><span class="sxs-lookup"><span data-stu-id="abbac-102">Including & Excluding Types</span></span>

<span data-ttu-id="abbac-103">모델에 형식을 포함 하면 EF는 해당 형식에 대 한 메타 데이터를 포함 하 고 데이터베이스에서 인스턴스를 읽고 쓰는 것을 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="abbac-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="abbac-104">규칙</span><span class="sxs-lookup"><span data-stu-id="abbac-104">Conventions</span></span>

<span data-ttu-id="abbac-105">규칙에 따라 컨텍스트의 `DbSet` 속성에 노출 된 형식이 모델에 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="abbac-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="abbac-106">또한 `OnModelCreating` 메서드에 설명 된 형식도 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="abbac-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="abbac-107">마지막으로 검색 된 형식의 탐색 속성을 재귀적으로 탐색 하 여 발견 되는 모든 형식은 모델에도 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="abbac-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="abbac-108">**예를 들어 다음 코드 목록에서는 세 가지 형식이 모두 검색 됩니다.**</span><span class="sxs-lookup"><span data-stu-id="abbac-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="abbac-109">`Blog`는 컨텍스트의 `DbSet` 속성에 노출 되기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="abbac-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="abbac-110">`Post` `Blog.Posts` 탐색 속성을 통해 검색 되기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="abbac-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="abbac-111">`AuditEntry` `OnModelCreating`에 설명 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="abbac-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a><span data-ttu-id="abbac-112">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="abbac-112">Data Annotations</span></span>

<span data-ttu-id="abbac-113">데이터 주석을 사용 하 여 모델에서 형식을 제외할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="abbac-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="abbac-114">흐름 API</span><span class="sxs-lookup"><span data-stu-id="abbac-114">Fluent API</span></span>

<span data-ttu-id="abbac-115">흐름 API를 사용 하 여 모델에서 형식을 제외할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="abbac-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
