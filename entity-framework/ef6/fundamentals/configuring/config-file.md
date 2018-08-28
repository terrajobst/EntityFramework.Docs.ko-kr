---
title: 구성 파일 설정-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 88c2439f3a89c9fb9ee22f828789df4decf39cc5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996505"
---
# <a name="configuration-file-settings"></a>구성 파일 설정
Entity Framework에는 다양을 한 설정 구성 파일에서 지정할 수 있습니다. 일반적 EF '구성 보다 규칙' 원칙을 따릅니다:이 게시물에 설명 된 모든 설정은 기본 동작을가 기본값에는 더 이상 요구 사항을 충족 하는 경우 설정을 변경 하는 방법에 대 한 걱정 해야 합니다.  

## <a name="a-code-based-alternative"></a>코드 기반 대체 항목  

이러한 모든 설정을 적용할 수도 있습니다 코드를 사용 합니다. 부터 도입 되었습니다 EF6 [코드 기반 구성](code-based.md), 코드에서 구성을 적용 하는 중앙 방법을 제공 하는 합니다. EF6를 이전 코드에서 구성도 적용할 수 있지만 다양 한 Api를 사용 하 여 다양 한 영역을 구성 하 해야 합니다. 구성 파일 옵션에는 이러한 설정을 배포 하는 동안 코드를 업데이트 하지 않고 쉽게 변경할 수 있습니다.

## <a name="the-entity-framework-configuration-section"></a>Entity Framework 구성 섹션  

EF4.1 있습니다부터 사용 하 여 상황에 맞는 데이터베이스 이니셜라이저를 설정할 수는 **appSettings** 구성 파일의 섹션입니다. 사용자 지정 소개 했습니다 EF 4.3 **entityFramework** 새 설정을 처리 하는 섹션입니다. Entity Framework에서 여전히 이전 형식을 사용 하 여 설정 하는 데이터베이스 이니셜라이저를 인식 하지만 가능한 경우 새 형식으로 이동 하는 것이 좋습니다.

합니다 **entityFramework** 섹션 EntityFramework NuGet 패키지를 설치할 때 자동으로 프로젝트의 구성 파일에 추가 되었습니다.  

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

[이 페이지](~/ef6/fundamentals/configuring/connection-strings.md) 구성 파일에서 연결 문자열을 포함 하 여 Entity Framework에서 사용할 데이터베이스를 결정 하는 방법에 자세한 정보를 제공 합니다.  

연결 문자열은 표준에서 이동 **connectionStrings** 요소 않아도 합니다 **entityFramework** 섹션.  

먼저 기반 코드 모델 일반 ADO.NET 연결 문자열을 사용 합니다. 예를 들어:  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

EF 디자이너 모델 사용 하 여 특수 한 EF 연결 문자열을 기반으로 합니다. 예를 들어:  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a>코드 기반 구성 유형 (EF6부터 해당)  

EF6부터 데 EF에 대 한 DbConfiguration 지정할 수 있습니다 [코드 기반 구성](code-based.md) 응용 프로그램에서 합니다. 대부분의 EF를 DbConfiguration에 자동으로 검색 하는 대로이 설정을 지정할 필요가 없습니다. DbConfiguration config 파일에서 지정 해야 하는 경우의 세부 정보 참조를 **이동 DbConfiguration** 부분 [코드 기반 구성](code-based.md)합니다.  

어셈블리 정규화 된 형식 이름을 DbConfiguration 형식을 설정 하려면 지정 합니다 **codeConfigurationType** 요소입니다.  

> [!NOTE]
> 정규화 된 어셈블리 이름은 쉼표, 어셈블리의 형식에 있는 다음 네임 스페이스 정규화 된 이름입니다. 필요에 따라 어셈블리 버전, 문화권 및 공개 키 토큰을 지정할 수도 있습니다.  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a>EF 데이터베이스 공급자 (EF6부터 해당)  

EF6을 하기 전에 데이터베이스 공급자의 Entity Framework 전용 부분 코어 ADO.NET 공급자의 일부로 포함 되도록 했습니다. EF6부터 EF 특정 부분은 이제 관리 되며 별도로 등록 합니다.  

일반적으로 공급자를 직접 등록할 필요가 없습니다. 일반적으로 이렇게 공급자를 설치할 때 설치 됩니다.  

공급자를 포함 하 여 등록 된를 **공급자** 요소 아래에 있는 **공급자** 의 하위 섹션을 **entityFramework** 섹션. 공급자 항목에 대 한 필수 특성을 두 가지 있습니다.  

- **invariantName** core ADO.NET 공급자를 식별 하는이 EF 공급자가 대상  
- **형식** EF 공급자 구현의 어셈블리 정규화 된 형식 이름  

> [!NOTE]
> 정규화 된 어셈블리 이름은 쉼표, 어셈블리의 형식에 있는 다음 네임 스페이스 정규화 된 이름입니다. 필요에 따라 어셈블리 버전, 문화권 및 공개 키 토큰을 지정할 수도 있습니다.  

예를 들어 Entity Framework를 설치한 경우 기본 SQL Server 공급자를 등록 하기 위해 만든 항목을 다음과 같습니다.  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a>인터셉터 (EF6.1부터 해당)  

EF6.1를 사용 하 여 시작 하면 인터셉터 구성 파일에 등록할 수 있습니다. 인터셉터를 사용 하면 EF 연결 등을 열어 데이터베이스 쿼리 실행과 같은 특정 작업을 수행 하는 경우 추가 논리를 실행할 수 있습니다.  

인터셉터를 포함 하 여 등록 된는 **인터셉터** 요소 아래에 있는 **인터셉터** 자식 섹션의 **entityFramework** 섹션. 예를 들어, 다음 구성을 등록 기본 제공 **DatabaseLogger** 인터셉터는 콘솔에 모든 데이터베이스 작업을 기록 합니다.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a>로깅 데이터베이스 작업 파일 (EF6.1부터 해당)  

인터셉터 구성 파일을 통해 등록 문제를 디버깅 하는 데 기존 응용 프로그램에 로깅을 추가 하려는 경우 특히 유용 합니다. **DatabaseLogger** 생성자 매개 변수로 파일 이름을 제공 하 여 파일에는 로깅을 지원 합니다.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

기본적으로이 로그 파일을 앱이 시작 될 때마다 새 파일을 사용 하 여 덮어쓸 수로 인해 됩니다. 로그에 대신 추가할 파일 이미 있는 경우에 같은 코드를 사용 합니다.  

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

에 대 한 자세한 **DatabaseLogger** 블로그 게시물을 참조 인터셉터를 등록 하 고 [EF 6.1: 다시 컴파일하지 않고도 로깅을 사용 하도록](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/)합니다.  

## <a name="code-first-default-connection-factory"></a>첫 번째 기본 연결 팩터리 코드  

구성 섹션을 사용 하면 Code First 컨텍스트에 대해 사용 하도록 데이터베이스를 찾으려고 사용 해야 하는 기본 연결 팩터리를 지정할 수 있습니다. 기본 연결 팩터리는 컨텍스트에 대 한 구성 파일에 연결 문자열을 추가한 경우에 사용 됩니다.  

EF NuGet 패키지를 설치한 경우 SQL Express 또는 LocalDB에 어떤 것에 따라 설치를 가리키는 기본 연결 팩터리가 등록 되었습니다.  

어셈블리 정규화 된 형식 이름을 지정 하면 연결 팩터리를 설정 하려면 합니다 **deafultConnectionFactory** 요소입니다.  

> [!NOTE]
> 정규화 된 어셈블리 이름은 쉼표, 어셈블리의 형식에 있는 다음 네임 스페이스 정규화 된 이름입니다. 필요에 따라 어셈블리 버전, 문화권 및 공개 키 토큰을 지정할 수도 있습니다.  

사용자 고유의 기본 연결 팩터리가 설정 예는 다음과 같습니다.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

위의 예제에는 매개 변수가 없는 생성자를 갖도록 사용자 지정 팩터리가 필요 합니다. 필요한 경우에 생성자 매개 변수를 사용 하 여 지정할 수 있습니다 합니다 **매개 변수** 요소입니다.  

예를 들어, Entity Framework에 포함 된, SqlCeConnectionFactory을 사용 하려면 공급자 고정 이름을 생성자에 제공 해야 합니다. 공급자 고정 이름을 사용 하려는 SQL Compact의 버전을 식별 합니다. 다음 구성은 기본적으로 SQL Compact 버전 4.0을 사용 하는 컨텍스트를 발생 합니다.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

기본 연결 팩터리가 설정 하지 않으면, Code First 사용을 가리키는 SqlConnectionFactory `.\SQLEXPRESS`합니다. SqlConnectionFactory 연결 문자열의 부분을 재정의할 수 있도록 하는 생성자가 있습니다. 이외의 SQL Server 인스턴스를 사용 하려는 경우 `.\SQLEXPRESS` 이 생성자를 사용 하 여 서버를 설정할 수 있습니다.  

다음 구성은 Code First를 사용 하면 **MyDatabaseServer** 는 명시적 연결 문자열 설정 되지 않은 컨텍스트에 대 한 합니다.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

기본적으로 문자열 형식의 생성자 인수는 가정 합니다. 이 설정을 변경 하려면 type 특성을 사용할 수 있습니다.  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a>데이터베이스 이니셜라이저  

데이터베이스 이니셜라이저 컨텍스트 당 별로 구성 됩니다. 사용 하 여 구성 파일에서 설정할 수는 **상황에 맞는** 요소입니다. 이 요소는 구성 중인 컨텍스트를 식별 하 어셈블리의 정규화 된 이름을 사용 합니다.  

기본적으로 Code First 컨텍스트에 CreateDatabaseIfNotExists 이니셜라이저를 사용 하도록 구성 됩니다. **disableDatabaseInitialization** 특성을 합니다 **상황에 맞는** 데이터베이스 초기화를 사용 하지 않도록 설정 하는 데 사용할 수 있는 요소입니다.  

예를 들어, 다음 구성을 MyAssembly.dll에 정의 된 Blogging.BlogContext 컨텍스트에 대 한 데이터베이스 초기화를 해제 합니다.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

사용할 수는 **databaseInitializer** 사용자 정의 이니셜라이저 함수를 설정할 요소입니다.  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

기본 연결 팩터리와 동일한 구문을 사용 하는 생성자 매개 변수입니다.  

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

Entity Framework에 포함 된 일반 데이터베이스 이니셜라이저 중 하나를 구성할 수 있습니다. 합니다 **형식** 특성 제네릭 형식에 대 한.NET Framework 형식을 사용 합니다.  

예를 들어, Code First 마이그레이션을 사용 하는 경우는 마이그레이션할 데이터베이스를 사용 하 여 자동으로 구성할 수는 `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` 이니셜라이저입니다.  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
