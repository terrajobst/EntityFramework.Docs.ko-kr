---
title: 모델 만들기 및 구성 - EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 0f44d9684ca5c8435d83085f9038860309bd82a2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78412778"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="b25ed-102">모델 만들기 및 구성</span><span class="sxs-lookup"><span data-stu-id="b25ed-102">Creating and configuring a model</span></span>

<span data-ttu-id="b25ed-103">Entity Framework는 엔터티 클래스의 형태에 따라 규칙 집합을 사용하여 모델을 작성합니다.</span><span class="sxs-lookup"><span data-stu-id="b25ed-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="b25ed-104">규칙에서 발견한 항목을 보충 및/또는 재정의하기 위해 추가적인 구성을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b25ed-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="b25ed-105">이 문서에서는 모든 데이터 저장소를 대상으로 하는 모델에 적용할 수 있는 구성과, 모든 관계형 데이터베이스를 대상으로 할 때 적용할 수 있는 구성을 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="b25ed-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="b25ed-106">공급자는 특정 데이터 저장소에만 해당하는 구성을 활성화할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b25ed-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="b25ed-107">공급자 특정 구성에 대한 설명서는  [데이터베이스 공급자](../providers/index.md)  섹션을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="b25ed-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="b25ed-108">GitHub에서 이 문서의  [샘플](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) 을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b25ed-108">You can view this article’s [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="b25ed-109">Fluent API를 사용하여 모델 구성</span><span class="sxs-lookup"><span data-stu-id="b25ed-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="b25ed-110">파생된 컨텍스트에서  `OnModelCreating`  메서드를 재정의하고  `ModelBuilder API`를 사용하여 모델을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b25ed-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="b25ed-111">이것은 가장 강력한 구성 방법으로, 엔터티 클래스를 수정하지 않고도 구성을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b25ed-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="b25ed-112">Fluent API 구성은 우선 순위가 가장 높으며 규칙과 데이터 주석을 재정의합니다.</span><span class="sxs-lookup"><span data-stu-id="b25ed-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="b25ed-113">데이터 주석을 사용하여 모델 구성</span><span class="sxs-lookup"><span data-stu-id="b25ed-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="b25ed-114">특성(데이터 주석이라고 함)을 클래스와 속성에 정의할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b25ed-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="b25ed-115">데이터 주석은 규칙을 재정의하지만 흐름 API 구성이 데이터 주석을 재정의합니다.</span><span class="sxs-lookup"><span data-stu-id="b25ed-115">Data annotations will override conventions, but will be overridden by Fluent API configuration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
