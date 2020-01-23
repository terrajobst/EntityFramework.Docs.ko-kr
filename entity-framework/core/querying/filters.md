---
title: 전역 쿼리 필터 - EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: f4ee9b77411290249e763f9cb8492eea61803e91
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124394"
---
# <a name="global-query-filters"></a><span data-ttu-id="34ae3-102">전역 쿼리 필터</span><span class="sxs-lookup"><span data-stu-id="34ae3-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="34ae3-103">이 기능은 EF Core 2.0에서 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="34ae3-104">전역 쿼리 필터는 메타데이터 모델(일반적으로 *OnModelCreating*)의 엔터티 형식에 적용되는 LINQ 쿼리 조건자(일반적으로 LINQ *Where* 쿼리 연산자에 전달되는 부울 식)입니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="34ae3-105">이러한 필터는 Include 사용이나 직접 탐색 속성 참조 등의 간접 참조되는 엔터티 형식 등을 포함하여 해당 엔터티 형식과 관련한 모든 LINQ 쿼리에 자동으로 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="34ae3-106">이 기능의 몇 가지 일반적인 용도는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="34ae3-107">**일시 삭제** - 엔터티 형식이 *IsDeleted* 속성을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="34ae3-108">**다중 테넌트** - 엔터티 형식이 *TenantId* 속성을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="34ae3-109">예제</span><span class="sxs-lookup"><span data-stu-id="34ae3-109">Example</span></span>

<span data-ttu-id="34ae3-110">다음 예제에서는 전역 쿼리 필터를 사용하여 간단한 블로그 모델에서 일시 삭제 및 다중 테넌트 쿼리 동작을 구현하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="34ae3-111">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="34ae3-112">먼저 엔터티를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="34ae3-113">_블로그_ 엔터티에서 _tenantId_ 필드의 선언을 확인하세요.</span><span class="sxs-lookup"><span data-stu-id="34ae3-113">Note the declaration of a _tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="34ae3-114">이 선언은 각 Blog 인스턴스를 특정 테넌트와 연결하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="34ae3-115">또한 _IsDeleted_ 속성은 _Post_ 엔터티 형식에 정의되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="34ae3-116">이 속성은 _Post_ 인스턴스가 “일시 삭제”되었는지 여부를 추적하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="34ae3-117">즉, 기본 데이터를 물리적으로 제거하지 않아도 인스턴스가 삭제된 것으로 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="34ae3-118">다음으로, `HasQueryFilter` API를 사용하여 _OnModelCreating_에서 쿼리 필터를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-118">Next, configure the query filters in _OnModelCreating_ using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="34ae3-119">이제 _HasQueryFilter_ 호출에 전달된 조건자 식이 해당 형식에 대한 모든 LINQ 쿼리에 자동으로 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="34ae3-120">DbContext 인스턴스 수준 필드를 사용합니다. `_tenantId`는 현재 테넌트를 설정하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-120">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="34ae3-121">모델 수준 필터는 올바른 컨텍스트 인스턴스(즉, 쿼리를 실행하는 인스턴스)의 값을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

> [!NOTE]
> <span data-ttu-id="34ae3-122">현재 동일한 엔터티에 여러 개의 쿼리 필터를 정의할 수 없습니다. 마지막 필터만 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-122">It is currently not possible to define multiple query filters on the same entity - only the last one will be applied.</span></span> <span data-ttu-id="34ae3-123">그러나 논리 _AND_ 연산자([C#의 `&&`](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-))를 사용하여 여러 조건으로 단일 필터를 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-123">However, you can define a single filter with multiple conditions using the logical _AND_ operator ([`&&` in C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="34ae3-124">필터 사용 안 함</span><span class="sxs-lookup"><span data-stu-id="34ae3-124">Disabling Filters</span></span>

<span data-ttu-id="34ae3-125">`IgnoreQueryFilters()` 연산자를 사용하여 개별 LINQ 쿼리에 대해 필터를 사용하지 않도록 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-125">Filters may be disabled for individual LINQ queries by using the `IgnoreQueryFilters()` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="34ae3-126">제한 사항</span><span class="sxs-lookup"><span data-stu-id="34ae3-126">Limitations</span></span>

<span data-ttu-id="34ae3-127">전역 쿼리 필터에는 다음 제한 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-127">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="34ae3-128">상속 계층 구조의 루트 엔터티 형식에 대해서만 필터를 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34ae3-128">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
