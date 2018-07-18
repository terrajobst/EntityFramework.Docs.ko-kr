---
title: 속성 값-EF6 사용
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
caps.latest.revision: 3
ms.openlocfilehash: 07b71c9efe4e1fc3fd25a52c9cfb25f61e92f859
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121917"
---
# <a name="working-with-property-values"></a>속성 값을 사용 하 여 작업
대부분의 경우 Entity Framework 상태, 원래 값 및 엔터티 인스턴스 속성의 현재 값을 추적 하는 처리 됩니다. 그러나 보거나 속성에 대 한 EF에서 정보 조작 하려는 연결이 끊긴된 시나리오와 같은 경우도 있을 수 있습니다. 이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

Entity Framework는 추적 된 엔터티의 각 속성에 대 한 두 값의 추적 합니다. 현재 값이를 이름으로 엔터티 속성의 현재 값입니다. 원래 값에는 엔터티가 데이터베이스에서 쿼리 또는 컨텍스트에 연결 되었을 때 속성 있던 값이입니다.  

속성 값을 사용 하 여 작업에 대 한 일반 메커니즘 두 가지가 있습니다.  

- 강력한 형식의 방식으로 속성 메서드를 사용 하 여 단일 속성의 값을 가져올 수 있습니다.  
- DbPropertyValues 개체로 엔터티의 모든 속성에 대 한 값을 읽을 수 있습니다. DbPropertyValues 속성 값을 읽고 설정할 수 있도록 비슷한 개체로 사용 합니다. 또 다른 엔터티 또는 단순 데이터 전송 개체 (DTO) 등의 다른 개체의 값 또는 다른 DbPropertyValues 개체의 값에서 DbPropertyValues 개체의 값을 설정할 수 있습니다.  

아래 섹션에서는 위의 메커니즘 중 두 가지 모두를 사용 하 여 예제를 보여 줍니다.  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a>가져오기 및 개별 속성의 현재 또는 원래 값 설정  

아래 예제에는 속성의 현재 값 수 읽기 및 새 값으로 설정 하는 방법을 보여 줍니다.  

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

읽기 또는 원래 값을 설정 하려면 CurrentValue 속성 대신 OriginalValue 속성을 사용 합니다.  

문자열 속성 이름을 지정 하는 경우 반환된 된 값 "object"로 입력 됩니다는 참고 합니다. 반면에 반환 되는 값은 람다 식을 사용 하는 경우에 강력한 형식입니다.  

새 값이 이전 값과 다른 경우 수정 된 것으로 다음과 같은 속성 값을 설정할 속성만 표시 합니다.  

속성 값을이 방식으로 설정 된 경우 AutoDetectChanges 해제 된 경우에 변경 내용은 자동으로 검색 됩니다.  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a>가져오기 및 매핑되지 않은 속성의 현재 값 설정  

데이터베이스에 매핑되지 않은 속성의 현재 값을 참조할 수 있습니다. 매핑되지 않은 속성의 예로 블로그의 RssLink 속성을 수 있습니다. 이 값 BlogId를 기준으로 계산 될 수 있습니다 및 데이터베이스에 저장할 필요 하지 않습니다. 예를 들어:  

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

지정 된 속성 setter를 노출 하는 경우에 설정할 수 있습니다.  

매핑되지 않은 속성의 값을 읽는 매핑되지 않은 속성의 Entity Framework 유효성 검사를 수행 하는 경우 유용 합니다. 마찬가지 이유로 현재 값 수 읽거나 설정 하 고 현재 컨텍스트에 의해 추적 되지 않습니다 하는 엔터티의 속성에 대 한 합니다. 예를 들어:  

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

원래 값 제공 하지 않는 컨텍스트에서 추적 하는 엔터티의 속성 또는 매핑되지 않은 속성에 대 한 참고 합니다.  

## <a name="checking-whether-a-property-is-marked-as-modified"></a>속성을 수정 된 것으로 표시 되는지 여부를 확인 합니다.  

아래 예제에서는 수정 된 것으로 개별 속성은 표시 여부를 확인 하는 방법을 보여 줍니다.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

SaveChanges가 호출 될 때 데이터베이스에 수정 된 속성의 값으로 업데이트에 전송 됩니다.  

##  <a name="marking-a-property-as-modified"></a>수정 된 속성을 표시 합니다.  

아래 예제에서는 수정 된 것으로 표시할 개별 속성을 적용 하는 방법을 보여 줍니다.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

수에 대 한 업데이트를 데이터베이스로 보내는 속성에 대 한 속성의 현재 값이 원래 값과 같을 경우에 SaveChanges가 호출 될 때 수정 된 힘으로 속성을 표시 합니다.  

수정 된 것으로 표시 된 후 수정 되지 개별 속성을 다시 설정 하려면 현재 불가능 합니다. 이 향후 릴리스에서 지원할 예정입니다.  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a>현재, 원래 값 및 엔터티의 모든 속성에 대 한 데이터베이스 값 읽기  

아래 예제에서는 현재 값, 원래 값 및 모든 매핑된 엔터티의 속성을 데이터베이스에 실제로 값을 읽는 방법을 보여 줍니다.  

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

현재 값은 엔터티의 속성을 현재 포함 하는 값입니다. 원래 값은 엔터티를 쿼리 하는 경우 데이터베이스에서 읽은 값입니다. 데이터베이스는 값은 값을 현재 데이터베이스에 저장 됩니다. 데이터베이스 값을 가져오고이 엔터티는 데이터베이스를 다른 사용자가 했습니다를 동시에 편집 하는 경우와 같은 쿼리가 실행 된 이후 데이터베이스의 값 변경 될 수 있습니다 하는 경우 유용 합니다.  

## <a name="setting-current-or-original-values-from-another-object"></a>다른 개체에서 현재 또는 원래 값을 설정합니다.  

다른 개체에서 값을 복사 하 여 추적 된 엔터티의 현재 또는 원래 값을 업데이트할 수 있습니다. 예를 들어:  

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

위의 코드를 실행 출력 됩니다.  

```  
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

N 계층 응용 프로그램에서 클라이언트 또는 서비스 호출에서 가져온 값을 사용 하 여 엔터티를 업데이트 하는 경우에 따라이 기술을 사용 됩니다. 개체에 사용 되는 이름과 일치 하는 엔터티의 해당 속성이 하기만 엔터티와 동일한 형식 이어야 필요가 없습니다. 위의 예에서 BlogDTO 인스턴스의 원래 값을 업데이트 하려면 사용 됩니다.  

다른 개체에서 복사 하는 경우 다른 값으로 설정 된 속성만 수정 된 것으로 표시 됩니다는 note 합니다.  

## <a name="setting-current-or-original-values-from-a-dictionary"></a>사전에서 현재 또는 원래 값을 설정합니다.  

추적 된 엔터티의 현재 또는 원래 값은 사전 또는 일부 기타 데이터 구조에서 값을 복사 하 여 업데이트할 수 있습니다. 예를 들어:  

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

원래 값을 설정 하려면 CurrentValues 속성 대신 OriginalValues 속성을 사용 합니다.  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a>속성을 사용 하 여 사전에서 현재 또는 원래 값을 설정 합니다.  

CurrentValues 또는 OriginalValues 위에 표시 된 대로 사용 하는 대신 각 속성의 값을 설정 하려면 속성 메서드를 사용 하는 것입니다. 이 복잡 한 속성의 값을 설정 해야 할 때 선호 수 있습니다. 예를 들어:  

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

복합 속성을 위의 예에서 액세스 점으로 구분 된 이름을 사용 하 여 합니다. 복잡 한 속성에 액세스 하는 다른 방법에 대 한 복합 속성에 대 한 구체적으로이 항목의 뒷부분에 나오는 두 섹션을 참조 합니다.  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a>현재, 원본 또는 데이터베이스 값을 포함 하는 복제 된 개체 만들기  

CurrentValues, OriginalValues에서 반환 된 DbPropertyValues 개체 또는 GetDatabaseValues 엔터티의 클론을 만드는 데 사용할 수 있습니다. 이 클론 생성 시 사용한 DbPropertyValues 개체에서 속성 값이 포함 됩니다. 예를 들어:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

반환 된 개체는 엔터티 및 컨텍스트에 의해 추적 되지 않습니다. 반환된 된 개체를 다른 개체에 설정 된 관계 없는 합니다.  

복제 된 개체는 특히 있는 특정 유형의 개체에 대 한 데이터 바인딩을 포함 하는 UI를 사용 중인 데이터베이스에 대 한 동시 업데이트와 관련 된 문제를 확인 하는 데 유용할 수 있습니다.  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a>가져오기 및 복잡 한 속성의 현재 또는 원래 값을 설정 합니다.  

전체 복잡 한 개체의 값은 읽고는 기본 속성에 대 한 것 처럼 속성 메서드를 사용 하 여 설정할 수 있습니다. 또한 복잡 한 개체 및 해당 개체의 중첩된 된 개체도 속성을 설정 하거나 읽거나 다운 드릴 수 있습니다. 다음은 몇 가지 예입니다.  

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

원래 값을 가져오거나 설정 하려면 CurrentValue 속성 대신 OriginalValue 속성을 사용 합니다.  

속성 또는 ComplexProperty 메서드를 사용할 수 있도록 속성에 액세스 하면 복잡 한 note 합니다. 그러나 ComplexProperty 메서드를 사용 하 여 추가 속성을 사용 하 여 복잡 한 개체에 드릴 다운 하려는 경우 ComplexProperty 호출 되어야 합니다.  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a>DbPropertyValues를 사용 하 여 복잡 한 속성에 액세스 하려면  

모든 현재 원래 가져오려는 CurrentValues, OriginalValues, 또는 GetDatabaseValues 사용 또는 경우 엔터티에 대 한 데이터베이스 값을 중첩 DbPropertyValues 개체로 복잡 한 속성의 값이 반환 됩니다. 이러한 개체 수를 중첩 된 다음 복합 개체의 값을 가져오는 데 사용할 수 있습니다. 예를 들어 다음 메서드를 복합 속성 및 중첩 된 복합 속성의 값을 포함 한 모든 속성의 값을 출력 됩니다.  

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

메서드가 모든 현재 속성 값을 출력 하는 다음과 같이 호출할 수 있습니다:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
