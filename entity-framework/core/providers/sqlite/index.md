---
title: SQLite 데이터베이스 공급자 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: e8c3d675322b163fdf1e2e7e01f3815e28f427a2
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502255"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="f9cbc-102">SQLite EF Core 데이터베이스 공급자</span><span class="sxs-lookup"><span data-stu-id="f9cbc-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="f9cbc-103">이 데이터베이스 공급자를 설치하면 Entity Framework Core를 SQLite에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f9cbc-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="f9cbc-104">공급자는 [Entity Framework Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore)의 일부로 유지 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="f9cbc-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="f9cbc-105">설치</span><span class="sxs-lookup"><span data-stu-id="f9cbc-105">Install</span></span>

<span data-ttu-id="f9cbc-106">[Microsoft.EntityFrameworkCore.Sqlite NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="f9cbc-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

### <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="f9cbc-107">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f9cbc-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studiotabvs"></a>[<span data-ttu-id="f9cbc-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f9cbc-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="f9cbc-109">지원되는 데이터베이스 엔진</span><span class="sxs-lookup"><span data-stu-id="f9cbc-109">Supported Database Engines</span></span>

* <span data-ttu-id="f9cbc-110">SQLite(3.7부터 해당)</span><span class="sxs-lookup"><span data-stu-id="f9cbc-110">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="f9cbc-111">제한 사항</span><span class="sxs-lookup"><span data-stu-id="f9cbc-111">Limitations</span></span>

<span data-ttu-id="f9cbc-112">SQLite 공급자에 대한 중요한 제한 사항은 [SQLite 제한](limitations.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="f9cbc-112">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
