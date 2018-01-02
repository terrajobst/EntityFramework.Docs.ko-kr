---
title: "PostgreSQL용 Npgsql 데이터베이스 공급자 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a>PostgreSQL용 Npgsql EF Core 데이터베이스 공급자

이 데이터베이스 공급자를 설치하면 Entity Framework Core를 PostgreSQL에서 사용할 수 있습니다. 이 공급자는 [Npgsql 프로젝트](http://www.npgsql.org)의 일부로 유지 관리됩니다.

> [!NOTE]  
> 이 공급자는 Entity Framework Core 프로젝트의 일부로 유지 관리되지 않습니다. 타사 공급자를 고려할 때는 품질, 라이선싱, 지원 등을 평가하여 요구 사항에 부합하는지 확인합니다.

## <a name="install"></a>설치

[Npgsql.EntityFrameworkCore.PostgreSQL NuGet 패키지](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)를 설치합니다.

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a>시작

시작은 [Npgsql 설명서](http://www.npgsql.org/efcore/index.html)를 참조하세요.

## <a name="supported-database-engines"></a>지원되는 데이터베이스 엔진

* PostgreSQL

## <a name="supported-platforms"></a>지원되는 플랫폼

* .NET Framework(4.5.1부터)

* .NET Core

* Mono(4.2.0부터)
