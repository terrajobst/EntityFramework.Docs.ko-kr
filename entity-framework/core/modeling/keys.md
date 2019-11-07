---
title: 키 (기본)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 66c64c389294e8e109a614a2bea8311932660dea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655942"
---
# <a name="keys-primary"></a><span data-ttu-id="181a0-102">키(기본)</span><span class="sxs-lookup"><span data-stu-id="181a0-102">Keys (primary)</span></span>

<span data-ttu-id="181a0-103">키는 각 엔터티 인스턴스에 대 한 기본 고유 식별자 역할을 합니다.</span><span class="sxs-lookup"><span data-stu-id="181a0-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="181a0-104">관계형 데이터베이스를 사용 하는 경우 *기본 키*의 개념에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="181a0-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="181a0-105">기본 키가 아닌 고유 식별자를 구성할 수도 있습니다 (자세한 내용은 [대체 키](alternate-keys.md) 참조).</span><span class="sxs-lookup"><span data-stu-id="181a0-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<span data-ttu-id="181a0-106">다음 방법 중 하나를 사용 하 여 기본 키를 설정/만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="181a0-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="181a0-107">규칙</span><span class="sxs-lookup"><span data-stu-id="181a0-107">Conventions</span></span>

<span data-ttu-id="181a0-108">규칙에 따라 `Id` 또는 `<type name>Id` 라는 속성이 엔터티의 키로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="181a0-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyIdhighlight=3)]

## <a name="data-annotations"></a><span data-ttu-id="181a0-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="181a0-109">Data Annotations</span></span>

<span data-ttu-id="181a0-110">데이터 주석을 사용 하 여 단일 속성을 엔터티의 키로 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="181a0-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="181a0-111">흐름 API</span><span class="sxs-lookup"><span data-stu-id="181a0-111">Fluent API</span></span>

<span data-ttu-id="181a0-112">흐름 API를 사용 하 여 단일 속성을 엔터티의 키로 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="181a0-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="181a0-113">흐름 API를 사용 하 여 여러 속성을 엔터티의 키 (복합 키 라고 함)로 구성할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="181a0-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="181a0-114">복합 키는 흐름 API를 사용 하 여 구성할 수 있습니다. 규칙은 복합 키를 설정 하지 않으며 데이터 주석을 사용 하 여 구성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="181a0-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
