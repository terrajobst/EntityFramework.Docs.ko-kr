---
title: 관계-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 793401362788e865c89ce01b6246b1ba14c36c8a
ms.sourcegitcommit: 8b9568211d37a1c36da9533fa1ac2ef063b0bf8c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2019
ms.locfileid: "66815003"
---
# <a name="relationships"></a><span data-ttu-id="a806f-102">관계</span><span class="sxs-lookup"><span data-stu-id="a806f-102">Relationships</span></span>

<span data-ttu-id="a806f-103">두 엔터티를 정의 하는 관계를 서로 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="a806f-104">관계형 데이터베이스에서 외래 키 제약 조건이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="a806f-105">대부분이 문서의 샘플의 개념을 설명 하에 일 대 다 관계를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="a806f-106">한 일 및 다 대 다 관계의 예제를 보려면 합니다 [다른 관계 패턴](#other-relationship-patterns) 문서의 끝에 있는 섹션입니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="a806f-107">용어 정의</span><span class="sxs-lookup"><span data-stu-id="a806f-107">Definition of Terms</span></span>

<span data-ttu-id="a806f-108">관계를 설명 하는 데 사용 되는 약관의 여러 가지</span><span class="sxs-lookup"><span data-stu-id="a806f-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="a806f-109">**종속 엔터티:** 이 외래 키 속성을 포함 하는 엔터티입니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="a806f-110">관계의 'child' 라고도 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="a806f-111">**주 엔터티:** 이 기본/대체 키 속성을 포함 하는 엔터티입니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="a806f-112">관계의 'parent' 라고도 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="a806f-113">**외래 키:** 엔터티 관련 된 보안 주체 키 속성의 값을 저장 하는 데 사용 되는 종속 엔터티의 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="a806f-114">**계정 키:** 주 엔터티를 고유 하 게 식별 하는 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="a806f-115">이 기본 키 또는 대체 키 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="a806f-116">**탐색 속성:** 관련된 엔터티를 대 한 참조를 포함 하는 보안 주체 및/또는 종속 엔터티에 정의 된 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="a806f-117">**컬렉션 탐색 속성:** 여러 관련된 엔터티에 대 한 참조를 포함 하는 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="a806f-118">**참조 탐색 속성:** 단일 관련 엔터티에 대 한 참조를 보유 하는 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="a806f-119">**역 탐색 속성:** 특정 탐색 속성에 설명 하는 경우이 용어 관계의 다른 쪽에 탐색 속성을 가리킵니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="a806f-120">다음 코드 목록 사이 일 대 다 관계를 보여 줍니다. `Blog` 및 `Post`</span><span class="sxs-lookup"><span data-stu-id="a806f-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="a806f-121">`Post` 종속 엔터티는</span><span class="sxs-lookup"><span data-stu-id="a806f-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="a806f-122">`Blog` 주 엔터티는</span><span class="sxs-lookup"><span data-stu-id="a806f-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="a806f-123">`Post.BlogId` 외래 키</span><span class="sxs-lookup"><span data-stu-id="a806f-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="a806f-124">`Blog.BlogId` 계정 키 (이 경우에 대체 키를 사용 하지 않고 기본 키)</span><span class="sxs-lookup"><span data-stu-id="a806f-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="a806f-125">`Post.Blog` 참조 탐색 속성</span><span class="sxs-lookup"><span data-stu-id="a806f-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="a806f-126">`Blog.Posts` 컬렉션 탐색 속성은</span><span class="sxs-lookup"><span data-stu-id="a806f-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="a806f-127">`Post.Blog` 역 탐색 속성인 `Blog.Posts` (또는 그 반대로)</span><span class="sxs-lookup"><span data-stu-id="a806f-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="a806f-128">규칙</span><span class="sxs-lookup"><span data-stu-id="a806f-128">Conventions</span></span>

<span data-ttu-id="a806f-129">규칙에 따라 형식에서 검색 하는 탐색 속성 경우 관계가 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="a806f-130">속성을 가리키는 형식이 매핑될 수 없습니다. 스칼라 형식으로 현재 데이터베이스 공급자가 하는 경우 탐색 속성을 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="a806f-131">주 엔터티의 기본 키 규칙에서 검색 된 관계 항상 대상으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="a806f-132">대체 키를 대상으로 Fluent API를 사용 하 여 추가 구성을 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="a806f-133">완벽 하 게 정의 된 관계</span><span class="sxs-lookup"><span data-stu-id="a806f-133">Fully Defined Relationships</span></span>

<span data-ttu-id="a806f-134">관계에 대 한 가장 일반적인 패턴은 종속 엔터티 클래스에 정의 된 외래 키 속성 및 관계의 양쪽에 정의 된 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="a806f-135">두 형식 간의 탐색 속성 쌍이 없으면 동일한 관계의 역 탐색 속성으로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="a806f-136">종속 엔터티 속성을 포함 하는 경우 `<primary key property name>`, `<navigation property name><primary key property name>`, 또는 `<principal entity name><primary key property name>` 다음 외래 키로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="a806f-137">두 형식 간에 정의 된 여러 탐색 속성이 많은 경우 (즉, 서로를 가리키는 탐색의 고유한 쌍을 둘 이상), 다음 관계가 없으면 규칙에서 만들어지고 수동으로 식별 하도록를 구성 해야 하는 방법을 위로 탐색 속성 쌍입니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="a806f-138">외래 키 속성 없음</span><span class="sxs-lookup"><span data-stu-id="a806f-138">No Foreign Key Property</span></span>

<span data-ttu-id="a806f-139">종속 엔터티 클래스에 정의 된 외래 키 속성이 있어야 하는 것이 좋습니다, 있지만 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="a806f-140">섀도 외래 키 속성 이름의 도입 될 외래 키 속성이 없으면 `<navigation property name><principal key property name>` (참조 [섀도 속성](shadow-properties.md) 자세한).</span><span class="sxs-lookup"><span data-stu-id="a806f-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="a806f-141">단일 탐색 속성</span><span class="sxs-lookup"><span data-stu-id="a806f-141">Single Navigation Property</span></span>

<span data-ttu-id="a806f-142">(역 없는 탐색 및 외래 키 속성이) 탐색 속성 중 하나를 포함 하 여 규칙에 따라 정의 된 관계에 적합 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="a806f-143">단일 탐색 속성 및 외래 키 속성 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="a806f-144">하위 삭제</span><span class="sxs-lookup"><span data-stu-id="a806f-144">Cascade Delete</span></span>

<span data-ttu-id="a806f-145">규칙에 따라 계단식 삭제로 설정 됩니다 *Cascade* 필요한 관계에 대 한 및 *ClientSetNull* 선택적 관계에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="a806f-146">*Cascade* 종속 엔터티도 삭제 됩니다 것을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="a806f-147">*ClientSetNull* 메모리에 로드 되지 않은 종속 엔터티가 남아 있음을 변경 되지 않음 및 수동으로 삭제 하거나 업데이트 해야 유효한 주 엔터티를 가리키도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="a806f-148">메모리에 로드 되는 엔터티의 경우 EF Core는 외래 키 속성을 null로 설정 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="a806f-149">참조 된 [필수 및 선택적 관계](#required-and-optional-relationships) 필수 및 선택적 관계 간의 차이점에 대 한 섹션입니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="a806f-150">참조 [Cascade Delete](../saving/cascade-delete.md) 다양 한 방법에 대 한 자세한 내용은 삭제 동작과 규칙에 따라 사용 하는 기본값에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a806f-151">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="a806f-151">Data Annotations</span></span>

<span data-ttu-id="a806f-152">관계를 구성 하려면 사용할 수 있는 두 데이터 주석이 `[ForeignKey]` 고 `[InverseProperty]`입니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span> <span data-ttu-id="a806f-153">사용할 수는 `System.ComponentModel.DataAnnotations.Schema` 네임 스페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-153">These are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="a806f-154">[ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="a806f-154">[ForeignKey]</span></span>

<span data-ttu-id="a806f-155">지정 된 관계에 대 한 외래 키 속성으로 사용할 해야 속성을 구성 하려면 데이터 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-155">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="a806f-156">외래 키 속성은 규칙에 따라 검색 되지 경우 일반적으로 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-156">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="a806f-157">`[ForeignKey]` 주석 관계의 탐색 속성 중 하나에 배치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-157">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="a806f-158">종속 엔터티 클래스의 탐색 속성을 이동 하지 않아도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-158">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="a806f-159">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="a806f-159">[InverseProperty]</span></span>

<span data-ttu-id="a806f-160">종속 및 주체 엔터티에서 탐색 속성 쌍 방법을 구성 하려면 데이터 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-160">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="a806f-161">두 엔터티 형식 간의 탐색 속성 쌍이 하나만 있으면 일반적으로 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-161">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a><span data-ttu-id="a806f-162">Fluent API</span><span class="sxs-lookup"><span data-stu-id="a806f-162">Fluent API</span></span>

<span data-ttu-id="a806f-163">Fluent API의 관계를 구성 하 여 관계를 구성 하는 탐색 속성을 식별 하 여 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-163">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="a806f-164">`HasOne` 또는 `HasMany` 에서 구성을 시작 하는 엔터티 형식의 탐색 속성을 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-164">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="a806f-165">다음에 대 한 호출을 연결할 `WithOne` 또는 `WithMany` 역 탐색을 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-165">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="a806f-166">`HasOne`/`WithOne` 참조 탐색 속성에 사용 되 고 `HasMany` / `WithMany` 컬렉션 탐색 속성에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-166">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a><span data-ttu-id="a806f-167">단일 탐색 속성</span><span class="sxs-lookup"><span data-stu-id="a806f-167">Single Navigation Property</span></span>

<span data-ttu-id="a806f-168">하나의 탐색 속성이 있는 경우 매개 변수가 없는 오버 로드가 `WithOne` 고 `WithMany`입니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-168">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="a806f-169">이 나타내지만 참조 또는 관계의 반대쪽 끝에서 수집 하는 개념적 엔터티 클래스에 포함 된 탐색 속성이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-169">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="a806f-170">외래 키</span><span class="sxs-lookup"><span data-stu-id="a806f-170">Foreign Key</span></span>

<span data-ttu-id="a806f-171">지정 된 관계에 대 한 외래 키 속성으로 사용할 해야 속성을 구성 하는 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-171">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?highlight=17)]

<span data-ttu-id="a806f-172">다음 코드 샘플에는 복합 외래 키를 구성 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-172">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="a806f-173">문자열 오버 로드를 사용할 수 있습니다 `HasForeignKey(...)` 외래 키로 섀도 속성을 구성 하려면 (참조 [섀도 속성](shadow-properties.md) 자세한).</span><span class="sxs-lookup"><span data-stu-id="a806f-173">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="a806f-174">(아래와 같이) 외래 키로 사용 하기 전에 모델에 섀도 속성을 명시적으로 추가 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-174">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a><span data-ttu-id="a806f-175">탐색 속성 사용 하지 않고</span><span class="sxs-lookup"><span data-stu-id="a806f-175">Without Navigation Property</span></span>

<span data-ttu-id="a806f-176">반드시 탐색 속성을 제공할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-176">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="a806f-177">단순히 관계의 한쪽 외래 키를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-177">You can simply provide a Foreign Key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a><span data-ttu-id="a806f-178">주체 키</span><span class="sxs-lookup"><span data-stu-id="a806f-178">Principal Key</span></span>

<span data-ttu-id="a806f-179">외래 키 기본 키 이외의 속성 참조를 원한다 면 관계에 대 한 주 키 속성을 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-179">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="a806f-180">대체 키로 설정할 키는 보안 주체를 자동으로 구성 하는 속성 (참조 [대체 키](alternate-keys.md) 자세한).</span><span class="sxs-lookup"><span data-stu-id="a806f-180">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

<span data-ttu-id="a806f-181">다음 코드 목록은 복합 기본 키를 구성 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-181">The following code listing shows how to configure a composite principal key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> <span data-ttu-id="a806f-182">주체 키 속성을 지정 하는 순서는 외래 키에 대 한 지정 된 순서를 일치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-182">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="a806f-183">필수 및 선택적 관계</span><span class="sxs-lookup"><span data-stu-id="a806f-183">Required and Optional Relationships</span></span>

<span data-ttu-id="a806f-184">관계 필수 인지 선택적인 지 여부를 구성 하는 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-184">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="a806f-185">궁극적으로이 외래 키 속성이 필수 인지 선택적인 지 여부를 제어 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-185">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="a806f-186">섀도 상태 외래 키를 사용 하는 경우 가장 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-186">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="a806f-187">외래 키 속성이 엔터티 클래스에 있는 경우 requiredness 관계의 외래 키 속성의 필수 또는 선택 인지 여부에 따라 결정 됩니다 (참조 [필수 및 선택적 속성](required-optional.md) 자세한 정보)입니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-187">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

### <a name="cascade-delete"></a><span data-ttu-id="a806f-188">하위 삭제</span><span class="sxs-lookup"><span data-stu-id="a806f-188">Cascade Delete</span></span>

<span data-ttu-id="a806f-189">지정 된 관계에 대 한 하위 삭제 동작을 명시적으로 구성 하는 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-189">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="a806f-190">참조 [Cascade Delete](../saving/cascade-delete.md) 각 옵션에 자세히 알아보려면 저장 데이터 섹션에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-190">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .OnDelete(DeleteBehavior.Cascade);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a><span data-ttu-id="a806f-191">다른 관계 패턴</span><span class="sxs-lookup"><span data-stu-id="a806f-191">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="a806f-192">한 일</span><span class="sxs-lookup"><span data-stu-id="a806f-192">One-to-one</span></span>

<span data-ttu-id="a806f-193">한 일 관계 양쪽 모두에 대 한 참조 탐색 속성을 가집니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-193">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="a806f-194">이러한에 일 대 다 관계와 동일한 규칙을 따르지만 고유 인덱스는 하나만 종속 각 보안 주체 관련 되도록 외래 키 속성에 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-194">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> <span data-ttu-id="a806f-195">EF는 외래 키 속성을 검색 하는 기능에 따라 종속 되도록 엔터티 중 하나를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-195">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="a806f-196">종속 항목으로 잘못 된 엔터티를 선택 하면이 문제를 해결 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-196">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="a806f-197">Fluent API를 사용 하 여 관계를 구성할 때 사용 합니다 `HasOne` 고 `WithOne` 메서드.</span><span class="sxs-lookup"><span data-stu-id="a806f-197">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="a806f-198">종속 엔터티 형식으로 지정 해야 하는 외래 키를 구성 하는 경우 제공 된 제네릭 매개 변수가 알 수 있습니다. `HasForeignKey` 아래 목록에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-198">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="a806f-199">1 대 다 관계 명확한 참조 탐색이 있는 엔터티가 종속 및 컬렉션을 사용 하 여 보안 주체가 되는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-199">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="a806f-200">이 이지만 있으므로 한 일 관계에서 명시적으로 정의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-200">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a><span data-ttu-id="a806f-201">다 대 다</span><span class="sxs-lookup"><span data-stu-id="a806f-201">Many-to-many</span></span>

<span data-ttu-id="a806f-202">다 대 다 관계 없이 조인 테이블을 나타내는 엔터티 클래스는 아직 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-202">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="a806f-203">그러나 조인 테이블에 별도 두 가지에 일 대 다 관계 매핑 엔터티 클래스를 포함 하 여 다 대 다 관계를 나타낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a806f-203">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
