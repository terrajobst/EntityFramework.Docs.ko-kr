---
title: 로그 공급자에 영향을 주는 변경 내용-EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: ee73940e3c0030b76e73438b1852cc29ebeadb45
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998342"
---
# <a name="provider-impacting-changes"></a><span data-ttu-id="9e7bb-102">공급자에 영향을 주는 변경 내용</span><span class="sxs-lookup"><span data-stu-id="9e7bb-102">Provider-impacting changes</span></span>

<span data-ttu-id="9e7bb-103">이 페이지에는 끌어오기 요청 react를 다른 데이터베이스 공급자의 작성자가 필요할 수 있는 EF Core 리포지토리에 대 한 링크가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9e7bb-103">This page contains links to pull requests made on the EF Core repo that may require authors of other database providers to react.</span></span> <span data-ttu-id="9e7bb-104">의도 공급자의 새 버전으로 업데이트할 때 기존 타사 데이터베이스 공급자의 작성자를 위한 시작점을 제공 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="9e7bb-104">The intention is to provide a starting point for authors of existing third-party database providers when updating their provider to a new version.</span></span>

<span data-ttu-id="9e7bb-105">이 로그 2.1에서 2.2로 변경 내용과 시작 했습니다.</span><span class="sxs-lookup"><span data-stu-id="9e7bb-105">We are starting this log with changes from 2.1 to 2.2.</span></span> <span data-ttu-id="9e7bb-106">2.1 이전 버전에서는 사용 된 [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) 및 [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) 당사의 문제 및 끌어오기 요청에는 레이블.</span><span class="sxs-lookup"><span data-stu-id="9e7bb-106">Prior to 2.1 we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our issues and pull requests.</span></span>

### <a name="21-----22"></a><span data-ttu-id="9e7bb-107">2.1 2.2---></span><span class="sxs-lookup"><span data-stu-id="9e7bb-107">2.1 ---> 2.2</span></span>

#### <a name="test-only-changes"></a><span data-ttu-id="9e7bb-108">테스트 전용 변경 내용</span><span class="sxs-lookup"><span data-stu-id="9e7bb-108">Test-only changes</span></span>

* <span data-ttu-id="9e7bb-109">https://github.com/aspnet/EntityFrameworkCore/pull/12057 -테스트의 사용자 지정 가능한 SQL 구분 기호를 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="9e7bb-109">https://github.com/aspnet/EntityFrameworkCore/pull/12057 - Allow customizable SQL delimeters in tests</span></span>
  * <span data-ttu-id="9e7bb-110">엄격한 비 부동 지점 비교 BuiltInDataTypesTestBase 허용 하는 변경 내용을 테스트합니다</span><span class="sxs-lookup"><span data-stu-id="9e7bb-110">Test changes that allow non-strict floating point comparisons in BuiltInDataTypesTestBase</span></span>
  * <span data-ttu-id="9e7bb-111">쿼리 테스트를 다른 SQL 구분 기호를 사용 하 여 다시 사용할 수 있도록 테스트 변경</span><span class="sxs-lookup"><span data-stu-id="9e7bb-111">Test changes that allow query tests to be re-used with different SQL delimeters</span></span>
* <span data-ttu-id="9e7bb-112">https://github.com/aspnet/EntityFrameworkCore/pull/12072 -관계형 사양 테스트 DbFunction 테스트를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="9e7bb-112">https://github.com/aspnet/EntityFrameworkCore/pull/12072 - Add DbFunction tests to the relational specification tests</span></span>
  * <span data-ttu-id="9e7bb-113">모든 데이터베이스 공급자에 대해 이러한 테스트를 실행할 수 있도록</span><span class="sxs-lookup"><span data-stu-id="9e7bb-113">Such that these tests can be run against all database providers</span></span>
* <span data-ttu-id="9e7bb-114">https://github.com/aspnet/EntityFrameworkCore/pull/12362 비동기 테스트 정리</span><span class="sxs-lookup"><span data-stu-id="9e7bb-114">https://github.com/aspnet/EntityFrameworkCore/pull/12362 - Async test cleanup</span></span>
  * <span data-ttu-id="9e7bb-115">제거 `Wait` 호출 비동기, 불필요 한 및 일부 테스트 메서드 이름을 바꿀</span><span class="sxs-lookup"><span data-stu-id="9e7bb-115">Remove `Wait` calls, unneeded async, and renamed some test methods</span></span>
* <span data-ttu-id="9e7bb-116">https://github.com/aspnet/EntityFrameworkCore/pull/12666 --로깅 테스트 인프라를 통합 하는 중</span><span class="sxs-lookup"><span data-stu-id="9e7bb-116">https://github.com/aspnet/EntityFrameworkCore/pull/12666 - Unify logging test infrastructure</span></span>
  * <span data-ttu-id="9e7bb-117">추가 `CreateListLoggerFactory` react에 이러한 테스트를 사용 하 여 공급자를 해야 하는 일부 이전 로깅 인프라를 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="9e7bb-117">Added `CreateListLoggerFactory` and removed some previous logging infrastructure, which will require providers using these tests to react</span></span>
* <span data-ttu-id="9e7bb-118">https://github.com/aspnet/EntityFrameworkCore/pull/12500 -동기적 및 비동기적으로 더 많은 쿼리 테스트 실행</span><span class="sxs-lookup"><span data-stu-id="9e7bb-118">https://github.com/aspnet/EntityFrameworkCore/pull/12500 - Run more query tests both synchronously and asynchronously</span></span>
  * <span data-ttu-id="9e7bb-119">테스트 이름 및 팩터링을 변경 된 이러한 테스트를 사용 하 여 대응할 수 공급자 필요</span><span class="sxs-lookup"><span data-stu-id="9e7bb-119">Test names and factoring has changed, which will require providers using these tests to react</span></span>
* <span data-ttu-id="9e7bb-120">https://github.com/aspnet/EntityFrameworkCore/pull/12766 이름 바꾸기 ComplexNavigations 모델 탐색</span><span class="sxs-lookup"><span data-stu-id="9e7bb-120">https://github.com/aspnet/EntityFrameworkCore/pull/12766 - Renaming navigations in the ComplexNavigations model</span></span>
  * <span data-ttu-id="9e7bb-121">이러한 테스트를 사용 하 여 공급자 대응 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9e7bb-121">Providers using these tests may need to react</span></span>
* <span data-ttu-id="9e7bb-122">https://github.com/aspnet/EntityFrameworkCore/pull/12141 -기능 테스트를 삭제 하는 대신 풀에 컨텍스트를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="9e7bb-122">https://github.com/aspnet/EntityFrameworkCore/pull/12141 - Return the context to the pool instead of disposing in functional tests</span></span>
  * <span data-ttu-id="9e7bb-123">이 변경 리팩터링 반응 하는 공급자가 필요할 수 있는 몇 가지 테스트 포함</span><span class="sxs-lookup"><span data-stu-id="9e7bb-123">This change includes some test refactoring which may require providers to react</span></span>


#### <a name="test-and-product-code-changes"></a><span data-ttu-id="9e7bb-124">테스트 및 제품 코드 변경 내용</span><span class="sxs-lookup"><span data-stu-id="9e7bb-124">Test and product code changes</span></span>

* <span data-ttu-id="9e7bb-125">https://github.com/aspnet/EntityFrameworkCore/pull/12109 --RelationalTypeMapping.Clone 메서드를 통합 하는 중</span><span class="sxs-lookup"><span data-stu-id="9e7bb-125">https://github.com/aspnet/EntityFrameworkCore/pull/12109 - Consolidate RelationalTypeMapping.Clone methods</span></span>
  * <span data-ttu-id="9e7bb-126">파생된 클래스에서 간소화 하기 위해 허용 RelationalTypeMapping 2.1에서 변경 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9e7bb-126">Changes in 2.1 to the RelationalTypeMapping allowed for a simplification in derived classes.</span></span> <span data-ttu-id="9e7bb-127">에서는 실감이 잘 나 공급자에 중단 된이 있지만 공급자 활용이 변경의 파생된 형식에 클래스 매핑.</span><span class="sxs-lookup"><span data-stu-id="9e7bb-127">We don't believe this was breaking to providers, but providers can take advantage of this change in their derived type mapping classes.</span></span>
* <span data-ttu-id="9e7bb-128">https://github.com/aspnet/EntityFrameworkCore/pull/12069 -태그가 지정 된 쿼리나 명명 된 쿼리</span><span class="sxs-lookup"><span data-stu-id="9e7bb-128">https://github.com/aspnet/EntityFrameworkCore/pull/12069 - Tagged or named queries</span></span>
  * <span data-ttu-id="9e7bb-129">LINQ 쿼리에 태그를 지정 하 고 SQL 주석으로 표시 하는 이러한 태그에 대 한 인프라를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="9e7bb-129">Adds infrastructure for tagging LINQ queries and having those tags show up as comments in the SQL.</span></span> <span data-ttu-id="9e7bb-130">이 공급자 SQL 생성에 반응 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9e7bb-130">This may require providers to react in SQL generation.</span></span>
