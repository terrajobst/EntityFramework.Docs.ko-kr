---
title: "IBM Data Server(DB2) 데이터베이스 공급자 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a><span data-ttu-id="ced5b-102">IBM Data Server (DB2) EF Core 데이터베이스 공급자</span><span class="sxs-lookup"><span data-stu-id="ced5b-102">IBM Data Server (DB2) EF Core Database Providers</span></span>

<span data-ttu-id="ced5b-103">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 IBM Data Server에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ced5b-103">This database provider allows Entity Framework Core to be used with IBM Data Server.</span></span> <span data-ttu-id="ced5b-104">이 공급자는 IBM에서 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="ced5b-104">The provider is maintained by IBM.</span></span>

> [!NOTE]  
> <span data-ttu-id="ced5b-105">이 공급자는 Entity Framework Core 프로젝트의 일부로 유지 관리되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ced5b-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="ced5b-106">타사 공급자를 고려할 때는 품질, 라이선싱, 지원 등을 평가하여 요구 사항에 부합하는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="ced5b-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="ced5b-107">설치</span><span class="sxs-lookup"><span data-stu-id="ced5b-107">Install</span></span>

<span data-ttu-id="ced5b-108">Windows에서 IBM Data Server를 사용하려면 [IBM.EntityFrameworkCore NuGet 패키지](https://www.nuget.org/packages/IBM.EntityFrameworkCore)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="ced5b-108">To work with IBM Data Server on Windows, install the [IBM.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore
```

<span data-ttu-id="ced5b-109">Linux에서 IBM Data Server를 사용하려면 [IBM.EntityFrameworkCore-lnx NuGet 패키지](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="ced5b-109">To work with IBM Data Server on Linux, install the [IBM.EntityFrameworkCore-lnx NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a><span data-ttu-id="ced5b-110">시작</span><span class="sxs-lookup"><span data-stu-id="ced5b-110">Get Started</span></span>

[<span data-ttu-id="ced5b-111">.NET Core용 IBM.NET 공급자 시작</span><span class="sxs-lookup"><span data-stu-id="ced5b-111">Getting started with IBM .NET Provider for .NET Core</span></span>](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a><span data-ttu-id="ced5b-112">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="ced5b-112">Supported Database Engines</span></span>

* <span data-ttu-id="ced5b-113">zOS</span><span class="sxs-lookup"><span data-stu-id="ced5b-113">zOS</span></span>
* <span data-ttu-id="ced5b-114">IBM dashDB를 포함한 LUW</span><span class="sxs-lookup"><span data-stu-id="ced5b-114">LUW including IBM dashDB</span></span>
* <span data-ttu-id="ced5b-115">IBM I</span><span class="sxs-lookup"><span data-stu-id="ced5b-115">IBM I</span></span>
* <span data-ttu-id="ced5b-116">Informix</span><span class="sxs-lookup"><span data-stu-id="ced5b-116">Informix</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="ced5b-117">지원되는 플랫폼</span><span class="sxs-lookup"><span data-stu-id="ced5b-117">Supported Platforms</span></span>

* <span data-ttu-id="ced5b-118">.NET Framework(4.6부터)</span><span class="sxs-lookup"><span data-stu-id="ced5b-118">.NET Framework (4.6 onwards)</span></span>
* <span data-ttu-id="ced5b-119">.NET Core</span><span class="sxs-lookup"><span data-stu-id="ced5b-119">.NET Core</span></span>
