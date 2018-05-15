---
title: 원시 SQL 쿼리-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: 29b7e20e875bf791a88a92636c1df4bc4e31656b
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/14/2018
---
# <a name="raw-sql-queries"></a>원시 SQL 쿼리

Entity Framework Core를 사용 하면 관계형 데이터베이스와 함께 사용할 때 원시 SQL 쿼리로 드롭다운 수 있습니다. LINQ를 사용 하 여 수행 하려는 쿼리를 표현할 수 없는 경우 또는 비효율적인 SQL 데이터베이스에 전송 되는 줄여주는 LINQ 쿼리를 사용 하는 경우에 유용할 수 있습니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)을 볼 수 있습니다.

## <a name="limitations"></a>제한 사항

원시 SQL 쿼리를 사용 하는 경우 고려해 야 할 몇 가지 제한이 있습니다.
* SQL 쿼리 엔터티 형식을 모델의 일부인 반환에 사용할 수 있습니다. 향상 된 기능을 통해 백로그에 없는 [원시 SQL 쿼리에서 임시 형식을 반환 하는 활성화](https://github.com/aspnet/EntityFramework/issues/1862)합니다.

* SQL 쿼리 엔터티 또는 쿼리 형식의 모든 속성에 대 한 데이터를 반환 해야 합니다.

* 결과 집합의 열 이름 속성에 매핑되는 열 이름이 일치 해야 합니다. 이 옵션은 원시 SQL 쿼리에 대 한 속성/열 매핑이 무시 되었습니다 여기서 EF6 다릅니다 및 결과 집합 열 이름은 속성 이름과 일치 해야 합니다.

* SQL 쿼리는 관련된 데이터를 포함할 수 없습니다. 그러나 대부분의 경우에서 구성할 수 있습니다를 사용 하 여 쿼리를 기반으로 `Include` 관련된 데이터를 반환 하도록 연산자 (참조 [관련된 데이터를 포함 하 여](#including-related-data)).

* `SELECT` 이 메서드에 전달 된 문은 일반적으로 구성 가능 해야: 서버에서 추가 쿼리 연산자를 평가 해야 하는 경우 EF 코어 (예: 다음에 적용 하는 LINQ 연산자를 변환 하 `FromSql`), 제공 된 SQL 하위 쿼리로 간주 합니다. 즉, 모든 문자 또는 유효 하지 않은 하위 쿼리,와 같은 옵션 전달 된 sql 문을 포함 하지 않아야 합니다.
  * 후행 세미콜론
  * SQL Server에서 후행 쿼리 수준 힌트, 예: `OPTION (HASH JOIN)`
  * SQL Server에는 `ORDER BY` 은 하지의 함께 사용 하는 절 `TOP 100 PERCENT` 에 `SELECT` 절

* 이외의 다른 SQL 문을 `SELECT` 으로 구성할 수 없는 자동으로 인식 됩니다. 이 인해 저장된 프로시저의 전체 결과 항상 클라이언트에 반환 하 고 모든 LINQ 연산자 뒤에 적용 `FromSql` 메모리에 평가 됩니다. 

## <a name="basic-raw-sql-queries"></a>기본 원시 SQL 쿼리

사용할 수는 *FromSql* 확장 메서드를 원시 SQL 쿼리를 기반으로 하는 LINQ 쿼리를 시작 합니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

저장된 프로시저를 실행할 원시 SQL 쿼리를 사용할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>매개 변수 전달

SQL을 허용 하는 API와 마찬가지로 SQL 삽입 공격 으로부터 보호 하기 위해 입력 하는 모든 사용자를 매개 변수화 해야 합니다. SQL 쿼리 문자열에 매개 변수 자리 표시자를 포함할 수 있으며 매개 변수 값으로 추가 인수를 제공 합니다. 제공한 매개 변수 값으로 자동으로 변환 됩니다는 `DbParameter`합니다.

다음 예제에서는 저장된 프로시저를 단일 매개 변수를 전달합니다. 처럼 보일 수이 하는 동안 `String.Format` 구문, 제공 된 값이 요소에 래핑되 생성 된 매개 변수 이름과 매개 변수 삽입 위치는 `{0}` 자리 표시자를 지정 합니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

이 동일한 쿼리는 있지만 이상 EF 코어 2.0 버전에서 지원 되는 문자열 보간 구문을 사용 하 여:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

DbParameter를 생성 하 고 매개 변수 값으로 제공할 수도 있습니다. SQL 쿼리 문자열의 명명 된 매개 변수를 사용할 수 있습니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>LINQ로 작성

데이터베이스에서에서 SQL 쿼리를 구성할 수, 있으면 LINQ 연산자를 사용 하 여 초기 원시 SQL 쿼리를 기반으로 구성할 수 있습니다. 사용 중인에 구성할 수 있는 SQL 쿼리는 `SELECT` 키워드입니다.

다음 예제에서는 LINQ를 사용 하 여 필터링 및 정렬을 수행 하기에 테이블 반환 함수 (TVF)에서 선택 하 고 다음을 구성 하는 원시 SQL 쿼리를 사용 합니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a>관련된 데이터를 포함 하 여

LINQ 연산자가 있는 작성 용도 쿼리에 관련 된 데이터를 포함 하도록 합니다.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> **항상 원시 SQL 쿼리를 매개 변수화를 사용 하도록:** 원시 SQL 허용 하는 Api와 같은 문자열 `FromSql` 및 `ExecuteSqlCommand` 쉽게 매개 변수로 전달 될 값을 허용 합니다. 사용자 입력의 유효성을 검사 하는 것 외에도 항상 원시 SQL 쿼리/명령에서 사용 되는 모든 값에 대 한 매개 변수화를 사용 합니다. 사용할 경우 문자열 연결을 동적으로 SQL 삽입 공격 으로부터 보호 하기 위해 모든 입력 유효성 검사를 담당 하는 다음 쿼리 문자열의 모든 부분을 구성 합니다.
