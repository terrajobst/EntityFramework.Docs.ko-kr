---
title: Entity Framework Glossary - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
ms.openlocfilehash: 4e42e5870879524b814cecdc361e688d36f0180f
ms.sourcegitcommit: 6c4e06bc62d98442530e93a44725e38e59483d42
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58131381"
---
# <a name="entity-framework-glossary"></a><span data-ttu-id="03488-102">Entity Framework 용어집</span><span class="sxs-lookup"><span data-stu-id="03488-102">Entity Framework Glossary</span></span>
## <a name="code-first"></a><span data-ttu-id="03488-103">Code First</span><span class="sxs-lookup"><span data-stu-id="03488-103">Code First</span></span>
<span data-ttu-id="03488-104">코드를 사용 하 여 Entity Framework 모델을 만드는 중입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-104">Creating an Entity Framework model using code.</span></span> <span data-ttu-id="03488-105">모델은 데이터베이스를 기존 또는 새 데이터베이스를 대상으로 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="03488-105">The model can target an existing database or a new database.</span></span>

## <a name="context"></a><span data-ttu-id="03488-106">컨텍스트</span><span class="sxs-lookup"><span data-stu-id="03488-106">Context</span></span>
<span data-ttu-id="03488-107">데이터베이스를 쿼리하고 데이터를 저장할 수 있도록 세션을 나타내는 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-107">A class that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="03488-108">컨텍스트는 DbContext 나 ObjectContext 클래스에서 파생 됩니다.</span><span class="sxs-lookup"><span data-stu-id="03488-108">A context derives from the DbContext or ObjectContext class.</span></span>

## <a name="convention-code-first"></a><span data-ttu-id="03488-109">규칙 (첫 번째 코드)</span><span class="sxs-lookup"><span data-stu-id="03488-109">Convention (Code First)</span></span>
<span data-ttu-id="03488-110">Entity Framework를 사용 하 여 수업에서 모델의 형태를 유추 하는 규칙.</span><span class="sxs-lookup"><span data-stu-id="03488-110">A rule that Entity Framework uses to infer the shape of you model from your classes.</span></span>

## <a name="database-first"></a><span data-ttu-id="03488-111">먼저 데이터베이스</span><span class="sxs-lookup"><span data-stu-id="03488-111">Database First</span></span>
<span data-ttu-id="03488-112">EF 디자이너를 사용 하 여 Entity Framework 모델을 만드는 대상으로 하는 기존 데이터베이스입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-112">Creating an Entity Framework model, using the EF Designer, that targets an existing database.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="03488-113">즉시 로드</span><span class="sxs-lookup"><span data-stu-id="03488-113">Eager loading</span></span>
<span data-ttu-id="03488-114">여기서 한 형식의 엔터티에 대 한 쿼리를 관련된 엔터티도 로드를 쿼리의 일부로 관련된 데이터를 로드 하는 패턴입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-114">A pattern of loading related data where a query for one type of entity also loads related entities as part of the query.</span></span>

## <a name="ef-designer"></a><span data-ttu-id="03488-115">EF 디자이너</span><span class="sxs-lookup"><span data-stu-id="03488-115">EF Designer</span></span>
<span data-ttu-id="03488-116">상자 및 줄을 사용 하 여 Entity Framework 모델을 만들 수 있도록 Visual Studio의 시각적 디자이너입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-116">A visual designer in Visual Studio that allows you to create an Entity Framework model using boxes and lines.</span></span>

## <a name="entity"></a><span data-ttu-id="03488-117">엔터티</span><span class="sxs-lookup"><span data-stu-id="03488-117">Entity</span></span>
<span data-ttu-id="03488-118">고객, 제품 및 주문과 같은 응용 프로그램 데이터를 나타내는 클래스 또는 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-118">A class or object that represents application data such as customers, products, and orders.</span></span>

## <a name="entity-data-model"></a><span data-ttu-id="03488-119">엔터티 데이터 모델</span><span class="sxs-lookup"><span data-stu-id="03488-119">Entity Data Model</span></span>
<span data-ttu-id="03488-120">엔터티 및 이들 간의 관계를 설명 하는 모델입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-120">A model that describes entities and the relationships between them.</span></span> <span data-ttu-id="03488-121">개념적 모델을 설명 하기 위해 EDM을 사용 하는 EF 개발자 프로그램입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-121">EF uses EDM to describe the conceptual model against which the developer programs.</span></span> <span data-ttu-id="03488-122">Dr에 도입 된 엔터티 관계 모델을 기반으로 EDM입니다. Peter Chen 합니다.</span><span class="sxs-lookup"><span data-stu-id="03488-122">EDM builds on the Entity Relationship model introduced by Dr. Peter Chen.</span></span> <span data-ttu-id="03488-123">EDM은 Microsoft의 개발자 및 서버 기술 제품군 한다는 공통 데이터 모델의 주요 목표를 사용 하 여 원래 개발 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="03488-123">The EDM was originally developed with the primary goal of becoming the common data model across a suite of developer and server technologies from Microsoft.</span></span> <span data-ttu-id="03488-124">EDM은 OData 프로토콜의 일부로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="03488-124">EDM is also used as part of the OData protocol.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="03488-125">명시적 로드</span><span class="sxs-lookup"><span data-stu-id="03488-125">Explicit loading</span></span>
<span data-ttu-id="03488-126">관련된 개체가 로드 되는 API를 호출 하 여 관련된 데이터를 로드 하는 패턴입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-126">A pattern of loading related data where related objects are loaded by calling an API.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="03488-127">Fluent API</span><span class="sxs-lookup"><span data-stu-id="03488-127">Fluent API</span></span>
<span data-ttu-id="03488-128">Code First 모델을 구성 하는 데 사용할 수 있는 API입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-128">An API that can be used to configure a Code First model.</span></span>

## <a name="foreign-key-association"></a><span data-ttu-id="03488-129">외래 키 연결</span><span class="sxs-lookup"><span data-stu-id="03488-129">Foreign key association</span></span>
<span data-ttu-id="03488-130">외래 키를 나타내는 속성의 종속 엔터티 클래스에 포함 되어 있는 엔터티 간의 연결입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-130">An association between entities where a property that represents the foreign key is included in the class of the dependent entity.</span></span> <span data-ttu-id="03488-131">예를 들어 제품 CategoryId 속성을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="03488-131">For example, Product contains a CategoryId property.</span></span>

## <a name="identifying-relationship"></a><span data-ttu-id="03488-132">관계를 식별합니다.</span><span class="sxs-lookup"><span data-stu-id="03488-132">Identifying relationship</span></span>
<span data-ttu-id="03488-133">주 엔터티의 기본 키가 종속 엔터티의 기본 키 일부인 관계입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-133">A relationship where the primary key of the principal entity is part of the primary key of the dependent entity.</span></span> <span data-ttu-id="03488-134">이러한 종류의 관계에서 종속 엔터티는 주 엔터티 없이 존재할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="03488-134">In this kind of relationship, the dependent entity cannot exist without the principal entity.</span></span>

## <a name="independent-association"></a><span data-ttu-id="03488-135">독립 연결</span><span class="sxs-lookup"><span data-stu-id="03488-135">Independent association</span></span>
<span data-ttu-id="03488-136">엔터티 간 연결의 종속 엔터티 클래스에 외래 키를 나타내는 속성이 없는 경우.</span><span class="sxs-lookup"><span data-stu-id="03488-136">An association between entities where there is no property representing the foreign key in the class of the dependent entity.</span></span> <span data-ttu-id="03488-137">예를 들어 Product 클래스는 범주 있지만 CategoryId 속성이 없는 관계를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="03488-137">For example, a Product class contains a relationship to Category but no CategoryId property.</span></span> <span data-ttu-id="03488-138">Entity Framework는 두 개의 연결 end에 있는 엔터티의 상태와 독립적으로 연결의 상태를 추적 합니다.</span><span class="sxs-lookup"><span data-stu-id="03488-138">Entity Framework tracks the state of the association independently of the state of the entities at the two association ends.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="03488-139">지연 로드</span><span class="sxs-lookup"><span data-stu-id="03488-139">Lazy loading</span></span>
<span data-ttu-id="03488-140">패턴 탐색 속성에 액세스할 때 관련된 개체가 로드 자동으로 관련된 데이터 로드입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-140">A pattern of loading related data where related objects are automatically loaded when a navigation property is accessed.</span></span>

## <a name="model-first"></a><span data-ttu-id="03488-141">먼저 모델</span><span class="sxs-lookup"><span data-stu-id="03488-141">Model First</span></span>
<span data-ttu-id="03488-142">EF 디자이너를 사용 하 여 Entity Framework 모델을 만드는 다음 데 사용 되는 새 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="03488-142">Creating an Entity Framework model, using the EF Designer, that is then used to create a new database.</span></span>

## <a name="navigation-property"></a><span data-ttu-id="03488-143">탐색 속성</span><span class="sxs-lookup"><span data-stu-id="03488-143">Navigation property</span></span>
<span data-ttu-id="03488-144">다른 엔터티를 참조 하는 엔터티는 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-144">A property of an entity that references another entity.</span></span> <span data-ttu-id="03488-145">예를 들어 제품 범주 탐색 속성을 포함 및 범주 제품 탐색 속성을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="03488-145">For example, Product contains a Category navigation property and Category contains a Products navigation property.</span></span>

## <a name="poco"></a><span data-ttu-id="03488-146">POCO</span><span class="sxs-lookup"><span data-stu-id="03488-146">POCO</span></span>
<span data-ttu-id="03488-147">Plain Old CLR 개체에 대 한 머리글자어입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-147">Acronym for Plain-Old CLR Object.</span></span> <span data-ttu-id="03488-148">간단한 사용자 정의 클래스에서 임의 프레임 워크 종속성이 없는입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-148">A simple user class that has no dependencies with any framework.</span></span> <span data-ttu-id="03488-149">EF EntityObject에서 파생 되지 않은 모든 인터페이스를 구현, EF에 정의 된 모든 특성을 전달 하는 엔터티 클래스의 컨텍스트에서 합니다.</span><span class="sxs-lookup"><span data-stu-id="03488-149">In the context of EF, an entity class that does not derive from EntityObject, implements any interfaces or carries any attributes defined in EF.</span></span> <span data-ttu-id="03488-150">지 속성 프레임 워크에서 분리 되는 이러한 엔터티 클래스는 "지 속성 무시" 있다고도 합니다.</span><span class="sxs-lookup"><span data-stu-id="03488-150">Such entity classes that are decoupled from the persistence framework are also said to be "persistence ignorant".</span></span>  

## <a name="relationship-inverse"></a><span data-ttu-id="03488-151">관계 역</span><span class="sxs-lookup"><span data-stu-id="03488-151">Relationship inverse</span></span>
<span data-ttu-id="03488-152">예를 들어 제품 관계의 반대쪽 끝에 있습니다. 범주 및 범주입니다. 제품입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-152">The opposite end of a relationship, for example, product.Category and category.Product.</span></span>

## <a name="self-tracking-entity"></a><span data-ttu-id="03488-153">자동 추적 엔터티</span><span class="sxs-lookup"><span data-stu-id="03488-153">Self-tracking entity</span></span>
<span data-ttu-id="03488-154">N 계층 개발에 도움이 되는 코드 생성 템플릿에서 만들어진 엔터티.</span><span class="sxs-lookup"><span data-stu-id="03488-154">An entity built from a code generation template that helps with N-Tier development.</span></span>

## <a name="table-per-concrete-type-tpc"></a><span data-ttu-id="03488-155">구체적 형식당 테이블 형식 (TPC)</span><span class="sxs-lookup"><span data-stu-id="03488-155">Table-per-concrete type (TPC)</span></span>
<span data-ttu-id="03488-156">상속 계층 구조의 각 추상이 아닌 형식 구분 데이터베이스의 테이블에 매핑되는 위치에 매핑하는 방법.</span><span class="sxs-lookup"><span data-stu-id="03488-156">A method of mapping the inheritance where each non-abstract type in the hierarchy is mapped to separate table in the database.</span></span>

## <a name="table-per-hierarchy-tph"></a><span data-ttu-id="03488-157">테이블-당-TPH (계층)</span><span class="sxs-lookup"><span data-stu-id="03488-157">Table-per-hierarchy (TPH)</span></span>
<span data-ttu-id="03488-158">상속 계층 구조의 모든 형식은 데이터베이스에서 동일한 테이블에 매핑되는 위치에 매핑하는 방법.</span><span class="sxs-lookup"><span data-stu-id="03488-158">A method of mapping the inheritance where all types in the hierarchy are mapped to the same table in the database.</span></span> <span data-ttu-id="03488-159">판별자 열은 각 행 형식을 찾는 데 사용 하 여 연결입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-159">A discriminator column(s) is used to identify what type each row is associated with.</span></span>

## <a name="table-per-type-tpt"></a><span data-ttu-id="03488-160">테이블 형식당 TPT)</span><span class="sxs-lookup"><span data-stu-id="03488-160">Table-per-type (TPT)</span></span>
<span data-ttu-id="03488-161">매핑 상속 계층 구조에서 모든 형식의 공용 속성을 데이터베이스에서 동일한 테이블에 매핑됩니다. 하지만 각 유형에 고유한 속성은 별도 테이블에 매핑됩니다 위치 하는 방법.</span><span class="sxs-lookup"><span data-stu-id="03488-161">A method of mapping the inheritance where the common properties of all types in the hierarchy are mapped to the same table in the database, but properties unique to each type are mapped to a separate table.</span></span>

## <a name="type-discovery"></a><span data-ttu-id="03488-162">형식 검색</span><span class="sxs-lookup"><span data-stu-id="03488-162">Type discovery</span></span>
<span data-ttu-id="03488-163">Entity Framework 모델의 일부 여야 하는 형식을 식별 하는 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="03488-163">The process of identifying the types that should be part of an Entity Framework model.</span></span>
