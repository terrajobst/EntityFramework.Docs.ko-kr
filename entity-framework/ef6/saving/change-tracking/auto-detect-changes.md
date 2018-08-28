---
title: 자동 변경 내용-EF6를 검색 합니다.
author: divega
ms.date: 2016-10-23
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
ms.openlocfilehash: bca33e12674c47cc7e047e85b11746c8e39246b4
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998101"
---
# <a name="automatic-detect-changes"></a>자동 변경 내용 검색
대부분의 POCO 엔터티를 사용 하는 경우 엔터티의 어떻게 변경 되었는지 (및 따라서 데이터베이스를 전송할 수 있도록 해야 하는 업데이트) 결정 변경 감지 알고리즘을 통해 처리 됩니다. 엔터티의 현재 속성 값 및 엔터티 쿼리 또는 연결 된 경우 스냅숏에 저장 된 원래 속성 값 간의 차이 감지 하 여 변경 내용을 작동을 검색 합니다. 이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

기본적으로 Entity Framework 다음 메서드를 호출 하면 자동으로 변경 감지를 수행 합니다.  

- DbSet.Find  
- DbSet.Local  
- DbSet.Add  
- DbSet.AddRange
- DbSet.Remove  
- DbSet.RemoveRange
- DbSet.Attach  
- DbContext.SaveChanges  
- DbContext.GetValidationErrors  
- DbContext.Entry  
- DbChangeTracker.Entries  

## <a name="disabling-automatic-detection-of-changes"></a>변경의 자동 검색을 사용 하지 않도록 설정  

많은 엔터티가 컨텍스트에서 추적 하는 경우 루프에서 여러 번 다음이 방법 중 하나 호출 하면 상당한 성능 향상 루프의 기간에 대 한 변경 내용 검색을 해제 하 여 발생할 수 있습니다. 예를 들어:  

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

루프 후 변경 내용 검색을 다시 사용 하도록 설정-항상 다시 설정한 고 루프의 코드가 예외를 throw 하는 경우에 되도록 try/finally 사용 했습니다.  

사용 하지 않도록 설정 하는 대신 모든 시간 및 호출 컨텍스트 중 하나에서 해제 하는 변경의 자동 검색을 확보 하는 다시 사용 하도록 설정 하 고 있습니다. ChangeTracker.DetectChanges 명시적으로 사용 하 여 변경 내용 추적 프록시가 열심히 또는 합니다. 두이 옵션 모두 고급 및 응용 프로그램에 미묘한 버그가 발생 쉽게 주의 해 서 사용 합니다.  

추가 하거나 컨텍스트에서 많은 개체를 제거 해야 할 경우 DbSet.AddRange 및 DbSet.RemoveRange를 사용 하는 것이 좋습니다. 이 메서드 변경을 자동으로 감지 한 번만 추가 또는 제거 작업이 완료 되 면 합니다. 
