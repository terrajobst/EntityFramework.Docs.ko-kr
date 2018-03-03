---
title: "Microsoft SQL Server Compact Edition 데이터베이스 공급자 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a>Microsoft SQL Server Compact Edition EF Core 데이터베이스 공급자 

이 데이터베이스 공급자를 설치하면 Entity Framework Core를 SQL Server Compact Edition에서 사용할 수 있습니다. 이 공급자는[ErikEJ/EntityFramework.SqlServerCompact GitHub 프로젝트](https://github.com/ErikEJ/EntityFramework.SqlServerCompact)의 일부로 유지 관리됩니다.

> [!NOTE]  
> 이 공급자는 Entity Framework Core 프로젝트의 일부로 유지 관리되지 않습니다. 타사 공급자를 고려할 때는 품질, 라이선싱, 지원 등을 평가하여 요구 사항에 부합하는지 확인합니다.

## <a name="install"></a>설치

SQL Server Compact Edition 4.0을 사용하려면 [EntityFrameworkCore.SqlServerCompact40 NuGet 패키지](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)를 설치합니다.

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

SQL Server Compact Edition 3.5를 사용하려면 [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)를 설치합니다.

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a>시작

[프로젝트 사이트의 시작 설명서](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications) 참조

## <a name="supported-database-engines"></a>지원되는 데이터베이스 엔진

* SQL Server Compact Edition 3.5

* SQL Server Compact Edition 4.0

## <a name="supported-platforms"></a>지원되는 플랫폼

* .NET Framework(4.5.1부터)
