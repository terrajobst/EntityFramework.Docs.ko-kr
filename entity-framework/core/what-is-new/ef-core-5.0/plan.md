---
title: Entity Framework Core 5.0 계획
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 0472841fdcd105ec8ea38db062c6768510b8735d
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125357"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="77baf-102">Entity Framework Core 5.0 계획</span><span class="sxs-lookup"><span data-stu-id="77baf-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="77baf-103">[계획 프로세스](../release-planning.md)에 설명된 바와 같이 Microsoft는 관련자가 제공하는 의견을 모아 잠정적 EF Core 5.0 릴리스 계획에 반영했습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-103">As described in the [planning process](../release-planning.md), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="77baf-104">이 계획은 지금도 진행 중입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="77baf-105">이 문서의 내용은 약정을 일체 구성하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-105">Nothing here is a commitment.</span></span> <span data-ttu-id="77baf-106">이 계획은 일종의 시작점으로서 학습이 늘어남에 따라 발전할 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="77baf-107">현재 5.0에 대해 계획되지 않은 몇몇 사항이 도입될 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="77baf-108">현재 5.0에 대해 계획된 몇몇 사항은 배제될 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-108">Some things currently planned for 5.0 may get punted out.</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="77baf-109">버전 번호 및 릴리스 날짜</span><span class="sxs-lookup"><span data-stu-id="77baf-109">Version number and release date.</span></span>

<span data-ttu-id="77baf-110">EF Core 5.0은 현재 [.NET 5.0과 동일한 시점에](https://devblogs.microsoft.com/dotnet/introducing-net-5/) 릴리스될 것으로 예정된 상태입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-110">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="77baf-111">버전 번호 “5.0”는 .NET 5.0와 통일되도록 선정하였습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-111">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="77baf-112">지원되는 플랫폼</span><span class="sxs-lookup"><span data-stu-id="77baf-112">Supported platforms</span></span>

<span data-ttu-id="77baf-113">EF Core 5.0은 [.NET Core로의 플랫폼 수렴](https://devblogs.microsoft.com/dotnet/introducing-net-5/)에 기반한 모든 .NET 5.0 플랫폼상의 실행을 목적으로 계획되었습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-113">EF Core 5.0 is planned to run on any .NET 5.0 platform based on the [convergence of these platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="77baf-114">.NET Standard와 실제로 사용된 TFM의 측면에서 이것이 의미하는 바는 아직 정해지지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-114">What this means in terms of .NET Standard and the actual TFM used is still TBD.</span></span>

<span data-ttu-id="77baf-115">EF Core 5.0은 .NET Framework에서 실행되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-115">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="77baf-116">호환성이 손상되는 변경</span><span class="sxs-lookup"><span data-stu-id="77baf-116">Breaking changes</span></span>

<span data-ttu-id="77baf-117">EF Core 5.0의 몇몇 변경 사항은 호환성 손상으로 이어지기는 하겠지만 EF Core 3.0에 비해선 그 심각도가 한참 경미한 수준입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-117">EF Core 5.0 will contain some breaking changes, but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="77baf-118">Microsoft는 대다수 애플리케이션이 호환성 손상 없이 업데이트되도록 하는 데 목표를 두었습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-118">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="77baf-119">데이터베이스 공급자의 호환성 손상으로 이어지는 변경 사항이 있을 것으로 예상되는데, 특히 TPT 지원과 관련해 문제가 발생할 것으로 내다보입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-119">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="77baf-120">물론 5.0의 공급자 업데이트 작업은 3.0에 필요했던 만큼보다는 부담이 줄 것으로 보입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-120">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="77baf-121">테마</span><span class="sxs-lookup"><span data-stu-id="77baf-121">Themes</span></span>

<span data-ttu-id="77baf-122">Microsoft는 EF Core 5.0 투자 상당분의 토대를 구성할 몇몇 주요 영역이나 테마를 추출했습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-122">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="77baf-123">다대다 탐색 속성(“건너뛰기 링크”라고도 함)</span><span class="sxs-lookup"><span data-stu-id="77baf-123">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="77baf-124">선임 개발자: @smitpatel 및 @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="77baf-124">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="77baf-125">추적 위치: [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span><span class="sxs-lookup"><span data-stu-id="77baf-125">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="77baf-126">티셔츠 크기: L</span><span class="sxs-lookup"><span data-stu-id="77baf-126">T-shirt size: L</span></span>

<span data-ttu-id="77baf-127">상태: 진행 중</span><span class="sxs-lookup"><span data-stu-id="77baf-127">Status: In-progress</span></span>

<span data-ttu-id="77baf-128">다대다는 GitHub 백로그에서 가장 많이 요청되는 기능(약 407표 투표됨)입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-128">Many-to-many is the most requested feature (~407 votes) on the GitHub backlog.</span></span> <span data-ttu-id="77baf-129">다대다 관계 지원은 크게 세 가지의 주요 영역으로 세분화가 가능합니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-129">Support for many-to-many relationships can be broken down into three major areas:</span></span>

* <span data-ttu-id="77baf-130">건너뛰기 링크 속성.</span><span class="sxs-lookup"><span data-stu-id="77baf-130">Skip navigation properties.</span></span> <span data-ttu-id="77baf-131">이 속성을 통해 모델은 기본 조인 테이블 엔터티 참조 없이 쿼리 등에 사용될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-131">These allow the model to be used for queries, etc. without reference to the underlying join table entity.</span></span>
* <span data-ttu-id="77baf-132">속성 모음 엔터티 형식.</span><span class="sxs-lookup"><span data-stu-id="77baf-132">Property-bag entity types.</span></span> <span data-ttu-id="77baf-133">이 형식을 통해 표준 CLR 형식(예: `Dictionary`)은 각 엔터티 형식에 대해 명시적 CLR 형식이 불필요한 엔터티 인스턴스에 사용될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-133">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span>
* <span data-ttu-id="77baf-134">간편한 다대다 관계 구성을 위한 Sugar.</span><span class="sxs-lookup"><span data-stu-id="77baf-134">Sugar for easy configuration of many-to-many relationships.</span></span>

<span data-ttu-id="77baf-135">이와 같이 필요한 다대다 지원의 가장 큰 걸림돌은 쿼리 같은 비즈니스 논리에서 조인 테이블 참조 없이 “자연” 관계를 사용할 수 없다는 점입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-135">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span> <span data-ttu-id="77baf-136">조인 테이블 엔터티 형식은 여전히 존재하겠지만 비즈니스 논리에 지장을 줘서는 안 됩니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-136">The join table entity type may still exist, but it should not get in the way of business logic.</span></span> <span data-ttu-id="77baf-137">이 때문에 Microsoft에서는 5.0에서 건너뛰기 링크 속성을 다루기로 했습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-137">This is why we have chosen to tackle skip navigation properties for 5.0.</span></span>

<span data-ttu-id="77baf-138">현재 다대다의 다른 부분은 EF Core 5.0의 추가적 목표로서 검토를 진행 중입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-138">At this time the other parts of many-to-many are being pursued as a stretch goal for EF Core 5.0.</span></span> <span data-ttu-id="77baf-139">따라서 지금으로선 5.0 관련 계획에 포함되지는 않았지만 진행이 순조롭다면 도입이 될 것으로 희망하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-139">This means they are not currently in the plan for 5.0, but if things go well we hope to pull them in.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="77baf-140">TPT(형식별 테이블) 상속 매핑</span><span class="sxs-lookup"><span data-stu-id="77baf-140">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="77baf-141">선임 개발자: @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="77baf-141">Lead developer: @AndriySvyryd</span></span>

<span data-ttu-id="77baf-142">추적 위치: [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span><span class="sxs-lookup"><span data-stu-id="77baf-142">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="77baf-143">티셔츠 크기: XL</span><span class="sxs-lookup"><span data-stu-id="77baf-143">T-shirt size: XL</span></span>

<span data-ttu-id="77baf-144">상태: 시작 안 됨</span><span class="sxs-lookup"><span data-stu-id="77baf-144">Status: Not started</span></span>

<span data-ttu-id="77baf-145">TPT는 여러 번 요청되는 기능(약 254표 투표되어 전체 3위 기록)인 데 더해 전체적인 .NET 5 계획의 기본 특성에 적합하다고 판단되는 몇몇 하위 수준 변경 사항을 요하므로 Microsoft에서는 TPT 상속을 사용하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-145">We're doing TPT because it is both a highly requested feature (~254 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="77baf-146">이에 따른 변경 사항은 데이터베이스 공급자에 있어 호환성을 손상할 것으로 보이지만 3.0에 필요한 변경 사항보다는 그 심각도가 한참 경미한 수준입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-146">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="77baf-147">필터링 적용 포함</span><span class="sxs-lookup"><span data-stu-id="77baf-147">Filtered Include</span></span>

<span data-ttu-id="77baf-148">선임 개발자: @maumar</span><span class="sxs-lookup"><span data-stu-id="77baf-148">Lead developer: @maumar</span></span>

<span data-ttu-id="77baf-149">추적 위치: [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span><span class="sxs-lookup"><span data-stu-id="77baf-149">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="77baf-150">티셔츠 크기: M</span><span class="sxs-lookup"><span data-stu-id="77baf-150">T-shirt size: M</span></span>

<span data-ttu-id="77baf-151">상태: 시작 안 됨</span><span class="sxs-lookup"><span data-stu-id="77baf-151">Status: Not started</span></span>

<span data-ttu-id="77baf-152">필터링 적용 포함은 여러 번 요청되는 기능(약 317표 투표되어 전체 2위 기록)으로서 작업량 부담이 크지 않고, 현재 모델 수준의 필터나 좀 더 복잡한 쿼리를 요하는 상당수의 시나리오에 편의를 더하거나 그 차단을 해제할 것으로 판단됩니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-152">Filtered Include is a highly-requested feature (~317 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="77baf-153">ToTable, ToQuery, ToView, FromSql 등의 합리화</span><span class="sxs-lookup"><span data-stu-id="77baf-153">Rationalize ToTable, ToQuery, ToView, FromSql, etc.</span></span>

<span data-ttu-id="77baf-154">선임 개발자: @maumar 및 @smitpatel</span><span class="sxs-lookup"><span data-stu-id="77baf-154">Lead developers: @maumar and @smitpatel</span></span>

<span data-ttu-id="77baf-155">추적 위치: [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span><span class="sxs-lookup"><span data-stu-id="77baf-155">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="77baf-156">티셔츠 크기: L</span><span class="sxs-lookup"><span data-stu-id="77baf-156">T-shirt size: L</span></span>

<span data-ttu-id="77baf-157">상태: 시작 안 됨</span><span class="sxs-lookup"><span data-stu-id="77baf-157">Status: Not started</span></span>

<span data-ttu-id="77baf-158">Microsoft는 원시 SQL, 키 없는 형식, 관련 영역에 대한 지원과 관련해 이전 릴리스에서 발전 성과를 거둘 수 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-158">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="77baf-159">하지만 모두가 하나로서 기능할 방법에 있어서는 여러 간극과 불일치성이라는 문제를 마주했습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-159">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="77baf-160">5\.0의 목표는 이와 같은 문제를 바로잡고 서로 다른 형식의 엔터티와 관련 쿼리, 데이터베이스 아티팩트를 정의, 마이그레이션, 사용하는 데 있어 원활한 환경을 선보이는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-160">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="77baf-161">여기에는 컴파일된 쿼리 API의 업데이트도 포함될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-161">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="77baf-162">Microsoft의 현재 기능 일부는 허용 범위가 너무 커서 빠르게 사용자의 오류로 이어질 수 있으므로 이 항목으로 인한 변경 사항이 애플리케이션 수준의 호환성 손상을 야기할 수 있음에 유의하시기 바랍니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-162">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="77baf-163">이러한 기능 중 몇몇은 차단되고 대안 관련 지침이 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-163">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="77baf-164">일반 쿼리 개선 사항</span><span class="sxs-lookup"><span data-stu-id="77baf-164">General query enhancements</span></span>

<span data-ttu-id="77baf-165">선임 개발자: @smitpatel 및 @maumar</span><span class="sxs-lookup"><span data-stu-id="77baf-165">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="77baf-166">추적 위치: [5.0 마일스톤에서 `area-query` 레이블 지정된 문제](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="77baf-166">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="77baf-167">티셔츠 크기: XL</span><span class="sxs-lookup"><span data-stu-id="77baf-167">T-shirt size: XL</span></span>

<span data-ttu-id="77baf-168">상태: 진행 중</span><span class="sxs-lookup"><span data-stu-id="77baf-168">Status: In-progress</span></span>

<span data-ttu-id="77baf-169">EF Core 3.0의 쿼리 변환 코드는 광범위하게 재작성되었습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-169">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="77baf-170">따라서 쿼리 코드는 일반적으로 좀 더 강력한 상태를 자랑합니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-170">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="77baf-171">5\.0의 경우에는 건너뛰기 링크 속성 및 TPT 지원에 필요한 사항 외에 주요 쿼리 변경을 계획하지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-171">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="77baf-172">그러나 3.0 정비에서 남은 일부 기술 문제를 해결하는 데 있어 상당한 작업이 필요하기는 합니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-172">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="77baf-173">또한 Microsoft는 전체적인 쿼리 환경을 한층 개선하고자 상당수의 버그를 고치고 소규모 향상 개선 작업을 시행할 계획입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-173">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="77baf-174">마이그레이션 및 배포 환경</span><span class="sxs-lookup"><span data-stu-id="77baf-174">Migrations and deployment experience</span></span>

<span data-ttu-id="77baf-175">선임 개발자: @bricelam</span><span class="sxs-lookup"><span data-stu-id="77baf-175">Lead developers: @bricelam</span></span>

<span data-ttu-id="77baf-176">추적 위치: [#19587](https://github.com/dotnet/efcore/issues/19587)</span><span class="sxs-lookup"><span data-stu-id="77baf-176">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="77baf-177">티셔츠 크기: L</span><span class="sxs-lookup"><span data-stu-id="77baf-177">T-shirt size: L</span></span>

<span data-ttu-id="77baf-178">상태: 진행 중</span><span class="sxs-lookup"><span data-stu-id="77baf-178">Status: In-progress</span></span>

<span data-ttu-id="77baf-179">현재 상당수의 개발자는 저마다의 데이터베이스를 애플리케이션 시작 시점에 마이그레이션하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-179">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="77baf-180">이는 간단한 방법이긴 하지만 다음의 이유로 권장되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-180">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="77baf-181">여러 스레드/프로세스/서버가 데이터베이스를 동시에 마이그레이션하려 할 수 있음</span><span class="sxs-lookup"><span data-stu-id="77baf-181">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="77baf-182">이 문제가 발생하는 동안 애플리케이션이 일관되지 않은 상태에 액세스하려 할 수 있음</span><span class="sxs-lookup"><span data-stu-id="77baf-182">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="77baf-183">일반적으로 데이터베이스의 스키마 수정 권한은 애플리케이션 실행에 대해 부여되어서는 안 됨</span><span class="sxs-lookup"><span data-stu-id="77baf-183">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="77baf-184">문제가 발생할 경우 클린 상태로 되돌리기 어려움</span><span class="sxs-lookup"><span data-stu-id="77baf-184">Its hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="77baf-185">Microsoft는 배포 시점에 데이터베이스를 간편히 마이그레이션할 수 있도록 보다 나은 환경을 제공하고자 합니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-185">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="77baf-186">이는 다음과 같은 환경을 말합니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-186">This should:</span></span>

* <span data-ttu-id="77baf-187">Linux, Mac 및 Windows에서 작업됨</span><span class="sxs-lookup"><span data-stu-id="77baf-187">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="77baf-188">명령줄 환경이 우수함</span><span class="sxs-lookup"><span data-stu-id="77baf-188">Be a good experience on the command line</span></span>
* <span data-ttu-id="77baf-189">컨테이너를 사용하는 시나리오 지원</span><span class="sxs-lookup"><span data-stu-id="77baf-189">Support scenarios with containers</span></span>
* <span data-ttu-id="77baf-190">일반적으로 사용되는 실제 배포 도구/흐름으로 작업됨</span><span class="sxs-lookup"><span data-stu-id="77baf-190">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="77baf-191">Visual Studio 이상에 통합됨</span><span class="sxs-lookup"><span data-stu-id="77baf-191">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="77baf-192">그에 따라 EF Core의 여러 작은 부분이 개선된다는 점과(예: SQLite의 마이그레이션 개선) EF의 범주를 넘어선 엔드투엔드 환경 개선을 위한 지침과 다른 팀과의 장기적 협업을 기대할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-192">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="77baf-193">EF Core 플랫폼 환경</span><span class="sxs-lookup"><span data-stu-id="77baf-193">EF Core platforms experience</span></span> 

<span data-ttu-id="77baf-194">선임 개발자: @roji 및 @bricelam</span><span class="sxs-lookup"><span data-stu-id="77baf-194">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="77baf-195">추적 위치: [#19588](https://github.com/dotnet/efcore/issues/19588)</span><span class="sxs-lookup"><span data-stu-id="77baf-195">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="77baf-196">티셔츠 크기: L</span><span class="sxs-lookup"><span data-stu-id="77baf-196">T-shirt size: L</span></span>

<span data-ttu-id="77baf-197">상태: 시작 안 됨</span><span class="sxs-lookup"><span data-stu-id="77baf-197">Status: Not started</span></span>

<span data-ttu-id="77baf-198">Microsoft의 지침은 기존의 MVC 유사 웹 애플리케이션에서 EF Core를 사용하는 데 매우 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-198">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="77baf-199">다른 플랫폼과 애플리케이션 모델에 대한 지침은 찾을 수 없거나 너무 오래되어 유용하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-199">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="77baf-200">EF Core 5.0에 대해서는 다음 항목과 함께 EF Core 사용했을 때의 환경을 조사, 개선 및 서류화할 계획입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-200">For EF Core 5.0 we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="77baf-201">Blazor</span><span class="sxs-lookup"><span data-stu-id="77baf-201">Blazor</span></span>
* <span data-ttu-id="77baf-202">Xamarin(AOT/링커 스토리 사용 포함)</span><span class="sxs-lookup"><span data-stu-id="77baf-202">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="77baf-203">WinForms/WPF/WinUI 및 기타 U.I.</span><span class="sxs-lookup"><span data-stu-id="77baf-203">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="77baf-204">frameworks</span><span class="sxs-lookup"><span data-stu-id="77baf-204">frameworks</span></span>

<span data-ttu-id="77baf-205">EF Core의 여러 부분이 소규모로 개선된다는 점과 EF의 범주를 넘어선 엔드투엔드 환경 개선을 위한 지침과 다른 팀과의 장기적 협업을 기대할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-205">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="77baf-206">검토 계획에 있는 구체적 영역은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-206">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="77baf-207">배포(EF 도구에 대한 사용 환경 포함, 예: 마이그레이션 시)</span><span class="sxs-lookup"><span data-stu-id="77baf-207">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="77baf-208">애플리케이션 모델(Xamarin, Blazor 등 포함)</span><span class="sxs-lookup"><span data-stu-id="77baf-208">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="77baf-209">SQLite 환경(공간적 환경 및 테이블 다시 빌드 포함)</span><span class="sxs-lookup"><span data-stu-id="77baf-209">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="77baf-210">AOT 및 연결 환경</span><span class="sxs-lookup"><span data-stu-id="77baf-210">AOT and linking experiences</span></span>
* <span data-ttu-id="77baf-211">진단 통합(성능 카운터 포함)</span><span class="sxs-lookup"><span data-stu-id="77baf-211">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="77baf-212">성능</span><span class="sxs-lookup"><span data-stu-id="77baf-212">Performance</span></span>

<span data-ttu-id="77baf-213">선임 개발자: @roji</span><span class="sxs-lookup"><span data-stu-id="77baf-213">Lead developer: @roji</span></span>

<span data-ttu-id="77baf-214">추적 위치: [5.0 마일스톤에서 `area-perf` 레이블 지정된 문제](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="77baf-214">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="77baf-215">티셔츠 크기: L</span><span class="sxs-lookup"><span data-stu-id="77baf-215">T-shirt size: L</span></span>

<span data-ttu-id="77baf-216">상태: 진행 중</span><span class="sxs-lookup"><span data-stu-id="77baf-216">Status: In-progress</span></span>

<span data-ttu-id="77baf-217">Microsoft는 EF Core에 대해 성능 벤치마크 도구 모음을 개선하고 런타임의 성능을 계획된 바에 따라 개선할 계획입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-217">For EF Core we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="77baf-218">또한 3.0 릴리스 주기에 프로토타입화된 새로운 ADO.NET 일괄 처리 API를 완료할 계획입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-218">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="77baf-219">또한 ADO.NET 레이어에서는 Npgsql 공급자의 성능 개선을 추가적으로 계획했습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-219">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="77baf-220">이 작업의 일환으로 ADO.NET/EF Core 성능 카운터 및 기타 진단을 적절히 추가할 계획도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-220">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="77baf-221">아키텍처/기여자 문서</span><span class="sxs-lookup"><span data-stu-id="77baf-221">Architectural/contributor documentation</span></span>

<span data-ttu-id="77baf-222">선임 작성자: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="77baf-222">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="77baf-223">추적 위치: [#1920](https://github.com/aspnet/EntityFramework.Docs/issues/1920)</span><span class="sxs-lookup"><span data-stu-id="77baf-223">Tracked by [#1920](https://github.com/aspnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="77baf-224">티셔츠 크기: L</span><span class="sxs-lookup"><span data-stu-id="77baf-224">T-shirt size: L</span></span>

<span data-ttu-id="77baf-225">상태: 시작 안 됨</span><span class="sxs-lookup"><span data-stu-id="77baf-225">Status: Not started</span></span>

<span data-ttu-id="77baf-226">이 문서화의 목적은 EF Core 내부 요소를 보다 쉽게 파악할 수 있게 하는 데 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-226">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="77baf-227">이는 EF Core 사용자 모두에게 유용할 수 있지만 주된 목적은 외부 인력이 다음을 좀 더 간편히 수행하도록 돕는 데 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-227">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="77baf-228">EF Core 코드 기여</span><span class="sxs-lookup"><span data-stu-id="77baf-228">Contribute to the EF Core code</span></span>
* <span data-ttu-id="77baf-229">데이터베이스 공급자 생성</span><span class="sxs-lookup"><span data-stu-id="77baf-229">Create database providers</span></span>
* <span data-ttu-id="77baf-230">기타 확장 빌드</span><span class="sxs-lookup"><span data-stu-id="77baf-230">Build other extensions</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="77baf-231">Microsoft.Data.Sqlite 문서</span><span class="sxs-lookup"><span data-stu-id="77baf-231">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="77baf-232">선임 작성자: @bricelam</span><span class="sxs-lookup"><span data-stu-id="77baf-232">Lead documenter: @bricelam</span></span>

<span data-ttu-id="77baf-233">추적 위치: [#1675](https://github.com/aspnet/EntityFramework.Docs/issues/1675)</span><span class="sxs-lookup"><span data-stu-id="77baf-233">Tracked by [#1675](https://github.com/aspnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="77baf-234">티셔츠 크기: M</span><span class="sxs-lookup"><span data-stu-id="77baf-234">T-shirt size: M</span></span>

<span data-ttu-id="77baf-235">상태: 완료.</span><span class="sxs-lookup"><span data-stu-id="77baf-235">Status: Completed.</span></span> <span data-ttu-id="77baf-236">새 문서는 [Microsoft 문서 사이트에 게시](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli)되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-236">The new documentation is [live on the Microsoft docs site](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="77baf-237">EF 팀은 Microsoft.Data.Sqlite ADO.NET 공급자도 소유합니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-237">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="77baf-238">Microsoft는 5.0 릴리스의 일환으로 이 공급자를 완전히 문서화할 계획입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-238">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="77baf-239">일반 문서</span><span class="sxs-lookup"><span data-stu-id="77baf-239">General documentation</span></span>

<span data-ttu-id="77baf-240">선임 작성자: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="77baf-240">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="77baf-241">추적 위치: [5.0 마일스톤에서 문서 리포지토리의 문제](https://github.com/aspnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="77baf-241">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/aspnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="77baf-242">티셔츠 크기: L</span><span class="sxs-lookup"><span data-stu-id="77baf-242">T-shirt size: L</span></span>

<span data-ttu-id="77baf-243">상태: 진행 중</span><span class="sxs-lookup"><span data-stu-id="77baf-243">Status: In-progress</span></span>

<span data-ttu-id="77baf-244">Microsoft는 이미 3.0 및 3.1 릴리스 관련 문서를 업데이트 중입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-244">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="77baf-245">그에 더하여 다음 작업도 현재 진행 중에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-245">We are also working on:</span></span>
  * <span data-ttu-id="77baf-246">시작하기 문서를 이용하고 따라하기 쉽도록 정비하는 작업</span><span class="sxs-lookup"><span data-stu-id="77baf-246">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
  * <span data-ttu-id="77baf-247">상호 참조를 보다 쉽게 검색 및 추가할 수 있도록 문서 재구성</span><span class="sxs-lookup"><span data-stu-id="77baf-247">Reorganization of docs to make things easier to find and to add cross-references</span></span>
  * <span data-ttu-id="77baf-248">기존 문서상의 세부 정보 및 설명 추가</span><span class="sxs-lookup"><span data-stu-id="77baf-248">Adding more details and clarifications to existing docs</span></span>
  * <span data-ttu-id="77baf-249">샘플 업데이트 및 사례 추가</span><span class="sxs-lookup"><span data-stu-id="77baf-249">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="77baf-250">버그 수정</span><span class="sxs-lookup"><span data-stu-id="77baf-250">Fixing bugs</span></span>

<span data-ttu-id="77baf-251">추적 위치: [5.0 마일스톤에서 `type-bug` 레이블 지정된 문제](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span><span class="sxs-lookup"><span data-stu-id="77baf-251">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="77baf-252">개발자: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="77baf-252">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="77baf-253">티셔츠 크기: L</span><span class="sxs-lookup"><span data-stu-id="77baf-253">T-shirt size: L</span></span>

<span data-ttu-id="77baf-254">상태: 진행 중</span><span class="sxs-lookup"><span data-stu-id="77baf-254">Status: In-progress</span></span>

<span data-ttu-id="77baf-255">작성 시점에 5.0 릴리스에서 수정이 필요하다고 심사된 버그는 135개이나(62개는 이미 수정됨), 위의 _일반 쿼리 개선 사항_ 섹션과 크게 중첩되는 부분이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-255">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="77baf-256">수신율(마일스톤 내 작업으로 귀결된 문제)은 3.0 릴리스 과정 전반에서 월별 약 23건으로 집계된 문제입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-256">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="77baf-257">이 문제를 5.0에서 전부 수정해야 하는 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-257">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="77baf-258">5\.0 기간에는 어림잡아 150건의 문제를 추가로 수정할 계획입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-258">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="77baf-259">소규모 개선 사항</span><span class="sxs-lookup"><span data-stu-id="77baf-259">Small enhancements</span></span>

<span data-ttu-id="77baf-260">추적 위치: [5.0 마일스톤에서 `type-enhancement` 레이블 지정된 문제](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span><span class="sxs-lookup"><span data-stu-id="77baf-260">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="77baf-261">개발자: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="77baf-261">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="77baf-262">티셔츠 크기: L</span><span class="sxs-lookup"><span data-stu-id="77baf-262">T-shirt size: L</span></span>

<span data-ttu-id="77baf-263">상태: 진행 중</span><span class="sxs-lookup"><span data-stu-id="77baf-263">Status: In-progress</span></span>

<span data-ttu-id="77baf-264">5\.0에서는 위에 개괄된 좀 더 큰 규모의 기능 외에도 경미한 문제의 해결을 위한 작은 수준의 개선 사항도 다수 계획되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-264">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="77baf-265">이러한 개선 사항 중 상당수는 위에서 간략히 소개된 일반적 테마에서도 다뤄집니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-265">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="77baf-266">BTL(Below-The-Line)</span><span class="sxs-lookup"><span data-stu-id="77baf-266">Below-the-line</span></span>

<span data-ttu-id="77baf-267">추적 위치: [`consider-for-next-release` 레이블 지정된 문제](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span><span class="sxs-lookup"><span data-stu-id="77baf-267">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="77baf-268">이는 버그 수정 사항 및 개선 사항으로서 현재 5.0 릴리스에 계획되지는 **않았지만** 위 작업의 추이에 따라 추가적인 목표로 설정할지도 검토할 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-268">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="77baf-269">또한 Microsoft는 계획 수립 시 [투표 수가 가장 많은 문제](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)를 항상 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-269">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="77baf-270">릴리스에서 이와 같은 문제를 줄이는 일은 언제나 만만치 않은 일이지만 현재 보유한 리소스에는 현실적인 계획이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-270">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="feedback"></a><span data-ttu-id="77baf-271">사용자 의견</span><span class="sxs-lookup"><span data-stu-id="77baf-271">Feedback</span></span>

<span data-ttu-id="77baf-272">계획에 대한 피드백은 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-272">Your feedback on planning is important.</span></span> <span data-ttu-id="77baf-273">이슈의 중요성을 나타내는 가장 좋은 방법은 GitHub에서 해당 이슈에 대해 투표(엄지손가락을 위로)하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-273">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="77baf-274">이 데이터는 이후 다음 릴리스의 [계획 프로세스](../release-planning.md)에 반영됩니다.</span><span class="sxs-lookup"><span data-stu-id="77baf-274">This data will then feed into the [planning process](../release-planning.md) for the next release.</span></span>
