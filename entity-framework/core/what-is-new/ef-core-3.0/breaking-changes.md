---
title: EF Core 3.0의 호환성이 손상되는 변경 - EF Core
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 6e0c17a22b56b206f18e47f678e3e237d5c42375
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888111"
---
# <a name="breaking-changes-included-in-ef-core-30"></a>EF Core 3.0에 포함된 주요 변경 내용

3\.0.0으로 업그레이드할 때 기존 애플리케이션의 호환성이 손상될 수 있는 API 및 동작 변경 내용은 다음과 같습니다.
데이터베이스 공급자에만 영향을 줄 것으로 예상되는 변경 내용은 [공급자 변경](xref:core/providers/provider-log)에 설명되어 있습니다.

## <a name="summary"></a>요약

| **주요 변경 내용**                                                                                               | **영향** |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [LINQ 쿼리는 더 이상 클라이언트에서 평가되지 않습니다.](#linq-queries-are-no-longer-evaluated-on-the-client)         | 높음       |
| [EF Core 3.0은 .NET Standard 2.0이 아니라 .NET Standard 2.1을 대상으로 합니다.](#netstandard21) | 높음      |
| [EF Core 명령줄 도구인 dotnet ef는 더 이상 .NET Core SDK의 일부가 아닙니다.](#dotnet-ef) | 높음      |
| [DetectChanges는 저장소 생성 키 값을 준수합니다.](#dc) | 높음      |
| [FromSql, ExecuteSql, ExecuteSqlAsync의 이름이 변경되었습니다.](#fromsql) | 높음      |
| [쿼리 형식은 엔터티 형식과 통합됩니다.](#qt) | 높음      |
| [Entity Framework Core는 더 이상 ASP.NET Core 공유 프레임워크의 일부가 아닙니다.](#no-longer) | 중간      |
| [계단식 삭제는 기본적으로 즉시 발생합니다.](#cascade) | 중간      |
| [관련 엔터티의 즉시 로드가 이제 단일 쿼리에서 이루어집니다.](#eager-loading-single-query) | 중간      |
| [DeleteBehavior.Restrict에 더 명확한 의미 체계가 적용되었습니다.](#deletebehavior) | 중간      |
| [소유 형식 관계에 대한 구성 API가 변경되었습니다.](#config) | 중간      |
| [각 속성은 독립적인 메모리 내 정수 키 생성을 사용합니다.](#each) | 중간      |
| [비 추적 쿼리가 더 이상 ID 확인을 수행하지 않습니다.](#notrackingresolution) | 중간      |
| [메타데이터 API 변경 내용](#metadata-api-changes) | 중간      |
| [공급자별 메타데이터 API 변경 내용](#provider) | 중간      |
| [UseRowNumberForPaging이 제거되었습니다.](#urn) | 중간      |
| [저장 프로시저와 함께 사용할 경우 FromSql 메서드를 구성할 수 없습니다.](#fromsqlsproc) | 중간      |
| [FromSql 메서드는 쿼리 루트에만 지정할 수 있습니다.](#fromsql) | 낮음      |
| [~~쿼리 실행은 디버그 수준에서 로깅됩니다.~~ 되돌림](#qe) | 낮음      |
| [임시 키 값은 더 이상 엔터티 인스턴스에 설정되지 않습니다.](#tkv) | 낮음      |
| [보안 주체와 테이블을 공유하는 종속 엔터티는 이제 선택 사항입니다.](#de) | 낮음      |
| [동시 토큰 열을 사용하여 테이블을 공유하는 모든 엔터티는 해당 열을 속성에 매핑해야 합니다.](#aes) | 낮음      |
| [추적 쿼리를 사용하는 소유자 없이 소유된 엔터티를 쿼리할 수 없습니다.](#owned-query) | 낮음      |
| [매핑되지 않은 형식에서 상속된 속성은 이제 모든 파생 형식에 대해 단일 열에 매핑됩니다.](#ip) | 낮음      |
| [외래 키 속성 규칙이 더 이상 보안 주체 속성과 동일한 이름을 일치시키지 않습니다.](#fkp) | 낮음      |
| [데이터베이스 연결은 이제 TransactionScope가 완료되기 전에 더 이상 사용되지 않으면 닫힙니다.](#dbc) | 낮음      |
| [지원 필드는 기본적으로 사용됩니다.](#backing-fields-are-used-by-default) | 낮음      |
| [호환이 가능한 여러 지원 필드가 발견되면 throw합니다.](#throw-if-multiple-compatible-backing-fields-are-found) | 낮음      |
| [필드 전용 속성 이름은 필드 이름과 일치해야 합니다.](#field-only-property-names-should-match-the-field-name) | 낮음      |
| [AddDbContext/AddDbContextPool이 더 이상 AddLogging 및 AddMemoryCache를 호출하지 않습니다.](#adddbc) | 낮음      |
| [AddEntityFramework*에서 IMemoryCache를 크기 제한하여 추가합니다.](#addentityframework-adds-imemorycache-with-a-size-limit) | 낮음      |
| [DbContext.Entry는 이제 로컬 DetectChanges를 수행합니다.](#dbe) | 낮음      |
| [문자열 및 바이트 배열 키는 기본적으로 클라이언트에서 생성되지 않습니다.](#string-and-byte-array-keys-are-not-client-generated-by-default) | 낮음      |
| [ILoggerFactory는 이제 범위가 지정된 서비스입니다.](#ilf) | 낮음      |
| [지연 로드 프록시는 더 이상 탐색 속성이 완전히 로드되었다고 가정하지 않습니다.](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | 낮음      |
| [내부 서비스 공급자의 과도한 생성은 이제 기본적으로 오류입니다.](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | 낮음      |
| [단일 문자열로 호출되는 HasOne/HasMany의 새 동작](#nbh) | 낮음      |
| [여러 비동기 메서드의 반환 형식이 작업에서 ValueTask로 변경되었습니다.](#rtnt) | 낮음      |
| [관계형:TypeMapping 주석은 이제 TypeMapping일 뿐입니다.](#rtt) | 낮음      |
| [파생된 형식의 ToTable에서 예외가 throw됩니다.](#totable-on-a-derived-type-throws-an-exception) | 낮음      |
| [EF Core는 더 이상 SQLite FK 적용을 위한 pragma를 보내지 않습니다.](#pragma) | 낮음      |
| [Microsoft.EntityFrameworkCore.Sqlite는 이제 SQLitePCLRaw.bundle_e_sqlite3에 종속됩니다.](#sqlite3) | 낮음      |
| [Guid 값은 이제 SQLite에 텍스트로 저장됩니다.](#guid) | 낮음      |
| [Char 값은 이제 SQLite에 텍스트로 저장됩니다.](#char) | 낮음      |
| [마이그레이션 ID가 이제 고정 문화권의 달력을 사용하여 생성됩니다.](#migid) | 낮음      |
| [확장 정보/메타데이터가 IDbContextOptionsExtension에서 제거되었습니다.](#xinfo) | 낮음      |
| [LogQueryPossibleExceptionWithAggregateOperator 이름이 바뀌었습니다.](#lqpe) | 낮음      |
| [외래 키 제약 조건 이름에 대한 API가 명확해졌습니다.](#clarify) | 낮음      |
| [IRelationalDatabaseCreator.HasTables/HasTablesAsync가 공개되었습니다.](#irdc2) | 낮음      |
| [이제 Microsoft.EntityFrameworkCore.Design은 DevelopmentDependency 패키지입니다.](#dip) | 낮음      |
| [SQLitePCL.raw가 버전 2.0.0으로 업데이트되었습니다.](#SQLitePCL) | 낮음      |
| [NetTopologySuite가 버전 2.0.0으로 업데이트되었습니다.](#NetTopologySuite) | 낮음      |
| [System.Data.SqlClient 대신 Microsoft.Data.SqlClient가 사용됩니다.](#SqlClient) | 낮음      |
| [복수의 모호한 자기 참조 관계를 구성해야 합니다.](#mersa) | 낮음      |
| [DbFunction.Schema가 null 또는 빈 문자열이면 모델의 기본 스키마에 있도록 구성됩니다.](#udf-empty-string) | 낮음      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a>LINQ 쿼리는 더 이상 클라이언트에서 평가되지 않습니다.

[추적 문제 #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[문제 #12795도 참조](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

**이전 동작**

3\.0 이전에는 EF Core가 쿼리의 일부인 식을 SQL 또는 매개 변수로 변환할 수 없었을 때 클라이언트에서 자동으로 식을 계산했습니다.
기본적으로, 잠재적 비용이 많이 드는 식에 대한 클라이언트 평가는 경고만 트리거합니다.

**새 동작**

3\.0부터 EF Core는 최상위 수준 프로젝션(쿼리의 마지막 `Select()` 호출)의 식만 클라이언트에서 평가할 수 있습니다.
쿼리의 다른 부분에서 식을 SQL 또는 매개 변수로 변환할 수 없을 때 예외가 throw됩니다.

**이유**

쿼리의 자동 클라이언트 평가는 중요한 부분을 변환할 수 없어도 많은 쿼리를 실행할 수 있게 합니다.
이 동작으로 인해 프로덕션 환경에서 분명하게 나타날 수 있는 예기치 않은 잠재적 손상을 초래할 수 있습니다.
예를 들어 변환할 수 없는 `Where()` 호출의 조건으로 인해 테이블의 모든 행이 데이터베이스 서버에서 전송되고 필터가 클라이언트에 적용될 수 있습니다.
이 경우 테이블에 개발 중인 몇 개 행만 포함하지만 애플리케이션이 수백만 개의 행이 포함될 수 있는 프로덕션으로 이동할 때 쉽게 감지되지 않을 수 있습니다.
클라이언트 평가 경고도 개발 중에 너무 쉽게 무시됩니다.

이 외에도 자동 클라이언트 평가는 특정 식에 대한 쿼리 변환 기능 개선으로 인해 릴리스 간의 의도하지 않은 호환성이 손상되는 변경이 발생할 수 있습니다.

**완화 방법**

쿼리를 완벽하게 변환할 수 없는 경우 변환할 수 있는 형식으로 쿼리를 다시 작성하거나 `AsEnumerable()`, `ToList()` 또는 유사한 형식을 사용하여 명시적으로 데이터를 클라이언트로 가져오면 LINQ to Objects를 사용하여 추가로 처리할 수 있습니다.

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a>EF Core 3.0은 .NET Standard 2.0이 아니라 .NET Standard 2.1을 대상으로 합니다.

[추적 문제 #15498](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

> [!IMPORTANT] 
> EF Core 3.1은 다시 .NET Standard 2.0을 대상으로 합니다. 따라서 .NET Framework가 다시 지원됩니다.

**이전 동작**

3\.0 이전에는 EF Core가 .NET Standard 2.0을 대상으로 했으며 .NET Framework 등 표준을 지원하는 모든 플랫폼에서 실행되었습니다.

**새 동작**

3\.0부터 EF Core는 .NET Standard 2.1을 대상으로 하며 이 표준을 지원하는 모든 플랫폼에서 실행됩니다. 여기에 .NET Framework는 포함되지 않습니다.

**이유**

이는 .NET Core와 Xamarin 같은 기타 최신 .NET 플랫폼에 에너지를 집중하기 위한 .NET 기술의 전략적 결정의 일부입니다.

**완화 방법**

EF Core 3.1을 사용합니다.

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a>Entity Framework Core는 더 이상 ASP.NET Core 공유 프레임워크에 포함되지 않습니다.

[추적 문제 공지 사항 #325](https://github.com/aspnet/Announcements/issues/325)

**이전 동작**

ASP.NET Core 3.0 이전에는 `Microsoft.AspNetCore.App` 또는 `Microsoft.AspNetCore.All`에 대한 패키지 참조를 추가하면 EF Core 및 SQL Server 공급자와 같은 일부 EF Core 데이터 공급자가 포함되었습니다.

**새 동작**

3\.0부터 ASP.NET Core 공유 프레임워크에는 EF Core 또는 EF Core 데이터 공급자가 포함되지 않습니다.

**이유**

이 변경 전에 EF Core를 가져오려면 애플리케이션이 ASP.NET Core 및 SQL Server를 대상으로 했는지 여부에 따라 다른 단계가 필요했습니다. 또한 ASP.NET Core를 업그레이드 하면 EF Core 및 SQL Server 업그레이드가 강제되었는데, 이는 항상 바람직하지는 않습니다.

이 변경으로 인해 EF Core를 가져오는 환경은 모든 공급자, 지원되는 .NET 구현 및 애플리케이션 유형에서 동일합니다.
또한 개발자는 이제 EF Core 및 EF Core 데이터 공급자의 업그레이드 시기를 정확히 제어할 수 있습니다.

**완화 방법**

ASP.NET Core 3.0 애플리케이션 또는 기타 지원되는 애플리케이션에서 EF Core를 사용하려면 애플리케이션에서 사용할 EF Core 데이터베이스 공급자에 대한 패키지 참조를 명시적으로 추가합니다.

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a>EF Core 명령줄 도구인 dotnet ef는 더 이상 .NET Core SDK의 일부가 아닙니다.

[추적 문제 #14016](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

**이전 동작**

3\.0 이전에는 `dotnet ef` 도구가 .NET Core SDK에 포함되었으며 추가 단계 없이도 모든 프로젝트의 명령줄에서 쉽게 사용할 수 있었습니다. 

**새 동작**

3\.0부터 .NET SDK에 `dotnet ef` 도구가 포함되어 있지 않으므로 사용하기 전에 명시적으로 로컬 또는 글로벌 도구로 설치해야 합니다. 

**이유**

이 변경으로 인해 `dotnet ef`을 NuGet에서 일반 .NET CLI 도구로 배포 및 업데이트할 수 있으며, EF Core 3.0도 항상 NuGet 패키지로 배포된다는 사실과 일치합니다.

**완화 방법**

마이그레이션을 관리하거나 `DbContext`의 스캐폴드를 수행하려면 `dotnet-ef`를 전역 도구로 설치합니다.

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

[도구 매니페스트 파일](https://github.com/dotnet/cli/issues/10288)을 사용하여 도구 종속성으로 선언한 프로젝트의 종속성을 복원할 때도 로컬 도구를 얻을 수 있습니다.

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a>FromSql, ExecuteSql, ExecuteSqlAsync의 이름이 변경되었습니다.

[추적 문제 #10996](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

**이전 동작**

EF Core 3.0이 나오기 전에는 이런 메서드 이름이 일반 문자열 또는 SQL 및 매개 변수로 보간해야 하는 문자열에 대해 작동하도록 오버로드되었습니다.

**새 동작**

EF Core 3.0부터는 `FromSqlRaw`, `ExecuteSqlRaw` 및 `ExecuteSqlRawAsync`를 사용하여 매개 변수를 생성합니다. 여기서 매개 변수는 보간된 쿼리 문자열의 일부로서 전달됩니다.
예:

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

`FromSqlInterpolated`, `ExecuteSqlInterpolated` 및 `ExecuteSqlInterpolatedAsync`를 사용하여 매개 변수를 생성합니다. 여기서 매개 변수는 보간된 쿼리 문자열의 일부로서 전달됩니다.
예:

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

위의 두 쿼리 모두 같은 SQL 매개 변수를 사용하여 같은 매개 변수가 있는 SQL을 생성합니다.

**이유**

이와 같은 메서드 오버로드를 적용할 경우 보간된 문자열 메서드를 호출하려다 실수로 원시 문자열 메서드를 호출하거나, 반대로 후자를 호출하려다 전자를 호출하는 실수를 저지를 가능성이 매우 높습니다.
쿼리에는 매개 변수가 있어야 하는데, 이 경우 매개 변수가 없는 쿼리가 나올 수 있습니다.

**완화 방법**

새 메서드 이름을 사용하도록 전환합니다.

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a>저장 프로시저와 함께 사용할 경우 FromSql 메서드를 구성할 수 없습니다.

[추적 문제 #15392](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

**이전 동작**

EF Core 3.0 전에는 FromSql 메서드가 전달된 SQL 위에 구성이 가능한지 감지하려고 시도했습니다. SQL이 저장 프로시저처럼 구성 가능하지 않은 경우 클라이언트 평가를 수행했습니다. 다음 쿼리는 서버에서 저장 프로시저를 실행하고 클라이언트 쪽에서 FirstOrDefault를 수행하여 작동했습니다.

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

**새 동작**

EF Core 3.0부터 EF Core가 SQL의 구문 분석을 시도하지 않습니다. 따라서 FromSqlRaw/FromSqlInterpolated 후에 구성하는 경우 EF Core가 하위 쿼리를 발생시켜 SQL을 구성합니다. 구성과 함께 저장 프로시저를 사용하는 경우에는 잘못된 SQL 구문이라는 예외가 발생하게 됩니다.

**이유**

EF Core 3.0은 [여기](#linq-queries-are-no-longer-evaluated-on-the-client)에서 설명하는 것처럼 오류가 많은 자동 클라이언트 평가를 지원하지 않습니다.

**완화**

FromSqlRaw/FromSqlInterpolated에서 저장 프로시저를 사용하는 경우, 이 위에 구성할 수 없다는 사실을 알고 있으므로 FromSql 메서드 호출 직후에 __AsEnumerable/AsAsyncEnumerable__을 추가하여 서버 쪽에서 구성이 이루어지지 않도록 할 수 있습니다.

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a>FromSql 메서드는 쿼리 루트에만 지정할 수 있습니다.

[추적 이슈 #15704](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

**이전 동작**

EF Core 3.0 이전에는 `FromSql` 메서드를 쿼리의 아무 곳에나 지정할 수 있었습니다.

**새 동작**

EF Core 3.0부터는 새로운 `FromSqlRaw` 및 `FromSqlInterpolated` 메서드(`FromSql` 대체)만 쿼리 루트에 지정할 수 있습니다(예: `DbSet<>`에 직접). 다른 위치에 지정하려고 하면 컴파일 오류가 발생합니다.

**이유**

`DbSet`가 아닌 위치에 `FromSql`을 지정하는 것은 추가 의미 또는 추가 가치가 없으므로 특정 시나리오에서 모호성을 발생시킬 수 있습니다.

**완화 방법**

`FromSql` 호출은 이 호출이 적용되는 `DbSet`로 직접 이동해야 합니다.

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a>비 추적 쿼리가 더 이상 ID 확인을 수행하지 않습니다.

[추적 문제 #13518](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

**이전 동작**

EF Core 3.0 이전에는 지정된 형식 및 ID의 엔터티가 발생할 때마다 동일한 엔터티 인스턴스가 사용되었습니다. 이는 추적 쿼리의 동작과 일치합니다. 예를 들어 다음 쿼리를 확인하세요.

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
지정된 범주와 연결된 각 `Product`에 대해 동일한 `Category` 인스턴스를 반환합니다.

**새 동작**

EF Core 3.0부터 지정된 형식 및 ID의 엔터티가 반환된 그래프의 여러 위치에서 발생할 때 여러 엔터티 인스턴스가 생성됩니다. 예를 들어 위 쿼리는 이제 두 제품이 동일한 범주에 연결되어 있는 경우에도 각 `Product`에 대해 새 `Category` 인스턴스를 반환합니다.

**이유**

ID 확인(즉 이전에 발생한 엔터티와 동일한 형식과 ID를 가진 엔터티 확인)이 추가 성능 및 메모리 오버헤드를 추가합니다. 이는 일반적으로 비 추적 쿼리가 먼저 사용되는 이유와 상반됩니다. 또한 ID 확인이 때때로 유용할 수 있지만, 엔터티가 직렬화되고 클라이언트로 전송되는 경우(비 추적 쿼리에 일반적임)에는 필요하지 않습니다.

**완화 방법**

ID 확인이 필요한 경우 추적 쿼리를 사용합니다.

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a>~~쿼리 실행은 디버그 수준에서 로깅됩니다.~~ 되돌림

[추적 문제 #14523](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

EF Core 3.0의 새 구성으로 모든 이벤트의 로그 수준을 애플리케이션에서 지정할 수 있기 때문에 이 변경 내용을 되돌렸습니다. 예를 들어 SQL의 로깅을 `Debug`로 전환하고 `OnConfiguring` 또는 `AddDbContext`에서 명시적으로 수준을 구성할 수 있습니다.
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a>임시 키 값은 더 이상 엔터티 인스턴스에 설정되지 않습니다.

[추적 문제 #12378](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

**이전 동작**

EF Core 3.0 이전에는 임시 값이 데이터베이스에서 생성된 실제 값을 나중에 가질 수 있는 모든 키 속성에 할당되었습니다.
일반적으로 이러한 임시 값은 큰 음수입니다.

**새 동작**

3\.0부터 EF Core는 엔터티의 추적 정보의 일부로 임시 키 값을 저장하고, 키 속성 자체는 변경하지 않고 그대로 둡니다.

**이유**

이 변경은 일부 `DbContext` 인스턴스의 의해 이전에 추적된 엔터티가 다른 `DbContext` 인스턴스로 이동할 때 임시 키 값이 틀리게 영구화되지 않도록 하기 위해 수행되었습니다. 

**완화 방법**

엔터티 간의 연결을 형성하기 위해 외래 키에 기본 키 값을 할당하는 애플리케이션은 기본 키가 저장 생성되고 `Added` 상태의 엔터티에 속하는 경우 이전 동작에 따라 달라질 수 있습니다.
이는 다음과 같은 방법으로 방지할 수 있습니다.
* 저장 생성 키를 사용하지 않습니다.
* 외래 키 값을 설정하는 대신 관계를 형성하도록 탐색 속성을 설정합니다.
* 엔터티의 추적 정보에서 실제 임시 키 값을 가져옵니다.
예를 들어 `context.Entry(blog).Property(e => e.Id).CurrentValue`는 `blog.Id` 자체가 설정되지 않았더라도 임시 값을 반환합니다.

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a>DetectChanges는 저장 생성 키 값을 준수합니다.

[추적 문제 #14616](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

**이전 동작**

EF Core 3.0 이전에는 `DetectChanges`에서 찾은 추적되는 않은 엔터티를 `Added` 상태에서 추적하여 `SaveChanges`가 호출될 때 새로운 행으로 삽입했습니다.

**새 동작**

EF Core 3.0부터 생성된 키 값을 사용하고 일부 키 값을 설정한 경우 `Modified` 상태에서 엔터티를 추적합니다.
즉, 엔터티에 대한 행이 있는 것으로 가정하고 `SaveChanges`가 호출될 때 업데이트됩니다.
키 값이 설정되지 않았거나 엔터티 형식이 생성된 키를 사용하지 않는 경우, 새 엔터티는 이전 버전과 마찬가지로 `Added`로 추적됩니다.

**이유**

이 변경으로 인해 저장 생성 키를 사용하는 동안 연결이 분리된 엔터티 그래프를 사용하여 작업을 보다 쉽고 일관되게 할 수 있습니다.

**완화 방법**

엔터티 형식이 생성된 키를 사용하도록 구성되었지만 키 값이 새 인스턴스에 명시적으로 설정된 경우, 이 변경으로 인해 애플리케이션이 중단될 수 있습니다.
해결 방법은 생성된 값을 사용하지 않도록 키 속성을 명시적으로 구성하는 것입니다.
예를 들어 흐름 API가 있는 경우:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

데이터 주석이 있는 경우:

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a>계단식 삭제는 기본적으로 즉시 발생합니다.

[추적 문제 #10114](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

**이전 동작**

3\.0 이전에는 SaveChanges가 호출될 때까지 EF Core에는 연계 작업(필요한 보안 주체가 삭제되거나 필요한 보안 주체에 대한 관계가 끊어질 때 종속 엔터티 삭제)이 적용되지 않았습니다.

**새 동작**

3\.0부터 EF Core는 트리거 조건이 검색되는 즉시 연계 동작을 적용합니다.
예를 들어, 보안 주체 엔터티를 삭제하기 위해 `context.Remove()`를 호출하면 추적된 모든 관련 필수 종속 항목도 즉시 `Deleted`로 설정됩니다.

**이유**

이 변경으로 인해 `SaveChanges`가 호출되기 _전에_ 삭제될 엔터티를 이해하는 것이 중요한 데이터 바인딩 및 감사 시나리오 환경이 개선되었습니다.

**완화 방법**

이전 동작은 `context.ChangeTracker`의 설정을 통해 복원할 수 있습니다.
예:

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a>관련 엔터티의 즉시 로드가 이제 단일 쿼리에서 이루어집니다.

[추적 문제 issue #18022](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

**이전 동작**

3\.0 전에는 `Include` 연산자를 통한 컬렉션 탐색의 즉시 로드로 인해 관계형 데이터베이스에서 각 관련된 엔터티 형식에 대해 하나씩, 여러 개의 쿼리를 생성했습니다.

**새 동작**

3\.0부터는 EF Core가 관계형 데이터베이스에서 JOIN으로 단일 쿼리를 생성합니다.

**이유**

단일 LINQ 쿼리를 구현하기 위해 여러 개의 쿼리를 발행하는 것은 여러 번의 데이터베이스 왕복이 필요하기 때문에 성능이 저하되고 각 쿼리가 서로 다른 데이터베이스 상태를 관찰할 수 있으므로 데이터 일관성 문제가 발생하는 등 여러 문제의 원인이 되었습니다.

**완화 방법**

엄밀히 말해 이것은 호환성이 손상되는 변경은 아니지만, 단일 쿼리가 컬렉션 탐색에 대해 다량의 `Include` 연산자를 포함하는 경우 애플리케이션 성능에 상당한 영향을 줄 수 있습니다. 자세한 내용 및 보다 효율적인 방법으로 쿼리를 재작성하는 방법은 [이 주석](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085)을 참조하세요.

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a>DeleteBehavior.Restrict에는 명확한 의미 체계가 있습니다.

[추적 문제 #12661](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

**이전 동작**

3\.0 이전에는 `DeleteBehavior.Restrict`가 `Restrict` 의미 체계를 사용하여 데이터베이스에 외래 키를 만들었지만, 확실치 않은 방식으로 fixup도 변경했습니다.

**새 동작**

3\.0부터 `DeleteBehavior.Restrict`는 EF 내부 fixup에 영향을 주지 않고 `Restrict` 의미 체계(즉, 계단식 없음, 제약 조건 위반을 throw함)로 외부 키를 생성하도록 보장합니다.

**이유**

이 변경은 예기치 않은 부작용 없이 직관적인 방식으로 `DeleteBehavior`를 사용하는 환경을 개선하기 위해 이루어졌습니다.

**완화 방법**

이전 동작은 `DeleteBehavior.ClientNoAction`을 사용하여 복원할 수 있습니다.

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a>쿼리 형식은 엔터티 형식과 통합됩니다.

[추적 문제 #14194](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

**이전 동작**

EF Core 3.0 이전에는 [쿼리 형식](xref:core/modeling/keyless-entity-types)이 기본 키를 구조화된 방법으로 정의하지 않는 데이터를 쿼리하는 수단이었습니다.
즉, 쿼리 형식은 키 없이 엔터티 형식을 매핑하는데 사용(보기에서 가능할 수도 있지만 테이블에서도 가능한 경우)되는 반면, 일반 엔터티 형식은 키가 사용 가능할 때(테이블에서 가능하지만 보기에서도 가능한 경우) 사용되었습니다.

**새 동작**

이제 쿼리 형식은 기본 키가 없는 엔터티 형식이 될 뿐입니다.
키 없는 엔터티 형식은 이전 버전의 쿼리 형식과 동일한 기능이 있습니다.

**이유**

이 변경으로 인해 쿼리 형식의 목적에 대한 혼동을 줄일 수 있습니다.
특히 키 없는 엔터티 형식이며 이로 인해 본질적으로 읽기 전용이지만 엔터티 형식이 읽기 전용으로 할 필요가 있다고 해서 사용해서는 안됩니다.
마찬가지로 보기에 매핑되는 경우가 있지만 이는 보기가 키를 정의하지 않는 경우가 있기 때문입니다.

**완화 방법**

API의 다음 부분은 이제 사용되지 않습니다.
* **`ModelBuilder.Query<>()`** - 대신 엔터티 형식을 키가 없는 것으로 표시하기 위해 `ModelBuilder.Entity<>().HasNoKey()`를 호출해야 합니다.
기본 키가 필요하지만 규칙과 일치하지 않는 경우 잘못된 구성을 피하기 위해 규칙에 따라 구성되지 않습니다.
* **`DbQuery<>`** - 대신 `DbSet<>`를 사용해야 합니다.
* **`DbContext.Query<>()`** - 대신 `DbContext.Set<>()`를 사용해야 합니다.

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a>소유 형식 관계에 대한 구성 API가 변경됨

[추적 문제 #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[추적 문제 #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[추적 문제 #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)

**이전 동작**

EF Core 3.0 이전에는 소유 관계의 구성은 `OwnsOne` 또는 `OwnsMany` 호출 직후에 수행되었습니다. 

**새 동작**

EF Core 3.0부터 이제 `WithOwner()`를 사용하여 소유자에게 탐색 속성을 구성하는 흐름 API가 있습니다.
예:

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

이제 소유자와 소유 간의 관계와 관련된 구성이 다른 관계가 구성되는 것과 유사하게 `WithOwner()` 후에 연결되어야 합니다.
소유 형식 자체에 대한 구성은 `OwnsOne()/OwnsMany()` 이후에도 여전히 연결됩니다.
예:

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

또한 소유 형식 대상으로 `Entity()`, `HasOne()` 또는 `Set()`를 호출하면 예외가 throw됩니다.

**이유**

이 변경으로 인해 소유 형식 자체의 구성과 소유 형식의 _관계 대상_ 사이를 명확하게 분리하게 되었습니다.
이는 `HasForeignKey`와 같은 메서드에 대한 모호성과 혼란을 제거합니다.

**완화 방법**

위의 예와 같이 소유 형식 관계의 구성을 변경하여 새 API 화면을 사용합니다.

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>보안 주체와 테이블을 공유하는 종속 엔터티는 이제 선택 사항입니다.

[추적 문제 #9005](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

**이전 동작**

다음 모델을 살펴보세요.
```csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```
EF Core 3.0 전에는 `OrderDetails`를 `Order`가 소유하거나 같은 테이블에 명시적으로 매핑되는 경우 새 `Order`를 추가할 때 `OrderDetails` 인스턴스가 언제나 필수였습니다.


**새 동작**

3\.0부터는 EF Core가 `OrderDetails` 없이 `Order`를 추가할 수 있으며 기본 키를 제외한 모든 `OrderDetails` 속성을 Null 허용 열에 매핑할 수 있습니다.
EF Core를 쿼리하는 경우 해당 필수 속성에 값이 없거나 기본 키 이외의 필수 속성이 없고 모든 속성이 `null`이면 `OrderDetails`를 `null`로 설정합니다.

**완화 방법**

사용하는 모델이 모든 선택적 열에 따라 다르게 공유되는 테이블을 포함하지만 해당 테이블을 가리키는 탐색이 `null`로 예상되지 않는 경우 `null`을 탐색하는 경우를 처리하도록 애플리케이션을 수정해야 합니다. 이렇게 할 수 없는 경우 엔터티 형식에 필수 속성을 추가하거나 하나 이상의 속성에 `null`이 아닌 값을 할당해야 합니다.

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a>동시 토큰 열을 사용하여 테이블을 공유하는 모든 엔터티는 해당 열을 속성에 매핑해야 합니다.

[추적 문제 #14154](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

**이전 동작**

다음 모델을 살펴보세요.
```csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
EF Core 3.0 전에는 `OrderDetails`를 `Order`가 소유하거나 같은 테이블에 명시적으로 매핑하는 경우 `OrderDetails`만 업데이트하면 클라이언트의 `Version` 값이 업데이트되지 않으며 다음 업데이트가 실패합니다.


**새 동작**

3\.0부터 EF Core는 `OrderDetails`를 소유하는 경우 새 `Version` 값을 `Order`에 전파합니다. 그렇지 않으면 모델 유효성 검사 중에 예외가 발생합니다.

**이유**

같은 테이블에 매핑된 엔터티 중 한 개만 업데이트하는 경우 부실 동시성 토큰 값을 방지하기 위해 이렇게 변경되었습니다.

**완화 방법**

테이블을 공유하는 모든 엔터티는 동시성 토큰 열에 매핑되는 속성을 포함해야 합니다. 해당 속성을 섀도 상태로 만들 수 있습니다.
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a>추적 쿼리를 사용하는 소유자 없이 소유된 엔터티를 쿼리할 수 없습니다.

[추적 문제 #18876](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

**이전 동작**

EF Core 3.0 전에는 소유된 엔터티를 다른 탐색으로 쿼리할 수 있었습니다.

```csharp
context.People.Select(p => p.Address);
```

**새 동작**

3\.0부터 추적 쿼리가 소유자 없이 소유된 엔터티를 프로젝션하는 경우 EF Core가 throw됩니다.

**이유**

소유된 엔터티는 소유자 없이 조작할 수 없으므로, 이러한 방식으로 쿼리하면 대부분의 경우에는 오류가 발생합니다.

**완화 방법**

나중에 어떤 방식으로든 소유된 엔터티를 수정해야 하는 경우, 소유자를 쿼리에 포함해야 합니다.

그렇지 않으면 `AsNoTracking()` 호출을 추가합니다.

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a>매핑되지 않은 형식에서 상속된 속성은 이제 모든 파생 형식에 대해 단일 열에 매핑됩니다.

[추적 문제 #13998](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

**이전 동작**

다음 모델을 살펴보세요.
```csharp
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

EF Core 3.0 전에는 `ShippingAddress` 속성이 기본적으로 `BulkOrder` 및 `Order`에 대한 별도의 열에 매핑되었습니다.

**새 동작**

3\.0부터 EF Core는 `ShippingAddress`에 대한 열을 한 개만 생성합니다.

**이유**

이전 동작은 예상할 수 없었습니다.

**완화 방법**

이 속성을 여전히 파생 형식에 대한 별도의 열에 명시적으로 매핑할 수 있습니다.

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

<a name="fkp"></a>

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a>외래 키 속성 규칙이 더 이상 보안 주체 속성과 동일한 이름을 일치시키지 않습니다.

[추적 문제 #13274](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

**이전 동작**

다음 모델을 살펴보세요.
```csharp
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```
EF Core 3.0 이전에는 규칙에 따라 외래 키에 대해 `CustomerId` 속성이 사용되었습니다.
그러나 `Order`가 소유 형식인 경우 `CustomerId`도 기본 키가 되며 이는 일반적으로 예상되는 것은 아닙니다.

**새 동작**

3\.0부터 EF Core는 보안 주체 속성과 동일한 이름을 가지는 경우 규칙에 따라 외래 키에 대한 속성을 사용하려고 하지 않습니다.
보안 주체 속성 이름과 연결된 보안 주체 유형 이름 및 보안 주체 속성 이름 패턴과 연결된 탐색 이름은 여전히 일치합니다.
예:

```csharp
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```csharp
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

**이유**

이 변경으로 인해 소유 형식에서 기본 키 속성을 잘못 정의하지 않게 되었습니다.

**완화 방법**

속성을 외래 키로 하고, 이에 따라서 기본 키의 일부가 되는 경우 명시적으로 이와 같이 구성합니다.

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a>데이터베이스 연결은 이제 TransactionScope가 완료되기 전에 더 이상 사용되지 않으면 닫힙니다.

[추적 문제 #14218](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

**이전 동작**

EF Core 3.0 전에는 컨텍스트가 `TransactionScope` 내에서 연결을 여는 경우 현재 `TransactionScope`가 활성화되어 있는 동안 해당 연결이 열린 상태로 유지됩니다.

```csharp
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point

        var categories = context.ProductCategories().ToList();
    }
}
```

**새 동작**

3\.0부터 EF Core는 연결의 사용이 완료되면 해당 연결을 즉시 닫습니다.

**이유**

같은 `TransactionScope`에 복수의 컨텍스트를 사용할 수 있도록 이렇게 변경했습니다. 새 동작은 EF6과도 일치합니다.

**완화 방법**

연결이 열린 상태로 유지되어야 하는 경우 `OpenConnection()`을 명시적으로 호출하여 EF Core가 연결을 조기에 닫지 않도록 합니다.

```csharp
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

<a name="each"></a>

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a>각 속성은 독립적인 메모리 내 정수 키 생성을 사용합니다.

[추적 문제 #6872](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

**이전 동작**

EF Core 3.0 이전에는 모든 메모리 내 정수 키 속성에 대해 하나의 공유 값 생성기가 사용되었습니다.

**새 동작**

EF Core 3.0부터 메모리 내 데이터베이스를 사용하는 경우 각 정수 키 속성은 자체 값 생성기를 가져옵니다.
또한 데이터베이스가 삭제되면 모든 테이블에 대해 키 생성이 재설정됩니다.

**이유**

이 변경으로 인해 메모리 내 키 생성을 실제 데이터베이스 키 생성에 보다 가깝게 정렬하고 메모리 내 데이터베이스를 사용할 때 테스트를 서로 격리하는 기능이 향상되었습니다.

**완화 방법**

이로 인해 특정 메모리 내 키 값을 사용하는 애플리케이션이 중단될 수 있습니다.
대신 특정 키 값을 사용하지 않거나 새 동작과 일치하도록 업데이트하지 않도록 합니다.

### <a name="backing-fields-are-used-by-default"></a>지원 필드는 기본적으로 사용됩니다.

[추적 문제 #12430](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

**이전 동작**

3\.0 이전에는 속성에 대한 지원 필드가 알려져 있더라도 EF Core는 기본적으로 속성 getter 및 setter 메서드를 사용하여 속성 값을 읽고 작성했습니다.
이에 대한 예외는 쿼리 실행으로 알려진 경우 지원 필드가 직접 설정되었습니다.

**새 동작**

EF Core 3.0부터 속성 지원 필드가 알려진 경우 EF Core는 항상 지원 필드를 사용하여 해당 속성을 읽고 씁니다.
이로 인해 애플리케이션이 getter 또는 setter 메서드로 코딩된 추가 동작을 사용하는 경우 애플리케이션이 중단될 수 있습니다.

**이유**

이 변경으로 인해 엔터티가 포함된 데이터베이스 작업을 수행할 때 EF Core가 기본적으로 비즈니스 논리를 잘못 트리거하는 것을 방지합니다.

**완화 방법**

`ModelBuilder`에서 속성 액세스 모드의 구성을 통해 3.0 이전 버전의 동작을 복원할 수 있습니다.
예:

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a>호환이 가능한 여러 지원 필드가 발견되면 throw됩니다.

[추적 문제 #12523](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

**이전 동작**

EF Core 3.0 이전에는 여러 필드가 속성의 지원 필드를 찾기 위한 규칙과 일치하면 우선순위에 따라 하나의 필드가 선택되었습니다.
이로 인해 모호한 경우 잘못된 필드가 사용될 수 있습니다.

**새 동작**

EF Core 3.0부터 여러 필드가 동일한 속성과 일치하면 예외가 throw됩니다.

**이유**

이 변경으로 인해 하나의 필드만 정확할 때 다른 필드보다 한 필드만 자동으로 사용하는 것을 방지했습니다.

**완화 방법**

모호한 지원 필드가 있는 속성은 사용할 필드가 명시적으로 지정되어야 합니다.
예를 들어 흐름 API 사용:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a>필드 전용 속성 이름은 필드 이름과 일치해야 합니다.

**이전 동작**

EF Core 3.0 이전에는 문자열 값으로 속성을 지정할 수 있었고 .NET 형식에서 해당 이름을 가진 속성을 찾을 수 없는 경우, EF Core는 변환 규칙을 사용하여 필드에 일치시키려고 했습니다.

```csharp
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

**새 동작**

EF Core 3.0부터 필드 전용 속성은 필드 이름과 정확히 일치해야 합니다.

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

**이유**

이 변경은 유사한 이름의 두 속성에 대해 동일한 필드를 사용하지 않도록 하기 위해 이루어졌으며, 필드 전용 속성에 대한 일치 규칙을 CLR 속성에 매핑된 속성과 동일하게 만듭니다.

**완화 방법**

필드 전용 속성의 이름은 매핑되는 필드와 동일한 이름을 지정해야 합니다.
3\.0 이후 EF Core의 향후 릴리스에서는 속성 이름과 다른 필드 이름을 명시적으로 다시 사용하도록 설정할 계획입니다([#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307) 이슈 참조).

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a>AddDbContext/AddDbContextPool이 더 이상 AddLogging 및 AddMemoryCache를 호출하지 않음

[추적 문제 #14756](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

**이전 동작**

EF Core 3.0 이전에는, `AddDbContext` 또는 `AddDbContextPool`을(를) 호출하면 [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) 및 [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)에 대한 호출을 통해 DI를 사용하여 로깅 및 메모리 캐싱 서비스도 등록되었습니다.

**새 동작**

EF Core 3.0부터 `AddDbContext` 및 `AddDbContextPool`은 DI(종속성 주입)를 사용하여 이러한 서비스를 더 이상 등록하지 않습니다.

**이유**

EF Core 3.0에서는 이러한 서비스가 애플리케이션의 DI 컨테이너에 있을 필요가 없습니다. 그러나 `ILoggerFactory`가 애플리케이션의 DI 컨테이너에 등록된 경우 EF Core에서 여전히 DI를 사용합니다.

**완화 방법**

애플리케이션에 이러한 서비스가 필요한 경우 [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) 또는 [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)를 사용하여 DI 컨테이너에 명시적으로 등록합니다.

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a>AddEntityFramework*에서 IMemoryCache를 크기 제한하여 추가합니다.

[추적 문제 #12905](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

**이전 동작**

EF Core 3.0 전에는 `AddEntityFramework*` 메서드를 호출하여 크기 제한 없이 DI로 메모리 캐싱 서비스를 등록합니다.

**새 동작**

EF Core 3.0부터 `AddEntityFramework*`은(는) 크기 제한으로 IMemoryCache 서비스를 등록합니다. 이후에 추가된 다른 서비스가 IMemoryCache에 따라 달라지는 경우 예외 또는 성능 저하를 야기하는 기본 제한에 빠르게 도달할 수 있습니다.

**이유**

제한 없이 IMemoryCache를 사용하면 쿼리 캐싱 논리에 버그가 있거나 쿼리가 동적으로 생성되는 경우 제어되지 않는 메모리 사용이 발생할 수 있습니다. 기본 제한을 사용하며 잠재적인 DoS 공격을 완화할 수 있습니다.

**완화 방법**

대부분의 경우 `AddDbContext` 또는 `AddDbContextPool`을(를) 호출하면 `AddEntityFramework*`을(를) 호출할 필요가 없습니다. 따라서 가장 좋은 완화 방법은 `AddEntityFramework*` 호출을 제거하는 것입니다.

애플리케이션에 이 서비스가 필요한 경우 [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)를 사용하여 미리 DI 컨테이너로 IMemoryCache 구현을 명시적으로 등록합니다.

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a>DbContext.Entry는 이제 로컬 DetectChanges를 수행합니다.

[추적 문제 #13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

**이전 동작**

EF Core 3.0 이전에는 `DbContext.Entry`를 호출하면 추적된 모든 엔터티에 대해 변경 내용이 검색되었습니다.
이로 인해 `EntityEntry`에 노출된 상태가 최신 상태로 유지되었습니다.

**새 동작**

EF Core 3.0부터 `DbContext.Entry` 호출은 지정된 엔터티와 이와 관련된 추적된 모든 주체 엔터티의 변경 내용만 검색하려고 합니다.
즉, 이 메서드를 호출하여 다른 곳의 변경 내용을 검색하지 못할 수 있으므로 애플리케이션 상태에 영향을 줄 수 있습니다.

`ChangeTracker.AutoDetectChangesEnabled`를 `false`로 설정하면 이 로컬 변경 검색도 비활성화됩니다.

변경 검색(예: `ChangeTracker.Entries` 및 `SaveChanges`)을 유발하는 다른 메서드는 추적된 모든 엔터티의 전체 `DetectChanges`를 유발합니다.

**이유**

이 변경으로 인해 `context.Entry`를 사용하는 기본 성능이 향상되었습니다.

**완화 방법**

`Entry`를 호출하기 전에 명시적으로 `ChangeTracker.DetectChanges()`를 호출하여 3.0 이전 버전의 동작을 확인합니다.

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a>문자열 및 바이트 배열 키는 기본적으로 클라이언트에서 생성되지 않습니다.

[추적 문제 #14617](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

**이전 동작**

EF Core 3.0 이전에는 null이 아닌 값을 명시적으로 설정하지 않고도 `string` 및 `byte[]` 키 속성을 사용할 수 있었습니다.
이 경우 키 값은 클라이언트에서 GUID로 생성되고 `byte[]`의 바이트로 직렬화됩니다.

**새 동작**

EF Core 3.0부터 키 값이 설정되지 않았음을 나타내는 예외가 throw됩니다.

**이유**

클라이언트에서 생성한 `string`/`byte[]` 값은 일반적으로 유용하지 않고 기본 동작으로 인해 생성된 키 값을 일반적인 방식으로 추론하기가 어려웠기 때문에 이 변경이 이루어졌습니다.

**완화 방법**

다른 null이 아닌 값이 설정되지 않은 경우 키 속성이 생성된 값을 사용해야 함을 명시적으로 지정하면 3.0 이전 버전 동작을 얻을 수 있습니다.
예를 들어 흐름 API가 있는 경우:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

데이터 주석이 있는 경우:

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a>ILoggerFactory는 이제 범위가 지정된 서비스입니다.

[추적 문제 #14698](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

**이전 동작**

EF Core 3.0 이전에는 `ILoggerFactory`가 싱글톤 서비스로 등록되었습니다.

**새 동작**

EF Core 3.0부터 이제 `ILoggerFactory`는 범위가 지정된 대로 등록됩니다.

**이유**

이 변경으로 인해 `DbContext` 인스턴스와 로거를 연결하여 다른 기능을 활성화하고 내부 서비스 공급자의 급증과 같은 병리적인 동작의 일부 사례가 제거됩니다.

**완화 방법**

이 변경 내용은 EF Core 내부 서비스 공급자에 사용자 지정 서비스를 등록하고 사용하지 않는 한 애플리케이션 코드에 영향을 미치지 않아야 합니다.
이는 일반적이지 않습니다.
이러한 경우 대부분의 작업은 여전히 작동하지만 `ILoggerFactory`에 종속된 싱글톤 서비스는 `ILoggerFactory`를 얻기 위해 다른 방식으로 변경해야 합니다.

이와 같은 상황에 처한 경우 [EF Core GitHub 문제 추적기](https://github.com/aspnet/EntityFrameworkCore/issues)에 문제를 제출하여 향후 이 문제를 해결할 수 있는 방법을 더 잘 이해할 수 있도록 `ILoggerFactory`를 사용하는 방법을 알려주세요.

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a>지연 로드 프록시는 더 이상 탐색 속성이 완전히 로드되었다고 가정하지 않습니다.

[추적 문제 #12780](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

**이전 동작**

EF Core 3.0 이전에는 `DbContext`가 삭제되면 해당 컨텍스트에서 가져온 엔터티의 지정된 탐색 속성이 완전히 로드되었는지 여부를 알 수 없었습니다.
대신 프록시는 참조 탐색에 null이 아닌 값이 있으면 로드되고 컬렉션 탐색이 비어 있지 않으면 로드된다고 가정합니다.
이러한 경우 지연 로드를 시도하면 아무런 작업도 수행되지 않습니다.

**새 동작**

EF Core 3.0부터 프록시는 탐색 속성이 로드되었는지 여부를 추적합니다.
즉, 컨텍스트가 삭제된 후에 로드되는 탐색 속성에 액세스하려고 하면 로드된 탐색이 비어 있거나 null인 경우에도 항상 아무런 작업도 수행되지 않습니다.
반대로, 로드되지 않은 탐색 속성에 액세스하려고 하면 탐색 속성이 비어 있지 않은 컬렉션이라도 컨텍스트가 삭제되면 예외가 throw됩니다.
이 상황이 발생하는 경우 애플리케이션 코드가 잘못된 시간에 지연 로드를 사용하려고 시도하고 있음을 의미하며, 애플리케이션이 이를 수행하지 않도록 변경해야 합니다.

**이유**

이 변경으로 인해 삭제된 `DbContext` 인스턴스에 지연 로드를 시도할 때 동작을 일관되고 정확하게 만듭니다.

**완화 방법**

애플리케이션을 업데이트하여 삭제된 컨텍스트로 지연 로드를 시도하지 않도록 하거나 예외 메시지에 설명된 대로 작업을 수행하지 않도록 구성합니다.

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a>내부 서비스 공급자의 과도한 생성은 이제 기본적으로 오류입니다.

[추적 문제 #10236](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

**이전 동작**

EF Core 3.0 이전에는 병리적인 수의 내부 서비스 공급자를 만드는 애플리케이션에 경고가 기록되었습니다.

**새 동작**

EF Core 3.0부터 이제 이 경고가 고려되며 오류와 예외가 throw됩니다. 

**이유**

이 변경으로 인해 병리적인 사례를 더 명시적으로 노출하여 더 나은 애플리케이션 코드를 구동하게 되었습니다.

**완화 방법**

이 오류가 발생했을 때 가장 적절한 조치는 근본 원인을 이해하고 많은 내부 서비스 공급자를 만드는 것을 중단하는 것입니다.
그러나 오류는 `DbContextOptionsBuilder`의 구성을 통해 경고(또는 무시)로 다시 변환될 수 있습니다.
예:

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a>단일 문자열로 호출되는 HasOne/HasMany의 새 동작

[추적 문제 #9171](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

**이전 동작**

EF Core 3.0 이전에는 단일 문자열을 사용하여 `HasOne` 또는 `HasMany`를 호출하는 코드가 혼란스러운 방식으로 해석되었습니다.
예:
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

코드는 비공개일 수 있는 `Entrance` 탐색 속성을 사용하여 `Samurai`를 다른 엔터티 형식과 관련시키려는 것 같습니다.

실제로 이 코드는 탐색 속성을 사용하지 않고 `Entrance`라고 하는 엔터티 형식과 관계를 생성하려고 합니다.

**새 동작**

EF Core 3.0부터 이제 위 코드는 이전에 수행했어야 하는 것처럼 보이는 방식으로 동작합니다.

**이유**

이전 동작은 특히 구성 코드를 읽고 오류를 찾을 때 매우 혼란스럽습니다.

**완화 방법**

새 동작은 형식 이름의 문자열을 사용하고 탐색 속성을 명시적으로 지정하지 않은 채 명시적으로 관계를 구성하는 애플리케이션만 중단됩니다.
이는 일반적이지 않습니다.
이전 동작은 탐색 속성 이름에 대해 `null`을 전달하여 명시적으로 얻을 수 있습니다.
예:

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a>여러 비동기 메서드의 반환 형식이 작업에서 ValueTask로 변경되었습니다.

[추적 문제 #15184](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

**이전 동작**

다음 비동기 메서드는 이전에 `Task<T>`를 반환했습니다.

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* `ValueGenerator.NextValueAsync()`(및 파생 클래스)

**새 동작**

앞서 언급한 메서드는 이제 이전과 같은 `T`를 통해 `ValueTask<T>`를 반환합니다.

**이유**

이 변경은 해당 메서드를 호출할 때 발생하는 힙 할당 수를 줄여서 일반적인 성능을 개선합니다.

**완화 방법**

단순히 위의 API를 대기 중인 애플리케이션은 다시 컴파일하기만 하면 됩니다. 소스 변경은 필요 없습니다.
더 복잡한 사용(예: 반환된 `Task`를 `Task.WhenAny()`에 전달)의 경우 일반적으로 반환된 `ValueTask<T>`에 대해 `AsTask()`를 호출하여 `Task<T>`로 변환해야 합니다.
이렇게 하면 이 변경으로 인한 할당 감소가 무효화됩니다.

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a>관계형:TypeMapping 주석은 이제 TypeMapping일 뿐입니다.

[추적 문제 #9913](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

**이전 동작**

형식 매핑 주석에 대한 주석 이름은 "관계형:TypeMapping"이었습니다.

**새 동작**

형식 매핑 주석에 대한 주석 이름은 이제 "TypeMapping"입니다.

**이유**

이제 형식 매핑은 관계형 데이터베이스 공급자 이상의 용도로 사용됩니다.

**완화 방법**

이는 일반적이지 않은 주석으로 형식 매핑에 직접 액세스하는 애플리케이션만 중단합니다.
수정하기 위한 가장 적절한 작업은 직접 주석을 사용하는 대신 API 화면을 사용하여 형식 매핑에 액세스하는 것입니다.

### <a name="totable-on-a-derived-type-throws-an-exception"></a>파생된 형식의 ToTable에서 예외가 throw됩니다. 

[추적 문제 #11811](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

**이전 동작**

EF Core 3.0 이전에는 유효하지 않은 경우 상속 매핑 전략만 TPH이므로 파생된 형식에 대해 호출된 `ToTable()`은 무시되었습니다. 

**새 동작**

EF Core 3.0부터 시작하여 이후 릴리스에서 TPT 및 TPC 지원을 추가하기 위해 파생 형식에서 호출된 `ToTable()`에서는 나중에 예기치 않은 매핑 변화를 방지하기 위해 예외가 throw됩니다.

**이유**

현재 파생된 형식을 다른 테이블에 매핑하는 것은 올바르지 않습니다.
이 변경으로 인해 할 일이 유효하게 될 때 나중에 중단되는 것을 방지할 수 있습니다.

**완화 방법**

파생된 형식을 다른 테이블에 매핑하려는 시도를 제거합니다.

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a>ForSqlServerHasIndex가 HasIndex로 바뀝니다. 

[추적 문제 #12366](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

**이전 동작**

EF Core 3.0 이전에는 `ForSqlServerHasIndex().ForSqlServerInclude()`가 `INCLUDE`와 함께 사용되는 열을 구성하는 방법을 제공했습니다.

**새 동작**

EF Core 3.0부터 인덱스에 `Include`를 사용하여 이제 관계형 수준에서 지원됩니다.
`HasIndex().ForSqlServerInclude()`을 사용하십시오.

**이유**

이 변경으로 인해 `Include`가 있는 인덱스의 API를 모든 데이터베이스 공급자에 대해 한 곳으로 통합할 수 있습니다.

**완화 방법**

위에 표시된 것처럼 새 API를 사용합니다.

### <a name="metadata-api-changes"></a>메타데이터 API 변경 내용

[추적 문제 #214](https://github.com/aspnet/EntityFrameworkCore/issues/214)

**새 동작**

다음 속성이 확장 메서드로 변환되었습니다.

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

**이유**

이 변경은 앞서 언급한 인터페이스의 구현을 단순화합니다.

**완화 방법**

새로운 확장 메서드를 사용합니다.

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a>공급자 고유의 메타데이터 API 변경 내용

[추적 문제 #214](https://github.com/aspnet/EntityFrameworkCore/issues/214)

**새 동작**

공급자 고유의 확장 메서드가 평면화됩니다.

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

**이유**

이 변경은 앞서 언급한 확장 메서드의 구현을 단순화합니다.

**완화 방법**

새로운 확장 메서드를 사용합니다.

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a>EF Core는 더 이상 SQLite FK 적용을 위한 pragma를 보내지 않습니다.

[추적 문제 #12151](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

**이전 동작**

EF Core 3.0 이전에는 EF Core가 SQLite에 대한 연결이 열릴 때 `PRAGMA foreign_keys = 1`을 보냈습니다.

**새 동작**

EF Core 3.0부터 EF Core는 더 이상 SQLite에 대한 연결이 열릴 때 `PRAGMA foreign_keys = 1`을 보내지 않습니다.

**이유**

이 변경은 EF Core가 기본적으로 `SQLitePCLRaw.bundle_e_sqlite3`을 사용하기 때문에 이루어졌으며, 이는 FK 적용이 기본적으로 켜져 있고 연결이 열릴 때마다 명시적으로 활성화할 필요가 없음을 의미합니다.

**완화 방법**

외래 키는 기본적으로 EF Core에 사용되는 SQLitePCLRaw.bundle_e_sqlite3에서 기본적으로 활성화됩니다.
다른 경우에는 연결 문자열에 `Foreign Keys=True`를 지정하여 외래 키를 활성화할 수 있습니다.

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a>Microsoft.EntityFrameworkCore.Sqlite는 이제 SQLitePCLRaw.bundle_e_sqlite3에 종속됩니다.

**이전 동작**

EF Core 3.0 이전에는 EF Core가 `SQLitePCLRaw.bundle_green`을 사용했습니다.

**새 동작**

EF Core 3.0부터 EF Core가 `SQLitePCLRaw.bundle_e_sqlite3`을 사용합니다.

**이유**

이 변경으로 인해 iOS에서 사용되는 SQLite의 버전이 다른 플랫폼과 일치되도록 할 수 있습니다.

**완화 방법**

iOS에서 네이티브 SQLite 버전을 사용하려면 다른 `SQLitePCLRaw` 번들을 사용하도록 `Microsoft.Data.Sqlite`를 구성합니다.

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a>Guid 값은 이제 SQLite에 텍스트로 저장되었습니다.

[추적 문제 #15078](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

**이전 동작**

Guid 값은 이전에 SQLite에 BLOB 값으로 저장되었습니다.

**새 동작**

Guid 값은 이제 텍스트로 저장됩니다.

**이유**

Guid의 이진 형식은 표준화되지 않았습니다. 값을 텍스트로 저장하면 데이터베이스가 다른 기술과 더 잘 호환됩니다.

**완화 방법**

다음과 같이 SQL을 실행하여 기존 데이터베이스를 새 형식으로 마이그레이션할 수 있습니다.

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

EF Core에서는 이러한 속성에 값 변환기를 구성하여 이전 동작을 계속 사용할 수도 있습니다.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

Microsoft.Data.Sqlite는 BLOB 및 텍스트 열 모두에서 Guid 값을 읽을 수 있습니다. 하지만 매개 변수 및 상수에 대한 기본 형식이 변경되었으므로 Guid를 포함하는 대부분의 시나리오에서 조치를 취해야 합니다.

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a>Char 값은 이제 SQLite에 텍스트로 저장됨

[추적 문제 #15020](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

**이전 동작**

Char 값은 이전에 SQLite에 정수 값으로 저장되었습니다. 예를 들어, *A*의 char 값은 정수 값 65로 저장되었습니다.

**새 동작**

Char 값은 이제 텍스트로 저장됩니다.

**이유**

값을 텍스트로 저장하는 것이 더 자연스럽고 데이터베이스가 다른 기술과 더 잘 호환됩니다.

**완화 방법**

다음과 같이 SQL을 실행하여 기존 데이터베이스를 새 형식으로 마이그레이션할 수 있습니다.

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

EF Core에서는 이러한 속성에 값 변환기를 구성하여 이전 동작을 계속 사용할 수도 있습니다.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

또한 Microsoft.Data.Sqlite는 정수 및 텍스트 열에서 문자 값을 읽을 수 있으므로 특정 시나리오에서는 아무런 조치도 필요하지 않을 수 있습니다.

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a>이제 마이그레이션 ID가 고정 문화권의 달력을 사용하여 생성됨

[추적 문제 #12978](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

**이전 동작**

마이그레이션 ID는 현재 문화권의 달력을 사용하여 의도치 않게 생성되었습니다.

**새 동작**

이제 마이그레이션 ID는 항상 고정 문화권의 달력(그레고리력)을 사용하여 생성됩니다.

**이유**

데이터베이스를 업데이트하거나 병합 충돌을 해결할 때 마이그레이션 순서가 중요합니다. 고정 달력을 사용하면 팀 멤버가 서로 다른 시스템 캘린더로 인해 발생할 수 있는 주문 문제를 방지할 수 있습니다.

**완화 방법**

이 변경은 그레고리력(예: 태국 불교식 달력)보다 큰 년도가 있는 그레고리력이 아닌 달력을 사용하는 사용자에게 영향을 줍니다. 기존 마이그레이션 ID를 업데이트하여 기존 마이그레이션 후 새 마이그레이션 순서를 지정해야 합니다.

마이그레이션 ID는 마이그레이션 디자이너 파일의 마이그레이션 속성에서 찾을 수 있습니다.

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

마이그레이션 기록 테이블도 업데이트해야 합니다.

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a>UseRowNumberForPaging가 제거되었습니다.

[추적 문제 #16400](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

**이전 동작**

EF Core 3.0 이전에는 `UseRowNumberForPaging`으로 SQL Server 2008과 호환되는 페이징을 위한 SQL을 생성할 수 있었습니다.

**새 동작**

EF Core 3.0부터 EF는 SQL Server 2008 이후 버전하고만 호환되는 페이징을 위한 SQL만 생성합니다. 

**이유**

[SQL Server 2008은 더 이상 지원되는 제품이 아니며](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/), EF Core 3.0에서 수행되는 쿼리 변경 내용에 맞게 이 기능을 업데이트하는 것이 중요하므로 이 변경 작업을 수행하고 있습니다.

**완화 방법**

생성된 SQL이 지원을 받을 수 있도록 최신 버전의 SQL Server 또는 더 높은 호환성 수준을 사용하여 업데이트하는 것이 좋습니다. 따라서, 이 작업을 수행할 수 없는 경우 세부 정보를 사용하여 [추적 문제에 대해 설명](https://github.com/aspnet/EntityFrameworkCore/issues/16400)해주세요. 사용자 의견에 따라 이 결정을 다시 검토할 수 있습니다.

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a>확장 정보/메타데이터가 IDbContextOptionsExtension에서 제거되었습니다.

[추적 이슈 #16119](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

**이전 동작**

`IDbContextOptionsExtension`은 확장에 대한 메타데이터를 제공하기 위한 메서드를 포함합니다.

**새 동작**

이러한 메서드가 새 `IDbContextOptionsExtension.Info` 속성에서 반환된 새 `DbContextOptionsExtensionInfo` 추상 기본 클래스로 옮겨졌습니다.

**이유**

2\.0부터 3.0까지의 릴리스에서 이러한 메서드를 추가하거나 여러 번 변경해야 했습니다.
해당 메서드를 새 추상 기본 클래스로 세분화하면 기존 확장을 중단하지 않고도 이러한 종류의 변경을 더 쉽게 할 수 있습니다.

**완화 방법**

새 패턴에 따라 확장을 업데이트합니다.
EF Core 소스 코드에서 다양한 종류의 확장을 위해 `IDbContextOptionsExtension`을 여러 번 구현한 예가 확인되었습니다.

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a>LogQueryPossibleExceptionWithAggregateOperator 이름이 바뀌었습니다.

[추적 문제 #10985](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

**변경 내용**

`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` 이름이 `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`으로 바뀌었습니다.

**이유**

이 경고 이벤트의 이름 지정을 다른 모든 경고 이벤트에 맞춥니다.

**완화 방법**

새 이름을 사용합니다. (이벤트 ID 번호가 변경되지 않았습니다.)

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a>외래 키 제약 조건 이름에 대한 API를 명확히 합니다.

[추적 문제 #10730](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

**이전 동작**

EF Core 3.0 이전에 외래 키 제약 조건 이름은 "이름"과 같이 간단했습니다. 예:

```csharp
var constraintName = myForeignKey.Name;
```

**새 동작**

이제 EF Core 3.0부터 외래 키 제약 조건 이름은 “제약 조건 이름”이라고 합니다. 예:

```csharp
var constraintName = myForeignKey.ConstraintName;
```

**이유**

이러한 변경으로 인해 이 영역에서 이름을 일관되게 지정하고, 또한 일관되게 외래 키가 정의된 열 또는 속성 이름이 아닌 외래 키 제약 조건의 이름임을 명확히 합니다.

**완화 방법**

새 이름을 사용합니다.

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a>IRelationalDatabaseCreator.HasTables/HasTablesAsync가 공개되었습니다.

[추적 이슈 #15997](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

**이전 동작**

EF Core 3.0 이전에는 이러한 메서드가 비공개였습니다.

**새 동작**

EF Core 3.0부터 이러한 메서드는 공개입니다.

**이유**

이러한 메서드는 데이터베이스가 생성되었지만 비어 있는지 확인하기 위해 EF에서 사용됩니다. 이것은 EF 외부에서 마이그레이션을 적용할지 여부를 결정할 때도 유용합니다.

**완화 방법**

재정의의 접근성을 변경합니다.

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a>이제 Microsoft.EntityFrameworkCore.Design은 DevelopmentDependency 패키지입니다.

[추적 이슈 #11506](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

**이전 동작**

EF Core 3.0 이전의 Microsoft.EntityFrameworkCore.Design은 정규 NuGet 패키지였으며, 이 패키지에 종속된 프로젝트에서 해당 어셈블리를 참조할 수 있었습니다.

**새 동작**

EF Core 3.0부터는 DevelopmentDependency 패키지입니다. 따라서 종속성이 다른 프로젝트로 전이되지 않으며 더는 기본적으로 해당 어셈블리를 참조할 수 없습니다.

**이유**

이 패키지는 디자인 타임에만 사용할 수 있습니다. 배포된 애플리케이션은 해당 패키지를 참조할 수 없습니다. 패키지를 DevelopmentDependency로 만들면 이 권장 사항이 강화됩니다.

**완화 방법**

이 패키지를 참조하여 EF Core의 디자인 타임 동작을 참조해야 하는 경우 프로젝트에서 PackageReference 항목 메타데이터를 업데이트할 수 있습니다.

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

패키지가 Microsoft.EntityFrameworkCore.Tools를 통해 전이적으로 참조되는 경우에는 명시적 PackageReference를 패키지에 추가하여 해당 메타데이터를 변경해야 합니다. 이러한 명시적 참조는 패키지의 형식이 필요한 모든 프로젝트에 추가되어야 합니다.

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a>SQLitePCL.raw가 버전 2.0.0으로 업데이트되었습니다.

[추적 이슈 #14824](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

**이전 동작**

이전에는 Microsoft.EntityFrameworkCore.Sqlite가 SQLitePCL.raw의 버전 1.1.12에 의존했습니다.

**새 동작**

버전 2.0.0에 의존하도록 패키지를 업데이트했습니다.

**이유**

SQLitePCL.raw의 버전 2.0.0은 .NET Standard 2.0을 대상으로 합니다. 이전에는 .NET Standard 1.1을 대상으로 했기 때문에 전이적 패키지를 대부분 종료해야 작동이 가능했습니다.

**완화 방법**

SQLitePCL.raw 버전 2.0.0에 중대한 변경이 포함되었습니다. 자세한 내용은 [릴리스 정보](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md)를 참조하세요.

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a>NetTopologySuite를 버전 2.0.0으로 업데이트

[이슈 추적 #14825](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

**이전 동작**

이전의 공간 패키지는 NetTopologySuite의 버전 1.15.1을 사용했습니다.

**새 동작**

버전 2.0.0에 의존하도록 패키지를 업데이트했습니다.

**이유**

NetTopologySuite 버전 2.0.0은 EF Core 사용자에게 발생하는 여러 가지 유용성 문제를 해결하는 것이 목적입니다.

**완화 방법**

NetTopologySuite 버전 2.0.0에는 호환성이 손상되는 변경이 포함되어 있습니다. 자세한 내용은 [릴리스 정보](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001)를 참조하세요.

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a>System.Data.SqlClient 대신 Microsoft.Data.SqlClient가 사용됩니다.

[추적 이슈 #15636](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

**이전 동작**

이전에는 Microsoft.EntityFrameworkCore.SqlServer가 System.Data.SqlClient에 의존했습니다.

**새 동작**

Microsoft.Data.SqlClient에 의존하도록 패키지를 업데이트했습니다.

**이유**

지금부터 Microsoft.Data.SqlClient가 SQL Server의 플래그십 데이터 액세스 드라이버로 사용되며, System.Data.SqlClient는 더 이상 개발 시 중점 사항이 되지 않습니다.
Always Encrypted와 같은 일부 중요한 기능은 Microsoft.Data.SqlClient에서만 사용할 수 있습니다.

**완화 방법**

코드가 System.Data.SqlClient에 직접적인 종속성을 갖는 경우, 대신 Microsoft.Data.SqlClient를 참조하도록 변경해야 합니다. 두 패키지는 높은 수준의 API 호환성을 유지하므로 간단하게 패키지와 네임스페이스를 변경하기만 하면 됩니다.

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a>복수의 모호한 자기 참조 관계를 구성해야 합니다. 

[추적 문제 #13573](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

**이전 동작**

복수의 자기 참조 단방향 탐색 속성 및 매칭되는 FK를 갖는 엔터티 형식이 단일 관계로 잘못 구성되었습니다. 예:

```csharp
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

**새 동작**

이 시나리오는 이제 모델 빌드에서 검색되고, 모델이 모호하다는 예외가 throw됩니다.

**이유**

결과로 생성되는 모델이 모호하고 잘못 사용될 가능성이 컸습니다.

**완화 방법**

관계의 전체 구성을 사용합니다. 예:

```csharp
modelBuilder
     .Entity<User>()
     .HasOne(e => e.CreatedBy)
     .WithMany();
 
 modelBuilder
     .Entity<User>()
     .HasOne(e => e.UpdatedBy)
     .WithMany();
```

<a name="udf-empty-string"></a>
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a>DbFunction.Schema가 null 또는 빈 문자열이면 모델의 기본 스키마에 있도록 구성됩니다.

[추적 이슈 #12757](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

**이전 동작**

스키마가 빈 문자열로 구성된 DbFunction은 스키마가 없는 기본 제공 함수로 처리되었습니다. 예를 들어 다음 코드는 `DatePart` CLR 함수를 SqlServer의 `DATEPART` 기본 제공 함수에 매핑합니다.

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

**새 동작**

모든 DbFunction 매핑은 사용자 정의 함수에 매핑되는 것으로 간주됩니다. 따라서 빈 문자열 값은 모델에 대한 기본 스키마 내에 함수를 배치합니다. 이것은 흐름 API `modelBuilder.HasDefaultSchema()` 또는 `dbo`를 통해 명시적으로 구성된 스키마일 수도 있고 그렇지 않을 수도 있습니다.

**이유**

이전에 스키마가 비어 있던 것은 해당 함수를 기본 제공으로 처리하는 방법이었지만 그 논리는 기본 제공 함수가 어떤 스키마에도 속하지 않는 SqlServer에만 적용됩니다.

**완화 방법**

DbFunction의 변환이 기본 제공 함수에 매핑되도록 수동으로 구성합니다.

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
