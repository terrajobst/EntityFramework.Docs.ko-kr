---
title: Load 메서드-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: bcea8ab2477f44281cd5de824457a72a84ccc766
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414496"
---
# <a name="the-load-method"></a>로드 메서드
이러한 엔터티를 사용 하 여 즉시 작업을 수행 하지 않고 데이터베이스에서 컨텍스트로 엔터티를 로드 하는 몇 가지 시나리오가 있습니다. 이에 대 한 좋은 예는 [로컬 데이터](~/ef6/querying/local-data.md)에 설명 된 대로 데이터 바인딩의 엔터티를 로드 하는 것입니다. 이 작업을 수행 하는 일반적인 방법은 LINQ 쿼리를 작성 한 다음 해당 쿼리에서 ToList를 호출 하 여 생성 된 목록을 즉시 삭제 하는 것입니다. 로드 확장 메서드는 목록 생성을 방지 하는 것을 제외 하 고는 ToList와 동일 하 게 작동 합니다.  

이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

다음은 Load를 사용 하는 두 가지 예제입니다. 첫 번째는 [로컬 데이터](~/ef6/querying/local-data.md)의 설명에 따라 로컬 컬렉션에 바인딩하기 전에 부하를 사용 하 여 엔터티를 쿼리 하는 Windows Forms 데이터 바인딩 응용 프로그램에서 가져옵니다.  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

두 번째 예제에서는 [관련 엔터티 로드](~/ef6/querying/related-data.md)에서 설명한 대로 load를 사용 하 여 관련 엔터티의 필터링 된 컬렉션을 로드 하는 방법을 보여 줍니다.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework"))
        .Load();
}
```  
