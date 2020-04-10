---
title: EF Core 5.0의 새로운 기능
description: EF Core 5.0의 새로운 기능 개요
author: ajcvickers
ms.date: 03/30/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: c047a308cadf44eea577dcab29b68b36942a50df
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634275"
---
# <a name="whats-new-in-ef-core-50"></a>EF Core 5.0의 새로운 기능

현재 EF Core 5.0을 개발하고 있습니다.
이 페이지에는 각 미리 보기에서 소개된 중요한 변경 내용의 개요가 포함되어 있습니다.

이 페이지는 [EF Core 5.0의 플랜](plan.md)과 중복되지 않습니다.
이 플랜에서는 최종 릴리스를 출시하기 전에 포함할 예정인 모든 항목을 비롯하여 EF Core 5.0의 전체 테마를 설명합니다.

공식 설명서가 게시되면 여기의 링크가 설명서에 추가됩니다.

## <a name="preview-2"></a>Preview 2

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a>C# 특성을 사용하여 속성 지원 필드 지정

이제 C# 특성을 사용하여 속성의 지원 필드를 지정할 수 있습니다.
이 특성을 사용하면 EF Core에서는 지원 필드를 자동으로 찾을 수 없는 경우에도 일반적으로 발생하는 지원 필드에 대해 쓰기 및 읽기를 계속 수행할 수 있습니다.
예를 들어:

```CSharp
public class Blog
{
    private string _mainTitle;

    public int Id { get; set; }

    [BackingField(nameof(_mainTitle))]
    public string Title
    {
        get => _mainTitle;
        set => _mainTitle = value;
    }
}
```

설명서는 문제 [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230)에서 찾아볼 수 있습니다.

### <a name="complete-discriminator-mapping"></a>완전한 판별자 매핑

EF Core는 [상속 계층 구조의 TPH 매핑](/ef/core/modeling/inheritance)을 위해 판별자 열을 사용합니다.
EF Core에서 판별자의 가능한 모든 값을 알고 있는 경우 몇 가지 성능 개선이 가능해집니다.
EF Core 5.0은 이제 이러한 향상된 기능을 구현합니다.

예를 들어 이전 버전의 EF Core는 항상 계층의 모든 형식을 반환하는 쿼리에 대해 이 SQL을 생성합니다.

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

EF Core 5.0은 이제 전체 판별자 매핑이 구성될 때 다음을 생성합니다.

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

미리 보기 3부터 기본 동작으로 사용됩니다.

### <a name="performance-improvements-in-microsoftdatasqlite"></a>Microsoft.Data.Sqlite의 성능 향상

SQLIte에 대해 다음과 같은 두 가지 개선된 성능 기능이 구현되었습니다.

* 이제 SqliteBlob 및 스트림을 사용하여 GetBytes, GetChars 및 GetTextReader를 사용한 이진 및 문자열 데이터 검색을 보다 효율적으로 진행할 수 있습니다.
* 이제 SqliteConnection의 초기화가 지연됩니다.

이러한 향상된 기능은 ADO.NET Microsoft.Data.Sqlite 공급자에서 제공되므로 EF Core 외부의 성능도 향상됩니다.

## <a name="preview-1"></a>미리 보기 1

### <a name="simple-logging"></a>간단한 로깅

이 기능은 EF6의 `Database.Log`와 유사한 기능을 추가합니다.
즉, 어떠한 외부 로깅 프레임워크도 구성할 필요 없이 EF Core에서 로그를 가져오는 간단한 방법을 제공합니다.

예비 설명서는 [2019년 12월 5일의 EF 주간 상태](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)에 포함되어 있습니다.

추가 설명서는 문제 [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085)에서 찾아볼 수 있습니다.

### <a name="simple-way-to-get-generated-sql"></a>SQL을 생성하는 간단한 방법

EF Core 5.0에는 EF Core가 LINQ 쿼리를 실행할 때 생성하는 SQL을 반환하는 `ToQueryString` 확장 메서드가 도입되었습니다.

예비 설명서는 [2020년 1월 9일의 EF 주간 상태](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246)에 포함되어 있습니다.

추가 설명서는 문제 [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331)에서 찾아볼 수 있습니다.

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a>C# 특성을 사용하여 엔터티에 키가 없음을 나타냄

이제 새 `KeylessAttribute`를 사용하는 키가 없는 것으로 엔터티 형식을 구성할 수 있습니다.
예를 들어:

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

설명서는 문제 [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186)에서 찾아볼 수 있습니다.

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>초기화된 DbContext에서 연결 또는 연결 문자열을 변경할 수 있습니다.

이제 연결 문자열 또는 연결 문자열 없이 DbContext 인스턴스를 보다 쉽게 만들 수 있습니다.
또한 이제 컨텍스트 인스턴스에서 연결 또는 연결 문자열을 변경할 수 있습니다.
이 기능을 사용하면 동일한 컨텍스트 인스턴스를 다른 데이터베이스에 동적으로 연결할 수 있습니다.

설명서는 문제 [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075)에서 찾아볼 수 있습니다.

### <a name="change-tracking-proxies"></a>변경 내용 추적 프록시

이제 EF Core에서 [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) 및 [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1)를 자동으로 구현하는 런타임 프록시를 생성할 수 있습니다.
이후 엔터티 속성의 값 변경 내용을 EF Core에 직접 보고하므로 변경 내용을 별도로 검사하지 않아도 됩니다.
그러나 프록시에는 자체적으로 제한 사항이 적용되어 있어 일부 사용자에게는 적합하지 않습니다.

설명서는 문제 [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076)에서 찾아볼 수 있습니다.

### <a name="enhanced-debug-views"></a>향상된 디버그 보기

디버그 보기를 통해 문제를 디버깅할 때 EF Core의 내부를 쉽게 확인할 수 있습니다.
모델의 디버그 보기가 며칠 전에 구현되었습니다.
EF Core 5.0에서는 모델 보기를 간편하게 읽을 수 있으며, 상태 관리자에서 추적된 엔터티를 확인할 수 있는 새 디버그 보기가 추가되었습니다.

예비 설명서는 [2019년 12월 12일의 EF 주간 상태](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206)에 포함되어 있습니다.

추가 설명서는 문제 [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086)에서 찾아볼 수 있습니다.

### <a name="improved-handling-of-database-null-semantics"></a>향상된 데이터베이스 null 의미 체계 처리

관계형 데이터베이스는 일반적으로 NULL을 알 수 없는 값으로 처리하므로 다른 NULL과 같지 않습니다.
그렇지만 C#은 null을 다른 null과 동일하게 비교하는 정의된 값으로 처리합니다.
기본적으로 EF Core는 쿼리를 변환하여 C# null 의미 체계를 사용합니다.
EF Core 5.0은 이 변환 과정의 효율성을 크게 향상시킵니다.

설명서는 문제 [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612)에서 찾아볼 수 있습니다.

### <a name="indexer-properties"></a>인덱서 속성

EF Core 5.0는 C# 인덱서 속성의 매핑을 지원합니다.
이러한 속성을 사용하여 엔터티를 열이 속성 모음의 명명된 속성으로 매핑되는 속성 모음으로 사용할 수 있습니다.

설명서는 문제 [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018)에서 찾아볼 수 있습니다.

### <a name="generation-of-check-constraints-for-enum-mappings"></a>열거형 매핑의 check 제약 조건 생성

이제 EF Core 5.0 마이그레이션을 통해 열거형 속성 매핑의 CHECK 제약 조건을 생성할 수 있습니다.
예를 들어:

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

설명서는 문제 [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082)에서 찾아볼 수 있습니다.

### <a name="isrelational"></a>IsRelational

기존의 `IsSqlServer`, `IsSqlite`, `IsInMemory` 외에 새로운 `IsRelational` 메서드가 추가되었습니다.
이 메서드는 DbContext가 관계형 데이터베이스 공급자를 사용 중인지를 테스트하는 데 사용할 수 있습니다.
예를 들어:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

설명서는 문제 [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185)에서 찾아볼 수 있습니다.

### <a name="cosmos-optimistic-concurrency-with-etags"></a>ETag를 사용한 Cosmos 낙관적 동시성

Azure Cosmos DB 데이터베이스 공급자는 이제 ETag를 사용하여 낙관적 동시성을 지원합니다.
OnModelCreating에서 모델 작성기를 사용하여 ETag를 구성합니다.

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

그런 다음 SaveChanges에서 동시성 충돌 시 `DbUpdateConcurrencyException`을 throw하며, 다시 시도를 구현하기 위해 충돌을 [처리할 수 있습니다](https://docs.microsoft.com/ef/core/saving/concurrency).

설명서는 문제 [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099)에서 찾아볼 수 있습니다.

### <a name="query-translations-for-more-datetime-constructs"></a>더 많은 날짜/시간 구문의 쿼리 변환

이제 새로운 DateTime 생성을 포함하는 쿼리가 변환됩니다.

또한 이제 다음과 같은 SQL Server 함수가 매핑됩니다.

* DateDiffWeek
* DateFromParts

예를 들어:

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

설명서는 문제 [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.

### <a name="query-translations-for-more-byte-array-constructs"></a>더 많은 바이트 배열 구문의 쿼리 변환

Contains, Length, SequenceEqual 등을 사용하는 쿼리는 byte[] 속성에서 이제 SQL로 변환됩니다.

예비 설명서는 [2019년 12월 5일의 EF 주간 상태](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)에 포함되어 있습니다.

추가 설명서는 문제 [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.

### <a name="query-translation-for-reverse"></a>역방향 쿼리 변환

이제 `Reverse`를 사용하는 쿼리가 변환됩니다.
예를 들어:

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

설명서는 문제 [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.

### <a name="query-translation-for-bitwise-operators"></a>비트 연산자의 쿼리 변환

비트 연산자를 사용하는 쿼리는 다음과 같이 더 다양한 경우에 변환됩니다.

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

설명서는 문제 [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.

### <a name="query-translation-for-strings-on-cosmos"></a>Cosmos 문자열의 쿼리 변환

이제 Azure Cosmos DB 공급자를 사용할 때 Contains, StartsWith 및 EndsWith 문자열 메서드를 사용하는 쿼리가 변환됩니다.

설명서는 문제 [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.
