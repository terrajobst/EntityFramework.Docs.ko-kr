---
title: Microsoft SQL Server 데이터베이스 공급자 - EF Core
description: Entity Framework Core를 Microsoft SQL Server에서 사용할 수 있도록 허용하는 데이터베이스 공급자에 관한 문서
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: baae668a7ec255e35ab0e23e5c5ddfa47bda917e
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502268"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Microsoft SQL Server EF Core 데이터베이스 공급자

이 데이터베이스 공급자를 설치하면 Entity Framework Core를 Microsoft SQL Server(Azure SQL Database 포함)에서 사용할 수 있습니다. 공급자는 [Entity Framework Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore)의 일부로 유지 관리됩니다.

## <a name="install"></a>설치

[Microsoft.EntityFrameworkCore.SqlServer NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)를 설치합니다.

### <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> 버전 3.0.0 이후 공급자는 Microsoft.Data.SqlClient를 참조합니다(System.Data.SqlClient에 종속된 이전 버전). 프로젝트가 SqlClient에 직접 종속될 경우, Microsoft.Data.SqlClient 패키지를 참조하세요.

## <a name="supported-database-engines"></a>지원되는 데이터베이스 엔진

* Microsoft SQL Server(2012 이상)
