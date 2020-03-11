---
title: 원시 SQL 쿼리-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 168aee67186535bf2a50705e86bfc5a88147e369
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414472"
---
# <a name="raw-sql-queries"></a>원시 SQL 쿼리
Entity Framework를 사용 하면 엔터티 클래스와 함께 LINQ를 사용 하 여 쿼리할 수 있습니다. 그러나 원시 SQL을 사용 하 여 데이터베이스에 대해 직접 쿼리를 실행 하려는 경우가 있을 수 있습니다. 여기에는 저장 프로시저를 호출 하는 작업이 포함 됩니다 .이 프로시저는 현재 저장 프로시저에 대 한 매핑을 지원 하지 않는 Code First 모델에 유용할 수 있습니다. 이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

## <a name="writing-sql-queries-for-entities"></a>엔터티에 대 한 SQL 쿼리 작성  

DbSet의 SqlQuery 메서드는 엔터티 인스턴스를 반환 하는 원시 SQL 쿼리를 작성할 수 있도록 합니다. 반환 된 개체는 LINQ 쿼리에서 반환 되는 것과 마찬가지로 컨텍스트에 의해 추적 됩니다. 예를 들면 다음과 같습니다.  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

LINQ 쿼리와 마찬가지로 쿼리는 결과가 열거 될 때까지 실행 되지 않습니다. 위의 예제에서이 작업은 ToList를 호출 하 여 수행 됩니다.  

두 가지 이유로 원시 SQL 쿼리를 작성할 때마다 주의를 기울여야 합니다. 먼저 요청 된 형식의 엔터티만 반환 되도록 쿼리를 작성 해야 합니다. 예를 들어 상속과 같은 기능을 사용 하는 경우 잘못 된 CLR 형식의 엔터티를 만드는 쿼리를 작성 하는 것이 쉽습니다.  

둘째, 일부 원시 SQL 쿼리 유형은 특히 SQL 삽입 공격과 관련 된 잠재적인 보안 위험을 노출 합니다. 이러한 공격 으로부터 보호 하기 위해 올바른 방법으로 쿼리에서 매개 변수를 사용 해야 합니다.  

### <a name="loading-entities-from-stored-procedures"></a>저장 프로시저에서 엔터티 로드  

DbSet를 사용 하 여 저장 프로시저의 결과에서 엔터티를 로드할 수 있습니다. 예를 들어 다음 코드에서는 dbo를 호출 합니다. 데이터베이스의 GetBlogs 프로시저:  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

다음 구문을 사용 하 여 저장 프로시저에 매개 변수를 전달할 수도 있습니다.  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a>비 엔터티 형식에 대 한 SQL 쿼리 작성  

기본 형식을 포함 하 여 모든 형식의 인스턴스를 반환 하는 SQL 쿼리는 데이터베이스 클래스의 SqlQuery 메서드를 사용 하 여 만들 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

개체가 엔터티 형식의 인스턴스인 경우에도 데이터베이스의 SqlQuery에서 반환 된 결과는 컨텍스트에 의해 추적 되지 않습니다.  

## <a name="sending-raw-commands-to-the-database"></a>데이터베이스에 원시 명령 보내기  

데이터베이스의 ExecuteSqlCommand 메서드를 사용 하 여 쿼리가 아닌 명령을 데이터베이스로 보낼 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

ExecuteSqlCommand를 사용 하는 데이터베이스의 데이터 변경 내용은 데이터베이스에서 엔터티가 로드 되거나 다시 로드 될 때까지 컨텍스트에 불투명 합니다.  

### <a name="output-parameters"></a>출력 매개 변수  

출력 매개 변수를 사용 하는 경우 결과를 완전히 읽을 때까지 해당 값을 사용할 수 없습니다. 이는 DbDataReader의 기본 동작 때문입니다. 자세한 내용은 [DataReader를 사용 하 여 데이터 검색](https://go.microsoft.com/fwlink/?LinkID=398589) 을 참조 하세요.  
