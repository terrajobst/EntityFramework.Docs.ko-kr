---
title: EF6-Load 메서드
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: 3a0d11552b6bfd8b83f15c58c6cb9f945d9d4536
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490898"
---
# <a name="the-load-method"></a><span data-ttu-id="61da8-102">로드 메서드</span><span class="sxs-lookup"><span data-stu-id="61da8-102">The Load Method</span></span>
<span data-ttu-id="61da8-103">데이터베이스에서 즉시 해당 엔터티를 사용 하 여 아무 것도 수행 하지 않고 컨텍스트에 엔터티를 로드 하려는 여러 시나리오가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="61da8-103">There are several scenarios where you may want to load entities from the database into the context without immediately doing anything with those entities.</span></span> <span data-ttu-id="61da8-104">에 설명 된 대로이 좋은 예가 데이터 바인딩에 대 한 엔터티를 로드 [로컬 데이터](~/ef6/querying/local-data.md)입니다.</span><span class="sxs-lookup"><span data-stu-id="61da8-104">A good example of this is loading entities for data binding as described in [Local Data](~/ef6/querying/local-data.md).</span></span> <span data-ttu-id="61da8-105">이 작업을 수행 하는 하나의 일반적인 방법은 LINQ 쿼리를 작성 한 다음 만든된 목록을 즉시 삭제 하는 데에이 ToList를 호출 하는 경우</span><span class="sxs-lookup"><span data-stu-id="61da8-105">One common way to do this is to write a LINQ query and then call ToList on it, only to immediately discard the created list.</span></span> <span data-ttu-id="61da8-106">Load 확장 메서드를 제외 하 고 완전히 목록 만들기를 방지 하기 ToList 처럼 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="61da8-106">The Load extension method works just like ToList except that it avoids the creation of the list altogether.</span></span>  

<span data-ttu-id="61da8-107">이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="61da8-107">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="61da8-108">부하를 사용 하 여 두 가지 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="61da8-108">Here are two examples of using Load.</span></span> <span data-ttu-id="61da8-109">첫 번째 로드 되는 Windows Forms 데이터 바인딩 응용 프로그램에서 가져온 것에 설명 된 대로 로컬 컬렉션에 바인딩 전에 엔터티에 대 한 쿼리 [로컬 데이터](~/ef6/querying/local-data.md):</span><span class="sxs-lookup"><span data-stu-id="61da8-109">The first is taken from a Windows Forms data binding application where Load is used to query for entities before binding to the local collection, as described in [Local Data](~/ef6/querying/local-data.md):</span></span>  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

<span data-ttu-id="61da8-110">에 설명 된 대로 필터링 된 관련된 엔터티 컬렉션을 로드 하려면 로드를 사용 하는 두 번째 예제 [관련 엔터티 로드](~/ef6/querying/related-data.md):</span><span class="sxs-lookup"><span data-stu-id="61da8-110">The second example shows using Load to load a filtered collection of related entities, as described in [Loading Related Entities](~/ef6/querying/related-data.md):</span></span>  

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
