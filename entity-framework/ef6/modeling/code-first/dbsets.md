---
title: EF6 Dbset-정의
author: divega
ms.date: 10/23/2016
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: 045b22d2b9d26804948689dd7c9dd694baadda7e
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489000"
---
# <a name="defining-dbsets"></a>Dbset 정의
Code First 워크플로 사용 하 여 개발 하는 경우 데이터베이스를 사용 하 여 세션을 나타내며 모델의 각 형식에 대 한 DbSet을 노출 하는 파생 된 DbContext 정의 합니다. 이 항목에서는 DbSet 속성을 정의할 수는 다양 한 방법을 설명 합니다.  

## <a name="dbcontext-with-dbset-properties"></a>DbSet 속성을 사용 하 여 DbContext  

첫 번째 코드 예제에 나와 있는 일반적인 사례는 모델의 엔터티 형식에 대 한 공용 자동 DbSet 속성을 사용 하 여 DbContext를 것입니다. 예를 들어:  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

Code First 모드에서 사용 하는 경우 여기에서 연결할 수 있는 다른 형식 구성 뿐만 아니라 엔터티 형식으로 블로그 및 게시물을 구성 합니다이 있습니다. 또한 DbContext 각 인스턴스의 적절 한 DbSet 설정 하려면 이러한 속성에 대 한 setter를 자동으로 호출 됩니다.  

## <a name="dbcontext-with-idbset-properties"></a>IDbSet 속성을 사용 하 여 DbContext  

때 모의 개체를 만들거나 등 fakes 유용한 인터페이스를 사용 하 여 집합 속성을 선언 하는 경우가 있습니다. 이러한 경우는 IDbSet DbSet 대신 인터페이스를 사용할 수 있습니다. 예를 들어:  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

이 컨텍스트에서 해당 설정 속성에 대 한 DbSet 클래스를 사용 하는 컨텍스트로 동일한 방식으로 작동 합니다.  

## <a name="dbcontext-with-read-only-set-properties"></a>읽기 전용으로 설정 된 속성을 사용 하 여 DbContext  

DbSet 또는 IDbSet 속성에 public setter를 노출 하 고 싶지 않은 경우 대신 읽기 전용 속성을 만들고 하 직접 집합 인스턴스를 만듭니다. 예를 들어:  

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

DbContext 인스턴스의 호출 될 때마다 동일한 인스턴스를 반환은 이러한 각 속성 집합 메서드에서 반환 하는 DbSet 캐시 하는 note 합니다.  

동일한 방식으로 여기에서 작동 하는 Code First에 대 한 엔터티 형식 검색 되는 공용 getter 및 setter를 사용 하 여 속성에 대 한 합니다.  
