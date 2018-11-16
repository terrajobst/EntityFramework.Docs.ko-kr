---
title: Api-EF Core 만들기 및 삭제
author: bricelam
ms.author: bricelam
ms.date: 11/7/2018
ms.openlocfilehash: 40d9e3aa0aba1bf2bc341f01dd815ed7cb7b48fa
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688631"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="47832-102">Api 만들기 및 삭제</span><span class="sxs-lookup"><span data-stu-id="47832-102">Create and Drop APIs</span></span>

<span data-ttu-id="47832-103">EnsureCreated 메서드와 EnsureDeleted 간단한 대안을 제공 [마이그레이션을](migrations/index.md) 데이터베이스 스키마를 관리 합니다.</span><span class="sxs-lookup"><span data-stu-id="47832-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="47832-104">이러한 메서드는 데이터는 일시적 이며 스키마가 변경 하는 경우에 삭제할 수 있습니다 하는 경우 시나리오에서 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="47832-104">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="47832-105">예를 들어 하는 동안 프로토타입 생성, 테스트 또는 로컬 캐시에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="47832-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="47832-106">일부 공급자 (비관계형 특히) 마이그레이션을 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="47832-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="47832-107">이러한 공급자에 대 한 EnsureCreated은 데이터베이스 스키마를 초기화 하는 가장 쉬운 방법은 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="47832-107">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="47832-108">EnsureCreated 및 마이그레이션 함께 잘 작동 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="47832-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="47832-109">마이그레이션을 사용 하는 경우 스키마를 초기화할 EnsureCreated를 사용 하지 마세요.</span><span class="sxs-lookup"><span data-stu-id="47832-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="47832-110">원활한 환경을 아닙니다 EnsureCreated에서 마이그레이션을로 전환 합니다.</span><span class="sxs-lookup"><span data-stu-id="47832-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="47832-111">그렇게 하려면 가장 간단한 방법은 데이터베이스를 삭제 하 고 마이그레이션을 사용 하 여 다시 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="47832-111">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="47832-112">나중에 마이그레이션을 사용 하 여 예상 되는 경우 EnsureCreated를 사용 하는 대신 마이그레이션을 사용 하 여 시작 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="47832-112">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="47832-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="47832-113">EnsureDeleted</span></span>

<span data-ttu-id="47832-114">EnsureDeleted 메서드는 존재 하는 경우 데이터베이스를 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="47832-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="47832-115">적절 한 권한이 없으면 예외가 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="47832-115">If you don't have the appropriate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="47832-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="47832-116">EnsureCreated</span></span>

<span data-ttu-id="47832-117">EnsureCreated 존재 하지 않습니다 고 데이터베이스 스키마를 초기화 하는 경우 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="47832-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="47832-118">모든 테이블이 존재 하는 경우 (다른 DbContext 클래스에 대 한 테이블 포함), 스키마 않습니다 초기화.</span><span class="sxs-lookup"><span data-stu-id="47832-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="47832-119">이러한 메서드의 비동기 버전도 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="47832-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="47832-120">SQL 스크립트</span><span class="sxs-lookup"><span data-stu-id="47832-120">SQL Script</span></span>

<span data-ttu-id="47832-121">EnsureCreated에서 사용 하는 SQL을 가져오려면 GenerateCreateScript 메서드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="47832-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="47832-122">여러 DbContext 클래스</span><span class="sxs-lookup"><span data-stu-id="47832-122">Multiple DbContext classes</span></span>

<span data-ttu-id="47832-123">EnsureCreated는 테이블이 데이터베이스에 있는 경우에 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="47832-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="47832-124">필요한 경우 스키마를 초기화 해야 하는 경우 사용자 고유의 확인 쓰고 기본 IRelationalDatabaseCreator 서비스를 사용 하 여 스키마를 초기화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="47832-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
