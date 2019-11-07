---
title: DbContext 유형이 같은 여러 모델 사이에서 교대로 반복 EF Core
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 034076b1595894e80b98467354f6c9f139bd7426
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655719"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="4984e-102">DbContext 유형이 같은 여러 모델 간에 교대로 반복</span><span class="sxs-lookup"><span data-stu-id="4984e-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="4984e-103">`OnModelCreating`에서 작성 된 모델은 컨텍스트의 속성을 사용 하 여 모델을 빌드하는 방법을 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4984e-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="4984e-104">예를 들어 특정 속성을 제외 하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4984e-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="4984e-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="4984e-105">IModelCacheKeyFactory</span></span>

<span data-ttu-id="4984e-106">그러나 추가 변경 없이 위의 작업을 수행 하려고 하면 `IgnoreIntProperty`값에 대해 새 컨텍스트가 만들어질 때마다 동일한 모델을 얻게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4984e-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="4984e-107">이는 EF가 `OnModelCreating`를 한 번만 호출 하 고 모델을 캐시 하 여 성능을 향상 시키기 위해 사용 하는 모델 캐싱 메커니즘에 의해 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="4984e-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="4984e-108">기본적으로 EF는 지정 된 컨텍스트 형식에 대해 모델이 동일 하다 고 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="4984e-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="4984e-109">이를 위해 `IModelCacheKeyFactory`의 기본 구현에서는 컨텍스트 형식만 포함 하는 키를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="4984e-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="4984e-110">이를 변경 하려면 `IModelCacheKeyFactory` 서비스를 대체 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4984e-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="4984e-111">새 구현에서는 모델에 영향을 주는 모든 변수를 고려 하는 `Equals` 메서드를 사용 하 여 다른 모델 키와 비교할 수 있는 개체를 반환 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4984e-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
