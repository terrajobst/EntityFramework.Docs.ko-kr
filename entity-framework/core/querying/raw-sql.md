---
title: 원시 SQL 쿼리 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 3024c0101c9d886ef844d1b7dc85aaf1be27e86b
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58914080"
---
# <a name="raw-sql-queries"></a>원시 SQL 쿼리

Entity Framework Core를 사용하면 관계형 데이터베이스로 작업할 때 원시 SQL 쿼리로 드롭다운할 수 있습니다. 이는 수행하려는 쿼리를 LINQ를 사용하여 표현할 수 없거나 LINQ 쿼리를 사용하면 비효율적인 SQL이 발생하는 경우에 유용할 수 있습니다. 원시 SQL 쿼리는 엔터티 형식을 반환하거나 EF Core 2.1부터 모델에 포함된 [쿼리 형식](xref:core/modeling/query-types)을 반환할 수 있습니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)을 볼 수 있습니다.

## <a name="basic-raw-sql-queries"></a>기본 원시 SQL 쿼리

*FromSql* 확장 메서드를 사용하여 원시 SQL 쿼리를 기반으로 한 LINQ 쿼리를 시작할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

원시 SQL 쿼리를 사용하여 저장 프로시저를 실행할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>매개 변수 전달

SQL을 허용하는 API와 마찬가지로 사용자 입력을 매개 변수화하여 SQL 삽입 공격으로부터 보호해야 합니다. SQL 쿼리 문자열에 매개 변수 자리 표시자를 포함한 다음, 매개 변수 값을 추가 인수로 제공할 수 있습니다. 제공하는 매개 변수 값은 모두 `DbParameter`로 자동 변환됩니다.

다음 예제에서는 저장 프로시저에 단일 매개 변수를 전달합니다. `String.Format` 구문처럼 보일 수 있지만 제공된 값은 매개 변수로 래핑되고 생성된 매개 변수 이름은 `{0}` 자리 표시자가 지정된 위치에 삽입됩니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

동일한 쿼리이지만 문자열 보간 구문을 사용하면 EF Core 2.0 이상에서 지원됩니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

또한 DbParameter를 생성하고 매개 변수 값으로 제공할 수도 있습니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

이렇게 하면 SQL 쿼리 문자열에 명명된 매개 변수를 사용할 수 있으며, 이는 저장 프로시저가 선택적 매개 변수를 포함하는 경우 유용합니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>LINQ로 작성

SQL 쿼리를 데이터베이스에서 작성할 수 있는 경우 LINQ 연산자를 사용하여 초기 원시 SQL 쿼리의 맨 위에 작성할 수 있습니다. SQL 쿼리는 `SELECT` 키워드로 시작하여 작성할 수 있습니다.

다음 예제에서는 TVF(테이블 반환 함수)에서 선택한 다음, LINQ로 이를 작성하여 필터링 및 정렬을 수행하는 원시 SQL 쿼리를 사용합니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

## <a name="change-tracking"></a>변경 내용 추적

`FromSql()`을 사용하는 쿼리는 EF Core에서 다른 LINQ 쿼리와 완전히 동일한 변경 내용 추적 규칙을 따릅니다. 예를 들어 쿼리 프로젝트 엔터티 형식의 경우 기본적으로 결과가 추적됩니다.  

다음 예제에서는 TVF(테이블 반환 함수)에서 선택하는 원시 SQL 쿼리를 사용한 이후에 .AsNoTracking()을 호출하여 변경 내용 추적을 사용하지 않도록 설정합니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>관련 데이터 포함

`Include()` 메서드는 다른 LINQ 쿼리와 마찬가지로 관련 데이터를 포함하는 데 사용할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

## <a name="limitations"></a>제한 사항

원시 SQL 쿼리를 사용할 때는 몇 가지 제한 사항에 유의해야 합니다.

* SQL 쿼리는 엔터티 또는 쿼리 형식의 모든 속성에 대해 데이터를 반환해야 합니다.

* 결과 집합의 열 이름은 속성이 매핑되는 열 이름과 일치해야 합니다. 속성/열 매핑이 원시 SQL 쿼리에 대해 무시되고 결과 집합 열 이름이 속성 이름과 일치해야 한다는 점에서 EF6와 다릅니다.

* SQL 쿼리에 관련 데이터가 포함될 수 없습니다. 그러나 대부분의 경우 쿼리의 맨 위에서 `Include` 연산자를 사용하여 관련 데이터를 반환하도록 작성할 수 있습니다([관련 데이터 포함](#including-related-data) 참조).

* 이 메서드에 전달된 `SELECT` 문은 일반적으로 구성 가능해야 합니다. EF Core가 서버에서 추가 쿼리 연산자를 평가해야 하는 경우(예: `FromSql` 다음에 적용된 LINQ 연산자를 변환해야 하는 경우) 제공된 SQL은 하위 쿼리로 처리됩니다. 즉, 전달된 SQL에는 다음과 같이 하위 쿼리에서 유효하지 않은 문자나 옵션을 포함할 수 없습니다.
  * 후행 세미콜론
  * SQL Server의 후행 쿼리 수준 힌트(예: `OPTION (HASH JOIN)`)
  * SQL Server의 `SELECT` 절에서 `TOP 100 PERCENT`와 함께 제공되지 않는 `ORDER BY` 절

* `SELECT` 이외의 SQL 문은 구성할 수 없는 것으로 자동으로 인식됩니다. 따라서 저장 프로시저의 전체 결과는 항상 클라이언트에 반환되며 `FromSql` 다음에 적용된 모든 LINQ 연산자는 메모리 내에서 평가됩니다.

> [!WARNING]  
> **원시 SQL 쿼리에 항상 매개 변수화를 사용하세요.** 사용자 입력의 유효성을 검사할 뿐만 아니라 원시 SQL 쿼리/명령에서 사용되는 모든 값에 항상 매개 변수화를 사용하세요. `FromSql` 및 `ExecuteSqlCommand`와 같이 원시 SQL 문자열을 허용하는 API에서는 값을 매개 변수로 쉽게 전달할 수 있습니다. FormattableString을 허용하는 `FromSql` 및 `ExecuteSqlCommand`의 오버로드는 SQL 삽입 공격으로부터 보호하는 방식으로 문자열 보간 구문을 사용할 수도 있습니다. 
> 
> 문자열 연결 또는 보간을 사용하여 쿼리 문자열의 일부를 동적으로 빌드하거나 동적 SQL로 해당 입력을 실행할 수 있는 명령문이나 저장 프로시저에 사용자 입력을 전달하는 경우, SQL 삽입 공격으로부터 보호하기 위해 입력의 유효성을 검사해야할 책임이 있습니다.
