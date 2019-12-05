---
title: 상속-EF Core
description: Entity Framework Core를 사용 하 여 엔터티 형식 상속을 구성 하는 방법
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 4d43a432174c92ab7f3f9d78a234aefb0a4a17e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824682"
---
# <a name="inheritance"></a><span data-ttu-id="c5065-103">상속</span><span class="sxs-lookup"><span data-stu-id="c5065-103">Inheritance</span></span>

<span data-ttu-id="c5065-104">EF 모델의 상속은 데이터베이스에서 엔터티 클래스의 상속이 표시 되는 방식을 제어 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c5065-104">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="c5065-105">표기 규칙</span><span class="sxs-lookup"><span data-stu-id="c5065-105">Conventions</span></span>

<span data-ttu-id="c5065-106">기본적으로 데이터베이스에서 상속이 표시 되는 방식을 결정 하는 것은 데이터베이스 공급자에 게 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c5065-106">By default, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="c5065-107">관계형 데이터베이스 공급자를 사용 하 여이를 처리 하는 방법은 [상속 (관계형 데이터베이스)](relational/inheritance.md) 을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c5065-107">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="c5065-108">EF는 둘 이상의 상속 된 형식이 모델에 명시적으로 포함 된 경우에만 상속을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c5065-108">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="c5065-109">EF는 모델에 포함 되지 않은 기본 또는 파생 형식을 검색 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c5065-109">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="c5065-110">상속 계층 구조의 각 형식에 대해 *Dbset\<* 를 노출 하 여 모델에 형식을 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c5065-110">You can include types in the model by exposing a *DbSet\<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="c5065-111">계층 구조에 있는 하나 이상의 엔터티에 대해 *Dbset\<* 를 노출 하지 않으려는 경우 흐름 API를 사용 하 여 모델에 포함 되도록 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c5065-111">If you don't want to expose a *DbSet\<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="c5065-112">규칙을 사용 하지 않는 경우 `HasBaseType`를 사용 하 여 명시적으로 기본 형식을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c5065-112">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="c5065-113">`.HasBaseType((Type)null)`를 사용 하 여 계층에서 엔터티 형식을 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c5065-113">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c5065-114">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="c5065-114">Data Annotations</span></span>

<span data-ttu-id="c5065-115">데이터 주석은 상속을 구성 하는 데 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c5065-115">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c5065-116">흐름 API</span><span class="sxs-lookup"><span data-stu-id="c5065-116">Fluent API</span></span>

<span data-ttu-id="c5065-117">상속에 대 한 흐름 API는 사용 중인 데이터베이스 공급자에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="c5065-117">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="c5065-118">관계형 데이터베이스 공급자에 대해 수행할 수 있는 구성에 대 한 [상속 (관계형 데이터베이스)](relational/inheritance.md) 을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c5065-118">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
