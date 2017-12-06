---
title: "작업-EF 코어를 쿼리 하는 방법"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 7fd2940d559f82016d7a8fc3fdcf3af0d5b8bc8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="how-queries-work"></a><span data-ttu-id="9ae33-102">쿼리 작업</span><span class="sxs-lookup"><span data-stu-id="9ae33-102">How Queries Work</span></span>

<span data-ttu-id="9ae33-103">Entity Framework Core 데이터베이스에서 데이터를 쿼리 언어 통합 쿼리 (LINQ)를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="9ae33-103">Entity Framework Core uses Language Integrate Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="9ae33-104">LINQ를 사용 하면 C# (또는)를 사용 하면.NET 언어를 선택 파생된 컨텍스트 및 엔터티 클래스에 기반 하는 강력한 형식의 쿼리를 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9ae33-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="9ae33-105">쿼리의 수명</span><span class="sxs-lookup"><span data-stu-id="9ae33-105">The life of a query</span></span>

<span data-ttu-id="9ae33-106">다음은 각 쿼리를 통해 이동 하는 프로세스의 높은 수준의 개요입니다.</span><span class="sxs-lookup"><span data-stu-id="9ae33-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="9ae33-107">Entity Framework Core 데이터베이스 공급자가 처리할 준비가 되 표현 작성 하 여 LINQ 쿼리가 처리</span><span class="sxs-lookup"><span data-stu-id="9ae33-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="9ae33-108">이러한 처리는 완료 하 여 쿼리가 실행 될 때마다 하지 않아도 되도록 결과 캐시 합니다.</span><span class="sxs-lookup"><span data-stu-id="9ae33-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="9ae33-109">결과 데이터베이스 공급자에 전달 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9ae33-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="9ae33-110">데이터베이스 공급자를 식별 하는 데이터베이스에서 쿼리 부분을 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9ae33-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="9ae33-111">이러한 쿼리 부분에는 데이터베이스 특정 쿼리 언어 (예: 관계형 데이터베이스에 대 한 SQL)으로 변환</span><span class="sxs-lookup"><span data-stu-id="9ae33-111">These parts of the query are translated to database specific query language (e.g. SQL for a relational database)</span></span>
   3. <span data-ttu-id="9ae33-112">하나 이상의 쿼리 결과 반환 된 집합 및 데이터베이스에 전송 됩니다 (결과가 하지 엔터티 인스턴스 데이터베이스의 값)</span><span class="sxs-lookup"><span data-stu-id="9ae33-112">One or more queries are sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="9ae33-113">결과 집합의 각 항목에 대 한</span><span class="sxs-lookup"><span data-stu-id="9ae33-113">For each item in the result set</span></span>
   1. <span data-ttu-id="9ae33-114">추적 쿼리 경우 EF 데이터가 컨텍스트 인스턴스에 대 한 변경 추적 장치에 이미 있는 엔터티를 나타내는 경우를 확인합니다</span><span class="sxs-lookup"><span data-stu-id="9ae33-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="9ae33-115">기존 엔터티가 반환 될 경우</span><span class="sxs-lookup"><span data-stu-id="9ae33-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="9ae33-116">그렇지 않은 경우 새 엔터티, 설치 프로그램은 변경 내용 추적 및 새 엔터티가 반환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9ae33-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="9ae33-117">No 추적 쿼리 경우 EF 데이터가이 쿼리 한 결과 집합에 이미 있는 엔터티를 나타내는 경우를 확인합니다</span><span class="sxs-lookup"><span data-stu-id="9ae33-117">If this is a no-tracking query, EF checks if the data represents an entity already in the result set for this query</span></span>
      * <span data-ttu-id="9ae33-118">따라서 기존 엔터티가 반환 되 면 <sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="9ae33-118">If so, the existing entity is returned <sup>(1)</sup></span></span>
      * <span data-ttu-id="9ae33-119">그렇지 않으면 새 엔터티가 만들어져 반환</span><span class="sxs-lookup"><span data-stu-id="9ae33-119">If not, a new entity is created and returned</span></span>

<span data-ttu-id="9ae33-120"><sup>(1) </sup> 이미 반환 된 엔터티를 추적 하기 위해 약한 참조를 사용 하는 추적 쿼리가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="9ae33-120"><sup>(1)</sup> No tracking queries use weak references to keep track of entities that have already been returned.</span></span> <span data-ttu-id="9ae33-121">동일한 id 가진 이전 결과 범위를 벗어나거나 가비지 수집을 실행 하는 경우 새 엔터티 인스턴스를 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9ae33-121">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="9ae33-122">쿼리를 실행 하는 경우</span><span class="sxs-lookup"><span data-stu-id="9ae33-122">When queries are executed</span></span>

<span data-ttu-id="9ae33-123">LINQ 연산자를 호출 하는 경우 쿼리는 메모리 내 표현을를 작성 하는 단순히 합니다.</span><span class="sxs-lookup"><span data-stu-id="9ae33-123">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="9ae33-124">쿼리는 결과 사용 하는 경우에 데이터베이스에 전송 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9ae33-124">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="9ae33-125">데이터베이스에 전송 되 고 쿼리 하는 가장 일반적인 작업은 같습니다.</span><span class="sxs-lookup"><span data-stu-id="9ae33-125">The most common operations that result in the query being sent to the database are:</span></span>
* <span data-ttu-id="9ae33-126">결과를 반복 하는 `for` 루프</span><span class="sxs-lookup"><span data-stu-id="9ae33-126">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="9ae33-127">와 같은 연산자를 사용 하 여 `ToList`, `ToArray`, `Single`,`Count`</span><span class="sxs-lookup"><span data-stu-id="9ae33-127">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`</span></span>
* <span data-ttu-id="9ae33-128">데이터 바인딩 UI에 대 한 쿼리 결과</span><span class="sxs-lookup"><span data-stu-id="9ae33-128">Databinding the results of a query to a UI</span></span>

> [!WARNING]  
> <span data-ttu-id="9ae33-129">**항상 사용자 입력 유효성 검사:** 동안 EF SQL 인젝션 공격 으로부터 보호 기능은, 입력의 모든 일반적인 유효성 검사는 수행 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="9ae33-129">**Always validate user input:** While EF does provide protection from SQL injection attacks, it does not do any general validation of input.</span></span> <span data-ttu-id="9ae33-130">따라서 엔터티 속성 등을 할당 하는 LINQ 쿼리에서 사용 되는 Api에 전달 되 고 값은 신뢰할 수 없는 소스 다음 응용 프로그램 요구 사항에 따라 적절 한 유효성 검사에서 제공 하는 경우 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9ae33-130">Therefore if values being passed to APIs, used in LINQ queries, assigned to entity properties, etc., come from an untrusted source then appropriate validation, per your application requirements, should be performed.</span></span> <span data-ttu-id="9ae33-131">동적으로 쿼리를 작성 하는 데 사용 하는 사용자 입력을 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9ae33-131">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="9ae33-132">의도 된 식만 보다 되도록 해야 하는 식을 작성 하기 위해 구성 될 수는 사용자 입력을 수락 하는 경우 LINQ를 사용 하 여 경우에.</span><span class="sxs-lookup"><span data-stu-id="9ae33-132">Even when using LINQ, if you are accepting user input to build expressions you need to make sure than only intended expressions can be constructed.</span></span>
