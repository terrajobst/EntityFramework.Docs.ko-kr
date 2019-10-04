---
title: 데이터베이스 공급자 - EF Core
author: rowanmiller
ms.date: 02/23/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/providers/index
ms.openlocfilehash: db06906e6af518a27a21f30b12d722ce06e9bd52
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813519"
---
# <a name="database-providers"></a>데이터베이스 공급자

Entity Framework Core는 플러그 인 라이브러리 호출 데이터베이스 공급자를 통해 다양한 데이터베이스에 액세스할 수 있습니다.

## <a name="current-providers"></a>현재 공급자
> [!IMPORTANT]  
> EF Core 공급자는 다양한 원본을 바탕으로 작성됩니다. 일부 공급자는 [Entity Framework Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore)의 일부로 유지되지 않습니다. 공급자를 고려할 때는 품질, 라이선싱, 지원 등을 평가하여 요구 사항을 충족하는지 확인해야 합니다. 또한 자세한 버전 호환성 정보는 각 공급자의 설명서를 검토해야 합니다.

| NuGet 패키지                                                                                                        | 지원되는 데이터베이스 엔진 | 유지 관리자/공급 업체                                                           | 참고/요구 사항 | 유용한 링크                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:---------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2012 이상    | [EF Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore/)(Microsoft) |                      | [docs](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | SQLite 3.7 이상         | [EF Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore/)(Microsoft) |                      | [docs](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | EF Core 메모리 내 데이터베이스 | [EF Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore/)(Microsoft) | 테스트 전용     | [docs](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos)          | Azure Cosmos DB SQL API    | [EF Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore/)(Microsoft) |                      | [docs](xref:core/providers/cosmos/index)                                                                                         |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)        | PostgreSQL                 | [Npgsql 개발 팀](https://github.com/npgsql)                          |                      | [docs](http://www.npgsql.org/efcore/index.html)                                                                                                                                                    |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Pomelo Foundation 프로젝트](https://github.com/PomeloFoundation)              |                      | [readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | MyCAT Server               | [Pomelo Foundation 프로젝트](https://github.com/PomeloFoundation)              | 시험판만      | [readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4.0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3.5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2.5 및 3.x       | [Jiří Činčura](https://github.com/cincuranet)                                 |                      | [docs](https://github.com/cincuranet/FirebirdSql.Data.FirebirdClient/blob/master/Provider/docs/entity-framework-core.md)                                                                           |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2.5 및 3.x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           |                      | [wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [MySQL 프로젝트](http://dev.mysql.com)(Oracle)                                |                      | [docs](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)                                                                                                         |
| [Oracle.EntityFrameworkCore](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/)                             | Oracle DB 11.2 이상     | [Oracle](https://www.oracle.com/technetwork/topics/dotnet/)                   | 시험판           | [웹 사이트](https://www.oracle.com/technetwork/topics/dotnet/)                                                                                                                                       |
| [IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2, Informix              | [IBM](https://ibm.com)                                                        | Windows 버전      | [블로그](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Linux 버전        | [블로그](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-osx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | macOS 버전        | [블로그](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Microsoft Access 파일     | [Bubi](https://github.com/bubibubi)                                           | .NET Framework       | [readme](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |
| [EntityFrameworkCore.OpenEdge](https://www.nuget.org/packages/EntityFrameworkCore.OpenEdge/)                         | Progress OpenEdge          | [Alex Wiese](https://github.com/alexwiese)                                    |                      | [readme](https://github.com/alexwiese/EntityFrameworkCore.OpenEdge/blob/master/README.md)                                                                                                          |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle DB 9.2.0.4 이상  | [DevArt](https://www.devart.com/)                                             | 지급                 | [docs](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8.0 이상     | [DevArt](https://www.devart.com/)                                             | 지급                 | [docs](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3 이상           | [DevArt](https://www.devart.com/)                                             | 지급                 | [docs](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5 이상            | [DevArt](https://www.devart.com/)                                             | 지급                 | [docs](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |

## <a name="adding-a-database-provider-to-your-application"></a>애플리케이션에 데이터베이스 공급자 추가

대부분의 EF Core용 데이터베이스 공급자는 NuGet 패키지로 배포되며 다음과 같이 설치할 수 있습니다.

# <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

``` console
dotnet add package provider_package_name
```

# <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
install-package provider_package_name
```

***

기능이 설치되면 종속성 주입 컨테이너를 사용하는 경우 `DbContext`, `OnConfiguring` 메서드 또는 `AddDbContext` 메서드 중 하나에서 공급자를 구성합니다.
예를 들어 다음 줄에서는 전달된 연결 문자열을 사용하여 SQL Server 공급자를 구성합니다.

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

데이터베이스 공급자는 EF Core를 확장하여 특정 데이터베이스에 고유한 기능을 사용할 수 있습니다.
개념은 상당 부분 대부분의 데이터베이스에서 공통이며 기본 EF Core 구성 요소에 포함됩니다.
이러한 개념에는 LINQ에서의 쿼리 표현, 트랜잭션, 데이터베이스에서 로드된 후 개체에 대한 변경 내용 추적 등이 포함됩니다.
일부 개념은 특정 공급자에게만 적용됩니다.
예를 들어 SQL Server 공급자에서는 [메모리 최적화 테이블을 구성](xref:core/providers/sql-server/memory-optimized-tables)할 수 있습니다(SQL Server 특정 기능).
다른 개념은 공급자 클래스에 특정합니다.
예를 들어 관계형 데이터베이스에 대한 EF Core 공급자는 공통 `Microsoft.EntityFrameworkCore.Relational` 라이브러리 위에 작성되며 테이블 및 열 매핑, 외부 키 제약 조건 등을 구성하기 위한 API를 제공합니다. 일반적으로 공급자는 NuGet 패키지로 배포됩니다.

> [!IMPORTANT]  
> EF Core의 새 패치 버전이 릴리스되면 `Microsoft.EntityFrameworkCore.Relational` 패키지에 대한 업데이트도 포함됩니다.
> 관계형 데이터베이스 공급자를 추가하면 이 패키지는 애플리케이션의 전이적 종속성으로 전환됩니다.
> 하지만 대부분의 공급자는 EF Core와 독립적으로 릴리스되고 해당 패키지의 최신 패치 버전에 따라 달라지도록 업데이트되지 않을 수 있습니다.
> 모든 버그 수정을 가져오기 위해 `Microsoft.EntityFrameworkCore.Relational`의 패치 버전을 애플리케이션의 직접 종속성으로 추가하는 것이 좋습니다.
