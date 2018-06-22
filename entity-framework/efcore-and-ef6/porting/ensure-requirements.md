---
title: EF6에서 EF 코어-로 이식 요구 사항을 확인
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 2f45039e63546d266ec6ce0bfa66ef7e9fb3d7e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052863"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>EF 코어로 EF6에서 이식 하기 전에: 응용 프로그램의 요구 사항을 확인

이식 프로세스를 시작 하기 전에는 EF 핵심 응용 프로그램에 대 한 데이터 액세스 요구 사항을 충족 하는지 확인 해야 합니다.

## <a name="missing-features"></a>누락 된 기능

EF 핵심 응용 프로그램에서 사용 하는 데 필요한 모든 기능에 있는지 확인 합니다. 참조 [기능 비교](../features.md) 의 EF 코어에는 기능이 EF6를 비교 하는 방법을 자세히 비교 합니다. 모든 필수 기능이 누락 되었는지, EF 코어로 이식 하기 전에 이러한 기능 부족 한 보정할 수를 확인 합니다.

## <a name="behavior-changes"></a>동작 변경 내용

EF6 및 EF 코어 간 동작의 일부 변경의 부분 목록입니다. 으로 유지 하 유념 포트도 응용 프로그램 방식으로 응용 프로그램의 동작 방식, 하지만 표시 되지 것입니다 컴파일 오류가로 바꾼 후 EF 코어로 변경 될 수는 것이 유용 합니다.

### <a name="dbsetaddattach-and-graph-behavior"></a>DbSet.Add/Attach 및 그래프 동작

EF6에서 호출 `DbSet.Add()` 해당 탐색 속성에서 참조 되는 모든 엔터티에 대 한 재귀 검색을 엔터티에서 합니다. 검색 되 고는 컨텍스트에서 이미 추적 되지 않습니다 하는 사용자 엔터티는 추가 하는 것으로 표시 합니다. `DbSet.Attach()`표시 된 모든 엔터티를 제외 하 고는 동일 하 게 작동으로 변경 되지 않습니다.

**EF 코어 유사한 재귀 검색을 수행 하지만 몇 가지 약간 다른 규칙입니다.**

*  루트 엔터티는 항상 요청 된 상태 (에 대 한 추가 `DbSet.Add` 에 대 한 변경 하지 않고 `DbSet.Attach`).

*  **탐색 속성의 회귀적 검색 중에 발견 된 엔터티에 대 한:**

    *  **엔터티의 기본 키가 생성 된 저장소**

        * 기본 키 값으로 설정 하지 않으면, 상태에 추가 된 설정 됩니다. 기본 키 값 것으로 간주 됩니다 "설정 안 함" 속성 형식에 대 한 CLR 기본 값이 지정 됩니다 (즉, `0` 에 대 한 `int`, `null` 에 대 한 `string`등.).

        * 기본 키 값으로 설정 되어, 하는 경우 상태 설정은 변경 되지 않은 합니다.

    *  기본 키가 데이터베이스에서 생성 된, 엔터티 루트는 것과 동일한 상태로 전환 됩니다.

### <a name="code-first-database-initialization"></a>첫 번째 데이터베이스 초기화 코드

**EF6 상당한 양의 매직 수행 데이터베이스 연결을 선택 하 고 데이터베이스를 초기화할 주위에 있습니다. 이러한 규칙 중 일부는 다음과 같습니다.**

* 구성 되어 EF6는 SQL Express 또는 LocalDb에서 데이터베이스를 선택 합니다.

* 컨텍스트와 같은 이름 가진 연결 문자열이 응용 프로그램에 있으면 `App/Web.config` 파일을이 연결을 사용 합니다.

* 데이터베이스가 존재 하지 않는 경우 자동으로 만들어집니다.

* 데이터베이스에 있는 테이블이 모델에서 현재 모델에 대 한 스키마는 데이터베이스에 추가 됩니다. 마이그레이션을 사용 하는 경우 데이터베이스를 만들 사용 됩니다 합니다.

* 데이터베이스가 존재 하는 경우 EF6 스키마 만들었으며, 이전에 스키마를 현재 모델과의 호환성을 검사 합니다. 모델 스키마를 만든 후 변경 된 경우 예외가 throw 됩니다.

**EF 코어가이 마법 수행 하지 않습니다.**

* 코드에서 데이터베이스 연결을 명시적으로 구성 되어야 합니다.

* 없음 초기화 수행 됩니다. 사용 해야 `DbContext.Database.Migrate()` 마이그레이션을 적용 하려면 (또는 `DbContext.Database.EnsureCreated()` 및 `EnsureDeleted()` 마이그레이션을 사용 하지 않고 데이터베이스 만들기/삭제에).

### <a name="code-first-table-naming-convention"></a>코드 첫 번째 테이블 명명 규칙

엔터티 클래스 이름에 매핑되는 엔터티는 기본 테이블 이름을 계산 하는 복수 적용 서비스를 통해 실행 하는 EF6 합니다.

EF 코어 이름을 사용 하는 `DbSet` 엔터티 파생 컨텍스트에서 제공 되는 속성입니다. 엔터티가 없는 경우는 `DbSet` 속성을 입력 한 다음 클래스 이름을 사용 합니다.
