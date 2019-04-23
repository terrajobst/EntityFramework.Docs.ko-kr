---
title: 키 (기본)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 51d163b867085f42f415dbd7afa9e311ab1781a0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929838"
---
# <a name="keys-primary"></a><span data-ttu-id="10379-102">키 (기본)</span><span class="sxs-lookup"><span data-stu-id="10379-102">Keys (primary)</span></span>

<span data-ttu-id="10379-103">키를 각 엔터티 인스턴스에 대 한 기본 고유 식별자로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="10379-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="10379-104">개념에 매핑됩니다 관계형 데이터베이스를 사용 하는 경우는 *기본 키*합니다.</span><span class="sxs-lookup"><span data-stu-id="10379-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="10379-105">기본 키 없는 고유 식별자를 구성할 수도 있습니다 (참조 [대체 키](alternate-keys.md) 자세한).</span><span class="sxs-lookup"><span data-stu-id="10379-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="10379-106">다음 방법 중 하나를 기본 키를 설치/생성할 수입니다.</span><span class="sxs-lookup"><span data-stu-id="10379-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="10379-107">규칙</span><span class="sxs-lookup"><span data-stu-id="10379-107">Conventions</span></span>

<span data-ttu-id="10379-108">이라는 속성이 규칙에 따라 `Id` 또는 `<type name>Id` 엔터티 키로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="10379-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="10379-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="10379-109">Data Annotations</span></span>

<span data-ttu-id="10379-110">엔터티 키로 단일 속성을 구성 하려면 데이터 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="10379-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="10379-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="10379-111">Fluent API</span></span>

<span data-ttu-id="10379-112">엔터티 키로 단일 속성을 구성 하는 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="10379-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="10379-113">또한 (복합 키로 알려짐) 엔터티 키로 여러 속성을 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="10379-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="10379-114">복합 키만 구성할 수 있습니다 Fluent API를 사용 하 여-규칙에서는 복합 키를 설정 하지 않습니다 하 고 데이터 주석을 하나를 구성 하려면 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="10379-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=11,12)]
