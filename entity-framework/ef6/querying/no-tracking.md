---
title: EF6-비 추적 쿼리
author: divega
ms.date: 2016-10-23
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: dba4127ade9481b40d4fd3c4323532ddfedf6980
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994242"
---
# <a name="no-tracking-queries"></a>비 추적 쿼리
경우에 따라 다음 쿼리에서 엔터티를 다시 가져오기 있지만 해당 엔터티가 컨텍스트에서 추적는 것이 좋습니다. 이 값은 다 수의 읽기 전용 시나리오에서 엔터티를 쿼리할 때 성능이 향상 될 수 있습니다. 이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

새 확장 메서드를 AsNoTracking이 방식으로 실행 되도록 쿼리를 허용 합니다. 예를 들어:  

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
