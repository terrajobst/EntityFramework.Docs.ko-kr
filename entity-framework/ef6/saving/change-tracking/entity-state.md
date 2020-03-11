---
title: 엔터티 상태 작업-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: ef0e8d5a5a9d66adab7046088c49d8cd472edc8a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416254"
---
# <a name="working-with-entity-states"></a>엔터티 상태 작업
이 항목에서는 컨텍스트에 엔터티를 추가 하 고 연결 하는 방법 및 SaveChanges에서 엔터티를 Entity Framework 처리 하는 방법을 설명 합니다.
Entity Framework는 컨텍스트에 연결 된 상태에서 엔터티 상태를 추적 하지만 연결이 끊어졌거나 N 계층 시나리오에서는 사용자의 엔터티가 엔터티를 사용 해야 하는 상태를 알 수 있습니다.
이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

## <a name="entity-states-and-savechanges"></a>엔터티 상태 및 SaveChanges

엔터티는 EntityState 열거형에 정의 된 5 가지 상태 중 하나일 수 있습니다. 상태는 다음과 같습니다.  

- 추가 됨: 엔터티가 컨텍스트에 의해 추적 되지만 데이터베이스에는 아직 존재 하지 않습니다.  
- 변경 되지 않음: 엔터티가 컨텍스트에 의해 추적 되 고 데이터베이스에 있으며 해당 속성 값이 데이터베이스의 값에서 변경 되지 않았습니다.  
- 수정 됨: 엔터티가 컨텍스트에 의해 추적 되 고 데이터베이스에 있으며 해당 속성 값의 일부 또는 전체가 수정 되었습니다.  
- 삭제 됨: 엔터티가 컨텍스트에 의해 추적 되 고 데이터베이스에 존재 하지만 다음에 SaveChanges가 호출 될 때 데이터베이스에서 삭제 되도록 표시 되었습니다.  
- 분리 됨: 컨텍스트에서 엔터티가 추적 되 고 있지 않습니다.  

SaveChanges는 여러 상태의 엔터티에 대해 서로 다른 작업을 수행 합니다.  

- 변경 되지 않은 엔터티는 SaveChanges의 작업을 수행 하지 않습니다. 변경 되지 않은 상태의 엔터티에 대 한 업데이트는 데이터베이스로 전송 되지 않습니다.  
- 추가 된 엔터티는 데이터베이스에 삽입 된 다음 SaveChanges가 반환 될 때 변경 되지 않습니다.  
- 수정 된 엔터티는 데이터베이스에서 업데이트 된 다음 SaveChanges가 반환 될 때 변경 되지 않습니다.  
- 삭제 된 엔터티는 데이터베이스에서 삭제 된 다음 컨텍스트에서 분리 됩니다.  

다음 예제에서는 엔터티 또는 엔터티 그래프의 상태를 변경 하는 방법을 보여 줍니다.  

## <a name="adding-a-new-entity-to-the-context"></a>컨텍스트에 새 엔터티 추가  

DbSet에서 Add 메서드를 호출 하 여 컨텍스트에 새 엔터티를 추가할 수 있습니다.
이렇게 하면 엔터티가 추가 된 상태로 전환 됩니다. 즉, 다음에 SaveChanges가 호출 될 때 데이터베이스에 삽입 됩니다.
예를 들면 다음과 같습니다.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

컨텍스트에 새 엔터티를 추가 하는 또 다른 방법은 상태를 추가로 변경 하는 것입니다. 예를 들면 다음과 같습니다.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

마지막으로 이미 추적 중인 다른 엔터티에 후크 하 여 컨텍스트에 새 엔터티를 추가할 수 있습니다.
이는 다른 엔터티의 컬렉션 탐색 속성에 새 엔터티를 추가 하거나 새 엔터티를 가리키도록 다른 엔터티의 참조 탐색 속성을 설정 하 여 발생할 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

이러한 모든 예제에서 추가 중인 엔터티가 아직 추적 되지 않은 다른 엔터티에 대 한 참조를 포함 하는 경우 이러한 새 엔터티도 컨텍스트에 추가 되 고 다음에 SaveChanges가 호출 될 때 데이터베이스에 삽입 됩니다.  

## <a name="attaching-an-existing-entity-to-the-context"></a>컨텍스트에 기존 엔터티 연결  

데이터베이스에 이미 존재 하지만 현재 컨텍스트에 의해 추적 되 고 있지 않은 엔터티가 있는 경우 DbSet의 Attach 메서드를 사용 하 여 엔터티를 추적 하도록 컨텍스트에 지시할 수 있습니다. 엔터티는 컨텍스트에서 변경 되지 않은 상태가 됩니다. 예를 들면 다음과 같습니다.  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

연결 된 엔터티를 다른 조작 하지 않고 SaveChanges를 호출 하는 경우 데이터베이스에 변경 내용이 적용 되지 않습니다. 엔터티가 변경 되지 않은 상태에 있기 때문입니다.  

컨텍스트에 기존 엔터티를 연결 하는 또 다른 방법은 상태를 변경 되지 않은 상태로 변경 하는 것입니다. 예를 들면 다음과 같습니다.  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

연결 되는 엔터티가 아직 추적 되지 않은 다른 엔터티에 대 한 참조를 포함 하는 경우 이러한 두 가지 경우에도 이러한 새 엔터티는 변경 되지 않은 상태의 컨텍스트에 연결 됩니다.  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a>컨텍스트에 기존의 수정 된 엔터티 연결  

데이터베이스에 이미 존재 하는 엔터티가 있지만 변경 내용이 적용 된 엔터티가 있는 경우 엔터티를 연결 하 고 해당 상태를 수정 됨으로 설정 하도록 컨텍스트에 지시할 수 있습니다.
예를 들면 다음과 같습니다.  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

상태를 수정 됨으로 변경 하면 해당 엔터티의 모든 속성이 수정 된 것으로 표시 되 고 SaveChanges가 호출 될 때 모든 속성 값이 데이터베이스에 전송 됩니다.  

연결 중인 엔터티가 아직 추적 되지 않은 다른 엔터티에 대 한 참조를 포함 하는 경우 이러한 새 엔터티는 변경 되지 않은 상태로 컨텍스트에 연결 되며 자동으로 수정 되지 않습니다.
수정 된 것으로 표시 해야 하는 엔터티가 여러 개 있는 경우 이러한 각 엔터티에 대 한 상태를 개별적으로 설정 해야 합니다.  

## <a name="changing-the-state-of-a-tracked-entity"></a>추적 된 엔터티의 상태 변경  

항목의 상태 속성을 설정 하 여 이미 추적 중인 엔터티의 상태를 변경할 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

이미 추적 된 엔터티에 대 한 Add 또는 Attach 호출은 엔터티 상태를 변경 하는 데에도 사용할 수 있습니다. 예를 들어 현재 추가 된 상태에 있는 엔터티에 대해 Attach를 호출 하면 해당 상태가 변경 되지 않음으로 변경 됩니다.  

## <a name="insert-or-update-pattern"></a>Insert 또는 update 패턴  

일부 응용 프로그램의 일반적인 패턴은 엔터티를 새로 추가 하거나 (데이터베이스 삽입이 발생 하는 경우), 기본 키의 값에 따라 엔터티를 수정 된 것으로 표시 하는 것입니다 (데이터베이스 업데이트의 결과로 표시).
예를 들어 데이터베이스에서 생성 된 정수 기본 키를 사용 하는 경우 일반적으로 키가 0이 아닌 엔터티를 new로, 0이 아닌 키가 있는 엔터티를 기존으로 처리 하는 것이 일반적입니다.
이 패턴은 기본 키 값을 확인 하 여 엔터티 상태를 설정 하 여 달성할 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp
public void InsertOrUpdate(Blog blog)
{
    using (var context = new BloggingContext())
    {
        context.Entry(blog).State = blog.BlogId == 0 ?
                                   EntityState.Added :
                                   EntityState.Modified;

        context.SaveChanges();
    }
}
```  

상태를 수정 됨으로 변경 하면 해당 엔터티의 모든 속성이 수정 된 것으로 표시 되 고 SaveChanges가 호출 될 때 모든 속성 값이 데이터베이스에 전송 됩니다.  
