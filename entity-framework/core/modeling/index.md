---
title: "모델 만들기 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
ms.technology: entity-framework-core
uid: core/modeling/index
ms.openlocfilehash: 1ad0f6891fbc8ba2e4d102cc9997f053a9dddb66
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="creating-a-model"></a><span data-ttu-id="a12c8-102">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="a12c8-102">Creating a Model</span></span>

<span data-ttu-id="a12c8-103">Entity Framework는 엔터티 클래스의 형태에 따라 규칙 집합을 사용하여 모델을 작성합니다.</span><span class="sxs-lookup"><span data-stu-id="a12c8-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="a12c8-104">규칙에서 발견한 항목을 보충 및/또는 재정의하기 위해 추가적인 구성을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a12c8-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="a12c8-105">이 문서에서는 모든 데이터 저장소를 대상으로 하는 모델에 적용할 수 있는 구성과, 모든 관계형 데이터베이스를 대상으로 할 때 적용할 수 있는 구성을 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="a12c8-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="a12c8-106">공급자는 특정 데이터 저장소에만 해당하는 구성을 활성화할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a12c8-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="a12c8-107">공급자 특정 구성에 대한 문서는 [데이터베이스 공급자](../providers/index.md) 섹션을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="a12c8-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="a12c8-108">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a12c8-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="methods-of-configuration"></a><span data-ttu-id="a12c8-109">구성의 메서드</span><span class="sxs-lookup"><span data-stu-id="a12c8-109">Methods of configuration</span></span>

### <a name="fluent-api"></a><span data-ttu-id="a12c8-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="a12c8-110">Fluent API</span></span>

<span data-ttu-id="a12c8-111">파생된 컨텍스트에서 `OnModelCreating` 메서드를 재정의하고 `ModelBuilder API`를 사용하여 모델을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a12c8-111">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="a12c8-112">이것은 가장 강력한 구성 방법으로, 엔터티 클래스를 수정하지 않고도 구성을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a12c8-112">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="a12c8-113">Fluent API 구성은 우선 순위가 가장 높으며 규칙과 데이터 주석을 재정의합니다.</span><span class="sxs-lookup"><span data-stu-id="a12c8-113">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

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

### <a name="data-annotations"></a><span data-ttu-id="a12c8-114">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="a12c8-114">Data Annotations</span></span>

<span data-ttu-id="a12c8-115">특성(데이터 주석이라고 함)을 클래스와 속성에 정의할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a12c8-115">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="a12c8-116">데이터 주석은 규칙을 재정의하지만 Fluent API 구성이 데이터 주석을 재정의합니다.</span><span class="sxs-lookup"><span data-stu-id="a12c8-116">Data annotations will override conventions, but will be overwritten by Fluent API configuration.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
