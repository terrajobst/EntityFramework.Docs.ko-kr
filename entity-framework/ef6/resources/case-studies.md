---
title: Entity Framework에 대 한 사례 연구-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cd5d3ae3-717d-4095-a2ef-0e8fd72b1a2f
ms.openlocfilehash: d7982a3f89ac1e57b48039d828f287adf6dc5068
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414460"
---
# <a name="microsoft-case-studies-for-entity-framework"></a>Entity Framework에 대 한 Microsoft 사례 연구
이 페이지의 사례 연구는 Entity Framework을 사용 하는 몇 가지 실제 프로덕션 프로젝트를 강조 표시 합니다.
> [!NOTE]
> 이러한 사례 연구의 자세한 버전은 Microsoft 웹 사이트에서 더 이상 사용할 수 없습니다. 따라서 링크가 제거 되었습니다.

## <a name="epicor"></a>Epicor
Epicor는 150 개국 이상에서 회사에 대 한 ERP (엔터프라이즈 리소스 계획) 솔루션을 개발 하는 뛰어난 글로벌 소프트웨어 회사 (400 명 이상의 개발자를 포함)입니다.
주력 제품인 Epicor 9는 .NET Framework을 사용 하는 SOA (서비스 지향 아키텍처)를 기반으로 합니다.
LINQ (통합 언어 쿼리)에 대 한 지원을 제공 하는 많은 고객 요청에 직면 하 고 백 엔드 SQL Server에 대 한 부하를 줄이려면 팀에서 Visual Studio 2010 및 .NET Framework 4.0로 업그레이드 하기로 결정 했습니다.
Entity Framework 4.0를 사용 하 여 이러한 목표를 달성 하 고 개발 및 유지 관리를 대폭 간소화할 수 있었습니다.
특히 Entity Framework의 다양 한 T4 지원에서는 생성 된 코드를 완전히 제어 하 고 미리 컴파일된 쿼리 및 캐싱과 같은 성능 저장 기능을 자동으로 빌드할 수 있습니다.

> "최근에 기존 코드를 사용 하 여 몇 가지 성능 테스트를 수행 했으며 SQL Server 요청을 90%까지 줄일 수 있었습니다.
ADO.NET Entity Framework 4 이기 때문입니다. " – Erik Johnson, 부사장, 제품 조사  

## <a name="veracity-solutions"></a>진실성 솔루션
장기적으로 유지 관리 하 고 확장 하기가 어려운 이벤트 계획 소프트웨어 시스템을 확보 진실성 솔루션은 Visual Studio 2010을 사용 하 여 Silverlight 4를 기반으로 하는 강력 하 고 사용 하기 쉬운 리치 인터넷 응용 프로그램으로 다시 작성 했습니다.
.NET RIA 서비스를 사용 하 여 코드 중복을 방지 하 고 계층 간의 일반적인 유효성 검사 및 인증 논리에 대해 허용 되는 Entity Framework 위에서 서비스 계층을 신속 하 게 빌드할 수 있었습니다.  

> "처음 도입 되었을 때 Entity Framework에 판매 되었으며 Entity Framework 4는 훨씬 더 잘 검증 되었습니다.
도구는 향상 되었으며 개념적 모델, 저장소 모델 및 해당 모델 간 매핑을 정의 하는 .edmx 파일을 보다 쉽게 조작할 수 있습니다. Entity Framework를 사용 하 여 데이터 액세스 계층을 하루에 사용할 수 있으며,이에 따라 진행 하면서 빌드할 수 있습니다.
Entity Framework은 사실상 데이터 액세스 계층입니다. 사용자가 사용 하지 않는 이유를 알 수 없습니다. " – Joe McBride, 수석 개발자

## <a name="nec-display-solutions-of-america"></a>아메리카 아메리카의 NEC 디스플레이 솔루션
NEC는 광고주와 네트워크 소유자의 혜택을 제공 하 고 수익을 높이기 위한 솔루션을 사용 하 여 디지털 장소 기반 광고를 위한 시장을 입력 하고자 했습니다.
이렇게 하기 위해 기존 ad 캠페인에 필요한 수동 프로세스를 자동화 하는 웹 응용 프로그램 쌍을 시작 합니다.
이 사이트는 ASP.NET, Silverlight 3, AJAX 및 WCF를 사용 하 여 데이터 액세스 계층의 Entity Framework와 함께 SQL Server 2008와 통신 하는 데 사용 됩니다.

> "SQL Server를 사용 하면 중요 업무용 응용 프로그램의 정보를 항상 사용할 수 있도록 하는 데 필요한 처리량을 실시간으로 제공 하 고 안정성을 제공 하는 데 필요한 처리량을 얻을 수 있습니다. IT 담당 이사

## <a name="darwin-dimensions"></a>다윈 차원
다윈의 팀은 다양 한 Microsoft 기술을 사용 하 여 Evolver를 만들도록 설정 합니다. 즉, 고객이 게임, 애니메이션 및 소셜 네트워킹 페이지에서 사용할 수 있는 멋진 아바타를 만드는 데 사용할 수 있는 온라인 아바타 포털입니다.
Entity Framework의 생산성 이점 및 Windows Workflow Foundation (WF) 및 Windows Server AppFabric (확장성이 뛰어난 메모리 내 응용 프로그램 캐시)와 같은 구성 요소를 끌어올 경우 팀은 35% 더 저렴 한 제품을 제공할 수 있었습니다. 개발 시간.
팀 멤버가 여러 국가에 걸쳐 분할 된 상태에도 불구 하 고, 팀은 주간 릴리스를 통해 agile 개발 프로세스를 수행 합니다.

 > "우리는 기술 전문가를 위한 기술을 만들지 않습니다. 시작으로 시간과 비용을 절감 하는 기술을 활용 하는 것이 중요 합니다.
 .NET은 빠르고 비용 효율적인 개발을 위해 선택 했습니다. " – Zachary Olsen, 설계자  

## <a name="silverware"></a>Silverware
중소 식당 그룹을 위한 POS (point of sale) 솔루션을 개발 하는 데 15 년간의 경험을 사용 하 여 Silverware의 개발 팀은 더 많은 엔터프라이즈 수준 기능을 통해 제품을 향상 시키기 위해 식당 체인.
최신 버전의 Microsoft 개발 도구를 사용 하 여 이전 보다 4 배 더 빠르게 새 솔루션을 구축할 수 있었습니다.
LINQ 및 Entity Framework와 같은 주요 새로운 기능을 통해 데이터 저장소 및 보고 요구를 위해 Crystal Reports에서 SQL Server 2008 및 SQL Server Reporting Services (SSRS)로 쉽게 이동할 수 있었습니다.

> "효과적인 데이터 관리는 SilverWare의 성공을 위한 핵심 이며,이 때문에 SQL Reporting을 채택 하기로 결정 했습니다." -있는 nicholas Romanidis, IT/소프트웨어 엔지니어링 담당 이사
