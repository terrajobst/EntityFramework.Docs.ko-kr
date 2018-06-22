---
title: 상속-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
ms.technology: entity-framework-core
uid: core/modeling/inheritance
ms.openlocfilehash: f0394bc55dfbfea8277b1ddf898361165dd1fe51
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052783"
---
# <a name="inheritance"></a><span data-ttu-id="75375-102">상속</span><span class="sxs-lookup"><span data-stu-id="75375-102">Inheritance</span></span>

<span data-ttu-id="75375-103">상속 EF 모델에는 데이터베이스에서 엔터티 클래스에서 상속 표현 방법을 제어 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="75375-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="75375-104">규칙</span><span class="sxs-lookup"><span data-stu-id="75375-104">Conventions</span></span>

<span data-ttu-id="75375-105">일반적으로 것은 데이터베이스에 상속 표시 되는 방법을 확인 하려면 데이터베이스 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="75375-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="75375-106">참조 [상속 (관계형 데이터베이스)](relational/inheritance.md) 관계형 데이터베이스 공급자와 처리 방법을이 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="75375-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="75375-107">두 개 이상의 상속 된 형식이 모델에 명시적으로 포함 된 경우 EF 상속을 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="75375-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="75375-108">EF는 모델에 포함 되지 않은 기본 또는 파생 된 형식에 대 한 검색 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="75375-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="75375-109">노출 하 여 모델에서 유형을 포함할 수 있습니다는 *DbSet<TEntity>*  상속 계층 구조에서 각 형식에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="75375-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="75375-110">노출 하지 않으려는 경우는 *DbSet<TEntity>*  계층 구조에서 하나 이상의 엔터티에 대 한 모델에 포함 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="75375-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="75375-111">규칙을 사용 하지 않는 경우 기본 형식을 사용 하 여 명시적으로 지정할 수 있습니다 및 `HasBaseType`합니다.</span><span class="sxs-lookup"><span data-stu-id="75375-111">And if you don't rely on conventions you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="75375-112">사용할 수 있습니다 `.HasBaseType((Type)null)` 엔터티 형식 계층에서 제거 하려면.</span><span class="sxs-lookup"><span data-stu-id="75375-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="75375-113">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="75375-113">Data Annotations</span></span>

<span data-ttu-id="75375-114">상속을 구성 하 데이터 주석을 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="75375-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="75375-115">Fluent API</span><span class="sxs-lookup"><span data-stu-id="75375-115">Fluent API</span></span>

<span data-ttu-id="75375-116">상속에 대 한 Fluent API 사용 하는 데이터베이스 공급자에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="75375-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="75375-117">참조 [상속 (관계형 데이터베이스)](relational/inheritance.md) 구성 관계형 데이터베이스 공급자에 대해 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="75375-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
