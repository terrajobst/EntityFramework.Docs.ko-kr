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
# <a name="sqlite-ef-core-database-provider"></a>SQLite EF Core 데이터베이스 공급자

이 데이터베이스 공급자를 설치하면 Entity Framework Core를 SQLite에서 사용할 수 있습니다. 공급자는 [Entity Framework Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore)의 일부로 유지 관리됩니다.

## <a name="install"></a>설치

[Microsoft.EntityFrameworkCore.Sqlite NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)를 설치합니다.

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="supported-database-engines"></a>지원되는 데이터베이스 엔진

* SQLite(3.7부터 해당)

## <a name="limitations"></a>제한 사항

SQLite 공급자에 대한 중요한 제한 사항은 [SQLite 제한](limitations.md)을 참조하세요.
