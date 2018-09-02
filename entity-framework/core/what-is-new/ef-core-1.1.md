---
title: EF Core 1.1의 새로운 기능 - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: 9f8f2d46f967c7d8ec4f8ea410e51531dfe3ca7b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995437"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="0463e-102">EF Core 1.1의 새로운 기능</span><span class="sxs-lookup"><span data-stu-id="0463e-102">New features in EF Core 1.1</span></span>

## <a name="modelling"></a><span data-ttu-id="0463e-103">모델링</span><span class="sxs-lookup"><span data-stu-id="0463e-103">Modelling</span></span>
### <a name="field-mapping"></a><span data-ttu-id="0463e-104">필드 매핑</span><span class="sxs-lookup"><span data-stu-id="0463e-104">Field mapping</span></span>
<span data-ttu-id="0463e-105">속성에 대한 지원 필드를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0463e-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="0463e-106">이는 속성보다는 Get/Set 메서드가 있는 데이터 또는 읽기 전용 속성에 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0463e-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>
### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="0463e-107">SQL Server의 메모리 최적화 테이블에 매핑</span><span class="sxs-lookup"><span data-stu-id="0463e-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>
<span data-ttu-id="0463e-108">엔터티가 매핑된 테이블이 메모리 최적화되도록 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0463e-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="0463e-109">EF Core를 사용하여 모델을 기반으로 데이터베이스를 만들고 유지 관리할 경우(마이그레이션 또는 `Database.EnsureCreated()` 사용) 이러한 엔터티에 대한 메모리 최적화 테이블이 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="0463e-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="0463e-110">Change tracking</span><span class="sxs-lookup"><span data-stu-id="0463e-110">Change tracking</span></span>
### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="0463e-111">EF6의 추가 변경 내용 추적 API</span><span class="sxs-lookup"><span data-stu-id="0463e-111">Additional change tracking APIs from EF6</span></span>
<span data-ttu-id="0463e-112">예: `Reload`, `GetModifiedProperties`, `GetDatabaseValues` 등</span><span class="sxs-lookup"><span data-stu-id="0463e-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="0463e-113">Query</span><span class="sxs-lookup"><span data-stu-id="0463e-113">Query</span></span>
### <a name="explicit-loading"></a><span data-ttu-id="0463e-114">명시적 로드</span><span class="sxs-lookup"><span data-stu-id="0463e-114">Explicit Loading</span></span>
<span data-ttu-id="0463e-115">이전에 데이터베이스에서 로드된 엔터티에서 탐색 속성 채우기를 트리거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0463e-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>
### <a name="dbsetfind"></a><span data-ttu-id="0463e-116">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="0463e-116">DbSet.Find</span></span>
<span data-ttu-id="0463e-117">기본 키 값을 기반으로 엔터티를 쉽게 페치하는 방법을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="0463e-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="0463e-118">기타</span><span class="sxs-lookup"><span data-stu-id="0463e-118">Other</span></span>
### <a name="connection-resiliency"></a><span data-ttu-id="0463e-119">연결 복원력</span><span class="sxs-lookup"><span data-stu-id="0463e-119">Connection resiliency</span></span>
<span data-ttu-id="0463e-120">실패한 데이터베이스 명령을 자동으로 다시 시도합니다.</span><span class="sxs-lookup"><span data-stu-id="0463e-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="0463e-121">이 기능은 일시적인 실패가 일반적인 SQL Azure에 연결할 때 특히 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="0463e-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>
### <a name="simplified-service-replacement"></a><span data-ttu-id="0463e-122">간단한 서비스 바꾸기</span><span class="sxs-lookup"><span data-stu-id="0463e-122">Simplified service replacement</span></span>
<span data-ttu-id="0463e-123">EF가 사용하는 내부 서비스를 더욱 쉽게 바꿀 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0463e-123">Makes it easier to replace internal services that EF uses.</span></span>
