---
title: 프록시-EF6 사용
author: divega
ms.date: 10/23/2016
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 8f7d2e8b41ece28efe8d1df3b0679e6e4510d64a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489819"
---
# <a name="working-with-proxies"></a><span data-ttu-id="fd20e-102">프록시 사용</span><span class="sxs-lookup"><span data-stu-id="fd20e-102">Working with proxies</span></span>
<span data-ttu-id="fd20e-103">POCO 엔터티 형식 인스턴스를 만들 때 Entity Framework는 종종 엔터티에 대 한 프록시로 작동 하는 동적으로 생성 된 파생 형식의 인스턴스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-103">When creating instances of POCO entity types, Entity Framework often creates instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="fd20e-104">이 프록시 속성에 액세스할 때 자동으로 작업을 수행 하는 것에 대 한 후크를 삽입할 엔터티의 일부 가상 속성을 재정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-104">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="fd20e-105">예를 들어,이 메커니즘은 관계의 지연 로드를 지원 하기 위해 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-105">For example, this mechanism is used to support lazy loading of relationships.</span></span> <span data-ttu-id="fd20e-106">이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="disabling-proxy-creation"></a><span data-ttu-id="fd20e-107">프록시 생성을 사용 하지 않도록 설정</span><span class="sxs-lookup"><span data-stu-id="fd20e-107">Disabling proxy creation</span></span>  

<span data-ttu-id="fd20e-108">경우에 따라 프록시 인스턴스를 만드는에서 Entity Framework를 방지 하는 데 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-108">Sometimes it is useful to prevent Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="fd20e-109">예를 들어 프록시가 아닌 인스턴스를 직렬화 하는 작업은 프록시 인스턴스를 직렬화 하는 작업 보다 훨씬 더 쉬워졌습니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-109">For example, serializing non-proxy instances is considerably easier than serializing proxy instances.</span></span> <span data-ttu-id="fd20e-110">프록시 생성 ProxyCreationEnabled 플래그 선택을 취소 하 여 해제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-110">Proxy creation can be turned off by clearing the ProxyCreationEnabled flag.</span></span> <span data-ttu-id="fd20e-111">한 곳이를 수행할 수 있습니다 컨텍스트의 생성자입니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-111">One place you could do this is in the constructor of your context.</span></span> <span data-ttu-id="fd20e-112">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="fd20e-112">For example:</span></span>  

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

<span data-ttu-id="fd20e-113">EF는 형식에 대 한 프록시를 만들지는 작업을 수행 하는 프록시에 대 한 항목이 없는 경우.</span><span class="sxs-lookup"><span data-stu-id="fd20e-113">Note that the EF will not create proxies for types where there is nothing for the proxy to do.</span></span> <span data-ttu-id="fd20e-114">즉,는 또한 여 방지할 수 있습니다 프록시 형식이 봉인 됩니다 및/또는 가상 속성이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-114">This means that you can also avoid proxies by having types that are sealed and/or have no virtual properties.</span></span>  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a><span data-ttu-id="fd20e-115">명시적으로 프록시 인스턴스를 만들기</span><span class="sxs-lookup"><span data-stu-id="fd20e-115">Explicitly creating an instance of a proxy</span></span>  

<span data-ttu-id="fd20e-116">New 연산자를 사용 하 여 엔터티의 인스턴스를 만든 경우 프록시 인스턴스를 만들 수는 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-116">A proxy instance will not be created if you create an instance of an entity using the new operator.</span></span> <span data-ttu-id="fd20e-117">이 문제가 되지 않을 수 있습니다 하지만 (예를 들어, 지연 로딩 또는 프록시 변경 내용 추적 작동할 수 있도록) 프록시 인스턴스를 생성 해야 하는 경우 다음 할 수 있는 DbSet의 Create 메서드를 사용 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-117">This may not be a problem, but if you need to create a proxy instance (for example, so that lazy loading or proxy change tracking will work) then you can do so using the Create method of DbSet.</span></span> <span data-ttu-id="fd20e-118">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="fd20e-118">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

<span data-ttu-id="fd20e-119">파생 된 엔터티 형식의 인스턴스를 만들려는 경우에 만들기의 제네릭 버전을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-119">The generic version of Create can be used if you want to create an instance of a derived entity type.</span></span> <span data-ttu-id="fd20e-120">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="fd20e-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

<span data-ttu-id="fd20e-121">참고 Create 메서드 추가 되거나 생성된 된 엔터티를 컨텍스트에 연결 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-121">Note that the Create method does not add or attach the created entity to the context.</span></span>  

<span data-ttu-id="fd20e-122">Create 메서드는 방금 만든 엔터티 형식 자체의 인스턴스 엔터티에 대 한 프록시 형식을 만드는 경우 참고는 아무 것도 수행 하는 때문에 값이 없는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-122">Note that the Create method will just create an instance of the entity type itself if creating a proxy type for the entity would have no value because it would not do anything.</span></span> <span data-ttu-id="fd20e-123">예를 들어, 엔터티 형식이 봉인 되어 만들기는 단순히 없는 가상 속성에 있는 경우 엔터티 형식의 인스턴스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-123">For example, if the entity type is sealed and/or has no virtual properties then Create will just create an instance of the entity type.</span></span>  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a><span data-ttu-id="fd20e-124">실제 엔터티 형식이 프록시 형식에서 가져오기</span><span class="sxs-lookup"><span data-stu-id="fd20e-124">Getting the actual entity type from a proxy type</span></span>  

<span data-ttu-id="fd20e-125">프록시 형식은 다음과 같아야 이름을 갖습니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-125">Proxy types have names that look something like this:</span></span>  

<span data-ttu-id="fd20e-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span><span class="sxs-lookup"><span data-stu-id="fd20e-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span></span>  

<span data-ttu-id="fd20e-127">ObjectContext에서 GetObjectType 메서드를 사용 하 여이 프록시 형식에 대 한 엔터티 형식을 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-127">You can find the entity type for this proxy type using the GetObjectType method from ObjectContext.</span></span> <span data-ttu-id="fd20e-128">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="fd20e-128">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

<span data-ttu-id="fd20e-129">GetObjectType에 전달 된 형식이 아닌 프록시 형식을 엔터티 형식은 엔터티 형식 인스턴스의 경우 여전히 반환 되는 참고 합니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-129">Note that if the type passed to GetObjectType is an instance of an entity type that is not a proxy type then the type of entity is still returned.</span></span> <span data-ttu-id="fd20e-130">즉, 다른 검사 유형 프록시 형식 인지 확인 하지 않고 실제 엔터티 형식을 가져오기 위해이 메서드를 항상 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fd20e-130">This means you can always use this method to get the actual entity type without any other checking to see if the type is a proxy type or not.</span></span>  
