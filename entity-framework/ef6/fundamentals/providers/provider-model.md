---
title: Entity Framework 6 공급자 모델-EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 066832F0-D51B-4655-8BE7-C983C557E0E4
ms.openlocfilehash: 8bda3f51e8934f2add862c30e60f1185f068c515
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181605"
---
# <a name="the-entity-framework-6-provider-model"></a>Entity Framework 6 공급자 모델

Entity Framework 공급자 모델을 사용 하면 다양 한 유형의 데이터베이스 서버에서 Entity Framework를 사용할 수 있습니다. 예를 들어, 한 공급자가 Microsoft SQL Server에 대해 EF를 사용할 수 있도록 하는 동시에 다른 공급자를 연결 하 여 EF가 Microsoft SQL Server Compact 버전에 대해 사용 되도록 할 수 있습니다. 알고 있는 EF6의 공급자는 [Entity Framework 공급자](~/ef6/fundamentals/providers/index.md) 페이지에서 찾을 수 있습니다.

Ef가 오픈 소스 라이선스에서 EF를 해제할 수 있도록 EF가 공급자와 상호 작용 하는 방식에 대 한 특정 변경이 필요 했습니다. 이러한 변경을 위해서는 공급자를 등록 하기 위한 새 메커니즘과 함께 EF6 어셈블리에 대해 EF 공급자를 다시 빌드해야 합니다.

## <a name="rebuilding"></a>재구성

EF6를 사용 하 여 이전에 .NET Framework의 일부인 핵심 코드는 이제 OOB (대역 외) 어셈블리로 제공 됩니다. EF6에 대해 응용 프로그램을 빌드하는 방법에 대 한 자세한 내용은 [EF6 응용 프로그램 업데이트](~/ef6/what-is-new/upgrading-to-ef6.md) 페이지에서 찾을 수 있습니다. 이러한 지침을 사용 하 여 공급자를 다시 작성 해야 합니다.

## <a name="provider-types-overview"></a>공급자 유형 개요

EF 공급자는 이러한 서비스가 확장 (기본 클래스의 경우) 또는 구현 (인터페이스의 경우) 하는 CLR 형식에 의해 정의 된 공급자별 서비스의 컬렉션입니다. 이러한 서비스 중 두 가지는 기본 이며 EF가 작동 하는 데 필요 합니다. 다른 항목은 선택 사항이 며 특정 기능이 필요한 경우에만 구현 하면 되 고, 해당 서비스의 기본 구현이 대상으로 지정 되는 특정 데이터베이스 서버에 대해서는 작동 하지 않습니다.

## <a name="fundamental-provider-types"></a>기본 공급자 유형

### <a name="dbproviderfactory"></a>DbProviderFactory

EF는 모든 하위 수준 데이터베이스 액세스를 수행 하기 위해 [DbProviderFactory](https://msdn.microsoft.com/library/system.data.common.dbproviderfactory.aspx) 에서 파생 된 형식을 사용 하는 방법에 따라 달라 집니다. DbProviderFactory는 실제로 EF의 일부가 아니라 EF, 다른 O/RMs에서 사용 하거나 응용 프로그램에서 직접 사용 하 여 연결, 명령, 매개 변수 및 인스턴스를 가져올 수 있는 ADO.NET 공급자의 진입점을 제공 하는 .NET Framework의 클래스입니다. 다른 ADO.NET 추상화는 공급자를 독립적으로 제공 합니다. DbProviderFactory에 대 한 자세한 내용은 [MSDN 설명서 (ADO.NET](https://msdn.microsoft.com/library/a6cd7c08.aspx))를 참조 하세요.

### <a name="dbproviderservices"></a>DbProviderServices

EF는 ADO.NET 공급자가 이미 제공 하는 기능 위에 EF에서 필요한 추가 기능을 제공 하기 위해 DbProviderServices에서 파생 된 형식을 포함 하는 것에 의존 합니다. 이전 버전의 EF에서 DbProviderServices 클래스는 .NET Framework의 일부 이며 System.web 네임 스페이스에서 찾을 수 있었습니다. EF6부터이 클래스는 이제 EntityFramework .dll의 일부 이며,는 System.object. Common 네임 스페이스에 있습니다.

DbProviderServices 구현의 기본 기능에 대 한 자세한 내용은 [MSDN](https://msdn.microsoft.com/library/ee789835.aspx)에서 찾을 수 있습니다. 그러나이 정보를 작성 하는 시간은 EF6에 대해 업데이트 되지 않지만 대부분의 개념은 여전히 유효 합니다. DbProviderServices의 SQL Server 및 SQL Server Compact 구현은 [오픈 소스 코드 베이스](https://github.com/aspnet/EntityFramework6/) 에도 체크 인하고 다른 구현에 대 한 유용한 참조로 사용할 수 있습니다.

이전 버전의 EF에서 사용할 DbProviderServices 구현은 ADO.NET 공급자에서 직접 가져온 것입니다. 이 작업은 DbProviderFactory를 IServiceProvider로 캐스팅 하 고 GetService 메서드를 호출 하 여 수행 되었습니다. 이는 EF 공급자와 DbProviderFactory를 긴밀 하 게 결합 합니다. 이러한 결합은 EF를 .NET Framework 외부로 이동 하는 것을 차단 했기 때문에이 밀접 한 결합이 제거 되었고 DbProviderServices 구현이 응용 프로그램의 구성 파일 또는 코드 기반에 직접 등록 되어 있습니다. 아래의 _DbProviderServices 등록_ 섹션에 자세히 설명 된 대로 구성 합니다.

## <a name="additional-services"></a>추가 서비스

위에 설명 된 기본 서비스 외에도 EF에서 사용 되는 다른 많은 서비스 (always 또는 공급자별 경우)도 있습니다. 이러한 서비스의 기본 공급자별 구현은 DbProviderServices 구현에서 제공 될 수 있습니다. 응용 프로그램은 이러한 서비스의 구현을 재정의 하거나 DbProviderServices 형식이 기본값을 제공 하지 않는 경우 구현을 제공할 수도 있습니다. 이 내용은 아래의 _추가 서비스 확인_ 섹션에 자세히 설명 되어 있습니다.

공급자에 게 제공 될 수 있는 공급자의 추가 서비스 유형은 아래에 나열 되어 있습니다. 이러한 각 서비스 유형에 대 한 자세한 내용은 API 설명서에서 찾을 수 있습니다.

### <a name="idbexecutionstrategy"></a>IDbExecutionStrategy

이는 데이터베이스에 대해 쿼리 및 명령이 실행 될 때 공급자가 재시도 또는 기타 동작을 구현할 수 있도록 하는 선택적 서비스입니다. 구현을 제공 하지 않으면 EF는 단순히 명령을 실행 하 고 throw 된 모든 예외를 전파 합니다. SQL Server이 서비스는 SQL Azure와 같은 클라우드 기반 데이터베이스 서버에 대해 실행 하는 경우에 특히 유용한 재시도 정책을 제공 하는 데 사용 됩니다.

### <a name="idbconnectionfactory"></a>IDbConnectionFactory

이는 공급자가 데이터베이스 이름만 지정 하는 경우 규칙에 따라 DbConnection 개체를 만들 수 있도록 하는 선택적 서비스입니다. DbProviderServices 구현에서이 서비스를 확인할 수 있지만이 서비스는 EF 4.1 및 구성 파일 또는 코드에서 명시적으로 설정 될 수도 있습니다. 공급자는 기본 공급자 (아래 _기본 공급자_ 참조)로 등록 된 경우 및 기본 연결 팩터리가 다른 곳에서 설정 되지 않은 경우에만이 서비스를 해결할 수 있습니다.

### <a name="dbspatialservices"></a>DbSpatialServices

공급자가 지리 및 기 하 도형 공간 형식에 대 한 지원을 추가할 수 있도록 하는 선택적 서비스입니다. 응용 프로그램에서 공간 형식이 포함 된 EF를 사용 하려면이 서비스의 구현을 제공 해야 합니다. DbSptialServices는 두 가지 방법으로에 대 한 요청을 받습니다. 먼저 공급자 특정 공간 서비스는 고정 이름 및 매니페스트 토큰을 포함 하는 DbProviderInfo 개체를 키로 사용 하 여 요청 됩니다. 둘째, DbSpatialServices는 키 없이에 대 한 요청을 받을 수 있습니다. 이는 독립 실행형 DbGeography 또는 DbGeometry 형식을 만들 때 사용 되는 "전역 공간 공급자"를 확인 하는 데 사용 됩니다.

### <a name="migrationsqlgenerator"></a>MigrationSqlGenerator

이는 Code First 하 여 데이터베이스 스키마를 만들고 수정 하는 데 사용 되는 SQL 생성에 EF 마이그레이션을 사용할 수 있도록 하는 선택적 서비스입니다. 마이그레이션은 마이그레이션을 지원 하기 위해 필요 합니다. 구현이 제공 되는 경우 데이터베이스 이니셜라이저 또는 Database. Create 메서드를 사용 하 여 데이터베이스를 만들 때에도 사용 됩니다.

### <a name="funcdbconnection-string-historycontextfactory"></a>Func < DbConnection, string, HistoryContextFactory >

이는 공급자가 EF 마이그레이션에 사용 되는 `__MigrationHistory` 테이블에 HistoryContext 매핑을 구성할 수 있도록 하는 선택적 서비스입니다. HistoryContext는 Code First DbContext, 일반 흐름 API를 사용 하 여 테이블 이름 및 열 매핑 사양과 같은 항목을 변경 하 여 구성할 수 있습니다. 모든 공급자에 대해 EF에서 반환 된이 서비스의 기본 구현은 모든 기본 테이블 및 열 매핑이 해당 공급자에 의해 지원 되는 경우 지정 된 데이터베이스 서버에 대해 작동할 수 있습니다. 이 경우 공급자는이 서비스의 구현을 제공할 필요가 없습니다.

### <a name="idbproviderfactoryresolver"></a>IDbProviderFactoryResolver

이는 지정 된 DbConnection 개체에서 올바른 DbProviderFactory를 가져오기 위한 선택적 서비스입니다. 모든 공급자에 대해 EF에서 반환 된이 서비스의 기본 구현은 모든 공급자에 대해 작동 하기 위한 것입니다. 그러나 .NET 4에서 실행 되는 경우 DbProviderFactory는 DbConnections에서 공개적으로 액세스할 수 없습니다. 따라서 EF는 일부 추론을 사용 하 여 등록 된 공급자를 검색 하 고 일치 하는 항목을 찾습니다. 일부 공급자의 경우 이러한 추론에 실패할 수 있으며,이 경우 공급자는 새로운 구현을 제공 해야 합니다.

## <a name="registering-dbproviderservices"></a>DbProviderServices 등록

사용할 DbProviderServices 구현은 응용 프로그램의 구성 파일 (app.config 또는 web.config)에서 또는 코드 기반 구성을 사용 하 여 등록할 수 있습니다. 두 경우 모두 등록에서는 공급자의 "고정 이름"을 키로 사용 합니다. 이를 통해 단일 응용 프로그램에서 여러 공급자를 등록 하 고 사용할 수 있습니다. EF 등록에 사용 되는 고정 이름은 ADO.NET 공급자 등록 및 연결 문자열에 사용 되는 고정 이름과 동일 합니다. 예를 들어 SQL Server 고정 이름 "System.object"가 사용 됩니다.

### <a name="config-file-registration"></a>구성 파일 등록

사용할 DbProviderServices 형식은 응용 프로그램의 구성 파일에 있는 entityFramework 섹션의 공급자 목록에 공급자 요소로 등록 됩니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

``` xml
<entityFramework>
  <providers>
    <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
  </providers>
</entityFramework>
```

_형식_ 문자열은 사용할 DbProviderServices 구현의 정규화 된 어셈블리 형식 이름 이어야 합니다.

### <a name="code-based-registration"></a>코드 기반 등록

EF6 공급자부터 코드를 사용 하 여 등록할 수도 있습니다. 이를 통해 응용 프로그램의 구성 파일을 변경 하지 않고 EF 공급자를 사용할 수 있습니다. 코드 기반 구성을 사용 하려면 응용 프로그램이 [코드 기반 구성 설명서](https://msdn.com/data/jj680699)에 설명 된 대로 dbconfiguration 클래스를 만들어야 합니다. 그러면 DbConfiguration 클래스의 생성자가 SetProviderServices를 호출 하 여 EF 공급자를 등록 해야 합니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetProviderServices("My.New.Provider", new MyProviderServices());
    }
}
```

## <a name="resolving-additional-services"></a>추가 서비스 확인

위에서 설명한 것 처럼 _공급자 유형 개요_ 섹션에서 DbProviderServices 클래스를 사용 하 여 추가 서비스를 확인할 수도 있습니다. 이는 DbProviderServices에서 IDbDependencyResolver을 구현 하 고 등록 된 각 DbProviderServices 형식이 "default 확인자"로 추가 되었기 때문에 가능 합니다. IDbDpendencyResolver 메커니즘은 [종속성 확인](~/ef6/fundamentals/configuring/dependency-resolution.md)에 자세히 설명 되어 있습니다. 그러나이 사양의 모든 개념을 이해 하 여 공급자의 추가 서비스를 해결 해야 하는 것은 아닙니다.

공급자가 추가 서비스를 확인 하는 가장 일반적인 방법은 DbProviderServices 클래스의 생성자에서 각 서비스에 대해 AddDependencyResolver를 호출 하는 것입니다. 예를 들어 SqlProviderServices (SQL Server 용 EF 공급자)는 초기화를 위해 다음과 유사한 코드를 포함 합니다.

``` csharp
private SqlProviderServices()
{
    AddDependencyResolver(new SingletonDependencyResolver<IDbConnectionFactory>(
        new SqlConnectionFactory()));

    AddDependencyResolver(new ExecutionStrategyResolver<DefaultSqlExecutionStrategy>(
        "System.data.SqlClient", null, () => new DefaultSqlExecutionStrategy()));

    AddDependencyResolver(new SingletonDependencyResolver<Func<MigrationSqlGenerator>>(
        () => new SqlServerMigrationSqlGenerator(), "System.data.SqlClient"));

    AddDependencyResolver(new SingletonDependencyResolver<DbSpatialServices>(
        SqlSpatialServices.Instance,
        k =>
        {
            var asSpatialKey = k as DbProviderInfo;
            return asSpatialKey == null
                || asSpatialKey.ProviderInvariantName == ProviderInvariantName;
        }));
}
```

이 생성자는 다음과 같은 도우미 클래스를 사용 합니다.

*   SingletonDependencyResolver: GetService가 호출 될 때마다 동일한 인스턴스가 반환 되는 서비스를 비롯 한 Singleton 서비스를 확인 하는 간단한 방법을 제공 합니다. 임시 서비스는 일반적으로 요청 시 임시 인스턴스를 만드는 데 사용 되는 단일 팩터리로 등록 됩니다.
*   ExecutionStrategyResolver: IExecutionStrategy 구현을 반환 하는 것과 관련 된 확인자입니다.

AddDependencyResolver를 사용 하는 대신 Dbproviderservices를 재정의 하 고 추가 서비스를 직접 해결할 수도 있습니다. EF가 특정 형식에 의해 정의 된 서비스를 필요로 하는 경우, 경우에 따라 지정 된 키에 대해이 메서드를 호출 합니다. 메서드가 가능 하면 서비스를 반환 하거나, 서비스 반환을 옵트아웃 하기 위해 null을 반환 하 고, 대신 다른 클래스에서 해당 서비스를 확인할 수 있도록 허용 해야 합니다. 예를 들어 기본 연결 팩터리를 해결 하려면 GetService의 코드가 다음과 같이 표시 될 수 있습니다.

``` csharp
public override object GetService(Type type, object key)
{
    if (type == typeof(IDbConnectionFactory))
    {
        return new SqlConnectionFactory();
    }
    return null;
}
```

### <a name="registration-order"></a>등록 순서

여러 DbProviderServices 구현이 응용 프로그램의 구성 파일에 등록 되 면 나열 된 순서 대로 보조 해결 프로그램으로 추가 됩니다. 확인자는 항상 보조 확인자 체인의 맨 위에 추가 되기 때문에 목록의 끝에 있는 공급자는 다른 사용자 보다 먼저 종속성을 확인할 수 있음을 의미 합니다. 이는 처음에는 간단 하 게 직관적이 지 않지만, 각 공급자를 목록에서 가져와 기존 공급자의 위에 누적 하는 경우에는 적합 합니다.

대부분의 공급자 서비스는 공급자별로 제공 되며 공급자 고정 이름으로 키를 지정 하기 때문에 일반적으로이 순서는 중요 하지 않습니다. 그러나 공급자 고정 이름 또는 다른 공급자별 키로 키가 지정 되지 않은 서비스의 경우이 순서에 따라 서비스가 확인 됩니다. 예를 들어, 다른 위치에서 명시적으로 다르게 설정 되지 않은 경우 기본 연결 팩터리는 체인의 최상위 공급자에서 제공 됩니다.

## <a name="additional-config-file-registrations"></a>추가 구성 파일 등록

위에 설명 된 추가 공급자 서비스 중 일부를 응용 프로그램의 구성 파일에 직접 등록할 수 있습니다. 이 작업이 완료 되 면 DbProviderServices 구현의 GetService 메서드에서 반환 하는 것 대신 구성 파일의 등록이 사용 됩니다.

### <a name="registering-the-default-connection-factory"></a>기본 연결 팩터리 등록

EF5부터 EntityFramework NuGet 패키지는 구성 파일에 SQL Express 연결 팩터리 또는 LocalDb 연결 팩터리를 자동으로 등록 합니다.

예를 들어 다음과 같은 가치를 제공해야 합니다.

``` xml
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework" >
</entityFramework>
```

_형식은_ 기본 연결 팩터리의 IDbConnectionFactory을 구현 해야 하는 어셈블리의 정규화 된 형식 이름입니다.

공급자 NuGet 패키지는 설치 시 기본 연결 팩터리를이 방식으로 설정 하는 것이 좋습니다. 아래 _공급자에 대 한 NuGet 패키지를_ 참조 하십시오.

## <a name="additional-ef6-provider-changes"></a>추가 EF6 공급자 변경 내용

### <a name="spatial-provider-changes"></a>공간 공급자 변경

공간 형식을 지 원하는 공급자는 이제 DbSpatialDataReader에서 파생 되는 클래스에 대 한 몇 가지 추가 메서드를 구현 해야 합니다.

*   `public abstract bool IsGeographyColumn(int ordinal)`
*   `public abstract bool IsGeometryColumn(int ordinal)`

동기 메서드에 대 한 기본 구현 대리자로 재정의 하는 것이 좋으며 비동기 방식으로 실행 되지 않는 기존 메서드의 새로운 비동기 버전도 있습니다.

*   `public virtual Task<DbGeography> GetGeographyAsync(int ordinal, CancellationToken cancellationToken)`
*   `public virtual Task<DbGeometry> GetGeometryAsync(int ordinal, CancellationToken cancellationToken)`

### <a name="native-support-for-enumerablecontains"></a>Enumerable. Contains에 대 한 기본 지원

EF6에는 LINQ 쿼리에서 열거 가능. Contains를 사용 하는 경우 성능 문제를 해결 하기 위해 추가 된 새 식 형식 (새 식 형식)이 도입 되었습니다. DbProviderManifest 클래스에는 공급자가 새 식 형식을 처리 하는지 확인 하기 위해 EF에 의해 호출 되는 새 가상 메서드인 SupportsInExpression이 있습니다. 기존 공급자 구현과의 호환성을 위해 메서드에서 false를 반환 합니다. 이러한 개선을 활용 하기 위해 EF6 공급자는 코드를 추가 하 여 DbInExpression을 처리 하 고 SupportsInExpression을 재정의 하 여 true를 반환할 수 있습니다. DbExpressionBuilder.In 메서드를 호출 하 여 dbinexpression의 인스턴스를 만들 수 있습니다. DbInExpression 인스턴스는 일반적으로 테이블 열을 나타내는 DbExpression과 일치 항목을 테스트할 DbConstantExpression의 목록으로 구성 됩니다.

## <a name="nuget-packages-for-providers"></a>공급자에 대 한 NuGet 패키지

EF6 공급자를 사용할 수 있도록 하는 한 가지 방법은 NuGet 패키지로 릴리스 하는 것입니다. NuGet 패키지를 사용 하면 다음과 같은 이점이 있습니다.

*   NuGet을 사용 하 여 응용 프로그램의 구성 파일에 공급자 등록을 추가 하는 것이 쉽습니다.
*   기본 연결 팩터리를 설정 하기 위해 구성 파일에 대 한 추가 변경 작업을 수행할 수 있습니다. 그러면 규칙에 따라 만든 연결에서 등록 된 공급자를 사용할 수 있습니다.
*   NuGet은 EF6 공급자가 새 EF 패키지를 릴리스한 후에도 계속 작동 하도록 바인딩 리디렉션 추가를 처리 합니다.

이에 대 한 예는 [오픈 소스 코드 베이스](https://github.com/aspnet/entityframework6)에 포함 되는 entityframework 패키지입니다. 이 패키지는 EF 공급자 NuGet 패키지를 만들기 위한 좋은 템플릿을 제공 합니다.

### <a name="powershell-commands"></a>PowerShell 명령

EntityFramework NuGet 패키지가 설치 되 면 공급자 패키지에 매우 유용한 두 개의 명령이 포함 된 PowerShell 모듈을 등록 합니다.

*   추가-EFProvider는 대상 프로젝트의 구성 파일에 공급자에 대 한 새 엔터티를 추가 하 고 등록 된 공급자 목록의 끝에 있습니다.
*   -EFDefaultConnectionFactory는 대상 프로젝트의 구성 파일에서 defaultConnectionFactory 등록을 추가 하거나 업데이트 합니다.

이러한 두 명령은 모두 구성 파일에 entityFramework 섹션을 추가 하 고 필요한 경우 공급자 컬렉션을 추가 합니다.

이러한 명령은 install. ps1 NuGet 스크립트에서 호출 됩니다. 예를 들어 SQL Compact 공급자에 대 한 install. p s 1은 다음과 유사 합니다.

``` powershell
param($installPath, $toolsPath, $package, $project)
Add-EFDefaultConnectionFactory $project 'System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework' -ConstructorArguments 'System.Data.SqlServerCe.4.0'
Add-EFProvider $project 'System.Data.SqlServerCe.4.0' 'System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact'</pre>
```

이러한 명령에 대 한 자세한 내용은 패키지 관리자 콘솔 창에서 get-help를 사용 하 여 얻을 수 있습니다.

## <a name="wrapping-providers"></a>래핑 공급자

래핑 공급자는 기존 공급자를 래핑하여 프로 파일링 또는 추적 기능과 같은 다른 기능으로 확장 하는 EF 및/또는 ADO.NET 공급자입니다. 래핑 공급자는 일반적인 방식으로 등록 될 수 있지만 공급자 관련 서비스의 해상도를 가로채 여 런타임에 래핑 공급자를 설정 하는 것이 더 편리한 경우가 많습니다. 이 작업을 수행 하는 데 DbConfiguration 클래스의 정적 이벤트 OnLockingConfiguration를 사용할 수 있습니다.

EF는 응용 프로그램 도메인에 대 한 모든 EF 구성이에서 얻을 수 있는 위치를 확인 한 후에 사용할 수 있도록 잠금 상태를 확인 한 후에 호출 됩니다. EF를 사용 하기 전에 앱을 시작할 때 앱은이 이벤트에 대 한 이벤트 처리기를 등록 해야 합니다. 이 처리기를 구성 파일에 등록 하기 위한 지원을 추가 하는 것을 고려 하 고 있지만 아직 지원 되지 않습니다. 그런 다음 이벤트 처리기는 래핑해야 하는 모든 서비스에 대해 ReplaceService를 호출 해야 합니다.  

예를 들어 IDbConnectionFactory 및 DbProviderService로 래핑하려면 다음과 같은 처리기를 등록 해야 합니다.

``` csharp
DbConfiguration.OnLockingConfiguration +=
    (_, a) =>
    {
        a.ReplaceService<DbProviderServices>(
            (s, k) => new MyWrappedProviderServices(s));

        a.ReplaceService<IDbConnectionFactory>(
            (s, k) => new MyWrappedConnectionFactory(s));
    };
```

해결 되었으며 이제 서비스를 확인 하는 데 사용 된 키와 함께 래핑해야 하는 서비스는 처리기에 전달 됩니다. 처리기는이 서비스를 래핑하고 반환 된 서비스를 래핑된 버전으로 바꿀 수 있습니다.

## <a name="resolving-a-dbproviderfactory-with-ef"></a>EF를 사용 하 여 DbProviderFactory 확인

DbProviderFactory는 위의 _공급자 유형 개요_ 섹션에 설명 된 대로 EF에 필요한 기본 공급자 유형 중 하나입니다. 이미 언급 했 듯이 EF 형식은 아니지만 등록은 일반적으로 EF 구성의 일부가 아니지만 machine.config 파일 및/또는 응용 프로그램의 구성 파일에 일반적인 ADO.NET 공급자를 등록 하는 것입니다.

이 EF는 사용할 DbProviderFactory를 찾을 때 여전히 일반 종속성 확인 메커니즘을 사용 하 고 있습니다. 기본 해결 프로그램은 구성 파일에서 일반 ADO.NET 등록을 사용 하므로 일반적으로 투명 합니다. 하지만 일반적인 종속성 확인 메커니즘으로 인해 정상적인 ADO.NET 등록이 수행 되지 않은 경우에도 IDbDependencyResolver를 사용 하 여 DbProviderFactory를 해결할 수 있습니다.

이러한 방식으로 DbProviderFactory를 확인 하는 데는 여러 가지 의미가 있습니다.

*   코드 기반 구성을 사용 하는 응용 프로그램은 DbConfiguration 클래스에 호출을 추가 하 여 적절 한 DbProviderFactory를 등록할 수 있습니다. 이는 파일 기반 구성을 전혀 사용 하지 않거나 사용 하지 않으려는 응용 프로그램에 특히 유용 합니다.
*   위의 _래핑 공급자_ 섹션에 설명 된 대로 ReplaceService를 사용 하 여 서비스를 래핑 또는 바꿀 수 있습니다.
*   이론적으로 DbProviderServices 구현에서는 DbProviderFactory를 해결할 수 있습니다.

이러한 작업을 수행 하는 것에 대 한 중요 한 점은 EF에서 DbProviderFactory을 조회 하는 경우에만 영향을 줍니다. 다른 비 EF 코드는 ADO.NET 공급자를 정상적으로 등록 하는 것으로 간주 하 고 등록을 찾을 수 없는 경우 실패할 수 있습니다. 따라서 일반적으로 DbProviderFactory를 일반적인 ADO.NET 방식으로 등록 하는 것이 더 좋습니다.

### <a name="related-services"></a>관련 서비스

EF를 사용 하 여 DbProviderFactory를 확인 하는 경우에도 IProviderInvariantName 및 IDbProviderFactoryResolver 서비스를 확인 해야 합니다.

IProviderInvariantName는 지정 된 유형의 DbProviderFactory에 대 한 공급자 고정 이름을 결정 하는 데 사용 되는 서비스입니다. 이 서비스의 기본 구현에서는 ADO.NET 공급자 등록을 사용 합니다. 즉, EF에 의해 DbProviderFactory가 확인 되 고 있기 때문에 ADO.NET 공급자가 일반적인 방식으로 등록 되지 않은 경우이 서비스를 해결 해야 합니다. 이 서비스에 대 한 해결 프로그램은 DbConfiguration 메서드를 사용할 때 자동으로 추가 됩니다.

위의 _공급자 유형 개요_ 섹션에 설명 된 대로 IDbProviderFactoryResolver는 지정 된 DbConnection 개체에서 올바른 DbProviderFactory를 가져오는 데 사용 됩니다. .NET 4에서 실행 될 때이 서비스의 기본 구현에서는 ADO.NET 공급자 등록을 사용 합니다. 즉, EF에 의해 DbProviderFactory가 확인 되 고 있기 때문에 ADO.NET 공급자가 일반적인 방식으로 등록 되지 않은 경우이 서비스를 해결 해야 합니다.
