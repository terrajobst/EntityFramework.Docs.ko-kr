---
title: 사용자 지정 코드 첫 번째 규칙-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: 79450790c6d3c8ce7fad209e3946e81d3fad4b75
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995830"
---
# <a name="custom-code-first-conventions"></a><span data-ttu-id="01c5f-102">사용자 지정 코드의 첫 번째 규칙</span><span class="sxs-lookup"><span data-stu-id="01c5f-102">Custom Code First Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="01c5f-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="01c5f-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="01c5f-105">Code First를 사용 하는 경우 모델은 규칙 집합을 사용 하 여 수업에서 계산 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-105">When using Code First your model is calculated from your classes using a set of conventions.</span></span> <span data-ttu-id="01c5f-106">기본값 [코드의 첫 번째 규칙](~/ef6/modeling/code-first/conventions/built-in.md) 는 같은 속성은 엔터티의 기본 키, 어떤 전체 자릿수 및 소수 10 진수 열에는 기본적으로 엔터티의 매핑될 테이블의 이름을 가지를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-106">The default [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) determine things like which property becomes the primary key of an entity, the name of the table an entity maps to, and what precision and scale a decimal column has by default.</span></span>

<span data-ttu-id="01c5f-107">경우에 따라 이러한 기본 규칙 모델에 대 한 적합 하지 않습니다 하 고 데이터 주석 또는 Fluent API를 사용 하 여 여러 개별 엔터티를 구성 하 여 해결 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-107">Sometimes these default conventions are not ideal for your model, and you have to work around them by configuring many individual entities using Data Annotations or the Fluent API.</span></span> <span data-ttu-id="01c5f-108">사용자 지정 코드의 첫 번째 규칙을 통해 모델에 대 한 구성 기본값을 제공 하는 사용자 고유의 규칙을 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-108">Custom Code First Conventions let you define your own conventions that provide configuration defaults for your model.</span></span> <span data-ttu-id="01c5f-109">이 연습에서는 다양 한 유형의 사용자 지정 규칙 및 각각을 만드는 방법에 살펴봅니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-109">In this walkthrough, we will explore the different types of custom conventions and how to create each of them.</span></span>


## <a name="model-based-conventions"></a><span data-ttu-id="01c5f-110">모델 기반 규칙</span><span class="sxs-lookup"><span data-stu-id="01c5f-110">Model-Based Conventions</span></span>

<span data-ttu-id="01c5f-111">이 페이지에서는 사용자 지정 규칙에 대 한 DbModelBuilder API를 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-111">This page covers the DbModelBuilder API for custom conventions.</span></span> <span data-ttu-id="01c5f-112">이 API는 대부분의 사용자 지정 규칙을 작성 하기 위한 충분 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-112">This API should be sufficient for authoring most custom conventions.</span></span> <span data-ttu-id="01c5f-113">그러나 모델 기반 규칙-생성 된 후 최종 모델을 조작 하는 규칙을 작성 하려면-고급 시나리오를 처리 하는 기능 또한 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-113">However, there is also the ability to author model-based conventions - conventions that manipulate the final model once it is created - to handle advanced scenarios.</span></span> <span data-ttu-id="01c5f-114">자세한 내용은 [모델 기반 규칙](~/ef6/modeling/code-first/conventions/model.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-114">For more information, see [Model-Based Conventions](~/ef6/modeling/code-first/conventions/model.md).</span></span>

 

## <a name="our-model"></a><span data-ttu-id="01c5f-115">모델</span><span class="sxs-lookup"><span data-stu-id="01c5f-115">Our Model</span></span>

<span data-ttu-id="01c5f-116">규칙을 사용 하 여 사용할 수 있는 간단한 모델을 정의 하 여 시작 해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-116">Let's start by defining a simple model that we can use with our conventions.</span></span> <span data-ttu-id="01c5f-117">다음 클래스를 프로젝트에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-117">Add the following classes to your project.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a><span data-ttu-id="01c5f-118">사용자 지정 규칙을 소개</span><span class="sxs-lookup"><span data-stu-id="01c5f-118">Introducing Custom Conventions</span></span>

<span data-ttu-id="01c5f-119">해당 엔터티 형식에 대 한 기본 키로 키 속성을 구성 하는 규칙을 작성해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-119">Let’s write a convention that configures any property named Key to be the primary key for its entity type.</span></span>

<span data-ttu-id="01c5f-120">규칙 컨텍스트에서 OnModelCreating을 재정의 하 여 액세스할 수 있는 모델 작성기를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-120">Conventions are enabled on the model builder, which can be accessed by overriding OnModelCreating in the context.</span></span> <span data-ttu-id="01c5f-121">ProductContext 클래스를 다음과 같이 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-121">Update the ProductContext class as follows:</span></span>

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

<span data-ttu-id="01c5f-122">키 이름이 모델의 속성 수는 이제 모든 엔터티의 기본 키로 해당 부분을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-122">Now, any property in our model named Key will be configured as the primary key of whatever entity its part of.</span></span>

<span data-ttu-id="01c5f-123">구성 하려는 속성의 형식을 필터링 하 여 규칙을 구체적인 만들 수도 했습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-123">We could also make our conventions more specific by filtering on the type of property that we are going to configure:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

<span data-ttu-id="01c5f-124">이렇게 하면 해당 엔터티의 키 이지만 정수 있는 경우에 기본 키라고 하는 모든 속성이 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-124">This will configure all properties called Key to be the primary key of their entity, but only if they are an integer.</span></span>

<span data-ttu-id="01c5f-125">IsKey 메서드는 흥미로운 기능은 것은 가산적입니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-125">An interesting feature of the IsKey method is that it is additive.</span></span> <span data-ttu-id="01c5f-126">즉, 여러 속성에 대해 IsKey를 호출 하 고 이러한 모든 복합 키의 일부가 될 경우.</span><span class="sxs-lookup"><span data-stu-id="01c5f-126">Which means that if you call IsKey on multiple properties and they will all become part of a composite key.</span></span> <span data-ttu-id="01c5f-127">이 대 한 중요 한 점은 키에 대 한 여러 속성을 지정 하는 경우 지정 해야 합니다도 해당 속성에 대 한 주문을 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-127">The one caveat for this is that when you specify multiple properties for a key you must also specify an order for those properties.</span></span> <span data-ttu-id="01c5f-128">아래와 같은 메서드 HasColumnOrder를 호출 하 여이 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-128">You can do this by calling the HasColumnOrder method like below:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

<span data-ttu-id="01c5f-129">이 코드에서 int 키 열과 문자열 이름 열으로 구성 된 복합 키를 갖는 모델 유형을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-129">This code will configure the types in our model to have a composite key consisting of the int Key column and the string Name column.</span></span> <span data-ttu-id="01c5f-130">디자이너에서 모델을 보면 다음과 같이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-130">If we view the model in the designer it would look like this:</span></span>

![compositeKey](~/ef6/media/compositekey.png)

<span data-ttu-id="01c5f-132">속성 규칙의 또 다른 예로 날짜/시간 대신 SQL server에서 datetime2 형식에 매핑할 모델 내에서 모든 날짜/시간 속성을 구성 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-132">Another example of property conventions is to configure all DateTime properties in my model to map to the datetime2 type in SQL Server instead of datetime.</span></span> <span data-ttu-id="01c5f-133">다음을 사용 하 여이 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-133">You can achieve this with the following:</span></span>

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a><span data-ttu-id="01c5f-134">규칙 클래스</span><span class="sxs-lookup"><span data-stu-id="01c5f-134">Convention Classes</span></span>

<span data-ttu-id="01c5f-135">규칙을 정의 하는 또 다른 방법은 규칙을 캡슐화 하는 규칙 클래스를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-135">Another way of defining conventions is to use a Convention Class to encapsulate your convention.</span></span> <span data-ttu-id="01c5f-136">규칙 클래스를 사용 하는 경우 System.Data.Entity.ModelConfiguration.Conventions 네임 스페이스의 규칙 클래스에서 상속 하는 형식을 만들 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-136">When using a Convention Class then you create a type that inherits from the Convention class in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>

<span data-ttu-id="01c5f-137">규칙 클래스는 다음을 수행 하 여 앞서 살펴본 datetime2 규칙을 사용 하 여 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-137">We can create a Convention Class with the datetime2 convention that we showed earlier by doing the following:</span></span>

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

<span data-ttu-id="01c5f-138">OnModelCreating 연습을 따라 수행 하는 경우 다음과 같이 표시 됩니다는 규칙 컬렉션에 추가한이 규칙을 사용 하는 EF를 알려야 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-138">To tell EF to use this convention you add it to the Conventions collection in OnModelCreating, which if you’ve been following along with the walkthrough will look like this:</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

<span data-ttu-id="01c5f-139">알 수 있듯이 규칙 컬렉션에이 규칙의 인스턴스를 추가 했습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-139">As you can see we add an instance of our convention to the conventions collection.</span></span> <span data-ttu-id="01c5f-140">규칙에서 상속 하는 그룹화 하 고 팀 또는 프로젝트에서 규칙을 공유 하는 편리한 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-140">Inheriting from Convention provides a convenient way of grouping and sharing conventions across teams or projects.</span></span> <span data-ttu-id="01c5f-141">예를 들어, 공통 집합을 사용 하 여를 프로젝트 조직의의 모든 규칙을 사용 하 여 클래스 라이브러리를 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-141">You could, for example, have a class library with a common set of conventions that all of your organizations projects use.</span></span>

 

## <a name="custom-attributes"></a><span data-ttu-id="01c5f-142">사용자 지정 특성</span><span class="sxs-lookup"><span data-stu-id="01c5f-142">Custom Attributes</span></span>

<span data-ttu-id="01c5f-143">규칙의 다른 좋은 사용 모델을 구성할 때 사용할 새 특성을 사용 하도록 설정 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-143">Another great use of conventions is to enable new attributes to be used when configuring a model.</span></span> <span data-ttu-id="01c5f-144">예를 들어 문자열 속성을 비유니코드 됨으로 표시 하려면 사용할 수 있는 특성을 만들어 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-144">To illustrate this, let’s create an attribute that we can use to mark String properties as non-Unicode.</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

<span data-ttu-id="01c5f-145">이제 모델에이 특성을 적용 하는 규칙을 만들어 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-145">Now, let’s create a convention to apply this attribute to our model:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

<span data-ttu-id="01c5f-146">이 규칙을 사용 하 여 nvarchar 대신 varchar로 저장할 유니코드가 아닌 특성 즉, 데이터베이스의 열이 문자열 속성을 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-146">With this convention we can add the NonUnicode attribute to any of our string properties, which means the column in the database will be stored as varchar instead of nvarchar.</span></span>

<span data-ttu-id="01c5f-147">이 규칙에 대해 한 가지는 경우 예외를 throw 하는 다음 문자열 속성 이외에 유니코드가 아닌 특성을 삽입 하면입니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-147">One thing to note about this convention is that if you put the NonUnicode attribute on anything other than a string property then it will throw an exception.</span></span> <span data-ttu-id="01c5f-148">문자열이 아닌 형식에서 IsUnicode를 구성할 수 없으므로이 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-148">It does this because you cannot configure IsUnicode on any type other than a string.</span></span> <span data-ttu-id="01c5f-149">이 경우 만들 수 있습니다 규칙에 따라 더 구체적으로 문자열이 있는 모든 항목이 필터링 되도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-149">If this happens, then you can make your convention more specific, so that it filters out anything that isn’t a string.</span></span>

<span data-ttu-id="01c5f-150">위의 규칙을 사용자 지정 특성을 정의 하기 위한 작동 하는 동안에 사용, 특히 하려는 경우 특성 클래스에서 속성을 사용 하기 쉽게 될 수 있는 다른 API입니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-150">While the above convention works for defining custom attributes there is another API that can be much easier to use, especially when you want to use properties from the attribute class.</span></span>

<span data-ttu-id="01c5f-151">이 예제에서는 것이 특성을 업데이트 하 고 다음과 같이 표시 됩니다 IsUnicode 특성을 변경 하려면:</span><span class="sxs-lookup"><span data-stu-id="01c5f-151">For this example we are going to update our attribute and change it to an IsUnicode attribute, so it looks like this:</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

<span data-ttu-id="01c5f-152">이 있으면, bool 속성 유니코드 해야 할지 여부는 규칙을 구별 하는 특성에 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-152">Once we have this, we can set a bool on our attribute to tell the convention whether or not a property should be Unicode.</span></span> <span data-ttu-id="01c5f-153">에서는 다음과 같은 구성 클래스의 ClrProperty에 액세스 하 여 이미 규칙에서 수행할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-153">We could do this in the convention we have already by accessing the ClrProperty of the configuration class like this:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

<span data-ttu-id="01c5f-154">이 쉽지만 이지만 Having을 사용 하 여이 달성 하는 보다 간결한 방법을 규칙 API의 메서드.</span><span class="sxs-lookup"><span data-stu-id="01c5f-154">This is easy enough, but there is a more succinct way of achieving this by using the Having method of the conventions API.</span></span> <span data-ttu-id="01c5f-155">메서드 매개 변수가 있는 Func를 입력&lt;PropertyInfo, T&gt; 허용 하는 PropertyInfo Where와 동일 메서드를 이지만 개체를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-155">The Having method has a parameter of type Func&lt;PropertyInfo, T&gt; which accepts the PropertyInfo the same as the Where method, but is expected to return an object.</span></span> <span data-ttu-id="01c5f-156">속성을 구성할 수는 반환 된 개체가 null 이면 Where, 마찬가지로 속성으로 필터링 할 수 있다는 의미입니다 이지만 다른는 또한으로 반환 되는 개체를 캡처 및 구성 메서드에 전달 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-156">If the returned object is null then the property will not be configured, which means you can filter out properties with it just like Where, but it is different in that it will also capture the returned object and pass it to the Configure method.</span></span> <span data-ttu-id="01c5f-157">이 다음과 같이 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-157">This works like the following:</span></span>

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

<span data-ttu-id="01c5f-158">사용자 지정 특성은 Having을 사용 하는 유일한 이유 메서드를 유용 프로그램 형식 또는 속성을 구성 하는 경우에 필터링 하는 것에 대해 추론 해야 하는 원격입니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-158">Custom attributes are not the only reason to use the Having method, it is useful anywhere that you need to reason about something that you are filtering on when configuring your types or properties.</span></span>

 

## <a name="configuring-types"></a><span data-ttu-id="01c5f-159">구성 형식</span><span class="sxs-lookup"><span data-stu-id="01c5f-159">Configuring Types</span></span>

<span data-ttu-id="01c5f-160">속성에 대 한 된 규칙을 모두 지금 이지만 규칙 API의 다른 영역 모델에서 형식을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-160">So far all of our conventions have been for properties, but there is another area of the conventions API for configuring the types in your model.</span></span> <span data-ttu-id="01c5f-161">환경을 지금까지 살펴본 규칙 유사 하지만 내부 구성에서 옵션 수준에서 속성 대신 엔터티 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-161">The experience is similar to the conventions we have seen so far, but the options inside configure will be at the entity instead of property level.</span></span>

<span data-ttu-id="01c5f-162">형식 수준 규칙에 매우 유용한 수 있는 작업 중 하나는 테이블 명명 규칙을 EF 기본값과에서 다른 기존 스키마에 매핑할 또는 다른 명명 규칙을 사용 하 여 새 데이터베이스를 만들려는 변경입니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-162">One of the things that Type level conventions can be really useful for is changing the table naming convention, either to map to an existing schema that differs from the EF default or to create a new database with a different naming convention.</span></span> <span data-ttu-id="01c5f-163">이렇게 하려면 먼저 모델의 형식에 대 한 TypeInfo를 수락 하 고 있어야 하는 형식에 대 한 테이블 이름을 반환할 수 있는 메서드를 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-163">To do this we first need a method that can accept the TypeInfo for a type in our model and return what the table name for that type should be:</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

<span data-ttu-id="01c5f-164">이 메서드는 형식을 사용 하 고 CamelCase 대신 밑줄을 사용 하 여 소문자를 사용 하는 문자열을 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-164">This method takes a type and returns a string that uses lower case with underscores instead of CamelCase.</span></span> <span data-ttu-id="01c5f-165">모델 즉 ProductCategory 클래스 제품 이라고 하는 테이블에 매핑될는\_ProductCategories 대신 범주입니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-165">In our model this means that the ProductCategory class will be mapped to a table called product\_category instead of ProductCategories.</span></span>

<span data-ttu-id="01c5f-166">해당 메서드가 있으면 다음과 같은 규칙에서 호출할 수 것:</span><span class="sxs-lookup"><span data-stu-id="01c5f-166">Once we have that method we can call it in a convention like this:</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

<span data-ttu-id="01c5f-167">GetTableName 메서드에서 반환 되는 테이블 이름에 매핑할 모델에서 모든 형식을 구성 하는이 규칙.</span><span class="sxs-lookup"><span data-stu-id="01c5f-167">This convention configures every type in our model to map to the table name that is returned from our GetTableName method.</span></span> <span data-ttu-id="01c5f-168">이 규칙은 Fluent API를 사용 하 여 모델의 각 엔터티에 대해 ToTable 메서드를 호출 하는 것과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-168">This convention is the equivalent to calling the ToTable method for each entity in the model using the Fluent API.</span></span>

<span data-ttu-id="01c5f-169">하나에 대해 알아두어야 할이 되는 호출 하는 경우 ToTable EF는 걸릴 정확한 테이블 이름을 복수화 테이블 이름을 확인할 때 수행 일반적으로 사용 하지 않고으로 제공 하는 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-169">One thing to note about this is that when you call ToTable EF will take the string that you provide as the exact table name, without any of the pluralization that it would normally do when determining table names.</span></span> <span data-ttu-id="01c5f-170">이 때문에 규칙에서 테이블 이름은 제품\_제품 대신 범주\_범주입니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-170">This is why the table name from our convention is product\_category instead of product\_categories.</span></span> <span data-ttu-id="01c5f-171">에서는 해결할 수는 규칙에서 직접 복수 적용 서비스를 호출 하 여.</span><span class="sxs-lookup"><span data-stu-id="01c5f-171">We can resolve that in our convention by making a call to the pluralization service ourselves.</span></span>

<span data-ttu-id="01c5f-172">다음 코드에서는 사용 하 여는 [종속성 확인](~/ef6/fundamentals/configuring/dependency-resolution.md) EF 사용 있는 복수 적용 서비스를 검색 하는 EF6에 추가 된 기능 및이 테이블 이름을 복수화 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-172">In the following code we will use the [Dependency Resolution](~/ef6/fundamentals/configuring/dependency-resolution.md) feature added in EF6 to retrieve the pluralization service that EF would have used and pluralize our table name.</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> <span data-ttu-id="01c5f-173">GetService의 제네릭 버전 System.Data.Entity.Infrastructure.DependencyResolution 네임 스페이스의 확장 메서드를 추가 하 여 해야 사용 하기 위해 컨텍스트를 문의 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-173">The generic version of GetService is an extension method in the System.Data.Entity.Infrastructure.DependencyResolution namespace, you will need to add a using statement to your context in order to use it.</span></span>

### <a name="totable-and-inheritance"></a><span data-ttu-id="01c5f-174">ToTable 및 상속</span><span class="sxs-lookup"><span data-stu-id="01c5f-174">ToTable and Inheritance</span></span>

<span data-ttu-id="01c5f-175">ToTable의 또 다른 중요 한 측면에 있으면는 명시적으로 형식이 지정된 된 테이블에 매핑하는 경우 EF를 사용 하는 매핑 전략을 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-175">Another important aspect of ToTable is that if you explicitly map a type to a given table, then you can alter the mapping strategy that EF will use.</span></span> <span data-ttu-id="01c5f-176">ToTable 상속 계층 구조의 모든 형식에 대해를 호출 하면 위에서 수행한 것과 같이 형식 이름을 테이블 이름으로 전달 됩니다 변경한 다음 기본 (TPH) 계층당 하나의 테이블 매핑 전략에 형식당 하나의 테이블 TPT ().</span><span class="sxs-lookup"><span data-stu-id="01c5f-176">If you call ToTable for every type in an inheritance hierarchy, passing the type name as the name of the table like we did above, then you will change the default Table-Per-Hierarchy (TPH) mapping strategy to Table-Per-Type (TPT).</span></span> <span data-ttu-id="01c5f-177">이 설명 하는 가장 좋은 방법은 whith 구체적인 예제를 사용 하는 것:</span><span class="sxs-lookup"><span data-stu-id="01c5f-177">The best way to describe this is whith a concrete example:</span></span>

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

<span data-ttu-id="01c5f-178">기본적으로 employee 및 manager 둘 다 동일한 테이블 (직원) 데이터베이스에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-178">By default both employee and manager are mapped to the same table (Employees) in the database.</span></span> <span data-ttu-id="01c5f-179">테이블에는 직원 및 관리자는 사실을 알려 주는 어떤 유형의 인스턴스는 각 행에 저장 되어 판별자 열을 사용 하 여 모두 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-179">The table will contain both employees and managers with a discriminator column that will tell you what type of instance is stored in each row.</span></span> <span data-ttu-id="01c5f-180">이 계층에 대 한 단일 테이블이 TPH 매핑이입니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-180">This is TPH mapping as there is a single table for the hierarchy.</span></span> <span data-ttu-id="01c5f-181">그러나 두 classe의 ToTable을 호출 하면 다음 각 형식 대신 매핑될 자체 테이블에 각 형식에는 자체 테이블 이므로 TPT 라고도 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-181">However, if you call ToTable on both classe then each type will instead be mapped to its own table, also known as TPT since each type has its own table.</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

<span data-ttu-id="01c5f-182">위의 코드는 다음과 같이 표시 된 테이블 구조에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-182">The code above will map to a table structure that looks like the following:</span></span>

![tptExample](~/ef6/media/tptexample.jpg)

<span data-ttu-id="01c5f-184">이 문제를 방지 하 고 기본 TPH 매핑이 두 가지 방법으로 유지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-184">You can avoid this, and maintain the default TPH mapping, in a couple ways:</span></span>

1.  <span data-ttu-id="01c5f-185">계층의 각 형식에 대 한 테이블 이름이 같은 ToTable을 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-185">Call ToTable with the same table name for each type in the hierarchy.</span></span>
2.  <span data-ttu-id="01c5f-186">예에서 직원 수는 해당 계층의 기본 클래스 에서만 ToTable을 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-186">Call ToTable only on the base class of the hierarchy, in our example that would be employee.</span></span>

 

## <a name="execution-order"></a><span data-ttu-id="01c5f-187">실행 순서</span><span class="sxs-lookup"><span data-stu-id="01c5f-187">Execution Order</span></span>

<span data-ttu-id="01c5f-188">규칙 마지막 wins 방식으로 Fluent API와 동일 하 게 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-188">Conventions operate in a last wins manner, the same as the Fluent API.</span></span> <span data-ttu-id="01c5f-189">이 의미 하는 동일한 속성의 동일한 옵션을 구성 하는 두 개의 규칙을 마지막 wins 실행 하나를 작성 하는 경우.</span><span class="sxs-lookup"><span data-stu-id="01c5f-189">What this means is that if you write two conventions that configure the same option of the same property, then the last one to execute wins.</span></span> <span data-ttu-id="01c5f-190">예를 들어 아래 코드에서 모든 문자열의 최대 길이 500 개로 설정 되어 있지만 다음 호출 이름 최대 길이 250 모델에서 모든 속성을 구성 했습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-190">As an example, in the code below the max length of all strings is set to 500 but we then configure all properties called Name in the model to have a max length of 250.</span></span>

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

<span data-ttu-id="01c5f-191">모든 문자열을 500으로 설정 된 후 최대 길이 250으로 설정 하는 규칙 이기 때문에 500 것를 모델의 Name 이라는 속성을 모두 설명 등의 다른 문자열 하는 동안 250의 MaxLength를 갖게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-191">Because the convention to set max length to 250 is after the one that sets all strings to 500, all the properties called Name in our model will have a MaxLength of 250 while any other strings, such as descriptions, would be 500.</span></span> <span data-ttu-id="01c5f-192">이 방식으로 규칙을 사용 하 여 형식 또는 모델 및 다음 오른쪽에 속성에 대 한 일반 규칙을 제공할 수 있음을 의미 하는 다른 하위 집합에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-192">Using conventions in this way means that you can provide a general convention for types or properties in your model and then overide them for subsets that are different.</span></span>

<span data-ttu-id="01c5f-193">Fluent API 및 데이터 주석 특정 사례에 규칙을 재정의 하려면 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-193">The Fluent API and Data Annotations can also be used to override a convention in specific cases.</span></span> <span data-ttu-id="01c5f-194">위의 예에서 Fluent API를 사용 하는 속성의 최대 길이 설정 하는 경우 다음 수 있는 배치 했으므로 전이나 후 규칙에 보다 구체적인 Fluent API는 보다 일반적인 구성 규칙을 통해 win 때문에 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-194">In our example above if we had used the Fluent API to set the max length of a property then we could have put it before or after the convention, because the more specific Fluent API will win over the more general Configuration Convention.</span></span>

 

## <a name="built-in-conventions"></a><span data-ttu-id="01c5f-195">기본 제공 규칙</span><span class="sxs-lookup"><span data-stu-id="01c5f-195">Built-in Conventions</span></span>

<span data-ttu-id="01c5f-196">사용자 지정 규칙은 기본 Code First 규칙에 의해 저하 될 수 있습니다, 되므로 전이나 후 다른 규칙을 실행 하는 규칙을 추가 하려면 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-196">Because custom conventions could be affected by the default Code First conventions, it can be useful to add conventions to run before or after another convention.</span></span> <span data-ttu-id="01c5f-197">이렇게 하려면 파생 된 DbContext에서 규칙 컬렉션의 AddBefore 및 addafter입니다 메서드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-197">To do this you can use the AddBefore and AddAfter methods of the Conventions collection on your derived DbContext.</span></span> <span data-ttu-id="01c5f-198">다음 코드를 작성 하기 전에 실행 됩니다 있도록 앞에서 만든 규칙 클래스를 추가할 키 검색 규칙에서입니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-198">The following code would add the convention class we created earlier so that it will run before the built in key discovery convention.</span></span>

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

<span data-ttu-id="01c5f-199">이 것 가장 사용 하기 전에 또는 기본 제공된 규칙 뒤에 실행 해야 하는 규칙을 추가 하는 경우, 기본 제공된 규칙의 목록을 여기: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .</span><span class="sxs-lookup"><span data-stu-id="01c5f-199">This is going to be of the most use when adding conventions that need to run before or after the built in conventions, a list of the built in conventions can be found here: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>

<span data-ttu-id="01c5f-200">또한 모델에 적용 하지 않을 규칙을 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-200">You can also remove conventions that you do not want applied to your model.</span></span> <span data-ttu-id="01c5f-201">규칙을 제거 하려면 Remove 메서드를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-201">To remove a convention, use the Remove method.</span></span> <span data-ttu-id="01c5f-202">제거 된 PluralizingTableNameConvention의 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="01c5f-202">Here is an example of removing the PluralizingTableNameConvention.</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
