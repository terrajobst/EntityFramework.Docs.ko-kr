---
title: SQLite 데이터베이스 공급자 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: e4cbdba46f901831892192a343db2920a5760042
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149273"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="a31eb-102">SQLite EF Core 데이터베이스 공급자</span><span class="sxs-lookup"><span data-stu-id="a31eb-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="a31eb-103">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 SQLite에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a31eb-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="a31eb-104">공급자는 [Entity Framework Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="a31eb-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="a31eb-105">설치</span><span class="sxs-lookup"><span data-stu-id="a31eb-105">Install</span></span>

<span data-ttu-id="a31eb-106">[Microsoft.EntityFrameworkCore.Sqlite NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="a31eb-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="supported-database-engines"></a><span data-ttu-id="a31eb-107">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="a31eb-107">Supported Database Engines</span></span>

* <span data-ttu-id="a31eb-108">SQLite(3.7부터 해당)</span><span class="sxs-lookup"><span data-stu-id="a31eb-108">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="a31eb-109">제한 사항</span><span class="sxs-lookup"><span data-stu-id="a31eb-109">Limitations</span></span>

<span data-ttu-id="a31eb-110">SQLite 공급자에 대한 중요한 제한 사항은 [SQLite 제한](limitations.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="a31eb-110">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
