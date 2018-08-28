---
title: 프록시-EF6 사용
author: divega
ms.date: 2016-10-23
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 7b82dd370e67d1622fc00ff5e5275721d0fc4fe1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997205"
---
# <a name="working-with-proxies"></a>프록시 사용
POCO 엔터티 형식 인스턴스를 만들 때 Entity Framework는 종종 엔터티에 대 한 프록시로 작동 하는 동적으로 생성 된 파생 형식의 인스턴스를 만듭니다. 이 프록시 속성에 액세스할 때 자동으로 작업을 수행 하는 것에 대 한 후크를 삽입할 엔터티의 일부 가상 속성을 재정의 합니다. 예를 들어,이 메커니즘은 관계의 지연 로드를 지원 하기 위해 사용 됩니다. 이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

## <a name="disabling-proxy-creation"></a>프록시 생성을 사용 하지 않도록 설정  

경우에 따라 프록시 인스턴스를 만드는에서 Entity Framework를 방지 하는 데 유용 합니다. 예를 들어 프록시가 아닌 인스턴스를 직렬화 하는 작업은 프록시 인스턴스를 직렬화 하는 작업 보다 훨씬 더 쉬워졌습니다. 프록시 생성 ProxyCreationEnabled 플래그 선택을 취소 하 여 해제할 수 있습니다. 한 곳이를 수행할 수 있습니다 컨텍스트의 생성자입니다. 예를 들어:  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.ProxyCreationEnabled = false;
    }  

    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

EF는 형식에 대 한 프록시를 만들지는 작업을 수행 하는 프록시에 대 한 항목이 없는 경우. 즉,는 또한 여 방지할 수 있습니다 프록시 형식이 봉인 됩니다 및/또는 가상 속성이 없습니다.  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a>명시적으로 프록시 인스턴스를 만들기  

New 연산자를 사용 하 여 엔터티의 인스턴스를 만든 경우 프록시 인스턴스를 만들 수는 있습니다. 이 문제가 되지 않을 수 있습니다 하지만 (예를 들어, 지연 로딩 또는 프록시 변경 내용 추적 작동할 수 있도록) 프록시 인스턴스를 생성 해야 하는 경우 다음 할 수 있는 DbSet의 Create 메서드를 사용 하 여 합니다. 예를 들어:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

파생 된 엔터티 형식의 인스턴스를 만들려는 경우에 만들기의 제네릭 버전을 사용할 수 있습니다. 예를 들어:  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

참고 Create 메서드 추가 되거나 생성된 된 엔터티를 컨텍스트에 연결 되지 않습니다.  

Create 메서드는 방금 만든 엔터티 형식 자체의 인스턴스 엔터티에 대 한 프록시 형식을 만드는 경우 참고는 아무 것도 수행 하는 때문에 값이 없는 것입니다. 예를 들어, 엔터티 형식이 봉인 되어 만들기는 단순히 없는 가상 속성에 있는 경우 엔터티 형식의 인스턴스를 만듭니다.  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a>실제 엔터티 형식이 프록시 형식에서 가져오기  

프록시 형식은 다음과 같아야 이름을 갖습니다.  

System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6  

ObjectContext에서 GetObjectType 메서드를 사용 하 여이 프록시 형식에 대 한 엔터티 형식을 찾을 수 있습니다. 예를 들어:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

GetObjectType에 전달 된 형식이 아닌 프록시 형식을 엔터티 형식은 엔터티 형식 인스턴스의 경우 여전히 반환 되는 참고 합니다. 즉, 다른 검사 유형 프록시 형식 인지 확인 하지 않고 실제 엔터티 형식을 가져오기 위해이 메서드를 항상 사용할 수 있습니다.  
