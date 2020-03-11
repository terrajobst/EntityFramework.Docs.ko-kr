---
title: 공급자에 영향을 주는 변경 내용에 대 한 로그-EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: b911a2da493e20c4e4ce6f1e25024bd0efd38b44
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414772"
---
# <a name="provider-impacting-changes"></a>공급자에게 영향을 미치는 변경 내용

이 페이지에는 다른 데이터베이스 공급자의 작성자가 반응할 수 있어야 하는 EF Core 리포지토리에 대 한 끌어오기 요청에 대 한 링크가 포함 되어 있습니다. 공급자를 새 버전으로 업데이트할 때 기존 타사 데이터베이스 공급자의 작성자를 위한 시작점을 제공 하려고 합니다.

2\.1에서 2.2로 변경 하 여이 로그를 시작 하는 중입니다. 2\.1 이전에는 [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) 를 사용 하 여 문제 및 끌어오기 요청에 대 한 레이블을 [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) 했습니다.

## <a name="22-----30"></a>2.2 ---> 3.0

[응용 프로그램 수준 주요 변경 내용](../what-is-new/ef-core-3.0/breaking-changes.md) 대부분이 공급자에도 영향을 줍니다.

* <https://github.com/aspnet/EntityFrameworkCore/pull/14022>
  * 사용 되지 않는 Api를 제거 하 고 선택적 매개 변수 오버 로드를 축소
  * Removed DatabaseColumn.GetUnderlyingStoreType()
* <https://github.com/aspnet/EntityFrameworkCore/pull/14589>
  * 사용 되지 않는 Api 제거
* <https://github.com/aspnet/EntityFrameworkCore/pull/15044>
  * 기본 구현에서 몇 가지 버그를 수정 하는 데 필요한 동작 변경으로 인해 CharTypeMapping의 서브 클래스가 손상 되었을 수 있습니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15090>
  * IDatabaseModelFactory에 대 한 기본 클래스를 추가 하 고 매개 변수 개체를 사용 하 여 이후 나누기를 완화 하도록 업데이트 했습니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15123>
  * MigrationsSqlGenerator에서 매개 변수 개체를 사용 하 여 향후 중단을 완화 합니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/14972>
  * 로그 수준의 명시적 구성은 공급자가 사용 중일 수 있는 Api를 약간 변경 해야 합니다. 특히 공급자가 로깅 인프라를 직접 사용 하는 경우이 변경으로 인해 사용이 중단 될 수 있습니다. 또한 인프라 (공용)를 사용 하는 공급자는 `LoggingDefinitions` 또는 `RelationalLoggingDefinitions`에서 파생 되어야 합니다. 예제는 SQL Server 및 메모리 내 공급자를 참조 하세요.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15091>
  * 이제 코어, 관계형 및 추상화 리소스 문자열이 공개 됩니다.
  * `CoreLoggerExtensions` 및 `RelationalLoggerExtensions` 이제 공용입니다. 공급자는 코어 또는 관계형 수준에서 정의 된 이벤트를 로깅할 때 이러한 Api를 사용 해야 합니다. 로깅 리소스에 직접 액세스 하지 마십시오. 이는 여전히 내부입니다.
  * `IRawSqlCommandBuilder` singleton 서비스에서 범위가 지정 된 서비스로 변경 되었습니다.
  * `IMigrationsSqlGenerator` singleton 서비스에서 범위가 지정 된 서비스로 변경 되었습니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/14706>
  * 관계형 명령을 빌드하기 위한 인프라는 public으로 설정 되었으므로 공급자가 안전 하 게 사용할 수 있으며 약간 리팩터링할 수 있습니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/14733>
  * 범위가 지정 된 서비스에서 임시 서비스로 `ILazyLoader` 변경 되었습니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/14610>
  * 범위가 지정 된 서비스에서 단일 서비스로 `IUpdateSqlGenerator` 변경 되었습니다.
  * 또한 `ISingletonUpdateSqlGenerator` 제거 되었습니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15067>
  * 공급자가 사용 하 고 있던 많은 내부 코드를 이제 공용으로 만들었습니다.
  * Necssary를 노출 하는 위치에서 제외 되었으므로 더 이상 `IndentedStringBuilder` 참조 하지 않아야 합니다.
  * `NonCapturingLazyInitializer` 사용은 BCL의 `LazyInitializer`으로 바꾸어야 합니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/14608>
  * 이 변경은 응용 프로그램의 주요 변경 내용 문서에 자세히 설명 되어 있습니다. 공급자의 경우 EF core 테스트로 인해이 문제가 발생할 수 있기 때문에 더 큰 영향을 줄 수 있으므로 테스트 인프라가 가능성을 최소화 하도록 변경 되었습니다.
* <https://github.com/aspnet/EntityFrameworkCore/issues/13961>
  * `EntityMaterializerSource` 간소화 됨
* <https://github.com/aspnet/EntityFrameworkCore/pull/14895>
  * 공급자가 필요 하거나 반응할 수 있는 방식으로 StartsWith 변환이 변경 되었습니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15168>
  * 규칙 집합 서비스가 변경 되었습니다. 공급자는 이제 "ProviderConventionSet" 또는 "RelationalConventionSet" 중 하나에서 상속 됩니다.
  * `IConventionSetCustomizer` 서비스를 통해 사용자 지정 항목을 추가할 수 있지만이는 공급자가 아닌 다른 확장에서 사용 하기 위한 것입니다.
  * 런타임에 사용 되는 규칙은 `IConventionSetBuilder`에서 확인 되어야 합니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15288>
  * 내부 형식을 사용할 필요가 없도록 공용 API로 데이터 시드를 리팩터링 했습니다. 이는 모든 관계형 공급자의 기본 관계형 클래스에서 시드가 처리 되므로 비관계형 공급자에만 영향을 줍니다.

## <a name="21-----22"></a>2.1 ---> 2.2

### <a name="test-only-changes"></a>테스트 전용 변경 내용

* <https://github.com/aspnet/EntityFrameworkCore/pull/12057>-테스트에서 사용자 지정 가능한 SQL 구분 기호을 허용 합니다.
  * BuiltInDataTypesTestBase에서 strict가 아닌 부동 소수점 비교를 허용 하는 테스트 변경 내용
  * 다른 SQL 구분 기호 쿼리 테스트를 다시 사용할 수 있도록 하는 변경 내용 테스트
* <https://github.com/aspnet/EntityFrameworkCore/pull/12072>-관계형 사양 테스트에 DbFunction 테스트 추가
  * 이러한 테스트는 모든 데이터베이스 공급자에 대해 실행할 수 있습니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/12362>-비동기 테스트 정리
  * `Wait` 호출을 제거 하 고, 불필요 한 비동기를 제거 하 고, 일부 테스트 메서드의 이름을 바꿨습니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/12666>-로깅 테스트 인프라 통합
  * 이러한 테스트를 사용 하 여 응답 하는 공급자가 필요한 이전 로깅 인프라를 추가 `CreateListLoggerFactory` 및 제거 했습니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/12500>-동기적 및 비동기식으로 더 많은 쿼리 테스트 실행
  * 테스트 이름 및 팩터링이 변경 되었으므로 이러한 테스트를 사용 하는 공급자가 응답 해야 합니다.
* ComplexNavigations 모델에서 <https://github.com/aspnet/EntityFrameworkCore/pull/12766> 이름 바꾸기 탐색
  * 이러한 테스트를 사용 하는 공급자는 반응 해야 할 수 있습니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/12141>-기능 테스트에서 삭제 하지 않고 컨텍스트를 풀로 반환 합니다.
  * 이 변경에는 공급자가 반응 해야 할 수 있는 일부 테스트 리팩터링이 포함 됩니다.

### <a name="test-and-product-code-changes"></a>테스트 및 제품 코드 변경

* <https://github.com/aspnet/EntityFrameworkCore/pull/12109> RelationalTypeMapping 메서드 통합
  * 2\.1의 변경 내용이 파생 클래스의 간소화를 위해 허용 되는 RelationalTypeMapping. 이는 공급자와는 그렇지 않다고 생각 하지만 공급자는 파생 형식 매핑 클래스에서 이러한 변경을 활용할 수 있습니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/12069> 태그 또는 명명 된 쿼리
  * LINQ 쿼리 태그를 지정 하 고 해당 태그가 SQL에서 주석으로 표시 되도록 하는 인프라를 추가 합니다. 이 경우 공급자가 SQL 생성에 반응할 수 있어야 합니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13115>-NTS을 통해 공간 데이터 지원
  * 공급자 외부에서 형식 매핑 및 멤버 변환기를 등록할 수 있습니다.
    * 공급자는 base를 호출 해야 합니다. ITypeMappingSource 구현에서 작동 하기 위한 FindMapping ()
  * 공급자 간에 일관 된 공간 지원을 공급자에 추가 하려면이 패턴을 따릅니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13199>-서비스 공급자 만들기에 대 한 향상 된 디버깅 추가
  * DbContextOptionsExtensions가 내부 서비스 공급자가 다시 빌드되는 이유를 이해 하는 데 도움이 되는 새 인터페이스를 구현할 수 있습니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13289>-상태 검사에서 사용할 CanConnect API를 추가 합니다.
  * 이 PR은 ASP.NET Core 상태 검사에서 데이터베이스를 사용할 수 있는지 확인 하는 데 사용 되는 `CanConnect` 개념을 추가 합니다. 기본적으로 관계형 구현은 `Exist`만 호출 하지만, 필요한 경우 공급자는 다른 항목을 구현할 수 있습니다. 비관계형 공급자는 상태 검사를 사용할 수 있도록 새 API를 구현 해야 합니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13306>-기본 RelationalTypeMapping을 업데이트 하 여 DbParameter 크기를 설정 하지 않음
  * 크기 설정은 잘림을 일으킬 수 있으므로 기본적으로 중지 합니다. 크기를 설정 해야 하는 경우 공급자가 자체 논리를 추가 해야 할 수 있습니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13372> RevEng: decimal 열에 대해 항상 열 형식을 지정 합니다.
  * 규칙에 따라 구성 하는 대신 스 캐 폴드 코드에서 decimal 열의 열 형식을 항상 구성 합니다.
  * 공급자에 게는 끝에 대 한 변경이 필요 하지 않습니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13469>-SQL 사례 식을 생성 하기 위한 CaseExpression를 추가 합니다.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13648>-SqlFunctionExpression에서 형식 매핑을 지정 하는 기능을 추가 하 여 인수 및 결과의 저장소 형식 유추를 향상 시킵니다.
