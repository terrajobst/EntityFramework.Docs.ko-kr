---
title: EF6-비 추적 쿼리
author: divega
ms.date: 10/23/2016
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: 44d58e14a2550bd08a8edd68b467237f6f5b5978
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490126"
---
# <a name="no-tracking-queries"></a><span data-ttu-id="8b001-102">비 추적 쿼리</span><span class="sxs-lookup"><span data-stu-id="8b001-102">No-Tracking Queries</span></span>
<span data-ttu-id="8b001-103">경우에 따라 다음 쿼리에서 엔터티를 다시 가져오기 있지만 해당 엔터티가 컨텍스트에서 추적는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="8b001-103">Sometimes you may want to get entities back from a query but not have those entities be tracked by the context.</span></span> <span data-ttu-id="8b001-104">이 값은 다 수의 읽기 전용 시나리오에서 엔터티를 쿼리할 때 성능이 향상 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8b001-104">This may result in better performance when querying for large numbers of entities in read-only scenarios.</span></span> <span data-ttu-id="8b001-105">이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="8b001-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="8b001-106">새 확장 메서드를 AsNoTracking이 방식으로 실행 되도록 쿼리를 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="8b001-106">A new extension method AsNoTracking allows any query to be run in this way.</span></span> <span data-ttu-id="8b001-107">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="8b001-107">For example:</span></span>  

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
