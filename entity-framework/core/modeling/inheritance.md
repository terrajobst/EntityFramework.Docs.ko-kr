---
title: 상속-EF Core
description: Entity Framework Core를 사용 하 여 엔터티 형식 상속을 구성 하는 방법
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 507854e3acc0347adee612e516b3e2e0b10f55cf
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502172"
---
# <a name="inheritance"></a><span data-ttu-id="37d44-103">상속</span><span class="sxs-lookup"><span data-stu-id="37d44-103">Inheritance</span></span>

<span data-ttu-id="37d44-104">EF는 .NET 형식 계층 구조를 데이터베이스에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-104">EF can map a .NET type hierarchy to a database.</span></span> <span data-ttu-id="37d44-105">이를 통해 기본 및 파생 형식을 사용 하 여 일반적인 방식으로 코드에 .NET 엔터티를 작성 하 고 EF에서 적절 한 데이터베이스 스키마를 원활 하 게 만들고 쿼리를 실행할 수 있습니다. 형식 계층 구조를 매핑하는 방법에 대 한 실제 세부 정보는 공급자에 따라 다릅니다. 이 페이지에서는 관계형 데이터베이스의 컨텍스트에서 상속 지원을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-105">This allows you to write your .NET entities in code as usual, using base and derived types, and have EF seamlessly create the appropriate database schema, issue queries, etc. The actual details of how a type hierarchy is mapped are provider-dependent; this page describes inheritance support in the context of a relational database.</span></span>

<span data-ttu-id="37d44-106">현재 EF Core는 TPH (계층당 하나의 테이블) 패턴만 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-106">At the moment, EF Core only supports the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="37d44-107">TPH는 단일 테이블을 사용 하 여 계층의 모든 형식에 대 한 데이터를 저장 하 고, 판별자 열은 각 행이 나타내는 유형을 식별 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-107">TPH uses a single table to store the data for all types in the hierarchy, and a discriminator column is used to identify which type each row represents.</span></span>

> [!NOTE]
> <span data-ttu-id="37d44-108">EF6에서 지원 되는 형식당 하나의 테이블 (TPT) 및 테이블당 (테이블당) (TPC)는 EF Core에서 아직 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-108">The table-per-type (TPT) and table-per-concrete-type (TPC), which are supported by EF6, are not yet supported by EF Core.</span></span> <span data-ttu-id="37d44-109">TPT는 EF Core 5.0에 대해 계획 된 주요 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-109">TPT is a major feature planned for EF Core 5.0.</span></span>

## <a name="entity-type-hierarchy-mapping"></a><span data-ttu-id="37d44-110">엔터티 형식 계층 구조 매핑</span><span class="sxs-lookup"><span data-stu-id="37d44-110">Entity type hierarchy mapping</span></span>

<span data-ttu-id="37d44-111">규칙에 따라 두 개 이상의 상속 된 형식이 모델에 명시적으로 포함 된 경우에만 EF가 상속을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-111">By convention, EF will only set up inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="37d44-112">EF는 모델에 포함 되지 않은 기본 또는 파생 형식을 자동으로 검색 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-112">EF will not automatically scan for base or derived types that are not otherwise included in the model.</span></span>

<span data-ttu-id="37d44-113">상속 계층 구조의 각 형식에 대해 DbSet를 노출 하 여 모델에 형식을 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-113">You can include types in the model by exposing a DbSet for each type in the inheritance hierarchy:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?name=InheritanceDbSets&highlight=3-4)]

<span data-ttu-id="37d44-114">이 모델은 다음 데이터베이스 스키마에 매핑됩니다 (각 행에 저장 되는 *블로그의* 유형을 식별 하는 암시적으로 만든 *판별자* 열에 유의).</span><span class="sxs-lookup"><span data-stu-id="37d44-114">This model be mapped to the following database schema (note the implicitly-created *Discriminator* column, which identifies which type of *Blog* is stored in each row):</span></span>

![image](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="37d44-116">TPH 매핑을 사용 하는 경우 필요에 따라 데이터베이스 열이 자동으로 nullable로 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span> <span data-ttu-id="37d44-117">예를 들어 일반 *블로그* 인스턴스에는 해당 속성이 없으므로 *rssurl* 열은 null을 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-117">For example, the *RssUrl* column is nullable because regular *Blog* instances do not have that property.</span></span>

<span data-ttu-id="37d44-118">계층 구조에 있는 하나 이상의 엔터티에 대해 DbSet을 노출 하지 않으려는 경우 흐름 API를 사용 하 여 모델에 포함 되도록 할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-118">If you don't want to expose a DbSet for one or more entities in the hierarchy, you can also use the Fluent API to ensure they are included in the model.</span></span>

> [!TIP]
> <span data-ttu-id="37d44-119">규칙을 사용 하지 않는 경우 `HasBaseType`를 사용 하 여 명시적으로 기본 형식을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-119">If you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span> <span data-ttu-id="37d44-120">`.HasBaseType((Type)null)`를 사용 하 여 계층에서 엔터티 형식을 제거할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-120">You can also use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="discriminator-configuration"></a><span data-ttu-id="37d44-121">판별자 구성</span><span class="sxs-lookup"><span data-stu-id="37d44-121">Discriminator configuration</span></span>

<span data-ttu-id="37d44-122">판별자 열의 이름 및 유형과 계층의 각 유형을 식별 하는 데 사용 되는 값을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-122">You can configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorConfiguration.cs?name=DiscriminatorConfiguration&highlight=4-6)]

<span data-ttu-id="37d44-123">위의 예제에서 EF는 계층의 기본 엔터티에서 명시적으로 판별자를 [그림자 속성](xref:core/modeling/shadow-properties) 으로 추가 했습니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-123">In the examples above, EF added the discriminator implicitly as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="37d44-124">이 속성은 다음과 같이 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-124">This property can be configured like any other:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorPropertyConfiguration.cs?name=DiscriminatorPropertyConfiguration&highlight=4-5)]

<span data-ttu-id="37d44-125">마지막으로, 판별자를 엔터티의 일반 .NET 속성에 매핑할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-125">Finally, the discriminator can also be mapped to a regular .NET property in your entity:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs?name=NonShadowDiscriminator&highlight=4)]

## <a name="shared-columns"></a><span data-ttu-id="37d44-126">공유 열</span><span class="sxs-lookup"><span data-stu-id="37d44-126">Shared columns</span></span>

<span data-ttu-id="37d44-127">기본적으로 계층의 두 형제 엔터티 형식에 동일한 이름의 속성이 있는 경우 두 개의 개별 열에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-127">By default, when two sibling entity types in the hierarchy have a property with the same name, they will be mapped to two separate columns.</span></span> <span data-ttu-id="37d44-128">그러나 형식이 동일한 경우 동일한 데이터베이스 열에 매핑될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="37d44-128">However, if their type is identical they can be mapped to the same database column:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs?name=SharedTPHColumns&highlight=9,13)]
