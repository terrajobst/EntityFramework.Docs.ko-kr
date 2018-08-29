---
title: EF6 Dbset-정의
author: divega
ms.date: 2016-10-23
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: cc45ed1ceb20bc90090adb3e93c10651c69c9a6a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993859"
---
# <a name="defining-dbsets"></a><span data-ttu-id="6615a-102">Dbset 정의</span><span class="sxs-lookup"><span data-stu-id="6615a-102">Defining DbSets</span></span>
<span data-ttu-id="6615a-103">Code First 워크플로 사용 하 여 개발 하는 경우 데이터베이스를 사용 하 여 세션을 나타내며 모델의 각 형식에 대 한 DbSet을 노출 하는 파생 된 DbContext 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="6615a-103">When developing with the Code First workflow you define a derived DbContext that represents your session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="6615a-104">이 항목에서는 DbSet 속성을 정의할 수는 다양 한 방법을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="6615a-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="6615a-105">DbSet 속성을 사용 하 여 DbContext</span><span class="sxs-lookup"><span data-stu-id="6615a-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="6615a-106">첫 번째 코드 예제에 나와 있는 일반적인 사례는 모델의 엔터티 형식에 대 한 공용 자동 DbSet 속성을 사용 하 여 DbContext를 것입니다.</span><span class="sxs-lookup"><span data-stu-id="6615a-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="6615a-107">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="6615a-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="6615a-108">Code First 모드에서 사용 하는 경우 여기에서 연결할 수 있는 다른 형식 구성 뿐만 아니라 엔터티 형식으로 블로그 및 게시물을 구성 합니다이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6615a-108">When used in Code First mode, this will configure Blogs and Posts as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="6615a-109">또한 DbContext 각 인스턴스의 적절 한 DbSet 설정 하려면 이러한 속성에 대 한 setter를 자동으로 호출 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6615a-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="6615a-110">IDbSet 속성을 사용 하 여 DbContext</span><span class="sxs-lookup"><span data-stu-id="6615a-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="6615a-111">때 모의 개체를 만들거나 등 fakes 유용한 인터페이스를 사용 하 여 집합 속성을 선언 하는 경우가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6615a-111">There are situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="6615a-112">이러한 경우는 IDbSet DbSet 대신 인터페이스를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6615a-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="6615a-113">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="6615a-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="6615a-114">이 컨텍스트에서 해당 설정 속성에 대 한 DbSet 클래스를 사용 하는 컨텍스트로 동일한 방식으로 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="6615a-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="6615a-115">읽기 전용으로 설정 된 속성을 사용 하 여 DbContext</span><span class="sxs-lookup"><span data-stu-id="6615a-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="6615a-116">DbSet 또는 IDbSet 속성에 public setter를 노출 하 고 싶지 않은 경우 대신 읽기 전용 속성을 만들고 하 직접 집합 인스턴스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="6615a-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="6615a-117">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="6615a-117">For example:</span></span>  

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

<span data-ttu-id="6615a-118">DbContext 인스턴스의 호출 될 때마다 동일한 인스턴스를 반환은 이러한 각 속성 집합 메서드에서 반환 하는 DbSet 캐시 하는 note 합니다.</span><span class="sxs-lookup"><span data-stu-id="6615a-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="6615a-119">동일한 방식으로 여기에서 작동 하는 Code First에 대 한 엔터티 형식 검색 되는 공용 getter 및 setter를 사용 하 여 속성에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="6615a-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  
