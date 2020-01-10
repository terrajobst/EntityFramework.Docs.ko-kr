---
title: DbContext 유형이 같은 여러 모델 사이에서 교대로 반복 EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 156d5666cbd9352b274ddc70c99704ca62aeb1fd
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781133"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="b898b-102">DbContext 유형이 같은 여러 모델 간에 교대로 반복</span><span class="sxs-lookup"><span data-stu-id="b898b-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="b898b-103">`OnModelCreating`에서 작성 된 모델은 컨텍스트의 속성을 사용 하 여 모델을 빌드하는 방법을 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b898b-103">The model built in `OnModelCreating` can use a property on the context to change how the model is built.</span></span> <span data-ttu-id="b898b-104">예를 들어 일부 속성에 따라 엔터티를 다르게 구성 한다고 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="b898b-104">For example, suppose you wanted to configure an entity differently based on some property:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

<span data-ttu-id="b898b-105">불행 하 게도 EF는 모델을 작성 하 고 한 번만 `OnModelCreating` 실행 하므로 성능상의 이유로 결과를 캐시 하기 때문에이 코드는 그대로 작동 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b898b-105">Unfortunately, this code wouldn't work as-is, since EF builds the model and runs `OnModelCreating` only once, caching the result for performance reasons.</span></span> <span data-ttu-id="b898b-106">그러나 모델 캐싱 메커니즘에 연결 하 여 여러 모델을 생성 하는 속성에 대해 EF를 인식할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b898b-106">However, you can hook into the model caching mechanism to make EF aware of the property producing different models.</span></span>

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="b898b-107">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="b898b-107">IModelCacheKeyFactory</span></span>

<span data-ttu-id="b898b-108">EF는 `IModelCacheKeyFactory`을 사용 하 여 모델에 대 한 캐시 키를 생성 합니다. 기본적으로 EF는 지정 된 컨텍스트 형식에 대해 모델이 동일할 것으로 가정 하므로이 서비스의 기본 구현은 컨텍스트 형식을 포함 하는 키를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="b898b-108">EF uses the `IModelCacheKeyFactory` to generate cache keys for models; by default, EF assumes that for any given context type the model will be the same, so the default implementation of this service returns a key that just contains the context type.</span></span> <span data-ttu-id="b898b-109">동일한 컨텍스트 형식에서 다른 모델을 생성 하려면 `IModelCacheKeyFactory` 서비스를 올바른 구현으로 바꾸어야 합니다. 생성 된 키는 모델에 영향을 주는 모든 변수를 고려 하 여 `Equals` 메서드를 사용 하는 다른 모델 키와 비교 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b898b-109">To produce different models from the same context type, you need to replace the `IModelCacheKeyFactory` service with the correct  implementation; the generated key will be compared to other model keys using the `Equals` method, taking into account all the variables that affect the model:</span></span>

<span data-ttu-id="b898b-110">다음 구현에서는 모델 캐시 키를 생성할 때 `IgnoreIntProperty`을 고려 합니다.</span><span class="sxs-lookup"><span data-stu-id="b898b-110">The following implementation takes the `IgnoreIntProperty` into account when producing a model cache key:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

<span data-ttu-id="b898b-111">마지막으로 컨텍스트의 `OnConfiguring`에 새 `IModelCacheKeyFactory`를 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="b898b-111">Finally, register your new `IModelCacheKeyFactory` in your context's `OnConfiguring`:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

<span data-ttu-id="b898b-112">자세한 컨텍스트는 [전체 샘플 프로젝트](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) 를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="b898b-112">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) for more context.</span></span>
