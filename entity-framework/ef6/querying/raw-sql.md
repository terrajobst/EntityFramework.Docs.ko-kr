---
title: 원시 SQL 쿼리-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 99893ca1c634ce6f2e4cf9dcb70b1a1e43532c60
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995736"
---
# <a name="raw-sql-queries"></a>원시 SQL 쿼리
Entity Framework를 사용 하면 LINQ를 사용 하 여 엔터티 클래스를 사용 하 여 쿼리할 수 있습니다. 그러나 데이터베이스에 대해 직접 원시 SQL을 사용 하 여 쿼리를 실행 하려는 경우가 있을 수 있습니다. 여기에 현재 저장된 프로시저 매핑을 지원 하지 않는 Code First 모델에 대 한 도움이 될 수 있는 저장된 프로시저를 호출 합니다. 이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

## <a name="writing-sql-queries-for-entities"></a>엔터티에 대 한 SQL 쿼리를 작성합니다.  

DbSet SqlQuery 메서드를 쓸 엔터티 인스턴스를 반환할 원시 SQL 쿼리를 허용 합니다. LINQ 쿼리에 의해 반환 된 경우 됩니다 것 처럼 반환 되는 개체 컨텍스트에 의해 추적 됩니다. 예를 들어:  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

LINQ 쿼리, 마찬가지로 쿼리가 실행 되지 않는 결과 열거 될 때까지 참고-위의 예제에서 이렇게 ToList 호출 합니다.  

원시 SQL 쿼리는 두 가지 이유로 작성 될 때마다 주의 해야 합니다. 먼저 쿼리는 실제로 요청 된 형식의 엔터티를만 반환 되도록 작성 되어야 합니다. 예를 들어, 상속 등의 기능을 사용 하는 경우 쉽습니다 CLR 형식이 잘못 된 엔터티를 통해 생성 되는 쿼리를 작성 하려면.  

둘째, 일부 유형의 원시 SQL 쿼리는 SQL 삽입 공격 특히 잠재적인 보안 위험을 노출합니다. 이러한 공격 방지 하기 위해 올바른 방법으로 쿼리에 매개 변수를 사용 해야 합니다.  

### <a name="loading-entities-from-stored-procedures"></a>저장된 프로시저에서 엔터티를 로드합니다.  

저장된 프로시저 결과에서 엔터티를 로드 하려면 DbSet.SqlQuery를 사용할 수 있습니다. 예를 들어, 다음 코드를 dbo를 호출합니다. 데이터베이스의 GetBlogs 프로시저:  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

또한 다음 구문을 사용 하 여 저장된 프로시저에 매개 변수를 전달할 수 있습니다.  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a>비 엔터티 형식에 대 한 SQL 쿼리를 작성합니다.  

SqlQuery 메서드를 사용 하 여 데이터베이스 클래스의 기본 형식을 비롯 한 모든 형식의 인스턴스를 반환 하 여 SQL 쿼리를 만들 수 있습니다. 예를 들어:  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

SqlQuery 데이터베이스에서 반환 된 결과 개체는 엔터티 형식의 인스턴스인 경우에 컨텍스트에 의해 추적 되지 않습니다.  

## <a name="sending-raw-commands-to-the-database"></a>데이터베이스에 원시 명령 보내기  

쿼리가 아닌 명령 ExecuteSqlCommand 메서드를 사용 하 여 데이터베이스에서 데이터베이스에 보낼 수 있습니다. 예를 들어:  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

엔터티가 로드 되거나 데이터베이스에서 다시 로드 될 때까지 ExecuteSqlCommand를 사용 하 여 데이터베이스의 데이터에 대 한 변경 내용을 컨텍스트에 불투명는 note 합니다.  

### <a name="output-parameters"></a>출력 매개 변수  

출력 매개 변수를 사용 하는 경우 결과 완전히 읽을 때까지 해당 값은 사용할 수 없습니다. 내용은 DbDataReader의 기본 동작으로 인해 이것이 [DataReader를 사용 하 여 데이터 검색](http://go.microsoft.com/fwlink/?LinkID=398589) 대 한 자세한 내용은 합니다.  
