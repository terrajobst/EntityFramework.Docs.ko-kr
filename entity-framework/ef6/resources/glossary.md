---
title: Entity Framework 용어집-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
uid: ef6/resources/glossary
ms.openlocfilehash: df0da4a68b3d2c882d9673417ee5fe335eccae2b
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656153"
---
# <a name="entity-framework-glossary"></a><span data-ttu-id="cfc7a-102">Entity Framework 용어집</span><span class="sxs-lookup"><span data-stu-id="cfc7a-102">Entity Framework Glossary</span></span>
## <a name="code-first"></a><span data-ttu-id="cfc7a-103">Code First</span><span class="sxs-lookup"><span data-stu-id="cfc7a-103">Code First</span></span>
<span data-ttu-id="cfc7a-104">코드를 사용 하 여 Entity Framework 모델 만들기</span><span class="sxs-lookup"><span data-stu-id="cfc7a-104">Creating an Entity Framework model using code.</span></span> <span data-ttu-id="cfc7a-105">모델은 기존 데이터베이스 또는 새 데이터베이스를 대상으로 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-105">The model can target an existing database or a new database.</span></span>

## <a name="context"></a><span data-ttu-id="cfc7a-106">컨텍스트</span><span class="sxs-lookup"><span data-stu-id="cfc7a-106">Context</span></span>
<span data-ttu-id="cfc7a-107">데이터베이스에 대 한 세션을 나타내는 클래스로,이를 통해 데이터를 쿼리하고 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-107">A class that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="cfc7a-108">컨텍스트는 DbContext 또는 ObjectContext 클래스에서 파생 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-108">A context derives from the DbContext or ObjectContext class.</span></span>

## <a name="convention-code-first"></a><span data-ttu-id="cfc7a-109">규칙 (Code First)</span><span class="sxs-lookup"><span data-stu-id="cfc7a-109">Convention (Code First)</span></span>
<span data-ttu-id="cfc7a-110">클래스에서 모델 셰이프를 유추 하는 데 사용 Entity Framework 하는 규칙입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-110">A rule that Entity Framework uses to infer the shape of you model from your classes.</span></span>

## <a name="database-first"></a><span data-ttu-id="cfc7a-111">Database First</span><span class="sxs-lookup"><span data-stu-id="cfc7a-111">Database First</span></span>
<span data-ttu-id="cfc7a-112">EF Designer를 사용 하 여 기존 데이터베이스를 대상으로 하는 Entity Framework 모델 만들기</span><span class="sxs-lookup"><span data-stu-id="cfc7a-112">Creating an Entity Framework model, using the EF Designer, that targets an existing database.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="cfc7a-113">즉시 로드</span><span class="sxs-lookup"><span data-stu-id="cfc7a-113">Eager loading</span></span>
<span data-ttu-id="cfc7a-114">특정 형식의 엔터티에 대 한 쿼리가 쿼리의 일부로 관련 엔터티를 로드 하는 관련 데이터를 로드 하는 패턴입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-114">A pattern of loading related data where a query for one type of entity also loads related entities as part of the query.</span></span>

## <a name="ef-designer"></a><span data-ttu-id="cfc7a-115">EF 디자이너</span><span class="sxs-lookup"><span data-stu-id="cfc7a-115">EF Designer</span></span>
<span data-ttu-id="cfc7a-116">상자와 선을 사용 하 여 Entity Framework 모델을 만들 수 있도록 하는 visual Studio의 비주얼 디자이너입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-116">A visual designer in Visual Studio that allows you to create an Entity Framework model using boxes and lines.</span></span>

## <a name="entity"></a><span data-ttu-id="cfc7a-117">엔터티</span><span class="sxs-lookup"><span data-stu-id="cfc7a-117">Entity</span></span>
<span data-ttu-id="cfc7a-118">고객, 제품 및 주문과 같은 애플리케이션 데이터를 나타내는 클래스 또는 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-118">A class or object that represents application data such as customers, products, and orders.</span></span>

## <a name="entity-data-model"></a><span data-ttu-id="cfc7a-119">엔터티 데이터 모델</span><span class="sxs-lookup"><span data-stu-id="cfc7a-119">Entity Data Model</span></span>
<span data-ttu-id="cfc7a-120">엔터티 및 엔터티 간의 관계를 설명 하는 모델입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-120">A model that describes entities and the relationships between them.</span></span> <span data-ttu-id="cfc7a-121">EF는 EDM을 사용 하 여 개발자 프로그램에 대 한 개념적 모델을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-121">EF uses EDM to describe the conceptual model against which the developer programs.</span></span> <span data-ttu-id="cfc7a-122">EDM은 Peter에 의해 도입 된 엔터티 관계 모델을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-122">EDM builds on the Entity Relationship model introduced by Dr. Peter Chen.</span></span> <span data-ttu-id="cfc7a-123">EDM은 원래 Microsoft의 개발자 및 서버 기술 제품군에 걸쳐 공통 데이터 모델을 사용 하는 주된 목표를 사용 하 여 개발 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-123">The EDM was originally developed with the primary goal of becoming the common data model across a suite of developer and server technologies from Microsoft.</span></span> <span data-ttu-id="cfc7a-124">EDM은 OData 프로토콜의 일부로도 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-124">EDM is also used as part of the OData protocol.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="cfc7a-125">명시적 로드</span><span class="sxs-lookup"><span data-stu-id="cfc7a-125">Explicit loading</span></span>
<span data-ttu-id="cfc7a-126">API를 호출 하 여 관련 개체를 로드 하는 관련 데이터를 로드 하는 패턴입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-126">A pattern of loading related data where related objects are loaded by calling an API.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="cfc7a-127">흐름 API</span><span class="sxs-lookup"><span data-stu-id="cfc7a-127">Fluent API</span></span>
<span data-ttu-id="cfc7a-128">Code First 모델을 구성 하는 데 사용할 수 있는 API입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-128">An API that can be used to configure a Code First model.</span></span>

## <a name="foreign-key-association"></a><span data-ttu-id="cfc7a-129">외래 키 연결</span><span class="sxs-lookup"><span data-stu-id="cfc7a-129">Foreign key association</span></span>
<span data-ttu-id="cfc7a-130">외래 키를 나타내는 속성이 종속 엔터티의 클래스에 포함 되는 엔터티 간의 연결입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-130">An association between entities where a property that represents the foreign key is included in the class of the dependent entity.</span></span> <span data-ttu-id="cfc7a-131">예를 들어 Product에는 CategoryId 속성이 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-131">For example, Product contains a CategoryId property.</span></span>

## <a name="identifying-relationship"></a><span data-ttu-id="cfc7a-132">식별 관계</span><span class="sxs-lookup"><span data-stu-id="cfc7a-132">Identifying relationship</span></span>
<span data-ttu-id="cfc7a-133">주 엔터티의 기본 키가 종속 엔터티의 기본 키 일부인 관계입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-133">A relationship where the primary key of the principal entity is part of the primary key of the dependent entity.</span></span> <span data-ttu-id="cfc7a-134">이러한 종류의 관계에서 종속 엔터티는 주 엔터티 없이 존재할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-134">In this kind of relationship, the dependent entity cannot exist without the principal entity.</span></span>

## <a name="independent-association"></a><span data-ttu-id="cfc7a-135">독립 연결</span><span class="sxs-lookup"><span data-stu-id="cfc7a-135">Independent association</span></span>
<span data-ttu-id="cfc7a-136">종속 엔터티의 클래스에서 외래 키를 나타내는 속성이 없는 엔터티 간의 연결입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-136">An association between entities where there is no property representing the foreign key in the class of the dependent entity.</span></span> <span data-ttu-id="cfc7a-137">예를 들어 Product 클래스는 Category에 대 한 관계를 포함 하지만 CategoryId 속성은 포함 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-137">For example, a Product class contains a relationship to Category but no CategoryId property.</span></span> <span data-ttu-id="cfc7a-138">Entity Framework는 연결의 상태를 두 연결 end에 있는 엔터티 상태와 독립적으로 추적 합니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-138">Entity Framework tracks the state of the association independently of the state of the entities at the two association ends.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="cfc7a-139">지연 로드</span><span class="sxs-lookup"><span data-stu-id="cfc7a-139">Lazy loading</span></span>
<span data-ttu-id="cfc7a-140">탐색 속성에 액세스할 때 관련 개체가 자동으로 로드 되는 관련 데이터를 로드 하는 패턴입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-140">A pattern of loading related data where related objects are automatically loaded when a navigation property is accessed.</span></span>

## <a name="model-first"></a><span data-ttu-id="cfc7a-141">Model First</span><span class="sxs-lookup"><span data-stu-id="cfc7a-141">Model First</span></span>
<span data-ttu-id="cfc7a-142">EF Designer를 사용 하 여 Entity Framework 모델을 만든 다음 새 데이터베이스를 만드는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-142">Creating an Entity Framework model, using the EF Designer, that is then used to create a new database.</span></span>

## <a name="navigation-property"></a><span data-ttu-id="cfc7a-143">탐색 속성</span><span class="sxs-lookup"><span data-stu-id="cfc7a-143">Navigation property</span></span>
<span data-ttu-id="cfc7a-144">다른 엔터티를 참조 하는 엔터티의 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-144">A property of an entity that references another entity.</span></span> <span data-ttu-id="cfc7a-145">예를 들어 Product에는 범주 탐색 속성이 포함 되 고 범주에는 Products 탐색 속성이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-145">For example, Product contains a Category navigation property and Category contains a Products navigation property.</span></span>

## <a name="poco"></a><span data-ttu-id="cfc7a-146">POCO</span><span class="sxs-lookup"><span data-stu-id="cfc7a-146">POCO</span></span>
<span data-ttu-id="cfc7a-147">일반-이전 CLR 개체의 머리글자어입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-147">Acronym for Plain-Old CLR Object.</span></span> <span data-ttu-id="cfc7a-148">모든 프레임 워크와 종속성이 없는 간단한 사용자 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-148">A simple user class that has no dependencies with any framework.</span></span> <span data-ttu-id="cfc7a-149">EF 컨텍스트에서 EntityObject에서 파생 되지 않은 엔터티 클래스는 인터페이스를 구현 하거나 EF에 정의 된 특성을 전달 합니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-149">In the context of EF, an entity class that does not derive from EntityObject, implements any interfaces or carries any attributes defined in EF.</span></span> <span data-ttu-id="cfc7a-150">지 속성 프레임 워크에서 분리 된 엔터티 클래스도 "지 속성 무시" 라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-150">Such entity classes that are decoupled from the persistence framework are also said to be "persistence ignorant".</span></span>  

## <a name="relationship-inverse"></a><span data-ttu-id="cfc7a-151">역 관계</span><span class="sxs-lookup"><span data-stu-id="cfc7a-151">Relationship inverse</span></span>
<span data-ttu-id="cfc7a-152">관계의 반대쪽 끝 (예: product)입니다. 범주 및 범주. 제품은.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-152">The opposite end of a relationship, for example, product.Category and category.Product.</span></span>

## <a name="self-tracking-entity"></a><span data-ttu-id="cfc7a-153">자동 추적 엔터티</span><span class="sxs-lookup"><span data-stu-id="cfc7a-153">Self-tracking entity</span></span>
<span data-ttu-id="cfc7a-154">N 계층 개발에 도움이 되는 코드 생성 템플릿에서 작성 된 엔터티입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-154">An entity built from a code generation template that helps with N-Tier development.</span></span>

## <a name="table-per-concrete-type-tpc"></a><span data-ttu-id="cfc7a-155">TPC (구체적 테이블 유형별)</span><span class="sxs-lookup"><span data-stu-id="cfc7a-155">Table-per-concrete type (TPC)</span></span>
<span data-ttu-id="cfc7a-156">계층의 각 비추상 형식이 데이터베이스의 개별 테이블에 매핑되는 상속을 매핑하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-156">A method of mapping the inheritance where each non-abstract type in the hierarchy is mapped to separate table in the database.</span></span>

## <a name="table-per-hierarchy-tph"></a><span data-ttu-id="cfc7a-157">계층당 하나의 테이블 (TPH)</span><span class="sxs-lookup"><span data-stu-id="cfc7a-157">Table-per-hierarchy (TPH)</span></span>
<span data-ttu-id="cfc7a-158">계층의 모든 형식이 데이터베이스의 동일한 테이블에 매핑되는 상속을 매핑하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-158">A method of mapping the inheritance where all types in the hierarchy are mapped to the same table in the database.</span></span> <span data-ttu-id="cfc7a-159">판별자 열은 각 행이 연결 된 유형을 식별 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-159">A discriminator column(s) is used to identify what type each row is associated with.</span></span>

## <a name="table-per-type-tpt"></a><span data-ttu-id="cfc7a-160">형식당 하나의 테이블 (TPT)</span><span class="sxs-lookup"><span data-stu-id="cfc7a-160">Table-per-type (TPT)</span></span>
<span data-ttu-id="cfc7a-161">계층 구조에 있는 모든 형식의 공통 속성이 데이터베이스의 동일한 테이블에 매핑되고 각 형식에 고유한 속성이 별도의 테이블에 매핑되는 상속을 매핑하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-161">A method of mapping the inheritance where the common properties of all types in the hierarchy are mapped to the same table in the database, but properties unique to each type are mapped to a separate table.</span></span>

## <a name="type-discovery"></a><span data-ttu-id="cfc7a-162">유형 검색</span><span class="sxs-lookup"><span data-stu-id="cfc7a-162">Type discovery</span></span>
<span data-ttu-id="cfc7a-163">Entity Framework 모델의 일부가 되어야 하는 형식을 식별 하는 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="cfc7a-163">The process of identifying the types that should be part of an Entity Framework model.</span></span>
