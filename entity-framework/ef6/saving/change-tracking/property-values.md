---
title: 속성 값-EF6 사용
author: divega
ms.date: 10/23/2016
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
ms.openlocfilehash: 97902021a671dea9854a365dc2f10eaecb9e5ab8
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488837"
---
# <a name="working-with-property-values"></a><span data-ttu-id="2c4ca-102">속성 값을 사용 하 여 작업</span><span class="sxs-lookup"><span data-stu-id="2c4ca-102">Working with property values</span></span>
<span data-ttu-id="2c4ca-103">대부분의 경우 Entity Framework 상태, 원래 값 및 엔터티 인스턴스 속성의 현재 값을 추적 하는 처리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-103">For the most part Entity Framework will take care of tracking the state, original values, and current values of the properties of your entity instances.</span></span> <span data-ttu-id="2c4ca-104">그러나 보거나 속성에 대 한 EF에서 정보 조작 하려는 연결이 끊긴된 시나리오와 같은 경우도 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-104">However, there may be some cases - such as disconnected scenarios - where you want to view or manipulate the information EF has about the properties.</span></span> <span data-ttu-id="2c4ca-105">이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="2c4ca-106">Entity Framework는 추적 된 엔터티의 각 속성에 대 한 두 값의 추적 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-106">Entity Framework keeps track of two values for each property of a tracked entity.</span></span> <span data-ttu-id="2c4ca-107">현재 값이를 이름으로 엔터티 속성의 현재 값입니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-107">The current value is, as the name indicates, the current value of the property in the entity.</span></span> <span data-ttu-id="2c4ca-108">원래 값에는 엔터티가 데이터베이스에서 쿼리 또는 컨텍스트에 연결 되었을 때 속성 있던 값이입니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-108">The original value is the value that the property had when the entity was queried from the database or attached to the context.</span></span>  

<span data-ttu-id="2c4ca-109">속성 값을 사용 하 여 작업에 대 한 일반 메커니즘 두 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-109">There are two general mechanisms for working with property values:</span></span>  

- <span data-ttu-id="2c4ca-110">강력한 형식의 방식으로 속성 메서드를 사용 하 여 단일 속성의 값을 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-110">The value of a single property can be obtained in a strongly typed way using the Property method.</span></span>  
- <span data-ttu-id="2c4ca-111">DbPropertyValues 개체로 엔터티의 모든 속성에 대 한 값을 읽을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-111">Values for all properties of an entity can be read into a DbPropertyValues object.</span></span> <span data-ttu-id="2c4ca-112">DbPropertyValues 속성 값을 읽고 설정할 수 있도록 비슷한 개체로 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-112">DbPropertyValues then acts as a dictionary-like object to allow property values to be read and set.</span></span> <span data-ttu-id="2c4ca-113">또 다른 엔터티 또는 단순 데이터 전송 개체 (DTO) 등의 다른 개체의 값 또는 다른 DbPropertyValues 개체의 값에서 DbPropertyValues 개체의 값을 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-113">The values in a DbPropertyValues object can be set from values in another DbPropertyValues object or from values in some other object, such as another copy of the entity or a simple data transfer object (DTO).</span></span>  

<span data-ttu-id="2c4ca-114">아래 섹션에서는 위의 메커니즘 중 두 가지 모두를 사용 하 여 예제를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-114">The sections below show examples of using both of the above mechanisms.</span></span>  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a><span data-ttu-id="2c4ca-115">가져오기 및 개별 속성의 현재 또는 원래 값 설정</span><span class="sxs-lookup"><span data-stu-id="2c4ca-115">Getting and setting the current or original value of an individual property</span></span>  

<span data-ttu-id="2c4ca-116">아래 예제에는 속성의 현재 값 수 읽기 및 새 값으로 설정 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-116">The example below shows how the current value of a property can be read and then set to a new value:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(3);

    // Read the current value of the Name property
    string currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;

    // Set the Name property to a new value
    context.Entry(name).Property(u => u.Name).CurrentValue = "My Fancy Blog";

    // Read the current value of the Name property using a string for the property name
    object currentName2 = context.Entry(blog).Property("Name").CurrentValue;

    // Set the Name property to a new value using a string for the property name
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

<span data-ttu-id="2c4ca-117">읽기 또는 원래 값을 설정 하려면 CurrentValue 속성 대신 OriginalValue 속성을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-117">Use the OriginalValue property instead of the CurrentValue property to read or set the original value.</span></span>  

<span data-ttu-id="2c4ca-118">문자열 속성 이름을 지정 하는 경우 반환된 된 값 "object"로 입력 됩니다는 참고 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-118">Note that the returned value is typed as “object” when a string is used to specify the property name.</span></span> <span data-ttu-id="2c4ca-119">반면에 반환 되는 값은 람다 식을 사용 하는 경우에 강력한 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-119">On the other hand, the returned value is strongly typed if a lambda expression is used.</span></span>  

<span data-ttu-id="2c4ca-120">새 값이 이전 값과 다른 경우 수정 된 것으로 다음과 같은 속성 값을 설정할 속성만 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-120">Setting the property value like this will only mark the property as modified if the new value is different from the old value.</span></span>  

<span data-ttu-id="2c4ca-121">속성 값을이 방식으로 설정 된 경우 AutoDetectChanges 해제 된 경우에 변경 내용은 자동으로 검색 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-121">When a property value is set in this way the change is automatically detected even if AutoDetectChanges is turned off.</span></span>  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a><span data-ttu-id="2c4ca-122">가져오기 및 매핑되지 않은 속성의 현재 값 설정</span><span class="sxs-lookup"><span data-stu-id="2c4ca-122">Getting and setting the current value of an unmapped property</span></span>  

<span data-ttu-id="2c4ca-123">데이터베이스에 매핑되지 않은 속성의 현재 값을 참조할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-123">The current value of a property that is not mapped to the database can also be read.</span></span> <span data-ttu-id="2c4ca-124">매핑되지 않은 속성의 예로 블로그의 RssLink 속성을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-124">An example of an unmapped property could be an RssLink property on Blog.</span></span> <span data-ttu-id="2c4ca-125">이 값 BlogId를 기준으로 계산 될 수 있습니다 및 데이터베이스에 저장할 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-125">This value may be calculated based on the BlogId, and therefore doesn't need to be stored in the database.</span></span> <span data-ttu-id="2c4ca-126">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="2c4ca-126">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    // Read the current value of an unmapped property
    var rssLink = context.Entry(blog).Property(p => p.RssLink).CurrentValue;

    // Use a string to specify the property name
    var rssLinkAgain = context.Entry(blog).Property("RssLink").CurrentValue;
}
```  

<span data-ttu-id="2c4ca-127">지정 된 속성 setter를 노출 하는 경우에 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-127">The current value can also be set if the property exposes a setter.</span></span>  

<span data-ttu-id="2c4ca-128">매핑되지 않은 속성의 값을 읽는 매핑되지 않은 속성의 Entity Framework 유효성 검사를 수행 하는 경우 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-128">Reading the values of unmapped properties is useful when performing Entity Framework validation of unmapped properties.</span></span> <span data-ttu-id="2c4ca-129">마찬가지 이유로 현재 값 수 읽거나 설정 하 고 현재 컨텍스트에 의해 추적 되지 않습니다 하는 엔터티의 속성에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-129">For the same reason current values can be read and set for properties of entities that are not currently being tracked by the context.</span></span> <span data-ttu-id="2c4ca-130">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="2c4ca-130">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Create an entity that is not being tracked
    var blog = new Blog { Name = "ADO.NET Blog" };

    // Read and set the current value of Name as before
    var currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";
    var currentName2 = context.Entry(blog).Property("Name").CurrentValue;
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

<span data-ttu-id="2c4ca-131">원래 값 제공 하지 않는 컨텍스트에서 추적 하는 엔터티의 속성 또는 매핑되지 않은 속성에 대 한 참고 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-131">Note that original values are not available for unmapped properties or for properties of entities that are not being tracked by the context.</span></span>  

## <a name="checking-whether-a-property-is-marked-as-modified"></a><span data-ttu-id="2c4ca-132">속성을 수정 된 것으로 표시 되는지 여부를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-132">Checking whether a property is marked as modified</span></span>  

<span data-ttu-id="2c4ca-133">아래 예제에서는 수정 된 것으로 개별 속성은 표시 여부를 확인 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-133">The example below shows how to check whether or not an individual property is marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

<span data-ttu-id="2c4ca-134">SaveChanges가 호출 될 때 데이터베이스에 수정 된 속성의 값으로 업데이트에 전송 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-134">The values of modified properties are sent as updates to the database when SaveChanges is called.</span></span>  

##  <a name="marking-a-property-as-modified"></a><span data-ttu-id="2c4ca-135">수정 된 속성을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-135">Marking a property as modified</span></span>  

<span data-ttu-id="2c4ca-136">아래 예제에서는 수정 된 것으로 표시할 개별 속성을 적용 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-136">The example below shows how to force an individual property to be marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

<span data-ttu-id="2c4ca-137">수에 대 한 업데이트를 데이터베이스로 보내는 속성에 대 한 속성의 현재 값이 원래 값과 같을 경우에 SaveChanges가 호출 될 때 수정 된 힘으로 속성을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-137">Marking a property as modified forces an update to be send to the database for the property when SaveChanges is called even if the current value of the property is the same as its original value.</span></span>  

<span data-ttu-id="2c4ca-138">수정 된 것으로 표시 된 후 수정 되지 개별 속성을 다시 설정 하려면 현재 불가능 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-138">It is not currently possible to reset an individual property to be not modified after it has been marked as modified.</span></span> <span data-ttu-id="2c4ca-139">이 향후 릴리스에서 지원할 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-139">This is something we plan to support in a future release.</span></span>  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a><span data-ttu-id="2c4ca-140">현재, 원래 값 및 엔터티의 모든 속성에 대 한 데이터베이스 값 읽기</span><span class="sxs-lookup"><span data-stu-id="2c4ca-140">Reading current, original, and database values for all properties of an entity</span></span>  

<span data-ttu-id="2c4ca-141">아래 예제에서는 현재 값, 원래 값 및 모든 매핑된 엔터티의 속성을 데이터베이스에 실제로 값을 읽는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-141">The example below shows how to read the current values, the original values, and the values actually in the database for all mapped properties of an entity.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Make a modification to Name in the tracked entity
    blog.Name = "My Cool Blog";

    // Make a modification to Name in the database
    context.Database.SqlCommand("update dbo.Blogs set Name = 'My Boring Blog' where Id = 1");

    // Print out current, original, and database values
    Console.WriteLine("Current values:");
    PrintValues(context.Entry(blog).CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(context.Entry(blog).OriginalValues);

    Console.WriteLine("\nDatabase values:");
    PrintValues(context.Entry(blog).GetDatabaseValues());
}

public static void PrintValues(DbPropertyValues values)
{
    foreach (var propertyName in values.PropertyNames)
    {
        Console.WriteLine("Property {0} has value {1}",
                          propertyName, values[propertyName]);
    }
}
```  

<span data-ttu-id="2c4ca-142">현재 값은 엔터티의 속성을 현재 포함 하는 값입니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-142">The current values are the values that the properties of the entity currently contain.</span></span> <span data-ttu-id="2c4ca-143">원래 값은 엔터티를 쿼리 하는 경우 데이터베이스에서 읽은 값입니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-143">The original values are the values that were read from the database when the entity was queried.</span></span> <span data-ttu-id="2c4ca-144">데이터베이스는 값은 값을 현재 데이터베이스에 저장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-144">The database values are the values as they are currently stored in the database.</span></span> <span data-ttu-id="2c4ca-145">데이터베이스 값을 가져오고이 엔터티는 데이터베이스를 다른 사용자가 했습니다를 동시에 편집 하는 경우와 같은 쿼리가 실행 된 이후 데이터베이스의 값 변경 될 수 있습니다 하는 경우 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-145">Getting the database values is useful when the values in the database may have changed since the entity was queried such as when a concurrent edit to the database has been made by another user.</span></span>  

## <a name="setting-current-or-original-values-from-another-object"></a><span data-ttu-id="2c4ca-146">다른 개체에서 현재 또는 원래 값을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-146">Setting current or original values from another object</span></span>  

<span data-ttu-id="2c4ca-147">다른 개체에서 값을 복사 하 여 추적 된 엔터티의 현재 또는 원래 값을 업데이트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-147">The current or original values of a tracked entity can be updated by copying values from another object.</span></span> <span data-ttu-id="2c4ca-148">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="2c4ca-148">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var coolBlog = new Blog { Id = 1, Name = "My Cool Blog" };
    var boringBlog = new BlogDto { Id = 1, Name = "My Boring Blog" };

    // Change the current and original values by copying the values from other objects
    var entry = context.Entry(blog);
    entry.CurrentValues.SetValues(coolBlog);
    entry.OriginalValues.SetValues(boringBlog);

    // Print out current and original values
    Console.WriteLine("Current values:");
    PrintValues(entry.CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(entry.OriginalValues);
}

public class BlogDto
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```  

<span data-ttu-id="2c4ca-149">위의 코드를 실행 출력 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-149">Running the code above will print out:</span></span>  

```  
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

<span data-ttu-id="2c4ca-150">N 계층 응용 프로그램에서 클라이언트 또는 서비스 호출에서 가져온 값을 사용 하 여 엔터티를 업데이트 하는 경우에 따라이 기술을 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-150">This technique is sometimes used when updating an entity with values obtained from a service call or a client in an n-tier application.</span></span> <span data-ttu-id="2c4ca-151">개체에 사용 되는 이름과 일치 하는 엔터티의 해당 속성이 하기만 엔터티와 동일한 형식 이어야 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-151">Note that the object used does not have to be of the same type as the entity so long as it has properties whose names match those of the entity.</span></span> <span data-ttu-id="2c4ca-152">위의 예에서 BlogDTO 인스턴스의 원래 값을 업데이트 하려면 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-152">In the example above, an instance of BlogDTO is used to update the original values.</span></span>  

<span data-ttu-id="2c4ca-153">다른 개체에서 복사 하는 경우 다른 값으로 설정 된 속성만 수정 된 것으로 표시 됩니다는 note 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-153">Note that only properties that are set to different values when copied from the other object will be marked as modified.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary"></a><span data-ttu-id="2c4ca-154">사전에서 현재 또는 원래 값을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-154">Setting current or original values from a dictionary</span></span>  

<span data-ttu-id="2c4ca-155">추적 된 엔터티의 현재 또는 원래 값은 사전 또는 일부 기타 데이터 구조에서 값을 복사 하 여 업데이트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-155">The current or original values of a tracked entity can be updated by copying values from a dictionary or some other data structure.</span></span> <span data-ttu-id="2c4ca-156">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="2c4ca-156">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "The New ADO.NET Blog" },
        { "Url", "blogs.msdn.com/adonet" },
    };

    var currentValues = context.Entry(blog).CurrentValues;

    foreach (var propertyName in newValues.Keys)
    {
        currentValues[propertyName] = newValues[propertyName];
    }

    PrintValues(currentValues);
}
```  

<span data-ttu-id="2c4ca-157">원래 값을 설정 하려면 CurrentValues 속성 대신 OriginalValues 속성을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-157">Use the OriginalValues property instead of the CurrentValues property to set original values.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a><span data-ttu-id="2c4ca-158">속성을 사용 하 여 사전에서 현재 또는 원래 값을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-158">Setting current or original values from a dictionary using Property</span></span>  

<span data-ttu-id="2c4ca-159">CurrentValues 또는 OriginalValues 위에 표시 된 대로 사용 하는 대신 각 속성의 값을 설정 하려면 속성 메서드를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-159">An alternative to using CurrentValues or OriginalValues as shown above is to use the Property method to set the value of each property.</span></span> <span data-ttu-id="2c4ca-160">이 복잡 한 속성의 값을 설정 해야 할 때 선호 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-160">This can be preferable when you need to set the values of complex properties.</span></span> <span data-ttu-id="2c4ca-161">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="2c4ca-161">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "John Doe" },
        { "Location.City", "Redmond" },
        { "Location.State.Name", "Washington" },
        { "Location.State.Code", "WA" },
    };

    var entry = context.Entry(user);

    foreach (var propertyName in newValues.Keys)
    {
        entry.Property(propertyName).CurrentValue = newValues[propertyName];
    }
}
```  

<span data-ttu-id="2c4ca-162">복합 속성을 위의 예에서 액세스 점으로 구분 된 이름을 사용 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-162">In the example above complex properties are accessed using dotted names.</span></span> <span data-ttu-id="2c4ca-163">복잡 한 속성에 액세스 하는 다른 방법에 대 한 복합 속성에 대 한 구체적으로이 항목의 뒷부분에 나오는 두 섹션을 참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-163">For other ways to access complex properties see the two sections later in this topic specifically about complex properties.</span></span>  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a><span data-ttu-id="2c4ca-164">현재, 원본 또는 데이터베이스 값을 포함 하는 복제 된 개체 만들기</span><span class="sxs-lookup"><span data-stu-id="2c4ca-164">Creating a cloned object containing current, original, or database values</span></span>  

<span data-ttu-id="2c4ca-165">CurrentValues, OriginalValues에서 반환 된 DbPropertyValues 개체 또는 GetDatabaseValues 엔터티의 클론을 만드는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-165">The DbPropertyValues object returned from CurrentValues, OriginalValues, or GetDatabaseValues can be used to create a clone of the entity.</span></span> <span data-ttu-id="2c4ca-166">이 클론 생성 시 사용한 DbPropertyValues 개체에서 속성 값이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-166">This clone will contain the property values from the DbPropertyValues object used to create it.</span></span> <span data-ttu-id="2c4ca-167">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="2c4ca-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

<span data-ttu-id="2c4ca-168">반환 된 개체는 엔터티 및 컨텍스트에 의해 추적 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-168">Note that the object returned is not the entity and is not being tracked by the context.</span></span> <span data-ttu-id="2c4ca-169">반환된 된 개체를 다른 개체에 설정 된 관계 없는 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-169">The returned object also does not have any relationships set to other objects.</span></span>  

<span data-ttu-id="2c4ca-170">복제 된 개체는 특히 있는 특정 유형의 개체에 대 한 데이터 바인딩을 포함 하는 UI를 사용 중인 데이터베이스에 대 한 동시 업데이트와 관련 된 문제를 확인 하는 데 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-170">The cloned object can be useful for resolving issues related to concurrent updates to the database, especially where a UI that involves data binding to objects of a certain type is being used.</span></span>  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a><span data-ttu-id="2c4ca-171">가져오기 및 복잡 한 속성의 현재 또는 원래 값을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-171">Getting and setting the current or original values of complex properties</span></span>  

<span data-ttu-id="2c4ca-172">전체 복잡 한 개체의 값은 읽고는 기본 속성에 대 한 것 처럼 속성 메서드를 사용 하 여 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-172">The value of an entire complex object can be read and set using the Property method just as it can be for a primitive property.</span></span> <span data-ttu-id="2c4ca-173">또한 복잡 한 개체 및 해당 개체의 중첩된 된 개체도 속성을 설정 하거나 읽거나 다운 드릴 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-173">In addition you can drill down into the complex object and read or set properties of that object, or even a nested object.</span></span> <span data-ttu-id="2c4ca-174">다음은 몇 가지 예입니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-174">Here are some examples:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    // Get the Location complex object
    var location = context.Entry(user)
                       .Property(u => u.Location)
                       .CurrentValue;

    // Get the nested State complex object using chained calls
    var state1 = context.Entry(user)
                     .ComplexProperty(u => u.Location)
                     .Property(l => l.State)
                     .CurrentValue;

    // Get the nested State complex object using a single lambda expression
    var state2 = context.Entry(user)
                     .Property(u => u.Location.State)
                     .CurrentValue;

    // Get the nested State complex object using a dotted string
    var state3 = context.Entry(user)
                     .Property("Location.State")
                     .CurrentValue;

    // Get the value of the Name property on the nested State complex object using chained calls
    var name1 = context.Entry(user)
                       .ComplexProperty(u => u.Location)
                       .ComplexProperty(l => l.State)
                       .Property(s => s.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a single lambda expression
    var name2 = context.Entry(user)
                       .Property(u => u.Location.State.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a dotted string
    var name3 = context.Entry(user)
                       .Property("Location.State.Name")
                       .CurrentValue;
}
```  

<span data-ttu-id="2c4ca-175">원래 값을 가져오거나 설정 하려면 CurrentValue 속성 대신 OriginalValue 속성을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-175">Use the OriginalValue property instead of the CurrentValue property to get or set an original value.</span></span>  

<span data-ttu-id="2c4ca-176">속성 또는 ComplexProperty 메서드를 사용할 수 있도록 속성에 액세스 하면 복잡 한 note 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-176">Note that either the Property or the ComplexProperty method can be used to access a complex property.</span></span> <span data-ttu-id="2c4ca-177">그러나 ComplexProperty 메서드를 사용 하 여 추가 속성을 사용 하 여 복잡 한 개체에 드릴 다운 하려는 경우 ComplexProperty 호출 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-177">However, the ComplexProperty method must be used if you wish to drill down into the complex object with additional Property or ComplexProperty calls.</span></span>  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a><span data-ttu-id="2c4ca-178">DbPropertyValues를 사용 하 여 복잡 한 속성에 액세스 하려면</span><span class="sxs-lookup"><span data-stu-id="2c4ca-178">Using DbPropertyValues to access complex properties</span></span>  

<span data-ttu-id="2c4ca-179">모든 현재 원래 가져오려는 CurrentValues, OriginalValues, 또는 GetDatabaseValues 사용 또는 경우 엔터티에 대 한 데이터베이스 값을 중첩 DbPropertyValues 개체로 복잡 한 속성의 값이 반환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-179">When you use CurrentValues, OriginalValues, or GetDatabaseValues to get all the current, original, or database values for an entity, the values of any complex properties are returned as nested DbPropertyValues objects.</span></span> <span data-ttu-id="2c4ca-180">이러한 개체 수를 중첩 된 다음 복합 개체의 값을 가져오는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-180">These nested objects can then be used to get values of the complex object.</span></span> <span data-ttu-id="2c4ca-181">예를 들어 다음 메서드를 복합 속성 및 중첩 된 복합 속성의 값을 포함 한 모든 속성의 값을 출력 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2c4ca-181">For example, the following method will print out the values of all properties, including values of any complex properties and nested complex properties.</span></span>  

``` csharp
public static void WritePropertyValues(string parentPropertyName, DbPropertyValues propertyValues)
{
    foreach (var propertyName in propertyValues.PropertyNames)
    {
        var nestedValues = propertyValues[propertyName] as DbPropertyValues;
        if (nestedValues != null)
        {
            WritePropertyValues(parentPropertyName + propertyName + ".", nestedValues);
        }
        else
        {
            Console.WriteLine("Property {0}{1} has value {2}",
                              parentPropertyName, propertyName,
                              propertyValues[propertyName]);
        }
    }
}
```  

<span data-ttu-id="2c4ca-182">메서드가 모든 현재 속성 값을 출력 하는 다음과 같이 호출할 수 있습니다:</span><span class="sxs-lookup"><span data-stu-id="2c4ca-182">To print out all current property values the method would be called like this:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
