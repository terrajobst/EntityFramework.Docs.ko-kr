---
title: Entity Framework 용어집-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
uid: ef6/resources/glossary
ms.openlocfilehash: df0da4a68b3d2c882d9673417ee5fe335eccae2b
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656153"
---
# <a name="entity-framework-glossary"></a>Entity Framework 용어집
## <a name="code-first"></a>Code First
코드를 사용 하 여 Entity Framework 모델 만들기 모델은 기존 데이터베이스 또는 새 데이터베이스를 대상으로 할 수 있습니다.

## <a name="context"></a>컨텍스트
데이터베이스에 대 한 세션을 나타내는 클래스로,이를 통해 데이터를 쿼리하고 저장할 수 있습니다. 컨텍스트는 DbContext 또는 ObjectContext 클래스에서 파생 됩니다.

## <a name="convention-code-first"></a>규칙 (Code First)
클래스에서 모델 셰이프를 유추 하는 데 사용 Entity Framework 하는 규칙입니다.

## <a name="database-first"></a>Database First
EF Designer를 사용 하 여 기존 데이터베이스를 대상으로 하는 Entity Framework 모델 만들기

## <a name="eager-loading"></a>즉시 로드
특정 형식의 엔터티에 대 한 쿼리가 쿼리의 일부로 관련 엔터티를 로드 하는 관련 데이터를 로드 하는 패턴입니다.

## <a name="ef-designer"></a>EF 디자이너
상자와 선을 사용 하 여 Entity Framework 모델을 만들 수 있도록 하는 visual Studio의 비주얼 디자이너입니다.

## <a name="entity"></a>엔터티
고객, 제품 및 주문과 같은 애플리케이션 데이터를 나타내는 클래스 또는 개체입니다.

## <a name="entity-data-model"></a>엔터티 데이터 모델
엔터티 및 엔터티 간의 관계를 설명 하는 모델입니다. EF는 EDM을 사용 하 여 개발자 프로그램에 대 한 개념적 모델을 설명 합니다. EDM은 Peter에 의해 도입 된 엔터티 관계 모델을 기반으로 합니다. EDM은 원래 Microsoft의 개발자 및 서버 기술 제품군에 걸쳐 공통 데이터 모델을 사용 하는 주된 목표를 사용 하 여 개발 되었습니다. EDM은 OData 프로토콜의 일부로도 사용 됩니다.

## <a name="explicit-loading"></a>명시적 로드
API를 호출 하 여 관련 개체를 로드 하는 관련 데이터를 로드 하는 패턴입니다.

## <a name="fluent-api"></a>흐름 API
Code First 모델을 구성 하는 데 사용할 수 있는 API입니다.

## <a name="foreign-key-association"></a>외래 키 연결
외래 키를 나타내는 속성이 종속 엔터티의 클래스에 포함 되는 엔터티 간의 연결입니다. 예를 들어 Product에는 CategoryId 속성이 포함 되어 있습니다.

## <a name="identifying-relationship"></a>식별 관계
주 엔터티의 기본 키가 종속 엔터티의 기본 키 일부인 관계입니다. 이러한 종류의 관계에서 종속 엔터티는 주 엔터티 없이 존재할 수 없습니다.

## <a name="independent-association"></a>독립 연결
종속 엔터티의 클래스에서 외래 키를 나타내는 속성이 없는 엔터티 간의 연결입니다. 예를 들어 Product 클래스는 Category에 대 한 관계를 포함 하지만 CategoryId 속성은 포함 하지 않습니다. Entity Framework는 연결의 상태를 두 연결 end에 있는 엔터티 상태와 독립적으로 추적 합니다.

## <a name="lazy-loading"></a>지연 로드
탐색 속성에 액세스할 때 관련 개체가 자동으로 로드 되는 관련 데이터를 로드 하는 패턴입니다.

## <a name="model-first"></a>Model First
EF Designer를 사용 하 여 Entity Framework 모델을 만든 다음 새 데이터베이스를 만드는 데 사용 됩니다.

## <a name="navigation-property"></a>탐색 속성
다른 엔터티를 참조 하는 엔터티의 속성입니다. 예를 들어 Product에는 범주 탐색 속성이 포함 되 고 범주에는 Products 탐색 속성이 포함 됩니다.

## <a name="poco"></a>POCO
일반-이전 CLR 개체의 머리글자어입니다. 모든 프레임 워크와 종속성이 없는 간단한 사용자 클래스입니다. EF 컨텍스트에서 EntityObject에서 파생 되지 않은 엔터티 클래스는 인터페이스를 구현 하거나 EF에 정의 된 특성을 전달 합니다. 지 속성 프레임 워크에서 분리 된 엔터티 클래스도 "지 속성 무시" 라고 합니다.  

## <a name="relationship-inverse"></a>역 관계
관계의 반대쪽 끝 (예: product)입니다. 범주 및 범주. 제품은.

## <a name="self-tracking-entity"></a>자동 추적 엔터티
N 계층 개발에 도움이 되는 코드 생성 템플릿에서 작성 된 엔터티입니다.

## <a name="table-per-concrete-type-tpc"></a>TPC (구체적 테이블 유형별)
계층의 각 비추상 형식이 데이터베이스의 개별 테이블에 매핑되는 상속을 매핑하는 방법입니다.

## <a name="table-per-hierarchy-tph"></a>계층당 하나의 테이블 (TPH)
계층의 모든 형식이 데이터베이스의 동일한 테이블에 매핑되는 상속을 매핑하는 방법입니다. 판별자 열은 각 행이 연결 된 유형을 식별 하는 데 사용 됩니다.

## <a name="table-per-type-tpt"></a>형식당 하나의 테이블 (TPT)
계층 구조에 있는 모든 형식의 공통 속성이 데이터베이스의 동일한 테이블에 매핑되고 각 형식에 고유한 속성이 별도의 테이블에 매핑되는 상속을 매핑하는 방법입니다.

## <a name="type-discovery"></a>유형 검색
Entity Framework 모델의 일부가 되어야 하는 형식을 식별 하는 프로세스입니다.
