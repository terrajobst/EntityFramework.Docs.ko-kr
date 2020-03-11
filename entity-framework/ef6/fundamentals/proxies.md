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
# <a name="working-with-proxies"></a><span data-ttu-id="f7ab8-102">프록시 사용</span><span class="sxs-lookup"><span data-stu-id="f7ab8-102">Working with proxies</span></span>
<span data-ttu-id="f7ab8-103">POCO 엔터티 형식의 인스턴스를 만들 때 Entity Framework은 종종 엔터티에 대 한 프록시 역할을 하는 동적으로 생성 된 파생 형식의 인스턴스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-103">When creating instances of POCO entity types, Entity Framework often creates instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="f7ab8-104">이 프록시는 엔터티의 일부 가상 속성을 재정의 하 여 속성에 액세스할 때 작업을 자동으로 수행 하기 위한 후크를 삽입 합니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-104">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="f7ab8-105">예를 들어이 메커니즘은 관계 지연 로드를 지 원하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-105">For example, this mechanism is used to support lazy loading of relationships.</span></span> <span data-ttu-id="f7ab8-106">이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="disabling-proxy-creation"></a><span data-ttu-id="f7ab8-107">프록시 생성 사용 안 함</span><span class="sxs-lookup"><span data-stu-id="f7ab8-107">Disabling proxy creation</span></span>  

<span data-ttu-id="f7ab8-108">Entity Framework 프록시 인스턴스를 만들지 못하도록 하는 것이 유용한 경우도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-108">Sometimes it is useful to prevent Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="f7ab8-109">예를 들어 프록시가 아닌 인스턴스를 serialize 하는 것은 프록시 인스턴스를 직렬화 하는 것 보다 훨씬 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-109">For example, serializing non-proxy instances is considerably easier than serializing proxy instances.</span></span> <span data-ttu-id="f7ab8-110">ProxyCreationEnabled 플래그를 지워 프록시를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-110">Proxy creation can be turned off by clearing the ProxyCreationEnabled flag.</span></span> <span data-ttu-id="f7ab8-111">이 작업을 수행 하는 한 가지 방법은 컨텍스트의 생성자에서 수행 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-111">One place you could do this is in the constructor of your context.</span></span> <span data-ttu-id="f7ab8-112">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-112">For example:</span></span>  

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

<span data-ttu-id="f7ab8-113">EF는 프록시가 수행할 항목이 없는 형식에 대 한 프록시를 만들지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-113">Note that the EF will not create proxies for types where there is nothing for the proxy to do.</span></span> <span data-ttu-id="f7ab8-114">즉, 봉인 되거나 가상 속성이 없는 형식을 사용 하 여 프록시를 방지할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-114">This means that you can also avoid proxies by having types that are sealed and/or have no virtual properties.</span></span>  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a><span data-ttu-id="f7ab8-115">명시적으로 프록시 인스턴스 만들기</span><span class="sxs-lookup"><span data-stu-id="f7ab8-115">Explicitly creating an instance of a proxy</span></span>  

<span data-ttu-id="f7ab8-116">New 연산자를 사용 하 여 엔터티 인스턴스를 만든 경우에는 프록시 인스턴스가 생성 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-116">A proxy instance will not be created if you create an instance of an entity using the new operator.</span></span> <span data-ttu-id="f7ab8-117">이는 문제가 되지 않을 수 있지만, 프록시 인스턴스를 만들어야 하는 경우 (예: 지연 로드 또는 프록시 변경 내용 추적이 작동 하는 경우) DbSet의 Create 메서드를 사용 하 여 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-117">This may not be a problem, but if you need to create a proxy instance (for example, so that lazy loading or proxy change tracking will work) then you can do so using the Create method of DbSet.</span></span> <span data-ttu-id="f7ab8-118">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-118">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

<span data-ttu-id="f7ab8-119">파생 엔터티 형식의 인스턴스를 만들려는 경우 제네릭 버전의 Create를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-119">The generic version of Create can be used if you want to create an instance of a derived entity type.</span></span> <span data-ttu-id="f7ab8-120">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

<span data-ttu-id="f7ab8-121">Create 메서드는 생성 된 엔터티를 컨텍스트에 추가 하거나 연결 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-121">Note that the Create method does not add or attach the created entity to the context.</span></span>  

<span data-ttu-id="f7ab8-122">Create 메서드는 엔터티에 대 한 프록시 형식을 만들 때 아무 작업도 수행 하지 않으므로 엔터티 형식 자체의 인스턴스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-122">Note that the Create method will just create an instance of the entity type itself if creating a proxy type for the entity would have no value because it would not do anything.</span></span> <span data-ttu-id="f7ab8-123">예를 들어 엔터티 형식이 sealed 이거나 가상 속성이 없는 경우 Create는 엔터티 형식의 인스턴스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-123">For example, if the entity type is sealed and/or has no virtual properties then Create will just create an instance of the entity type.</span></span>  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a><span data-ttu-id="f7ab8-124">프록시 형식에서 실제 엔터티 형식 가져오기</span><span class="sxs-lookup"><span data-stu-id="f7ab8-124">Getting the actual entity type from a proxy type</span></span>  

<span data-ttu-id="f7ab8-125">프록시 형식의 이름은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-125">Proxy types have names that look something like this:</span></span>  

<span data-ttu-id="f7ab8-126">System.object. DynamicProxies Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span><span class="sxs-lookup"><span data-stu-id="f7ab8-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span></span>  

<span data-ttu-id="f7ab8-127">ObjectContext의 GetObjectType 메서드를 사용 하 여이 프록시 형식에 대 한 엔터티 형식을 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-127">You can find the entity type for this proxy type using the GetObjectType method from ObjectContext.</span></span> <span data-ttu-id="f7ab8-128">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-128">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

<span data-ttu-id="f7ab8-129">GetObjectType에 전달 된 형식이 프록시 형식이 아닌 엔터티 형식의 인스턴스인 경우 엔터티 형식이 계속 반환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-129">Note that if the type passed to GetObjectType is an instance of an entity type that is not a proxy type then the type of entity is still returned.</span></span> <span data-ttu-id="f7ab8-130">즉, 항상이 메서드를 사용 하 여 형식이 프록시 형식 인지 여부를 확인 하기 위해 다른 확인 없이 실제 엔터티 형식을 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f7ab8-130">This means you can always use this method to get the actual entity type without any other checking to see if the type is a proxy type or not.</span></span>  
