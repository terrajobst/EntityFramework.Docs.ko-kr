---
title: EF6-Load 메서드
author: divega
ms.date: 2016-10-23
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: f7e8410b8fb8b5c3e86c51cd61868604a7566d0c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996653"
---
# <a name="the-load-method"></a>로드 메서드
데이터베이스에서 즉시 해당 엔터티를 사용 하 여 아무 것도 수행 하지 않고 컨텍스트에 엔터티를 로드 하려는 여러 시나리오가 있습니다. 에 설명 된 대로이 좋은 예가 데이터 바인딩에 대 한 엔터티를 로드 [로컬 데이터](~/ef6/querying/local-data.md)입니다. 이 작업을 수행 하는 하나의 일반적인 방법은 LINQ 쿼리를 작성 한 다음 만든된 목록을 즉시 삭제 하는 데에이 ToList를 호출 하는 경우 Load 확장 메서드를 제외 하 고 완전히 목록 만들기를 방지 하기 ToList 처럼 작동 합니다.  

이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

부하를 사용 하 여 두 가지 예는 다음과 같습니다. 첫 번째 로드 되는 Windows Forms 데이터 바인딩 응용 프로그램에서 가져온 것에 설명 된 대로 로컬 컬렉션에 바인딩 전에 엔터티에 대 한 쿼리 [로컬 데이터](~/ef6/querying/local-data.md):  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

에 설명 된 대로 필터링 된 관련된 엔터티 컬렉션을 로드 하려면 로드를 사용 하는 두 번째 예제 [관련 엔터티 로드](~/ef6/querying/related-data.md):  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  
