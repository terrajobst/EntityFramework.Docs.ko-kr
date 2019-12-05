---
title: 키 (기본)-EF Core
description: Entity Framework Core를 사용할 때 엔터티 형식에 대 한 키를 구성 하는 방법
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: fdaccb42259ba9dad97a05c626edd0291ca96cb0
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824613"
---
# <a name="keys-primary"></a><span data-ttu-id="d4799-103">키(기본)</span><span class="sxs-lookup"><span data-stu-id="d4799-103">Keys (primary)</span></span>

<span data-ttu-id="d4799-104">키는 각 엔터티 인스턴스에 대 한 기본 고유 식별자 역할을 합니다.</span><span class="sxs-lookup"><span data-stu-id="d4799-104">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="d4799-105">관계형 데이터베이스를 사용 하는 경우 *기본 키*의 개념에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="d4799-105">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="d4799-106">기본 키가 아닌 고유 식별자를 구성할 수도 있습니다 (자세한 내용은 [대체 키](alternate-keys.md) 참조).</span><span class="sxs-lookup"><span data-stu-id="d4799-106">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<span data-ttu-id="d4799-107">다음 방법 중 하나를 사용 하 여 기본 키를 설정/만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d4799-107">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="d4799-108">표기 규칙</span><span class="sxs-lookup"><span data-stu-id="d4799-108">Conventions</span></span>

<span data-ttu-id="d4799-109">기본적으로 `Id` 또는 `<type name>Id` 라는 속성은 엔터티의 키로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d4799-109">By default, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyId&highlight=3)]

> [!NOTE]
> <span data-ttu-id="d4799-110">[소유 하는 엔터티 형식은](xref:core/modeling/owned-entities) 다른 규칙을 사용 하 여 키를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="d4799-110">[Owned entity types](xref:core/modeling/owned-entities) use different rules to define keys.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d4799-111">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="d4799-111">Data Annotations</span></span>

<span data-ttu-id="d4799-112">데이터 주석을 사용 하 여 단일 속성을 엔터티의 키로 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d4799-112">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="d4799-113">흐름 API</span><span class="sxs-lookup"><span data-stu-id="d4799-113">Fluent API</span></span>

<span data-ttu-id="d4799-114">흐름 API를 사용 하 여 단일 속성을 엔터티의 키로 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d4799-114">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="d4799-115">흐름 API를 사용 하 여 여러 속성을 엔터티의 키 (복합 키 라고 함)로 구성할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d4799-115">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="d4799-116">복합 키는 흐름 API를 사용 하 여 구성할 수 있습니다. 규칙은 복합 키를 설정 하지 않으며 데이터 주석을 사용 하 여 구성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="d4799-116">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]

## <a name="key-types-and-values"></a><span data-ttu-id="d4799-117">키 유형 및 값</span><span class="sxs-lookup"><span data-stu-id="d4799-117">Key types and values</span></span>

<span data-ttu-id="d4799-118">EF Core는 `string`, `Guid`, `byte[]` 등을 비롯 한 기본 키로 기본 형식의 속성을 사용 하도록 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="d4799-118">EF Core supports using properties of any primitive type as the primary key, including `string`, `Guid`, `byte[]` and others.</span></span> <span data-ttu-id="d4799-119">그러나 일부 데이터베이스는이를 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="d4799-119">But not all databases support them.</span></span> <span data-ttu-id="d4799-120">경우에 따라 키 값을 지원 되는 형식으로 자동으로 변환할 수 있습니다. 그렇지 않으면 변환을 [수동으로 지정](xref:core/modeling/value-conversions)해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d4799-120">In some cases the key values can be converted to a supported type automatically, otherwise the conversion should be [specified manually](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="d4799-121">컨텍스트에 새 엔터티를 추가 하는 경우 키 속성은 항상 기본값이 아닌 값을 가져야 하지만 일부 유형은 [데이터베이스에 의해 생성](xref:core/modeling/generated-properties)됩니다.</span><span class="sxs-lookup"><span data-stu-id="d4799-121">Key properties must always have a non-default value when adding a new entity to the context, but some types will be [generated by the database](xref:core/modeling/generated-properties).</span></span> <span data-ttu-id="d4799-122">이 경우 EF는 추적을 위해 엔터티가 추가 될 때 임시 값을 생성 하려고 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="d4799-122">In that case EF will try to generate a temporary value when the entity is added for tracking purposes.</span></span> <span data-ttu-id="d4799-123">[SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) 가 호출 되 면 임시 값이 데이터베이스에 의해 생성 된 값으로 바뀝니다.</span><span class="sxs-lookup"><span data-stu-id="d4799-123">After [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) is called the temporary value will be replaced by the value generated by the database.</span></span>

> [!Important]
> <span data-ttu-id="d4799-124">키 속성의 값이 데이터베이스에 의해 생성 되 고 엔터티가 추가 될 때 기본값이 아닌 값이 지정 된 경우 EF는 엔터티가 데이터베이스에 이미 있는 것으로 가정 하 고 새 엔터티를 삽입 하는 대신 업데이트 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="d4799-124">If a key property has value generated by the database and a non-default value is specified when an entity is added then EF will assume that the entity already exists in the database and will try to update it instead of inserting a new one.</span></span> <span data-ttu-id="d4799-125">이 값을 해제 하지 않으려면 [생성 된 속성에 대해 명시적 값을 지정 하는 방법](../saving/explicit-values-generated-properties.md)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="d4799-125">To avoid this turn off value generation or see [how to specify explicit values for generated properties](../saving/explicit-values-generated-properties.md).</span></span>