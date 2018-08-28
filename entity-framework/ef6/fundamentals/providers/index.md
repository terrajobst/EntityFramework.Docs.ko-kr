---
title: Entity Framework 공급자 - EF6
author: divega
ms.date: 2018-06-27
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
ms.openlocfilehash: 98dc34591e8b2724401810a13f363382dce53218
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997920"
---
# <a name="entity-framework-6-providers"></a>Entity Framework 6 공급자
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

현재 Entity Framework는 오픈 소스 라이선스 하에서 개발 중이며 EF6 이상은 .NET Framework의 일부로 제공되지 않을 것입니다. 이 방법은 여러 가지 장점이 있지만, EF6 어셈블리에 맞게 EF 공급자를 다시 빌드해야 합니다. 즉, EF5 이하의 EF 공급자는 다시 빌드될 때까지 EF6를 사용하지 않습니다.

## <a name="which-providers-are-available-for-ef6"></a>EF6에 어떤 공급자를 사용할 수 있나요?

EF6에 맞게 다시 빌드된 것으로 파악된 공급자는 다음과 같습니다.

*   **Microsoft SQL Server 공급자**
    *   [Entity Framework 오픈 소스 코드베이스](http://github.com/aspnet/EntityFramework6)로 빌드
    *   [EntityFramework NuGet 패키지](http://nuget.org/packages/EntityFramework)의 일부로 제공
*   **Microsoft SQL Server Compact Edition 공급자**
    *   [Entity Framework 오픈 소스 코드베이스](http://github.com/aspnet/EntityFramework6)로 빌드
    *   [EntityFramework.SqlServerCompact NuGet 패키지](http://nuget.org/packages/EntityFramework.SqlServerCompact)에 포함
*   [**Devart dotConnect Data 공급자**](http://www.devart.com/dotconnect/)
    *   Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 및 SQL Server를 포함하여 다양한 데이터베이스에 대한 [Devart](http://www.devart.com/)의 타사 공급자가 있습니다.
*   [**CData Software 공급자**](http://www.cdata.com/ado/)
    *   Salesforce, Azure Table Storage, MySql 등을 포함하여 다양한 데이터 저장소에 대한 [CData Software](http://www.cdata.com/ado/)의 타사 공급자가 있습니다.
*   **Firebird 공급자**
    *   [NuGet 패키지](http://www.nuget.org/packages/FirebirdSql.Data.FirebirdClient/)의 일부로 제공
*   **Visual Fox Pro 공급자**
    *   [NuGet 패키지](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)의 일부로 제공
*   **MySQL**
    *   [MySQL Connector/Net](http://dev.mysql.com/downloads/connector/net/)
*   **PostgreSQL**
    *   Npgsql은 [NuGet 패키지](http://www.nuget.org/packages/Npgsql.EF6/)의 일부로 제공
*   **Oracle**
    *   ODP.NET은 [NuGet 패키지](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)의 일부로 제공

이 목록에 포함된다는 것이 특정 공급자의 기능 또는 지원 수준을 나타내는 것은 아니며, EF6용 빌드를 사용할 수 있다는 것만 나타냅니다.

## <a name="registering-ef-providers"></a>EF 공급자 등록

Entity Framework 6부터 코드 기반 구성을 사용하여 또는 응용 프로그램의 구성 파일에서 EF 공급자를 등록할 수 있습니다.

### <a name="config-file-registration"></a>구성 파일 등록

app.config 또는 web.config에서 EF 공급자를 등록하는 형식은 다음과 같습니다.


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

EF 공급자가 NuGet에서 설치되는 경우 종종 NuGet 패키지가 자동으로 이 등록을 구성 파일에 추가합니다. 앱의 시작 프로젝트가 아닌 프로젝트에 NuGet 패키지를 설치하는 경우 등록을 시작 프로젝트의 구성 파일에 복사해야 할 수도 있습니다.

이 등록의 "invariantName"은 ADO.NET 공급자를 식별하는 데 사용되는 것과 동일한 고정 이름입니다. DbProviderFactories 등록의 "고정" 특성 및 연결 문자열 등록의 "providerName" 특성으로 찾을 수 있습니다. 사용할 고정 이름이 공급자에 대한 설명서에도 포함되어야 합니다. 고정 이름의 예로 SQL Server의 “System.Data.SqlClient” 및 SQL Server Compact의 “System.Data.SqlServerCe.4.0”을 들 수 있습니다.

이 등록의 "형식"은 "System.Data.Entity.Core.Common.DbProviderServices"에서 파생되는 공급자 형식의 정규화된 어셈블리 이름입니다. 예를 들어 SQL Compact에 사용할 문자열은 “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”입니다. 여기서 사용할 형식은 공급자에 대한 설명서에 포함되어야 합니다.

### <a name="code-based-registration"></a>코드 기반 등록

Entity Framework 6부터 EF에 대한 응용 프로그램 수준 구성을 코드에서 지정할 수 있습니다. 자세한 내용은 _[Entity Framework 코드 기반 구성](https://msdn.microsoft.com/en-us/data/jj680699)_ 을 참조하세요. 코드 기반 구성을 사용하여 EF 공급자를 등록하는 일반적인 방법은 System.Data.Entity.DbConfiguration에서 파생되는 새 클래스를 만들어서 DbContext 클래스와 동일한 어셈블리에 배치하는 것입니다. 그러면 DbConfiguration 클래스가 공급자를 생성자에 등록합니다. 예를 들어 SQL Compact 공급자를 등록하려는 경우 DbConfiguration 클래스는 다음과 비슷합니다.

``` csharp
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetProviderServices(
                SqlCeProviderServices.ProviderInvariantName,
                SqlCeProviderServices.Instance);
        }
    }
```

이 코드에서 "SqlCeProviderServices.ProviderInvariantName"은 SQL Server Compact 공급자 고정 이름 문자열(“System.Data.SqlServerCe.4.0”)에 유용하고 SqlCeProviderServices.Instance는 SQL Compact EF 공급자의 singleton 인스턴스를 반환합니다.

## <a name="what-if-the-provider-i-need-isnt-available"></a>필요한 공급자를 사용할 수 없으면 어떻게 되나요?

이전 버전의 EF에 대한 공급자를 사용할 수 있는 경우 공급자의 소유자에게 연락하여 EF6 버전을 만들어 달라고 요청하세요. [EF6 공급자 모델에 대한 설명서](~/ef6/fundamentals/providers/provider-model.md) 참조를 포함해야 합니다.

## <a name="can-i-write-a-provider-myself"></a>공급자를 직접 작성할 수 있나요?

EF 공급자를 직접 만들 수 있지만, 쉬운 일은 아닙니다. EF6 공급자 모델에 대한 위의 링크는 시작하기에 좋은 위치입니다. 또한 [EF 오픈 소스 코드베이스](https://github.com/aspnet/EntityFramework6)에 포함된 SQL Server 및 SQL CE 공급자에 대한 코드를 시작 지점으로 또는 참조로 사용하면 도움이 될 수 있습니다.

EF6를 시작하면 EF 공급자가 기본 ADO.NET 공급자와 덜 긴밀하게 결합됩니다. 따라서 ADO.NET 클래스를 작성 또는 래핑할 필요 없이 EF 공급자를 손쉽게 작성할 수 있습니다.
