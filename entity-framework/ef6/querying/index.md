---
title: 엔터티 쿼리 및 찾기 - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
caps.latest.revision: 3
ms.openlocfilehash: 92467e1a93f576eca627cf7b7d2351054a882c2c
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2018
ms.locfileid: "39067549"
---
# <a name="querying-and-finding-entities"></a>엔터티 쿼리 및 찾기
이 토픽에서는 LINQ 및 Find 메서드를 포함하여 Entity Framework를 사용하여 데이터를 쿼리할 수 있는 다양한 방법을 설명합니다. 이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

## <a name="finding-entities-using-a-query"></a>쿼리를 사용하여 엔터티 찾기  

DbSet 및 IDbSet는 IQueryable을 구현하므로 데이터베이스에 대한 LINQ 쿼리를 작성하는 시작점으로 사용할 수 있습니다. 지금은 LINQ에 대해 자세히 알아보는 대신, 몇 가지 간단한 예제만 살펴보겠습니다.  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs with names starting with B
    var blogs = from b in context.Blogs
                   where b.Name.StartsWith("B")
                   select b;

    // Query for the Blog named ADO.NET Blog
    var blog = context.Blogs
                    .Where(b => b.Name == "ADO.NET Blog")
                    .FirstOrDefault();
}
```  

DbSet IDbSet는 항상 데이터베이스에 대한 쿼리 만들며 반환된 엔터티가 이미 컨텍스트에 있더라도 항상 데이터베이스에 대한 왕복이 포함됩니다. 다음과 같은 경우 데이터베이스에 대해 쿼리가 실행됩니다.  

- **foreach**(C#) 또는 **For Each**(Visual Basic) 문에 의해 열거된 경우.  
- [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary) 또는 [ToList](https://msdn.microsoft.com/library/bb342261)와 같은 컬렉션 작업에 의해 열거된 경우.  
- 쿼리의 가장 바깥쪽 부분에 [First](https://msdn.microsoft.com/library/bb291976) 또는 [Any](https://msdn.microsoft.com/library/bb337697) 같은 LINQ 연산자가 지정된 경우.  
- DbSet의 [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) 확장 메서드, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx) 메서드 및 Database.ExecuteSqlCommand 메서드가 호출됩니다.  

데이터베이스에서 결과가 반환되면 컨텍스트에 없는 개체는 컨텍스트에 연결됩니다. 개체가 이미 컨텍스트에 있는 경우 기존 개체가 반환됩니다(항목 개체 속성의 현재 값과 원래 값을 데이터베이스 값으로 덮어쓰지 **않습니다**).  

쿼리를 수행하면 컨텍스트에 추가되었지만 아직 데이터베이스에 저장되지 않은 엔터티는 결과 집합의 일부로 반환되지 않습니다. 컨텍스트에 있는 데이터를 가져오는 방법은 [로컬 데이터](~/ef6/querying/local-data.md)를 참조하세요.  

쿼리가 데이터베이스의 어떤 행도 반환하지 않는 경우 결과는 **null**이 아니라 빈 컬렉션입니다.  

## <a name="finding-entities-using-primary-keys"></a>기본 키를 사용하여 엔터티 찾기  

DbSet의 Find 메서드는 기본 키 값을 사용하여 컨텍스트에서 추적하는 엔터티를 찾으려고 시도합니다. 컨텍스트에서 엔터티를 찾을 수 없는 경우 쿼리를 데이터베이스로 보내서 데이터베이스에서 엔터티를 찾습니다. 컨텍스트 또는 데이터베이스에 엔터티가 없으면 null이 반환됩니다.  

Find는 다음과 같은 두 가지 측면에서 쿼리 사용과 큰 차이가 있습니다.  

- 지정된 키가 있는 엔터티를 컨텍스트에서 찾을 수 없는 경우에만 데이터베이스에 대한 왕복이 만들어집니다.  
- Find는 추가됨 상태인 엔터티를 반환합니다. 즉, Find는 컨텍스트에 추가되었지만 아직 데이터베이스에 저장되지 않은 엔터티를 반환합니다.  
### <a name="finding-an-entity-by-primary-key"></a>기본 키로 엔터티 찾기  

다음 코드는 몇 가지 Find 사용 사례를 보여줍니다.  

``` csharp
using (var context = new BloggingContext())
{
    // Will hit the database
    var blog = context.Blogs.Find(3);

    // Will return the same instance without hitting the database
    var blogAgain = context.Blogs.Find(3);

    context.Blogs.Add(new Blog { Id = -1 });

    // Will find the new blog even though it does not exist in the database
    var newBlog = context.Blogs.Find(-1);

    // Will find a User which has a string primary key
    var user = context.Users.Find("johndoe1987");
}
```  

### <a name="finding-an-entity-by-composite-primary-key"></a>복합 기본 키로 엔터티 찾기  

Entity Framework는 엔터티에 두 개 이상의 속성으로 구성되는 복합 키를 허용합니다. 예를 들어 특정 블로그에 대한 사용자 설정을 나타내는 BlogSettings 엔터티가 있을 수 있습니다. 사용자는 BlogId 및 Username의 조합을 BlogSettings 기본 키로 만들기 위해 선택할 수 있는 블로그마다 오직 하나의 BlogSettings만 갖기 때문입니다. 다음 코드는 BlogId = 3이고 Username = "johndoe1987"인 BlogSettings를 찾으려고 시도합니다.  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

복합 키가 있는 경우 ColumnAttribute 또는 흐름 API를 사용하여 복합 키 속성의 순서를 지정해야 합니다. 키를 구성하는 값을 지정할 때 Find 호출에서 이 순서를 사용해야 합니다.  
