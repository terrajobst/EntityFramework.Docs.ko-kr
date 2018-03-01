---
title: "쿼리 유형-EF 코어"
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: d03c4b1d5635530e63b93e051cb69583718deb4e
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="query-types"></a><span data-ttu-id="bcee9-102">쿼리 유형</span><span class="sxs-lookup"><span data-stu-id="bcee9-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="bcee9-103">이 기능은 EF 코어 2.1의 새로운 기능</span><span class="sxs-lookup"><span data-stu-id="bcee9-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="bcee9-104">쿼리 유형은 EF 핵심 모델에 추가할 수 있는 읽기 전용 쿼리 결과 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-104">Query Types are read-only query result types that can be added to the EF Core model.</span></span> <span data-ttu-id="bcee9-105">쿼리 유형 (예: 익명 형식), 임시 쿼리를 활성화 하지만 지정 된 매핑 구성 수 있으므로 유연 합니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-105">Query Types enable ad-hoc querying (like anonymous types), but are more flexible because they can have mapping configuration specified.</span></span>

<span data-ttu-id="bcee9-106">이러한은 있는 엔터티 형식에 개념적으로 유사 합니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-106">They are conceptually similar to Entity Types in that:</span></span>

- <span data-ttu-id="bcee9-107">POCO C#에서 모델에 추가 된 형식이 ```OnModelCreating``` 를 사용 하 여는 ```ModelBuilder.Query``` 메서드, 또는 DbContext "설정" 속성을 통해 (쿼리는 이러한 속성 형식으로 입력 된 ```DbQuery<T>``` 하는 대신 ```DbSet<T>```).</span><span class="sxs-lookup"><span data-stu-id="bcee9-107">They are POCO C# types that are added to the model, either in ```OnModelCreating``` using the ```ModelBuilder.Query``` method, or via a DbContext "set" property (for query types such a property is typed as ```DbQuery<T>``` rather that ```DbSet<T>```).</span></span>
- <span data-ttu-id="bcee9-108">대부분의 일반 엔터티 형식으로 동일한 매핑 기능 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-108">They support much of the same mapping capabilities as regular entity types.</span></span> <span data-ttu-id="bcee9-109">예: 상속 매핑, 탐색 (limitiations 아래 참조) 및 관계형 저장소를 통해 대상 데이터베이스 스키마 개체를 구성 하는 기능에서 ```ToTable```, ```HasColumn``` fluent api 메서드 (또는 데이터 주석).</span><span class="sxs-lookup"><span data-stu-id="bcee9-109">For example, inheritance mapping, navigations (see limitiations below) and, on relational stores, the ability to configure the target database schema objects via ```ToTable```, ```HasColumn``` fluent-api methods (or data annotations).</span></span>

<span data-ttu-id="bcee9-110">쿼리 유형으로는 엔터티에서 다른 것을 형식:</span><span class="sxs-lookup"><span data-stu-id="bcee9-110">Query Types are different from entity types in that they:</span></span>

- <span data-ttu-id="bcee9-111">키를 정의할 수는 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-111">Do not require a key to be defined.</span></span>
- <span data-ttu-id="bcee9-112">변경 추적 장치에 의해 추적 되지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-112">Are never tracked by the Change Tracker.</span></span>
- <span data-ttu-id="bcee9-113">규칙으로 검색 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="bcee9-114">특히 탐색 매핑 기능이-의 일부 지원, 관계의 주 끝으로 작동 하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-114">Only support a subset of navigation mapping capabilities - Specifically, they may never act as the principal end of a relationship.</span></span>
- <span data-ttu-id="bcee9-115">에 매핑할 수 있습니다는 _쿼리 정의_ -A 정의 쿼리는 쿼리 유형에 대 한 데이터 소스 역할을 하는 보조 쿼리입니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-115">May be mapped to a _defining query_ - A Defining Query is a secondary query that acts a data source for a Query Type.</span></span>

<span data-ttu-id="bcee9-116">쿼리 형식에 대 한 주요 사용 시나리오는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-116">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="bcee9-117">데이터베이스 뷰 매핑입니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-117">Mapping to database views.</span></span>
- <span data-ttu-id="bcee9-118">기본 키가 정의 되지 않은 테이블을 매핑하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-118">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="bcee9-119">에 대 한 반환 형식으로 처리 하는 임시 ```FromSql()``` 쿼리 합니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-119">Serving as the return type for ad hoc ```FromSql()``` queries.</span></span>
- <span data-ttu-id="bcee9-120">쿼리는 모델에 정의 된 매핑.</span><span class="sxs-lookup"><span data-stu-id="bcee9-120">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="bcee9-121">사용 하 여 이루어집니다 데이터베이스 뷰를 쿼리 형식을 매핑하는 ```ToTable``` fluent API입니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-121">Mapping a query type to a database view is achieved using the ```ToTable``` fluent API.</span></span>

## <a name="example"></a><span data-ttu-id="bcee9-122">예</span><span class="sxs-lookup"><span data-stu-id="bcee9-122">Example</span></span>

<span data-ttu-id="bcee9-123">다음 예제에서는 쿼리 유형을 사용 하 여 데이터베이스 뷰를 쿼리 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-123">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="bcee9-124">GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes)을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-124">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="bcee9-125">첫째, 간단한 블로그 및 Post 모델 정의:</span><span class="sxs-lookup"><span data-stu-id="bcee9-125">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="bcee9-126">다음으로 연결 된 각 블로그 게시물의 수를 쿼리할 수 있도록 간단한 데이터베이스 뷰를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-126">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="bcee9-127">다음으로에서는 구성에서 쿼리 유형을 _OnModelCreating_ 를 사용 하 여는 ```modelBuilder.Query<T>``` API입니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-127">Next, we configure the query type in _OnModelCreating_ using the ```modelBuilder.Query<T>``` API.</span></span>
<span data-ttu-id="bcee9-128">쿼리 형식에 대 한 매핑을 구성 하려면 표준 fluent 구성 Api를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-128">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="bcee9-129">마지막으로, 표준 방식으로 데이터베이스 뷰를 쿼리할 수 했습니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-129">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="bcee9-130">Note도 컨텍스트 수준 쿼리 속성 (DbQuery)이이 형식에 대 한 쿼리의 루트 역할을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="bcee9-130">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
