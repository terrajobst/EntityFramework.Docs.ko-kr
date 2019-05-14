---
title: Entity Framework Core 로드맵
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: 937bfff4cbc3ec0b2235a78cb2f1f246697128d5
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929786"
---
# <a name="entity-framework-core-roadmap"></a><span data-ttu-id="1247a-102">Entity Framework Core 로드맵</span><span class="sxs-lookup"><span data-stu-id="1247a-102">Entity Framework Core Roadmap</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1247a-103">이후 릴리스의 기능 집합 및 일정은 항상 변경될 수 있으며 이 페이지를 최신 상태로 유지하려고 노력함에도 불구하고 최신 계획을 항상 반영할 수 없을 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

## <a name="ef-core-30"></a><span data-ttu-id="1247a-104">EF Core 3.0</span><span class="sxs-lookup"><span data-stu-id="1247a-104">EF Core 3.0</span></span>

<span data-ttu-id="1247a-105">EF Core 2.2가 출시됨에 따라 이제 EF Core 3.0이 주요 관심사입니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-105">With EF Core 2.2 out the door, our main focus is now EF Core 3.0.</span></span>
<span data-ttu-id="1247a-106">이 릴리스에 포함되는 계획된 [새로운 기능](xref:core/what-is-new/ef-core-3.0/features) 및 의도적인 [호환성이 손상되는 변경](xref:core/what-is-new/ef-core-3.0/breaking-changes)에 대한 정보는 [EF Core 3.0의 새로운 기능](xref:core/what-is-new/ef-core-3.0/index)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="1247a-106">See [What's new in EF Core 3.0](xref:core/what-is-new/ef-core-3.0/index) for information on planned [new features](xref:core/what-is-new/ef-core-3.0/features) and intentional [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes) included in this release.</span></span>

## <a name="schedule"></a><span data-ttu-id="1247a-107">일정</span><span class="sxs-lookup"><span data-stu-id="1247a-107">Schedule</span></span>

<span data-ttu-id="1247a-108">EF Core의 릴리스 일정은 [.NET Core 릴리스 일정](https://github.com/dotnet/core/blob/master/roadmap.md)과 동기화됩니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-108">The release schedule for EF Core is in-sync with the [.NET Core release schedule](https://github.com/dotnet/core/blob/master/roadmap.md).</span></span>

## <a name="backlog"></a><span data-ttu-id="1247a-109">백로그</span><span class="sxs-lookup"><span data-stu-id="1247a-109">Backlog</span></span>

<span data-ttu-id="1247a-110">이슈 추적기의 [백로그 마일스톤](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc)에는 언젠가 작업할 것으로 예상되거나 커뮤니티의 누군가가 해결할 수 있을 것으로 생각하는 문제가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-110">The [Backlog Milestone](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) in our issue tracker contains issues that we either expect to work on someday, or we think someone from the community could tackle.</span></span>
<span data-ttu-id="1247a-111">고객은 이러한 문제에 대해 의견을 제출하고 투표할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-111">Customers are welcome to submit comments and votes on these issues.</span></span>
<span data-ttu-id="1247a-112">이러한 문제를 작업하려는 참가자는 문제에 대한 접근 방법을 토론하는 것부터 시작하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-112">Contributors looking to work on any of these issues are encouraged to first start a discussion on how to approach them.</span></span>

<span data-ttu-id="1247a-113">특정 버전의 EF Core에서 제공된 기능을 다룰 것이라는 보장은 없습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-113">There's never a guarantee that we'll work on any given feature in a specific version of EF Core.</span></span>
<span data-ttu-id="1247a-114">모든 소프트웨어 프로젝트, 우선 순위, 릴리스 일정 및 사용 가능한 리소스는 언제든지 변경될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-114">As in all software projects, priorities, release schedules, and available resources can change at any point.</span></span>
<span data-ttu-id="1247a-115">그러나 특정 시기에 문제를 해결하려는 경우 백로그 마일스톤이 아닌 릴리스 마일스톤에 해당 문제를 할당합니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-115">But if we intend to resolve an issue in a specific timeframe, we'll assign it to a release milestone instead of the backlog milestone.</span></span>
<span data-ttu-id="1247a-116">Microsoft는 [릴리스 계획 프로세스](#release-planning-process)의 일부로써 백로그 마일스톤과 릴리스 마일스톤 간에 일상적으로 문제를 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-116">We routinely move issues between the backlog and release milestones as part of our [release planning process](#release-planning-process).</span></span>

<span data-ttu-id="1247a-117">문제를 해결할 계획이 없는 경우 문제를 종결할 가능성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-117">We'll likely close an issue if we don't plan to ever address it.</span></span>
<span data-ttu-id="1247a-118">그러나 새로운 정보가 있는 경우 이전에 종결한 문제를 다시 고려할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-118">But we can reconsider an issue that we previously closed if we get new information about it.</span></span>

## <a name="release-planning-process"></a><span data-ttu-id="1247a-119">릴리스 계획 프로세스</span><span class="sxs-lookup"><span data-stu-id="1247a-119">Release planning process</span></span>

<span data-ttu-id="1247a-120">특정 릴리스로 들어가는 특정 기능 선택 방법에 대한 질문을 종종 받습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-120">We often get questions about how we choose specific features to go into a particular release.</span></span>
<span data-ttu-id="1247a-121">확실히 백로그는 릴리스 계획으로 자동으로 변환되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-121">Our backlog certainly doesn't automatically translate into release plans.</span></span>
<span data-ttu-id="1247a-122">EF6에서 기능의 존재 여부도 기능이 EF Core에서 구현되어야 함을 자동으로 의미하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-122">The presence of a feature in EF6 also doesn't automatically mean that the feature needs to be implemented in EF Core.</span></span>

<span data-ttu-id="1247a-123">릴리스를 계획하기 위해 수행하는 전체 프로세스를 자세히 설명하기는 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-123">It's difficult to detail the whole process we follow to plan a release.</span></span>
<span data-ttu-id="1247a-124">대부분의 경우 특정 기능, 기회 및 우선 순위에 대해 이야기하며 프로세스 자체는 모든 릴리스와 함께 진행됩니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-124">Much of it is discussing the specific features, opportunities and priorities, and the process itself also evolves with every release.</span></span>
<span data-ttu-id="1247a-125">그러나 다음에 작업할 항목을 결정할 때 답변하려는 일반적인 질문은 요약할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-125">However, we can summarize the common questions we try to answer when deciding what to work on next:</span></span>

1. <span data-ttu-id="1247a-126">**얼마나 많은 개발자가 이 기능을 사용하고 해당 애플리케이션 또는 환경을 얼마나 더 좋게 만들 수 있을까요?**</span><span class="sxs-lookup"><span data-stu-id="1247a-126">**How many developers we think will use the feature and how much better will it make their applications or experience?**</span></span> <span data-ttu-id="1247a-127">이 질문에 답변하기 위해 많은 소스에서 피드백을 수집하며, 이슈에 대한 의견 및 투표는 이러한 소스 중 하나입니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-127">To answer this question, we collect feedback from many sources — Comments and votes on issues is one of those sources.</span></span>

2. <span data-ttu-id="1247a-128">**이 기능을 아직 구현하지 않은 경우 사람들이 사용할 수 있는 해결 방법은 무엇인가요?**</span><span class="sxs-lookup"><span data-stu-id="1247a-128">**What are the workarounds people can use if we don't implement this feature yet?**</span></span> <span data-ttu-id="1247a-129">예를 들어 많은 개발자는 네이티브 다대다 지원 부족의 문제를 해결하기 위해 조인 테이블을 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-129">For example, many developers can map a join table to work around lack of native many-to-many support.</span></span> <span data-ttu-id="1247a-130">분명히 일부 개발자는 그렇게 하고 싶어 하지 않지만 많은 개발자가 그렇게 할 수 있으며, 이는 결정의 한 요인으로 고려됩니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-130">Obviously, not all developers want to do it, but many can, and that counts as a factor in our decision.</span></span>

3. <span data-ttu-id="1247a-131">**이 기능의 구현은 다른 기능의 구현으로 가까이 다가가도록 하는 EF Core의 아키텍처를 진화시키나요?**</span><span class="sxs-lookup"><span data-stu-id="1247a-131">**Does implementing this feature evolve the architecture of EF Core such that it moves us closer to implementing other features?**</span></span> <span data-ttu-id="1247a-132">Microsoft는 다른 기능의 구성 요소로 작동하는 기능을 선호하는 경향이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-132">We tend to favor features that act as building blocks for other features.</span></span> <span data-ttu-id="1247a-133">예를 들어 속성 모음 엔터티는 다 대 다 지원을 지향하는 데 도움이 되며, 엔터티 생성자는 지연 로드 지원을 사용하도록 설정했습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-133">For instance, property bag entities can help us move towards many-to-many support, and entity constructors enabled our lazy loading support.</span></span>

4. <span data-ttu-id="1247a-134">**기능은 확장성 지점인가요?**</span><span class="sxs-lookup"><span data-stu-id="1247a-134">**Is the feature an extensibility point?**</span></span> <span data-ttu-id="1247a-135">확장성 지점을 통해 개발자는 해당 동작에 연결하고 누락된 기능을 보완할 수 있으므로 Microsoft는 확장성 지점을 선호하는 경향이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-135">We tend to favor extensibility points over normal features because they enable developers to hook their own behaviors and compensate for any missing functionality.</span></span>

5. <span data-ttu-id="1247a-136">**다른 제품과 함께 사용할 경우 기능의 시너지 효과는 무엇인가요?**</span><span class="sxs-lookup"><span data-stu-id="1247a-136">**What is the synergy of the feature when used in combination with other products?**</span></span> <span data-ttu-id="1247a-137">Microsoft는 EF Core를 .NET Core, 최신 버전의 Visual Studio, Microsoft Azure 등과 같은 다른 제품과 함께 사용하도록 하거나 그러한 환경을 크게 향상하는 기능을 선호합니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-137">We favor features that enable or significantly improve the experience of using EF Core with other products, such as .NET Core, the latest version of Visual Studio, Microsoft Azure, and so on.</span></span>

6. <span data-ttu-id="1247a-138">**사람들이 기능에 사용할 수 있는 기술은 무엇이며 이러한 리소스를 가장 잘 활용하는 방법은 무엇인가요?**</span><span class="sxs-lookup"><span data-stu-id="1247a-138">**What are the skills of the people available to work on a feature, and how to best leverage these resources?**</span></span> <span data-ttu-id="1247a-139">EF 팀의 각 구성원과 커뮤니티 참가자는 서로 다른 영역에서 다양한 수준의 경험이 있으므로 그에 따라 계획해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-139">Each member of the EF team and our community contributors has different levels of experience in different areas, so we have to plan accordingly.</span></span> <span data-ttu-id="1247a-140">GroupBy 번역 또는 다대다와 같은 특정 기능을 작업하기 위해 “모든 인력을 동원”하려고 해도 이는 현실성이 떨어질 것입니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-140">Even if we wanted to have "all hands on deck" to work on a specific feature like GroupBy translations, or many-to-many, that wouldn't be practical.</span></span>

<span data-ttu-id="1247a-141">이전에 언급한 것처럼 프로세스는 모든 릴리스에서 진행됩니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-141">As mentioned before, the process evolves on every release.</span></span>
<span data-ttu-id="1247a-142">향후 커뮤니티 구성원이 릴리스 계획에 대한 의견을 제공하는 기회를 더 많이 추가하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-142">In the future, we'll try to add more opportunities for members of the community to provide inputs into our release plans.</span></span>
<span data-ttu-id="1247a-143">예를 들어 기능과 릴리스 계획 자체의 초안 디자인을 더 쉽게 검토할 수 있도록 하고 싶습니다.</span><span class="sxs-lookup"><span data-stu-id="1247a-143">For example, we would like to make it easier to review draft designs of the features and of the release plan itself.</span></span>
