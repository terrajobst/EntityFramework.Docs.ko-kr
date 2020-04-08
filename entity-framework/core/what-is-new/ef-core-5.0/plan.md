---
title: Entity Framework Core 5.0 계획
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 8b4ca32524869019c04d5a4d4d55967f68181cd7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136231"
---
# <a name="plan-for-entity-framework-core-50"></a>Entity Framework Core 5.0 계획

[계획 프로세스](../release-planning.md)에 설명된 바와 같이 Microsoft는 관련자가 제공하는 의견을 모아 잠정적 EF Core 5.0 릴리스 계획에 반영했습니다.

> [!IMPORTANT] 
> 이 계획은 지금도 진행 중입니다. 이 문서의 내용은 약정을 일체 구성하지 않습니다. 이 계획은 일종의 시작점으로서 학습이 늘어남에 따라 발전할 예정입니다. 현재 5.0에 대해 계획되지 않은 몇몇 사항이 도입될 수도 있습니다. 현재 5.0에 대해 계획된 몇몇 사항은 배제될 수도 있습니다.

### <a name="version-number-and-release-date"></a>버전 번호 및 릴리스 날짜

EF Core 5.0은 현재 [.NET 5.0과 동일한 시점에](https://devblogs.microsoft.com/dotnet/introducing-net-5/) 릴리스될 것으로 예정된 상태입니다. 버전 번호 “5.0”는 .NET 5.0와 통일되도록 선정하였습니다.

### <a name="supported-platforms"></a>지원되는 플랫폼

EF Core 5.0은 [.NET Core로의 플랫폼 수렴](https://devblogs.microsoft.com/dotnet/introducing-net-5/)에 기반한 모든 .NET 5.0 플랫폼상의 실행을 목적으로 계획되었습니다. .NET Standard와 실제로 사용된 TFM의 측면에서 이것이 의미하는 바는 아직 정해지지 않았습니다.

EF Core 5.0은 .NET Framework에서 실행되지 않습니다.

### <a name="breaking-changes"></a>호환성이 손상되는 변경

EF Core 5.0의 몇몇 변경 사항은 호환성 손상으로 이어지기는 하겠지만 EF Core 3.0에 비해선 그 심각도가 한참 경미한 수준입니다. Microsoft는 대다수 애플리케이션이 호환성 손상 없이 업데이트되도록 하는 데 목표를 두었습니다.

데이터베이스 공급자의 호환성 손상으로 이어지는 변경 사항이 있을 것으로 예상되는데, 특히 TPT 지원과 관련해 문제가 발생할 것으로 내다보입니다. 물론 5.0의 공급자 업데이트 작업은 3.0에 필요했던 만큼보다는 부담이 줄 것으로 보입니다.

## <a name="themes"></a>테마

Microsoft는 EF Core 5.0 투자 상당분의 토대를 구성할 몇몇 주요 영역이나 테마를 추출했습니다.

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a>다대다 탐색 속성(“건너뛰기 링크”라고도 함)

선임 개발자: @smitpatel 및 @AndriySvyryd

추적 위치: [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)

티셔츠 크기: L

상태: 진행 중

다대다는 GitHub 백로그에서 [가장 많이 요청되는 기능](https://github.com/aspnet/EntityFrameworkCore/issues/1368)(약 407표 투표됨)입니다.

다대다 관계 전체에 대한 지원은 [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508)으로 추적됩니다. 다음 세 가지 주요 영역으로 나눌 수 있습니다.

* 건너뛰기 링크 속성. 이 속성을 통해 모델은 기본 조인 테이블 엔터티 참조 없이 쿼리 등에 사용될 수 있습니다. ([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))
* 속성 모음 엔터티 형식. 이 형식을 통해 표준 CLR 형식(예: `Dictionary`)은 각 엔터티 형식에 대해 명시적 CLR 형식이 불필요한 엔터티 인스턴스에 사용될 수 있습니다. (5.0용 Stretch: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)
* 간편한 다대다 관계 구성을 위한 Sugar. (5.0용 Stretch.)

이와 같이 필요한 다대다 지원의 가장 큰 걸림돌은 쿼리 같은 비즈니스 논리에서 조인 테이블 참조 없이 “자연” 관계를 사용할 수 없다는 점입니다. 조인 테이블 엔터티 형식은 여전히 존재하겠지만 비즈니스 논리에 지장을 줘서는 안 됩니다. 이 때문에 Microsoft에서는 5.0에서 건너뛰기 링크 속성을 다루기로 했습니다.

현재 다대다의 다른 부분은 EF Core 5.0의 추가적 목표로서 검토를 진행 중입니다. 따라서 지금으로선 5.0 관련 계획에 포함되지는 않았지만 진행이 순조롭다면 도입이 될 것으로 희망하고 있습니다.

## <a name="table-per-type-tpt-inheritance-mapping"></a>TPT(형식별 테이블) 상속 매핑

선임 개발자: @AndriySvyryd

추적 위치: [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)

티셔츠 크기: XL

상태: 진행 중

TPT는 여러 번 요청되는 기능(약 254표 투표되어 전체 3위 기록)인 데 더해 전체적인 .NET 5 계획의 기본 특성에 적합하다고 판단되는 몇몇 하위 수준 변경 사항을 요하므로 Microsoft에서는 TPT 상속을 사용하고 있습니다. 이에 따른 변경 사항은 데이터베이스 공급자에 있어 호환성을 손상할 것으로 보이지만 3.0에 필요한 변경 사항보다는 그 심각도가 한참 경미한 수준입니다.

## <a name="filtered-include"></a>필터링 적용 포함

선임 개발자: @maumar

추적 위치: [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)

티셔츠 크기: M

상태: 진행 중

필터링 적용 포함은 여러 번 요청되는 기능(약 317표 투표되어 전체 2위 기록)으로서 작업량 부담이 크지 않고, 현재 모델 수준의 필터나 좀 더 복잡한 쿼리를 요하는 상당수의 시나리오에 편의를 더하거나 그 차단을 해제할 것으로 판단됩니다.

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a>ToTable, ToQuery, ToView, FromSql 등의 합리화

선임 개발자: @maumar 및 @smitpatel

추적 위치: [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)

티셔츠 크기: L

상태: 진행 중

Microsoft는 원시 SQL, 키 없는 형식, 관련 영역에 대한 지원과 관련해 이전 릴리스에서 발전 성과를 거둘 수 있었습니다. 하지만 모두가 하나로서 기능할 방법에 있어서는 여러 간극과 불일치성이라는 문제를 마주했습니다. 5\.0의 목표는 이와 같은 문제를 바로잡고 서로 다른 형식의 엔터티와 관련 쿼리, 데이터베이스 아티팩트를 정의, 마이그레이션, 사용하는 데 있어 원활한 환경을 선보이는 것입니다. 여기에는 컴파일된 쿼리 API의 업데이트도 포함될 수 있습니다.

Microsoft의 현재 기능 일부는 허용 범위가 너무 커서 빠르게 사용자의 오류로 이어질 수 있으므로 이 항목으로 인한 변경 사항이 애플리케이션 수준의 호환성 손상을 야기할 수 있음에 유의하시기 바랍니다. 이러한 기능 중 몇몇은 차단되고 대안 관련 지침이 제공됩니다.

## <a name="general-query-enhancements"></a>일반 쿼리 개선 사항

선임 개발자: @smitpatel 및 @maumar

추적 위치: [5.0 마일스톤에서 `area-query` 레이블 지정된 문제](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)

티셔츠 크기: XL

상태: 진행 중

EF Core 3.0의 쿼리 변환 코드는 광범위하게 재작성되었습니다. 따라서 쿼리 코드는 일반적으로 좀 더 강력한 상태를 자랑합니다. 5\.0의 경우에는 건너뛰기 링크 속성 및 TPT 지원에 필요한 사항 외에 주요 쿼리 변경을 계획하지 않았습니다. 그러나 3.0 정비에서 남은 일부 기술 문제를 해결하는 데 있어 상당한 작업이 필요하기는 합니다. 또한 Microsoft는 전체적인 쿼리 환경을 한층 개선하고자 상당수의 버그를 고치고 소규모 향상 개선 작업을 시행할 계획입니다.

## <a name="migrations-and-deployment-experience"></a>마이그레이션 및 배포 환경

선임 개발자: @bricelam

추적 위치: [#19587](https://github.com/dotnet/efcore/issues/19587)

티셔츠 크기: L

상태: 진행 중

현재 상당수의 개발자는 저마다의 데이터베이스를 애플리케이션 시작 시점에 마이그레이션하고 있습니다. 이는 간단한 방법이긴 하지만 다음의 이유로 권장되지 않습니다.

* 여러 스레드/프로세스/서버가 데이터베이스를 동시에 마이그레이션하려 할 수 있음
* 이 문제가 발생하는 동안 애플리케이션이 일관되지 않은 상태에 액세스하려 할 수 있음
* 일반적으로 데이터베이스의 스키마 수정 권한은 애플리케이션 실행에 대해 부여되어서는 안 됨
* 문제가 발생할 경우 클린 상태로 되돌리기 어려움

Microsoft는 배포 시점에 데이터베이스를 간편히 마이그레이션할 수 있도록 보다 나은 환경을 제공하고자 합니다. 이는 다음과 같은 환경을 말합니다.

* Linux, Mac 및 Windows에서 작업됨
* 명령줄 환경이 우수함
* 컨테이너를 사용하는 시나리오 지원
* 일반적으로 사용되는 실제 배포 도구/흐름으로 작업됨
* Visual Studio 이상에 통합됨

그에 따라 EF Core의 여러 작은 부분이 개선된다는 점과(예: SQLite의 마이그레이션 개선) EF의 범주를 넘어선 엔드투엔드 환경 개선을 위한 지침과 다른 팀과의 장기적 협업을 기대할 수 있습니다.

## <a name="ef-core-platforms-experience"></a>EF Core 플랫폼 환경 

선임 개발자: @roji 및 @bricelam

추적 위치: [#19588](https://github.com/dotnet/efcore/issues/19588)

티셔츠 크기: L

상태: 시작 안 됨

Microsoft의 지침은 기존의 MVC 유사 웹 애플리케이션에서 EF Core를 사용하는 데 매우 유용합니다. 다른 플랫폼과 애플리케이션 모델에 대한 지침은 찾을 수 없거나 너무 오래되어 유용하지 않습니다. EF Core 5.0에 대해서는 다음 항목과 함께 EF Core을 사용했을 때의 환경을 조사, 개선 및 서류화할 계획입니다.

* Blazor
* Xamarin(AOT/링커 스토리 사용 포함)
* WinForms/WPF/WinUI 및 기타 U.I. frameworks

EF Core의 여러 부분이 소규모로 개선된다는 점과 EF의 범주를 넘어선 엔드투엔드 환경 개선을 위한 지침과 다른 팀과의 장기적 협업을 기대할 수 있습니다.

검토 계획에 있는 구체적 영역은 다음과 같습니다.

* 배포(EF 도구에 대한 사용 환경 포함, 예: 마이그레이션 시)
* 애플리케이션 모델(Xamarin, Blazor 등 포함)
* SQLite 환경(공간적 환경 및 테이블 다시 빌드 포함)
* AOT 및 연결 환경
* 진단 통합(성능 카운터 포함)

## <a name="performance"></a>성능

선임 개발자: @roji

추적 위치: [5.0 마일스톤에서 `area-perf` 레이블 지정된 문제](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)

티셔츠 크기: L

상태: 진행 중

Microsoft는 EF Core에 대해 성능 벤치마크 도구 모음을 개선하고 런타임의 성능을 계획된 바에 따라 개선할 계획입니다. 또한 3.0 릴리스 주기에 프로토타입화된 새로운 ADO.NET 일괄 처리 API를 완료할 계획입니다. 또한 ADO.NET 레이어에서는 Npgsql 공급자의 성능 개선을 추가적으로 계획했습니다.

이 작업의 일환으로 ADO.NET/EF Core 성능 카운터 및 기타 진단을 적절히 추가할 계획도 있습니다.

## <a name="architecturalcontributor-documentation"></a>아키텍처/기여자 문서

선임 작성자: @ajcvickers

추적 위치: [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)

티셔츠 크기: L

상태: 진행 중

이 문서화의 목적은 EF Core 내부 요소를 보다 쉽게 파악할 수 있게 하는 데 있습니다. 이는 EF Core 사용자 모두에게 유용할 수 있지만 주된 목적은 외부 인력이 다음을 좀 더 간편히 수행하도록 돕는 데 있습니다.

* EF Core 코드 기여
* 데이터베이스 공급자 생성
* 기타 확장 빌드

## <a name="microsoftdatasqlite-documentation"></a>Microsoft.Data.Sqlite 문서

선임 작성자: @bricelam

추적 위치: [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)

티셔츠 크기: M

상태: 완료. 새 문서는 [Microsoft 문서 사이트에 게시](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli)되어 있습니다.

EF 팀은 Microsoft.Data.Sqlite ADO.NET 공급자도 소유합니다. Microsoft는 5.0 릴리스의 일환으로 이 공급자를 완전히 문서화할 계획입니다.

## <a name="general-documentation"></a>일반 문서

선임 작성자: @ajcvickers

추적 위치: [5.0 마일스톤에서 문서 리포지토리의 문제](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)

티셔츠 크기: L

상태: 진행 중

Microsoft는 이미 3.0 및 3.1 릴리스 관련 문서를 업데이트 중입니다. 그에 더하여 다음 작업도 현재 진행 중에 있습니다.
  * 시작하기 문서를 이용하고 따라하기 쉽도록 정비하는 작업
  * 상호 참조를 보다 쉽게 검색 및 추가할 수 있도록 문서 재구성
  * 기존 문서상의 세부 정보 및 설명 추가
  * 샘플 업데이트 및 사례 추가

## <a name="fixing-bugs"></a>버그 수정

추적 위치: [5.0 마일스톤에서 `type-bug` 레이블 지정된 문제](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)

개발자: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers

티셔츠 크기: L

상태: 진행 중

작성 시점에 5.0 릴리스에서 수정이 필요하다고 심사된 버그는 135개이나(62개는 이미 수정됨), 위의 _일반 쿼리 개선 사항_ 섹션과 크게 중첩되는 부분이 있습니다.

수신율(마일스톤 내 작업으로 귀결된 문제)은 3.0 릴리스 과정 전반에서 월별 약 23건으로 집계된 문제입니다. 이 문제를 5.0에서 전부 수정해야 하는 것은 아닙니다. 5\.0 기간에는 어림잡아 150건의 문제를 추가로 수정할 계획입니다.

## <a name="small-enhancements"></a>소규모 개선 사항

추적 위치: [5.0 마일스톤에서 `type-enhancement` 레이블 지정된 문제](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)

개발자: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers

티셔츠 크기: L

상태: 진행 중

5\.0에서는 위에 개괄된 좀 더 큰 규모의 기능 외에도 경미한 문제의 해결을 위한 작은 수준의 개선 사항도 다수 계획되어 있습니다. 이러한 개선 사항 중 상당수는 위에서 간략히 소개된 일반적 테마에서도 다뤄집니다.

## <a name="below-the-line"></a>BTL(Below-The-Line)

추적 위치: [`consider-for-next-release` 레이블 지정된 문제](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)

이는 버그 수정 사항 및 개선 사항으로서 현재 5.0 릴리스에 계획되지는 **않았지만** 위 작업의 추이에 따라 추가적인 목표로 설정할지도 검토할 예정입니다.

또한 Microsoft는 계획 수립 시 [투표 수가 가장 많은 문제](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)를 항상 고려합니다. 릴리스에서 이와 같은 문제를 줄이는 일은 언제나 만만치 않은 일이지만 현재 보유한 리소스에는 현실적인 계획이 필요합니다.

## <a name="feedback"></a>사용자 의견

계획에 대한 피드백은 중요합니다. 이슈의 중요성을 나타내는 가장 좋은 방법은 GitHub에서 해당 이슈에 대해 투표(엄지손가락을 위로)하는 것입니다. 이 데이터는 이후 다음 릴리스의 [계획 프로세스](../release-planning.md)에 반영됩니다.
