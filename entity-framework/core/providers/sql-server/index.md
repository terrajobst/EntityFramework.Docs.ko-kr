---
title: Microsoft SQL Server 데이터베이스 공급자 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: 2ed7c0dd127db03d5e7340fde1ef83cf01b30135
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678652"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="0043e-102">Microsoft SQL Server EF Core 데이터베이스 공급자 </span><span class="sxs-lookup"><span data-stu-id="0043e-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="0043e-103">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 Microsoft SQL Server(SQL Azure 포함)에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0043e-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="0043e-104">공급자는 [Entity Framework Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="0043e-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="0043e-105">설치</span><span class="sxs-lookup"><span data-stu-id="0043e-105">Install</span></span>

<span data-ttu-id="0043e-106">[Microsoft.EntityFrameworkCore.SqlServer NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="0043e-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="0043e-107">시작</span><span class="sxs-lookup"><span data-stu-id="0043e-107">Get Started</span></span>

<span data-ttu-id="0043e-108">다음 리소스는 이 공급자를 시작하는 데 도움이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0043e-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="0043e-109">.NET Framework(Console, WinForms, WPF 등)에서 시작</span><span class="sxs-lookup"><span data-stu-id="0043e-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="0043e-110">ASP.NET Core에서 시작</span><span class="sxs-lookup"><span data-stu-id="0043e-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* <span data-ttu-id="0043e-111">[UnicornStore 샘플 응용 프로그램](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore) </span><span class="sxs-lookup"><span data-stu-id="0043e-111">[UnicornStore Sample Application](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="0043e-112">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="0043e-112">Supported Database Engines</span></span>

* <span data-ttu-id="0043e-113">Microsoft SQL Server(2008 이후)</span><span class="sxs-lookup"><span data-stu-id="0043e-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="0043e-114">지원되는 플랫폼</span><span class="sxs-lookup"><span data-stu-id="0043e-114">Supported Platforms</span></span>

* <span data-ttu-id="0043e-115">.NET Framework(4.5.1부터)</span><span class="sxs-lookup"><span data-stu-id="0043e-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="0043e-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="0043e-116">.NET Core</span></span>

* <span data-ttu-id="0043e-117">Mono(4.2.0부터)</span><span class="sxs-lookup"><span data-stu-id="0043e-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
