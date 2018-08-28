---
title: SQLite 데이터베이스 공급자 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 31de8449a12a10d4f98ebb4bb6125389606e9bbd
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994004"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="fd11d-102">SQLite EF Core 데이터베이스 공급자</span><span class="sxs-lookup"><span data-stu-id="fd11d-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="fd11d-103">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 SQLite에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fd11d-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="fd11d-104">공급자는 [Entity Framework Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="fd11d-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="fd11d-105">설치</span><span class="sxs-lookup"><span data-stu-id="fd11d-105">Install</span></span>

<span data-ttu-id="fd11d-106">[Microsoft.EntityFrameworkCore.Sqlite NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="fd11d-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="fd11d-107">시작</span><span class="sxs-lookup"><span data-stu-id="fd11d-107">Get Started</span></span>

<span data-ttu-id="fd11d-108">다음 리소스는 이 공급자를 시작하는 데 도움이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fd11d-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="fd11d-109">UWP의 로컬 SQLite</span><span class="sxs-lookup"><span data-stu-id="fd11d-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="fd11d-110">.NET Core 응용 프로그램에서 새 SQLite 데이터베이스로</span><span class="sxs-lookup"><span data-stu-id="fd11d-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="fd11d-111">Unicorn Clicker 샘플 응용 프로그램</span><span class="sxs-lookup"><span data-stu-id="fd11d-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="fd11d-112">Unicorn Packer 샘플 응용 프로그램</span><span class="sxs-lookup"><span data-stu-id="fd11d-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="fd11d-113">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="fd11d-113">Supported Database Engines</span></span>

* <span data-ttu-id="fd11d-114">SQLite(3.7부터 해당)</span><span class="sxs-lookup"><span data-stu-id="fd11d-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="fd11d-115">지원되는 플랫폼</span><span class="sxs-lookup"><span data-stu-id="fd11d-115">Supported Platforms</span></span>

* <span data-ttu-id="fd11d-116">.NET Framework(4.5.1부터)</span><span class="sxs-lookup"><span data-stu-id="fd11d-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="fd11d-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd11d-117">.NET Core</span></span>

* <span data-ttu-id="fd11d-118">Mono(4.2.0부터)</span><span class="sxs-lookup"><span data-stu-id="fd11d-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="fd11d-119">유니버설 Windows 플랫폼</span><span class="sxs-lookup"><span data-stu-id="fd11d-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="fd11d-120">제한 사항</span><span class="sxs-lookup"><span data-stu-id="fd11d-120">Limitations</span></span>

<span data-ttu-id="fd11d-121">SQLite 공급자에 대한 중요한 제한 사항은 [SQLite 제한](limitations.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="fd11d-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
