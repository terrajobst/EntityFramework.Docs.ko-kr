---
title: EF6에서 EF Core로 포팅-요구 사항 유효성 검사
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 07caa39e8a56db71e493331ea9f0286309abc52a
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565348"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>EF6에서 EF Core로 포팅 하기 전: 응용 프로그램의 요구 사항 확인

포팅 프로세스를 시작 하기 전에 EF Core 응용 프로그램에 대 한 데이터 액세스 요구 사항을 충족 하는지 확인 하는 것이 중요 합니다.

## <a name="missing-features"></a>누락 된 기능

EF Core에 응용 프로그램에서 사용 해야 하는 모든 기능이 포함 되어 있는지 확인 합니다. EF Core의 기능 집합을 EF6와 비교 하는 방법에 대 한 자세한 내용은 [기능 비교](../features.md) 를 참조 하세요. 필요한 기능이 누락 된 경우 EF Core으로 포팅 하기 전에 이러한 기능이 없다는 것을 보완할 수 있는지 확인 합니다.

## <a name="behavior-changes"></a>동작 변경

EF6와 EF Core 간의 동작에 대 한 일부 변경 내용에 대 한 완전 하지 않은 목록입니다. 응용 프로그램이 동작 하는 방식을 변경할 수 있지만 EF Core로 바꾼 후에 컴파일 오류로 표시 되지 않는 응용 프로그램의 포트를 염두에 두는 것이 중요 합니다.

### <a name="dbsetaddattach-and-graph-behavior"></a>DbSet. 추가/연결 및 그래프 동작

EF6에서는 엔터티에 대해 `DbSet.Add()` 를 호출 하면 해당 탐색 속성에서 참조 되는 모든 엔터티에 대 한 재귀 검색이 생성 됩니다. 검색 된 모든 엔터티는 아직 컨텍스트에 의해 추적 되지 않으며 추가 된 것으로 표시 되기도 합니다. `DbSet.Attach()`모든 엔터티가 변경 되지 않은 것으로 표시 된 경우를 제외 하 고는 동일 하 게 동작 합니다.

**EF Core는 비슷한 재귀 검색을 수행 하지만 약간 다른 규칙을 사용 합니다.**

*  루트 엔터티는 항상 요청 된 상태 (에 대해 `DbSet.Add` 추가 되 고 `DbSet.Attach`변경 되지 않음)입니다.

*  **탐색 속성을 재귀적으로 검색 하는 동안 발견 된 엔터티:**

    *  **엔터티의 기본 키가 저장소로 생성 된 경우**

        * 기본 키가 값으로 설정 되지 않으면 상태가 추가로 설정 됩니다. 기본 키 값은 속성 `0` 형식 (예: `int` `null` for, for `string`, 등)에 대 한 CLR 기본값이 할당 된 경우 "설정 되지 않음"으로 간주 됩니다.

        * 기본 키가 값으로 설정 된 경우 상태는 변경 되지 않음으로 설정 됩니다.

    *  기본 키가 데이터베이스에서 생성 되지 않은 경우 엔터티는 루트와 동일한 상태에 배치 됩니다.

### <a name="code-first-database-initialization"></a>Code First 데이터베이스 초기화

**EF6는 데이터베이스 연결을 선택 하 고 데이터베이스를 초기화 하는 데 상당한 양의 매직을 수행 합니다. 이러한 규칙 중 일부는 다음과 같습니다.**

* 구성을 수행 하지 않으면 EF6는 SQL Express 또는 LocalDb에서 데이터베이스를 선택 합니다.

* 컨텍스트와 이름이 같은 연결 문자열이 응용 프로그램 `App/Web.config` 파일에 있는 경우이 연결이 사용 됩니다.

* 데이터베이스가 존재 하지 않는 경우 만들어집니다.

* 데이터베이스에 모델의 테이블이 없으면 현재 모델에 대 한 스키마가 데이터베이스에 추가 됩니다. 마이그레이션을 사용 하는 경우에는 데이터베이스를 만드는 데 사용 됩니다.

* 데이터베이스가 존재 하 고 EF6에서 이전에 스키마를 만든 경우 스키마가 현재 모델과의 호환성을 검사 합니다. 스키마가 만들어진 후 모델이 변경 되 면 예외가 throw 됩니다.

**EF Core는이 매직을 수행 하지 않습니다.**

* 데이터베이스 연결은 코드에서 명시적으로 구성 해야 합니다.

* 초기화가 수행 되지 않습니다. 를 사용 하 `DbContext.Database.Migrate()` 여 마이그레이션을 적용 하거나 `DbContext.Database.EnsureCreated()` , `EnsureDeleted()` 마이그레이션을 사용 하지 않고 데이터베이스를 만들거나 삭제 하려면를 사용 해야 합니다.

### <a name="code-first-table-naming-convention"></a>Code First 테이블 명명 규칙

EF6는 복수화 서비스를 통해 엔터티 클래스 이름을 실행 하 여 엔터티가 매핑되는 기본 테이블 이름을 계산 합니다.

EF Core는 엔터티가 파생 컨텍스트에서 노출 `DbSet` 되는 속성의 이름을 사용 합니다. 엔터티에 `DbSet` 속성이 없으면 클래스 이름이 사용 됩니다.
