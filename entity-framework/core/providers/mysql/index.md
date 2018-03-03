---
title: "MySQL 데이터베이스 공급자 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: 1500d017cb463c3f394131a79b9063ff90cce5e2
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="mysql-ef-core-database-provider"></a><span data-ttu-id="c9f67-102">MySQL EF Core 데이터베이스 공급자</span><span class="sxs-lookup"><span data-stu-id="c9f67-102">MySQL EF Core Database Provider</span></span>

<span data-ttu-id="c9f67-103">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 MySQL에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c9f67-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="c9f67-104">이 공급자는 [MySQL 프로젝트](http://dev.mysql.com)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="c9f67-104">The provider is maintained as part of the [MySQL project](http://dev.mysql.com).</span></span>

> [!WARNING]  
> <span data-ttu-id="c9f67-105">이 공급자는 시험판 버전입니다.</span><span class="sxs-lookup"><span data-stu-id="c9f67-105">This provider is pre-release.</span></span>

> [!NOTE]  
> <span data-ttu-id="c9f67-106">이 공급자는 Entity Framework Core 프로젝트의 일부로 유지 관리되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c9f67-106">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="c9f67-107">타사 공급자를 고려할 때는 품질, 라이선싱, 지원 등을 평가하여 요구 사항에 부합하는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="c9f67-107">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="c9f67-108">설치</span><span class="sxs-lookup"><span data-stu-id="c9f67-108">Install</span></span>

<span data-ttu-id="c9f67-109">[MySql.Data.EntityFrameworkCore NuGet 패키지](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="c9f67-109">Install the [MySql.Data.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span></span>

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a><span data-ttu-id="c9f67-110">시작</span><span class="sxs-lookup"><span data-stu-id="c9f67-110">Get Started</span></span>

<span data-ttu-id="c9f67-111">[MySQL EF Core 공급자 및 Connector/Net 7.0.4 시작](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="c9f67-111">See [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="c9f67-112">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="c9f67-112">Supported Database Engines</span></span>

* <span data-ttu-id="c9f67-113">MySQL</span><span class="sxs-lookup"><span data-stu-id="c9f67-113">MySQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="c9f67-114">지원되는 플랫폼</span><span class="sxs-lookup"><span data-stu-id="c9f67-114">Supported Platforms</span></span>

* <span data-ttu-id="c9f67-115">.NET Framework(4.5.1부터)</span><span class="sxs-lookup"><span data-stu-id="c9f67-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="c9f67-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="c9f67-116">.NET Core</span></span>

<span data-ttu-id="c9f67-117">[여기](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) 및 [여기](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)에서 버전 호환성 정보에 대한 MySQL 설명서를 검토하십시오.</span><span class="sxs-lookup"><span data-stu-id="c9f67-117">Be sure to review the MySQL documentation for version compatibility information [here](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) and [here](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)</span></span>
