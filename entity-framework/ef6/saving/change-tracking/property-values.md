---
title: 속성 값 작업-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
ms.openlocfilehash: d8a18182754980d79b71df3f227b30c4ce40366f
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182139"
---
# <a name="working-with-property-values"></a>속성 값 사용
대부분 Entity Framework에서는 엔터티 인스턴스의 속성에 대 한 상태, 원래 값 및 현재 값을 추적 하는 작업을 처리 합니다. 그러나 연결 되지 않은 시나리오 (예: EF에 속성에 대 한 정보를 보거나 조작 하려는 경우)가 있을 수 있습니다. 이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

Entity Framework 추적 된 엔터티의 각 속성에 대 한 두 값을 추적 합니다. 현재 값은 이름으로, 엔터티에서 속성의 현재 값을 나타냅니다. 원래 값은 엔터티가 데이터베이스에서 쿼리 되거나 컨텍스트에 연결 되었을 때 속성에 포함 된 값입니다.  

속성 값으로 작업 하는 데는 다음과 같은 두 가지 일반적인 메커니즘이 있습니다.  

- 속성 메서드를 사용 하 여 강력한 형식의 방식으로 단일 속성의 값을 가져올 수 있습니다.  
- 엔터티의 모든 속성 값은 DbPropertyValues 개체로 읽을 수 있습니다. 그런 다음 DbPropertyValues는 사전 유사 개체 역할을 하 여 속성 값을 읽고 설정할 수 있도록 합니다. DbPropertyValues 개체의 값은 다른 DbPropertyValues 개체의 값 또는 다른 개체의 값 (예: 엔터티의 다른 복사본 또는 DTO (단순 데이터 전송 개체))에서 설정할 수 있습니다.  

다음 섹션에서는 위의 두 메커니즘을 사용 하는 예제를 보여 줍니다.  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a>개별 속성의 현재 값과 원래 값 가져오기 및 설정  

아래 예제에서는 속성의 현재 값을 읽은 다음 새 값으로 설정 하는 방법을 보여 줍니다.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(3);

    // Read the current value of the Name property
    string currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;

    // Set the Name property to a new value
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";

    // Read the current value of the Name property using a string for the property name
    object currentName2 = context.Entry(blog).Property("Name").CurrentValue;

    // Set the Name property to a new value using a string for the property name
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

CurrentValue 속성 대신 OriginalValue 속성을 사용 하 여 원래 값을 읽거나 설정 합니다.  

반환 된 값은 문자열을 사용 하 여 속성 이름을 지정 하는 경우 "object"로 형식화 됩니다. 반면에 람다 식을 사용 하는 경우 반환 되는 값은 강력한 형식입니다.  

속성과 같이 속성 값을 설정 하면 새 값이 이전 값과 다른 경우에만 속성을 수정 된 것으로 표시 합니다.  

이러한 방식으로 속성 값을 설정 하면 AutoDetectChanges가 해제 된 경우에도 변경 내용이 자동으로 검색 됩니다.  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a>매핑되지 않은 속성의 현재 값 가져오기 및 설정  

데이터베이스에 매핑되지 않은 속성의 현재 값을 읽을 수도 있습니다. 매핑되지 않은 속성의 예는 블로그의 RssLink 속성 일 수 있습니다. 이 값은 BlogId를 기반으로 계산 될 수 있으므로 데이터베이스에 저장할 필요가 없습니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.  

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

속성이 setter를 노출 하는 경우에도 현재 값을 설정할 수 있습니다.  

매핑되지 않은 속성의 값을 읽으면 매핑되지 않은 속성의 Entity Framework 유효성 검사를 수행할 때 유용 합니다. 동일한 이유로 현재 컨텍스트에 의해 추적 되 고 있지 않은 엔터티의 속성에 대해 현재 값을 읽고 설정할 수 있습니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.  

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

매핑되지 않은 속성이 나 컨텍스트에 의해 추적 되지 않는 엔터티의 속성에는 원래 값을 사용할 수 없습니다.  

## <a name="checking-whether-a-property-is-marked-as-modified"></a>속성이 수정 된 것으로 표시 되었는지 확인  

아래 예제에서는 개별 속성이 수정 된 것으로 표시 되는지 여부를 확인 하는 방법을 보여 줍니다.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

SaveChanges가 호출 되 면 수정 된 속성의 값이 데이터베이스에 대 한 업데이트로 전송 됩니다.  

##  <a name="marking-a-property-as-modified"></a>속성을 수정 된 것으로 표시  

아래 예제에서는 개별 속성이 수정 된 것으로 표시 되도록 하는 방법을 보여 줍니다.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

속성을 수정 된 것으로 표시 하면 속성의 현재 값이 원래 값과 동일한 경우에도 SaveChanges가 호출 될 때 속성의 데이터베이스에 업데이트가 전송 됩니다.  

현재 개별 속성이 수정 된 것으로 표시 된 후에는 수정 되지 않도록 다시 설정할 수 없습니다. 향후 릴리스에서 지원할 계획입니다.  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a>엔터티의 모든 속성에 대 한 현재, 원본 및 데이터베이스 값 읽기  

아래 예제에서는 엔터티의 모든 매핑된 속성에 대해 현재 값, 원래 값 및 실제로 데이터베이스에서 값을 읽는 방법을 보여 줍니다.  

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

현재 값은 엔터티의 속성이 현재 포함 하는 값입니다. 원래 값은 엔터티를 쿼리할 때 데이터베이스에서 읽은 값입니다. 데이터베이스 값은 현재 데이터베이스에 저장 된 값입니다. 데이터베이스 값 가져오기는 다른 사용자가 데이터베이스에 대 한 동시 편집을 수행한 경우와 같이 엔터티가 쿼리 된 후 데이터베이스의 값이 변경 된 경우에 유용 합니다.  

## <a name="setting-current-or-original-values-from-another-object"></a>다른 개체의 현재 값 또는 원래 값 설정  

추적 된 엔터티의 현재 값 또는 원래 값을 다른 개체의 값을 복사 하 여 업데이트할 수 있습니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.  

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

위의 코드를 실행 하면 다음과 같이 출력 됩니다.  

```console
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

이 기술은 서비스 호출 또는 n 계층 응용 프로그램의 클라이언트에서 가져온 값으로 엔터티를 업데이트할 때 사용 되는 경우가 있습니다. 엔터티의 이름과 이름이 일치 하는 속성이 있는 경우 사용 된 개체는 엔터티와 동일한 형식일 필요가 없습니다. 위의 예제에서 BlogDTO의 인스턴스는 원래 값을 업데이트 하는 데 사용 됩니다.  

다른 개체에서 복사 하는 경우 다른 값으로 설정 된 속성만 수정 된 것으로 표시 됩니다.  

## <a name="setting-current-or-original-values-from-a-dictionary"></a>사전에서 현재 값 또는 원래 값 설정  

추적 된 엔터티의 현재 값 또는 원래 값은 사전 또는 다른 데이터 구조에서 값을 복사 하 여 업데이트할 수 있습니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.  

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

CurrentValues 속성 대신 OriginalValues 속성을 사용 하 여 원래 값을 설정 합니다.  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a>속성을 사용 하 여 사전의 현재 값 또는 원래 값 설정  

위와 같이 CurrentValues 또는 OriginalValues를 사용 하는 대신 Property 메서드를 사용 하 여 각 속성의 값을 설정 합니다. 이는 복합 속성의 값을 설정 해야 하는 경우에 더 적합할 수 있습니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.  

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

위의 예에서 위의 복합 속성은 점으로 구분 된 이름을 사용 하 여 액세스 됩니다. 복잡 한 속성에 액세스 하는 다른 방법은이 항목의 뒷부분에 있는 두 섹션 (특히 복잡 한 속성)을 참조 하세요.  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a>현재, 원래 값 또는 데이터베이스 값을 포함 하는 복제 된 개체 만들기  

CurrentValues, OriginalValues 또는 GetDatabaseValues에서 반환 된 DbPropertyValues 개체를 사용 하 여 엔터티의 클론을 만들 수 있습니다. 이 복제본은이를 만드는 데 사용 된 DbPropertyValues 개체의 속성 값을 포함 합니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

반환 된 개체는 엔터티가 아니고 컨텍스트에 의해 추적 되지 않습니다. 반환 된 개체에도 다른 개체로 설정 된 관계가 없습니다.  

복제 된 개체는 데이터베이스에 대 한 동시 업데이트와 관련 된 문제를 해결 하는 데 유용할 수 있습니다. 특히 특정 형식의 개체에 대 한 데이터 바인딩을 포함 하는 UI를 사용 하는 경우가 여기에 해당 합니다.  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a>복합 속성의 현재 값과 원래 값 가져오기 및 설정  

전체 복합 개체의 값은 기본 속성의 경우와 마찬가지로 속성 메서드를 사용 하 여 읽고 설정할 수 있습니다. 또한 복잡 한 개체를 드릴 다운 하 고 해당 개체 또는 중첩 된 개체의 속성을 읽거나 설정할 수 있습니다. 다음은 몇 가지 예입니다.  

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

CurrentValue 속성 대신 OriginalValue 속성을 사용 하 여 원래 값을 가져오거나 설정 합니다.  

속성 또는 ComplexProperty 메서드를 사용 하 여 복잡 한 속성에 액세스할 수 있습니다. 그러나 추가 속성 또는 ComplexProperty 호출을 사용 하 여 복합 개체로 드릴 다운 하려는 경우에는 ComplexProperty 메서드를 사용 해야 합니다.  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a>DbPropertyValues를 사용 하 여 복잡 한 속성에 액세스  

CurrentValues, OriginalValues 또는 GetDatabaseValues를 사용 하 여 엔터티에 대 한 현재, 원래 또는 데이터베이스 값을 모두 가져오는 경우 모든 복합 속성의 값이 중첩 된 DbPropertyValues 개체로 반환 됩니다. 그러면 이러한 중첩 된 개체를 사용 하 여 복합 개체의 값을 가져올 수 있습니다. 예를 들어, 다음 메서드는 복합 속성 및 중첩 된 복합 속성의 값을 포함 하 여 모든 속성의 값을 출력 합니다.  

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

모든 현재 속성 값을 출력 하기 위해 메서드는 다음과 같이 호출 됩니다.  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
