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
# <a name="configuration-file-settings"></a><span data-ttu-id="da78c-102">구성 파일 설정</span><span class="sxs-lookup"><span data-stu-id="da78c-102">Configuration File Settings</span></span>
<span data-ttu-id="da78c-103">Entity Framework 구성 파일에서 여러 가지 설정을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-103">Entity Framework allows a number of settings to be specified from the configuration file.</span></span> <span data-ttu-id="da78c-104">일반적으로 EF는 ' 구성 규칙 ' 원칙을 따릅니다 .이 게시물에 설명 된 모든 설정은 기본 동작을 수행 합니다. 기본값이 더 이상 요구 사항을 충족 하지 않는 경우에만 설정을 변경 하는 것에 대해서만 걱정할 필요가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-104">In general EF follows a ‘convention over configuration’ principle: all the settings discussed in this post have a default behavior, you only need to worry about changing the setting when the default no longer satisfies your requirements.</span></span>  

## <a name="a-code-based-alternative"></a><span data-ttu-id="da78c-105">코드 기반 대안</span><span class="sxs-lookup"><span data-stu-id="da78c-105">A Code-Based Alternative</span></span>  

<span data-ttu-id="da78c-106">이러한 설정은 모두 코드를 사용 하 여 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-106">All of these settings can also be applied using code.</span></span> <span data-ttu-id="da78c-107">EF6 부터는 코드에서 구성을 적용 하는 중심 방법을 제공 하는 [코드 기반 구성을](code-based.md)도입 했습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-107">Starting in EF6 we introduced [code-based configuration](code-based.md), which provides a central way of applying configuration from code.</span></span> <span data-ttu-id="da78c-108">EF6 이전에는 코드에서 구성을 계속 적용할 수 있지만 다양 한 Api를 사용 하 여 다양 한 영역을 구성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-108">Prior to EF6, configuration can still be applied from code but you need to use various APIs to configure different areas.</span></span> <span data-ttu-id="da78c-109">구성 파일 옵션을 사용 하면 코드를 업데이트 하지 않고 배포 중에 이러한 설정을 쉽게 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-109">The configuration file option allows these settings to be easily changed during deployment without updating your code.</span></span>

## <a name="the-entity-framework-configuration-section"></a><span data-ttu-id="da78c-110">Entity Framework 구성 섹션</span><span class="sxs-lookup"><span data-stu-id="da78c-110">The Entity Framework Configuration Section</span></span>  

<span data-ttu-id="da78c-111">EF 4.1부터 구성 파일의 **appSettings** 섹션을 사용 하 여 컨텍스트에 대 한 데이터베이스 이니셜라이저를 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-111">Starting with EF4.1 you could set the database initializer for a context using the **appSettings** section of the configuration file.</span></span> <span data-ttu-id="da78c-112">EF 4.3에는 새 설정을 처리 하는 사용자 지정 **Entityframework** 섹션이 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-112">In EF 4.3 we introduced the custom **entityFramework** section to handle the new settings.</span></span> <span data-ttu-id="da78c-113">Entity Framework는 이전 형식을 사용 하 여 데이터베이스 이니셜라이저 집합을 계속 인식 하지만 가능한 경우 새 형식으로 이동 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-113">Entity Framework will still recognize database initializers set using the old format, but we recommend moving to the new format where possible.</span></span>

<span data-ttu-id="da78c-114">Entityframework NuGet 패키지를 설치할 때 **entityframework** 섹션이 프로젝트의 구성 파일에 자동으로 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-114">The **entityFramework** section was automatically added to the configuration file of your project when you installed the EntityFramework NuGet package.</span></span>  

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

## <a name="connection-strings"></a><span data-ttu-id="da78c-115">연결 문자열</span><span class="sxs-lookup"><span data-stu-id="da78c-115">Connection Strings</span></span>  

<span data-ttu-id="da78c-116">[이 페이지](~/ef6/fundamentals/configuring/connection-strings.md) 에서는 구성 파일의 연결 문자열을 포함 하 여 사용할 데이터베이스를 Entity Framework 결정 하는 방법에 대해 자세히 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-116">[This page](~/ef6/fundamentals/configuring/connection-strings.md) provides more details on how Entity Framework determines the database to be used, including connection strings in the configuration file.</span></span>  

<span data-ttu-id="da78c-117">연결 문자열은 표준 **connectionStrings** 요소로 이동 하며 **entityframework** 섹션이 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-117">Connection strings go in the standard **connectionStrings** element and do not require the **entityFramework** section.</span></span>  

<span data-ttu-id="da78c-118">Code First 기반 모델은 일반 ADO.NET 연결 문자열을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-118">Code First based models use normal ADO.NET connection strings.</span></span> <span data-ttu-id="da78c-119">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="da78c-119">For example:</span></span>  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

<span data-ttu-id="da78c-120">EF Designer 기반 모델은 특수 EF 연결 문자열을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-120">EF Designer based models use special EF connection strings.</span></span> <span data-ttu-id="da78c-121">예:</span><span class="sxs-lookup"><span data-stu-id="da78c-121">For example:</span></span>  

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

## <a name="code-based-configuration-type-ef6-onwards"></a><span data-ttu-id="da78c-122">코드 기반 구성 유형 (EF6)</span><span class="sxs-lookup"><span data-stu-id="da78c-122">Code-Based Configuration Type (EF6 Onwards)</span></span>  

<span data-ttu-id="da78c-123">EF6부터 응용 프로그램의 [코드 기반 구성](code-based.md) 에 사용할 DBCONFIGURATION을 EF에 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-123">Starting with EF6, you can specify the DbConfiguration for EF to use for [code-based configuration](code-based.md) in your application.</span></span> <span data-ttu-id="da78c-124">대부분의 경우 EF에서 DbConfiguration를 자동으로 검색 하므로이 설정을 지정할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-124">In most cases you don't need to specify this setting as EF will automatically discover your DbConfiguration.</span></span> <span data-ttu-id="da78c-125">구성 파일에서 DbConfiguration을 지정 해야 하는 경우에 대 한 자세한 내용은 [코드 기반 구성](code-based.md)의 **이동 dbconfiguration** 섹션을 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="da78c-125">For details of when you may need to specify DbConfiguration in your config file see the **Moving DbConfiguration** section of [Code-Based Configuration](code-based.md).</span></span>  

<span data-ttu-id="da78c-126">DbConfiguration 형식을 설정 하려면 **codeConfigurationType** 요소에 정규화 된 어셈블리 형식 이름을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-126">To set a DbConfiguration type, you specify the assembly qualified type name in the **codeConfigurationType** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="da78c-127">정규화 된 어셈블리 이름은 네임 스페이스로 정규화 된 이름이 며, 그 다음에는 해당 형식이 있는 어셈블리입니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-127">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="da78c-128">어셈블리 버전, culture 및 공개 키 토큰을 선택적으로 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-128">You can optionally also specify the assembly version, culture and public key token.</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a><span data-ttu-id="da78c-129">EF 데이터베이스 공급자 (EF6 이상)</span><span class="sxs-lookup"><span data-stu-id="da78c-129">EF Database Providers (EF6 Onwards)</span></span>  

<span data-ttu-id="da78c-130">EF6 이전에는 데이터베이스 공급자의 Entity Framework 관련 부분이 핵심 ADO.NET 공급자의 일부로 포함 되어야 했습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-130">Prior to EF6, Entity Framework-specific parts of a database provider had to be included as part of the core ADO.NET provider.</span></span> <span data-ttu-id="da78c-131">EF6부터 EF 관련 부분은 이제 개별적으로 관리 및 등록 됩니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-131">Starting with EF6, the EF specific parts are now managed and registered separately.</span></span>  

<span data-ttu-id="da78c-132">일반적으로 공급자를 직접 등록할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-132">Normally you won't need to register providers yourself.</span></span> <span data-ttu-id="da78c-133">이러한 작업은 일반적으로 공급자를 설치할 때 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-133">This will typically be done by the provider when you install it.</span></span>  

<span data-ttu-id="da78c-134">공급자는 **Entityframework** 섹션의 **providers** 자식 섹션 아래에 **공급자** 요소를 포함 하 여 등록 됩니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-134">Providers are registered by including a **provider** element under the **providers** child section of the **entityFramework** section.</span></span> <span data-ttu-id="da78c-135">공급자 항목에는 다음과 같은 두 가지 필수 특성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-135">There are two required attributes for a provider entry:</span></span>  

- <span data-ttu-id="da78c-136">**invariantName** 는이 EF 공급자가 대상으로 하는 핵심 ADO.NET 공급자를 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-136">**invariantName** identifies the core ADO.NET provider that this EF provider targets</span></span>  
- <span data-ttu-id="da78c-137">**type** 은 EF 공급자 구현에 대 한 어셈블리의 정규화 된 형식 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-137">**type** is the assembly qualified type name of the EF provider implementation</span></span>  

> [!NOTE]
> <span data-ttu-id="da78c-138">정규화 된 어셈블리 이름은 네임 스페이스로 정규화 된 이름이 며, 그 다음에는 해당 형식이 있는 어셈블리입니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-138">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="da78c-139">어셈블리 버전, culture 및 공개 키 토큰을 선택적으로 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-139">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="da78c-140">예를 들어 Entity Framework를 설치할 때 기본 SQL Server 공급자를 등록 하기 위해 만든 항목이 여기에 나와 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-140">As an example here is the entry created to register the default SQL Server provider when you install Entity Framework.</span></span>  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a><span data-ttu-id="da78c-141">인터셉터 (EF 6.1 이상)</span><span class="sxs-lookup"><span data-stu-id="da78c-141">Interceptors (EF6.1 Onwards)</span></span>  

<span data-ttu-id="da78c-142">EF 6.1부터 구성 파일에 인터셉터를 등록할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-142">Starting with EF6.1 you can register interceptors in the configuration file.</span></span> <span data-ttu-id="da78c-143">인터셉터를 사용 하면 EF가 데이터베이스 쿼리 실행, 연결 열기 등의 특정 작업을 수행할 때 추가 논리를 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-143">Interceptors allow you to run additional logic when EF performs certain operations, such as executing database queries, opening connections, etc.</span></span>  

<span data-ttu-id="da78c-144">인터셉터는 **Entityframework** 섹션의 **인터셉터** 자식 섹션 아래에 **인터셉터** 요소를 포함 하 여 등록 됩니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-144">Interceptors are registered by including an **interceptor** element under the **interceptors** child section of the **entityFramework** section.</span></span> <span data-ttu-id="da78c-145">예를 들어 다음 구성은 모든 데이터베이스 작업을 콘솔에 기록 하는 기본 제공 **Databaselogger 거** 인터셉터를 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-145">For example, the following configuration registers the built-in **DatabaseLogger** interceptor that will log all database operations to the Console.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a><span data-ttu-id="da78c-146">파일에 데이터베이스 작업 로깅 (EF 6.1 이상)</span><span class="sxs-lookup"><span data-stu-id="da78c-146">Logging Database Operations to a File (EF6.1 Onwards)</span></span>  

<span data-ttu-id="da78c-147">구성 파일을 통해 인터셉터를 등록 하는 것은 문제를 디버그 하는 데 도움이 되도록 기존 응용 프로그램에 로깅을 추가 하려는 경우에 특히 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-147">Registering interceptors via the config file is especially useful when you want to add logging to an existing application to help debug an issue.</span></span> <span data-ttu-id="da78c-148">**Databaselogger** 파일 이름을 생성자 매개 변수로 제공 하 여 파일에 대 한 로깅을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-148">**DatabaseLogger** supports logging to a file by supplying the file name as a constructor parameter.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="da78c-149">기본적으로이 파일은 앱이 시작 될 때마다 새 파일을 사용 하 여 로그 파일을 덮어씁니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-149">By default this will cause the log file to be overwritten with a new file each time the app starts.</span></span> <span data-ttu-id="da78c-150">이미 존재 하는 경우 로그 파일에 추가 하려면 다음과 같은 항목을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-150">To instead append to the log file if it already exists use something like:</span></span>  

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

<span data-ttu-id="da78c-151">**Databaselogger** 나 인터셉터 등록에 대 한 자세한 내용은 블로그 게시물 [EF 6.1: 다시 컴파일하지](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/)않고 로깅을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-151">For additional information on **DatabaseLogger** and registering interceptors, see the blog post [EF 6.1: Turning on logging without recompiling](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span></span>  

## <a name="code-first-default-connection-factory"></a><span data-ttu-id="da78c-152">Code First 기본 연결 팩터리</span><span class="sxs-lookup"><span data-stu-id="da78c-152">Code First Default Connection Factory</span></span>  

<span data-ttu-id="da78c-153">구성 섹션에서는를 사용 하 여 컨텍스트에 사용할 데이터베이스를 찾는 Code First 기본 연결 팩터리를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-153">The configuration section allows you to specify a default connection factory that Code First should use to locate a database to use for a context.</span></span> <span data-ttu-id="da78c-154">기본 연결 팩터리는 컨텍스트의 구성 파일에 연결 문자열을 추가 하지 않은 경우에만 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-154">The default connection factory is only used when no connection string has been added to the configuration file for a context.</span></span>  

<span data-ttu-id="da78c-155">EF NuGet 패키지를 설치 하는 경우 설치 된 기본 연결 팩터리가 SQL Express 또는 LocalDB를 가리키도록 등록 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-155">When you installed the EF NuGet package a default connection factory was registered that points to either SQL Express or LocalDB, depending on which one you have installed.</span></span>  

<span data-ttu-id="da78c-156">연결 팩터리를 설정 하려면 **Defaultconnectionfactory** 요소에 정규화 된 어셈블리 형식 이름을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-156">To set a connection factory, you specify the assembly qualified type name in the **defaultConnectionFactory** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="da78c-157">정규화 된 어셈블리 이름은 네임 스페이스로 정규화 된 이름이 며, 그 다음에는 해당 형식이 있는 어셈블리입니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-157">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="da78c-158">어셈블리 버전, culture 및 공개 키 토큰을 선택적으로 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-158">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="da78c-159">기본 연결 팩터리를 설정 하는 예제는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-159">Here is an example of setting your own default connection factory:</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

<span data-ttu-id="da78c-160">위의 예제에서는 사용자 지정 팩터리에서 매개 변수가 없는 생성자를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-160">The above example requires the custom factory to have a parameterless constructor.</span></span> <span data-ttu-id="da78c-161">필요한 경우 **parameters** 요소를 사용 하 여 생성자 매개 변수를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-161">If needed, you can specify constructor parameters using the **parameters** element.</span></span>  

<span data-ttu-id="da78c-162">예를 들어 Entity Framework에 포함 된 SqlCeConnectionFactory에는 생성자에 공급자 고정 이름을 제공 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-162">For example, the SqlCeConnectionFactory, that is included in Entity Framework, requires you to supply a provider invariant name to the constructor.</span></span> <span data-ttu-id="da78c-163">공급자 고정 이름은 사용 하려는 SQL Compact의 버전을 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-163">The provider invariant name identifies the version of SQL Compact you want to use.</span></span> <span data-ttu-id="da78c-164">다음 구성을 사용 하면 기본적으로 컨텍스트가 SQL Compact 버전 4.0을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-164">The following configuration will cause contexts to use SQL Compact version 4.0 by default.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="da78c-165">기본 연결 팩터리를 설정 하지 않으면 Code First SqlConnectionFactory `.\SQLEXPRESS`를 사용 하 여를 가리킵니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-165">If you don’t set a default connection factory, Code First uses the SqlConnectionFactory, pointing to `.\SQLEXPRESS`.</span></span> <span data-ttu-id="da78c-166">또한 SqlConnectionFactory에는 연결 문자열의 일부를 재정의할 수 있는 생성자가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-166">SqlConnectionFactory also has a constructor that allows you to override parts of the connection string.</span></span> <span data-ttu-id="da78c-167">이외의 `.\SQLEXPRESS` SQL Server 인스턴스를 사용 하려는 경우이 생성자를 사용 하 여 서버를 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-167">If you want to use a SQL Server instance other than `.\SQLEXPRESS` you can use this constructor to set the server.</span></span>  

<span data-ttu-id="da78c-168">다음 구성에서는 명시적 연결 문자열이 설정 되지 않은 컨텍스트에 대해 **Mydatabaseserver** 를 사용 Code First 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-168">The following configuration will cause Code First to use **MyDatabaseServer** for contexts that don’t have an explicit connection string set.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="da78c-169">기본적으로 생성자 인수는 문자열 형식 이라고 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-169">By default, it’s assumed that constructor arguments are of type string.</span></span> <span data-ttu-id="da78c-170">Type 특성을 사용 하 여이를 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-170">You can use the type attribute to change this.</span></span>  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a><span data-ttu-id="da78c-171">데이터베이스 이니셜라이저</span><span class="sxs-lookup"><span data-stu-id="da78c-171">Database Initializers</span></span>  

<span data-ttu-id="da78c-172">데이터베이스 이니셜라이저는 컨텍스트 별로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-172">Database initializers are configured on a per-context basis.</span></span> <span data-ttu-id="da78c-173">**컨텍스트** 요소를 사용 하 여 구성 파일에서 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-173">They can be set in the configuration file using the **context** element.</span></span> <span data-ttu-id="da78c-174">이 요소는 어셈블리의 정규화 된 이름을 사용 하 여 구성 중인 컨텍스트를 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-174">This element uses the assembly qualified name to identify the context being configured.</span></span>  

<span data-ttu-id="da78c-175">기본적으로 Code First 컨텍스트는 CreateDatabaseIfNotExists 이니셜라이저를 사용 하도록 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-175">By default, Code First contexts are configured to use the CreateDatabaseIfNotExists initializer.</span></span> <span data-ttu-id="da78c-176">데이터베이스 초기화를 사용 하지 않도록 설정 하는 데 사용할 수 있는 **context** 요소에 **disabledatabaseinitialization** 특성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-176">There is a **disableDatabaseInitialization** attribute on the **context** element that can be used to disable database initialization.</span></span>  

<span data-ttu-id="da78c-177">예를 들어 다음 구성은 MyAssembly에 정의 된 블로그의 데이터베이스 초기화를 사용 하지 않도록 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-177">For example, the following configuration disables database initialization for the Blogging.BlogContext context defined in MyAssembly.dll.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

<span data-ttu-id="da78c-178">**Databaseinitializer** 요소를 사용 하 여 사용자 지정 이니셜라이저를 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-178">You can use the **databaseInitializer** element to set a custom initializer.</span></span>  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

<span data-ttu-id="da78c-179">생성자 매개 변수는 기본 연결 팩터리와 동일한 구문을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-179">Constructor parameters use the same syntax as default connection factories.</span></span>  

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

<span data-ttu-id="da78c-180">Entity Framework에 포함 된 일반 데이터베이스 이니셜라이저 중 하나를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-180">You can configure one of the generic database initializers that are included in Entity Framework.</span></span> <span data-ttu-id="da78c-181">**Type** 특성은 제네릭 형식에 대해 .NET Framework 형식을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-181">The **type** attribute uses the .NET Framework format for generic types.</span></span>  

<span data-ttu-id="da78c-182">예를 들어 Code First 마이그레이션를 사용 하는 경우 `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` 이니셜라이저를 사용 하 여 자동으로 마이그레이션되는 데이터베이스를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="da78c-182">For example, if you are using Code First Migrations, you can configure the database to be migrated automatically using the `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initializer.</span></span>  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
