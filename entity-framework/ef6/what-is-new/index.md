---
title: 새로운 기능 - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: fcd6339f67a1512dd66220c59537d12cf0b22620
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490300"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="112a7-102">EF6의 새로운 기능</span><span class="sxs-lookup"><span data-stu-id="112a7-102">What's New in EF6</span></span>

<span data-ttu-id="112a7-103">최신 기능 및 높은 안정성을 얻을 수 있도록 Entity Framework 최신 릴리스 버전을 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="112a7-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="112a7-104">하지만 이전 버전을 사용해야 하거나 최신 시험판 릴리스의 개선된 기능을 사용해 보려는 분들도 있다는 사실을 잘 알고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="112a7-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="112a7-105">EF 특정 버전을 설치하는 방법은 [Entity Framework 받기](~/ef6/fundamentals/install.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="112a7-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

<span data-ttu-id="112a7-106">이 페이지에서는 각 새 릴리스에 포함된 기능을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="112a7-106">This page documents the features that are included on each new release.</span></span>

## <a name="recent-releases"></a><span data-ttu-id="112a7-107">최신 릴리스</span><span class="sxs-lookup"><span data-stu-id="112a7-107">Recent releases</span></span>

### <a name="ef-tools-update-in-visual-studio-2017-157"></a><span data-ttu-id="112a7-108">Visual Studio 2017 15.7의 EF 도구 업데이트</span><span class="sxs-lookup"><span data-stu-id="112a7-108">EF Tools Update in Visual Studio 2017 15.7</span></span>

<span data-ttu-id="112a7-109">2018년 5월에 Visual Studio 2017 15.7의 일부로 업데이트된 버전의 EF 도구가 릴리스되었습니다.</span><span class="sxs-lookup"><span data-stu-id="112a7-109">In May 2018, we released an updated version of the EF Tools as part of Visual Studio 2017 15.7.</span></span>
<span data-ttu-id="112a7-110">공통적인 몇 가지 문제점이 이 버전에서 개선되었습니다.</span><span class="sxs-lookup"><span data-stu-id="112a7-110">It includes improvements for some common pain points:</span></span>

- <span data-ttu-id="112a7-111">여러 사용자 인터페이스 접근성 버그 수정</span><span class="sxs-lookup"><span data-stu-id="112a7-111">Fixes for several user interface accessibility bugs</span></span>
- <span data-ttu-id="112a7-112">기존 데이터베이스에서 모델을 생성하는 경우 SQL Server 성능 회귀에 대한 해결 [#4](https://github.com/aspnet/entityframework6/issues/4)</span><span class="sxs-lookup"><span data-stu-id="112a7-112">Workaround for SQL Server performance regression when generating models from existing databases [#4](https://github.com/aspnet/entityframework6/issues/4)</span></span>
- <span data-ttu-id="112a7-113">SQL Server의 대규모 모델에 모델을 업데이트하는 지원 [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span><span class="sxs-lookup"><span data-stu-id="112a7-113">Support for updating models for larger models on SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span></span>

<span data-ttu-id="112a7-114">또한 새 버전의 EF 도구는 새 프로젝트에서 모델을 만들 때 EF 6.2 런타임을 설치하도록 개선되었습니다.</span><span class="sxs-lookup"><span data-stu-id="112a7-114">Another improvement in this this new version of EF Tools is that it installs the EF 6.2 runtime when creating a model in a new project.</span></span> <span data-ttu-id="112a7-115">이전 버전의 Visual Studio를 사용하는 경우 해당하는 NuGet 패키지 버전을 설치하여 EF 6.2 런타임(그리고 이전 버전의 EF)을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="112a7-115">With older versions of Visual Studio, it is possible to use the EF 6.2 runtime (as well as any past version of EF) by installing the corresponding version of the NuGet package.</span></span>

### <a name="ef-62-runtime"></a><span data-ttu-id="112a7-116">EF 6.2 런타임</span><span class="sxs-lookup"><span data-stu-id="112a7-116">EF 6.2 Runtime</span></span>

<span data-ttu-id="112a7-117">EF 6.2 런타임은 2017년 10월에 NuGet에 출시되었습니다.</span><span class="sxs-lookup"><span data-stu-id="112a7-117">The EF 6.2 runtime was released to NuGet in October of 2017.</span></span>
<span data-ttu-id="112a7-118">오픈 소스 기여자 커뮤니티의 적극적인 참여 덕분에 EF 6.2는 여러 [버그 픽스](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) 및 [향상된 제품 기능](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20)을 포함하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="112a7-118">Thanks in great part to the efforts our community of open source contributors, EF 6.2 includes numerous [bugs fixes](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) and [product enhancements](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span></span>

<span data-ttu-id="112a7-119">다음은 EF 6.2 런타임에 영향을 주는 가장 중요한 변경 내용을 간략하게 정리한 목록입니다.</span><span class="sxs-lookup"><span data-stu-id="112a7-119">Here is a brief list of the most important changes affecting the EF 6.2 runtime:</span></span>

- <span data-ttu-id="112a7-120">영구 캐시에서 완료된 Code First 모델을 로드하여 시작 시간 단축 [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span><span class="sxs-lookup"><span data-stu-id="112a7-120">Reduce start up time by loading finished code first models from a persistent cache [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span></span>
- <span data-ttu-id="112a7-121">인덱스를 정의할 수 있는 흐름 API [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span><span class="sxs-lookup"><span data-stu-id="112a7-121">Fluent API to define indexes [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span></span>
- <span data-ttu-id="112a7-122">SQL에서 LIKE로 해석되는 LINQ 쿼리를 쓸 수 있게 해주는 DbFunctions.Like() [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span><span class="sxs-lookup"><span data-stu-id="112a7-122">DbFunctions.Like() to enable writing LINQ queries that translate to LIKE in SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span></span>
- <span data-ttu-id="112a7-123">이제 Migrate.exe가 -script 옵션 지원 [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span><span class="sxs-lookup"><span data-stu-id="112a7-123">Migrate.exe now supports -script option [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span></span>
- <span data-ttu-id="112a7-124">이제 EF6는 SQL Server에서 시퀀스를 통해 생성된 키 값 사용 가능 [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span><span class="sxs-lookup"><span data-stu-id="112a7-124">EF6 can now work with key values generated by a sequence in SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span></span>
- <span data-ttu-id="112a7-125">SQL Azure 실행 전략에 대한 일시적인 오류 목록 업데이트 [83](https://github.com/aspnet/EntityFramework6/issues/83)</span><span class="sxs-lookup"><span data-stu-id="112a7-125">Update list of transient errors for SQL Azure Execution Strategy [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span></span>
- <span data-ttu-id="112a7-126">버그: "SqlParameter가 이미 다른 SqlParameterCollection에 포함되어 있음" 메시지와 함께 쿼리 재시도 또는 SQL 명령이 실패 [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span><span class="sxs-lookup"><span data-stu-id="112a7-126">Bug: Retrying queries or SQL commands fails with "The SqlParameter is already contained by another SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span></span>
- <span data-ttu-id="112a7-127">버그: 디버거에서 DbQuery.ToString() 평가가 자주 시간 초과 [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span><span class="sxs-lookup"><span data-stu-id="112a7-127">Bug: Evaluation of DbQuery.ToString() frequently times out in the debugger [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span></span>

## <a name="future-releases"></a><span data-ttu-id="112a7-128">이후 릴리스</span><span class="sxs-lookup"><span data-stu-id="112a7-128">Future Releases</span></span>

<span data-ttu-id="112a7-129">EF6 이후 버전에 대한 내용은 [로드맵](roadmap.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="112a7-129">For information on future version of EF6, please look at our [Roadmap](roadmap.md).</span></span>

## <a name="past-releases"></a><span data-ttu-id="112a7-130">이전 릴리스</span><span class="sxs-lookup"><span data-stu-id="112a7-130">Past Releases</span></span>

<span data-ttu-id="112a7-131">[이전 릴리스](past-releases.md) 페이지에는 모든 EF 이전 버전 및 각 릴리스에 도입된 주요 기능 아카이브가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="112a7-131">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
