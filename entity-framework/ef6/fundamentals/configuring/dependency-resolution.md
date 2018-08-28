---
title: EF6-종속성 확인
author: divega
ms.date: 2016-10-23
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 45681bb0cedecd502b1968b90b7f682d3257dd23
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998164"
---
# <a name="dependency-resolution"></a>종속성 확인
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

EF6부터 Entity Framework는 필요한 서비스의 구현을 가져오기 위한 일반 용도의 메커니즘을 포함 합니다. 즉, EF 일부 인터페이스 또는 기본 클래스의 인스턴스를 사용 하는 경우 묻습니다 인터페이스 또는 기본 클래스의 구체적 구현은 사용 합니다. 이 작업은 IDbDependencyResolver 인터페이스를 사용 하 여 수행 됩니다.  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

GetService 메서드는 일반적으로 EF에서 호출 및 EF에서 또는 응용 프로그램에서 제공 하는 IDbDependencyResolver의 구현에 의해 처리 됩니다. 형식 인수는 요청 되는 서비스의 인터페이스 또는 기본 클래스 형식을 호출 하는 경우 및 키 개체가 null 이거나 요청된 된 서비스에 대 한 컨텍스트 정보를 제공 하는 개체입니다.  

이 문서에서는 IDbDependencyResolver를 구현 하는 방법에 대 한 자세한 내용은 포함 하지 않지만 대신 담아 두는 EF GetService 및 키 개체의 의미 체계에 대 한 호출의 이러한 각 서비스 형식 (즉, 인터페이스 및 기본 클래스 형식)에 대 한 참조 호출합니다. 이 문서는 추가 서비스를 추가할 때 최신 상태로 유지 됩니다.  

## <a name="services-resolved"></a>서비스 확인  

언급이 단일 항목으로 사용할 수 있으므로 반환 되는 모든 개체 스레드로부터 안전 해야 합니다. 대부분의 경우에 팩터리 개체를 반환 팩터리 자체 스레드로부터 안전 해야 하지만 공장에서 반환 되는 개체 스레드로부터 안전한 새 인스턴스는 각 사용에 대 한 공장에서 요청한 이후의 되도록 필요는 없습니다.  

### <a name="systemdataentityidatabaseinitializertcontext"></a>것과 < TContext\>  

**도입 된 버전**: EF6.0.0  

**반환 된 개체**: 지정된 된 컨텍스트 형식에 대 한 데이터베이스 이니셜라이저  

**키**: 하지 사용; null  

### <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a>Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\>  

**도입 된 버전**: EF6.0.0

**반환 된 개체**: 마이그레이션과 만들려는 데이터베이스 이니셜라이저를 사용 하 여 데이터베이스 생성과 같은 데이터베이스를 유발 하는 다른 작업에 사용할 수 있는 SQL 생성기를 만들기 위한 팩터리입니다.  

**키**: SQL 생성 될 데이터베이스의 유형을 지정 하는 ADO.NET 공급자 고정 이름을 포함 하는 문자열입니다. 예를 들어, SQL Server SQL 생성기 키 "System.Data.SqlClient"에 대 한 반환 됩니다.  

>[!NOTE]
> EF6에서 공급자 관련 서비스에 대 한 자세한 내용은 참조는 [EF6 공급자 모델](~/ef6/fundamentals/providers/provider-model.md) 섹션입니다.  

### <a name="systemdataentitycorecommondbproviderservices"></a>System.Data.Entity.Core.Common.DbProviderServices  

**도입 된 버전**: EF6.0.0  

**반환 된 개체**: 지정 된 공급자 고정 이름을 사용 하는 EF 공급자  

**키**: ADO.NET 공급자 고정 이름에는 공급자가 필요한 데이터베이스의 형식 지정을 포함 하는 문자열입니다. 예를 들어, SQL Server 공급자는 키 "System.Data.SqlClient"에 대 한 반환 됩니다.  

>[!NOTE]
> EF6에서 공급자 관련 서비스에 대 한 자세한 내용은 참조는 [EF6 공급자 모델](~/ef6/fundamentals/providers/provider-model.md) 섹션입니다.  

### <a name="systemdataentityinfrastructureidbconnectionfactory"></a>System.Data.Entity.Infrastructure.IDbConnectionFactory  

**도입 된 버전**: EF6.0.0  

**반환 된 개체**: 규칙에 따라 EF가 데이터베이스 연결을 만들 때 사용 될 연결 팩터리입니다. 즉, EF에 연결 하지 못하거나 연결 문자열 지정 app.config 또는 web.config에서 연결 문자열을 찾을 수 있습니다, 다음이 서비스는 하는 데 사용 연결을 만들 규칙에 따라 합니다. 연결 팩터리를 변경 합니다. 기본적으로 다른 유형의 데이터베이스 (예를 들어, SQL Server Compact Edition)를 사용 하는 EF를 허용할 수 있습니다.  

**키**: 하지 사용; null  

>[!NOTE]
> EF6에서 공급자 관련 서비스에 대 한 자세한 내용은 참조는 [EF6 공급자 모델](~/ef6/fundamentals/providers/provider-model.md) 섹션입니다.  

### <a name="systemdataentityinfrastructureimanifesttokenservice"></a>System.Data.Entity.Infrastructure.IManifestTokenService  

**도입 된 버전**: EF6.0.0  

**반환 된 개체**: 연결에서 공급자 매니페스트 토큰을 생성할 수 있는 서비스입니다. 이 서비스는 일반적으로 두 가지 방법으로 사용 됩니다. 첫째, Code First 모델을 빌드할 때 데이터베이스에 연결 하지 않으려면 사용 될 수 있습니다. 둘째, Code First SQL Server 2008을 사용 하는 경우에 따라 하는 경우에 SQL Server 2005에 대 한 모델을 적용할 특정 데이터베이스 버전-예를 들어 모델을 작성 하도록 사용할 수 있습니다.  

**개체 수명**: 단일-동일한 개체에는 시간 및 다른 스레드에서 동시에 사용 되는 여러 수 있습니다  

**키**: 하지 사용; null  

### <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a>System.Data.Entity.Infrastructure.IDbProviderFactoryService  

**도입 된 버전**: EF6.0.0  

**반환 된 개체**: 지정된 된 연결에서 공급자 팩터리를 얻을 수 있는 서비스입니다. .NET 4.5에서 공급자는 연결에서 공개적으로 액세스할 수 있습니다. .NET 4에서이 서비스의 기본 구현은 일부 추론을 사용 하 여 일치 하는 공급자를 찾을 수 있습니다. 적절 한 확인을 제공 하는 이러한 실패 한 후이 서비스의 새 구현을 등록할 수 있습니다.  

**키**: 하지 사용; null  

### <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a>Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\>  

**도입 된 버전**: EF6.0.0  

**반환 된 개체**: 지정 된 컨텍스트에 대 한 모델 캐시 키를 생성 하는 팩터리입니다. 기본적으로 EF DbContext 형식 공급자별 당 하나의 모델을 캐시합니다. 캐시 키에 스키마 이름 등의 기타 정보를 추가 하려면이 서비스의 다른 구현을 사용할 수 있습니다.  

**키**: 하지 사용; null  

### <a name="systemdataentityspatialdbspatialservices"></a>System.Data.Entity.Spatial.DbSpatialServices  

**도입 된 버전**: EF6.0.0  

**반환 된 개체**: geography 및 geometry 공간 형식에 대 한 기본 EF 공급자 지원을 추가 하는 공간 공급자는 EF 합니다.  

**키**: DbSptialServices에 대 한 두 가지 방법으로 라는 메시지가 표시 됩니다. 첫째, 공급자별 공간 서비스 DbProviderInfo 개체를 사용 하 여 요청 된 (invariant를 포함 하는 이름 및 매니페스트 토큰) 키로 합니다. 둘째, 키가 없는 DbSpatialServices에 요청할 수 있습니다. 해결 "전역 공간 공급자" 독립 실행형 DbGeography 또는 DbGeometry 유형을 만들 때 사용 되는 데 사용 됩니다.  

>[!NOTE]
> EF6에서 공급자 관련 서비스에 대 한 자세한 내용은 참조는 [EF6 공급자 모델](~/ef6/fundamentals/providers/provider-model.md) 섹션입니다.  

### <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a>Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\>  

**도입 된 버전**: EF6.0.0  

**반환 된 개체**: 쿼리와 명령을 데이터베이스에 대해 실행 될 때 다시 시도 하거나 다른 동작을 구현 하는 공급자 수 있는 서비스를 만들기 위한 팩터리입니다. 구현이 제공 하는 경우 다음 EF 단순히 명령을 실행 하 여를 throw 된 예외를 전파 합니다. SQL Server에 대 한이 서비스는 SQL Azure 같은 클라우드 기반 데이터베이스 서버에 대해 실행할 때 특히 유용 하며 재시도 정책을 제공에 사용 됩니다.  

**키**: 공급자 고정 이름 및 필요에 따라 실행 전략은 사용할 서버 이름을 포함 하는 ExecutionStrategyKey 개체입니다.  

>[!NOTE]
> EF6에서 공급자 관련 서비스에 대 한 자세한 내용은 참조는 [EF6 공급자 모델](~/ef6/fundamentals/providers/provider-model.md) 섹션입니다.  

### <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a>Func < DbConnection, 문자열, System.Data.Entity.Migrations.History.HistoryContext\>  

**도입 된 버전**: EF6.0.0  

**반환 된 개체**: 공급자를 HistoryContext의 매핑을 구성할 수 있도록 하는 팩터리를 `__MigrationHistory` EF 마이그레이션을 사용 하는 테이블입니다. HistoryContext 코드 첫 번째 DbContext 되며 테이블 및 열 매핑 사양 이름과 같은 항목을 변경 하려면 일반 흐름 API를 사용 하 여 구성할 수 있습니다.  

**키**: 하지 사용; null  

>[!NOTE]
> EF6에서 공급자 관련 서비스에 대 한 자세한 내용은 참조는 [EF6 공급자 모델](~/ef6/fundamentals/providers/provider-model.md) 섹션입니다.  

### <a name="systemdatacommondbproviderfactory"></a>System.Data.Common.DbProviderFactory  

**도입 된 버전**: EF6.0.0  

**반환 된 개체**: 지정 된 공급자 고정 이름을 사용 하는 ADO.NET 공급자입니다.  

**키**: ADO.NET 공급자 고정 이름을 포함 하는 문자열  

>[!NOTE]
> 이 서비스는 일반적으로 변경 되지 직접 기본 구현에서는 일반 ADO.NET 공급자 등록을 사용 하기 때문입니다. EF6에서 공급자 관련 서비스에 대 한 자세한 내용은 참조는 [EF6 공급자 모델](~/ef6/fundamentals/providers/provider-model.md) 섹션입니다.  

### <a name="systemdataentityinfrastructureiproviderinvariantname"></a>System.Data.Entity.Infrastructure.IProviderInvariantName  

**도입 된 버전**: EF6.0.0  

**반환 된 개체**: DbProviderFactory의 지정된 된 형식에 대 한 공급자 고정 이름을 확인 하는 데 사용 되는 서비스입니다. 이 서비스의 기본 구현을 ADO.NET 공급자 등록을 사용합니다. 이 ADO.NET 공급자 DbProviderFactory를 확인 하는 EF에서 때문에 일반적인 방법으로 등록 되지 않은, 경우 다음도 해야이 서비스를 확인할 것을 의미 합니다.  

**키**: The DbProviderFactory 인스턴스 고정 이름이 필수입니다.  

>[!NOTE]
> EF6에서 공급자 관련 서비스에 대 한 자세한 내용은 참조는 [EF6 공급자 모델](~/ef6/fundamentals/providers/provider-model.md) 섹션입니다.  

### <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a>System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache  

**도입 된 버전**: EF6.0.0  

**반환 된 개체**: 미리 생성 된 뷰가 포함 된 어셈블리를 캐시 합니다. 대체 EF 모든 검색을 수행 하지 않고 미리 생성 된 뷰를 포함 하는 어셈블리를 알 수 있도록 일반적으로 사용 됩니다.  

**키**: 하지 사용; null  

### <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a>System.Data.Entity.Infrastructure.Pluralization.IPluralizationService

**도입 된 버전**: EF6.0.0  

**반환 된 개체**: 복수화 이름을 단 수 화를 EF에서 사용 되는 서비스입니다. 기본적으로 영어 복수 적용 서비스를 사용 됩니다.  

**키**: 하지 사용; null  

### <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a>System.Data.Entity.Infrastructure.Interception.IDbInterceptor  

**도입 된 버전**: EF6.0.0

**반환 된 개체**: 시작 하면 응용 프로그램을 등록 해야 하는 모든 인터셉터입니다. GetServices 호출을 사용 하 여 이러한 개체는 요청 및 모든 종속성 확인자를 반환한 모든 인터셉터는를 등록 합니다.

**키**: 하지 사용; null이 됩니다.  

### <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a>Func < System.Data.Entity.DbContext, 작업 < 문자열\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\>  

**도입 된 버전**: EF6.0.0  

**반환 된 개체**: 팩터리를 사용할 수 있는 데이터베이스 로그 포맷터를 만들 때 사용 되는 컨텍스트. 지정 된 컨텍스트에 따라 Database.Log 속성이 설정 됩니다.  

**키**: 하지 사용; null이 됩니다.  

### <a name="funcsystemdataentitydbcontext"></a>Func < System.Data.Entity.DbContext\>  

**도입 된 버전**: EF6.1.0  

**반환 된 개체**: 컨텍스트 매개 변수가 없는 생성자를 액세스할 수 없는 경우 마이그레이션에 대 한 컨텍스트 인스턴스를 만드는 데 사용할 팩터리입니다.  

**키**: 파생 된 DbContext에는 팩터리를 필요한 형식의 Type 개체입니다.  

### <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a>Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\>  

**도입 된 버전**: EF6.1.0  

**반환 된 개체**: 직렬화 및에서 Code First 마이그레이션을 사용 하기 위해 XML로 desterilized 수 있도록 강력한 사용자 지정 주석의 serialization에 대 한 serializer를 만드는 데 사용할 팩터리입니다.  

**키**: 되는 주석의 이름을 serialize 하거나 deserialize 합니다.  

### <a name="funcsystemdataentityinfrastructuretransactionhandler"></a>Func < System.Data.Entity.Infrastructure.TransactionHandler\>  

**도입 된 버전**: EF6.1.0  

**반환 된 개체**: 커밋 실패를 처리 하는 등 상황에 대 한 특수 처리를 적용할 수 있도록 트랜잭션에 대 한 처리기를 만들려면 사용할 팩터리입니다.  

**키**: 공급자 고정 이름 및 필요에 따라 트랜잭션 처리기를 사용할 서버 이름을 포함 하는 ExecutionStrategyKey 개체입니다.  
