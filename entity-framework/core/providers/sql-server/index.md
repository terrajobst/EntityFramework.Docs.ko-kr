---
title: Microsoft SQL Server 데이터베이스 공급자 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: a524794a61a9f5078998aea04b45c31c19357f2b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995671"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Microsoft SQL Server EF Core 데이터베이스 공급자 

이 데이터베이스 공급자를 설치하면 Entity Framework Core를 Microsoft SQL Server(SQL Azure 포함)에서 사용할 수 있습니다. 공급자는 [Entity Framework Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore)의 일부로 유지 관리됩니다.

## <a name="install"></a>설치

[Microsoft.EntityFrameworkCore.SqlServer NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)를 설치합니다.

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a>시작

다음 리소스는 이 공급자를 시작하는 데 도움이 될 수 있습니다.
* [.NET Framework(Console, WinForms, WPF 등)에서 시작](../../get-started/full-dotnet/index.md)

* [ASP.NET Core에서 시작](../../get-started/aspnetcore/index.md)

* [UnicornStore 샘플 응용 프로그램](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore) 

## <a name="supported-database-engines"></a>지원되는 데이터베이스 엔진

* Microsoft SQL Server(2008 이후)

## <a name="supported-platforms"></a>지원되는 플랫폼

* .NET Framework(4.5.1부터)

* .NET Core

* Mono(4.2.0부터)

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
