---
title: Entity Framework의 이전 릴리스-EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 1060bb99-765f-4f32-aaeb-d6635d3dbd3e
uid: ef6/what-is-new/past-releases
ms.openlocfilehash: b7181334cd125c5cbf296d5b3674c0b5f087f438
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2020
ms.locfileid: "79402122"
---
# <a name="past-releases-of-entity-framework"></a>Entity Framework의 이전 릴리스

Entity Framework의 첫 번째 버전은 .NET Framework 3.5 SP1 및 Visual Studio 2008 s p 1의 일부로 2008에 릴리스 되었습니다.

EF 4.1 릴리스부터는 [Entityframework NuGet 패키지로](https://www.nuget.org/packages/EntityFramework/) 제공 됩니다. 현재 NuGet.org에서 가장 인기 있는 패키지 중 하나입니다.

버전 4.1과 5.0 사이의 EntityFramework NuGet 패키지는 .NET Framework의 일부로 제공 된 EF 라이브러리를 확장 했습니다.

버전 6부터 EF는 오픈 소스 프로젝트 이며 .NET Framework에서 대역 외로도 완전히 이동 했습니다.
즉, 응용 프로그램에 EntityFramework 버전 6 NuGet 패키지를 추가 하는 경우 .NET Framework의 일부로 제공 되는 EF 비트에 의존 하지 않는 EF 라이브러리의 전체 복사본을 가져옵니다.
이렇게 하면 개발 속도를 높이고 새로운 기능을 제공 하는 데 도움이 됩니다.

6 월 2016에 EF Core 1.0이 출시 되었습니다. EF Core는 새 코드 베이스를 기반으로 하며 보다 간단 하 고 확장 가능한 EF 버전으로 설계 되었습니다.
현재 EF Core는 Microsoft의 Entity Framework 팀을 위한 개발의 주요 장점입니다.
이는 EF6에 대해 계획 된 새로운 주요 기능이 없다는 것을 의미 합니다. 그러나 EF6는 여전히 오픈 소스 프로젝트와 지원 되는 Microsoft 제품으로 유지 관리 됩니다.

다음은 각 릴리스에 도입 된 새로운 기능에 대 한 정보를 포함 하 여 역순으로 이전 릴리스의 목록입니다.

## <a name="ef-tools-update-in-visual-studio-2017-157"></a>Visual Studio 2017 15.7의 EF 도구 업데이트
2018년 5월에 Visual Studio 2017 15.7의 일부로 업데이트된 버전의 EF 도구가 릴리스되었습니다.
공통적인 몇 가지 문제점이 이 버전에서 개선되었습니다.

- 여러 사용자 인터페이스 접근성 버그 수정
- 기존 데이터베이스에서 모델을 생성하는 경우 SQL Server 성능 회귀에 대한 해결 [#4](https://github.com/aspnet/entityframework6/issues/4)
- SQL Server의 대규모 모델에 모델을 업데이트하는 지원 [#185](https://github.com/aspnet/EntityFramework6/issues/185)

또한 새 버전의 EF 도구는 새 프로젝트에서 모델을 만들 때 EF 6.2 런타임을 설치하도록 개선되었습니다. 이전 버전의 Visual Studio를 사용하는 경우 해당하는 NuGet 패키지 버전을 설치하여 EF 6.2 런타임(그리고 이전 버전의 EF)을 사용할 수 있습니다.

## <a name="ef-620"></a>EF 6.2.0
EF 6.2 런타임은 2017년 10월에 NuGet에 출시되었습니다.
오픈 소스 기여자 커뮤니티의 적극적인 참여 덕분에 EF 6.2는 여러 [버그 픽스](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) 및 [향상된 제품 기능](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20)을 포함하고 있습니다.

다음은 EF 6.2 런타임에 영향을 주는 가장 중요한 변경 내용을 간략하게 정리한 목록입니다.

- 영구 캐시에서 완료된 Code First 모델을 로드하여 시작 시간 단축 [#275](https://github.com/aspnet/EntityFramework6/issues/275)
- 인덱스를 정의할 수 있는 흐름 API [#274](https://github.com/aspnet/EntityFramework6/issues/274)
- SQL에서 LIKE로 해석되는 LINQ 쿼리를 쓸 수 있게 해주는 DbFunctions.Like() [#241](https://github.com/aspnet/EntityFramework6/issues/241)
- 이제 Migrate.exe가 -script 옵션 지원 [#240](https://github.com/aspnet/EntityFramework6/issues/240)
- 이제 EF6는 SQL Server에서 시퀀스를 통해 생성된 키 값 사용 가능 [#165](https://github.com/aspnet/EntityFramework6/issues/165)
- SQL Azure 실행 전략에 대한 일시적인 오류 목록 업데이트 [83](https://github.com/aspnet/EntityFramework6/issues/83)
- 버그: "SqlParameter가 이미 다른 SqlParameterCollection에 포함되어 있음" 메시지와 함께 쿼리 재시도 또는 SQL 명령이 실패 [#81](https://github.com/aspnet/EntityFramework6/issues/81)
- 버그: 디버거에서 DbQuery.ToString() 평가가 자주 시간 초과 [#73](https://github.com/aspnet/EntityFramework6/issues/73)

## <a name="ef-613"></a>EF 6.1.3
EF 6.1.3 런타임은 2015 년 10 월에 NuGet에 릴리스 되었습니다.
이 릴리스에는 6.1.2 릴리스에 보고 된 높은 우선 순위 오류 및 재발에 대 한 수정 사항만 포함 됩니다.
수정 사항은 다음과 같습니다.

- 쿼리: EF 6.1.2의 회귀: 1:1 관계 및 "let" 절에 대해 외부 적용이 도입 되 고 더 복잡 한 쿼리
- 상속 된 클래스에서 기본 클래스 속성을 숨기면 TPT 문제가 발생 합니다.
- DbMigration. ' go ' 단어가 텍스트에 포함 되어 있으면 Sql이 실패 합니다.
- Unionall 변환이 및 Intersect 평면화 지원에 대 한 호환성 플래그 만들기
- 여러 포함 된 쿼리가 6.1.2에서 작동 하지 않습니다 (6.1.1에서 작업).
- EF 6.1.1에서 6.1.2로 업그레이드 한 후 "SQL 구문에 오류가 있습니다." 예외

## <a name="ef-612"></a>EF 6.1.2
EF 6.1.2 런타임은 2014 년 12 월에 NuGet에 릴리스 되었습니다.
이 버전은 주로 버그 수정과 관련 됩니다. 커뮤니티의 구성원에 대 한 몇 가지 주목할 만한 변경 사항도 수락 했습니다.
- **쿼리 캐시 매개 변수는 앱/웹 구성 파일에서 구성할 수 있습니다.**
    ``` xml
    <entityFramework>
      <queryCache size='1000' cleaningIntervalInSeconds='-1'/>
    </entityFramework>
    ```
- **DbMigration의 Sqlfile 및 sqlfile 메서드** 를 사용 하 여 파일 또는 포함 리소스로 저장 된 SQL 스크립트를 실행할 수 있습니다.

## <a name="ef-611"></a>EF 6.1.1
EF 6.1.1 runtime은 2014 년 6 월에 NuGet에 릴리스 되었습니다.
이 버전에는 여러 사용자가 직면 한 문제에 대 한 수정 사항이 포함 되어 있습니다. 다른 항목:
- 디자이너: EF6 디자이너에서 소수 자릿수로 EF5 edmx을 여는 동안 오류가 발생 했습니다.
- LocalDB에 대 한 기본 인스턴스 검색 논리는 SQL Server 2014에서 작동 하지 않습니다.

## <a name="ef-610"></a>EF 6.1.0
EF 6.1.0 런타임은 2014 년 3 월에 NuGet에 릴리스 되었습니다.
이 부 업데이트는 다음과 같은 많은 새 기능을 포함 합니다.

- **도구 통합** 은 새 EF 모델을 만드는 일관 된 방법을 제공 합니다. 이 기능은 [ADO.NET 엔터티 데이터 모델 마법사를 확장 하](~/ef6/modeling/code-first/workflows/existing-database.md)여 기존 데이터베이스에서 리버스 엔지니어링을 비롯 한 Code First 모델 만들기를 지원 합니다. 이러한 기능은 이전에 EF Power Tools의 베타 품질로 제공 되었습니다.
- **[트랜잭션 커밋 실패를 처리](~/ef6/fundamentals/connection-resiliency/commit-failures.md)** 하면 새로 도입 된 기능을 사용 하 여 트랜잭션 작업을 가로채는 CommitFailureHandler 제공 됩니다. CommitFailureHandler는 트랜잭션을 커밋하는 동안 연결 오류를 자동으로 복구할 수 있도록 합니다.
- **[Indexattribute](~/ef6/modeling/code-first/data-annotations.md)** 를 사용 하면 Code First 모델의 속성에 `[Index]` 특성을 배치 하 여 인덱스를 지정할 수 있습니다. 그러면 Code First는 데이터베이스에 해당 인덱스를 만듭니다.
- **공용 매핑 API는** 속성 및 형식이 데이터베이스의 열과 테이블에 매핑되는 방법에 대 한 정보에 대 한 액세스를 제공 합니다. 이전 릴리스에서이 API는 내부용입니다.
- **[앱/web.config 파일을 통해 인터셉터를 구성](~/ef6/fundamentals/configuring/config-file.md)** 하면 응용 프로그램을 다시 컴파일하지 않고도 인터셉터를 추가할 수 있습니다.
- **System.object**는 모든 데이터베이스 작업을 파일에 쉽게 기록할 수 있게 해 주는 새로운 인터셉터입니다. 이전 기능과 함께이 기능을 사용 하면 다시 컴파일하지 않아도 [배포 된 응용 프로그램에 대 한 데이터베이스 작업의 로깅을 쉽게 전환할](~/ef6/fundamentals/configuring/config-file.md)수 있습니다.
- **마이그레이션 모델 변경 검색** 은 스 캐 폴드 마이그레이션의 정확성을 높이기 위해 개선 되었습니다. 변경 내용 검색 프로세스의 성능도 향상 되었습니다.
- 초기화 중 데이터베이스 작업 감소, LINQ 쿼리의 null 같음 비교에 대 한 최적화, 더 많은 시나리오에서 더 빠른 보기 생성 (모델 생성), 여러 연결이 있는 추적 된 엔터티의 보다 효율적인 구체화 등의 **성능 향상**

## <a name="ef-602"></a>EF 6.0.2
EF 6.0.2 런타임은 2013 년 12 월에 NuGet에 릴리스 되었습니다.
이 패치 릴리스는 EF6 릴리스에 도입 된 문제를 해결 하는 것으로 제한 됩니다 (EF5 이후 성능/동작의 재발).

## <a name="ef-601"></a>EF 6.0.1
EF 6.0.1 런타임은 2013 년 10 월부터 EF 6.0.0와 동시에 NuGet에 릴리스 되었습니다. 후자는 몇 개월 전에 잠긴 Visual Studio 버전에 포함 되어 있기 때문입니다.
이 패치 릴리스는 EF6 릴리스에 도입 된 문제를 해결 하는 것으로 제한 됩니다 (EF5 이후 성능/동작의 재발).
가장 주목할 만한 변경 사항은 EF 모델에 대해 준비 하는 동안 몇 가지 성능 문제를 해결 하는 것입니다.
이는 준비 성능이 EF6에 집중 된 영역 이지만 이러한 문제로 인해 EF6에서 수행 되는 다른 성능상의 이점이 약간의 것 이기 때문에 중요 했습니다.

## <a name="ef-60"></a>EF 6.0
EF 6.0.0 런타임은 2013 년 10 월에 NuGet에 릴리스 되었습니다.
이 버전은 전체 EF 런타임이 .NET Framework의 일부인 EF 비트에 의존 하지 않는 [Entityframework NuGet 패키지](https://www.nuget.org/packages/EntityFramework/) 에 포함 되는 첫 번째 버전입니다.
런타임의 나머지 부분을 NuGet 패키지로 이동 하려면 기존 코드에 대 한 몇 가지 주요 변경 사항이 필요 합니다.
업그레이드 하는 데 필요한 수동 단계에 대 한 자세한 내용은 [Entity Framework 6으로 업그레이드](upgrading-to-ef6.md) 섹션을 참조 하세요.

이 릴리스에는 다양 한 새로운 기능이 포함 되어 있습니다.
다음 기능은 Code First 또는 EF Designer를 사용 하 여 만든 모델에 대해 작동 합니다.

- **[비동기 쿼리 및 저장](~/ef6/fundamentals/async.md)** 은 .net 4.5에 도입 된 작업 기반 비동기 패턴에 대 한 지원을 추가 합니다.
- **[연결 복원 력을](~/ef6/fundamentals/connection-resiliency/retry-logic.md)** 통해 일시적인 연결 오류를 자동으로 복구할 수 있습니다.
- **[코드 기반 구성은](~/ef6/fundamentals/configuring/code-based.md)** 일반적으로 구성 파일에서 수행 된 구성 (코드에서)을 수행 하는 옵션을 제공 합니다.
- **[종속성 확인](~/ef6/fundamentals/configuring/dependency-resolution.md)** 에는 서비스 로케이터 패턴에 대 한 지원이 도입 되었으며 사용자 지정 구현으로 바꿀 수 있는 몇 가지 기능이 포함 되어 있습니다.
- **[가로채기/sql 로깅은](~/ef6/fundamentals/logging-and-interception.md)** 위에 구축 된 간단한 SQL 로깅을 사용 하 여 EF 작업 가로채기를 위한 하위 수준의 빌딩 블록을 제공 합니다.
- 테스트 **용이성 기능** 을 [사용 하면 모의 프레임 워크를 사용](~/ef6/fundamentals/testing/mocking.md) 하거나 [사용자 고유의 테스트를 작성](~/ef6/fundamentals/testing/writing-test-doubles.md)하는 경우 DbContext 및 dbset에 대 한 테스트 double을 보다 쉽게 만들 수 있습니다.
- **[이제 이미 열려 있는 DbConnection을 사용 하 여 DbContext를 만들 수 있습니다](~/ef6/fundamentals/connection-management.md)** .이 경우에는 컨텍스트를 만들 때 연결을 확인할 수 없는 경우 (예: 연결 상태를 보장할 수 없는 구성 요소 간 연결 공유) 연결을 열 수 있는 경우에 유용 합니다.
- **[향상 된 트랜잭션 지원은](~/ef6/saving/transactions.md)** 프레임 워크 외부의 트랜잭션을 지원 하 고 프레임 워크 내에서 트랜잭션을 만드는 향상 된 방법을 제공 합니다.
- **.Net 4.0의 열거형, 공간 및 향상 된 성능** -.NET Framework에 사용 되는 핵심 구성 요소를 EF NuGet 패키지로 이동 하 여 이제 열거형 지원, 공간 데이터 형식 및 .net 4.0에서 EF5의 성능 향상을 제공할 수 있습니다.
- **LINQ 쿼리에서 열거 가능. Contains의 성능 향상**.
- 특히 규모가 많은 모델의 경우 **준비 시간 (뷰 생성)이 향상**되었습니다.
- **플러그형 복수화 &amp; 단수형 Service**.
- 이제 엔터티 클래스에서 **Equals 또는 GetHashCode의 사용자 지정 구현이** 지원 됩니다.
- **Dbset/RemoveRange** 는 집합에서 여러 엔터티를 추가 하거나 제거 하는 최적화 된 방법을 제공 합니다.
- **DbChangeTracker** 는 데이터베이스에 저장 되는 보류 중인 변경 내용이 있는지 확인 하는 쉽고 효율적인 방법을 제공 합니다.
- **Sqlcefunctions** 는 sql함수에 해당 하는 SQL Compact를 제공 합니다.

다음 기능은 Code First에만 적용 됩니다.

- **[사용자 지정 Code First 규칙](~/ef6/modeling/code-first/conventions/custom.md)** 을 사용 하면 반복적인 구성을 방지 하기 위해 고유한 규칙을 작성할 수 있습니다. 간단한 규칙 뿐만 아니라 보다 복잡 한 규칙을 작성 하는 데 사용할 수 있는 좀 더 복잡 한 구성 요소에 대 한 간단한 API를 제공 합니다.
- 이제 **[삽입/업데이트/삭제 저장 프로시저에 대 한 Code First 매핑이](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md)** 지원 됩니다.
- **[Idempotent 마이그레이션 스크립트](~/ef6/modeling/code-first/migrations/index.md)** 를 사용 하 여 모든 버전의 데이터베이스를 최신 버전으로 업그레이드할 수 있는 SQL 스크립트를 생성할 수 있습니다.
- **[구성 가능한 마이그레이션 기록 테이블](~/ef6/modeling/code-first/migrations/history-customization.md)** 을 사용 하 여 마이그레이션 기록 테이블의 정의를 사용자 지정할 수 있습니다. 이는 마이그레이션 기록 테이블이 제대로 작동 하도록 지정 하기 위해 적절 한 데이터 형식이 필요한 데이터베이스 공급자에 게 특히 유용 합니다.
- **데이터베이스당 여러 컨텍스트** 는 마이그레이션을 사용할 때 또는 Code First 자동으로 데이터베이스를 만들 때 데이터베이스 당 Code First 모델 하나에 대 한 이전 제한을 제거 합니다.
- **[Dbmodelbuilder. HasDefaultSchema](~/ef6/modeling/code-first/fluent/types-and-properties.md)** 는 Code First 모델에 대 한 기본 데이터베이스 스키마를 한 곳에서 구성할 수 있도록 하는 새로운 Code First API입니다. 이전에는 Code First 기본 스키마가 dbo&quot; &quot;하드 코딩 되었으며 ToTable API를 통해 테이블이 속한 스키마를 구성 하는 유일한 방법이 있습니다.
- **AddFromAssembly 메서드** 를 사용 하면 CODE FIRST 흐름 API와 함께 구성 클래스를 사용 하는 경우 어셈블리에 정의 된 모든 구성 클래스를 쉽게 추가할 수 있습니다.
- **[사용자 지정 마이그레이션 작업](https://romiller.com/2013/02/27/ef6-writing-your-own-code-first-migration-operations/)** 을 통해 코드 기반 마이그레이션에 사용할 추가 작업을 추가할 수 있습니다.
- **기본 트랜잭션 격리 수준은** Code First를 사용 하 여 만든 데이터베이스에 대 한 READ_COMMITTED_SNAPSHOT로 변경 되므로 확장성이 향상 되 고 교착 상태가 줄어듭니다.
- **이제 엔터티 및 복합 형식이 nestedinside 클래스 일 수 있습니다**.

## <a name="ef-50"></a>EF 5.0
EF 5.0.0 런타임은 2012 년 8 월에 NuGet에 릴리스 되었습니다.
이 릴리스에는 열거형 지원, 테이블 반환 함수, 공간 데이터 형식 및 다양 한 성능 향상을 비롯 한 몇 가지 새로운 기능이 도입 되었습니다.

Visual Studio 2012의 Entity Framework Designer에는 모델 당 다중 다이어그램, 디자인 화면에서 도형의 색 지정, 저장 프로시저의 일괄 가져오기에 대 한 지원도 도입 되었습니다.

EF 5 릴리스에 대해 구체적으로 함께 제공 되는 콘텐츠 목록은 다음과 같습니다.

-   [EF 5 릴리스 게시물](https://blogs.msdn.com/b/adonet/archive/2012/08/15/ef5-released.aspx)
-   EF5의 새로운 기능
    -   [Code First에서 열거형 지원](~/ef6/modeling/code-first/data-types/enums.md)
    -   [EF 디자이너의 열거형 지원](~/ef6/modeling/designer/data-types/enums.md)
    -   [Code First의 공간 데이터 형식](~/ef6/modeling/code-first/data-types/spatial.md)
    -   [EF 디자이너의 공간 데이터 형식](~/ef6/modeling/designer/data-types/spatial.md)
    -   [공간 형식에 대 한 공급자 지원](~/ef6/fundamentals/providers/spatial-support.md)
    -   [테이블 반환 함수](~/ef6/modeling/designer/advanced/tvfs.md)
    -   [모델 당 여러 다이어그램](~/ef6/modeling/designer/multiple-diagrams.md)
-   모델 설정
    -   [모델 만들기](~/ef6/modeling/index.md)
    -   [연결 및 모델](~/ef6/fundamentals/configuring/connection-strings.md)
    -   [성능 고려 사항](~/ef6/fundamentals/performance/perf-whitepaper.md)
    -   [Microsoft SQL Azure 사용](~/ef6/fundamentals/connection-resiliency/retry-logic.md)
    -   [구성 파일 설정](~/ef6/fundamentals/configuring/config-file.md)
    -   [용어 설명](~/ef6/resources/glossary.md)
    -   Code First
        -   [새 데이터베이스에 Code First (연습 및 비디오)](~/ef6/modeling/code-first/workflows/new-database.md)
        -   [기존 데이터베이스에 Code First (연습 및 비디오)](~/ef6/modeling/code-first/workflows/existing-database.md)
        -   [규칙](~/ef6/modeling/code-first/conventions/built-in.md)
        -   [데이터 주석](~/ef6/modeling/code-first/data-annotations.md)
        -   [흐름 API-속성 & 형식 구성/매핑](~/ef6/modeling/code-first/fluent/types-and-properties.md)
        -   [흐름 API-관계 구성](~/ef6/modeling/code-first/fluent/relationships.md)
        -   [VB.NET를 사용 하는 흐름 API](~/ef6/modeling/code-first/fluent/vb.md)
        -   [Code First 마이그레이션](~/ef6/modeling/code-first/migrations/index.md)
        -   [자동 Code First 마이그레이션](~/ef6/modeling/code-first/migrations/automatic.md)
        -   [마이그레이션 .exe](~/ef6/modeling/code-first/migrations/migrate-exe.md)
        -   [DbSets 정의](~/ef6/modeling/code-first/dbsets.md)
    -   EF 디자이너
        -   [Model First (연습 및 비디오)](~/ef6/modeling/designer/workflows/model-first.md)
        -   [Database First (연습 및 비디오)](~/ef6/modeling/designer/workflows/database-first.md)
        -   [복합 형식](~/ef6/modeling/designer/data-types/complex-types.md)
        -   [연결/관계](~/ef6/modeling/designer/relationships.md)
        -   [TPT 상속 패턴](~/ef6/modeling/designer/inheritance/tpt.md)
        -   [TPH 상속 패턴](~/ef6/modeling/designer/inheritance/tph.md)
        -   [저장 프로시저를 사용 하 여 쿼리](~/ef6/modeling/designer/stored-procedures/query.md)
        -   [여러 결과 집합을 포함 하는 저장 프로시저](~/ef6/modeling/designer/advanced/multiple-result-sets.md)
        -   [저장 프로시저를 사용 하 여 Insert, Update & Delete](~/ef6/modeling/designer/stored-procedures/cud.md)
        -   [여러 테이블에 엔터티 매핑 (엔터티 분할)](~/ef6/modeling/designer/entity-splitting.md)
        -   [단일 테이블에 여러 엔터티 매핑 (테이블 분할)](~/ef6/modeling/designer/table-splitting.md)
        -   [쿼리 정의](~/ef6/modeling/designer/advanced/defining-query.md)
        -   [코드 생성 템플릿](~/ef6/modeling/designer/codegen/index.md)
        -   [ObjectContext로 되돌리기](~/ef6/modeling/designer/codegen/legacy-objectcontext.md)
-   모델 사용
    -   [DbContext 작업](~/ef6/fundamentals/working-with-dbcontext.md)
    -   [엔터티 쿼리/찾기](~/ef6/querying/index.md)
    -   [관계 작업](~/ef6/fundamentals/relationships.md)
    -   [관련 엔터티 로드](~/ef6/querying/related-data.md)
    -   [로컬 데이터 작업](~/ef6/querying/local-data.md)
    -   [N 계층 응용 프로그램](~/ef6/fundamentals/disconnected-entities/index.md)
    -   [원시 SQL 쿼리](~/ef6/querying/raw-sql.md)
    -   [낙관적 동시성 패턴](~/ef6/saving/concurrency.md)
    -   [프록시 사용](~/ef6/fundamentals/proxies.md)
    -   [자동으로 변경 내용 검색](~/ef6/saving/change-tracking/auto-detect-changes.md)
    -   [추적 안 함 쿼리](~/ef6/querying/no-tracking.md)
    -   [로드 메서드](~/ef6/querying/load-method.md)
    -   [추가/연결 및 엔터티 상태](~/ef6/saving/change-tracking/entity-state.md)
    -   [속성 값 사용](~/ef6/saving/change-tracking/property-values.md)
    -   [WPF를 사용 하 여 데이터 바인딩 (Windows Presentation Foundation)](~/ef6/fundamentals/databinding/wpf.md)
    -   [WinForms를 사용 하 여 데이터 바인딩 (Windows Forms)](~/ef6/fundamentals/databinding/winforms.md)

## <a name="ef-431"></a>EF 4.3.1
Ef 4.3.1 런타임은 EF 4.3.0 직후 2012 2 월에 NuGet에 릴리스 되었습니다.
이 패치 릴리스에는 EF 4.3 릴리스에 대 한 일부 버그 수정이 포함 되어 있으며, Visual Studio 2012에서 EF 4.3을 사용 하는 고객에 게 더 나은 LocalDB 지원이 도입 되었습니다.

Ef 4.3.1 릴리스에 대해 특별히 함께 제공 되는 콘텐츠 목록은 ef 4.1에 제공 되는 대부분의 콘텐츠가 EF 4.3에도 여전히 적용 됩니다.

-   [EF 4.3.1 릴리스 블로그 게시물](https://blogs.msdn.com/b/adonet/archive/2012/02/29/ef4-3-1-and-ef5-beta-1-available-on-nuget.aspx)

## <a name="ef-43"></a>EF 4.3
EF 4.3.0 런타임은 2012 년 2 월에 NuGet에 릴리스 되었습니다.
이 릴리스에는 Code First 모델이 진화 함에 따라 Code First 생성 된 데이터베이스를 증분 변경할 수 있도록 하는 새로운 Code First 마이그레이션 기능이 포함 되어 있습니다.

Ef 4.3 릴리스를 위해 특별히 함께 제공 되는 콘텐츠 목록은 ef 4.1에 제공 되는 대부분의 콘텐츠가 EF 4.3에도 계속 적용 됩니다.
-   [EF 4.3 릴리스 게시물](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-released.aspx)
-   [EF 4.3 코드 기반 마이그레이션 연습](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-code-based-migrations-walkthrough.aspx)
-   [EF 4.3 자동 마이그레이션 연습](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-automatic-migrations-walkthrough.aspx)

## <a name="ef-42"></a>EF 4.2
EF 4.2.0 런타임은 2011 년 11 월에 NuGet에 릴리스 되었습니다.
이 릴리스에는 EF 4.1.1 릴리스에 대 한 버그 수정이 포함 되어 있습니다.
이 릴리스에는 버그 수정만 포함 되어 있지만 EF 4.1.2 patch 릴리스 었 기는 하지만 4.2로 이동 하 여이를로 이동 하 여 [의미 체계 버전](https://semver.org) 관리를 위해 사용 했던 날짜 기반 패치 버전 번호에서 벗어나 이동할 수 있도록 했습니다.

Ef 4.2 릴리스를 위해 특별히 함께 제공 되는 콘텐츠 목록은 ef 4.1에 제공 된 콘텐츠가 여전히 EF 4.2에도 적용 됩니다.

-   [EF 4.2 릴리스 게시물](https://blogs.msdn.com/b/adonet/archive/2011/11/01/ef-4-2-released.aspx)
-   [Code First 연습](https://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-code-first-walkthrough.aspx)
-   [모델 & Database First 연습](https://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-model-amp-database-first-walkthrough.aspx)

## <a name="ef-411"></a>EF 4.1.1
EF 4.1.10715 런타임은 2011 년 7 월에 NuGet에 릴리스 되었습니다.
이 패치 릴리스에는 버그 수정 외에도 디자인 타임 도구를 사용 하 여 Code First 모델을 보다 쉽게 사용할 수 있도록 하는 몇 가지 구성 요소가 도입 되었습니다.
이러한 구성 요소는 Code First 마이그레이션 (EF 4.3에 포함) 및 EF Power Tools에서 사용 됩니다.

패키지의 이상한 버전 번호가 4.1.10715 것을 알 수 있습니다.
[의미 체계 버전 관리](https://semver.org)를 도입 하기로 결정 하기 전에 날짜 기반 패치 버전을 사용 합니다.
이 버전은 EF 4.1 patch 1 (또는 EF 4.1.1)로 간주 합니다.

4\.1.1 릴리스에 대해 함께 제공 되는 콘텐츠 목록은 다음과 같습니다.

-   [EF 4.1.1 릴리스 게시물](https://blogs.msdn.com/b/adonet/archive/2011/07/25/ef-4-1-update-1-released.aspx)

## <a name="ef-41"></a>EF 4.1
EF 4.1.10331 런타임은 2011 년 4 월에 NuGet에 게시 되는 첫 번째입니다.
이 릴리스에는 단순화 된 DbContext API 및 Code First 워크플로가 포함 되어 있습니다.

이상 4.1이 아닌 이상한 버전 번호 4.1.10331이 표시 됩니다. 또한 4.1.0 (' rc '는 ' 릴리스 후보 '를 나타냄) 4.1.10311 버전이 있습니다.
[의미 체계 버전 관리](https://semver.org)를 도입 하기로 결정 하기 전에 날짜 기반 패치 버전을 사용 합니다.

4\.1 릴리스에 대해 함께 제공 되는 콘텐츠의 목록은 다음과 같습니다. 그 중 상당수는 Entity Framework의 이후 릴리스에 여전히 적용 됩니다.

-   [EF 4.1 릴리스 게시물](https://blogs.msdn.com/b/adonet/archive/2011/04/11/ef-4-1-released.aspx)
-   [Code First 연습](https://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-code-first-walkthrough.aspx)
-   [모델 & Database First 연습](https://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-model-amp-database-first-walkthrough.aspx)
-   [SQL Azure 페더레이션 및 Entity Framework](https://blogs.msdn.com/b/adonet/archive/2012/01/10/sql-azure-federations-and-the-entity-framework.aspx)

## <a name="ef-40"></a>EF 4.0
이 릴리스는 2010 년 4 월에 .NET Framework 4 및 Visual Studio 2010에 포함 되었습니다.
이 릴리스의 중요 한 새로운 기능에는 POCO 지원, 외래 키 매핑, 지연 로드, 테스트 용이성 향상, 사용자 지정 가능한 코드 생성 및 Model First 워크플로가 포함 되어 있습니다.

Entity Framework의 두 번째 릴리스 었 더라도 제공 된 .NET Framework 버전에 맞게 EF 4로 이름이 지정 되었습니다.
이 릴리스 후에는 더 이상 .NET Framework 버전에 연결 되지 않았기 때문에 NuGet 및 채택 된 의미 체계 버전 관리에 Entity Framework 사용 하기 시작 했습니다.

일부 후속 버전의 .NET Framework에는 포함 된 EF 비트에 대 한 중요 업데이트가 제공 됩니다.
실제로 EF 5.0의 새로운 기능 중 상당수는 이러한 비트에 대 한 향상 된 기능으로 구현 되었습니다.
그러나 EF에 대 한 버전 관리 스토리를 합리화 하기 위해, 모든 최신 버전은 [Entityframework NuGet 패키지로](https://www.nuget.org/packages/EntityFramework/)구성 되는 반면 .NET Framework의 일부인 ef 비트를 ef 4.0 런타임으로 계속 참조 합니다.

## <a name="ef-35"></a>EF 3.5
Entity Framework의 초기 버전은 2008 년 8 월에 릴리스된 .NET 3.5 서비스 팩 1 및 Visual Studio 2008 s p 1에 포함 되었습니다.
이 릴리스에서는 Database First 워크플로를 사용 하 여 기본 O/RM 지원을 제공 했습니다.
