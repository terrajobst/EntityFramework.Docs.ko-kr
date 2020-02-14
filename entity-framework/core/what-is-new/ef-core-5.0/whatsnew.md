---
title: EF Core 5.0의 새로운 기능
author: ajcvickers
ms.date: 01/29/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: e858379cc46abbef999fd32a3685e1d522524889
ms.sourcegitcommit: 89567d08c9d8bf9c33bb55a62f17067094a4065a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77052019"
---
# <a name="whats-new-in-ef-core-50"></a>EF Core 5.0의 새로운 기능

현재 EF Core 5.0을 개발하고 있습니다.
이 페이지에는 각 미리 보기에서 소개된 중요한 변경 내용의 개요가 포함되어 있습니다.
EF Core 5.0의 첫 번째 미리 보기는 2020년 1분기에 공개될 예정입니다.

이 페이지는 [EF Core 5.0의 플랜](plan.md)과 중복되지 않습니다.
이 플랜에서는 최종 릴리스를 출시하기 전에 포함할 예정인 모든 항목을 비롯하여 EF Core 5.0의 전체 테마를 설명합니다.

공식 설명서가 게시되면 여기의 링크가 설명서에 추가됩니다.

## <a name="preview-1-not-yet-shipped"></a>미리 보기 1(아직 출시되지 않음)

### <a name="simple-logging"></a>간단한 로깅

이 기능은 EF6의 `Database.Log`와 유사한 기능을 추가합니다.
즉, 어떠한 외부 로깅 프레임워크도 구성할 필요 없이 EF Core에서 로그를 가져오는 간단한 방법을 제공합니다.

예비 설명서는 [2019년 12월 5일의 EF 주간 상태](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)에 포함되어 있습니다.

추가 설명서는 문제 [#2085](https://github.com/aspnet/EntityFramework.Docs/issues/2085)에서 찾아볼 수 있습니다.

### <a name="simple-way-to-get-generated-sql"></a>SQL을 생성하는 간단한 방법

EF Core 5.0에는 EF Core가 LINQ 쿼리를 실행할 때 생성하는 SQL을 반환하는 `ToQueryString` 확장 메서드가 도입되었습니다.

예비 설명서는 [2020년 1월 9일의 EF 주간 상태](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246)에 포함되어 있습니다.

추가 설명서는 문제 [#1331](https://github.com/aspnet/EntityFramework.Docs/issues/1331)에서 찾아볼 수 있습니다.

### <a name="enhanced-debug-views"></a>향상된 디버그 보기

디버그 보기를 통해 문제를 디버깅할 때 EF Core의 내부를 쉽게 확인할 수 있습니다.
모델의 디버그 보기가 며칠 전에 구현되었습니다.
EF Core 5.0에서는 모델 보기를 간편하게 읽을 수 있으며, 상태 관리자에서 추적된 엔터티를 확인할 수 있는 새 디버그 보기가 추가되었습니다.

예비 설명서는 [2019년 12월 12일의 EF 주간 상태](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206)에 포함되어 있습니다.

추가 설명서는 문제 [#2086](https://github.com/aspnet/EntityFramework.Docs/issues/2086)에서 찾아볼 수 있습니다.

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>초기화된 DbContext에서 연결 또는 연결 문자열을 변경할 수 있습니다.

이제 연결 문자열 또는 연결 문자열 없이 DbContext 인스턴스를 보다 쉽게 만들 수 있습니다.
또한 이제 컨텍스트 인스턴스에서 연결 또는 연결 문자열을 변경할 수 있습니다.
이에 따라 동일한 컨텍스트 인스턴스를 다른 데이터베이스에 동적으로 연결할 수 있습니다.

설명서는 문제 [#2075](https://github.com/aspnet/EntityFramework.Docs/issues/2075)에서 찾아볼 수 있습니다.

### <a name="change-tracking-proxies"></a>변경 내용 추적 프록시

이제 EF Core에서 [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) 및 [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1)를 자동으로 구현하는 런타임 프록시를 생성할 수 있습니다.
이후 엔터티 속성의 값 변경 내용을 EF Core에 직접 보고하므로 변경 내용을 별도로 검사하지 않아도 됩니다.
그러나 프록시에는 자체적으로 제한 사항이 적용되어 있어 일부 사용자에게는 적합하지 않습니다.

설명서는 문제 [#2076](https://github.com/aspnet/EntityFramework.Docs/issues/2076)에서 찾아볼 수 있습니다.

### <a name="improved-handling-of-database-null-semantics"></a>향상된 데이터베이스 null 의미 체계 처리

관계형 데이터베이스는 일반적으로 NULL을 알 수 없는 값으로 처리하므로 다른 NULL과 같지 않습니다.
반면 C#은 null을 다른 null과 동일하게 비교하는 정의된 값으로 처리합니다.
기본적으로 EF Core는 쿼리를 변환하여 C# null 의미 체계를 사용합니다.
EF Core 5.0은 이 변환 과정의 효율성을 크게 향상시킵니다.

설명서는 문제 [#1612](https://github.com/aspnet/EntityFramework.Docs/issues/1612)에서 찾아볼 수 있습니다.

### <a name="indexer-properties"></a>인덱서 속성

EF Core 5.0는 C# 인덱서 속성의 매핑을 지원합니다.
이에 따라 엔터티를 속성 모음으로 사용할 수 있으며 속성 모음의 명명된 속성으로 열이 매핑됩니다.

설명서는 문제 [#2018](https://github.com/aspnet/EntityFramework.Docs/issues/2018)에서 찾아볼 수 있습니다.

### <a name="generation-of-check-constraints-for-enum-mappings"></a>열거형 매핑의 check 제약 조건 생성

이제 EF Core 5.0 마이그레이션을 통해 열거형 속성 매핑의 CHECK 제약 조건을 생성할 수 있습니다.
예를 들어:

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

설명서는 문제 [#2082](https://github.com/aspnet/EntityFramework.Docs/issues/2082)에서 찾아볼 수 있습니다.

### <a name="query-translations-for-more-datetime-constructs"></a>더 많은 날짜/시간 구문의 쿼리 변환

이제 새로운 DataTime 생성을 포함하는 쿼리가 변환됩니다.
또한 이제 SQL Server 함수 DateDiffWeek가 매핑됩니다.

설명서는 문제 [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.

### <a name="query-translations-for-more-byte-array-constructs"></a>더 많은 바이트 배열 구문의 쿼리 변환

Contains, Length, SequenceEqual 등을 사용하는 쿼리는 byte[] 속성에서 이제 SQL로 변환됩니다.

예비 설명서는 [2019년 12월 5일의 EF 주간 상태](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)에 포함되어 있습니다.

추가 설명서는 문제 [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.

### <a name="query-translation-for-reverse"></a>역방향 쿼리 변환

이제 `Reverse`를 사용하는 쿼리가 변환됩니다.
예를 들어:

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

설명서는 문제 [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.

### <a name="query-translation-for-bitwise-operators"></a>비트 연산자의 쿼리 변환

비트 연산자를 사용하는 쿼리는 다음과 같이 더 다양한 경우에 변환됩니다.

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

설명서는 문제 [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.

### <a name="query-translation-for-strings-on-cosmos"></a>Cosmos 문자열의 쿼리 변환

이제 Azure Cosmos DB 공급자를 사용할 때 Contains, StartsWith 및 EndsWith 문자열 메서드를 사용하는 쿼리가 변환됩니다.

설명서는 문제 [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079)에서 찾아볼 수 있습니다.
