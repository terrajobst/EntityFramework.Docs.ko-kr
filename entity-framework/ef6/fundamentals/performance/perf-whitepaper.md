---
title: EF4, EF5 및 EF6에 대 한 성능 고려 사항
author: divega
ms.date: 10/23/2016
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
ms.openlocfilehash: 07eb605f0d39f0c1bcfe781540525180f0dd0b22
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181662"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a><span data-ttu-id="718a2-102">EF 4, 5 및 6에 대 한 성능 고려 사항</span><span class="sxs-lookup"><span data-stu-id="718a2-102">Performance considerations for EF 4, 5, and 6</span></span>
<span data-ttu-id="718a2-103">David Obando, Eric Dettinger 및 기타</span><span class="sxs-lookup"><span data-stu-id="718a2-103">By David Obando, Eric Dettinger and others</span></span>

<span data-ttu-id="718a2-104">게시할지 4 월 2012</span><span class="sxs-lookup"><span data-stu-id="718a2-104">Published: April 2012</span></span>

<span data-ttu-id="718a2-105">마지막 업데이트: 2014 년 5 월</span><span class="sxs-lookup"><span data-stu-id="718a2-105">Last updated: May 2014</span></span>

------------------------------------------------------------------------

## <a name="1-introduction"></a><span data-ttu-id="718a2-106">1. 소개</span><span class="sxs-lookup"><span data-stu-id="718a2-106">1. Introduction</span></span>

<span data-ttu-id="718a2-107">개체-관계형 매핑 프레임 워크를 사용 하면 개체 지향 응용 프로그램의 데이터 액세스에 대 한 추상화를 편리 하 게 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-107">Object-Relational Mapping frameworks are a convenient way to provide an abstraction for data access in an object-oriented application.</span></span> <span data-ttu-id="718a2-108">.NET 응용 프로그램의 경우 Microsoft의 권장 O/RM이 Entity Framework 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-108">For .NET applications, Microsoft's recommended O/RM is Entity Framework.</span></span> <span data-ttu-id="718a2-109">그러나 모든 추상화를 사용 하면 성능이 중요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-109">With any abstraction though, performance can become a concern.</span></span>

<span data-ttu-id="718a2-110">이 백서에는 Entity Framework를 사용 하 여 응용 프로그램을 개발할 때 성능에 영향을 줄 수 있는 Entity Framework 내부 알고리즘을 이해 하 고 조사를 위한 팁을 제공할 수 있는 성능 고려 사항이 나와 있습니다. Entity Framework를 사용 하는 응용 프로그램의 성능 향상</span><span class="sxs-lookup"><span data-stu-id="718a2-110">This whitepaper was written to show the performance considerations when developing applications using Entity Framework, to give developers an idea of the Entity Framework internal algorithms that can affect performance, and to provide tips for investigation and improving performance in their applications that use Entity Framework.</span></span> <span data-ttu-id="718a2-111">웹에서 이미 사용할 수 있는 성능에 대 한 유용한 토픽이 많이 있으며, 가능한 경우 이러한 리소스를 가리키기도 시도 했습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-111">There are a number of good topics on performance already available on the web, and we've also tried pointing to these resources where possible.</span></span>

<span data-ttu-id="718a2-112">성능은 까다로운 주제입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-112">Performance is a tricky topic.</span></span> <span data-ttu-id="718a2-113">이 백서는 Entity Framework를 사용 하는 응용 프로그램에 대 한 성능 관련 결정을 내리는 데 도움이 되는 리소스로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-113">This whitepaper is intended as a resource to help you make performance related decisions for your applications that use Entity Framework.</span></span> <span data-ttu-id="718a2-114">성능을 보여 주기 위해 일부 테스트 메트릭을 포함 했지만 이러한 메트릭은 응용 프로그램에서 볼 수 있는 성능에 대 한 절대 표시기로 제공 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-114">We have included some test metrics to demonstrate performance, but these metrics aren't intended as absolute indicators of the performance you will see in your application.</span></span>

<span data-ttu-id="718a2-115">실용적인 목적으로이 문서에서는 Entity Framework 4가 .NET 4.0에서 실행 되 고 Entity Framework 5와 6이 .NET 4.5에서 실행 된다고 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-115">For practical purposes, this document assumes Entity Framework 4 is run under .NET 4.0 and Entity Framework 5 and 6 are run under .NET 4.5.</span></span> <span data-ttu-id="718a2-116">Entity Framework 5에 대 한 성능 향상의 대부분은 .NET 4.5와 함께 제공 되는 핵심 구성 요소 내에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-116">Many of the performance improvements made for Entity Framework 5 reside within the core components that ship with .NET 4.5.</span></span>

<span data-ttu-id="718a2-117">Entity Framework 6은 대역 외 릴리스 이며 .NET과 함께 제공 되는 Entity Framework 구성 요소에 의존 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-117">Entity Framework 6 is an out of band release and does not depend on the Entity Framework components that ship with .NET.</span></span> <span data-ttu-id="718a2-118">Entity Framework 6은 .NET 4.0 및 .NET 4.5 둘 다에서 작동 하며, .NET 4.0에서 업그레이드 하지 않았지만 응용 프로그램에서 최신 Entity Framework 비트가 필요한 사용자에 게 큰 성능 이점을 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-118">Entity Framework 6 work on both .NET 4.0 and .NET 4.5, and can offer a big performance benefit to those who haven’t upgraded from .NET 4.0 but want the latest Entity Framework bits in their application.</span></span> <span data-ttu-id="718a2-119">이 문서에 Entity Framework 6 인 경우이 문서를 작성할 당시에 사용할 수 있는 최신 버전을 참조 하세요. 버전 6.1.0.</span><span class="sxs-lookup"><span data-stu-id="718a2-119">When this document mentions Entity Framework 6, it refers to the latest version available at the time of this writing: version 6.1.0.</span></span>

## <a name="2-cold-vs-warm-query-execution"></a><span data-ttu-id="718a2-120">2. 콜드 및 웜 쿼리 실행</span><span class="sxs-lookup"><span data-stu-id="718a2-120">2. Cold vs. Warm Query Execution</span></span>

<span data-ttu-id="718a2-121">지정 된 모델에 대 한 쿼리가 처음으로 수행 되는 경우 Entity Framework은 모델을 로드 하 고 유효성을 검사 하기 위해 백그라운드에서 많은 작업을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-121">The very first time any query is made against a given model, the Entity Framework does a lot of work behind the scenes to load and validate the model.</span></span> <span data-ttu-id="718a2-122">이 첫 번째 쿼리를 "콜드" 쿼리로 자주 참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-122">We frequently refer to this first query as a "cold" query.</span></span><span data-ttu-id="718a2-123">  이미 로드 된 모델에 대 한 추가 쿼리는 "웜" 쿼리 라고 하며 훨씬 더 빠릅니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-123">  Further queries against an already loaded model are known as "warm" queries, and are much faster.</span></span>

<span data-ttu-id="718a2-124">Entity Framework를 사용 하 여 쿼리를 실행할 때 시간이 소요 되는 위치를 개략적으로 확인 하 고 Entity Framework 6에서 개선 되는 기능을 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-124">Let’s take a high-level view of where time is spent when executing a query using Entity Framework, and see where things are improving in Entity Framework 6.</span></span>

<span data-ttu-id="718a2-125">**첫 번째 쿼리 실행-콜드 쿼리**</span><span class="sxs-lookup"><span data-stu-id="718a2-125">**First Query Execution – cold query**</span></span>

| <span data-ttu-id="718a2-126">코드 사용자 쓰기</span><span class="sxs-lookup"><span data-stu-id="718a2-126">Code User Writes</span></span>                                                                                     | <span data-ttu-id="718a2-127">작업</span><span class="sxs-lookup"><span data-stu-id="718a2-127">Action</span></span>                    | <span data-ttu-id="718a2-128">EF4 성능 영향</span><span class="sxs-lookup"><span data-stu-id="718a2-128">EF4 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                        | <span data-ttu-id="718a2-129">EF5 성능 영향</span><span class="sxs-lookup"><span data-stu-id="718a2-129">EF5 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                    | <span data-ttu-id="718a2-130">EF6 성능 영향</span><span class="sxs-lookup"><span data-stu-id="718a2-130">EF6 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | <span data-ttu-id="718a2-131">컨텍스트 만들기</span><span class="sxs-lookup"><span data-stu-id="718a2-131">Context creation</span></span>          | <span data-ttu-id="718a2-132">보통</span><span class="sxs-lookup"><span data-stu-id="718a2-132">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                        | <span data-ttu-id="718a2-133">보통</span><span class="sxs-lookup"><span data-stu-id="718a2-133">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | <span data-ttu-id="718a2-134">낮음</span><span class="sxs-lookup"><span data-stu-id="718a2-134">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | <span data-ttu-id="718a2-135">쿼리 식 생성</span><span class="sxs-lookup"><span data-stu-id="718a2-135">Query expression creation</span></span> | <span data-ttu-id="718a2-136">낮음</span><span class="sxs-lookup"><span data-stu-id="718a2-136">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="718a2-137">낮음</span><span class="sxs-lookup"><span data-stu-id="718a2-137">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="718a2-138">낮음</span><span class="sxs-lookup"><span data-stu-id="718a2-138">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | <span data-ttu-id="718a2-139">LINQ 쿼리 실행</span><span class="sxs-lookup"><span data-stu-id="718a2-139">LINQ query execution</span></span>      | <span data-ttu-id="718a2-140">-메타 데이터 로드: 높음 (캐시 됨)</span><span class="sxs-lookup"><span data-stu-id="718a2-140">- Metadata loading: High but cached</span></span> <br/> <span data-ttu-id="718a2-141">-뷰 생성: 잠재적으로 매우 높음 (캐시 됨)</span><span class="sxs-lookup"><span data-stu-id="718a2-141">- View generation: Potentially very high but cached</span></span> <br/> <span data-ttu-id="718a2-142">-매개 변수 평가: 보통</span><span class="sxs-lookup"><span data-stu-id="718a2-142">- Parameter evaluation: Medium</span></span> <br/> <span data-ttu-id="718a2-143">-쿼리 변환: 보통</span><span class="sxs-lookup"><span data-stu-id="718a2-143">- Query translation: Medium</span></span> <br/> <span data-ttu-id="718a2-144">-구체화 생성: 보통 (캐시 됨)</span><span class="sxs-lookup"><span data-stu-id="718a2-144">- Materializer generation: Medium but cached</span></span> <br/> <span data-ttu-id="718a2-145">-데이터베이스 쿼리 실행: 잠재적으로 높음</span><span class="sxs-lookup"><span data-stu-id="718a2-145">- Database query execution: Potentially high</span></span> <br/> <span data-ttu-id="718a2-146">+ 연결 열기</span><span class="sxs-lookup"><span data-stu-id="718a2-146">+ Connection.Open</span></span> <br/> <span data-ttu-id="718a2-147">+ ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="718a2-147">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="718a2-148">+ DataReader.Read</span><span class="sxs-lookup"><span data-stu-id="718a2-148">+ DataReader.Read</span></span> <br/> <span data-ttu-id="718a2-149">개체 구체화: 보통</span><span class="sxs-lookup"><span data-stu-id="718a2-149">Object materialization: Medium</span></span> <br/> <span data-ttu-id="718a2-150">-Id 조회: 보통</span><span class="sxs-lookup"><span data-stu-id="718a2-150">- Identity lookup: Medium</span></span> | <span data-ttu-id="718a2-151">-메타 데이터 로드: 높음 (캐시 됨)</span><span class="sxs-lookup"><span data-stu-id="718a2-151">- Metadata loading: High but cached</span></span> <br/> <span data-ttu-id="718a2-152">-뷰 생성: 잠재적으로 매우 높음 (캐시 됨)</span><span class="sxs-lookup"><span data-stu-id="718a2-152">- View generation: Potentially very high but cached</span></span> <br/> <span data-ttu-id="718a2-153">-매개 변수 평가: 낮음</span><span class="sxs-lookup"><span data-stu-id="718a2-153">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="718a2-154">-쿼리 변환: 보통 (캐시 됨)</span><span class="sxs-lookup"><span data-stu-id="718a2-154">- Query translation: Medium but cached</span></span> <br/> <span data-ttu-id="718a2-155">-구체화 생성: 보통 (캐시 됨)</span><span class="sxs-lookup"><span data-stu-id="718a2-155">- Materializer generation: Medium but cached</span></span> <br/> <span data-ttu-id="718a2-156">-데이터베이스 쿼리 실행: 잠재적으로 높음 (경우에 따라 더 나은 쿼리)</span><span class="sxs-lookup"><span data-stu-id="718a2-156">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="718a2-157">+ 연결 열기</span><span class="sxs-lookup"><span data-stu-id="718a2-157">+ Connection.Open</span></span> <br/> <span data-ttu-id="718a2-158">+ ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="718a2-158">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="718a2-159">+ DataReader.Read</span><span class="sxs-lookup"><span data-stu-id="718a2-159">+ DataReader.Read</span></span> <br/> <span data-ttu-id="718a2-160">개체 구체화: 보통</span><span class="sxs-lookup"><span data-stu-id="718a2-160">Object materialization: Medium</span></span> <br/> <span data-ttu-id="718a2-161">-Id 조회: 보통</span><span class="sxs-lookup"><span data-stu-id="718a2-161">- Identity lookup: Medium</span></span> | <span data-ttu-id="718a2-162">-메타 데이터 로드: 높음 (캐시 됨)</span><span class="sxs-lookup"><span data-stu-id="718a2-162">- Metadata loading: High but cached</span></span> <br/> <span data-ttu-id="718a2-163">-뷰 생성: 보통 (캐시 됨)</span><span class="sxs-lookup"><span data-stu-id="718a2-163">- View generation: Medium but cached</span></span> <br/> <span data-ttu-id="718a2-164">-매개 변수 평가: 낮음</span><span class="sxs-lookup"><span data-stu-id="718a2-164">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="718a2-165">-쿼리 변환: 보통 (캐시 됨)</span><span class="sxs-lookup"><span data-stu-id="718a2-165">- Query translation: Medium but cached</span></span> <br/> <span data-ttu-id="718a2-166">-구체화 생성: 보통 (캐시 됨)</span><span class="sxs-lookup"><span data-stu-id="718a2-166">- Materializer generation: Medium but cached</span></span> <br/> <span data-ttu-id="718a2-167">-데이터베이스 쿼리 실행: 잠재적으로 높음 (경우에 따라 더 나은 쿼리)</span><span class="sxs-lookup"><span data-stu-id="718a2-167">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="718a2-168">+ 연결 열기</span><span class="sxs-lookup"><span data-stu-id="718a2-168">+ Connection.Open</span></span> <br/> <span data-ttu-id="718a2-169">+ ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="718a2-169">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="718a2-170">+ DataReader.Read</span><span class="sxs-lookup"><span data-stu-id="718a2-170">+ DataReader.Read</span></span> <br/> <span data-ttu-id="718a2-171">개체 구체화: 중간 (EF5 보다 빠름)</span><span class="sxs-lookup"><span data-stu-id="718a2-171">Object materialization: Medium (Faster than EF5)</span></span> <br/> <span data-ttu-id="718a2-172">-Id 조회: 보통</span><span class="sxs-lookup"><span data-stu-id="718a2-172">- Identity lookup: Medium</span></span> |
| `}`                                                                                                  | <span data-ttu-id="718a2-173">연결. 닫기</span><span class="sxs-lookup"><span data-stu-id="718a2-173">Connection.Close</span></span>          | <span data-ttu-id="718a2-174">낮음</span><span class="sxs-lookup"><span data-stu-id="718a2-174">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="718a2-175">낮음</span><span class="sxs-lookup"><span data-stu-id="718a2-175">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="718a2-176">낮음</span><span class="sxs-lookup"><span data-stu-id="718a2-176">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


<span data-ttu-id="718a2-177">**두 번째 쿼리 실행-웜 쿼리**</span><span class="sxs-lookup"><span data-stu-id="718a2-177">**Second Query Execution – warm query**</span></span>

| <span data-ttu-id="718a2-178">코드 사용자 쓰기</span><span class="sxs-lookup"><span data-stu-id="718a2-178">Code User Writes</span></span>                                                                                     | <span data-ttu-id="718a2-179">작업</span><span class="sxs-lookup"><span data-stu-id="718a2-179">Action</span></span>                    | <span data-ttu-id="718a2-180">EF4 성능 영향</span><span class="sxs-lookup"><span data-stu-id="718a2-180">EF4 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | <span data-ttu-id="718a2-181">EF5 성능 영향</span><span class="sxs-lookup"><span data-stu-id="718a2-181">EF5 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | <span data-ttu-id="718a2-182">EF6 성능 영향</span><span class="sxs-lookup"><span data-stu-id="718a2-182">EF6 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | <span data-ttu-id="718a2-183">컨텍스트 만들기</span><span class="sxs-lookup"><span data-stu-id="718a2-183">Context creation</span></span>          | <span data-ttu-id="718a2-184">보통</span><span class="sxs-lookup"><span data-stu-id="718a2-184">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | <span data-ttu-id="718a2-185">보통</span><span class="sxs-lookup"><span data-stu-id="718a2-185">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | <span data-ttu-id="718a2-186">낮음</span><span class="sxs-lookup"><span data-stu-id="718a2-186">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | <span data-ttu-id="718a2-187">쿼리 식 생성</span><span class="sxs-lookup"><span data-stu-id="718a2-187">Query expression creation</span></span> | <span data-ttu-id="718a2-188">낮음</span><span class="sxs-lookup"><span data-stu-id="718a2-188">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | <span data-ttu-id="718a2-189">낮음</span><span class="sxs-lookup"><span data-stu-id="718a2-189">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | <span data-ttu-id="718a2-190">낮음</span><span class="sxs-lookup"><span data-stu-id="718a2-190">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | <span data-ttu-id="718a2-191">LINQ 쿼리 실행</span><span class="sxs-lookup"><span data-stu-id="718a2-191">LINQ query execution</span></span>      | <span data-ttu-id="718a2-192">-메타 데이터 ~~로드~~ 조회: ~~높음 (캐시~~ 됨) 거의</span><span class="sxs-lookup"><span data-stu-id="718a2-192">- Metadata ~~loading~~ lookup: ~~High but cached~~ Low</span></span> <br/> <span data-ttu-id="718a2-193">- ~~생성~~ 조회 보기: ~~잠재적으로 매우 높음 (캐시~~ 됨) 거의</span><span class="sxs-lookup"><span data-stu-id="718a2-193">- View ~~generation~~ lookup: ~~Potentially very high but cached~~ Low</span></span> <br/> <span data-ttu-id="718a2-194">-매개 변수 평가: 보통</span><span class="sxs-lookup"><span data-stu-id="718a2-194">- Parameter evaluation: Medium</span></span> <br/> <span data-ttu-id="718a2-195">-쿼리 ~~변환~~ 조회: 보통</span><span class="sxs-lookup"><span data-stu-id="718a2-195">- Query ~~translation~~ lookup: Medium</span></span> <br/> <span data-ttu-id="718a2-196">-구체화 ~~생성~~ 조회: ~~보통 (캐시~~ 됨) 거의</span><span class="sxs-lookup"><span data-stu-id="718a2-196">- Materializer ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="718a2-197">-데이터베이스 쿼리 실행: 잠재적으로 높음</span><span class="sxs-lookup"><span data-stu-id="718a2-197">- Database query execution: Potentially high</span></span> <br/> <span data-ttu-id="718a2-198">+ 연결 열기</span><span class="sxs-lookup"><span data-stu-id="718a2-198">+ Connection.Open</span></span> <br/> <span data-ttu-id="718a2-199">+ ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="718a2-199">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="718a2-200">+ DataReader.Read</span><span class="sxs-lookup"><span data-stu-id="718a2-200">+ DataReader.Read</span></span> <br/> <span data-ttu-id="718a2-201">개체 구체화: 보통</span><span class="sxs-lookup"><span data-stu-id="718a2-201">Object materialization: Medium</span></span> <br/> <span data-ttu-id="718a2-202">-Id 조회: 보통</span><span class="sxs-lookup"><span data-stu-id="718a2-202">- Identity lookup: Medium</span></span> | <span data-ttu-id="718a2-203">-메타 데이터 ~~로드~~ 조회: ~~높음 (캐시~~ 됨) 거의</span><span class="sxs-lookup"><span data-stu-id="718a2-203">- Metadata ~~loading~~ lookup: ~~High but cached~~ Low</span></span> <br/> <span data-ttu-id="718a2-204">- ~~생성~~ 조회 보기: ~~잠재적으로 매우 높음 (캐시~~ 됨) 거의</span><span class="sxs-lookup"><span data-stu-id="718a2-204">- View ~~generation~~ lookup: ~~Potentially very high but cached~~ Low</span></span> <br/> <span data-ttu-id="718a2-205">-매개 변수 평가: 낮음</span><span class="sxs-lookup"><span data-stu-id="718a2-205">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="718a2-206">-쿼리 ~~변환~~ 조회: ~~보통 (캐시~~ 됨) 거의</span><span class="sxs-lookup"><span data-stu-id="718a2-206">- Query ~~translation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="718a2-207">-구체화 ~~생성~~ 조회: ~~보통 (캐시~~ 됨) 거의</span><span class="sxs-lookup"><span data-stu-id="718a2-207">- Materializer ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="718a2-208">-데이터베이스 쿼리 실행: 잠재적으로 높음 (경우에 따라 더 나은 쿼리)</span><span class="sxs-lookup"><span data-stu-id="718a2-208">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="718a2-209">+ 연결 열기</span><span class="sxs-lookup"><span data-stu-id="718a2-209">+ Connection.Open</span></span> <br/> <span data-ttu-id="718a2-210">+ ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="718a2-210">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="718a2-211">+ DataReader.Read</span><span class="sxs-lookup"><span data-stu-id="718a2-211">+ DataReader.Read</span></span> <br/> <span data-ttu-id="718a2-212">개체 구체화: 보통</span><span class="sxs-lookup"><span data-stu-id="718a2-212">Object materialization: Medium</span></span> <br/> <span data-ttu-id="718a2-213">-Id 조회: 보통</span><span class="sxs-lookup"><span data-stu-id="718a2-213">- Identity lookup: Medium</span></span> | <span data-ttu-id="718a2-214">-메타 데이터 ~~로드~~ 조회: ~~높음 (캐시~~ 됨) 거의</span><span class="sxs-lookup"><span data-stu-id="718a2-214">- Metadata ~~loading~~ lookup: ~~High but cached~~ Low</span></span> <br/> <span data-ttu-id="718a2-215">- ~~생성~~ 조회 보기: ~~보통 (캐시~~ 됨) 거의</span><span class="sxs-lookup"><span data-stu-id="718a2-215">- View ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="718a2-216">-매개 변수 평가: 낮음</span><span class="sxs-lookup"><span data-stu-id="718a2-216">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="718a2-217">-쿼리 ~~변환~~ 조회: ~~보통 (캐시~~ 됨) 거의</span><span class="sxs-lookup"><span data-stu-id="718a2-217">- Query ~~translation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="718a2-218">-구체화 ~~생성~~ 조회: ~~보통 (캐시~~ 됨) 거의</span><span class="sxs-lookup"><span data-stu-id="718a2-218">- Materializer ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="718a2-219">-데이터베이스 쿼리 실행: 잠재적으로 높음 (경우에 따라 더 나은 쿼리)</span><span class="sxs-lookup"><span data-stu-id="718a2-219">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="718a2-220">+ 연결 열기</span><span class="sxs-lookup"><span data-stu-id="718a2-220">+ Connection.Open</span></span> <br/> <span data-ttu-id="718a2-221">+ ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="718a2-221">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="718a2-222">+ DataReader.Read</span><span class="sxs-lookup"><span data-stu-id="718a2-222">+ DataReader.Read</span></span> <br/> <span data-ttu-id="718a2-223">개체 구체화: 중간 (EF5 보다 빠름)</span><span class="sxs-lookup"><span data-stu-id="718a2-223">Object materialization: Medium (Faster than EF5)</span></span> <br/> <span data-ttu-id="718a2-224">-Id 조회: 보통</span><span class="sxs-lookup"><span data-stu-id="718a2-224">- Identity lookup: Medium</span></span> |
| `}`                                                                                                  | <span data-ttu-id="718a2-225">연결. 닫기</span><span class="sxs-lookup"><span data-stu-id="718a2-225">Connection.Close</span></span>          | <span data-ttu-id="718a2-226">낮음</span><span class="sxs-lookup"><span data-stu-id="718a2-226">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | <span data-ttu-id="718a2-227">낮음</span><span class="sxs-lookup"><span data-stu-id="718a2-227">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | <span data-ttu-id="718a2-228">낮음</span><span class="sxs-lookup"><span data-stu-id="718a2-228">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


<span data-ttu-id="718a2-229">콜드 쿼리와 웜 쿼리의 성능 비용을 줄이는 몇 가지 방법이 있으며, 다음 섹션에서이에 대해 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-229">There are several ways to reduce the performance cost of both cold and warm queries, and we'll take a look at these in the following section.</span></span> <span data-ttu-id="718a2-230">특히 미리 생성 된 뷰를 사용 하 여 콜드 쿼리에서 모델 로드 비용을 줄이는 것을 살펴보겠습니다 .이는 뷰 생성 중에 발생 하는 성능 pains를 완화 하는 데 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-230">Specifically, we'll look at reducing the cost of model loading in cold queries by using pre-generated views, which should help alleviate performance pains experienced during view generation.</span></span> <span data-ttu-id="718a2-231">웜 쿼리의 경우 쿼리 계획 캐싱, 추적 쿼리 및 다른 쿼리 실행 옵션을 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-231">For warm queries, we'll cover query plan caching, no tracking queries, and different query execution options.</span></span>

### <a name="21-what-is-view-generation"></a><span data-ttu-id="718a2-232">2.1 뷰 생성 이란?</span><span class="sxs-lookup"><span data-stu-id="718a2-232">2.1 What is View Generation?</span></span>

<span data-ttu-id="718a2-233">뷰 생성을 이해 하려면 먼저 "매핑 뷰"를 이해 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-233">In order to understand what view generation is, we must first understand what “Mapping Views” are.</span></span> <span data-ttu-id="718a2-234">매핑 뷰는 각 엔터티 집합 및 연결에 대 한 매핑에 지정 된 변환의 실행 가능한 표현입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-234">Mapping Views are executable representations of the transformations specified in the mapping for each entity set and association.</span></span> <span data-ttu-id="718a2-235">내부적으로 이러한 매핑 뷰는 CQTs (정식 쿼리 트리)의 형태를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-235">Internally, these mapping views take the shape of CQTs (canonical query trees).</span></span> <span data-ttu-id="718a2-236">매핑 뷰에는 다음과 같은 두 가지 유형이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-236">There are two types of mapping views:</span></span>

-   <span data-ttu-id="718a2-237">쿼리 뷰: 데이터베이스 스키마에서 개념적 모델로 이동 하는 데 필요한 변환을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-237">Query views: these represent the transformation necessary to go from the database schema to the conceptual model.</span></span>
-   <span data-ttu-id="718a2-238">업데이트 뷰: 개념적 모델에서 데이터베이스 스키마로 이동 하는 데 필요한 변환을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-238">Update views: these represent the transformation necessary to go from the conceptual model to the database schema.</span></span>

<span data-ttu-id="718a2-239">개념적 모델은 다양 한 방식으로 데이터베이스 스키마와 다를 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-239">Keep in mind that the conceptual model might differ from the database schema in various ways.</span></span> <span data-ttu-id="718a2-240">예를 들어 하나의 단일 테이블을 사용 하 여 두 개의 서로 다른 엔터티 형식에 대 한 데이터를 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-240">For example, one single table might be used to store the data for two different entity types.</span></span> <span data-ttu-id="718a2-241">상속 및 특수 한 매핑은 매핑 뷰의 복잡성에서 역할을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-241">Inheritance and non-trivial mappings play a role in the complexity of the mapping views.</span></span>

<span data-ttu-id="718a2-242">매핑 사양에 따라 이러한 뷰를 계산 하는 프로세스는 뷰 생성을 호출 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-242">The process of computing these views based on the specification of the mapping is what we call view generation.</span></span> <span data-ttu-id="718a2-243">뷰 생성은 모델이 로드 될 때 또는 빌드 시 "미리 생성 된 뷰"를 사용 하 여 동적으로 발생 될 수 있습니다. 후자는 C @ no__t-0 또는 VB 파일에 대 한 Entity SQL 문 형식으로 직렬화 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-243">View generation can either take place dynamically when a model is loaded, or at build time, by using "pre-generated views"; the latter are serialized in the form of Entity SQL statements to a C\# or VB file.</span></span>

<span data-ttu-id="718a2-244">뷰가 생성 될 때에도 유효성 검사가 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-244">When views are generated, they are also validated.</span></span> <span data-ttu-id="718a2-245">성능 관점에서 볼 때 대부분의 뷰 생성 비용은 실제로 엔터티 간의 연결이 적합 하 고 지원 되는 모든 작업에 대해 올바른 카디널리티를 갖도록 하는 뷰의 유효성을 검사 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-245">From a performance standpoint, the vast majority of the cost of view generation is actually the validation of the views which ensures that the connections between the entities make sense and have the correct cardinality for all the supported operations.</span></span>

<span data-ttu-id="718a2-246">엔터티 집합에 대 한 쿼리가 실행 되 면 쿼리는 해당 쿼리 뷰와 결합 되며이 컴퍼지션의 결과는 계획 컴파일러를 통해 실행 되어 백업 저장소에서 이해할 수 있는 쿼리 표현을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-246">When a query over an entity set is executed, the query is combined with the corresponding query view, and the result of this composition is run through the plan compiler to create the representation of the query that the backing store can understand.</span></span> <span data-ttu-id="718a2-247">SQL Server의 경우이 컴파일의 최종 결과는 T-sql SELECT 문이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-247">For SQL Server, the final result of this compilation will be a T-SQL SELECT statement.</span></span> <span data-ttu-id="718a2-248">엔터티 집합에 대 한 업데이트를 처음 수행 하는 경우 유사한 프로세스를 통해 업데이트 뷰를 실행 하 여 대상 데이터베이스에 대 한 DML 문으로 변환 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-248">The first time an update over an entity set is performed, the update view is run through a similar process to transform it into DML statements for the target database.</span></span>

### <a name="22-factors-that-affect-view-generation-performance"></a><span data-ttu-id="718a2-249">2.2 뷰 생성 성능에 영향을 주는 요소</span><span class="sxs-lookup"><span data-stu-id="718a2-249">2.2 Factors that affect View Generation performance</span></span>

<span data-ttu-id="718a2-250">뷰 생성 단계의 성능은 모델의 크기에 따라 달라 지 며 모델을 상호 연결 하는 방법에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-250">The performance of view generation step not only depends on the size of your model but also on how interconnected the model is.</span></span> <span data-ttu-id="718a2-251">두 엔터티가 상속 체인이 나 연결을 통해 연결 된 경우 연결 된 것으로 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-251">If two Entities are connected via an inheritance chain or an Association, they are said to be connected.</span></span> <span data-ttu-id="718a2-252">마찬가지로 두 테이블이 외래 키를 통해 연결 된 경우 연결 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-252">Similarly if two tables are connected via a foreign key, they are connected.</span></span> <span data-ttu-id="718a2-253">스키마의 연결 된 엔터티 및 테이블 수가 늘어나면 뷰 생성 비용이 증가 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-253">As the number of connected Entities and tables in your schemas increase, the view generation cost increases.</span></span>

<span data-ttu-id="718a2-254">뷰를 생성 하 고 유효성을 검사 하는 데 사용 하는 알고리즘은 최악의 경우에는 유용 하지만 약간의 최적화를 사용 하 여이를 개선 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-254">The algorithm that we use to generate and validate views is exponential in the worst case, though we do use some optimizations to improve this.</span></span> <span data-ttu-id="718a2-255">성능에 부정적인 영향을 주는 가장 큰 요인은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-255">The biggest factors that seem to negatively affect performance are:</span></span>

-   <span data-ttu-id="718a2-256">엔터티 수와 이러한 엔터티 간의 연결 크기를 참조 하는 모델 크기</span><span class="sxs-lookup"><span data-stu-id="718a2-256">Model size, referring to the number of entities and the amount of associations between these entities.</span></span>
-   <span data-ttu-id="718a2-257">모델 복잡성, 특히 많은 수의 형식을 포함 하는 상속.</span><span class="sxs-lookup"><span data-stu-id="718a2-257">Model complexity, specifically inheritance involving a large number of types.</span></span>
-   <span data-ttu-id="718a2-258">외래 키 연결 대신 독립 연결을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-258">Using Independent Associations, instead of Foreign Key Associations.</span></span>

<span data-ttu-id="718a2-259">작고 간단한 모델의 경우 비용이 미리 생성 된 보기를 사용 하지 않도록 충분히 작을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-259">For small, simple models the cost may be small enough to not bother using pre-generated views.</span></span> <span data-ttu-id="718a2-260">모델 크기와 복잡성이 증가 함에 따라 보기 생성 및 유효성 검사의 비용을 줄이는 데 사용할 수 있는 몇 가지 옵션이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-260">As model size and complexity increase, there are several options available to reduce the cost of view generation and validation.</span></span>

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a><span data-ttu-id="718a2-261">2.3 미리 생성 된 뷰를 사용 하 여 모델 로드 시간 줄이기</span><span class="sxs-lookup"><span data-stu-id="718a2-261">2.3 Using Pre-Generated Views to decrease model load time</span></span>

<span data-ttu-id="718a2-262">Entity Framework 6에서 미리 생성 된 뷰를 사용 하는 방법에 대 한 자세한 내용은 [미리 생성 된 매핑 보기](~/ef6/fundamentals/performance/pre-generated-views.md) 를 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="718a2-262">For detailed information on how to use pre-generated views on Entity Framework 6 visit [Pre-Generated Mapping Views](~/ef6/fundamentals/performance/pre-generated-views.md)</span></span>

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools-community-edition"></a><span data-ttu-id="718a2-263">Entity Framework Power Tools Community Edition을 사용 하 여 미리 생성 된 뷰 2.3.1</span><span class="sxs-lookup"><span data-stu-id="718a2-263">2.3.1 Pre-Generated views using the Entity Framework Power Tools Community Edition</span></span>

<span data-ttu-id="718a2-264">[Entity Framework 6 파워 도구 Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) 을 사용 하 여 모델 클래스 파일을 마우스 오른쪽 단추로 클릭 하 고 Entity Framework 메뉴를 사용 하 여 "뷰 생성"을 선택 하 여 EDMX 및 Code First 모델의 뷰를 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-264">You can use the [Entity Framework 6 Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) to generate views of EDMX and Code First models by right-clicking the model class file and using the Entity Framework menu to select “Generate Views”.</span></span> <span data-ttu-id="718a2-265">Entity Framework Power Tools Community Edition은 DbContext 파생 컨텍스트에서만 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-265">The Entity Framework Power Tools Community Edition work only on DbContext-derived contexts.</span></span>

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a><span data-ttu-id="718a2-266">Edmgen.exe에서 만든 모델을 사용 하 여 미리 생성 된 뷰를 사용 하는 방법 2.3.2</span><span class="sxs-lookup"><span data-stu-id="718a2-266">2.3.2 How to use Pre-generated views with a model created by EDMGen</span></span>

<span data-ttu-id="718a2-267">Edmgen.exe는 .NET과 함께 제공 되 고 Entity Framework 4 및 5와 함께 작동 하지만 Entity Framework 6에서는 작동 하지 않는 유틸리티입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-267">EDMGen is a utility that ships with .NET and works with Entity Framework 4 and 5, but not with Entity Framework 6.</span></span> <span data-ttu-id="718a2-268">Edmgen.exe를 사용 하면 명령줄에서 모델 파일, 개체 계층 및 뷰를 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-268">EDMGen allows you to generate a model file, the object layer and the views from the command line.</span></span> <span data-ttu-id="718a2-269">출력 중 하나는 원하는 언어의 뷰 파일 (VB 또는 C @ no__t-0)입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-269">One of the outputs will be a Views file in your language of choice, VB or C\#.</span></span> <span data-ttu-id="718a2-270">각 엔터티 집합에 대 한 Entity SQL 코드 조각이 포함 된 코드 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-270">This is a code file containing Entity SQL snippets for each entity set.</span></span> <span data-ttu-id="718a2-271">미리 생성 된 뷰를 사용 하도록 설정 하려면 프로젝트에 파일을 포함 하기만 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-271">To enable pre-generated views, you simply include the file in your project.</span></span>

<span data-ttu-id="718a2-272">모델에 대 한 스키마 파일을 수동으로 편집 하는 경우 뷰 파일을 다시 생성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-272">If you manually make edits to the schema files for the model, you will need to re-generate the views file.</span></span> <span data-ttu-id="718a2-273">**/Mode: ViewGeneration** 플래그를 사용 하 여 edmgen.exe를 실행 하 여이 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-273">You can do this by running EDMGen with the **/mode:ViewGeneration** flag.</span></span>

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a><span data-ttu-id="718a2-274">2.3.3 EDMX 파일을 사용 하 여 미리 생성 된 뷰를 사용 하는 방법</span><span class="sxs-lookup"><span data-stu-id="718a2-274">2.3.3 How to use Pre-Generated Views with an EDMX file</span></span>

<span data-ttu-id="718a2-275">Edmgen.exe를 사용 하 여 EDMX 파일에 대 한 보기를 생성할 수도 있습니다. 이전에 참조 한 MSDN 항목에서는이 작업을 수행 하기 위해 빌드 전 이벤트를 추가 하는 방법에 대해 설명 합니다. 그러나이 작업은 복잡 하며 가능 하지 않은 경우도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-275">You can also use EDMGen to generate views for an EDMX file - the previously referenced MSDN topic describes how to add a pre-build event to do this - but this is complicated and there are some cases where it isn't possible.</span></span> <span data-ttu-id="718a2-276">일반적으로 모델이 edmx 파일에 있을 때 T4 템플릿을 사용 하 여 뷰를 생성 하는 것이 더 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-276">It's generally easier to use a T4 template to generate the views when your model is in an edmx file.</span></span>

<span data-ttu-id="718a2-277">ADO.NET 팀 블로그는 뷰를 생성 하기 위해 T4 템플릿을 사용 하는 방법에 설명 하는 게시물 ( \<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>) 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-277">The ADO.NET team blog has a post that describes how to use a T4 template for view generation ( \<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>).</span></span> <span data-ttu-id="718a2-278">이 게시물에는 다운로드 하 여 프로젝트에 추가할 수 있는 템플릿이 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-278">This post includes a template that can be downloaded and added to your project.</span></span> <span data-ttu-id="718a2-279">템플릿은 Entity Framework의 첫 번째 버전용으로 작성 되었기 때문에 최신 버전의 Entity Framework에서 작동 하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-279">The template was written for the first version of Entity Framework, so they aren’t guaranteed to work with the latest versions of Entity Framework.</span></span> <span data-ttu-id="718a2-280">그러나 Visual Studio 갤러리에서 Entity Framework 4 및 5에 대 한 최신 보기 생성 템플릿 집합을 다운로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-280">However, you can download a more up-to-date set of view generation templates for Entity Framework 4 and 5from the Visual Studio Gallery:</span></span>

-   <span data-ttu-id="718a2-281">VB.NET: \< @ NO__T-1</span><span class="sxs-lookup"><span data-stu-id="718a2-281">VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d></span></span>
-   <span data-ttu-id="718a2-282">C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d></span><span class="sxs-lookup"><span data-stu-id="718a2-282">C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d></span></span>

<span data-ttu-id="718a2-283">Entity Framework 6을 사용 하는 경우에서 가져올 수 있습니다 뷰 생성 T4 템플릿 아래에 있는 Visual Studio 갤러리 \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f> 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-283">If you’re using Entity Framework 6 you can get the view generation T4 templates from the Visual Studio Gallery at \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.</span></span>

### <a name="24-reducing-the-cost-of-view-generation"></a><span data-ttu-id="718a2-284">2.4 보기 생성 비용 절감</span><span class="sxs-lookup"><span data-stu-id="718a2-284">2.4 Reducing the cost of view generation</span></span>

<span data-ttu-id="718a2-285">미리 생성 된 뷰를 사용 하면 뷰 생성 비용을 모델 로드 (런타임)에서 디자인 시간으로 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-285">Using pre-generated views moves the cost of view generation from model loading (run time) to design time.</span></span> <span data-ttu-id="718a2-286">이렇게 하면 런타임 시 시작 성능이 향상 되지만 개발 하는 동안에도 보기 생성의 어려움을 겪을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-286">While this improves startup performance at runtime, you will still experience the pain of view generation while you are developing.</span></span> <span data-ttu-id="718a2-287">컴파일 시간과 런타임에 뷰 생성 비용을 줄이는 데 도움이 되는 몇 가지 추가 트릭을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-287">There are several additional tricks that can help reduce the cost of view generation, both at compile time and run time.</span></span>

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a><span data-ttu-id="718a2-288">2.4.1는 외래 키 연결을 사용 하 여 보기 생성 비용 절감</span><span class="sxs-lookup"><span data-stu-id="718a2-288">2.4.1 Using Foreign Key Associations to reduce view generation cost</span></span>

<span data-ttu-id="718a2-289">모델의 연결을 독립 연결에서 외래 키 연결로 전환 하는 것이 보기 생성에 소요 된 시간을 크게 향상 시키는 여러 가지 사례를 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-289">We have seen a number of cases where switching the associations in the model from Independent Associations to Foreign Key Associations dramatically improved the time spent in view generation.</span></span>

<span data-ttu-id="718a2-290">이러한 개선을 보여 주기 위해 Edmgen.exe를 사용 하 여 두 가지 버전의 Navision 모델을 생성 했습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-290">To demonstrate this improvement, we generated two versions of the Navision model by using EDMGen.</span></span> <span data-ttu-id="718a2-291">*참고: Navision 모델에 대 한 설명은 부록 C를 참조 하세요.*</span><span class="sxs-lookup"><span data-stu-id="718a2-291">*Note: see appendix C for a description of the Navision model.*</span></span> <span data-ttu-id="718a2-292">Navision 모델은 매우 많은 엔터티 및 관계 간의 관계로 인해이 연습에서 흥미롭습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-292">The Navision model is interesting for this exercise due to its very large amount of entities and relationships between them.</span></span>

<span data-ttu-id="718a2-293">이 매우 큰 모델의 한 버전은 외래 키 연결을 사용 하 여 생성 되 고 다른 버전은 독립 연결을 사용 하 여 생성 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-293">One version of this very large model was generated with Foreign Keys Associations and the other was generated with Independent Associations.</span></span> <span data-ttu-id="718a2-294">그런 다음 각 모델에 대 한 뷰를 생성 하는 데 걸린 시간을 시간 지정 했습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-294">We then timed how long it took to generate the views for each model.</span></span> <span data-ttu-id="718a2-295">Entity Framework 5 테스트는 EntityViewGenerator 클래스의 GenerateViews () 메서드를 사용 하 여 뷰를 생성 하 고, Entity Framework 6 테스트는 StorageMappingItemCollection 클래스의 GenerateViews () 메서드를 사용 했습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-295">Entity Framework 5 test used the GenerateViews() method from class EntityViewGenerator to generate the views, while the Entity Framework 6 test used the GenerateViews() method from class StorageMappingItemCollection.</span></span> <span data-ttu-id="718a2-296">이는 Entity Framework 6 코드 베이스에서 발생 한 코드 재구성으로 인해 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-296">This due to code restructuring that occurred in the Entity Framework 6 codebase.</span></span>

<span data-ttu-id="718a2-297">Entity Framework 5를 사용 하 여, 외래 키가 있는 모델에 대 한 생성 보기는 랩 컴퓨터에서 65 분이 걸렸습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-297">Using Entity Framework 5, view generation for the model with Foreign Keys took 65 minutes in a lab machine.</span></span> <span data-ttu-id="718a2-298">독립 연결을 사용 하는 모델에 대 한 뷰를 생성 하는 데 걸리는 시간을 알 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-298">It's unknown how long it would have taken to generate the views for the model that used independent associations.</span></span> <span data-ttu-id="718a2-299">매월 업데이트를 설치 하기 위해 랩에 컴퓨터가 다시 부팅 되기 전에 한 달 동안 테스트를 실행 하 고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-299">We left the test running for over a month before the machine was rebooted in our lab to install monthly updates.</span></span>

<span data-ttu-id="718a2-300">Entity Framework 6을 사용 하는 경우 외래 키가 있는 모델에 대 한 생성 보기는 동일한 랩 컴퓨터에서 28 초가 걸렸습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-300">Using Entity Framework 6, view generation for the model with Foreign Keys took 28 seconds in the same lab machine.</span></span> <span data-ttu-id="718a2-301">독립 연결을 사용 하는 모델에 대 한 생성 보기는 58 초 걸렸습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-301">View generation for the model that uses Independent Associations took 58 seconds.</span></span> <span data-ttu-id="718a2-302">해당 뷰 생성 코드에서 6 Entity Framework에 대 한 향상 된 기능은 많은 프로젝트에서 시작 시간을 단축 하기 위해 미리 생성 된 뷰가 필요 하지 않음을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-302">The improvements done to Entity Framework 6 on its view generation code mean that many projects won’t need pre-generated views to obtain faster startup times.</span></span>

<span data-ttu-id="718a2-303">Entity Framework 4 및 5의 미리 생성 보기는 Edmgen.exe 또는 Entity Framework 파워 도구를 사용 하 여 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-303">It’s important to remark that pre-generating views in Entity Framework 4 and 5 can be done with EDMGen or the Entity Framework Power Tools.</span></span> <span data-ttu-id="718a2-304">Entity Framework 6은 Entity Framework 파워 도구를 통해 또는 [미리 생성 된 매핑 보기](~/ef6/fundamentals/performance/pre-generated-views.md)에서 설명한 대로 프로그래밍 방식으로 뷰 생성을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-304">For Entity Framework 6 view generation can be done via the Entity Framework Power Tools or programmatically as described in [Pre-Generated Mapping Views](~/ef6/fundamentals/performance/pre-generated-views.md).</span></span>

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a><span data-ttu-id="718a2-305">독립 연결 대신 외래 키를 사용 하는 방법 2.4.1.1</span><span class="sxs-lookup"><span data-stu-id="718a2-305">2.4.1.1 How to use Foreign Keys instead of Independent Associations</span></span>

<span data-ttu-id="718a2-306">Visual Studio에서 Edmgen.exe 또는 Entity Designer를 사용 하는 경우 기본적으로 FKs를 가져오고, FKs와 IAs 간을 전환 하는 단일 확인란 또는 명령줄 플래그를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-306">When using EDMGen or the Entity Designer in Visual Studio, you get FKs by default, and it only takes a single checkbox or command line flag to switch between FKs and IAs.</span></span>

<span data-ttu-id="718a2-307">Code First 모델이 클 경우 독립 연결을 사용 하는 것은 뷰 생성에 동일한 효과를 가집니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-307">If you have a large Code First model, using Independent Associations will have the same effect on view generation.</span></span> <span data-ttu-id="718a2-308">종속 개체에 대 한 클래스에 외래 키 속성을 포함 하 여 이러한 영향을 방지할 수 있습니다. 하지만 일부 개발자는 개체 모델을 polluting 하는 것으로 간주 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-308">You can avoid this impact by including Foreign Key properties on the classes for your dependent objects, though some developers will consider this to be polluting their object model.</span></span> <span data-ttu-id="718a2-309">이 주제에 대 한 자세한 정보를 찾을 수 있습니다 \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/> 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-309">You can find more information on this subject in \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.</span></span>

| <span data-ttu-id="718a2-310">사용 하는 경우</span><span class="sxs-lookup"><span data-stu-id="718a2-310">When using</span></span>      | <span data-ttu-id="718a2-311">단계</span><span class="sxs-lookup"><span data-stu-id="718a2-311">Do this</span></span>                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="718a2-312">Entity Designer</span><span class="sxs-lookup"><span data-stu-id="718a2-312">Entity Designer</span></span> | <span data-ttu-id="718a2-313">두 엔터티 간의 연결을 추가한 후에는 참조 제약 조건이 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-313">After adding an association between two entities, make sure you have a referential constraint.</span></span> <span data-ttu-id="718a2-314">참조 제약 조건은 독립적인 연결 대신 외래 키를 사용 Entity Framework에 게 알려 줍니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-314">Referential constraints tell Entity Framework to use Foreign Keys instead of Independent Associations.</span></span> <span data-ttu-id="718a2-315">추가 세부 정보를 참조 하세요 \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx> 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-315">For additional details visit \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>.</span></span> |
| <span data-ttu-id="718a2-316">EDMGen</span><span class="sxs-lookup"><span data-stu-id="718a2-316">EDMGen</span></span>          | <span data-ttu-id="718a2-317">Edmgen.exe를 사용 하 여 데이터베이스에서 파일을 생성 하는 경우 외래 키가 적용 되 고 모델에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-317">When using EDMGen to generate your files from the database, your Foreign Keys will be respected and added to the model as such.</span></span> <span data-ttu-id="718a2-318">EDMGen에 의해 노출 된 다양 한 옵션에 대 한 자세한 내용은 방문 [http://msdn.microsoft.com/library/bb387165.aspx](https://msdn.microsoft.com/library/bb387165.aspx)합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-318">For more information on the different options exposed by EDMGen visit [http://msdn.microsoft.com/library/bb387165.aspx](https://msdn.microsoft.com/library/bb387165.aspx).</span></span>                           |
| <span data-ttu-id="718a2-319">Code First</span><span class="sxs-lookup"><span data-stu-id="718a2-319">Code First</span></span>      | <span data-ttu-id="718a2-320">Code First를 사용할 때 종속 개체에 외래 키 속성을 포함 하는 방법에 대 한 자세한 내용은 [Code First 규칙](~/ef6/modeling/code-first/conventions/built-in.md) 항목의 "관계 규칙" 섹션을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="718a2-320">See the "Relationship Convention" section of the [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) topic for information on how to include foreign key properties on dependent objects when using Code First.</span></span>                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a><span data-ttu-id="718a2-321">모델을 별도 어셈블리로 이동 2.4.2 sections</span><span class="sxs-lookup"><span data-stu-id="718a2-321">2.4.2 Moving your model to a separate assembly</span></span>

<span data-ttu-id="718a2-322">모델이 응용 프로그램의 프로젝트에 직접 포함 되어 있고 빌드 전 이벤트 또는 T4 템플릿을 통해 뷰를 생성 하는 경우 모델이 변경 되지 않은 경우에도 프로젝트를 다시 빌드할 때마다 생성 및 유효성 검사가 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-322">When your model is included directly in your application's project and you generate views through a pre-build event or a T4 template, view generation and validation will take place whenever the project is rebuilt, even if the model wasn't changed.</span></span> <span data-ttu-id="718a2-323">모델을 별도의 어셈블리로 이동 하 고 응용 프로그램의 프로젝트에서 참조 하는 경우에는 모델을 포함 하는 프로젝트를 다시 빌드할 필요 없이 응용 프로그램을 다른 방법으로 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-323">If you move the model to a separate assembly and reference it from your application's project, you can make other changes to your application without needing to rebuild the project containing the model.</span></span>

<span data-ttu-id="718a2-324">*참고:*   모델을 개별 어셈블리로 이동 하는 경우 모델에 대 한 연결 문자열을 클라이언트 프로젝트의 응용 프로그램 구성 파일에 복사 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-324">*Note:*  when moving your model to separate assemblies remember to copy the connection strings for the model into the application configuration file of the client project.</span></span>

#### <a name="243-disable-validation-of-an-edmx-based-model"></a><span data-ttu-id="718a2-325">2.4.3 edmx 기반 모델의 유효성 검사를 사용 하지 않도록 설정</span><span class="sxs-lookup"><span data-stu-id="718a2-325">2.4.3 Disable validation of an edmx-based model</span></span>

<span data-ttu-id="718a2-326">모델을 변경 하지 않은 경우에도 EDMX 모델은 컴파일 시간에 유효성이 검사 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-326">EDMX models are validated at compile time, even if the model is unchanged.</span></span> <span data-ttu-id="718a2-327">모델의 유효성이 이미 검사 된 경우 속성 창에서 "빌드 유효성 검사" 속성을 false로 설정 하 여 컴파일 타임에 유효성 검사를 표시 하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-327">If your model has already been validated, you can suppress validation at compile time by setting the "Validate on Build" property to false in the properties window.</span></span> <span data-ttu-id="718a2-328">매핑 또는 모델을 변경 하는 경우 유효성 검사를 일시적으로 다시 사용 하도록 설정 하 여 변경 내용을 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-328">When you change your mapping or model, you can temporarily re-enable validation to verify your changes.</span></span>

<span data-ttu-id="718a2-329">Entity Framework 6의 Entity Framework Designer에 대 한 성능이 향상 되었으며 "빌드의 유효성 검사" 비용은 이전 버전의 디자이너 에서보다 훨씬 낮습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-329">Note that performance improvements were made to the Entity Framework Designer for Entity Framework 6, and the cost of the “Validate on Build” is much lower than in previous versions of the designer.</span></span>

## <a name="3-caching-in-the-entity-framework"></a><span data-ttu-id="718a2-330">3 Entity Framework 캐싱</span><span class="sxs-lookup"><span data-stu-id="718a2-330">3 Caching in the Entity Framework</span></span>

<span data-ttu-id="718a2-331">Entity Framework에는 기본 제공 되는 다음과 같은 형식의 캐싱이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-331">Entity Framework has the following forms of caching built-in:</span></span>

1.  <span data-ttu-id="718a2-332">개체 캐싱 – ObjectContext 인스턴스에 기본 제공 되는 ObjectStateManager는 해당 인스턴스를 사용 하 여 검색 된 개체의 메모리에서 추적을 유지 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-332">Object caching – the ObjectStateManager built into an ObjectContext instance keeps track in memory of the objects that have been retrieved using that instance.</span></span> <span data-ttu-id="718a2-333">이를 첫 번째 수준의 캐시 라고도 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-333">This is also known as first-level cache.</span></span>
2.  <span data-ttu-id="718a2-334">쿼리 계획 캐싱-쿼리가 두 번 이상 실행 될 때 생성 된 저장소 명령을 다시 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-334">Query Plan Caching - reusing the generated store command when a query is executed more than once.</span></span>
3.  <span data-ttu-id="718a2-335">메타 데이터 캐싱-동일한 모델에 대 한 여러 연결에서 모델에 대 한 메타 데이터를 공유 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-335">Metadata caching - sharing the metadata for a model across different connections to the same model.</span></span>

<span data-ttu-id="718a2-336">EF에서 제공 하는 캐시 외에도 줄 바꿈 공급자 라는 특수 한 종류의 ADO.NET 데이터 공급자를 사용 하 여 데이터베이스에서 검색 된 결과에 대 한 캐시를 사용 하 여 Entity Framework를 확장할 수 있습니다 .이는 두 번째 수준 캐싱이 라고도 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-336">Besides the caches that EF provides out of the box, a special kind of ADO.NET data provider known as a wrapping provider can also be used to extend Entity Framework with a cache for the results retrieved from the database, also known as second-level caching.</span></span>

### <a name="31-object-caching"></a><span data-ttu-id="718a2-337">3.1 개체 캐싱</span><span class="sxs-lookup"><span data-stu-id="718a2-337">3.1 Object Caching</span></span>

<span data-ttu-id="718a2-338">기본적으로 쿼리 결과에서 엔터티가 반환 될 때 EF가이를 구체화 하기 직전에 ObjectContext는 동일한 키를 가진 엔터티가 해당 ObjectStateManager에 이미 로드 되었는지 여부를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-338">By default when an entity is returned in the results of a query, just before EF materializes it, the ObjectContext will check if an entity with the same key has already been loaded into its ObjectStateManager.</span></span> <span data-ttu-id="718a2-339">동일한 키를 가진 엔터티가 이미 있는 경우 EF는 쿼리 결과에이를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-339">If an entity with the same keys is already present EF will include it in the results of the query.</span></span> <span data-ttu-id="718a2-340">EF는 데이터베이스에 대해 계속 쿼리를 실행 하지만이 동작은 엔터티를 여러 번 구체화 하는 비용을 많이 우회할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-340">Although EF will still issue the query against the database, this behavior can bypass much of the cost of materializing the entity multiple times.</span></span>

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a><span data-ttu-id="718a2-341">DbContext Find를 사용 하 여 개체 캐시에서 엔터티 가져오기 3.1.1</span><span class="sxs-lookup"><span data-stu-id="718a2-341">3.1.1 Getting entities from the object cache using DbContext Find</span></span>

<span data-ttu-id="718a2-342">일반 쿼리와 달리 DbSet (EF 4.1에서는 처음으로 포함 된 Api)의 Find 메서드는 데이터베이스에 대해 쿼리를 실행 하기 전에 메모리에서 검색을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-342">Unlike a regular query, the Find method in DbSet (APIs included for the first time in EF 4.1) will perform a search in memory before even issuing the query against the database.</span></span> <span data-ttu-id="718a2-343">두 개의 다른 ObjectContext 인스턴스에는 별도의 개체 캐시가 있음을 의미 하는 두 개의 서로 다른 ObjectStateManager 인스턴스가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-343">It’s important to note that two different ObjectContext instances will have two different ObjectStateManager instances, meaning that they have separate object caches.</span></span>

<span data-ttu-id="718a2-344">Find는 기본 키 값을 사용 하 여 컨텍스트에 의해 추적 된 엔터티를 찾으려고 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-344">Find uses the primary key value to attempt to find an entity tracked by the context.</span></span> <span data-ttu-id="718a2-345">엔터티가 컨텍스트에 있지 않으면 쿼리를 실행 하 고 데이터베이스에 대해 계산 하며, 엔터티가 컨텍스트 또는 데이터베이스에 없는 경우 null이 반환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-345">If the entity is not in the context then a query will be executed and evaluated against the database, and null is returned if the entity is not found in the context or in the database.</span></span> <span data-ttu-id="718a2-346">또한 Find는 컨텍스트에 추가 되었지만 아직 데이터베이스에 저장 되지 않은 엔터티를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-346">Note that Find also returns entities that have been added to the context but have not yet been saved to the database.</span></span>

<span data-ttu-id="718a2-347">찾기를 사용할 때 고려해 야 할 성능 고려 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-347">There is a performance consideration to be taken when using Find.</span></span> <span data-ttu-id="718a2-348">이 메서드를 호출 하면 기본적으로 데이터베이스에 대 한 커밋 보류 중인 변경 내용을 감지 하기 위해 개체 캐시의 유효성 검사가 트리거됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-348">Invocations to this method by default will trigger a validation of the object cache in order to detect changes that are still pending commit to the database.</span></span> <span data-ttu-id="718a2-349">이 프로세스는 개체 캐시에 매우 많은 개체가 있거나 개체 캐시에 추가 되는 큰 개체 그래프에 있는 경우 비용이 매우 클 수 있지만, 사용 하지 않도록 설정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-349">This process can be very expensive if there are a very large number of objects in the object cache or in a large object graph being added to the object cache, but it can also be disabled.</span></span> <span data-ttu-id="718a2-350">일부 경우에는 자동 검색을 사용 하지 않도록 설정할 때 Find 메서드를 호출할 때의 차이점을 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-350">In certain cases, you may perceive over an order of magnitude of difference in calling the Find method when you disable auto detect changes.</span></span> <span data-ttu-id="718a2-351">하지만 개체가 실제로 캐시에 있는 경우와 데이터베이스에서 개체를 검색 해야 하는 경우에는 두 번째 크기를 인식 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-351">Yet a second order of magnitude is perceived when the object actually is in the cache versus when the object has to be retrieved from the database.</span></span> <span data-ttu-id="718a2-352">다음은 5000 엔터티를 로드 하 여 밀리초 단위로 표현 된 마이크로 벤치 마크 중 일부를 사용 하 여 측정 한 예제 그래프입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-352">Here is an example graph with measurements taken using some of our microbenchmarks, expressed in milliseconds, with a load of 5000 entities:</span></span>

<span data-ttu-id="718a2-353">![.Net 4.5 로그 크기](~/ef6/media/net45logscale.png ".net 4.5-로그") 눈금 간격</span><span class="sxs-lookup"><span data-stu-id="718a2-353">![.NET 4.5 logarithmic scale](~/ef6/media/net45logscale.png ".NET 4.5 - logarithmic scale")</span></span>

<span data-ttu-id="718a2-354">자동 검색을 사용 하지 않도록 설정 된 찾기의 예:</span><span class="sxs-lookup"><span data-stu-id="718a2-354">Example of Find with auto-detect changes disabled:</span></span>

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

<span data-ttu-id="718a2-355">Find 메서드를 사용할 때 고려해 야 할 사항은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-355">What you have to consider when using the Find method is:</span></span>

1.  <span data-ttu-id="718a2-356">개체가 캐시에 없으면 Find의 이점이 부정 되지만 구문은 키로 쿼리 하는 것 보다 간단 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-356">If the object is not in the cache the benefits of Find are negated, but the syntax is still simpler than a query by key.</span></span>
2.  <span data-ttu-id="718a2-357">자동 검색을 사용 하는 경우에는 찾기 방법의 비용이 한 크기 만큼 증가 하거나 모델의 복잡도와 개체 캐시에 있는 엔터티의 양에 따라 달라질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-357">If auto detect changes is enabled the cost of the Find method may increase by one order of magnitude, or even more depending on the complexity of your model and the amount of entities in your object cache.</span></span>

<span data-ttu-id="718a2-358">또한 Find는 찾으려는 엔터티만 반환 하 고, 개체 캐시에 아직 연결 되지 않은 경우 연결 된 엔터티를 자동으로 로드 하지 않는다는 점에 유의 하세요.</span><span class="sxs-lookup"><span data-stu-id="718a2-358">Also, keep in mind that Find only returns the entity you are looking for and it does not automatically loads its associated entities if they are not already in the object cache.</span></span> <span data-ttu-id="718a2-359">연결 된 엔터티를 검색 해야 하는 경우 즉시 로드와 함께 키로 쿼리를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-359">If you need to retrieve associated entities, you can use a query by key with eager loading.</span></span> <span data-ttu-id="718a2-360">자세한 내용은 \*\*8.1 지연 로드 및 즉시 로드 @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="718a2-360">For more information see **8.1 Lazy Loading vs. Eager Loading**.</span></span>

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a><span data-ttu-id="718a2-361">개체 캐시에 많은 엔터티가 있는 경우 성능 문제 3.1.2</span><span class="sxs-lookup"><span data-stu-id="718a2-361">3.1.2 Performance issues when the object cache has many entities</span></span>

<span data-ttu-id="718a2-362">개체 캐시를 사용 하면 Entity Framework의 전체 응답성을 높일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-362">The object cache helps to increase the overall responsiveness of Entity Framework.</span></span> <span data-ttu-id="718a2-363">그러나 개체 캐시가 대량의 엔터티를 로드 한 경우 추가, 제거, 찾기, 항목, SaveChanges 등의 특정 작업에 영향을 줄 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-363">However, when the object cache has a very large amount of entities loaded it may affect certain operations such as Add, Remove, Find, Entry, SaveChanges and more.</span></span> <span data-ttu-id="718a2-364">특히 DetectChanges에 대 한 호출을 트리거하는 작업은 매우 큰 개체 캐시에 의해 부정적인 영향을 받습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-364">In particular, operations that trigger a call to DetectChanges will be negatively affected by very large object caches.</span></span> <span data-ttu-id="718a2-365">DetectChanges는 개체 그래프를 개체 상태 관리자와 동기화 하 고 해당 성능이 개체 그래프의 크기에 따라 직접 결정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-365">DetectChanges synchronizes the object graph with the object state manager and its performance will determined directly by the size of the object graph.</span></span> <span data-ttu-id="718a2-366">DetectChanges에 대 한 자세한 내용은 [POCO 엔터티의 변경 내용 추적](https://msdn.microsoft.com/library/dd456848.aspx)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="718a2-366">For more information about DetectChanges, see [Tracking Changes in POCO Entities](https://msdn.microsoft.com/library/dd456848.aspx).</span></span>

<span data-ttu-id="718a2-367">Entity Framework 6을 사용 하는 경우 개발자는 컬렉션을 반복 하 고 인스턴스당 한 번 추가를 호출 하는 대신 DbSet에서 직접 AddRange 및 RemoveRange를 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-367">When using Entity Framework 6, developers are able to call AddRange and RemoveRange directly on a DbSet, instead of iterating on a collection and calling Add once per instance.</span></span> <span data-ttu-id="718a2-368">범위 메서드를 사용 하는 경우의 이점은 추가 된 각 엔터티에 대해 한 번이 아니라 전체 엔터티 집합에 대해 한 번만 DetectChanges 비용을 지불 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-368">The advantage of using the range methods is that the cost of DetectChanges is only paid once for the entire set of entities as opposed to once per each added entity.</span></span>

### <a name="32-query-plan-caching"></a><span data-ttu-id="718a2-369">3.2 쿼리 계획 캐싱</span><span class="sxs-lookup"><span data-stu-id="718a2-369">3.2 Query Plan Caching</span></span>

<span data-ttu-id="718a2-370">쿼리가 처음 실행 될 때 내부 계획 컴파일러를 통해 개념적 쿼리를 저장 명령으로 변환 합니다 (예: SQL Server에 대해 실행 될 때 실행 되는 T-sql).</span><span class="sxs-lookup"><span data-stu-id="718a2-370">The first time a query is executed, it goes through the internal plan compiler to translate the conceptual query into the store command (for example, the T-SQL which is executed when run against SQL Server).</span></span><span data-ttu-id="718a2-371">  쿼리 계획 캐싱이 설정 된 경우 다음에 쿼리를 실행할 때 계획 컴파일러를 우회 하 여 실행을 위해 쿼리 계획 캐시에서 직접 store 명령이 검색 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-371">  If query plan caching is enabled, the next time the query is executed the store command is retrieved directly from the query plan cache for execution, bypassing the plan compiler.</span></span>

<span data-ttu-id="718a2-372">쿼리 계획 캐시는 동일한 AppDomain 내의 ObjectContext 인스턴스 간에 공유 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-372">The query plan cache is shared across ObjectContext instances within the same AppDomain.</span></span> <span data-ttu-id="718a2-373">쿼리 계획 캐싱을 활용 하려면 ObjectContext 인스턴스를 보관할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-373">You don't need to hold onto an ObjectContext instance to benefit from query plan caching.</span></span>

#### <a name="321-some-notes-about-query-plan-caching"></a><span data-ttu-id="718a2-374">쿼리 계획 캐싱에 대 한 몇 가지 참고 사항 3.2.1</span><span class="sxs-lookup"><span data-stu-id="718a2-374">3.2.1 Some notes about Query Plan Caching</span></span>

-   <span data-ttu-id="718a2-375">쿼리 계획 캐시는 모든 쿼리 유형에 대해 공유 됩니다. Entity SQL, LINQ to Entities 및 CompiledQuery 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-375">The query plan cache is shared for all query types: Entity SQL, LINQ to Entities, and CompiledQuery objects.</span></span>
-   <span data-ttu-id="718a2-376">기본적으로 EntityCommand 또는 ObjectQuery를 통해 실행 되는지에 관계 없이 쿼리 계획 캐싱은 Entity SQL 쿼리에 대해 사용 하도록 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-376">By default, query plan caching is enabled for Entity SQL queries, whether executed through an EntityCommand or through an ObjectQuery.</span></span> <span data-ttu-id="718a2-377">.NET 4.5에서 Entity Framework의 LINQ to Entities 쿼리에 대해서도 기본적으로 사용 하도록 설정 되며, Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="718a2-377">It is also enabled by default for LINQ to Entities queries in Entity Framework on .NET 4.5, and in Entity Framework 6</span></span>
    -   <span data-ttu-id="718a2-378">EnablePlanCaching 속성 (EntityCommand 또는 ObjectQuery)을 false로 설정 하 여 쿼리 계획 캐싱을 사용 하지 않도록 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-378">Query plan caching can be disabled by setting the EnablePlanCaching property (on EntityCommand or ObjectQuery) to false.</span></span> <span data-ttu-id="718a2-379">예를 들어 다음과 같은 가치를 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-379">For example:</span></span>
``` csharp
                    var query = from customer in context.Customer
                                where customer.CustomerId == id
                                select new
                                {
                                    customer.CustomerId,
                                    customer.Name
                                };
                    ObjectQuery oQuery = query as ObjectQuery;
                    oQuery.EnablePlanCaching = false;
```
-   <span data-ttu-id="718a2-380">매개 변수가 있는 쿼리의 경우 매개 변수의 값을 변경 하면 캐시 된 쿼리가 계속 적중 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-380">For parameterized queries, changing the parameter's value will still hit the cached query.</span></span> <span data-ttu-id="718a2-381">그러나 매개 변수의 패싯 (예: 크기, 전체 자릿수 또는 소수 자릿수)을 변경 하면 캐시의 다른 항목에 도달 하 게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-381">But changing a parameter's facets (for example, size, precision, or scale) will hit a different entry in the cache.</span></span>
-   <span data-ttu-id="718a2-382">Entity SQL 사용 하는 경우 쿼리 문자열은 키의 일부입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-382">When using Entity SQL, the query string is part of the key.</span></span> <span data-ttu-id="718a2-383">쿼리가 기능적으로 동일한 경우에도 쿼리를 변경 하면 다른 캐시 항목이 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-383">Changing the query at all will result in different cache entries, even if the queries are functionally equivalent.</span></span> <span data-ttu-id="718a2-384">여기에는 대/소문자 또는 공백을 변경 하는 작업이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-384">This includes changes to casing or whitespace.</span></span>
-   <span data-ttu-id="718a2-385">LINQ를 사용 하는 경우 쿼리를 처리 하 여 키의 일부를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-385">When using LINQ, the query is processed to generate a part of the key.</span></span> <span data-ttu-id="718a2-386">따라서 LINQ 식을 변경 하면 다른 키가 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-386">Changing the LINQ expression will therefore generate a different key.</span></span>
-   <span data-ttu-id="718a2-387">기타 기술 제한이 적용 될 수 있습니다. 자세한 내용은 Autocompiled 쿼리를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="718a2-387">Other technical limitations may apply; see Autocompiled Queries for more details.</span></span>

#### <a name="322-cache-eviction-algorithm"></a><span data-ttu-id="718a2-388">3.2.2 캐시 제거 알고리즘</span><span class="sxs-lookup"><span data-stu-id="718a2-388">3.2.2      Cache eviction algorithm</span></span>

<span data-ttu-id="718a2-389">내부 알고리즘의 작동 방식을 이해 하면 쿼리 계획 캐싱을 사용 하거나 사용 하지 않도록 설정 하는 경우를 파악할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-389">Understanding how the internal algorithm works will help you figure out when to enable or disable query plan caching.</span></span> <span data-ttu-id="718a2-390">정리 알고리즘은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-390">The cleanup algorithm is as follows:</span></span>

1.  <span data-ttu-id="718a2-391">캐시에 설정 된 항목 수 (800)가 포함 되 면 주기적으로 (분당 한 번) 캐시를 스윕 하는 타이머가 시작 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-391">Once the cache contains a set number of entries (800), we start a timer that periodically (once-per-minute) sweeps the cache.</span></span>
2.  <span data-ttu-id="718a2-392">캐시를 사용 하는 동안 LFRU (가장 자주 사용 하지 않음)의 캐시에서 항목이 제거 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-392">During cache sweeps, entries are removed from the cache on a LFRU (Least frequently – recently used) basis.</span></span> <span data-ttu-id="718a2-393">이 알고리즘은 꺼낼 항목을 결정할 때 적중 횟수와 보존 기간을 모두 고려 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-393">This algorithm takes both hit count and age into account when deciding which entries are ejected.</span></span>
3.  <span data-ttu-id="718a2-394">각 캐시 스윕의 끝에는 캐시에 800 항목이 다시 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-394">At the end of each cache sweep, the cache again contains 800 entries.</span></span>

<span data-ttu-id="718a2-395">모든 캐시 항목은 제거할 항목을 결정할 때 동일 하 게 처리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-395">All cache entries are treated equally when determining which entries to evict.</span></span> <span data-ttu-id="718a2-396">즉, CompiledQuery에 대 한 store 명령이 Entity SQL 쿼리에 대 한 저장소 명령과 동일한 제거 가능성을 갖습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-396">This means the store command for a CompiledQuery has the same chance of eviction as the store command for an Entity SQL query.</span></span>

<span data-ttu-id="718a2-397">캐시에 800 엔터티가 있는 경우 캐시 제거 타이머가 시작 되지만이 타이머가 시작 된 후에만 캐시가 스윕 60 초입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-397">Note that the cache eviction timer is kicked in when there are 800 entities in the cache, but the cache is only swept 60 seconds after this timer is started.</span></span> <span data-ttu-id="718a2-398">즉, 최대 60 초 동안 캐시가 상당히 커질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-398">That means that for up to 60 seconds your cache may grow to be quite large.</span></span>

#### <a name="323-test-metrics-demonstrating-query-plan-caching-performance"></a><span data-ttu-id="718a2-399">쿼리 계획 캐싱 성능을 보여 주는 3.2.3 테스트 메트릭</span><span class="sxs-lookup"><span data-stu-id="718a2-399">3.2.3       Test Metrics demonstrating query plan caching performance</span></span>

<span data-ttu-id="718a2-400">응용 프로그램의 성능에 대 한 쿼리 계획 캐싱의 영향을 보여 주기 위해 Navision 모델에 대해 많은 Entity SQL 쿼리를 실행 한 테스트를 수행 했습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-400">To demonstrate the effect of query plan caching on your application's performance, we performed a test where we executed a number of Entity SQL queries against the Navision model.</span></span> <span data-ttu-id="718a2-401">Navision 모델에 대 한 설명 및 실행 된 쿼리 유형에 대해서는 부록을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="718a2-401">See the appendix for a description of the Navision model and the types of queries which were executed.</span></span> <span data-ttu-id="718a2-402">이 테스트에서는 먼저 쿼리 목록을 반복 하 고 각 쿼리를 한 번 실행 하 여 캐시에 추가 합니다 (캐싱이 설정 된 경우).</span><span class="sxs-lookup"><span data-stu-id="718a2-402">In this test, we first iterate through the list of queries and execute each one once to add them to the cache (if caching is enabled).</span></span> <span data-ttu-id="718a2-403">이 단계는 시간이 초과 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-403">This step is untimed.</span></span> <span data-ttu-id="718a2-404">다음으로, 캐시 비우기가 수행 될 수 있도록 60 초 넘게 주 스레드를 대기 시킵니다. 마지막으로 캐시 된 쿼리를 실행 하기 위해 목록 전체를 반복 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-404">Next, we sleep the main thread for over 60 seconds to allow cache sweeping to take place; finally, we iterate through the list a 2nd time to execute the cached queries.</span></span> <span data-ttu-id="718a2-405">또한 각 쿼리 집합이 실행 되기 전에 SQL Server 계획 캐시가 플러시되고 쿼리 계획 캐시에서 제공 하는 혜택을 정확 하 게 반영 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-405">Additionally, the SQL Server plan cache is flushed before each set of queries is executed so that the times we obtain accurately reflect the benefit given by the query plan cache.</span></span>

##### <a name="3231-test-results"></a><span data-ttu-id="718a2-406">3.2.3.1 테스트 결과</span><span class="sxs-lookup"><span data-stu-id="718a2-406">3.2.3.1       Test Results</span></span>

| <span data-ttu-id="718a2-407">테스트</span><span class="sxs-lookup"><span data-stu-id="718a2-407">Test</span></span>                                                                   | <span data-ttu-id="718a2-408">캐시 없음 EF5</span><span class="sxs-lookup"><span data-stu-id="718a2-408">EF5 no cache</span></span> | <span data-ttu-id="718a2-409">EF5 캐시 됨</span><span class="sxs-lookup"><span data-stu-id="718a2-409">EF5 cached</span></span> | <span data-ttu-id="718a2-410">캐시 없음 EF6</span><span class="sxs-lookup"><span data-stu-id="718a2-410">EF6 no cache</span></span> | <span data-ttu-id="718a2-411">EF6 캐시 됨</span><span class="sxs-lookup"><span data-stu-id="718a2-411">EF6 cached</span></span> |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| <span data-ttu-id="718a2-412">모든 18723 쿼리 열거</span><span class="sxs-lookup"><span data-stu-id="718a2-412">Enumerating all 18723 queries</span></span>                                          | <span data-ttu-id="718a2-413">124</span><span class="sxs-lookup"><span data-stu-id="718a2-413">124</span></span>          | <span data-ttu-id="718a2-414">125.4</span><span class="sxs-lookup"><span data-stu-id="718a2-414">125.4</span></span>      | <span data-ttu-id="718a2-415">124.3</span><span class="sxs-lookup"><span data-stu-id="718a2-415">124.3</span></span>        | <span data-ttu-id="718a2-416">125.3</span><span class="sxs-lookup"><span data-stu-id="718a2-416">125.3</span></span>      |
| <span data-ttu-id="718a2-417">스윕 방지 (복잡성에 관계 없이 첫 번째 800 쿼리만)</span><span class="sxs-lookup"><span data-stu-id="718a2-417">Avoiding sweep (just the first 800 queries, regardless of complexity)</span></span>  | <span data-ttu-id="718a2-418">41.7</span><span class="sxs-lookup"><span data-stu-id="718a2-418">41.7</span></span>         | <span data-ttu-id="718a2-419">5.5</span><span class="sxs-lookup"><span data-stu-id="718a2-419">5.5</span></span>        | <span data-ttu-id="718a2-420">40.5</span><span class="sxs-lookup"><span data-stu-id="718a2-420">40.5</span></span>         | <span data-ttu-id="718a2-421">5.4</span><span class="sxs-lookup"><span data-stu-id="718a2-421">5.4</span></span>        |
| <span data-ttu-id="718a2-422">AggregatingSubtotals 쿼리만 (178 total-sweep을 방지 하는)</span><span class="sxs-lookup"><span data-stu-id="718a2-422">Just the AggregatingSubtotals queries (178 total - which avoids sweep)</span></span> | <span data-ttu-id="718a2-423">39.5</span><span class="sxs-lookup"><span data-stu-id="718a2-423">39.5</span></span>         | <span data-ttu-id="718a2-424">4.5</span><span class="sxs-lookup"><span data-stu-id="718a2-424">4.5</span></span>        | <span data-ttu-id="718a2-425">38.1</span><span class="sxs-lookup"><span data-stu-id="718a2-425">38.1</span></span>         | <span data-ttu-id="718a2-426">4.6</span><span class="sxs-lookup"><span data-stu-id="718a2-426">4.6</span></span>        |

<span data-ttu-id="718a2-427">*모든 시간 (초)입니다.*</span><span class="sxs-lookup"><span data-stu-id="718a2-427">*All times in seconds.*</span></span>

<span data-ttu-id="718a2-428">도덕적-여러 개의 고유 쿼리를 실행 하는 경우 (예: 동적으로 생성 된 쿼리) 캐싱이 도움이 되지 않으며 캐시의 결과로 생성 되는 캐시를 사용 하면 실제로 계획 캐싱에 가장 유용한 쿼리를 유지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-428">Moral - when executing lots of distinct queries (for example,  dynamically created queries), caching doesn't help and the resulting flushing of the cache can keep the queries that would benefit the most from plan caching from actually using it.</span></span>

<span data-ttu-id="718a2-429">AggregatingSubtotals 쿼리는를 사용 하 여 테스트 한 쿼리와 가장 복잡 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-429">The AggregatingSubtotals queries are the most complex of the queries we tested with.</span></span> <span data-ttu-id="718a2-430">예상 대로 쿼리가 복잡할수록 쿼리 계획 캐싱에서 더 많은 이점을 얻게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-430">As expected, the more complex the query is, the more benefit you will see from query plan caching.</span></span>

<span data-ttu-id="718a2-431">CompiledQuery는 계획을 캐시 하는 LINQ 쿼리 이기 때문에 CompiledQuery와 동등한 Entity SQL 쿼리의 비교 결과는 유사 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-431">Because a CompiledQuery is really a LINQ query with its plan cached, the comparison of a CompiledQuery versus the equivalent Entity SQL query should have similar results.</span></span> <span data-ttu-id="718a2-432">실제로 응용 프로그램에 동적 Entity SQL 쿼리가 많이 있는 경우 캐시를 쿼리를 사용 하 여 채우면 캐시에서 플러시된 경우에도 CompiledQueries이 "디컴파일"로 인해 효과적으로 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-432">In fact, if an app has lots of dynamic Entity SQL queries, filling the cache with queries will also effectively cause CompiledQueries to “decompile” when they are flushed from the cache.</span></span> <span data-ttu-id="718a2-433">이 시나리오에서는 동적 쿼리에서 캐싱을 사용 하지 않도록 설정 하 여 CompiledQueries의 우선 순위를 지정 함으로써 성능을 향상 시킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-433">In this scenario, performance may be improved by disabling caching on the dynamic queries to prioritize the CompiledQueries.</span></span> <span data-ttu-id="718a2-434">물론, 아직 동적 쿼리 대신 매개 변수가 있는 쿼리를 사용 하도록 앱을 다시 작성 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-434">Better yet, of course, would be to rewrite the app to use parameterized queries instead of dynamic queries.</span></span>

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a><span data-ttu-id="718a2-435">3.3 LINQ 쿼리를 사용 하 여 성능을 향상 시키기 위해 CompiledQuery 사용</span><span class="sxs-lookup"><span data-stu-id="718a2-435">3.3 Using CompiledQuery to improve performance with LINQ queries</span></span>

<span data-ttu-id="718a2-436">테스트 결과, CompiledQuery를 사용 하면 autocompiled LINQ 쿼리를 통해 7%의 이점을 누릴 수 있습니다. 즉, Entity Framework 스택에서 코드를 실행 하는 데 시간을 7% 줄일 수 있습니다. 응용 프로그램이 7% 더 빠르게 수행 되는 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-436">Our tests indicate that using CompiledQuery can bring a benefit of 7% over autocompiled LINQ queries; this means that you’ll spend 7% less time executing code from the Entity Framework stack; it does not mean your application will be 7% faster.</span></span> <span data-ttu-id="718a2-437">일반적으로 EF 5.0에서 CompiledQuery 개체를 작성 하 고 유지 관리 하는 데 드는 비용은 이점과 비교할 때 문제가 되지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-437">Generally speaking, the cost of writing and maintaining CompiledQuery objects in EF 5.0 may not be worth the trouble when compared to the benefits.</span></span> <span data-ttu-id="718a2-438">진행이 다를 수 있으므로 프로젝트에 추가 푸시가 필요한 경우이 옵션을 연습 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-438">Your mileage may vary, so exercise this option if your project requires the extra push.</span></span> <span data-ttu-id="718a2-439">CompiledQueries는 ObjectContext 파생 모델과만 호환 되며 DbContext 파생 모델과 호환 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-439">Note that CompiledQueries are only compatible with ObjectContext-derived models, and not compatible with DbContext-derived models.</span></span>

<span data-ttu-id="718a2-440">CompiledQuery을 만들고 호출 하는 방법에 대 한 자세한 내용은 [컴파일된 쿼리 (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="718a2-440">For more information on creating and invoking a CompiledQuery, see [Compiled Queries (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).</span></span>

<span data-ttu-id="718a2-441">CompiledQuery를 사용할 때 수행 해야 하는 두 가지 고려 사항이 있습니다. 즉, 정적 인스턴스를 사용 하는 데 필요한 요구 사항 및 composability를 사용 하 여 발생 하는 문제입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-441">There are two considerations you have to take when using a CompiledQuery, namely the requirement to use static instances and the problems they have with composability.</span></span> <span data-ttu-id="718a2-442">다음은 이러한 두 가지 고려 사항에 대 한 자세한 설명입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-442">Here follows an in-depth explanation of these two considerations.</span></span>

#### <a name="331-use-static-compiledquery-instances"></a><span data-ttu-id="718a2-443">3.3.1 Use static CompiledQuery instances</span><span class="sxs-lookup"><span data-stu-id="718a2-443">3.3.1       Use static CompiledQuery instances</span></span>

<span data-ttu-id="718a2-444">LINQ 쿼리를 컴파일하면 시간이 많이 소요 되기 때문에 데이터베이스에서 데이터를 인출 해야 할 때마다이 작업을 수행 하지 않으려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-444">Since compiling a LINQ query is a time-consuming process, we don’t want to do it every time we need to fetch data from the database.</span></span> <span data-ttu-id="718a2-445">CompiledQuery 인스턴스를 사용 하면 한 번만 컴파일하거나 여러 번 실행할 수 있지만, 다시 컴파일하는 대신 매번 동일한 CompiledQuery 인스턴스를 다시 사용 하려면 신중 하 게 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-445">CompiledQuery instances allow you to compile once and run multiple times, but you have to be careful and procure to re-use the same CompiledQuery instance every time instead of compiling it over and over again.</span></span> <span data-ttu-id="718a2-446">CompiledQuery 인스턴스를 저장 하는 데 정적 멤버를 사용 해야 합니다. 그렇지 않으면 어떤 이점도 표시 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-446">The use of static members to store the CompiledQuery instances becomes necessary; otherwise you won’t see any benefit.</span></span>

<span data-ttu-id="718a2-447">예를 들어, 선택한 범주에 대 한 제품 표시를 처리 하는 다음 메서드 본문이 페이지에 있다고 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-447">For example, suppose your page has the following method body to handle displaying the products for the selected category:</span></span>

``` csharp
    // Warning: this is the wrong way of using CompiledQuery
    using (NorthwindEntities context = new NorthwindEntities())
    {
        string selectedCategory = this.categoriesList.SelectedValue;

        var productsForCategory = CompiledQuery.Compile<NorthwindEntities, string, IQueryable<Product>>(
            (NorthwindEntities nwnd, string category) =>
                nwnd.Products.Where(p => p.Category.CategoryName == category)
        );

        this.productsGrid.DataSource = productsForCategory.Invoke(context, selectedCategory).ToList();
        this.productsGrid.DataBind();
    }

    this.productsGrid.Visible = true;
```

<span data-ttu-id="718a2-448">이 경우 메서드가 호출 될 때마다 즉석에서 새 CompiledQuery 인스턴스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-448">In this case, you will create a new CompiledQuery instance on the fly every time the method is called.</span></span> <span data-ttu-id="718a2-449">쿼리 계획 캐시에서 저장소 명령을 검색 하 여 성능상의 이점을 확인 하는 대신 새 인스턴스를 만들 때마다 CompiledQuery가 계획 컴파일러를 진행 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-449">Instead of seeing performance benefits by retrieving the store command from the query plan cache, the CompiledQuery will go through the plan compiler every time a new instance is created.</span></span> <span data-ttu-id="718a2-450">실제로 메서드를 호출할 때마다 새 CompiledQuery 항목을 사용 하 여 쿼리 계획 캐시에 polluting 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-450">In fact, you will be polluting your query plan cache with a new CompiledQuery entry every time the method is called.</span></span>

<span data-ttu-id="718a2-451">대신, 컴파일된 쿼리의 정적 인스턴스를 만들려는 경우 메서드가 호출 될 때마다 동일한 컴파일된 쿼리를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-451">Instead, you want to create a static instance of the compiled query, so you are invoking the same compiled query every time the method is called.</span></span> <span data-ttu-id="718a2-452">이를 위한 한 가지 방법은 개체 컨텍스트의 멤버로 CompiledQuery 인스턴스를 추가 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-452">One way to so this is by adding the CompiledQuery instance as a member of your object context.</span></span><span data-ttu-id="718a2-453">  그런 다음 도우미 메서드를 통해 CompiledQuery에 액세스 하 여 작업을 간단 하 게 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-453">  You can then make things a little cleaner by accessing the CompiledQuery through a helper method:</span></span>

``` csharp
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IEnumerable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
            );

        public IEnumerable<Product> GetProductsForCategory(string categoryName)
        {
            return productsForCategoryCQ.Invoke(this, categoryName).ToList();
        }
```

<span data-ttu-id="718a2-454">이 도우미 메서드는 다음과 같이 호출 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-454">This helper method would be invoked as follows:</span></span>

``` csharp
    this.productsGrid.DataSource = context.GetProductsForCategory(selectedCategory);
```

#### <a name="332-composing-over-a-compiledquery"></a><span data-ttu-id="718a2-455">CompiledQuery에 대 한 3.3.2 작성</span><span class="sxs-lookup"><span data-stu-id="718a2-455">3.3.2       Composing over a CompiledQuery</span></span>

<span data-ttu-id="718a2-456">LINQ 쿼리를 통해 작성 하는 기능은 매우 유용 합니다. 이렇게 하려면 *Skip ()* 또는 *Count ()* 와 같은 IQueryable 후에 메서드를 호출 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-456">The ability to compose over any LINQ query is extremely useful; to do this, you simply invoke a method after the IQueryable such as *Skip()* or *Count()*.</span></span> <span data-ttu-id="718a2-457">그러나 이렇게 하면 기본적으로 새 IQueryable 개체가 반환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-457">However, doing so essentially returns a new IQueryable object.</span></span> <span data-ttu-id="718a2-458">기술적으로 CompiledQuery을 통해 작성 하지 않아도 되는 것은 아니지만, 이렇게 하면 계획 컴파일러를 통해 다시 전달 해야 하는 새 IQueryable 개체가 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-458">While there’s nothing to stop you technically from composing over a CompiledQuery, doing so will cause the generation of a new IQueryable object that requires passing through the plan compiler again.</span></span>

<span data-ttu-id="718a2-459">일부 구성 요소는 구성 된 IQueryable 개체를 사용 하 여 고급 기능을 사용 하도록 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-459">Some components will make use of composed IQueryable objects to enable advanced functionality.</span></span> <span data-ttu-id="718a2-460">예: ASP. 네트워크 GridView는 SelectMethod 속성을 통해 IQueryable 개체에 데이터 바인딩할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-460">For example, ASP.NET’s GridView can be data-bound to an IQueryable object via the SelectMethod property.</span></span> <span data-ttu-id="718a2-461">그러면 GridView가이 IQueryable 개체를 통해 데이터 모델에 대 한 정렬과 페이징을 수행할 수 있도록 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-461">The GridView will then compose over this IQueryable object to allow sorting and paging over the data model.</span></span> <span data-ttu-id="718a2-462">여기에서 볼 수 있듯이 GridView에 대해 CompiledQuery를 사용 하면 컴파일된 쿼리에 적중 되지 않지만 새로운 autocompiled 쿼리가 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-462">As you can see, using a CompiledQuery for the GridView would not hit the compiled query but would generate a new autocompiled query.</span></span>

<span data-ttu-id="718a2-463">이를 실행할 수 있는 한 가지 위치는 쿼리에 프로그레시브 필터를 추가 하는 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-463">One place where you may run into this is when adding progressive filters to a query.</span></span> <span data-ttu-id="718a2-464">예를 들어, 선택 사항인 필터 (예: Country 및 OrdersCount)에 대 한 몇 가지 드롭다운 목록을 포함 하는 고객 페이지가 있다고 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-464">For example, suppose you had a Customers page with several drop-down lists for optional filters (for example, Country and OrdersCount).</span></span> <span data-ttu-id="718a2-465">CompiledQuery의 IQueryable 결과에 대해 이러한 필터를 작성할 수 있지만 이렇게 하면 새 쿼리가 실행 될 때마다 계획 컴파일러를 통해 새 쿼리가 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-465">You can compose these filters over the IQueryable results of a CompiledQuery, but doing so will result in the new query going through the plan compiler every time you execute it.</span></span>

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployee();

        if (this.orderCountFilterList.SelectedItem.Value != defaultFilterText)
        {
            int orderCount = int.Parse(orderCountFilterList.SelectedValue);
            myCustomers = myCustomers.Where(c => c.Orders.Count > orderCount);
        }

        if (this.countryFilterList.SelectedItem.Value != defaultFilterText)
        {
            myCustomers = myCustomers.Where(c => c.Address.Country == countryFilterList.SelectedValue);
        }

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 <span data-ttu-id="718a2-466">이 다시 컴파일이 발생 하지 않도록 하려면 CompiledQuery를 다시 작성 하 여 가능한 필터를 고려해 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-466">To avoid this re-compilation, you can rewrite the CompiledQuery to take the possible filters into account:</span></span>

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

<span data-ttu-id="718a2-467">다음과 같은 UI에서 호출 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-467">Which would be invoked in the UI like:</span></span>

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        int? countFilter = (this.orderCountFilterList.SelectedIndex == 0) ?
            (int?)null :
            int.Parse(this.orderCountFilterList.SelectedValue);

        string countryFilter = (this.countryFilterList.SelectedIndex == 0) ?
            null :
            this.countryFilterList.SelectedValue;

        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployeeWithFilters(
                countFilter, countryFilter);

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 <span data-ttu-id="718a2-468">여기서의 단점은 생성 된 저장소 명령은 항상 null 검사를 포함 하는 필터를 포함 하지만 데이터베이스 서버가 최적화 되려면 매우 간단 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-468">A tradeoff here is the generated store command will always have the filters with the null checks, but these should be fairly simple for the database server to optimize:</span></span>

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a><span data-ttu-id="718a2-469">3.4 메타 데이터 캐싱</span><span class="sxs-lookup"><span data-stu-id="718a2-469">3.4 Metadata caching</span></span>

<span data-ttu-id="718a2-470">Entity Framework는 메타 데이터 캐싱도 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-470">The Entity Framework also supports Metadata caching.</span></span> <span data-ttu-id="718a2-471">이는 기본적으로 동일한 모델에 대 한 여러 연결에서 유형 정보 및 데이터베이스 간 매핑 정보를 캐싱하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-471">This is essentially caching of type information and type-to-database mapping information across different connections to the same model.</span></span> <span data-ttu-id="718a2-472">메타 데이터 캐시는 AppDomain 마다 고유 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-472">The Metadata cache is unique per AppDomain.</span></span>

#### <a name="341-metadata-caching-algorithm"></a><span data-ttu-id="718a2-473">3.4.1 메타 데이터 캐싱 알고리즘</span><span class="sxs-lookup"><span data-stu-id="718a2-473">3.4.1 Metadata Caching algorithm</span></span>

1.  <span data-ttu-id="718a2-474">모델에 대 한 메타 데이터 정보는 각 EntityConnection에 대해 ItemCollection에 저장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-474">Metadata information for a model is stored in an ItemCollection for each EntityConnection.</span></span>
    -   <span data-ttu-id="718a2-475">참고로 모델의 각 부분에 대해 서로 다른 ItemCollection 개체가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-475">As a side note, there are different ItemCollection objects for different parts of the model.</span></span> <span data-ttu-id="718a2-476">예를 들어, 데이터 모델에 대 한 정보를 포함 하는 데이터 집합 Itemcollections ObjectItemCollection에는 데이터 모델에 대 한 정보가 포함 되어 있습니다. EdmItemCollection에는 개념적 모델에 대 한 정보가 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-476">For example, StoreItemCollections contains the information about the database model; ObjectItemCollection contains information about the data model; EdmItemCollection contains information about the conceptual model.</span></span>

2.  <span data-ttu-id="718a2-477">두 연결이 동일한 연결 문자열을 사용 하는 경우 동일한 ItemCollection 인스턴스를 공유 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-477">If two connections use the same connection string, they will share the same ItemCollection instance.</span></span>
3.  <span data-ttu-id="718a2-478">기능적으로 동일 하지만 서로 다른 연결 문자열은 메타 데이터 캐시를 발생 시킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-478">Functionally equivalent but textually different connection strings may result in different metadata caches.</span></span> <span data-ttu-id="718a2-479">연결 문자열을 토큰화 때문에 토큰의 순서를 변경 하면 공유 메타 데이터를 생성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-479">We do tokenize connection strings, so simply changing the order of the tokens should result in shared metadata.</span></span> <span data-ttu-id="718a2-480">하지만 기능적으로 보이는 두 연결 문자열은 토큰화 후 동일 하 게 계산 되지 않을 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-480">But two connection strings that seem functionally the same may not be evaluated as identical after tokenization.</span></span>
4.  <span data-ttu-id="718a2-481">ItemCollection은 주기적으로 사용 하기 위해 확인 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-481">The ItemCollection is periodically checked for use.</span></span> <span data-ttu-id="718a2-482">작업 영역에 최근에 액세스 하지 않은 것으로 확인 되 면 다음 캐시 스윕에서 정리 하도록 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-482">If it is determined that a workspace has not been accessed recently, it will be marked for cleanup on the next cache sweep.</span></span>
5.  <span data-ttu-id="718a2-483">EntityConnection을 만들면 메타 데이터 캐시가 생성 됩니다 .이 경우에는 연결을 열 때까지 항목 컬렉션이 초기화 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-483">Merely creating an EntityConnection will cause a metadata cache to be created (though the item collections in it will not be initialized until the connection is opened).</span></span> <span data-ttu-id="718a2-484">이 작업 영역은 캐싱 알고리즘이 "사용 도중"이 아닌 것으로 확인 될 때까지 메모리에 유지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-484">This workspace will remain in-memory until the caching algorithm determines it is not “in use”.</span></span>

<span data-ttu-id="718a2-485">고객 자문 팀 설명 큰 모델을 사용 하는 경우 "사용 중단"를 방지 하기 위해 ItemCollection에 대 한 참조를 보유 하는 블로그 게시물을 작성 했습니다: \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx> 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-485">The Customer Advisory Team has written a blog post that describes holding a reference to an ItemCollection in order to avoid "deprecation" when using large models: \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.</span></span>

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a><span data-ttu-id="718a2-486">3.4.2 메타 데이터 캐싱 및 쿼리 계획 캐싱 간의 관계</span><span class="sxs-lookup"><span data-stu-id="718a2-486">3.4.2 The relationship between Metadata Caching and Query Plan Caching</span></span>

<span data-ttu-id="718a2-487">쿼리 계획 캐시 인스턴스는 MetadataWorkspace의 저장소 형식 ItemCollection에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-487">The query plan cache instance lives in the MetadataWorkspace's ItemCollection of store types.</span></span> <span data-ttu-id="718a2-488">이는 지정 된 MetadataWorkspace를 사용 하 여 인스턴스화된 모든 컨텍스트에 대 한 쿼리에 캐시 된 저장소 명령이 사용 됨을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-488">This means that cached store commands will be used for queries against any context instantiated using a given MetadataWorkspace.</span></span> <span data-ttu-id="718a2-489">또한 두 개의 연결 문자열이 약간 다르고 토큰화 이후 일치 하지 않는 경우 다른 쿼리 계획 캐시 인스턴스가 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-489">It also means that if you have two connections strings that are slightly different and don't match after tokenizing, you will have different query plan cache instances.</span></span>

### <a name="35-results-caching"></a><span data-ttu-id="718a2-490">3.5 결과 캐싱</span><span class="sxs-lookup"><span data-stu-id="718a2-490">3.5 Results caching</span></span>

<span data-ttu-id="718a2-491">결과 캐싱 ("두 번째 수준 캐싱"이 라고도 함)을 사용 하면 쿼리 결과를 로컬 캐시에 유지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-491">With results caching (also known as "second-level caching"), you keep the results of queries in a local cache.</span></span> <span data-ttu-id="718a2-492">쿼리를 실행 하는 경우 저장소에 대해 쿼리 하기 전에 먼저 결과가 로컬에서 사용 가능한 지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-492">When issuing a query, you first see if the results are available locally before you query against the store.</span></span> <span data-ttu-id="718a2-493">결과 캐싱은 Entity Framework에서 직접 지원 되지 않지만 래핑 공급자를 사용 하 여 두 번째 수준 캐시를 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-493">While results caching isn't directly supported by Entity Framework, it's possible to add a second level cache by using a wrapping provider.</span></span> <span data-ttu-id="718a2-494">두 번째 수준 캐시를 사용 하는 Alachisoft의 예제 래핑 공급자는 [NCache를 기반으로 하는 두 번째 수준 캐시 Entity Framework](https://www.alachisoft.com/ncache/entity-framework.html)입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-494">An example wrapping provider with a second-level cache is Alachisoft's [Entity Framework Second Level Cache based on NCache](https://www.alachisoft.com/ncache/entity-framework.html).</span></span>

<span data-ttu-id="718a2-495">두 번째 수준 캐싱 구현은 LINQ 식이 평가 (및 funcletized) 되 고 쿼리 실행 계획이 첫 번째 수준 캐시에서 계산 되거나 검색 된 후에 발생 하는 삽입 된 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-495">This implementation of second-level caching is an injected functionality that takes place after the LINQ expression has been evaluated (and funcletized) and the query execution plan is computed or retrieved from the first-level cache.</span></span> <span data-ttu-id="718a2-496">그런 다음 두 번째 수준 캐시는 원시 데이터베이스 결과만 저장 하므로 구체화 파이프라인은 나중에 계속 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-496">The second-level cache will then store only the raw database results, so the materialization pipeline still executes afterwards.</span></span>

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a><span data-ttu-id="718a2-497">래핑 공급자를 사용 하 여 결과 캐싱에 대 한 추가 참조 3.5.1</span><span class="sxs-lookup"><span data-stu-id="718a2-497">3.5.1 Additional references for results caching with the wrapping provider</span></span>

-   <span data-ttu-id="718a2-498">Julie Lerman는 Windows Server AppFabric 캐싱을 사용 하도록 샘플 래핑 공급자를 업데이트 하는 방법을 포함 하는 "두 번째 수준 캐싱 Entity Framework 및 Windows Azure" MSDN 문서를 작성 했습니다. [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)</span><span class="sxs-lookup"><span data-stu-id="718a2-498">Julie Lerman has written a "Second-Level Caching in Entity Framework and Windows Azure" MSDN article that includes how to update the sample wrapping provider to use Windows Server AppFabric caching: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)</span></span>
-   <span data-ttu-id="718a2-499">팀 블로그 Entity Framework 5에 대 한 캐싱 공급자와 함께 실행 하는 방법을 설명 하는 게시물에 Entity Framework 5를 사용 하 여 작업 하는 경우: \<http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx> 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-499">If you are working with Entity Framework 5, the team blog has a post that describes how to get things running with the caching provider for Entity Framework 5: \<http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>.</span></span> <span data-ttu-id="718a2-500">또한 프로젝트에 2 단계 캐싱 추가를 자동화 하는 데 도움이 되는 T4 템플릿도 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-500">It also includes a T4 template to help automate adding the 2nd-level caching to your project.</span></span>

## <a name="4-autocompiled-queries"></a><span data-ttu-id="718a2-501">자동 컴파일된 쿼리 4 개</span><span class="sxs-lookup"><span data-stu-id="718a2-501">4 Autocompiled Queries</span></span>

<span data-ttu-id="718a2-502">Entity Framework를 사용 하 여 데이터베이스에 대해 쿼리를 실행 하는 경우 결과를 실제로 구체화 하기 전에 일련의 단계를 수행 해야 합니다. 이러한 단계 중 하나는 쿼리 컴파일입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-502">When a query is issued against a database using Entity Framework, it must go through a series of steps before actually materializing the results; one such step is Query Compilation.</span></span> <span data-ttu-id="718a2-503">Entity SQL 쿼리는 자동으로 캐시 되기 때문에 성능이 좋은 것으로 알려져 있으므로 동일한 쿼리를 실행 하는 두 번째 또는 세 번째는 계획 컴파일러를 건너뛰고 캐시 된 계획을 대신 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-503">Entity SQL queries were known to have good performance as they are automatically cached, so the second or third time you execute the same query it can skip the plan compiler and use the cached plan instead.</span></span>

<span data-ttu-id="718a2-504">Entity Framework 5에서는 LINQ to Entities 쿼리를 위한 자동 캐싱도 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-504">Entity Framework 5 introduced automatic caching for LINQ to Entities queries as well.</span></span> <span data-ttu-id="718a2-505">이전 버전의 Entity Framework는 성능을 향상 시킬 수 있는 CompiledQuery를 만드는 것은 LINQ to Entities 쿼리를 캐시할 수 있기 때문에 일반적인 방법 이었습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-505">In past editions of Entity Framework creating a CompiledQuery to speed your performance was a common practice, as this would make your LINQ to Entities query cacheable.</span></span> <span data-ttu-id="718a2-506">이제 CompiledQuery를 사용 하지 않고 캐싱이 자동으로 수행 되므로 "autocompiled 쿼리" 기능을 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-506">Since caching is now done automatically without the use of a CompiledQuery, we call this feature “autocompiled queries”.</span></span> <span data-ttu-id="718a2-507">쿼리 계획 캐시 및 해당 메커니즘에 대 한 자세한 내용은 쿼리 계획 캐싱을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="718a2-507">For more information about the query plan cache and its mechanics, see Query Plan Caching.</span></span>

<span data-ttu-id="718a2-508">Entity Framework는 쿼리를 다시 컴파일해야 하는 경우를 감지 하 고 이전에 컴파일된 경우라도 쿼리를 호출할 때이를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-508">Entity Framework detects when a query requires to be recompiled, and does so when the query is invoked even if it had been compiled before.</span></span> <span data-ttu-id="718a2-509">쿼리가 다시 컴파일되는 일반적인 조건은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-509">Common conditions that cause the query to be recompiled are:</span></span>

-   <span data-ttu-id="718a2-510">쿼리에 연결 된 MergeOption 변경</span><span class="sxs-lookup"><span data-stu-id="718a2-510">Changing the MergeOption associated to your query.</span></span> <span data-ttu-id="718a2-511">캐시 된 쿼리를 사용 하지 않고, 대신 계획 컴파일러를 다시 실행 하 고 새로 만든 계획이 캐시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-511">The cached query will not be used, instead the plan compiler will run again and the newly created plan gets cached.</span></span>
-   <span data-ttu-id="718a2-512">ContextOptions의 값을 변경 합니다. UseCSharpNullComparisonBehavior.</span><span class="sxs-lookup"><span data-stu-id="718a2-512">Changing the value of ContextOptions.UseCSharpNullComparisonBehavior.</span></span> <span data-ttu-id="718a2-513">MergeOption을 변경 하는 것과 동일한 결과를 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-513">You get the same effect as changing the MergeOption.</span></span>

<span data-ttu-id="718a2-514">다른 조건으로 인해 쿼리에서 캐시를 사용 하지 못할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-514">Other conditions can prevent your query from using the cache.</span></span> <span data-ttu-id="718a2-515">일반적인 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-515">Common examples are:</span></span>

-   <span data-ttu-id="718a2-516">IEnumerable @ no__t-0T @ no__t-1을 사용 합니다. @ No__t-2 @ no__t (T 값)를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-516">Using IEnumerable&lt;T&gt;.Contains&lt;&gt;(T value).</span></span>
-   <span data-ttu-id="718a2-517">상수를 사용 하 여 쿼리를 생성 하는 함수 사용.</span><span class="sxs-lookup"><span data-stu-id="718a2-517">Using functions that produce queries with constants.</span></span>
-   <span data-ttu-id="718a2-518">매핑되지 않은 개체의 속성 사용</span><span class="sxs-lookup"><span data-stu-id="718a2-518">Using the properties of a non-mapped object.</span></span>
-   <span data-ttu-id="718a2-519">다시 컴파일해야 하는 다른 쿼리에 쿼리를 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-519">Linking your query to another query that requires to be recompiled.</span></span>

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a><span data-ttu-id="718a2-520">4.1 IEnumerable @ no__t-0T @ no__t-1을 사용 합니다. Contains @ no__t-2T @ no__t-3 (T 값)</span><span class="sxs-lookup"><span data-stu-id="718a2-520">4.1 Using IEnumerable&lt;T&gt;.Contains&lt;T&gt;(T value)</span></span>

<span data-ttu-id="718a2-521">Entity Framework는 IEnumerable @ no__t-0T @ no__t-1을 호출 하는 쿼리를 캐시 하지 않습니다. 컬렉션의 값이 volatile로 간주 되기 때문에 메모리 내 컬렉션에 대해 @ no__t-2T @ no__t-3 (T 값)을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-521">Entity Framework does not cache queries that invoke IEnumerable&lt;T&gt;.Contains&lt;T&gt;(T value) against an in-memory collection, since the values of the collection are considered volatile.</span></span> <span data-ttu-id="718a2-522">다음 예제 쿼리는 캐시 되지 않으므로 항상 계획 컴파일러에 의해 처리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-522">The following example query will not be cached, so it will always be processed by the plan compiler:</span></span>

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var query = context.MyEntities
                    .Where(entity => ids.Contains(entity.Id));

    var results = query.ToList();
    ...
}
```

<span data-ttu-id="718a2-523">Contains를 포함 하는 IEnumerable의 크기는 쿼리 컴파일 속도 또는 속도를 결정 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-523">Note that the size of the IEnumerable against which Contains is executed determines how fast or how slow your query is compiled.</span></span> <span data-ttu-id="718a2-524">위의 예제에 나와 있는 것과 같은 큰 컬렉션을 사용 하는 경우 성능이 크게 저하 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-524">Performance can suffer significantly when using large collections such as the one shown in the example above.</span></span>

<span data-ttu-id="718a2-525">Entity Framework 6에는 IEnumerable @ no__t-0T @ no__t-1 방법에 대 한 최적화가 포함 되어 있습니다. 쿼리가 실행 될 때 @ no__t-2T @ no__t-3 (T 값)이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-525">Entity Framework 6 contains optimizations to the way IEnumerable&lt;T&gt;.Contains&lt;T&gt;(T value) works when queries are executed.</span></span> <span data-ttu-id="718a2-526">생성 되는 SQL 코드는 생성 하 고 읽기는 훨씬 더 빠르며, 대부분의 경우 서버 에서도 더 빠르게 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-526">The SQL code that is generated is much faster to produce and more readable, and in most cases it also executes faster in the server.</span></span>

### <a name="42-using-functions-that-produce-queries-with-constants"></a><span data-ttu-id="718a2-527">4.2 상수를 사용 하 여 쿼리를 생성 하는 함수 사용</span><span class="sxs-lookup"><span data-stu-id="718a2-527">4.2 Using functions that produce queries with constants</span></span>

<span data-ttu-id="718a2-528">Skip (), Take (), Contains () 및 DefautIfEmpty () LINQ 연산자는 매개 변수를 사용 하 여 SQL 쿼리를 생성 하지 않고 대신 전달 되는 값을 상수로 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-528">The Skip(), Take(), Contains() and DefautIfEmpty() LINQ operators do not produce SQL queries with parameters but instead put the values passed to them as constants.</span></span> <span data-ttu-id="718a2-529">이로 인해 동일 하지 않을 수 있는 쿼리는 EF stack과 데이터베이스 서버 모두에서 쿼리 계획 캐시를 polluting 하 고 후속 쿼리 실행에서 동일한 상수를 사용 하지 않으면 다시 사용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-529">Because of this, queries that might otherwise be identical end up polluting the query plan cache, both on the EF stack and on the database server, and do not get reutilized unless the same constants are used in a subsequent query execution.</span></span> <span data-ttu-id="718a2-530">예를 들어 다음과 같은 가치를 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-530">For example:</span></span>

``` csharp
var id = 10;
...
using (var context = new MyContext())
{
    var query = context.MyEntities.Select(entity => entity.Id).Contains(id);

    var results = query.ToList();
    ...
}
```

<span data-ttu-id="718a2-531">이 예에서는이 쿼리가 실행 될 때마다 id에 대해 다른 값을 사용 하 여 쿼리를 새 계획으로 컴파일합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-531">In this example, each time this query is executed with a different value for id the query will be compiled into a new plan.</span></span>

<span data-ttu-id="718a2-532">특히 페이징을 수행할 때 Skip 및 Take 사용에 주의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-532">In particular pay attention to the use of Skip and Take when doing paging.</span></span> <span data-ttu-id="718a2-533">EF6에서 이러한 메서드에는 이러한 메서드에 전달 된 변수를 캡처하여 SQLparameters로 변환할 수 있기 때문에 캐시 된 쿼리 계획을 다시 사용할 수 있도록 하는 람다 오버 로드가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-533">In EF6 these methods have a lambda overload that effectively makes the cached query plan reusable because EF can capture variables passed to these methods and translate them to SQLparameters.</span></span> <span data-ttu-id="718a2-534">이를 통해 Skip 및 Take에 대해 다른 상수로 지정 된 각 쿼리는 자체 쿼리 계획 캐시 항목을 가져올 수 있으므로 캐시를 깔끔하게 유지할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-534">This also helps keep the cache cleaner since otherwise each query with a different constant for Skip and Take would get its own query plan cache entry.</span></span>

<span data-ttu-id="718a2-535">다음 코드는 최적화 되지 않지만이 클래스의 쿼리를 exemplify 하는 데만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-535">Consider the following code, which is suboptimal but is only meant to exemplify this class of queries:</span></span>

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

<span data-ttu-id="718a2-536">이 동일한 코드의 더 빠른 버전은 람다를 사용 하 여 Skip을 호출 하는 것과 관련이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-536">A faster version of this same code would involve calling Skip with a lambda:</span></span>

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

<span data-ttu-id="718a2-537">두 번째 조각은 쿼리가 실행 될 때마다 동일한 쿼리 계획이 사용 되므로 CPU 시간이 절약 되 고 쿼리 캐시의 polluting 방지 되기 때문에 최대 11%까지 더 빠르게 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-537">The second snippet may run up to 11% faster because the same query plan is used every time the query is run, which saves CPU time and avoids polluting the query cache.</span></span> <span data-ttu-id="718a2-538">또한 건너뛸 매개 변수는 클로저 이므로 코드는 이제 다음과 같이 표시 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-538">Furthermore, because the parameter to Skip is in a closure the code might as well look like this now:</span></span>

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a><span data-ttu-id="718a2-539">4.3 매핑되지 않은 개체의 속성 사용</span><span class="sxs-lookup"><span data-stu-id="718a2-539">4.3 Using the properties of a non-mapped object</span></span>

<span data-ttu-id="718a2-540">쿼리에서 매핑되지 않은 개체 형식의 속성을 매개 변수로 사용 하는 경우 쿼리가 캐시 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-540">When a query uses the properties of a non-mapped object type as a parameter then the query will not get cached.</span></span> <span data-ttu-id="718a2-541">예를 들어 다음과 같은 가치를 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-541">For example:</span></span>

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();

    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myObject.MyProperty)
                select entity;

   var results = query.ToList();
    ...
}
```

<span data-ttu-id="718a2-542">이 예제에서는 NonMappedType 클래스가 엔터티 모델의 일부가 아닌 것으로 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-542">In this example, assume that class NonMappedType is not part of the Entity model.</span></span> <span data-ttu-id="718a2-543">이 쿼리는 매핑되지 않은 형식을 사용 하지 않고 쿼리에 대 한 매개 변수로 지역 변수를 사용 하도록 쉽게 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-543">This query can easily be changed to not use a non-mapped type and instead use a local variable as the parameter to the query:</span></span>

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();
    var myValue = myObject.MyProperty;
    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myValue)
                select entity;

    var results = query.ToList();
    ...
}
```

<span data-ttu-id="718a2-544">이 경우 쿼리는 캐시 되 고 쿼리 계획 캐시에서 이점을 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-544">In this case, the query will be able to get cached and will benefit from the query plan cache.</span></span>

### <a name="44-linking-to-queries-that-require-recompiling"></a><span data-ttu-id="718a2-545">4.4 다시 컴파일하지 않아도 되는 쿼리에 연결</span><span class="sxs-lookup"><span data-stu-id="718a2-545">4.4 Linking to queries that require recompiling</span></span>

<span data-ttu-id="718a2-546">위와 동일한 예제를 사용 하는 경우 다시 컴파일해야 하는 쿼리를 사용 하는 두 번째 쿼리가 있으면 전체 두 번째 쿼리도 다시 컴파일됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-546">Following the same example as above, if you have a second query that relies on a query that needs to be recompiled, your entire second query will also be recompiled.</span></span> <span data-ttu-id="718a2-547">이 시나리오를 보여 주는 예제는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-547">Here’s an example to illustrate this scenario:</span></span>

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var firstQuery = from entity in context.MyEntities
                        where ids.Contains(entity.Id)
                        select entity;

    var secondQuery = from entity in context.MyEntities
                        where firstQuery.Any(otherEntity => otherEntity.Id == entity.Id)
                        select entity;

    var results = secondQuery.ToList();
    ...
}
```

<span data-ttu-id="718a2-548">이 예는 일반적 이지만 firstQuery에 연결 하 여 secondQuery를 캐시 하지 못하게 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-548">The example is generic, but it illustrates how linking to firstQuery is causing secondQuery to be unable to get cached.</span></span> <span data-ttu-id="718a2-549">FirstQuery가 다시 컴파일해야 하는 쿼리가 아니면 secondQuery가 캐시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-549">If firstQuery had not been a query that requires recompiling, then secondQuery would have been cached.</span></span>

## <a name="5-notracking-queries"></a><span data-ttu-id="718a2-550">5 NoTracking 쿼리</span><span class="sxs-lookup"><span data-stu-id="718a2-550">5 NoTracking Queries</span></span>

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a><span data-ttu-id="718a2-551">5.1 상태 관리 오버 헤드를 줄이기 위해 변경 내용 추적을 사용 하지 않도록 설정</span><span class="sxs-lookup"><span data-stu-id="718a2-551">5.1 Disabling change tracking to reduce state management overhead</span></span>

<span data-ttu-id="718a2-552">읽기 전용 시나리오에서 개체를 ObjectStateManager로 로드 하는 오버 헤드를 방지 하려는 경우 "추적 안 함" 쿼리를 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-552">If you are in a read-only scenario and want to avoid the overhead of loading the objects into the ObjectStateManager, you can issue "No Tracking" queries.</span></span><span data-ttu-id="718a2-553">  변경 내용 추적은 쿼리 수준에서 사용 하지 않도록 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-553">  Change tracking can be disabled at the query level.</span></span>

<span data-ttu-id="718a2-554">그러나 변경 내용 추적을 사용 하지 않도록 설정 하면 개체 캐시가 효과적으로 해제 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-554">Note though that by disabling change tracking you are effectively turning off the object cache.</span></span> <span data-ttu-id="718a2-555">엔터티를 쿼리하면 이전에는 ObjectStateManager에서 구체화 된 쿼리 결과를 끌어 구체화를 건너뛸 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-555">When you query for an entity, we can't skip materialization by pulling the previously-materialized query results from the ObjectStateManager.</span></span> <span data-ttu-id="718a2-556">동일한 컨텍스트에서 동일한 엔터티에 대해 반복적으로 쿼리 하는 경우 변경 내용 추적을 사용 하도록 설정 하면 성능상의 이점을 실제로 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-556">If you are repeatedly querying for the same entities on the same context, you might actually see a performance benefit from enabling change tracking.</span></span>

<span data-ttu-id="718a2-557">ObjectContext를 사용 하 여 쿼리 하는 경우 ObjectQuery 및 ObjectSet 인스턴스는 설정 된 후 MergeOption을 사용 하 고이를 구성 하는 쿼리는 부모 쿼리의 유효 MergeOption을 상속 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-557">When querying using ObjectContext, ObjectQuery and ObjectSet instances will remember a MergeOption once it is set, and queries that are composed on them will inherit the effective MergeOption of the parent query.</span></span> <span data-ttu-id="718a2-558">DbContext를 사용 하는 경우 DbSet에서 AsNoTracking () 한정자를 호출 하 여 추적을 사용 하지 않도록 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-558">When using DbContext, tracking can be disabled by calling the AsNoTracking() modifier on the DbSet.</span></span>

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a><span data-ttu-id="718a2-559">5.1.1 DbContext를 사용 하는 경우 쿼리에 대 한 변경 내용 추적 해제</span><span class="sxs-lookup"><span data-stu-id="718a2-559">5.1.1 Disabling change tracking for a query when using DbContext</span></span>

<span data-ttu-id="718a2-560">쿼리의 AsNoTracking () 메서드에 대 한 호출을 연결 하 여 쿼리 모드를 NoTracking로 전환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-560">You can switch the mode of a query to NoTracking by chaining a call to the AsNoTracking() method in the query.</span></span> <span data-ttu-id="718a2-561">ObjectQuery와 달리 DbContext API의 DbSet 및 Dbset 클래스에는 MergeOption에 대 한 mutable 속성이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-561">Unlike ObjectQuery, the DbSet and DbQuery classes in the DbContext API don’t have a mutable property for the MergeOption.</span></span>

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a><span data-ttu-id="718a2-562">5.1.2 ObjectContext를 사용 하 여 쿼리 수준에서 변경 내용 추적 해제</span><span class="sxs-lookup"><span data-stu-id="718a2-562">5.1.2 Disabling change tracking at the query level using ObjectContext</span></span>

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a><span data-ttu-id="718a2-563">5.1.3 ObjectContext를 사용 하 여 전체 엔터티 집합에 대 한 변경 내용 추적 해제</span><span class="sxs-lookup"><span data-stu-id="718a2-563">5.1.3 Disabling change tracking for an entire entity set using ObjectContext</span></span>

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a><span data-ttu-id="718a2-564">5.2 쿼리 NoTracking의 성능 이점을 보여 주는 테스트 메트릭</span><span class="sxs-lookup"><span data-stu-id="718a2-564">5.2 Test Metrics demonstrating the performance benefit of NoTracking queries</span></span>

<span data-ttu-id="718a2-565">이 테스트에서는 Navision 모델에 대 한 추적을 NoTracking 쿼리와 비교 하 여 ObjectStateManager를 채우는 비용을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-565">In this test we look at the cost of filling the ObjectStateManager by comparing Tracking to NoTracking queries for the Navision model.</span></span> <span data-ttu-id="718a2-566">Navision 모델에 대 한 설명 및 실행 된 쿼리 유형에 대해서는 부록을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="718a2-566">See the appendix for a description of the Navision model and the types of queries which were executed.</span></span> <span data-ttu-id="718a2-567">이 테스트에서는 쿼리 목록을 반복 하 고 각 쿼리를 한 번 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-567">In this test, we iterate through the list of queries and execute each one once.</span></span> <span data-ttu-id="718a2-568">NoTracking 쿼리를 사용 하 고 "AppendOnly"의 기본 병합 옵션을 사용 하 여 한 번 테스트의 두 가지 변형을 실행 했습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-568">We ran two variations of the test, once with NoTracking queries and once with the default merge option of "AppendOnly".</span></span> <span data-ttu-id="718a2-569">각 변형을 3 번 실행 하 고 평균 실행 값을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-569">We ran each variation 3 times and take the mean value of the runs.</span></span> <span data-ttu-id="718a2-570">테스트 중에는 SQL Server에 대 한 쿼리 캐시를 지우고 다음 명령을 실행 하 여 tempdb를 축소 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-570">Between the tests we clear the query cache on the SQL Server and shrink the tempdb by running the following commands:</span></span>

1.  <span data-ttu-id="718a2-571">DBCC DROPCLEANBUFFERS</span><span class="sxs-lookup"><span data-stu-id="718a2-571">DBCC DROPCLEANBUFFERS</span></span>
2.  <span data-ttu-id="718a2-572">DBCC FREEPROCCACHE</span><span class="sxs-lookup"><span data-stu-id="718a2-572">DBCC FREEPROCCACHE</span></span>
3.  <span data-ttu-id="718a2-573">DBCC SHRINKDATABASE (tempdb, 0)</span><span class="sxs-lookup"><span data-stu-id="718a2-573">DBCC SHRINKDATABASE (tempdb, 0)</span></span>

<span data-ttu-id="718a2-574">테스트 결과 3 개 이상 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-574">Test Results, median over 3 runs:</span></span>

|                        | <span data-ttu-id="718a2-575">추적 안 함 – 작업 집합</span><span class="sxs-lookup"><span data-stu-id="718a2-575">NO TRACKING – WORKING SET</span></span> | <span data-ttu-id="718a2-576">추적 안 함-시간</span><span class="sxs-lookup"><span data-stu-id="718a2-576">NO TRACKING – TIME</span></span> | <span data-ttu-id="718a2-577">추가 전용 – 작업 집합</span><span class="sxs-lookup"><span data-stu-id="718a2-577">APPEND ONLY – WORKING SET</span></span> | <span data-ttu-id="718a2-578">추가 전용 – 시간</span><span class="sxs-lookup"><span data-stu-id="718a2-578">APPEND ONLY – TIME</span></span> |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| <span data-ttu-id="718a2-579">**Entity Framework 5**</span><span class="sxs-lookup"><span data-stu-id="718a2-579">**Entity Framework 5**</span></span> | <span data-ttu-id="718a2-580">460361728</span><span class="sxs-lookup"><span data-stu-id="718a2-580">460361728</span></span>                 | <span data-ttu-id="718a2-581">1163536 밀리초</span><span class="sxs-lookup"><span data-stu-id="718a2-581">1163536 ms</span></span>         | <span data-ttu-id="718a2-582">596545536</span><span class="sxs-lookup"><span data-stu-id="718a2-582">596545536</span></span>                 | <span data-ttu-id="718a2-583">1273042 밀리초</span><span class="sxs-lookup"><span data-stu-id="718a2-583">1273042 ms</span></span>         |
| <span data-ttu-id="718a2-584">**Entity Framework 6**</span><span class="sxs-lookup"><span data-stu-id="718a2-584">**Entity Framework 6**</span></span> | <span data-ttu-id="718a2-585">647127040</span><span class="sxs-lookup"><span data-stu-id="718a2-585">647127040</span></span>                 | <span data-ttu-id="718a2-586">190228 밀리초</span><span class="sxs-lookup"><span data-stu-id="718a2-586">190228 ms</span></span>          | <span data-ttu-id="718a2-587">832798720</span><span class="sxs-lookup"><span data-stu-id="718a2-587">832798720</span></span>                 | <span data-ttu-id="718a2-588">195521 밀리초</span><span class="sxs-lookup"><span data-stu-id="718a2-588">195521 ms</span></span>          |

<span data-ttu-id="718a2-589">Entity Framework 5는 실행이 끝날 때 Entity Framework 6 보다 작은 메모리 사용 공간을 차지 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-589">Entity Framework 5 will have a smaller memory footprint at the end of the run than Entity Framework 6 does.</span></span> <span data-ttu-id="718a2-590">Entity Framework 6에서 사용 하는 추가 메모리는 추가 메모리 구조와 새 기능 및 성능 향상을 위한 코드의 결과입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-590">The additional memory consumed by Entity Framework 6 is the result of additional memory structures and code that enable new features and better performance.</span></span>

<span data-ttu-id="718a2-591">또한 ObjectStateManager를 사용 하는 경우 메모리 사용 공간에 명확한 차이가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-591">There’s also a clear difference in memory footprint when using the ObjectStateManager.</span></span> <span data-ttu-id="718a2-592">데이터베이스에서 구체화 한 모든 엔터티를 추적할 때 5 Entity Framework의 공간이 30% 늘어납니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-592">Entity Framework 5 increased its footprint by 30% when keeping track of all the entities we materialized from the database.</span></span> <span data-ttu-id="718a2-593">이 작업을 수행 하는 경우 6%의 공간 크기가 28% Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="718a2-593">Entity Framework 6 increased its footprint by 28% when doing so.</span></span>

<span data-ttu-id="718a2-594">시간을 기준으로이 테스트에서 우수함 6을 Entity Framework 하 여 Entity Framework 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-594">In terms of time, Entity Framework 6 outperforms Entity Framework 5 in this test by a large margin.</span></span> <span data-ttu-id="718a2-595">Entity Framework 6 Entity Framework 5에서 사용한 시간의 약 16%에 해당 하는 테스트를 완료 했습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-595">Entity Framework 6 completed the test in roughly 16% of the time consumed by Entity Framework 5.</span></span> <span data-ttu-id="718a2-596">또한 Entity Framework 5는 ObjectStateManager를 사용 하는 경우 완료 하는 데 9% 더 많은 시간이 걸립니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-596">Additionally, Entity Framework 5 takes 9% more time to complete when the ObjectStateManager is being used.</span></span> <span data-ttu-id="718a2-597">반면, Entity Framework 6은 ObjectStateManager를 사용할 때 3% 더 많은 시간을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-597">In comparison, Entity Framework 6 is using 3% more time when using the ObjectStateManager.</span></span>

## <a name="6-query-execution-options"></a><span data-ttu-id="718a2-598">6 쿼리 실행 옵션</span><span class="sxs-lookup"><span data-stu-id="718a2-598">6 Query Execution Options</span></span>

<span data-ttu-id="718a2-599">Entity Framework는 다양 한 쿼리 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-599">Entity Framework offers several different ways to query.</span></span> <span data-ttu-id="718a2-600">다음 옵션을 살펴보고 각 옵션의 장단점을 비교 하 고 성능 특성을 검토 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-600">We'll take a look at the following options, compare the pros and cons of each, and examine their performance characteristics:</span></span>

-   <span data-ttu-id="718a2-601">LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="718a2-601">LINQ to Entities.</span></span>
-   <span data-ttu-id="718a2-602">추적 LINQ to Entities 없습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-602">No Tracking LINQ to Entities.</span></span>
-   <span data-ttu-id="718a2-603">ObjectQuery를 Entity SQL 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-603">Entity SQL over an ObjectQuery.</span></span>
-   <span data-ttu-id="718a2-604">EntityCommand를 Entity SQL 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-604">Entity SQL over an EntityCommand.</span></span>
-   <span data-ttu-id="718a2-605">System.data.objects.objectcontext.executestorequery.</span><span class="sxs-lookup"><span data-stu-id="718a2-605">ExecuteStoreQuery.</span></span>
-   <span data-ttu-id="718a2-606">SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="718a2-606">SqlQuery.</span></span>
-   <span data-ttu-id="718a2-607">CompiledQuery.</span><span class="sxs-lookup"><span data-stu-id="718a2-607">CompiledQuery.</span></span>

### <a name="61-linq-to-entities-queries"></a><span data-ttu-id="718a2-608">6.1 LINQ to Entities 쿼리</span><span class="sxs-lookup"><span data-stu-id="718a2-608">6.1       LINQ to Entities queries</span></span>

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

<span data-ttu-id="718a2-609">**들**</span><span class="sxs-lookup"><span data-stu-id="718a2-609">**Pros**</span></span>

-   <span data-ttu-id="718a2-610">CUD 작업에 적합 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-610">Suitable for CUD operations.</span></span>
-   <span data-ttu-id="718a2-611">완전히 구체화 된 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-611">Fully materialized objects.</span></span>
-   <span data-ttu-id="718a2-612">프로그래밍 언어에 기본 제공 되는 구문을 사용 하 여 작성 하는 것이 가장 간단 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-612">Simplest to write with syntax built into the programming language.</span></span>
-   <span data-ttu-id="718a2-613">뛰어난 성능.</span><span class="sxs-lookup"><span data-stu-id="718a2-613">Good performance.</span></span>

<span data-ttu-id="718a2-614">**단점**</span><span class="sxs-lookup"><span data-stu-id="718a2-614">**Cons**</span></span>

-   <span data-ttu-id="718a2-615">특정 기술적 제한 사항 (예:</span><span class="sxs-lookup"><span data-stu-id="718a2-615">Certain technical restrictions, such as:</span></span>
    -   <span data-ttu-id="718a2-616">외부 조인 쿼리에 대해 System.linq.enumerable.defaultifempty를 사용 하는 패턴은 Entity SQL의 단순 외부 조인 문 보다 더 복잡 한 쿼리를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-616">Patterns using DefaultIfEmpty for OUTER JOIN queries result in more complex queries than simple OUTER JOIN statements in Entity SQL.</span></span>
    -   <span data-ttu-id="718a2-617">여전히 일반 패턴 일치와 마찬가지로를 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-617">You still can’t use LIKE with general pattern matching.</span></span>

### <a name="62-no-tracking-linq-to-entities-queries"></a><span data-ttu-id="718a2-618">6.2 추적 LINQ to Entities 쿼리가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-618">6.2       No Tracking LINQ to Entities queries</span></span>

<span data-ttu-id="718a2-619">컨텍스트가 ObjectContext를 파생 하는 경우:</span><span class="sxs-lookup"><span data-stu-id="718a2-619">When the context derives ObjectContext:</span></span>

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

<span data-ttu-id="718a2-620">컨텍스트가 DbContext에서 파생 되는 경우:</span><span class="sxs-lookup"><span data-stu-id="718a2-620">When the context derives DbContext:</span></span>

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

<span data-ttu-id="718a2-621">**들**</span><span class="sxs-lookup"><span data-stu-id="718a2-621">**Pros**</span></span>

-   <span data-ttu-id="718a2-622">정기 LINQ 쿼리를 통해 성능이 향상 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-622">Improved performance over regular LINQ queries.</span></span>
-   <span data-ttu-id="718a2-623">완전히 구체화 된 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-623">Fully materialized objects.</span></span>
-   <span data-ttu-id="718a2-624">프로그래밍 언어에 기본 제공 되는 구문을 사용 하 여 작성 하는 것이 가장 간단 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-624">Simplest to write with syntax built into the programming language.</span></span>

<span data-ttu-id="718a2-625">**단점**</span><span class="sxs-lookup"><span data-stu-id="718a2-625">**Cons**</span></span>

-   <span data-ttu-id="718a2-626">CUD 작업에 적합 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-626">Not suitable for CUD operations.</span></span>
-   <span data-ttu-id="718a2-627">특정 기술적 제한 사항 (예:</span><span class="sxs-lookup"><span data-stu-id="718a2-627">Certain technical restrictions, such as:</span></span>
    -   <span data-ttu-id="718a2-628">외부 조인 쿼리에 대해 System.linq.enumerable.defaultifempty를 사용 하는 패턴은 Entity SQL의 단순 외부 조인 문 보다 더 복잡 한 쿼리를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-628">Patterns using DefaultIfEmpty for OUTER JOIN queries result in more complex queries than simple OUTER JOIN statements in Entity SQL.</span></span>
    -   <span data-ttu-id="718a2-629">여전히 일반 패턴 일치와 마찬가지로를 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-629">You still can’t use LIKE with general pattern matching.</span></span>

<span data-ttu-id="718a2-630">스칼라 속성을 프로젝션 하는 쿼리는 NoTracking가 지정 되지 않은 경우에도 추적 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-630">Note that queries that project scalar properties are not tracked even if the NoTracking is not specified.</span></span> <span data-ttu-id="718a2-631">예를 들어 다음과 같은 가치를 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-631">For example:</span></span>

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

<span data-ttu-id="718a2-632">이 특정 쿼리는 명시적으로 NoTracking를 지정 하지 않지만 개체 상태 관리자에 알려진 형식을 구체화 하지 않으므로 구체화 된 결과가 추적 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-632">This particular query doesn’t explicitly specify being NoTracking, but since it’s not materializing a type that’s known to the object state manager then the materialized result is not tracked.</span></span>

### <a name="63-entity-sql-over-an-objectquery"></a><span data-ttu-id="718a2-633">6.3 Entity SQL ObjectQuery</span><span class="sxs-lookup"><span data-stu-id="718a2-633">6.3       Entity SQL over an ObjectQuery</span></span>

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

<span data-ttu-id="718a2-634">**들**</span><span class="sxs-lookup"><span data-stu-id="718a2-634">**Pros**</span></span>

-   <span data-ttu-id="718a2-635">CUD 작업에 적합 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-635">Suitable for CUD operations.</span></span>
-   <span data-ttu-id="718a2-636">완전히 구체화 된 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-636">Fully materialized objects.</span></span>
-   <span data-ttu-id="718a2-637">는 쿼리 계획 캐싱을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-637">Supports query plan caching.</span></span>

<span data-ttu-id="718a2-638">**단점**</span><span class="sxs-lookup"><span data-stu-id="718a2-638">**Cons**</span></span>

-   <span data-ttu-id="718a2-639">에는 언어에 기본 제공 되는 쿼리 구문 보다 사용자 오류가 발생 하기 쉬운 텍스트 쿼리 문자열이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-639">Involves textual query strings which are more prone to user error than query constructs built into the language.</span></span>

### <a name="64-entity-sql-over-an-entity-command"></a><span data-ttu-id="718a2-640">6.4 엔터티 명령 Entity SQL</span><span class="sxs-lookup"><span data-stu-id="718a2-640">6.4       Entity SQL over an Entity Command</span></span>

``` csharp
EntityCommand cmd = eConn.CreateCommand();
cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
{
    while (reader.Read())
    {
        // manually 'materialize' the product
    }
}
```

<span data-ttu-id="718a2-641">**들**</span><span class="sxs-lookup"><span data-stu-id="718a2-641">**Pros**</span></span>

-   <span data-ttu-id="718a2-642">.NET 4.0에서 쿼리 계획 캐싱을 지원 합니다. 계획 캐싱은 .NET 4.5의 다른 모든 쿼리 형식에서 지원 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-642">Supports query plan caching in .NET 4.0 (plan caching is supported by all other query types in .NET 4.5).</span></span>

<span data-ttu-id="718a2-643">**단점**</span><span class="sxs-lookup"><span data-stu-id="718a2-643">**Cons**</span></span>

-   <span data-ttu-id="718a2-644">에는 언어에 기본 제공 되는 쿼리 구문 보다 사용자 오류가 발생 하기 쉬운 텍스트 쿼리 문자열이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-644">Involves textual query strings which are more prone to user error than query constructs built into the language.</span></span>
-   <span data-ttu-id="718a2-645">CUD 작업에 적합 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-645">Not suitable for CUD operations.</span></span>
-   <span data-ttu-id="718a2-646">결과는 자동으로 구체화 되지 않으므로 데이터 판독기에서 읽어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-646">Results are not automatically materialized, and must be read from the data reader.</span></span>

### <a name="65-sqlquery-and-executestorequery"></a><span data-ttu-id="718a2-647">6.5 SqlQuery 및 System.data.objects.objectcontext.executestorequery</span><span class="sxs-lookup"><span data-stu-id="718a2-647">6.5       SqlQuery and ExecuteStoreQuery</span></span>

<span data-ttu-id="718a2-648">데이터베이스의 SqlQuery:</span><span class="sxs-lookup"><span data-stu-id="718a2-648">SqlQuery on Database:</span></span>

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

<span data-ttu-id="718a2-649">DbSet의 SqlQuery:</span><span class="sxs-lookup"><span data-stu-id="718a2-649">SqlQuery on DbSet:</span></span>

``` csharp
// use this to obtain entities and have them tracked
var q2 = context.Products.SqlQuery("select * from products");
```

<span data-ttu-id="718a2-650">ExecyteStoreQuery:</span><span class="sxs-lookup"><span data-stu-id="718a2-650">ExecyteStoreQuery:</span></span>

``` csharp
var beverages = context.ExecuteStoreQuery<Product>(
@"     SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued, P.DiscontinuedDate
       FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
       WHERE        (C.CategoryName = 'Beverages')"
);
```

<span data-ttu-id="718a2-651">**들**</span><span class="sxs-lookup"><span data-stu-id="718a2-651">**Pros**</span></span>

-   <span data-ttu-id="718a2-652">일반적으로 계획 컴파일러부터 가장 빠른 성능을 무시 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-652">Generally fastest performance since plan compiler is bypassed.</span></span>
-   <span data-ttu-id="718a2-653">완전히 구체화 된 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-653">Fully materialized objects.</span></span>
-   <span data-ttu-id="718a2-654">DbSet에서 사용 될 경우 CUD 작업에 적합 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-654">Suitable for CUD operations when used from the DbSet.</span></span>

<span data-ttu-id="718a2-655">**단점**</span><span class="sxs-lookup"><span data-stu-id="718a2-655">**Cons**</span></span>

-   <span data-ttu-id="718a2-656">쿼리가 텍스트 이며 오류가 발생 하기 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-656">Query is textual and error prone.</span></span>
-   <span data-ttu-id="718a2-657">쿼리는 개념적 의미 체계 대신 저장소 의미 체계를 사용 하 여 특정 백 엔드에 연결 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-657">Query is tied to a specific backend by using store semantics instead of conceptual semantics.</span></span>
-   <span data-ttu-id="718a2-658">상속이 있으면 작업 query는 요청 된 유형에 대 한 매핑 조건을 고려해 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-658">When inheritance is present, handcrafted query needs to account for mapping conditions for the type requested.</span></span>

### <a name="66-compiledquery"></a><span data-ttu-id="718a2-659">6.6 CompiledQuery</span><span class="sxs-lookup"><span data-stu-id="718a2-659">6.6       CompiledQuery</span></span>

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

<span data-ttu-id="718a2-660">**들**</span><span class="sxs-lookup"><span data-stu-id="718a2-660">**Pros**</span></span>

-   <span data-ttu-id="718a2-661">는 일반 LINQ 쿼리에 비해 최대 7% 성능 향상을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-661">Provides up to a 7% performance improvement over regular LINQ queries.</span></span>
-   <span data-ttu-id="718a2-662">완전히 구체화 된 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-662">Fully materialized objects.</span></span>
-   <span data-ttu-id="718a2-663">CUD 작업에 적합 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-663">Suitable for CUD operations.</span></span>

<span data-ttu-id="718a2-664">**단점**</span><span class="sxs-lookup"><span data-stu-id="718a2-664">**Cons**</span></span>

-   <span data-ttu-id="718a2-665">복잡성 및 프로그래밍 오버 헤드가 증가 했습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-665">Increased complexity and programming overhead.</span></span>
-   <span data-ttu-id="718a2-666">컴파일된 쿼리를 기반으로 작성 하는 경우 성능 향상이 손실 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-666">The performance improvement is lost when composing on top of a compiled query.</span></span>
-   <span data-ttu-id="718a2-667">일부 LINQ 쿼리는 CompiledQuery으로 작성할 수 없습니다 (예: 무명 형식의 프로젝션).</span><span class="sxs-lookup"><span data-stu-id="718a2-667">Some LINQ queries can't be written as a CompiledQuery - for example, projections of anonymous types.</span></span>

### <a name="67-performance-comparison-of-different-query-options"></a><span data-ttu-id="718a2-668">6.7 다른 쿼리 옵션의 성능 비교</span><span class="sxs-lookup"><span data-stu-id="718a2-668">6.7       Performance Comparison of different query options</span></span>

<span data-ttu-id="718a2-669">컨텍스트 만들기가 시간 지정 되지 않은 간단한 마이크로 벤치 마크가 테스트에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-669">Simple microbenchmarks where the context creation was not timed were put to the test.</span></span> <span data-ttu-id="718a2-670">제어 되는 환경에서 캐시 되지 않은 엔터티 집합에 대 한 5000 번 쿼리를 측정 했습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-670">We measured querying 5000 times for a set of non-cached entities in a controlled environment.</span></span> <span data-ttu-id="718a2-671">이러한 숫자는 경고와 함께 사용 됩니다. 응용 프로그램에서 생성 된 실제 숫자를 반영 하지는 않지만, 대신 다른 쿼리 옵션을 비교할 때의 성능 차이를 매우 정확 하 게 측정 하는 것입니다. 새 컨텍스트를 만드는 비용을 제외 하 고 사과를 사과 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-671">These numbers are to be taken with a warning: they do not reflect actual numbers produced by an application, but instead they are a very accurate measurement of how much of a performance difference there is when different querying options are compared apples-to-apples, excluding the cost of creating a new context.</span></span>

| <span data-ttu-id="718a2-672">EF</span><span class="sxs-lookup"><span data-stu-id="718a2-672">EF</span></span>  | <span data-ttu-id="718a2-673">테스트</span><span class="sxs-lookup"><span data-stu-id="718a2-673">Test</span></span>                                 | <span data-ttu-id="718a2-674">시간 (ms)</span><span class="sxs-lookup"><span data-stu-id="718a2-674">Time (ms)</span></span> | <span data-ttu-id="718a2-675">메모리</span><span class="sxs-lookup"><span data-stu-id="718a2-675">Memory</span></span>   |
|:----|:-------------------------------------|:----------|:---------|
| <span data-ttu-id="718a2-676">EF5</span><span class="sxs-lookup"><span data-stu-id="718a2-676">EF5</span></span> | <span data-ttu-id="718a2-677">ObjectContext ESQL</span><span class="sxs-lookup"><span data-stu-id="718a2-677">ObjectContext ESQL</span></span>                   | <span data-ttu-id="718a2-678">2414</span><span class="sxs-lookup"><span data-stu-id="718a2-678">2414</span></span>      | <span data-ttu-id="718a2-679">38801408</span><span class="sxs-lookup"><span data-stu-id="718a2-679">38801408</span></span> |
| <span data-ttu-id="718a2-680">EF5</span><span class="sxs-lookup"><span data-stu-id="718a2-680">EF5</span></span> | <span data-ttu-id="718a2-681">ObjectContext Linq 쿼리</span><span class="sxs-lookup"><span data-stu-id="718a2-681">ObjectContext Linq Query</span></span>             | <span data-ttu-id="718a2-682">2692</span><span class="sxs-lookup"><span data-stu-id="718a2-682">2692</span></span>      | <span data-ttu-id="718a2-683">38277120</span><span class="sxs-lookup"><span data-stu-id="718a2-683">38277120</span></span> |
| <span data-ttu-id="718a2-684">EF5</span><span class="sxs-lookup"><span data-stu-id="718a2-684">EF5</span></span> | <span data-ttu-id="718a2-685">DbContext Linq 쿼리 추적 안 함</span><span class="sxs-lookup"><span data-stu-id="718a2-685">DbContext Linq Query No Tracking</span></span>     | <span data-ttu-id="718a2-686">2818</span><span class="sxs-lookup"><span data-stu-id="718a2-686">2818</span></span>      | <span data-ttu-id="718a2-687">41840640</span><span class="sxs-lookup"><span data-stu-id="718a2-687">41840640</span></span> |
| <span data-ttu-id="718a2-688">EF5</span><span class="sxs-lookup"><span data-stu-id="718a2-688">EF5</span></span> | <span data-ttu-id="718a2-689">DbContext Linq Query</span><span class="sxs-lookup"><span data-stu-id="718a2-689">DbContext Linq Query</span></span>                 | <span data-ttu-id="718a2-690">2930</span><span class="sxs-lookup"><span data-stu-id="718a2-690">2930</span></span>      | <span data-ttu-id="718a2-691">41771008</span><span class="sxs-lookup"><span data-stu-id="718a2-691">41771008</span></span> |
| <span data-ttu-id="718a2-692">EF5</span><span class="sxs-lookup"><span data-stu-id="718a2-692">EF5</span></span> | <span data-ttu-id="718a2-693">ObjectContext Linq 쿼리 추적 안 함</span><span class="sxs-lookup"><span data-stu-id="718a2-693">ObjectContext Linq Query No Tracking</span></span> | <span data-ttu-id="718a2-694">3013</span><span class="sxs-lookup"><span data-stu-id="718a2-694">3013</span></span>      | <span data-ttu-id="718a2-695">38412288</span><span class="sxs-lookup"><span data-stu-id="718a2-695">38412288</span></span> |
|     |                                      |           |          |
| <span data-ttu-id="718a2-696">EF6</span><span class="sxs-lookup"><span data-stu-id="718a2-696">EF6</span></span> | <span data-ttu-id="718a2-697">ObjectContext ESQL</span><span class="sxs-lookup"><span data-stu-id="718a2-697">ObjectContext ESQL</span></span>                   | <span data-ttu-id="718a2-698">2059</span><span class="sxs-lookup"><span data-stu-id="718a2-698">2059</span></span>      | <span data-ttu-id="718a2-699">46039040</span><span class="sxs-lookup"><span data-stu-id="718a2-699">46039040</span></span> |
| <span data-ttu-id="718a2-700">EF6</span><span class="sxs-lookup"><span data-stu-id="718a2-700">EF6</span></span> | <span data-ttu-id="718a2-701">ObjectContext Linq 쿼리</span><span class="sxs-lookup"><span data-stu-id="718a2-701">ObjectContext Linq Query</span></span>             | <span data-ttu-id="718a2-702">3074</span><span class="sxs-lookup"><span data-stu-id="718a2-702">3074</span></span>      | <span data-ttu-id="718a2-703">45248512</span><span class="sxs-lookup"><span data-stu-id="718a2-703">45248512</span></span> |
| <span data-ttu-id="718a2-704">EF6</span><span class="sxs-lookup"><span data-stu-id="718a2-704">EF6</span></span> | <span data-ttu-id="718a2-705">DbContext Linq 쿼리 추적 안 함</span><span class="sxs-lookup"><span data-stu-id="718a2-705">DbContext Linq Query No Tracking</span></span>     | <span data-ttu-id="718a2-706">3125</span><span class="sxs-lookup"><span data-stu-id="718a2-706">3125</span></span>      | <span data-ttu-id="718a2-707">47575040</span><span class="sxs-lookup"><span data-stu-id="718a2-707">47575040</span></span> |
| <span data-ttu-id="718a2-708">EF6</span><span class="sxs-lookup"><span data-stu-id="718a2-708">EF6</span></span> | <span data-ttu-id="718a2-709">DbContext Linq Query</span><span class="sxs-lookup"><span data-stu-id="718a2-709">DbContext Linq Query</span></span>                 | <span data-ttu-id="718a2-710">3420</span><span class="sxs-lookup"><span data-stu-id="718a2-710">3420</span></span>      | <span data-ttu-id="718a2-711">47652864</span><span class="sxs-lookup"><span data-stu-id="718a2-711">47652864</span></span> |
| <span data-ttu-id="718a2-712">EF6</span><span class="sxs-lookup"><span data-stu-id="718a2-712">EF6</span></span> | <span data-ttu-id="718a2-713">ObjectContext Linq 쿼리 추적 안 함</span><span class="sxs-lookup"><span data-stu-id="718a2-713">ObjectContext Linq Query No Tracking</span></span> | <span data-ttu-id="718a2-714">3593</span><span class="sxs-lookup"><span data-stu-id="718a2-714">3593</span></span>      | <span data-ttu-id="718a2-715">45260800</span><span class="sxs-lookup"><span data-stu-id="718a2-715">45260800</span></span> |

![EF5 마이크로 벤치 마크, 5000 웜 반복](~/ef6/media/ef5micro5000warm.png)

![EF6 마이크로 벤치 마크, 5000 웜 반복](~/ef6/media/ef6micro5000warm.png)

<span data-ttu-id="718a2-718">마이크로 벤치 마크는 코드의 작은 변경 내용에 매우 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-718">Microbenchmarks are very sensitive to small changes in the code.</span></span> <span data-ttu-id="718a2-719">이 경우 Entity Framework 5와 Entity Framework 6의 비용 간 차이는 [가로채기](~/ef6/fundamentals/logging-and-interception.md) 및 [트랜잭션 개선 사항이](~/ef6/saving/transactions.md)추가 되었기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-719">In this case, the difference between the costs of Entity Framework 5 and Entity Framework 6 are due to the addition of [interception](~/ef6/fundamentals/logging-and-interception.md) and [transactional improvements](~/ef6/saving/transactions.md).</span></span> <span data-ttu-id="718a2-720">그러나 이러한 마이크로 벤치 마크 수치는 Entity Framework의 매우 작은 부분으로 증폭 된 비전입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-720">These microbenchmarks numbers, however, are an amplified vision into a very small fragment of what Entity Framework does.</span></span> <span data-ttu-id="718a2-721">웜 쿼리의 실제 시나리오에서는 Entity Framework 5에서 Entity Framework 6으로 업그레이드할 때 성능 재발이 표시 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-721">Real-world scenarios of warm queries should not see a performance regression when upgrading from Entity Framework 5 to Entity Framework 6.</span></span>

<span data-ttu-id="718a2-722">다른 쿼리 옵션의 실제 성능을 비교 하기 위해 다른 쿼리 옵션을 사용 하 여 범주 이름이 "음료" 인 모든 제품을 선택 하는 별도의 테스트 변형을 5 개 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-722">To compare the real-world performance of the different query options, we created 5 separate test variations where we use a different query option to select all products whose category name is "Beverages".</span></span> <span data-ttu-id="718a2-723">각 반복에는 컨텍스트를 만드는 비용과 반환 된 모든 엔터티를 구체화 하는 비용이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-723">Each iteration includes the cost of creating the context, and the cost of materializing all returned entities.</span></span> <span data-ttu-id="718a2-724">1000 시간 반복의 합계를 계산 하기 전에 10 회 반복 실행이 시간 초과 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-724">10 iterations are run untimed before taking the sum of 1000 timed iterations.</span></span> <span data-ttu-id="718a2-725">표시 되는 결과는 각 테스트의 5 번 실행에서 가져온 중앙값입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-725">The results shown are the median run taken from 5 runs of each test.</span></span> <span data-ttu-id="718a2-726">자세한 내용은 테스트에 대 한 코드를 포함 하는 부록 B를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="718a2-726">For more information, see Appendix B which includes the code for the test.</span></span>

| <span data-ttu-id="718a2-727">EF</span><span class="sxs-lookup"><span data-stu-id="718a2-727">EF</span></span>  | <span data-ttu-id="718a2-728">테스트</span><span class="sxs-lookup"><span data-stu-id="718a2-728">Test</span></span>                                        | <span data-ttu-id="718a2-729">시간 (ms)</span><span class="sxs-lookup"><span data-stu-id="718a2-729">Time (ms)</span></span> | <span data-ttu-id="718a2-730">메모리</span><span class="sxs-lookup"><span data-stu-id="718a2-730">Memory</span></span>   |
|:----|:--------------------------------------------|:----------|:---------|
| <span data-ttu-id="718a2-731">EF5</span><span class="sxs-lookup"><span data-stu-id="718a2-731">EF5</span></span> | <span data-ttu-id="718a2-732">ObjectContext 엔터티 명령</span><span class="sxs-lookup"><span data-stu-id="718a2-732">ObjectContext Entity Command</span></span>                | <span data-ttu-id="718a2-733">621</span><span class="sxs-lookup"><span data-stu-id="718a2-733">621</span></span>       | <span data-ttu-id="718a2-734">39350272</span><span class="sxs-lookup"><span data-stu-id="718a2-734">39350272</span></span> |
| <span data-ttu-id="718a2-735">EF5</span><span class="sxs-lookup"><span data-stu-id="718a2-735">EF5</span></span> | <span data-ttu-id="718a2-736">데이터베이스의 DbContext Sql 쿼리</span><span class="sxs-lookup"><span data-stu-id="718a2-736">DbContext Sql Query on Database</span></span>             | <span data-ttu-id="718a2-737">825</span><span class="sxs-lookup"><span data-stu-id="718a2-737">825</span></span>       | <span data-ttu-id="718a2-738">37519360</span><span class="sxs-lookup"><span data-stu-id="718a2-738">37519360</span></span> |
| <span data-ttu-id="718a2-739">EF5</span><span class="sxs-lookup"><span data-stu-id="718a2-739">EF5</span></span> | <span data-ttu-id="718a2-740">ObjectContext 저장소 쿼리</span><span class="sxs-lookup"><span data-stu-id="718a2-740">ObjectContext Store Query</span></span>                   | <span data-ttu-id="718a2-741">878</span><span class="sxs-lookup"><span data-stu-id="718a2-741">878</span></span>       | <span data-ttu-id="718a2-742">39460864</span><span class="sxs-lookup"><span data-stu-id="718a2-742">39460864</span></span> |
| <span data-ttu-id="718a2-743">EF5</span><span class="sxs-lookup"><span data-stu-id="718a2-743">EF5</span></span> | <span data-ttu-id="718a2-744">ObjectContext Linq 쿼리 추적 안 함</span><span class="sxs-lookup"><span data-stu-id="718a2-744">ObjectContext Linq Query No Tracking</span></span>        | <span data-ttu-id="718a2-745">969</span><span class="sxs-lookup"><span data-stu-id="718a2-745">969</span></span>       | <span data-ttu-id="718a2-746">38293504</span><span class="sxs-lookup"><span data-stu-id="718a2-746">38293504</span></span> |
| <span data-ttu-id="718a2-747">EF5</span><span class="sxs-lookup"><span data-stu-id="718a2-747">EF5</span></span> | <span data-ttu-id="718a2-748">개체 쿼리를 사용 하는 ObjectContext Entity Sql</span><span class="sxs-lookup"><span data-stu-id="718a2-748">ObjectContext Entity Sql using Object Query</span></span> | <span data-ttu-id="718a2-749">1089</span><span class="sxs-lookup"><span data-stu-id="718a2-749">1089</span></span>      | <span data-ttu-id="718a2-750">38981632</span><span class="sxs-lookup"><span data-stu-id="718a2-750">38981632</span></span> |
| <span data-ttu-id="718a2-751">EF5</span><span class="sxs-lookup"><span data-stu-id="718a2-751">EF5</span></span> | <span data-ttu-id="718a2-752">ObjectContext 컴파일된 쿼리</span><span class="sxs-lookup"><span data-stu-id="718a2-752">ObjectContext Compiled Query</span></span>                | <span data-ttu-id="718a2-753">1099</span><span class="sxs-lookup"><span data-stu-id="718a2-753">1099</span></span>      | <span data-ttu-id="718a2-754">38682624</span><span class="sxs-lookup"><span data-stu-id="718a2-754">38682624</span></span> |
| <span data-ttu-id="718a2-755">EF5</span><span class="sxs-lookup"><span data-stu-id="718a2-755">EF5</span></span> | <span data-ttu-id="718a2-756">ObjectContext Linq 쿼리</span><span class="sxs-lookup"><span data-stu-id="718a2-756">ObjectContext Linq Query</span></span>                    | <span data-ttu-id="718a2-757">1152</span><span class="sxs-lookup"><span data-stu-id="718a2-757">1152</span></span>      | <span data-ttu-id="718a2-758">38178816</span><span class="sxs-lookup"><span data-stu-id="718a2-758">38178816</span></span> |
| <span data-ttu-id="718a2-759">EF5</span><span class="sxs-lookup"><span data-stu-id="718a2-759">EF5</span></span> | <span data-ttu-id="718a2-760">DbContext Linq 쿼리 추적 안 함</span><span class="sxs-lookup"><span data-stu-id="718a2-760">DbContext Linq Query No Tracking</span></span>            | <span data-ttu-id="718a2-761">1208</span><span class="sxs-lookup"><span data-stu-id="718a2-761">1208</span></span>      | <span data-ttu-id="718a2-762">41803776</span><span class="sxs-lookup"><span data-stu-id="718a2-762">41803776</span></span> |
| <span data-ttu-id="718a2-763">EF5</span><span class="sxs-lookup"><span data-stu-id="718a2-763">EF5</span></span> | <span data-ttu-id="718a2-764">DbSet의 DbContext Sql 쿼리</span><span class="sxs-lookup"><span data-stu-id="718a2-764">DbContext Sql Query on DbSet</span></span>                | <span data-ttu-id="718a2-765">1414</span><span class="sxs-lookup"><span data-stu-id="718a2-765">1414</span></span>      | <span data-ttu-id="718a2-766">37982208</span><span class="sxs-lookup"><span data-stu-id="718a2-766">37982208</span></span> |
| <span data-ttu-id="718a2-767">EF5</span><span class="sxs-lookup"><span data-stu-id="718a2-767">EF5</span></span> | <span data-ttu-id="718a2-768">DbContext Linq Query</span><span class="sxs-lookup"><span data-stu-id="718a2-768">DbContext Linq Query</span></span>                        | <span data-ttu-id="718a2-769">1574</span><span class="sxs-lookup"><span data-stu-id="718a2-769">1574</span></span>      | <span data-ttu-id="718a2-770">41738240</span><span class="sxs-lookup"><span data-stu-id="718a2-770">41738240</span></span> |
|     |                                             |           |          |
| <span data-ttu-id="718a2-771">EF6</span><span class="sxs-lookup"><span data-stu-id="718a2-771">EF6</span></span> | <span data-ttu-id="718a2-772">ObjectContext 엔터티 명령</span><span class="sxs-lookup"><span data-stu-id="718a2-772">ObjectContext Entity Command</span></span>                | <span data-ttu-id="718a2-773">480</span><span class="sxs-lookup"><span data-stu-id="718a2-773">480</span></span>       | <span data-ttu-id="718a2-774">47247360</span><span class="sxs-lookup"><span data-stu-id="718a2-774">47247360</span></span> |
| <span data-ttu-id="718a2-775">EF6</span><span class="sxs-lookup"><span data-stu-id="718a2-775">EF6</span></span> | <span data-ttu-id="718a2-776">ObjectContext 저장소 쿼리</span><span class="sxs-lookup"><span data-stu-id="718a2-776">ObjectContext Store Query</span></span>                   | <span data-ttu-id="718a2-777">493</span><span class="sxs-lookup"><span data-stu-id="718a2-777">493</span></span>       | <span data-ttu-id="718a2-778">46739456</span><span class="sxs-lookup"><span data-stu-id="718a2-778">46739456</span></span> |
| <span data-ttu-id="718a2-779">EF6</span><span class="sxs-lookup"><span data-stu-id="718a2-779">EF6</span></span> | <span data-ttu-id="718a2-780">데이터베이스의 DbContext Sql 쿼리</span><span class="sxs-lookup"><span data-stu-id="718a2-780">DbContext Sql Query on Database</span></span>             | <span data-ttu-id="718a2-781">614</span><span class="sxs-lookup"><span data-stu-id="718a2-781">614</span></span>       | <span data-ttu-id="718a2-782">41607168</span><span class="sxs-lookup"><span data-stu-id="718a2-782">41607168</span></span> |
| <span data-ttu-id="718a2-783">EF6</span><span class="sxs-lookup"><span data-stu-id="718a2-783">EF6</span></span> | <span data-ttu-id="718a2-784">ObjectContext Linq 쿼리 추적 안 함</span><span class="sxs-lookup"><span data-stu-id="718a2-784">ObjectContext Linq Query No Tracking</span></span>        | <span data-ttu-id="718a2-785">684</span><span class="sxs-lookup"><span data-stu-id="718a2-785">684</span></span>       | <span data-ttu-id="718a2-786">46333952</span><span class="sxs-lookup"><span data-stu-id="718a2-786">46333952</span></span> |
| <span data-ttu-id="718a2-787">EF6</span><span class="sxs-lookup"><span data-stu-id="718a2-787">EF6</span></span> | <span data-ttu-id="718a2-788">개체 쿼리를 사용 하는 ObjectContext Entity Sql</span><span class="sxs-lookup"><span data-stu-id="718a2-788">ObjectContext Entity Sql using Object Query</span></span> | <span data-ttu-id="718a2-789">767</span><span class="sxs-lookup"><span data-stu-id="718a2-789">767</span></span>       | <span data-ttu-id="718a2-790">48865280</span><span class="sxs-lookup"><span data-stu-id="718a2-790">48865280</span></span> |
| <span data-ttu-id="718a2-791">EF6</span><span class="sxs-lookup"><span data-stu-id="718a2-791">EF6</span></span> | <span data-ttu-id="718a2-792">ObjectContext 컴파일된 쿼리</span><span class="sxs-lookup"><span data-stu-id="718a2-792">ObjectContext Compiled Query</span></span>                | <span data-ttu-id="718a2-793">788</span><span class="sxs-lookup"><span data-stu-id="718a2-793">788</span></span>       | <span data-ttu-id="718a2-794">48467968</span><span class="sxs-lookup"><span data-stu-id="718a2-794">48467968</span></span> |
| <span data-ttu-id="718a2-795">EF6</span><span class="sxs-lookup"><span data-stu-id="718a2-795">EF6</span></span> | <span data-ttu-id="718a2-796">DbContext Linq 쿼리 추적 안 함</span><span class="sxs-lookup"><span data-stu-id="718a2-796">DbContext Linq Query No Tracking</span></span>            | <span data-ttu-id="718a2-797">878</span><span class="sxs-lookup"><span data-stu-id="718a2-797">878</span></span>       | <span data-ttu-id="718a2-798">47554560</span><span class="sxs-lookup"><span data-stu-id="718a2-798">47554560</span></span> |
| <span data-ttu-id="718a2-799">EF6</span><span class="sxs-lookup"><span data-stu-id="718a2-799">EF6</span></span> | <span data-ttu-id="718a2-800">ObjectContext Linq 쿼리</span><span class="sxs-lookup"><span data-stu-id="718a2-800">ObjectContext Linq Query</span></span>                    | <span data-ttu-id="718a2-801">953</span><span class="sxs-lookup"><span data-stu-id="718a2-801">953</span></span>       | <span data-ttu-id="718a2-802">47632384</span><span class="sxs-lookup"><span data-stu-id="718a2-802">47632384</span></span> |
| <span data-ttu-id="718a2-803">EF6</span><span class="sxs-lookup"><span data-stu-id="718a2-803">EF6</span></span> | <span data-ttu-id="718a2-804">DbSet의 DbContext Sql 쿼리</span><span class="sxs-lookup"><span data-stu-id="718a2-804">DbContext Sql Query on DbSet</span></span>                | <span data-ttu-id="718a2-805">1023</span><span class="sxs-lookup"><span data-stu-id="718a2-805">1023</span></span>      | <span data-ttu-id="718a2-806">41992192</span><span class="sxs-lookup"><span data-stu-id="718a2-806">41992192</span></span> |
| <span data-ttu-id="718a2-807">EF6</span><span class="sxs-lookup"><span data-stu-id="718a2-807">EF6</span></span> | <span data-ttu-id="718a2-808">DbContext Linq Query</span><span class="sxs-lookup"><span data-stu-id="718a2-808">DbContext Linq Query</span></span>                        | <span data-ttu-id="718a2-809">1290</span><span class="sxs-lookup"><span data-stu-id="718a2-809">1290</span></span>      | <span data-ttu-id="718a2-810">47529984</span><span class="sxs-lookup"><span data-stu-id="718a2-810">47529984</span></span> |


![EF5 웜 쿼리 1000 반복](~/ef6/media/ef5warmquery1000.png)

![EF6 웜 쿼리 1000 반복](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> <span data-ttu-id="718a2-813">완전성을 위해 EntityCommand에 대 한 Entity SQL 쿼리를 실행 하는 변형이 포함 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-813">For completeness, we included a variation where we execute an Entity SQL query on an EntityCommand.</span></span> <span data-ttu-id="718a2-814">그러나 이러한 쿼리에 대 한 결과가 구체화 되지 않기 때문에 이러한 비교는 반드시 사과를 사과로 변환할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-814">However, because results are not materialized for such queries, the comparison isn't necessarily apples-to-apples.</span></span> <span data-ttu-id="718a2-815">테스트에는 비교 fairer를 시도 하기 위해 구체화 하는 근접 한 근사값이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-815">The test includes a close approximation to materializing to try making the comparison fairer.</span></span>

<span data-ttu-id="718a2-816">이 종단 간 사례에서는 DbContext 초기화 속도가 훨씬 더 작고 MetadataCollection @ no__t-0T @ no__t 조회를 포함 하 여 스택의 여러 부분에 대 한 성능 향상으로 인해 6 우수함 Entity Framework 5를 Entity Framework 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-816">In this end-to-end case, Entity Framework 6 outperforms Entity Framework 5 due to performance improvements made on several parts of the stack, including a much lighter DbContext initialization and faster MetadataCollection&lt;T&gt; lookups.</span></span>

## <a name="7-design-time-performance-considerations"></a><span data-ttu-id="718a2-817">7 디자인 타임 성능 고려 사항</span><span class="sxs-lookup"><span data-stu-id="718a2-817">7 Design time performance considerations</span></span>

### <a name="71-inheritance-strategies"></a><span data-ttu-id="718a2-818">7.1 상속 전략</span><span class="sxs-lookup"><span data-stu-id="718a2-818">7.1       Inheritance Strategies</span></span>

<span data-ttu-id="718a2-819">Entity Framework를 사용 하는 경우 다른 성능 고려 사항은 사용 하는 상속 전략입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-819">Another performance consideration when using Entity Framework is the inheritance strategy you use.</span></span> <span data-ttu-id="718a2-820">Entity Framework는 세 가지 기본 유형의 상속과 조합을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-820">Entity Framework supports 3 basic types of inheritance and their combinations:</span></span>

-   <span data-ttu-id="718a2-821">TPH (계층당 하나의 테이블) – 각 상속 집합이 행에 표시 되는 계층의 특정 형식을 나타내는 판별자 열이 있는 테이블에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-821">Table per Hierarchy (TPH) – where each inheritance set maps to a table with a discriminator column to indicate which particular type in the hierarchy is being represented in the row.</span></span>
-   <span data-ttu-id="718a2-822">형식당 하나의 테이블 (TPT) – 각 형식에는 데이터베이스에 고유한 테이블이 있습니다. 자식 테이블은 부모 테이블에 포함 되지 않은 열만 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-822">Table per Type (TPT) – where each type has its own table in the database; the child tables only define the columns that the parent table doesn’t contain.</span></span>
-   <span data-ttu-id="718a2-823">TPC (클래스 단위)-각 형식에는 데이터베이스의 자체 전체 테이블이 있습니다. 자식 테이블은 부모 형식에 정의 된 필드를 포함 하 여 모든 필드를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-823">Table per Class (TPC) – where each type has its own full table in the database; the child tables define all their fields, including those defined in parent types.</span></span>

<span data-ttu-id="718a2-824">모델에서 TPT 상속을 사용 하는 경우 생성 되는 쿼리는 다른 상속 전략을 사용 하 여 생성 되는 쿼리보다 복잡 하므로 저장소에서 실행 시간이 길어질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-824">If your model uses TPT inheritance, the queries which are generated will be more complex than those that are generated with the other inheritance strategies, which may result on longer execution times on the store.</span></span><span data-ttu-id="718a2-825">  일반적으로 TPT 모델에 대 한 쿼리를 생성 하 고 결과 개체를 구체화 하는 데 더 많은 시간이 걸립니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-825">  It will generally take longer to generate queries over a TPT model, and to materialize the resulting objects.</span></span>

<span data-ttu-id="718a2-826">"성능 고려 사항을 참조 Entity Framework에서 TPT (형식당 하나의 테이블) 상속을 사용 하는 경우" MSDN 블로그 게시: \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx> 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-826">See the "Performance Considerations when using TPT (Table per Type) Inheritance in the Entity Framework" MSDN blog post: \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.</span></span>

#### <a name="711-avoiding-tpt-in-model-first-or-code-first-applications"></a><span data-ttu-id="718a2-827">Model First 또는 Code First 응용 프로그램의 TPT 방지 7.1.1</span><span class="sxs-lookup"><span data-stu-id="718a2-827">7.1.1       Avoiding TPT in Model First or Code First applications</span></span>

<span data-ttu-id="718a2-828">TPT 스키마가 있는 기존 데이터베이스를 통해 모델을 만드는 경우에는 여러 가지 옵션을 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-828">When you create a model over an existing database that has a TPT schema, you don't have many options.</span></span> <span data-ttu-id="718a2-829">그러나 Model First 또는 Code First를 사용 하 여 응용 프로그램을 만들 때 성능 문제에 대 한 TPT 상속을 피해 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-829">But when creating an application using Model First or Code First, you should avoid TPT inheritance for performance concerns.</span></span>

<span data-ttu-id="718a2-830">Entity Designer 마법사에서 Model First 사용 하는 경우 모델의 모든 상속에 대 한 TPT을 얻게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-830">When you use Model First in the Entity Designer Wizard, you will get TPT for any inheritance in your model.</span></span> <span data-ttu-id="718a2-831">Model First를 사용 하 여 TPH 상속 전략으로 전환 하려는 경우 Visual Studio 갤러리에서 "엔터티 디자이너 데이터베이스 생성 전원 팩" 사용 가능한를 사용할 수 있습니다 ( \<http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>) 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-831">If you want to switch to a TPH inheritance strategy with Model First, you can use the "Entity Designer Database Generation Power Pack" available from the Visual Studio Gallery ( \<http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).</span></span>

<span data-ttu-id="718a2-832">Code First를 사용 하 여 상속 된 모델의 매핑을 구성 하는 경우 EF는 기본적으로 TPH를 사용 하므로 상속 계층 구조의 모든 엔터티는 동일한 테이블에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-832">When using Code First to configure the mapping of a model with inheritance, EF will use TPH by default, therefore all entities in the inheritance hierarchy will be mapped to the same table.</span></span> <span data-ttu-id="718a2-833">MSDN Magazine의 "코드 엔터티 Framework4.1에서 첫 번째." 문서 "매핑 Fluent API를 사용한" 섹션을 참조 하세요 ( [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)) 대 한 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-833">See the "Mapping with the Fluent API" section of the "Code First in Entity Framework4.1" article in MSDN Magazine ( [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)) for more details.</span></span>

### <a name="72-upgrading-from-ef4-to-improve-model-generation-time"></a><span data-ttu-id="718a2-834">7.2 모델 생성 시간을 개선 하기 위해 EF4에서 업그레이드</span><span class="sxs-lookup"><span data-stu-id="718a2-834">7.2       Upgrading from EF4 to improve model generation time</span></span>

<span data-ttu-id="718a2-835">모델의 SSDL (저장소 계층)을 생성 하는 알고리즘에 대 한 SQL Server의 향상 된 기능은 Entity Framework 5와 6에서 사용할 수 있으며, Visual Studio 2010 s p 1이 설치 될 때 Entity Framework 4로 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-835">A SQL Server-specific improvement to the algorithm that generates the store-layer (SSDL) of the model is available in Entity Framework 5 and 6, and as an update to Entity Framework 4 when Visual Studio 2010 SP1 is installed.</span></span> <span data-ttu-id="718a2-836">다음 테스트 결과는 매우 큰 모델 (이 경우 Navision 모델)을 생성할 때 향상 된 기능을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-836">The following test results demonstrate the improvement when generating a very big model, in this case the Navision model.</span></span> <span data-ttu-id="718a2-837">자세한 내용은 부록 C를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="718a2-837">See Appendix C for more details about it.</span></span>

<span data-ttu-id="718a2-838">모델에는 1005 엔터티 집합과 4227 연결 집합이 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-838">The model contains 1005 entity sets and 4227 association sets.</span></span>

| <span data-ttu-id="718a2-839">Configuration</span><span class="sxs-lookup"><span data-stu-id="718a2-839">Configuration</span></span>                              | <span data-ttu-id="718a2-840">사용 된 시간 분석</span><span class="sxs-lookup"><span data-stu-id="718a2-840">Breakdown of time consumed</span></span>                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="718a2-841">Visual Studio 2010, Entity Framework 4</span><span class="sxs-lookup"><span data-stu-id="718a2-841">Visual Studio 2010, Entity Framework 4</span></span>     | <span data-ttu-id="718a2-842">SSDL 생성: 2 hr 27 분</span><span class="sxs-lookup"><span data-stu-id="718a2-842">SSDL Generation: 2 hr 27 min</span></span> <br/> <span data-ttu-id="718a2-843">매핑 생성: 1 초</span><span class="sxs-lookup"><span data-stu-id="718a2-843">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="718a2-844">CSDL 생성: 1 초</span><span class="sxs-lookup"><span data-stu-id="718a2-844">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="718a2-845">ObjectLayer 생성: 1 초</span><span class="sxs-lookup"><span data-stu-id="718a2-845">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="718a2-846">생성 보기: 2 h 14 분</span><span class="sxs-lookup"><span data-stu-id="718a2-846">View Generation: 2 h 14 min</span></span> |
| <span data-ttu-id="718a2-847">Visual Studio 2010 SP1, Entity Framework 4</span><span class="sxs-lookup"><span data-stu-id="718a2-847">Visual Studio 2010 SP1, Entity Framework 4</span></span> | <span data-ttu-id="718a2-848">SSDL 생성: 1 초</span><span class="sxs-lookup"><span data-stu-id="718a2-848">SSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="718a2-849">매핑 생성: 1 초</span><span class="sxs-lookup"><span data-stu-id="718a2-849">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="718a2-850">CSDL 생성: 1 초</span><span class="sxs-lookup"><span data-stu-id="718a2-850">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="718a2-851">ObjectLayer 생성: 1 초</span><span class="sxs-lookup"><span data-stu-id="718a2-851">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="718a2-852">생성 보기: 1 hr 53 분</span><span class="sxs-lookup"><span data-stu-id="718a2-852">View Generation: 1 hr 53 min</span></span>   |
| <span data-ttu-id="718a2-853">Visual Studio 2013, Entity Framework 5</span><span class="sxs-lookup"><span data-stu-id="718a2-853">Visual Studio 2013, Entity Framework 5</span></span>     | <span data-ttu-id="718a2-854">SSDL 생성: 1 초</span><span class="sxs-lookup"><span data-stu-id="718a2-854">SSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="718a2-855">매핑 생성: 1 초</span><span class="sxs-lookup"><span data-stu-id="718a2-855">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="718a2-856">CSDL 생성: 1 초</span><span class="sxs-lookup"><span data-stu-id="718a2-856">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="718a2-857">ObjectLayer 생성: 1 초</span><span class="sxs-lookup"><span data-stu-id="718a2-857">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="718a2-858">생성 보기: 65 분</span><span class="sxs-lookup"><span data-stu-id="718a2-858">View Generation: 65 minutes</span></span>    |
| <span data-ttu-id="718a2-859">Visual Studio 2013, Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="718a2-859">Visual Studio 2013, Entity Framework 6</span></span>     | <span data-ttu-id="718a2-860">SSDL 생성: 1 초</span><span class="sxs-lookup"><span data-stu-id="718a2-860">SSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="718a2-861">매핑 생성: 1 초</span><span class="sxs-lookup"><span data-stu-id="718a2-861">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="718a2-862">CSDL 생성: 1 초</span><span class="sxs-lookup"><span data-stu-id="718a2-862">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="718a2-863">ObjectLayer 생성: 1 초</span><span class="sxs-lookup"><span data-stu-id="718a2-863">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="718a2-864">생성 보기: 28 초.</span><span class="sxs-lookup"><span data-stu-id="718a2-864">View Generation: 28 seconds.</span></span>   |


<span data-ttu-id="718a2-865">SSDL을 생성할 때 클라이언트 개발 컴퓨터가 서버에서 결과가 반환 될 때까지 유휴 상태로 대기 하는 동안에는 SQL Server에서 로드가 거의 완전히 소비 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-865">It's worth noting that when generating the SSDL, the load is almost entirely spent on the SQL Server, while the client development machine is waiting idle for results to come back from the server.</span></span> <span data-ttu-id="718a2-866">Dba는 특히 이러한 개선 사항을 이해 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-866">DBAs should particularly appreciate this improvement.</span></span> <span data-ttu-id="718a2-867">또한 기본적으로 모델 생성의 전체 비용이 생성 된 보기에서 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-867">It's also worth noting that essentially the entire cost of model generation takes place in View Generation now.</span></span>

### <a name="73-splitting-large-models-with-database-first-and-model-first"></a><span data-ttu-id="718a2-868">7.3 Database First 및 Model First으로 많은 모델 분할</span><span class="sxs-lookup"><span data-stu-id="718a2-868">7.3       Splitting Large Models with Database First and Model First</span></span>

<span data-ttu-id="718a2-869">모델 크기가 늘어나면 디자이너 화면이 복잡해 집니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-869">As model size increases, the designer surface becomes cluttered and difficult to use.</span></span> <span data-ttu-id="718a2-870">일반적으로 디자이너를 효과적으로 사용 하기 위해 너무 많은 엔터티가 300 개 이상인 모델을 고려 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-870">We typically consider a model with more than 300 entities to be too large to effectively use the designer.</span></span> <span data-ttu-id="718a2-871">큰 모델을 분할 하기 위한 여러 옵션을 설명 하는 다음 블로그 게시물: \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx> 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-871">The following blog post describes several options for splitting large models: \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.</span></span>

<span data-ttu-id="718a2-872">첫 번째 버전의 Entity Framework에 대 한 게시물이 작성 되었지만 단계가 여전히 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-872">The post was written for the first version of Entity Framework, but the steps still apply.</span></span>

### <a name="74-performance-considerations-with-the-entity-data-source-control"></a><span data-ttu-id="718a2-873">7.4 엔터티 데이터 소스 컨트롤을 사용 하 여 성능 고려 사항</span><span class="sxs-lookup"><span data-stu-id="718a2-873">7.4       Performance considerations with the Entity Data Source Control</span></span>

<span data-ttu-id="718a2-874">EntityDataSource 컨트롤을 사용 하는 웹 응용 프로그램의 성능이 상당히 deteriorates는 다중 스레드 성능 및 스트레스 테스트 사례를 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-874">We've seen cases in multi-threaded performance and stress tests where the performance of a web application using the EntityDataSource Control deteriorates significantly.</span></span> <span data-ttu-id="718a2-875">근본 원인은 EntityDataSource가 웹 응용 프로그램에서 참조 하는 어셈블리에 대해 LoadFromAssembly를 반복적으로 호출 하 여 엔터티로 사용할 형식을 검색 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-875">The underlying cause is that the EntityDataSource repeatedly calls MetadataWorkspace.LoadFromAssembly on the assemblies referenced by the Web application to discover the types to be used as entities.</span></span>

<span data-ttu-id="718a2-876">해결 방법은 EntityDataSource의 ContextTypeName를 파생 된 ObjectContext 클래스의 형식 이름으로 설정 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-876">The solution is to set the ContextTypeName of the EntityDataSource to the type name of your derived ObjectContext class.</span></span> <span data-ttu-id="718a2-877">그러면 엔터티 형식에 대해 참조 된 모든 어셈블리를 검색 하는 메커니즘이 꺼집니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-877">This turns off the mechanism that scans all referenced assemblies for entity types.</span></span>

<span data-ttu-id="718a2-878">또한 ContextTypeName 필드를 설정 하면 리플렉션을 통해 어셈블리에서 형식을 로드할 수 없을 때 .NET 4.0의 EntityDataSource가 ReflectionTypeLoadException를 throw 하는 기능 문제가 방지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-878">Setting the ContextTypeName field also prevents a functional problem where the EntityDataSource in .NET 4.0 throws a ReflectionTypeLoadException when it can't load a type from an assembly via reflection.</span></span> <span data-ttu-id="718a2-879">이 문제는 .NET 4.5에서 해결 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-879">This issue has been fixed in .NET 4.5.</span></span>

### <a name="75-poco-entities-and-change-tracking-proxies"></a><span data-ttu-id="718a2-880">7.5 POCO 엔터티 및 변경 내용 추적 프록시</span><span class="sxs-lookup"><span data-stu-id="718a2-880">7.5       POCO entities and change tracking proxies</span></span>

<span data-ttu-id="718a2-881">Entity Framework를 사용 하면 데이터 클래스 자체를 수정 하지 않고도 데이터 모델과 함께 사용자 지정 데이터 클래스를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-881">Entity Framework enables you to use custom data classes together with your data model without making any modifications to the data classes themselves.</span></span> <span data-ttu-id="718a2-882">즉, 기존 도메인 개체 등의 POCO(Plain Old CLR Object)를 데이터 모델과 함께 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-882">This means that you can use "plain-old" CLR objects (POCO), such as existing domain objects, with your data model.</span></span> <span data-ttu-id="718a2-883">데이터 모델에 정의 된 엔터티에 매핑되는 이러한 POCO 데이터 클래스 (지 속성 무시 개체 라고도 함)는 엔터티 데이터 모델 도구에서 생성 된 엔터티 형식과 동일한 쿼리, 삽입, 업데이트 및 삭제 동작을 대부분 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-883">These POCO data classes (also known as persistence-ignorant objects), which are mapped to entities that are defined in a data model, support most of the same query, insert, update, and delete behaviors as entity types that are generated by the Entity Data Model tools.</span></span>

<span data-ttu-id="718a2-884">Entity Framework는 poco 형식에서 파생 된 프록시 클래스를 만들 수도 있습니다 .이 클래스는 POCO 엔터티에 대 한 지연 로드 및 자동 변경 내용 추적과 같은 기능을 사용 하도록 설정할 때 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-884">Entity Framework can also create proxy classes derived from your POCO types, which are used when you want to enable features such as lazy loading and automatic change tracking on POCO entities.</span></span> <span data-ttu-id="718a2-885">POCO 클래스에는 여기에 설명 된 대로 Entity Framework 프록시를 사용할 수 있도록 특정 요구 사항을 충족 해야 합니다. [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx)합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-885">Your POCO classes must meet certain requirements to allow Entity Framework to use proxies, as described here: [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx).</span></span>

<span data-ttu-id="718a2-886">엔터티 속성의 값이 변경 될 때마다 추적 프록시가 개체 상태 관리자에 게 알리기 때문에 Entity Framework 엔터티의 실제 상태를 항상 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-886">Chance tracking proxies will notify the object state manager each time any of the properties of your entities has its value changed, so Entity Framework knows the actual state of your entities all the time.</span></span> <span data-ttu-id="718a2-887">이렇게 하려면 속성의 setter 메서드 본문에 알림 이벤트를 추가 하 고 해당 이벤트를 처리 하는 개체 상태 관리자를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-887">This is done by adding notification events to the body of the setter methods of your properties, and having the object state manager processing such events.</span></span> <span data-ttu-id="718a2-888">프록시 엔터티를 만드는 것은 Entity Framework에서 만든 이벤트의 추가 된 집합으로 인해 프록시 POCO 엔터티를 만드는 것 보다 일반적으로 비용이 많이 듭니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-888">Note that creating a proxy entity will typically be more expensive than creating a non-proxy POCO entity due to the added set of events created by Entity Framework.</span></span>

<span data-ttu-id="718a2-889">POCO 엔터티에 변경 내용 추적 프록시가 없으면 엔터티의 콘텐츠를 이전에 저장 된 상태의 복사본과 비교 하 여 변경 내용을 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-889">When a POCO entity does not have a change tracking proxy, changes are found by comparing the contents of your entities against a copy of a previous saved state.</span></span> <span data-ttu-id="718a2-890">이러한 심층 비교는 컨텍스트에 많은 엔터티가 있거나, 마지막 비교가 수행 된 이후에 변경 된 내용이 없는 경우에도 엔터티에 매우 많은 양의 속성이 있는 경우 시간이 오래 걸리는 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-890">This deep comparison will become a lengthy process when you have many entities in your context, or when your entities have a very large amount of properties, even if none of them changed since the last comparison took place.</span></span>

<span data-ttu-id="718a2-891">요약 하자면, 변경 내용 추적 프록시를 만들 때 성능 저하가 발생 하지만 변경 내용 추적을 통해 엔터티에 많은 속성이 있거나 모델에 많은 엔터티가 있는 경우 변경 내용 검색 프로세스의 속도를 높일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-891">In summary: you’ll pay a performance hit when creating the change tracking proxy, but change tracking will help you speed up the change detection process when your entities have many properties or when you have many entities in your model.</span></span> <span data-ttu-id="718a2-892">엔터티 양이 너무 많이 증가 하지 않는 적은 수의 속성을 가진 엔터티의 경우 변경 내용 추적 프록시가 그다지 유용 하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-892">For entities with a small number of properties where the amount of entities doesn’t grow too much, having change tracking proxies may not be of much benefit.</span></span>

## <a name="8-loading-related-entities"></a><span data-ttu-id="718a2-893">8 개 관련 엔터티 로드 중</span><span class="sxs-lookup"><span data-stu-id="718a2-893">8 Loading Related Entities</span></span>

### <a name="81-lazy-loading-vs-eager-loading"></a><span data-ttu-id="718a2-894">8.1 지연 로드 및 즉시 로드</span><span class="sxs-lookup"><span data-stu-id="718a2-894">8.1 Lazy Loading vs. Eager Loading</span></span>

<span data-ttu-id="718a2-895">Entity Framework는 대상 엔터티와 관련 된 엔터티를 로드 하는 여러 가지 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-895">Entity Framework offers several different ways to load the entities that are related to your target entity.</span></span> <span data-ttu-id="718a2-896">예를 들어 제품을 쿼리 하는 경우 관련 주문이 개체 상태 관리자에 로드 되는 방법이 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-896">For example, when you query for Products, there are different ways that the related Orders will be loaded into the Object State Manager.</span></span> <span data-ttu-id="718a2-897">성능 관점에서 볼 때 관련 엔터티를 로드할 때 가장 중요 한 질문은 지연 로드를 사용할지 즉시 로드를 사용할지 여부입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-897">From a performance standpoint, the biggest question to consider when loading related entities will be whether to use Lazy Loading or Eager Loading.</span></span>

<span data-ttu-id="718a2-898">즉시 로드를 사용 하는 경우 관련 엔터티가 대상 엔터티 집합과 함께 로드 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-898">When using Eager Loading, the related entities are loaded along with your target entity set.</span></span> <span data-ttu-id="718a2-899">쿼리에서 Include 문을 사용 하 여 가져올 관련 엔터티를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-899">You use an Include statement in your query to indicate which related entities you want to bring in.</span></span>

<span data-ttu-id="718a2-900">지연 로드를 사용 하는 경우 초기 쿼리는 대상 엔터티 집합에만 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-900">When using Lazy Loading, your initial query only brings in the target entity set.</span></span> <span data-ttu-id="718a2-901">그러나 탐색 속성에 액세스할 때마다 저장소에 대해 다른 쿼리를 실행 하 여 관련 엔터티를 로드 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-901">But whenever you access a navigation property, another query is issued against the store to load the related entity.</span></span>

<span data-ttu-id="718a2-902">엔터티가 로드 된 후에는 지연 로드를 사용 하 든 즉시 로드를 사용 하는지에 관계 없이 엔터티를 위한 추가 쿼리는 개체 상태 관리자에서 직접 로드 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-902">Once an entity has been loaded, any further queries for the entity will load it directly from the Object State Manager, whether you are using lazy loading or eager loading.</span></span>

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a><span data-ttu-id="718a2-903">8.2 지연 로드와 즉시 로드 중에서 선택 하는 방법</span><span class="sxs-lookup"><span data-stu-id="718a2-903">8.2 How to choose between Lazy Loading and Eager Loading</span></span>

<span data-ttu-id="718a2-904">중요 한 것은 응용 프로그램에 대해 적절 한 선택을 수행할 수 있도록 지연 로드와 즉시 로드 간의 차이점을 이해 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-904">The important thing is that you understand the difference between Lazy Loading and Eager Loading so that you can make the correct choice for your application.</span></span> <span data-ttu-id="718a2-905">이를 통해 데이터베이스에 대 한 여러 요청과 많은 페이로드를 포함할 수 있는 단일 요청 간의 균형을 평가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-905">This will help you evaluate the tradeoff between multiple requests against the database versus a single request that may contain a large payload.</span></span> <span data-ttu-id="718a2-906">응용 프로그램의 일부에서 즉시 로드를 사용 하 고 다른 부분의 지연 로드를 사용 하는 것이 적합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-906">It may be appropriate to use eager loading in some parts of your application and lazy loading in other parts.</span></span>

<span data-ttu-id="718a2-907">내부적으로 발생 하는 상황에 대 한 예로 영국에 거주 하는 고객 및 주문 수를 쿼리해야 한다고 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-907">As an example of what's happening under the hood, suppose you want to query for the customers who live in the UK and their order count.</span></span>

<span data-ttu-id="718a2-908">**즉시 로드 사용**</span><span class="sxs-lookup"><span data-stu-id="718a2-908">**Using Eager Loading**</span></span>

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

<span data-ttu-id="718a2-909">**지연 로드 사용**</span><span class="sxs-lookup"><span data-stu-id="718a2-909">**Using Lazy Loading**</span></span>

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    context.ContextOptions.LazyLoadingEnabled = true;

    //Notice that the Include method call is missing in the query
    var ukCustomers = context.Customers.Where(c => c.Address.Country == "UK");

    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

<span data-ttu-id="718a2-910">즉시 로드를 사용 하는 경우 모든 고객 및 모든 주문을 반환 하는 단일 쿼리를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-910">When using eager loading, you'll issue a single query that returns all customers and all orders.</span></span> <span data-ttu-id="718a2-911">저장소 명령은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-911">The store command looks like:</span></span>

``` SQL
SELECT
[Project1].[C1] AS [C1],
[Project1].[CustomerID] AS [CustomerID],
[Project1].[CompanyName] AS [CompanyName],
[Project1].[ContactName] AS [ContactName],
[Project1].[ContactTitle] AS [ContactTitle],
[Project1].[Address] AS [Address],
[Project1].[City] AS [City],
[Project1].[Region] AS [Region],
[Project1].[PostalCode] AS [PostalCode],
[Project1].[Country] AS [Country],
[Project1].[Phone] AS [Phone],
[Project1].[Fax] AS [Fax],
[Project1].[C2] AS [C2],
[Project1].[OrderID] AS [OrderID],
[Project1].[CustomerID1] AS [CustomerID1],
[Project1].[EmployeeID] AS [EmployeeID],
[Project1].[OrderDate] AS [OrderDate],
[Project1].[RequiredDate] AS [RequiredDate],
[Project1].[ShippedDate] AS [ShippedDate],
[Project1].[ShipVia] AS [ShipVia],
[Project1].[Freight] AS [Freight],
[Project1].[ShipName] AS [ShipName],
[Project1].[ShipAddress] AS [ShipAddress],
[Project1].[ShipCity] AS [ShipCity],
[Project1].[ShipRegion] AS [ShipRegion],
[Project1].[ShipPostalCode] AS [ShipPostalCode],
[Project1].[ShipCountry] AS [ShipCountry]
FROM ( SELECT
      [Extent1].[CustomerID] AS [CustomerID],
       [Extent1].[CompanyName] AS [CompanyName],
       [Extent1].[ContactName] AS [ContactName],
       [Extent1].[ContactTitle] AS [ContactTitle],
       [Extent1].[Address] AS [Address],
       [Extent1].[City] AS [City],
       [Extent1].[Region] AS [Region],
       [Extent1].[PostalCode] AS [PostalCode],
       [Extent1].[Country] AS [Country],
       [Extent1].[Phone] AS [Phone],
       [Extent1].[Fax] AS [Fax],
      1 AS [C1],
       [Extent2].[OrderID] AS [OrderID],
       [Extent2].[CustomerID] AS [CustomerID1],
       [Extent2].[EmployeeID] AS [EmployeeID],
       [Extent2].[OrderDate] AS [OrderDate],
       [Extent2].[RequiredDate] AS [RequiredDate],
       [Extent2].[ShippedDate] AS [ShippedDate],
       [Extent2].[ShipVia] AS [ShipVia],
       [Extent2].[Freight] AS [Freight],
       [Extent2].[ShipName] AS [ShipName],
       [Extent2].[ShipAddress] AS [ShipAddress],
       [Extent2].[ShipCity] AS [ShipCity],
       [Extent2].[ShipRegion] AS [ShipRegion],
       [Extent2].[ShipPostalCode] AS [ShipPostalCode],
       [Extent2].[ShipCountry] AS [ShipCountry],
      CASE WHEN ([Extent2].[OrderID] IS NULL) THEN CAST(NULL AS int) ELSE 1 END AS [C2]
      FROM  [dbo].[Customers] AS [Extent1]
      LEFT OUTER JOIN [dbo].[Orders] AS [Extent2] ON [Extent1].[CustomerID] = [Extent2].[CustomerID]
      WHERE N'UK' = [Extent1].[Country]
)  AS [Project1]
ORDER BY [Project1].[CustomerID] ASC, [Project1].[C2] ASC
```

<span data-ttu-id="718a2-912">지연 로드를 사용 하는 경우 처음에 다음 쿼리를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-912">When using lazy loading, you'll issue the following query initially:</span></span>

``` SQL
SELECT
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[CompanyName] AS [CompanyName],
[Extent1].[ContactName] AS [ContactName],
[Extent1].[ContactTitle] AS [ContactTitle],
[Extent1].[Address] AS [Address],
[Extent1].[City] AS [City],
[Extent1].[Region] AS [Region],
[Extent1].[PostalCode] AS [PostalCode],
[Extent1].[Country] AS [Country],
[Extent1].[Phone] AS [Phone],
[Extent1].[Fax] AS [Fax]
FROM [dbo].[Customers] AS [Extent1]
WHERE N'UK' = [Extent1].[Country]
```

<span data-ttu-id="718a2-913">그리고 고객의 주문 탐색 속성에 액세스할 때마다 다음과 같은 다른 쿼리가 스토어에 대해 발급 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-913">And each time you access the Orders navigation property of a customer another query like the following is issued against the store:</span></span>

``` SQL
exec sp_executesql N'SELECT
[Extent1].[OrderID] AS [OrderID],
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[EmployeeID] AS [EmployeeID],
[Extent1].[OrderDate] AS [OrderDate],
[Extent1].[RequiredDate] AS [RequiredDate],
[Extent1].[ShippedDate] AS [ShippedDate],
[Extent1].[ShipVia] AS [ShipVia],
[Extent1].[Freight] AS [Freight],
[Extent1].[ShipName] AS [ShipName],
[Extent1].[ShipAddress] AS [ShipAddress],
[Extent1].[ShipCity] AS [ShipCity],
[Extent1].[ShipRegion] AS [ShipRegion],
[Extent1].[ShipPostalCode] AS [ShipPostalCode],
[Extent1].[ShipCountry] AS [ShipCountry]
FROM [dbo].[Orders] AS [Extent1]
WHERE [Extent1].[CustomerID] = @EntityKeyValue1',N'@EntityKeyValue1 nchar(5)',@EntityKeyValue1=N'AROUT'
```

<span data-ttu-id="718a2-914">자세한 내용은 [관련 개체 로드](https://msdn.microsoft.com/library/bb896272.aspx)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="718a2-914">For more information, see the [Loading Related Objects](https://msdn.microsoft.com/library/bb896272.aspx).</span></span>

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a><span data-ttu-id="718a2-915">8.2.1 지연 로드 및 즉시 로드 참고 자료 시트</span><span class="sxs-lookup"><span data-stu-id="718a2-915">8.2.1 Lazy Loading versus Eager Loading cheat sheet</span></span>

<span data-ttu-id="718a2-916">즉시 로드와 지연 로드를 선택 하는 데에는 한 가지 크기의 제한이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-916">There’s no such thing as a one-size-fits-all to choosing eager loading versus lazy loading.</span></span> <span data-ttu-id="718a2-917">잘 의사 결정을 내릴 수 있도록 두 전략 간의 차이점을 이해 하기 위해 먼저 시도 합니다. 또한 코드가 다음 시나리오에 적합 한지 고려해 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-917">Try first to understand the differences between both strategies so you can do a well informed decision; also, consider if your code fits to any of the following scenarios:</span></span>

| <span data-ttu-id="718a2-918">시나리오</span><span class="sxs-lookup"><span data-stu-id="718a2-918">Scenario</span></span>                                                                    | <span data-ttu-id="718a2-919">제안 사항</span><span class="sxs-lookup"><span data-stu-id="718a2-919">Our Suggestion</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="718a2-920">인출 된 엔터티에서 많은 탐색 속성에 액세스 해야 하나요?</span><span class="sxs-lookup"><span data-stu-id="718a2-920">Do you need to access many navigation properties from the fetched entities?</span></span> | <span data-ttu-id="718a2-921">**아니요** -두 옵션을 모두 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-921">**No** - Both options will probably do.</span></span> <span data-ttu-id="718a2-922">그러나 쿼리가 가져오는 페이로드가 너무 크지 않은 경우 즉시 로드를 사용 하면 개체를 구체화 하는 데 네트워크 왕복이 더 필요 하지 않으므로 성능이 향상 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-922">However, if the payload your query is bringing is not too big, you may experience performance benefits by using Eager loading as it’ll require less network round trips to materialize your objects.</span></span> <br/> <br/> <span data-ttu-id="718a2-923">**예** -엔터티에서 많은 탐색 속성에 액세스 해야 하는 경우 즉시 로드를 사용 하 여 쿼리에 여러 개의 include 문을 사용 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-923">**Yes** -  If you need to access many navigation properties from the entities, you’d do that by using multiple include statements in your query with Eager loading.</span></span> <span data-ttu-id="718a2-924">포함 하는 엔터티가 많을 수록 쿼리에서 반환 하는 페이로드가 커집니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-924">The more entities you include, the bigger the payload your query will return.</span></span> <span data-ttu-id="718a2-925">쿼리에 세 개 이상의 엔터티를 포함 한 후에는 지연 로드로 전환 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-925">Once you include three or more entities into your query, consider switching to Lazy loading.</span></span> |
| <span data-ttu-id="718a2-926">런타임에 필요한 데이터를 정확 하 게 알고 있나요?</span><span class="sxs-lookup"><span data-stu-id="718a2-926">Do you know exactly what data will be needed at run time?</span></span>                   | <span data-ttu-id="718a2-927">지연 로드는 사용자에 게 더 적합 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-927">**No** - Lazy loading will be better for you.</span></span> <span data-ttu-id="718a2-928">그렇지 않은 경우에는 필요 하지 않은 데이터에 대 한 쿼리를 종료할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-928">Otherwise, you may end up querying for data that you will not need.</span></span> <br/> <br/> <span data-ttu-id="718a2-929">**예** -즉시 로드가 최상의 방법일 수 있습니다. 이를 통해 전체 집합을 더 빠르게 로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-929">**Yes** - Eager loading is probably your best bet; it will help loading entire sets faster.</span></span> <span data-ttu-id="718a2-930">쿼리에서 매우 많은 양의 데이터를 인출 해야 하는데 속도가 너무 느리면 대신 지연 로드를 시도 하십시오.</span><span class="sxs-lookup"><span data-stu-id="718a2-930">If your query requires fetching a very large amount of data, and this becomes too slow, then try Lazy loading instead.</span></span>                                                                                                                                                                                                                                                       |
| <span data-ttu-id="718a2-931">코드가 데이터베이스에서 지금 실행 되 고 있나요?</span><span class="sxs-lookup"><span data-stu-id="718a2-931">Is your code executing far from your database?</span></span> <span data-ttu-id="718a2-932">(네트워크 대기 시간 증가)</span><span class="sxs-lookup"><span data-stu-id="718a2-932">(increased network latency)</span></span>  | <span data-ttu-id="718a2-933">**아니요** -네트워크 대기 시간이 문제가 아니면 지연 로드를 사용 하면 코드를 간소화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-933">**No** - When the network latency is not an issue, using Lazy loading may simplify your code.</span></span> <span data-ttu-id="718a2-934">응용 프로그램의 토폴로지가 변경 될 수 있으므로 부여 된 데이터베이스에 대 한 근접성을 사용 하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="718a2-934">Remember that the topology of your application may change, so don’t take database proximity for granted.</span></span> <br/> <br/> <span data-ttu-id="718a2-935">**예** -네트워크가 문제인 경우 시나리오에 적합 한 항목을 결정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-935">**Yes** - When the network is a problem, only you can decide what fits better for your scenario.</span></span> <span data-ttu-id="718a2-936">일반적으로 즉시 로드는 왕복이 더 적기 때문에 더 효율적입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-936">Typically Eager loading will be better because it requires fewer round trips.</span></span>                                                                                                                                                                                                      |


#### <a name="822-performance-concerns-with-multiple-includes"></a><span data-ttu-id="718a2-937">여러 포함의 성능 문제 8.2.2</span><span class="sxs-lookup"><span data-stu-id="718a2-937">8.2.2       Performance concerns with multiple Includes</span></span>

<span data-ttu-id="718a2-938">서버 응답 시간 문제를 포함 하는 성능 관련 질문이 있는 경우 문제의 원인에는 여러 개의 Include 문이 있는 쿼리가 자주 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-938">When we hear performance questions that involve server response time problems, the source of the issue is frequently queries with multiple Include statements.</span></span> <span data-ttu-id="718a2-939">쿼리에 관련 된 엔터티를 포함 하는 것은 매우 강력 하지만 내부적으로 발생 하는 일을 이해 하는 것이 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-939">While including related entities in a query is powerful, it's important to understand what's happening under the covers.</span></span>

<span data-ttu-id="718a2-940">내부 계획 컴파일러를 통해 저장 명령을 생성 하기 위해 여러 Include 문을 포함 하는 쿼리는 상대적으로 시간이 오래 걸립니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-940">It takes a relatively long time for a query with multiple Include statements in it to go through our internal plan compiler to produce the store command.</span></span> <span data-ttu-id="718a2-941">이 시간의 대부분은 결과 쿼리를 최적화 하는 데 소요 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-941">The majority of this time is spent trying to optimize the resulting query.</span></span> <span data-ttu-id="718a2-942">생성 된 저장소 명령은 매핑에 따라 각 포함에 대 한 외부 조인 또는 공용 구조체를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-942">The generated store command will contain an Outer Join or Union for each Include, depending on your mapping.</span></span> <span data-ttu-id="718a2-943">이와 같은 쿼리는 데이터베이스에서 큰 연결 된 그래프를 단일 페이로드에 acerbate, 특히 페이로드에 중복성이 많은 경우 (예: 여러 수준의 포함을 사용 하는 경우)에는 모든 대역폭 문제를 해결 합니다. 일대다 방향의 연결입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-943">Queries like this will bring in large connected graphs from your database in a single payload, which will acerbate any bandwidth issues, especially when there is a lot of redundancy in the payload (for example, when multiple levels of Include are used to traverse associations in the one-to-many direction).</span></span>

<span data-ttu-id="718a2-944">ToTraceString을 사용 하 고 SQL Server Management Studio의 저장소 명령을 실행 하 여 페이로드 크기를 확인 하 여 쿼리가 과도 하 게 큰 페이로드를 반환 하는 경우를 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-944">You can check for cases where your queries are returning excessively large payloads by accessing the underlying TSQL for the query by using ToTraceString and executing the store command in SQL Server Management Studio to see the payload size.</span></span> <span data-ttu-id="718a2-945">이러한 경우 필요한 데이터를 가져오기 위해 쿼리에서 Include 문의 수를 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-945">In such cases you can try to reduce the number of Include statements in your query to just bring in the data you need.</span></span> <span data-ttu-id="718a2-946">또는 다음과 같이 더 작은 하위 쿼리 시퀀스로 쿼리를 중단할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-946">Or you may be able to break your query into a smaller sequence of subqueries, for example:</span></span>

<span data-ttu-id="718a2-947">**쿼리를 중단 하기 전에:**</span><span class="sxs-lookup"><span data-stu-id="718a2-947">**Before breaking the query:**</span></span>

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var customers = from c in context.Customers.Include(c => c.Orders)
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

<span data-ttu-id="718a2-948">**쿼리를 중단 한 후:**</span><span class="sxs-lookup"><span data-stu-id="718a2-948">**After breaking the query:**</span></span>

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var orders = from o in context.Orders
                 where o.Customer.LastName.StartsWith(lastNameParameter)
                 select o;

    orders.Load();

    var customers = from c in context.Customers
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

<span data-ttu-id="718a2-949">이는 컨텍스트에서 id 확인 및 연결 픽스업을 자동으로 수행 해야 하는 기능을 사용 하기 때문에 추적 된 쿼리에만 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-949">This will work only on tracked queries, as we are making use of the ability the context has to perform identity resolution and association fixup automatically.</span></span>

<span data-ttu-id="718a2-950">지연 로드와 마찬가지로 더 작은 페이로드에 대해 더 많은 쿼리가 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-950">As with lazy loading, the tradeoff will be more queries for smaller payloads.</span></span> <span data-ttu-id="718a2-951">개별 속성의 프로젝션을 사용 하 여 각 엔터티에서 필요한 데이터만 명시적으로 선택할 수도 있지만이 경우에는 엔터티를 로드 하지 않으며 업데이트가 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-951">You can also use projections of individual properties to explicitly select only the data you need from each entity, but you will not be loading entities in this case, and updates will not be supported.</span></span>

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a><span data-ttu-id="718a2-952">8.2.3 속성의 지연 로드를 가져오는 방법</span><span class="sxs-lookup"><span data-stu-id="718a2-952">8.2.3 Workaround to get lazy loading of properties</span></span>

<span data-ttu-id="718a2-953">Entity Framework 현재 스칼라 또는 복합 속성의 지연 로드를 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-953">Entity Framework currently doesn’t support lazy loading of scalar or complex properties.</span></span> <span data-ttu-id="718a2-954">그러나 BLOB과 같은 커다란 개체를 포함 하는 테이블이 있는 경우 테이블 분할을 사용 하 여 대량 속성을 개별 엔터티로 구분할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-954">However, in cases where you have a table that includes a large object such as a BLOB, you can use table splitting to separate the large properties into a separate entity.</span></span> <span data-ttu-id="718a2-955">예를 들어 varbinary photo 열을 포함 하는 Product 테이블이 있다고 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-955">For example, suppose you have a Product table that includes a varbinary photo column.</span></span> <span data-ttu-id="718a2-956">쿼리에서이 속성에 액세스할 필요가 없는 경우 테이블 분할을 사용 하 여 일반적으로 필요한 엔터티 부분만 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-956">If you don't frequently need to access this property in your queries, you can use table splitting to bring in only the parts of the entity that you normally need.</span></span> <span data-ttu-id="718a2-957">제품 사진을 나타내는 엔터티는 명시적으로 필요한 경우에만 로드 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-957">The entity representing the product photo will only be loaded when you explicitly need it.</span></span>

<span data-ttu-id="718a2-958">테이블 분할을 사용 하도록 설정 하는 방법을 보여주는 좋은 리소스는 Gil Fink "테이블 분할에서 Entity Framework" 블로그 게시물: \<http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx> 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-958">A good resource that shows how to enable table splitting is Gil Fink's "Table Splitting in Entity Framework" blog post: \<http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.</span></span>

## <a name="9-other-considerations"></a><span data-ttu-id="718a2-959">9 기타 고려 사항</span><span class="sxs-lookup"><span data-stu-id="718a2-959">9 Other considerations</span></span>

### <a name="91-server-garbage-collection"></a><span data-ttu-id="718a2-960">9.1 서버 가비지 수집</span><span class="sxs-lookup"><span data-stu-id="718a2-960">9.1      Server Garbage Collection</span></span>

<span data-ttu-id="718a2-961">일부 사용자는 가비지 수집기가 제대로 구성 되지 않은 경우 예상 되는 병렬 처리를 제한 하는 리소스 경합이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-961">Some users might experience resource contention that limits the parallelism they are expecting when the Garbage Collector is not properly configured.</span></span> <span data-ttu-id="718a2-962">다중 스레드 시나리오 또는 서버 쪽 시스템과 유사한 응용 프로그램에서 EF를 사용할 때마다 서버 가비지 수집을 사용 하도록 설정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-962">Whenever EF is used in a multithreaded scenario, or in any application that resembles a server-side system, make sure to enable Server Garbage Collection.</span></span> <span data-ttu-id="718a2-963">이 작업은 응용 프로그램 구성 파일의 간단한 설정을 통해 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-963">This is done via a simple setting in your application config file:</span></span>

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

<span data-ttu-id="718a2-964">이렇게 하면 스레드 경합이 줄어들고 CPU 포화 시나리오에서 최대 30%까지 처리량이 증가 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-964">This should decrease your thread contention and increase your throughput by up to 30% in CPU saturated scenarios.</span></span> <span data-ttu-id="718a2-965">일반적으로 서버 가비지 컬렉션 뿐만 아니라 클래식 가비지 수집 (UI 및 클라이언트 쪽 시나리오에 맞게 조정 됨)을 사용 하 여 응용 프로그램의 동작 방식을 테스트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-965">In general terms, you should always test how your application behaves using the classic Garbage Collection (which is better tuned for UI and client side scenarios) as well as the Server Garbage Collection.</span></span>

### <a name="92-autodetectchanges"></a><span data-ttu-id="718a2-966">9.2 AutoDetectChanges</span><span class="sxs-lookup"><span data-stu-id="718a2-966">9.2      AutoDetectChanges</span></span>

<span data-ttu-id="718a2-967">앞서 설명한 것 처럼 개체 캐시에 많은 엔터티가 있을 때 Entity Framework 성능 문제를 표시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-967">As mentioned earlier, Entity Framework might show performance issues when the object cache has many entities.</span></span> <span data-ttu-id="718a2-968">Add, Remove, Find, Entry 및 SaveChanges와 같은 특정 작업은 개체 캐시가 얼마나 큰지를 기준으로 많은 양의 CPU를 사용할 수 있는 DetectChanges에 대 한 호출을 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-968">Certain operations, such as Add, Remove, Find, Entry and SaveChanges, trigger calls to DetectChanges which might consume a large amount of CPU based on how large the object cache has become.</span></span> <span data-ttu-id="718a2-969">그 이유는 개체 캐시와 개체 상태 관리자가 컨텍스트에 수행 된 각 작업에서 가능한 한 동기화 된 상태를 유지 하려고 하기 때문입니다. 이렇게 하면 생성 된 데이터가 광범위 한 시나리오에서 정확 하 게 유지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-969">The reason for this is that the object cache and the object state manager try to stay as synchronized as possible on each operation performed to a context so that the produced data is guaranteed to be correct under a wide array of scenarios.</span></span>

<span data-ttu-id="718a2-970">일반적으로 응용 프로그램의 전체 수명 동안 Entity Framework의 자동 변경 검색을 사용 하도록 설정 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-970">It is generally a good practice to leave Entity Framework’s automatic change detection enabled for the entire life of your application.</span></span> <span data-ttu-id="718a2-971">높은 CPU 사용량으로 인 한 시나리오의 영향을 받고 프로필이 DetectChanges에 대 한 호출 임을 나타내는 경우 코드의 중요 한 부분에서 AutoDetectChanges를 일시적으로 해제 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-971">If your scenario is being negatively affected by high CPU usage and your profiles indicate that the culprit is the call to DetectChanges, consider temporarily turning off AutoDetectChanges in the sensitive portion of your code:</span></span>

``` csharp
try
{
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    ...
}
finally
{
    context.Configuration.AutoDetectChangesEnabled = true;
}
```

<span data-ttu-id="718a2-972">AutoDetectChanges를 해제 하기 전에 엔터티에서 발생 하는 변경 내용에 대 한 특정 정보를 추적 하는 기능이 손실 될 Entity Framework 수 있다는 것을 이해 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-972">Before turning off AutoDetectChanges, it’s good to understand that this might cause Entity Framework to lose its ability to track certain information about the changes that are taking place on the entities.</span></span> <span data-ttu-id="718a2-973">올바르게 처리 되지 않으면 응용 프로그램에서 데이터가 일치 하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-973">If handled incorrectly, this might cause data inconsistency on your application.</span></span> <span data-ttu-id="718a2-974">AutoDetectChanges 해제에 대 한 자세한 내용은 읽을 \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/> 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-974">For more information on turning off AutoDetectChanges, read \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.</span></span>

### <a name="93-context-per-request"></a><span data-ttu-id="718a2-975">9.3 요청 당 컨텍스트</span><span class="sxs-lookup"><span data-stu-id="718a2-975">9.3      Context per request</span></span>

<span data-ttu-id="718a2-976">가장 최적의 성능 환경을 제공 하기 위해 Entity Framework의 컨텍스트는 수명이 짧은 인스턴스로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-976">Entity Framework’s contexts are meant to be used as short-lived instances in order to provide the most optimal performance experience.</span></span> <span data-ttu-id="718a2-977">컨텍스트는 수명이 짧고 폐기 되기 때문에 가능 하면 매우 간단 하 고 메타 데이터를 다시 활용할 수 있도록 구현 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-977">Contexts are expected to be short lived and discarded, and as such have been implemented to be very lightweight and reutilize metadata whenever possible.</span></span> <span data-ttu-id="718a2-978">웹 시나리오에서는이를 염두에 두고 단일 요청 기간 이상으로 컨텍스트를 포함 하지 않는 것이 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-978">In web scenarios it’s important to keep this in mind and not have a context for more than the duration of a single request.</span></span> <span data-ttu-id="718a2-979">마찬가지로 비 웹 시나리오에서는 Entity Framework의 다양 한 캐싱 수준을 이해 하 여 컨텍스트를 삭제 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-979">Similarly, in non-web scenarios, context should be discarded based on your understanding of the different levels of caching in the Entity Framework.</span></span> <span data-ttu-id="718a2-980">일반적으로 응용 프로그램의 수명 내내 컨텍스트 인스턴스를 사용 하지 않아야 하 고 스레드 및 정적 컨텍스트 당 컨텍스트를 사용 하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-980">Generally speaking, one should avoid having a context instance throughout the life of the application, as well as contexts per thread and static contexts.</span></span>

### <a name="94-database-null-semantics"></a><span data-ttu-id="718a2-981">9.4 데이터베이스 null 의미 체계</span><span class="sxs-lookup"><span data-stu-id="718a2-981">9.4      Database null semantics</span></span>

<span data-ttu-id="718a2-982">기본적으로 Entity Framework C @ no__t-0 null 비교 의미 체계가 있는 SQL 코드를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-982">Entity Framework by default will generate SQL code that has C\# null comparison semantics.</span></span> <span data-ttu-id="718a2-983">다음 예제 쿼리를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="718a2-983">Consider the following example query:</span></span>

``` csharp
            int? categoryId = 7;
            int? supplierId = 8;
            decimal? unitPrice = 0;
            short? unitsInStock = 100;
            short? unitsOnOrder = 20;
            short? reorderLevel = null;

            var q = from p incontext.Products
                    wherep.Category.CategoryName == "Beverages"
                          || (p.CategoryID == categoryId
                                || p.SupplierID == supplierId
                                || p.UnitPrice == unitPrice
                                || p.UnitsInStock == unitsInStock
                                || p.UnitsOnOrder == unitsOnOrder
                                || p.ReorderLevel == reorderLevel)
                    select p;

            var r = q.ToList();
```

<span data-ttu-id="718a2-984">이 예제에서는 공급자 및 UnitPrice와 같이 엔터티의 nullable 속성에 대해 다양 한 nullable 변수를 비교 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-984">In this example, we’re comparing a number of nullable variables against nullable properties on the entity, such as SupplierID and UnitPrice.</span></span> <span data-ttu-id="718a2-985">이 쿼리에 대해 생성 된 SQL은 매개 변수 값이 열 값과 같은지 또는 매개 변수와 열 값이 모두 null 인지를 요청 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-985">The generated SQL for this query will ask if the parameter value is the same as the column value, or if both the parameter and the column values are null.</span></span> <span data-ttu-id="718a2-986">이렇게 하면 데이터베이스 서버에서 null을 처리 하는 방식이 숨겨지고 여러 데이터베이스 공급 업체에 일관 된 C @ no__t null 환경이 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-986">This will hide the way the database server handles nulls and will provide a consistent C\# null experience across different database vendors.</span></span> <span data-ttu-id="718a2-987">반면에 생성 된 코드는 비트 복잡 쿼리의 where 문에 있는 비교의 양이 증가 하는 경우 제대로 수행 되지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-987">On the other hand, the generated code is a bit convoluted and may not perform well when the amount of comparisons in the where statement of the query grows to a large number.</span></span>

<span data-ttu-id="718a2-988">이러한 상황을 처리 하는 한 가지 방법은 데이터베이스 null 의미 체계를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-988">One way to deal with this situation is by using database null semantics.</span></span> <span data-ttu-id="718a2-989">이는 no__t null 의미 체계와 다르게 동작할 수 있으며,이는 데이터베이스 엔진에서 null 값을 처리 하는 방법을 제공 하는 간단한 SQL을 생성 하기 Entity Framework 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-989">Note that this might potentially behave differently to the C\# null semantics since now Entity Framework will generate simpler SQL that exposes the way the database engine handles null values.</span></span> <span data-ttu-id="718a2-990">데이터베이스 null 의미 체계는 컨텍스트 구성에 대해 하나의 단일 구성 줄을 사용 하 여 컨텍스트별으로 활성화 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-990">Database null semantics can be activated per-context with one single configuration line against the context configuration:</span></span>

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

<span data-ttu-id="718a2-991">데이터베이스 null 의미 체계를 사용 하는 경우 중소 규모의 쿼리에는 크게 성능 개선이 표시 되지 않지만 잠재적 null 비교가 많은 쿼리에서는 차이가 더 두드러집니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-991">Small to medium sized queries will not display a perceptible performance improvement when using database null semantics, but the difference will become more noticeable on queries with a large number of potential null comparisons.</span></span>

<span data-ttu-id="718a2-992">위의 예제 쿼리에서 성능 차이는 제어 되는 환경에서 실행 되는 마이크로 벤치 마크의 2% 미만 이었습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-992">In the example query above, the performance difference was less than 2% in a microbenchmark running in a controlled environment.</span></span>

### <a name="95-async"></a><span data-ttu-id="718a2-993">9.5 비동기</span><span class="sxs-lookup"><span data-stu-id="718a2-993">9.5      Async</span></span>

<span data-ttu-id="718a2-994">Entity Framework 6에서는 .NET 4.5 이상에서 실행 될 때 비동기 작업에 대 한 지원이 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-994">Entity Framework 6 introduced support of async operations when running on .NET 4.5 or later.</span></span> <span data-ttu-id="718a2-995">대부분의 경우 IO 관련 경합이 있는 응용 프로그램은 비동기 쿼리 및 저장 작업을 사용 하는 데 가장 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-995">For the most part, applications that have IO related contention will benefit the most from using asynchronous query and save operations.</span></span> <span data-ttu-id="718a2-996">응용 프로그램의 IO 경합이 발생 하지 않는 경우에는 async를 사용 하는 것이 가장 좋습니다. 비동기를 사용 하는 경우 동기식으로 실행 되 고 동기 호출과 같은 시간 내에 결과를 반환 하는 것입니다. 또는 최악의 경우에는 단순히 비동기 작업에 대 한 실행을 연기 하 고 추가 tim을 추가 합니다. e-시나리오를 완료 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-996">If your application does not suffer from IO contention, the use of async will, in the best cases, run synchronously and return the result in the same amount of time as a synchronous call, or in the worst case, simply defer execution to an asynchronous task and add extra time to the completion of your scenario.</span></span>

<span data-ttu-id="718a2-997">방문 비동기 응용 프로그램의 성능이 향상 됩니다 하는 경우를 결정 하는 데 도움이 되는 비동기 프로그래밍 작업에 대 한 내용은 [http://msdn.microsoft.com/library/hh191443.aspx ](https://msdn.microsoft.com/library/hh191443.aspx)합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-997">For information on how asynchronous programming work that will help you deciding if async will improve the performance of your application visit [http://msdn.microsoft.com/library/hh191443.aspx](https://msdn.microsoft.com/library/hh191443.aspx).</span></span> <span data-ttu-id="718a2-998">Entity Framework에서 비동기 작업을 사용 하는 방법에 대 한 자세한 내용은 [ 비동기 쿼리 및 저장 @ no__t-1을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="718a2-998">For more information on the use of async operations on Entity Framework, see [Async Query and Save](~/ef6/fundamentals/async.md
).</span></span>

### <a name="96-ngen"></a><span data-ttu-id="718a2-999">9.6 NGEN</span><span class="sxs-lookup"><span data-stu-id="718a2-999">9.6      NGEN</span></span>

<span data-ttu-id="718a2-1000">Entity Framework 6은 .NET Framework의 기본 설치에는 제공 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1000">Entity Framework 6 does not come in the default installation of .NET framework.</span></span> <span data-ttu-id="718a2-1001">따라서 Entity Framework 어셈블리는 기본적으로 NGEN이 아닙니다. 즉, 모든 Entity Framework 코드는 다른 MSIL 어셈블리와 동일한 JIT'ing 비용을 적용 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1001">As such, the Entity Framework assemblies are not NGEN’d by default which means that all the Entity Framework code is subject to the same JIT’ing costs as any other MSIL assembly.</span></span> <span data-ttu-id="718a2-1002">이렇게 하면 프로덕션 환경에서 응용 프로그램을 개발 하는 동안 F5 환경이 저하 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1002">This might degrade the F5 experience while developing and also the cold startup of your application in the production environments.</span></span> <span data-ttu-id="718a2-1003">JIT'ing의 CPU 및 메모리 비용을 줄이기 위해 Entity Framework 이미지를 적절 하 게 NGEN 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1003">In order to reduce the CPU and memory costs of JIT’ing it is advisable to NGEN the Entity Framework images as appropriate.</span></span> <span data-ttu-id="718a2-1004">NGEN을 사용 하 여 Entity Framework 6의 시작 성능을 개선 하는 방법에 대 한 자세한 내용은 [ngen을 사용 하 여 시작 성능 향상](~/ef6/fundamentals/performance/ngen.md)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="718a2-1004">For more information on how to improve the startup performance of Entity Framework 6 with NGEN, see [Improving Startup Performance with NGen](~/ef6/fundamentals/performance/ngen.md).</span></span>

### <a name="97-code-first-versus-edmx"></a><span data-ttu-id="718a2-1005">9.7 Code First 및 EDMX</span><span class="sxs-lookup"><span data-stu-id="718a2-1005">9.7      Code First versus EDMX</span></span>

<span data-ttu-id="718a2-1006">개념적 모델의 메모리 내 표현 (개체), 저장소 스키마 (데이터베이스) 및 간의 매핑을 포함 하 여 개체 지향 프로그래밍과 관계형 데이터베이스 간의 임피던스 불일치 문제에 대 한 이유를 Entity Framework 합니다. 두.</span><span class="sxs-lookup"><span data-stu-id="718a2-1006">Entity Framework reasons about the impedance mismatch problem between object oriented programming and relational databases by having an in-memory representation of the conceptual model (the objects), the storage schema (the database) and a mapping between the two.</span></span> <span data-ttu-id="718a2-1007">이 메타 데이터를 엔터티 데이터 모델 또는 간단한 EDM 이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1007">This metadata is called an Entity Data Model, or EDM for short.</span></span> <span data-ttu-id="718a2-1008">이 EDM에서 Entity Framework는 뷰를 파생 시켜 메모리의 개체에서 데이터베이스로 데이터를 왕복 하 고 그 뒤로 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1008">From this EDM, Entity Framework will derive the views to roundtrip data from the objects in memory to the database and back.</span></span>

<span data-ttu-id="718a2-1009">개념적 모델, 저장소 스키마 및 매핑을 공식적으로 지정 하는 EDMX 파일에 Entity Framework를 사용 하는 경우 모델 로드 단계는 EDM이 올바른지 확인 해야 합니다. 예를 들어 매핑이 누락 되었는지 확인 해야 합니다. 뷰를 생성 한 다음 뷰의 유효성을 검사 하 고이 메타 데이터를 사용할 수 있도록 준비 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1009">When Entity Framework is used with an EDMX file that formally specifies the conceptual model, the storage schema, and the mapping, then the model loading stage only has to validate that the EDM is correct (for example, make sure that no mappings are missing), then generate the views, then validate the views and have this metadata ready for use.</span></span> <span data-ttu-id="718a2-1010">그런 다음 쿼리를 실행 하거나 새 데이터를 데이터 저장소에 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1010">Only then can a query be executed or new data be saved to the data store.</span></span>

<span data-ttu-id="718a2-1011">Code First 접근 방식은 매우 정교한 엔터티 데이터 모델 생성기입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1011">The Code First approach is, at its heart, a sophisticated Entity Data Model generator.</span></span> <span data-ttu-id="718a2-1012">Entity Framework는 제공 된 코드에서 EDM을 생성 해야 합니다. 모델에 관련 된 클래스를 분석 하 고 규칙을 적용 하 고 흐름 API를 통해 모델을 구성 하 여이를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1012">The Entity Framework has to produce an EDM from the provided code; it does so by analyzing the classes involved in the model, applying conventions and configuring the model via the Fluent API.</span></span> <span data-ttu-id="718a2-1013">EDM이 빌드된 후에는 기본적으로 프로젝트에 EDMX 파일이 있는 것과 동일한 방식으로 Entity Framework 동작 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1013">After the EDM is built, the Entity Framework essentially behaves the same way as it would had an EDMX file been present in the project.</span></span> <span data-ttu-id="718a2-1014">따라서 Code First에서 모델을 빌드하면 EDMX가 있는 것과 비교 하 여 Entity Framework에 대 한 시작 시간을 더 느리게 하는 복잡성이 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1014">Thus, building the model from Code First adds extra complexity that translates into a slower startup time for the Entity Framework when compared to having an EDMX.</span></span> <span data-ttu-id="718a2-1015">비용은 빌드 중인 모델의 크기 및 복잡성에 따라 완전히 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1015">The cost is completely dependent on the size and complexity of the model that’s being built.</span></span>

<span data-ttu-id="718a2-1016">EDMX와 Code First를 사용 하도록 선택 하는 경우 Code First에서 도입 된 유연성으로 인해 처음으로 모델을 작성 하는 비용이 늘어납니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1016">When choosing to use EDMX versus Code First, it’s important to know that the flexibility introduced by Code First increases the cost of building the model for the first time.</span></span> <span data-ttu-id="718a2-1017">응용 프로그램에서이 최초 로드의 비용을 견딜 수 있는 경우 일반적으로 Code First 기본 설정 된 방법이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1017">If your application can withstand the cost of this first-time load then typically Code First will be the preferred way to go.</span></span>

## <a name="10-investigating-performance"></a><span data-ttu-id="718a2-1018">10 성능 조사</span><span class="sxs-lookup"><span data-stu-id="718a2-1018">10 Investigating Performance</span></span>

### <a name="101-using-the-visual-studio-profiler"></a><span data-ttu-id="718a2-1019">10.1 Visual Studio Profiler 사용</span><span class="sxs-lookup"><span data-stu-id="718a2-1019">10.1 Using the Visual Studio Profiler</span></span>

<span data-ttu-id="718a2-1020">Entity Framework에서 성능 문제가 발생 하는 경우 Visual Studio에 기본 제공 된 것과 같은 프로파일러를 사용 하 여 응용 프로그램이 시간을 소비 하는 위치를 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1020">If you are having performance issues with the Entity Framework, you can use a profiler like the one built into Visual Studio to see where your application is spending its time.</span></span> <span data-ttu-id="718a2-1021">"ADO.NET Entity Framework-1 부의 성능 알아보기" 블로그 게시물에서 원형 차트를 생성 하는 데에서는 도구입니다 ( \<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) Entity Framework 동작 콜드 및 웜 쿼리 중의 시간을 소모 하는 위치를 보여 주는 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1021">This is the tool we used to generate the pie charts in the “Exploring the Performance of the ADO.NET Entity Framework - Part 1” blog post ( \<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) that show where Entity Framework spends its time during cold and warm queries.</span></span>

<span data-ttu-id="718a2-1022">데이터 및 모델링 고객 자문 팀에서 작성 한 "Visual Studio 2010 Profiler를 사용 하 여 프로 파일링 Entity Framework" 블로그 게시물에서는 프로파일러를 사용 하 여 성능 문제를 조사 하는 방법에 대 한 실제 예제를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1022">The "Profiling Entity Framework using the Visual Studio 2010 Profiler" blog post written by the Data and Modeling Customer Advisory Team shows a real-world example of how they used the profiler to investigate a performance problem.</span></span><span data-ttu-id="718a2-1023">  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>.</span><span class="sxs-lookup"><span data-stu-id="718a2-1023">  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>.</span></span> <span data-ttu-id="718a2-1024">이 게시물은 windows 응용 프로그램용으로 작성 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1024">This post was written for a windows application.</span></span> <span data-ttu-id="718a2-1025">웹 응용 프로그램을 프로 파일링 해야 하는 경우 Visual Studio에서 작업 하는 것 보다 Windows 성능 레코더 (WPR) 및 Windows 성능 분석기 (WPA) 도구를 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1025">If you need to profile a web application the Windows Performance Recorder (WPR) and Windows Performance Analyzer (WPA) tools may work better than working from Visual Studio.</span></span> <span data-ttu-id="718a2-1026">WPR 및 WPA는 Windows 성능 도구 키트는 Windows 평가 및 배포 키트에 포함 된 부분 ( [http://www.microsoft.com/download/details.aspx?id=39982](https://www.microsoft.com/download/details.aspx?id=39982)).</span><span class="sxs-lookup"><span data-stu-id="718a2-1026">WPR and WPA are part of the Windows Performance Toolkit which is included with the Windows Assessment and Deployment Kit ( [http://www.microsoft.com/download/details.aspx?id=39982](https://www.microsoft.com/download/details.aspx?id=39982)).</span></span>

### <a name="102-applicationdatabase-profiling"></a><span data-ttu-id="718a2-1027">10.2 응용 프로그램/데이터베이스 프로 파일링</span><span class="sxs-lookup"><span data-stu-id="718a2-1027">10.2 Application/Database profiling</span></span>

<span data-ttu-id="718a2-1028">Visual Studio에 기본 제공 되는 프로파일러와 같은 도구를 통해 응용 프로그램에서 시간이 소요 되는 위치를 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1028">Tools like the profiler built into Visual Studio tell you where your application is spending time.</span></span><span data-ttu-id="718a2-1029">  실행 중인 응용 프로그램에 대 한 동적 분석을 수행 하는 또 다른 유형의 프로파일러를 사용할 수 있으며,이는 요구 사항에 따라 프로덕션 또는 사전 프로덕션 환경에서 실행 되는 응용 프로그램을 확인 하 고 일반적인 문제 및 데이터베이스 액세스의 패턴</span><span class="sxs-lookup"><span data-stu-id="718a2-1029">  Another type of profiler is available that performs dynamic analysis of your running application, either in production or pre-production depending on needs, and looks for common pitfalls and anti-patterns of database access.</span></span>

<span data-ttu-id="718a2-1030">상업적으로 사용할 수 있는 두 가지 프로파일러는 Entity Framework Profiler ( \<http://efprof.com>) ORMProfiler 및 ( \<http://ormprofiler.com>) 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1030">Two commercially available profilers are the Entity Framework Profiler ( \<http://efprof.com>) and ORMProfiler ( \<http://ormprofiler.com>).</span></span>

<span data-ttu-id="718a2-1031">응용 프로그램이 Code First를 사용 하는 MVC 응용 프로그램 인 경우 StackExchange의 미니 프로파일러를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1031">If your application is an MVC application using Code First, you can use StackExchange's MiniProfiler.</span></span> <span data-ttu-id="718a2-1032">Scott Hanselman 블로그가이 도구 설명: \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx> 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1032">Scott Hanselman describes this tool in his blog at: \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.</span></span>

<span data-ttu-id="718a2-1033">응용 프로그램의 데이터베이스 작업을 프로 파일링 하는 방법에 대 한 자세한 내용은 [Entity Framework의 데이터베이스 작업 프로 파일링](https://msdn.microsoft.com/magazine/gg490349.aspx)이라는 Julie LERMAN의 MSDN Magazine 문서를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="718a2-1033">For more information on profiling your application's database activity, see Julie Lerman's MSDN Magazine article titled [Profiling Database Activity in the Entity Framework](https://msdn.microsoft.com/magazine/gg490349.aspx).</span></span>

### <a name="103-database-logger"></a><span data-ttu-id="718a2-1034">10.3 데이터베이스로 거</span><span class="sxs-lookup"><span data-stu-id="718a2-1034">10.3 Database logger</span></span>

<span data-ttu-id="718a2-1035">Entity Framework 6을 사용 하는 경우 기본 제공 로깅 기능을 사용 하는 것도 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1035">If you are using Entity Framework 6 also consider using the built-in logging functionality.</span></span> <span data-ttu-id="718a2-1036">간단한 한 줄 구성을 통해 해당 작업을 기록 하도록 컨텍스트의 데이터베이스 속성에 지시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1036">The Database property of the context can be instructed to log its activity via a simple one-line configuration:</span></span>

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

<span data-ttu-id="718a2-1037">이 예에서 데이터베이스 작업은 콘솔에 기록 되지만 Log 속성은 모든 Action @ no__t-0string @ no__t-1 대리자를 호출 하도록 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1037">In this example the database activity will be logged to the console, but the Log property can be configured to call any Action&lt;string&gt; delegate.</span></span>

<span data-ttu-id="718a2-1038">다시 컴파일하지 않고 데이터베이스 로깅을 사용 하도록 설정 하 고 Entity Framework 6.1 이상을 사용 하려는 경우 응용 프로그램의 web.config 또는 app.config 파일에 인터셉터를 추가 하 여이 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1038">If you want to enable database logging without recompiling, and you are using Entity Framework 6.1 or later, you can do so by adding an interceptor in the web.config or app.config file of your application.</span></span>

``` xml
  <interceptors>
    <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
      <parameters>
        <parameter value="C:\Path\To\My\LogOutput.txt"/>
      </parameters>
    </interceptor>
  </interceptors>
```

<span data-ttu-id="718a2-1039">이동 다시 컴파일하지 않고도 로깅을 추가 하는 방법에 대 한 자세한 내용은 \<http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/> 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1039">For more information on how to add logging without recompiling go to \<http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.</span></span>

## <a name="11-appendix"></a><span data-ttu-id="718a2-1040">11 부록</span><span class="sxs-lookup"><span data-stu-id="718a2-1040">11 Appendix</span></span>

### <a name="111-a-test-environment"></a><span data-ttu-id="718a2-1041">11.1 A. 테스트 환경</span><span class="sxs-lookup"><span data-stu-id="718a2-1041">11.1 A. Test Environment</span></span>

<span data-ttu-id="718a2-1042">이 환경에서는 클라이언트 응용 프로그램을 사용 하 여 별도의 컴퓨터에 있는 2 컴퓨터 설치를 데이터베이스에 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1042">This environment uses a 2-machine setup with the database on a separate machine from the client application.</span></span> <span data-ttu-id="718a2-1043">컴퓨터가 동일한 랙에 있으므로 네트워크 대기 시간은 상대적으로 낮지만 단일 컴퓨터 환경 보다 더 현실적인 것입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1043">Machines are in the same rack, so network latency is relatively low, but more realistic than a single-machine environment.</span></span>

#### <a name="1111-app-server"></a><span data-ttu-id="718a2-1044">11.1.1 App 서버</span><span class="sxs-lookup"><span data-stu-id="718a2-1044">11.1.1       App Server</span></span>

##### <a name="11111-software-environment"></a><span data-ttu-id="718a2-1045">11.1.1.1 소프트웨어 환경</span><span class="sxs-lookup"><span data-stu-id="718a2-1045">11.1.1.1      Software Environment</span></span>

-   <span data-ttu-id="718a2-1046">Entity Framework 4 소프트웨어 환경</span><span class="sxs-lookup"><span data-stu-id="718a2-1046">Entity Framework 4 Software Environment</span></span>
    -   <span data-ttu-id="718a2-1047">OS 이름: Windows Server 2008 R2 Enterprise SP1</span><span class="sxs-lookup"><span data-stu-id="718a2-1047">OS Name: Windows Server 2008 R2 Enterprise SP1.</span></span>
    -   <span data-ttu-id="718a2-1048">Visual Studio 2010 – Ultimate.</span><span class="sxs-lookup"><span data-stu-id="718a2-1048">Visual Studio 2010 – Ultimate.</span></span>
    -   <span data-ttu-id="718a2-1049">Visual Studio 2010 SP1 (일부 비교에만 해당)</span><span class="sxs-lookup"><span data-stu-id="718a2-1049">Visual Studio 2010 SP1 (only for some comparisons).</span></span>
-   <span data-ttu-id="718a2-1050">Entity Framework 5 및 6 소프트웨어 환경</span><span class="sxs-lookup"><span data-stu-id="718a2-1050">Entity Framework 5 and 6 Software Environment</span></span>
    -   <span data-ttu-id="718a2-1051">OS 이름: Windows 8.1 Enterprise</span><span class="sxs-lookup"><span data-stu-id="718a2-1051">OS Name: Windows 8.1 Enterprise</span></span>
    -   <span data-ttu-id="718a2-1052">Visual Studio 2013 – Ultimate.</span><span class="sxs-lookup"><span data-stu-id="718a2-1052">Visual Studio 2013 – Ultimate.</span></span>

##### <a name="11112-hardware-environment"></a><span data-ttu-id="718a2-1053">11.1.1.2 하드웨어 환경</span><span class="sxs-lookup"><span data-stu-id="718a2-1053">11.1.1.2      Hardware Environment</span></span>

-   <span data-ttu-id="718a2-1054">이중 프로세서:     Intel (R) Xeon (R) CPU L5520 W3530 @ 2.27 GHz, 2261 Mhz8 GHz, 4 개 코어, 84 개의 논리적 프로세서.</span><span class="sxs-lookup"><span data-stu-id="718a2-1054">Dual Processor:     Intel(R) Xeon(R) CPU L5520 W3530 @ 2.27GHz, 2261 Mhz8 GHz, 4 Core(s), 84 Logical Processor(s).</span></span>
-   <span data-ttu-id="718a2-1055">2412 GB의 Ram</span><span class="sxs-lookup"><span data-stu-id="718a2-1055">2412 GB RamRAM.</span></span>
-   <span data-ttu-id="718a2-1056">136 GB SCSI250GB SATA 7200 rpm/s 드라이브를 4 개의 파티션으로 분할 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1056">136 GB SCSI250GB SATA 7200 rpm 3GB/s drive split into 4 partitions.</span></span>

#### <a name="1112-db-server"></a><span data-ttu-id="718a2-1057">11.1.2 DB 서버</span><span class="sxs-lookup"><span data-stu-id="718a2-1057">11.1.2       DB server</span></span>

##### <a name="11121-software-environment"></a><span data-ttu-id="718a2-1058">11.1.2.1 소프트웨어 환경</span><span class="sxs-lookup"><span data-stu-id="718a2-1058">11.1.2.1      Software Environment</span></span>

-   <span data-ttu-id="718a2-1059">OS 이름: Windows Server 2008 R 28.1 Enterprise SP1</span><span class="sxs-lookup"><span data-stu-id="718a2-1059">OS Name: Windows Server 2008 R28.1 Enterprise SP1.</span></span>
-   <span data-ttu-id="718a2-1060">SQL Server 2008 R22012.</span><span class="sxs-lookup"><span data-stu-id="718a2-1060">SQL Server 2008 R22012.</span></span>

##### <a name="11122-hardware-environment"></a><span data-ttu-id="718a2-1061">11.1.2.2 하드웨어 환경</span><span class="sxs-lookup"><span data-stu-id="718a2-1061">11.1.2.2      Hardware Environment</span></span>

-   <span data-ttu-id="718a2-1062">단일 프로세서: Intel (R) Xeon (R) CPU L5520 @ 2.27 GHz, 2261 MhzES-1620 0 @ 3.60 GHz, 4 개 코어, 8 개의 논리적 프로세서.</span><span class="sxs-lookup"><span data-stu-id="718a2-1062">Single Processor: Intel(R) Xeon(R) CPU L5520  @ 2.27GHz, 2261 MhzES-1620 0 @ 3.60GHz, 4 Core(s), 8 Logical Processor(s).</span></span>
-   <span data-ttu-id="718a2-1063">824 GB의 Ram</span><span class="sxs-lookup"><span data-stu-id="718a2-1063">824 GB RamRAM.</span></span>
-   <span data-ttu-id="718a2-1064">465 GB ATA500GB SATA 7200 rpm 6GB/s 드라이브를 4 개의 파티션으로 분할 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1064">465 GB ATA500GB SATA 7200 rpm 6GB/s drive split into 4 partitions.</span></span>

### <a name="112-b-query-performance-comparison-tests"></a><span data-ttu-id="718a2-1065">11.2 B. 쿼리 성능 비교 테스트</span><span class="sxs-lookup"><span data-stu-id="718a2-1065">11.2      B. Query performance comparison tests</span></span>

<span data-ttu-id="718a2-1066">Northwind 모델은 이러한 테스트를 실행 하는 데 사용 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1066">The Northwind model was used to execute these tests.</span></span> <span data-ttu-id="718a2-1067">Entity Framework 디자이너를 사용 하 여 데이터베이스에서 생성 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1067">It was generated from the database using the Entity Framework designer.</span></span> <span data-ttu-id="718a2-1068">그런 다음, 다음 코드를 사용 하 여 쿼리 실행 옵션의 성능을 비교 합니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1068">Then, the following code was used to compare the performance of the query execution options:</span></span>

``` csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Common;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Data.Objects;
using System.Linq;

namespace QueryComparison
{
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
                );

        public IQueryable<Product> InvokeProductsForCategoryCQ(string categoryName)
        {
            return productsForCategoryCQ(this, categoryName);
        }
    }

    public class QueryTypePerfComparison
    {
        private static string entityConnectionStr = @"metadata=res://*/Northwind.csdl|res://*/Northwind.ssdl|res://*/Northwind.msl;provider=System.Data.SqlClient;provider connection string='data source=.;initial catalog=Northwind;integrated security=True;multipleactiveresultsets=True;App=EntityFramework'";

        public void LINQIncludingContextCreation()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTracking()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                context.Products.MergeOption = MergeOption.NoTracking;

                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void CompiledQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                var q = context.InvokeProductsForCategoryCQ("Beverages");
                q.ToList();
            }
        }

        public void ObjectQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
                products.ToList();
            }
        }

        public void EntityCommand()
        {
            using (EntityConnection eConn = new EntityConnection(entityConnectionStr))
            {
                eConn.Open();
                EntityCommand cmd = eConn.CreateCommand();
                cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

                using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
                {
                    List<Product> productsList = new List<Product>();
                    while (reader.Read())
                    {
                        DbDataRecord record = (DbDataRecord)reader.GetValue(0);

                        // 'materialize' the product by accessing each field and value. Because we are materializing products, we won't have any nested data readers or records.
                        int fieldCount = record.FieldCount;

                        // Treat all products as Product, even if they are the subtype DiscontinuedProduct.
                        Product product = new Product();  

                        product.ProductID = record.GetInt32(0);
                        product.ProductName = record.GetString(1);
                        product.SupplierID = record.GetInt32(2);
                        product.CategoryID = record.GetInt32(3);
                        product.QuantityPerUnit = record.GetString(4);
                        product.UnitPrice = record.GetDecimal(5);
                        product.UnitsInStock = record.GetInt16(6);
                        product.UnitsOnOrder = record.GetInt16(7);
                        product.ReorderLevel = record.GetInt16(8);
                        product.Discontinued = record.GetBoolean(9);

                        productsList.Add(product);
                    }
                }
            }
        }

        public void ExecuteStoreQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectResult<Product> beverages = context.ExecuteStoreQuery<Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Database.SqlQuery\<QueryComparison.DbC.Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbSet()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Products.SqlQuery(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void LINQIncludingContextCreationDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTrackingDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var q = context.Products.AsNoTracking().Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }
    }
}
```

### <a name="113-c-navision-model"></a><span data-ttu-id="718a2-1069">11.3 Navision 모델</span><span class="sxs-lookup"><span data-stu-id="718a2-1069">11.3 C. Navision Model</span></span>

<span data-ttu-id="718a2-1070">Navision 데이터베이스는 Microsoft Dynamics – NAV를 시연 하는 데 사용 되는 많은 데이터베이스입니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1070">The Navision database is a large database used to demo Microsoft Dynamics – NAV.</span></span> <span data-ttu-id="718a2-1071">생성 된 개념적 모델에는 1005 엔터티 집합과 4227 연결 집합이 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1071">The generated conceptual model contains 1005 entity sets and 4227 association sets.</span></span> <span data-ttu-id="718a2-1072">테스트에 사용 되는 모델은 "flat" 이며 상속을 추가 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1072">The model used in the test is “flat” – no inheritance has been added to it.</span></span>

#### <a name="1131-queries-used-for-navision-tests"></a><span data-ttu-id="718a2-1073">Navision 테스트에 사용 되는 11.3.1 쿼리</span><span class="sxs-lookup"><span data-stu-id="718a2-1073">11.3.1 Queries used for Navision tests</span></span>

<span data-ttu-id="718a2-1074">Navision 모델에 사용 되는 쿼리 목록에는 다음과 같은 3 가지 범주의 Entity SQL 쿼리가 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1074">The queries list used with the Navision model contains 3 categories of Entity SQL queries:</span></span>

##### <a name="11311-lookup"></a><span data-ttu-id="718a2-1075">11.3.1.1 조회</span><span class="sxs-lookup"><span data-stu-id="718a2-1075">11.3.1.1 Lookup</span></span>

<span data-ttu-id="718a2-1076">집계가 없는 간단한 조회 쿼리</span><span class="sxs-lookup"><span data-stu-id="718a2-1076">A simple lookup query with no aggregations</span></span>

-   <span data-ttu-id="718a2-1077">개수: 16232</span><span class="sxs-lookup"><span data-stu-id="718a2-1077">Count: 16232</span></span>
-   <span data-ttu-id="718a2-1078">예:</span><span class="sxs-lookup"><span data-stu-id="718a2-1078">Example:</span></span>

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312singleaggregating"></a><span data-ttu-id="718a2-1079">11.3.1.2 SingleAggregating</span><span class="sxs-lookup"><span data-stu-id="718a2-1079">11.3.1.2 SingleAggregating</span></span>

<span data-ttu-id="718a2-1080">여러 집계가 있지만 부분합이 없는 일반 BI 쿼리 (단일 쿼리)</span><span class="sxs-lookup"><span data-stu-id="718a2-1080">A normal BI query with multiple aggregations, but no subtotals (single query)</span></span>

-   <span data-ttu-id="718a2-1081">개수: 2313</span><span class="sxs-lookup"><span data-stu-id="718a2-1081">Count: 2313</span></span>
-   <span data-ttu-id="718a2-1082">예:</span><span class="sxs-lookup"><span data-stu-id="718a2-1082">Example:</span></span>

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

<span data-ttu-id="718a2-1083">여기서 MDF @ no__t-0SessionLogin @ no__t-1Time @ no__t-2Max ()는 모델에서 다음과 같이 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="718a2-1083">Where MDF\_SessionLogin\_Time\_Max() is defined in the model as:</span></span>

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313aggregatingsubtotals"></a><span data-ttu-id="718a2-1084">11.3.1.3 AggregatingSubtotals</span><span class="sxs-lookup"><span data-stu-id="718a2-1084">11.3.1.3 AggregatingSubtotals</span></span>

<span data-ttu-id="718a2-1085">집계와 부분합이 있는 BI 쿼리 (union all을 통해)</span><span class="sxs-lookup"><span data-stu-id="718a2-1085">A BI query with aggregations and subtotals (via union all)</span></span>

-   <span data-ttu-id="718a2-1086">개수: 178</span><span class="sxs-lookup"><span data-stu-id="718a2-1086">Count: 178</span></span>
-   <span data-ttu-id="718a2-1087">예:</span><span class="sxs-lookup"><span data-stu-id="718a2-1087">Example:</span></span>

``` xml
  <Query complexity="AggregatingSubtotals">
    <CommandText>
using NavisionFK;
function AmountConsumed(entities Collection([CRONUS_International_Ltd__Zone])) as
(
    Edm.Sum(select value N.Block_Movement FROM entities as E, E.CRONUS_International_Ltd__Bin as N)
)
function AmountConsumed(P1 Edm.Int32) as
(
    AmountConsumed(select value e from NavisionFKContext.CRONUS_International_Ltd__Zone as e where e.Zone_Ranking = P1)
)
----------------------------------------------------------------------------------------------------------------------
(
    select top(10) Zone_Ranking, Cross_Dock_Bin_Zone, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking, E.Cross_Dock_Bin_Zone
)
union all
(
    select top(10) Zone_Ranking, Cast(null as Edm.Byte) as P2, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking
)
union all
{
    Row(Cast(null as Edm.Int32) as P1, Cast(null as Edm.Byte) as P2, AmountConsumed(select value E
                                                                         from NavisionFKContext.CRONUS_International_Ltd__Zone as E
                                                                         where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed))
}</CommandText>
    <Parameters>
      <Parameter Name="MinAmountConsumed" DbType="Int32" Value="10000" />
    </Parameters>
  </Query>
```
