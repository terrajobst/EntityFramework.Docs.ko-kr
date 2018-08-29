---
title: EF6에서 EF Core-로 이식 요구 사항 유효성 검사
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: fd378c72a3c8de4a441861b3c52b94eba5f7e5b4
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993442"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>EF6에서 EF Core로 이식 하기 전에: 응용 프로그램의 요구 사항 유효성 검사

이식 프로세스를 시작 하기 전에 EF Core 응용 프로그램에 대 한 데이터 액세스 요구 사항을 충족 하는지 유효성을 검사 하는 것이 반드시 합니다.

## <a name="missing-features"></a>누락 된 기능

EF Core 응용 프로그램에서 사용 하는 데 필요한 모든 기능에 있는지 확인 합니다. 참조 [기능 비교](../features.md) EF6에 EF Core에서 설정 하는 기능을 비교 하는 하는 방법을 자세히 비교 합니다. 모든 필수 기능이 누락 되었는지, EF Core로 이식 하기 전에 기능이 부족 한 보완할 수 있는지를 확인 합니다.

## <a name="behavior-changes"></a>동작 변경 내용

EF6 및 EF Core 간의 동작 변경 목록이 완전 하지 않은 경우 유념 하 포트로 응용 프로그램으로 서 응용 프로그램의 동작 방식, 하지만 표시 되지 것입니다 컴파일 오류로 바꾼 후 EF Core로 변경할 수 있지만 두는 것이 반드시 합니다.

### <a name="dbsetaddattach-and-graph-behavior"></a>DbSet.Add/Attach 및 그래프 동작

Ef6과 호출 `DbSet.Add()` 해당 탐색 속성에서 참조 된 모든 엔터티를 재귀적으로 검색에는 엔터티 결과에서 합니다. 검색 되 고, 이미 컨텍스트에 의해 추적 되지 않습니다 하는 모든 엔터티도 추가 하는 것으로 표시 되어야 합니다. `DbSet.Attach()` 모든 엔터티가 표시 됩니다 점을 제외 하 고는 동일 하 게 동작으로 변경 합니다.

**EF Core는 비슷한 재귀 검색을 수행 하지만 약간 다른 몇 가지 규칙이 포함 됩니다.**

*  루트 엔터티는 항상 요청 된 상태 (에 대 한 추가 `DbSet.Add` 에 대 한 채 `DbSet.Attach`).

*  **탐색 속성의 재귀 검색 중에 발견 된 엔터티:**

    *  **엔터티의 기본 키 저장소가 생성 된 경우**

        * 기본 키 값으로 설정 하지 않으면, 상태에 추가 설정 됩니다. 기본 키 값 경우 것으로 간주 "설정 안 함" 속성 형식에 대 한 CLR 기본 값이 할당 됩니다 (예를 들어 `0` 에 대 한 `int`를 `null` 에 대 한 `string`등.).

        * 기본 키를 값으로 설정 되 면 상태는 unchanged로 설정 합니다.

    *  기본 키가 데이터베이스에서 생성 된, 엔터티 루트로 동일한 상태로 전환 됩니다.

### <a name="code-first-database-initialization"></a>첫 번째 데이터베이스 초기화 코드

**EF6에 상당한 양의 데이터베이스 연결을 선택 하 고 데이터베이스를 초기화할 주위 수행 하는 기능이 있습니다. 이러한 규칙 중 일부는 다음과 같습니다.**

* 구성 되어 EF6는 SQL Express 또는 LocalDb 데이터베이스를 선택 합니다.

* 동일한 이름으로 컨텍스트를 사용 하 여 연결 문자열은 응용 프로그램의 경우 `App/Web.config` 파일을이 연결을 사용 합니다.

* 데이터베이스 존재 하지 않는 경우 만들어집니다.

* 모델에서 테이블이 데이터베이스에 있으면 현재 모델에 대 한 스키마는 데이터베이스에 추가 됩니다. 마이그레이션을 사용 하는 경우 다음 사용할 데이터베이스를 만들려고 합니다.

* 데이터베이스가 존재 하 고 EF6 이전에 스키마를 만들었으며, 경우에 현재 모델을 사용 하 여 호환성에 대 한 스키마 확인 됩니다. 스키마 만들어진 후 모델이 변경 된 경우 예외가 throw 됩니다.

**EF Core이 매직이 중 하나를 수행 하지 않습니다.**

* 코드에서 데이터베이스 연결을 명시적으로 구성 되어야 합니다.

* 초기화가 수행 됩니다. 사용 해야 합니다 `DbContext.Database.Migrate()` 마이그레이션을 적용할 (또는 `DbContext.Database.EnsureCreated()` 및 `EnsureDeleted()` 마이그레이션을 사용 하지 않고 데이터베이스 만들기/삭제).

### <a name="code-first-table-naming-convention"></a>코드 첫 번째 테이블 명명 규칙

EF6은 엔터티에 매핑되는 기본 테이블 이름을 계산 하는 복수 적용 서비스를 통해 엔터티 클래스 이름을 실행 합니다.

EF Core의 이름을 사용 하 여 `DbSet` 엔터티는 파생 컨텍스트의 제공 되는 속성입니다. 엔터티가 없는 경우는 `DbSet` 속성을 클래스 이름이 사용 됩니다.
