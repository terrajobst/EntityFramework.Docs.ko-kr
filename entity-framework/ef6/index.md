---
title: Entity Framework 6 개요 - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 28a13879416a52cbe8035c23013f16390c75c4c9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78412758"
---
# <a name="entity-framework-6"></a>Entity Framework 6
EF6(Entity Framework 6)는 수년에 걸친 기능 개발 및 안정화 과정을 통해 테스트를 거친 .NET용 O/RM(개체 관계형 매퍼)입니다.

O/RM으로써 EF6는 관계형 데이터베이스와 개체 중심 데이터베이스 사이의 불일치를 완화하고, 개발자가 애플리케이션의 도메인을 나타내는 강력한 형식의 .NET 개체를 사용하여 관계형 데이터베이스에 저장된 데이터와 상호 작용할 수 있게 해주고, 일반적으로 개발자가 작성해야 하는 데이터 액세스 "내부" 코드의 많은 부분을 할 필요가 없게 만들어 줍니다.

EF6는 다양한 인기 O/RM 기능을 구현합니다.
- EF 형식에 따라 달라지지 않는 [POCO](xref:ef6/resources/glossary#poco) 엔터티 클래스 매핑
- 자동 변경 내용 추적
- ID 확인 및 작업 단위
- 즉시 로드, 지연 로드 및 명시적 로드
- [LINQ(Language INtegrated Query)](https://aka.ms/AA6hsvu)를 사용한 강력한 형식의 쿼리 변환
- 다음 지원을 포함한 풍부한 매핑 기능:
  - 일대일, 일대다 및 다대다 관계
  - 상속(계층 구조별 테이블, 형식별 테이블, 구체적인 클래스별 테이블)
  - 복합 형식
  - 저장 프로시저
- 엔터티 모델을 만드는 시각적 디자이너.
- 코드를 작성하여 엔터티 모델을 만드는 "Code First" 환경
- 기존 데이터베이스에서 모델을 생성한 후 직접 편집할 수도 있고, 처음부터 새로 만든 후 새 데이터베이스를 생성하는 데 사용할 수도 있습니다.
- ASP.NET을 포함한 .NET Framework 애플리케이션 모델과 통합, 데이터 바인딩을 통해 WPF 및 WinForms와 통합.
- ADO.NET 및 SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 등에 연결할 수 있는 다양한 [공급자](xref:ef6/fundamentals/providers/index)를 기반으로 하는 데이터베이스 연결.

## <a name="should-i-use-ef6-or-ef-core"></a>EF6 또는 EF Core를 사용해야 하나요?

EF Core는 가볍고 확장 가능한 최신 버전의 Entity Framework로, EF6와 매우 비슷한 기능을 제공합니다.
EF Core는 완전히 다시 작성되었으며, EF6의 고급 매핑 기능 중 일부를 제공하지 않지만 EF6에 없는 여러 새 기능을 포함하고 있습니다.
기능 집합이 요구 사항과 일치하는 경우 새 애플리케이션에서 EF Core를 사용해 보세요.
[EF Core & EF6 비교](xref:efcore-and-ef6/index)에서는 이 선택에 대해 자세히 살펴봅니다.

## <a name="get-started"></a>[시작](xref:ef6/get-started)

프로젝트에 EntityFramework NuGet 패키지를 추가하거나 [Visual Studio용 Entity Framework Tools](https://aka.ms/AA6i8c5)를 설치합니다. 그런 다음, EF6를 최대한 활용하는 방법을 알려주는 비디오를 시청하고, 자습서 및 고급 설명서를 읽습니다.

## <a name="past-entity-framework-versions"></a>이전 Entity Framework 버전

Entity Framework 6 최신 버전에 대한 설명서지만, 많은 부분이 이전 릴리스에도 적용됩니다.
EF 릴리스 및 포함된 기능의 전체 목록은 [새로운 기능](xref:ef6/what-is-new/index) 및 [이전 릴리스](xref:ef6/what-is-new/past-releases)를 확인하세요.
