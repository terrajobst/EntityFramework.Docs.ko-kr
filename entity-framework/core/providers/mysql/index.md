---
title: "MySQL 데이터베이스 공급자 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: 1500d017cb463c3f394131a79b9063ff90cce5e2
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/22/2017
---
# <a name="mysql-ef-core-database-provider"></a>MySQL EF Core 데이터베이스 공급자

이 데이터베이스 공급자를 설치하면 Entity Framework Core를 MySQL에서 사용할 수 있습니다. 이 공급자는 [MySQL 프로젝트](http://dev.mysql.com)의 일부로 유지 관리됩니다.

> [!WARNING]  
> 이 공급자는 시험판 버전입니다.

> [!NOTE]  
> 이 공급자는 Entity Framework Core 프로젝트의 일부로 유지 관리되지 않습니다. 타사 공급자를 고려할 때는 품질, 라이선싱, 지원 등을 평가하여 요구 사항에 부합하는지 확인합니다.

## <a name="install"></a>설치

[MySql.Data.EntityFrameworkCore NuGet 패키지](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)를 설치합니다.

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a>시작

[MySQL EF Core 공급자 및 Connector/Net 7.0.4 시작](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/)을 참조하세요.

## <a name="supported-database-engines"></a>지원되는 데이터베이스 엔진

* MySQL

## <a name="supported-platforms"></a>지원되는 플랫폼

* .NET Framework(4.5.1부터)

* .NET Core

[여기](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) 및 [여기](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)에서 버전 호환성 정보에 대한 MySQL 설명서를 검토하십시오.
