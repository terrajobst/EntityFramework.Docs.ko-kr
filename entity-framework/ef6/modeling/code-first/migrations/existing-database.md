---
title: 기존 데이터베이스-EF6 사용 하 여 code First 마이그레이션
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: f0cc4f93-67dd-4664-9753-0a9f913814db
caps.latest.revision: 3
ms.openlocfilehash: 77e139a29bb4708b00fc6198a57780ce75197252
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122773"
---
# <a name="code-first-migrations-with-an-existing-database"></a>기존 데이터베이스를 사용 하 여 code First 마이그레이션
> [!NOTE]
> **EF4.3 이상만** -Api 기능 등이이 페이지에 설명 된 Entity Framework 4.1에서 도입 되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

이 문서에서는 Code First 마이그레이션을 사용 하 여 Entity Framework에서 만들어지지 않았으면 하는 기존 데이터베이스를 사용 하 여 설명 합니다.

> [!NOTE]
> 이 문서에서는 기본 시나리오에서 Code First 마이그레이션을 사용 하는 방법을 알고 있다고 가정 합니다. 이렇게 하지 않으면 경우 읽기 해야 [Code First 마이그레이션을](~/ef6/modeling/code-first/migrations/index.md) 계속 하기 전에 합니다.

## <a name="screencasts"></a>스크린 캐스트

이 문서 보다 동영상을 가이드를 보려면 대신는 경우 다음 두 동영상이이 문서와 동일한 콘텐츠를 설명 합니다.

### <a name="video-one-migrations---under-the-hood"></a>비디오 1: "마이그레이션-내부 살펴보기"

[이 스크린 캐스트](http://channel9.msdn.com/blogs/ef/migrations-under-the-hood) 마이그레이션을 추적 하는 방법을 설명 하 고 모델에 대 한 정보를 사용 하 여 모델 변경 내용을 검색 합니다.

### <a name="video-two-migrations---existing-databases"></a>비디오 2: "마이그레이션-기존 데이터베이스"

이전 비디오에서 개념을 토대로 [이 스크린 캐스트](http://channel9.msdn.com/blogs/ef/migrations-existing-databases) 활성화 하 고 기존 데이터베이스를 사용 하 여 마이그레이션을 사용 하는 방법을 설명 합니다.

## <a name="step-1-create-a-model"></a>1 단계: 모델 만들기

먼저 기존 데이터베이스를 대상으로 하는 Code First 모델을 만들 수 있습니다. 합니다 [기존 데이터베이스에 Code First](~/ef6/modeling/code-first/workflows/existing-database.md) 항목에서는이 작업을 수행 하는 방법에 자세한 지침을 제공 합니다.

>[!NOTE]
> 데이터베이스 스키마를 변경 해야 하는 모델을 변경 하기 전에이 항목의 단계를 따릅니다 두는 것이 반드시 합니다. 다음 단계는 필요 동기화 되도록 모델 데이터베이스 스키마를 사용 하 여 합니다.

## <a name="step-2-enable-migrations"></a>2 단계: 마이그레이션 사용

마이그레이션을 사용 하도록 설정 하려면 다음 단계가입니다. 실행 하 여 이렇게 합니다 **Enable-migrations** 패키지 관리자 콘솔에서 명령을 합니다.

이 명령은 마이그레이션을 호출 하 여 솔루션에서 폴더를 만들고 구성 호출 내 단일 클래스에 저장 됩니다. 구성 클래스를 구성 하는 마이그레이션 응용 프로그램에 대 한에 대 한 자세한 내용을 확인할 수는 [Code First 마이그레이션을](~/ef6/modeling/code-first/migrations/index.md) 항목입니다.

## <a name="step-3-add-an-initial-migration"></a>3 단계: 초기 마이그레이션 추가

마이그레이션을 만든 후에 적용 되 면 다른 데이터베이스에 적용 하려면 로컬 데이터베이스 변경 합니다. 예를 들어, 로컬 데이터베이스 테스트 데이터베이스 및도 프로덕션 데이터베이스에 변경 내용을 적용 하려는 궁극적으로 및/또는 다른 개발자가 데이터베이스를 테스트 합니다. 이 단계는 방법은 두 가지가 및 다른 데이터베이스의 스키마가 비어 있거나 현재 로컬 데이터베이스의 스키마와 일치 여부를 선택 해야 하는 것에 따라 달라 집니다.

-   **옵션 1: 시작 지점으로 기존 스키마를 사용 합니다.** 로컬 데이터베이스가 현재 되었으므로 나중에 마이그레이션에 적용 됩니다는 다른 데이터베이스에서 동일한 스키마를 해야 하는 경우이 방법을 사용 해야 합니다. 예를 들어, 로컬 테스트 데이터베이스에는 현재 프로덕션 데이터베이스의 v1 일치 하 고 이러한 마이그레이션을 v2로 프로덕션 데이터베이스를 업데이트 하려면 나중에 적용할 경우이 사용할 수 있습니다.
-   **옵션 2: 시작 지점으로 빈 데이터베이스를 사용 합니다.** 비어 있는 (또는 아직 존재 하지 않는) 나중에 마이그레이션에 적용 됩니다는 다른 데이터베이스가이 방법을 사용 해야 합니다. 예를 들어 테스트 데이터베이스를 사용 하 여 응용 프로그램 개발을 시작 해도 마이그레이션을 하를 사용 하지 않고 나중에 할부터 프로덕션 데이터베이스를 만드는 경우이 사용할 수 있습니다.

### <a name="option-one-use-existing-schema-as-a-starting-point"></a>옵션 1: 기존 스키마를 사용 하 여 시작 지점으로

Code First 마이그레이션을 가장 최근 마이그레이션에 저장 된 모델의 스냅숏을 사용 하 여 모델 변경 내용을 검색 (이에 대 한 자세한 정보를 찾을 수 있습니다 [팀 환경에서 Code First 마이그레이션을](~/ef6/modeling/code-first/migrations/teams.md)). 데이터베이스에 이미 현재 모델의 스키마는 가정 것 이므로 스냅숏으로 현재 모델이 비어 있는 (아무) 마이그레이션을 생성 합니다.

1.  실행 합니다 **Add-migration InitialCreate – IgnoreChanges** 패키지 관리자 콘솔에서 명령을 합니다. 이 빈 마이그레이션 스냅숏으로 현재 모델을 만듭니다.
2.  실행 합니다 **Update-database** 패키지 관리자 콘솔에서 명령을 합니다. InitialCreate 마이그레이션을 데이터베이스에 적용 됩니다. 행을 간단히 추가 실제 마이그레이션을 내용이 들어 있으므로 합니다 \_ \_MigrationsHistory 테이블 나타내는이 마이그레이션이 이미 적용 되었습니다.

### <a name="option-two-use-empty-database-as-a-starting-point"></a>옵션 2: 빈 데이터베이스를 사용 하 여 시작 지점으로

이 시나리오에서는 로컬 데이터베이스에 존재 하는 테이블을 포함 하 여부터 전체 데이터베이스를 만들 수로 마이그레이션이 필요 합니다. 기존 스키마를 만들기 위한 논리를 포함 하는 InitialCreate 마이그레이션을 생성 하겠습니다. 그런 다음 같습니다이 마이그레이션이 이미 적용 된 기존 데이터베이스를 만들 수 것입니다.

1.  실행 합니다 **Add-migration InitialCreate** 패키지 관리자 콘솔에서 명령을 합니다. 이 스키마를 만들려면 기존 마이그레이션을 만듭니다.
2.  새로 만든 마이그레이션의 최대 메서드의 모든 코드 주석 처리 합니다. 이렇게 하면 '적용' 마이그레이션을 로컬 데이터베이스에 모든 테이블 등 이미 존재 하는 다시 만들지 않고 수 있습니다.
3.  실행 합니다 **Update-database** 패키지 관리자 콘솔에서 명령을 합니다. InitialCreate 마이그레이션을 데이터베이스에 적용 됩니다. 실제 마이그레이션 포함 되어 있지 않습니다 (에서는 일시적으로 주석 처리 된 수) 때문에 변경, 행을 추가 하기만 하면 됩니다 합니다 \_ \_MigrationsHistory 테이블 나타내는이 마이그레이션이 이미 적용 되었습니다.
4.  최대 메서드의 코드에서에서 취소-주석입니다. 즉,이 마이그레이션 이후 데이터베이스에 적용 될 때 로컬 데이터베이스에 이미 존재 하는 스키마가 마이그레이션 하 여 생성 됩니다.

## <a name="things-to-be-aware-of"></a>알아야 할 사항

몇 가지 방법으로 기존 데이터베이스에 대 한 마이그레이션을 사용할 때 주의 해야 합니다.

### <a name="defaultcalculated-names-may-not-match-existing-schema"></a>기본/계산 이름을 기존 스키마와 일치 하지 않을 수 있습니다.

마이그레이션은 마이그레이션을 스 캐 폴딩이 열 및 테이블 이름을 명시적으로 지정 합니다. 그러나 마이그레이션을 적용 하는 경우 마이그레이션에 대 한 기본 이름이 계산 하는 다른 데이터베이스 개체가 있습니다. 인덱스와 foreign key 제약 조건이 포함 됩니다. 기존 스키마를 대상으로 하는 경우 이러한 계산된 이름이 실제로 존재 하는 데이터베이스에서 일치 하지 않을 수 있습니다.

이러한 문제에 유의 해야 할 경우 몇 가지 예가 다음과 같습니다.

**사용 하는 경우 ' 옵션 1: 기존 스키마를 사용 하 여 시작 지점으로 ' 3 단계에서에서:**

-   향후 변경 내용을 모델에 필요한 변경 하거나 다르게 명명 된 데이터베이스 개체 중 하나를 삭제 하는 경우 올바른 이름을 지정 하는 스 캐 폴드 된 마이그레이션 수정 해야 합니다. 마이그레이션 Api에는 이렇게 할 수 있는 선택적 이름 매개 변수입니다.
    예를 들어, 기존 스키마 IndexFk 라는 인덱스를 가진 BlogId 외래 키 열을 사용 하 여 Post 테이블 있을\_BlogId 합니다. 그러나 기본적으로 마이그레이션 기대 IX 이라는 이름으로이 인덱스\_BlogId 합니다. 지정 된 IndexFk 스 캐 폴드 된 DropIndex 호출을 수정 해야이 인덱스를 삭제 하는 모델을 변경한 경우\_BlogId 이름입니다.

**사용 하는 경우 ' 옵션 2: 시작 점으로 사용 하 여 빈 데이터베이스 ' 3 단계에서에서:**

-   마이그레이션에서는 인덱스 및 잘못 된 이름을 사용 하 여 foreign key 제약 조건을 삭제 하려고 하 여 로컬 데이터베이스에 대해 초기 마이그레이션 (즉, 빈 데이터베이스를 되돌리기) 아래쪽 메서드를 실행 하는 동안 실패할 수 있습니다. 다른 데이터베이스를 초기 마이그레이션의 최대 메서드를 사용 하 여 처음부터 만들 때문에이 로컬 데이터베이스에 적용 됩니다.
    빈 상태로 기존 로컬 데이터베이스를 다운 그레이드 하려면 데이터베이스를 삭제 하거나 모든 테이블을 삭제 하 여이 작업을 수행 하려면 쉽습니다. 이 초기 다운 그레이드 후 모든 데이터베이스 개체를 기본 이름으로 재생성 됩니다, 따라서이 문제를 제공 하지 않습니다 자체 다시 합니다.
-   향후 변경 내용을 모델에 필요한 변경 하거나 다르게 명명 된 데이터베이스 개체 중 하나를 삭제 하는 경우이 작동 하지 않습니다 – 기존 로컬 데이터베이스에 대해 이름을 기본값와 일치 하지 않습니다. 그러나 마이그레이션 하 여 선택 된 기본 이름을 사용 하는 '부터' 생성 된 데이터베이스에 대해 작동 합니다.
    로컬 기존 데이터베이스에서 이러한 변경을 수동으로 수행 또는 다른 컴퓨터 에서도 마이그레이션을부터 데이터베이스를 다시 고려할 수 있습니다.
-   초기 마이그레이션 위쪽 메서드를 사용 하 여 만든 데이터베이스 수 약간과 다를 로컬 데이터베이스 인덱스에 대 한 계산 된 기본 이름을 이후 foreign key 제약 조건이 사용 됩니다. 얻게 될 수 있습니다도 추가 인덱스를 사용 하 여 마이그레이션을 기본적으로 외래 키 열에 인덱스를 만들는 –이 되지 않은 원래 로컬 데이터베이스의 경우 처럼 합니다.

### <a name="not-all-database-objects-are-represented-in-the-model"></a>일부 데이터베이스 개체는 모델에 표시 됩니다.

데이터베이스 개체 모델의 일부분이 아닌 마이그레이션을 하 여 처리 되지 않습니다. 이 뷰, 저장된 프로시저, 권한, 모델, 추가 인덱스, 등 포함 되지 않은 테이블에 포함할 수 있습니다.

이러한 문제에 유의 해야 할 경우 몇 가지 예가 다음과 같습니다.

-   옵션에 관계 없이 선택한 '3 단계의' 모델의 향후 변경 내용을 변경 하거나 마이그레이션은 이러한 변경을 수행 하지 못합니다 이러한 추가 개체를 삭제 해야 하는 경우입니다. 예를 들어 추가 인덱스에 있는 열을 삭제 하는 경우 마이그레이션은 하지 못합니다 인덱스를 삭제 합니다. 스 캐 폴드 된 마이그레이션에 수동으로 추가 해야 합니다.
-   사용 하는 경우 ' 옵션 2: 시작 점으로 사용 하 여 빈 데이터베이스 ', 이러한 추가 개체 초기 마이그레이션의 최대 메서드에 의해 생성 되지 것입니다.
    원하는 경우 이러한 추가 개체 주의 하는 메서드 및 위쪽을 수정할 수 있습니다. 보기 – 예: 마이그레이션 api – 고유 하 게 지원 되지 않는 개체에 대해 사용할 수 있습니다 합니다 [Sql](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.sql.aspx) 하 만들기/삭제를 원시 SQL을 실행 하는 방법입니다.