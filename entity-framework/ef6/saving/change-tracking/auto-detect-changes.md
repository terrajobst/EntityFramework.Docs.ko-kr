---
title: 자동 검색 변경 내용-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
ms.openlocfilehash: 9af85fd7ca48a14432a1f33c59079fc438ef8810
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414382"
---
# <a name="automatic-detect-changes"></a><span data-ttu-id="d0ed7-102">자동으로 변경 내용 검색</span><span class="sxs-lookup"><span data-stu-id="d0ed7-102">Automatic detect changes</span></span>
<span data-ttu-id="d0ed7-103">대부분의 POCO 엔터티를 사용 하는 경우 엔터티 변경 (그리고 데이터베이스에 전송 해야 하는 업데이트)에 대 한 결정이 변경 내용 검색 알고리즘에 의해 처리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d0ed7-103">When using most POCO entities the determination of how an entity has changed (and therefore which updates need to be sent to the database) is handled by the Detect Changes algorithm.</span></span> <span data-ttu-id="d0ed7-104">엔터티에 대 한 현재 속성 값과 엔터티가 쿼리 또는 연결 될 때 스냅숏에 저장 된 원래 속성 값 간의 차이를 검색 하 여 변경 내용 검색 작업을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="d0ed7-104">Detect Changes works by detecting the differences between the current property values of the entity and the original property values that are stored in a snapshot when the entity was queried or attached.</span></span> <span data-ttu-id="d0ed7-105">이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="d0ed7-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="d0ed7-106">기본적으로 Entity Framework는 다음 메서드를 호출할 때 변경 내용을 자동으로 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="d0ed7-106">By default, Entity Framework performs Detect Changes automatically when the following methods are called:</span></span>  

- <span data-ttu-id="d0ed7-107">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="d0ed7-107">DbSet.Find</span></span>  
- <span data-ttu-id="d0ed7-108">DbSet. Local</span><span class="sxs-lookup"><span data-stu-id="d0ed7-108">DbSet.Local</span></span>  
- <span data-ttu-id="d0ed7-109">DbSet. Add</span><span class="sxs-lookup"><span data-stu-id="d0ed7-109">DbSet.Add</span></span>  
- <span data-ttu-id="d0ed7-110">DbSet</span><span class="sxs-lookup"><span data-stu-id="d0ed7-110">DbSet.AddRange</span></span>
- <span data-ttu-id="d0ed7-111">DbSet. Remove</span><span class="sxs-lookup"><span data-stu-id="d0ed7-111">DbSet.Remove</span></span>  
- <span data-ttu-id="d0ed7-112">DbSet</span><span class="sxs-lookup"><span data-stu-id="d0ed7-112">DbSet.RemoveRange</span></span>
- <span data-ttu-id="d0ed7-113">DbSet. Attach</span><span class="sxs-lookup"><span data-stu-id="d0ed7-113">DbSet.Attach</span></span>  
- <span data-ttu-id="d0ed7-114">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="d0ed7-114">DbContext.SaveChanges</span></span>  
- <span data-ttu-id="d0ed7-115">DbContext 오류</span><span class="sxs-lookup"><span data-stu-id="d0ed7-115">DbContext.GetValidationErrors</span></span>  
- <span data-ttu-id="d0ed7-116">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="d0ed7-116">DbContext.Entry</span></span>  
- <span data-ttu-id="d0ed7-117">DbChangeTracker</span><span class="sxs-lookup"><span data-stu-id="d0ed7-117">DbChangeTracker.Entries</span></span>  

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="d0ed7-118">변경 내용 자동 검색 사용 안 함</span><span class="sxs-lookup"><span data-stu-id="d0ed7-118">Disabling automatic detection of changes</span></span>  

<span data-ttu-id="d0ed7-119">컨텍스트에서 많은 엔터티를 추적 하 고 루프에서 이러한 메서드 중 하나를 여러 번 호출 하는 경우 루프의 지속 시간에 대 한 변경 감지 기능을 해제 하 여 성능을 크게 향상 시킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d0ed7-119">If you are tracking a lot of entities in your context and you call one of these methods many times in a loop, then you may get significant performance improvements by turning off detection of changes for the duration of the loop.</span></span> <span data-ttu-id="d0ed7-120">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="d0ed7-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    try
    {
        context.Configuration.AutoDetectChangesEnabled = false;

        // Make many calls in a loop
        foreach (var blog in aLotOfBlogs)
        {
            context.Blogs.Add(blog);
        }
    }
    finally
    {
        context.Configuration.AutoDetectChangesEnabled = true;
    }
}
```  

<span data-ttu-id="d0ed7-121">루프 후에 변경 내용 검색을 다시 사용 하도록 설정 해야 합니다. 즉, 루프의 코드가 예외를 throw 하는 경우에도 try/finally를 사용 하 여 항상 다시 사용 하도록 설정 했습니다.</span><span class="sxs-lookup"><span data-stu-id="d0ed7-121">Don’t forget to re-enable detection of changes after the loop — We've used a try/finally to ensure it is always re-enabled even if code in the loop throws an exception.</span></span>  

<span data-ttu-id="d0ed7-122">을 사용 하지 않도록 설정 하 고 다시 활성화 하는 대신 변경 내용을 항상 해제 하 고 컨텍스트를 호출 하는 것이 좋습니다. ChangeTracker DetectChanges를 명시적으로 사용 하거나 변경 내용 추적 프록시 노력를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="d0ed7-122">An alternative to disabling and re-enabling is to leave automatic detection of changes turned off at all times and either call context.ChangeTracker.DetectChanges explicitly or use change tracking proxies diligently.</span></span> <span data-ttu-id="d0ed7-123">이러한 두 옵션은 모두 고급 이며 신중 하 게 사용할 수 있도록 응용 프로그램에 미묘한 버그를 쉽게 도입할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d0ed7-123">Both of these options are advanced and can easily introduce subtle bugs into your application so use them with care.</span></span>  

<span data-ttu-id="d0ed7-124">컨텍스트에서 많은 개체를 추가 하거나 제거 해야 하는 경우에는 Dbset 및 Dbset를 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="d0ed7-124">If you need to add or remove many objects from a context, consider using DbSet.AddRange and DbSet.RemoveRange.</span></span> <span data-ttu-id="d0ed7-125">이 메서드는 추가 또는 제거 작업이 완료 된 후에만 변경 내용을 자동으로 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="d0ed7-125">This methods automatically detect changes only once after the add or remove operations are completed.</span></span> 
