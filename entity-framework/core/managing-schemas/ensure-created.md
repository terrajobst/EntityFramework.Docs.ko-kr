---
title: Api 만들기 및 삭제-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2018
ms.openlocfilehash: 88c1403d2fae740ad78bb7c41d404b0dd91e86ae
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813437"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="a4500-102">API 만들기 및 삭제</span><span class="sxs-lookup"><span data-stu-id="a4500-102">Create and Drop APIs</span></span>

<span data-ttu-id="a4500-103">EnsureCreated 및 EnsureDeleted 메서드는 데이터베이스 스키마를 관리 하기 위한 [마이그레이션](migrations/index.md) 에 대 한 간단한 대안을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="a4500-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="a4500-104">이러한 메서드는 데이터가 일시적 이며 스키마가 변경 될 때 삭제할 수 있는 시나리오에서 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="a4500-104">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="a4500-105">예를 들어 프로토타입, 테스트 또는 로컬 캐시에 대 한입니다.</span><span class="sxs-lookup"><span data-stu-id="a4500-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="a4500-106">일부 공급자 (특히 비관계형가 아님)는 마이그레이션을 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="a4500-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="a4500-107">이러한 공급자의 경우 EnsureCreated은 종종 데이터베이스 스키마를 초기화 하는 가장 쉬운 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="a4500-107">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="a4500-108">EnsureCreated와 마이그레이션은 함께 제대로 작동 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="a4500-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="a4500-109">마이그레이션을 사용 하는 경우 스키마를 초기화 하는 데 EnsureCreated를 사용 하지 마세요.</span><span class="sxs-lookup"><span data-stu-id="a4500-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="a4500-110">EnsureCreated에서 마이그레이션으로의 변환은 원활한 환경이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="a4500-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="a4500-111">이 작업을 수행 하는 가장 간단한 방법은 데이터베이스를 삭제 하 고 마이그레이션을 사용 하 여 다시 만드는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="a4500-111">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="a4500-112">나중에 마이그레이션을 사용할 것으로 예상 되는 경우 EnsureCreated를 사용 하는 대신 마이그레이션을 시작 하는 것이 가장 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="a4500-112">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="a4500-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="a4500-113">EnsureDeleted</span></span>

<span data-ttu-id="a4500-114">EnsureDeleted 메서드는 데이터베이스 (있는 경우)를 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="a4500-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="a4500-115">적절 한 권한이 없는 경우 예외가 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a4500-115">If you don't have the appropriate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="a4500-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="a4500-116">EnsureCreated</span></span>

<span data-ttu-id="a4500-117">EnsureCreated가 존재 하지 않는 경우 데이터베이스를 만들고 데이터베이스 스키마를 초기화 합니다.</span><span class="sxs-lookup"><span data-stu-id="a4500-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="a4500-118">테이블이 있으면 (다른 DbContext 클래스에 대 한 테이블 포함) 스키마가 초기화 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="a4500-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="a4500-119">이러한 메서드의 비동기 버전도 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a4500-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="a4500-120">SQL 스크립트</span><span class="sxs-lookup"><span data-stu-id="a4500-120">SQL Script</span></span>

<span data-ttu-id="a4500-121">EnsureCreated에서 사용 하는 SQL을 가져오기 위해 GenerateCreateScript 메서드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a4500-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="a4500-122">여러 DbContext 클래스</span><span class="sxs-lookup"><span data-stu-id="a4500-122">Multiple DbContext classes</span></span>

<span data-ttu-id="a4500-123">EnsureCreated는 데이터베이스에 테이블이 없는 경우에만 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="a4500-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="a4500-124">필요한 경우 직접 검사를 작성 하 여 스키마를 초기화 해야 하는지 여부를 확인 하 고 기본 IRelationalDatabaseCreator 서비스를 사용 하 여 스키마를 초기화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a4500-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
