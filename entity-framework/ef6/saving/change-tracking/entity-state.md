---
title: 엔터티 상태-EF6 사용
author: divega
ms.date: 2016-10-23
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: 1a415e7c6090365a66d58fc9a2dd3256984d7a8e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997949"
---
# <a name="working-with-entity-states"></a>엔터티 상태를 사용 하 여 작업
추가 및 컨텍스트에 엔터티를 연결 하는 방법 및 Entity Framework SaveChanges 하는 동안 이러한을 처리 하는 방법을 설명 합니다.
Entity Framework에 상태의 엔터티가 컨텍스트에 연결 되어 있지만 연결이 끊긴 또는 N 계층 시나리오에서는 EF 엔터티 상태를 알 수 있어야 하는 추적을 처리 합니다.
이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

## <a name="entity-states-and-savechanges"></a>엔터티 상태 및 SaveChanges

엔터티의는 EntityState 열거에 의해 정의 된 대로 5 상태 중 하나일 수 있습니다. 이러한 상태는:  

- 추가: 엔터티가 컨텍스트에서 추적 되 고 있지만 아직 데이터베이스에 존재 하지 않습니다.  
- 변경 되지 않습니다: 엔터티 컨텍스트에 의해 추적 되 고 데이터베이스에 존재 하 고 데이터베이스의 값에서 해당 속성 값이 변경 되지 않았습니다.  
- 수정: 엔터티 컨텍스트에 의해 추적 되 고 데이터베이스에 존재 하 고 일부 또는 모든 속성 값이 수정 되었습니다.  
- 삭제 됨: 엔터티 컨텍스트에 의해 추적 되 고 데이터베이스에 있지만 다음에 SaveChanges가 호출 될 데이터베이스에서 삭제 되도록 표시 되었습니다.  
- 분리 된: 엔터티 추적 중이지 않은 컨텍스트에서  

SaveChanges 다양 한 상태의 엔터티에 대 한 여러 가지를 수행합니다.  

- SaveChanges에서 변경 되지 않은 엔터티를 검사 하지 않습니다. 업데이트 변경 되지 않은 상태의 엔터티에 대 한 데이터베이스에 전송 되지 않습니다.  
- 추가 엔터티를 데이터베이스에 삽입 되 고 후 될 때 변경 되지 않기 SaveChanges를 반환 합니다.  
- 수정 된 엔터티의 데이터베이스에서 업데이트 되 고 후 될 때 변경 되지 않기 SaveChanges를 반환 합니다.  
- 삭제 된 엔터티는 데이터베이스에서 삭제 되 고 컨텍스트에서 분리 됩니다.  

다음 예제에서는 엔터티 또는 엔터티 그래프의 상태를 변경할 수 있습니다 하는 방법을 보여 줍니다.  

## <a name="adding-a-new-entity-to-the-context"></a>새 엔터티를 컨텍스트에 추가  

DbSet에 Add 메서드를 호출 하 여 새 엔터티를 컨텍스트에 추가할 수 있습니다.
추가 엔터티를 Added 상태로 삽입 되도록 데이터베이스에 다음에 SaveChanges가 호출 될 때를 의미 합니다.
예를 들어:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

새 엔터티를 컨텍스트에 추가 하는 다른 방법은 Added로 상태를 변경할 것입니다. 예를 들어:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

마지막으로, 다른 엔터티를 이미 추적 중인 최대 연결 하 여 새 엔터티를 컨텍스트에 추가할 수 있습니다.
다른 엔터티의 컬렉션 탐색 속성에 새 엔터티를 추가 하거나 새 엔터티를 가리키도록 다른 엔터티의 참조 탐색 속성을 설정 하 여 수 있습니다. 예를 들어:  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    var blog = context.Blogs.Find(2);
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

모든 엔터티에 추가 되지 않은 다른 엔터티에 대 한 참조 있으면 이러한 예제에 대 한 이러한 새 엔터티를 아직 추적 다음 컨텍스트에 추가 됩니다 않으며 다음에 SaveChanges가 호출 될 때 데이터베이스에 삽입 됩니다.  

## <a name="attaching-an-existing-entity-to-the-context"></a>기존 엔터티를 컨텍스트에 연결  

있는 경우 이미 알고 있는 엔터티가 에서만 데이터베이스는 현재 추적 중이지 않은 컨텍스트에서 DbSet에서 Attach 메서드를 사용 하 여 엔터티를 추적 하는 컨텍스트를 지정할 수 있습니다. 엔터티가 Unchanged 상태로 컨텍스트에 됩니다. 예를 들어:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

내용이 적용 될 데이터베이스에 연결 된 엔터티를 조작 하는 다른 수행 하지 않고 SaveChanges가 호출 될 note 합니다. 엔터티가 Unchanged 상태로 때문입니다.  

기존 엔터티를 컨텍스트에 연결 하는 다른 방법은 Unchanged로 상태를 변경할 것입니다. 예를 들어:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

두이 예제 모두에 대 한 연결 되 고 있는 엔터티에 아직 추적 되지 않는 다른 엔터티에 대 한 참조 하는 경우 이러한 새 엔터티는 또한 연결 Unchanged 상태로 컨텍스트에 참고 합니다.  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a>기존 하지만 수정 된 엔터티를 컨텍스트에 연결 합니다.  

있는 경우 이미 알고 있는 엔터티의 있지만 데이터베이스에 변경 내용의 되었을 엔터티를 연결 하 고 해당 상태를 Modified로 설정 하는 컨텍스트를 지정할 수 있습니다.
예를 들어:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

수정 된 상태를 변경 하면 수정 된 엔터티의 모든 속성이 표시 됩니다 및 SaveChanges가 호출 될 때 모든 속성 값을이 데이터베이스로 전송 됩니다.  

연결 중인 엔터티가 아직 추적 되지 않는 다른 엔터티에 대 한 참조에 이러한 새 엔터티가 Unchanged 상태로 컨텍스트에 연결 됩니다 참고-이러한는 자동으로 만들지 수정 합니다.
수정 된 표시 해야 하는 여러 엔터티가 있는 경우 이러한 각 엔터티에 대 한 상태가 개별적으로 설정 해야 합니다.  

## <a name="changing-the-state-of-a-tracked-entity"></a>추적 된 엔터티의 상태를 변경합니다.  

해당 항목의 상태 속성을 설정 하 여 이미 추적 중인 엔터티의 상태를 변경할 수 있습니다. 예를 들어:  

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

이미 추적 된 엔터티에 대 한 추가 또는 연결 호출 사용할 수도 있습니다 엔터티 상태를 변경 하려면 note 합니다. 예를 들어, 현재 추가 상태에 있는 엔터티에 대 한 연결을 호출 Unchanged로 상태로 변경 됩니다.  

## <a name="insert-or-update-pattern"></a>삽입 또는 업데이트 패턴  

새 (따라서 데이터베이스 삽입)으로 엔터티를 추가 하거나 기존 엔터티를 연결 및 수정 (데이터베이스 업데이트의 결과)으로 표시 하려면 일부 응용 프로그램에 대 한 일반적인 패턴은 기본 키 값에 따라 합니다.
예를 들어, 데이터베이스에서 생성 된 정수 기본 키를 사용 하는 경우 새 0 키를 사용 하 여 엔터티 및 엔터티의 기존 0이 아닌 키를 사용 하 여 처리에 공통적으로 적용 됩니다.
기본 키 값의 검사에 따라 엔터티 상태를 설정 하 여이 패턴을 구현할 수 있습니다. 예를 들어:  

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

수정 된 상태를 변경 하면 수정 된 엔터티의 모든 속성이 표시 됩니다 및 SaveChanges가 호출 될 때 데이터베이스에 모든 속성 값을 보낼 수는 note 합니다.  
