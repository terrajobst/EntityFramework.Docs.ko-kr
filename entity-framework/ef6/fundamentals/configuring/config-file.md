---
title: 구성 파일 설정-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 86389e4a3a3bac46e2a4cf2da648a4b19e29f3c3
ms.sourcegitcommit: 299011fc4bd576eed58a4274f967639fa13fec53
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886563"
---
# <a name="configuration-file-settings"></a>구성 파일 설정
Entity Framework 구성 파일에서 여러 가지 설정을 지정할 수 있습니다. 일반적으로 EF는 ' 구성 규칙 ' 원칙을 따릅니다 .이 게시물에 설명 된 모든 설정은 기본 동작을 수행 합니다. 기본값이 더 이상 요구 사항을 충족 하지 않는 경우에만 설정을 변경 하는 것에 대해서만 걱정할 필요가 있습니다.  

## <a name="a-code-based-alternative"></a>코드 기반 대안  

이러한 설정은 모두 코드를 사용 하 여 적용할 수 있습니다. EF6 부터는 코드에서 구성을 적용 하는 중심 방법을 제공 하는 [코드 기반 구성을](code-based.md)도입 했습니다. EF6 이전에는 코드에서 구성을 계속 적용할 수 있지만 다양 한 Api를 사용 하 여 다양 한 영역을 구성 해야 합니다. 구성 파일 옵션을 사용 하면 코드를 업데이트 하지 않고 배포 중에 이러한 설정을 쉽게 변경할 수 있습니다.

## <a name="the-entity-framework-configuration-section"></a>Entity Framework 구성 섹션  

EF 4.1부터 구성 파일의 **appSettings** 섹션을 사용 하 여 컨텍스트에 대 한 데이터베이스 이니셜라이저를 설정할 수 있습니다. EF 4.3에는 새 설정을 처리 하는 사용자 지정 **Entityframework** 섹션이 도입 되었습니다. Entity Framework는 이전 형식을 사용 하 여 데이터베이스 이니셜라이저 집합을 계속 인식 하지만 가능한 경우 새 형식으로 이동 하는 것이 좋습니다.

Entityframework NuGet 패키지를 설치할 때 **entityframework** 섹션이 프로젝트의 구성 파일에 자동으로 추가 되었습니다.  

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework"
       type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=4.3.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
  </configSections>
</configuration>
```  

## <a name="connection-strings"></a>연결 문자열  

[이 페이지](~/ef6/fundamentals/configuring/connection-strings.md) 에서는 구성 파일의 연결 문자열을 포함 하 여 사용할 데이터베이스를 Entity Framework 결정 하는 방법에 대해 자세히 설명 합니다.  

연결 문자열은 표준 **connectionStrings** 요소로 이동 하며 **entityframework** 섹션이 필요 하지 않습니다.  

Code First 기반 모델은 일반 ADO.NET 연결 문자열을 사용 합니다. 예를 들어:  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

EF Designer 기반 모델은 특수 EF 연결 문자열을 사용 합니다. 예:  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient;
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a>코드 기반 구성 유형 (EF6)  

EF6부터 응용 프로그램의 [코드 기반 구성](code-based.md) 에 사용할 DBCONFIGURATION을 EF에 지정할 수 있습니다. 대부분의 경우 EF에서 DbConfiguration를 자동으로 검색 하므로이 설정을 지정할 필요가 없습니다. 구성 파일에서 DbConfiguration을 지정 해야 하는 경우에 대 한 자세한 내용은 [코드 기반 구성](code-based.md)의 **이동 dbconfiguration** 섹션을 참조 하십시오.  

DbConfiguration 형식을 설정 하려면 **codeConfigurationType** 요소에 정규화 된 어셈블리 형식 이름을 지정 합니다.  

> [!NOTE]
> 정규화 된 어셈블리 이름은 네임 스페이스로 정규화 된 이름이 며, 그 다음에는 해당 형식이 있는 어셈블리입니다. 어셈블리 버전, culture 및 공개 키 토큰을 선택적으로 지정할 수도 있습니다.  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a>EF 데이터베이스 공급자 (EF6 이상)  

EF6 이전에는 데이터베이스 공급자의 Entity Framework 관련 부분이 핵심 ADO.NET 공급자의 일부로 포함 되어야 했습니다. EF6부터 EF 관련 부분은 이제 개별적으로 관리 및 등록 됩니다.  

일반적으로 공급자를 직접 등록할 필요가 없습니다. 이러한 작업은 일반적으로 공급자를 설치할 때 수행 됩니다.  

공급자는 **Entityframework** 섹션의 **providers** 자식 섹션 아래에 **공급자** 요소를 포함 하 여 등록 됩니다. 공급자 항목에는 다음과 같은 두 가지 필수 특성이 있습니다.  

- **invariantName** 는이 EF 공급자가 대상으로 하는 핵심 ADO.NET 공급자를 식별 합니다.  
- **type** 은 EF 공급자 구현에 대 한 어셈블리의 정규화 된 형식 이름입니다.  

> [!NOTE]
> 정규화 된 어셈블리 이름은 네임 스페이스로 정규화 된 이름이 며, 그 다음에는 해당 형식이 있는 어셈블리입니다. 어셈블리 버전, culture 및 공개 키 토큰을 선택적으로 지정할 수도 있습니다.  

예를 들어 Entity Framework를 설치할 때 기본 SQL Server 공급자를 등록 하기 위해 만든 항목이 여기에 나와 있습니다.  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a>인터셉터 (EF 6.1 이상)  

EF 6.1부터 구성 파일에 인터셉터를 등록할 수 있습니다. 인터셉터를 사용 하면 EF가 데이터베이스 쿼리 실행, 연결 열기 등의 특정 작업을 수행할 때 추가 논리를 실행할 수 있습니다.  

인터셉터는 **Entityframework** 섹션의 **인터셉터** 자식 섹션 아래에 **인터셉터** 요소를 포함 하 여 등록 됩니다. 예를 들어 다음 구성은 모든 데이터베이스 작업을 콘솔에 기록 하는 기본 제공 **Databaselogger 거** 인터셉터를 등록 합니다.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a>파일에 데이터베이스 작업 로깅 (EF 6.1 이상)  

구성 파일을 통해 인터셉터를 등록 하는 것은 문제를 디버그 하는 데 도움이 되도록 기존 응용 프로그램에 로깅을 추가 하려는 경우에 특히 유용 합니다. **Databaselogger** 파일 이름을 생성자 매개 변수로 제공 하 여 파일에 대 한 로깅을 지원 합니다.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

기본적으로이 파일은 앱이 시작 될 때마다 새 파일을 사용 하 여 로그 파일을 덮어씁니다. 이미 존재 하는 경우 로그 파일에 추가 하려면 다음과 같은 항목을 사용 합니다.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
      <parameter value="true" type="System.Boolean"/>
    </parameters>
  </interceptor>
</interceptors>
```  

**Databaselogger** 나 인터셉터 등록에 대 한 자세한 내용은 블로그 게시물 [EF 6.1: 다시 컴파일하지](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/)않고 로깅을 설정 합니다.  

## <a name="code-first-default-connection-factory"></a>Code First 기본 연결 팩터리  

구성 섹션에서는를 사용 하 여 컨텍스트에 사용할 데이터베이스를 찾는 Code First 기본 연결 팩터리를 지정할 수 있습니다. 기본 연결 팩터리는 컨텍스트의 구성 파일에 연결 문자열을 추가 하지 않은 경우에만 사용 됩니다.  

EF NuGet 패키지를 설치 하는 경우 설치 된 기본 연결 팩터리가 SQL Express 또는 LocalDB를 가리키도록 등록 되었습니다.  

연결 팩터리를 설정 하려면 **Defaultconnectionfactory** 요소에 정규화 된 어셈블리 형식 이름을 지정 합니다.  

> [!NOTE]
> 정규화 된 어셈블리 이름은 네임 스페이스로 정규화 된 이름이 며, 그 다음에는 해당 형식이 있는 어셈블리입니다. 어셈블리 버전, culture 및 공개 키 토큰을 선택적으로 지정할 수도 있습니다.  

기본 연결 팩터리를 설정 하는 예제는 다음과 같습니다.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

위의 예제에서는 사용자 지정 팩터리에서 매개 변수가 없는 생성자를 사용 해야 합니다. 필요한 경우 **parameters** 요소를 사용 하 여 생성자 매개 변수를 지정할 수 있습니다.  

예를 들어 Entity Framework에 포함 된 SqlCeConnectionFactory에는 생성자에 공급자 고정 이름을 제공 해야 합니다. 공급자 고정 이름은 사용 하려는 SQL Compact의 버전을 식별 합니다. 다음 구성을 사용 하면 기본적으로 컨텍스트가 SQL Compact 버전 4.0을 사용 합니다.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

기본 연결 팩터리를 설정 하지 않으면 Code First SqlConnectionFactory `.\SQLEXPRESS`를 사용 하 여를 가리킵니다. 또한 SqlConnectionFactory에는 연결 문자열의 일부를 재정의할 수 있는 생성자가 있습니다. 이외의 `.\SQLEXPRESS` SQL Server 인스턴스를 사용 하려는 경우이 생성자를 사용 하 여 서버를 설정할 수 있습니다.  

다음 구성에서는 명시적 연결 문자열이 설정 되지 않은 컨텍스트에 대해 **Mydatabaseserver** 를 사용 Code First 합니다.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

기본적으로 생성자 인수는 문자열 형식 이라고 가정 합니다. Type 특성을 사용 하 여이를 변경할 수 있습니다.  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a>데이터베이스 이니셜라이저  

데이터베이스 이니셜라이저는 컨텍스트 별로 구성 됩니다. **컨텍스트** 요소를 사용 하 여 구성 파일에서 설정할 수 있습니다. 이 요소는 어셈블리의 정규화 된 이름을 사용 하 여 구성 중인 컨텍스트를 식별 합니다.  

기본적으로 Code First 컨텍스트는 CreateDatabaseIfNotExists 이니셜라이저를 사용 하도록 구성 됩니다. 데이터베이스 초기화를 사용 하지 않도록 설정 하는 데 사용할 수 있는 **context** 요소에 **disabledatabaseinitialization** 특성이 있습니다.  

예를 들어 다음 구성은 MyAssembly에 정의 된 블로그의 데이터베이스 초기화를 사용 하지 않도록 설정 합니다.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

**Databaseinitializer** 요소를 사용 하 여 사용자 지정 이니셜라이저를 설정할 수 있습니다.  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

생성자 매개 변수는 기본 연결 팩터리와 동일한 구문을 사용 합니다.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly">
      <parameters>
        <parameter value="MyConstructorParameter" />
      </parameters>
    </databaseInitializer>
  </context>
</contexts>
```  

Entity Framework에 포함 된 일반 데이터베이스 이니셜라이저 중 하나를 구성할 수 있습니다. **Type** 특성은 제네릭 형식에 대해 .NET Framework 형식을 사용 합니다.  

예를 들어 Code First 마이그레이션를 사용 하는 경우 `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` 이니셜라이저를 사용 하 여 자동으로 마이그레이션되는 데이터베이스를 구성할 수 있습니다.  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
