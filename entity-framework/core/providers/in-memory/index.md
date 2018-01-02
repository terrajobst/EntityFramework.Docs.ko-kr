---
title: "InMemory 데이터베이스 공급자 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: a8e05f50837f3da554b338475d24215706dfa2ec
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="d4fd0-102">EF 코어 In-Memory 데이터베이스 공급자</span><span class="sxs-lookup"><span data-stu-id="d4fd0-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="d4fd0-103">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 메모리 내 데이터베이스에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d4fd0-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="d4fd0-104">Entity Framework Core를 사용하는 코드를 테스트할 때 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="d4fd0-104">This is useful when testing code that uses Entity Framework Core.</span></span> <span data-ttu-id="d4fd0-105">이 공급자는 [EntityFramework GitHub 프로젝트](https://github.com/aspnet/EntityFramework)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="d4fd0-105">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="d4fd0-106">설치</span><span class="sxs-lookup"><span data-stu-id="d4fd0-106">Install</span></span>

<span data-ttu-id="d4fd0-107">[Microsoft.EntityFrameworkCore.InMemory NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="d4fd0-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="d4fd0-108">시작</span><span class="sxs-lookup"><span data-stu-id="d4fd0-108">Get Started</span></span>

<span data-ttu-id="d4fd0-109">다음 리소스는 이 공급자를 시작하는 데 도움이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d4fd0-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="d4fd0-110">InMemory 테스트</span><span class="sxs-lookup"><span data-stu-id="d4fd0-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* <span data-ttu-id="d4fd0-111">[UnicornStore 샘플 응용 프로그램 테스트](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs) </span><span class="sxs-lookup"><span data-stu-id="d4fd0-111">[UnicornStore Sample Application Tests](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="d4fd0-112">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="d4fd0-112">Supported Database Engines</span></span>

* <span data-ttu-id="d4fd0-113">기본 제공 메모리 내 데이터베이스(테스트 전용으로 설계)</span><span class="sxs-lookup"><span data-stu-id="d4fd0-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="d4fd0-114">지원되는 플랫폼</span><span class="sxs-lookup"><span data-stu-id="d4fd0-114">Supported Platforms</span></span>

* <span data-ttu-id="d4fd0-115">.NET Framework(4.5.1부터)</span><span class="sxs-lookup"><span data-stu-id="d4fd0-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="d4fd0-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="d4fd0-116">.NET Core</span></span>

* <span data-ttu-id="d4fd0-117">Mono(4.2.0부터)</span><span class="sxs-lookup"><span data-stu-id="d4fd0-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="d4fd0-118">유니버설 Windows 플랫폼</span><span class="sxs-lookup"><span data-stu-id="d4fd0-118">Universal Windows Platform</span></span>
