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
# <a name="dependency-resolution"></a><span data-ttu-id="beb95-102">종속성 확인</span><span class="sxs-lookup"><span data-stu-id="beb95-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="beb95-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="beb95-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="beb95-105">EF6부터 Entity Framework에는 필요한 서비스의 구현을 얻기 위한 범용 메커니즘이 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="beb95-106">즉, EF에서 일부 인터페이스나 기본 클래스의 인스턴스를 사용 하는 경우 사용할 인터페이스 또는 기본 클래스의 구체적인 구현을 요청 합니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="beb95-107">IDbDependencyResolver 인터페이스를 사용 하 여이 작업을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="beb95-108">GetService 메서드는 일반적으로 EF에서 호출 되며 EF 또는 응용 프로그램에서 제공 하는 IDbDependencyResolver 구현에 의해 처리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="beb95-109">호출 될 때 형식 인수는 요청 되는 서비스의 인터페이스 또는 기본 클래스 형식이 고, 키 개체는 요청 된 서비스에 대 한 컨텍스트 정보를 제공 하는 개체 또는 null입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="beb95-110">달리 명시 되지 않은 경우 반환 되는 개체는 단일 항목으로 사용할 수 있으므로 스레드로부터 안전 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-110">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="beb95-111">대부분의 경우 반환 되는 개체는 팩터리 자체가 스레드로부터 안전 해야 하지만 팩터리에서 반환 되는 개체는 각 사용에 대해 팩터리에서 새 인스턴스를 요청 하기 때문에 스레드로부터 안전 하지 않아도 되는 팩터리입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-111">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>

<span data-ttu-id="beb95-112">이 문서에는 IDbDependencyResolver을 구현 하는 방법에 대 한 자세한 내용은 포함 되어 있지 않지만, EF가 GetService를 호출 하는 서비스 형식 (즉, 인터페이스 및 기본 클래스 형식)에 대 한 참조의 역할을 하 고 각각에 대해 키 개체의 의미 체계를 확인 합니다. 요청.</span><span class="sxs-lookup"><span data-stu-id="beb95-112">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span>

## <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="beb95-113">TContext\> < Tdatabase이니셜라이저가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-113">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="beb95-114">**소개 된 버전**: ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="beb95-114">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="beb95-115">**반환 된 개체**: 지정 된 컨텍스트 형식에 대 한 데이터베이스 이니셜라이저</span><span class="sxs-lookup"><span data-stu-id="beb95-115">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="beb95-116">**키**: 사용 되지 않습니다. null이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-116">**Key**: Not used; will be null</span></span>  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="beb95-117">MigrationSqlGenerator\> 함수를 < 합니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-117">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="beb95-118">**소개 된 버전**: ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="beb95-118">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="beb95-119">**반환 된 개체**: 데이터베이스를 사용 하 여 데이터베이스를 만드는 등의 데이터베이스 생성을 유발 하는 마이그레이션 및 기타 작업에 사용할 수 있는 SQL 생성기를 만드는 팩터리입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-119">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="beb95-120">**키**: SQL을 생성할 데이터베이스의 유형을 지정 하는 ADO.NET 공급자 고정 이름을 포함 하는 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-120">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="beb95-121">예를 들어 "System.object" 키에 대 한 SQL Server SQL 생성기가 반환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-121">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="beb95-122">EF6의 공급자 관련 서비스에 대 한 자세한 내용은 [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) 섹션을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="beb95-122">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="beb95-123">System.object. 일반적인 DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="beb95-123">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="beb95-124">**소개 된 버전**: ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="beb95-124">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="beb95-125">**반환 된 개체**: 지정 된 공급자 고정 이름에 사용할 EF 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-125">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="beb95-126">**키**: 공급자가 필요한 데이터베이스 유형을 지정 하는 ADO.NET 공급자 고정 이름을 포함 하는 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-126">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="beb95-127">예를 들어 "System.object" 키에 대 한 SQL Server 공급자가 반환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-127">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="beb95-128">EF6의 공급자 관련 서비스에 대 한 자세한 내용은 [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) 섹션을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="beb95-128">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="beb95-129">IDbConnectionFactory입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="beb95-130">**소개 된 버전**: ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="beb95-130">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="beb95-131">**반환 된 개체**: EF가 규칙에 따라 데이터베이스 연결을 만들 때 사용 되는 연결 팩터리입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-131">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="beb95-132">즉, EF에 연결 또는 연결 문자열을 지정 하지 않고 app.config 또는 web.config에서 연결 문자열을 찾을 수 없는 경우이 서비스는 규칙에 따라 연결을 만드는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-132">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="beb95-133">연결 팩터리를 변경 하면 EF에서 기본적으로 다른 유형의 데이터베이스 (예: SQL Server Compact Edition)를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-133">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="beb95-134">**키**: 사용 되지 않습니다. null이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-134">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="beb95-135">EF6의 공급자 관련 서비스에 대 한 자세한 내용은 [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) 섹션을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="beb95-135">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="beb95-136">IManifestTokenService입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-136">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="beb95-137">**소개 된 버전**: ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="beb95-137">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="beb95-138">**반환 된 개체**: 연결에서 공급자 매니페스트 토큰을 생성할 수 있는 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-138">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="beb95-139">이 서비스는 일반적으로 두 가지 방법으로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-139">This service is typically used in two ways.</span></span> <span data-ttu-id="beb95-140">먼저 모델을 작성할 때 데이터베이스에 연결 Code First를 방지 하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-140">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="beb95-141">두 번째로 Code First 하 여 특정 데이터베이스 버전에 대 한 모델을 작성 하는 데 사용할 수 있습니다. 예를 들어 SQL Server 2008를 사용 하는 경우에도 SQL Server 2005에 대 한 모델을 강제로 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-141">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="beb95-142">**개체 수명**: Singleton-동일한 개체를 여러 번 사용 하 고 다른 스레드에서 동시에 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-142">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="beb95-143">**키**: 사용 되지 않습니다. null이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-143">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="beb95-144">IDbProviderFactoryService입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="beb95-145">**소개 된 버전**: ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="beb95-145">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="beb95-146">**반환 된 개체**: 지정 된 연결에서 공급자 팩터리를 가져올 수 있는 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-146">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="beb95-147">.NET 4.5에서는 연결에서 공급자에 공개적으로 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-147">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="beb95-148">.NET 4에서이 서비스의 기본 구현은 일부 추론을 사용 하 여 일치 하는 공급자를 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-148">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="beb95-149">이러한 오류가 발생 하면 적절 한 해결 방법을 제공 하기 위해이 서비스의 새 구현을 등록할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-149">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="beb95-150">**키**: 사용 되지 않습니다. null이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-150">**Key**: Not used; will be null</span></span>  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="beb95-151">Func < DbContext, IDbModelCacheKey.\></span><span class="sxs-lookup"><span data-stu-id="beb95-151">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="beb95-152">**소개 된 버전**: ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="beb95-152">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="beb95-153">**반환 된 개체**: 지정 된 컨텍스트에 대 한 모델 캐시 키를 생성 하는 팩터리입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-153">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="beb95-154">기본적으로 EF는 공급자 당 DbContext 유형별 모델 하나를 캐시 합니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-154">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="beb95-155">이 서비스의 다른 구현은 스키마 이름과 같은 다른 정보를 캐시 키에 추가 하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-155">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="beb95-156">**키**: 사용 되지 않습니다. null이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-156">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="beb95-157">DbSpatialServices입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-157">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="beb95-158">**소개 된 버전**: ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="beb95-158">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="beb95-159">**반환 된 개체**: 지리 및 기 하 도형 공간 형식에 대 한 기본 ef 공급자에 지원을 추가 하는 EF 공간 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-159">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="beb95-160">**키**: DbSptialServices는 두 가지 방법으로에 대 한 요청을 받습니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-160">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="beb95-161">먼저 DbProviderInfo 개체 (고정 이름 및 매니페스트 토큰 포함)를 키로 사용 하 여 공급자별 공간 서비스를 요청 합니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-161">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="beb95-162">둘째, DbSpatialServices는 키 없이에 대 한 요청을 받을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-162">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="beb95-163">이는 독립 실행형 DbGeography 또는 DbGeometry 형식을 만들 때 사용 되는 "전역 공간 공급자"를 확인 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-163">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="beb95-164">EF6의 공급자 관련 서비스에 대 한 자세한 내용은 [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) 섹션을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="beb95-164">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="beb95-165">IDbExecutionStrategy\> 함수를 < 합니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-165">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="beb95-166">**소개 된 버전**: ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="beb95-166">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="beb95-167">**반환 된 개체**: 데이터베이스에 대해 쿼리 및 명령이 실행 될 때 공급자가 재시도 또는 다른 동작을 구현할 수 있도록 하는 서비스를 만드는 팩터리입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-167">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="beb95-168">구현을 제공 하지 않으면 EF는 단순히 명령을 실행 하 고 throw 된 모든 예외를 전파 합니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-168">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="beb95-169">SQL Server이 서비스는 SQL Azure와 같은 클라우드 기반 데이터베이스 서버에 대해 실행 하는 경우에 특히 유용한 재시도 정책을 제공 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-169">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="beb95-170">**키**: 공급자 고정 이름을 포함 하 고 필요에 따라 실행 전략을 사용할 서버 이름을 포함 하는 ExecutionStrategyKey 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-170">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="beb95-171">EF6의 공급자 관련 서비스에 대 한 자세한 내용은 [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) 섹션을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="beb95-171">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="beb95-172">Func < DbConnection, string, HistoryContext.\></span><span class="sxs-lookup"><span data-stu-id="beb95-172">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="beb95-173">**소개 된 버전**: ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="beb95-173">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="beb95-174">**반환 된 개체**: 공급자가 EF 마이그레이션에 사용 되는 `__MigrationHistory` 테이블에 대 한 HistoryContext 매핑을 구성할 수 있도록 하는 팩터리입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-174">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="beb95-175">HistoryContext는 Code First DbContext, 일반 흐름 API를 사용 하 여 테이블 이름 및 열 매핑 사양과 같은 항목을 변경 하 여 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-175">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="beb95-176">**키**: 사용 되지 않습니다. null이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-176">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="beb95-177">EF6의 공급자 관련 서비스에 대 한 자세한 내용은 [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) 섹션을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="beb95-177">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="beb95-178">DbProviderFactory.</span><span class="sxs-lookup"><span data-stu-id="beb95-178">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="beb95-179">**소개 된 버전**: ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="beb95-179">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="beb95-180">**반환 된 개체**: 지정 된 공급자 고정 이름에 사용할 ADO.NET 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-180">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="beb95-181">**키**: ADO.NET 공급자 고정 이름을 포함 하는 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-181">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="beb95-182">기본 구현에서 일반적인 ADO.NET 공급자 등록을 사용 하기 때문에 일반적으로이 서비스는 직접 변경 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-182">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="beb95-183">EF6의 공급자 관련 서비스에 대 한 자세한 내용은 [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) 섹션을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="beb95-183">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="beb95-184">IProviderInvariantName입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="beb95-185">**소개 된 버전**: ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="beb95-185">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="beb95-186">**반환 된 개체**: 지정 된 형식의 DbProviderFactory에 대 한 공급자 고정 이름을 결정 하는 데 사용 되는 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-186">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="beb95-187">이 서비스의 기본 구현에서는 ADO.NET 공급자 등록을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-187">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="beb95-188">즉, EF에 의해 DbProviderFactory가 확인 되 고 있기 때문에 ADO.NET 공급자가 일반적인 방식으로 등록 되지 않은 경우이 서비스를 해결 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-188">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="beb95-189">**키**: 고정 이름이 필요한 DbProviderFactory 인스턴스입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-189">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="beb95-190">EF6의 공급자 관련 서비스에 대 한 자세한 내용은 [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) 섹션을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="beb95-190">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="beb95-191">System.object. IViewAssemblyCache. IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="beb95-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="beb95-192">**소개 된 버전**: ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="beb95-192">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="beb95-193">**반환 된 개체**: 미리 생성 된 뷰를 포함 하는 어셈블리의 캐시입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-193">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="beb95-194">일반적으로 대체를 사용 하 여 검색을 수행 하지 않고 미리 생성 된 뷰를 포함 하는 어셈블리를 EF에서 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-194">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="beb95-195">**키**: 사용 되지 않습니다. null이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-195">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="beb95-196">복수화. IPluralizationService입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="beb95-197">**소개 된 버전**: ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="beb95-197">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="beb95-198">**반환 된 개체**: EF에서 이름을 복수화 및 단 수 하는 데 사용 하는 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-198">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="beb95-199">기본적으로 영어 복수화 서비스를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-199">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="beb95-200">**키**: 사용 되지 않습니다. null이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-200">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="beb95-201">IDbInterceptor을 (를)</span><span class="sxs-lookup"><span data-stu-id="beb95-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="beb95-202">**소개 된 버전**: ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="beb95-202">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="beb95-203">**반환 된 개체**: 응용 프로그램이 시작 될 때 등록 해야 하는 모든 인터셉터입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-203">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="beb95-204">이러한 개체는 GetServices 호출을 사용 하 여 요청 되며 종속성 확인 기에서 반환 된 모든 인터셉터가 등록 됩니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-204">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="beb95-205">**키**: 사용 되지 않습니다. 는 null입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-205">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="beb95-206">Func < DbContext, Action < string\>,\> System.web. m a c. m a c.</span><span class="sxs-lookup"><span data-stu-id="beb95-206">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="beb95-207">**소개 된 버전**: ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="beb95-207">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="beb95-208">**반환 된 개체**: 컨텍스트에 사용 될 데이터베이스 로그 포맷터를 만드는 데 사용 되는 팩터리입니다. 지정 된 컨텍스트에서 Database. Log 속성을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-208">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="beb95-209">**키**: 사용 되지 않습니다. 는 null입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-209">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="beb95-210">Func < DbContext\></span><span class="sxs-lookup"><span data-stu-id="beb95-210">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="beb95-211">**소개 된 버전**: ef 6.1.0</span><span class="sxs-lookup"><span data-stu-id="beb95-211">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="beb95-212">**반환 된 개체**: 컨텍스트에 액세스할 수 있는 매개 변수가 없는 생성자가 없는 경우 마이그레이션에 대 한 컨텍스트 인스턴스를 만드는 데 사용 되는 팩터리입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-212">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="beb95-213">**키**: 팩터리가 필요한 파생 DbContext의 형식에 대 한 형식 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-213">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="beb95-214">IMetadataAnnotationSerializer\> (Func <)입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-214">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="beb95-215">**소개 된 버전**: ef 6.1.0</span><span class="sxs-lookup"><span data-stu-id="beb95-215">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="beb95-216">**반환 된 개체**: 강력한 형식의 사용자 지정 주석을 직렬화 하는 데 사용 하는 팩터리로,이를 serialize 하 고 Code First 마이그레이션에 사용 하기 위해 XML로 전달할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-216">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="beb95-217">**키**: serialize 되거나 deserialize 되는 주석의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-217">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="beb95-218">Func <의\>입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-218">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="beb95-219">**소개 된 버전**: ef 6.1.0</span><span class="sxs-lookup"><span data-stu-id="beb95-219">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="beb95-220">**반환 된 개체**: 커밋 실패를 처리 하는 등의 상황에 특별 한 처리를 적용할 수 있도록 트랜잭션에 대 한 처리기를 만드는 데 사용 되는 팩터리입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-220">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="beb95-221">**키**: 공급자 고정 이름을 포함 하 고 필요에 따라 트랜잭션 처리기를 사용할 서버 이름을 포함 하는 ExecutionStrategyKey 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="beb95-221">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
