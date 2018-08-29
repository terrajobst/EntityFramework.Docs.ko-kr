---
title: 동일한 DbContext 형식-EF Core를 사용 하 여 여러 모델 간 전환
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 1d87efb668c12a2862583fba16a6c444b3cda9de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994988"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="2e27f-102">동일한 DbContext 형식 사용 하 여 여러 모델 간 전환</span><span class="sxs-lookup"><span data-stu-id="2e27f-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="2e27f-103">작성 된 모델 `OnModelCreating` 모델 빌드되는 방식을 변경 하려면 컨텍스트에서 속성을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2e27f-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="2e27f-104">예를 들어 특정 속성을 제외 하려면 사용 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2e27f-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="2e27f-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="2e27f-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="2e27f-106">그러나 추가로 변경 하지 않고 위에서 수행 하 려 한 경우 얻게 동일한 모델의 모든 값에 대 한 새 컨텍스트를 만들 때마다 `IgnoreIntProperty`합니다.</span><span class="sxs-lookup"><span data-stu-id="2e27f-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="2e27f-107">캐싱 메커니즘만 호출 하 여 성능을 개선 하기 위해 사용 하 여 EF 모델에 의해 발생 `OnModelCreating` 번 및 모델을 캐시 합니다.</span><span class="sxs-lookup"><span data-stu-id="2e27f-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="2e27f-108">기본적으로 EF는 모든 지정 된 컨텍스트에 대 한 형식 모델은 동일 하 게 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="2e27f-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="2e27f-109">이를 위해 기본 구현의 `IModelCacheKeyFactory` 컨텍스트 형식에만 포함 하는 키를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="2e27f-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="2e27f-110">교체 해야이 변경 된 `IModelCacheKeyFactory` 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="2e27f-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="2e27f-111">새 구현을 사용 하 여 다른 모델 키를 비교할 수 있는 개체를 반환 해야 하는 경우는 `Equals` 모델에 영향을 주는 모든 변수를 고려 하는 메서드:</span><span class="sxs-lookup"><span data-stu-id="2e27f-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
