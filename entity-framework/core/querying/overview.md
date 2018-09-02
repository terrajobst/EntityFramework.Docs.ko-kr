---
title: 쿼리 작동 방식 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/overview
ms.openlocfilehash: f1c23471bfbc998b2d4f9dc579d1404d6202e109
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993205"
---
# <a name="how-queries-work"></a><span data-ttu-id="e211c-102">쿼리 작동 방식</span><span class="sxs-lookup"><span data-stu-id="e211c-102">How Queries Work</span></span>

<span data-ttu-id="e211c-103">Entity Framework Core는 LINQ(Language-Integrated Query)를 사용하여 데이터베이스에서 데이터를 쿼리합니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="e211c-104">LINQ를 사용하면 C#(원하는 .NET 언어)을 사용하여 파생된 컨텍스트 및 엔터티 클래스를 기반으로 강력한 형식의 쿼리를 작성할 수 있습니다. </span><span class="sxs-lookup"><span data-stu-id="e211c-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="e211c-105">쿼리의 수명</span><span class="sxs-lookup"><span data-stu-id="e211c-105">The life of a query</span></span>

<span data-ttu-id="e211c-106">다음은 각 쿼리가 진행되는 프로세스에 대란 간략한 개요입니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="e211c-107">LINQ 쿼리는 Entity Framework Core에서 처리되어 데이터베이스 공급자가 처리할 수 있는 표현을 작성합니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="e211c-108">쿼리가 실행될 때마다 이 처리를 수행할 필요가 없도록 결과가 캐시됩니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="e211c-109">결과가 데이터베이스 공급자에 전달됩니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="e211c-110">데이터베이스 공급자가 데이터베이스에서 평가할 수 있는 쿼리 부분을 식별합니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="e211c-111">이러한 쿼리 부분이 데이터베이스별 쿼리 언어(예: 관계형 데이터베이스의 경우 SQL)로 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-111">These parts of the query are translated to database specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="e211c-112">하나 이상의 쿼리가 데이터베이스로 전송되고 결과 집합이 반환됩니다. 결과는 엔터티 인스턴스가 아닌 데이터베이스의 값입니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-112">One or more queries are sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="e211c-113">결과 집합의 각 항목에 대해 다음을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-113">For each item in the result set</span></span>
   1. <span data-ttu-id="e211c-114">추적 쿼리인 경우 EF는 데이터가 컨텍스트 인스턴스에 대한 변경 추적 장치에 이미 있는 엔터티를 나타내는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="e211c-115">그럴 경우 기존 엔터티가 반환됩니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="e211c-116">그렇지 않을 경우 새 엔터티가 만들어지고 변경 내용 추적이 설정되며 새 엔터티가 반환됩니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="e211c-117">비 추적 쿼리인 경우 EF는 데이터가 이 쿼리의 결과 집합에 이미 있는 엔터티를 나타내는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-117">If this is a no-tracking query, EF checks if the data represents an entity already in the result set for this query</span></span>
      * <span data-ttu-id="e211c-118">그럴 경우 기존 엔터티가 반환됩니다.<sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="e211c-118">If so, the existing entity is returned <sup>(1)</sup></span></span>
      * <span data-ttu-id="e211c-119">그렇지 않을 경우 새 엔터티가 만들어지고 반환됩니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-119">If not, a new entity is created and returned</span></span>

<span data-ttu-id="e211c-120"><sup>(1)</sup> 추적 쿼리는 약한 참조를 사용하여 이미 반환된 엔터티를 추적하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-120"><sup>(1)</sup> No tracking queries use weak references to keep track of entities that have already been returned.</span></span> <span data-ttu-id="e211c-121">ID가 동일한 이전 결과가 범위를 벗어나고 가비지 수집이 실행되는 경우 새 엔터티 인스턴스를 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-121">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="e211c-122">쿼리가 실행되는 경우</span><span class="sxs-lookup"><span data-stu-id="e211c-122">When queries are executed</span></span>

<span data-ttu-id="e211c-123">LINQ 연산자를 호출할 때는 쿼리의 메모리 내 표현을 작성하기만 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-123">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="e211c-124">쿼리는 결과가 사용될 때만 데이터베이스로 전송됩니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-124">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="e211c-125">쿼리를 데이터베이스로 전송하는 가장 일반적인 작업은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-125">The most common operations that result in the query being sent to the database are:</span></span>
* <span data-ttu-id="e211c-126">`for` 루프에서 결과 반복</span><span class="sxs-lookup"><span data-stu-id="e211c-126">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="e211c-127">`ToList`, `ToArray`, `Single`, `Count` 같은 연산자 사용</span><span class="sxs-lookup"><span data-stu-id="e211c-127">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`</span></span>
* <span data-ttu-id="e211c-128">쿼리 결과를 UI에 데이터 바인딩</span><span class="sxs-lookup"><span data-stu-id="e211c-128">Databinding the results of a query to a UI</span></span>

> [!WARNING]  
> <span data-ttu-id="e211c-129">**항상 사용자 입력의 유효성 검사:** EF는 SQL 삽입 공격으로부터 보호를 제공하지만 입력의 일반적인 유효성 검사는 수행하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-129">**Always validate user input:** While EF does provide protection from SQL injection attacks, it does not do any general validation of input.</span></span> <span data-ttu-id="e211c-130">따라서 값이 API에 전달되거나, LINQ 쿼리에서 사용되거나, 엔터티 속성에 할당되는 경우 신뢰할 수 없는 소스에서 가져오면 응용 프로그램 요구 사항에 따라 적절한 유효성 검사를 수행해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-130">Therefore if values being passed to APIs, used in LINQ queries, assigned to entity properties, etc., come from an untrusted source then appropriate validation, per your application requirements, should be performed.</span></span> <span data-ttu-id="e211c-131">여기에는 쿼리를 동적으로 생성하는 데 사용되는 사용자 입력이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-131">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="e211c-132">LINQ를 사용하는 경우에도 식을 작성하는 사용자 입력을 허용하려면 의도한 식만 생성되도록 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e211c-132">Even when using LINQ, if you are accepting user input to build expressions you need to make sure than only intended expressions can be constructed.</span></span>
