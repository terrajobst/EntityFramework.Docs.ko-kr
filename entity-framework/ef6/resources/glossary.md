---
title: Entity Framework Glossary - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
ms.openlocfilehash: 4e42e5870879524b814cecdc361e688d36f0180f
ms.sourcegitcommit: 6c4e06bc62d98442530e93a44725e38e59483d42
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58131381"
---
# <a name="entity-framework-glossary"></a>Entity Framework 용어집
## <a name="code-first"></a>Code First
코드를 사용 하 여 Entity Framework 모델을 만드는 중입니다. 모델은 데이터베이스를 기존 또는 새 데이터베이스를 대상으로 지정할 수 있습니다.

## <a name="context"></a>컨텍스트
데이터베이스를 쿼리하고 데이터를 저장할 수 있도록 세션을 나타내는 클래스입니다. 컨텍스트는 DbContext 나 ObjectContext 클래스에서 파생 됩니다.

## <a name="convention-code-first"></a>규칙 (첫 번째 코드)
Entity Framework를 사용 하 여 수업에서 모델의 형태를 유추 하는 규칙.

## <a name="database-first"></a>먼저 데이터베이스
EF 디자이너를 사용 하 여 Entity Framework 모델을 만드는 대상으로 하는 기존 데이터베이스입니다.

## <a name="eager-loading"></a>즉시 로드
여기서 한 형식의 엔터티에 대 한 쿼리를 관련된 엔터티도 로드를 쿼리의 일부로 관련된 데이터를 로드 하는 패턴입니다.

## <a name="ef-designer"></a>EF 디자이너
상자 및 줄을 사용 하 여 Entity Framework 모델을 만들 수 있도록 Visual Studio의 시각적 디자이너입니다.

## <a name="entity"></a>엔터티
고객, 제품 및 주문과 같은 응용 프로그램 데이터를 나타내는 클래스 또는 개체입니다.

## <a name="entity-data-model"></a>엔터티 데이터 모델
엔터티 및 이들 간의 관계를 설명 하는 모델입니다. 개념적 모델을 설명 하기 위해 EDM을 사용 하는 EF 개발자 프로그램입니다. Dr에 도입 된 엔터티 관계 모델을 기반으로 EDM입니다. Peter Chen 합니다. EDM은 Microsoft의 개발자 및 서버 기술 제품군 한다는 공통 데이터 모델의 주요 목표를 사용 하 여 원래 개발 되었습니다. EDM은 OData 프로토콜의 일부로 사용 됩니다.

## <a name="explicit-loading"></a>명시적 로드
관련된 개체가 로드 되는 API를 호출 하 여 관련된 데이터를 로드 하는 패턴입니다.

## <a name="fluent-api"></a>Fluent API
Code First 모델을 구성 하는 데 사용할 수 있는 API입니다.

## <a name="foreign-key-association"></a>외래 키 연결
외래 키를 나타내는 속성의 종속 엔터티 클래스에 포함 되어 있는 엔터티 간의 연결입니다. 예를 들어 제품 CategoryId 속성을 포함합니다.

## <a name="identifying-relationship"></a>관계를 식별합니다.
주 엔터티의 기본 키가 종속 엔터티의 기본 키 일부인 관계입니다. 이러한 종류의 관계에서 종속 엔터티는 주 엔터티 없이 존재할 수 없습니다.

## <a name="independent-association"></a>독립 연결
엔터티 간 연결의 종속 엔터티 클래스에 외래 키를 나타내는 속성이 없는 경우. 예를 들어 Product 클래스는 범주 있지만 CategoryId 속성이 없는 관계를 포함합니다. Entity Framework는 두 개의 연결 end에 있는 엔터티의 상태와 독립적으로 연결의 상태를 추적 합니다.

## <a name="lazy-loading"></a>지연 로드
패턴 탐색 속성에 액세스할 때 관련된 개체가 로드 자동으로 관련된 데이터 로드입니다.

## <a name="model-first"></a>먼저 모델
EF 디자이너를 사용 하 여 Entity Framework 모델을 만드는 다음 데 사용 되는 새 데이터베이스를 만듭니다.

## <a name="navigation-property"></a>탐색 속성
다른 엔터티를 참조 하는 엔터티는 속성입니다. 예를 들어 제품 범주 탐색 속성을 포함 및 범주 제품 탐색 속성을 포함 합니다.

## <a name="poco"></a>POCO
Plain Old CLR 개체에 대 한 머리글자어입니다. 간단한 사용자 정의 클래스에서 임의 프레임 워크 종속성이 없는입니다. EF EntityObject에서 파생 되지 않은 모든 인터페이스를 구현, EF에 정의 된 모든 특성을 전달 하는 엔터티 클래스의 컨텍스트에서 합니다. 지 속성 프레임 워크에서 분리 되는 이러한 엔터티 클래스는 "지 속성 무시" 있다고도 합니다.  

## <a name="relationship-inverse"></a>관계 역
예를 들어 제품 관계의 반대쪽 끝에 있습니다. 범주 및 범주입니다. 제품입니다.

## <a name="self-tracking-entity"></a>자동 추적 엔터티
N 계층 개발에 도움이 되는 코드 생성 템플릿에서 만들어진 엔터티.

## <a name="table-per-concrete-type-tpc"></a>구체적 형식당 테이블 형식 (TPC)
상속 계층 구조의 각 추상이 아닌 형식 구분 데이터베이스의 테이블에 매핑되는 위치에 매핑하는 방법.

## <a name="table-per-hierarchy-tph"></a>테이블-당-TPH (계층)
상속 계층 구조의 모든 형식은 데이터베이스에서 동일한 테이블에 매핑되는 위치에 매핑하는 방법. 판별자 열은 각 행 형식을 찾는 데 사용 하 여 연결입니다.

## <a name="table-per-type-tpt"></a>테이블 형식당 TPT)
매핑 상속 계층 구조에서 모든 형식의 공용 속성을 데이터베이스에서 동일한 테이블에 매핑됩니다. 하지만 각 유형에 고유한 속성은 별도 테이블에 매핑됩니다 위치 하는 방법.

## <a name="type-discovery"></a>형식 검색
Entity Framework 모델의 일부 여야 하는 형식을 식별 하는 프로세스입니다.
