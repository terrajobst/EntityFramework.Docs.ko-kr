---
title: 모델 만들기 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: e4eed480178ce43cbc5ece8db8e584032da7b2b9
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250350"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="a804a-102">모델 만들기 및 구성</span><span class="sxs-lookup"><span data-stu-id="a804a-102">Creating and configuring a Model</span></span>

<span data-ttu-id="a804a-103">Entity Framework는 엔터티 클래스의 형태에 따라 규칙 집합을 사용하여 모델을 작성합니다.</span><span class="sxs-lookup"><span data-stu-id="a804a-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="a804a-104">규칙에서 발견한 항목을 보충 및/또는 재정의하기 위해 추가적인 구성을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a804a-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="a804a-105">이 문서에서는 모든 데이터 저장소를 대상으로 하는 모델에 적용할 수 있는 구성과, 모든 관계형 데이터베이스를 대상으로 할 때 적용할 수 있는 구성을 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="a804a-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="a804a-106">공급자는 특정 데이터 저장소에만 해당하는 구성을 활성화할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a804a-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="a804a-107">공급자 특정 구성에 대한 문서는 [데이터베이스 공급자](../providers/index.md) 섹션을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="a804a-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="a804a-108">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a804a-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="a804a-109">Fluent API를 사용하여 모델 구성</span><span class="sxs-lookup"><span data-stu-id="a804a-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="a804a-110">파생된 컨텍스트에서 `OnModelCreating` 메서드를 재정의하고 `ModelBuilder API`를 사용하여 모델을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a804a-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="a804a-111">이것은 가장 강력한 구성 방법으로, 엔터티 클래스를 수정하지 않고도 구성을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a804a-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="a804a-112">Fluent API 구성은 우선 순위가 가장 높으며 규칙과 데이터 주석을 재정의합니다.</span><span class="sxs-lookup"><span data-stu-id="a804a-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?range=5-15&highlight=5-10)] -->

``` csharp
    class MyContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>()
                .Property(b => b.Url)
                .IsRequired();
        }
    }
```

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="a804a-113">데이터 주석을 사용하여 모델 구성</span><span class="sxs-lookup"><span data-stu-id="a804a-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="a804a-114">특성(데이터 주석이라고 함)을 클래스와 속성에 정의할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a804a-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="a804a-115">데이터 주석은 규칙을 재정의하지만 Fluent API 구성이 데이터 주석을 재정의합니다.</span><span class="sxs-lookup"><span data-stu-id="a804a-115">Data annotations will override conventions, but will be overwritten by Fluent API configuration.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
