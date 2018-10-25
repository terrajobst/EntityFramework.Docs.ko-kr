---
title: Entity Framework 6와 Entity Framework Core 비교
description: Entity Framework 6와 Entity Framework Core 중에 선택하는 방법을 제공합니다.
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 8568f0a3c6c4585c4fe05508fd610614107c8f66
ms.sourcegitcommit: 8b42045cd21f80f425a92f5e4e9dd4972a31720b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/14/2018
ms.locfileid: "49315648"
---
# <a name="compare-ef-core--ef6"></a>EF Core & EF6 비교

Entity Framework는 .NET용의 개체-관계형 매퍼(O/RM)입니다. 이 문서에서는 Entity Framework 6와 Entity Framework Core, 이 두 버전을 비교합니다.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6(EF6)은 테스트와 시험을 거친 데이터 액세스 기술입니다. 2008년에 .NET Framework 3.5 SP1 및 Visual Studio 2008 SP1의 일부로 처음 릴리스되었습니다. 4.1 릴리스부터는 [EntityFramework](https://www.nuget.org/packages/EntityFramework/) NuGet 패키지로 탑재되었습니다. EF6는 .NET Framework 4.x에서 실행되며, 이는 Windows에서만 실행됨을 의미합니다. 

EF6는 계속 지원되는 제품이며 버그 수정 사항과 작은 개선이 이어질 것입니다.

## <a name="entity-framework-core"></a>Entity Framework Core

EF Core(Entity Framework Core)는 2016년 처음 릴리스된 EF6를 완전히 다시 작성한 버전입니다. Nuget 패키지로 제공되며 [Microsoft.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/)가 기본입니다. EF Core는 .NET Core 또는 .NET Framework에서 실행할 수 있는 크로스 플랫폼 제품입니다.

EF Core는 EF6와 유사한 개발자 환경을 제공하도록 디자인되었습니다. 대부분의 상위 API가 그대로 유지되므로 EF6를 사용한 개발자에게는 EF Core가 친숙할 것입니다.

## <a name="feature-comparison"></a>기능 비교

EF Core에서는 EF6에서 구현되지 않을 몇 가지 중요한 기능(예: LINQ 쿼리에서의 [대체 키](xref:core/modeling/alternate-keys), [일괄 업데이트](xref:core/what-is-new/ef-core-1.0#relational-batching-of-statements), [혼합 클라이언트/데이터베이스 평가](xref:core/querying/client-eval))을 제공합니다. 그러나 EF Core는 새로운 코드 기반이므로 EF6에 있는 일부 기능이 EF Core에 없습니다.

다음 표에서는 EF Core 및 EF6에서 사용할 수 있는 기능을 비교합니다. 이는 상위 수준준 비교로서, 모든 기능을 나열하거나 다른 EF 버전에 있는 동일한 기능 간의 차이를 설명하지 않습니다.

EF Core 열은 기능이 처음 나타나는 제품 버전을 나타냅니다.

### <a name="creating-a-model"></a>모델 만들기

| **기능**                                           | **EF 6** | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| 기본 클래스 매핑                                   | 예      | 1.0                                   |
| 매개 변수가 포함된 생성자                          |          | 2.1                                   |
| 속성 값 변환                            |          | 2.1                                   |
| 키 없이 매핑된 형식(쿼리 형식)               |          | 2.1                                   |
| 규칙                                           | 예      | 1.0                                   |
| 사용자 지정 규칙                                    | 예      | 1.0(부분)                         |
| 데이터 주석                                      | 예      | 1.0                                   |
| Fluent API                                            | 예      | 1.0                                   |
| 상속: TPH(계층 구조별 테이블)                | 예      | 1.0                                   |
| 상속: TPT(형식별 테이블)                     | 예      |                                       |
| 상속: TPC(구체적인 클래스별 테이블)           | 예      |                                       |
| 섀도 상태 속성                               |          | 1.0                                   |
| 대체 키                                        |          | 1.0                                   |
| 조인 엔터티가 없는 다 대 다                      | 예      |                                       |
| 키 생성: 데이터베이스                              | 예      | 1.0                                   |
| 키 생성: 클라이언트                                |          | 1.0                                   |
| 복합/소유된 형식                                   | 예      | 2.0                                   |
| 공간 데이터                                          | 예      |                                       |
| 모델의 그래픽 시각화                      | 예      |                                       |
| 그래픽 모델 편집기                                | 예      |                                       |
| 모델 형식: 코드                                    | 예      | 1.0                                   |
| 모델 형식: EDMX(XML)                              | 예      |                                       |
| 데이터베이스에서 모델 만들기: 명령줄              | 예      | 1.0                                   |
| 데이터베이스에서 모델 만들기: VS 마법사                 | 예      |                                       |
| 데이터베이스에서 모델 업데이트                            | Partial  |                                       |
| 전역 쿼리 필터                                  |          | 2.0                                   |
| 테이블 분할                                       | 예      | 2.0                                   |
| 엔터티 분할                                      | 예      |                                       |
| 데이터베이스 스칼라 함수 매핑                      | 나쁨     | 2.0                                   |
| 필드 매핑                                         |          | 1.1                                   |

### <a name="querying-data"></a>데이터 쿼리

| **기능                                             | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| LINQ 쿼리                                          | 예      | 1.0(복합 쿼리에 대해 진행 중) |
| 읽기 가능한 생성된 SQL                                | 나쁨     | 1.0                                   |
| 혼합 클라이언트/서버 평가                        |          | 1.0                                   |
| GroupBy 변환                                   | 예      | 2.1                                   |
| 관련 데이터 로드: 즉시                           | 예      | 1.0                                   |
| 관련 데이터 로드: 파생된 형식에 즉시 로드 |          | 2.1                                   |
| 관련 데이터 로드: 지연                            | 예      | 2.1                                   |
| 관련 데이터 로드: 명시적                        | 예      | 1.1                                   |
| 원시 SQL 쿼리: 엔터티 형식                         | 예      | 1.0                                   |
| 원시 SQL 쿼리: 비 엔터티 형식(쿼리 형식)       | 예      | 2.1                                   |
| 원시 SQL 쿼리: LINQ로 작성                  |          | 1.0                                   |
| 명시적으로 컴파일된 쿼리                           | 나쁨     | 2.0                                   |
| 텍스트 기반 쿼리 언어(Entity SQL)                | 예      |                                       |

### <a name="saving-data"></a>데이터 저장

| **기능**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| 변경 내용 추적: 스냅숏                             | 예      | 1.0                                   |
| 변경 내용 추적: 알림                         | 예      | 1.0                                   |
| 변경 내용 추적: 프록시                              | 예      |                                       |
| 추적된 상태 액세스                               | 예      | 1.0                                   |
| 낙관적 동시성                                | 예      | 1.0                                   |
| 트랜잭션                                          | 예      | 1.0                                   |
| 문 일괄 처리                                |          | 1.0                                   |
| 저장 프로시저 매핑                              | 예      |                                       |
| 연결이 끊긴 그래프 하위 수준 API                     | 나쁨     | 1.0                                   |
| 연결이 끊긴 그래프 종단 간                         |          | 1.0(부분)                         |

### <a name="other-features"></a>기타 기능

| **기능**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| 마이그레이션                                            | 예      | 1.0                                   |
| 데이터베이스 만들기/삭제 API                       | 예      | 1.0                                   |
| 시드 데이터                                             | 예      | 2.1                                   |
| 연결 복원력                                 | 예      | 1.1                                   |
| 수명 주기 후크(이벤트, 인터셉션)                | 예      |                                       |
| 간단한 로깅(Database.Log)                         | 예      |                                       |
| DbContext 풀링                                     |          | 2.0                                   |

### <a name="database-providers"></a>데이터베이스 공급자

| **기능**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| SQL Server                                            | 예      | 1.0                                   |
| MySQL                                                 | 예      | 1.0                                   |
| PostgreSQL                                            | 예      | 1.0                                   |
| Oracle                                                | 예      | 1.0 <sup>(1)</sup>                    |
| SQLite                                                | 예      | 1.0                                   |
| SQL Server Compact                                    | 예      | 1.0 <sup>(2)</sup>                    |
| DB2                                                   | 예      | 1.0                                   |
| Firebird                                              | 예      | 2.0                                   |
| Jet(Microsoft Access)                                |          | 2.0 <sup>(2)</sup>                    |
| 메모리 내(테스트용)                               |          | 1.0                                   |

<sup>1</sup> 현재 Oracle에 사용할 수 있는 유료 공급자가 지원됩니다. Oracle의 무료 공식 공급자를 사용 중입니다.

<sup>2</sup> SQL Server Compact 및 Jet 공급자는 .NET Core가 아닌 .NET Framework에서만 작동합니다.

### <a name="net-implementations"></a>.NET 구현

| **기능**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| .NET Framework(콘솔, WinForms, WPF, ASP.NET)      | 예      | 1.0                                   |
| .NET Core(콘솔, ASP.NET Core)                     |          | 1.0                                   |
| Mono 및 Xamarin                                        |          | 1.0(진행 중)                     |
| UWP                                                   |          | 1.0(진행 중)                     |

## <a name="guidance-for-new-applications"></a>새 응용 프로그램에 대한 지침

다음 조건이 모두 충족되면 새 응용 프로그램에 EF Core를 사용해 보세요.
* 이 앱을 사용하려면 .NET Core의 기능이 필요합니다. 자세한 내용은 [서버 앱에 대해 .NET Core와 .NET Framework 중에 선택](https://docs.microsoft.com/en-us/dotnet/standard/choosing-core-framework-server)을 참조하세요.
* EF Core는 앱에 필요한 모든 기능을 지원합니다. 원하는 기능이 없는 경우에는 [EF Core 로드맵](xref:core/what-is-new/roadmap)을 확인하여 나중에 지원할 계획이 있는지 확인하세요. 

다음 조건이 모두 충족되면 EF6를 사용해 보세요.
* 이 앱은 Windows 및 .NET Framework 4.0 이상에서 실행됩니다.
* EF6는 앱에 필요한 모든 기능을 지원합니다.

## <a name="guidance-for-existing-ef6-applications"></a>기존 EF6 응용 프로그램에 대한 지침

EF Core의 기본적인 변경 내용 때문에, 변경할 중요한 이유가 있는 경우에만 EF6 응용 프로그램을 EF Core로 이동하는 것이 좋습니다. 새로운 기능을 사용하기 위해 EF Core로 이동하려면 제한 사항을 알고 있어야 합니다. 자세한 내용은 [EF6에서 EF Core로 이식](porting/index.md)을 참조하세요. **EF6에서 EF Core로의 이동은 업그레이드보다는 이식에 가깝습니다.** 

## <a name="next-steps"></a>다음 단계

자세한 내용은 설명서를 참조하세요.
* <xref:core/index>
* <xref:ef6/index>
