---
title: EF Core 릴리스 계획
author: ajcvickers
ms.date: 01/28/2020
uid: core/what-is-new/release_planning.md
ms.openlocfilehash: 71045b8d49c319a73f74443612bedd84ee33ab8a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413542"
---
# <a name="release-planning-process"></a>릴리스 계획 프로세스

특정 릴리스로 들어가는 특정 기능 선택 방법에 대한 질문을 종종 받습니다.
이 문서에서는 사용하는 프로세스에 대해 간략하게 설명합니다.
이 프로세스는 더 나은 계획 방법을 찾으면서 지속적으로 발전하고 있지만 일반적인 아이디어는 동일하게 유지됩니다.

## <a name="different-kinds-of-releases"></a>다양한 종류의 릴리스

릴리스마다 다양한 종류의 변경 내용이 포함되어 있습니다.
이는 릴리스 계획이 릴리스의 종류에 따라 다르다는 것을 의미합니다.

### <a name="patch-releases"></a>패치 릴리스

패치 릴리스는 버전의 “패치” 부분만 변경합니다.
예를 들어 EF Core 3.1.**1**은 EF Core 3.1.**0**에서 발견된 문제를 패치하는 릴리스입니다.

패치 릴리스는 중요한 고객 버그를 수정하기 위한 것입니다.
따라서 패치 릴리스는 새로운 기능을 포함하지 않습니다.
특수한 경우를 제외하고는 패치 릴리스에서 API를 변경할 수 없습니다.

패치 릴리스에서 변경할 막대가 매우 높습니다.
이는 패치 릴리스에 새로운 버그가 도입되지 않아야 하기 때문입니다.
따라서 의사 결정 프로세스는 높은 값과 위험 수준 낮음을 강조합니다.

다음과 같은 경우 문제를 패치할 가능성이 높습니다.
  * 여러 고객에게 영향을 줍니다.
  * 이전 릴리스의 회귀입니다.
  * 오류로 인해 데이터가 손상됩니다.

다음과 같은 경우 문제를 패치할 가능성이 적습니다.
  * 적절한 해결 방법이 있습니다.
  * 문제 해결이 다른 항목을 손상시킬 위험이 있습니다.
  * 코너 케이스에 버그가 있습니다.

이 막대는 [LTS(장기 지원)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) 릴리스의 수명 동안 점진적으로 높아집니다. 이는 LTS 릴리스가 안정성을 강조하기 때문입니다.

문제를 패치할지 여부에 대한 최종 결정은 Microsoft의 .NET Directors에서 이루어집니다.

### <a name="minor-releases"></a>부 버전

부 버전은 버전의 “사소한” 부분만 변경합니다.
예를 들어 EF Core 3.**1**.0은 EF Core 3.**0**.0에서 개선된 릴리스입니다.

부 버전:
* 이전 릴리스의 품질 및 기능을 개선하기 위한 것입니다.
* 일반적으로 버그 수정과 새로운 기능이 포함됩니다.
* 의도적인 주요 변경 내용은 포함하지 않습니다.
* 몇 가지 시험판 미리 보기가 NuGet에 푸시됩니다.

### <a name="major-releases"></a>주 버전

주 버전은 EF “주” 버전 번호를 변경합니다.
예를 들어 EF Core **3**.0.0은 EF Core 2.2.x보다 크게 개선된 주 버전입니다.

주 버전:
* 이전 릴리스의 품질 및 기능을 개선하기 위한 것입니다.
* 일반적으로 버그 수정과 새로운 기능이 포함됩니다.
  * 새 기능 중 일부는 EF Core의 작동 방식에 대한 근본적인 변경일 수 있습니다.
* 일반적으로 의도적인 주요 변경 내용을 포함합니다.
  * 주요 변경 내용은 학습을 통해 진화하는 EF Core의 필수 부분입니다.
  * 그러나 잠재적 고객에게 영향을 줄 수 있으므로 주요 변경에 대해 신중하게 고려해야 합니다. 과거에는 주요 변경에 대해 너무 적극적이었을 수 있습니다. 앞으로는 애플리케이션을 중단하는 변경을 최소화하고 데이터베이스 공급자 및 확장을 중단하는 변경을 줄이도록 노력할 것입니다.
* 많은 시험판 미리 보기가 NuGet에 푸시됩니다.

## <a name="planning-for-majorminor-releases"></a>주/부 버전 계획

### <a name="github-issue-tracking"></a>GitHub 문제 추적

GitHub([https://github.com/dotnet/efcore](https://github.com/dotnet/efcore))는 모든 EF Core 계획을 위한 원본입니다.

GitHub에 대한 문제는 다음과 같습니다.

* 상태
  * [미해결](https://github.com/dotnet/efcore/issues) 문제는 해결되지 않았습니다.
  * [종결된](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed) 문제는 해결되었습니다.
    * 수정된 모든 문제는 닫힌 고정[종결-수정 태그가 지정되었습니다](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed). 종결-수정 태그가 지정된 문제는 해결 및 병합되었지만 릴리스되지 않았을 수 있습니다.
    * 기타 `closed-` 레이블은 문제를 종결하는 다른 이유를 나타냅니다. 예를 들어 중복 항목은 종결-중복 태그가 지정됩니다.
* 형식
  * [버그](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug)는 버그를 나타냅니다.
  * [향상](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement)은 새로운 기능 또는 기존 기능에서 향상된 기능을 나타냅니다.
* 마일스톤
  * [마일스톤이 없는 문제](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone)는 팀에서 고려하고 있습니다. 이 문제와 관련해 수행할 작업에 대한 결정이 아직 이루어지지 않았거나 결정의 변경을 고려하고 있습니다.
  * [백로그 마일스톤의 문제](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog)는 EF 팀이 향후 릴리스에서 작업을 고려할 항목입니다.
    * 백로그의 문제에는 이 작업 항목이 다음 릴리스에 대한 목록의 상위에 있음을 나타내는 [다음 릴리스에 고려 태그](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release)가 지정될 수 있습니다.
  * 버전이 지정된 마일스톤의 미해결 문제는 해당 버전에서 팀이 작업을 계획하는 항목입니다. 예를 들어 [이것은 EF Core 5.0에 대해 작업 수행을 계획하는 문제입니다](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0).
  * 버전 관리 마일스톤에서 종결된 문제는 해당 버전에 대해 완료된 문제입니다. 버전이 아직 릴리스되지 않았을 수 있습니다. 예를 들어 [이것은 EF Core 3.0에 대해 완료된 문제입니다](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed).
* 투표하세요!
  * 투표는 중요한 문제임을 나타내는 최선의 방법입니다.
  * 투표하려면 문제에 “엄지 손가락 들기” 👍를 추가하면 됩니다. 예를 들어 [이것은 가장 많은 표를 받은 문제입니다.](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)
  * 이것으로 값이 추가된다고 판단되는 경우 해당 기능이 필요한 구체적인 이유도 설명해 주세요. “+1” 또는 이와 유사한 댓글을 달면 값이 추가되지 않습니다.

### <a name="the-planning-process"></a>계획 프로세스

계획 프로세스는 백로그에서 가장 요청이 많은 기능을 선택하는 것보다 더 복잡한 과정입니다.
여러 관련자의 피드백을 여러 가지 방법으로 수집하기 때문입니다.
그런 다음에는 다음을 기반으로 릴리스를 구체화합니다.

* 고객의 입력
* 다른 관련자의 입력
* 전략적 방향
* 사용 가능한 리소스
* 일정

몇 가지 질문은 다음과 같습니다.

1. **얼마나 많은 개발자가 이 기능을 사용하고 해당 애플리케이션 또는 환경을 얼마나 더 좋게 만들 수 있을까요?** 이 질문에 답변하기 위해 많은 소스에서 피드백을 수집하며, 이슈에 대한 의견 및 투표는 이러한 소스 중 하나입니다. 또 다른 소스는 중요한 고객과의 구체적인 계약입니다.

2. **이 기능을 아직 구현하지 않은 경우 사람들이 사용할 수 있는 해결 방법은 무엇인가요?** 예를 들어 많은 개발자는 네이티브 다대다 지원 부족의 문제를 해결하기 위해 조인 테이블을 매핑할 수 있습니다. 분명히 일부 개발자는 그렇게 하고 싶어 하지 않지만 많은 개발자가 그렇게 할 수 있으며, 이는 결정의 한 요인으로 고려됩니다.

3. **이 기능의 구현은 다른 기능의 구현으로 가까이 다가가도록 하는 EF Core의 아키텍처를 진화시키나요?** Microsoft는 다른 기능의 구성 요소로 작동하는 기능을 선호하는 경향이 있습니다. 예를 들어 속성 모음 엔터티는 다 대 다 지원을 지향하는 데 도움이 되며, 엔터티 생성자는 지연 로드 지원을 사용하도록 설정했습니다.

4. **기능은 확장성 지점인가요?** 확장성 지점을 통해 개발자는 해당 동작에 연결하고 누락된 기능을 보완할 수 있으므로 Microsoft는 확장성 지점을 선호하는 경향이 있습니다.

5. **다른 제품과 함께 사용할 경우 기능의 시너지 효과는 무엇인가요?** Microsoft는 EF Core를 .NET Core, 최신 버전의 Visual Studio, Microsoft Azure 등과 같은 다른 제품과 함께 사용하도록 하거나 그러한 환경을 크게 향상하는 기능을 선호합니다.

6. **사람들이 기능에 사용할 수 있는 기술은 무엇이며 이러한 리소스를 가장 잘 활용하는 방법은 무엇인가요?** EF 팀의 각 구성원과 커뮤니티 참가자는 서로 다른 영역에서 다양한 수준의 경험이 있으므로 그에 따라 계획해야 합니다. GroupBy 번역 또는 다대다와 같은 특정 기능을 작업하기 위해 “모든 인력을 동원”하려고 해도 이는 현실성이 떨어질 것입니다.
