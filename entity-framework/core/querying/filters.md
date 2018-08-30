---
title: 전역 쿼리 필터 - EF Core
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 6b7a4069917c93015a218c131ff0d0a3920fb69d
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/01/2018
ms.locfileid: "42447639"
---
# <a name="global-query-filters"></a><span data-ttu-id="d438f-102">전역 쿼리 필터</span><span class="sxs-lookup"><span data-stu-id="d438f-102">Global Query Filters</span></span>

<span data-ttu-id="d438f-103">전역 쿼리 필터는 메타데이터 모델(일반적으로 *OnModelCreating*)의 엔터티 형식에 적용되는 LINQ 쿼리 조건자(일반적으로 LINQ *Where* 쿼리 연산자에 전달되는 부울 식)입니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="d438f-104">이러한 필터는 Include 사용이나 직접 탐색 속성 참조 등의 간접 참조되는 엔터티 형식 등을 포함하여 해당 엔터티 형식과 관련한 모든 LINQ 쿼리에 자동으로 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="d438f-105">이 기능의 몇 가지 일반적인 용도는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="d438f-106">**일시 삭제** - 엔터티 형식이 *IsDeleted* 속성을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="d438f-107">**다중 테넌트** - 엔터티 형식이 *TenantId* 속성을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="d438f-108">예</span><span class="sxs-lookup"><span data-stu-id="d438f-108">Example</span></span>

<span data-ttu-id="d438f-109">다음 예제에서는 전역 쿼리 필터를 사용하여 간단한 블로그 모델에서 일시 삭제 및 다중 테넌트 쿼리 동작을 구현하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="d438f-110">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryFilters)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="d438f-111">먼저 엔터티를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="d438f-112">_Blog_ 엔터티에서 __tenantId_ 필드의 선언을 확인하세요.</span><span class="sxs-lookup"><span data-stu-id="d438f-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="d438f-113">이 선언은 각 Blog 인스턴스를 특정 테넌트와 연결하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="d438f-114">또한 _IsDeleted_ 속성은 _Post_ 엔터티 형식에 정의되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="d438f-115">이 속성은 _Post_ 인스턴스가 “일시 삭제”되었는지 여부를 추적하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-115">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="d438f-116">즉, 기본 데이터를 물리적으로 제거하지 않아도 인스턴스가 삭제된 것으로 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-116">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="d438f-117">다음으로, ```HasQueryFilter``` API를 사용하여 _OnModelCreating_에서 쿼리 필터를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="d438f-118">이제 _HasQueryFilter_ 호출에 전달된 조건자 식이 해당 형식에 대한 모든 LINQ 쿼리에 자동으로 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="d438f-119">DbContext 인스턴스 수준 필드를 사용합니다. ```_tenantId```는 현재 테넌트를 설정하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="d438f-120">모델 수준 필터는 올바른 컨텍스트 인스턴스(즉, 쿼리를 실행하는 인스턴스)의 값을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-120">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="d438f-121">필터 사용 안 함</span><span class="sxs-lookup"><span data-stu-id="d438f-121">Disabling Filters</span></span>

<span data-ttu-id="d438f-122">```IgnoreQueryFilters()``` 연산자를 사용하여 개별 LINQ 쿼리에 대해 필터를 사용하지 않도록 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-122">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="d438f-123">제한 사항</span><span class="sxs-lookup"><span data-stu-id="d438f-123">Limitations</span></span>

<span data-ttu-id="d438f-124">전역 쿼리 필터에는 다음 제한 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-124">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="d438f-125">필터에 탐색 속성에 대한 참조를 포함할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-125">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="d438f-126">상속 계층 구조의 루트 엔터티 형식에 대해서만 필터를 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d438f-126">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
