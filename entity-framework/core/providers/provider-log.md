---
title: 로그 공급자에 영향을 주는 변경 내용-EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: 5da1043310e2858638c81a0654a9cab23e39c220
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250818"
---
# <a name="provider-impacting-changes"></a>공급자에 영향을 주는 변경 내용

이 페이지에는 끌어오기 요청 react를 다른 데이터베이스 공급자의 작성자가 필요할 수 있는 EF Core 리포지토리에 대 한 링크가 있습니다. 의도 공급자의 새 버전으로 업데이트할 때 기존 타사 데이터베이스 공급자의 작성자를 위한 시작점을 제공 하는 것입니다.

이 로그 2.1에서 2.2로 변경 내용과 시작 했습니다. 2.1 이전 버전에서는 사용 된 [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) 및 [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) 당사의 문제 및 끌어오기 요청에는 레이블.

## <a name="21-----22"></a>2.1 2.2--->

### <a name="test-only-changes"></a>테스트 전용 변경 내용

* https://github.com/aspnet/EntityFrameworkCore/pull/12057 -테스트의 사용자 지정 가능한 SQL 구분 기호를 허용 합니다.
  * 엄격한 비 부동 지점 비교 BuiltInDataTypesTestBase 허용 하는 변경 내용을 테스트합니다
  * 쿼리 테스트를 다른 SQL 구분 기호를 사용 하 여 다시 사용할 수 있도록 테스트 변경
* https://github.com/aspnet/EntityFrameworkCore/pull/12072 -관계형 사양 테스트 DbFunction 테스트를 추가 합니다.
  * 모든 데이터베이스 공급자에 대해 이러한 테스트를 실행할 수 있도록
* https://github.com/aspnet/EntityFrameworkCore/pull/12362 비동기 테스트 정리
  * 제거 `Wait` 호출 비동기, 불필요 한 및 일부 테스트 메서드 이름을 바꿀
* https://github.com/aspnet/EntityFrameworkCore/pull/12666 --로깅 테스트 인프라를 통합 하는 중
  * 추가 `CreateListLoggerFactory` react에 이러한 테스트를 사용 하 여 공급자를 해야 하는 일부 이전 로깅 인프라를 제거 합니다.
* https://github.com/aspnet/EntityFrameworkCore/pull/12500 -동기적 및 비동기적으로 더 많은 쿼리 테스트 실행
  * 테스트 이름 및 팩터링을 변경 된 이러한 테스트를 사용 하 여 대응할 수 공급자 필요
* https://github.com/aspnet/EntityFrameworkCore/pull/12766 이름 바꾸기 ComplexNavigations 모델 탐색
  * 이러한 테스트를 사용 하 여 공급자 대응 해야 합니다.
* https://github.com/aspnet/EntityFrameworkCore/pull/12141 -기능 테스트를 삭제 하는 대신 풀에 컨텍스트를 반환 합니다.
  * 이 변경 리팩터링 반응 하는 공급자가 필요할 수 있는 몇 가지 테스트 포함


### <a name="test-and-product-code-changes"></a>테스트 및 제품 코드 변경 내용

* https://github.com/aspnet/EntityFrameworkCore/pull/12109 --RelationalTypeMapping.Clone 메서드를 통합 하는 중
  * 파생된 클래스에서 간소화 하기 위해 허용 RelationalTypeMapping 2.1에서 변경 됩니다. 에서는 실감이 잘 나 공급자에 중단 된이 있지만 공급자 활용이 변경의 파생된 형식에 클래스 매핑.
* https://github.com/aspnet/EntityFrameworkCore/pull/12069 -태그가 지정 된 쿼리나 명명 된 쿼리
  * LINQ 쿼리에 태그를 지정 하 고 SQL 주석으로 표시 하는 이러한 태그에 대 한 인프라를 추가 합니다. 이 공급자 SQL 생성에 반응 해야 합니다.
* https://github.com/aspnet/EntityFrameworkCore/pull/13115 -NTS 통해 공간 데이터를 지원 합니다.
  * 형식 매핑 및 멤버를 사용 하면 외부 공급자를 등록 하는 변환기
    * 공급자는 기본 호출 해야 합니다. 작동 하도록 하기 위해 ITypeMappingSource 구현에서 FindMapping()
  * 공급자에서 일관 된 공급자에 공간 지원을 추가 하려면이 패턴을 따릅니다.
* https://github.com/aspnet/EntityFrameworkCore/pull/13199 추가 서비스 공급자 만들기에 대 한 향상 된 디버깅
  * DbContextOptionsExtensions를 내부 서비스 공급자를 다시 작성 되는 이유를 이해 하는 데 도움이 되는 새 인터페이스를 구현할 수 있습니다.
