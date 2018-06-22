---
title: InMemory 데이터베이스 공급자 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: 356af9390a8aafa5afe35f333cd1e6ac1988390d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678988"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="32bf4-102">EF 코어 In-Memory 데이터베이스 공급자</span><span class="sxs-lookup"><span data-stu-id="32bf4-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="32bf4-103">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 메모리 내 데이터베이스에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="32bf4-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="32bf4-104">메모리 내 모드인 SQLite 공급자가 관계형 데이터베이스에 보다 적절한 테스트로 대체될 수 있지만 이 기능이 테스트에 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="32bf4-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="32bf4-105">공급자는 [Entity Framework Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="32bf4-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="32bf4-106">설치</span><span class="sxs-lookup"><span data-stu-id="32bf4-106">Install</span></span>

<span data-ttu-id="32bf4-107">[Microsoft.EntityFrameworkCore.InMemory NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="32bf4-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="32bf4-108">시작</span><span class="sxs-lookup"><span data-stu-id="32bf4-108">Get Started</span></span>

<span data-ttu-id="32bf4-109">다음 리소스는 이 공급자를 시작하는 데 도움이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="32bf4-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="32bf4-110">InMemory 테스트</span><span class="sxs-lookup"><span data-stu-id="32bf4-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* <span data-ttu-id="32bf4-111">[UnicornStore 샘플 응용 프로그램 테스트](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs) </span><span class="sxs-lookup"><span data-stu-id="32bf4-111">[UnicornStore Sample Application Tests](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="32bf4-112">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="32bf4-112">Supported Database Engines</span></span>

* <span data-ttu-id="32bf4-113">기본 제공 메모리 내 데이터베이스(테스트 전용으로 설계)</span><span class="sxs-lookup"><span data-stu-id="32bf4-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="32bf4-114">지원되는 플랫폼</span><span class="sxs-lookup"><span data-stu-id="32bf4-114">Supported Platforms</span></span>

* <span data-ttu-id="32bf4-115">.NET Framework(4.5.1부터)</span><span class="sxs-lookup"><span data-stu-id="32bf4-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="32bf4-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="32bf4-116">.NET Core</span></span>

* <span data-ttu-id="32bf4-117">Mono(4.2.0부터)</span><span class="sxs-lookup"><span data-stu-id="32bf4-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="32bf4-118">유니버설 Windows 플랫폼</span><span class="sxs-lookup"><span data-stu-id="32bf4-118">Universal Windows Platform</span></span>
