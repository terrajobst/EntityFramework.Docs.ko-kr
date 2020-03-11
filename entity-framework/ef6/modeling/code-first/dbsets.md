---
title: DbSets-EF6 정의
author: divega
ms.date: 10/23/2016
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: 045b22d2b9d26804948689dd7c9dd694baadda7e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415780"
---
# <a name="defining-dbsets"></a>DbSets 정의
Code First 워크플로를 사용 하 여 개발 하는 경우 데이터베이스와의 세션을 나타내는 파생 DbContext를 정의 하 고 모델의 각 형식에 대해 DbSet을 노출 합니다. 이 항목에서는 DbSet 속성을 정의 하는 다양 한 방법에 대해 설명 합니다.  

## <a name="dbcontext-with-dbset-properties"></a>DbSet 속성을 사용 하는 DbContext  

Code First 예제에 표시 되는 일반적인 경우는 모델의 엔터티 형식에 대 한 공용 자동 DbSet 속성이 있는 DbContext를 사용 하는 것입니다. 예를 들면 다음과 같습니다.  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

Code First 모드에서 사용 하는 경우 블로그 및 게시물을 엔터티 형식으로 구성 하 고 이러한 형식에서 연결할 수 있는 다른 형식을 구성 합니다. 또한 DbContext는 이러한 각 속성에 대해 setter를 자동으로 호출 하 여 적절 한 DbSet의 인스턴스를 설정 합니다.  

## <a name="dbcontext-with-idbset-properties"></a>IDbSet 속성이 있는 DbContext  

Mock 또는 fakes를 만드는 경우와 같이 인터페이스를 사용 하 여 집합 속성을 선언 하는 것이 더 유용한 상황이 있습니다. 이러한 경우 DbSet 대신 IDbSet 인터페이스를 사용할 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

이 컨텍스트는 해당 set 속성에 대해 DbSet 클래스를 사용 하는 컨텍스트와 정확히 동일한 방식으로 작동 합니다.  

## <a name="dbcontext-with-read-only-set-properties"></a>읽기 전용 집합 속성을 사용 하는 DbContext  

DbSet 또는 IDbSet 속성에 대 한 공용 setter를 노출 하지 않으려면 읽기 전용 속성을 만들고 직접 설정 인스턴스를 만들 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs
    {
        get { return Set<Blog>(); }
    }

    public DbSet<Post> Posts
    {
        get { return Set<Post>(); }
    }
}
```  

DbContext는 Set 메서드에서 반환 된 DbSet 인스턴스를 캐시 하므로 이러한 각 속성은 호출 될 때마다 동일한 인스턴스를 반환 합니다.  

Code First에 대 한 엔터티 형식의 검색은 public getter 및 setter를 사용 하는 속성의 경우와 동일한 방식으로 작동 합니다.  
