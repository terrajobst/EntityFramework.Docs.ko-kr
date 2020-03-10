---
title: EF6 및 EF Core 비교
description: EF6 및 EF Core 중에서 선택하는 방법에 대한 지침입니다.
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: e28c7d0299e5089f56fb0795d00917cfc30f5cf1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413398"
---
# <a name="compare-ef-core--ef6"></a>EF Core & EF6 비교

## <a name="ef-core"></a>EF Core

[EF Core](../core/index.md)(Entity Framework Core)는 .NET용 최신 개체 데이터베이스 매퍼입니다. LINQ 쿼리, 변경 내용 추적, 업데이트 및 스키마 마이그레이션을 지원합니다.

EF Core는 SQL Server/SQL Azure, SQLite, Azure Cosmos DB, MySQL, PostgreSQL에서 작동할 뿐 아니라 [데이터베이스 공급자 플러그 인 모델](../core/providers/index.md)을 통해 많은 데이터베이스에서 작동합니다.

## <a name="ef6"></a>EF6

[EF6](../ef6/index.md)(Entity Framework 6)은 .NET Framework용으로 디자인된 개체 관계형 매퍼이나 .NET Core를 지원합니다. EF6은 안정적인 지원 제품이지만 더 이상 활발히 개발되고 있지 않습니다.

## <a name="feature-comparison"></a>기능 비교

EF Core는 EF6에서 구현되지 않을 새로운 기능을 제공합니다. 그러나 일부 EF6 기능은 현재 EF Core에서 구현되지 않았습니다.

다음 표에서는 EF Core 및 EF6에서 사용할 수 있는 기능을 비교합니다. 이는 개략적인 비교로서, 모든 기능을 나열하거나 다른 EF 버전에 있는 동일한 기능 간의 차이를 설명하지는 않습니다.

EF Core 열은 기능이 처음 나타나는 제품 버전을 나타냅니다.

### <a name="creating-a-model"></a>모델 만들기

| **기능**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| 기본 클래스 매핑                                   | 예      | 1.0                                   |
| 매개 변수가 포함된 생성자                          |          | 2.1                                   |
| 속성 값 변환                            |          | 2.1                                   |
| 키가 없는 매핑된 형식                             |          | 2.1                                   |
| 규칙                                           | 예      | 1.0                                   |
| 사용자 지정 규칙                                    | 예      | 1.0(부분, [#214](https://github.com/dotnet/efcore/issues/214)) |
| 데이터 주석                                      | 예      | 1.0                                   |
| Fluent API                                            | 예      | 1.0                                   |
| 상속: TPH(계층당 하나의 테이블)                | 예      | 1.0                                   |
| 상속: TPT(형식당 테이블)                     | 예      | 5\.0용으로 계획됨([#2266](https://github.com/dotnet/efcore/issues/2266)) |
| 상속: TPC(구체적인 클래스별 테이블)           | 예      | 5\.0용 Stretch([#3170](https://github.com/dotnet/efcore/issues/3170)) <sup>(1)</sup> |
| 섀도 상태 속성                               |          | 1.0                                   |
| 대체 키                                        |          | 1.0                                   |
| 다 대 다 탐색                              | 예      | 5\.0용으로 계획됨([#19003](https://github.com/dotnet/efcore/issues/19003)) |
| 조인 엔터티가 없는 다 대 다                      | 예      | 백로그에서([#1368](https://github.com/dotnet/efcore/issues/1368)) |
| 키 생성: 데이터베이스                              | 예      | 1.0                                   |
| 키 생성: 클라이언트                                |          | 1.0                                   |
| 복합/소유된 형식                                   | 예      | 2.0                                   |
| 공간 데이터                                          | 예      | 2.2                                   |
| 모델 형식: 코드                                    | 예      | 1.0                                   |
| 데이터베이스에서 모델 만들기: 명령줄              | 예      | 1.0                                   |
| 데이터베이스에서 모델 업데이트                            | Partial  | 백로그에서([#831](https://github.com/dotnet/efcore/issues/831)) |
| 전역 쿼리 필터                                  |          | 2.0                                   |
| 테이블 분할                                       | 예      | 2.0                                   |
| 엔터티 분할                                      | 예      | 5\.0용 Stretch([#620](https://github.com/dotnet/efcore/issues/620)) <sup>(1)</sup> |
| 데이터베이스 스칼라 함수 매핑                      | 나쁨     | 2.0                                   |
| 필드 매핑                                         |          | 1.1                                   |
| nullable 참조 형식(C# 8.0)                     |          | 3.0                                   |
| 모델의 그래픽 시각화                      | 예      | 지원 계획 없음 <sup>(2)</sup>     |
| 그래픽 모델 편집기                                | 예      | 지원 계획 없음 <sup>(2)</sup>     |
| 모델 형식: EDMX(XML)                              | 예      | 지원 계획 없음 <sup>(2)</sup>     |
| 데이터베이스에서 모델 만들기: VS 마법사                 | 예      | 지원 계획 없음 <sup>(2)</sup>     |

### <a name="querying-data"></a>데이터 쿼리

| **기능**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| LINQ 쿼리                                          | 예      | 1.0                                   |
| 읽기 가능한 생성된 SQL                                | 나쁨     | 1.0                                   |
| GroupBy 변환                                   | 예      | 2.1                                   |
| 관련 데이터 로드: 즉시                           | 예      | 1.0                                   |
| 관련 데이터 로드: 파생 형식에 대한 즉시 로드 |          | 2.1                                   |
| 관련 데이터 로드: 지연                            | 예      | 2.1                                   |
| 관련 데이터 로드: 명시적 방법                        | 예      | 1.1                                   |
| 원시 SQL 쿼리: 엔터티 형식                         | 예      | 1.0                                   |
| 원시 SQL 쿼리: 키 없는 엔터티 형식                 | 예      | 2.1                                   |
| 원시 SQL 쿼리: LINQ로 작성                  |          | 1.0                                   |
| 명시적으로 컴파일된 쿼리                           | 나쁨     | 2.0                                   |
| await foreach(C# 8.0)                                |          | 3.0                                   |
| 텍스트 기반 쿼리 언어(Entity SQL)                | 예      | 지원 계획 없음 <sup>(2)</sup>     |

### <a name="saving-data"></a>데이터 저장

| **기능**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| 변경 내용 추적: 스냅샷                             | 예      | 1.0                                   |
| 변경 내용 추적: 알림                         | 예      | 1.0                                   |
| 변경 내용 추적: Proxy                              | 예      | 5\.0용으로 병합됨([#10949](https://github.com/dotnet/efcore/issues/10949)) |
| 추적된 상태 액세스                               | 예      | 1.0                                   |
| 낙관적 동시성                                | 예      | 1.0                                   |
| 의                                          | 예      | 1.0                                   |
| 문 일괄 처리                                |          | 1.0                                   |
| 저장 프로시저 매핑                              | 예      | 백로그에서([#245](https://github.com/dotnet/efcore/issues/245)) |
| 연결이 끊긴 그래프 하위 수준 API                     | 나쁨     | 1.0                                   |
| 연결이 끊긴 그래프 엔드투엔드                         |          | 1.0(부분, [#5536](https://github.com/dotnet/efcore/issues/5536)) |

### <a name="other-features"></a>기타 기능

| **기능**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| 마이그레이션                                            | 예      | 1.0                                   |
| 데이터베이스 만들기/삭제 API                       | 예      | 1.0                                   |
| 시드 데이터                                             | 예      | 2.1                                   |
| 연결 복원력                                 | 예      | 1.1                                   |
| 인터셉터                                          | 예      | 3.0                                   |
| 이벤트                                                | 예      | 3.0(부분, [#626](https://github.com/dotnet/efcore/issues/626)) |
| 간단한 로깅(Database.Log)                         | 예      | 5\.0용으로 병합됨([#1199](https://github.com/dotnet/efcore/issues/1199)) |
| DbContext 풀링                                     |          | 2.0                                   |

### <a name="database-providers-sup3sup"></a>데이터베이스 공급자 <sup>(3)</sup>

| **기능**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| SQL Server                                            | 예      | 1.0                                   |
| MySQL                                                 | 예      | 1.0                                   |
| PostgreSQL                                            | 예      | 1.0                                   |
| Oracle                                                | 예      | 1.0                                   |
| SQLite                                                | 예      | 1.0                                   |
| SQL Server Compact                                    | 예      | 1.0 <sup>(4)</sup>                    |
| DB2                                                   | 예      | 1.0                                   |
| Firebird                                              | 예      | 2.0                                   |
| Jet(Microsoft Access)                                |          | 2.0 <sup>(4)</sup>                    |
| Azure Cosmos DB                                       |          | 3.0                                   |
| 메모리 내(테스트용)                               |          | 1.0                                   |

<sup>1</sup> 제공된 릴리스에 대해 Stretch 목표를 달성할 가능성이 없습니다. 그러나 제대로 작동하는 경우 끌어오기를 시도합니다.

<sup>2</sup> 일부 EF6 기능이 EF Core에서 구현되지 않습니다. 해당 기능은 EF6의 기본 EDM(엔터티 데이터 모델)을 사용하거나 투자 수익률이 비교적 낮은 복잡한 기능입니다. 항상 피드백을 환영하지만, EF Core는 EF6에서 가능하지 않은 많은 작업을 가능하게 하는 반면 EF Core가 EF6의 모든 기능을 지원할 가능성은 없습니다.

<sup>3</sup> 타사에서 구현하는 EF Core 데이터베이스 공급자는 EF Core의 새로운 주 버전으로 업데이트될 때 지연될 수 있습니다. 자세한 내용은 [데이터베이스 공급자](../core/providers/index.md)를 참조하세요.

<sup>4</sup> SQL Server Compact 및 Jet 공급자는 .NET Core가 아닌 .NET Framework에서만 작동합니다.

### <a name="supported-platforms"></a>지원되는 플랫폼

EF Core 3.1은 .NET Standard 2.0을 사용하여 .NET Core 및 .NET Framework에서 실행됩니다. 그러나 EF Core 5.0은 .NET Framework에서 실행되지 않습니다. 자세한 내용은 [플랫폼](../core/platforms/index.md)을 참조하세요.

EF6.4는 멀티타기팅을 통해 .NET Core 및 .NET Framework에서 실행됩니다.

## <a name="guidance-for-new-applications"></a>새 애플리케이션에 대한 지침

앱에 [.NET Framework에서만 지원되는](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) 항목이 필요하지 않은 한 .NET Core에서는 모든 새 애플리케이션용으로 EF Core를 사용합니다.

## <a name="guidance-for-existing-ef6-applications"></a>기존 EF6 애플리케이션에 대한 지침

EF Core는 EF6을 즉시 대체하지 않습니다. EF6에서 EF Core로 이동하는 경우 애플리케이션을 변경해야 할 수 있습니다.

EF6 앱을 .NET Core로 이동하는 경우:
* 데이터 액세스 코드가 안정적이며 개선 가능성이 없거나 새로운 기능이 필요하지 않을 경우 EF6를 계속 사용합니다.
* 데이터 액세스 코드가 개선되거나 앱에 EF Core에서만 제공하는 새로운 기능이 필요한 경우 EF Core로 포팅합니다.
* 종종 성능을 위해서도 EF Core로 포팅합니다. 그러나 일부 시나리오는 더 빠르지 않으므로 일부 프로파일링을 먼저 수행합니다.

자세한 내용은 [EF6에서 EF Core로 포팅](porting/index.md)을 참조하세요.
