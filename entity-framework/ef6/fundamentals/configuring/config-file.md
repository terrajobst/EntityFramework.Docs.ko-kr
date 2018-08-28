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
# <a name="configuration-file-settings"></a><span data-ttu-id="31cda-102">구성 파일 설정</span><span class="sxs-lookup"><span data-stu-id="31cda-102">Configuration File Settings</span></span>
<span data-ttu-id="31cda-103">Entity Framework에는 다양을 한 설정 구성 파일에서 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-103">Entity Framework allows a number of settings to be specified from the configuration file.</span></span> <span data-ttu-id="31cda-104">일반적 EF '구성 보다 규칙' 원칙을 따릅니다:이 게시물에 설명 된 모든 설정은 기본 동작을가 기본값에는 더 이상 요구 사항을 충족 하는 경우 설정을 변경 하는 방법에 대 한 걱정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-104">In general EF follows a ‘convention over configuration’ principle: all the settings discussed in this post have a default behavior, you only need to worry about changing the setting when the default no longer satisfies your requirements.</span></span>  

## <a name="a-code-based-alternative"></a><span data-ttu-id="31cda-105">코드 기반 대체 항목</span><span class="sxs-lookup"><span data-stu-id="31cda-105">A Code-Based Alternative</span></span>  

<span data-ttu-id="31cda-106">이러한 모든 설정을 적용할 수도 있습니다 코드를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-106">All of these settings can also be applied using code.</span></span> <span data-ttu-id="31cda-107">부터 도입 되었습니다 EF6 [코드 기반 구성](code-based.md), 코드에서 구성을 적용 하는 중앙 방법을 제공 하는 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-107">Starting in EF6 we introduced [code-based configuration](code-based.md), which provides a central way of applying configuration from code.</span></span> <span data-ttu-id="31cda-108">EF6를 이전 코드에서 구성도 적용할 수 있지만 다양 한 Api를 사용 하 여 다양 한 영역을 구성 하 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-108">Prior to EF6, configuration can still be applied from code but you need to use various APIs to configure different areas.</span></span> <span data-ttu-id="31cda-109">구성 파일 옵션에는 이러한 설정을 배포 하는 동안 코드를 업데이트 하지 않고 쉽게 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-109">The configuration file option allows these settings to be easily changed during deployment without updating your code.</span></span>

## <a name="the-entity-framework-configuration-section"></a><span data-ttu-id="31cda-110">Entity Framework 구성 섹션</span><span class="sxs-lookup"><span data-stu-id="31cda-110">The Entity Framework Configuration Section</span></span>  

<span data-ttu-id="31cda-111">EF4.1 있습니다부터 사용 하 여 상황에 맞는 데이터베이스 이니셜라이저를 설정할 수는 **appSettings** 구성 파일의 섹션입니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-111">Starting with EF4.1 you could set the database initializer for a context using the **appSettings** section of the configuration file.</span></span> <span data-ttu-id="31cda-112">사용자 지정 소개 했습니다 EF 4.3 **entityFramework** 새 설정을 처리 하는 섹션입니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-112">In EF 4.3 we introduced the custom **entityFramework** section to handle the new settings.</span></span> <span data-ttu-id="31cda-113">Entity Framework에서 여전히 이전 형식을 사용 하 여 설정 하는 데이터베이스 이니셜라이저를 인식 하지만 가능한 경우 새 형식으로 이동 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-113">Entity Framework will still recognize database initializers set using the old format, but we recommend moving to the new format where possible.</span></span>

<span data-ttu-id="31cda-114">합니다 **entityFramework** 섹션 EntityFramework NuGet 패키지를 설치할 때 자동으로 프로젝트의 구성 파일에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-114">The **entityFramework** section was automatically added to the configuration file of your project when you installed the EntityFramework NuGet package.</span></span>  

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

## <a name="connection-strings"></a><span data-ttu-id="31cda-115">연결 문자열</span><span class="sxs-lookup"><span data-stu-id="31cda-115">Connection Strings</span></span>  

<span data-ttu-id="31cda-116">[이 페이지](~/ef6/fundamentals/configuring/connection-strings.md) 구성 파일에서 연결 문자열을 포함 하 여 Entity Framework에서 사용할 데이터베이스를 결정 하는 방법에 자세한 정보를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-116">[This page](~/ef6/fundamentals/configuring/connection-strings.md) provides more details on how Entity Framework determines the database to be used, including connection strings in the configuration file.</span></span>  

<span data-ttu-id="31cda-117">연결 문자열은 표준에서 이동 **connectionStrings** 요소 않아도 합니다 **entityFramework** 섹션.</span><span class="sxs-lookup"><span data-stu-id="31cda-117">Connection strings go in the standard **connectionStrings** element and do not require the **entityFramework** section.</span></span>  

<span data-ttu-id="31cda-118">먼저 기반 코드 모델 일반 ADO.NET 연결 문자열을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-118">Code First based models use normal ADO.NET connection strings.</span></span> <span data-ttu-id="31cda-119">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="31cda-119">For example:</span></span>  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

<span data-ttu-id="31cda-120">EF 디자이너 모델 사용 하 여 특수 한 EF 연결 문자열을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-120">EF Designer based models use special EF connection strings.</span></span> <span data-ttu-id="31cda-121">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="31cda-121">For example:</span></span>  

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

## <a name="code-based-configuration-type-ef6-onwards"></a><span data-ttu-id="31cda-122">코드 기반 구성 유형 (EF6부터 해당)</span><span class="sxs-lookup"><span data-stu-id="31cda-122">Code-Based Configuration Type (EF6 Onwards)</span></span>  

<span data-ttu-id="31cda-123">EF6부터 데 EF에 대 한 DbConfiguration 지정할 수 있습니다 [코드 기반 구성](code-based.md) 응용 프로그램에서 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-123">Starting with EF6, you can specify the DbConfiguration for EF to use for [code-based configuration](code-based.md) in your application.</span></span> <span data-ttu-id="31cda-124">대부분의 EF를 DbConfiguration에 자동으로 검색 하는 대로이 설정을 지정할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-124">In most cases you don't need to specify this setting as EF will automatically discover your DbConfiguration.</span></span> <span data-ttu-id="31cda-125">DbConfiguration config 파일에서 지정 해야 하는 경우의 세부 정보 참조를 **이동 DbConfiguration** 부분 [코드 기반 구성](code-based.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-125">For details of when you may need to specify DbConfiguration in your config file see the **Moving DbConfiguration** section of [Code-Based Configuration](code-based.md).</span></span>  

<span data-ttu-id="31cda-126">어셈블리 정규화 된 형식 이름을 DbConfiguration 형식을 설정 하려면 지정 합니다 **codeConfigurationType** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-126">To set a DbConfiguration type, you specify the assembly qualified type name in the **codeConfigurationType** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="31cda-127">정규화 된 어셈블리 이름은 쉼표, 어셈블리의 형식에 있는 다음 네임 스페이스 정규화 된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-127">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="31cda-128">필요에 따라 어셈블리 버전, 문화권 및 공개 키 토큰을 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-128">You can optionally also specify the assembly version, culture and public key token.</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a><span data-ttu-id="31cda-129">EF 데이터베이스 공급자 (EF6부터 해당)</span><span class="sxs-lookup"><span data-stu-id="31cda-129">EF Database Providers (EF6 Onwards)</span></span>  

<span data-ttu-id="31cda-130">EF6을 하기 전에 데이터베이스 공급자의 Entity Framework 전용 부분 코어 ADO.NET 공급자의 일부로 포함 되도록 했습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-130">Prior to EF6, Entity Framework-specific parts of a database provider had to be included as part of the core ADO.NET provider.</span></span> <span data-ttu-id="31cda-131">EF6부터 EF 특정 부분은 이제 관리 되며 별도로 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-131">Starting with EF6, the EF specific parts are now managed and registered separately.</span></span>  

<span data-ttu-id="31cda-132">일반적으로 공급자를 직접 등록할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-132">Normally you won't need to register providers yourself.</span></span> <span data-ttu-id="31cda-133">일반적으로 이렇게 공급자를 설치할 때 설치 됩니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-133">This will typically be done by the provider when you install it.</span></span>  

<span data-ttu-id="31cda-134">공급자를 포함 하 여 등록 된를 **공급자** 요소 아래에 있는 **공급자** 의 하위 섹션을 **entityFramework** 섹션.</span><span class="sxs-lookup"><span data-stu-id="31cda-134">Providers are registered by including a **provider** element under the **providers** child section of the **entityFramework** section.</span></span> <span data-ttu-id="31cda-135">공급자 항목에 대 한 필수 특성을 두 가지 있습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-135">There are two required attributes for a provider entry:</span></span>  

- <span data-ttu-id="31cda-136">**invariantName** core ADO.NET 공급자를 식별 하는이 EF 공급자가 대상</span><span class="sxs-lookup"><span data-stu-id="31cda-136">**invariantName** identifies the core ADO.NET provider that this EF provider targets</span></span>  
- <span data-ttu-id="31cda-137">**형식** EF 공급자 구현의 어셈블리 정규화 된 형식 이름</span><span class="sxs-lookup"><span data-stu-id="31cda-137">**type** is the assembly qualified type name of the EF provider implementation</span></span>  

> [!NOTE]
> <span data-ttu-id="31cda-138">정규화 된 어셈블리 이름은 쉼표, 어셈블리의 형식에 있는 다음 네임 스페이스 정규화 된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-138">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="31cda-139">필요에 따라 어셈블리 버전, 문화권 및 공개 키 토큰을 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-139">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="31cda-140">예를 들어 Entity Framework를 설치한 경우 기본 SQL Server 공급자를 등록 하기 위해 만든 항목을 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-140">As an example here is the entry created to register the default SQL Server provider when you install Entity Framework.</span></span>  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a><span data-ttu-id="31cda-141">인터셉터 (EF6.1부터 해당)</span><span class="sxs-lookup"><span data-stu-id="31cda-141">Interceptors (EF6.1 Onwards)</span></span>  

<span data-ttu-id="31cda-142">EF6.1를 사용 하 여 시작 하면 인터셉터 구성 파일에 등록할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-142">Starting with EF6.1 you can register interceptors in the configuration file.</span></span> <span data-ttu-id="31cda-143">인터셉터를 사용 하면 EF 연결 등을 열어 데이터베이스 쿼리 실행과 같은 특정 작업을 수행 하는 경우 추가 논리를 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-143">Interceptors allow you to run additional logic when EF performs certain operations, such as executing database queries, opening connections, etc.</span></span>  

<span data-ttu-id="31cda-144">인터셉터를 포함 하 여 등록 된는 **인터셉터** 요소 아래에 있는 **인터셉터** 자식 섹션의 **entityFramework** 섹션.</span><span class="sxs-lookup"><span data-stu-id="31cda-144">Interceptors are registered by including an **interceptor** element under the **interceptors** child section of the **entityFramework** section.</span></span> <span data-ttu-id="31cda-145">예를 들어, 다음 구성을 등록 기본 제공 **DatabaseLogger** 인터셉터는 콘솔에 모든 데이터베이스 작업을 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-145">For example, the following configuration registers the built-in **DatabaseLogger** interceptor that will log all database operations to the Console.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a><span data-ttu-id="31cda-146">로깅 데이터베이스 작업 파일 (EF6.1부터 해당)</span><span class="sxs-lookup"><span data-stu-id="31cda-146">Logging Database Operations to a File (EF6.1 Onwards)</span></span>  

<span data-ttu-id="31cda-147">인터셉터 구성 파일을 통해 등록 문제를 디버깅 하는 데 기존 응용 프로그램에 로깅을 추가 하려는 경우 특히 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-147">Registering interceptors via the config file is especially useful when you want to add logging to an existing application to help debug an issue.</span></span> <span data-ttu-id="31cda-148">**DatabaseLogger** 생성자 매개 변수로 파일 이름을 제공 하 여 파일에는 로깅을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-148">**DatabaseLogger** supports logging to a file by supplying the file name as a constructor parameter.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="31cda-149">기본적으로이 로그 파일을 앱이 시작 될 때마다 새 파일을 사용 하 여 덮어쓸 수로 인해 됩니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-149">By default this will cause the log file to be overwritten with a new file each time the app starts.</span></span> <span data-ttu-id="31cda-150">로그에 대신 추가할 파일 이미 있는 경우에 같은 코드를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-150">To instead append to the log file if it already exists use something like:</span></span>  

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

<span data-ttu-id="31cda-151">에 대 한 자세한 **DatabaseLogger** 블로그 게시물을 참조 인터셉터를 등록 하 고 [EF 6.1: 다시 컴파일하지 않고도 로깅을 사용 하도록](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/)합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-151">For additional information on **DatabaseLogger** and registering interceptors, see the blog post [EF 6.1: Turning on logging without recompiling](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span></span>  

## <a name="code-first-default-connection-factory"></a><span data-ttu-id="31cda-152">첫 번째 기본 연결 팩터리 코드</span><span class="sxs-lookup"><span data-stu-id="31cda-152">Code First Default Connection Factory</span></span>  

<span data-ttu-id="31cda-153">구성 섹션을 사용 하면 Code First 컨텍스트에 대해 사용 하도록 데이터베이스를 찾으려고 사용 해야 하는 기본 연결 팩터리를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-153">The configuration section allows you to specify a default connection factory that Code First should use to locate a database to use for a context.</span></span> <span data-ttu-id="31cda-154">기본 연결 팩터리는 컨텍스트에 대 한 구성 파일에 연결 문자열을 추가한 경우에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-154">The default connection factory is only used when no connection string has been added to the configuration file for a context.</span></span>  

<span data-ttu-id="31cda-155">EF NuGet 패키지를 설치한 경우 SQL Express 또는 LocalDB에 어떤 것에 따라 설치를 가리키는 기본 연결 팩터리가 등록 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-155">When you installed the EF NuGet package a default connection factory was registered that points to either SQL Express or LocalDB, depending on which one you have installed.</span></span>  

<span data-ttu-id="31cda-156">어셈블리 정규화 된 형식 이름을 지정 하면 연결 팩터리를 설정 하려면 합니다 **deafultConnectionFactory** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-156">To set a connection factory, you specify the assembly qualified type name in the **deafultConnectionFactory** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="31cda-157">정규화 된 어셈블리 이름은 쉼표, 어셈블리의 형식에 있는 다음 네임 스페이스 정규화 된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-157">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="31cda-158">필요에 따라 어셈블리 버전, 문화권 및 공개 키 토큰을 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-158">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="31cda-159">사용자 고유의 기본 연결 팩터리가 설정 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-159">Here is an example of setting your own default connection factory:</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

<span data-ttu-id="31cda-160">위의 예제에는 매개 변수가 없는 생성자를 갖도록 사용자 지정 팩터리가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-160">The above example requires the custom factory to have a parameterless constructor.</span></span> <span data-ttu-id="31cda-161">필요한 경우에 생성자 매개 변수를 사용 하 여 지정할 수 있습니다 합니다 **매개 변수** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-161">If needed, you can specify constructor parameters using the **parameters** element.</span></span>  

<span data-ttu-id="31cda-162">예를 들어, Entity Framework에 포함 된, SqlCeConnectionFactory을 사용 하려면 공급자 고정 이름을 생성자에 제공 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-162">For example, the SqlCeConnectionFactory, that is included in Entity Framework, requires you to supply a provider invariant name to the constructor.</span></span> <span data-ttu-id="31cda-163">공급자 고정 이름을 사용 하려는 SQL Compact의 버전을 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-163">The provider invariant name identifies the version of SQL Compact you want to use.</span></span> <span data-ttu-id="31cda-164">다음 구성은 기본적으로 SQL Compact 버전 4.0을 사용 하는 컨텍스트를 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-164">The following configuration will cause contexts to use SQL Compact version 4.0 by default.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="31cda-165">기본 연결 팩터리가 설정 하지 않으면, Code First 사용을 가리키는 SqlConnectionFactory `.\SQLEXPRESS`합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-165">If you don’t set a default connection factory, Code First uses the SqlConnectionFactory, pointing to `.\SQLEXPRESS`.</span></span> <span data-ttu-id="31cda-166">SqlConnectionFactory 연결 문자열의 부분을 재정의할 수 있도록 하는 생성자가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-166">SqlConnectionFactory also has a constructor that allows you to override parts of the connection string.</span></span> <span data-ttu-id="31cda-167">이외의 SQL Server 인스턴스를 사용 하려는 경우 `.\SQLEXPRESS` 이 생성자를 사용 하 여 서버를 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-167">If you want to use a SQL Server instance other than `.\SQLEXPRESS` you can use this constructor to set the server.</span></span>  

<span data-ttu-id="31cda-168">다음 구성은 Code First를 사용 하면 **MyDatabaseServer** 는 명시적 연결 문자열 설정 되지 않은 컨텍스트에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-168">The following configuration will cause Code First to use **MyDatabaseServer** for contexts that don’t have an explicit connection string set.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="31cda-169">기본적으로 문자열 형식의 생성자 인수는 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-169">By default, it’s assumed that constructor arguments are of type string.</span></span> <span data-ttu-id="31cda-170">이 설정을 변경 하려면 type 특성을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-170">You can use the type attribute to change this.</span></span>  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a><span data-ttu-id="31cda-171">데이터베이스 이니셜라이저</span><span class="sxs-lookup"><span data-stu-id="31cda-171">Database Initializers</span></span>  

<span data-ttu-id="31cda-172">데이터베이스 이니셜라이저 컨텍스트 당 별로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-172">Database initializers are configured on a per-context basis.</span></span> <span data-ttu-id="31cda-173">사용 하 여 구성 파일에서 설정할 수는 **상황에 맞는** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-173">They can be set in the configuration file using the **context** element.</span></span> <span data-ttu-id="31cda-174">이 요소는 구성 중인 컨텍스트를 식별 하 어셈블리의 정규화 된 이름을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-174">This element uses the assembly qualified name to identify the context being configured.</span></span>  

<span data-ttu-id="31cda-175">기본적으로 Code First 컨텍스트에 CreateDatabaseIfNotExists 이니셜라이저를 사용 하도록 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-175">By default, Code First contexts are configured to use the CreateDatabaseIfNotExists initializer.</span></span> <span data-ttu-id="31cda-176">**disableDatabaseInitialization** 특성을 합니다 **상황에 맞는** 데이터베이스 초기화를 사용 하지 않도록 설정 하는 데 사용할 수 있는 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-176">There is a **disableDatabaseInitialization** attribute on the **context** element that can be used to disable database initialization.</span></span>  

<span data-ttu-id="31cda-177">예를 들어, 다음 구성을 MyAssembly.dll에 정의 된 Blogging.BlogContext 컨텍스트에 대 한 데이터베이스 초기화를 해제 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-177">For example, the following configuration disables database initialization for the Blogging.BlogContext context defined in MyAssembly.dll.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

<span data-ttu-id="31cda-178">사용할 수는 **databaseInitializer** 사용자 정의 이니셜라이저 함수를 설정할 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-178">You can use the **databaseInitializer** element to set a custom initializer.</span></span>  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

<span data-ttu-id="31cda-179">기본 연결 팩터리와 동일한 구문을 사용 하는 생성자 매개 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-179">Constructor parameters use the same syntax as default connection factories.</span></span>  

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

<span data-ttu-id="31cda-180">Entity Framework에 포함 된 일반 데이터베이스 이니셜라이저 중 하나를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-180">You can configure one of the generic database initializers that are included in Entity Framework.</span></span> <span data-ttu-id="31cda-181">합니다 **형식** 특성 제네릭 형식에 대 한.NET Framework 형식을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-181">The **type** attribute uses the .NET Framework format for generic types.</span></span>  

<span data-ttu-id="31cda-182">예를 들어, Code First 마이그레이션을 사용 하는 경우는 마이그레이션할 데이터베이스를 사용 하 여 자동으로 구성할 수는 `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` 이니셜라이저입니다.</span><span class="sxs-lookup"><span data-stu-id="31cda-182">For example, if you are using Code First Migrations, you can configure the database to be migrated automatically using the `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initializer.</span></span>  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
