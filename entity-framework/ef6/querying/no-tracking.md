---
title: 추적 안 함 쿼리-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: 44d58e14a2550bd08a8edd68b467237f6f5b5978
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414478"
---
# <a name="no-tracking-queries"></a>비 추적 쿼리
쿼리에서 엔터티를 다시 가져올 수 있지만 컨텍스트에서 엔터티를 추적할 수 있는 경우가 있습니다. 이로 인해 읽기 전용 시나리오에서 많은 수의 엔터티를 쿼리 하는 경우 성능이 향상 될 수 있습니다. 이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

새 확장 메서드 AsNoTracking를 사용 하면 모든 쿼리를 이런 방식으로 실행할 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs without tracking them
    var blogs1 = context.Blogs.AsNoTracking();

    // Query for some blogs without tracking them
    var blogs2 = context.Blogs
                        .Where(b => b.Name.Contains(".NET"))
                        .AsNoTracking()
                        .ToList();
}
```  
