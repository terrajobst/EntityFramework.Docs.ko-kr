---
title: "Microsoft SQL Server 데이터베이스 공급자 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: b2faf932e0484da4df0c1774afa7ba7ae2d077a5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="6f39f-102">Microsoft SQL Server EF Core 데이터베이스 공급자 </span><span class="sxs-lookup"><span data-stu-id="6f39f-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="6f39f-103">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 Microsoft SQL Server(SQL Azure 포함)에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6f39f-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="6f39f-104">이 공급자는 [EntityFramework GitHub 프로젝트](https://github.com/aspnet/EntityFramework)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="6f39f-104">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="6f39f-105">설치</span><span class="sxs-lookup"><span data-stu-id="6f39f-105">Install</span></span>

<span data-ttu-id="6f39f-106">[Microsoft.EntityFrameworkCore.SqlServer NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="6f39f-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="6f39f-107">시작</span><span class="sxs-lookup"><span data-stu-id="6f39f-107">Get Started</span></span>

<span data-ttu-id="6f39f-108">다음 리소스는 이 공급자를 시작하는 데 도움이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6f39f-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="6f39f-109">.NET Framework(Console, WinForms, WPF 등)에서 시작</span><span class="sxs-lookup"><span data-stu-id="6f39f-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="6f39f-110">ASP.NET Core에서 시작</span><span class="sxs-lookup"><span data-stu-id="6f39f-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* <span data-ttu-id="6f39f-111">[UnicornStore 샘플 응용 프로그램](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore) </span><span class="sxs-lookup"><span data-stu-id="6f39f-111">[UnicornStore Sample Application](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="6f39f-112">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="6f39f-112">Supported Database Engines</span></span>

* <span data-ttu-id="6f39f-113">Microsoft SQL Server(2008 이후)</span><span class="sxs-lookup"><span data-stu-id="6f39f-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="6f39f-114">지원되는 플랫폼</span><span class="sxs-lookup"><span data-stu-id="6f39f-114">Supported Platforms</span></span>

* <span data-ttu-id="6f39f-115">.NET Framework(4.5.1부터)</span><span class="sxs-lookup"><span data-stu-id="6f39f-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="6f39f-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="6f39f-116">.NET Core</span></span>

* <span data-ttu-id="6f39f-117">Mono(4.2.0부터)</span><span class="sxs-lookup"><span data-stu-id="6f39f-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
