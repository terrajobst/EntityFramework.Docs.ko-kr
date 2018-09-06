---
title: EF6-Entity Framework 6 공급자 모델을
author: divega
ms.date: 2018-06-27
ms.assetid: 066832F0-D51B-4655-8BE7-C983C557E0E4
ms.openlocfilehash: e8b0552ec083d8ab276aa9de109650f423160269
ms.sourcegitcommit: a81aed575372637997b18a0f9466d8fefb33350a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2018
ms.locfileid: "43821389"
---
# <a name="the-entity-framework-6-provider-model"></a>Entity Framework 6 공급자 모델

Entity Framework 공급자 모델에는 Entity Framework 다양 한 유형의 데이터베이스 서버와 함께 사용 될 수 있습니다. 예를 들어, EF EF Microsoft SQL Server Compact Edition에 대해 사용할 수 있도록 다른 공급자에 연결할 수 있습니다 하는 동안 Microsoft SQL Server에 대해 사용할 수 있도록 하나 이상의 공급자 연결할 수 있습니다. 알고 있는 EF6의 공급자에서 찾을 수 있습니다 합니다 [Entity Framework 공급자](~/ef6/fundamentals/providers/index.md) 페이지입니다.

EF EF는 오픈 소스 라이선스로 출시 될 수 있도록 공급자와 상호 작용 하는 방법은 특정 변경 했습니다. 이러한 변경 내용을 함께 공급자의 등록을 위한 새로운 메커니즘 EF6 어셈블리에 대해 EF 공급자를 다시 작성 해야 합니다.

## <a name="rebuilding"></a>다시 작성

EF6을 사용 하 여 아웃 외 (OOB) 어셈블리와 이전에.NET Framework의 일부인 핵심 코드가 이제 배송할 됩니다. EF6 응용 프로그램을 빌드하는 방법에 대 한 세부 정보에서 확인할 수 있습니다 합니다 [EF6 응용 프로그램 업데이트](~/ef6/what-is-new/upgrading-to-ef6.md) 페이지입니다. 공급자가 이러한 지침을 사용 하 여 다시 작성 해야 합니다.

## <a name="provider-types-overview"></a>공급자 형식 개요

EF 공급자로 실제로 이러한 서비스 (대 한 기본 클래스)에서 확장 하거나 (인터페이스)를 구현 하는 CLR 형식으로 정의 하는 공급자 관련 서비스의 컬렉션. 이러한 서비스의 두 가지 기본적인 및 EF 전혀 작동 하는 데 필요한입니다. 다른 선택 사항이 며 특정 기능은 필수 및/또는 대상으로 지정 되는 특정 데이터베이스 서버에 이러한 서비스의 기본 구현이 작동 하지 않습니다 하는 경우에 구현 해야 합니다.

### <a name="fundamental-provider-types"></a>기본 공급자 형식

#### <a name="dbproviderfactory"></a>DbProviderFactory

EF에서 파생 된 형식에 따라 달라 집니다 [System.Data.Common.DbProviderFactory](http://msdn.microsoft.com/en-us/library/system.data.common.dbproviderfactory.aspx) 기본적인 수준의 데이터베이스에 대 한 모든 액세스를 수행 합니다. DbProviderFactory EF에 실제로 포함 되지 않지만 대신 클래스는 ADO.NET 공급자에 대 한 진입점을 제공 하는.NET Framework에 사용할 수 있습니다 다른 O/RMs EF 또는 응용 프로그램에서 직접 연결, 명령, 매개 변수 인스턴스를 얻는 및 공급자의 다른 ADO.NET 추상화 알 수 없는 방식으로 합니다. DbProviderFactory에 대 한 자세한 정보는에서 찾을 수 합니다 [ADO.NET에 대 한 MSDN 설명서](http://msdn.microsoft.com/en-us/library/a6cd7c08.aspx)합니다.

#### <a name="dbproviderservices"></a>DbProviderServices

EF는 ADO.NET 공급자가 이미 제공 하는 기능을 기반으로 EF에 필요한 추가 기능을 제공 하는 데 DbProviderServices에서 파생 된 형식에 따라 달라 집니다. 이전 버전의 EF DbProviderServices 클래스의.NET Framework 구성 요소 였으며 System.Data.Common 네임 스페이스에서 찾을 수 있습니다. EF6을 사용 하 여이 클래스는 이제 EntityFramework.dll의 일부 시작한 System.Data.Entity.Core.Common 네임 스페이스.

DbProviderServices 구현의 기본 기능에 대 한 자세한 내용은에서 확인할 수 있습니다 [MSDN](http://msdn.microsoft.com/en-us/library/ee789835.aspx)합니다. 그러나이 정보를 작성 하는 시간을 기준으로 업데이트 되지 않았다는 EF6에 대 한 대부분의 개념은 여전히 유효 하지만 note 합니다. DbProviderServices의 SQL Server 및 SQL Server Compact 구현도 확인 됩니다에 [오픈 소스 코드 베이스](https://github.com/aspnet/EntityFramework6/) 및 다른 구현에 대 한 유용한 참조로 사용할 수 있습니다.

이전 버전의 EF DbProviderServices 구현은 사용 하는 ADO.NET 공급자에서 직접 가져온 합니다. 이 작업은 DbProviderFactory를 IServiceProvider로 캐스팅 하 고 GetService 메서드를 호출 하 여 수행 되었습니다. 이 DbProviderFactory를 EF 공급자 긴밀 하 게 결합 합니다. 이 결합에서.NET Framework 외부로 이동 되 고 EF를 차단 하 고 따라서 EF6에 대 한 밀접 한 결합이 제거 되었습니다 DbProviderServices 구현의 응용 프로그램의 구성 파일에서 직접 또는 코드 기반에 지금 등록 좀 더 자세히 설명 된 대로 구성 합니다 _DbProviderServices 등록_ 아래의 섹션입니다.

### <a name="additional-services"></a>추가 서비스

위에서 설명한 기본적인 서비스 외에도 많은 다른 서비스 EF에서 사용 하는 경우에 따라 또는 항상 공급자별도 있습니다. 이러한 서비스의 기본 공급자별 구현은 DbProviderServices 구현으로 제공할 수 있습니다. 응용 프로그램도 이러한 서비스의 구현 재정의 하거나 DbProviderServices 형식 기본값을 제공 하지 않는 경우에 구현할 수 있습니다. 자세히 설명 되어이 _추가 서비스 확인_ 아래의 섹션입니다.

공급자 공급자에 유용할 수 있는 추가 서비스 형식은 다음과 같습니다. 이러한 각 유형의 서비스에 대 한 자세한 내용은 API 설명서에서 찾을 수 있습니다.

#### <a name="idbexecutionstrategy"></a>IDbExecutionStrategy

이 쿼리와 명령을 데이터베이스에 대해 실행 될 때 다시 시도 하거나 다른 동작을 구현 하는 공급자 수 있는 선택적 서비스입니다. 구현이 제공 하는 경우 다음 EF 단순히 명령을 실행 하 여를 throw 된 예외를 전파 합니다. SQL Server에 대 한이 서비스는 SQL Azure 같은 클라우드 기반 데이터베이스 서버에 대해 실행할 때 특히 유용 하며 재시도 정책을 제공에 사용 됩니다.

#### <a name="idbconnectionfactory"></a>IDbConnectionFactory

이 공급자는 데이터베이스 이름만 지정 하는 경우 규칙에 따라 DbConnection 개체를 만들 수 있도록 하는 선택적 서비스입니다. 이 서비스 하는 동안 DbProviderServices 구현 EF 4.1 이후 제공 된 고도 설정할 수 있습니다 명시적으로 구성 파일 또는 코드에서 확인할 수 있는 참고 합니다. 공급자만을 기본 공급자로 등록 하는 경우이 서비스를 해결할 기회 받습니다 (참조 _기본 공급자_ 아래) 기본 연결 팩터리가 설정 되어 있지 않은 다른 곳에서 하는 경우.

#### <a name="dbspatialservices"></a>DbSpatialServices

Geography 및 geometry 공간 형식에 대 한 지원을 추가할 공급자를 허용 하는 선택적 서비스입니다. 이 서비스의 구현은 공간 형식을 사용 하 여 EF를 사용 하도록 응용 프로그램에 대 한 순서로 제공 되어야 합니다. DbSptialServices에 대 한 두 가지 방식에서 하 라는 메시지가 표시 됩니다. 첫째, 공급자별 공간 서비스 DbProviderInfo 개체를 사용 하 여 요청 된 (invariant를 포함 하는 이름 및 매니페스트 토큰) 키로 합니다. 둘째, 키가 없는 DbSpatialServices에 요청할 수 있습니다. 해결 "전역 공간 공급자" 독립 실행형 DbGeography 또는 DbGeometry 유형을 만들 때 사용 되는 데 사용 됩니다.

#### <a name="migrationsqlgenerator"></a>MigrationSqlGenerator

이것이 EF 마이그레이션을 만들고 데이터베이스 스키마 수정에 사용 되는 SQL 생성에 대 한 Code First에서 사용할 수 있는 선택적 서비스입니다. 구현을은 마이그레이션을 지원 하기 위해 필요 합니다. 구현을 제공 하는 경우 다음 것도 사용 됩니다 Database.Create 메서드나 데이터베이스 이니셜라이저를 사용 하 여 데이터베이스를 만들 때.

#### <a name="funcdbconnection-string-historycontextfactory"></a>Func < DbConnection, HistoryContextFactory 문자열 >

이 공급자를 HistoryContext의 매핑을 구성할 수 있도록 하는 선택적 서비스를 `__MigrationHistory` EF 마이그레이션을 사용 하는 테이블입니다. HistoryContext 코드 첫 번째 DbContext 되며 테이블 및 열 매핑 사양 이름과 같은 항목을 변경 하려면 일반 흐름 API를 사용 하 여 구성할 수 있습니다. 해당 공급자에서 모든 기본 테이블 및 열 매핑을 지 원하는 경우 지정 된 데이터베이스 서버에 대 한 모든 공급자에 대 한 EF를 반환한이 서비스의 기본 구현을 작동할 수 있습니다. 이 경우 공급자는이 서비스의 구현을 제공 필요가 없습니다.

#### <a name="idbproviderfactoryresolver"></a>IDbProviderFactoryResolver

이것이 지정된 DbConnection 개체에서 올바른 dbproviderfactory에 대 한 선택적 서비스입니다. 모든 공급자에 대 한 EF를 반환한이 서비스의 기본 구현은 모든 공급자에 대해 작동 됩니다. 그러나.NET 4를 실행 하는 경우는 DbProviderFactory 공개적으로 액세스할 수 없는 경우 1에서 해당 DbConnections 합니다. 따라서 EF 일부 추론을 사용 하 여 일치 하는 것에 등록된 된 공급자를 검색 합니다. 일부 공급자에 대 한 이러한 추론이 실패 하 고 이러한 상황에서는 공급자에는 새 구현을 제공 해야는 가능성이 있습니다.

## <a name="registering-dbproviderservices"></a>DbProviderServices 등록

DbProviderServices 구현은 사용 하는 응용 프로그램의 구성 파일 (app.config 또는 web.config) 또는 코드 기반 구성을 사용 하 여 등록할 수 있습니다. 두 경우 모두 등록 공급자의 "고정 이름" 키로 사용합니다. 따라서 여러 공급자를를 등록 하 고 단일 응용 프로그램에서 사용할 수 있습니다. 고정 이름 EF 등록에 사용 되는 ADO.NET 공급자 등록 및 연결 문자열에 사용 되는 고정 이름을와 같습니다. 예를 들어, SQL Server 고정 이름 "System.Data.SqlClient" 사용 됩니다.

### <a name="config-file-registration"></a>구성 파일 등록

DbProviderServices 형식은 사용 하는 응용 프로그램의 구성 파일의 entityFramework 섹션의 공급자 목록에서 요소를으로 등록 됩니다. 예를 들어:

``` xml
<entityFramework>
  <providers>
    <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
  </providers>
</entityFramework>
```

합니다 _형식_ 문자열 사용할 DbProviderServices 구현의 정규화 된 어셈블리 형식 이름 이어야 합니다.

### <a name="code-based-registration"></a>코드 기반 등록

EF6 공급자를 사용 하 여 시작 수 등록 코드를 사용 하 여. 이렇게 하면 EF 공급자를 응용 프로그램의 구성 파일을 변경 하지 않고 사용할 수 있습니다. 코드 기반 구성을 사용 하려면 응용 프로그램 클래스를 만들어야 DbConfiguration에 설명 된 대로 합니다 [코드 기반 구성 설명서](http://msdn.com/data/jj680699)합니다. DbConfiguration 클래스의 생성자 EF 공급자를 등록 하는 SetProviderServices 호출 해야 합니다. 예를 들어:

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

위에서 설명한 것 처럼 합니다 _공급자 형식 개요_ 섹션을 DbProviderServices 클래스 추가 서비스 확인에 사용할 수도 있습니다. DbProviderServices IDbDependencyResolver 구현 및 등록 된 각 DbProviderServices 형식 "기본 해결 프로그램"으로 추가 됩니다 때문에 이것이 가능 합니다. IDbDpendencyResolver 메커니즘에서 자세히 설명 되어 [종속성 확인](~/ef6/fundamentals/configuring/dependency-resolution.md)합니다. 그러나 추가 서비스 공급자를 해결 하려면이 사양에서 모든 개념을 이해 하는 데 필요한 아닙니다.

추가 서비스를 확인 하는 공급자에 대 한 가장 일반적인 방법은 DbProviderServices 클래스의 생성자에서 각 서비스에 대 한 DbProviderServices.AddDependencyResolver를 호출 하는 것입니다. 예를 들어 SqlProviderServices (SQL Server에 대 한 EF 공급자)는 초기화를 위해 다음과 유사한 코드에 있습니다.

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

이 생성자는 다음 도우미 클래스를 사용합니다.

*   SingletonDependencyResolver: 단일 서비스를 확인 하는 간단한 방법을 제공-동일한 인스턴스에 반환 되는 각 시간 GetService 호출 되도록 하는 서비스 이므로 합니다. 일시적인 서비스는 요청 시 일시적인 인스턴스를 만들려면 사용할 단일 팩터리로 종종 등록 됩니다.
*   ExecutionStrategyResolver: 확인자를 관련 IExecutionStrategy 구현을 반환 합니다.

DbProviderServices.AddDependencyResolver를 사용 하는 대신 DbProviderServices.GetService를 재정의 하 고 추가 서비스를 직접 해결할 수 이기도 합니다. EF 및 정의 된 특정 형식으로, 경우에 따라 지정된 된 키에 대 한 서비스 해야 하는 경우이 메서드를 호출 합니다. 메서드는 수 또는 옵트아웃 서비스를 반환 하려면 null을 반환 하 고 대신 문제를 해결 하는 다른 클래스를 허용 하는 경우 서비스를 반환 해야 합니다. 예를 들어, 기본 연결 팩터리를 해결 하려면 GetService의 코드 다음과 같이

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

여러 DbProviderServices 구현을 응용 프로그램의 구성 파일에 등록 된 경우 나열 된 순서로 보조 확인자로 추가 됩니다. 확인자 항상 추가 되는 보조 확인자 체인의 맨 위에 즉, 목록의 끝에 공급자를 다른 종속 해결할 기회를 받게 됩니다. (처음에 약간 낯설고 보일 수 있지만 각 공급자 목록에서 가져와 기존 공급자 위에 쌓는 상상할 하는 경우는 것이 좋습니다.)

이 정렬은 일반적으로 문제가 되지 않습니다 대부분의 공급자 서비스 공급자 및 공급자 고정 이름으로 키가 지정 되므로 합니다. 그러나 서비스에 대 한는 하지 키가 지정 된 공급자 고정 이름 또는 일부 기타 공급자별 키 서비스 해결 될 예정이 순서에 따라 합니다. 예를 들어 설정 되지 않은 명시적으로 다르게 어딘가에 그렇지 않으면, 경우 기본 연결 팩터리는 체인의 맨 위에 있는 공급자 로부터 제공 됩니다.

## <a name="additional-config-file-registrations"></a>추가 구성 파일 등록

명시적으로 일부 응용 프로그램의 구성 파일에서 직접 위에서 설명한 추가 공급자 서비스를 등록 하는 것이 가능 합니다. 이 구성 파일의 등록을 완료 되 면 DbProviderServices 구현의 GetService 메서드를 반환한 모든 대신 사용 됩니다.

### <a name="registering-the-default-connection-factory"></a>기본 연결 팩터리를 등록합니다.

부터 EF5 EntityFramework NuGet 패키지가 자동으로 SQL Express 연결 팩터리 또는 LocalDb 연결 팩터리 구성 파일에 등록 합니다.

예를 들어:

``` xml
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework" >
</entityFramework>
```

합니다 _형식_ IDbConnectionFactory를 구현 해야 하는 기본 연결 팩터리는에 대 한 어셈블리의 정규화 된 유형 이름입니다.

공급자 NuGet 패키지를 설치 하는 경우이 방식으로 기본 연결 팩터리를 설정 하는 것이 좋습니다. 참조 _공급자에 대 한 NuGet 패키지_ 아래.

## <a name="additional-ef6-provider-changes"></a>추가 된 EF6 공급자 변경

### <a name="spatial-provider-changes"></a>공간 공급자가 변경

이제 공간 형식을 지 원하는 공급자 DbSpatialDataReader에서 파생 된 클래스에 일부 추가 메서드를 구현 해야 합니다.

*   `public abstract bool IsGeographyColumn(int ordinal)`
*   `public abstract bool IsGeometryColumn(int ordinal)`

기본 구현은 동기 메서드를 대리자로 위임 하 고 따라서 실행 하지 않도록 비동기적으로 재정의 될 것을 권장 하는 기존 메서드의 비동기 버전을 새 있습니다.

*   `public virtual Task<DbGeography> GetGeographyAsync(int ordinal, CancellationToken cancellationToken)`
*   `public virtual Task<DbGeometry> GetGeometryAsync(int ordinal, CancellationToken cancellationToken)`

### <a name="native-support-for-enumerablecontains"></a>Enumerable.Contains에 대 한 기본 지원

EF6은 DbInExpression Enumerable.Contains LINQ 쿼리에서 사용 관련 성능 문제를 해결 하려면 추가 된 새로운 식 형식을 소개 합니다. DbProviderManifest 클래스에는 새 가상 메서드로 SupportsInExpression 공급자가 새로운 식 형식을 처리 하는 경우를 확인 하려면 EF에서 호출 하는. 기존 공급자 구현 사용 하 여 호환성에 대 한 메서드에서 false를 반환 합니다. 이러한 향상 된이 기능을 활용할 수 EF6 공급자로 DbInExpression 처리 SupportsInExpression true를 반환 하도록 재정의 하는 코드를 추가할 수 있습니다. DbInExpression 인스턴스의 DbExpressionBuilder.In 메서드를 호출 하 여 만들 수 있습니다. 일반적으로 일치 항목을 테스트 하려면 DbConstantExpression 목록 및 테이블 열을 나타내는 DbExpression DbInExpression 인스턴스 구성 됩니다.

## <a name="nuget-packages-for-providers"></a>공급자에 대 한 NuGet 패키지

EF6 공급자를 사용할 수 있도록 하나의 방법은 NuGet 패키지로 릴리스 것입니다. NuGet 패키지를 사용 하 여 다음과 같은 이점이 있습니다.

*   사용 하기 쉬운 NuGet 공급자 등록 응용 프로그램의 구성 파일에 추가 하는 것
*   추가로 변경할 수 있습니다 구성 파일에 연결 규칙에는 등록 된 공급자를 사용 하 여 기본 연결 팩터리를 설정 하려면
*   NuGet은 EF6 공급자 계속 새 EF 패키지 해제 된 후에 작동 되도록 바인딩 리디렉션을 추가 처리

이 예로 EntityFramework.SqlServerCompact 패키지에 포함 되는 [오픈 소스 코드 베이스](http://github.com/aspnet/entityframework6)합니다. 이 패키지는 EF 공급자 NuGet 패키지를 만들기 위한 좋은 템플릿으로 제공 합니다.

### <a name="powershell-commands"></a>PowerShell 명령

EntityFramework NuGet 패키지를 설치한 경우에 공급자 패키지에 대 한 매우 유용한 두 개의 명령이 포함 된 PowerShell 모듈을 등록 합니다.

*   추가 EFProvider 대상 프로젝트의 구성 파일에서 공급자에 대 한 새 엔터티를 추가 하 고 등록 된 공급자 목록 끝에는 확인 합니다.
*   추가 EFDefaultConnectionFactory 추가 하거나 대상 프로젝트의 구성 파일에서 defaultConnectionFactory 등록을 업데이트 합니다.

이러한 명령을 구성 파일에는 entityFramework 섹션을 추가 하 고 필요한 경우 공급자 컬렉션을 추가 주의 해야 합니다.

Install.ps1 NuGet 스크립트에서에서 이러한 명령을 호출할 수는 것입니다. 예를 들어, SQL Compact 공급자는 install.ps1이 유사합니다.

``` powershell
param($installPath, $toolsPath, $package, $project)
Add-EFDefaultConnectionFactory $project 'System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework' -ConstructorArguments 'System.Data.SqlServerCe.4.0'
Add-EFProvider $project 'System.Data.SqlServerCe.4.0' 'System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact'</pre>
```

패키지 관리자 콘솔 창에서 get-help를 사용 하 여 이러한 명령에 대 한 자세한 정보를 가져올 수 있습니다.

## <a name="wrapping-providers"></a>래핑 공급자

래핑 공급자가 프로 파일링 또는 추적 기능 등의 다른 기능을 사용 하 여 확장을 기존 공급자를 래핑하는 EF 및/또는 ADO.NET 공급자입니다. 일반적인 방법으로 래핑 공급자를 등록할 수 있습니다 하지만 것이 편리 하 게 공급자 관련 서비스의 해상도 해석 하 여 런타임 시 래핑 공급자를 설치 합니다. 이렇게 하려면 정적 이벤트 OnLockingConfiguration DbConfiguration 클래스에 사용할 수 있습니다.

응용 프로그램 도메인에 대 한 모든 EF 구성을 가져올 수는 있는 EF에서 확인 한 이후 이지만 용도로 잠기기 전까지 OnLockingConfiguration 라고 합니다. 앱 시작 시 (EF가 사용 됨) 전에 앱 등록 해야이 이벤트에 대 한 이벤트 처리기입니다. (구성 파일에서이 처리기를 등록 하는 것에 대 한 지원을 추가할 예정 이지만 아직 지원 되지 않습니다.) 이벤트 처리기 다음 ReplaceService 래핑되어야 하는 모든 서비스에 대 한 호출을 해야 합니다.  

예를 들어 IDbConnectionFactory 및 DbProviderService 래핑할이 같은 처리기를 등록 해야 합니다.

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

서비스 해결 되었으면 이제 서비스를 해결 하기 위해 사용 된 키와 함께 래핑되어야 하는 처리기에 전달 됩니다. 그런 다음 처리기를이 서비스를 배치 하 고 래핑된 버전을 사용 하 여 반환 되는 서비스를 대체 수 있습니다.

## <a name="resolving-a-dbproviderfactory-with-ef"></a>EF 사용 하 여 DbProviderFactory를 확인합니다.

DbProviderFactory를 사용 하면 EF에 설명 된 대로 필요한 기본 공급자 형식 중 하나인 합니다 _공급자 형식 개요_ 위의 섹션입니다. 이미 언급 했 듯이, 해당 형식이 아닌 EF 및 등록 되지 일반적으로 EF 구성의 일부분 이지만 대신 합니다 machine.config 파일 및/또는 응용 프로그램의 구성 파일에서 일반 ADO.NET 공급자 등록 합니다.

이 EF 불구 하 고 여전히 해당 일반적인 종속성 해결 메커니즘 DbProviderFactory를 찾을 때 사용 하기 위해 사용 합니다. 기본 해결 프로그램 구성 파일에서는 일반 ADO.NET 등록 하 고 이것이 일반적으로 투명 합니다. 일반 종속성 확인으로 인해 메커니즘을 사용 하지만 일반 ADO.NET 등록 수행 하지 않은 경우에 DbProviderFactory를 확인 하는 IDbDependencyResolver를 사용할 수 있도록 의미 합니다.

DbProviderFactory를 이러한 방식으로 해결에 몇 가지 의미가 있습니다.

*   코드 기반 구성을 사용 하 여 응용 프로그램에 적절 한 DbProviderFactory를 등록할 해당 DbConfiguration 클래스에서 호출을 추가할 수 있습니다. 이 기능은 특히 유용 하지 않을 (또는 없나요)는 응용 프로그램에 대 한 확인 전혀 모든 파일 기반 구성을 사용 합니다.
*   서비스를 래핑할 수 있습니다 하거나에 설명 된 대로 ReplaceService를 사용 하 여 대체 합니다 _래핑 공급자_ 위의 섹션
*   이론적으로 DbProviderServices 구현 DbProviderFactory를 확인할 수 있습니다.

이러한 작업 중 하나를 수행 하는 방법에 대 한 주의할 중요 한 점은 EF에서 DbProviderFactory의 조회만 적용 됩니다는 경우 다른 비 EF 코드는 일반적인 방법으로 등록할 ADO.NET 공급자를 여전히 예상할 수 및 등록이 없는 경우 실패할 수 있습니다. 이 따라서 일반적으로 일반 ADO.NET 방식에서 등록할 DbProviderFactory를 위한 더 나은 것.

### <a name="related-services"></a>관련된 서비스

EF는 DbProviderFactory를 해결 하려면 사용 하는 경우 다음이 확인 해야 IProviderInvariantName 및 IDbProviderFactoryResolver 서비스입니다.

IProviderInvariantName는 DbProviderFactory의 지정된 된 형식에 대 한 공급자 고정 이름을 확인 하는 데 사용 되는 서비스입니다. 이 서비스의 기본 구현을 ADO.NET 공급자 등록을 사용합니다. 이 ADO.NET 공급자 DbProviderFactory를 확인 하는 EF에서 때문에 일반적인 방법으로 등록 되지 않은, 경우 다음도 해야이 서비스를 확인할 것을 의미 합니다. Note DbConfiguration.SetProviderFactory 메서드를 사용 하는 경우이 서비스에 대 한 해결 프로그램을 자동으로 추가 됩니다.

에 설명 된 대로 합니다 _공급자 형식 개요_ 위의 섹션에서 IDbProviderFactoryResolver 지정된 DbConnection 개체에서 올바른 DbProviderFactory를 가져오는 데 사용 됩니다. .NET 4에서 실행 되는 ADO.NET 공급자 등록을 사용 하는 경우이 서비스의 기본 구현입니다. 이 ADO.NET 공급자 DbProviderFactory를 확인 하는 EF에서 때문에 일반적인 방법으로 등록 되지 않은, 경우 다음도 해야이 서비스를 확인할 것을 의미 합니다.
