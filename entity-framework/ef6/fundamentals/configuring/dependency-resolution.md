---
title: 종속성 확인-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 6082124481f5795bbcb62fff2bb6a58ecdcb48e4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414796"
---
# <a name="dependency-resolution"></a>종속성 확인
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

EF6부터 Entity Framework에는 필요한 서비스의 구현을 얻기 위한 범용 메커니즘이 포함 되어 있습니다. 즉, EF에서 일부 인터페이스나 기본 클래스의 인스턴스를 사용 하는 경우 사용할 인터페이스 또는 기본 클래스의 구체적인 구현을 요청 합니다. IDbDependencyResolver 인터페이스를 사용 하 여이 작업을 수행 합니다.  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

GetService 메서드는 일반적으로 EF에서 호출 되며 EF 또는 응용 프로그램에서 제공 하는 IDbDependencyResolver 구현에 의해 처리 됩니다. 호출 될 때 형식 인수는 요청 되는 서비스의 인터페이스 또는 기본 클래스 형식이 고, 키 개체는 요청 된 서비스에 대 한 컨텍스트 정보를 제공 하는 개체 또는 null입니다.  

달리 명시 되지 않은 경우 반환 되는 개체는 단일 항목으로 사용할 수 있으므로 스레드로부터 안전 해야 합니다. 대부분의 경우 반환 되는 개체는 팩터리 자체가 스레드로부터 안전 해야 하지만 팩터리에서 반환 되는 개체는 각 사용에 대해 팩터리에서 새 인스턴스를 요청 하기 때문에 스레드로부터 안전 하지 않아도 되는 팩터리입니다.

이 문서에는 IDbDependencyResolver을 구현 하는 방법에 대 한 자세한 내용은 포함 되어 있지 않지만, EF가 GetService를 호출 하는 서비스 형식 (즉, 인터페이스 및 기본 클래스 형식)에 대 한 참조의 역할을 하 고 각각에 대해 키 개체의 의미 체계를 확인 합니다. 요청.

## <a name="systemdataentityidatabaseinitializertcontext"></a>TContext\> < Tdatabase이니셜라이저가 있습니다.  

**소개 된 버전**: ef 6.0.0  

**반환 된 개체**: 지정 된 컨텍스트 형식에 대 한 데이터베이스 이니셜라이저  

**키**: 사용 되지 않습니다. null이 됩니다.  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a>MigrationSqlGenerator\> 함수를 < 합니다.  

**소개 된 버전**: ef 6.0.0

**반환 된 개체**: 데이터베이스를 사용 하 여 데이터베이스를 만드는 등의 데이터베이스 생성을 유발 하는 마이그레이션 및 기타 작업에 사용할 수 있는 SQL 생성기를 만드는 팩터리입니다.  

**키**: SQL을 생성할 데이터베이스의 유형을 지정 하는 ADO.NET 공급자 고정 이름을 포함 하는 문자열입니다. 예를 들어 "System.object" 키에 대 한 SQL Server SQL 생성기가 반환 됩니다.  

>[!NOTE]
> EF6의 공급자 관련 서비스에 대 한 자세한 내용은 [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) 섹션을 참조 하세요.  

## <a name="systemdataentitycorecommondbproviderservices"></a>System.object. 일반적인 DbProviderServices  

**소개 된 버전**: ef 6.0.0  

**반환 된 개체**: 지정 된 공급자 고정 이름에 사용할 EF 공급자입니다.  

**키**: 공급자가 필요한 데이터베이스 유형을 지정 하는 ADO.NET 공급자 고정 이름을 포함 하는 문자열입니다. 예를 들어 "System.object" 키에 대 한 SQL Server 공급자가 반환 됩니다.  

>[!NOTE]
> EF6의 공급자 관련 서비스에 대 한 자세한 내용은 [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) 섹션을 참조 하세요.  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a>IDbConnectionFactory입니다.  

**소개 된 버전**: ef 6.0.0  

**반환 된 개체**: EF가 규칙에 따라 데이터베이스 연결을 만들 때 사용 되는 연결 팩터리입니다. 즉, EF에 연결 또는 연결 문자열을 지정 하지 않고 app.config 또는 web.config에서 연결 문자열을 찾을 수 없는 경우이 서비스는 규칙에 따라 연결을 만드는 데 사용 됩니다. 연결 팩터리를 변경 하면 EF에서 기본적으로 다른 유형의 데이터베이스 (예: SQL Server Compact Edition)를 사용할 수 있습니다.  

**키**: 사용 되지 않습니다. null이 됩니다.  

>[!NOTE]
> EF6의 공급자 관련 서비스에 대 한 자세한 내용은 [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) 섹션을 참조 하세요.  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a>IManifestTokenService입니다.  

**소개 된 버전**: ef 6.0.0  

**반환 된 개체**: 연결에서 공급자 매니페스트 토큰을 생성할 수 있는 서비스입니다. 이 서비스는 일반적으로 두 가지 방법으로 사용 됩니다. 먼저 모델을 작성할 때 데이터베이스에 연결 Code First를 방지 하는 데 사용할 수 있습니다. 두 번째로 Code First 하 여 특정 데이터베이스 버전에 대 한 모델을 작성 하는 데 사용할 수 있습니다. 예를 들어 SQL Server 2008를 사용 하는 경우에도 SQL Server 2005에 대 한 모델을 강제로 작성 합니다.  

**개체 수명**: Singleton-동일한 개체를 여러 번 사용 하 고 다른 스레드에서 동시에 사용할 수 있습니다.  

**키**: 사용 되지 않습니다. null이 됩니다.  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a>IDbProviderFactoryService입니다.  

**소개 된 버전**: ef 6.0.0  

**반환 된 개체**: 지정 된 연결에서 공급자 팩터리를 가져올 수 있는 서비스입니다. .NET 4.5에서는 연결에서 공급자에 공개적으로 액세스할 수 있습니다. .NET 4에서이 서비스의 기본 구현은 일부 추론을 사용 하 여 일치 하는 공급자를 찾습니다. 이러한 오류가 발생 하면 적절 한 해결 방법을 제공 하기 위해이 서비스의 새 구현을 등록할 수 있습니다.  

**키**: 사용 되지 않습니다. null이 됩니다.  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a>Func < DbContext, IDbModelCacheKey.\>  

**소개 된 버전**: ef 6.0.0  

**반환 된 개체**: 지정 된 컨텍스트에 대 한 모델 캐시 키를 생성 하는 팩터리입니다. 기본적으로 EF는 공급자 당 DbContext 유형별 모델 하나를 캐시 합니다. 이 서비스의 다른 구현은 스키마 이름과 같은 다른 정보를 캐시 키에 추가 하는 데 사용할 수 있습니다.  

**키**: 사용 되지 않습니다. null이 됩니다.  

## <a name="systemdataentityspatialdbspatialservices"></a>DbSpatialServices입니다.  

**소개 된 버전**: ef 6.0.0  

**반환 된 개체**: 지리 및 기 하 도형 공간 형식에 대 한 기본 ef 공급자에 지원을 추가 하는 EF 공간 공급자입니다.  

**키**: DbSptialServices는 두 가지 방법으로에 대 한 요청을 받습니다. 먼저 DbProviderInfo 개체 (고정 이름 및 매니페스트 토큰 포함)를 키로 사용 하 여 공급자별 공간 서비스를 요청 합니다. 둘째, DbSpatialServices는 키 없이에 대 한 요청을 받을 수 있습니다. 이는 독립 실행형 DbGeography 또는 DbGeometry 형식을 만들 때 사용 되는 "전역 공간 공급자"를 확인 하는 데 사용 됩니다.  

>[!NOTE]
> EF6의 공급자 관련 서비스에 대 한 자세한 내용은 [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) 섹션을 참조 하세요.  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a>IDbExecutionStrategy\> 함수를 < 합니다.  

**소개 된 버전**: ef 6.0.0  

**반환 된 개체**: 데이터베이스에 대해 쿼리 및 명령이 실행 될 때 공급자가 재시도 또는 다른 동작을 구현할 수 있도록 하는 서비스를 만드는 팩터리입니다. 구현을 제공 하지 않으면 EF는 단순히 명령을 실행 하 고 throw 된 모든 예외를 전파 합니다. SQL Server이 서비스는 SQL Azure와 같은 클라우드 기반 데이터베이스 서버에 대해 실행 하는 경우에 특히 유용한 재시도 정책을 제공 하는 데 사용 됩니다.  

**키**: 공급자 고정 이름을 포함 하 고 필요에 따라 실행 전략을 사용할 서버 이름을 포함 하는 ExecutionStrategyKey 개체입니다.  

>[!NOTE]
> EF6의 공급자 관련 서비스에 대 한 자세한 내용은 [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) 섹션을 참조 하세요.  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a>Func < DbConnection, string, HistoryContext.\>  

**소개 된 버전**: ef 6.0.0  

**반환 된 개체**: 공급자가 EF 마이그레이션에 사용 되는 `__MigrationHistory` 테이블에 대 한 HistoryContext 매핑을 구성할 수 있도록 하는 팩터리입니다. HistoryContext는 Code First DbContext, 일반 흐름 API를 사용 하 여 테이블 이름 및 열 매핑 사양과 같은 항목을 변경 하 여 구성할 수 있습니다.  

**키**: 사용 되지 않습니다. null이 됩니다.  

>[!NOTE]
> EF6의 공급자 관련 서비스에 대 한 자세한 내용은 [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) 섹션을 참조 하세요.  

## <a name="systemdatacommondbproviderfactory"></a>DbProviderFactory.  

**소개 된 버전**: ef 6.0.0  

**반환 된 개체**: 지정 된 공급자 고정 이름에 사용할 ADO.NET 공급자입니다.  

**키**: ADO.NET 공급자 고정 이름을 포함 하는 문자열입니다.  

>[!NOTE]
> 기본 구현에서 일반적인 ADO.NET 공급자 등록을 사용 하기 때문에 일반적으로이 서비스는 직접 변경 되지 않습니다. EF6의 공급자 관련 서비스에 대 한 자세한 내용은 [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) 섹션을 참조 하세요.  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a>IProviderInvariantName입니다.  

**소개 된 버전**: ef 6.0.0  

**반환 된 개체**: 지정 된 형식의 DbProviderFactory에 대 한 공급자 고정 이름을 결정 하는 데 사용 되는 서비스입니다. 이 서비스의 기본 구현에서는 ADO.NET 공급자 등록을 사용 합니다. 즉, EF에 의해 DbProviderFactory가 확인 되 고 있기 때문에 ADO.NET 공급자가 일반적인 방식으로 등록 되지 않은 경우이 서비스를 해결 해야 합니다.  

**키**: 고정 이름이 필요한 DbProviderFactory 인스턴스입니다.  

>[!NOTE]
> EF6의 공급자 관련 서비스에 대 한 자세한 내용은 [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) 섹션을 참조 하세요.  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a>System.object. IViewAssemblyCache. IViewAssemblyCache  

**소개 된 버전**: ef 6.0.0  

**반환 된 개체**: 미리 생성 된 뷰를 포함 하는 어셈블리의 캐시입니다. 일반적으로 대체를 사용 하 여 검색을 수행 하지 않고 미리 생성 된 뷰를 포함 하는 어셈블리를 EF에서 알 수 있습니다.  

**키**: 사용 되지 않습니다. null이 됩니다.  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a>복수화. IPluralizationService입니다.

**소개 된 버전**: ef 6.0.0  

**반환 된 개체**: EF에서 이름을 복수화 및 단 수 하는 데 사용 하는 서비스입니다. 기본적으로 영어 복수화 서비스를 사용 합니다.  

**키**: 사용 되지 않습니다. null이 됩니다.  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a>IDbInterceptor을 (를)  

**소개 된 버전**: ef 6.0.0

**반환 된 개체**: 응용 프로그램이 시작 될 때 등록 해야 하는 모든 인터셉터입니다. 이러한 개체는 GetServices 호출을 사용 하 여 요청 되며 종속성 확인 기에서 반환 된 모든 인터셉터가 등록 됩니다.

**키**: 사용 되지 않습니다. 는 null입니다.  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a>Func < DbContext, Action < string\>,\> System.web. m a c. m a c.  

**소개 된 버전**: ef 6.0.0  

**반환 된 개체**: 컨텍스트에 사용 될 데이터베이스 로그 포맷터를 만드는 데 사용 되는 팩터리입니다. 지정 된 컨텍스트에서 Database. Log 속성을 설정 합니다.  

**키**: 사용 되지 않습니다. 는 null입니다.  

## <a name="funcsystemdataentitydbcontext"></a>Func < DbContext\>  

**소개 된 버전**: ef 6.1.0  

**반환 된 개체**: 컨텍스트에 액세스할 수 있는 매개 변수가 없는 생성자가 없는 경우 마이그레이션에 대 한 컨텍스트 인스턴스를 만드는 데 사용 되는 팩터리입니다.  

**키**: 팩터리가 필요한 파생 DbContext의 형식에 대 한 형식 개체입니다.  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a>IMetadataAnnotationSerializer\> (Func <)입니다.  

**소개 된 버전**: ef 6.1.0  

**반환 된 개체**: 강력한 형식의 사용자 지정 주석을 직렬화 하는 데 사용 하는 팩터리로,이를 serialize 하 고 Code First 마이그레이션에 사용 하기 위해 XML로 전달할 수 있습니다.  

**키**: serialize 되거나 deserialize 되는 주석의 이름입니다.  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a>Func <의\>입니다.  

**소개 된 버전**: ef 6.1.0  

**반환 된 개체**: 커밋 실패를 처리 하는 등의 상황에 특별 한 처리를 적용할 수 있도록 트랜잭션에 대 한 처리기를 만드는 데 사용 되는 팩터리입니다.  

**키**: 공급자 고정 이름을 포함 하 고 필요에 따라 트랜잭션 처리기를 사용할 서버 이름을 포함 하는 ExecutionStrategyKey 개체입니다.  
