---
title: DbContext 유형이 같은 여러 모델 사이에서 교대로 반복 EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: a160f0d382ee2a3ac7130ce1ac98eb24b3f79394
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413914"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>DbContext 유형이 같은 여러 모델 간에 교대로 반복

`OnModelCreating`에서 작성 된 모델은 컨텍스트의 속성을 사용 하 여 모델을 빌드하는 방법을 변경할 수 있습니다. 예를 들어 일부 속성에 따라 엔터티를 다르게 구성 한다고 가정 합니다.

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

불행 하 게도 EF는 모델을 작성 하 고 한 번만 `OnModelCreating` 실행 하므로 성능상의 이유로 결과를 캐시 하기 때문에이 코드는 그대로 작동 하지 않습니다. 그러나 모델 캐싱 메커니즘에 연결 하 여 여러 모델을 생성 하는 속성에 대해 EF를 인식할 수 있습니다.

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory

EF는 `IModelCacheKeyFactory`을 사용 하 여 모델에 대 한 캐시 키를 생성 합니다. 기본적으로 EF는 지정 된 컨텍스트 형식에 대해 모델이 동일할 것으로 가정 하므로이 서비스의 기본 구현은 컨텍스트 형식을 포함 하는 키를 반환 합니다. 동일한 컨텍스트 형식에서 다른 모델을 생성 하려면 `IModelCacheKeyFactory` 서비스를 올바른 구현으로 바꾸어야 합니다. 생성 된 키는 모델에 영향을 주는 모든 변수를 고려 하 여 `Equals` 메서드를 사용 하는 다른 모델 키와 비교 됩니다.

다음 구현에서는 모델 캐시 키를 생성할 때 `IgnoreIntProperty`을 고려 합니다.

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

마지막으로 컨텍스트의 `OnConfiguring`에 새 `IModelCacheKeyFactory`를 등록 합니다.

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

자세한 컨텍스트는 [전체 샘플 프로젝트](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) 를 참조 하세요.
