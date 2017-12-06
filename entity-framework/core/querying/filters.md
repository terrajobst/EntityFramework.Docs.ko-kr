---
title: "전역 쿼리 필터-EF 코어"
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
# <a name="global-query-filters"></a><span data-ttu-id="6f340-102">전역 쿼리 필터</span><span class="sxs-lookup"><span data-stu-id="6f340-102">Global Query Filters</span></span>

<span data-ttu-id="6f340-103">전역 쿼리 필터는 LINQ 쿼리 조건자 (linq 일반적으로 전달 되는 부울 식 *여기서* 쿼리 연산자) 메타 데이터 모델의 엔터티 형식에 적용 (대개 *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="6f340-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="6f340-104">이러한 필터는 엔터티 형식 또는 직접 포함을 사용 하 여 같은 간접적으로 참조 탐색 속성 참조를 포함 하 여 해당 엔터티 형식과 관련 된 모든 LINQ 쿼리를 자동으로 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6f340-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="6f340-105">이 기능의 일부 일반적인 응용 프로그램은:</span><span class="sxs-lookup"><span data-stu-id="6f340-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="6f340-106">**일시 삭제** -는 엔터티 형식 정의 *IsDeleted* 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="6f340-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="6f340-107">**다중 테 넌 트** -는 엔터티 형식 정의 *TenantId* 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="6f340-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="6f340-108">예제</span><span class="sxs-lookup"><span data-stu-id="6f340-108">Example</span></span>

<span data-ttu-id="6f340-109">다음 예제에서는 간단한 블로깅 모델의 소프트 삭제 및 다중 테 넌 트 쿼리 동작을 구현 하 전역 쿼리 필터를 사용 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="6f340-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="6f340-110">이 문서를 볼 수 있습니다 [샘플](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) GitHub에서 합니다.</span><span class="sxs-lookup"><span data-stu-id="6f340-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="6f340-111">첫째, 엔터티를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="6f340-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="6f340-112">_의 선언을 확인_tenantId_ 필드에 _블로그_ 엔터티.</span><span class="sxs-lookup"><span data-stu-id="6f340-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="6f340-113">블로그 인스턴스마다 특정 테 넌 트와 연결할 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6f340-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="6f340-114">또한 정의 _IsDeleted_ 속성에는 _Post_ 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="6f340-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="6f340-115">이 옵션은 사용 여부를 추적 하려면이 옵션을 _Post_ 인스턴스가 "일시 삭제 된" 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="6f340-115">This is used this to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="6f340-116">즉 인스턴스는 기본 데이터를 제거 하는 물리적으로 삭제 된 withouth로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6f340-116">I.e. The instance is marked as deleted withouth physically removing the underlying data.</span></span>

<span data-ttu-id="6f340-117">그런 다음에 있는 쿼리 필터를 구성 _OnModelCreating_ 를 사용 하는 ```HasQueryFilter``` API입니다.</span><span class="sxs-lookup"><span data-stu-id="6f340-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="6f340-118">조건자 식에 전달 되는 _HasQueryFilter_ 호출 이러한 형식에 대 한 LINQ 쿼리를 자동으로 적용 이제 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6f340-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="6f340-119">DbContext 인스턴스 수준 필드를 사용 하 여: ```_tenantId``` 현재 테 넌 트를 설정 하는 데 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="6f340-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="6f340-120">모델 수준 필터에서 올바른 컨텍스트 인스턴스 값이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6f340-120">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="6f340-121">즉 쿼리를 실행 하는 인스턴스.</span><span class="sxs-lookup"><span data-stu-id="6f340-121">I.e. The instance that is executing the query.</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="6f340-122">필터를 사용 하지 않도록 설정</span><span class="sxs-lookup"><span data-stu-id="6f340-122">Disabling Filters</span></span>

<span data-ttu-id="6f340-123">필터를 사용 하 여 개별 LINQ 쿼리에 대 한 비활성화 될 수 있습니다는 ```IgnoreQueryFilters()``` 연산자입니다.</span><span class="sxs-lookup"><span data-stu-id="6f340-123">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="6f340-124">제한 사항</span><span class="sxs-lookup"><span data-stu-id="6f340-124">Limitations</span></span>

<span data-ttu-id="6f340-125">전역 쿼리 필터는 다음과 같은 제한 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6f340-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="6f340-126">필터는 탐색 속성에 대 한 참조를 포함할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="6f340-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="6f340-127">엔터티 유형의 상속 계층 구조의 루트에 대 한 필터를 정의할 수만 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6f340-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
