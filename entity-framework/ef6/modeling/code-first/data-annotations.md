---
title: Code First 데이터 주석-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 9fac2a90c46d78ff5fd632800cc0050276467773
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415858"
---
# <a name="code-first-data-annotations"></a><span data-ttu-id="dabb9-102">Code First 데이터 주석</span><span class="sxs-lookup"><span data-stu-id="dabb9-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="dabb9-103">**Ef 4.1** 이상-이 페이지에서 설명 하는 기능, api 등은 Entity Framework 4.1에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="dabb9-104">이전 버전을 사용 하는 경우이 정보 중 일부 또는 전체가 적용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-104">If you are using an earlier version, some or all of this information does not apply.</span></span>

<span data-ttu-id="dabb9-105">이 페이지의 내용은 원래 Julie Lerman (\<http://thedatafarm.com>)에서 작성 한 문서에서 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-105">The content on this page is adapted from an article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="dabb9-106">Entity Framework Code First를 사용 하면 EF가 쿼리, 변경 내용 추적 및 업데이트 기능을 수행 하기 위해 사용 하는 모델을 나타내는 데 고유한 도메인 클래스를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-106">Entity Framework Code First allows you to use your own domain classes to represent the model that EF relies on to perform querying, change tracking, and updating functions.</span></span> <span data-ttu-id="dabb9-107">Code First은 ' 구성에 대 한 규칙 ' 이라고 하는 프로그래밍 패턴을 활용 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-107">Code First leverages a programming pattern referred to as 'convention over configuration.'</span></span> <span data-ttu-id="dabb9-108">Code First는 클래스가 Entity Framework 규칙을 따르는 것으로 가정 하며,이 경우에서 해당 작업을 수행 하는 방법을 자동으로 해결 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-108">Code First will assume that your classes follow the conventions of Entity Framework, and in that case, will automatically work out how to perform its job.</span></span> <span data-ttu-id="dabb9-109">그러나 클래스가 이러한 규칙을 따르지 않는 경우 클래스에 구성을 추가 하 여 필수 정보를 사용 하 여 EF를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-109">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the requisite information.</span></span>

<span data-ttu-id="dabb9-110">Code First은 이러한 구성을 클래스에 추가 하는 두 가지 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-110">Code First gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="dabb9-111">하나는 DataAnnotations 이라는 간단한 특성을 사용 하 고 두 번째는 Code First의 흐름 API를 사용 하는 것입니다 .이 API는 코드에서 명령적으로 구성을 설명 하는 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-111">One is using simple attributes called DataAnnotations, and the second is using Code First’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="dabb9-112">이 문서에서는 System.componentmodel 네임 스페이스에서 DataAnnotations을 사용 하 여 클래스를 구성 하는 방법을 집중적으로 설명 합니다. 가장 일반적으로 필요한 구성을 강조 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-112">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="dabb9-113">이러한 응용 프로그램이 클라이언트 쪽 유효성 검사에 동일한 주석을 활용할 수 있도록 하는 ASP.NET MVC와 같은 여러 .NET 응용 프로그램 에서도 DataAnnotations을 인식 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-113">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="dabb9-114">모델</span><span class="sxs-lookup"><span data-stu-id="dabb9-114">The model</span></span>

<span data-ttu-id="dabb9-115">간단한 클래스 쌍으로 블로그 및 게시물을 사용 하 여 Code First DataAnnotations을 보여 드리겠습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-115">I’ll demonstrate Code First DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
    public class Blog
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }

    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public DateTime DateCreated { get; set; }
        public string Content { get; set; }
        public int BlogId { get; set; }
        public ICollection<Comment> Comments { get; set; }
    }
```

<span data-ttu-id="dabb9-116">이와 같이 블로그 및 게시물 클래스는 코드 첫 번째 규칙을 편리 하 게 따르고 EF 호환성을 사용 하도록 조정 하지 않아도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-116">As they are, the Blog and Post classes conveniently follow code first convention and require no tweaks to enable EF compatability.</span></span> <span data-ttu-id="dabb9-117">그러나 주석을 사용 하 여 클래스와 해당 클래스가 매핑되는 데이터베이스에 대 한 추가 정보를 EF에 제공할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-117">However, you can also use the annotations to provide more information to EF about the classes and the database to which they map.</span></span>

 

## <a name="key"></a><span data-ttu-id="dabb9-118">Key</span><span class="sxs-lookup"><span data-stu-id="dabb9-118">Key</span></span>

<span data-ttu-id="dabb9-119">Entity Framework는 엔터티 추적에 사용 되는 키 값이 있는 모든 엔터티에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-119">Entity Framework relies on every entity having a key value that is used for entity tracking.</span></span> <span data-ttu-id="dabb9-120">Code First의 한 가지 규칙은 암시적 키 속성입니다. Code First는 "Id" 라는 속성 또는 클래스 이름 및 "Id"의 조합 (예: "BlogId")을 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-120">One convention of Code First is implicit key properties; Code First will look for a property named “Id”, or a combination of class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="dabb9-121">이 속성은 데이터베이스의 기본 키 열에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-121">This property will map to a primary key column in the database.</span></span>

<span data-ttu-id="dabb9-122">블로그 및 게시물 클래스는 모두이 규칙을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-122">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="dabb9-123">그렇지 않으면 어떻게 되나요?</span><span class="sxs-lookup"><span data-stu-id="dabb9-123">What if they didn’t?</span></span> <span data-ttu-id="dabb9-124">블로그에서 이름이 *Primarytrackingkey* 대신 또는 *foo*를 사용 하는 경우</span><span class="sxs-lookup"><span data-stu-id="dabb9-124">What if Blog used the name *PrimaryTrackingKey* instead, or even *foo*?</span></span> <span data-ttu-id="dabb9-125">Code first가이 규칙과 일치 하는 속성을 찾지 못하면 키 속성이 있어야 Entity Framework 요구 사항으로 인해 예외가 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-125">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="dabb9-126">키 주석을 사용 하 여 EntityKey로 사용할 속성을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-126">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

``` csharp
    public class Blog
    {
        [Key]
        public int PrimaryTrackingKey { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }
```

<span data-ttu-id="dabb9-127">Code first의 데이터베이스 생성 기능을 사용 하는 경우 블로그 테이블에는 기본적으로 Id로 정의 되는 PrimaryTrackingKey 라는 기본 키 열이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-127">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey, which is also defined as Identity by default.</span></span>

![기본 키가 포함 된 블로그 테이블](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="dabb9-129">복합 키</span><span class="sxs-lookup"><span data-stu-id="dabb9-129">Composite keys</span></span>

<span data-ttu-id="dabb9-130">Entity Framework는 둘 이상의 속성으로 구성 된 복합 키-기본 키를 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-130">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="dabb9-131">예를 들어 기본 키가 PassportNumber 및 IssuingCountry의 조합 인 Passport 클래스가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-131">For example, you could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        public int PassportNumber { get; set; }
        [Key]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="dabb9-132">EF 모델에서 위의 클래스를 사용 하려고 하면 `InvalidOperationException`발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-132">Attempting to use the above class in your EF model would result in an `InvalidOperationException`:</span></span>

<span data-ttu-id="dabb9-133">*' Passport ' 형식의 복합 기본 키 순서를 확인할 수 없습니다. ColumnAttribute 또는 HasKey 메서드를 사용 하 여 복합 기본 키의 순서를 지정 합니다.*</span><span class="sxs-lookup"><span data-stu-id="dabb9-133">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="dabb9-134">복합 키를 사용 하려면 Entity Framework 키 속성의 순서를 정의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-134">In order to use composite keys, Entity Framework requires you to define an order for the key properties.</span></span> <span data-ttu-id="dabb9-135">열 주석을 사용 하 여 순서를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-135">You can do this by using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="dabb9-136">순서 값은 인덱스 기반이 아니라 상대적 이므로 값을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-136">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="dabb9-137">예를 들어 100 및 200은 1과 2 대신 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-137">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        [Column(Order=1)]
        public int PassportNumber { get; set; }
        [Key]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="dabb9-138">복합 외래 키가 있는 엔터티가 있는 경우 해당 하는 기본 키 속성에 사용한 것과 동일한 열 순서 지정을 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-138">If you have entities with composite foreign keys, then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="dabb9-139">외래 키 속성 내의 상대 정렬만 동일 해야 합니다. **Order** 에 할당 된 정확한 값은 일치할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-139">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="dabb9-140">예를 들어 다음 클래스에서 3과 4는 1과 2 대신 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-140">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

``` csharp
    public class PassportStamp
    {
        [Key]
        public int StampId { get; set; }
        public DateTime Stamped { get; set; }
        public string StampingCountry { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 1)]
        public int PassportNumber { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }

        public Passport Passport { get; set; }
    }
```

## <a name="required"></a><span data-ttu-id="dabb9-141">필수</span><span class="sxs-lookup"><span data-stu-id="dabb9-141">Required</span></span>

<span data-ttu-id="dabb9-142">필요한 주석은 EF에 특정 속성이 필요 함을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-142">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="dabb9-143">Required 속성에 Required를 추가 하면 EF (및 MVC)가 속성에 데이터를 포함 하는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-143">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="dabb9-144">응용 프로그램에서 코드 또는 태그를 추가로 변경 하지 않으면 MVC 응용 프로그램은 클라이언트 쪽 유효성 검사를 수행 하 고 속성 및 주석 이름을 사용 하 여 동적으로 메시지를 작성 하는 경우에도 마찬가지입니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-144">With no additional code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![제목이 있는 만들기 페이지가 필요 합니다. 오류](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="dabb9-146">필수 특성은 매핑된 속성이 null을 허용 하지 않도록 설정 하 여 생성 된 데이터베이스에도 영향을 줍니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-146">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="dabb9-147">제목 필드가 "not null"로 변경 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-147">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="dabb9-148">속성이 필요한 경우에도 데이터베이스의 열을 null을 허용 하지 않는 경우도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-148">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="dabb9-149">예를 들어 여러 형식에 대해 TPH 상속 전략 데이터를 사용 하는 경우 단일 테이블에 저장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-149">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="dabb9-150">파생 된 형식에 필수 속성이 포함 된 경우에는 해당 열을 null을 허용 하지 않는 형식으로 만들 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-150">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![블로그 테이블](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="dabb9-152">MaxLength 및 MinLength</span><span class="sxs-lookup"><span data-stu-id="dabb9-152">MaxLength and MinLength</span></span>

<span data-ttu-id="dabb9-153">MaxLength 및 MinLength 특성을 사용 하면 필요한 경우와 마찬가지로 추가 속성 유효성 검사를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-153">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="dabb9-154">다음은 길이 요구 사항이 있는 BloggerName입니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-154">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="dabb9-155">또한이 예제에서는 특성을 결합 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-155">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="dabb9-156">MaxLength 주석은 속성의 길이를 10으로 설정 하 여 데이터베이스에 영향을 줍니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-156">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![BloggerName 열의 최대 길이를 표시 하는 블로그 테이블](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="dabb9-158">MVC 클라이언트 쪽 주석 및 EF 4.1 서버 쪽 주석은이 유효성 검사를 모두 수행 하 고, 다시 동적으로 오류 메시지를 작성 합니다. "BloggerName 필드는 최대 길이가 ' 10 ' 인 문자열 또는 배열 형식 이어야 합니다." 이 메시지는 약간 깁니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-158">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="dabb9-159">많은 주석을 사용 하면 ErrorMessage 특성으로 오류 메시지를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-159">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="dabb9-160">필요한 주석에 ErrorMessage를 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-160">You can also specify ErrorMessage in the Required annotation.</span></span>

![사용자 지정 오류 메시지를 사용 하 여 페이지 만들기](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="dabb9-162">NotMapped</span><span class="sxs-lookup"><span data-stu-id="dabb9-162">NotMapped</span></span>

<span data-ttu-id="dabb9-163">Code first 규칙은 지원 되는 데이터 형식에 해당 하는 모든 속성이 데이터베이스에 표시 되도록 규정 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-163">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="dabb9-164">그러나 응용 프로그램에서 항상 그렇지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-164">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="dabb9-165">예를 들어 Title 및 BloggerName 필드를 기반으로 코드를 만드는 블로그 클래스의 속성이 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-165">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="dabb9-166">이 속성은 동적으로 만들 수 있으며 저장할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-166">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="dabb9-167">이 BlogCode 속성과 같이 NotMapped 않은 주석을 사용 하 여 데이터베이스에 매핑되지 않는 모든 속성을 표시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-167">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

``` csharp
    [NotMapped]
    public string BlogCode
    {
        get
        {
            return Title.Substring(0, 1) + ":" + BloggerName.Substring(0, 1);
        }
    }
```

 

## <a name="complextype"></a><span data-ttu-id="dabb9-168">ComplexType</span><span class="sxs-lookup"><span data-stu-id="dabb9-168">ComplexType</span></span>

<span data-ttu-id="dabb9-169">클래스 집합에서 도메인 엔터티를 설명한 다음 해당 클래스를 계층화 하 여 전체 엔터티를 설명 하는 것은 드문 일이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-169">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="dabb9-170">예를 들어 BlogDetails 라는 클래스를 모델에 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-170">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="dabb9-171">BlogDetails에는 어떤 유형의 키 속성도 없습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-171">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="dabb9-172">도메인 기반 디자인에서 BlogDetails를 값 개체 라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-172">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="dabb9-173">Entity Framework은 값 개체를 복합 형식으로 참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-173">Entity Framework refers to value objects as complex types.</span></span><span data-ttu-id="dabb9-174">  복합 형식은 자체적으로 추적할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-174">  Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="dabb9-175">그러나 블로그 클래스의 속성으로 BlogDetails는 블로그 개체의 일부로 추적 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-175">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="dabb9-176">Code first에서이를 인식할 수 있도록 하려면 BlogDetails 클래스를 ComplexType로 표시 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-176">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="dabb9-177">이제 블로그 클래스에서 속성을 추가 하 여 해당 블로그의 BlogDetails를 나타낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-177">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="dabb9-178">데이터베이스에서 블로그 표에는 BlogDetail 속성에 포함 된 속성을 포함 하 여 블로그의 모든 속성이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-178">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="dabb9-179">기본적으로 각 항목은 복합 유형인 BlogDetail의 이름 앞에와 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-179">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![복합 형식의 블로그 테이블](~/ef6/media/jj591583-figure06.png)


## <a name="concurrencycheck"></a><span data-ttu-id="dabb9-181">ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="dabb9-181">ConcurrencyCheck</span></span>

<span data-ttu-id="dabb9-182">ConcurrencyCheck 주석을 사용 하면 사용자가 엔터티를 편집 하거나 삭제할 때 데이터베이스에서 동시성 검사에 사용할 하나 이상의 속성에 플래그를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-182">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="dabb9-183">EF Designer를 사용 하 여 작업 한 경우 속성의 ConcurrencyMode을 Fixed로 설정 하 여 정렬 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-183">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="dabb9-184">ConcurrencyCheck을 BloggerName 속성에 추가 하 여 어떻게 작동 하는지 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-184">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="dabb9-185">SaveChanges가 호출 될 때 BloggerName 필드의 ConcurrencyCheck 주석으로 인해 해당 속성의 원래 값이 업데이트에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-185">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="dabb9-186">이 명령은 키 값 뿐만 아니라 BloggerName의 원래 값에 대해서만 필터링 하 여 올바른 행을 찾으려고 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-186">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span><span data-ttu-id="dabb9-187">  다음은 데이터베이스에 전송 되는 UPDATE 명령의 중요 한 부분입니다. 여기서 명령은 PrimaryTrackingKey가 1이 고 BloggerName가 데이터베이스에서 검색 되었을 때 원래 값인 "Julie" 인 행을 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-187">  Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="dabb9-188">다른 사람이 해당 블로그의 블로거 이름을 변경한 경우이 업데이트는 실패 하 고 처리 해야 하는 DbUpdateConcurrencyException을 받게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-188">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="dabb9-189">TimeStamp</span><span class="sxs-lookup"><span data-stu-id="dabb9-189">TimeStamp</span></span>

<span data-ttu-id="dabb9-190">동시성 검사를 위해 rowversion 또는 timestamp 필드를 사용 하는 것이 더 일반적입니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-190">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="dabb9-191">그러나 ConcurrencyCheck 주석을 사용 하는 대신 속성 형식이 바이트 배열인 경우 더 구체적인 타임 스탬프 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-191">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="dabb9-192">Code first는 타임 스탬프 속성을 ConcurrencyCheck 속성과 동일 하 게 처리 하지만 code first에서 생성 하는 데이터베이스 필드가 null을 허용 하지 않는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-192">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="dabb9-193">지정 된 클래스에는 타임 스탬프 속성을 하나만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-193">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="dabb9-194">블로그 클래스에 다음 속성을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-194">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="dabb9-195">code first에서 데이터베이스 테이블에 null을 허용 하지 않는 timestamp 열을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-195">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![타임 스탬프 열이 있는 블로그 테이블](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="dabb9-197">테이블 및 열</span><span class="sxs-lookup"><span data-stu-id="dabb9-197">Table and Column</span></span>

<span data-ttu-id="dabb9-198">데이터베이스 Code First 만들 수 있도록 하려면 만들고 있는 테이블 및 열의 이름을 변경 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-198">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="dabb9-199">기존 데이터베이스에서 Code First를 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-199">You can also use Code First with an existing database.</span></span> <span data-ttu-id="dabb9-200">그러나 도메인의 클래스 및 속성 이름이 데이터베이스의 테이블 및 열 이름과 일치 하지 않는 경우도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-200">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="dabb9-201">내 클래스의 이름은 블로그 이며 규칙에 따라 먼저 code first로 가정 하 고이는 블로그 라는 테이블에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-201">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="dabb9-202">그렇지 않은 경우 테이블 특성이 있는 테이블의 이름을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-202">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="dabb9-203">예를 들어 주석은 테이블 이름이 InternalBlogs 임을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-203">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="dabb9-204">열 주석은 매핑된 열의 특성을 지정 하는 것 보다 더 많은 어 뎁 트입니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-204">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="dabb9-205">테이블에서 이름, 데이터 형식 또는 열이 표시 되는 순서를 규정 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-205">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="dabb9-206">열 특성의 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-206">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column("BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="dabb9-207">열의 TypeName 특성과 DataType DataAnnotation을 혼동 하지 마세요.</span><span class="sxs-lookup"><span data-stu-id="dabb9-207">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="dabb9-208">DataType은 UI에 사용 되는 주석이 며 Code First에서 무시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-208">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="dabb9-209">다시 생성 된 후의 테이블은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-209">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="dabb9-210">테이블 이름이 InternalBlogs로 변경 되었으며 복합 형식의 설명 열은 이제 BlogDescription입니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-210">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="dabb9-211">주석에 이름이 지정 되었으므로 code first는 복합 형식의 이름으로 열 이름을 시작 하는 규칙을 사용 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-211">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![블로그 테이블 및 열 이름 바꾸기](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="dabb9-213">DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="dabb9-213">DatabaseGenerated</span></span>

<span data-ttu-id="dabb9-214">중요 한 데이터베이스 기능은 계산 된 속성을 가질 수 있는 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-214">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="dabb9-215">Code First 클래스를 계산 열이 포함 된 테이블에 매핑하는 경우에는 Entity Framework 해당 열을 업데이트 하려고 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-215">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="dabb9-216">그러나 데이터를 삽입 하거나 업데이트 한 후 EF가 데이터베이스에서 해당 값을 반환 하도록 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-216">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="dabb9-217">DatabaseGenerated 주석을 사용 하 여 계산 된 열거형과 함께 클래스의 해당 속성에 플래그를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-217">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="dabb9-218">다른 열거형은 None 및 Identity입니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-218">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="dabb9-219">Code first가 데이터베이스를 생성할 때 byte 또는 timestamp 열에 생성 된 데이터베이스를 사용할 수 있습니다. 그렇지 않으면 code first가 계산 열에 대 한 수식을 확인할 수 없기 때문에 기존 데이터베이스를 가리킬 때만이를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-219">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="dabb9-220">위에서 읽은 것은 기본적으로 정수 인 키 속성은 데이터베이스의 id 키가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-220">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="dabb9-221">이는 생성 된 DatabaseGenerated DatabaseGeneratedOption로 설정 하는 것과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-221">That would be the same as setting DatabaseGenerated to DatabaseGeneratedOption.Identity.</span></span> <span data-ttu-id="dabb9-222">Id 키가 되도록 하려면 값을 DatabaseGeneratedOption로 설정 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-222">If you do not want it to be an identity key, you can set the value to DatabaseGeneratedOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="dabb9-223">Index</span><span class="sxs-lookup"><span data-stu-id="dabb9-223">Index</span></span>

> [!NOTE]
> <span data-ttu-id="dabb9-224">**Ef 6.1** 이상-인덱스 특성이 Entity Framework 6.1에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-224">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="dabb9-225">이전 버전을 사용 하는 경우이 섹션의 정보는 적용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-225">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="dabb9-226">**Indexattribute**를 사용 하 여 하나 이상의 열에 인덱스를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-226">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="dabb9-227">하나 이상의 속성에 특성을 추가 하면 EF가 데이터베이스를 만들 때 데이터베이스에 해당 인덱스를 만들거나 Code First 마이그레이션를 사용 하는 경우 해당 **createindex** 호출을 스 캐 폴드 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-227">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="dabb9-228">예를 들어 다음 코드는 데이터베이스에 있는 **게시물** 테이블의 **등급** 열에 인덱스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-228">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index]
        public int Rating { get; set; }
        public int BlogId { get; set; }
    }
```

<span data-ttu-id="dabb9-229">기본적으로 인덱스는 **ix\_&lt;속성 이름&gt;** (위 예제의 Ix\_등급)으로 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-229">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="dabb9-230">하지만 인덱스의 이름을 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-230">You can also specify a name for the index though.</span></span> <span data-ttu-id="dabb9-231">다음 예에서는 인덱스 이름을 **PostRatingIndex**로 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-231">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="dabb9-232">기본적으로 인덱스는 고유 하지 않지만 **IsUnique** 명명 된 매개 변수를 사용 하 여 인덱스를 고유 하 게 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-232">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="dabb9-233">다음 예에서는 **사용자**의 로그인 이름에 대 한 고유 인덱스를 소개 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-233">The following example introduces a unique index on a **User**'s login name.</span></span>

``` csharp
    public class User
    {
        public int UserId { get; set; }

        [Index(IsUnique = true)]
        [StringLength(200)]
        public string Username { get; set; }

        public string DisplayName { get; set; }
    }
```

### <a name="multiple-column-indexes"></a><span data-ttu-id="dabb9-234">여러 열 인덱스</span><span class="sxs-lookup"><span data-stu-id="dabb9-234">Multiple-Column Indexes</span></span>

<span data-ttu-id="dabb9-235">여러 열에 걸쳐 있는 인덱스는 지정 된 테이블에 대 한 여러 인덱스 주석에 동일한 이름을 사용 하 여 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-235">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="dabb9-236">다중 열 인덱스를 만들 때는 인덱스의 열에 대 한 순서를 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-236">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="dabb9-237">예를 들어 다음 코드는 **IX\_BlogIdAndRating**이라는 **등급** 및 **BlogId** 에 대 한 다중 열 인덱스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-237">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogIdAndRating**.</span></span> <span data-ttu-id="dabb9-238">**BlogId** 는 인덱스의 첫 번째 열이 고 **등급** 은 second입니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-238">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index("IX_BlogIdAndRating", 2)]
        public int Rating { get; set; }
        [Index("IX_BlogIdAndRating", 1)]
        public int BlogId { get; set; }
    }
```

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="dabb9-239">관계 특성: InverseProperty 및 ForeignKey</span><span class="sxs-lookup"><span data-stu-id="dabb9-239">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="dabb9-240">이 페이지에서는 데이터 주석을 사용 하 여 Code First 모델에서 관계를 설정 하는 방법에 대 한 정보를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-240">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="dabb9-241">EF의 관계 및 관계를 사용 하 여 데이터에 액세스 하 고 조작 하는 방법에 대 한 일반적인 내용은 [관계 & 탐색 속성](~/ef6/fundamentals/relationships.md)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="dabb9-241">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="dabb9-242">Code first 규칙은 모델에서 가장 일반적으로 사용 되는 관계를 처리 하지만 도움이 필요한 경우도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-242">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="dabb9-243">블로그 클래스의 키 속성 이름을 변경 하면 Post와의 관계에 문제가 발생 했습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-243">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="dabb9-244">데이터베이스를 생성할 때 code first는 Post 클래스에서 BlogId 속성을 확인 하 고 클래스 이름과 "Id"와 일치 하는 규칙에 따라 블로그 클래스에 대 한 외래 키로이 속성을 인식 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-244">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="dabb9-245">그러나 블로그 클래스에는 BlogId 속성이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-245">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="dabb9-246">이를 위한 솔루션은 Post에서 탐색 속성을 만들고 외래 DataAnnotation을 사용 하 여 코드에서 BlogId 속성을 사용 하 여 두 클래스 간의 관계를 만드는 방법 뿐만 아니라에서 제약 조건을 지정 하는 방법을 이해 하는 데 도움이 됩니다. 데이터.</span><span class="sxs-lookup"><span data-stu-id="dabb9-246">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

``` csharp
    public class Post
    {
            public int Id { get; set; }
            public string Title { get; set; }
            public DateTime DateCreated { get; set; }
            public string Content { get; set; }
            public int BlogId { get; set; }
            [ForeignKey("BlogId")]
            public Blog Blog { get; set; }
            public ICollection<Comment> Comments { get; set; }
    }
```

<span data-ttu-id="dabb9-247">데이터베이스의 제약 조건은 InternalBlogs. PrimaryTrackingKey 및 BlogId 간의 관계를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-247">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![InternalBlogs 관계. PrimaryTrackingKey 및 BlogId](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="dabb9-249">InverseProperty는 클래스 간에 여러 관계가 있는 경우에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-249">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="dabb9-250">Post 클래스에서는 블로그 게시물을 수정한 사람 뿐만 아니라 블로그 게시물을 편집한 사람을 추적 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-250">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="dabb9-251">다음은 Post 클래스에 대 한 두 가지 새로운 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-251">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="dabb9-252">또한 이러한 속성이 참조 하는 Person 클래스에를 추가 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-252">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="dabb9-253">Person 클래스에는 사람이 기록한 모든 게시물에 대 한 탐색 속성과 해당 사용자가 업데이트 한 모든 게시물에 대 한 탐색 속성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-253">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="dabb9-254">Code first는 두 클래스의 속성을 고유 하 게 일치 시킬 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-254">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="dabb9-255">게시물에 대 한 데이터베이스 테이블에는 CreatedBy person에 대 한 하나의 외래 키와 UpdatedBy person에 대 한 외래 키가 있어야 하지만 code first는 Person\_Id, Person\_Id1, CreatedBy\_Id 및 UpdatedBy\_Id 라는 4 개의 외래 키 속성을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-255">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![추가 외래 키가 있는 테이블 게시](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="dabb9-257">이러한 문제를 해결 하기 위해 InverseProperty 주석을 사용 하 여 속성의 맞춤을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-257">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="dabb9-258">사용자의 PostsWritten 속성은이가 Post 형식을 참조 한다는 것을 알고 있으므로 CreatedBy에 대 한 관계를 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-258">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="dabb9-259">마찬가지로 PostUpdatedBy에 연결 됩니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-259">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="dabb9-260">그리고 code first는 추가 외래 키를 만들지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-260">And code first will not create the extra foreign keys.</span></span>

![추가 외래 키가 없는 게시물 테이블](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="dabb9-262">요약</span><span class="sxs-lookup"><span data-stu-id="dabb9-262">Summary</span></span>

<span data-ttu-id="dabb9-263">DataAnnotations을 사용 하면 코드의 첫 번째 클래스에서 클라이언트 및 서버 쪽 유효성 검사를 설명할 수 있을 뿐만 아니라 해당 규칙에 따라 코드에서 가장 먼저 수행 하는 가정을 개선 하 고 수정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-263">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="dabb9-264">DataAnnotations을 사용 하면 데이터베이스 스키마를 생성할 수 있을 뿐만 아니라 code first 클래스를 기존 데이터베이스에 매핑할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-264">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="dabb9-265">데이터 주석은 매우 유연 하지만, 코드 첫 번째 클래스에서 수행할 수 있는 가장 일반적으로 필요한 구성 변경만 제공 한다는 점에 유의 하세요.</span><span class="sxs-lookup"><span data-stu-id="dabb9-265">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="dabb9-266">일부 edge 사례에 대 한 클래스를 구성 하려면 대체 구성 메커니즘 Code First의 흐름 API를 확인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="dabb9-266">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
