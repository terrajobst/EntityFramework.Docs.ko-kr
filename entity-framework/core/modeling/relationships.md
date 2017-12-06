---
title: "관계-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 1732d32643effb0f12111191ed4ba3abb4182d93
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="relationships"></a><span data-ttu-id="004d6-102">Relationships</span><span class="sxs-lookup"><span data-stu-id="004d6-102">Relationships</span></span>

<span data-ttu-id="004d6-103">관계는 두 엔터티 간에 서로 관계 된 합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="004d6-104">관계형 데이터베이스에 외래 키 제약 조건에 의해 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="004d6-105">이 문서에 있는 샘플 대부분 개념을 보여 주기 위해는 일 대 다 관계를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="004d6-106">일대일 및 다 대 다 관계의 예제는 [다른 관계 패턴](#other-relationship-patterns) 문서의 끝에 나오는 섹션.</span><span class="sxs-lookup"><span data-stu-id="004d6-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="004d6-107">용어 정의</span><span class="sxs-lookup"><span data-stu-id="004d6-107">Definition of Terms</span></span>

<span data-ttu-id="004d6-108">관계를 설명 하는 데 사용 하는 약관의 여러 가지</span><span class="sxs-lookup"><span data-stu-id="004d6-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="004d6-109">**종속 엔터티에:** 외래 키 속성을 포함 하는 엔터티입니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="004d6-110">관계의 'child' 라고도 합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="004d6-111">**주 엔터티:** 기본/대체 키 속성을 포함 하는 엔터티입니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="004d6-112">관계의 'parent' 라고도 합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="004d6-113">**외래 키:** 엔터티 관련 된 주 키 속성의 값을 저장 하는 데 사용 되는 종속 엔터티의 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="004d6-114">**주 키:** 주 엔터티를 고유 하 게 식별 하는 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="004d6-115">이 기본 키 또는 대체 키 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="004d6-116">**탐색 속성:** 관련된 엔터티를 참조를 포함 하는 보안 주체 및/또는 종속 엔터티에 정의 된 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="004d6-117">**컬렉션 탐색 속성:** 많은 관련된 엔터티에 대 한 참조를 포함 하는 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="004d6-118">**탐색 속성을 참조:** 단일 관련 엔터티에 대 한 참조를 포함 하는 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="004d6-119">**역 탐색 속성:** 특정 탐색 속성을 설명할 때이 용어는 관계의 반대쪽에 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="004d6-120">다음 코드 목록 간의 일 대 다 관계를 보여 줍니다. `Blog` 및`Post`</span><span class="sxs-lookup"><span data-stu-id="004d6-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="004d6-121">`Post`종속 엔터티는</span><span class="sxs-lookup"><span data-stu-id="004d6-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="004d6-122">`Blog`주 엔터티는</span><span class="sxs-lookup"><span data-stu-id="004d6-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="004d6-123">`Post.BlogId`외래 키</span><span class="sxs-lookup"><span data-stu-id="004d6-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="004d6-124">`Blog.BlogId`(이 경우에 대체 키가 아닌 기본 키)는 주 키</span><span class="sxs-lookup"><span data-stu-id="004d6-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="004d6-125">`Post.Blog`속성은 참조 탐색 속성은</span><span class="sxs-lookup"><span data-stu-id="004d6-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="004d6-126">`Blog.Posts`컬렉션 탐색 속성</span><span class="sxs-lookup"><span data-stu-id="004d6-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="004d6-127">`Post.Blog`역 탐색 속성은 `Blog.Posts` (및 그 반대의 경우도 마찬가지)</span><span class="sxs-lookup"><span data-stu-id="004d6-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="004d6-128">규칙</span><span class="sxs-lookup"><span data-stu-id="004d6-128">Conventions</span></span>

<span data-ttu-id="004d6-129">규칙에 따라 형식에서 검색 하는 탐색 속성에 있으면 관계가 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="004d6-130">이것이 가리키는 형식이 매핑될 수 없습니다. 스칼라 형식으로 현재 데이터베이스 공급자가 하는 경우 속성 탐색 속성을 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="004d6-131">규칙으로 검색 되는 관계를 주 엔터티의 기본 키를 대상 항상 합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="004d6-132">대체 키를 대상으로 Fluent API를 사용 하 여 추가 구성을 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="004d6-133">완전 하 게 정의 된 관계</span><span class="sxs-lookup"><span data-stu-id="004d6-133">Fully Defined Relationships</span></span>

<span data-ttu-id="004d6-134">관계에 대 한 가장 일반적인 패턴은 종속 엔터티 클래스에 정의 된 외래 키 속성 및 관계의 양쪽 끝에 정의 된 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="004d6-135">두 형식 간에 한 쌍의 탐색 속성 항목이 있으면 동일한 관계의 역 탐색 속성으로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="004d6-136">종속 엔터티 속성을 포함 하는 경우 `<primary key property name>`, `<navigation property name><primary key property name>`, 또는 `<principal entity name><primary key property name>` 다음 외래 키로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="004d6-137">두 형식 간의 정의 된 여러 탐색 속성이 있는 경우 (즉, 서로를 가리키는 탐색의 고유한 쌍을 둘 이상의) 및 규칙에 따라 관계가 없으면 만들어집니다 다음 수동으로 식별 하도록를 구성 해야 합니다는 방법을 위쪽 탐색 속성 쌍입니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-137">If there are multiple navigation properties defined between two types (i.e. more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="004d6-138">외래 키 속성 없음</span><span class="sxs-lookup"><span data-stu-id="004d6-138">No Foreign Key Property</span></span>

<span data-ttu-id="004d6-139">종속 엔터티 클래스에 정의 된 외래 키 속성이 있어야 하는 것이 좋지만, 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="004d6-140">그림자 외래 키 속성 이름으로 도입 될 것 없는 외래 키 속성이 있으면 `<navigation property name><principal key property name>` (참조 [그림자 속성](shadow-properties.md) 자세한 정보에 대 한).</span><span class="sxs-lookup"><span data-stu-id="004d6-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="004d6-141">단일 탐색 속성</span><span class="sxs-lookup"><span data-stu-id="004d6-141">Single Navigation Property</span></span>

<span data-ttu-id="004d6-142">(없음 역 탐색 및 외래 키 속성 없음) 한 탐색 속성을 포함 하 여 관계가 규칙에 따라 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="004d6-143">또한 단일 탐색 속성 및 외래 키 속성을 가질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="004d6-144">하위 삭제</span><span class="sxs-lookup"><span data-stu-id="004d6-144">Cascade Delete</span></span>

<span data-ttu-id="004d6-145">규칙에 따라 하위 삭제로 설정 됩니다 *Cascade* 필요한 관계에 대 한 및 *ClientSetNull* 선택적 관계에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="004d6-146">*Cascade* 의미 종속 된 엔터티도 삭제 됩니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="004d6-147">*ClientSetNull* 메모리에 로드 되지 않는 종속 엔터티가 유지 되는 의미 변경 하지 않고 및 수동으로 삭제 하거나 업데이트 해야 유효한 주 엔터티를 가리키도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="004d6-148">메모리에 로드 되는 엔터티, EF 코어 외래 키 속성을 null로 설정 하려고 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="004d6-149">참조는 [필수 및 선택적 관계](#required-and-optional-relationships) 필수 및 선택적 관계 간의 차이 대 한 섹션.</span><span class="sxs-lookup"><span data-stu-id="004d6-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="004d6-150">참조 [모두 삭제](../saving/cascade-delete.md) 서로 다른 방법에 대 한 자세한 내용은 동작 및 규칙에 따라 사용 하는 기본값을 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="004d6-151">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="004d6-151">Data Annotations</span></span>

<span data-ttu-id="004d6-152">관계를 구성 하는 데 사용할 수 있는 두 개의 데이터 주석이 `[ForeignKey]` 및 `[InverseProperty]`합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="004d6-153">[ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="004d6-153">[ForeignKey]</span></span>

<span data-ttu-id="004d6-154">주어진된 관계에 대 한 외래 키 속성으로 사용할 해야 속성을 구성 하려면 데이터 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-154">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="004d6-155">이 규칙에 따라 외래 키 속성이 검색 되지 않는 경우 일반적으로 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-155">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> <span data-ttu-id="004d6-156">`[ForeignKey]` 주석 관계에서 두 탐색 속성 중 하나에 배치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-156">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="004d6-157">종속 엔터티 클래스에는 탐색 속성에는 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-157">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="004d6-158">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="004d6-158">[InverseProperty]</span></span>

<span data-ttu-id="004d6-159">종속 및 주체 엔터티에 대 한 탐색 속성 쌍 연결 하는 방법을 구성 하려면 데이터 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-159">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="004d6-160">이 두 엔터티 형식 간의 탐색 속성 쌍 하나만 있으면 일반적으로 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-160">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a><span data-ttu-id="004d6-161">Fluent API</span><span class="sxs-lookup"><span data-stu-id="004d6-161">Fluent API</span></span>

<span data-ttu-id="004d6-162">Fluent API에서 관계를 구성 하려면 먼저 관계를 구성 하는 탐색 속성을 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="004d6-163">`HasOne`또는 `HasMany` 에서 구성을 시작 하는 엔터티 형식의 탐색 속성을 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="004d6-164">다음에 대 한 호출 체인 `WithOne` 또는 `WithMany` 역 탐색을 식별할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="004d6-165">`HasOne`/`WithOne`참조 탐색 속성에 사용 되 고 `HasMany` / `WithMany` 컬렉션 탐색 속성에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a><span data-ttu-id="004d6-166">단일 탐색 속성</span><span class="sxs-lookup"><span data-stu-id="004d6-166">Single Navigation Property</span></span>

<span data-ttu-id="004d6-167">한 탐색 속성만 있는 경우 매개 변수가 없는 오버 로드가 `WithOne` 및 `WithMany`합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-167">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="004d6-168">개념적으로 참조 또는 컬렉션, 관계의 다른 쪽 end에 있지만 엔터티 클래스에 포함 된 탐색 속성이 없기 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-168">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a><span data-ttu-id="004d6-169">외래 키</span><span class="sxs-lookup"><span data-stu-id="004d6-169">Foreign Key</span></span>

<span data-ttu-id="004d6-170">주어진된 관계에 대 한 외래 키 속성으로 사용할 해야 속성을 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-170">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

<span data-ttu-id="004d6-171">다음 코드 샘플에는 복합 외래 키를 구성 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-171">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

<span data-ttu-id="004d6-172">문자열 오버 로드를 사용할 수 있습니다 `HasForeignKey(...)` 외래 키로 그림자 속성을 구성 하려면 (참조 [그림자 속성](shadow-properties.md) 자세한 정보에 대 한).</span><span class="sxs-lookup"><span data-stu-id="004d6-172">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="004d6-173">모델에 (아래와 같이) 외래 키로 사용 하기 전에 섀도 속성을 명시적으로 추가 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-173">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a><span data-ttu-id="004d6-174">보안 주체가 키</span><span class="sxs-lookup"><span data-stu-id="004d6-174">Principal Key</span></span>

<span data-ttu-id="004d6-175">외래 키 기본 키 이외의 속성을 참조 하려는 경우에 관계에 대 한 주 키 속성을 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-175">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="004d6-176">대체 키 파일로 설치할 하면 주 키가 자동으로 구성 하는 속성 (참조 [대체 키](alternate-keys.md) 자세한 정보에 대 한).</span><span class="sxs-lookup"><span data-stu-id="004d6-176">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

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

<span data-ttu-id="004d6-177">다음 코드 샘플에 복합 기본 키를 구성 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-177">The following code listing shows how to configure a composite principal key.</span></span>

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
> <span data-ttu-id="004d6-178">주 키 속성을 지정할 수 있는 순서에는 외래 키에 대 한 지정 된 순서와 일치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-178">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="004d6-179">필수 및 선택적 관계</span><span class="sxs-lookup"><span data-stu-id="004d6-179">Required and Optional Relationships</span></span>

<span data-ttu-id="004d6-180">관계를 필수 또는 선택적 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-180">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="004d6-181">궁극적으로 외래 키 속성이 필수 인지 선택적인 지 여부를 제어 합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-181">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="004d6-182">그림자 상태 외래 키를 사용 하는 경우 가장 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-182">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="004d6-183">엔터티 클래스에서 외래 키 속성이 있는 경우 requiredness 관계의 외래 키 속성이 필수 인지 선택적인 여부에 따라 결정 됩니다 (참조 [필수 및 선택적 속성](required-optional.md) 더 정보)입니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-183">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

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

### <a name="cascade-delete"></a><span data-ttu-id="004d6-184">하위 삭제</span><span class="sxs-lookup"><span data-stu-id="004d6-184">Cascade Delete</span></span>

<span data-ttu-id="004d6-185">주어진된 관계에 대 한 cascade delete 동작을 명시적으로 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-185">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="004d6-186">참조 [모두 삭제](../saving/cascade-delete.md) 각 옵션에 대 한 자세한 내용은 대 한 Data 섹션.</span><span class="sxs-lookup"><span data-stu-id="004d6-186">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

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

## <a name="other-relationship-patterns"></a><span data-ttu-id="004d6-187">다른 관계 패턴</span><span class="sxs-lookup"><span data-stu-id="004d6-187">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="004d6-188">한 일</span><span class="sxs-lookup"><span data-stu-id="004d6-188">One-to-one</span></span>

<span data-ttu-id="004d6-189">한 일 관계 양쪽에서 참조 탐색 속성을 가집니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-189">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="004d6-190">일 대 다 관계와 동일한 규칙을 따르지만 고유 인덱스는 하나만 종속 각 보안 주체에 관련 되도록 외래 키 속성에 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-190">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

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
> <span data-ttu-id="004d6-191">EF 외래 키 속성을 검색 하는 기능에 따라 종속 되도록 엔터티 중 하나를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-191">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="004d6-192">종속 항목으로 잘못 된 엔터티를 선택 하면이 문제를 해결 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-192">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="004d6-193">관계 Fluent API를 구성할 때 사용 된 `HasOne` 및 `WithOne` 메서드.</span><span class="sxs-lookup"><span data-stu-id="004d6-193">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="004d6-194">종속 엔터티 형식-을 지정 해야 하는 외래 키를 구성 하는 경우에 제공 되는 제네릭 매개 변수 확인 `HasForeignKey` 아래 목록에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-194">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="004d6-195">1 대 다 관계에서 참조 탐색 엔터티가 종속 항목 및 컬렉션에 있는 주 서버는 명확있지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-195">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="004d6-196">있지만이 따라서 일대일 관계에는 명시적으로 정의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-196">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

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

### <a name="many-to-many"></a><span data-ttu-id="004d6-197">다 대 다</span><span class="sxs-lookup"><span data-stu-id="004d6-197">Many-to-many</span></span>

<span data-ttu-id="004d6-198">다 대 다 관계를 나타내는 조인 테이블 엔터티 클래스 없이 아직 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-198">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="004d6-199">그러나 별도 두 대 다 관계 매핑 및 조인 테이블에 대 한 엔터티 클래스를 포함 하 여 다 대 다 관계를 나타낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="004d6-199">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

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
