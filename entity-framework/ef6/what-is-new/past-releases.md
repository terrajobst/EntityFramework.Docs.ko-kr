---
title: Entity Framework-EF6의 이전 릴리스
author: divega
ms.date: 10/23/2016
ms.assetid: 1060bb99-765f-4f32-aaeb-d6635d3dbd3e
ms.openlocfilehash: 4fa27f8259ecc011d9d30080aee3c44353ef533d
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283994"
---
# <a name="past-releases-of-entity-framework"></a>Entity Framework의 이전 릴리스

Entity Framework의 첫 번째 버전 2008에서는.NET Framework 3.5 SP1 및 Visual Studio 2008 SP1의 일부로 릴리스 되었습니다.

로 제공에 EF4.1 릴리스부터 [EntityFramework NuGet 패키지](https://www.nuget.org/packages/EntityFramework/) -현재 NuGet.org에서 가장 인기 있는 패키지 중 하나입니다.

버전 4.1 및 5.0 간에 EntityFramework NuGet 패키지가.NET Framework의 일부로 제공 된 EF 라이브러리를 확장 합니다.   

버전 6 부터는 EF가 오픈 소스 프로젝트 및도 이동 완전히 대역 외 형식에서.NET Framework입니다.
이 EntityFramework 버전 6 NuGet 패키지를 응용 프로그램에 추가할 때 발생 하는.NET Framework의 일부로 제공 되는 EF 비트에 종속 되지 않는 EF 라이브러리의 전체 복사본을 의미 합니다.
이 통해 새 기능 및 개발 속도 가속화 다소 있었습니다.

2016 년 6 월에에서 EF Core 1.0 발표 했습니다. EF Core는 새 코드 베이스를 기반으로 및 EF의 가볍고 확장 가능 버전으로 설계 되었습니다.
현재 EF Core는 microsoft Entity Framework 팀에 대 한 개발의 주요 초점은입니다.
즉,는 EF6에 대 한 계획 된 주요 기능이 없습니다. 그러나 EF6 오픈 소스 프로젝트와 지원 되는 Microsoft 제품으로 계속 유지 됩니다.

각 릴리스에서 도입 된 새로운 기능에 대 한 정보를 사용 하 여 역방향 시간 순서로 과거 버전의 목록은 다음과 같습니다.

## <a name="ef-613"></a>EF 6.1.3
EF 6.1.3 런타임 2015 년 10 월에에서 nuget 릴리스 되었습니다.
이 릴리스에 포함 우선 순위가 높은 결함 및 재발을 6.1.2 보고만 수정 됩니다. 릴리스 합니다.
수정 사항은 다음과 같습니다.

- 쿼리: EF 6.1.2에에서 회귀: OUTER APPLY 도입 및 1:1 관계 및 "let" 절에 대 한 더 복잡 한 쿼리
- TPT 상속 된 클래스에서 기본 클래스 속성 숨기기 문제가
- DbMigration.Sql 실패 ' go ' 텍스트에 포함 된 경우
- UnionAll 및 Intersect 평면화 지원에 대 한 호환성 플래그 만들기
- 여러 포함을 사용 하 여 쿼리 6.1.2 (6.1.1 작업)에서 작동 하지 않습니다.
- EF 6.1.2에 6.1.1에서에서 업그레이드 한 후 "오류가 있는 SQL 구문에서" 예외

## <a name="ef-612"></a>EF 6.1.2
EF 6.1.2 런타임 2014 년 12 월에에서 nuget 릴리스 되었습니다.
이 버전은 주로 버그 수정에 대 한 합니다. 또한 커뮤니티의 멤버에서 중요 한 변경 내용이 몇을 수락 했습니다.
- **App/web.configuration 파일에서 쿼리 캐시 매개 변수를 구성할 수 있습니다.**
    ``` xml
    <entityFramework>
      <queryCache size='1000' cleaningIntervalInSeconds='-1'/>
    </entityFramework>
    ```
- **SqlFile 및 SqlResource DbMigration 메서드** SQL을 실행할 수 있도록 스크립트 파일로 저장 하거나 리소스를 포함 합니다.

## <a name="ef-611"></a>EF 6.1.1
6.1.1 EF 런타임 2014 년 6 월에에서 nuget 릴리스 되었습니다.
이 버전 사람의 수에서 발견 된 문제에 대 한 수정 프로그램이 포함 되어 있습니다. 특히:
- EF6 디자이너에서 전체 자릿수를 사용 하 여 EF5 edmx를 열어 디자이너: 오류
- LocalDB에 대 한 기본 인스턴스 검색 논리 SQL Server 2014에서 작동 하지 않습니다.

## <a name="ef-610"></a>EF 6.1.0
EF 6.1.0 런타임 2014 년 3 월에에서 nuget 릴리스 되었습니다.
이 부 버전 업데이트는 많은 새로운 기능을 포함합니다.

- **통합 도구** 새 EF 모델을 만드는 일관적인 방법을 제공 합니다. 이 기능은 [Code First 모델을 만드는 지원 하기 위해 ADO.NET 엔터티 데이터 모델 마법사 확장](~/ef6/modeling/code-first/workflows/existing-database.md)를 포함 하 여 기존 데이터베이스에서 리버스 엔지니어링 합니다. 이러한 기능은 이전에 베타 품질 EF Power Tools에서 사용할 수 있었습니다.
- **[트랜잭션 커밋 오류 처리](~/ef6/fundamentals/connection-resiliency/commit-failures.md)**  사용 하는 새로 도입 된 기능의 트랜잭션 작업을 가로채 CommitFailureHandler를 제공 합니다. CommitFailureHandler 트랜잭션 커밋 시 연결 오류 로부터 자동 복구를 허용 합니다.
- **[IndexAttribute](~/ef6/modeling/code-first/data-annotations.md)**  인덱스를 배치 하 여 지정할 수는 `[Index]` 속성 (또는 속성)에서 Code First 모델의 특성입니다. 먼저 코드 데이터베이스에서 해당 인덱스를 만들 다음 됩니다.
- **공용 매핑 API** EF가 속성 및 형식에 매핑되는 방법을 데이터베이스의 열과 테이블에 정보에 대 한 액세스를 제공 합니다. 이전 릴리스에서에서이 API를 내부.
- **[App/Web.config 파일을 통해 인터셉터를 구성 하는 기능](~/ef6/fundamentals/configuring/config-file.md)**  인터셉터를 응용 프로그램을 다시 컴파일하지 않고 추가할 수 있습니다.
- **System.Data.Entity.Infrastructure.Interception.DatabaseLogger**쉽게 파일에 모든 데이터베이스 작업을 기록할 새 인터셉터가 있습니다. 이전 기능와 함께에서이 수 있습니다 쉽게 [배포 된 응용 프로그램에 대 한 데이터베이스 작업의 로깅을 켜려면](~/ef6/fundamentals/configuring/config-file.md)를 다시 컴파일할 필요 없이 합니다.
- **마이그레이션 모델 변경 내용 검색** 스 캐 폴드 된 마이그레이션 보다 정확한;도 변경 검색 프로세스의 성능이 향상 되었습니다 되도록 향상 되었습니다.
- **성능 향상** 초기화 하는 동안 감소 데이터베이스 작업을 포함 하 여 LINQ 쿼리에 null 같음 비교에 대 한 최적화 더 빠르게 생성 (모델 생성) 및에서 볼 더 많은 시나리오를 더 효율적 여러 연결을 사용 하 여 추적 된 엔터티의 구체화 합니다.

## <a name="ef-602"></a>EF 6.0.2
EF 6.0.2 런타임은 NuGet에 2013 년 12 월에에서 발표 되었습니다.
이 패치 릴리스에서 도입 된 문제 해결로 제한 됩니다 (이후 EF5 성능/동작에서 재발) EF6 릴리스에서 합니다.

## <a name="ef-601"></a>EF 6.0.1
EF 6.0.1 런타임 nuget에 릴리스된 2013 년 10 월 6.0.0, EF 사용 하 여 동시에 몇 개월 전에 잠겨 있었습니다 Visual Studio의 버전에 포함 된 두 번째 때문입니다.
이 패치 릴리스에서 도입 된 문제 해결로 제한 됩니다 (이후 EF5 성능/동작에서 재발) EF6 릴리스에서 합니다.
EF 모델에 대 한 준비 하는 동안 몇 가지 성능 문제를 해결 하려면 가장 두드러진 변화 되었습니다.
EF6에서 다른 성능 향상의 일부 영역 EF6 및 이러한 문제에 초점을 부정 된 준비 성능 처럼 중요 한 경우

## <a name="ef-60"></a>EF 6.0
EF 6.0.0 런타임은 NuGet에 2013 년 10 월에에서 발표 되었습니다.
이 첫 번째 버전에 포함 되어 있는 전체 EF 런타임 합니다 [EntityFramework NuGet 패키지](https://www.nuget.org/packages/EntityFramework/) 는.NET Framework의 일부인 EF 비트에 종속 되지 않습니다.
다양 한 주요 변경이 기존 코드에 대 한 필요 런타임에서의 나머지 부분은 NuGet 패키지를 이동 합니다.
섹션을 참조 하세요 [Entity Framework 6으로 업그레이드](upgrading-to-ef6.md) 업그레이드 하는 데 필요한 수동 단계에 대 한 자세한 내용은 합니다.

이 릴리스에서 다양 한 새 기능을 포함합니다.
다음 기능은 Code First 또는 EF 디자이너를 사용 하 여 만든 모델에 대 한 작동 합니다.

- **[비동기 쿼리 및 저장](~/ef6/fundamentals/async.md)**  .NET 4.5에서 도입 된 작업 기반 비동기 패턴에 대 한 지원을 추가 합니다.
- **[연결 복원 력](~/ef6/fundamentals/connection-resiliency/retry-logic.md)**  일시적인 연결 오류 로부터 자동 복구를 사용 하도록 설정 합니다.
- **[코드 기반 구성을](~/ef6/fundamentals/configuring/code-based.md)**  구성 – 구성 파일에 일반적으로 수행 된 코드에서 수행 하는 옵션을 제공 합니다.
- **[종속성 확인](~/ef6/fundamentals/configuring/dependency-resolution.md)**  지원이 도입 되어 서비스 로케이터에 대 한 패턴 및에서는 했습니다 팩터링 일부의 기능을 사용자 지정 구현으로 바꿀 수 있습니다.
- **[인터 셉 션/SQL 로깅](~/ef6/fundamentals/logging-and-interception.md)**  에 구축 하는 간단한 SQL 로깅을 사용 하 여 EF 연산의 인터 셉 션 용 저수준 구성 요소를 제공 합니다.
- **테스트 용이성 향상** 쉽게 DbContext 및 DbSet에 대 한 test double을 만들 때 [모의 프레임 워크를 사용 하 여](~/ef6/fundamentals/testing/mocking.md) 또는 [test double을 직접 작성](~/ef6/fundamentals/testing/writing-test-doubles.md)합니다.
- **[DbContext 이미 열려 있는 DbConnection를 사용 하 여 만들 수 있습니다](~/ef6/fundamentals/connection-management.md)**  여기서는 것이 좋습니다 컨텍스트를 만드는 경우 연결 열기 수 있으면 시나리오 수 있습니다 (구성 요소 간의 연결을 공유 하는 같은 위치 보장할 수 있습니다 하지 연결의 상태).
- **[트랜잭션 지원 향상](~/ef6/saving/transactions.md)**  트랜잭션 프레임 워크 내에서 만드는 향상 된 방법 뿐만 아니라 프레임 워크 외부 트랜잭션에 대 한 지원을 제공 합니다.
- **열거형, 공간 및.NET 4.0에서 더 나은 성능을** -.NET에서 EF5에서 열거형 지원, 공간 데이터 형식 및 성능 향상을 제공할 수 있게 되었습니다 EF NuGet 패키지에.NET Framework의 수 하는 핵심 구성 요소를 이동 하 여 4.0입니다.
- **LINQ 쿼리에서 Enumerable.Contains의 성능이 향상**합니다.
- **향상 된 준비 시간 (view generation)**, 특히 큰 모델에 대 한 합니다.
- **플러그형 복수화 &amp; 단수형 서비스**합니다.
- **사용자 지정 구현을 GetHashCode 또는 Equals** 클래스는 이제 엔터티에 대해 지원 합니다.
- **DbSet.AddRange/RemoveRange** 추가 하거나 여러 엔터티 집합에서 제거 하는 최적화 된 방법을 제공 합니다.
- **DbChangeTracker.HasChanges** 보류 중인 변경 내용을 데이터베이스에 저장 될 수 있는지 확인 하는 쉽고 효율적인 방법을 제공 합니다.
- **SqlCeFunctions** 는 SQL Compact는 SqlFunctions 같음 제공 합니다.

다음 기능은 Code First에 적용 됩니다.

- **[사용자 지정 코드의 첫 번째 규칙](~/ef6/modeling/code-first/conventions/custom.md)**  허용 반복적인 구성을 방지 하려면 사용자 고유의 규칙을 작성 합니다. 더 복잡 한 규칙을 작성할 수 있도록 몇 가지 더 복잡 한 구성 요소 뿐만 아니라 간단한 규칙에 대 한 간단한 API를 제공 합니다.
- **[Insert/Update/Delete 저장 프로시저에 첫 번째 매핑 코드](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md)**  이제 지원 됩니다.
- **[Idempotent 마이그레이션 스크립트](~/ef6/modeling/code-first/migrations/index.md)**  최신 버전까지 모든 버전에서 데이터베이스를 업그레이드할 수 있는 SQL 스크립트를 생성할 수 있습니다.
- **[마이그레이션 기록 테이블을 구성할 수 있습니다](~/ef6/modeling/code-first/migrations/history-customization.md)**  마이그레이션 기록 테이블의 정의 사용자 지정할 수 있습니다. 적절 한 데이터 형식 등 제대로 작동 하려면 마이그레이션 기록 테이블에 지정 해야 하는 데이터베이스 공급자에 대 한 경우 특히 유용 합니다.
- **데이터베이스별 다중 컨텍스트** 마이그레이션을 사용 하는 경우 데이터베이스당 또는 Code First 자동으로 만들 때 데이터베이스를 한 Code First 모델의 이전 제한 사항을 제거 합니다.
- **[DbModelBuilder.HasDefaultSchema](~/ef6/modeling/code-first/fluent/types-and-properties.md)**  는 한 곳에서 구성할 Code First 모델에 대 한 기본 데이터베이스 스키마를 허용 하는 새로운 코드 첫 번째 API. Code First 기본 스키마를 하드 코드 된 이전 &quot;dbo&quot; 테이블이 속한 스키마를 구성 하는 유일한 방법은 ToTable API를 통해 였습니다.
- **DbModelBuilder.Configurations.AddFromAssembly 메서드** Code First Fluent API를 사용 하 여 구성 클래스를 사용 하는 경우 어셈블리에 정의 된 모든 구성 클래스를 쉽게 추가할 수 있습니다.
- **[사용자 지정 마이그레이션 작업](http://romiller.com/2013/02/27/ef6-writing-your-own-code-first-migration-operations/)**  코드 기반 마이그레이션을 사용할 추가 작업을 추가할 수 있었습니다.
- **기본 트랜잭션 격리 수준 READ_COMMITTED_SNAPSHOT으로 변경 됩니다** 확장성과 더 적은 교착 상태에 대 한 허용는 Code First를 사용 하 여 만든 데이터베이스에 대 한 합니다.
- **엔터티 및 복합 형식 이제 수 nestedinside 클래스**합니다. |

## <a name="ef-50"></a>EF 5.0
EF 5.0.0 런타임은 NuGet에 2012 년 8 월에에서 출시 되었습니다.
이 릴리스에 열거형 지원, 테이블 반환 함수, 공간 데이터 형식 및 다양 한 성능 향상을 비롯 한 몇 가지 새로운 기능이 도입 되었습니다.

Visual Studio 2012에서 Entity Framework 디자이너에는 또한 모델에 대해 여러 다이어그램에 대 한 지원이 도입 되었습니다 저장된 프로시저의 디자인 화면 및 일괄 처리 가져오기를 모양의 색을 지정 합니다.

에서는 EF 5 릴리스를 위해 특별히 마련 하는 콘텐츠의 목록을 다음과 같습니다.

-   [EF 5 릴리스 게시물](https://blogs.msdn.com/b/adonet/archive/2012/08/15/ef5-released.aspx)
-   EF5의 새로운 기능
    -   [먼저 코드에서 열거형 지원](~/ef6/modeling/code-first/data-types/enums.md)
    -   [EF 디자이너에서 열거형 지원](~/ef6/modeling/designer/data-types/enums.md)
    -   [먼저 코드에서 공간 데이터 형식](~/ef6/modeling/code-first/data-types/spatial.md)
    -   [EF 디자이너에서 공간 데이터 형식](~/ef6/modeling/designer/data-types/spatial.md)
    -   [공간 형식에 대 한 공급자 지원](~/ef6/fundamentals/providers/spatial-support.md)
    -   [테이블 반환 함수](~/ef6/modeling/designer/advanced/tvfs.md)
    -   [모델에 대해 여러 다이어그램](~/ef6/modeling/designer/multiple-diagrams.md)
-   모델을 구성 하는 설정
    -   [모델 만들기](~/ef6/modeling/index.md)
    -   [연결 및 모델](~/ef6/fundamentals/configuring/connection-strings.md)
    -   [성능 고려 사항](~/ef6/fundamentals/performance/perf-whitepaper.md)
    -   [Microsoft SQL Azure 사용 하 여 작업](~/ef6/fundamentals/connection-resiliency/retry-logic.md)
    -   [구성 파일 설정](~/ef6/fundamentals/configuring/config-file.md)
    -   [용어](~/ef6/resources/glossary.md)
    -   Code First
        -   [새 데이터베이스 (연습 및 비디오)에 대 한 code First](~/ef6/modeling/code-first/workflows/new-database.md)
        -   [Code First는 기존 데이터베이스 (연습 및 비디오)](~/ef6/modeling/code-first/workflows/existing-database.md)
        -   [규칙](~/ef6/modeling/code-first/conventions/built-in.md)
        -   [데이터 주석](~/ef6/modeling/code-first/data-annotations.md)
        -   [Fluent API-구성/매핑 속성 & 형식](~/ef6/modeling/code-first/fluent/types-and-properties.md)
        -   [Fluent API-관계 구성](~/ef6/modeling/code-first/fluent/relationships.md)
        -   [VB.NET 사용 하 여 Fluent API](~/ef6/modeling/code-first/fluent/vb.md)
        -   [Code First 마이그레이션](~/ef6/modeling/code-first/migrations/index.md)
        -   [자동 Code First 마이그레이션](~/ef6/modeling/code-first/migrations/automatic.md)
        -   [Migrate.exe](~/ef6/modeling/code-first/migrations/migrate-exe.md)
        -   [Dbset 정의](~/ef6/modeling/code-first/dbsets.md)
    -   EF 디자이너
        -   [모델 첫 번째 (연습 및 비디오)](~/ef6/modeling/designer/workflows/model-first.md)
        -   [Database First (연습 및 비디오)](~/ef6/modeling/designer/workflows/database-first.md)
        -   [복합 형식](~/ef6/modeling/designer/data-types/complex-types.md)
        -   [연결/관계](~/ef6/modeling/designer/relationships.md)
        -   [TPT 상속 패턴](~/ef6/modeling/designer/inheritance/tpt.md)
        -   [TPH 상속 패턴](~/ef6/modeling/designer/inheritance/tph.md)
        -   [저장된 프로시저를 사용 하 여 쿼리](~/ef6/modeling/designer/stored-procedures/query.md)
        -   [여러 결과 집합을 사용 하 여 저장된 프로시저](~/ef6/modeling/designer/advanced/multiple-result-sets.md)
        -   [삽입, 업데이트 및 저장된 프로시저를 사용 하 여 삭제](~/ef6/modeling/designer/stored-procedures/cud.md)
        -   [(엔터티 분할) 하는 여러 테이블에 엔터티 매핑](~/ef6/modeling/designer/entity-splitting.md)
        -   [(테이블 분할) 하는 한 테이블에 여러 엔터티 매핑](~/ef6/modeling/designer/table-splitting.md)
        -   [쿼리 정의](~/ef6/modeling/designer/advanced/defining-query.md)
        -   [코드 생성 템플릿](~/ef6/modeling/designer/codegen/index.md)
        -   [ObjectContext 되돌리기](~/ef6/modeling/designer/codegen/legacy-objectcontext.md)
-   모델을 사용 하 여
    -   [DbContext 작업](~/ef6/fundamentals/working-with-dbcontext.md)
    -   [엔터티 쿼리 찾기](~/ef6/querying/index.md)
    -   [관계 작업](~/ef6/fundamentals/relationships.md)
    -   [관련된 엔터티 로드](~/ef6/querying/related-data.md)
    -   [로컬 데이터 사용](~/ef6/querying/local-data.md)
    -   [N 계층 응용 프로그램](~/ef6/fundamentals/disconnected-entities/index.md)
    -   [원시 SQL 쿼리](~/ef6/querying/raw-sql.md)
    -   [낙관적 동시성 패턴](~/ef6/saving/concurrency.md)
    -   [프록시 사용](~/ef6/fundamentals/proxies.md)
    -   [자동 변경 내용 검색](~/ef6/saving/change-tracking/auto-detect-changes.md)
    -   [비 추적 쿼리](~/ef6/querying/no-tracking.md)
    -   [로드 메서드](~/ef6/querying/load-method.md)
    -   [추가 하 고 연결 및 엔터티 상태](~/ef6/saving/change-tracking/entity-state.md)
    -   [속성 값을 사용 하 여 작업](~/ef6/saving/change-tracking/property-values.md)
    -   [WPF (Windows Presentation Foundation)를 사용 하 여 바인딩 데이터](~/ef6/fundamentals/databinding/wpf.md)
    -   [데이터 바인딩 WinForms (Windows Forms)](~/ef6/fundamentals/databinding/winforms.md)

## <a name="ef-431"></a>EF 4.3.1
EF 4.3.1 런타임은 nuget 4.3.0 EF 직후 2012 년 2 월에에서 출시 되었습니다.
이 패치 릴리스에서 EF 4.3 릴리스에 몇 가지 버그 수정 포함 및 Visual Studio 2012를 사용한 EF 4.3을 사용 하는 고객에 대 한 더 나은 LocalDB 지원 기능이 도입 되었습니다.

다음은 하겠습니다 함께 EF 4.3.1 릴리스 위해 콘텐츠 목록, EF 4.1에 대 한 제공 되는 콘텐츠의 대부분 여전히에 적용 됩니다 EF 4.3.

-   [EF 4.3.1 릴리스 블로그 게시물](https://blogs.msdn.com/b/adonet/archive/2012/02/29/ef4-3-1-and-ef5-beta-1-available-on-nuget.aspx)

## <a name="ef-43"></a>EF 4.3
EF 4.3.0 런타임은 NuGet에 2012 년 2 월에에서 출시 되었습니다.
이 릴리스는 Code First 모델 발전 함에 따라 증분 방식으로 변경할 Code First에서 생성 된 데이터베이스를 허용 하는 Code First 마이그레이션 기능이 포함 되어 있습니다.

여기에서는 EF 4.3 릴리스에 대 한 구체적으로 결합 하는 콘텐츠의 목록을 EF 4.1에 대 한 제공 되는 콘텐츠의 대부분 여전히에 적용 됩니다 EF 4.3:
-   [EF 4.3 릴리스 게시물](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-released.aspx)
-   [EF 4.3 코드 기반 마이그레이션 연습](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-code-based-migrations-walkthrough.aspx)
-   [EF 4.3 자동 마이그레이션 연습](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-automatic-migrations-walkthrough.aspx)

## <a name="ef-42"></a>EF 4.2
EF 4.2.0 런타임 2011 년 11 월에에서 nuget 릴리스 되었습니다.
이번이 릴리스에 버그 수정 사항 4.1.1 EF 릴리스 합니다.
이 릴리스만 포함 하기 때문에 버그 수정이 했을 수 EF 4.1.2 패치 릴리스 되지만 4.2는 4.1.x에서 사용한 패치 버전 번호를 해제 하 고 채택 기반 날짜에서 이동할 수 있도록 이동 하기로 했습니다는 [의미 체계 Versionsing](https://semver.org) 의미 체계 버전 관리를 위한 표준입니다.

여기에서는 EF 4.2 릴리스를 위해 특별히 마련 하는 콘텐츠의 목록을 이면 EF 4.1 제공 된 콘텐츠가 계속 적용 됩니다 EF 4.2도 합니다.

-   [EF 4.2 릴리스 게시물](https://blogs.msdn.com/b/adonet/archive/2011/11/01/ef-4-2-released.aspx)
-   [Code First 연습](https://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-code-first-walkthrough.aspx)
-   [모델 및 데이터베이스 첫 번째 연습](https://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-model-amp-database-first-walkthrough.aspx)

## <a name="ef-411"></a>EF 4.1.1
EF 4.1.10715 런타임 2011 년 7 월에에서 nuget 릴리스 되었습니다.
버그 수정 외에도이 패치 릴리스에서 쉽게 디자인 타임 Code First 모델을 사용 하 여 작동 하도록 도구에 대 한 일부 구성 요소를 도입 합니다.
이러한 구성 요소는 Code First 마이그레이션 (EF 4.3에 포함) 및 EF Power Tools에서 사용 됩니다.

보면 이상한 버전 번호가 패키지의 4.1.10715는 합니다.
채택 하기로 전에 날짜 기반 패치 버전을 사용 하는 데 우리 [유의 적 버전](https://semver.org)합니다.
이 버전 EF 4.1 패치 1 (또는 EF 4.1.1)로 생각할 수 있습니다.

다음은 우리가 마련 된 4.1.1 콘텐츠의 목록 릴리스:

-   [4.1.1 EF 릴리스 게시물](https://blogs.msdn.com/b/adonet/archive/2011/07/25/ef-4-1-update-1-released.aspx)

## <a name="ef-41"></a>EF 4.1
EF 4.1.10331 런타임을 가져온 첫 번째 2011 년 4 월에에서 NuGet에 게시 합니다.
이 릴리스에서 간소화 된 DbContext API와 Code First 워크플로가 포함 되어 있습니다.

이상한 버전 번호를 확인할 수 있습니다 4.1.10331 4.1 된 실제로 해야 합니다. 또한 있기를 4.1.10311 4.1.0-rc 었어야 버전 ('release candidate'는 'rc').
채택 하기로 전에 날짜 기반 패치 버전을 사용 하는 데 우리 [유의 적 버전](https://semver.org)합니다.

콘텐츠에서는 4.1 릴리스에 대 한 결합의 목록을 다음과 같습니다. 대부분의 Entity Framework의 이후 릴리스를 여전히 적용 됩니다.

-   [EF 4.1 릴리스 게시물](https://blogs.msdn.com/b/adonet/archive/2011/04/11/ef-4-1-released.aspx)
-   [Code First 연습](https://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-code-first-walkthrough.aspx)
-   [모델 및 데이터베이스 첫 번째 연습](https://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-model-amp-database-first-walkthrough.aspx)
-   [SQL Azure 페더레이션 및 Entity Framework](https://blogs.msdn.com/b/adonet/archive/2012/01/10/sql-azure-federations-and-the-entity-framework.aspx)

## <a name="ef-40"></a>EF 4.0
이 릴리스에서 2010 년 4 월.NET Framework 4 및 Visual Studio 2010에 포함 되었습니다.
이 릴리스에서 중요 한 새로운 기능 POCO 지원, 외래 키 매핑, 지연 로드, 테스트 용이성 향상, 사용자 지정 가능한 코드 생성 및 Model First 워크플로 포함 합니다.

Entity Framework의 두 번째 릴리스 이지만 EF 4와 함께 제공 되는.NET Framework 버전에 맞게 이름이 것입니다.
이 릴리스 후 NuGet에서 Entity Framework를 사용할 수 있도록 시작 하 고.NET Framework 버전에 더 이상 연결 된 것 이므로 의미 체계 버전 관리를 도입 합니다.

에 포함 된 EF 비트에 대 한 중요 업데이트를 사용 하 여 제공 된 몇 가지 후속 버전의.NET Framework 참고 합니다.
사실, EF 5.0의 새로운 기능을 많이 이러한 비트에서 향상 된 기능으로 구현 되었습니다.
그러나 EF에 대 한 버전 관리 스토리를 합리화 하기 위해 계속의 일부인.NET Framework 4.0 EF runtime의 모든 최신 버전으로 구성 하는 동안 EF 비트를 참조 하는 [EntityFramework NuGet 패키지](https://www.nuget.org/packages/EntityFramework/)합니다.         

## <a name="ef-35"></a>EF 3.5
Entity Framework의 초기 버전은.NET 3.5 서비스 팩 1 및 2008 년 8 월에에서 릴리스된 Visual Studio 2008 SP1에 포함 되었습니다.
이 릴리스의 데이터베이스 첫 번째 워크플로 사용 하 여 기본 O/RM 지원을 제공 했습니다.
