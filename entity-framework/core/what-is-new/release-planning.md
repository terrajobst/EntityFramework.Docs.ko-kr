---
title: EF Core 릴리스 계획
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/release_planning.md
ms.openlocfilehash: c60040aa4acc33ba8b5a54b619539b275690f70e
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125369"
---
# <a name="release-planning-process"></a><span data-ttu-id="055f2-102">릴리스 계획 프로세스</span><span class="sxs-lookup"><span data-stu-id="055f2-102">Release planning process</span></span>

<span data-ttu-id="055f2-103">특정 릴리스로 들어가는 특정 기능 선택 방법에 대한 질문을 종종 받습니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-103">We often get questions about how we choose specific features to go into a particular release.</span></span>
<span data-ttu-id="055f2-104">확실히 백로그는 릴리스 계획으로 자동으로 변환되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-104">Our backlog certainly doesn't automatically translate into release plans.</span></span>
<span data-ttu-id="055f2-105">EF6에서 기능의 존재 여부도 기능이 EF Core에서 구현되어야 함을 자동으로 의미하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-105">The presence of a feature in EF6 also doesn't automatically mean that the feature needs to be implemented in EF Core.</span></span>

<span data-ttu-id="055f2-106">릴리스를 계획하기 위해 수행하는 전체 프로세스를 자세히 설명하기는 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-106">It's difficult to detail the whole process we follow to plan a release.</span></span>
<span data-ttu-id="055f2-107">대부분의 경우 특정 기능, 기회 및 우선 순위에 대해 이야기하며 프로세스 자체는 모든 릴리스와 함께 진행됩니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-107">Much of it is discussing the specific features, opportunities and priorities, and the process itself also evolves with every release.</span></span>
<span data-ttu-id="055f2-108">그러나 다음에 작업할 항목을 결정할 때 답변하려는 일반적인 질문은 요약할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-108">However, we can summarize the common questions we try to answer when deciding what to work on next:</span></span>

1. <span data-ttu-id="055f2-109">**얼마나 많은 개발자가 이 기능을 사용하고 해당 애플리케이션 또는 환경을 얼마나 더 좋게 만들 수 있을까요?**</span><span class="sxs-lookup"><span data-stu-id="055f2-109">**How many developers we think will use the feature and how much better will it make their applications or experience?**</span></span> <span data-ttu-id="055f2-110">이 질문에 답변하기 위해 많은 소스에서 피드백을 수집하며, 이슈에 대한 의견 및 투표는 이러한 소스 중 하나입니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-110">To answer this question, we collect feedback from many sources — Comments and votes on issues is one of those sources.</span></span>

2. <span data-ttu-id="055f2-111">**이 기능을 아직 구현하지 않은 경우 사람들이 사용할 수 있는 해결 방법은 무엇인가요?**</span><span class="sxs-lookup"><span data-stu-id="055f2-111">**What are the workarounds people can use if we don't implement this feature yet?**</span></span> <span data-ttu-id="055f2-112">예를 들어 많은 개발자는 네이티브 다대다 지원 부족의 문제를 해결하기 위해 조인 테이블을 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-112">For example, many developers can map a join table to work around lack of native many-to-many support.</span></span> <span data-ttu-id="055f2-113">분명히 일부 개발자는 그렇게 하고 싶어 하지 않지만 많은 개발자가 그렇게 할 수 있으며, 이는 결정의 한 요인으로 고려됩니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-113">Obviously, not all developers want to do it, but many can, and that counts as a factor in our decision.</span></span>

3. <span data-ttu-id="055f2-114">**이 기능의 구현은 다른 기능의 구현으로 가까이 다가가도록 하는 EF Core의 아키텍처를 진화시키나요?**</span><span class="sxs-lookup"><span data-stu-id="055f2-114">**Does implementing this feature evolve the architecture of EF Core such that it moves us closer to implementing other features?**</span></span> <span data-ttu-id="055f2-115">Microsoft는 다른 기능의 구성 요소로 작동하는 기능을 선호하는 경향이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-115">We tend to favor features that act as building blocks for other features.</span></span> <span data-ttu-id="055f2-116">예를 들어 속성 모음 엔터티는 다 대 다 지원을 지향하는 데 도움이 되며, 엔터티 생성자는 지연 로드 지원을 사용하도록 설정했습니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-116">For instance, property bag entities can help us move towards many-to-many support, and entity constructors enabled our lazy loading support.</span></span>

4. <span data-ttu-id="055f2-117">**기능은 확장성 지점인가요?**</span><span class="sxs-lookup"><span data-stu-id="055f2-117">**Is the feature an extensibility point?**</span></span> <span data-ttu-id="055f2-118">확장성 지점을 통해 개발자는 해당 동작에 연결하고 누락된 기능을 보완할 수 있으므로 Microsoft는 확장성 지점을 선호하는 경향이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-118">We tend to favor extensibility points over normal features because they enable developers to hook their own behaviors and compensate for any missing functionality.</span></span>

5. <span data-ttu-id="055f2-119">**다른 제품과 함께 사용할 경우 기능의 시너지 효과는 무엇인가요?**</span><span class="sxs-lookup"><span data-stu-id="055f2-119">**What is the synergy of the feature when used in combination with other products?**</span></span> <span data-ttu-id="055f2-120">Microsoft는 EF Core를 .NET Core, 최신 버전의 Visual Studio, Microsoft Azure 등과 같은 다른 제품과 함께 사용하도록 하거나 그러한 환경을 크게 향상하는 기능을 선호합니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-120">We favor features that enable or significantly improve the experience of using EF Core with other products, such as .NET Core, the latest version of Visual Studio, Microsoft Azure, and so on.</span></span>

6. <span data-ttu-id="055f2-121">**사람들이 기능에 사용할 수 있는 기술은 무엇이며 이러한 리소스를 가장 잘 활용하는 방법은 무엇인가요?**</span><span class="sxs-lookup"><span data-stu-id="055f2-121">**What are the skills of the people available to work on a feature, and how to best leverage these resources?**</span></span> <span data-ttu-id="055f2-122">EF 팀의 각 구성원과 커뮤니티 참가자는 서로 다른 영역에서 다양한 수준의 경험이 있으므로 그에 따라 계획해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-122">Each member of the EF team and our community contributors has different levels of experience in different areas, so we have to plan accordingly.</span></span> <span data-ttu-id="055f2-123">GroupBy 번역 또는 다대다와 같은 특정 기능을 작업하기 위해 “모든 인력을 동원”하려고 해도 이는 현실성이 떨어질 것입니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-123">Even if we wanted to have "all hands on deck" to work on a specific feature like GroupBy translations, or many-to-many, that wouldn't be practical.</span></span>

<span data-ttu-id="055f2-124">이전에 언급한 것처럼 프로세스는 모든 릴리스에서 진행됩니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-124">As mentioned before, the process evolves on every release.</span></span>
<span data-ttu-id="055f2-125">향후 커뮤니티 구성원이 릴리스 계획에 대한 의견을 제공하는 기회를 더 많이 추가하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-125">In the future, we'll try to add more opportunities for members of the community to provide inputs into our release plans.</span></span>
<span data-ttu-id="055f2-126">예를 들어 기능과 릴리스 계획 자체의 초안 디자인을 더 쉽게 검토할 수 있도록 하고 싶습니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-126">For example, we would like to make it easier to review draft designs of the features and of the release plan itself.</span></span>

## <a name="backlog"></a><span data-ttu-id="055f2-127">백로그</span><span class="sxs-lookup"><span data-stu-id="055f2-127">Backlog</span></span>

<span data-ttu-id="055f2-128">이슈 추적기의 [백로그 마일스톤](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc)에는 언젠가 작업할 것으로 예상되거나 커뮤니티의 누군가가 해결할 수 있을 것으로 생각하는 문제가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-128">The [Backlog Milestone](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) in our issue tracker contains issues that we either expect to work on someday, or we think someone from the community could tackle.</span></span>
<span data-ttu-id="055f2-129">고객은 이러한 문제에 대해 의견을 제출하고 투표할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-129">Customers are welcome to submit comments and votes on these issues.</span></span>
<span data-ttu-id="055f2-130">이러한 문제를 작업하려는 참가자는 문제에 대한 접근 방법을 토론하는 것부터 시작하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-130">Contributors looking to work on any of these issues are encouraged to first start a discussion on how to approach them.</span></span>

<span data-ttu-id="055f2-131">특정 버전의 EF Core에서 제공된 기능을 다룰 것이라는 보장은 없습니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-131">There's never a guarantee that we'll work on any given feature in a specific version of EF Core.</span></span>
<span data-ttu-id="055f2-132">모든 소프트웨어 프로젝트, 우선 순위, 릴리스 일정 및 사용 가능한 리소스는 언제든지 변경될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-132">As in all software projects, priorities, release schedules, and available resources can change at any point.</span></span>
<span data-ttu-id="055f2-133">그러나 특정 시기에 문제를 해결하려는 경우 백로그 마일스톤이 아닌 릴리스 마일스톤에 해당 문제를 할당합니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-133">But if we intend to resolve an issue in a specific time-frame, we'll assign it to a release milestone instead of the backlog milestone.</span></span>

<span data-ttu-id="055f2-134">문제를 해결할 계획이 없는 경우 문제를 종결할 가능성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-134">We'll likely close an issue if we don't plan to ever address it.</span></span>
<span data-ttu-id="055f2-135">그러나 새로운 정보가 있는 경우 이전에 종결한 문제를 다시 고려할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="055f2-135">But we can reconsider an issue that we previously closed if we get new information about it.</span></span>
