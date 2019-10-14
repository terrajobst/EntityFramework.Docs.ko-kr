---
title: 복합 쿼리 연산자-EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 2e187a2a-4072-4198-9040-aaad68e424fd
uid: core/querying/complex-query-operators
ms.openlocfilehash: 350a7fa6a3ee1de16bad4b63e10842f9356a1b60
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72186235"
---
# <a name="complex-query-operators"></a>복합 쿼리 연산자

LINQ(통합 언어 쿼리)에는 여러 데이터 원본을 결합하거나 복잡한 처리를 수행하는 여러 복합 연산자가 포함되어 있습니다. 모든 LINQ 연산자가 서버 쪽에 적절한 변환을 가지고 있는 것은 아닙니다. 쿼리가 특정 형식에서는 서버에서 변환되지만 다른 형식에서는 결과가 동일하더라도 변환되지 않는 경우가 있습니다. 이 페이지에서는 몇 가지 복합 연산자와 지원되는 변형에 대해 설명합니다. 이후 릴리스에서는 더 많은 패턴을 인식하고 해당 변환을 추가할 수 있습니다. 변환 지원은 공급자 간에 다를 수 있다는 점을 염두에 두어야 합니다. SqlServer에서는 변환되는 쿼리가 SQLite 데이터베이스에서는 작동하지 않을 수 있습니다.

> [!TIP]
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)을 볼 수 있습니다.

## <a name="join"></a>Join

LINQ Join 연산자를 사용하면 각 소스의 키 선택기에 따라 두 데이터 원본을 연결하여 키가 일치하면 값 튜플을 생성할 수 있습니다. Join은 관계형 데이터베이스에서 `INNER JOIN`으로 변환됩니다. LINQ Join에는 외부 및 내부 키 선택기가 있지만 데이터베이스에는 하나의 조인 조건만 필요합니다. 따라서 EF Core는 외부 키 선택기와 내부 키 선택기를 비교하고 동일성을 확인하여 조인 조건을 생성합니다. 키 선택기가 무명 형식인 경우, EF Core는 동일성을 구성 요소별로 비교하는 조인 조건을 생성합니다.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a>GroupJoin

LINQ GroupJoin 연산자를 사용하면 Join과 비슷하게 두 데이터 원본을 연결할 수 있는데, 이때 일치하는 외부 요소에 대한 내부 값 그룹이 생성됩니다. 다음 예와 같이 쿼리를 실행하면 `Blog` & `IEnumerable<Post>` 결과가 생성됩니다. 데이터베이스(특히 관계형 데이터베이스)에는 클라이언트 쪽 개체를 나타낼 방법이 없으므로 GroupJoin은 많은 경우에 서버 쪽으로 변환되지 않습니다. 특수 선택기 없이 GroupJoin을 수행하려면 서버에서 모든 데이터를 가져와야 합니다(아래의 첫 번째 쿼리). 그러나 선택기가 선택되는 데이터를 제한하는 경우에는 서버에서 모든 데이터를 가져오는 것이 성능 문제를 야기할 수 있습니다(아래의 두 번째 쿼리). 따라서 EF Core는 GroupJoin을 변환하지 않습니다.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a>SelectMany

LINQ SelectMany 연산자를 사용하면 각 외부 요소에 대해 컬렉션 선택기를 기준으로 열거하고 각 데이터 원본으로부터 값 튜플을 생성할 수 있습니다. 어떤 면에서는 Join과 비슷하지만 조건이 없으므로 모든 외부 요소가 컬렉션 소스의 요소에 연결됩니다. 컬렉션 선택기가 외부 데이터 원본과 어떤 관계를 맺고 있는지에 따라 SelectMany는 서버 쪽에서 각종 쿼리로 변환될 수 있습니다.

### <a name="collection-selector-doesnt-reference-outer"></a>컬렉션 선택기는 외부를 참조하지 않음

컬렉션 선택기가 외부 소스의 무엇도 참조하지 않을 경우, 결과는 두 데이터 원본의 카티전 곱이 됩니다. 이는 관계형 데이터베이스에서 `CROSS JOIN`으로 변환됩니다.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a>컬렉션 선택기는 where 절에서 외부를 참조함

컬렉션 선택기에 외부 요소를 참조하는 where 절이 있는 경우 EF Core는 이를 데이터베이스 조인으로 변환하고 조건자를 조인 조건으로 사용합니다. 일반적으로 이 사례는 외부 요소에 대한 컬렉션 탐색을 컬렉션 선택기로 사용하는 경우에 발생합니다. 외부 요소에 대한 컬렉션이 비어 있으면 해당 외부 요소에 대해 결과가 생성되지 않습니다. 그러나 컬렉션 선택기에 `DefaultIfEmpty`가 적용된 경우에는 외부 요소가 내부 요소의 기본값에 연결됩니다. 이러한 차이 때문에 이 유형의 쿼리는 `DefaultIfEmpty`가 적용되지 않은 경우 `INNER JOIN`으로 변환되고 `DefaultIfEmpty`가 적용된 경우 `LEFT JOIN`으로 변환됩니다.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a>where가 아닌 케이스에서 컬렉션 선택기가 외부를 참조함

컬렉션 선택기가 위 사례와 달리 where 절에 있지 않은 외부 요소를 참조하는 경우에는 데이터베이스 조인으로 변환되지 않습니다. 따라서 각 외부 요소에 대해 컬렉션 선택기를 평가해야 합니다. 많은 관계형 데이터베이스에서 이는 `APPLY` 연산으로 변환됩니다. 외부 요소에 대한 컬렉션이 비어 있으면 해당 외부 요소에 대해 결과가 생성되지 않습니다. 그러나 컬렉션 선택기에 `DefaultIfEmpty`가 적용된 경우에는 외부 요소가 내부 요소의 기본값에 연결됩니다. 이러한 차이 때문에 이 유형의 쿼리는 `DefaultIfEmpty`가 적용되지 않은 경우 `CROSS APPLY`로 변환되고 `DefaultIfEmpty`가 적용된 경우 `OUTER APPLY`로 변환됩니다. SQLite와 같은 특정 데이터베이스에서는 `APPLY` 연산자를 지원하지 않으므로 이러한 유형의 쿼리가 변환되지 않을 수 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a>GroupBy

LINQ GroupBy 연산자는 `IGrouping<TKey, TElement>` 유형의 결과를 만듭니다. 여기서 `TKey`와 `TElement`는 어떠한 임의의 유형도 될 수 있습니다. 이에 더해, `IGrouping`은 `IEnumerable<TElement>`를 구현합니다. 즉, 그룹화 뒤에 모든 LINQ 연산자를 사용하여 이에 대해 구성할 수 있습니다. `IGrouping`을 나타낼 수 있는 데이터베이스 구조는 없으므로 GroupBy 연산자는 대부분의 경우 변환이 없습니다. 각 그룹에 스칼라를 반환하는 집계 연산자가 적용되면 관계형 데이터베이스에서 SQL `GROUP BY`로 변환될 수 있습니다. SQL `GROUP BY`도 제한적입니다. 이것을 사용하려면 스칼라 값으로만 그룹화해야 합니다. 프로젝션은 그룹화 키 열 또는 열에 대해 적용된 집계만 포함할 수 있습니다. EF Core는 이 패턴을 식별한 후 다음 예제와 같이 서버로 변환합니다.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

EF Core는 그룹화에 대한 집계 연산자가 Where 또는 OrderBy(또는 여타 정렬) LINQ 연산자에 나타나는 쿼리도 변환합니다. EF Core는 where 절에 대해 SQL의 `HAVING` 절을 사용합니다. GroupBy 연산자를 적용하기 전에 오는 쿼리 부분은 서버로 변환될 수만 있다면 어떠한 복합 쿼리도 될 수 있습니다. 그뿐만 아니라, 결과 원본에서 그룹화를 제거하기 위해 그룹화 쿼리에 집계 연산자를 적용한 후에는 이 위에 다른 쿼리처럼 구성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

EF Core에서 지원하는 집계 연산자는 다음과 같습니다.

- 평균
- 개수
- LongCount
- 최대
- 최소
- Sum

## <a name="left-join"></a>왼쪽 조인

왼쪽 조인은 LINQ 연산자는 아니지만, 관계형 데이터베이스에는 쿼리에서 자주 사용되는 왼쪽 조인이라는 개념이 있습니다. LINQ 쿼리의 특정 패턴이 서버에서 `LEFT JOIN`과 같은 결과를 제공합니다. EF Core는 이러한 패턴을 식별하고 서버 쪽에서 여기에 해당하는 `LEFT JOIN`을 생성합니다. 패턴에는 데이터 원본 간에 GroupJoin을 만든 다음 내부에 관련 요소가 없는 경우 NULL과 일치하도록 그룹화 소스에 DefaultIfEmpty를 적용한 상태로 SelectMany를 연산자를 사용하여 그룹화를 평면화하는 과정이 수반됩니다. 다음 예에서는 패턴이 어떤 모양이며 무엇을 생성하는지 보여 줍니다.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

위 패턴은 식 트리에서 복합 구조체를 만듭니다. 그렇기 때문에 EF Core는 연산자 직후에 오는 단계에서 GroupJoin 연산자의 그룹화 결과를 평면화할 것을 요구합니다. GroupJoin-DefaultIfEmpty-SelectMany가 다른 패턴에서 사용되더라도 이를 왼쪽 조인으로 식별하지 못할 수 있습니다.
