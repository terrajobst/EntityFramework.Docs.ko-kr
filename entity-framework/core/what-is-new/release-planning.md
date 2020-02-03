---
title: EF Core 릴리스 계획
author: ajcvickers
ms.date: 01/28/2020
uid: core/what-is-new/release_planning.md
ms.openlocfilehash: 71045b8d49c319a73f74443612bedd84ee33ab8a
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888061"
---
# <a name="release-planning-process"></a><span data-ttu-id="bbb7b-102">릴리스 계획 프로세스</span><span class="sxs-lookup"><span data-stu-id="bbb7b-102">Release planning process</span></span>

<span data-ttu-id="bbb7b-103">특정 릴리스로 들어가는 특정 기능 선택 방법에 대한 질문을 종종 받습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-103">We often get questions about how we choose specific features to go into a particular release.</span></span>
<span data-ttu-id="bbb7b-104">이 문서에서는 사용하는 프로세스에 대해 간략하게 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-104">This document outlines the process we use.</span></span>
<span data-ttu-id="bbb7b-105">이 프로세스는 더 나은 계획 방법을 찾으면서 지속적으로 발전하고 있지만 일반적인 아이디어는 동일하게 유지됩니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-105">The process is continually evolving as we find better ways to plan, but the general ideas remain the same.</span></span>

## <a name="different-kinds-of-releases"></a><span data-ttu-id="bbb7b-106">다양한 종류의 릴리스</span><span class="sxs-lookup"><span data-stu-id="bbb7b-106">Different kinds of releases</span></span>

<span data-ttu-id="bbb7b-107">릴리스마다 다양한 종류의 변경 내용이 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-107">Different kinds of release contain different kinds of changes.</span></span>
<span data-ttu-id="bbb7b-108">이는 릴리스 계획이 릴리스의 종류에 따라 다르다는 것을 의미합니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-108">This in turn means that the release planning is different for different kinds of release.</span></span>

### <a name="patch-releases"></a><span data-ttu-id="bbb7b-109">패치 릴리스</span><span class="sxs-lookup"><span data-stu-id="bbb7b-109">Patch releases</span></span>

<span data-ttu-id="bbb7b-110">패치 릴리스는 버전의 “패치” 부분만 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-110">Patch releases change only the "patch" part of the version.</span></span>
<span data-ttu-id="bbb7b-111">예를 들어 EF Core 3.1.**1**은 EF Core 3.1.**0**에서 발견된 문제를 패치하는 릴리스입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-111">For example, EF Core 3.1.**1** is a release that patches issues found in EF Core 3.1.**0**.</span></span>

<span data-ttu-id="bbb7b-112">패치 릴리스는 중요한 고객 버그를 수정하기 위한 것입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-112">Patch releases are intended to fix critical customer bugs.</span></span>
<span data-ttu-id="bbb7b-113">따라서 패치 릴리스는 새로운 기능을 포함하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-113">This means patch releases do not include new features.</span></span>
<span data-ttu-id="bbb7b-114">특수한 경우를 제외하고는 패치 릴리스에서 API를 변경할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-114">API changes are not allowed in patch releases except in special circumstances.</span></span>

<span data-ttu-id="bbb7b-115">패치 릴리스에서 변경할 막대가 매우 높습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-115">The bar to make a change in a patch release is very high.</span></span>
<span data-ttu-id="bbb7b-116">이는 패치 릴리스에 새로운 버그가 도입되지 않아야 하기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-116">This is because it is critical that patch releases do not introduce new bugs.</span></span>
<span data-ttu-id="bbb7b-117">따라서 의사 결정 프로세스는 높은 값과 위험 수준 낮음을 강조합니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-117">Therefore, the decision process emphasizes high value and low risk.</span></span>

<span data-ttu-id="bbb7b-118">다음과 같은 경우 문제를 패치할 가능성이 높습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-118">We are more likely to patch an issue if:</span></span>
  * <span data-ttu-id="bbb7b-119">여러 고객에게 영향을 줍니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-119">It is impacting multiple customers</span></span>
  * <span data-ttu-id="bbb7b-120">이전 릴리스의 회귀입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-120">It is a regression from a previous release</span></span>
  * <span data-ttu-id="bbb7b-121">오류로 인해 데이터가 손상됩니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-121">The failure causes data corruption</span></span>

<span data-ttu-id="bbb7b-122">다음과 같은 경우 문제를 패치할 가능성이 적습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-122">We are less likely to patch an issue if:</span></span>
  * <span data-ttu-id="bbb7b-123">적절한 해결 방법이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-123">There are reasonable workarounds</span></span>
  * <span data-ttu-id="bbb7b-124">문제 해결이 다른 항목을 손상시킬 위험이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-124">The fix has high risk of breaking something else</span></span>
  * <span data-ttu-id="bbb7b-125">코너 케이스에 버그가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-125">The bug is in a corner-case</span></span>

<span data-ttu-id="bbb7b-126">이 막대는 [LTS(장기 지원)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) 릴리스의 수명 동안 점진적으로 높아집니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-126">This bar gradually rises through the lifetime of a [long-term support (LTS)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) release.</span></span> <span data-ttu-id="bbb7b-127">이는 LTS 릴리스가 안정성을 강조하기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-127">This is because LTS releases emphasize stability.</span></span>

<span data-ttu-id="bbb7b-128">문제를 패치할지 여부에 대한 최종 결정은 Microsoft의 .NET Directors에서 이루어집니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-128">The final decision about whether or not to patch an issue is made by the .NET Directors at Microsoft.</span></span>

### <a name="minor-releases"></a><span data-ttu-id="bbb7b-129">부 버전</span><span class="sxs-lookup"><span data-stu-id="bbb7b-129">Minor releases</span></span>

<span data-ttu-id="bbb7b-130">부 버전은 버전의 “사소한” 부분만 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-130">Minor releases change only the "minor" part of the version.</span></span>
<span data-ttu-id="bbb7b-131">예를 들어 EF Core 3.**1**.0은 EF Core 3.**0**.0에서 개선된 릴리스입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-131">For example, EF Core 3.**1**.0 is a release that improves on EF Core 3.**0**.0.</span></span>

<span data-ttu-id="bbb7b-132">부 버전:</span><span class="sxs-lookup"><span data-stu-id="bbb7b-132">Minor releases:</span></span>
* <span data-ttu-id="bbb7b-133">이전 릴리스의 품질 및 기능을 개선하기 위한 것입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-133">Are intended to improve the quality and features of the previous release</span></span>
* <span data-ttu-id="bbb7b-134">일반적으로 버그 수정과 새로운 기능이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-134">Typically contain bug fixes and new features</span></span>
* <span data-ttu-id="bbb7b-135">의도적인 주요 변경 내용은 포함하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-135">Do not include intentional breaking changes</span></span>
* <span data-ttu-id="bbb7b-136">몇 가지 시험판 미리 보기가 NuGet에 푸시됩니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-136">Have a few prerelease previews pushed to NuGet</span></span>

### <a name="major-releases"></a><span data-ttu-id="bbb7b-137">주 버전</span><span class="sxs-lookup"><span data-stu-id="bbb7b-137">Major releases</span></span>

<span data-ttu-id="bbb7b-138">주 버전은 EF “주” 버전 번호를 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-138">Major releases change the EF "major" version number.</span></span>
<span data-ttu-id="bbb7b-139">예를 들어 EF Core **3**.0.0은 EF Core 2.2.x보다 크게 개선된 주 버전입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-139">For example, EF Core **3**.0.0 is a major release that makes a big step forward over EF Core 2.2.x.</span></span>

<span data-ttu-id="bbb7b-140">주 버전:</span><span class="sxs-lookup"><span data-stu-id="bbb7b-140">Major releases:</span></span>
* <span data-ttu-id="bbb7b-141">이전 릴리스의 품질 및 기능을 개선하기 위한 것입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-141">Are intended to improve the quality and features of the previous release</span></span>
* <span data-ttu-id="bbb7b-142">일반적으로 버그 수정과 새로운 기능이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-142">Typically contain bug fixes and new features</span></span>
  * <span data-ttu-id="bbb7b-143">새 기능 중 일부는 EF Core의 작동 방식에 대한 근본적인 변경일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-143">Some of the new features may be fundamental changes to the way EF Core works</span></span>
* <span data-ttu-id="bbb7b-144">일반적으로 의도적인 주요 변경 내용을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-144">Typically include intentional breaking changes</span></span>
  * <span data-ttu-id="bbb7b-145">주요 변경 내용은 학습을 통해 진화하는 EF Core의 필수 부분입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-145">Breaking changes are necessary part of evolving EF Core as we learn</span></span>
  * <span data-ttu-id="bbb7b-146">그러나 잠재적 고객에게 영향을 줄 수 있으므로 주요 변경에 대해 신중하게 고려해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-146">However, we think very carefully about making any breaking change because of the potential customer impact.</span></span> <span data-ttu-id="bbb7b-147">과거에는 주요 변경에 대해 너무 적극적이었을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-147">We may have been too aggressive with breaking changes in the past.</span></span> <span data-ttu-id="bbb7b-148">앞으로는 애플리케이션을 중단하는 변경을 최소화하고 데이터베이스 공급자 및 확장을 중단하는 변경을 줄이도록 노력할 것입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-148">Going forward, we will strive to minimize changes that break applications, and to reduce changes that break database providers and extensions.</span></span>
* <span data-ttu-id="bbb7b-149">많은 시험판 미리 보기가 NuGet에 푸시됩니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-149">Have many prerelease previews pushed to NuGet</span></span>

## <a name="planning-for-majorminor-releases"></a><span data-ttu-id="bbb7b-150">주/부 버전 계획</span><span class="sxs-lookup"><span data-stu-id="bbb7b-150">Planning for major/minor releases</span></span>

### <a name="github-issue-tracking"></a><span data-ttu-id="bbb7b-151">GitHub 문제 추적</span><span class="sxs-lookup"><span data-stu-id="bbb7b-151">GitHub issue tracking</span></span>

<span data-ttu-id="bbb7b-152">GitHub([https://github.com/dotnet/efcore](https://github.com/dotnet/efcore))는 모든 EF Core 계획을 위한 원본입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-152">GitHub ([https://github.com/dotnet/efcore](https://github.com/dotnet/efcore)) is the source-of-truth for all EF Core planning.</span></span>

<span data-ttu-id="bbb7b-153">GitHub에 대한 문제는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-153">Issues on GitHub have:</span></span>

* <span data-ttu-id="bbb7b-154">상태</span><span class="sxs-lookup"><span data-stu-id="bbb7b-154">A state</span></span>
  * <span data-ttu-id="bbb7b-155">[미해결](https://github.com/dotnet/efcore/issues) 문제는 해결되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-155">[Open](https://github.com/dotnet/efcore/issues) issues have not been addressed.</span></span>
  * <span data-ttu-id="bbb7b-156">[종결된](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed) 문제는 해결되었습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-156">[Closed](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed) issues have been addressed.</span></span>
    * <span data-ttu-id="bbb7b-157">수정된 모든 문제는 닫힌 고정[종결-수정 태그가 지정되었습니다](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed).</span><span class="sxs-lookup"><span data-stu-id="bbb7b-157">All issues that have been fixed are [tagged with closed-fixed](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed).</span></span> <span data-ttu-id="bbb7b-158">종결-수정 태그가 지정된 문제는 해결 및 병합되었지만 릴리스되지 않았을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-158">An issue tagged with closed-fixed is fixed and merged, but may not have been released.</span></span>
    * <span data-ttu-id="bbb7b-159">기타 `closed-` 레이블은 문제를 종결하는 다른 이유를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-159">Other `closed-` labels indicate other reasons for closing an issue.</span></span> <span data-ttu-id="bbb7b-160">예를 들어 중복 항목은 종결-중복 태그가 지정됩니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-160">For example, duplicates are tagged with closed-duplicate.</span></span>
* <span data-ttu-id="bbb7b-161">형식</span><span class="sxs-lookup"><span data-stu-id="bbb7b-161">A type</span></span>
  * <span data-ttu-id="bbb7b-162">[버그](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug)는 버그를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-162">[Bugs](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug) represent bugs.</span></span>
  * <span data-ttu-id="bbb7b-163">[향상](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement)은 새로운 기능 또는 기존 기능에서 향상된 기능을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-163">[Enhancements](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement) represent new features or better functionality in existing features.</span></span>
* <span data-ttu-id="bbb7b-164">마일스톤</span><span class="sxs-lookup"><span data-stu-id="bbb7b-164">A milestone</span></span>
  * <span data-ttu-id="bbb7b-165">[마일스톤이 없는 문제](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone)는 팀에서 고려하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-165">[Issues with no milestone](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone) are being considered by the team.</span></span> <span data-ttu-id="bbb7b-166">이 문제와 관련해 수행할 작업에 대한 결정이 아직 이루어지지 않았거나 결정의 변경을 고려하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-166">The decision on what to do with the issue has not yet been made or a change in the decision is being considered.</span></span>
  * <span data-ttu-id="bbb7b-167">[백로그 마일스톤의 문제](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog)는 EF 팀이 향후 릴리스에서 작업을 고려할 항목입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-167">[Issues in the Backlog milestone](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog) are items that the EF team will consider working on in a future release</span></span>
    * <span data-ttu-id="bbb7b-168">백로그의 문제에는 이 작업 항목이 다음 릴리스에 대한 목록의 상위에 있음을 나타내는 [다음 릴리스에 고려 태그](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release)가 지정될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-168">Issues in the backlog may be [tagged with consider-for-next-release](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release) indicating that this work item is high on the list for the next release.</span></span>
  * <span data-ttu-id="bbb7b-169">버전이 지정된 마일스톤의 미해결 문제는 해당 버전에서 팀이 작업을 계획하는 항목입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-169">Open issues in a versioned milestone are items that the team plans to work on in that version.</span></span> <span data-ttu-id="bbb7b-170">예를 들어 [이것은 EF Core 5.0에 대해 작업 수행을 계획하는 문제입니다](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0).</span><span class="sxs-lookup"><span data-stu-id="bbb7b-170">For example, [these are the issues we plan to work on for EF Core 5.0](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0).</span></span>
  * <span data-ttu-id="bbb7b-171">버전 관리 마일스톤에서 종결된 문제는 해당 버전에 대해 완료된 문제입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-171">Closed issues in a versioned milestone are issues that are completed for that version.</span></span> <span data-ttu-id="bbb7b-172">버전이 아직 릴리스되지 않았을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-172">Note that the version may not yet have been released.</span></span> <span data-ttu-id="bbb7b-173">예를 들어 [이것은 EF Core 3.0에 대해 완료된 문제입니다](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed).</span><span class="sxs-lookup"><span data-stu-id="bbb7b-173">For example, [these are the issues completed for EF Core 3.0](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed).</span></span>
* <span data-ttu-id="bbb7b-174">투표하세요!</span><span class="sxs-lookup"><span data-stu-id="bbb7b-174">Votes!</span></span>
  * <span data-ttu-id="bbb7b-175">투표는 중요한 문제임을 나타내는 최선의 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-175">Voting is the best way to indicate that an issue is important to you.</span></span>
  * <span data-ttu-id="bbb7b-176">투표하려면 문제에 “엄지 손가락 들기” 👍를 추가하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-176">To vote, just add a "thumbs-up" 👍 to the issue.</span></span> <span data-ttu-id="bbb7b-177">예를 들어 [이것은 가장 많은 표를 받은 문제입니다.](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)</span><span class="sxs-lookup"><span data-stu-id="bbb7b-177">For example, [these are the top-voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)</span></span>
  * <span data-ttu-id="bbb7b-178">이것으로 값이 추가된다고 판단되는 경우 해당 기능이 필요한 구체적인 이유도 설명해 주세요.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-178">Please also comment describing specific reasons you need the feature if you feel this adds value.</span></span> <span data-ttu-id="bbb7b-179">“+1” 또는 이와 유사한 댓글을 달면 값이 추가되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-179">Commenting "+1" or similar does not add value.</span></span>

### <a name="the-planning-process"></a><span data-ttu-id="bbb7b-180">계획 프로세스</span><span class="sxs-lookup"><span data-stu-id="bbb7b-180">The planning process</span></span>

<span data-ttu-id="bbb7b-181">계획 프로세스는 백로그에서 가장 요청이 많은 기능을 선택하는 것보다 더 복잡한 과정입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-181">The planning process is more involved than just taking the top-most requested features from the backlog.</span></span>
<span data-ttu-id="bbb7b-182">여러 관련자의 피드백을 여러 가지 방법으로 수집하기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-182">This is because we gather feedback from multiple stakeholders in multiple ways.</span></span>
<span data-ttu-id="bbb7b-183">그런 다음에는 다음을 기반으로 릴리스를 구체화합니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-183">We then shape a release based on:</span></span>

* <span data-ttu-id="bbb7b-184">고객의 입력</span><span class="sxs-lookup"><span data-stu-id="bbb7b-184">Input from customers</span></span>
* <span data-ttu-id="bbb7b-185">다른 관련자의 입력</span><span class="sxs-lookup"><span data-stu-id="bbb7b-185">Input from other stakeholders</span></span>
* <span data-ttu-id="bbb7b-186">전략적 방향</span><span class="sxs-lookup"><span data-stu-id="bbb7b-186">Strategic direction</span></span>
* <span data-ttu-id="bbb7b-187">사용 가능한 리소스</span><span class="sxs-lookup"><span data-stu-id="bbb7b-187">Resources available</span></span>
* <span data-ttu-id="bbb7b-188">일정</span><span class="sxs-lookup"><span data-stu-id="bbb7b-188">Schedule</span></span>

<span data-ttu-id="bbb7b-189">몇 가지 질문은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-189">Some of the questions we ask are:</span></span>

1. <span data-ttu-id="bbb7b-190">**얼마나 많은 개발자가 이 기능을 사용하고 해당 애플리케이션 또는 환경을 얼마나 더 좋게 만들 수 있을까요?**</span><span class="sxs-lookup"><span data-stu-id="bbb7b-190">**How many developers we think will use the feature and how much better will it make their applications or experience?**</span></span> <span data-ttu-id="bbb7b-191">이 질문에 답변하기 위해 많은 소스에서 피드백을 수집하며, 이슈에 대한 의견 및 투표는 이러한 소스 중 하나입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-191">To answer this question, we collect feedback from many sources — Comments and votes on issues is one of those sources.</span></span> <span data-ttu-id="bbb7b-192">또 다른 소스는 중요한 고객과의 구체적인 계약입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-192">Specific engagements with important customers is another.</span></span>

2. <span data-ttu-id="bbb7b-193">**이 기능을 아직 구현하지 않은 경우 사람들이 사용할 수 있는 해결 방법은 무엇인가요?**</span><span class="sxs-lookup"><span data-stu-id="bbb7b-193">**What are the workarounds people can use if we don't implement this feature yet?**</span></span> <span data-ttu-id="bbb7b-194">예를 들어 많은 개발자는 네이티브 다대다 지원 부족의 문제를 해결하기 위해 조인 테이블을 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-194">For example, many developers can map a join table to work around lack of native many-to-many support.</span></span> <span data-ttu-id="bbb7b-195">분명히 일부 개발자는 그렇게 하고 싶어 하지 않지만 많은 개발자가 그렇게 할 수 있으며, 이는 결정의 한 요인으로 고려됩니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-195">Obviously, not all developers want to do it, but many can, and that counts as a factor in our decision.</span></span>

3. <span data-ttu-id="bbb7b-196">**이 기능의 구현은 다른 기능의 구현으로 가까이 다가가도록 하는 EF Core의 아키텍처를 진화시키나요?**</span><span class="sxs-lookup"><span data-stu-id="bbb7b-196">**Does implementing this feature evolve the architecture of EF Core such that it moves us closer to implementing other features?**</span></span> <span data-ttu-id="bbb7b-197">Microsoft는 다른 기능의 구성 요소로 작동하는 기능을 선호하는 경향이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-197">We tend to favor features that act as building blocks for other features.</span></span> <span data-ttu-id="bbb7b-198">예를 들어 속성 모음 엔터티는 다 대 다 지원을 지향하는 데 도움이 되며, 엔터티 생성자는 지연 로드 지원을 사용하도록 설정했습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-198">For instance, property bag entities can help us move towards many-to-many support, and entity constructors enabled our lazy loading support.</span></span>

4. <span data-ttu-id="bbb7b-199">**기능은 확장성 지점인가요?**</span><span class="sxs-lookup"><span data-stu-id="bbb7b-199">**Is the feature an extensibility point?**</span></span> <span data-ttu-id="bbb7b-200">확장성 지점을 통해 개발자는 해당 동작에 연결하고 누락된 기능을 보완할 수 있으므로 Microsoft는 확장성 지점을 선호하는 경향이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-200">We tend to favor extensibility points over normal features because they enable developers to hook their own behaviors and compensate for any missing functionality.</span></span>

5. <span data-ttu-id="bbb7b-201">**다른 제품과 함께 사용할 경우 기능의 시너지 효과는 무엇인가요?**</span><span class="sxs-lookup"><span data-stu-id="bbb7b-201">**What is the synergy of the feature when used in combination with other products?**</span></span> <span data-ttu-id="bbb7b-202">Microsoft는 EF Core를 .NET Core, 최신 버전의 Visual Studio, Microsoft Azure 등과 같은 다른 제품과 함께 사용하도록 하거나 그러한 환경을 크게 향상하는 기능을 선호합니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-202">We favor features that enable or significantly improve the experience of using EF Core with other products, such as .NET Core, the latest version of Visual Studio, Microsoft Azure, and so on.</span></span>

6. <span data-ttu-id="bbb7b-203">**사람들이 기능에 사용할 수 있는 기술은 무엇이며 이러한 리소스를 가장 잘 활용하는 방법은 무엇인가요?**</span><span class="sxs-lookup"><span data-stu-id="bbb7b-203">**What are the skills of the people available to work on a feature, and how can we best leverage these resources?**</span></span> <span data-ttu-id="bbb7b-204">EF 팀의 각 구성원과 커뮤니티 참가자는 서로 다른 영역에서 다양한 수준의 경험이 있으므로 그에 따라 계획해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-204">Each member of the EF team and our community contributors has different levels of experience in different areas, so we have to plan accordingly.</span></span> <span data-ttu-id="bbb7b-205">GroupBy 번역 또는 다대다와 같은 특정 기능을 작업하기 위해 “모든 인력을 동원”하려고 해도 이는 현실성이 떨어질 것입니다.</span><span class="sxs-lookup"><span data-stu-id="bbb7b-205">Even if we wanted to have "all hands on deck" to work on a specific feature like GroupBy translations, or many-to-many, that wouldn't be practical.</span></span>
