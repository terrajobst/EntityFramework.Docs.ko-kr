---
title: Entity Framework Core 로드맵
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: 7eba9e1a8e145ef407f844ff3a3ab3069495b7ae
ms.sourcegitcommit: e66745c9f91258b2cacf5ff263141be3cba4b09e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/06/2019
ms.locfileid: "54058736"
---
# <a name="entity-framework-core-roadmap"></a>Entity Framework Core 로드맵

> [!IMPORTANT]
> 이후 릴리스의 기능 집합 및 일정은 항상 변경될 수 있으며 이 페이지를 최신 상태로 유지하려고 노력함에도 불구하고 최신 계획을 항상 반영할 수 없을 수도 있습니다.

### <a name="ef-core-30"></a>EF Core 3.0

뛰어난 EF Core 2.2와 함께 Microsoft는 이제 .NET Core 3.0 및 ASP.NET 3.0 릴리스에 맞춰질 EF Core 3.0에 집중하고 있습니다.

아직 새 기능이 완성되지 않았으므로 2018년 12월에  [NuGet 갤러리에 게시된 EF Core 3.0 미리 보기 1 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.0-preview.18572.1) 에는 [버그 수정, 부분 개선 사항 및 3.0 작업에 대비해 수행한 변경 사항](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed+label%3Aclosed-fixed)만 포함됩니다.

사실, Microsoft는 할당된 시간 내에 완성할 수 있는 적절한 기능 모음을 갖추도록 3.0 [릴리스 계획](#release-planning-process)을 계속 조정해야 합니다.
계획이 더 분명해지면 더 많은 작업을 공유하겠습니다. 작업하려는 높은 수준의 테마와 기능 몇 가지는 다음과 같습니다.

- **LINQ 기능 향상([#12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795))**: LINQ를 사용하면 선택한 언어로 데이터베이스 쿼리를 작성할 수 있어서 다양한 종류의 정보를 활용하여 IntelliSense 및 컴파일 시간 형식 검사를 수행할 수 있습니다.
  그러나 LINQ를 사용하면 복잡한 쿼리를 무제한으로 작성할 수도 있습니다. 이는 LINQ 공급자에게 항상 큰 문제였습니다.
  EF Core의 처음 몇 버전에서는 쿼리 중 SQL로 변환할 수 있는 부분을 생각한 다음, 나머지 쿼리가 클라이언트의 메모리에서 실행되도록 허용하여 이 문제를 부분적으로 해결했습니다.
  이러한 클라이언트 쪽 실행은 일부 상황에서는 바람직하지만, 다른 많은 경우에는 애플리케이션이 프로덕션에 배포될 때까지 식별되지 않을 수 있는 비효율적인 쿼리가 발생할 수 있습니다.
  EF Core 3.0에서는 LINQ 구현 방식과 테스트 방법을 크게 변화시킬 계획입니다.
  목표는 패치 릴리스에서 쿼리 중단을 방지하는 등 더욱 강력한 EF Core를 만들고, 더 많은 식을 SQL로 정확하게 변환할 수 있고, 더 많은 경우에 효율적인 쿼리를 생성하며, 비효율적인 쿼리가 검색되지 않는 것을 방지하는 것입니다.

- **Cosmos DB 지원([#8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443))**: Microsoft는 개발자가 EF 프로그래밍 모델에 친숙해져서 애플리케이션 데이터베이스로 Azure Cosmos DB를 쉽게 대상으로 지정할 수 있도록 하기 위해 EF Core의 Cosmos DB 공급자에 공을 들이고 있습니다.
  목표는 전역 배포, “always on” 가용성, 탄력적인 확장성, 짧은 대기 시간, .NET 개발자에 훨씬 더 많은 액세스 가능성과 같은 몇몇 Cosmos DB 기능을 활용하는 것입니다.
  공급자는 Cosmos DB의 SQL API에 대해 자동 변경 내용 추적, LINQ, 값 변환 같은 대부분의 EF Core 기능을 사용할 수 있습니다. Microsoft는 EF Core 2.2 전에 이러한 노력을 시작했으며, [공급자의 몇몇 미리 보기 버전을 제공했습니다](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
  새 계획은 EF Core 3.0과 함께 공급자를 계속 개발하는 것입니다.   

- **C# 8.0 지원([#12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047))**: Microsoft는 고객이 EF Core를 사용하면서 비동기 스트림(각각에 대한 대기 포함), nullable 참조 형식 같은 일부 [C# 8.0의 새 기능](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/)을 활용하기를 바랍니다.

- **쿼리 형식의 리버스 엔지니어링 데이터베이스 뷰([#1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679))**: EF Core 2.1에서는 데이터베이스에서 읽을 수는 있지만 업데이트할 수 없는 데이터를 나타낼 수 있는 쿼리 형식에 대한 지원을 추가했습니다.
  쿼리 형식은 데이터베이스 뷰 매핑에 아주 적합하므로 EF Core 3.0에서는 데이터베이스 뷰에 대한 쿼리 형식 생성을 자동화하고자 합니다.

- **속성 모음 엔터티([#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) 및 [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914))**: 이 기능은 일반 속성 대신 인덱싱된 속성에 데이터를 저장하는 엔터티를 사용하고 동일한 .NET 클래스의 인스턴스(잠재적으로 `Dictionary<string, object>`만큼 단순한 인스턴스)를 사용하여 동일한 EF Core 모델에서 여러 엔터티 형식을 나타낼 수 있도록 하는 기능입니다.
  이 기능은 EF Core에 대해 가장 요청이 많았던 기능 향상 중 하나인 조인 엔터티가 없는 다 대 다 관계를 지원하는 발판입니다.

- **.NET Core의 EF 6.3([EF6 #271](https://github.com/aspnet/EntityFramework6/issues/271))**: 많은 기존 애플리케이션이 이전 버전의 EF를 사용하고 단지 .NET Core를 활용하기 위해 이전 버전의 EF를 EF Core로 포팅하는 일은 상당한 노력이 필요할 수 있다는 점을 이해합니다.
  그러한 이유로, Microsoft는 다음 버전의 EF 6가 .NET Core 3.0에서 실행되도록 조정할 예정입니다.
  이는 최소한의 변경으로 기존 애플리케이션의 포팅을 간단히 수행하기 위해서입니다.
  예를 들어 새 공급자가 필요하고 SQL Server에서 공간 지원이 불가능한 것과 같은 몇 가지 제한 사항이 있을 예정이며, EF 6에 대해 계획된 새 기능은 없습니다.

그동안에는 [Issue Tracker에서 이 쿼리](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc)를 사용하여 3.0에 할당된 작업 항목을 임시로 확인하세요.

## <a name="schedule"></a>일정

EF Core에 대한 일정은 [.NET Core 일정](https://github.com/dotnet/core/blob/master/roadmap.md) 및 [ASP.NET Core 일정](https://github.com/aspnet/Home/wiki/Roadmap)과 동기화됩니다.

## <a name="backlog"></a>백로그

Issue Tracker의 [백로그 마일스톤](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc)에는 언젠가 작업할 문제 또는 커뮤니티의 누군가가 해결할 수 있을 것으로 생각하는 문제가 포함되어 있습니다.
고객은 이러한 문제에 대해 의견을 제출하고 투표할 수 있습니다.
이러한 문제를 작업하려는 참가자는 문제에 대한 접근 방법을 토론하는 것부터 시작하는 것이 좋습니다.

특정 버전의 EF Core에서 제공된 기능에 대해 Microsoft가 작업한다는 보장은 없습니다.
모든 소프트웨어 프로젝트, 우선 순위, 릴리스 일정 및 사용 가능한 리소스는 언제든지 변경될 수 있습니다.
그러나 특정 시기에 문제를 해결하려는 경우 백로그 마일스톤이 아닌 릴리스 마일스톤에 해당 문제를 할당합니다.
Microsoft는 [릴리스 계획 프로세스](#release-planning-process)의 일부로써 백로그 마일스톤과 릴리스 마일스톤 간에 일상적으로 문제를 이동합니다.

문제를 해결할 계획이 없는 경우 문제를 종결할 가능성이 있습니다.
그러나 새로운 정보가 있는 경우 이전에 종결한 문제를 다시 고려할 수 있습니다.

## <a name="release-planning-process"></a>릴리스 계획 프로세스

특정 릴리스로 들어가는 특정 기능 선택 방법에 대한 질문을 종종 받습니다.
확실히 백로그는 릴리스 계획으로 자동으로 변환되지 않습니다.
EF6에서 기능의 존재 여부도 기능이 EF Core에서 구현되어야 함을 자동으로 의미하지 않습니다.

릴리스를 계획하기 위해 수행하는 전체 프로세스를 자세히 설명하기는 어렵습니다.
대부분의 경우 특정 기능, 기회 및 우선 순위에 대해 이야기하며 프로세스 자체는 모든 릴리스와 함께 진행됩니다.
그러나 다음에 작업할 항목을 결정할 때 답변하려는 일반적인 질문은 요약할 수 있습니다.

1. **얼마나 많은 개발자가 해당 기능을 사용하고 해당 애플리케이션/환경을 얼마나 더 좋게 만들 수 있을까요?** 이 질문에 답변하기 위해 Microsoft는 많은 소스에서 피드백을 수집하며, 문제에 대한 의견 및 투표는 이러한 소스 중 하나입니다.

2. **이 기능을 아직 구현하지 않은 경우 사람들이 사용할 수 있는 해결 방법은 무엇인가요?** 예를 들어 많은 개발자는 네이티브 다대다 지원 부족의 문제를 해결하기 위해 조인 테이블을 매핑할 수 있습니다. 분명히 일부 개발자는 그렇게 하고 싶어 하지 않지만 많은 개발자가 그렇게 할 수 있으며, 이는 결정의 한 요인으로 고려됩니다.

3. **이 기능의 구현은 다른 기능의 구현으로 가까이 다가가도록 하는 EF Core의 아키텍처를 진화시키나요?** Microsoft는 다른 기능의 구성 요소로 작동하는 기능을 선호하는 경향이 있습니다. 예를 들어 속성 모음 엔터티는 다대다 지원을 지향하는 데 도움이 되며, 엔터티 생성자는 지연 로드 지원을 사용하도록 설정했습니다. 

4. **기능은 확장성 지점인가요?** 확장성 지점을 통해 개발자는 해당 동작에 연결하고 누락된 기능을 보완할 수 있으므로 Microsoft는 확장성 지점을 선호하는 경향이 있습니다. 

5. **다른 제품과 함께 사용할 경우 기능의 시너지 효과는 무엇인가요?** Microsoft는 EF Core를 .NET Core, 최신 버전의 Visual Studio, Microsoft Azure 등과 같은 다른 제품과 함께 사용하도록 하거나 그러한 환경을 크게 향상하는 기능을 선호합니다.

6. **사람들이 기능에 사용할 수 있는 기술은 무엇이며 이러한 리소스를 가장 잘 활용하는 방법은 무엇인가요?** EF 팀의 각 구성원과 커뮤니티 참가자는 서로 다른 영역에서 다양한 수준의 경험이 있으므로 그에 따라 계획해야 합니다. GroupBy 번역 또는 다대다와 같은 특정 기능을 작업하기 위해 “모든 인력을 동원”하려고 해도 이는 현실성이 떨어질 것입니다.

이전에 언급한 것처럼 프로세스는 모든 릴리스에서 진행됩니다.
향후 커뮤니티 구성원이 릴리스 계획에 대한 의견을 제공하는 기회를 더 많이 추가하겠습니다.
예를 들어 기능과 릴리스 계획 자체의 초안 디자인을 더 쉽게 검토할 수 있도록 하고 싶습니다.
