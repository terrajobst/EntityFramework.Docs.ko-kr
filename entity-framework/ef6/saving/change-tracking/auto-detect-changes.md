---
title: 자동 변경 내용-EF6를 검색 합니다.
author: divega
ms.date: 10/23/2016
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
ms.openlocfilehash: 9af85fd7ca48a14432a1f33c59079fc438ef8810
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490991"
---
# <a name="automatic-detect-changes"></a><span data-ttu-id="f9585-102">자동 변경 내용 검색</span><span class="sxs-lookup"><span data-stu-id="f9585-102">Automatic detect changes</span></span>
<span data-ttu-id="f9585-103">대부분의 POCO 엔터티를 사용 하는 경우 엔터티의 어떻게 변경 되었는지 (및 따라서 데이터베이스를 전송할 수 있도록 해야 하는 업데이트) 결정 변경 감지 알고리즘을 통해 처리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f9585-103">When using most POCO entities the determination of how an entity has changed (and therefore which updates need to be sent to the database) is handled by the Detect Changes algorithm.</span></span> <span data-ttu-id="f9585-104">엔터티의 현재 속성 값 및 엔터티 쿼리 또는 연결 된 경우 스냅숏에 저장 된 원래 속성 값 간의 차이 감지 하 여 변경 내용을 작동을 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="f9585-104">Detect Changes works by detecting the differences between the current property values of the entity and the original property values that are stored in a snapshot when the entity was queried or attached.</span></span> <span data-ttu-id="f9585-105">이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="f9585-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="f9585-106">기본적으로 Entity Framework 다음 메서드를 호출 하면 자동으로 변경 감지를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="f9585-106">By default, Entity Framework performs Detect Changes automatically when the following methods are called:</span></span>  

- <span data-ttu-id="f9585-107">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="f9585-107">DbSet.Find</span></span>  
- <span data-ttu-id="f9585-108">DbSet.Local</span><span class="sxs-lookup"><span data-stu-id="f9585-108">DbSet.Local</span></span>  
- <span data-ttu-id="f9585-109">DbSet.Add</span><span class="sxs-lookup"><span data-stu-id="f9585-109">DbSet.Add</span></span>  
- <span data-ttu-id="f9585-110">DbSet.AddRange</span><span class="sxs-lookup"><span data-stu-id="f9585-110">DbSet.AddRange</span></span>
- <span data-ttu-id="f9585-111">DbSet.Remove</span><span class="sxs-lookup"><span data-stu-id="f9585-111">DbSet.Remove</span></span>  
- <span data-ttu-id="f9585-112">DbSet.RemoveRange</span><span class="sxs-lookup"><span data-stu-id="f9585-112">DbSet.RemoveRange</span></span>
- <span data-ttu-id="f9585-113">DbSet.Attach</span><span class="sxs-lookup"><span data-stu-id="f9585-113">DbSet.Attach</span></span>  
- <span data-ttu-id="f9585-114">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="f9585-114">DbContext.SaveChanges</span></span>  
- <span data-ttu-id="f9585-115">DbContext.GetValidationErrors</span><span class="sxs-lookup"><span data-stu-id="f9585-115">DbContext.GetValidationErrors</span></span>  
- <span data-ttu-id="f9585-116">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="f9585-116">DbContext.Entry</span></span>  
- <span data-ttu-id="f9585-117">DbChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="f9585-117">DbChangeTracker.Entries</span></span>  

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="f9585-118">변경의 자동 검색을 사용 하지 않도록 설정</span><span class="sxs-lookup"><span data-stu-id="f9585-118">Disabling automatic detection of changes</span></span>  

<span data-ttu-id="f9585-119">많은 엔터티가 컨텍스트에서 추적 하는 경우 루프에서 여러 번 다음이 방법 중 하나 호출 하면 상당한 성능 향상 루프의 기간에 대 한 변경 내용 검색을 해제 하 여 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f9585-119">If you are tracking a lot of entities in your context and you call one of these methods many times in a loop, then you may get significant performance improvements by turning off detection of changes for the duration of the loop.</span></span> <span data-ttu-id="f9585-120">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="f9585-120">For example:</span></span>  

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

<span data-ttu-id="f9585-121">루프 후 변경 내용 검색을 다시 사용 하도록 설정-항상 다시 설정한 고 루프의 코드가 예외를 throw 하는 경우에 되도록 try/finally 사용 했습니다.</span><span class="sxs-lookup"><span data-stu-id="f9585-121">Don’t forget to re-enable detection of changes after the loop — We've used a try/finally to ensure it is always re-enabled even if code in the loop throws an exception.</span></span>  

<span data-ttu-id="f9585-122">사용 하지 않도록 설정 하는 대신 모든 시간 및 호출 컨텍스트 중 하나에서 해제 하는 변경의 자동 검색을 확보 하는 다시 사용 하도록 설정 하 고 있습니다. ChangeTracker.DetectChanges 명시적으로 사용 하 여 변경 내용 추적 프록시가 열심히 또는 합니다.</span><span class="sxs-lookup"><span data-stu-id="f9585-122">An alternative to disabling and re-enabling is to leave automatic detection of changes turned off at all times and either call context.ChangeTracker.DetectChanges explicitly or use change tracking proxies diligently.</span></span> <span data-ttu-id="f9585-123">두이 옵션 모두 고급 및 응용 프로그램에 미묘한 버그가 발생 쉽게 주의 해 서 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="f9585-123">Both of these options are advanced and can easily introduce subtle bugs into your application so use them with care.</span></span>  

<span data-ttu-id="f9585-124">추가 하거나 컨텍스트에서 많은 개체를 제거 해야 할 경우 DbSet.AddRange 및 DbSet.RemoveRange를 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="f9585-124">If you need to add or remove many objects from a context, consider using DbSet.AddRange and DbSet.RemoveRange.</span></span> <span data-ttu-id="f9585-125">이 메서드 변경을 자동으로 감지 한 번만 추가 또는 제거 작업이 완료 되 면 합니다.</span><span class="sxs-lookup"><span data-stu-id="f9585-125">This methods automatically detect changes only once after the add or remove operations are completed.</span></span> 
