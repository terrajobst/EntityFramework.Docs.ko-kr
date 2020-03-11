---
title: 프록시 사용-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 8f7d2e8b41ece28efe8d1df3b0679e6e4510d64a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415990"
---
# <a name="working-with-proxies"></a>프록시 사용
POCO 엔터티 형식의 인스턴스를 만들 때 Entity Framework은 종종 엔터티에 대 한 프록시 역할을 하는 동적으로 생성 된 파생 형식의 인스턴스를 만듭니다. 이 프록시는 엔터티의 일부 가상 속성을 재정의 하 여 속성에 액세스할 때 작업을 자동으로 수행 하기 위한 후크를 삽입 합니다. 예를 들어이 메커니즘은 관계 지연 로드를 지 원하는 데 사용 됩니다. 이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

## <a name="disabling-proxy-creation"></a>프록시 생성 사용 안 함  

Entity Framework 프록시 인스턴스를 만들지 못하도록 하는 것이 유용한 경우도 있습니다. 예를 들어 프록시가 아닌 인스턴스를 serialize 하는 것은 프록시 인스턴스를 직렬화 하는 것 보다 훨씬 쉽습니다. ProxyCreationEnabled 플래그를 지워 프록시를 만들 수 있습니다. 이 작업을 수행 하는 한 가지 방법은 컨텍스트의 생성자에서 수행 하는 것입니다. 예를 들면 다음과 같습니다.  

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

EF는 프록시가 수행할 항목이 없는 형식에 대 한 프록시를 만들지 않습니다. 즉, 봉인 되거나 가상 속성이 없는 형식을 사용 하 여 프록시를 방지할 수도 있습니다.  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a>명시적으로 프록시 인스턴스 만들기  

New 연산자를 사용 하 여 엔터티 인스턴스를 만든 경우에는 프록시 인스턴스가 생성 되지 않습니다. 이는 문제가 되지 않을 수 있지만, 프록시 인스턴스를 만들어야 하는 경우 (예: 지연 로드 또는 프록시 변경 내용 추적이 작동 하는 경우) DbSet의 Create 메서드를 사용 하 여 수행할 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

파생 엔터티 형식의 인스턴스를 만들려는 경우 제네릭 버전의 Create를 사용할 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

Create 메서드는 생성 된 엔터티를 컨텍스트에 추가 하거나 연결 하지 않습니다.  

Create 메서드는 엔터티에 대 한 프록시 형식을 만들 때 아무 작업도 수행 하지 않으므로 엔터티 형식 자체의 인스턴스를 만듭니다. 예를 들어 엔터티 형식이 sealed 이거나 가상 속성이 없는 경우 Create는 엔터티 형식의 인스턴스를 만듭니다.  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a>프록시 형식에서 실제 엔터티 형식 가져오기  

프록시 형식의 이름은 다음과 같습니다.  

System.object. DynamicProxies Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6  

ObjectContext의 GetObjectType 메서드를 사용 하 여이 프록시 형식에 대 한 엔터티 형식을 찾을 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

GetObjectType에 전달 된 형식이 프록시 형식이 아닌 엔터티 형식의 인스턴스인 경우 엔터티 형식이 계속 반환 됩니다. 즉, 항상이 메서드를 사용 하 여 형식이 프록시 형식 인지 여부를 확인 하기 위해 다른 확인 없이 실제 엔터티 형식을 가져올 수 있습니다.  
