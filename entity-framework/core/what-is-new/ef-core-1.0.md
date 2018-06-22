---
title: EF Core 1.0의 새로운 기능 - EF Core
author: divega
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: e5b9e57a01ff302b1d7bd0fc5419aa5b8213865e
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26049686"
---
# <a name="features-included-in-ef-core-10"></a>EF Core 1.0에 포함된 기능

## <a name="platforms"></a>플랫폼
### <a name="net-framework-451"></a>.NET Framework 4.5.1
콘솔, WPF, WinForms, ASP.NET 4 등을 포함합니다.
### <a name="net-standard-13"></a>.NET Standard 1.3
Windows, OSX 및 Linux에서 .NET Framework 및 .NET Core를 둘 다 대상으로 하는 ASP.NET Core를 포함합니다.

## <a name="modelling"></a>모델링
### <a name="basic-modelling"></a>기본 모델링
일반적인 스칼라 형식(`int`, `string` 등)의 get/set 속성이 있는 POCO 엔터티를 기반으로 합니다.
### <a name="relationships-and-navigation-properties"></a>관계 및 탐색 속성
외래 키를 기반으로 모델에서 일 대 다 및 일 대 영/일 관계를 지정할 수 있습니다. 단순 컬렉션 또는 참조 형식의 탐색 속성은 이러한 관계와 연결될 수 있습니다.
### <a name="built-in-conventions"></a>기본 제공 규칙
이러한 기능은 엔터티 클래스의 모양을 기반으로 초기 모델을 빌드합니다.
### <a name="fluent-api"></a>흐름 API
컨텍스트에서 `OnModelCreating` 메서드를 재정의하여 규칙에 의해 발견된 모델을 추가로 구성할 수 있습니다.
### <a name="data-annotations"></a>데이터 주석
엔터티 클래스/속성에 추가할 수 있고 EF 모델에 영향을 주는 속성입니다([필수]를 추가하여 EF에 속성이 추가임을 알림).
### <a name="relational-table-mapping"></a>관계형 테이블 매핑
엔터티를 테이블/열에 매핑할 수 있습니다.
### <a name="key-value-generation"></a>키 값 생성
클라이언트 쪽 생성 및 데이터베이스 생성을 포함합니다.
### <a name="database-generated-values"></a>데이터베이스 생성 값
삽입(기본값) 또는 업데이트(계산된 열)에 대한 값을 데이터베이스에서 생성할 수 있습니다.
### <a name="sequences-in-sql-server"></a>SQL Server의 시퀀스
시퀀스 개체를 모델에 정의할 수 있습니다.
### <a name="unique-constraints"></a>Unique 제약 조건
대체 키 정의 및 해당 키를 대상으로 하는 관계를 정의하는 기능을 허용합니다.
### <a name="indexes"></a>인덱스
모델의 인덱스를 정의하면 데이터베이스에 인덱스가 자동으로 포함됩니다. 고유 인덱스도 지원됩니다.
### <a name="shadow-state-properties"></a>섀도 상태 속성
선언되지 않고 .NET 클래스에 저장되지 않았지만 EF Core에서 추적 및 업데이트할 수 있는 속성을 모델에 정의할 수 있습니다. 외래 키를 개체에 노출하지 않는 것이 좋을 경우 일반적으로 외래 키 속성에 사용됩니다.
### <a name="table-per-hierarchy-inheritance-pattern"></a>계층별 테이블 상속 패턴
데이터베이스의 지정된 레코드에 대한 엔터티 형식을 식별하기 위해 판별자 열을 사용하여 상속 계층 구조의 엔터티를 단일 테이블에 저장할 수 있습니다.
### <a name="model-validation"></a>모델 유효성 검사
모델에서 잘못된 패턴을 검색하고 유용한 오류 메시지를 제공합니다.

## <a name="change-tracking"></a>Change tracking
### <a name="snapshot-change-tracking"></a>스냅숏 변경 내용 추적
현재 상태를 원래 상태의 복사본(스냅숏)과 비교하여 엔터티의 변경 내용을 자동으로 검색할 수 있습니다.
### <a name="notification-change-tracking"></a>알림 변경 내용 추적
속성 값이 수정될 경우 엔터티가 변경 추적 프로그램에 알릴 수 있습니다.
### <a name="accessing-tracked-state"></a>추적된 상태 액세스
`DbContext.Entry` 및 `DbContext.ChangeTracker`를 사용합니다.
### <a name="attaching-detached-entitiesgraphs"></a>분리된 엔터티/그래프 연결
새 `DbContext.AttachGraph` API를 통해 새로운/수정된 엔터티를 저장하기 위해 엔터티를 컨텍스트에 다시 연결할 수 있습니다.

## <a name="saving-data"></a>데이터 저장
### <a name="basic-save-functionality"></a>기본 저장 기능
엔터티 인스턴스의 변경 내용이 데이터베이스에 지속될 수 있습니다.
### <a name="optimistic-concurrency"></a>낙관적 동시성
데이터베이스에서 데이터가 페치된 이후 다른 사용자가 변경한 내용을 덮어쓰지 못하도록 합니다.
### <a name="async-savechanges"></a>비동기 SaveChanges
데이터베이스가 `SaveChanges`에서 실행된 명령을 처리하는 동안 다른 요청을 처리하도록 현재 스레드를 해제할 수 있습니다.
### <a name="database-transactions"></a>데이터베이스 트랜잭션
`SaveChanges`는 항상 원자성입니다(완전히 성공하거나 데이터베이스가 변경되지 않음). 컨텍스트 인스턴스 간에 트랜잭션 공유를 허용하는 트랜잭션 관련 API도 있습니다.
### <a name="relational-batching-of-statements"></a>관계형: 문 일괄 처리
여러 INSERT/UPDATE/DELETE 명령을 데이터베이스에 대한 단일 왕복으로 일괄 처리하여 성능을 개선합니다.

## <a name="query"></a>Query
### <a name="basic-linq-support"></a>기본 LINQ 지원
LINQ를 사용하여 데이터베이스에서 데이터를 검색하는 기능을 제공합니다.
### <a name="mixed-clientserver-evaluation"></a>혼합 클라이언트/서버 평가
데이터베이스에서 평가할 수 없는 논리를 쿼리에 포함할 수 있으므로 데이터가 메모리로 검색된 후 평가되어야 합니다.
### <a name="notracking"></a>NoTracking
쿼리를 사용하면 컨텍스트가 엔터티 인스턴스의 변경 내용을 모니터링할 필요가 없을 경우(결과가 읽기 전용일 경우) 쿼리 실행이 빨라집니다.
### <a name="eager-loading"></a>즉시 로드
쿼리 시 페치해야하는 관련 데이터를 식별하는 `Include` 및 `ThenInclude` 메서드를 제공합니다.
### <a name="async-query"></a>비동기 쿼리
데이터베이스가 쿼리를 처리하는 동안 다른 요청을 처리하도록 현재 스레드 및 연결된 리소스를 해제할 수 있습니다.
### <a name="raw-sql-queries"></a>원시 SQL 쿼리
원시 SQL 쿼리를 사용하여 데이터를 페치하는 `DbSet.FromSql` 메서드를 제공합니다. 이러한 쿼리는 LINQ 사용 시 작성할 수도 있습니다.

## <a name="database-schema-management"></a>데이터베이스 스키마 관리       
### <a name="database-creationdeletion-apis"></a>데이터베이스 만들기/삭제 API
일반적으로 마이그레이션을 사용하지 않고 데이터베이스를 빠르게 생성/삭제하려는 테스트용으로 디자인됩니다.
### <a name="relational-database-migrations"></a>관계형 데이터베이스 마이그레이션
시간이 지나면서 모델이 변경됨에 따라 관계형 데이터베이스 스키마를 진전시킬 수 있습니다.
### <a name="reverse-engineer-from-database"></a>데이터베이스의 리버스 엔지니어링
기존 관계형 데이터베이스 스키마를 기반으로 EF 모델을 스캐폴딩합니다.

## <a name="database-providers"></a>데이터베이스 공급자
### <a name="sql-server"></a>SQL Server
Microsoft SQL Server 2008 이상에 연결합니다.
### <a name="sqlite"></a>SQLite
SQLite 3 데이터베이스에 연결합니다.
### <a name="in-memory"></a>메모리 내
실제 데이터베이스에 연결하지 않고 쉽게 테스트할 수 있도록 디자인되었습니다.
### <a name="3rd-party-providers"></a>타사 공급자
다른 데이터베이스 엔진에 대해 여러 공급자를 사용할 수 있습니다. 전체 목록은 [데이터베이스 공급자](../providers/index.md)를 참조하세요.
