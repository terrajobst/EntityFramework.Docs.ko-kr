---
title: "Pomelo MySQL 데이터베이스 공급자 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d0198c04-d30d-4419-98f8-a54690cea3c8
ms.technology: entity-framework-core
uid: core/providers/pomelo/index
ms.openlocfilehash: 9ddcda1ae7b058c01937867817e2666b97e7f37a
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
# <a name="pomelo-ef-core-database-provider-for-mysql"></a>MySQL용 Pomelo EF Core 데이터베이스 공급자

이 데이터베이스 공급자를 설치하면 Entity Framework Core를 MySQL에서 사용할 수 있습니다. 이 공급자는 [Pomelo Foundation 프로젝트](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)의 일부로 유지 관리됩니다.

> [!NOTE]  
>
> 이 공급자는 Entity Framework Core 프로젝트의 일부로 유지 관리되지 않습니다. 타사 공급자를 고려할 때는 품질, 라이선싱, 지원 등을 평가하여 요구 사항에 부합하는지 확인합니다.

## <a name="install"></a>설치

[Pomelo.EntityFrameworkCore.MySql NuGet 패키지](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)를 설치합니다.

``` powershell
Install-Package Pomelo.EntityFrameworkCore.MySql
```

## <a name="get-started"></a>시작

다음 리소스는 이 공급자를 시작하는 데 도움이 될 수 있습니다.
* [시작 설명서](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md#getting-started)

* [Yuuko 블로그 샘플 응용 프로그램](https://github.com/PomeloFoundation/YuukoBlog)

## <a name="supported-database-engines"></a>지원되는 데이터베이스 엔진

* MySQL
* MariaDB

## <a name="supported-platforms"></a>지원되는 플랫폼

* .NET Framework(4.5.1부터)

* .NET Core

* Mono(4.2.0부터)
