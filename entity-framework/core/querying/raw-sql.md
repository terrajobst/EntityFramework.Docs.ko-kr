---
title: 원시 SQL 쿼리 - EF Core
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: b7087771f1a9e8ee5e044cfea367d74a0b1c1d35
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445931"
---
# <a name="raw-sql-queries"></a>원시 SQL 쿼리

Entity Framework Core를 사용하면 관계형 데이터베이스로 작업할 때 원시 SQL 쿼리로 드롭다운할 수 있습니다. 원시 SQL 쿼리는 LINQ를 사용하여 원하는 쿼리를 표현할 수 없는 경우에 유용합니다. 원시 SQL 쿼리는 LINQ 쿼리를 사용하면 비효율적인 SQL 쿼리가 발생하는 경우에도 유용합니다. 원시 SQL 쿼리는 기본 엔터티 형식 또는 모델의 일부인 [키 없는 엔터티 형식](xref:core/modeling/keyless-entity-types)을 반환할 수 있습니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/)을 볼 수 있습니다.

## <a name="basic-raw-sql-queries"></a>기본 원시 SQL 쿼리

`FromSqlRaw` 확장 메서드를 사용하여 원시 SQL 쿼리를 기반으로 한 LINQ 쿼리를 시작할 수 있습니다. `FromSqlRaw`는 쿼리 루트에만 사용할 수 있습니다. 즉, `DbSet<>`에 직접 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

원시 SQL 쿼리를 사용하여 저장 프로시저를 실행할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a>매개 변수 전달

> [!WARNING]
> **원시 SQL 쿼리에 항상 매개 변수화를 사용하세요.**
>
> 사용자가 제공한 값을 원시 SQL 쿼리에 도입할 때는 SQL 삽입 공격을 방지하기 위해 주의를 기울여야 합니다. 이러한 값이 잘못된 문자를 포함하지 않은 것을 확인하는 것 외에도 항상 매개 변수화를 사용하여 SQL 텍스트와 별도로 값을 보내야 합니다.
>
> 특히 유효성이 검사되지 않은 사용자 제공 값을 사용하여 연결되거나 보간된 문자열(`$""`)을 `FromSqlRaw` 또는 `ExecuteSqlRaw`로 전달하면 안 됩니다. `FromSqlInterpolated` 및`ExecuteSqlInterpolated` 메서드를 사용하면 SQL 삽입 공격으로부터 보호하는 방식으로 문자열 보간 구문을 사용할 수 있습니다.

다음 예에서는 SQL 쿼리 문자열에 매개 변수 자리 표시자를 포함하고 추가 인수를 제공하여 단일 매개 변수를 저장 프로시저에 전달합니다. 이 구문은 `String.Format` 구문처럼 보일 수 있지만 제공된 값은 `DbParameter`로 래핑되고 생성된 매개 변수 이름은 `{0}` 자리 표시자가 지정된 위치에 삽입됩니다.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

`FromSqlInterpolated`는 `FromSqlRaw`와 비슷하지만, 이를 사용하여 문자열 보간 구문을 사용할 수 있습니다. `FromSqlRaw`와 마찬가지로 `FromSqlInterpolated`는 쿼리 루트에만 사용할 수 있습니다. 이전 예제와 마찬가지로 값은 `DbParameter`로 변환되므로 SQL 삽입에 취약하지 않습니다.

> [!NOTE]
> 버전 3.0 이전에는 `FromSqlRaw` 및 `FromSqlInterpolated`가 `FromSql`이라는 두 오버로드였습니다. 자세한 내용은 [이전 버전 섹션](#previous-versions)을 참조하세요.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

DbParameter를 생성하고 매개 변수 값으로 제공할 수도 있습니다. 문자열 자리 표시자가 아닌 일반 SQL 매개 변수 자리 표시자가 사용되므로 `FromSqlRaw`를 안전하게 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

`FromSqlRaw`를 사용하면 SQL 쿼리 문자열에 명명된 매개 변수를 사용할 수 있으며, 이는 저장 프로시저가 선택적 매개 변수를 포함하는 경우 유용합니다.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a>LINQ로 작성

LINQ 연산자를 사용하여 초기 원시 SQL 쿼리의 맨 위에 구성할 수 있습니다. EF Core는 이를 하위 쿼리로 취급하고 데이터베이스에서 이에 대해 구성합니다. 다음 예제에서는 TVF(테이블 반환 함수)에서 선택하는 원시 SQL 쿼리를 사용한 후 LINQ를 사용하여 이에 대해 구성하여 필터링과 정렬을 수행합니다.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

위 쿼리는 다음 SQL을 생성합니다.

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a>관련 데이터 포함

`Include` 메서드는 다른 LINQ 쿼리와 마찬가지로 관련 데이터를 포함하는 데 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

LINQ를 사용하여 구성하려면 원시 SQL 쿼리가 구성 가능해야 합니다. EF Core가 제공된 SQL을 하위 쿼리로 취급하기 때문입니다. SQL 쿼리는 `SELECT` 키워드로 시작하여 작성할 수 있습니다. 또한 전달된 SQL에는 다음과 같이 하위 쿼리에서 유효하지 않은 문자나 옵션을 포함할 수 없습니다.

- 후행 세미콜론
- SQL Server의 후행 쿼리 수준 힌트(예: `OPTION (HASH JOIN)`)
- SQL Server에서, `SELECT` 절에서 `OFFSET 0` 또는 `TOP 100 PERCENT`와 함께 사용되지 않는 `ORDER BY` 절

SQL Server는 저장 프로시저 호출에 대한 구성을 허용하지 않으므로 그러한 호출에 추가 쿼리 연산자를 적용하려고 하면 잘못된 SQL이 발생합니다. EF Core가 저장 프로시저에 대해 구성을 시도하지 않도록 `FromSqlRaw` 또는 `FromSqlInterpolated` 메서드 직후에 `AsEnumerable` 또는 `AsAsyncEnumerable` 메서드를 사용하세요.

## <a name="change-tracking"></a>변경 내용 추적

`FromSqlRaw` 또는 `FromSqlInterpolated` 메서드를 사용하는 쿼리는 EF Core에서 다른 LINQ 쿼리와 완전히 동일한 변경 내용 추적 규칙을 따릅니다. 예를 들어 쿼리 프로젝트 엔터티 형식의 경우 기본적으로 결과가 추적됩니다.

다음 예제에서는 TVF(테이블 반환 함수)에서 선택하는 원시 SQL 쿼리를 사용한 이후에 `AsNoTracking`을 호출하여 변경 내용 추적을 사용하지 않도록 설정합니다.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a>제한 사항

원시 SQL 쿼리를 사용할 때는 몇 가지 제한 사항에 유의해야 합니다.

- SQL 쿼리는 엔터티 형식의 모든 속성에 대해 데이터를 반환해야 합니다.
- 결과 집합의 열 이름은 속성이 매핑되는 열 이름과 일치해야 합니다. 이 동작은 EF6와 다릅니다. EF6은 속성/열 매핑을 원시 SQL 쿼리에 대해 무시했고 결과 집합 열 이름이 속성 이름과 일치해야 했습니다.
- SQL 쿼리는 관련 데이터를 포함할 수 없습니다. 그러나 대부분의 경우 쿼리의 맨 위에서 `Include` 연산자를 사용하여 관련 데이터를 반환하도록 작성할 수 있습니다([관련 데이터 포함](#including-related-data) 참조).

## <a name="previous-versions"></a>이전 버전

EF Core 버전 2.2 및 이전 버전에는 최신 `FromSqlRaw` 및 `FromSqlInterpolated`와 동일한 방식으로 작동하는 `FromSql` 메서드라는 두 개의 오버로드가 있습니다. 이로 인해 보간된 문자열 메서드를 호출하려다 실수로 원시 문자열 메서드를 호출하거나, 반대로 후자를 호출하려다 전자를 호출하는 실수를 저지를 가능성이 매우 높습니다. 실수로 잘못된 오버로드를 호출하면 매개 변수가 있어야 하는 쿼리에 매개 변수가 있지 않게 될 수 있습니다.
