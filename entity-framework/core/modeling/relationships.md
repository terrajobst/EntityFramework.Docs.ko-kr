---
title: 관계-EF Core
description: Entity Framework Core를 사용할 때 엔터티 형식 간의 관계를 구성 하는 방법
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relationships
ms.openlocfilehash: 6d68e813cec6c989e8e4cb848f8740489645c65c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2020
ms.locfileid: "79402116"
---
# <a name="relationships"></a><span data-ttu-id="c6f20-103">관계</span><span class="sxs-lookup"><span data-stu-id="c6f20-103">Relationships</span></span>

<span data-ttu-id="c6f20-104">관계는 두 엔터티가 서로 관련 되는 방식을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-104">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="c6f20-105">관계형 데이터베이스에서이는 foreign key 제약 조건으로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-105">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="c6f20-106">이 문서의 샘플 대부분은 일 대 다 관계를 사용 하 여 개념을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-106">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="c6f20-107">일 대 일 및 다 대 다 관계의 예는이 문서의 끝에 있는 [다른 관계 패턴](#other-relationship-patterns) 섹션을 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="c6f20-107">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="c6f20-108">용어 정의</span><span class="sxs-lookup"><span data-stu-id="c6f20-108">Definition of terms</span></span>

<span data-ttu-id="c6f20-109">관계를 설명 하는 데 사용 되는 용어는 여러 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-109">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="c6f20-110">**종속 엔터티:** 외래 키 속성을 포함 하는 엔터티입니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-110">**Dependent entity:** This is the entity that contains the foreign key properties.</span></span> <span data-ttu-id="c6f20-111">경우에 따라 관계의 ' 자식 '이 라고도 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-111">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="c6f20-112">**주 엔터티:** 기본/대체 키 속성을 포함 하는 엔터티입니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-112">**Principal entity:** This is the entity that contains the primary/alternate key properties.</span></span> <span data-ttu-id="c6f20-113">경우에 따라 관계의 ' 부모 ' 라고도 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-113">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="c6f20-114">**주 키:** 주 엔터티를 고유 하 게 식별 하는 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-114">**Principal key:** The properties that uniquely identify the principal entity.</span></span> <span data-ttu-id="c6f20-115">기본 키 또는 대체 키 일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="c6f20-116">**외래 키:** 관련 엔터티의 주 키 값을 저장 하는 데 사용 되는 종속 엔터티의 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-116">**Foreign key:** The properties in the dependent entity that are used to store the principal key values for the related entity.</span></span>

* <span data-ttu-id="c6f20-117">**탐색 속성:** 관련 엔터티를 참조 하는 보안 주체 및/또는 종속 엔터티에 대해 정의 된 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-117">**Navigation property:** A property defined on the principal and/or dependent entity that references the related entity.</span></span>

  * <span data-ttu-id="c6f20-118">**컬렉션 탐색 속성:** 여러 관련 엔터티에 대 한 참조를 포함 하는 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-118">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="c6f20-119">**참조 탐색 속성:** 단일 관련 엔터티에 대 한 참조를 포함 하는 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-119">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="c6f20-120">**역방향 탐색 속성:** 특정 탐색 속성을 설명할 때이 용어는 관계의 다른 쪽 end에 있는 탐색 속성을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-120">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>
  
* <span data-ttu-id="c6f20-121">**자체 참조 관계:** 종속 엔터티와 주 엔터티 형식이 동일한 관계입니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-121">**Self-referencing relationship:** A relationship in which the dependent and the principal entity types are the same.</span></span>

<span data-ttu-id="c6f20-122">다음 코드에서는 `Blog`와 간의 일 대 다 관계를 보여 줍니다 `Post`</span><span class="sxs-lookup"><span data-stu-id="c6f20-122">The following code shows a one-to-many relationship between `Blog` and `Post`</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Full)]

* <span data-ttu-id="c6f20-123">`Post` 종속 엔터티입니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-123">`Post` is the dependent entity</span></span>

* <span data-ttu-id="c6f20-124">`Blog`는 주 엔터티입니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-124">`Blog` is the principal entity</span></span>

* <span data-ttu-id="c6f20-125">`Blog.BlogId`는 주 키 (이 경우에는 대체 키가 아닌 기본 키)입니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-125">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="c6f20-126">외래 키 `Post.BlogId`</span><span class="sxs-lookup"><span data-stu-id="c6f20-126">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="c6f20-127">`Post.Blog`은 참조 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-127">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="c6f20-128">`Blog.Posts` 컬렉션 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-128">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="c6f20-129">`Post.Blog`은 `Blog.Posts`의 역 탐색 속성 이며 그 반대의 경우도 마찬가지입니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-129">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

## <a name="conventions"></a><span data-ttu-id="c6f20-130">규칙</span><span class="sxs-lookup"><span data-stu-id="c6f20-130">Conventions</span></span>

<span data-ttu-id="c6f20-131">기본적으로 형식에서 탐색 속성이 검색 되 면 관계가 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-131">By default, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="c6f20-132">속성은 가리키는 형식이 현재 데이터베이스 공급자에 의해 스칼라 형식으로 매핑될 수 없는 경우 탐색 속성으로 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-132">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="c6f20-133">규칙에 따라 검색 되는 관계는 항상 주 엔터티의 기본 키를 대상으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-133">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="c6f20-134">대체 키를 대상으로 하려면 흐름 API를 사용 하 여 추가 구성을 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-134">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="c6f20-135">완전히 정의 된 관계</span><span class="sxs-lookup"><span data-stu-id="c6f20-135">Fully defined relationships</span></span>

<span data-ttu-id="c6f20-136">관계의 가장 일반적인 패턴은 관계의 양쪽 end에 정의 된 탐색 속성 및 종속 엔터티 클래스에 정의 된 외래 키 속성을 포함 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-136">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="c6f20-137">두 형식 사이에 탐색 속성 쌍이 있으면 동일한 관계의 반전 탐색 속성으로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-137">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="c6f20-138">종속 엔터티에 이러한 패턴 중 하 나와 일치 하는 이름의 속성이 포함 되어 있으면 외래 키로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-138">If the dependent entity contains a property with a name matching one of these patterns then it will be configured as the foreign key:</span></span>
  * `<navigation property name><principal key property name>`
  * `<navigation property name>Id`
  * `<principal entity name><principal key property name>`
  * `<principal entity name>Id`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Full&highlight=6,15-16)]

<span data-ttu-id="c6f20-139">이 예제에서는 강조 표시 된 속성을 사용 하 여 관계를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-139">In this example the highlighted properties will be used to configure the relationship.</span></span>

> [!NOTE]
> <span data-ttu-id="c6f20-140">속성이 기본 키 이거나 주 키와 호환 되지 않는 유형인 경우 외래 키로 구성 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-140">If the property is the primary key or is of a type not compatible with the principal key then it won't be configured as the foreign key.</span></span>

> [!NOTE]
> <span data-ttu-id="c6f20-141">EF Core 3.0 이전에 주 키 속성과 정확히 같은 이름의 속성이 [외래 키와 일치 했습니다](https://github.com/aspnet/EntityFrameworkCore/issues/13274) .</span><span class="sxs-lookup"><span data-stu-id="c6f20-141">Before EF Core 3.0 the property named exactly the same as the principal key property [was also matched as the foreign key](https://github.com/aspnet/EntityFrameworkCore/issues/13274)</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="c6f20-142">외래 키 속성이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-142">No foreign key property</span></span>

<span data-ttu-id="c6f20-143">종속 엔터티 클래스에 외래 키 속성을 정의 하는 것이 좋지만 필수는 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-143">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="c6f20-144">외래 키 속성을 찾을 수 없는 경우에는 이름 `<navigation property name><principal key property name>` 또는 종속 형식에 탐색이 없는 경우 `<principal entity name><principal key property name>` [섀도 외래 키 속성이](shadow-properties.md) 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-144">If no foreign key property is found, a [shadow foreign key property](shadow-properties.md) will be introduced with the name `<navigation property name><principal key property name>` or `<principal entity name><principal key property name>` if no navigation is present on the dependent type.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=NoForeignKey&highlight=6,15)]

<span data-ttu-id="c6f20-145">이 예제에서는 탐색 이름이 중복 되기 때문에 그림자 외래 키가 `BlogId` 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-145">In this example the shadow foreign key is `BlogId` because prepending the navigation name would be redundant.</span></span>

> [!NOTE]
> <span data-ttu-id="c6f20-146">이름이 같은 속성이 이미 있으면 섀도 속성 이름 뒤에 숫자가 붙습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-146">If a property with the same name already exists then the shadow property name will be suffixed with a number.</span></span>

### <a name="single-navigation-property"></a><span data-ttu-id="c6f20-147">단일 탐색 속성</span><span class="sxs-lookup"><span data-stu-id="c6f20-147">Single navigation property</span></span>

<span data-ttu-id="c6f20-148">탐색 속성을 하나만 포함 하 고 (역 탐색은 없고 외래 키 속성이 없는 경우) 규칙으로 정의 된 관계를 가질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-148">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="c6f20-149">단일 탐색 속성과 외래 키 속성을 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-149">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=OneNavigation&highlight=6)]

### <a name="limitations"></a><span data-ttu-id="c6f20-150">제한 사항</span><span class="sxs-lookup"><span data-stu-id="c6f20-150">Limitations</span></span>

<span data-ttu-id="c6f20-151">두 형식 간에 여러 탐색 속성을 정의 하는 경우 (즉, 서로를 가리키는 탐색 쌍이 둘 이상인 경우) 탐색 속성이 나타내는 관계가 모호 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-151">When there are multiple navigation properties defined between two types (that is, more than just one pair of navigations that point to each other) the relationships represented by the navigation properties are ambiguous.</span></span> <span data-ttu-id="c6f20-152">모호성을 해결 하려면 수동으로 구성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-152">You will need to manually configure them to resolve the ambiguity.</span></span>

### <a name="cascade-delete"></a><span data-ttu-id="c6f20-153">계단식 삭제</span><span class="sxs-lookup"><span data-stu-id="c6f20-153">Cascade delete</span></span>

<span data-ttu-id="c6f20-154">규칙에 따라 cascade delete는 필수 관계의 경우 *cascade* 로 설정 되 고 선택적 관계의 경우 *clientsetnull* 로 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-154">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="c6f20-155">*Cascade* 는 종속 엔터티만 삭제 됨을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-155">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="c6f20-156">*Clientsetnull* 은 메모리에 로드 되지 않은 종속 엔터티가 변경 되지 않은 상태로 유지 되며 수동으로 삭제 하거나 유효한 주 엔터티를 가리키도록 업데이트 해야 함을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-156">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="c6f20-157">메모리에 로드 된 엔터티의 경우 EF Core는 외래 키 속성을 null로 설정 하려고 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-157">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="c6f20-158">필수 및 선택적 관계 간의 차이점은 [필수 및 선택적 관계](#required-and-optional-relationships) 섹션을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6f20-158">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="c6f20-159">규칙에 사용 되는 다른 삭제 동작 및 기본값에 대 한 자세한 내용은 [Cascade delete](../saving/cascade-delete.md) 를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6f20-159">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="manual-configuration"></a><span data-ttu-id="c6f20-160">수동 구성</span><span class="sxs-lookup"><span data-stu-id="c6f20-160">Manual configuration</span></span>

### <a name="fluent-api"></a>[<span data-ttu-id="c6f20-161">흐름 API</span><span class="sxs-lookup"><span data-stu-id="c6f20-161">Fluent API</span></span>](#tab/fluent-api)

<span data-ttu-id="c6f20-162">흐름 API에서 관계를 구성 하려면 먼저 관계를 구성 하는 탐색 속성을 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="c6f20-163">`HasOne` 또는 `HasMany`는 구성을 시작 하는 엔터티 형식에 대 한 탐색 속성을 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="c6f20-164">그런 다음 `WithOne` 또는 `WithMany`에 대 한 호출을 연결 하 여 역 탐색을 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="c6f20-165">`HasOne`/`WithOne` 참조 탐색 속성에 사용 되 고 `HasMany`/`WithMany` 컬렉션 탐색 속성에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?name=NoForeignKey&highlight=8-10)]

### <a name="data-annotations"></a>[<span data-ttu-id="c6f20-166">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="c6f20-166">Data annotations</span></span>](#tab/data-annotations)

<span data-ttu-id="c6f20-167">데이터 주석을 사용 하 여 종속 및 주요 엔터티의 탐색 속성이 페어링 되는 방법을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-167">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="c6f20-168">이는 일반적으로 두 엔터티 형식 간에 둘 이상의 탐색 속성 쌍이 있는 경우에 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-168">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?name=InverseProperty&highlight=20,23)]

> [!NOTE]
> <span data-ttu-id="c6f20-169">종속 엔터티의 속성에 [Required]만 사용 하 여 관계의 requiredness에 영향을 줄 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-169">You can only use [Required] on properties on the dependent entity to impact the requiredness of the relationship.</span></span> <span data-ttu-id="c6f20-170">[필수]는 일반적으로 주 엔터티 탐색에서 무시 되지만 엔터티가 종속 항목이 되도록 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-170">[Required] on the navigation from the principal entity is usually ignored, but it may cause the entity to become the dependent one.</span></span>

> [!NOTE]
> <span data-ttu-id="c6f20-171">데이터 주석 `[ForeignKey]` 및 `[InverseProperty]`는 `System.ComponentModel.DataAnnotations.Schema` 네임 스페이스에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-171">The data annotations `[ForeignKey]` and `[InverseProperty]` are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span> <span data-ttu-id="c6f20-172">`[Required]` `System.ComponentModel.DataAnnotations` 네임 스페이스에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-172">`[Required]` is available in the `System.ComponentModel.DataAnnotations` namespace.</span></span>

---

### <a name="single-navigation-property"></a><span data-ttu-id="c6f20-173">단일 탐색 속성</span><span class="sxs-lookup"><span data-stu-id="c6f20-173">Single navigation property</span></span>

<span data-ttu-id="c6f20-174">탐색 속성이 하나만 있는 경우에는 `WithOne` 및 `WithMany`에 대 한 매개 변수가 없는 오버 로드가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-174">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="c6f20-175">이는 개념적으로 관계의 다른 쪽 끝에 참조 또는 컬렉션이 있지만 엔터티 클래스에 포함 된 탐색 속성이 없다는 것을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-175">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?name=OneNavigation&highlight=8-10)]

### <a name="foreign-key"></a><span data-ttu-id="c6f20-176">외래 키</span><span class="sxs-lookup"><span data-stu-id="c6f20-176">Foreign key</span></span>

#### <a name="fluent-api-simple-key"></a>[<span data-ttu-id="c6f20-177">흐름 API (단순 키)</span><span class="sxs-lookup"><span data-stu-id="c6f20-177">Fluent API (simple key)</span></span>](#tab/fluent-api-simple-key)

<span data-ttu-id="c6f20-178">흐름 API를 사용 하 여 지정 된 관계에 대 한 외래 키 속성으로 사용할 속성을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-178">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?name=ForeignKey&highlight=11)]

#### <a name="fluent-api-composite-key"></a>[<span data-ttu-id="c6f20-179">흐름 API (복합 키)</span><span class="sxs-lookup"><span data-stu-id="c6f20-179">Fluent API (composite key)</span></span>](#tab/fluent-api-composite-key)

<span data-ttu-id="c6f20-180">흐름 API를 사용 하 여 지정 된 관계에 대 한 복합 외래 키 속성으로 사용할 속성을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-180">You can use the Fluent API to configure which properties should be used as the composite foreign key properties for a given relationship:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?name=CompositeForeignKey&highlight=13)]

#### <a name="data-annotations-simple-key"></a>[<span data-ttu-id="c6f20-181">데이터 주석 (단순 키)</span><span class="sxs-lookup"><span data-stu-id="c6f20-181">Data annotations (simple key)</span></span>](#tab/data-annotations-simple-key)

<span data-ttu-id="c6f20-182">데이터 주석을 사용 하 여 지정 된 관계에 대 한 외래 키 속성으로 사용할 속성을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-182">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="c6f20-183">이는 일반적으로 외래 키 속성이 규칙에 의해 검색 되지 않는 경우에 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-183">This is typically done when the foreign key property is not discovered by convention:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?name=ForeignKey&highlight=17)]

> [!TIP]  
> <span data-ttu-id="c6f20-184">`[ForeignKey]` 주석은 관계의 탐색 속성 중 하나에 배치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-184">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="c6f20-185">종속 엔터티 클래스의 탐색 속성으로 이동할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-185">It does not need to go on the navigation property in the dependent entity class.</span></span>

> [!NOTE]
> <span data-ttu-id="c6f20-186">탐색 속성에서 `[ForeignKey]`를 사용 하 여 지정 된 속성은 종속 형식에 존재할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-186">The property specified using `[ForeignKey]` on a navigation property doesn't need to exist the the dependent type.</span></span> <span data-ttu-id="c6f20-187">이 경우 지정 된 이름이 섀도 외래 키를 만드는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-187">In this case the specified name will be used to create a shadow foreign key.</span></span>

---

#### <a name="shadow-foreign-key"></a><span data-ttu-id="c6f20-188">섀도 외래 키</span><span class="sxs-lookup"><span data-stu-id="c6f20-188">Shadow foreign key</span></span>

<span data-ttu-id="c6f20-189">`HasForeignKey(...)`의 문자열 오버 로드를 사용 하 여 그림자 속성을 외래 키로 구성할 수 있습니다. 자세한 내용은 [섀도 속성](shadow-properties.md) 을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6f20-189">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="c6f20-190">섀도 속성을 외래 키로 사용 하기 전에이를 명시적으로 모델에 추가 하는 것이 좋습니다 (아래 참조).</span><span class="sxs-lookup"><span data-stu-id="c6f20-190">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs?name=ShadowForeignKey&highlight=10,16)]

#### <a name="foreign-key-constraint-name"></a><span data-ttu-id="c6f20-191">Foreign key 제약 조건 이름</span><span class="sxs-lookup"><span data-stu-id="c6f20-191">Foreign key constraint name</span></span>

<span data-ttu-id="c6f20-192">규칙에 따라 관계형 데이터베이스를 대상으로 지정 하는 경우 foreign key 제약 조건의 이름은<foreign key property name> _<principal type name>_ <dependent type name>FK_ 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-192">By convention, when targeting a relational database, foreign key constraints are named FK_<dependent type name>_<principal type name>_<foreign key property name>.</span></span> <span data-ttu-id="c6f20-193">복합 외래 키의 경우에는 쉼표로 구분 된 외래 키 속성 이름 목록이 <foreign key property name> 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-193">For composite foreign keys <foreign key property name> becomes an underscore separated list of foreign key property names.</span></span>

<span data-ttu-id="c6f20-194">제약 조건 이름을 다음과 같이 구성할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-194">You can also configure the constraint name as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ConstraintName.cs?name=ConstraintName&highlight=6-7)]

### <a name="without-navigation-property"></a><span data-ttu-id="c6f20-195">탐색 속성 없음</span><span class="sxs-lookup"><span data-stu-id="c6f20-195">Without navigation property</span></span>

<span data-ttu-id="c6f20-196">탐색 속성을 반드시 제공할 필요는 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-196">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="c6f20-197">관계의 한쪽에 외래 키를 제공 하기만 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-197">You can simply provide a foreign key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?name=NoNavigation&highlight=8-11)]

### <a name="principal-key"></a><span data-ttu-id="c6f20-198">주 키</span><span class="sxs-lookup"><span data-stu-id="c6f20-198">Principal key</span></span>

<span data-ttu-id="c6f20-199">외래 키가 기본 키 이외의 속성을 참조 하도록 하려는 경우 흐름 API를 사용 하 여 관계에 대 한 principal key 속성을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-199">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="c6f20-200">주 키로 구성 하는 속성은 자동으로 [대체 키](alternate-keys.md)로 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-200">The property that you configure as the principal key will automatically be setup as an [alternate key](alternate-keys.md).</span></span>

#### <a name="simple-key"></a>[<span data-ttu-id="c6f20-201">단순 키</span><span class="sxs-lookup"><span data-stu-id="c6f20-201">Simple key</span></span>](#tab/simple-key)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

#### <a name="composite-key"></a>[<span data-ttu-id="c6f20-202">복합 키</span><span class="sxs-lookup"><span data-stu-id="c6f20-202">Composite key</span></span>](#tab/composite-key)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=CompositePrincipalKey&highlight=11)]

> [!WARNING]  
> <span data-ttu-id="c6f20-203">보안 주체 키 속성을 지정 하는 순서는 외래 키에 대해 지정 된 순서와 일치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-203">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

---

### <a name="required-and-optional-relationships"></a><span data-ttu-id="c6f20-204">필수 및 선택적 관계</span><span class="sxs-lookup"><span data-stu-id="c6f20-204">Required and optional relationships</span></span>

<span data-ttu-id="c6f20-205">흐름 API를 사용 하 여 관계가 필수 또는 선택 사항 인지 여부를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-205">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="c6f20-206">이는 궁극적으로 외래 키 속성이 필수 또는 선택 사항 인지 여부를 제어 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-206">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="c6f20-207">이는 섀도 상태 외래 키를 사용 하는 경우에 가장 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-207">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="c6f20-208">엔터티 클래스에 외래 키 속성이 있는 경우 외래 키 속성이 필수 인지 선택 사항에 따라 관계를 결정 합니다. 자세한 내용은 [필수 및 선택적 속성](required-optional.md) 을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6f20-208">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=6)]

> [!NOTE]
> <span data-ttu-id="c6f20-209">또한 `IsRequired(false)`를 호출 하면 외래 키 속성이 다르게 구성 된 경우를 제외 하 고이 옵션을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-209">Calling `IsRequired(false)` also makes the foreign key property optional unless it's configured otherwise.</span></span>

### <a name="cascade-delete"></a><span data-ttu-id="c6f20-210">계단식 삭제</span><span class="sxs-lookup"><span data-stu-id="c6f20-210">Cascade delete</span></span>

<span data-ttu-id="c6f20-211">흐름 API를 사용 하 여 지정 된 관계에 대 한 cascade delete 동작을 명시적으로 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-211">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="c6f20-212">각 옵션에 대 한 자세한 내용은 [계단식 삭제](../saving/cascade-delete.md) 를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6f20-212">See [Cascade Delete](../saving/cascade-delete.md) for a detailed discussion of each option.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=6)]

## <a name="other-relationship-patterns"></a><span data-ttu-id="c6f20-213">기타 관계 패턴</span><span class="sxs-lookup"><span data-stu-id="c6f20-213">Other relationship patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="c6f20-214">일 대 일</span><span class="sxs-lookup"><span data-stu-id="c6f20-214">One-to-one</span></span>

<span data-ttu-id="c6f20-215">일대일 관계에는 양쪽 모두에 대 한 참조 탐색 속성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-215">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="c6f20-216">이는 일 대 다 관계와 동일한 규칙을 따르고 외래 키 속성에 고유 인덱스가 도입 되어 각 보안 주체와 하나의 종속만 관련 되도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-216">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=OneToOne&highlight=6,15-16)]

> [!NOTE]  
> <span data-ttu-id="c6f20-217">EF는 외래 키 속성을 검색 하는 기능에 따라 엔터티 중 하나를 종속 항목으로 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-217">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="c6f20-218">잘못 된 엔터티가 종속 항목으로 선택 된 경우 흐름 API를 사용 하 여이를 수정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-218">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="c6f20-219">흐름 API를 사용 하 여 관계를 구성 하는 경우 `HasOne` 및 `WithOne` 메서드를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-219">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="c6f20-220">외래 키를 구성 하는 경우 종속 엔터티 형식-아래 목록에서 `HasForeignKey`에 제공 된 제네릭 매개 변수를 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-220">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="c6f20-221">일 대 다 관계에서 참조 탐색을 사용 하는 엔터티가 종속 항목 이며 컬렉션이 포함 된 엔터티가 주 서버 인지를 명확 하 게 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-221">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="c6f20-222">그러나이는 일 대 일 관계가 아니므로 명시적으로 정의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-222">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a><span data-ttu-id="c6f20-223">다대다</span><span class="sxs-lookup"><span data-stu-id="c6f20-223">Many-to-many</span></span>

<span data-ttu-id="c6f20-224">조인 테이블을 나타내기 위해 엔터티 클래스가 없는 다 대 다 관계는 아직 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-224">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="c6f20-225">그러나 조인 테이블에 대해 엔터티 클래스를 포함 하 고 두 개의 개별 일대다 관계를 매핑하여 다대다 관계를 나타낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6f20-225">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11-14,16-19,39-46)]
