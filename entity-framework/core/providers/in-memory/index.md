---
title: InMemory 데이터베이스 공급자 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: 2aed8db79157f994bcc259acdf1f7c161017a50a
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824396"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="513b2-102">EF 코어 In-Memory 데이터베이스 공급자</span><span class="sxs-lookup"><span data-stu-id="513b2-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="513b2-103">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 메모리 내 데이터베이스에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="513b2-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="513b2-104">메모리 내 모드인 SQLite 공급자가 관계형 데이터베이스에 보다 적절한 테스트로 대체될 수 있지만 이 기능이 테스트에 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="513b2-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="513b2-105">공급자는 [Entity Framework Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="513b2-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="513b2-106">설치</span><span class="sxs-lookup"><span data-stu-id="513b2-106">Install</span></span>

<span data-ttu-id="513b2-107">[Microsoft.EntityFrameworkCore.InMemory NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="513b2-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="513b2-108">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="513b2-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="513b2-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="513b2-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="513b2-110">시작</span><span class="sxs-lookup"><span data-stu-id="513b2-110">Get Started</span></span>

<span data-ttu-id="513b2-111">다음 리소스는 이 공급자를 시작하는 데 도움이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="513b2-111">The following resources will help you get started with this provider.</span></span>

* [<span data-ttu-id="513b2-112">InMemory 테스트</span><span class="sxs-lookup"><span data-stu-id="513b2-112">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)
* [<span data-ttu-id="513b2-113">UnicornStore 샘플 애플리케이션 테스트</span><span class="sxs-lookup"><span data-stu-id="513b2-113">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="513b2-114">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="513b2-114">Supported Database Engines</span></span>

<span data-ttu-id="513b2-115">In Process 메모리 데이터베이스(테스트 전용으로 설계)</span><span class="sxs-lookup"><span data-stu-id="513b2-115">In-process memory database (designed for testing purposes only)</span></span>
